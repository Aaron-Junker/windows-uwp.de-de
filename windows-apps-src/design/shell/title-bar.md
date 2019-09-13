---
description: Anpassen der Titelleiste einer Desktop-App, damit sie das gleiche Erscheinungsbild wie die App aufweist
title: Anpassen der Titelleiste
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Titelleiste
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 323b9b80a7d0087a07faf34d598f51d643e1324c
ms.sourcegitcommit: 5687e5340f8d78da95c3ac28304d1c9b8960c47d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70930334"
---
# <a name="title-bar-customization"></a>Anpassen der Titelleiste



Wenn Ihre App in einem Desktop-Fenster ausgeführt wird, können Sie die Titelleisten der Fenster anpassen, damit sie das gleiche Erscheinungsbild wie die App aufweisen Mit den APIs zur Anpassung der Titelleiste können Sie die Farben für die Elemente der Titelleiste angeben, oder Ihren App-Inhalte in den Bereich der Titelleiste anpassen und volle Kontrolle darüber erhalten.

> **Wichtige APIs:** [Applicationview. TitleBar-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview), [applicationviewtitlebar-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [coreapplicationviewtitlebar-Klasse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>Ausmaße der Anpassung der Titelleiste

Es gibt zwei Ebenen der Anpassung, die Sie auf der Titelleiste anwenden können.

Um einfach die Farbe anzupassen, legen Sie die [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar)-Eigenschaften fest, um die Farben anzugeben, die Sie für die Elemente der Titelleiste verwenden möchten. Das System behält in diesem Fall die Verantwortung für alle anderen angezeigten Aspekte der Titelleiste bei wie z. B. den App-Titel zeichnen und verschiebbare Bereiche definieren.

Die andere Möglichkeit ist das Ausblenden der Standard-Titelleiste und das Ersetzen durch Ihren eigenen XAML-Inhalt. Sie können z. B. Text, Schaltflächen oder benutzerdefinierte Menüs im Bereich der Titelleiste platzieren. Sie müssen ebenfalls diese Option verwenden, um einen [Acryl](../style/acrylic.md)-Hintergrund in den Bereich der Titelleiste einzufügen.

Wenn Sie ein umfassendes Anpassen wünschen, sind Sie für das Ablegen von Inhalten im Bereich der Titelleiste verantwortlich, und Sie können Ihren eigenen verschiebbaren Bereich definieren. Die Minimieren-, Maximieren-, Zurück- und Schließ-Schaltflächen sind auch weiterhin verfügbar und vom System verwaltet. Elemente wie z. B. der App-Titel gehören jedoch nicht dazu. Sie müssen diese Elemente selbst und je nach Anforderungen Ihrer App erstellen.

> [!NOTE]
> Das einfache Anpassen der Farbe ist für UWP-Apps mit XAML, DirectX und HTML verfügbar. Das umfassende Anpassen ist nur für UWP-Apps mit XAML verfügbar.

## <a name="simple-color-customization"></a>Einfaches Anspassen der Farbe

Wenn Sie nur die Farben der Titelleiste und keine weitere Aktion anpassen möchten (z. B. Registerkarten in der Titelleiste einfügen) können Sie die Eigenschaften auf der [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) für Ihre App-Fenster festlegen.

Dieses Beispiel zeigt, wie Sie eine Instanz der ApplicationViewTitleBar aufrufen und die Farbeigenschaften festlegen.

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> Dieser Code kann in die [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)-Methode Ihre App eingefügt werden (_App.xaml.cs_) – nach dem Aufruf von [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) oder auf der ersten Seite Ihrer App.

> [!TIP]
> Das Windows Community Toolkit enthält Erweiterungen, mit denen Sie diese Farbeigenschaften in XAML festlegen können. Weitere Informationen finden Sie in der [Windows Community Toolkit-Dokumentation](https://docs.microsoft.com/windows/uwpcommunitytoolkit/extensions/viewextensions).

Es gibt einige Dinge, die beim Festlegen von Farben für Titelleisten beachtet werden müssen:

- Die Hintergrundfarbe der Schaltfläche wird nicht auf den Zeigeeffekt und das Gedrückthalten der Schaltfläche "Schließen" angewendet. Die Schaltfläche "Schließen" verwendet immer die systemdefinierte Farbe für diese Zustände.
- Die Farbeigenschaften der Schaltfläche gelten für die Systemschaltfläche "Zurück", wenn sie verwendet wird. ([Siehe Navigationsverlauf und Rückwärtsnavigation](../basics/navigation-history-and-backwards-navigation.md).)
- Das Festlegen der Farbeigenschaft auf **null** setzt es auf die Standardfarbe für das System zurück.
- Sie können keine transparenten Farben festlegen. Die Alpha-Farbkanal wird ignoriert.

Windows bietet dem Benutzer die Möglichkeit, die ausgewählte [Akzentfarbe](https://docs.microsoft.com/windows/uwp/style/color#accent-color) auf die Titelleiste anzuwenden. Wenn Sie eine Titelfarbe festlegen, wird empfohlen, dass Sie alle Farben explizit festlegen. Dadurch wird sichergestellt, dass keine unerwünschten Farbkombinationen erhalten werden, die bei benutzerdefinierten Farbeinstellungen auftreten.

## <a name="full-customization"></a>Die umfassende Anpassung

Wenn Sie die umfassende Anpassung der Titelleiste wünschen, wird Ihr App-Clientbereich über das gesamte Fenster erweitert, einschließlich des Bereichs der Titelleiste. Sie sind für das Zeichnen und die Eingabe-Behandlung für das gesamte Fenster mit Ausnahme der Titelleistenschaltfläche verantwortlich, die auf dem App-Canvas überlagert werden.

Legen Sie zum Ausblenden der Standard-Titelleiste und zum Erweitern des Inhalts in den Bereich der Titelleiste die [CoreApplicationViewTitleBar.ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)-Eigenschaft auf **true** fest.

Dieses Beispiel zeigt, wie Sie die CoreApplicationViewTitleBar erhalten und legen die Eigenschaft ExtendViewIntoTitleBar auf **true** fest. Dies kann in der [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)-Methode der App (_App.xaml.cs_) oder auf der ersten Seite Ihrer App durchgeführt werden.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> Diese Einstellung wird beibehalten, wenn Ihre App beendet und neu gestartet wird. Wenn Sie in Visual Studio ExtendViewIntoTitleBar auf **true** festlegen und dann die Standardeinstellungen wiederherstellen möchten, sollten Sie es explizit auf **false** festlegen und Ihre App ausführen, um die beibehaltene Einstellung zu überschrieben.

### <a name="draggable-regions"></a>Ziehbare Bereiche

Der ziehbare Bereich der Titelleiste definiert, worauf der Benutzer klicken das Fenster verschieben kann (im Gegensatz zum einfachen Ziehen von Inhalten innerhalb des App-Canvas). Sie geben den ziehbaren Bereich durch Aufrufen der [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar)-Methode und der Übergabe eines UIElements an, das den verschiebbare Bereich definiert. (Das UIElement ist häufig ein Panel, das andere Elemente enthält.)

Hier erfahren Sie, wie ein Raster mit Inhalten als ziehbare Region der Titelleiste festgelegt wird. Dieser Code wird XAML und CodeBehind für die erste Seite Ihrer App hinzugefügt. Den vollständigen Code finden Sie im Abschnitt [umfassendes Anpassungsbeispiel](./title-bar.md#full-customization-example).

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;
    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    AppTitleBar.Height = sender.Height;
}
```

UIElement (`AppTitleBar`) ist Teil des XAML-Codes für Ihre App. Sie können die Titelleiste entweder in einer Stammseite deklarieren und festlegen, die sich nicht ändert, oder eine Region der Titelleiste auf jeder Seite deklarieren und festlegen, auf die Ihre App navigieren kann. Wenn Sie diese auf jeder Seite festlegen, sollten Sie sicherstellen, dass der ziehbare Bereich konsistent bleibt, wenn ein Benutzer in Ihrer App navigiert.

Rufen Sie SetTitleBar auf, um auf ein neues Titelleisten-Element zu wechseln, während die App ausgeführt wird. Sie können auch **null** als Parameter für SetTitleBar übergeben, um das standardmäßige Ziehverhalten wiederherzustellen. (Siehe "Ziehbarer Standardbereich" für weitere Informationen).

> [!IMPORTANT]
> Der ziehbare Bereich muss auf Treffer getestet werden, d. h. Sie müssen für einige Elemente möglicherweise einen transparenten Hintergrundpinsel festlegen. Weitere Informationen finden Sie in den Hinweisen auf [VisualTreeHelper.FindElementsInHostCoordinates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates).
>
>Wenn Sie ein Raster als ziehbare Region definieren, legen Sie z. B. `Background="Transparent"` fest, um es ziehbar zu machen.
>
>Dieses Raster wird nicht gezogen (nur die sichtbaren Elemente darin): `<Grid x:Name="AppTitleBar">`.
>
>Dieses Raster sieht genauso aus, das gesamte Raster ist allerdings ziehbar: `<Grid x:Name="AppTitleBar" Background="Transparent">`.

#### <a name="default-draggable-region"></a>Ziehbarer Standardbereich

Wenn Sie keinen ziehbaren Bereich angeben, wird ein Rechteck, das die Breite des Fensters und die Höhe der Titelleistenschaltfläche hat, am oberen Rand des Fensters positioniert und als ziehbare Standardregion festgelegt.

Wenn Sie einen verschiebbare Bereich definieren, verkleinert das System die ziehbaren Standardregion auf einen kleinen Bereich der Größe der Titelleistenschaltfläche, der links neben der Titelleistenschaltfläche (oder auf der rechten Seite der Titelleistenschaltfläche auf der linken Seite des Fensters) positioniert wird. Dadurch wird sichergestellt, dass immer ein konsistenter Bereich vorhanden ist, den der Benutzer ziehen kann.

### <a name="system-caption-buttons"></a>System-Titelleistenschaltflächen

Das System reserviert die obere linke oder obere rechte Ecke des App-Fensters für die Titelleistenschaltflächen des Systems (Minimieren-, Maximieren-, Zurück- und Schließ-Schaltflächen). Das System behält die Kontrolle über den Titelleistensteuerungsbereich, um die Mindestfunktionen für das Ziehen, Minimieren, Maximieren und Schließen des Fensters zu garantieren. Das System zeichnet die Schaltfläche "Schließen" in der oberen rechten Ecke für von links nach rechts gelesene Sprachen und die linke obere Ecke für von rechts nach links gelesene Sprachen.

Die Größe und Position des Titelleistensteuerungsbereichs wird von der CoreApplicationViewTitleBar-Klasse mitgeteilt, damit Sie das Layout der Titel Ihrer Benutzeroberfläche berücksichtigen können. Die Breite der reservierten Region auf jeder Seite wird durch die [SystemOverlayLeftInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) oder [SystemOverlayRightInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset)-Eigenschaften angegeben und die Höhe wird durch die [Height](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height)-Eigenschaft angegeben.

Sie können Inhalte unter den Titelleistensteuerungsbereich zeichnen, der von diesen Eigenschaften definiert wird wie z. B. Ihren App-Hintergrund, aber Sie sollten kein Benutzeroberflächenelemente dort zeichnen, mit denen der Benutzer interagieren kann. Er erhält keine Eingaben, da die Eingabe für die Titelsteuerungen vom System behandelt werden.

Sie können das [LayoutMetricsChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged)-Ereignis so festlegen, dass es auf die Änderungen der Größe der Titelleistenschaltfläche reagiert. Dies kann beispielsweise der Fall sein, wenn die Schaltfläche des Systems „Zurück” angezeigt oder ausgeblendet wird. Verwenden Sie dieses Ereignis, um das Positionieren von Benutzeroberflächenelementen zu überprüfen und aktualisieren, die von der Größe der Titelleiste abhängen.

In diesem Beispiel wird veranschaulicht, wie das Layouts der Titelleiste für Änderungen der Systemschaltflächen wie „Zurück” angezeigt oder ausgeblendet wird. `AppTitleBar`, `LeftPaddingColumn` und`RightPaddingColumn` werden in der zuvor gezeigten XAML deklariert.

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>interaktiver Inhalt

Sie können interaktive Steuerelemente, z. B. Schaltflächen, Menüs oder ein Suchfeld im oberen Teil der App festlegen, sodass sie in der Titelleiste angezeigt werden. Es gibt jedoch einige Regeln, die Sie befolgen müssen, um sicherzustellen, dass Ihre interaktiven Elemente Benutzereingaben erhalten.
- Rufen Sie SetTitleBar auf, um einen Bereich als ziehbare Region der Titelleiste zu definieren. Falls nicht, setzt das System die ziehbare Standardregion am oberen Rand der Seite fest. Das System wird dann alle Benutzereingaben in diesem Bereich behandeln und verhindern, dass Eingaben ihre Steuerelemente erreichen.
- Legen Sie die interaktiven Steuerelemente am oberen Rand des ziehbaren Bereichs fest, der durch den Aufruf von SetTitleBar (mit einer höheren Z-Reihenfolge) definiert ist. Legen Sie die interaktiven Steuerelemente des UIElements, die als SetTitleBar übergeben wurden, nicht als untergeordnete Elemente fest. Nachdem Sie ein Element an SetTitleBar übergeben haben, behandelt das System es wie die System-Titelleiste und behandelt alle Zeigereingaben auf das Element.

Hier verfügt das `TitleBarButton`-Element über eine höhere Z-Reihenfolge als `AppTitleBar`, damit es Benutzereingaben empfängt.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>Transparenz in Titelleistenschaltflächen

Wenn Sie ExtendViewIntoTitleBar auf **true** festlegen, können Sie den Hintergrund der Titelleistenschaltflächen transparent machen, damit der App-Hintergrund durchscheint. Generell wird der Hintergrund auf [Colors.Transparent](https://docs.microsoft.com/uwp/api/windows.ui.colors.Transparent) für vollständigen Transparenz festgelegt. Legen Sie für eine partielle Transparenz den Alpha-Kanal für die [Farbe](https://docs.microsoft.com/uwp/api/windows.ui.color) auf die Eigenschaft fest.

Diese ApplicationViewTitleBar-Eigenschaften können transparent sein:

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

Die Hintergrundfarbe der Schaltfläche wird nicht auf den Zeigeeffekt und das Gedrückthalten der Schaltfläche "Schließen" angewendet. Die Schaltfläche "Schließen" verwendet immer die systemdefinierte Farbe für diese Zustände.

Alle anderen Farbeigenschaften werden auch weiterhin den Alphakanal ignorieren. Wenn ExtendViewIntoTitleBar auf **false** festgelegt wird, wird der Alpha-Kanal immer für alle ApplicationViewTitleBar-Farbeigenschaften ignoriert.

### <a name="full-screen-and-tablet-mode"></a>Vollbildschirm- und Tablet-Modus

Wenn Ihre App im _Vollbildmodus_ oder _Tablet-Modus_ ausgeführt wird, blendet das System die Titelleisten- und Untertitel-Steuerungschaltflächen aus. Der Benutzer kann allerdings die Titelleiste aufrufen, um sie als Overlay über der Benutzeroberfläche anzuzeigen.
Sie können das [CoreApplicationViewTitleBar.IsVisibleChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged)-Ereignis so festlegen, dass Sie benachrichtigt werden, wenn die Titelleiste aufgerufen oder ausgeblendet wird und den benutzerdefinierten Inhalt Ihrer Titelleiste nach Wunsch anzuzeigen oder auszublenden.

In diesem Beispiel wird veranschaulicht, wie IsVisibleChanged zum Einblenden und Ausblenden des oben dargestellten `AppTitleBar`- Elements behandelt wird.

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>Der _Vollbild_ Modus kann nur angezeigt werden, wenn er von Ihrer App unterstützt wird. Weitere Informationen finden Sie unter [ApplicationView.IsFullScreenMode](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode). Der [_Tablet-Modus_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) ist eine Benutzer Option auf unterstützter Hardware, sodass ein Benutzer eine beliebige App im Tablet-Modus ausführen kann.

## <a name="full-customization-example"></a>Umfassendes Anpassungsbeispiel

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Machen Sie es offensichtlich, wenn Ihr Fenster aktiv oder inaktiv ist. Ändern Sie mindestens die Farbe von Text, Symbolen und Schaltflächen in der Titelleiste.
- Definieren Sie einen ziehbaren Bereich am oberen Rand des App-Canvas. Durch das Abstimmen der Platzierung der System-Titelleisten sind diese einfacher zu finden.
- Definieren Sie einen ziehbaren Bereich, der mit der visuellen Titelleiste (sofern vorhanden) in der App Canvas übereinstimmt.

## <a name="related-articles"></a>Verwandte Artikel

- [Acrylic](../style/acrylic.md)
- [Farbe](../style/color.md)
