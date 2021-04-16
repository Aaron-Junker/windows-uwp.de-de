---
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: Übersicht über Datenbindung
description: In diesem Thema erfahren Sie, wie Sie in einer UWP-App (Universelle Windows-Plattform) ein Steuerelement (oder ein anderes Benutzeroberflächenelement) an ein einzelnes Element oder ein Elementsteuerelement an eine Sammlung von Elementen binden.
ms.date: 10/05/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cppcx
ms.openlocfilehash: c21ea3baaa5992877d1fc0f695e36dda53f69ce2
ms.sourcegitcommit: 27787a579e497d097382338654ed371b661cc3b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107321711"
---
# <a name="data-binding-overview"></a>Übersicht über Datenbindung

In diesem Thema erfahren Sie, wie Sie in einer UWP-App (Universelle Windows-Plattform) ein Steuerelement (oder ein anderes Benutzeroberflächenelement) an ein einzelnes Element oder ein Elementsteuerelement an eine Sammlung von Elementen binden. Darüber hinaus wird erläutert, wie Sie die Anzeige von Elementen steuern, eine Detailansicht auf Grundlage einer Auswahl implementieren und Daten für die Anzeige umwandeln. Ausführliche Informationen finden Sie unter [Datenbindung im Detail](data-binding-in-depth.md).

## <a name="prerequisites"></a>Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie mit dem Erstellen von UWP-Apps vertraut sind. Eine Anleitung zum Erstellen Ihrer ersten UWP-App finden Sie unter [Erste Schritte mit Windows-Apps](../get-started/index.md).

## <a name="create-the-project"></a>Erstellen des Projekts

Erstellen Sie ein neues Projekt vom Typ **Leere Anwendung (Windows Universal)** . Nennen Sie sie „Schnellstart“.

## <a name="binding-to-a-single-item"></a>Binden an ein einzelnes Element

Jede Bindung besteht aus einem Bindungsziel und einer Bindungsquelle. In der Regel ist das Ziel eine Eigenschaft eines Steuerelements oder anderen Benutzeroberflächenelements, und die Quelle ist eine Eigenschaft einer Klasseninstanz (ein Datenmodell oder ein Ansichtsmodell). In diesem Beispiel wird veranschaulicht, wie Sie ein Steuerelement an ein einzelnes Element binden. Das Ziel ist die **Text**-Eigenschaft eines **TextBlock**. Die Quelle ist eine Instanz einer einfachen Klasse namens **Recording**, die eine Audioaufnahme darstellt. Befassen wir uns zuerst mit der Klasse.

Wenn du C# oder C++/CX verwendest, füge deinem Projekt eine neue Klasse hinzu, und nenne sie **Recording.cs**.

Wenn du [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) verwendest, füge dem Projekt neue **Midl-Datei**-Elemente (.idl) hinzu, benannt wie im folgenden C++/WinRT-Codebeispiellisting gezeigt. Ersetze den Inhalt dieser neuen Dateien durch den im Listing gezeigten [MIDL 3.0](/uwp/midl-3/intro)-Code, erstellen das Projekt, um `Recording.h` und `.cpp` sowie `RecordingViewModel.h` und `.cpp` zu generieren, und füge dann den generierten Dateien Code hinzu, der dem des Listings entspricht. Weitere Informationen zu diesen generierten Dateien und dazu, wie du sie in dein Projekt kopierst, findest du unter [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](../cpp-and-winrt-apis/binding-property.md).

```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }
        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }
        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }
    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

```cppwinrt
// Recording.idl
namespace Quickstart
{
    runtimeclass Recording
    {
        Recording(String artistName, String compositionName, Windows.Globalization.Calendar releaseDateTime);
        String ArtistName{ get; };
        String CompositionName{ get; };
        Windows.Globalization.Calendar ReleaseDateTime{ get; };
        String OneLineSummary{ get; };
    }
}

// RecordingViewModel.idl
import "Recording.idl";

namespace Quickstart
{
    runtimeclass RecordingViewModel
    {
        RecordingViewModel();
        Quickstart.Recording DefaultRecording{ get; };
    }
}

// Recording.h
// Add these fields:
...
#include <sstream>
...
private:
    std::wstring m_artistName;
    std::wstring m_compositionName;
    Windows::Globalization::Calendar m_releaseDateTime;
...

// Recording.cpp
// Implement like this:
...
Recording::Recording(hstring const& artistName, hstring const& compositionName, Windows::Globalization::Calendar const& releaseDateTime) :
    m_artistName{ artistName.c_str() },
    m_compositionName{ compositionName.c_str() },
    m_releaseDateTime{ releaseDateTime } {}

hstring Recording::ArtistName(){ return hstring{ m_artistName }; }
hstring Recording::CompositionName(){ return hstring{ m_compositionName }; }
Windows::Globalization::Calendar Recording::ReleaseDateTime(){ return m_releaseDateTime; }

hstring Recording::OneLineSummary()
{
    std::wstringstream wstringstream;
    wstringstream << m_compositionName.c_str();
    wstringstream << L" by " << m_artistName.c_str();
    wstringstream << L", released: " << m_releaseDateTime.MonthAsNumericString().c_str();
    wstringstream << L"/" << m_releaseDateTime.DayAsString().c_str();
    wstringstream << L"/" << m_releaseDateTime.YearAsString().c_str();
    return hstring{ wstringstream.str().c_str() };
}
...

// RecordingViewModel.h
// Add this field:
...
#include "Recording.h"
...
private:
    Quickstart::Recording m_defaultRecording{ nullptr };
...

// RecordingViewModel.cpp
// Implement like this:
...
Quickstart::Recording RecordingViewModel::DefaultRecording()
{
    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Year(1761);
    releaseDateTime.Month(1);
    releaseDateTime.Day(1);
    m_defaultRecording = winrt::make<Recording>(L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime);
    return m_defaultRecording;
}
...
```

```cppcx
// Recording.h
#include <sstream>
namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;
    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}
        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }
        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }
        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }
        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c_str());
            }
        }
    };
    public ref class RecordingViewModel sealed
    {
    private:
        Recording ^ defaultRecording;
    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Year = 1761;
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }
        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}

// Recording.cpp
#include "pch.h"
#include "Recording.h"
```

Machen Sie als Nächstes die Bindungsquellklasse aus der Klasse verfügbar, die die Markupseite darstellt. Zu diesem Zweck fügen Sie eine Eigenschaft vom Typ **RecordingViewModel** zu **MainPage** hinzu.

Wenn du [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) verwendest, aktualisieren zuerst `MainPage.idl`. Erstelle das Projekt, um `MainPage.h` und `.cpp` neu zu generieren, und führe die Änderungen in diesen generierten Dateien mit denen in deinem Projekt zusammen.

```csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }
        public RecordingViewModel ViewModel{ get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
// Add this property:
import "RecordingViewModel.idl";
...
RecordingViewModel ViewModel{ get; };
...

// MainPage.h
// Add this property and this field:
...
#include "RecordingViewModel.h"
...
    Quickstart::RecordingViewModel ViewModel();

private:
    Quickstart::RecordingViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();
    m_viewModel = winrt::make<RecordingViewModel>();
}
Quickstart::RecordingViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

```cppcx
// MainPage.h
...
#include "Recording.h"

namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel ^ viewModel;
    public:
        MainPage();

        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();
    this->viewModel = ref new RecordingViewModel();
}
```

Der letzte Codeteil ist das Binden eines **TextBlock** an die **ViewModel.DefaultRecording.OneLiner**-Eigenschaft.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Wenn du [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)verwendest, musst du die **MainPage::ClickHandler**-Funktion entfernen, damit das Projekt erstellt werden kann.

Dies ist das Ergebnis.

![Binden eines Textblocks](images/xaml-databinding0.png)

## <a name="binding-to-a-collection-of-items"></a>Binden an eine Sammlung von Elementen

Ein häufiges Szenario ist das Binden an eine Sammlung von Geschäftsobjekten. In C# und Visual Basic stellt die generische [**ObservableCollection&lt;T&gt;**](/dotnet/api/system.collections.objectmodel.observablecollection-1)-Klasse eine gute Wahl für die Datenbindung bei Sammlungen dar, da sie die  [**INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged)-Schnittstelle und die [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged)-Schnittstelle implementiert. Diese Schnittstellen bieten eine Änderungsbenachrichtigung für Bindungen, wenn Elemente hinzugefügt oder entfernt werden oder eine Eigenschaft der Liste selbst geändert wird. Wenn Ihre gebundenen Steuerelemente bei Änderungen an Eigenschaften von Objekten in der Sammlung aktualisiert werden sollen, muss das Geschäftsobjekt auch **INotifyPropertyChanged** implementieren. Weitere Informationen finden Sie unter [Datenbindung im Detail](data-binding-in-depth.md).

Wenn du [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) verwendest, findest du weitere Informationen zum Binden an eine beobachtbare Sammlung unter [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](../cpp-and-winrt-apis/binding-collection.md). Wenn du dieses Thema zuerst liest, wird der Zweck des unten angezeigten C++/WinRT-Codelistings klarer.

In diesem nächsten Beispiel wird eine [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) an eine Sammlung von `Recording`-Objekten gebunden. Beginnen wir, indem wir die Sammlung zum Ansichtsmodell hinzufügen. Fügen Sie einfach diese neuen Member zur **RecordingViewModel**-Klasse hinzu.

```csharp
public class RecordingViewModel
{
    ...
    private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
    public ObservableCollection<Recording> Recordings{ get{ return this.recordings; } }
    public RecordingViewModel()
    {
        this.recordings.Add(new Recording(){ ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
        this.recordings.Add(new Recording(){ ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
        this.recordings.Add(new Recording(){ ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
    }
}
```

```cppwinrt
// RecordingViewModel.idl
// Add this property:
...
#include <winrt/Windows.Foundation.Collections.h>
...
Windows.Foundation.Collections.IVector<IInspectable> Recordings{ get; };
...

// RecordingViewModel.h
// Change the constructor declaration, and add this property and this field:
...
    RecordingViewModel();
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> Recordings();

private:
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_recordings;
...

// RecordingViewModel.cpp
// Update/add implementations like this:
...
RecordingViewModel::RecordingViewModel()
{
    std::vector<Windows::Foundation::IInspectable> recordings;

    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Month(7); releaseDateTime.Day(8); releaseDateTime.Year(1748);
    recordings.push_back(winrt::make<Recording>(L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(11); releaseDateTime.Day(2); releaseDateTime.Year(1805);
    recordings.push_back(winrt::make<Recording>(L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(3); releaseDateTime.Day(12); releaseDateTime.Year(1737);
    recordings.push_back(winrt::make<Recording>(L"George Frideric Handel", L"Serse", releaseDateTime));

    m_recordings = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>(std::move(recordings));
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> RecordingViewModel::Recordings() { return m_recordings; }
...
```

```cppcx
// Recording.h
...
public ref class RecordingViewModel sealed
{
private:
    ...
    Windows::Foundation::Collections::IVector<Recording^>^ recordings;
public:
    RecordingViewModel()
    {
        ...
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1748;
        releaseDateTime->Month = 7;
        releaseDateTime->Day = 8;
        Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1805;
        releaseDateTime->Month = 2;
        releaseDateTime->Day = 11;
        recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1737;
        releaseDateTime->Month = 12;
        releaseDateTime->Day = 3;
        recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
        this->Recordings->Append(recording);
    }
    ...
    property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
    {
        Windows::Foundation::Collections::IVector<Recording^>^ get()
        {
            if (this->recordings == nullptr)
            {
                this->recordings = ref new Platform::Collections::Vector<Recording^>();
            }
            return this->recordings;
        };
    }
};
```

Binden Sie dann eine [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) an die **ViewModel.Recordings**-Eigenschaft.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Wir haben noch keine Datenvorlage für die **Recording**-Klasse bereitgestellt. Daher kann das Benutzeroberflächenframework nur [**ToString**](/dotnet/api/system.object.tostring#System_Object_ToString) für jedes Element in der [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) aufrufen. Die Standardimplementierung von **ToString** ist die Rückgabe des Typnamens.

![Binden einer Listenansicht 1](images/xaml-databinding1.png)

Um dies zu beheben, können wir entweder [**ToString**](/dotnet/api/system.object.tostring#System_Object_ToString) so überschreiben, dass der Wert von **OneLineSummary** zurückgegeben wird, oder wir können eine Datenvorlage bereitstellen. Die Datenvorlagenoption ist eine gängigere Lösung, die außerdem flexibler ist. Sie legen eine Datenvorlage mithilfe der [**ContentTemplate**](/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate)-Eigenschaft eines Inhaltssteuerelements oder mit der [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)-Eigenschaft eines Elementsteuerelements fest. Nachfolgend sind zwei Möglichkeiten zum Entwerfen einer Datenvorlage für **Recording** dargestellt, zusammen mit einer Abbildung des Ergebnisses.

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <TextBlock Text="{x:Bind OneLineSummary}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![Binden einer Listenansicht 2](images/xaml-databinding2.png)

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Margin="6">
                <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                <StackPanel>
                    <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                    <TextBlock Text="{x:Bind CompositionName}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![Binden einer Listenansicht 3](images/xaml-databinding3.png)

Weitere Informationen zur XAML-Syntax finden Sie unter [Erstellen einer Benutzeroberfläche mit XAML](../design/basics/xaml-basics-ui.md). Weitere Informationen zum Steuerelementlayout finden Sie unter [Definieren von Layouts mit XAML](../design/layout/layouts-with-xaml.md).

## <a name="adding-a-details-view"></a>Hinzufügen einer Detailansicht

Sie können auch alle Details der **Recording**-Objekte in [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)-Elementen anzeigen. Dies nimmt jedoch sehr viel Platz in Anspruch. Stattdessen können Sie gerade so viele Daten im Element anzeigen, um es zu identifizieren, und wenn der Benutzer dann eine Auswahl vornimmt, können Sie alle Details des ausgewählten Elements in einem separaten Teil der Benutzeroberfläche anzeigen, der als „Detailansicht“ bezeichnet wird. Diese Anordnung wird auch als „Haupt-/Detailansicht“ oder „Listen-/Detailansicht“ bezeichnet.

Sie haben zwei Möglichkeiten, dieses Verhalten zu implementieren. Sie können die Detailansicht an die [**SelectedItem**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)-Eigenschaft der [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) binden. Alternativ kannst du eine [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) verwenden. In diesem Fall bindest du dann sowohl die **ListView** als auch die Detailansicht an die **CollectionViewSource** (hierdurch wird das derzeit ausgewählte Element für dich behandelt). Die beiden Methoden sind unten aufgeführt, und haben dieselben Ergebnisse (in der Abbildung dargestellt).

> [!NOTE]
> Bisher haben wir in diesem Thema nur die [{x:Bind}-Markuperweiterung](../xaml-platform/x-bind-markup-extension.md) verwendet. Die beiden weiter unten aufgeführten Methoden benötigen jedoch die flexiblere (aber weniger leistungsfähige) [{Binding}-Markuperweiterung](../xaml-platform/binding-markup-extension.md).

Wenn du C++/WinRT oder Visual C++-Komponentenerweiterungen (C++/CX) verwendest, musst du, um die [{Binding}](../xaml-platform/binding-markup-extension.md)-Markuperweiterung zu verwenden, das [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)-Attribut einer beliebigen Laufzeitklasse hinzufügen, an die du binden möchtest. Um [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) zu verwenden, ist dieses Attribut nicht erforderlich.

> [!IMPORTANT]
> Wenn Sie [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) verwenden, ist das Attribut [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) verfügbar, wenn Sie die Version 10.0.17763.0 oder höher des Windows SDK (Windows 10, Version 1809) installiert haben. Ohne dieses Attribut musst du die Schnittstellen [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) und [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) implementieren, um die [{Binding}](../xaml-platform/binding-markup-extension.md)-Markuperweiterung verwenden zu können.

Sehen wir uns zuerst die [**SelectedItem**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)-Methode an.

```csharp
// No code changes necessary for C#.
```

```cppwinrt
// Recording.idl
// Add this attribute:
...
[Windows.UI.Xaml.Data.Bindable]
runtimeclass Recording
...
```

```cppcx
[Windows::UI::Xaml::Data::Bindable]
public ref class Recording sealed
{
    ...
};
```

Darüber hinaus muss nur noch eine Änderung am Markup vorgenommen werden.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

Fügen Sie für die [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)-Methode zuerst eine **CollectionViewSource** als Seitenressource hinzu.

```xml
<Page.Resources>
    <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
</Page.Resources>
```

Passen Sie dann die Bindungen in der [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) (die nicht mehr benannt werden muss) und in der Detailansicht so an, dass die [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) verwendet wird. Beachten Sie, dass Sie durch die direkte Bindung der Detailansicht an die **CollectionViewSource** implizieren, dass Sie in Bindungen, in denen der Pfad in der Sammlung selbst nicht gefunden werden kann, an das aktuelle Element binden möchten. Die **CurrentItem**-Eigenschaft muss nicht als Pfad für die Bindung angegeben werden, obwohl dies im Zweifelsfall möglich ist.

```xml
...
<ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">
...
<StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
...
```

Nachfolgend ist das identische Ergebnis für die beiden Methoden dargestellt.

> [!NOTE]
> Wenn du C++ verwendest, sieht deine Benutzeroberfläche nicht genau wie in der folgenden Abbildung aus: das Rendering der **ReleaseDateTime**-Eigenschaft unterscheidet sich. Dieser Aspekt wird im folgenden Abschnitt weiter besprochen.

![Binden einer Listenansicht 4](images/xaml-databinding4.png)

## <a name="formatting-or-converting-data-values-for-display"></a>Formatieren oder Konvertieren von Datenwerten für die Anzeige

Es gibt ein Problem mit dem oben erwähnten Rendering. Die **ReleaseDateTime**-Eigenschaft ist nicht nur ein Datum, sondern ein [**DateTime**](/uwp/api/windows.foundation.datetime) (wenn du C++ verwendest, ist es ein [**Calendar**](/uwp/api/windows.globalization.calendar)). In C# wird sie also mit höherer Genauigkeit angezeigt, als wir benötigen. Und in C++ wird sie als Typname gerendert. Eine Lösung besteht darin, eine Zeichenfolgeneigenschaft zur **Recording**-Klasse hinzuzufügen, die das Äquivalent von `this.ReleaseDateTime.ToString("d")` zurückgibt. Das Benennen dieser Eigenschaft als **ReleaseDate**, würde anzeigen, dass sie ein Datum und nicht ein Datum mit Uhrzeit zurückgibt. Die Benennung als **ReleaseDateAsString** gibt dann auch noch an, dass sie eine Zeichenfolge zurückgibt.

Eine flexiblere Lösung ist die Verwendung eines so genannten „Wertkonverters“. Nachfolgend ist ein Beispiel zum Erstellen Ihres eigenen Wertkonverter aufgeführt. Wenn du C# verwendest, füge deiner `Recording.cs`-Quellcodedatei den folgenden Code hinzu. Wenn du C++/WinRT verwendest, füge dem Projekt ein neues **Midl-Datei** element (.idl) hinzu, das, wie im unten stehenden C++/WinRT-Codebeispiellisting gezeigt, benannt wird, erstelle das Projekt, um `StringFormatter.h` und `.cpp` zu generieren, füge diese Dateien deinem Projekt hinzu, und füge dann Codeauflistungen in diese ein. Füge außerdem 9`#include "StringFormatter.h"` zu `MainPage.h` hinzu.

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// pch.h
...
#include <winrt/Windows.Globalization.h>

// StringFormatter.idl
namespace Quickstart
{
    runtimeclass StringFormatter : [default] Windows.UI.Xaml.Data.IValueConverter
    {
        StringFormatter();
    }
}

// StringFormatter.h
#pragma once

#include "StringFormatter.g.h"
#include <sstream>

namespace winrt::Quickstart::implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter>
    {
        StringFormatter() = default;

        Windows::Foundation::IInspectable Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
        Windows::Foundation::IInspectable ConvertBack(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
    };
}

namespace winrt::Quickstart::factory_implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter, implementation::StringFormatter>
    {
    };
}

// StringFormatter.cpp
#include "pch.h"
#include "StringFormatter.h"
#include "StringFormatter.g.cpp"

namespace winrt::Quickstart::implementation
{
    Windows::Foundation::IInspectable StringFormatter::Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar valueAsCalendar{ value.as<Windows::Globalization::Calendar>() };

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar.MonthAsNumericString().c_str();
        wstringstream << L"/" << valueAsCalendar.DayAsString().c_str();
        wstringstream << L"/" << valueAsCalendar.YearAsString().c_str();
        return winrt::box_value(hstring{ wstringstream.str().c_str() });
    }

    Windows::Foundation::IInspectable StringFormatter::ConvertBack(Windows::Foundation::IInspectable const& /* value */, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        throw hresult_not_implemented();
    }
}
```

```cppcx
...
public ref class StringFormatter sealed : Windows::UI::Xaml::Data::IValueConverter
{
public:
    virtual Platform::Object^ Convert(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar^ valueAsCalendar = dynamic_cast<Windows::Globalization::Calendar^>(value);

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar->MonthAsNumericString()->Data();
        wstringstream << L"/" << valueAsCalendar->DayAsString()->Data();
        wstringstream << L"/" << valueAsCalendar->YearAsString()->Data();
        return ref new Platform::String(wstringstream.str().c_str());
    }

    // No need to implement converting back on a one-way binding
    virtual Platform::Object^ ConvertBack(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        throw ref new Platform::NotImplementedException();
    }
};
...
```

> [!NOTE]
> Für das obige C++/WinRT-Codelisting in `StringFormatter.idl` verwenden wir das [default-Attribut](/windows/desktop/midl/default), um **IValueConverter** als Standardschnittstelle zu deklarieren. Im Listing verfügt **StringFormatter** nur über einen Konstruktor und keine Methoden, sodass keine Standardschnittstelle dafür generiert wird. Das `default`-Attribut ist optimal, wenn du **StringFormatter** keine Instanzmember hinzufügst, da keine „QueryInterface“ erforderlich ist, um die **IValueConverter**-Methoden aufzurufen. Alternativ kannst du zur Generierung einer **IStringFormatter-** -Standardschnittstelle auffordern, was du erreichst, indem du die Laufzeitklasse selbst mit dem [default_interface-Attribut](/uwp/midl-3/predefined-attributes#the-default_interface-attribute) kommentierst. Diese Option ist optimal, wenn du **StringFormatter** Instanzmember hinzufügst, die häufiger als die Methoden von **IValueConverter** aufgerufen werden, weil dann keine „QueryInterface“ erforderlich ist, um die Instanzmember aufzurufen.

Nun können wir eine Instanz von **StringFormatter** als Seitenressource hinzufügen und sie in der Bindung von **TextBlock** verwenden, in dem die **ReleaseDateTime**-Eigenschaft angezeigt wird.

```xml
<Page.Resources>
    <local:StringFormatter x:Key="StringFormatterValueConverter"/>
</Page.Resources>
...
<TextBlock Text="{Binding ReleaseDateTime,
    Converter={StaticResource StringFormatterValueConverter},
    ConverterParameter=Released: \{0:d\}}"/>
...
```

Wie du oben sehen kannst, verwenden wir für die Flexibilität der Formatierung das Markup, um eine Formatzeichenfolge mithilfe des Konverterparameters in den Konverter zu übergeben. In den in diesem Thema gezeigten Codebeispielen verwendet nur der C#-Wertekonverter diesen Parameter. Du könntest jedoch problemlos eine C++-artige Formatzeichenfolge als Konverterparameter übergeben und diese in deinem Wertekonverter mit einer Formatierungsfunktion verwenden, wie z. B. **wprintf** oder **swprintf**.

Dies ist das Ergebnis.

![Anzeigen eines Datums mit benutzerdefinierter Formatierung](images/xaml-databinding5.png)

> [!NOTE]
> Ab Windows 10, Version 1607, wird über das XAML-Framework ein integrierter Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand bereitgestellt. Der Konverter verknüpft **true** mit dem Enumerationswert **Visibility.Visible** und **false** mit dem Wert **Visibility.Collapsed**, sodass Sie eine Visibility-Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Für die Verwendung des integrierten Konverters muss die SDK-Zielversion der App mindestens 14393 lauten. Die Verwendung ist nicht möglich, wenn Ihre App für frühere Versionen von Windows 10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](../debug-test-perf/version-adaptive-code.md).

## <a name="see-also"></a>Siehe auch
* [Datenbindung](index.md)