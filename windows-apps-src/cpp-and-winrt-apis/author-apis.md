---
description: Dieses Thema zeigt, wie man C++/WinRT-APIs mit Hilfe der **winrt::implements**-Basisstruktur direkt oder indirekt erstellt.
title: Erstellen von APIs mit C++/WinRT
ms.date: 01/10/2019
ms.topic: article
keywords: Windows 10, uwp, Standard, c++, cpp, winrt, projiziert, Projektion, Implementierung, implementieren, Laufzeitklasse, Aktivierung
ms.localizationpriority: medium
ms.openlocfilehash: e4ca6946df327dbe6697a71d1050e6401ed531fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626665"
---
# <a name="author-apis-with-cwinrt"></a>Erstellen von APIs mit C++/WinRT

In diesem Thema wird gezeigt, wie zum Erstellen von [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) APIs mithilfe der [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) Struktur basieren, entweder direkt oder indirekt. In diesem Zusammenhang werden die Synonyme *Produzieren* oder *Implementieren* für den Begriff *Erstellen* verwendet. Dieses Thema behandelt die folgenden Szenarien für die Implementierung von APIs für einen C++/WinRT-Typ in dieser Reihenfolge.

- Sie erstellen *keine* Windows-Runtime-Klasse (Runtime-Klasse); Sie möchten lediglich eine oder mehrere Windows-Runtime-Schnittstellen für den lokalen Gebrauch in Ihrer App implementieren. Sie leiten sich in diesem Fall direkt von **winrt::implements** ab und implementieren Funktionen.
- Sie schreiben eine *Laufzeitklasse*. Möglicherweise erstellen Sie eine Komponente, die von einer App genutzt werden soll. Oder Sie erstellen einen Typ, der von der XAML-Benutzeroberfläche (UI) genutzt werden soll, und in diesem Fall implementieren und nutzen Sie sowohl eine Laufzeitklasse innerhalb derselben Kompilierungseinheit. In diesen Fällen lassen Sie sich von den Tools Klassen generieren, die von **winrt::implements** abgeleitet sind.

In beiden Fällen wird der Typ, der Ihre C++/WinRT-APIs implementiert, als *Implementierungstyp* bezeichnet.

> [!IMPORTANT]
> Es ist wichtig, das Konzept eines Implementierungstyps von dem eines projektierten Typs zu unterscheiden. Der projektierte Typ ist in [Verwenden von APIs mit C++/WinRT](consume-apis.md) beschrieben.

## <a name="if-youre-not-authoring-a-runtime-class"></a>Wenn Sie *keine* Laufzeitklasse erstellen
Das einfachste Szenario ist die Implementierung einer Windows-Runtime-Schnittstelle für die lokale Nutzung. Sie benötigen keine Laufzeitklasse, sondern nur eine normale C++ Klasse. Beispielsweise können Sie eine App rund um [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication) schreiben.

> [!NOTE]
> Informationen zum Installieren und Verwenden von C++ / WinRT Visual Studio-Erweiterung (VSIX) (die projektunterstützung für die Vorlage bereitstellt) finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

In Visual Studio die **Visual C++** > **Windows Universal** > **Core-App (C++ / WinRT)** Projektvorlage veranschaulicht die **"Coreapplication"** Muster. Das Muster beginnt mit der Übergabe einer Implementierung von [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) an [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** verwendet die Schnittstelle, um die erste Ansicht der App zu erstellen. Konzeptionell sieht **IFrameworkViewSource** so aus.

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

Auch hier ist die Implementierung von **CoreApplication::Run** konzeptionell sinnvoll.

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

Sie als Entwickler implementieren also die Schnittstelle **IFrameworkViewSource**. C++/WinRT beinhaltet das Basisstruktur-Template [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), um es einfach zu machen, eine Schnittstelle (oder mehrere) zu implementieren, ohne auf eine Programmierung im COM-Stil zurückgreifen zu müssen. Sie leiten Ihren Typ einfach aus **Implementierungen** ab und implementieren dann die Funktionen der Schnittstelle. Gehen Sie dazu wie folgt vor:

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

Das hat sich um **IFrameworkViewSource** gekümmert. Im nächsten Schritt wird ein Objekt zurückgegeben, das die Schnittstelle **IFrameworkView** implementiert. Sie können diese Schnittstelle auch für **App** implementieren. Dieses nächste Codebeispiel stellt eine minimale App dar, die zumindest ein Fenster auf dem Desktop öffnet.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

Da Ihre **App** Typ *ist ein* **IFrameworkViewSource**, können Sie nur einen übergeben **ausführen**.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App{});
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Wenn Sie eine Laufzeitklasse in einer Komponente für Windows-Runtime erstellen, gehen Sie wie folgt vor
Wenn Ihr Typ in einer Komponente für Windows-Runtime für den Einsatz in einer Anwendung verpackt ist, dann muss es sich um eine Laufzeitklasse handeln.

> [!TIP]
> Wir empfehlen, jede Laufzeitklasse in einer eigenen Interface Definition Language (IDL)-Datei zu deklarieren, um die Buildleistung beim Bearbeiten einer IDL-Datei zu optimieren und um die logische Übereinstimmung einer IDL-Datei mit den generierten Quellcodedateien sicherzustellen. Visual Studio führt alle resultierenden `.winmd`-Dateien in einer einzigen Datei zusammen, die denselben Namen erhält wie der Stammnamespace. Die endgültige `.winmd`-Datei wird die sein, die von den Nutzern Ihrer Komponente referenziert wird.

Hier sehen Sie ein Beispiel.

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

Diese IDL deklariert eine Windows-Runtime-Klasse (Runtime). Eine Laufzeitklasse ist ein Typ, der über moderne COM-Schnittstellen aktiviert und genutzt werden kann, typischerweise über Ausführungsgrenzen hinweg. Wenn Sie eine IDL-Datei zu Ihrem Projekt hinzufügen und erstellen, generiert die C++/WinRT-Toolchain (`midl.exe` und `cppwinrt.exe`) einen Implementierungstyp für Sie. Unter Verwendung der obigen Beispiel-IDL ist der Implementierungstyp ein C++ Struktur-Stub namens **winrt::MyProject::implementation::MyRuntimeClass** in Quellcodedateien namens `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` und `MyRuntimeClass.cpp`.

Der Implementierungstyp sieht so aus.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        hstring Name();
        void Name(hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Beachten Sie das verwendete F-gebundene Polymorphismusmuster (**MyRuntimeClass** verwendet sich selbst als Vorlage für seine Basis, **MyRuntimeClassT**). Dies wird auch als „curiously recurring template pattern” (CRTP) bezeichnet. Wenn Sie der Vererbungskette nach oben folgen, stoßen Sie auf **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

In diesem Szenario steht also wieder die Basisstrukturvorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) an der Wurzel der Vererbungshierarchie.

Weitere Details, Code und eine exemplarische Vorgehensweise bei der Erstellung von APIs in einer Komponente für Windows-Runtime finden Sie unter [Erstellen von Ereignissen in C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Wenn Sie eine Laufzeitklasse erstellen, die in Ihrer XAML-Benutzeroberfläche referenziert werden soll, gehen Sie wie folgt vor
Wenn Ihr Typ von Ihrer XAML-Benutzeroberfläche referenziert wird, dann muss er eine Laufzeitklasse sein (auch, wenn er sich im selben Projekt wie das XAML befindet). Obwohl diese typischerweise über Ausführungsgrenzen hinweg aktiviert werden, kann eine Laufzeitklasse stattdessen innerhalb der Kompilierungseinheit verwendet werden, die sie implementiert.

In diesem Szenario erstellen *und* nutzen Sie die APIs. Die Vorgehensweise bei der Implementierung Ihrer Laufzeitklasse ist im Wesentlichen die gleiche wie bei einer Komponente für Windows-Runtime. Weitere Informationen finden Sie im vorherigen Abschnitt &mdash;[Wenn Sie eine Laufzeitklasse in einer Komponente für Windows-Runtime erstellen, gehen Sie wie folgt vor](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component) Das einzige Detail, das sich von der IDL unterscheidet, ist, dass die C++/WinRT-Toolchain nicht nur einen Implementierungstyp, sondern auch einen Projizierungsyp generiert. Es ist wichtig zu verstehen, dass in diesem Szenario die Angabe „**MyRuntimeClass**” mehrdeutig sein kann. Es gibt mehrere Entitäten von unterschiedlicher Art mit diesem Namen.

- **MyRuntimeClass** ist der Name einer Runtime-Klasse. Tatsächlich handelt es sich aber um eine Abstraktion – in IDL deklariert und in einer Programmiersprache implementiert.
- **MyRuntimeClass** ist der Name der C++ Struktur **winrt::MyProject::implementation::MyRuntimeClass**, die die C++/WinRT-Implementierung der Laufzeitklasse ist. Wenn es getrennte Implementierungs- und Nutzungsprojekte gibt, dann existiert diese Struktur nur im Implementierungsprojekt (wie gesehen). Dies ist der *Implementierungstyp* oder *die Implementierung*. Dieser Typ wird (durch das `cppwinrt.exe` Tool) in den Dateien `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` und `MyRuntimeClass.cpp` erzeugt.
- **MyRuntimeClass** ist der Name des projizierten Typs in Form der C++ Struktur **winrt::MyProject::MyRuntimeClass**. Wenn es getrennte Implementierungs- und Nutzungsprojekte gibt, dann existiert diese Struktur nur im Nutzungsprojekt. Dies ist der *projizierte Typ* oder die *Projizierung*. Dieser Typ wird (von `cppwinrt.exe`) in der Datei `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` generiert.

![Projizierten Typ und Implementierungstyp](images/myruntimeclass.png)

Hier sind die Teile des projizierten Typs, die für dieses Thema relevant sind.

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

Ein Beispiel für die Implementierung der **INotifyPropertyChanged**-Schnittstelle in einer Laufzeitklasse finden Sie unter [XAML-Steuerelelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md).

Die Vorgehensweise für die Verwendung Ihrer Laufzeitklasse in diesem Szenario ist unter [APIs mit C++/WinRT konsumieren](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project) beschrieben.

## <a name="runtime-class-constructors"></a>Laufzeitklassenkonstruktoren
Hier sind einige Punkte, die wir aus den obigen Listings entfernen können.

- Jeder Konstruktor, den Sie in Ihrer IDL deklarieren, bewirkt, dass ein Konstruktor sowohl für Ihren Implementierungstyp als auch für Ihren projizierten Typ generiert wird. IDL-deklarierte Konstruktoren werden verwendet, um die Laufzeitklasse aus einer *anderen Kompiliereinheit* zu nutzen.
- Unabhängig davon, ob Sie IDL-deklarierte Konstruktoren haben oder nicht, wird eine Konstruktorüberladung von `nullptr_t` für Ihren projizierten Typ erzeugt. Der Aufruf des `nullptr_t`-Konstruktors ist *der erste von zwei Schritten*, um die Laufzeitklasse aus *derselben* Kompiliereinheit zu verwenden. Weitere Details und ein Codebeispiel finden Sie unter [APIs mit C++/WinRT nutzen](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Wenn Sie die Laufzeitklasse in *derselben* Kompilierungseinheit nutzen, können Sie auch Nicht-Default-Konstruktoren direkt für den Implementierungstyp implementieren (die sich in `MyRuntimeClass.h` befindet).

> [!NOTE]
> Wenn Sie erwarten, dass Ihre Laufzeitklasse von einer anderen Kompilierungseinheit genutzt wird (was üblich ist), nehmen Sie Konstruktor(en) in Ihre IDL auf (mindestens einen Standardkonstruktor). Dadurch erhalten Sie neben Ihrem Implementierungstyp auch eine Factory-Implementierung.
> 
> Wenn Sie Ihre Laufzeitklasse nur innerhalb derselben Kompilierungseinheit erstellen und nutzen wollen, dann deklarieren Sie keine Konstruktoren in Ihrer IDL. Sie benötigen keine Factory-Implementierung, und es wird auch keine generiert. Der Standardkonstruktor Ihres Implementierungstyps wird gelöscht, aber Sie können ihn einfach bearbeiten und den Standard verwenden.
> 
> Wenn Sie Ihre Laufzeitklasse nur innerhalb derselben Kompilierungseinheit erstellen und nutzen wollen und Sie Konstruktorparameter benötigen, erstellen Sie den/die Konstruktor(en), die Sie direkt in Ihrem Implementierungstyp benötigen.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>Instanziierung und Rückgabe von Implementierungstypen und Schnittstellen
In diesem Abschnitt nehmen wir als Beispiel einen Implementierungstyp namens **MyType**, der die Schnittstellen [**IStringable**](/uwp/api/windows.foundation.istringable) und [**IClosable**](/uwp/api/windows.foundation.iclosable) implementiert.

Sie können **MyType** direkt von [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) ableiten (es ist keine Laufzeitklasse).

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

Oder Sie können sie aus der IDL generieren (es ist eine Laufzeitklasse).

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

Um von **MyType** zu einem **IStringable**- oder **IClosable**-Objekt zu gelangen, das Sie als Teil Ihrer Projizierung verwenden oder zurückgeben können, können Sie die Funktionsvorlage [**winrt::make**](/uwp/cpp-ref-for-winrt/make) aufrufen. **Stellen** Standardschnittstelle für die des Implementierungstyps zurückgibt.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> Wenn Sie Ihren Typ jedoch in Ihrer XAML-Benutzeroberfläche referenzieren, gibt es sowohl einen Implementierungstyp als auch einen projizierten Typ im selben Projekt. In diesem Fall **stellen** gibt eine Instanz der projizierten Typ zurück. Ein Codebeispiel für dieses Szenario finden Sie unter [XAML-Steuererlemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

Wir können nur `istringable` (im obigen Codebeispiel) verwenden, um die Mitglieder der **IStringable**-Schnittstelle aufzurufen. Aber eine C++/WinRT-Schnittstelle (die eine projizierte Schnittstelle ist) ist von [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) abgeleitet. Sie können also Aufrufen [ **IUnknown::as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (oder [ **IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) darauf, um Abfragen für andere projizierten Typen oder Schnittstellen, die Sie Außerdem verwenden oder zurückgeben.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Wenn Sie auf alle Mitglieder der Implementierung zugreifen und später eine Schnittstelle an einen Aufrufer zurückgeben müssen, verwenden Sie die Funktionsvorlage [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** gibt einen [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)-Wrapper mit dem Implementierungstyp zurück. Sie können auf die Mitglieder aller Schnittstellen zugreifen (mit dem Pfeiloperator), sie **unverändert** an einen Aufrufer zurückgeben oder das resultierende Interface-Objekt an einen Aufrufer zurückgeben.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

Die **MyType**-Klasse ist nicht Teil der Projizierung, sondern die Implementierung. Aber auf diese Weise können Sie die Implementierungsmethoden direkt aufrufen, ohne den Aufwand für einen virtuellen Funktionsaufruf. Obwohl im obigen Beispiel **MyType::ToString** die gleiche Signatur wie die projizierte Methode von **IStringable** verwendet, rufen wir die nicht-virtuelle Methode direkt auf, ohne die binäre Schnittstelle der Anwendung (ABI) zu verwenden. Der **com_ptr** enthält einfach einen Zeiger auf die **MyType**-Struktur, sodass Sie auch auf alle anderen internen Details von **MyType** über die Variable `myimpl` und den Pfeiloperator zugreifen können.

Im Fall, in dem Sie einem Schnittstellenobjekt, und Sie zufällig wissen, dass eine Schnittstelle auf Ihre Implementierung, erhalten Sie wieder auf die Implementierung mithilfe, der [ **winrt::get_self** ](/uwp/cpp-ref-for-winrt/get-self) Funktion Vorlage. Auch hier handelt es sich um eine Technik, die virtuelle Funktionsaufrufe vermeidet und Sie direkt die Implementierung nutzen lässt.

> [!NOTE]
> Wenn die Windows-SDK-Version 10.0.17763.0 (Windows 10, Version 1809) nicht installiert haben, oder höher verwenden, Sie zum Aufrufen müssen [ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) anstelle von [ **winrt::get_ Self-Service**](/uwp/cpp-ref-for-winrt/get-self).

Hier sehen Sie ein Beispiel. Ist es ein weiteres Beispiel [implementieren die **BgLabelControl** benutzerdefinierten Steuerelementklasse](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class).

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Aber nur das ursprüngliche Interface-Objekt behält eine Referenz. Wenn *Sie* es behalten wollen, können Sie [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function) aufrufen.

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

Der Implementierungstyp selbst ist nicht von **winrt::Windows::Foundation::IUnknown** abgeleitet. Daher hat er keine **as**-Funktion. Trotzdem können Sie eine instanziieren und auf die Mitglieder aller Schnittstellen zugreifen. Wenn Sie dies tun, geben Sie die Instanz des Raw-Implementierungstyps jedoch nicht an den Aufrufer zurück. Verwenden Sie stattdessen eine der oben gezeigten Techniken und geben Sie eine projizierte Schnittstelle oder einen **com_ptr** zurück.

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

Wenn Sie eine Instanz Ihres Implementierungstyps haben und diese an eine Funktion übergeben müssen, die den entsprechenden projizierten Typ erwartet, dann können Sie dies tun. Ein Konvertierungsoperator für Ihre Implementierungstyp vorhanden sind (vorausgesetzt, dass der Implementierungstyp von generiert wurde die `cppwinrt.exe` Tool), das macht dies möglich. Sie können einen Wert für den Implementierung direkt auf eine Methode übergeben, die einen Wert vom entsprechenden projizierten Typ erwartet. Sie können aus einer Implementierung typmemberfunktion übergeben `*this` an eine Methode, die einen Wert vom entsprechenden projizierten Typ erwartet.

```cppwinrt
// MyProject::MyType is the projected type; the implementation type would be MyProject::implementation::MyType.

void MyOtherType::DoWork(MyProject::MyType const&){ ... }

...

void FreeFunction(MyProject::MyOtherType const& ot)
{
    MyType myimpl;
    ot.DoWork(myimpl);
}

...

void MyType::MemberFunction(MyProject::MyOtherType const& ot)
{
    ot.DoWork(*this);
}
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Ableiten von einem Typ, der einen nicht trivialen Konstruktor aufweist
[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) ist ein Beispiel für einen nicht standardmäßigen Konstruktor. Es gibt keinen Standardkonstruktor. Um ein **ToggleButtonAutomationPeer** zu erstellen, müssen Sie also einen *Owner*übergeben. Wenn Sie von **ToggleButtonAutomationPeer** ableiten, dann müssen Sie also einen Konstruktor zur Verfügung stellen, der einen *Owner* entgegen nimmt und ihn an die Basisklasse übergibt. Mal sehen, wie das in der Praxis aussieht.

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

Der generierte Konstruktor für Ihren Implementierungstyp sieht so aus.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

Der einzige noch fehlende Teil ist, dass Sie diesen Konstruktorparameter an die Basisklasse übergeben müssen. Erinnern Sie sich an das oben erwähnte, F-gebundene Polymorphisierungsmuster? Sobald Sie die Details dieses von C++/WinRT verwendeten Musters kennen, können Sie feststellen, wie Ihre Basisklasse heißt (oder Sie können einfach in der Header-Datei Ihrer Implementierungsklasse nachsehen). So rufen Sie in diesem Fall den Konstruktor der Basisklasse auf.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

Der Konstruktor der Basisklasse erwartet ein **ToggleButton**. Und **MySpecializedToggleButton** *ist eine* **ToggleButton**.

Bis Sie die oben beschriebene Änderung vornehmen (um den Konstruktorparameter an die Basisklasse weiterzugeben), wird der Compiler Ihren Konstruktor markieren und darauf hinweisen, dass es (in diesem Fall) keinen geeigneten Standardkonstruktor für einen Typ namens **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** gibt. Das ist die Basisklasse der Basisklasse Ihres Implementierungstyps.

## <a name="important-apis"></a>Wichtige APIs
* [Vorlage für WinRT::com_ptr-Struktur](/uwp/cpp-ref-for-winrt/com-ptr)
* [WinRT::com_ptr::copy_from-Funktion](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)
* [Vorlage für WinRT::from_abi-Funktion](/uwp/cpp-ref-for-winrt/from-abi)
* [Vorlage für WinRT::get_self-Funktion](/uwp/cpp-ref-for-winrt/get-self)
* [Vorlage für WinRT::Implements-Struktur](/uwp/cpp-ref-for-winrt/implements)
* [Vorlage für WinRT::Make-Funktion](/uwp/cpp-ref-for-winrt/make)
* [Vorlage für WinRT::make_self-Funktion](/uwp/cpp-ref-for-winrt/make-self)
* [WinRT::Windows::Foundation::IUnknown:: als Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
