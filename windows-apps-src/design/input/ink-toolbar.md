---
Description: Fügen Sie einer App für die Freihandeingabe in der universellen Windows-Plattform (UWP) eine Standard-InkToolbar hinzu. Fügen Sie der InkToolbar eine anpassbare Stiftschaltfläche hinzu, und binden Sie diese an eine benutzerdefinierte Definition für den Stift.
title: Hinzufügen von InkToolbar zu einer App für die Universelle Windows-Plattform (UWP)
label: Add an InkToolbar to a Universal Windows Platform (UWP) app
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universelle Windows-Platform, UWP, Benutzerinteraktion, Eingabe
ms.date: 02/08/2017
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: 0dd16eac9fe61f292b4f05e1fbd515c0e7255377
ms.sourcegitcommit: d00acf229aa41be2d41ef3e5a7d1f40fa7153acc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65187518"
---
# <a name="add-an-inktoolbar-to-a-universal-windows-platform-uwp-app"></a>Hinzufügen von InkToolbar zu einer App für die Universelle Windows-Plattform (UWP)



Es gibt zwei verschiedene Steuerelemente, die die erleichtern Freihand in apps für universelle Windows-Plattform (UWP): [**InkCanvas** ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) und [ **InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

Über das [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)-Steuerelement werden grundlegende Windows Ink-Funktionen bereitgestellt. Rendern Sie damit die Stifteingabe entweder als letzten Strich (mit den Standardeinstellungen für Farbe und Stärke) oder ausradierten Strich.

> Details zur Implementierung von InkCanvas finden Sie unter [Zeichen- und Eingabestiftinteraktionen in UWP-Apps](pen-and-stylus-interactions.md).

Als vollständig transparente Überlagerung bietet InkCanvas keine integrierte Benutzeroberfläche zum Festlegen von Freihandstricheinstellungen. Wenn Sie die Standard-Freihandfunktionen ändern, Benutzern das Festlegen von Freihandstricheinstellungen ermöglichen und andere benutzerdefinierte Freihandeingabefeatures unterstützen möchten, haben Sie zwei Optionen:

- Verwenden Sie im CodeBehind das an InkCanvas gebundene zugrunde liegende [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)-Objekt.

  Die InkPresenter-APIs unterstützen die umfassende Anpassung der Freihandfunktionen. Weitere Informationen finden Sie unter [Zeichen- und Eingabestiftinteraktionen in UWP-Apps](pen-and-stylus-interactions.md).

- Binden Sie [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) an InkCanvas. InkToolbar bietet standardmäßig eine anpassbare und erweiterbare Sammlung von Schaltflächen zum Aktivieren von Freihandfunktionen wie Strichgröße, Freihandfarbe und Form der Stiftspitze.

  InkToolbar wird in diesem Thema erläutert.

> **Wichtige APIs:** [**InkCanvas-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx), [ **InkToolbar Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx), [ **InkPresenter-Klasse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx), [ **Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)

## <a name="default-inktoolbar"></a>Standard-InkToolbar

Das [**InkToolbar-Steuerelement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) enthält standardmäßig Schaltflächen zum Zeichnen, Löschen, Hervorheben sowie zum Anzeigen einer Schablone(Lineal oder Winkelmesser). Abhängig vom Feature werden in einem Flyout weitere Einstellungen und Befehle bereitgestellt, beispielsweise für Freihandfarbe, Strichstärke und das Löschen aller Freihandeingaben.

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*Standardmäßige Windows Ink-Symbolleiste*

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
| *Standardspeicherort für Windows Ink-Symbolleiste und die Ausrichtung* | *Windows Ink Symbolleiste expliziten Ort und die Ausrichtung* |

Im Folgenden sehen Sie den Code, mit dem Sie den Standort und die Ausrichtung der Freihandsymbolleiste explizit in XAML festlegen.
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**Initialisieren, die basierend auf benutzereinstellungen oder des Geräts**

In einigen Fällen möchten Sie möglicherweise den Standort und die Ausrichtung der Freihandsymbolleiste basierend auf Benutzereinstellungen oder Gerätestatus festlegen. Das folgende Beispiel veranschaulicht, wie Sie den Standort und die Ausrichtung der Freihandsymbolleiste basierend auf den Einstellungen für Rechts- oder Linkshänder festlegen, die über **Einstellungen > Geräte > Stift & Windows Ink > Stift > Schreibhand auswählen** angegeben werden.

![Vorherrschende manuell festlegen](./images/ink/location-handedness-setting.png)  
*Vorherrschende manuell festlegen*

Sie können diese Einstellung über die HandPreference-Eigenschaft von Windows.UI.ViewManagement abfragen und [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) basierend auf dem zurückgegebenen Wert festlegen. In diesem Beispiel platzieren wir die Symbolleiste auf der linken Seite der App für einen Linkshänder und auf der rechten Seite für einen Rechtshänder.

**Laden Sie dieses Beispiel von [Freihandeingaben Symbolleiste Ort und die Ausrichtung Beispiel (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)**

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

**Dynamische Anpassungen Sie an Benutzer oder Gerät-Status**

Sie können auch eine Bindung verwenden, um nach Aktualisierungen der Benutzeroberfläche basierend auf Benutzereinstellungen, Geräteeinstellungen oder Gerätestatus zu suchen. Im folgenden Beispiel wird das vorherige Beispiel erweitert und gezeigt, wie Sie die Freihandsymbolleiste basierend auf der Ausrichtung des Geräts mithilfe von Bindung, einem ViewMOdel-Objekt und der [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)-Schnittstelle dynamisch positionieren. 

**Laden Sie dieses Beispiel von [Freihandeingaben Symbolleiste Ort und die Ausrichtung Beispiel (dynamisch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)**

1. Zuerst fügen wir unser ViewModel hinzu.
    1. Fügen Sie Ihrem Projekt einen neuen Ordner hinzu, und nennen Sie ihn **ViewModels**.
    1. Fügen Sie eine neue Klasse zum ViewModels-Ordner hinzu (für dieses Beispiel nennen wir sie **InkToolbarSnippetHostViewModel.cs**).
        > [!NOTE] 
        > Wir haben das [Singleton-Muster](https://msdn.microsoft.com/library/ff650849.aspx) verwendet, da wir nur ein Objekt dieses Typs für die Lebensdauer der Anwendung benötigen.

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

    1. Fügen Sie zwei boolesche Eigenschaften, auf die InkToolbarSnippetHostViewModel-Klasse: **LeftHandedLayout** (dieselbe Funktionalität wie das vorherige nur XAML-Beispiel) und **PortraitLayout** (Ausrichtung des Geräts).
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

1. Öffnen Sie nun die Datei "MainPage.Xaml.cs" ein.
    1. Hinzufügen `using using locationandorientation.ViewModels` zur Liste der Namespaces, in unserem ViewModel zu verknüpfen.
    1. Hinzufügen `using Windows.UI.ViewManagement` zur Liste der Namespaces, in dem Lauschen auf Änderungen an der geräteausrichtung zu aktivieren.
    1. Hinzufügen der [WindowSizeChangedEventHandler](https://docs.microsoft.com/uwp/api/windows.ui.xaml.windowsizechangedeventhandler) Code.
    1. Legen Sie die [DataContext](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext) für die Sicht auf die Singletoninstanz der Klasse InkToolbarSnippetHostViewModel. 
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

1. Öffnen Sie als Nächstes die Datei "MainPage.xaml" ein.
    1. Hinzufügen `xmlns:converters="using:locationandorientation.Converters"` auf die `Page` Element für die Bindung an unsere Konverter.
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

    1. Hinzufügen einer `PageResources` Element, und geben Sie die Verweise auf unsere Konverter.
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. Fügen Sie der InkCanvas-Steuerelement und InkToolbar Elemente hinzu und Binden der Eigenschaften "VerticalAlignment" und "HorizontalAlignment" von der InkToolbar.
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. Zurück zu den InkToolbarSnippetHostViewModel.cs hinzuzufügenden Datei zu unserer `PortraitLayout` und `LeftHandedLayout` boolesche Eigenschaften, die `InkToolbarSnippetHostViewModel` -Klasse, zusammen mit der Unterstützung für das erneute Binden `PortraitLayout` Wenn dieser Eigenschaftswert geändert wird. 
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

Sie verfügen nun über eine bereitgestelltem app, die passt sich sowohl die vorherrschende Hand-Einstellung des Benutzers und die Ausrichtung des Gerät des Benutzers dynamisch reagiert.

### <a name="specify-the-selected-button"></a>Angeben der ausgewählten Schaltfläche  
![Schaltfläche "Stift" ausgewählt, bei der Initialisierung](./images/ink/ink-tools-default-toolbar.png)  
*Windows Ink Symbolleisten-Schaltfläche "Stift" ausgewählt, bei der Initialisierung*

Standardmäßig wird die erste (oder äußerste linke) Schaltfläche aktiviert, wenn die App gestartet und die Symbolleiste initialisiert wird. In der standardmäßigen Windows Ink-Symbolleiste ist dies die Kugelschreiberschaltfläche.

Da das Framework die Reihenfolge der integrierten Schaltflächen definiert, ist möglicherweise die erste Schaltfläche nicht der Stift oder das Tool, das Sie standardmäßig aktivieren möchten.

Sie können das Standardverhalten überschreiben und die ausgewählte Schaltfläche auf der Symbolleiste angeben.

In diesem Beispiel initialisieren wir die standardmäßige Symbolleiste mit ausgewählter Stiftschaltfläche und aktiviertem Stift (anstelle des Kugelschreibers).

1. Verwenden Sie die XAML-Deklaration für InkCanvas und InkToolbar aus dem vorherigen Beispiel.
2. Richten Sie im CodeBehind einen Handler für das [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx)-Ereignis des [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)-Objekts ein.

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

3. Im Handler für das [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx)-Ereignis:
    1. Rufen Sie einen Verweis auf die integrierte [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) ab.

    Das Übergeben eines [InkToolbarTool.Pencil](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx)-Objekts in der [GetToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx)-Methode gibt ein [InkToolbarToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx)-Objekt für [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) zurück.

    2. Legen Sie [ActiveTool](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) auf das im vorherigen Schritt zurückgegebene Objekt fest.

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

![Bestimmte Schaltflächen enthalten, die bei der Initialisierung](./images/ink/ink-tools-specific.png)  
*Bestimmte Schaltflächen enthalten, die bei der Initialisierung*

Wie bereits erwähnt, enthält die Windows Ink-Symbolleiste eine Sammlung von standardmäßigen, integrierten Schaltflächen. Diese Schaltflächen werden in der folgenden Reihenfolge angezeigt (von links nach rechts):

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

In diesem Beispiel initialisieren wir die Symbolleiste nur mit den integrierten Schaltflächen für Kugelschreiber, Stift und Radierer.

Verwenden Sie hierzu XAML oder CodeBehind.

**XAML**

Ändern Sie die XAML-Deklaration für InkCanvas und InkToolbar aus dem ersten Beispiel.
- Fügen Sie ein [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx)-Attribut hinzu, und legen Sie dessen Wert auf „[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)“ fest. Damit löschen Sie die Standardsammlung der integrierten Schaltflächen.
- Fügen Sie die für Ihre App erforderlichen spezifischen InkToolbar-Schaltflächen hinzu. Hier fügen wir nur [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) und [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx) hinzu.
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

**Code-behind**
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

2. Richten Sie im CodeBehind einen Handler für das [Loading](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx)-Ereignis des [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)-Objekts ein.

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

3. Legen Sie [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) auf „[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)“ fest.
4. Erstellen Sie Objektverweise für die für Ihre App erforderlichen Schaltflächen. Hier fügen wir nur [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) und [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx) hinzu.
  > [!NOTE]
  > Schaltflächen werden der Symbolleiste in der vom Framework definierten Reihenfolge hinzugefügt, nicht in der hier angegebenen Reihenfolge.

5. [Fügen Sie](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx) die Schaltflächen der InkToolbar hinzu.

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
> **Beachten Sie**&nbsp;&nbsp;Funktionsauswahl wird sich gegenseitig ausschließende.

2. Eine Gruppe von „Umschaltflächen“ mit der integrierten Linealschaltfläche. Hier werden benutzerdefinierte Umschaltflächen hinzugefügt.
> **Beachten Sie**&nbsp;&nbsp;Features sind nicht gegenseitig und können gleichzeitig mit anderen Tools für die aktive verwendet werden.

Je nach Anwendung und erforderlicher Freihandfunktion können Sie dem InkToolbar-Steuerelement eine der folgenden Schaltflächen (die an die benutzerdefinierten Freihandfunktionen gebunden sind) hinzufügen:

- Benutzerdefinierter Stift: Ein Stift, für den die Farbpaletten- und Stiftspitzeneigenschaften der Freihandeingabe wie Form, Drehung und Größe von der Host-App definiert werden.
- Benutzerdefiniertes Tool: Ein Tool ohne Stift, das von der Host-App definiert wird.
- Benutzerdefiniertes Umschalten: Legt den Zustand eines durch die App definierten Features auf „aktiviert“ oder „deaktiviert“ fest. Wenn die Schaltfläche aktiviert ist, funktioniert das Feature in Verbindung mit dem aktiven Tool.

> **Beachten Sie**&nbsp;&nbsp;Sie können nicht ändern, die Anzeigereihenfolge von den integrierten Schaltflächen. Die Standardreihenfolge für die Anzeige ist: Kugelschreiber, Stift, Textmarker, Radierer und Lineal. Benutzerdefinierte Stifte werden an den letzten Standardstift angefügt, benutzerdefinierte Tool-Schaltflächen werden zwischen der letzten Stiftschaltfläche und der Radiererschaltfläche hinzugefügt, und benutzerdefinierte Umschaltflächen werden nach der Linealschaltfläche hinzugefügt. (Benutzerdefinierte Schaltflächen werden in der Reihenfolge hinzugefügt, in der sie angegeben werden.)

### <a name="custom-pen"></a>Benutzerdefinierter Stift

Sie können einen benutzerdefinierten Stift (zur Aktivierung über eine entsprechende Schaltfläche) erstellen, wobei Sie die Tintenfarbpalette und die Eigenschaften der Stiftspitze (Form, Rotation, Größe) definieren.

![Benutzerdefinierte kalligrafische Stifttaste](./images/ink/ink-tools-custompen.png)  
*Benutzerdefinierte kalligrafische Stifttaste*

In diesem Beispiel legen wir einen benutzerdefinierten Stift mit einer breiten Spitze fest, die einfache kalligrafische Freihandstriche ermöglicht. Wir passen außerdem die Sammlung der Pinsel in der auf dem Schaltflächen-Flyout angezeigten Palette an.

**Code-behind**

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

4. Geben Sie an, dass die CalligraphicPen-Klasse von [InkToolbarCustomPen](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx) abgeleitet wird.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Überschreiben Sie [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx), um Ihre eigene Pinsel- und Strichgröße anzugeben.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Erstellen Sie ein [InkDrawingAttributes](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx)-Objekt, und legen Sie die [Form der Stiftspitze](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), [Drehung der Spitze](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), [Strichgröße](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx) und [Freihandfarbe](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx) fest.
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

1. Wir deklarieren ein lokales Seitenressourcenverzeichnis, das einen Verweis auf den in „CalligraphicPen.cs“ festgelegten benutzerdefinierten Stift (`CalligraphicPen`) und eine [Pinselsammlung](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) erstellt, die vom benutzerdefinierten Stift unterstützt wird (`CalligraphicPenPalette`).
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

2. Wir fügen dann eine InkToolbar mit einem untergeordneten [InkToolbarCustomPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx)-Element hinzu.

  Die benutzerdefinierte Stiftschaltfläche enthält zwei in den Seitenressourcen deklarierte statische Ressourcenverweise: `CalligraphicPen` und `CalligraphicPenPalette`.

  Wir geben außerdem den Bereich für den Strichgröße-Schieberegler ([MinStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) und [SelectedStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), den ausgewählten Pinsel ([SelectedBrushIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)) und das Symbol für die benutzerdefinierte Stiftschaltfläche ([SymbolIcon](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)) an.
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
> Unter [Freihand-Steuerelemente](../controls-and-patterns/inking-controls.md) finden Sie Anleitungen zu [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) und [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar) UX. Für dieses Beispiel gelten die folgenden Empfehlungen:
> - Die [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar) und die Freihandeingabe allgemein werden am besten über einen aktiven Stift bedient. Die Freihandeingabe mit Maus- und Toucheingabe kann aber unterstützt werden, wenn dies für Ihre App erforderlich ist. 
> - Bei Unterstützung der Freihandfunktion per Toucheingabe wird empfohlen, das Symbol „ED5F“ aus der Schriftart „Segoe MLD2 Assets” für die Umschaltfläche mit einer QuickInfo „Schreiben durch Berühren” zu verwenden. 

**XAML**

1. Zuerst deklarieren wir ein [**InkToolbarCustomToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton)-Element (toggleButton) mit einem Klickereignis-Listener, der den Ereignishandler (Toggle_Custom) angibt.

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

**Code-behind**

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

Standardmäßig verarbeitet ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) sämtliche Eingaben als Freihandstrich oder als Radierstrich. Hierzu zählen auch Eingaben, die durch ein sekundäres Hardwareangebot wie etwa eine Zeichenstift-Drucktaste, eine rechte Maustaste oder ein ähnliches Element geändert werden. Allerdings kann ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) so konfiguriert werden, dass bestimmte Eingaben unverarbeitet gelassen werden. Diese können dann für die benutzerdefinierte Verarbeitung an Ihre App weitergeleitet werden.

In diesem Beispiel definieren wir eine Schaltfläche für ein benutzerdefiniertes Tool, das bei Auswahl bewirkt, dass nachfolgende Striche verarbeitet und als Auswahllasso (gestrichelte Linie) und nicht als Freihandeingabe wiedergegeben werden. Alle Freihandstriche innerhalb der Grenzen des Auswahlbereichs werden auf [**Ausgewählt**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkStroke.Selected) gesetzt.

> [!NOTE]
> Weitere Informationen zu InkCanvas und InkToolbar UX finden Sie in den Anleitungen zu Freihandsteuerelementen. Für dieses Beispiel gilt die folgende Empfehlung:
> - Für die Bereitstellung der Strichauswahl empfehlen wir die Verwendung des „EF20“-Symbols aus der Schriftart „Segoe MLD2 Assets“ für die Toolschaltfläche, mit einer QuickInfo „Auswahltool“. 
 
**XAML**

1. Zunächst deklarieren wir ein [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton)-Element (CustomToolButton) mit einem Klickereignis-Listener, der den Ereignishandler (customToolButton_Click) angibt, in dem die Strichauswahl konfiguriert ist. (Es wurden außerdem verschiedene Schaltflächen zum Kopieren, Ausschneiden und Einfügen der Strichauswahl hinzugefügt.)

2. Ebenfalls hinzu kommt ein Zeichenbereichelement zum Zeichnen unseres Auswahlstrichs. Durch die Verwendung einer eigenen Ebene zum Zeichnen der Strichauswahl bleiben der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) und dessen Inhalt unverändert. 

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

**Code-behind**

2. Anschließend behandeln wir das Klickereignis für [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) in der CodeBehind-Datei MainPage.xaml.cs.

   Dieser Handler konfiguriert den [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) so, dass unverarbeitete Eingaben an die App weitergeleitet werden. 

   Für eine ausführlichere Schritt durch diesen Code:  Finden Sie unter der Pass-Through-Eingabe für erweiterten Verarbeitung-Abschnitt des [Pen Interaktionen und Windows Ink in UWP-apps](pen-and-stylus-interactions.md).

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

Standardmäßig werden Freihandeingaben in einem Hintergrundthread mit geringer Wartezeit verarbeitet und während des Zeichnens „nass“ gerendert. Wenn der Strich abgeschlossen ist (der Stift oder Finger wurde angehoben oder die Maustaste losgelassen), wird er im UI-Thread verarbeitet und auf der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Ebene „trocken“ gerendert (über dem Anwendungsinhalt, wo er die nasse Freihandeingabe ersetzt).

Die Freihandplattform ermöglicht es Ihnen, dieses Verhalten zu überschreiben und die Freihandfunktionen durch benutzerdefiniertes Trocknen der Freihandeingabe umfassend anzupassen.

Weitere Informationen zum benutzerdefinierten Trocknen finden Sie unter [Stiftinteraktionen und Windows Ink in UWP-Apps](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions#custom-ink-rendering).

> [!NOTE]
> Benutzerdefiniertes Trocknen und die [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Wenn Ihre App das Standard-Renderverhalten für Freihandeingaben von [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) mit einer benutzerdefinierten Trockenimplementierung außer Kraft setzt, sind die gerenderten Freihandstriche für die InkToolbar nicht mehr verfügbar und die integrierten Löschbefehle der InkToolbar funktionieren nicht wie erwartet. Damit Sie Löschfunktionen bereitstellen können, müssen Sie alle Zeigerereignisse verarbeiten, für jeden Strich einen Treffertest ausführen und den integrierten Befehl „Freihand vollständig löschen“ außer Kraft setzen.

## <a name="related-articles"></a>Verwandte Artikel

- [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Themenbeispiele

- [Ink-Symbolleiste Ort und die Ausrichtung Beispiel (basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
- [Ink-Symbolleiste Ort und die Ausrichtung Beispiel (dynamisch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

### <a name="other-samples"></a>Andere Beispiele

- [Beispiel für einfache Freihandeingaben (C#/C++)](https://go.microsoft.com/fwlink/p/?LinkID=620312)
- [Komplexe Freihand-Beispiel (C++)](https://go.microsoft.com/fwlink/p/?LinkID=620314)
- [Freihand-Beispiel (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
- [Einstiegstutorial: Unterstützung von Freihandeingaben in Ihre UWP-app](https://aka.ms/appsample-ink)
- [Codefarben Book-Beispiel](https://aka.ms/cpubsample-coloringbook)
- [Anmerkungen zu dieser Version Familie-Beispiel](https://aka.ms/cpubsample-familynotessample)
