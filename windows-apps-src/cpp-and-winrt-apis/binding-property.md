---
description: Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *Observable*-Eigenschaft bezeichnet. Dieses Thema zeigt, wie man eine Observable-Eigenschaft implementiert und nutzt und ein XAML-Steuerelement daran bindet.
title: 'XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft'
ms.date: 09/25/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, XAML, Steuerelement, binden, Eigenschaft
ms.localizationpriority: medium
ms.openlocfilehash: 77155b92c126f2aae7f798c8ecd67cb255182445
ms.sourcegitcommit: bcf60b6d460dc4855f207ba21da2e42644651ef6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2020
ms.locfileid: "91376238"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft

Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *Observable*-Eigenschaft bezeichnet. Dieses Konzept basiert auf dem Softwareentwurfsmuster, das als *Beobachter-Muster* bekannt ist. In diesem Thema erfährst du, wie du beobachtbare Eigenschaften in [C++/WinRT](./intro-to-using-cpp-with-winrt.md) implementierst und XAML-Steuerelemente an sie bindest (Hintergrundinformationen findest Du unter [Datenbindung](../data-binding/index.md)).

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe im Zusammenhang mit der Nutzung und Erstellung von Laufzeitklassen mit C++/WinRT findest du unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) sowie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>Was bedeutet *beobachtbar* für eine Eigenschaft?

Angenommen, eine Laufzeitklasse namens **BookSku** besitzt eine Eigenschaft namens **Title**. Falls **BookSku** bei jeder Änderung des Werts von **Title** das Ereignis [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) auslöst, heißt das, dass **Title** eine beobachtbare Eigenschaft ist. Das Verhalten von **BookSku** (Auslösen oder nicht Auslösen des Ereignisses) bestimmt, welche der zugehörigen Eigenschaften beobachtbar sind.

Ein XAML-Textelement oder Steuerelement kann an diese Ereignisse gebunden werden und sie behandeln. Ein solches Element oder Steuerelement behandelt das Ereignis, indem es die aktualisierten Werte abruft und sich anschließend selbst mit dem neuen Wert aktualisiert.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="create-a-blank-app-bookstore"></a>Erstellen einer leeren App (Bookstore)

Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Erstelle ein Projekt vom Typ **Leere App (C++/WinRT)** , und nenne es *Bookstore*. Stelle sicher, dass **Platzieren Sie die Projektmappe und das Projekt im selben Verzeichnis** deaktiviert ist. Die neueste allgemein verfügbare Version von Windows SDK (d. h. keine Vorschauversion).

Wir erstellen eine neue Klasse, um ein Buch mit einer beobachtbaren Titeleigenschaft darzustellen. Die Klasse wird innerhalb der gleichen Kompilierungseinheit erstellt und genutzt. Da wir jedoch die Möglichkeit haben möchten, über XAML eine Bindung mit dieser Klasse herzustellen, verwenden wir eine Laufzeitklasse. Und wir verwenden C++/WinRT, um sie zu schreiben und zu nutzen.

Um eine neue Laufzeitklasse zu erstellen, müssen wir dem Projekt zunächst ein neues Element vom Typ **Midl-Datei (.idl)** hinzufügen. Benennen Sie das neue Element `BookSku.idl`. Lösche den Standardinhalt von `BookSku.idl`, und füge die folgende Laufzeitklassendeklaration ein:

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        BookSku(String title);
        String Title;
    }
}
```

> [!NOTE]
> Deine ViewModel-Klassen müssen nicht von einer Basisklasse abgeleitet werden. (Das gilt eigentlich für alle Laufzeitklassen, die du in deiner Anwendung deklarierst.) Die oben deklarierte Klasse **BookSku** ist ein Beispiel hierfür. Sie implementiert eine Schnittstelle, ist aber von keiner Basisklasse abgeleitet.
>
> In der Anwendung deklarierte Laufzeitklassen, *die von einer Basisklasse abgeleitet sind*, werden als *zusammensetzbare* Klassen bezeichnet. Für zusammensetzbare Klassen gelten bestimmte Einschränkungen. Damit eine Anwendung die Tests des [Zertifizierungskits für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) besteht, das von Visual Studio sowie vom Microsoft Store zur Überprüfung von Übermittlungen verwendet wird, und erfolgreich in den Microsoft Store aufgenommen werden kann, muss eine zusammensetzbare Klasse letztendlich von einer Windows-Basisklasse abgeleitet sein. Das bedeutet, dass es sich am Stamm der Vererbungshierarchie um einen Klassentyp aus einem Windows.*-Namespace handeln muss. Wenn du eine Laufzeitklasse von einer Basisklasse ableiten musst, um beispielsweise eine Klasse vom Typ **BindableBase** zur Ableitung deiner Ansichtsmodelle zu implementieren, kannst du [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject) als Grundlage für die Ableitung verwenden.
>
> Ein Ansichtsmodell ist eine Abstraktion einer Ansicht und somit direkt an die Ansicht (XAML-Markup) gebunden. Ein Datenmodell ist eine Abstraktion von Daten. Es wird nur über deine Ansichtsmodelle genutzt und nicht direkt an XAML gebunden. Sie können Ihre Datenmodelle also als C++-Strukturen oder -Klassen deklarieren und nicht als Laufzeitklassen. Sie müssen nicht in MIDL deklariert werden, und du kannst eine beliebige Vererbungshierarchie verwenden.

Speichern Sie die Datei, und erstellen Sie das Projekt. Der Build ist noch nicht vollständig erfolgreich, aber es werden einige erforderliche Dinge für uns durchgeführt. Insbesondere wird während des Buildprozesses das Tool `midl.exe` ausgeführt, um eine Windows-Runtime-Metadatendatei (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) zu erstellen, die die Laufzeitklasse beschreibt. Danach wird das Tool `cppwinrt.exe` ausgeführt, um Quellcodedateien zu generieren, die dich bei der Erstellung und Nutzung deiner Laufzeitklasse unterstützen. Diese Dateien enthalten Stubs zur Implementierung der Laufzeitklasse **BookSku**, die du in deiner IDL deklariert hast. Diese Stubs sind `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` und `BookSku.cpp`.

Klicke mit der rechten Maustaste auf den Projektknoten, und klicke auf **Ordner in Datei-Explorer öffnen**. Dadurch wird der Projektordner im Datei-Explorer geöffnet. Kopiere dort die Stub-Dateien `BookSku.h` und `BookSku.cpp` aus dem Ordner `\Bookstore\Bookstore\Generated Files\sources\` in den Projektordner `\Bookstore\Bookstore\`. Vergewissere dich im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicke mit der rechten Maustaste auf die kopierten Stub-Dateien, und klicke auf **Zu Projekt hinzufügen**.

## <a name="implement-booksku"></a>Implementieren von **BookSku**
Als Nächstes öffnen wir `\Bookstore\Bookstore\BookSku.h` und `BookSku.cpp` und implementieren unsere Laufzeitklasse. Zuerst wird eine `static_assert`-Deklaration am Anfang von `BookSku.h` und `BookSku.cpp` angezeigt, die Sie entfernen müssen.

Anschließend nehmen Sie diese Änderungen in `BookSku.h` vor.

- Ändern Sie in Standardkonstruktor `= default` in `= delete`. Das ist erforderlich, da wir keinen Standardkonstruktor wünschen.
- Fügen Sie einen privaten Member zum Speichern der Titelzeichenfolge hinzu. Beachten Sie, dass wir über einen Konstruktor verfügen, der den Wert [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) annimmt. Dieser Wert ist die Titelzeichenfolge.
- Fügen Sie einen weiteren privaten Member für das Ereignis hinzu, das bei einer Änderung des Titels ausgelöst wird.

Nach diesen Änderungen sieht `BookSku.h` wie folgt aus:

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
namespace winrt::Bookstore::factory_implementation
{
    struct BookSku : BookSkuT<BookSku, implementation::BookSku>
    {
    };
}
```

Implementiere die Funktionen in `BookSku.cpp`:

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

In der Mutatorfunktion **Title** wird überprüft, ob ein Wert festgelegt wird, der sich vom aktuellen Wert unterscheidet. Falls ja, aktualisieren wir anschließend den Titel und lösen außerdem das Ereignis [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) mit einem Argument aus, das dem Namen der geänderten Eigenschaft entspricht. Dadurch weiß die Benutzeroberfläche (User Interface, UI), welcher Eigenschaftswert erneut abgefragt werden muss.

Nun wird ein neuer Build des Projekts erstellt, wenn Sie dies überprüfen wollen.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Deklarieren und Implementieren von **BookstoreViewModel**
Unsere XAML-Hauptseite wird an ein Hauptansichtsmodell gebunden. Dieses Ansichtsmodell erhält mehrere Eigenschaften –unter anderem eine vom Typ **BookSku**. In diesem Schritt deklarieren und implementieren wir unsere Laufzeitklasse für das Hauptansichtsmodell.

Füge ein neues Element vom Typ **Midl-Datei (.idl)** namens `BookstoreViewModel.idl` hinzu. Siehe auch [Einbeziehen von Laufzeitklassen in Midl-Dateien (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl).

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookstoreViewModel();
        BookSku BookSku{ get; };
    }
}
```

Speichern und Erstellen (der Build wird noch nicht vollständig erfolgreich sein, aber der Grund für die Erstellung des Builds ist die erneute Generierung von Stubdateien).

Kopiere `BookstoreViewModel.h` und `BookstoreViewModel.cpp` aus dem Ordner `Generated Files\sources` in den Projektordner, und füge sie dem Projekt hinzu. Öffnen Sie die Dateien (entfernen Sie `static_assert` wieder), und implementieren Sie die Laufzeitklasse, wie im Anschluss gezeigt. Beachte, dass wir `BookSku.h` zu `BookstoreViewModel.h` hinzufügen, um den Implementierungstyp **winrt::Bookstore::implementation::BookSku** für **BookSku** zu deklarieren. Außerdem entfernen wir `= default` aus dem Standardkonstruktor.

> [!NOTE]
> In den folgenden Auflistungen für `BookstoreViewModel.h` und `BookstoreViewModel.cpp` veranschaulicht der Code die Standardmethode zur Konstruktion des *m_bookSku*-Datenelements. Das ist die Methode, die auf das erste Release von C++/WinRT zurückgeht, und es ist sinnvoll, zumindest mit dem Muster vertraut zu sein. Mit C++/WinRT Version 2.0 und höher steht Ihnen eine optimierte Form der Konstruktion zur Verfügung, die als *einheitliche Konstruktion* bezeichnet wird (siehe [Neuerungen und Änderungen in C++/WinRT 2.0](./news.md#news-and-changes-in-cwinrt-20)). Später in diesem Thema zeigen wir ein Beispiel für eine einheitliche Konstruktion.

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
namespace winrt::Bookstore::factory_implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel, implementation::BookstoreViewModel>
    {
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
> Der Typ von `m_bookSku` ist der projizierte Typ (**winrt::Bookstore::BookSku**), und der mit [**winrt::make**](/uwp/cpp-ref-for-winrt/make) verwendete Vorlagenparameter ist der Implementierungstyp (**winrt::Bookstore::implementation::BookSku**). Dennoch gibt **make** eine Instanz des projizierten Typs zurück.

Jetzt wird das Projekt erneut erstellt.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Hinzufügen einer Eigenschaft vom Typ **BookstoreViewModel** zu **MainPage**
Öffne `MainPage.idl`. Darin wird die Laufzeitklasse deklariert, die unsere UI-Hauptseite darstellt.

- Fügen Sie eine `import`-Direktive zum Importieren von `BookstoreViewModel.idl` hinzu.
- Fügen Sie eine schreibgeschützte Eigenschaft namens **MainViewModel** vom Typ **BookstoreViewModel** hinzu.
- Entfernen Sie die Eigenschaft **MyProperty**.

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

Speichern Sie die Datei. Das Projekt wird zwar noch nicht vollständig erfolgreich erstellt, die Erstellung ist jedoch hilfreich, da dadurch die Quellcodedateien neu generiert werden, in denen die Laufzeitklasse **MainPage** implementiert ist (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` und `MainPage.cpp`). Erstelle daher als Nächstes das Projekt. In dieser Phase ist mit folgendem Buildfehler zu rechnen: **'MainViewModel' ist kein Member von 'winrt::Bookstore::implementation::MainPage'** .

Wenn du `BookstoreViewModel.idl` nicht hinzufügst (siehe Listing von `MainPage.idl` weiter oben), tritt der Fehler **expecting \< near "MainViewModel"** (In der Nähe von „MainViewModel“ wird < erwartet.) auf. Ein weiterer Tipp: Achten Sie darauf, dass sich alle Typen im gleichen Namespace befinden (und zwar in dem Namespace, der in den Codeauflistungen zu sehen ist).

Zur Behebung des erwarteten Fehlers musst du die Accessor-Stubs für die Eigenschaft **MainViewModel** aus den generierten Dateien (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` und `MainPage.cpp`) kopieren und in `\Bookstore\Bookstore\MainPage.h` und `MainPage.cpp` einfügen. Dies wird als Nächstes beschrieben.

Führen Sie in `\Bookstore\Bookstore\MainPage.h` diese Schritte aus.

- Fügen Sie `BookstoreViewModel.h` hinzu, um den Implementierungstyp **winrt::Bookstore::implementation::BookstoreViewModel** für **BookstoreViewModel** zu deklarieren.
- Füge einen privaten Member zum Speichern des Ansichtsmodells hinzu. Beachten Sie, dass die Accessorfunktion für die Eigenschaft (und der Member *m_mainViewModel*) als projizierter Typ für **BookstoreViewModel** (**Bookstore::BookstoreViewModel**) implementiert wird.
- Da sich der Implementierungstyp im gleichen Projekt (Kompilierungseinheit) befindet wie die Anwendung, konstruieren wir *m_mainViewModel* über die Konstruktorüberladung mit **std::nullptr_t**.
- Entfernen Sie die Eigenschaft **MyProperty**.

> [!NOTE]
> Im folgenden Auflistungspaar für `MainPage.h` und `MainPage.cpp` veranschaulicht der Code die Standardmethode zur Konstruktion des *m_mainViewModel*-Datenelements. Im folgenden Abschnitt wird eine Version gezeigt, die stattdessen eine einheitliche Konstruktion verwendet.

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

Nimm in `\Bookstore\Bookstore\MainPage.cpp`, wie in dem folgenden Listing gezeigt, die folgenden Änderungen vor.

- Rufe [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (mit dem Implementierungstyp**BookstoreViewModel**) auf, um **m_mainViewModel** eine neue Instanz des projizierten Typs *BookstoreViewModel* zuzuweisen. Wie zuvor gesehen, erstellt der **BookstoreViewModel**-Konstruktor ein neues **BookSku**-Objekt als privaten Datenmember, wobei dessen Titel anfänglich auf `L"Atticus"` festgelegt wird.
- Aktualisiere den Buchtitel im Ereignishandler (**ClickHandler**) auf den veröffentlichten Titel.
- Implementieren Sie den Accessor für die **MainViewModel**-Eigenschaft.
- Entfernen Sie die Eigenschaft **MyProperty**.

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

### <a name="uniform-construction"></a>Einheitliche Konstruktion
Um eine einheitliche Konstruktion anstelle von [**winrt::make**](/uwp/cpp-ref-for-winrt/make) zu verwenden, deklarieren und initialisieren Sie *m_mainViewModel* in `MainPage.h` in nur einem Schritt, wie unten gezeigt.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
struct MainPage : MainPageT<MainPage>
{
    ...
private:
    Bookstore::BookstoreViewModel m_mainViewModel;
};
...
```

Im **MainPage**-Konstruktor in `MainPage.cpp` ist der Code `m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();` dann nicht erforderlich.

Weitere Informationen zur einheitlichen Konstruktion und Codebeispiele finden Sie unter [Aktivieren von einheitlicher Konstruktion und direktem Implementierungszugriff](./author-apis.md#opt-in-to-uniform-construction-and-direct-implementation-access).

## <a name="bind-the-button-to-the-title-property"></a>Binden der Schaltfläche an die Eigenschaft **Title**
Öffne `MainPage.xaml`. Darin befindet sich das XAML-Markup für unsere UI-Hauptseite. Entferne wie im folgenden Listing zu sehen den Namen aus der Schaltfläche, und ändere den Wert der Eigenschaft **Content** von einem literalen Ausdruck in einen Bindungsausdruck. Beachte die Eigenschaft `Mode=OneWay` für den Bindungsausdruck (unidirektional vom Ansichtsmodell zur UI). Ohne diese Eigenschaft reagiert die Benutzeroberfläche nicht auf Eigenschaftsänderungsereignisse.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Erstelle nun das Projekt, und führe es aus. Klicke auf die Schaltfläche, um den Ereignishandler für **Click** auszuführen. Der Handler ruft die Titelmutatorfunktion des Buchs auf. Dieser Mutator löst ein Ereignis aus, um die Benutzeroberfläche darauf hinzuweisen, dass sich die Eigenschaft **Title** geändert hat, und die Schaltfläche fragt den Wert dieser Eigenschaft erneut ab, um den eigenen Wert für **Content** zu aktualisieren.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Verwenden der {Binding}-Markuperweiterung mit C++/WinRT
Um in der aktuellen Version von C++/WinRT die {Binding}-Markuperweiterung verwenden zu können, müssen die Schnittstellen [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) und [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) implementiert werden.

## <a name="element-to-element-binding"></a>Element-an-Element-Bindung

Sie können die Eigenschaft eines XAML-Elements an die Eigenschaft eines anderen XAML-Elements binden. Dies ist ein Beispiel hierfür im Markup.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

Sie müssen in der Midl-Datei (.idl) die benannte XAML-Entität `myTextBox` als schreibgeschützte Eigenschaft deklarieren.

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

Dies hat den folgenden Grund. Alle Typen, die der XAML-Compiler überprüfen muss (einschließlich der in [{X:Bind}](../xaml-platform/x-bind-markup-extension.md) verwendeten), werden aus Windows-Metadaten (WinMD) gelesen. Sie müssen lediglich der Midl-Datei die schreibgeschützte Eigenschaft hinzufügen. Implementieren Sie dies nicht, denn die Implementierung erfolgt durch den automatisch generierten XAML-CodeBehind.

## <a name="consuming-objects-from-xaml-markup"></a>Verwenden von Objekten aus XAML-Markup

Alle Entitäten, die über die XAML-[ **{x:Bind}-Markuperweiterung**](../xaml-platform/x-bind-markup-extension.md) genutzt werden, müssen in IDL öffentlich verfügbar gemacht werden. Wenn das XAML-Markup einen Verweis auf ein anderes Element enthält, das sich ebenfalls im Markup befindet, muss darüber hinaus der Getter für dieses Markup in IDL vorhanden sein.

```xaml
<Page x:Name="MyPage">
    <StackPanel>
        <CheckBox x:Name="UseCustomColorCheckBox" Content="Use custom color"
             Click="UseCustomColorCheckBox_Click" />
        <Button x:Name="ChangeColorButton" Content="Change color"
            Click="{x:Bind ChangeColorButton_OnClick}"
            IsEnabled="{x:Bind UseCustomColorCheckBox.IsChecked.Value, Mode=OneWay}"/>
    </StackPanel>
</Page>
```

Das *ChangeColorButton*-Element verweist über eine Bindung auf das *UseCustomColorCheckBox*-Element. Daher muss die IDL für diese Seite eine schreibgeschützte Eigenschaft namens *UseCustomColorCheckBox* deklarieren, um für die Bindung zugänglich zu sein.

Der Delegat für den Click-Ereignishandler für *UseCustomColorCheckBox* verwendet die klassische XAML-Delegatsyntax, benötigt daher keinen Eintrag in der IDL, sondern muss nur in deiner Implementierungsklasse öffentlich sein. Auf der anderen Seite verfügt *ChangeColorButton* auch über einen `{x:Bind}`-Click-Ereignishandler, der in die IDL eingefügt werden muss.

```idl
runtimeclass MyPage : Windows.UI.Xaml.Controls.Page
{
    MyPage();

    // These members are consumed by binding.
    void ChangeColorButton_OnClick();
    Windows.UI.Xaml.Controls.CheckBox UseCustomColorCheckBox{ get; };
}
```

Du musst keine Implementierung für die **UseCustomColorCheckBox**-Eigenschaft bereitstellen. Das übernimmt der XAML-Codegenerator.

### <a name="binding-to-boolean"></a>Binden an einen booleschen Typ

Hierfür kannst du den Diagnosemodus verwenden.

<syntaxhighlight lang="xml">
<TextBlock Text="{Binding CanPair}"/>
</syntaxhighlight>

Dieses Beispiel zeigt `true` oder `false` in C++/CX, aber **Windows.Foundation.IReference`1<Boolean>** in C++/WinRT an.

Verwende `x:Bind` beim Binden an einen booleschen Typ.

```xaml
<TextBlock Text="{x:Bind CanPair}"/>
```

## <a name="important-apis"></a>Wichtige APIs
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make function template](/uwp/cpp-ref-for-winrt/make) (Funktionsvorlage „winrt::make“)

## <a name="related-topics"></a>Zugehörige Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Erstellen von APIs mit C++/WinRT](author-apis.md)