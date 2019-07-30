---
description: In diesem Thema wird gezeigt, wie vom Benutzer selbst, von Windows oder von Drittanbietern implementierte C++/WinRT-APIs verwendet werden.
title: Nutzen von APIs mit C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, projiziert, Projektion, Implementierung, Laufzeitklasse, Aktivierung
ms.localizationpriority: medium
ms.openlocfilehash: 5a3d4b554fafeb2053e4e6af831c224b5eacd151
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328868"
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

`WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

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

    int port{ contosoUri.Port() }; // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri{
        contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>() };
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>Verzögerte Initialisierung

In C++/WinRT verfügt jeder projizierte Typ über einen speziellen C++/WinRT-Konstruktor: **std::nullptr_t**. Abgesehen von dieser einen Ausnahme führen alle Konstruktoren eines projizierten Typs – einschließlich des Standardkonstruktors – zur Erstellung eines Windows-Runtime-Sicherungsobjekts und stellen einen intelligenten Zeiger darauf bereit. Diese Regel gilt also überall dort, wo der Standardkonstruktor verwendet wird, z.B. bei nicht initialisierten lokalen Variablen, nicht initialisierten globalen Variablen und nicht initialisierten Membervariablen.

Es ist aber auch möglich, eine Variable eines projizierten Typs zu erstellen, ohne dass dadurch ein Windows-Runtime-Sicherungsobjekt erstellt wird (und diese Arbeit auf einen späteren Zeitpunkt zu verschieben). Deklariere die Variable oder das Feld mit diesem speziellen C++/WinRT-Konstruktor **std::nullptr_t** (den die C++/WinRT-Projektion in jede Laufzeitklasse einfügt). Wir verwenden diesen speziellen Konstruktor mit *m_gamerPicBuffer* im folgenden Codebeispiel.

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

Alle Konstruktoren des projizierten Typs (*mit Ausnahme* des **std::nullptr_t**-Konstruktors) bewirken die Erstellung eines Windows-Runtime-Unterstützungsobjekts. Dem **std::nullptr_t**-Konstruktor ist im Wesentlichen keine Aktion zugeordnet. Er erwartet, dass das projizierte Objekt zu einem späteren Zeitpunkt initialisiert wird. Du kannst dieses Verfahren also für eine effiziente verzögerte Initialisierung verwenden – unabhängig davon, ob eine Laufzeitklasse über einen Standardkonstruktor verfügt oder nicht.

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

Siehe auch [Auswirkungen des Standardkonstruktors auf Sammlungen](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#how-the-default-constructor-affects-collections).

### <a name="dont-delay-initialize-by-mistake"></a>Verwenden Sie nicht irrtümlicherweise die verzögerte Initialisierung

Rufen Sie den **std::nullptr_t**-Konstruktor nicht versehentlich auf. Bei der Konfliktauflösung durch den Compiler wird er vor den Factorykonstruktoren bevorzugt. Betrachten Sie beispielsweise diese beiden Laufzeitklassendefinitionen.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox();
}

// Gift.idl
runtimeclass Gift
{
    Gift(GiftBox giftBox); // You can create a gift inside a box.
}
```

Angenommen, Sie möchten ein Geschenk (**Gift**) erstellen, das sich nicht in einem Geschenkkarton befindet (ein mit einer nicht initialisierten **GiftBox** erstelltes **Gift**). Betrachten wir zunächst die *falsche* Vorgehensweise. Wir wissen, dass ein **Gift**-Konstruktor vorhanden ist, der eine **GiftBox** akzeptiert. Falls wir jedoch für **GiftBox** einen NULL-Wert übergeben (den **Gift**-Konstruktor über eine einheitliche Initialisierung aufrufen, wie dies unten geschieht), erhalten wir *nicht* das gewünschte Ergebnis.

```cppwinrt
// These are *not* what you intended. Doing it in one of these two ways
// actually *doesn't* create the intended backing Windows Runtime Gift object;
// only an empty smart pointer.

Gift gift{ nullptr };
auto gift{ Gift(nullptr) };
```

Sie erhalten stattdessen ein nicht initialisiertes **Gift**. Sie erhalten kein **Gift** mit einer nicht initialisierten **GiftBox**. Hier ist die *richtige* Vorgehensweise.

```cppwinrt
// Doing it in one of these two ways creates an initialized
// Gift with an uninitialized GiftBox.

Gift gift{ GiftBox{ nullptr } };
auto gift{ Gift(GiftBox{ nullptr }) };
```

Im Beispiel für die falsche Vorgehensweise erfolgt die Auflösung durch Übergabe eines `nullptr`-Literals zugunsten des Konstruktors mit verzögerter Initialisierung. Damit die Auflösung zugunsten des Factorykonstruktors erfolgt, muss der Typ des Parameters **GiftBox** lauten. Sie haben weiterhin die Möglichkeit, eine **GiftBox** mit explizit verzögerter Initialisierung zu übergeben, wie im Beispiel für die richtige Vorgehensweise gezeigt.

Das folgende Beispiel zeigt eine *ebenfalls* richtige Vorgehensweise, weil der Typ des Parameters „GiftBox“ und nicht **std::nullptr_t** lautet.

```cppwinrt
GiftBox giftBox{ nullptr };
Gift gift{ giftBox }; // Calls factory constructor.
```

Mehrdeutigkeit entsteht nur, wenn Sie ein `nullptr`-Literal übergeben.

## <a name="dont-copy-construct-by-mistake"></a>Verwenden Sie nicht irrtümlicherweise einen Kopierkonstruktor

Dieser Hinweis ähnelt dem Hinweis im obigen Abschnitt [Verwenden Sie nicht irrtümlicherweise die verzögerte Initialisierung](#dont-delay-initialize-by-mistake).

C++/WinRT-Projektion fügt zusätzlich zum Konstruktor mit verzögerter Initialisierung auch einen Kopierkonstruktor in jede Laufzeitklasse ein. Dies ist ein Konstruktor mit einem einzelnen Parameter, der den gleichen Typ wie das zu erstellende Objekt akzeptiert. Der resultierende intelligente Zeiger verweist auf das Windows-Runtime-Unterstützungsobjekt, auf das auch sein Konstruktorparameter verweist. Das Ergebnis sind zwei intelligente Zeiger-Objekte, die auf das gleiche Unterstützungsobjekt verweisen.

In den Codebeispielen wird die folgende Laufzeitklassendefinition verwendet.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox(GiftBox biggerBox); // You can place a box inside a bigger box.
}
```

Angenommen, wir möchten eine **GiftBox** in einer größeren **GiftBox** erstellen.

```cppwinrt
GiftBox bigBox{ ... };

// These are *not* what you intended. Doing it in one of these two ways
// copies bigBox's backing-object-pointer into smallBox.
// The result is that smallBox == bigBox.

GiftBox smallBox{ bigBox };
auto smallBox{ GiftBox(bigBox) };
```

Die *richtige* Vorgehensweise ist das explizite Aufrufen der Aktivierungsfactory.

```cppwinrt
GiftBox bigBox{ ... };

// These two ways call the activation factory explicitly.

GiftBox smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
auto smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
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

Für dieses Szenario wird auf der Grundlage der Windows-Runtime-Metadaten der Laufzeitklasse (`.winmd`) ein projizierter Typ generiert. Auch hier wird ein Header eingeschlossen. Diesmal wird der projizierte Typ allerdings über den zugehörigen Konstruktor vom Typ **std::nullptr_t** konstruiert. Dieser Konstruktor führt keine Initialisierung durch. Daher musst du der Instanz über die Hilfsfunktion [**winrt::make**](/uwp/cpp-ref-for-winrt/make) einen Wert zuweisen und alle notwendigen Konstruktorargumente übergeben. Eine Laufzeitklasse, die im gleichen Projekt implementiert wird wie der verwendende Code, muss weder registriert noch per Windows-Runtime/COM-Aktivierung instanziiert werden.

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

## <a name="membertype-ambiguities"></a>Mehrdeutigkeiten bei Membern/Typen

Wenn eine Memberfunktion den gleichen Namen hat wie ein Typ, entsteht Mehrdeutigkeit. Die Regeln für eine Suche nach nicht qualifizierten C++-Namen in Memberfunktionen führen dazu, dass zuerst in der Klasse und dann erst in Namespaces gesucht wird. Die Regel *substitution failure is not an error* (SFINAE) wird nicht angewendet (sie wird während der Überladungsauflösung von Funktionsvorlagen angewendet). Wenn also der Name innerhalb der Klasse nicht sinnvoll ist, sucht der Compiler nicht weiter nach einem besseren Treffer, sondern meldet einfach einen Fehler.

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // This doesn't compile. You get the error
        // "'winrt::Windows::Foundation::IUnknown::as':
        // no matching overloaded function found".
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Style>() };
    }
}
```

Im obigen Beispiel geht der Compiler davon aus, dass [**FrameworkElement.Style()** ](/uwp/api/windows.ui.xaml.frameworkelement.style) (eine Memberfunktion in C++/WinRT) als Vorlagenparameter an [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) übergeben wird. Die Lösung hierfür besteht darin, die Interpretation des Namens `Style` als Typ [**Windows::UI::Xaml::Style**](/uwp/api/windows.ui.xaml.style) zu erzwingen.

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // One option is to fully-qualify it.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Windows::UI::Xaml::Style>() };

        // Another is to force it to be interpreted as a struct name.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<struct Style>() };

        // If you have "using namespace Windows::UI;", then this is sufficient.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Xaml::Style>() };

        // Or you can force it to be resolved in the global namespace (into which
        // you imported the Windows::UI::Xaml namespace when you did
        // "using namespace Windows::UI::Xaml;".
        auto style = Application::Current().Resources().
            Lookup(L"MyStyle").as<::Style>();
    }
}
```

Bei der Suche nach nicht qualifizierten Namen gibt es eine spezielle Ausnahme, wenn `::` auf den Namen folgt – in diesem Fall werden Funktionen, Variablen und Enumerationswerte ignoriert. Damit kannst du beispielsweise Folgendes ausführen.

```cppwinrt
struct MyPage : Page
{
    void DoSomething()
    {
        Visibility(Visibility::Collapsed); // No ambiguity here (special exception).
    }
}
```

Der Aufruf von `Visibility()` wird in den Memberfunktionsnamen von [**UIElement.Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) aufgelöst. Aber im Parameter `Visibility::Collapsed` folgt `::` auf das Wort `Visibility`. Daher wird der Methodenname ignoriert, und der Compiler findet die Enumerationsklasse.

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
