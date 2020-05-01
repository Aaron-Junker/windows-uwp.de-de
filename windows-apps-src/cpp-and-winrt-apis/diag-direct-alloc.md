---
description: Dieses Thema erläutert eingehend ein C++/WinRT 2.0-Feature, das Ihnen beim Diagnostizieren eines Fehlers hilft, der entsteht, weil ein Objekt des Implementierungstyps im Stapel statt wie vorgesehen durch Verwendung der [**winrt::make**](/uwp/cpp-ref-for-winrt/make)-Hilfsprogrammfamilie erstellt wurde.
title: Diagnostizieren direkter Zuordnungen
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, direkt, stapel, zuweisungen, projiziert, implementierung
ms.localizationpriority: medium
ms.openlocfilehash: 7fe8ff6653b8655ee25cd9adc0c11acb22d42a11
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "68372791"
---
# <a name="diagnosing-direct-allocations"></a>Diagnostizieren direkter Zuordnungen

Wie in [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis) erläutert, sollten Sie beim Erstellen eines Objekts mit dem Implementierungstyp die Gruppe der [**winrt::make**](/uwp/cpp-ref-for-winrt/make)-Hilfsfunktionen verwenden. Dieses Thema erläutert eingehend ein C++/WinRT 2.0-Feature, das Ihnen beim Diagnostizieren eines Fehlers hilft, der auftritt, wenn ein Objekt des Implementierungstyps direkt im Stapel zugewiesen wurde.

Solche Fehler können zu unerklärlichen Abstürzen oder Beschädigungen führen, die schwierig und zeitaufwendig zu debuggen sind. Das ist also ein wichtiges Feature, und es lohnt sich, die Hintergründe zu verstehen.

## <a name="setting-the-scene-with-mystringable"></a>Die Vorgeschichte: **MyStringable**

Wir gehen von einer einfachen Implementierung von [**IStringable**](/uwp/api/windows.foundation.istringable) aus.

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

Stellen Sie sich nun vor, dass Sie eine Funktion (innerhalb ihrer Implementierung) aufrufen müssen, die eine **IStringable** als Argument erwartet.

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

Das Problem besteht darin, dass es sich bei dem **MyStringable**-Typ *nicht* um eine **IStringable** handelt.

- Der **MyStringable**-Typ ist eine Implementierung der **IStringable**-Schnittstelle.
- Der **IStringable**-Typ ist ein projizierter Typ.

> [!IMPORTANT]
> Der Unterschied zwischen einem *Implementierungstyp* und einem *projizierten Typ* ist hierbei entscheidend. Zu den zentralen Konzepten und Begriffe sollten Sie [Nutzen von APIs mit C++/WinRT](consume-apis.md) und [Erstellen von APIs mit C++/WinRT](author-apis.md) lesen.

Zwischen einer Implementierung und der Projektion besteht ein feiner Unterschied. Damit sich die Implementierung mehr wie die Projektion verhält, bietet sie implizite Konvertierungen in jeden der projizierten Typen, die von ihr implementiert werden. Das heißt jedoch nicht, dass dies problemlos möglich ist.

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

Stattdessen muss ein Verweis abgerufen werden, damit Konvertierungsoperatoren als Kandidaten für die Auflösung des Aufrufs verwendet werden können.

```cppwinrt
void Call()
{
    Print(*this);
}
```

Das wiederum funktioniert. Eine implizite Konvertierung bietet eine (sehr effiziente) Konvertierung vom Implementierungstyp in den projizierten Typ und erweist sich in vielen Szenarien als sehr praktisch. Ohne diese Möglichkeit wären viele Implementierungstypen nur sehr umständlich zu erstellen. Vorausgesetzt, dass Sie nur die [**winrt::make**](/uwp/cpp-ref-for-winrt/make)-Funktionsvorlage (oder [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)) verwenden, um die Implementierung zuzuordnen, ist alles in Ordnung.

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>Potenzielle Probleme bei C++/WinRT 1.0

Dennoch können implizite Konvertierungen Probleme verursachen. Betrachten Sie diese wenig hilfreiche Hilfsfunktion.

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

Oder auch nur diese scheinbar harmlose Anweisung.

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

Leider lässt sich solcher Code aufgrund der impliziten Konvertierung *durchaus* mit C++/WinRT 1.0 konvertieren. Das (schwerwiegende) Problem besteht darin, dass wir möglicherweise einen projizierten Typ zurückgeben, der auf ein Objekt mit Verweiszählung verweist, dessen zugrunde liegender Speicher auf dem kurzlebigen Stapel liegt.

Hier ein anderes mit C++/WinRT 1.0 kompiliertes Beispiel.

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

Unformatierte Zeiger sind riskant und Ursache aufwendiger Fehler. Verwenden Sie sie nur, wenn es sein muss. C++/WinRT bietet alle Möglichkeiten des effizienten Programmierens ohne die Notwendigkeit, jemals unformatierte Zeiger verwenden zu müssen. Hier ein anderes mit C++/WinRT 1.0 kompiliertes Beispiel.

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

Dieser Fehler umfasst mehrere Ebenen. Wir haben zwei verschiedene Verweiszähler für das gleiche Objekt. Die Windows-Runtime (und das vorausgegangene klassische COM) basiert auf einem systeminternen Verweiszähler, der nicht mit **std::shared_ptr** kompatibel ist. Für **std::shared_ptr** gibt es natürlich viele sinnvolle Anwendungen, aber er ist völlig unnötig, wenn Sie Windows-Runtime-Objekte (und klassische COM-Objekte) freigeben. Schließlich lässt sich auch dieses Beispiel mit C++/WinRT 1.0 kompilieren.

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

Dies ist wieder recht fraglich. Der eindeutige Besitz steht in Konflikt mit der gemeinsamen Lebensdauer des systeminternen Verweiszählers von **MyStringable**.

## <a name="the-solution-with-cwinrt-20"></a>Die Lösung bei C++/WinRT 2.0

Bei C++/WinRT 2.0 führt der Versuch, Implementierungstypen direkt zuzuordnen, zu einem Compilerfehler. So muss ein Fehler behandelt werden. Diese Lösung ist deutlich besser als ein mysteriöser Laufzeitfehler.

Wenn Sie eine Implementierung vornehmen müssen, können Sie einfach wie oben [**winrt::make**](/uwp/cpp-ref-for-winrt/make) oder [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) verwenden. Und sollten Sie dies jetzt einmal vergessen, werden Sie von einem Compilerfehler darauf hingewiesen, der auf eine abstrakte Funktion mit dem Namen **use_make_function_to_create_this_object** verweist. Das ist noch kein `static_assert`, kommt diesem aber nahe. Dies bleibt die zuverlässigste Methode, alle beschriebenen Fehler zu erkennen.

Das bedeutet, dass wir einige geringfügige Einschränkungen für die Implementierung festlegen müssen. Da wir uns auf das Fehlen einer Überschreibung zur Erkennung der direkten Zuordnung stützen, muss die Funktionsvorlage **winrt::make** die abstrakte virtuelle Funktion irgendwie mit einer Überschreibung erfüllen. Dies erfolgt durch Ableiten von der Implementierung mit einer `final` Klasse, die die Überschreibung bereitstellt. Bei diesem Prozess müssen einige Punkte beachtet werden.

Zunächst ist die virtuelle Funktion nur in Debugbuilds vorhanden. Dies bedeutet, dass die Erkennung die Größe der vtable in Ihren optimierten Builds nicht beeinträchtigt.

Zweitens: Da die abgeleitete Klasse, die **winrt::make** verwendet, `final` ist, erfolgt jede Devirtualisierung, die der Optimierer möglicherweise ableiten kann, auch dann, wenn Sie sich zuvor entschieden haben, die Implementierungsklasse nicht als `final` zu markieren. Das stellt also eine Verbesserung dar. Der Nachteil ist, dass Ihre Implementierung *nicht*`final` sein kann. Dies hat wiederum keine Folgen, da der instanziierte Typ immer `final` ist.

Drittens hindert Sie nichts daran, virtuelle Funktionen in ihrer Implementierung als `final` zu markieren. C++/WinRT unterscheidet sich natürlich stark von klassischem COM und Implementierungen wie WRL, bei denen alles an Ihrer Implementierung eher virtuell ist. In C++/WinRT ist die virtuelle Verteilung auf die ABI (Application Binary Interface) beschränkt (die immer `final` ist), und ihre Implementierungsmethoden stützen sich auf die Kompilierzeit oder statische Polymorphie. Dadurch lässt sich eine unnötige Laufzeitpolymorphie vermeiden, und dies bedeutet auch, dass es kaum einen Grund für virtuelle Funktionen in Ihrer C++/WinRT-Implementierung gibt. Das ist ein klarer Vorteil und führt zu besser vorhersagbarem Inlining.

Viertens: Da **winrt::make** eine abgeleitete Klasse einfügt, darf Ihre Implementierung keinen privaten Destruktor besitzen. Private Destruktoren waren bei klassischen COM-Implementierungen beliebt, da ebenfalls alles virtuell war und üblicherweise unformatierte Zeiger direkt behandelt wurden. Daher war es nicht weiter schwierig, versehentlich `delete` anstelle von [**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) aufzurufen. Bei C++/WinRT wurden alle möglichen Vorkehrungen getroffen, um die direkte Behandlung von unformatierten Zeigern zu erschweren. Und Sie müssen sich *wirklich* Mühe geben, um einen unformatierten Zeiger in C++/WinRT zu erstellen, für den `delete` aufgerufen werden könnte. Wertsemantik bedeutet, dass Sie mit Werten und Verweisen, nicht jedoch mit Zeigern arbeiten.

C++/WinRT verlangt also ein Umdenken gegenüber dem klassischen COM-Code. Das ist durchaus vernünftig, da WinRT eben kein klassisches COM ist. Klassisches COM ist die Assemblysprache der Windows-Runtime. Das ist kein Code, in dem Sie täglich schreiben sollten. Stattdessen legt C++/WinRT Code nahe, der eher modernem C++ als klassischem COM ähnelt.

## <a name="important-apis"></a>Wichtige APIs
* [winrt::make function template](/uwp/cpp-ref-for-winrt/make) (Funktionsvorlage „winrt::make“)
* [winrt::make_self-Funktionsvorlage](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)