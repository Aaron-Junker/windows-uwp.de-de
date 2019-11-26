---
Description: Fügen Sie einer App für die Freihandeingabe in der universellen Windows-Plattform (UWP) eine Standard-InkToolbar hinzu. Fügen Sie der InkToolbar einen anpassbaren Stift hinzu und binden Sie diesen an eine benutzerdefinierte Definition für den Stift.
title: Hinzufügen von InkToolbar zu einer App für die Universelle Windows-Plattform (UWP)
label: Add an InkToolbar to a Universal Windows Platform (UWP) app
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universelle Windows-Platform, UWP, Benutzerinteraktion, Eingabe
ms.date: 02/08/2017
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: 8ae67e5d4d6da3cc9716c5f0efd276023bae9af0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258379"
---
# <a name="add-an-inktoolbar-to-a-universal-windows-platform-uwp-app"></a>Hinzufügen von InkToolbar zu einer App für die Universelle Windows-Plattform (UWP)



Es gibt zwei verschiedene Steuerelemente, die die Freihandeingabe in Apps in der universellen Windows-Plattform (UWP) vereinfachen: [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) und [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar).

Über das [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)-Steuerelement werden grundlegende Windows Ink-Funktionen bereitgestellt. Rendern Sie damit die Stifteingabe entweder als letzten Strich (mit den Standardeinstellungen für Farbe und Stärke) oder ausradierten Strich.

> Details zur Implementierung von InkCanvas finden Sie unter [Zeichen- und Eingabestiftinteraktionen in UWP-Apps](pen-and-stylus-interactions.md).

Als vollständig transparente Überlagerung bietet InkCanvas keine integrierte Benutzeroberfläche zum Festlegen von Freihandstricheinstellungen. Wenn Sie die Standard-Freihandfunktionen ändern, Benutzern das Festlegen von Freihandstricheinstellungen ermöglichen und andere benutzerdefinierte Freihandeingabefeatures unterstützen möchten, haben Sie zwei Optionen:

- Verwenden Sie im CodeBehind das an InkCanvas gebundene zugrunde liegende [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter)-Objekt.

  Die InkPresenter-APIs unterstützen die umfassende Anpassung der Freihandfunktionen. Weitere Informationen finden Sie unter [Zeichen- und Eingabestiftinteraktionen in UWP-Apps](pen-and-stylus-interactions.md).

- Binden Sie [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) an InkCanvas. InkToolbar bietet standardmäßig eine anpassbare und erweiterbare Sammlung von Schaltflächen zum Aktivieren von Freihandfunktionen wie Strichgröße, Freihandfarbe und Form der Stiftspitze.

  InkToolbar wird in diesem Thema erläutert.

> **Wichtige APIs**: [**InkCanvas class**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [**InkToolbar class**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), [**InkPresenter class**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter), [**Windows.UI.Input.Inking**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="default-inktoolbar"></a>Standard-InkToolbar

Das [**InkToolbar-Steuerelement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) enthält standardmäßig Schaltflächen zum Zeichnen, Löschen, Hervorheben sowie zum Anzeigen einer Schablone(Lineal oder Winkelmesser). Abhängig vom Feature werden in einem Flyout weitere Einstellungen und Befehle bereitgestellt, beispielsweise für Freihandfarbe, Strichstärke und das Löschen aller Freihandeingaben.

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*Standard Symbolleiste von Windows Ink*

Um eine standardmäßige [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) zu einer App für die Freihandeingabe hinzuzufügen, platzieren Sie sie einfach auf derselben Seite wie Ihr [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), und verbinden Sie die beiden Steuerelemente.

1. Deklarieren Sie in „MainPage.xaml“ ein Containerobjekt (in diesem Beispiel verwenden wir ein Grid-Steuerelement) für die Freihand-Oberfläche.
2. Deklarieren Sie ein InkCanvas-Objekt als untergeordnetes Element des Containers. (Die InkCanvas-Größe wird vom Container geerbt.)
3. Deklarieren Sie eine InkToolbar, und verwenden Sie das TargetInkCanvas-Attribut zum Binden an InkCanvas.

> [!NOTE]
> Stellen Sie sicher, dass die InkToolbar nach InkCanvas deklariert wird. Andernfalls verhindert die InkCanvas-Überlagerung den Zugriff auf die InkToolbar.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>Einfache Anpassung

In diesem Abschnitt behandeln wir einige grundlegende Anpassungsszenarien der Windows Ink-Symbolleiste.

### <a name="specify-location-and-orientation"></a>Ort und Ausrichtung angeben

Wenn Sie eine Freihandsymbolleiste zu Ihrer App hinzufügen, können Sie Standardstandort und -ausrichtung der Symbolleiste übernehmen oder diese gemäß den Anforderungen Ihrer App oder Benutzer festlegen.

**XAML**

Geben Sie den Standort und die Ausrichtung der Symbolleiste explizit über die Eigenschaften [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.VerticalAlignment), [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) und [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar?branch=rs3.Orientation) an.

| Standard | Explizit |
| --- | --- |
| ![Standardstandort und -ausrichtung der Freihandsymbolleiste](./images/ink/location-default-small.png) | ![Explizite Angabe für Standort und Ausrichtung der Freihandsymbolleiste](./images/ink/location-explicit-small.png) |
| *Standard Speicherort und Ausrichtung der Windows Ink-Symbolleiste* | *Explizite Position und Ausrichtung der Windows Ink-Symbolleiste* |

Im Folgenden sehen Sie den Code, mit dem Sie den Standort und die Ausrichtung der Freihandsymbolleiste explizit in XAML festlegen.
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**Initialisieren basierend auf den Benutzereinstellungen oder dem Gerätestatus**

In einigen Fällen möchten Sie möglicherweise den Standort und die Ausrichtung der Freihandsymbolleiste basierend auf Benutzereinstellungen oder Gerätestatus festlegen. Das folgende Beispiel veranschaulicht, wie Sie den Standort und die Ausrichtung der Freihandsymbolleiste basierend auf den Einstellungen für Rechts- oder Linkshänder festlegen, die über **Einstellungen > Geräte > Stift & Windows Ink > Stift > Schreibhand auswählen** angegeben werden.

![dominante Handeinstellung](./images/ink/location-handedness-setting.png)  
*Bestimmende Handeinstellung*

Sie können diese Einstellung über die HandPreference-Eigenschaft von Windows.UI.ViewManagement abfragen und [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) basierend auf dem zurückgegebenen Wert festlegen. In diesem Beispiel platzieren wir die Symbolleiste auf der linken Seite der App für einen Linkshänder und auf der rechten Seite für einen Rechtshänder.

**Herunterladen dieses Beispiels von frei Hand [Symbolleiste Speicherort und Ausrichtung Beispiel (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)**

```csharp
public MainPage()
{
    this.InitializeComponent();

    Windows.UI.ViewManagement.UISettings settings = 
        new Windows.UI.ViewManagement.UISettings();
    HorizontalAlignment alignment = 
        (settings.HandPreference == 
            Windows.UI.ViewManagement.HandPreference.LeftHanded) ? 
            HorizontalAlignment.Left : HorizontalAlignment.Right;
    inkToolbar.HorizontalAlignment = alignment;
}
```

**Dynamisches anpassen an Benutzer-oder Gerätestatus**

Sie können auch eine Bindung verwenden, um nach Aktualisierungen der Benutzeroberfläche basierend auf Benutzereinstellungen, Geräteeinstellungen oder Gerätestatus zu suchen. Im folgenden Beispiel wird das vorherige Beispiel erweitert und gezeigt, wie Sie die Freihandsymbolleiste basierend auf der Ausrichtung des Geräts mithilfe von Bindung, einem ViewMOdel-Objekt und der [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)-Schnittstelle dynamisch positionieren. 

**Dieses Beispiel von frei Hand [Symbolleiste Speicherort und-Ausrichtung herunterladen (dynamisch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)**

1. Zuerst fügen wir unser ViewModel hinzu.
    1. Fügen Sie Ihrem Projekt einen neuen Ordner hinzu, und nennen Sie ihn **ViewModels**.
    1. Fügen Sie eine neue Klasse zum ViewModels-Ordner hinzu (für dieses Beispiel nennen wir sie **InkToolbarSnippetHostViewModel.cs**).
        > [!NOTE] 
        > Wir haben das [Singleton-Muster](https://docs.microsoft.com/previous-versions/msp-n-p/ff650849(v=pandp.10)) verwendet, da wir nur ein Objekt dieses Typs für die Lebensdauer der Anwendung benötigen.

    1. Fügen Sie den `using System.ComponentModel`-Namespace zu der Datei hinzu.
    1. Fügen Sie eine statische Membervariable namens **instance** und eine statische schreibgeschützte Eigenschaft namens **Instance** hinzu. Sorgen Sie dafür, dass der Konstruktor privat ist, um sicherzustellen, dass nur über die Instance-Eigenschaft auf diese Klasse zugegriffen werden kann.   
        > [!NOTE] 
        > Diese Klasse erbt von der [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)-Schnittstelle, die verwendet wird, um Clients, die in der Regel Bindungsclients sind, zu benachrichtigen, dass sich ein Eigenschaftswert geändert hat. Wir verwenden dies, um Änderungen an der Geräteausrichtung zu verarbeiten (dieser Code wird in einem späteren Schritt erweitert und weiter erläutert).  

        ```csharp
        using System.ComponentModel;

        namespace locationandorientation.ViewModels
        {
            public class InkToolbarSnippetHostViewModel : INotifyPropertyChanged
            {
                private static InkToolbarSnippetHostViewModel instance;
                
                public static InkToolbarSnippetHostViewModel Instance
                {
                    get
                    {
                        if (null == instance)
                        {
                            instance = new InkToolbarSnippetHostViewModel();
                        }
                        return instance;
                    }
                }
            }

            private InkToolbarSnippetHostViewModel() { }
        }
        ```

    1. Fügen Sie zwei boolesche Eigenschaften zur Klasse InkToolbarSnippetHostViewModel hinzu: **LeftHandedLayout** (dieselbe Funktion wie im vorherigen Beispiel reinen XAML-Beispiel) und **PortraitLayout** (Ausrichtung des Geräts).
        >[!NOTE] 
        > Die Eigenschaft PortraitLayout ist einstellbar und enthält die Definition für das Ereignis [PropertyChanged](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged).

        ```csharp
        public bool LeftHandedLayout
        {
            get
            {
                bool leftHandedLayout = false;
                Windows.UI.ViewManagement.UISettings settings =
                    new Windows.UI.ViewManagement.UISettings();
                leftHandedLayout = (settings.HandPreference ==
                    Windows.UI.ViewManagement.HandPreference.LeftHanded);
                return leftHandedLayout;
            }
        }

        public bool portraitLayout = false;
        public bool PortraitLayout
        {
            get
            {
                Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                    Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
                portraitLayout = 
                    (winOrientation == 
                        Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
                return portraitLayout;
            }
            set
            {
                if (value.Equals(portraitLayout)) return;
                portraitLayout = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
            }
        }
        ```

1. Nun fügen wir unserem Projekt verschiedene Klassen des Konverters hinzu. Jede Klasse enthält ein Convert-Objekt, das einen Ausrichtungswert zurückgibt (entweder [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.horizontalalignment) oder [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.verticalalignment)).
    1. Fügen Sie Ihrem Projekt einen neuen Ordner hinzu, und nennen Sie ihn **Converters**.
    1. Fügen Sie zwei neue Klassen zum Converters-Ordner hinzu (für dieses Beispiel nennen wir diese **HorizontalAlignmentFromHandednessConverter.cs** und **VerticalAlignmentFromAppViewConverter.cs**).
    1. Fügen Sie die Namespaces `using Windows.UI.Xaml` und `using Windows.UI.Xaml.Data` zu jeder Datei hinzu.
    1. Ändern Sie jede Klasse in `public`, und geben Sie an, dass damit die [IValueConverter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter)-Schnittstelle implementiert wird.
    1. Fügen Sie die Methoden [Convert](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) und [ConvertBack](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) zu jeder Datei hinzu, wie hier gezeigt (die ConvertBack-Methode wird nicht implementiert).
        - HorizontalAlignmentFromHandednessConverter positioniert die Freihandsymbolleiste auf der rechten Seite der App für Rechtshänder und auf der linken Seite der App für Linkshänder.
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class HorizontalAlignmentFromHandednessConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool leftHanded = (bool)value;
                    HorizontalAlignment alignment = HorizontalAlignment.Right;
                    if (leftHanded)
                    {
                        alignment = HorizontalAlignment.Left;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

        - VerticalAlignmentFromAppViewConverter positioniert die Freihandsymbolleiste in der Mitte der App für das Hochformat und an den Anfang der App für das Querformat (auch wenn dies die Verwendbarkeit verbessern soll, ist es nur eine zufällige Wahl für Demonstrationszwecke).
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class VerticalAlignmentFromAppViewConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool portraitOrientation = (bool)value;
                    VerticalAlignment alignment = VerticalAlignment.Top;
                    if (portraitOrientation)
                    {
                        alignment = VerticalAlignment.Center;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1. Öffnen Sie nun die Datei MainPage.XAML.cs.
    1. Fügen Sie `using using locationandorientation.ViewModels` der Liste der Namespaces hinzu, um das ViewModel zuzuordnen.
    1. Fügen Sie `using Windows.UI.ViewManagement` der Liste der Namespaces hinzu, um das Lauschen auf Änderungen an der Geräte Ausrichtung zu aktivieren.
    1. Fügen Sie den [windowsizechangedeventhandler](https://docs.microsoft.com/uwp/api/windows.ui.xaml.windowsizechangedeventhandler) -Code hinzu.
    1. Legen Sie den [DataContext](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext) für die Ansicht auf die Singleton-Instanz der inktoolbarsnippthostviewmodel-Klasse fest. 
    ```csharp
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;

    using locationandorientation.ViewModels;
    using Windows.UI.ViewManagement;

    namespace locationandorientation
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();

                Window.Current.SizeChanged += (sender, args) =>
                {
                    ApplicationView currentView = ApplicationView.GetForCurrentView();

                    if (currentView.Orientation == ApplicationViewOrientation.Landscape)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = false;
                    }
                    else if (currentView.Orientation == ApplicationViewOrientation.Portrait)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = true;
                    }
                };

                DataContext = InkToolbarSnippetHostViewModel.Instance;
            }
        }
    }
    ```

1. Öffnen Sie als nächstes die Datei "MainPage. XAML".
    1. Fügen Sie `xmlns:converters="using:locationandorientation.Converters"` dem `Page`-Element hinzu, um die Bindung an unsere Konverter zu binden.
        ```xaml
        <Page
        x:Class="locationandorientation.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:locationandorientation"
        xmlns:converters="using:locationandorientation.Converters"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        ```

    1. Fügen Sie ein `PageResources` Element hinzu, und geben Sie Verweise auf unsere Konverter an.
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. Fügen Sie die Elemente InkCanvas und inktoolbar hinzu, und binden Sie die Eigenschaften VerticalAlignment und HorizontalAlignment von inktoolbar.
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. Kehren Sie zur Datei "InkToolbarSnippetHostViewModel.cs" zurück, um die `PortraitLayout`-und `LeftHandedLayout` booleschen Eigenschaften zur `InkToolbarSnippetHostViewModel`-Klasse zusammen mit der Unterstützung für das erneute Binden `PortraitLayout` hinzuzufügen, wenn sich der Eigenschafts Wert ändert 
    ```csharp
    public bool LeftHandedLayout
    {
        get
        {
            bool leftHandedLayout = false;
            Windows.UI.ViewManagement.UISettings settings =
                new Windows.UI.ViewManagement.UISettings();
            leftHandedLayout = (settings.HandPreference ==
                Windows.UI.ViewManagement.HandPreference.LeftHanded);
            return leftHandedLayout;
        }
    }

    public bool portraitLayout = false;
    public bool PortraitLayout
    {
        get
        {
            Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            portraitLayout = 
                (winOrientation == 
                    Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
            return portraitLayout;
        }
        set
        {
            if (value.Equals(portraitLayout)) return;
            portraitLayout = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
        }
    }

    #region INotifyPropertyChanged Members

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }

    #endregion
    ```

Sie sollten jetzt über eine Freihand-App verfügen, die sich sowohl an die vorherrschende als auch an den Benutzer anpasst und dynamisch auf die Ausrichtung des Geräts des Benutzers antwortet.

### <a name="specify-the-selected-button"></a>Angeben der ausgewählten Schaltfläche  
![Stift Schaltfläche bei Initialisierung ausgewählt](./images/ink/ink-tools-default-toolbar.png)  
*Windows Ink-Symbolleiste mit aktivierter Schaltfläche bei der Initialisierung ausgewählt*

Standardmäßig wird die erste (oder äußerste linke) Schaltfläche aktiviert, wenn die App gestartet und die Symbolleiste initialisiert wird. In der standardmäßigen Windows Ink-Symbolleiste ist dies die Kugelschreiberschaltfläche.

Da das Framework die Reihenfolge der integrierten Schaltflächen definiert, ist möglicherweise die erste Schaltfläche nicht der Stift oder das Tool, das Sie standardmäßig aktivieren möchten.

Sie können das Standardverhalten überschreiben und die ausgewählte Schaltfläche auf der Symbolleiste angeben.

In diesem Beispiel initialisieren wir die standardmäßige Symbolleiste mit ausgewählter Stiftschaltfläche und aktiviertem Stift (anstelle des Kugelschreibers).

1. Verwenden Sie die XAML-Deklaration für InkCanvas und InkToolbar aus dem vorherigen Beispiel.
2. Richten Sie im CodeBehind einen Handler für das [Loaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded)-Ereignis des [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)-Objekts ein.

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. Im Handler für das [Loaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded)-Ereignis:
    1. Rufen Sie einen Verweis auf die integrierte [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) ab.

    Das Übergeben eines [InkToolbarTool.Pencil](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbartool)-Objekts in der [GetToolButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.gettoolbutton)-Methode gibt ein [InkToolbarToolButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbartoolbutton)-Objekt für [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) zurück.

    2. Legen Sie [ActiveTool](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.activetool) auf das im vorherigen Schritt zurückgegebene Objekt fest.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>Angeben der integrierten Schaltflächen

![bestimmte Schaltflächen, die bei der Initialisierung enthalten](./images/ink/ink-tools-specific.png)  
*Bestimmte, bei der Initialisierung enthaltene Schaltflächen*

Wie bereits erwähnt, enthält die Windows Ink-Symbolleiste eine Sammlung von standardmäßigen, integrierten Schaltflächen. Diese Schaltflächen werden in der folgenden Reihenfolge angezeigt (von links nach rechts):

- [Inktoolbarballpointpobutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)
- [Inktoolbarpcilbutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)
- [Inktoolbarhighlighterbutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarhighlighterbutton)
- [Inktoolbareraserbutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)
- [Inktoolbarrulerbutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarrulerbutton)

In diesem Beispiel initialisieren wir die Symbolleiste nur mit den integrierten Schaltflächen für Kugelschreiber, Stift und Radierer.

Verwenden Sie hierzu XAML oder CodeBehind.

**XAML**

Ändern Sie die XAML-Deklaration für InkCanvas und InkToolbar aus dem ersten Beispiel.
- Fügen Sie ein [InitialControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols)-Attribut hinzu, und legen Sie dessen Wert auf „[None](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)“ fest. Damit löschen Sie die Standardsammlung der integrierten Schaltflächen.
- Fügen Sie die für Ihre App erforderlichen spezifischen InkToolbar-Schaltflächen hinzu. Hier fügen wir nur [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) und [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton) hinzu.
> [!NOTE]
> Schaltflächen werden der Symbolleiste in der vom Framework definierten Reihenfolge hinzugefügt, nicht in der hier angegebenen Reihenfolge.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**CodeBehind**
1. Verwenden Sie die XAML-Deklaration für InkCanvas und InkToolbar aus dem ersten Beispiel.

  ```xaml
  <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
          <TextBlock x:Name="Header"
                     Text="Basic ink sample"
                     Style="{ThemeResource HeaderTextBlockStyle}"
                     Margin="10,0,0,0" />
      </StackPanel>
      <Grid Grid.Row="1">
          <Image Source="Assets\StoreLogo.png" />
          <InkCanvas x:Name="inkCanvas" />
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. Richten Sie im CodeBehind einen Handler für das [Loading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loading)-Ereignis des [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)-Objekts ein.

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. Legen Sie [InitialControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) auf „[None](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)“ fest.
4. Erstellen Sie Objektverweise für die für Ihre App erforderlichen Schaltflächen. Hier fügen wir nur [InkToolbarBallpointPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) und [InkToolbarEraserButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton) hinzu.
  > [!NOTE]
  > Schaltflächen werden der Symbolleiste in der vom Framework definierten Reihenfolge hinzugefügt, nicht in der hier angegebenen Reihenfolge.

5. [Fügen Sie](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobjectcollection.add) die Schaltflächen der InkToolbar hinzu.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>Benutzerdefinierte Schaltflächen und Freihandeingabefeatures

Sie können die Sammlung von Schaltflächen (und zugehörige Freihandeingabefeatures) anpassen und erweitern, die über die InkToolbar bereitgestellt werden.

Das InkToolbar-Steuerelement besteht aus zwei unterschiedlichen Gruppen von Schaltflächentypen:

1. Eine Gruppe von „Toolschaltflächen“ mit den integrierten Schaltflächen zum Zeichnen, Radieren und Hervorheben. Hier werden benutzerdefinierte Stifte und Tools hinzugefügt.
> **Beachten Sie** , dass sich die Funktionsauswahl&nbsp;&nbsp;gegenseitig ausschließt.

2. Eine Gruppe von „Umschaltflächen“ mit der integrierten Linealschaltfläche. Hier werden benutzerdefinierte Umschaltflächen hinzugefügt.
> **Beachten Sie**&nbsp;&nbsp;Features sich nicht gegenseitig ausschließen und gleichzeitig mit anderen aktiven Tools verwendet werden können.

Je nach Anwendung und erforderlicher Freihandfunktion können Sie dem InkToolbar-Steuerelement eine der folgenden Schaltflächen (die an die benutzerdefinierten Freihandfunktionen gebunden sind) hinzufügen:

- Benutzerdefinierter Stift: Ein Stift, für den die Farbpaletten- und Stiftspitzeneigenschaften der Freihandeingabe wie Form, Drehung und Größe von der Host-App definiert werden.
- Benutzerdefiniertes Tool: Ein Tool ohne Stift, das von der Host-App definiert wird.
- Benutzerdefiniertes Umschalten: Legt den Zustand eines durch die App definierten Features auf „aktiviert“ oder „deaktiviert“ fest. Wenn die Schaltfläche aktiviert ist, funktioniert das Feature in Verbindung mit dem aktiven Tool.

> **Beachten** Sie&nbsp;&nbsp;Sie die Anzeigereihenfolge der integrierten Schaltflächen nicht ändern können. Die standardmäßige Anzeigereihenfolge lautet wie folgt: Kugelschreiber, Stift, Textmarker, Radierer und Lineal. Benutzerdefinierte Stifte werden an den letzten Standardstift angefügt, benutzerdefinierte Tool-Schaltflächen werden zwischen der letzten Stiftschaltfläche und der Radiererschaltfläche hinzugefügt, und benutzerdefinierte Umschaltflächen werden nach der Linealschaltfläche hinzugefügt. (Benutzerdefinierte Schaltflächen werden in der Reihenfolge hinzugefügt, in der sie angegeben werden.)

### <a name="custom-pen"></a>Benutzerdefinierter Stift

Sie können einen benutzerdefinierten Stift (zur Aktivierung über eine entsprechende Schaltfläche) erstellen, wobei Sie die Tintenfarbpalette und die Eigenschaften der Stiftspitze (Form, Rotation, Größe) definieren.

![benutzerdefinierte kalligrafikschaltfläche](./images/ink/ink-tools-custompen.png)  
*Benutzerdefinierte kalligrafische Stift Schaltfläche*

In diesem Beispiel legen wir einen benutzerdefinierten Stift mit einer breiten Spitze fest, die einfache kalligrafische Freihandstriche ermöglicht. Wir passen außerdem die Sammlung der Pinsel in der auf dem Schaltflächen-Flyout angezeigten Palette an.

**CodeBehind**

Zuerst definieren wir unseren benutzerdefinierten Stift und geben die Zeichnungsattribute im CodeBehind an. Wir verweisen später aus XAML auf diesen benutzerdefinierten Stift.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie „Hinzufügen > Neues Element“ aus.
2. Fügen Sie unter „Visual C# -> Code“ eine neue Klassendatei hinzu, und nennen Sie sie „CalligraphicPen.cs“.
3. Ersetzen Sie in „CalligraphicPen.cs“ den standardmäßigen using-Block durch Folgendes:
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. Geben Sie an, dass die CalligraphicPen-Klasse von [InkToolbarCustomPen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen) abgeleitet wird.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Überschreiben Sie [CreateInkDrawingAttributesCore](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore), um Ihre eigene Pinsel- und Strichgröße anzugeben.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Erstellen Sie ein [InkDrawingAttributes](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes)-Objekt, und legen Sie die [Form der Stiftspitze](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentip), [Drehung der Spitze](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentiptransform), [Strichgröße](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.size) und [Freihandfarbe](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkdrawingattributes.color) fest.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

Als Nächstes fügen wir die erforderlichen Verweise auf den benutzerdefinierten Stift in „MainPage.xaml“ hinzu.

1. Wir deklarieren ein lokales Seitenressourcenverzeichnis, das einen Verweis auf den in „CalligraphicPen.cs“ festgelegten benutzerdefinierten Stift (`CalligraphicPen`) und eine [Pinselsammlung](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BrushCollection) erstellt, die vom benutzerdefinierten Stift unterstützt wird (`CalligraphicPenPalette`).
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. Wir fügen dann eine InkToolbar mit einem untergeordneten [InkToolbarCustomPenButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarcustompenbutton)-Element hinzu.

  Die benutzerdefinierte Stiftschaltfläche enthält zwei in den Seitenressourcen deklarierte statische Ressourcenverweise: `CalligraphicPen` und `CalligraphicPenPalette`.

  Wir geben außerdem den Bereich für den Strichgröße-Schieberegler ([MinStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth), [MaxStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth) und [SelectedStrokeWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty)), den ausgewählten Pinsel ([SelectedBrushIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex)) und das Symbol für die benutzerdefinierte Stiftschaltfläche ([SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)) an.
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>Benutzerdefiniertes Umschalten

Sie können eine benutzerdefinierte Umschaltung (zur Aktivierung über eine entsprechende Schaltfläche) erstellen, um den Zustand eines von der App definierten Features als „aktiviert“ oder „nicht aktiviert“ einzurichten. Wenn die Schaltfläche aktiviert ist, funktioniert das Feature in Verbindung mit dem aktiven Tool.

In diesem Beispiel definieren wir eine Schaltfläche für das benutzerdefinierte Umschalten, die das Freihandzeichnen mit Toucheingabe ermöglicht (standardmäßig ist die Touch-Freihandeingabe nicht aktiviert).

> [!NOTE]  
> Wenn Sie das Freihandzeichnen mit der Toucheingabe unterstützen müssen, sollten Sie es mit einem CustomToggleButton mit dem in diesem Beispiel angegebenen Symbol und der QuickInfo aktivieren.

Typischerweise wird die Toucheingabe für die direkte Manipulation eines Objekts oder der Benutzeroberfläche der App verwendet. Zur Demonstration der Unterschiede in der Verhaltensweise bei aktiviertem Freihandzeichen mit Toucheingabe setzen wir den InkCanvas in einen ScrollView-Container und stellen die ScrollViewer-Abmessungen kleiner als die des InkCanvas ein. 

Beim Starten der App wird nur die Stift-Freihandeingabe unterstützt, und die Toucheingabe wird zum Schwenken oder Zoomen der Freihand-Oberfläche verwendet. Wenn die Touch-Freihandeingabe aktiviert ist, kann die Freihand-Oberfläche nicht über die Toucheingabe geschwenkt oder gezoomt werden.

> [!NOTE]
> Unter [Freihand-Steuerelemente](../controls-and-patterns/inking-controls.md) finden Sie Anleitungen zu [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) und [**InkToolbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) UX. Für dieses Beispiel gelten die folgenden Empfehlungen:
> - Die [**InkToolbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) und die Freihandeingabe allgemein werden am besten über einen aktiven Stift bedient. Die Freihandeingabe mit Maus- und Toucheingabe kann aber unterstützt werden, wenn dies für Ihre App erforderlich ist. 
> - Bei Unterstützung der Freihandfunktion per Toucheingabe wird empfohlen, das Symbol „ED5F“ aus der Schriftart „Segoe MLD2 Assets” für die Umschaltfläche mit einer QuickInfo „Schreiben durch Berühren” zu verwenden. 

**XAML**

1. Zuerst deklarieren wir ein [**InkToolbarCustomToggleButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton)-Element (toggleButton) mit einem Klickereignis-Listener, der den Ereignishandler (Toggle_Custom) angibt.

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**CodeBehind**

2. Im vorigen Codeausschnitt haben wir einen Klickereignis-Listener und -Handler (Toggle_Custom) auf der Schaltfläche für benutzerdefiniertes Umschalten für die benutzerdefinierte Schaltfläche für die Touch-Freihandeingabe (toggleButton) deklariert. Dieser Handler wechselt einfach die Unterstützung für CoreInputDeviceTypes.Touch über die Eigenschaft InputDeviceTypes des InkPresenter.

   Hierzu haben wir ein Symbol für die Schaltfläche angegeben, wobei das SymbolIcon-Element und die Markuperweiterung [x:Bind], die dieses an ein in der CodeBehind-Datei (TouchWritingIcon) definiertes Feld bindet, verwendet wurden.

   Der folgende Codeausschnitt enthält den Klickereignis-Handler und die Definition von TouchWritingIcon.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>Benutzerdefiniertes Tool

Sie können eine Schaltfläche für ein benutzerdefiniertes Tool erstellen, die ein von Ihrer App definiertes Nicht-Stift-Tool aufruft.

Standardmäßig verarbeitet ein [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) sämtliche Eingaben als Freihandstrich oder als Radierstrich. Hierzu zählen auch Eingaben, die durch ein sekundäres Hardwareangebot wie etwa eine Zeichenstift-Drucktaste, eine rechte Maustaste oder ein ähnliches Element geändert werden. Allerdings kann ein [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) so konfiguriert werden, dass bestimmte Eingaben unverarbeitet gelassen werden. Diese können dann für die benutzerdefinierte Verarbeitung an Ihre App weitergeleitet werden.

In diesem Beispiel definieren wir eine Schaltfläche für ein benutzerdefiniertes Tool, das bei Auswahl bewirkt, dass nachfolgende Striche verarbeitet und als Auswahllasso (gestrichelte Linie) und nicht als Freihandeingabe wiedergegeben werden. Alle Freihandstriche innerhalb der Grenzen des Auswahlbereichs werden auf [**Ausgewählt**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke.selected) gesetzt.

> [!NOTE]
> Weitere Informationen zu InkCanvas und InkToolbar UX finden Sie in den Anleitungen zu Freihandsteuerelementen. Für dieses Beispiel gilt die folgende Empfehlung:
> - Für die Bereitstellung der Strichauswahl empfehlen wir die Verwendung des „EF20“-Symbols aus der Schriftart „Segoe MLD2 Assets“ für die Toolschaltfläche, mit einer QuickInfo „Auswahltool“. 
 
**XAML**

1. Zunächst deklarieren wir ein [**InkToolbarCustomToolButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton)-Element (CustomToolButton) mit einem Klickereignis-Listener, der den Ereignishandler (customToolButton_Click) angibt, in dem die Strichauswahl konfiguriert ist. (Es wurden außerdem verschiedene Schaltflächen zum Kopieren, Ausschneiden und Einfügen der Strichauswahl hinzugefügt.)

2. Ebenfalls hinzu kommt ein Zeichenbereichelement zum Zeichnen unseres Auswahlstrichs. Durch die Verwendung einer eigenen Ebene zum Zeichnen der Strichauswahl bleiben der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) und dessen Inhalt unverändert. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**CodeBehind**

2. Anschließend behandeln wir das Klickereignis für [**InkToolbarCustomToolButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) in der CodeBehind-Datei MainPage.xaml.cs.

   Dieser Handler konfiguriert den [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) so, dass unverarbeitete Eingaben an die App weitergeleitet werden. 

   Eine ausführlichere Schritt-für-Schritt-Anleitung zu diesem Code finden Sie im Abschnitt „Weitergabe der Eingabe für die erweiterte Verarbeitung“ von [Stiftinteraktionen und Windows Ink in UWP-Apps](pen-and-stylus-interactions.md).

   Hierzu haben wir ein Symbol für die Schaltfläche angegeben, wobei das SymbolIcon-Element und die Markuperweiterung [x:Bind], die dieses an ein in der CodeBehind-Datei (SelectIcon) definiertes Feld bindet, verwendet wurden.

   Der folgende Codeausschnitt enthält den Klickereignis-Handler und die Definition von SelectIcon.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
            // By default, the InkPresenter processes input modified by 
            // a secondary affordance (pen barrel button, right mouse 
            // button, or similar) as ink.
            // To pass through modified input to the app for custom processing 
            // on the app UI thread instead of the background ink thread, set 
            // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
            inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
                InkInputRightDragAction.LeaveUnprocessed;

            // Listen for unprocessed pointer events from modified input.
            // The input is used to provide selection functionality.
            inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
                UnprocessedInput_PointerPressed;
            inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
                UnprocessedInput_PointerMoved;
            inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
                UnprocessedInput_PointerReleased;
        }

        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
            InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
            ClearSelection();
        }

        private void InkPresenter_StrokesErased(
            InkPresenter sender, InkStrokesErasedEventArgs args)
        {
            ClearSelection();
        }

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Initialize a selection lasso.
            lasso = new Polyline()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
            };

            lasso.Points.Add(args.CurrentPoint.RawPosition);

            selectionCanvas.Children.Add(lasso);
        }

        private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
        }

        private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add the final point to the Polyline object and 
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas 
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
                inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                    lasso.Points);

            DrawBoundingRect();
        }

        // Draw a bounding rectangle, on the selection canvas, encompassing 
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
            // Clear all existing content from the selection canvas.
            selectionCanvas.Children.Clear();

            // Draw a bounding rectangle only if there are ink strokes 
            // within the lasso area.
            if (!((boundingRect.Width == 0) ||
                (boundingRect.Height == 0) ||
                boundingRect.IsEmpty))
            {
                var rectangle = new Rectangle()
                {
                    Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                    StrokeThickness = 1,
                    StrokeDashArray = new DoubleCollection() { 5, 2 },
                    Width = boundingRect.Width,
                    Height = boundingRect.Height
                };

                Canvas.SetLeft(rectangle, boundingRect.X);
                Canvas.SetTop(rectangle, boundingRect.Y);

                selectionCanvas.Children.Add(rectangle);
            }
        }
    }
}
```



### <a name="custom-ink-rendering"></a>Benutzerdefiniertes Rendern von Freihandeingaben

Standardmäßig werden Freihandeingaben in einem Hintergrundthread mit geringer Wartezeit verarbeitet und während des Zeichnens „nass“ gerendert. Wenn der Strich abgeschlossen ist (der Stift oder Finger wurde angehoben oder die Maustaste losgelassen), wird er im UI-Thread verarbeitet und auf der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Ebene „trocken“ gerendert (über dem Anwendungsinhalt, wo er die nasse Freihandeingabe ersetzt).

Die Freihandplattform ermöglicht es Ihnen, dieses Verhalten zu überschreiben und die Freihandfunktionen durch benutzerdefiniertes Trocknen der Freihandeingabe umfassend anzupassen.

Weitere Informationen zum benutzerdefinierten Trocknen finden Sie unter [Stiftinteraktionen und Windows Ink in UWP-Apps](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions#custom-ink-rendering).

> [!NOTE]
> Benutzerdefiniertes Trocknen und die [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Wenn Ihre App das Standard-Renderverhalten für Freihandeingaben von [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) mit einer benutzerdefinierten Trockenimplementierung außer Kraft setzt, sind die gerenderten Freihandstriche für die InkToolbar nicht mehr verfügbar und die integrierten Löschbefehle der InkToolbar funktionieren nicht wie erwartet. Damit Sie Löschfunktionen bereitstellen können, müssen Sie alle Zeigerereignisse verarbeiten, für jeden Strich einen Treffertest ausführen und den integrierten Befehl „Freihand vollständig löschen“ überschreiben.

## <a name="related-articles"></a>Verwandte Artikel

- [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Themenbeispiele

- [Symbolleisten-Speicherort und-Ausrichtung (Beispiel) (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
- [Symbolleisten-Speicherort und-Ausrichtung Beispiel (dynamisch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

### <a name="other-samples"></a>Andere Beispiele

- [Einfaches Ink-BeispielC#(C++/)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Complex Ink Sample (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink-Beispiel (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
- [Tutorial zu den ersten Schritten: Unterstützung von frei Hand Eingaben in ihrer UWP](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Beispiel für ein Farb Buch](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Beispiel für Familien Notizen](https://github.com/Microsoft/Windows-appsample-familynotes)
