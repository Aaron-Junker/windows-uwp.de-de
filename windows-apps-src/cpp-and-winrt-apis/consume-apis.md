---
description: Dieses Thema zeigt, wie man selbst, von Windows oder von Drittanbietern implementierte C++/WinRT-APIs verwendet.
title: Verwenden von APIs mit C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projiziert, projektion, implementierung, laufzeitklasse, aktivierung
ms.localizationpriority: medium
ms.openlocfilehash: e6bf1e7fb32533aa9d7b865ac7c8afc374290e54
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360349"
---
# <a name="consume-apis-with-cwinrt"></a>Verwenden von APIs mit C++/WinRT

In diesem Thema wird gezeigt, wie [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) -APIs, unabhängig davon, ob sie Teil von Windows, von einem Komponentenanbieter für Drittanbieter-implementiert oder selbst implementiert.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Wenn sich die API in einem Windows-Namespace befindet
Dies ist der häufigste Fall, bei dem Sie eine Windows-Runtime-API verwenden. Für jeden Typ in einem Windows-Namespace, der in den Metadaten festgelegt ist, definiert C++/WinRT ein C++-freundliches Äquivalent (den *projizierten Typ*). Ein projizierter Typ verfügt über den gleichen vollqualifizierten Namen wie der Windows-Typ, wird unter Verwendung der C++-Syntax jedoch im C++-**winrt**-Namespace abgelegt. Beispielsweise wird [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) in C++/WinRT als **winrt::Windows::Foundation::Uri** projiziert.

Es folgt ein einfaches Codebeispiel. Wenn Sie möchten die kopieren und Einfügen von den folgenden Codebeispielen direkt in der Codedatei main Quelle des eine **Windows-Konsolenanwendung (C++"/ WinRT")** -Projekt, und die erste Gruppe **vorkompilierte Header nicht verwenden** in den Projekteigenschaften.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
    Uri combinedUri = contosoUri.CombineUri(L"products");
}
```

Der enthaltene Header `winrt/Windows.Foundation.h` ist Teil des SDKs und befindet sich im Ordner `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Die Header in diesem Ordner enthalten Windows-Namespace-Typen, die in C++/WinRT projiziert werden. In diesem Beispiel enthält `winrt/Windows.Foundation.h` den Typ **winrt::Windows::Foundation::Uri**, der der projizierte Typ für die Laufzeitklasse [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) ist.

> [!TIP]
> Wenn Sie einen Typ aus einem Windows-Namespace verwenden möchten, fügen Sie den C++/WinRT-Header ein, der diesem Namespace entspricht. Die `using namespace`-Direktiven sind optional, aber praktisch.

Im obigen Codebeispiel wird nach der Initialisierung von C++/WinRT ein Wert des projizierten Typs **winrt::Windows::Foundation::Uri** mit Stapelzuordnung über einen seiner öffentlich dokumentierten Konstruktoren (in diesem Beispiel [**Uri(String)** ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_)) zugewiesen. Für diesen häufigsten Anwendungsfall müssen Sie in der Regel nichts weiter tun. Wenn Sie einen projizierten C++/WinRT-Typwert haben, können Sie diesen behandeln, als wäre er eine Instanz des tatsächlichen Windows-Runtime-Typs, da er über die gleichen Mitglieder verfügt.

Tatsächlich ist dieser projizierte Wert ein Proxy; im Grunde ist er nur ein intelligenter Zeiger auf ein zugrunde liegendes Objekt. Die Konstruktoren des projizierten Werts rufen [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) auf, um eine Instanz der zugrunde liegenden Windows-Runtime-Klasse (in diesem Fall **Windows.Foundation.Uri**) zu erstellen und die Standardschnittstelle dieses Objekts im neuen projizierten Wert zu speichern. Wie unten dargestellt, Delegaten Aufrufen der projizierte Wert Member tatsächlich, über den intelligenten Zeiger, auf das dahinter liegende-Objekt. Das ist, in denen Änderungen des Ansichtszustands auftreten.

![Der projizierte Windows::Foundation::Uri-Typ](images/uri.png)

Wenn der `contosoUri`-Wert außerhalb des Bereichs liegt, wird er zerstört und gibt seinen Verweis zur Standardschnittstelle frei. Wenn dieser Verweis der letzte Verweis auf das zugrunde liegende Windows-Runtime-Objekt **Windows.Foundation.Uri** ist, wird auch das zugrunde liegende Objekt zerstört.

> [!TIP]
> Ein *projizierter Typ* ist ein Wrapper für eine Laufzeitklasse, um deren APIs zu nutzen. Eine *projizierte Schnittstelle* ist ein Wrapper für eine Windows-Runtime-Schnittstelle.

## <a name="cwinrt-projection-headers"></a>C++/WinRT-Projektionsheader
Um Windows-Namespace-APIs von C++/WinRT zu verwenden, schließen Sie Header aus dem Ordner `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` ein. Üblicherweise verweist ein Typ in einem untergeordneten Namespace auf Typen in seinem unmittelbar übergeordneten Namespace. Folglich schließt jeder C++/WinRT-Projektionsheader automatisch seine übergeordnete Namespace-Headerdatei ein, sodass Sie sie nicht explizit einschließen *müssen*. Aber auch wenn Sie dies tun, tritt kein Fehler auf.

Beispielsweise befinden sich für den Namespace [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates) die entsprechenden C++/WinRT-Typdefinitionen in `winrt/Windows.Security.Cryptography.Certificates.h`. Die Typen in **Windows::Security::Cryptography::Certificates** erfordern Typen im übergeordneten Namespace **Windows::Security::Cryptography**, und die Typen in diesem Namespace könnten Typen in ihrem eigenen übergeordneten Namespace **Windows::Security** benötigen.

Wenn Sie also `winrt/Windows.Security.Cryptography.Certificates.h` einschließen, enthält diese Datei wiederum `winrt/Windows.Security.Cryptography.h` und `winrt/Windows.Security.Cryptography.h` enthält `winrt/Windows.Security.h`. Hier ist der Trail dann beendet, da es kein `winrt/Windows.h` gibt. Dieses transitive Einschlussverfahren endet beim Namespace auf zweiter Ebene.

Das Verfahren schließt transitiv die Headerdateien ein, die die erforderlichen *Deklarationen* und *Implementierungen* für die Klassen bereitstellen, die in übergeordneten Namespaces definiert sind.

Ein Mitglied eines Typs in einem Namespace kann auf einen oder mehrere Typen in anderen nicht verbundenen Namespaces verweisen. Damit der Compiler diese Mitglieddefinitionen erfolgreich kompilieren kann, muss der Compiler die Typdeklarationen für das Schließen aller dieser Typen anzeigen können. Folglich umfasst jeder C++/WinRT-Projektionsheader die Namespace-Header, die er braucht, um alle abhängigen Typen zu *deklarieren*. Im Gegensatz zu übergeordneten Namespaces werden bei diesem Verfahren *die Implementierungen* für die referenzierten Typen *nicht* im Pull-Verfahren abgerufen.

> [!IMPORTANT]
> Möchten Sie tatsächlich einen Typ *verwenden* (Instanziieren, Aufrufen von Methoden usw.), der in einem nicht verbundenen Namespace deklariert ist, müssen Sie die entsprechende Namespace-Headerdatei für diesen Typ einschließen. Nur *Deklarationen*, nicht *Implementierungen*, sind automatisch eingeschlossen.

Wenn Sie beispielsweise nur `winrt/Windows.Security.Cryptography.Certificates.h` einschließen, führt dies dazu, dass die Deklarationen von diesen Namespaces (und so weiter, transitiv) abgerufen werden.

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

Anders ausgedrückt: Einige APIs sind in einem Header forward-deklariert, den Sie eingeschlossen haben. Ihre Definitionen befinden sich aber in einem Header, den Sie noch nicht eingeschlossen haben. Wenn Sie also dann [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri) aufrufen, erhalten Sie eine Linker-Fehlermeldung, die darauf hinweist, dass das Mitglied nicht definiert ist. Die Lösung besteht in einem expliziten `#include <winrt/Windows.Foundation.h>`. Wenn Sie einen Linkerfehler wie diesen sehen, schließen Sie den Header ein, der für den API-Namespace benannt ist, und wiederholen Sie die Erstellung.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>Zugreifen auf Mitglieder über das Objekt, eine Schnittstelle oder die ABI
Mit der C++/WinRT-Projektion ist die Laufzeitdarstellung einer Windows-Runtime-Klasse nicht mehr als die zugrunde liegenden ABI-Schnittstellen. Der Einfachheit halber können Sie Code für die Klassen aber auf eine Weise ausführen, die der jeweilige Autor beabsichtigte. Rufen Sie z. B. die **ToString**-Methode eines [**Uri**](/uwp/api/windows.foundation.uri) auf, als handelte es sich um eine Methode der Klasse (tatsächlich ist es eine Methode auf der separaten **IStringable**-Schnittstelle).

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

Sie erreichen dies über eine Abfrage für die entsprechende Schnittstelle. Sie haben aber immer die Kontrolle. Sie können ein wenig Komfort gegen etwas mehr Leistung eintauschen – indem Sie die IStringable-Schnittstelle selbst abrufen und direkt verwenden. Im folgenden Codebeispiel erhalten Sie einen tatsächlichen IStringable-Schnittstellenzeiger zur Laufzeit (über eine einmalige Abfrage). Danach ist Ihr Aufruf von **ToString** direkter Art und verhindert alle weiteren Aufrufe an **QueryInterface**.

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

Sie können diese Technik wählen, wenn Sie wissen, dass Sie mehrere Methoden auf der gleichen Schnittstelle aufrufen.

Wenn Sie im Übrigen nicht auf die Mitglieder auf ABI-Ebene zugreifen möchten, ist dies möglich. Im Codebeispiel unten sehen Sie, wie das geht. Sie finden weitere Informationen und Codebeispiele unter [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md).

```cppwinrt
#include <Windows.Foundation.h>
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
using namespace winrt::Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };

    int port = contosoUri.Port(); // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri = contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>();
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>Verzögerte Initialisierung
Selbst der Standardkonstruktor eines projizierten Typs bewirkt, dass ein zugrunde liegendes Windows-Runtime-Objekt erstellt wird. Wenn Sie eine Variable eines projizierten Typs erstellen möchten, ohne dass es wiederum ein Windows-Runtime-Objekt erstellt (sodass Sie diese Arbeit auf einen späteren Zeitpunkt verschieben können), so können Sie das tun. Deklarieren Sie die Variable oder das Feld mit dem speziellen C++/WinRT-Konstruktor `nullptr_t` des projizierten Typs.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

#define MAX_IMAGE_SIZE 1024

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};

int main()
{
    winrt::init_apartment();
    Sample s;
    // ...
    s.DelayedInit();
}
```

Alle Konstruktoren des projizierten Typs, *ausgenommen* der Konstruktor `nullptr_t`, bewirken, dass ein zugrunde liegendes Windows-Runtime-Objekt erstellt wird. Dem Konstruktor `nullptr_t` ist im Wesentlichen keine Aktion zugeordnet. Er erwartet, dass das projizierte Objekt zu einem späteren Zeitpunkt initialisiert wird. Ob eine Laufzeitklasse also über einen Standardkonstruktor verfügt oder nicht, Sie können dieses Verfahren für eine effiziente verzögerte Initialisierung verwenden.

Diese Überlegung wirkt sich auf anderen Orten, in dem Sie den Standardkonstruktor, wie z. B. in Vektoren und Zuordnungen aufrufen können. Betrachten Sie dieses Codebeispiel veranschaulicht, die für die Sie benötigen eine **leere App (C++"/ WinRT")** Projekt.

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

Die Zuweisung erstellt eine neue **TextBlock**, und klicken Sie dann sofort überschrieben mit `value`. Hier ist die Lösung.

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Wenn die API in einer Komponente für Windows-Runtime implementiert ist
Dieser Abschnitt gilt unabhängig davon, ob Sie die Komponente selbst erstellt haben oder ob sie von einem Anbieter stammt.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++WinRT Visual Studio-Erweiterung (VSIX) und das NuGet-Paket (die zusammen bieten die Projektvorlage und Buildunterstützung) finden Sie unter [Visual Studio-Unterstützung für C++"/ WinRT"](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Verweisen Sie in Ihrem Anwendungsprojekt auf die Windows-Runtime-Metadaten-Datei (`.winmd`) der Komponente für Windows-Runtime und erstellen Sie diese. Während des Builds erzeugt das `cppwinrt.exe`-Tool eine Standard-C++ Bibliothek, die die API-Oberfläche der Komponente vollständig beschreibt (bzw. *projiziert*). Mit anderen Worten, die generierte Bibliothek enthält die projizierten Typen für die Komponente.

Dann fügen Sie wie bei einem Windows-Namespace-Typ einen Header ein und konstruieren den projizierten Typ über einen seiner Konstruktoren. Der Startcode Ihres Anwendungsprojekts registriert die Laufzeitklasse, und der Konstruktor des projizierten Typs ruft [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) auf, um die Laufzeitklasse der referenzierten Komponente zu aktivieren.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Weitere Details, Code und eine exemplarische Vorgehensweise bei der Verwendung von APIs, die in einer Komponente für Windows-Runtime implementiert sind, finden Sie unter [Erstellen von Ereignissen in C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>Wenn die API im verwendenden Projekt implementiert ist
Ein Typ, der vom XAML-UI verwendet wird, muss eine Laufzeitklasse sein (auch, wenn er sich im selben Projekt wie das XAML-UI befindet).

Für dieses Szenario generieren Sie einen projizierten Typ aus den Windows-Runtime-Metadaten der Laufzeitklasse (`.winmd`). Auch hier fügen Sie einen Header ein. Aber diesmal konstruieren Sie den projizierten Typ über seinen `nullptr`-Konstruktor. Dieser Konstruktor führt keine Initialisierung durch. Daher müssen Sie der Instanz über die [**winrt::make**](/uwp/cpp-ref-for-winrt/make)-Helper-Funktion einen Wert zuweisen und alle notwendigen Konstruktorargumente übergeben. Eine Laufzeitklasse, die im selben Projekt wie der verwendende Code implementiert ist, muss weder registriert noch über die Windows-Runtime/COM-Aktivierung instanziiert werden.

Sie müssen eine **leere App (C++"/ WinRT")** -Projekt für dieses Codebeispiel.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

Weitere Details, Code und eine exemplarische Vorgehensweise für die Nutzung einer im verwendenden Projekt implementierten Laufzeitklasse finden Sie unter [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Instanziierung und Rückgabe von projizierten Typen und Schnittstellen
Hier ist ein Beispiel dafür, wie projizierte Typen und Schnittstellen in Ihrem Projekt aussehen könnten. Denken Sie daran, dass ein projizierten Typ (wie in diesem Beispiel), ist der Tool-generierte, und nicht etwas, dass Sie selbst erstellen würde.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** ist ein projizierter Typ. Projizierte Schnittstellen sind **IMyRuntimeClass**, **IStringable** und **IClosable**. Dieses Thema hat die verschiedenen Möglichkeiten gezeigt, über die Sie einen projizierten Typ instanziieren können. Hier ist eine Zusammenfassung, die **MyRuntimeClass** als Beispiel verwendet.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- Sie können auf die Mitglieder aller Schnittstellen eines projizierten Typs zugreifen.
- Sie können einen projizierten Typ an einen Aufrufer zurückgeben.
- Projizierte Typen und Schnittstellen sind von [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) abgeleitet. Sie können [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) für einen projizierten Typ oder eine Interface aufrufen, um nach anderen projizierten Schnittstellen zu suchen, die Sie ebenfalls verwenden oder an einen Aufrufer zurückgeben können. Die **as**Mitgliedsfunktion arbeitet wie [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)).

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>Aktivierungsfactorys
Es folgt eine einfache, direkte Methode zur Erstellung eines C++/WinRT-Objekts.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

Es kann jedoch Fälle geben, in denen Sie die Aktivierungsfactory selbst erstellen und dann Objekte daraus anlegen möchten. Es folgen einige Beispiele, die Ihnen zeigen, wie dies mit der Funktionsvorlage [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory) geht.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
auto factory = winrt::get_activation_factory<CurrencyFormatter, ICurrencyFormatterFactory>();
CurrencyFormatter currency = factory.CreateCurrencyFormatterCode(L"USD");
```

```cppwinrt
using namespace winrt::Windows::Foundation;
...
auto factory = winrt::get_activation_factory<Uri, IUriRuntimeClassFactory>();
Uri account = factory.CreateUri(L"http://www.contoso.com");
```

Die Klassen in den beiden obigen Beispielen sind Typen aus einem Windows-Namespace. Im nächsten Beispiel ist **BankAccountWRC::BankAccount** ein benutzerdefinierter Typ, der in einer Komponente für Windows-Runtime implementiert ist.

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="important-apis"></a>Wichtige APIs
* [QueryInterface-Schnittstelle](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [RoActivateInstance-Funktion](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Windows::Foundation::Uri class](/uwp/api/windows.foundation.uri)
* [Vorlage für WinRT::get_activation_factory-Funktion](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [Vorlage für WinRT::Make-Funktion](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von Ereignissen in C++ / WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md)
* [Einführung in C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
