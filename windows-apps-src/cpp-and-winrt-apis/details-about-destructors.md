---
description: C++/WinRT 2.0 ermöglicht es Ihnen, die Zerstörung Ihrer Implementierungstypen zu verzögern und während der Zerstörung sicher Abfragen auszuführen. Dieses Thema beschreibt diese Features und erläutert die Verwendung.
title: Informationen zu Destruktoren
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, verzögerte zerstörung, sichere abfragen
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387797"
---
# <a name="details-about-destructors"></a>Informationen zu Destruktoren

C++/WinRT 2.0 ermöglicht es Ihnen, die Zerstörung Ihrer Implementierungstypen zu verzögern und während der Zerstörung sicher Abfragen auszuführen. Dieses Thema beschreibt diese Features und erläutert die Verwendung.

## <a name="deferred-destruction"></a>Verzögerte Zerstörung

Im Artikel [Diagnostizieren direkter Zuordnungen](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc) wurde darauf hingewiesen, dass Ihr Implementierungstyp keinen privaten Destruktor aufweisen kann.

Der Vorteil eines öffentlichen Destruktors besteht darin, dass er die verzögerte Zerstörung ermöglicht. Dabei handelt es sich um die Fähigkeit, den endgültigen Aufruf von [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) für das Objekt zu erkennen und den Besitz des Objekts zu übernehmen, um dessen Zerstörung unbefristet zu verzögern.

Bekanntlich werden herkömmliche COM-Objekte intrinsisch anhand von Verweisen gezählt; die Verweisanzahl wird über die Funktionen [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) und **IUnknown::Release** verwaltet. In einer herkömmlichen Implementierung von **Release** wird der C++-Destruktor eines herkömmlichen COM-Objekts aufgerufen, sobald die Verweisanzahl 0 erreicht.

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;` ruft den Destruktor des Objekts auf, bevor der vom Objekt belegte Speicher freigegeben wird. Dies funktioniert gut, so lange Sie in Ihrem Destruktor keine wichtigen Vorgänge ausführen müssen.

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

Was bedeutet in diesem Zusammenhang *interessant*? Erstens ist ein Destruktor von Natur aus synchron. Das Wechseln zwischen Threads ist nicht möglich, z. B. um einige threadspezifischen Ressourcen in einem anderen Kontext zu zerstören. Sie können das Objekt nicht zuverlässig nach einer anderen Schnittstelle abfragen, die Sie eventuell benötigen, um bestimmte Ressourcen freizugeben. Und das ist noch nicht alles. In Szenarien, in denen die Zerstörung weniger unkompliziert ist, benötigen Sie eine flexiblere Lösung. Hier kommt die **final_release**-Funktion von C++/WinRT zur Anwendung.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

Wir haben die C++/WinRT-Implementierung von **Release** so aktualisiert, dass **final_release** genau dann aufgerufen wird, wenn die Verweisanzahl 0 erreicht. In diesem Zustand kann für das Objekt mit Sicherheit davon ausgegangen werden, dass keine weiteren ausstehenden Verweise vorhanden sind und dass sich das Objekt ausschließlich selbst besitzt. Daher kann es den Besitz an die statische **final_release**-Funktion übertragen.

Mit anderen Worten: Es findet eine Transformation von einem Objekt, das den gemeinsamen Besitz unterstützt, zu einem Objekt mit exklusivem Besitz statt. Der **std::unique_ptr** besitzt das Objekt exklusiv, und daher zerstört er das Objekt naturgemäß entsprechend seiner Semantik. Daher wird ein öffentlicher Destruktor benötigt, wenn der **std::unique_ptr** seinen Gültigkeitsbereich verlässt (vorausgesetzt, er wird nicht zuvor an eine andere Stelle verschoben). Dies ist der wichtige Punkt. Sie können das Objekt unbefristet verwenden, so lange der **std::unique_ptr** das Objekt beibehält. Im Folgenden wird veranschaulicht, wie Sie das Objekt an eine andere Stelle verschieben können.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

Dies ist mit einem stärker deterministischen Garbage Collector vergleichbar. Praktischer und effizienter wäre es, die **final_release**-Funktion in eine Coroutine umzuwandeln und schließlich die Zerstörung an einer zentralen Stelle zu behandeln, wobei sie gleichzeitig in der Lage sind, Threads ggf. zu unterbrechen und zu wechseln.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

Eine Unterbrechung bewirkt eine Rückgabe des aufrufenden Threads, der den Aufruf der **IUnknown::Release**-Funktion ursprünglich initiiert hat, und damit wird dem Aufrufer signalisiert, dass das ursprünglich enthaltene Objekt über diesen Schnittstellenzeiger nicht mehr verfügbar ist. UI-Frameworks müssen oft sicherstellen, dass Objekte im konkreten UI-Thread zerstört werden, die das Objekt ursprünglich erstellt haben. Durch diese Funktion kann eine solche Anforderung problemlos erfüllt werden, da die Zerstörung des Objekts von seiner Freigabe getrennt erfolgt.

## <a name="safe-queries-during-destruction"></a>Sichere Abfragen während der Zerstörung

Auf dem Konzept der verzögerten Zerstörung baut die Fähigkeit auf, Schnittstellen während der Zerstörung sicher abzufragen.

Das klassische COM basiert auf zwei zentralen Konzepten. Das erste ist die Verweiszählung, das zweite die Abfrage von Schnittstellen. Zusätzlich zu **AddRef** und **Release** stellt die **IUnknown**-Schnittstelle [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void) bereit. Diese Methode wird von bestimmten UI-Frameworks ausgiebig genutzt, z. B. XAML, um beim Simulieren des zusammensetzbaren Typsystems die XAML-Hierarchie zu durchlaufen. Betrachten wir dazu ein einfaches Beispiel.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

Dies mag harmlos *erscheinen*. Diese XAML-Seite möchte ihren Datenkontext im Destruktor löschen. [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) ist jedoch eine Eigenschaft der **FrameworkElement**-Basisklasse, und sie ist in der eindeutigen **IFrameworkElement**-Schnittstelle aktiv. Daher muss C++/WinRT einen Aufruf von **QueryInterface** einfügen, um in der richtigen vtable nachzuschlagen und die **DataContext**-Eigenschaft aufrufen zu können. Wir befinden uns jedoch nur im Destruktor, weil die Verweisanzahl 0 erreicht hat. Durch den Aufruf von **QueryInterface** wird diese Verweisanzahl vorübergehend erhöht; wenn sie wieder 0 erreicht, wird das Objekt erneut zerstört.

C++/WinRT 2.0 wurde verstärkt und bietet nun eine solche Unterstützung. Hier finden Sie die C++/WinRT 2.0-Implementierung von Release in vereinfachter Form.

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

Wie vorauszusehen, verringert sie zunächst die Verweisanzahl und wird erst dann tätig, wenn keine ausstehenden Verweise vorhanden sind. Vor dem Aufrufen der statischen **final_release**-Funktion (die in diesem Artikel bereits beschrieben wurde) stabilisiert sie die Verweisanzahl, indem sie auf 1 festgelegt wird. Ein solches Vorgehen wird als *Entprellen* bezeichnet (ein aus der Elektrotechnik entlehnter Begriff). Dies ist unerlässlich, um zu verhindern, dass der endgültige Verweis freigegeben wird. Sobald dies geschieht, ist die Verweisanzahl instabil, und ein Aufruf von **QueryInterface** kann nicht zuverlässig unterstützt werden.

Ein Aufruf von **QueryInterface** ist nach der Freigabe des endgültigen Verweises gefährlich, da die Verweisanzahl dann möglicherweise unbegrenzt ansteigen kann. Sie sollten daher nur bekannte Codepfade aufrufen, welche die Lebensdauer des Objekts nicht verlängern. C++/WinRT kommt ihnen ein Stück entgegen und stellt sicher, dass solche Aufrufe von **QueryInterface** zuverlässig ausgeführt werden können.

Dazu wird die Verweisanzahl stabilisiert. Wenn der abschließende Verweis freigegeben wurde, ist die tatsächliche Verweisanzahl entweder 0 oder ein völlig unvorhersehbarer Wert. Letzteres kann der Fall sein, wenn schwache Verweise vorhanden sind. Solche Umstände sind jedoch in keinem Fall akzeptabel, wenn ein nachfolgender Aufruf von **QueryInterface** auftritt; hierbei wird notwendigerweise ein vorübergehender Anstieg der Verweisanzahl bewirkt – daher der Verweis auf das Entprellen. Durch das Festlegen auf 1 wird sichergestellt, dass für dieses Objekt künftig kein endgültiger Aufruf von **Release** stattfindet. Genau dies ist beabsichtigt, da der **std::unique_ptr** nun das Objekt besitzt, gebundene Aufrufe von **QueryInterface**/**Release**-Paaren sind jedoch sicher.

Schauen wir uns ein interessanteres Beispiel an.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

Zuerst wird die **final_release**-Funktion aufgerufen, die die Implementierung benachrichtigt, dass eine Bereinigung erforderlich ist. Hier ist **final_release** eine Coroutine. Um einen ersten Unterbrechungspunkt zu simulieren, wird zunächst einige Sekunden im Threadpool gewartet. Im Verteilerthread der Seite wird die Ausführung fortgesetzt. Der letzte Schritt schließt eine Abfrage ein, da [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) eine Eigenschaft der **DependencyObject**-Basisklasse ist. Schließlich wird die Seite tatsächlich gelöscht; hierfür wird `nullptr` dem **std::unique_ptr** zugewiesen. Dadurch wiederum wird der Destruktor der Seite aufgerufen.

Im Destruktor löschen wir den Datenkontext, der bekanntlich eine Abfrage der **FrameworkElement**-Basisklasse benötigt.

All dies ist möglich aufgrund der Entprellung der Verweisanzahl (bzw. Stabilisierung der Verweisanzahl) in C++/WinRT 2.0.