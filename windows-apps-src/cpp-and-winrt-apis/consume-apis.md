---
description: In diesem Thema wird gezeigt, wie vom Benutzer selbst, von Windows oder von Drittanbietern implementierte C++/WinRT-APIs verwendet werden.
title: Nutzen von APIs mit C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, projiziert, Projektion, Implementierung, Laufzeitklasse, Aktivierung
ms.localizationpriority: medium
ms.openlocfilehash: e6bf1e7fb32533aa9d7b865ac7c8afc374290e54
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360349"
---
# <a name="consume-apis-with-cwinrt"></a>Nutzen von APIs mit C++/WinRT

In diesem Thema wird die Nutzung von [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)-APIs veranschaulicht. Dabei spielt es keine Rolle, ob diese APIs zu Windows gehören oder von einem Drittanbieter oder von dir selbst implementiert wurden.

## <a name="if-the-api-is-in-a-windows-namespace"></a>API in einem Windows-Namespace
Dies ist der gängigste Anwendungsfall für die Nutzung einer Windows-Runtime-API. Für jeden Typ in einem Windows-Namespace, der in den Metadaten definiert ist, definiert C++/WinRT ein für C++ geeignetes Äquivalent (den *projizierten Typ*). Ein projizierter Typ besitzt den gleichen vollqualifizierten Namen wie der Windows-Typ, wird jedoch unter Verwendung der C++-Syntax im C++-Namespace **winrt** platziert. [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) wird beispielsweise in C++/WinRT als **winrt::Windows::Foundation::Uri** projiziert.

Hier siehst du ein einfaches Codebeispiel: Wenn du die folgenden Codebeispiele kopieren und direkt in die Hauptquellcodedatei eines Projekts vom Typ **Windows-Konsolenanwendung (C++/WinRT)** einfügen möchtest, musst du in den Projekteigenschaften zuerst die Option **Vorkompilierte Header nicht verwenden** festlegen.

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

Der enthaltene Header `winrt/Windows.Foundation.h` ist Teil des SDK und befindet sich im Ordner `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Die Header in diesem Ordner enthalten für C++/WinRT projizierte Windows-Namespacetypen. In diesem Beispiel enthält `winrt/Windows.Foundation.h` den Typ **winrt::Windows::Foundation::Uri**. Dies ist der projizierte Typ für die Laufzeitklasse [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Wenn du einen Typ aus einem Windows-Namespace verwenden möchtest, musst du den C++/WinRT-Header einschließen, der diesem Namespace entspricht. Die `using namespace`-Anweisungen sind optional, aber praktisch.

Im obigen Codebeispiel wird nach der Initialisierung von C++/WinRT ein Wert des projizierten Typs **winrt::Windows::Foundation::Uri** mit Stapelzuordnung über einen seiner öffentlich dokumentierten Konstruktoren (in diesem Beispiel [**Uri(String)** ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_)) zugewiesen. Für diesen häufigsten Anwendungsfall sind in der Regel keine weiteren Schritte erforderlich. Projizierte C++/WinRT-Typwerte können wie eine Instanz des tatsächlichen Windows-Runtime-Typs behandelt werden, da sie über genau die gleichen Member verfügen.

Der projizierte Wert ist genau genommen ein Proxy und im Grunde lediglich ein intelligenter Zeiger auf ein Unterstützungsobjekt. Die Konstruktoren des projizierten Werts rufen [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) auf, um eine Instanz der zugrunde liegenden Windows-Runtime-Klasse (in diesem Fall **Windows.Foundation.Uri**) zu erstellen und die Standardschnittstelle dieses Objekts im neuen projizierten Wert zu speichern. Wie unten zu sehen werden die Aufrufe der Member des projizierten Werts tatsächlich über den intelligenten Zeiger an das Unterstützungsobjekt delegiert. Hier erfolgen die Zustandsänderungen.

![Der projizierte Typ „Windows::Foundation::Uri“](images/uri.png)

Wenn der Wert `contosoUri` außerhalb des zulässigen Bereichs liegt, wird er gelöscht und gibt den Verweis auf die Standardschnittstelle frei. Ist dieser Verweis der letzte Verweis auf das Windows-Runtime-Unterstützungsobjekt **Windows.Foundation.Uri**, wird auch das Unterstützungsobjekt gelöscht.

> [!TIP]
> Ein *projizierter Typ* ist ein Wrapper für eine Laufzeitklasse, um deren APIs nutzen zu können. Eine *projizierte Schnittstelle* ist ein Wrapper für eine Windows-Runtime-Schnittstelle.

## <a name="cwinrt-projection-headers"></a>C++/WinRT-Projektionsheader
Um Windows-Namespace-APIs über C++/WinRT nutzen zu können, müssen Header aus dem Ordner `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` eingeschlossen werden. Üblicherweise verweist ein Typ in einem untergeordneten Namespace auf Typen in seinem unmittelbar übergeordneten Namespace. Folglich beinhaltet jeder C++/WinRT-Projektionsheader automatisch seine übergeordnete Namespace-Headerdatei, sodass sie nicht explizit eingeschlossen werden *muss*. Es tritt aber auch kein Fehler auf, wenn du sie einschließt.

Ein Beispiel: Für den Namespace [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates) befinden sich die entsprechenden C++/WinRT-Typdefinitionen in `winrt/Windows.Security.Cryptography.Certificates.h`. Typen in **Windows::Security::Cryptography::Certificates** benötigen Typen im übergeordneten Namespace **Windows::Security::Cryptography**, und Typen in diesem Namespace benötigen unter Umständen Typen in ihrem eigenen übergeordneten Namespace (**Windows::Security**).

Wenn du also `winrt/Windows.Security.Cryptography.Certificates.h` einschließt, enthält diese Datei wiederum `winrt/Windows.Security.Cryptography.h`, und `winrt/Windows.Security.Cryptography.h` enthält `winrt/Windows.Security.h`. Da kein `winrt/Windows.h` vorhanden ist, ist an dieser Stelle Schluss. Dieses transitive Einschlussverfahren endet beim Namespace auf zweiter Ebene.

Das Verfahren schließt transitiv die Headerdateien ein, die die erforderlichen *Deklarationen* und *Implementierungen* für die Klassen bereitstellen, die in übergeordneten Namespaces definiert sind.

Ein Member eines Typs in einem Namespace kann auf einen oder mehrere Typen in anderen (nicht verbundenen) Namespaces verweisen. Damit der Compiler diese Memberdefinitionen erfolgreich kompilieren kann, benötigt er die Typdeklarationen für den Abschluss dieser Typen. Folglich umfasst jeder C++/WinRT-Projektionsheader die Namespaceheader, die er benötigt, um alle abhängigen Typen zu *deklarieren*. Im Gegensatz zu übergeordneten Namespaces werden bei diesem Verfahren die *Implementierungen* für die referenzierten Typen *nicht* im Pull-Verfahren abgerufen.

> [!IMPORTANT]
> Wenn du einen in einem nicht verbundenen Namespace deklarierten Typ tatsächlich *verwenden* möchtest (Instanziieren, Aufrufen von Methoden usw.), musst du die entsprechende Namespace-Headerdatei für diesen Typ einschließen. Nur *Deklarationen* sind automatisch eingeschlossen (*Implementierungen* nicht).

Wenn du also beispielsweise nur `winrt/Windows.Security.Cryptography.Certificates.h` einschließt, werden Deklarationen aus diesen Namespaces (und so weiter, transitiv) abgerufen.

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

Anders ausgedrückt: Einige APIs sind in einem von dir eingeschlossenen Header vorwärts deklariert. Ihre Definitionen befinden sich aber in einem Header, den du noch nicht eingeschlossen hast. Wenn du nun also [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri) aufrufst, erhältst du einen Linker-Fehler mit dem Hinweis, dass der Member nicht definiert ist. Die Lösung besteht in einem expliziten `#include <winrt/Windows.Foundation.h>`. Bei einem Linker-Fehler wie diesem empfiehlt es sich grundsätzlich, den nach dem API-Namespace benannten Header einzuschließen und den Buildvorgang zu wiederholen.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>Zugreifen auf Member über das Objekt, eine Schnittstelle oder die ABI
Mit der C++/WinRT-Projektion handelt es sich bei der Laufzeitdarstellung einer Windows-Runtime-Klasse um nichts weiter als um die zugrunde liegenden ABI-Schnittstellen. Der Einfachheit halber kannst du für Klassen aber so programmieren, wie dies vom jeweiligen Autor vorgesehen war. So kannst du beispielsweise die Methode **ToString** eines [**URI**](/uwp/api/windows.foundation.uri) so aufrufen, als ob es sich um eine Methode der Klasse handelt. (Tatsächlich ist es jedoch eine Methode der separaten Schnittstelle **IStringable**.)

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

Dies wird über eine Abfrage für die entsprechende Schnittstelle erreicht. Du hast aber stets die Kontrolle. Du kannst etwas Komfort für etwas mehr Leistung opfern, indem du die IStringable-Schnittstelle selbst abrufst und direkt verwendest. Im folgenden Codebeispiel wird ein tatsächlicher IStringable-Schnittstellenzeiger (über eine einmalige Abfrage) zur Laufzeit abgerufen. Danach wird **ToString** direkt aufgerufen, und weitere Aufrufe von **QueryInterface** werden vermieden:

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

Diese Technik kannst du verwenden, wenn du weißt, dass du mehrere Methoden für die gleiche Schnittstelle aufrufst.

Wenn du außerdem auf Member auf der ABI-Ebene zugreifen möchtest, ist auch das möglich. Wie das geht, siehst du im folgenden Codebeispiel. Weitere Informationen und Codebeispiele findest du unter [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md).

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
Selbst der Standardkonstruktor eines projizierten Typs bewirkt die Erstellung eines Windows-Runtime-Unterstützungsobjekts. Es ist aber auch möglich, eine Variable eines projizierten Typs zu erstellen, ohne dass dadurch ein Windows-Runtime-Objekt erstellt wird (und diese Arbeit auf einen späteren Zeitpunkt zu verschieben). Deklariere die Variable oder das Feld mithilfe des speziellen C++/WinRT-Konstruktors `nullptr_t` des projizierten Typs.

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

Alle Konstruktoren des projizierten Typs (*ausgenommen* der Konstruktor `nullptr_t`) bewirken die Erstellung eines Windows-Runtime-Unterstützungsobjekts. Dem Konstruktor `nullptr_t` ist im Wesentlichen keine Aktion zugeordnet. Er erwartet, dass das projizierte Objekt zu einem späteren Zeitpunkt initialisiert wird. Du kannst dieses Verfahren also für eine effiziente verzögerte Initialisierung verwenden – unabhängig davon, ob eine Laufzeitklasse über einen Standardkonstruktor verfügt oder nicht.

Diese Überlegung betrifft auch andere Orte, an denen der Standardkonstruktor aufgerufen wird (etwa Vektoren und Zuordnungen). Sieh dir das folgende Codebeispiel an (Projekt vom Typ **Leere App (C++/WinRT)** erforderlich):

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

Die Zuweisung erstellt einen neuen Textblock (**TextBlock**) und überschreibt ihn sofort mit `value`. Hier ist die Lösung:

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>In einer Komponente für Windows-Runtime implementierte API
Dieser Abschnitt gilt unabhängig davon, ob du die Komponente selbst erstellt hast oder ob sie von einem Anbieter stammt.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Verweise in deinem Anwendungsprojekt auf die Windows-Runtime-Metadatendatei (`.winmd`) der Komponente für Windows-Runtime, und führe den Buildvorgang aus. Im Zuge des Buildvorgangs generiert das Tool `cppwinrt.exe` eine C++-Standardbibliothek, die die API-Oberfläche der Komponente vollständig beschreibt (bzw. *projiziert*). Anders ausgedrückt: Die generierte Bibliothek enthält die projizierten Typen für die Komponente.

Anschließend wird genau wie bei einem Windows-Namespacetyp ein Header eingeschlossen und der projizierte Typ über einen seiner Konstruktoren konstruiert. Der Startcode deines Anwendungsprojekts registriert die Laufzeitklasse, und der Konstruktor des projizierten Typs ruft [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) auf, um die Laufzeitklasse der referenzierten Komponente zu aktivieren.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Ausführlichere Informationen, Code und eine exemplarische Vorgehensweise für die Nutzung von APIs, die in einer Komponente für Windows-Runtime implementiert sind, findest du unter [Erstellen von Ereignissen in C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>Im verwendenden Projekt implementierte API
Ein Typ, der über die XAML-UI genutzt wird, muss eine Laufzeitklasse sein (auch wenn er sich im gleichen Projekt wie der XAML-Code befindet).

Für dieses Szenario wird auf der Grundlage der Windows-Runtime-Metadaten der Laufzeitklasse (`.winmd`) ein projizierter Typ generiert. Auch hier wird ein Header eingeschlossen. Diesmal wird der projizierte Typ allerdings über den zugehörigen Konstruktor vom Typ `nullptr` konstruiert. Dieser Konstruktor führt keine Initialisierung durch. Daher musst du der Instanz über die Hilfsfunktion [**winrt::make**](/uwp/cpp-ref-for-winrt/make) einen Wert zuweisen und alle notwendigen Konstruktorargumente übergeben. Eine Laufzeitklasse, die im gleichen Projekt implementiert wird wie der verwendende Code, muss weder registriert noch per Windows-Runtime/COM-Aktivierung instanziiert werden.

Für das folgende Codebeispiel wird ein Projekt vom Typ **Leere App (C++/WinRT)** benötigt:

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

Ausführlichere Informationen, Code und eine exemplarische Vorgehensweise für die Nutzung einer im verwendenden Projekt implementierten Laufzeitklasse findest du unter [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Instanziieren und Zurückgeben projizierter Typen und Schnittstellen
Das folgende Beispiel zeigt, wie projizierte Typen und Schnittstellen in deinem verwendenden Projekt aussehen können. Vergiss nicht, dass ein projizierter Typ (wie in diesem Beispiel) von einem Tool generiert und nicht von dir selbst erstellt wird.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** ist ein projizierter Typ. Projizierte Schnittstellen sind **IMyRuntimeClass**, **IStringable** und **IClosable**. In diesem Thema wurden die verschiedenen Methoden gezeigt, mit denen du einen projizierten Typ instanziieren kannst. Im Anschluss findest du eine Zusammenfassung anhand des Beispiels **MyRuntimeClass**.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- Du kannst auf die Member aller Schnittstellen eines projizierten Typs zugreifen.
- Du kannst einen projizierten Typ an einen Aufrufer zurückgeben.
- Projizierte Typen und Schnittstellen werden von [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) abgeleitet. Du kannst also [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) für einen projizierten Typ oder für eine projizierte Schnittstelle aufrufen, um nach anderen projizierten Schnittstellen zu suchen, die du ebenfalls verwenden oder an einen Aufrufer zurückgeben kannst. Die Memberfunktion **as** funktioniert wie [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)).

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
Im Anschluss wird die einfache, direkte Methode für die Erstellung eines C++/WinRT-Objekts beschrieben:

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

Manchmal möchtest du aber vielleicht die Aktivierungsfactory selbst erstellen und anschließend auf deren Grundlage Objekte erstellen. Hier sind einige entsprechende Beispiele unter Verwendung der Funktionsvorlage [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory):

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

Bei den Klassen in den beiden obigen Beispielen handelt es sich um Typen aus einem Windows-Namespace. Im nächsten Beispiel ist **BankAccountWRC::BankAccount** ein benutzerdefinierter Typ, der in einer Komponente für Windows-Runtime implementiert ist:

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="important-apis"></a>Wichtige APIs
* [Schnittstelle „QueryInterface“](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Funktion „RoActivateInstance“](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Klasse „Windows::Foundation::Uri“](/uwp/api/windows.foundation.uri)
* [Funktionsvorlage „winrt::get_activation_factory“](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [Funktionsvorlage „winrt::make“](/uwp/cpp-ref-for-winrt/make)
* [Struktur „winrt::Windows::Foundation::IUnknown“](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von Ereignissen in C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md)
* [Einführung in C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
