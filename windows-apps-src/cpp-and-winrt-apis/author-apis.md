---
description: Dieses Thema zeigt, wie C++/WinRT-APIs mithilfe der **winrt::implements**-Basisstruktur direkt oder indirekt erstellt werden.
title: Erstellen von APIs mit C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: Windows 10, uwp, Standard, c++, cpp, winrt, projiziert, Projektion, Implementierung, implementieren, Laufzeitklasse, Aktivierung
ms.localizationpriority: medium
ms.openlocfilehash: 84c0e9315950541e51bf49f5c0eec370f3188c4d
ms.sourcegitcommit: 58f6643510a27d6b9cd673da850c191ee23b813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74701487"
---
# <a name="author-apis-with-cwinrt"></a>Erstellen von APIs mit C++/WinRT

Dieses Thema zeigt, wie [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)-APIs mithilfe der [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)-Basisstruktur direkt oder indirekt erstellt werden. In diesem Zusammenhang werden die Synonyme *Produzieren* oder *Implementieren* für den Begriff *Erstellen* verwendet. Dieses Thema behandelt die folgenden Szenarien für die Implementierung von APIs für einen C++/WinRT-Typ in dieser Reihenfolge.

> [!NOTE]
> In diesem Thema werden Komponenten für Windows-Runtime erwähnt, jedoch nur im Kontext von C++/WinRT. Informationen zu Komponenten für Windows-Runtime, die alle Sprachen für Windows-Runtime abdecken, finden Sie unter [Komponenten für Windows-Runtime](/windows/uwp/winrt-components/).

- Sie erstellen *keine* Windows-Runtime-Klasse (Runtime-Klasse). Sie möchten lediglich mindestens eine Windows-Runtime-Schnittstelle für den lokalen Gebrauch in Ihrer App implementieren. Die Ableitung erfolgt in diesem Fall direkt von **winrt::implements**, und Sie implementieren Funktionen.
- Sie *erstellen eine* Laufzeitklasse. Möglicherweise erstellen Sie eine Komponente, die von einer App genutzt werden soll. Oder Sie erstellen einen Typ, der von der XAML-Benutzeroberfläche (UI) genutzt werden soll, und in diesem Fall implementieren und nutzen Sie eine Laufzeitklasse innerhalb derselben Kompilierungseinheit. In diesen Fällen lassen Sie die Tools Klassen generieren, die von **winrt::implements** abgeleitet sind.

In beiden Fällen wird der Typ, der Ihre C++/WinRT-APIs implementiert, als *Implementierungstyp* bezeichnet.

> [!IMPORTANT]
> Es ist wichtig, das Konzept eines Implementierungstyps von dem eines projizierten Typs zu unterscheiden. Der projizierte Typ ist in [Verwenden von APIs mit C++/WinRT](consume-apis.md) beschrieben.

## <a name="if-youre-not-authoring-a-runtime-class"></a>Wenn Sie *keine* Laufzeitklasse erstellen

Das einfachste Szenario ist die Implementierung einer Windows-Runtime-Schnittstelle für die lokale Nutzung. Sie benötigen keine Laufzeitklasse, sondern nur eine normale C++-Klasse. Beispielsweise können Sie eine App rund um [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication) schreiben.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) finden Sie unter [Visual Studio-Unterstützung für C++/ WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

In Visual Studio zeigt die Projektvorlage **Visual C++**  > **Windows Universal** > **Core App (C++/WinRT)** das **CoreApplication**-Muster. Das Muster beginnt mit der Übergabe einer Implementierung von [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) an [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

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

Sie als Entwickler implementieren also die Schnittstelle **IFrameworkViewSource**. C++/WinRT beinhaltet die Basisstrukturvorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), um es einfach zu machen, eine Schnittstelle (oder mehrere) zu implementieren, ohne auf eine Programmierung im COM-Stil zurückgreifen zu müssen. Sie leiten Ihren Typ einfach aus **implements** ab und implementieren dann die Funktionen der Schnittstelle. Gehen Sie dazu wie folgt vor:

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

Damit wurde **IFrameworkViewSource** behandelt. Im nächsten Schritt wird ein Objekt zurückgegeben, das die Schnittstelle **IFrameworkView** implementiert. Sie können diese Schnittstelle auch für **App** implementieren. Dieses nächste Codebeispiel stellt eine minimale App dar, die zumindest ein Fenster auf dem Desktop öffnet.

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

Da Ihr **App**-Typ *eine* **IFrameworkViewSource** ist, können Sie einfach ein solches Element an **Run** übergeben.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Wenn Sie eine Laufzeitklasse in einer Komponente für Windows-Runtime erstellen, gehen Sie wie folgt vor:

Wenn Ihr Typ in einer Komponente für Windows-Runtime für den Einsatz in einer Anwendung verpackt ist, dann muss es sich um eine Laufzeitklasse handeln. Eine Laufzeitklasse wird in einer IDL-Datei (Microsoft Interface Definition Language) deklariert – siehe [Einbeziehen von Laufzeitklassen in Midl-Dateien (.idl)](#factoring-runtime-classes-into-midl-files-idl).

Jede IDL-Datei resultiert in einer `.winmd`-Datei, und Visual Studio führt all diese Dateien in einer einzigen Datei zusammen, die denselben Namen erhält wie der Stammnamespace. Die endgültige `.winmd`-Datei wird die Datei sein, die von den Nutzern Ihrer Komponente referenziert wird.

Hier ein Beispiel für das Deklarieren einer Laufzeitklasse in einer IDL-Datei.

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

Diese IDL deklariert eine Windows-Runtime-Klasse (Runtime). Eine Laufzeitklasse ist ein Typ, der über moderne COM-Schnittstellen aktiviert und genutzt werden kann, typischerweise über Ausführungsgrenzen hinweg. Wenn Sie eine IDL-Datei zu Ihrem Projekt hinzufügen und erstellen, generiert die C++/WinRT-Toolkette (`midl.exe` und `cppwinrt.exe`) einen Implementierungstyp für Sie. Ein Beispiel für die Verwendung des IDL-Dateiworkflows finden Sie unter [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md).

Unter Verwendung der obigen Beispiel-IDL ist der Implementierungstyp ein C++ Strukturstub namens **winrt::MyProject::implementation::MyRuntimeClass** in Quellcodedateien namens `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` und `MyRuntimeClass.cpp`.

Der Implementierungstyp sieht so aus.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Beachten Sie das verwendete F-gebundene Polymorphismusmuster (**MyRuntimeClass** verwendet sich selbst als Vorlagenargument für seine Basis **MyRuntimeClassT**). Dies wird auch als „Curiously Recurring Template Pattern” (CRTP) bezeichnet. Wenn Sie der Vererbungskette nach oben folgen, stoßen Sie auf **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

In diesem Szenario steht also wieder die Basisstrukturvorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) im Stamm der Vererbungshierarchie.

Weitere Details, Code und eine exemplarische Vorgehensweise bei der Erstellung von APIs in einer Komponente für Windows-Runtime finden Sie unter [Erstellen von Ereignissen in C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Wenn Sie eine Laufzeitklasse erstellen, die in Ihrer XAML-Benutzeroberfläche referenziert werden soll, gehen Sie wie folgt vor

Wenn Ihr Typ von Ihrer XAML-Benutzeroberfläche referenziert wird, dann muss er eine Laufzeitklasse sein (auch, wenn er sich im selben Projekt wie die XAML befindet). Obwohl diese typischerweise über Ausführungsgrenzen hinweg aktiviert werden, kann eine Laufzeitklasse stattdessen innerhalb der Kompilierungseinheit verwendet werden, die sie implementiert.

In diesem Szenario erstellen *und* nutzen Sie die APIs. Die Vorgehensweise bei der Implementierung Ihrer Laufzeitklasse ist im Wesentlichen die gleiche wie bei einer Komponente für Windows-Runtime. Weitere Informationen finden Sie im vorherigen Abschnitt &mdash;[Wenn Sie eine Laufzeitklasse in einer Komponente für Windows-Runtime erstellen, gehen Sie wie folgt vor](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component). Das einzige Detail, das sich von der IDL unterscheidet, ist, dass die C++/WinRT-Toolkette nicht nur einen Implementierungstyp, sondern auch einen projizierten Typ generiert. Es ist wichtig zu verstehen, dass in diesem Szenario die Angabe „**MyRuntimeClass**” mehrdeutig sein kann. Es gibt mehrere Entitäten von unterschiedlicher Art mit diesem Namen.

- **MyRuntimeClass** ist der Name einer Runtime-Klasse. Tatsächlich handelt es sich aber um eine Abstraktion – in IDL deklariert und in einer Programmiersprache implementiert.
- **MyRuntimeClass** ist der Name der C++-Struktur **winrt::MyProject::implementation::MyRuntimeClass**, die die C++/WinRT-Implementierung der Laufzeitklasse ist. Wenn es getrennte Implementierungs- und Nutzungsprojekte gibt, dann ist diese Struktur nur im Implementierungsprojekt vorhanden (wie gezeigt). Dies ist der *Implementierungstyp* oder *die Implementierung*. Dieser Typ wird (durch das `cppwinrt.exe` Tool) in den Dateien `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` und `MyRuntimeClass.cpp` generiert.
- **MyRuntimeClass** ist der Name des projizierten Typs in Form der C++-Struktur **winrt::MyProject::MyRuntimeClass**. Wenn es getrennte Implementierungs- und Nutzungsprojekte gibt, dann ist diese Struktur nur im Nutzungsprojekt vorhanden. Dies ist der *projizierte Typ* oder die *Projektion*. Dieser Typ wird (von `cppwinrt.exe`) in der Datei `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` generiert.

![Projizierter Typ und Implementierungstyp](images/myruntimeclass.png)

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

Die Vorgehensweise für die Verwendung Ihrer Laufzeitklasse in diesem Szenario ist unter [APIs mit C++/WinRT nutzen](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project) beschrieben.

## <a name="factoring-runtime-classes-into-midl-files-idl"></a>Einbeziehen von Laufzeitklassen in Midl-Dateien (.idl)

Das Visual Studio-Projekt und die Elementvorlagen erzeugen eine separate IDL-Datei für jede Laufzeitklasse. Dadurch entsteht ein logischer Zusammenhang zwischen einer IDL-Datei und den zugehörigen generierten Quellcodedateien.

Durch Konsolidieren aller Laufzeitklassen eines Projekts in eine einzige IDL-Datei lässt sich die Buildzeit erheblich verbessern. Bei komplexen (oder zirkulären) `import`-Abhängigkeiten zwischen den Klassen ist eine solche Konsolidierung möglicherweise sogar notwendig. Auch das Erstellen und Überprüfen von Laufzeitklassen ist einfacher, wenn sie alle zusammen sind.

## <a name="runtime-class-constructors"></a>Laufzeitklassenkonstruktoren

Hier sind einige Punkte, die wir aus den oben gezeigten Listings entfernen können.

- Jeder Konstruktor, den Sie in Ihrer IDL deklarieren, bewirkt, dass ein Konstruktor sowohl für Ihren Implementierungstyp als auch für Ihren projizierten Typ generiert wird. IDL-deklarierte Konstruktoren werden verwendet, um die Laufzeitklasse aus einer *anderen Kompiliereinheit* zu nutzen.
- Unabhängig davon, ob Sie IDL-deklarierte Konstruktoren verwenden, wird eine Konstruktorüberladung von **std::nullptr_t** für den projizierten Typ generiert. Der Aufruf des **std::nullptr_t**-Konstruktors ist *der erste von zwei Schritten*, um die Laufzeitklasse aus *derselben* Kompiliereinheit zu verwenden. Weitere Details und ein Codebeispiel finden Sie unter [APIs mit C++/WinRT nutzen](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Wenn Sie die Laufzeitklasse in *derselben* Kompilierungseinheit nutzen, können Sie auch Nicht-Standardkonstruktoren direkt für den Implementierungstyp implementieren (der sich in `MyRuntimeClass.h` befindet).

> [!NOTE]
> Wenn Sie erwarten, dass Ihre Laufzeitklasse von einer anderen Kompilierungseinheit genutzt wird (was üblich ist), nehmen Sie Konstruktoren in Ihre IDL auf (mindestens einen Standardkonstruktor). Dadurch erhalten Sie neben Ihrem Implementierungstyp auch eine Factoryimplementierung.
> 
> Wenn Sie Ihre Laufzeitklasse nur innerhalb derselben Kompilierungseinheit erstellen und nutzen möchten, deklarieren Sie keine Konstruktoren in Ihrer IDL. Sie benötigen keine Factoryimplementierung, und es wird auch keine generiert. Der Standardkonstruktor Ihres Implementierungstyps wird gelöscht, aber Sie können ihn einfach bearbeiten und den Standard verwenden.
> 
> Wenn Sie Ihre Laufzeitklasse nur innerhalb derselben Kompilierungseinheit erstellen und nutzen möchten und Sie Konstruktorparameter benötigen, erstellen Sie Konstruktoren, die Sie direkt in Ihrem Implementierungstyp benötigen.

## <a name="runtime-class-methods-properties-and-events"></a>Laufzeitklassenmethoden, -eigenschaften und -ereignisse

Wir haben gesehen, dass der Workflow das Deklarieren der Laufzeitklasse und ihrer Member mithilfe der IDL umfasst und dann Tools Prototypen und Stubimplementierungen generieren. Sie *können* die automatisch generierten Prototypen für die Member der Laufzeitklasse bearbeiten, damit andere Typen als die Typen übergeben werden, die Sie in der IDL deklarieren. Dies ist jedoch nur möglich, solange der Typ, den Sie in der IDL deklarieren, an den Typ weitergeleitet werden kann, den Sie in der implementierten Version deklarieren.

Beispiele:

- Sie können Parametertypen lockern. Wenn z. B. in der IDL die Methode eine **SomeClass** akzeptiert, können Sie diese in der Implementierung in **IInspectable** ändern. Dies ist möglich, da jede **SomeClass** an **IInspectable** weitergeleitet werden kann (der umgekehrte Vorgang ist selbstverständlich nicht möglich).
- Ein kopierbarer Parameter kann nach Wert statt nach Verweis akzeptiert werden. Sie können z. B. `SomeClass` in `SomeClass const&` ändern. Dies ist erforderlich, wenn Sie das Erfassen eines Verweises in einer Coroutine vermeiden müssen (siehe [Parameterübergabe](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)).
- Sie können den Rückgabewert lockern. Sie können z. B. **void** in [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) ändern.

Die beiden letzten sind sehr hilfreich, wenn Sie einen Handler für asynchrone Ereignisse schreiben.

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

Der Implementierungstyp kann nicht direkt zugewiesen werden.

```cppwinrt
MyType myimpl; // error C2259: 'MyType': cannot instantiate abstract class
```

Sie können aber von **MyType** zu einem **IStringable**- oder **IClosable**-Objekt gelangen, das Sie als Teil Ihrer Projektion verwenden oder zurückgeben können, indem Sie die Funktionsvorlage [**winrt::make**](/uwp/cpp-ref-for-winrt/make) aufrufen. **make** gibt die Standardschnittstelle des Implementierungstyps zurück.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> Wenn Sie Ihren Typ jedoch in Ihrer XAML-Benutzeroberfläche referenzieren, gibt es sowohl einen Implementierungstyp als auch einen projizierten Typ im selben Projekt. In diesem Fall gibt **make** eine Instanz des projizierten Typs zurück. Ein Codebeispiel für dieses Szenario finden Sie unter [XAML-Steuererlemente, Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

Wir können nur `istringable` (im Codebeispiel oben) verwenden, um die Member der **IStringable**-Schnittstelle aufzurufen. Aber eine C++/WinRT-Schnittstelle (die eine projizierte Schnittstelle ist) ist von [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) abgeleitet. Sie können [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (oder [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) dafür aufrufen, um nach anderen projizierten Typen oder Schnittstellen zu suchen, die Sie ebenfalls verwenden oder zurückgeben können.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Wenn Sie auf alle Member der Implementierung zugreifen und später eine Schnittstelle an einen Aufrufer zurückgeben müssen, verwenden Sie die Funktionsvorlage [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** gibt einen [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)-Wrapper mit dem Implementierungstyp zurück. Sie können auf die Member aller Schnittstellen zugreifen (mit dem Pfeiloperator), sie **unverändert** an einen Aufrufer zurückgeben oder das sich ergebende Schnittstellenobjekt an einen Aufrufer zurückgeben.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

Die **MyType**-Klasse ist nicht Teil der Projektion, sondern die Implementierung. Aber auf diese Weise können Sie die Implementierungsmethoden direkt ohne den Aufwand für einen virtuellen Funktionsaufruf aufrufen. Obwohl im Beispiel oben **MyType::ToString** die gleiche Signatur wie die projizierte Methode von **IStringable** verwendet, rufen wir die nicht-virtuelle Methode direkt auf, ohne die binäre Schnittstelle der Anwendung (ABI) zu verwenden. **com_ptr** enthält einfach einen Zeiger auf die **MyType**-Struktur, sodass Sie auch auf alle anderen internen Details von **MyType** über die Variable `myimpl` und den Pfeiloperator zugreifen können.

Falls Sie ein Interface-Objekt haben und wissen, dass es sich um eine Schnittstelle für Ihre Implementierung handelt, können Sie mit der Funktionsvorlage [**from_abi**](/uwp/cpp-ref-for-winrt/get-self) zur Implementierung zurückkehren. Auch hier handelt es sich um eine Technik, die virtuelle Funktionsaufrufe vermeidet und Sie direkt die Implementierung nutzen lässt.

> [!NOTE]
> Wenn die Windows SDK-Version 10.0.17763.0 (Windows 10, Version 1809) oder höher nicht installiert ist, müssen Sie [**winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) anstelle von [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) aufrufen.

Hier sehen Sie ein Beispiel. Ein weiteres Beispiel ist unter [Implementieren der benutzerdefinierten **BgLabelControl**-Steuerelementklasse](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class) verfügbar.

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Aber nur das ursprüngliche Interface-Objekt hält an einem Verweis fest. Wenn *Sie* an ihm festhalten möchten, können Sie [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function) aufrufen.

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

Der Implementierungstyp selbst ist nicht von **winrt::Windows::Foundation::IUnknown** abgeleitet. Daher hat er keine **as**-Funktion. Trotzdem können Sie, wie in der **ImplFromIClosable**-Funktion oben zu sehen, auf die Member aller seiner Schnittstellen zugreifen. Wenn Sie so vorgehen, dürfen Sie die Instanz des rohen Implementierungstyps jedoch nicht an den Aufrufer zurückgeben. Verwenden Sie stattdessen eine der bereits gezeigten Techniken, und geben Sie eine projizierte Schnittstelle oder ein **com_ptr**-Element zurück.

Wenn Sie über eine Instanz Ihres Implementierungstyps verfügen und diese an eine Funktion übergeben müssen, die den entsprechenden projizierten Typ erwartet, dann können Sie so vorgehen, wie im Beispiel unten dargestellt. Für Ihren Implementierungstyp ist ein Konvertierungsoperator vorhanden (sofern der Implementierungstyp vom `cppwinrt.exe`-Tool generiert wurde), der dies ermöglicht. Sie können den Wert des Implementierungstyps direkt an eine Methode übergeben, die einen Wert des entsprechenden projizierten Typs erwartet. Aus der Memberfunktion eines Implementierungstyps können Sie `*this` an eine Methode übergeben, die einen Wert des entsprechenden projizierten Typs erwartet.

```cppwinrt
// MyClass.idl
import "MyOtherClass.idl";
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void MemberFunction(MyOtherClass oc);
    }
}

// MyClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyClass : MyClassT<MyClass>
    {
        MyClass() = default;
        void MemberFunction(MyProject::MyOtherClass const& oc) { oc.DoWork(*this); }
    };
}
...

// MyOtherClass.idl
import "MyClass.idl";
namespace MyProject
{
    runtimeclass MyOtherClass
    {
        MyOtherClass();
        void DoWork(MyClass c);
    }
}

// MyOtherClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyOtherClass : MyOtherClassT<MyOtherClass>
    {
        MyOtherClass() = default;
        void DoWork(MyProject::MyClass const& c){ /* ... */ }
    };
}
...

//main.cpp
#include "pch.h"
#include <winrt/base.h>
#include "MyClass.h"
#include "MyOtherClass.h"
using namespace winrt;

// MyProject::MyClass is the projected type; the implementation type would be MyProject::implementation::MyClass.

void FreeFunction(MyProject::MyOtherClass const& oc)
{
    auto defaultInterface = winrt::make<MyProject::implementation::MyClass>();
    MyProject::implementation::MyClass* myimpl = winrt::get_self<MyProject::implementation::MyClass>(defaultInterface);
    oc.DoWork(*myimpl);
}
...
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Ableiten von einem Typ, der keinen standardmäßigen Konstruktor aufweist

[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) ist ein Beispiel für einen nicht standardmäßigen Konstruktor. Es gibt keinen Standardkonstruktor. Um ein **ToggleButtonAutomationPeer**-Element zu erstellen, müssen Sie also einen *Owner* übergeben. Wenn Sie von **ToggleButtonAutomationPeer** ableiten, dann müssen Sie also einen Konstruktor zur Verfügung stellen, der einen *Owner* annimmt und ihn an die Basisklasse übergibt. Sehen wir uns an, wie dies in der Praxis aussieht.

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

Sie müssen nun nur noch diesen Konstruktorparameter an die Basisklasse übergeben. Erinnern Sie sich an das oben erwähnte, F-gebundene Polymorphisierungsmuster? Sobald Sie die Details dieses von C++/WinRT verwendeten Musters kennen, können Sie feststellen, wie Ihre Basisklasse heißt (oder Sie können einfach in der Headerdatei Ihrer Implementierungsklasse nachsehen). So rufen Sie in diesem Fall den Konstruktor der Basisklasse auf.

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

Der Konstruktor der Basisklasse erwartet ein **ToggleButton**-Element. Und **MySpecializedToggleButton** *ist ein* **ToggleButton**-Element.

Bis Sie die oben beschriebene Änderung vornehmen (um den Konstruktorparameter an die Basisklasse weiterzugeben), wird der Compiler Ihren Konstruktor markieren und darauf hinweisen, dass es (in diesem Fall) keinen geeigneten Standardkonstruktor für einen Typ namens **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** gibt. Das ist die Basisklasse der Basisklasse Ihres Implementierungstyps.

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>Namespaces: projizierte Typen, Implementierungstypen und Factorys

Wie Sie weiter oben in diesem Thema gesehen haben, ist eine C++/WinRT-Laufzeitklasse in Form mehrerer C++Klassen in mehreren Namespaces vorhanden. Deshalb hat der Name **MyRuntimeClass** im **winrt::MyProject**-Namespace eine andere Bedeutung als im **winrt::MyProject::implementation**-Namespace. Achten Sie auf den Namespace im aktuellen Kontext, und verwenden Sie dann Namespacepräfixe, wenn Sie einen Namen aus einem anderen Namespace benötigen. Lassen Sie uns die betreffenden Namespaces genauer betrachten.

- **winrt::MyProject**. Dieser Namespace enthält projizierte Typen. Ein Objekt eines projizierten Typs ist ein Proxy und im Grunde lediglich ein intelligenter Zeiger auf ein Unterstützungsobjekt. Dabei kann das Unterstützungsobjekt in Ihrem Projekt oder in einer anderen Kompilierungseinheit implementiert werden.
- **winrt::MyProject::implementation**. Dieser Namespace enthält Implementierungstypen. Ein Objekt eines Implementierungstyps ist kein Zeiger, sondern ein Wert – ein vollständiges C++-Stapelobjekt. Erstellen Sie Implementierungstypen nicht direkt. Rufen Sie stattdessen [ **winrt::make**](/uwp/cpp-ref-for-winrt/make) auf, und übergeben Sie den Implementierungstyp als Vorlagenparameter. Weiter oben in diesem Thema wurden Beispiele für die Verwendung von **winrt::make** gezeigt, und [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage) enthält ein weiteres Beispiel. Siehe auch [Diagnostizieren direkter Zuordnungen](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).
- **winrt::MyProject::factory_implementation**. Dieser Namespace enthält Factorys. Ein Objekt in diesem Namespace unterstützt [**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory).

In dieser Tabelle ist die minimale Namespacequalifikation aufgeführt, die in unterschiedlichen Kontexten verwendet werden muss.

|Der Namespace im Kontext|Für die Angabe des projizierten Typs|So geben Sie den Implementierungstyp an|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> Wenn aus der Implementierung ein projizierter Typ zurückgegeben werden soll, achten Sie darauf, dass Sie den Implementierungstyp nicht durch Schreiben von `MyRuntimeClass myRuntimeClass;` instanziieren. Die richtigen Verfahren und der Code für dieses Szenario wurden weiter oben in diesem Thema im Abschnitt [Instanziierung und Rückgabe von Implementierungstypen und Schnittstellen](#instantiating-and-returning-implementation-types-and-interfaces) gezeigt.
>
> Das Problem bei `MyRuntimeClass myRuntimeClass;` in diesem Szenario ist, dass im Stapel ein **winrt::MyProject::implementation::MyRuntimeClass**-Objekt erstellt wird. Dieses Objekt (vom Typ „implementation“) verhält sich in gewisser Hinsicht wie der projizierte Typ – Sie können auf die gleiche Weise Methoden für es aufrufen, und es kann sogar in einen projizierten Typ konvertiert werden. Das Objekt wird jedoch gemäß den C++-Regeln gelöscht, wenn der Bereich verlassen wird. Wenn ein projizierter Typ (ein intelligenter Zeiger) an das Objekt zurückgegeben wurde, verweist dieser Zeiger auf kein Objekt.
>
> Dieser Fehler, bei dem es zu Speicherbeschädigung kommt, ist schwierig zu diagnostizieren. Für Debugbuilds hilft eine C++/WinRT-Assertion, den Fehler mithilfe einer Stapelerkennung abzufangen. Da jedoch Coroutinen auf dem Heap zugeordnet werden, können Sie diesen Fehler nicht abfangen, wenn er in einer Coroutine erfolgt. Weitere Informationen finden Sie unter [Diagnostizieren direkter Zuordnungen](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>Verwenden von projizierten Typen und Implementierungstypen mit verschiedenen C++/ WinRT-Funktionen

In der folgenden Tabelle werden C++/-WinRT-Funktionen, die einen Typ erwarten, sowie der erwartete Typ (projizierter Typ, Implementierungstyp oder beide Typen) aufgeführt.

|Feature|Akzeptierter Typ|Anmerkungen|
|-|-|-|
|`T` (stellt einen intelligenten Zeiger dar)|Projiziert|Siehe den Hinweis in [Namespaces: projizierte Typen, Implementierungstypen und Factorys](#namespaces-projected-types-implementation-types-and-factories) zur irrtümlichen Verwendung des Implementierungstyps.|
|`agile_ref<T>`|Beide|Wenn Sie den Implementierungstyp verwenden, muss das Konstruktorargument `com_ptr<T>` lauten.|
|`com_ptr<T>`|Implementierung|Die Verwendung des projizierten Typs generiert den Fehler `'Release' is not a member of 'T'`.|
|`default_interface<T>`|Beide|Wenn Sie den Implementierungstyp verwenden, wird die erste implementierte Schnittstelle zurückgegeben.|
|`get_self<T>`|Implementierung|Die Verwendung des projizierten Typs generiert den Fehler `'_abi_TrustLevel': is not a member of 'T'`.|
|`guid_of<T>()`|Beide|Gibt die GUID der Standardschnittstelle zurück.|
|`IWinRTTemplateInterface<T>`<br>|Projiziert|Bei Verwendung des Kompilierungstyps wird der Code kompiliert, dies ist jedoch ein Fehler. Siehe den Hinweis in [Namespaces: projizierte Typen, Implementierungstypen und Factorys.](#namespaces-projected-types-implementation-types-and-factories)|
|`make<T>`|Implementierung|Bei Verwendung des projizierten Typs wird der folgende Fehler generiert: `'implements_type': is not a member of any direct or indirect base class of 'T'`|
| `make_agile(T const&amp;)`|Beide|Wenn Sie den Implementierungstyp verwenden, muss das Argument `com_ptr<T>` lauten.|
| `make_self<T>`|Implementierung|Bei Verwendung des projizierten Typs wird der folgende Fehler generiert: `'Release': is not a member of any direct or indirect base class of 'T'`|
| `name_of<T>`|Projiziert|Wenn Sie den Implementierungstyp verwenden, erhalten Sie die GUID der Standardschnittstelle in Form einer Zeichenfolge.|
| `weak_ref<T>`|Beide|Wenn Sie den Implementierungstyp verwenden, muss das Konstruktorargument `com_ptr<T>` lauten.|

## <a name="opt-in-to-uniform-construction-and-direct-implementation-access"></a>Aktivieren von einheitlicher Konstruktion und direktem Implementierungszugriff

In diesem Abschnitt wird ein Feature von C++/WinRT 2.0 beschrieben, das aktiviert werden kann, für neue Projekte jedoch bereits standardmäßig aktiviert ist. Für vorhandene Projekte müssen Sie es festlegen, indem Sie das Tool `cppwinrt.exe` konfigurieren. Legen Sie in Visual Studio die Projekteigenschaft **Allgemeine Eigenschaften** > **C++/WinRT** > **Optimiert** auf *Ja* fest. Hierdurch wird der Projektdatei `<CppWinRTOptimized>true</CppWinRTOptimized>` hinzugefügt. Dies hat dieselbe Wirkung wie das Hinzufügen des Schalters, wenn `cppwinrt.exe` über die Befehlszeile aufgerufen wird.

Mit dem Schalter `-opt[imize]` wird ein Feature aktiviert, das häufig als *einheitliche Konstruktion* bezeichnet wird. Bei der *einheitlichen*  Konstruktion verwenden Sie die C++/WinRT-Sprachprojektion selbst, um Ihre Implementierungstypen (von Ihrer Komponente implementierte Typen für die Verwendung durch Anwendungen) effizient und ohne Probleme bei Ladeprogrammen zu erstellen und zu verwenden.

Bevor wir das Feature beschreiben, zeigen wir zunächst die Situation *ohne* einheitliche Konstruktion. Zur Veranschaulichung beginnen wir mit dieser Windows-Runtime-Beispielklasse.

```idl
// MyClass.idl
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void Method();
        static void StaticMethod();
    }
}
```

Als mit der C++/WinRT-Bibliothek vertrauter Entwickler verwenden Sie die Klasse wahrscheinlich folgendermaßen.

```cppwinrt
using namespace winrt::MyProject;

MyClass c;
c.Method();
MyClass::StaticMethod();
```

Sofern sich der dargestellte verwendende Code nicht in der derselben Komponente befindet, die diese Klasse implementiert, ist dies durchaus vernünftig. C++/WinRT als Sprachprojektion schirmt Sie als Entwickler von der ABI (die binäre COM-basierte Anwendungsschnittstelle, die von der Windows-Runtime definiert wird) ab. C++/WinRT greift für den Aufruf nicht direkt auf die Implementierung zu, sondern durchläuft die ABI.

Folglich ruft die C++/WinRT-Projektion in der Codezeile, in der Sie ein **MyClass**-Objekt (`MyClass c;`) erstellen, [**RoGetActivationFactory**](/windows/win32/api/roapi/nf-roapi-rogetactivationfactory) auf, um die Klasse oder Aktivierungsfactory abzurufen, und erstellt dann das Objekt mithilfe dieser Factory. In der letzten Zeile wird die Factory für etwas verwendet, was der Aufruf einer statischen Methode zu sein scheint. Für all dies muss die Klasse registriert sein und das Modul den Einstiegspunkt [**DllGetActivationFactory**](/previous-versions/br205771(v=vs.85)) implementieren. C++/WinRT verfügt über einen sehr schnellen Factorycache, sodass der oben beschriebene Prozess kein Problem für Anwendungen verursacht, die Ihre Komponente verwenden. Dennoch ist der oben beschriebene Code für Ihre Komponente nicht optimal.

Erstens erfolgt der Aufruf über **RoGetActivationFactory** (oder sogar nachfolgende Aufrufe über den Factorycache) immer langsamer als der Aufruf mit direktem Zugriff auf die Implementierung, egal wie schnell der C++/WinRT-Factorycache ist. Der Aufruf von **RoGetActivationFactory**, gefolgt von [**IActivationFactory::ActivateInstance**](/windows/win32/api/activation/nf-activation-iactivationfactory-activateinstance) und dann von [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)) ist offensichtlich weniger effizient als die Verwendung des C++-Ausdrucks `new` für einen lokal definierten Typ. Deshalb verwenden erfahrene C++/WinRT-Entwickler i. d. R. die Hilfsfunktion [**winrt::make**](/uwp/cpp-ref-for-winrt/make) oder [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self), wenn sie Objekte in einer Komponente erstellen.

```cppwinrt
// MyClass c;
MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
```

Sie können jedoch ersehen, dass dies weitaus weniger einfach und präzise ist. Sie müssen zum Erstellen des Objekts eine Hilfsfunktion verwenden, und Sie müssen außerdem zwischen dem Implementierungstyp und dem projizierten Typ unterscheiden.

Außerdem bedeutet das Erstellen der Klasse mithilfe der Projektion, dass die Aktivierungsfactory zwischengespeichert wird. Normalerweise ist dies gewünscht. Wenn sich jedoch die Factory in demselben Modul (DLL) befindet, das den Befehl aufruft, haben Sie die DLL fixiert, und sie kann nicht entladen werden. In vielen Fällen spielt dies keine Rolle. Einige Systemkomponenten *müssen* jedoch das Entladen unterstützen.

In solchen Fällen ist das Konzept der *einheitlichen Konstruktion* hilfreich. Unabhängig davon, ob sich der Erstellungscode in einem Projekt befindet, das die Klasse lediglich verwendet, oder ob er sich in dem Projekt befindet, das die Klasse *implementiert* – Sie können zum Erstellen des Objekts die gleiche Syntax verwenden.

```cppwinrt
// MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
MyClass c;
```

Wenn Sie das Komponentenprojekt mit dem Schalter `-opt[imize]` erstellen, wird der Aufruf über die Sprachprojektion bis zu dem effizienten Aufruf der **winrt::make**-Funktion kompiliert, mit dem der Implementierungstyp direkt erstellt wird. Dadurch wird die Syntax einfach und vorhersagbar, es erfolgt keine Leistungseinbuße durch den Aufruf über die Factory, und die Komponente bleibt nicht im Prozess fixiert. Das Konzept ist nicht nur für Komponentenprojekte, sondern auch für XAML-Anwendungen hilfreich. Durch das Umgehen von **RoGetActivationFactory** für in derselben Anwendung implementierte Klassen können Sie sie auf die gleiche Weise wie Klassen außerhalb der Komponente erstellen (ohne sie registrieren zu müssen).

Die einheitliche Konstruktion gilt für *alle* Aufrufe, die von der Factory im Hintergrund verarbeitet werden. Praktisch bedeutet dies, dass von der Optimierung sowohl Konstruktoren als auch statische Member profitieren. Hier ist noch einmal das ursprüngliche Beispiel.

```cppwinrt
MyClass c;
c.Method();
MyClass::StaticMethod();
```

Ohne `-opt[imize]` erfordern die erste und letzte Anweisung Aufrufe über das Factoryobjekt. *Mit* `-opt[imize]` erfordert dies keiner der Aufrufe. Und diese Aufrufe werden direkt für die Implementierung kompiliert und können sogar inline erfolgen. Dies verweist auf den anderen Begriff, der häufig im Zusammenhang mit `-opt[imize]` verwendet wird, nämlich *direkter Zugriff auf die Implementierung*.

Sprachprojektionen sind praktisch, wenn Sie jedoch direkt auf die Implementierung zugreifen können, sollten Sie dies nutzen, um einen möglichst effizientesten Code zu erstellen. Dies ist in C++/WinRT möglich, ohne dass Sie auf die Sicherheit und Produktivität der Projektion verzichten müssen.

Dies ist ein Breaking Change, da die Komponente die Voraussetzungen erfüllen muss, um den direkten Zugriff der Sprachprojektion auf ihre Implementierungstypen zuzulassen. Da C++/WinRT eine headerbasierte Bibliothek ist, können Sie die Funktionsweise überprüfen. Ohne `-opt[imize]` werden der **MyClass**-Konstruktor und der **StaticMethod**-Member wie folgt durch die Projektion definiert.

```cppwinrt
namespace winrt::MyProject
{
    inline MyClass::MyClass() :
        MyClass(impl::call_factory<MyClass>([](auto&& f){
            return f.template ActivateInstance<MyClass>(); }))
    {
    }
    inline void MyClass::StaticMethod()
    {
        impl::call_factory<MyClass, MyProject::IClassStatics>([&](auto&& f) {
            return f.StaticMethod(); });
    }
}
```

Sie müssen nicht den gesamten obigen Codeausschnitt nachvollziehen. Mit ihm soll lediglich gezeigt werden, dass beide Aufrufe den Aufruf einer Funktion mit dem Namen **call_factory** beinhalten. Daran können Sie erkennen, dass diese Aufrufe den Factorycache beinhalten und nicht direkt auf die Implementierung zugreifen. *Mit*  `-opt[imize]` werden diese Funktionen überhaupt nicht definiert. Stattdessen werden sie von der Projektion deklariert, und ihre Definitionen werden der Komponente überlassen.

Die Komponente kann dann Definitionen bereitstellen, mit denen die Aufrufe direkt auf die Implementierung zugreifen. Dies ist jetzt der Breaking Change. Diese Definitionen werden generiert, wenn Sie sowohl `-component` als auch `-opt[imize]` verwenden, und sie befinden sich in einer Datei mit dem Namen `Type.g.cpp`, wobei *Type* der Name der zu implementierenden Laufzeitklasse ist. Deshalb können verschiedene Linkerfehler auftreten, wenn Sie `-opt[imize]` erstmals in einem vorhandenen Projekt aktivieren. Zum Beheben der Probleme müssen Sie diese generierte Datei in Ihre Implementierung einschließen.

In unserem Beispiel kann `MyClass.h` wie folgt aussehen (unabhängig davon, ob `-opt[imize]` verwendet wird).

```cppwinrt
// MyClass.h
#pragma once
#include "MyClass.g.h"
 
namespace winrt::MyProject::implementation
{
    struct MyClass : ClassT<MyClass>
    {
        MyClass() = default;
 
        static void StaticMethod();
        void Method();
    };
}
namespace winrt::MyProject::factory_implementation
{
    struct MyClass : ClassT<MyClass, implementation::MyClass>
    {
    };
}
```

In `MyClass.cpp` ist alles enthalten.

```cppwinrt
#include "pch.h"
#include "MyClass.h"
#include "MyClass.g.cpp" // !!It's important that you add this line!!
 
namespace winrt::MyProject::implementation
{
    void MyClass::StaticMethod()
    {
    }
 
    void MyClass::Method()
    {
    }
}
```

Um die einheitliche Konstruktion in einem vorhandenen Projekt zu verwenden, müssen Sie daher die `.cpp`-Datei jeder Implementierung bearbeiten, damit nach dem Einschluss (und der Definition) der Implementierungsklasse `#include <Sub/Namespace/Type.g.cpp>` enthalten ist. Diese Datei enthält die Definitionen der Funktionen, die von der Projektion nicht definiert wurden. So sehen diese Definitionen in der Datei `MyClass.g.cpp` aus.

```cppwinrt
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

Und so wird die Projektion mit effizienten Aufrufen durchgeführt, die direkt auf die Implementierung zugreifen, ohne Aufrufe des Factorycaches und ohne Linkerfehler.

`-opt[imize]` ändert außerdem die Implementierung der Datei `module.g.cpp` (die Datei, die das Implementieren der **DllGetActivationFactory**- und **DllCanUnloadNow**-Exporte der DLL unterstützt) des Projekts: Inkrementelle Builds erfolgen in der Regel weitaus schneller, da die starke Typisierung entfällt, die in C++/WinRT 1.0 erforderlich war. Dies wird häufig als *Factorys ohne Typen* bezeichnet. Ohne `-opt[imize]` enthält die für die Komponente generierte Datei `module.g.cpp` die Definitionen aller Implementierungsklassen – in diesem Beispiel `MyClass.h`. Anschließend wird die Implementierungsfactory für jede Klasse wie folgt direkt erstellt.

```cppwinrt
if (requal(name, L"MyProject.MyClass"))
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
```

Sie müssen auch hier nicht alle Details beachten. Es ist jedoch wichtig zu beachten, dass für alle von der Komponente implementierten Klassen die vollständige Definition erforderlich ist. Dies kann sich erheblich auf die innere Schleife auswirken, da jede Änderung einer einzelnen Implementierung die erneute Kompilierung von `module.g.cpp` verursacht. Mit `-opt[imize]` ist dies nicht mehr der Fall. Stattdessen ändern sich in der Datei `module.g.cpp` zweierlei Dinge. Erstens enthält sie keine Implementierungsklassen mehr. In diesem Beispiel ist `MyClass.h` nicht in ihr enthalten. Stattdessen werden die Implementierungsfactorys ohne Informationen zu ihrer Implementierung erstellt.

```cppwinrt
void* winrt_make_MyProject_MyClass();
 
if (requal(name, L"MyProject.MyClass"))
{
    return winrt_make_MyProject_MyClass();
}
```

Offensichtlich ist es nicht erforderlich, ihre Definitionen einzuschließen, und es ist Aufgabe des Linkers, die Definition der **winrt_make_Component_Class**-Funktion aufzulösen. Sie müssen sich darüber keine Gedanken machen, da in der Datei `MyClass.g.cpp`, die für Sie generiert wird (und die Sie zuvor zur Unterstützung der einheitlichen Konstruktion eingeschlossen haben), auch diese Funktion definiert ist. Hier ist die gesamte Datei `MyClass.g.cpp`, die für dieses Beispiel generiert wird.

```cppwinrt
void* winrt_make_MyProject_MyClass()
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

Wie Sie sehen können, wird die Factory der Implementierung direkt von der **winrt_make_MyProject_MyClass**-Funktion erstellt. Dies bedeutet, dass Sie jede Implementierung ändern können und `module.g.cpp` nicht neu kompiliert werden muss. Nur wenn Sie Windows-Runtime-Klassen hinzufügen oder entfernen, wird `module.g.cpp` aktualisiert und muss neu kompiliert werden.

## <a name="overriding-base-class-virtual-methods"></a>Überschreiben von virtuellen Methoden der Basisklasse

In einer abgeleiteten Klasse können Probleme mit virtuellen Methoden auftreten, wenn sowohl die Basisklasse als auch die abgeleitete Klasse App-definierte Klassen sind, aber die virtuelle Methode in einer über-übergeordneten Windows-Runtime-Klasse definiert ist. In der Praxis geschieht dies beim Ableiten von XAML-Klassen. Im Rest dieses Abschnitts wird das Beispiel aus [Abgeleitete Klassen](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#derived-classes) fortgesetzt.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

Die Hierarchie lautet [**Windows::UI::Xaml::Controls::Page**](/uwp/api/windows.ui.xaml.controls.page) \<- **BasePage** \<- **DerivedPage**. Die **BasePage::OnNavigatedFrom**-Methode überschreibt ordnungsgemäß [**Page::OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom), aber **DerivedPage::OnNavigatedFrom** überschreibt nicht **BasePage::OnNavigatedFrom**.

Hier verwendet **DerivedPage** die **IPageOverrides**-vtable aus **BasePage** wieder, was bedeutet, dass die **IPageOverrides::OnNavigatedFrom**-Methode nicht überschrieben wird. Eine mögliche Lösung erfordert, dass **BasePage** selbst eine Vorlagenklasse ist und die Implementierung vollständig in einer Headerdatei erfolgt, die dadurch entstehende Komplexität ist jedoch nicht akzeptabel.

Um dieses Problem zu umgehen, kann die **OnNavigatedFrom**-Methode in der Basisklasse explizit als virtuell deklariert werden. Dann passiert Folgendes: Der vtable-Eintrag für **DerivedPage::IPageOverrides::OnNavigatedFrom** ruft **BasePage::IPageOverrides::OnNavigatedFrom** auf. Daraufhin ruft der Producer **BasePage::OnNavigatedFrom** auf, wodurch (aufgrund der virtuellen Natur) letztendlich **DerivedPage::OnNavigatedFrom** aufgerufen wird.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        // Note the `virtual` keyword here.
        virtual void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

Dafür ist erforderlich, dass alle Member der Klassenhierarchie den gleichen Rückgabewert und die gleichen Parametertypen der **OnNavigatedFrom**-Methode akzeptieren. Andernfalls sollte die oben genannte Version als virtuelle Methode verwendet und die Alternativen umschlossen werden.

## <a name="important-apis"></a>Wichtige APIs
* [winrt::com_ptr-Strukturvorlage](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from-Funktion](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [winrt::from_abi-Funktionsvorlage](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self-Funktionsvorlage](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements-Strukturvorlage](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make-Funktionsvorlage](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self-Funktionsvorlage](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown::as-Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
