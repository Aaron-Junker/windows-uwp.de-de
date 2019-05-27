---
description: Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *observable*-Eigenschaft bezeichnet. Dieses Thema zeigt, wie man eine Observable-Eigenschaft implementiert und nutzt und wie man ein XAML-Steuerelement daran bindet.
title: XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, XAML, steuerelement, binden, eigenschaft
ms.localizationpriority: medium
ms.openlocfilehash: 2fe5c03eebd2b68e98ae908ea4624471fbd2b3d2
ms.sourcegitcommit: d23dab1533893b7fe0f01ca6eb273edfac4705e6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/15/2019
ms.locfileid: "65627670"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft
Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *observable*-Eigenschaft bezeichnet. Dieses Konzept basiert auf dem Software-Design-Muster, das als *Observer-Pattern* bekannt ist. In diesem Thema wird gezeigt, wie zum Implementieren von Observable-Eigenschaften in [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), und wie sie XAML-Steuerelemente binden.

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe, die Ihr Verständnis für die Verwendung von Laufzeitklassen mit C++/WinRT unterstützen, finden Sie unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) und [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>Was bedeutet *observable* für eine Eigenschaft?
Angenommen, eine Laufzeitklasse namens **BookSku** hat eine Eigenschaft namens **Title**. Wenn **BookSku** das Ereignis [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) auslöst, wenn sich der Wert von **Title** ändert, dann ist **Title** eine Observable-Eigenschaft. Es ist das Verhalten von **BookSku** (Auslösen oder nicht Auslösen des Ereignisses), das bestimmt, welche seiner Eigenschaften Observable-Eigenschaften sind.

Ein XAML-Textelement oder -Steuerelement kann sich an diese Ereignisse binden und sie verarbeiten, indem es den/die aktualisierten Wert(e) abruft und dann eine Aktualisierung durchführt, um den neuen Wert anzuzeigen.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++WinRT Visual Studio-Erweiterung (VSIX) und das NuGet-Paket (die zusammen bieten die Projektvorlage und Buildunterstützung) finden Sie unter [Visual Studio-Unterstützung für C++"/ WinRT"](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="create-a-blank-app-bookstore"></a>Erstellen einer leeren App (Bookstore)
Erstellen Sie zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie eine **leere App (C++"/ WinRT")** Projekt, und nennen Sie sie *Bookstore*.

Wir werden eine neue Klasse schreiben, um ein Buch mit einer Observable-Eigenschaft namens „Titel” darzustellen. Wir erstellen und nutzen die Klasse innerhalb derselben Kompilierungseinheit. Aber wir wollen in der Lage sein, aus XAML eine Bindung an diese Klasse zu nutzen. Daher wird es eine Laufzeitklasse sein. Und wir werden C++/WinRT verwenden, um diese zu schreiben und zu nutzen.

Der erste Schritt beim Erstellen einer neuen Laufzeitklasse besteht darin, dem Projekt ein neues **Midl-Datei (.idl)**-Element hinzuzufügen. Nennen Sie es `BookSku.idl`. Löschen Sie den Standardinhalt von `BookSku.idl` und fügen Sie diese Laufzeitklassendeklaration ein.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> Die ViewModel-Klassen&mdash;tatsächlich alle Runtime-Klasse, die Sie in Ihrer Anwendung deklarieren&mdash;müssen nicht von einer Basisklasse abgeleitet. Die **BookSku** Klasse deklariert wird, oben ist ein Beispiel. Sie implementiert eine Schnittstelle, aber es nicht von einer Basisklasse ableiten.
>
> Common Language Runtime-Klasse, die Sie in der Anwendung deklarieren, *ist* leiten Sie von einer Basis Klasse heißt eine *zusammensetzbar* Klasse. Und es gibt Einschränkungen zusammensetzbar Klassen. Für eine Anwendung übergeben die [Windows App Certification Kit](../debug-test-perf/windows-app-certification-kit.md) Tests durch Visual Studio und dem Microsoft Store zum Überprüfen von Übermittlungen verwendet (und somit auch für die Anwendung wurde erfolgreich in den Microsoft Store erfasst werden), zusammensetzbare Klasse muss letztendlich von einer Windows-Basisklasse abgeleitet werden. Das bedeutet, dass die Klasse im sehr Stamm der Vererbungshierarchie einen Typ, die in einem Windows-Namespace stammen sein muss. Wenn Sie eine Common Language Runtime-Klasse, die von einer Basisklasse abgeleitet werden müssen&mdash;implementiert beispielsweise eine **BindableBase** -Klasse für alle Ihre Ansichtsmodelle abgeleitet&mdash;dann abgeleitet werden können [ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).
>
> Ein Ansichtsmodell ist eine Abstraktion einer Ansicht, und es wird also direkt auf die Sicht (das XAML-Markup) gebunden. Ein Datenmodell ist eine Abstraktion der Daten, und es ist nur über Ihre Ansichtsmodelle genutzt und direkt in XAML nicht gebunden. Daher können Sie Ihre Datenmodelle nicht als Common Language Runtime-Klassen, sondern als C++-Strukturen oder Klassen deklarieren. Sie müssen nicht in MIDL deklariert werden, und Sie können beliebige Hierarchie der klassenvererbung verwenden.

Speichern Sie die Datei, und erstellen Sie das Projekt. Während des Buildprozesses wird das `midl.exe`-Tool ausgeführt, um eine Windows-Runtime-Metadaten-Datei (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) zu erstellen, die die Laufzeitklasse beschreibt. Dann wird das `cppwinrt.exe`-Tool ausgeführt, um Quellcodedateien zu erzeugen, die Sie bei der Erstellung und Nutzung Ihrer Laufzeitklasse unterstützen. Diese Dateien enthalten Stubs, um mit der Implementierung der **BookSku**-Laufzeitklasse zu beginnen, die Sie in Ihrer IDL deklariert haben. Diese Stubs sind `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` und `BookSku.cpp`.

Maustaste auf den Projektknoten, und klicken Sie auf **Ordner in Datei-Explorer öffnen**. Dadurch wird den Projektordner im Datei-Explorer geöffnet. Kopieren Sie die Stub-Dateien, `BookSku.h` und `BookSku.cpp` aus der `\Bookstore\Bookstore\Generated Files\sources\` Ordner und in den Projektordner, d.h. `\Bookstore\Bookstore\`. In **Projektmappen-Explorer**, der Knoten "Projekt" ausgewählt ist, stellen Sie sicher, dass **alle Dateien anzeigen** eingeschaltet ist. Klicken Sie mit der rechten Maustaste auf die kopierten Stub-Dateien und klicken Sie auf **In Projekt aufnehmen**.

## <a name="implement-booksku"></a>Implementieren von **BookSku**
Nun öffnen wir `\Bookstore\Bookstore\BookSku.h` und `BookSku.cpp` und implementieren unsere Laufzeitklasse. Fügen Sie in `BookSku.h` einen Konstruktor hinzu, der ein [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)-Argument (ein privates Mitglied zum Speichern des Titels) und eine weiteres für das bei einer Titeländerung ausgelöste Ereignis entgegen nimmt. Nach diesen Änderungen Ihrer `BookSku.h` sieht wie folgt.

```cppwinrt
// BookSku.h
#pragma once
#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

Implementieren Sie in `BookSku.cpp` die Funktionen wie folgt.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"
#include "BookSku.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

In der **Titel** Mutator-Funktion überprüft werden, ob ein Wert festgelegt wird, die von den aktuellen Wert unterscheidet. Und falls Ja, wir aktualisieren Sie den Titel und zudem Auslösen der [ **INotifyPropertyChanged::PropertyChanged** ](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) Ereignis mit einem Argument gleich dem Namen der Eigenschaft, die geändert wurde. Dies dient dazu, dass die Benutzeroberfläche (UI) erkennt, welcher Eigenschaftswert erneut abgefragt werden muss.

## <a name="declare-and-implement-bookstoreviewmodel"></a>**BookstoreViewModel** deklarieren und implementieren
Unsere XAML-Hauptseite wird sich an ein Hauptansichtsmodell gebunden. Dieses Ansichtsmodell wird mehrere Eigenschaften haben, darunter eine vom Typ **BookSku**. In diesem Schritt deklarieren und implementieren wir unsere Hauptansichtsmodell-Laufzeitklasse.

Fügen Sie eine neue **Midl-Datei (.idl)** mit dem Namen `BookstoreViewModel.idl` hinzu.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

Speichern und erstellen Sie das Projekt. Kopieren Sie `BookstoreViewModel.h` und `BookstoreViewModel.cpp` aus dem `Generated Files\sources`-Ordner in den Projektordner und nehmen Sie sie in das Projekt auf. Öffnen Sie diese Dateien, und implementieren Sie die Common Language Runtime-Klasse, wie unten dargestellt. Beachten Sie wie im `BookstoreViewModel.h`, wir sind einschließlich `BookSku.h`, die deklariert wird, des Implementierungstyps für **BookSku** (d.h. **Winrt::Bookstore::implementation::BookSku**). Und wir sind entfernen `= default` vom Standardkonstruktor.

```cppwinrt
// BookstoreViewModel.h
#pragma once
#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();

        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"
#include "BookstoreViewModel.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> Der Typ des `m_bookSku` ist der projizierte Typ (**Winrt::Bookstore::BookSku**), und die Template-Parameter, die mit [ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) ist die Implementierungstyp (**Winrt::Bookstore::implementation::BookSku**). Dennoch gibt **make** eine Instanz des projizierten Typs zurück.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Hinzufügen einer Eigenschaft vom Typ **BookstoreViewModel** zu **MainPage**
Öffnen Sie `MainPage.idl` (unsere Haupt-UI-Seite) mit der Deklaration der Laufzeitklasse. Fügen Sie eine Import-Anweisung zum Import von `BookstoreViewModel.idl` hinzu und fügen Sie eine schreibgeschützte Eigenschaft namens MainViewModel vom Typ **BookstoreViewModel** hinzu. Entfernen Sie auch die **MyProperty** Eigenschaft. Beachten Sie außerdem die `import` in der Liste unten die Richtlinie.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Speichern Sie die Datei. Erstellen des Projekts wird nicht vollständig im Moment, aber jetzt ist ein Vorteil, weil es die Quellcodedateien in dem generiert die **"MainPage"** -Runtime-Klasse implementiert ist (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` und `MainPage.cpp`). So fahren Sie fort, und erstellen Sie jetzt. Der Build Fehler erwartet werden können in dieser Phase finden Sie unter **"MainViewModel": ist kein Mitglied von "Winrt::Bookstore::implementation::MainPage"**.

Wenn Sie das Einschließen von weglassen `BookstoreViewModel.idl` (finden Sie in der Auflistung der `MainPage.idl` oben), und klicken Sie dann die Fehlermeldung angezeigt werden **erwartet \< in Ihrer Nähe "MainViewModel"**. Ein weiterer Tipp ist, um sicherzustellen, dass Sie alle Typen im selben Namespace lassen: der Namespace, der in den codeauflistungen angezeigt wird.

Um den Fehler zu beheben, die wir erwarten Sie nun müssen die Accessor-Stubs für Kopieren der **"MainViewModel"** -Eigenschaft aus die generierten Dateien (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` und `MainPage.cpp`) und in `\Bookstore\Bookstore\MainPage.h` und `MainPage.cpp`. Die Schritte hierzu werden nachfolgend beschrieben.

In `\Bookstore\Bookstore\MainPage.h`, umfassen `BookstoreViewModel.h`, die deklariert wird, des Implementierungstyps für **BookstoreViewModel** (d.h. **Winrt::Bookstore::implementation::BookstoreViewModel**). Fügen Sie einen privaten Member zum Speichern des Ansichtsmodells hinzu. Beachten Sie, dass die Accessor-Funktion (und die Member M_mainViewModel), im Hinblick auf den projizierten Typ für implementiert werden **BookstoreViewModel** (d.h. **Bookstore::BookstoreViewModel**). Der Implementierungstyp im selben Projekt (Kompilierungseinheit) wie die Anwendung, daher erstellen wir M_mainViewModel über die Überladung des Konstruktors, der verwendet wird `nullptr_t`. Entfernen Sie auch die **MyProperty** Eigenschaft.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

In `\Bookstore\Bookstore\MainPage.cpp`, rufen Sie [ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) (mit den Implementierungstyp) M_mainViewModel eine neue Instanz der projizierten Typ zuweisen. Weisen Sie einen Initialwert für den Titel des Buches zu. Implementieren Sie die Zugriffsfunktion für die MainViewModel-Eigenschaft. Aktualisieren Sie zum Schluss den Titel des Buches im Event-Handler der Schaltfläche. Entfernen Sie auch die **MyProperty** Eigenschaft.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Binden Sie die Schaltfläche an die **Title**-Eigenschaft.
Öffnen Sie `MainPage.xaml` mit dem XAML-Markup für unsere UI-Hauptseite. Wie in der folgenden Liste gezeigt wird, entfernen Sie den Namen der Schaltfläche, und ändern die **Content** -Eigenschaftswert von Literal zu einem Bindungsausdruck. Beachten Sie die Eigenschaft `Mode=OneWay` des Bindungsausdrucks (einseitig vom Ansichtsmodell zum UI). Ohne diese Eigenschaft reagiert die Benutzeroberfläche nicht auf Ereignisse zu Eigenschaftsänderungen.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Erstellen Sie nun das Projekt und führen Sie es aus. Klicken Sie auf die Schaltfläche, um den **Click**-Ereignis-Handler auszuführen. Dieser Handler ruft die Zugriffsfunktion des Buches auf; diese Zugriffsfunktion löst ein Ereignis aus, um die Benutzeroberfläche wissen zu lassen, dass sich die **Title**-Eigenschaft geändert hat; die Schaltfläche fragt den Wert dieser Eigenschaft erneut ab, um ihren eigenen **Content**-Wert zu aktualisieren.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Verwenden die Markuperweiterung {Binding} mit C++ / WinRT
Für die derzeit veröffentlichte Version von C++ / WinRT, damit Sie die {Binding}-Markuperweiterung verwenden, zum implementieren müssen, der [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) und [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) Schnittstellen.

## <a name="important-apis"></a>Wichtige APIs
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Vorlage für WinRT::Make-Funktion](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Erstellen von APIs mit C++/WinRT](author-apis.md)
