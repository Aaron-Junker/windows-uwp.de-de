---
description: Passen Sie die Titelleiste einer Desktop-App so an, dass Sie mit der Persönlichkeit der APP identisch ist.
title: Anpassen der Titelleiste
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Titelleiste
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 9d8aa92ec320c18b1947cb9b3fa7777070e19726
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220083"
---
# <a name="title-bar-customization"></a>Anpassen der Titelleiste



Wenn Ihre APP in einem Desktop Fenster ausgeführt wird, können Sie die Titelleisten so anpassen, dass Sie der Persönlichkeit ihrer App entsprechen. Mit den Anpassungs-APIs für die Titelleiste können Sie Farben für Titelleisten Elemente angeben oder Ihren app-Inhalt auf den Titelleisten Bereich erweitern und die vollständige Kontrolle über die Anwendung übernehmen.

> **Wichtige APIs**: [applicationview. TitleBar-Eigenschaft](/uwp/api/windows.ui.viewmanagement.applicationview), [applicationviewtitlebar-Klasse](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [coreapplicationviewtitlebar-Klasse](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>Wie viel Anpassen der Titelleiste

Es gibt zwei Anpassungs Ebenen, die Sie auf die Titelleiste anwenden können.

Für eine einfache Farbanpassung können Sie die [applicationviewtitlebar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) -Eigenschaften festlegen, um die Farben anzugeben, die Sie für Titelleisten Elemente verwenden möchten. In diesem Fall behält das System die Verantwortung für alle anderen Aspekte der Titelleiste bei, wie z. b. das Zeichnen des App-Titels und das Definieren von dragfähigen Bereichen.

Die andere Option besteht darin, die Standard Titelleiste auszublenden und durch ihren eigenen XAML-Inhalt zu ersetzen. Sie können z. b. Text, Schaltflächen oder benutzerdefinierte Menüs im Titelleisten Bereich platzieren. Sie müssen diese Option auch verwenden, um einen [Acryl](../style/acrylic.md) Hintergrund in den Titelleisten Bereich zu erweitern.

Wenn Sie sich für eine vollständige Anpassung entscheiden, sind Sie dafür verantwortlich, Inhalte in den Titelleisten Bereich zu versetzen, und Sie können einen eigenen dragfähigen Bereich definieren. Die Schaltflächen "zurück", "Schließen", "minimieren" und "maximieren" sind weiterhin verfügbar und werden vom System verarbeitet, Elemente wie der APP-Titel hingegen nicht. Sie müssen diese Elemente selbst erstellen, wenn Sie von Ihrer APP benötigt werden.

> [!NOTE]
> Die einfache Farbanpassung ist für Windows-apps mit XAML, DirectX und HTML verfügbar. Die vollständige Anpassung ist nur für Windows-apps verfügbar, die XAML verwenden.

## <a name="simple-color-customization"></a>Einfache Farbanpassung

Wenn Sie nur die Titelleisten Farben anpassen möchten und nichts zu tun haben (z. b. das Platzieren von Registerkarten in der Titelleiste), können Sie die Farbeigenschaften auf der [applicationviewtitlebar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) für das App-Fenster festlegen.

In diesem Beispiel wird gezeigt, wie eine Instanz von applicationviewtitlebar abgerufen und seine Farbeigenschaften festgelegt werden.

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
> Sie können diesen Code in der [ongestarteten](/uwp/api/windows.ui.xaml.application.onlaunched) -Methode Ihrer APP (_app.XAML.cs_) nach dem aufgerufenen [Fenster. aktivieren](/uwp/api/windows.ui.xaml.window.Activate)oder auf der ersten Seite Ihrer APP platzieren.

> [!TIP]
> Das Windows Community Toolkit bietet Erweiterungen, mit denen Sie diese Farbeigenschaften in XAML festlegen können. Weitere Informationen finden Sie in der [Dokumentation zum Windows Community Toolkit](/windows/uwpcommunitytoolkit/extensions/viewextensions).

Beim Festlegen der Farben der Titelleiste sind einige Punkte zu beachten:

- Die Hintergrundfarbe der Schaltfläche wird nicht auf die Schaltflächen "Schließen" und "gedrückt" angewendet. Die Schaltfläche Schließen verwendet immer die System definierte Farbe für diese Zustände.
- Die Eigenschaften der Schaltflächen Farbe werden bei der Verwendung auf die zurück-Schaltfläche des Systems angewendet. ([Siehe Navigationsverlauf und rückwärts Navigation](../basics/navigation-history-and-backwards-navigation.md).)
- Wenn eine Color-Eigenschaft auf **null** festgelegt wird, wird diese auf die Standardsystem Farbe zurückgesetzt.
- Transparente Farben können nicht festgelegt werden. Der Alphakanal der Farbe wird ignoriert.

Windows gibt einem Benutzer die Möglichkeit, die ausgewählte [Akzentfarbe](../style/color.md#accent-color) auf die Titelleiste anzuwenden. Wenn Sie eine Titelleisten Farbe festlegen, wird empfohlen, alle Farben explizit festzulegen. Dadurch wird sichergestellt, dass aufgrund benutzerdefinierter Farbeinstellungen keine unbeabsichtigten Farbkombinationen vorhanden sind.

## <a name="full-customization"></a>Vollständige Anpassung

Wenn Sie sich für die vollständige Anpassung der Titelleiste entscheiden, wird der Client Bereich Ihrer APP so erweitert, dass das gesamte Fenster einschließlich der Titelleisten Fläche abgedeckt wird. Sie sind verantwortlich für die Zeichnung und Eingabe Behandlung für das gesamte Fenster außer den Beschriftungs Schaltflächen, die oberhalb der Canvas der APP überlagert werden.

Um die Standard Titelleiste auszublenden und ihren Inhalt auf den Titelleisten Bereich zu erweitern, legen Sie die Eigenschaft [coreapplicationviewtitlebar. extendviewindetitlebar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) auf **true**fest.

In diesem Beispiel wird gezeigt, wie die coreapplicationviewtitlebar abgerufen und die extendviewindetitlebar-Eigenschaft auf **true**festgelegt wird. Dies kann in der [OnStart](/uwp/api/windows.ui.xaml.application.onlaunched) -Methode Ihrer APP (_app.XAML.cs_) oder auf der ersten Seite Ihrer APP erfolgen.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> Diese Einstellung bleibt erhalten, wenn Ihre APP geschlossen und neu gestartet wird. Wenn Sie in Visual Studio "extendviewindetitlebar" auf " **true**" festlegen und die Standardeinstellung wiederherstellen möchten, sollten Sie diese explizit auf " **false** " festlegen und die app ausführen, um die persistente Einstellung zu überschreiben.

### <a name="draggable-regions"></a>Dragfähige Regionen

Der dragfähige Bereich der Titelleiste definiert, wo der Benutzer klicken und ziehen kann, um das Fenster zu verschieben (im Gegensatz zum einfachen ziehen von Inhalt innerhalb der Canvas der APP). Sie geben den dragfähigen Bereich an, indem Sie die [Window. settitlebar](/uwp/api/windows.ui.xaml.window.settitlebar) -Methode aufrufen und ein UIElement übergeben, das den dragfähigen Bereich definiert. (Das UIElement ist oft ein Panel, das andere Elemente enthält.)

So legen Sie ein Inhalts Raster als dragfähigen Titelleisten Bereich fest. Dieser Code wird in XAML und Code Behind für die erste Seite der App angezeigt. Den vollständigen Code finden Sie im Abschnitt [vollständige Anpassung](./title-bar.md#full-customization-example) .


> [!IMPORTANT]
> Standardmäßig sind einige Elemente der Benutzeroberfläche, z. b. Raster, nicht an Treffer Tests beteiligt, wenn Sie nicht über einen Hintergrund Satz verfügen.
> Damit das Raster `AppTitleBar` im Beispiel unten das ziehen zulässt, müssen wir den Hintergrund auf festlegen `Transparent` .

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

Das UIElement ( `AppTitleBar` ) ist Teil des XAML für Ihre APP. Sie können entweder das Deklarieren und Festlegen der Titelleiste auf einer Stamm Seite, die sich nicht ändert, oder das Deklarieren und Festlegen eines Titelleisten Bereichs auf jeder Seite festlegen, zu der Ihre APP navigieren kann. Wenn Sie es auf jeder Seite festlegen, sollten Sie sicherstellen, dass der dragfähige Bereich konsistent bleibt, wenn ein Benutzer um Ihre APP navigiert.

Sie können settitlebar aufrufen, um zu einem neuen Titelleisten Element zu wechseln, während Ihre APP ausgeführt wird. Sie können auch **null** als Parameter an settitlebar übergeben, um das standardmäßige Zieh Verhalten wiederherzustellen. (Weitere Informationen finden Sie unter "standardmäßige dragfähige Region".)

> [!IMPORTANT]
> Der von Ihnen angegebene dragfähige Bereich muss gedrückt werden, was bedeutet, dass Sie für einige Elemente möglicherweise einen transparenten Hintergrund Pinsel festlegen müssen. Weitere Informationen finden Sie in den Hinweisen zu [VisualTreeHelper. findelta ementsinhostkoordinaten](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) .
>
>Wenn Sie z. b. ein Raster als den dragfähigen Bereich definieren, legen Sie fest, damit `Background="Transparent"` es dragfähig ist.
>
>Dieses Raster ist nicht dragfähig (aber sichtbare Elemente darin): `<Grid x:Name="AppTitleBar">` .
>
>Dieses Raster sieht identisch aus, aber das gesamte Raster ist dragfähig: `<Grid x:Name="AppTitleBar" Background="Transparent">` .

#### <a name="default-draggable-region"></a>Standardmäßige dragfähige Region

Wenn Sie keinen dragfähigen Bereich angeben, wird ein Rechteck, das die Breite des Fensters, die Höhe der Beschriftungs Schaltflächen und die Position am oberen Rand des Fensters ist, als der standardmäßige dragfähige Bereich festgelegt.

Wenn Sie einen dragfähigen Bereich definieren, verkleinert das System den standardmäßigen dragfähigen Bereich auf einen kleinen Bereich der Größe einer Titelleisten Schaltfläche, die sich links neben den Beschriftungs Schaltflächen befindet (oder nach rechts, wenn sich die Beschriftungs Schaltflächen auf der linken Seite des Fensters befinden). Dadurch wird sichergestellt, dass immer ein einheitlicher Bereich vorhanden ist, den der Benutzer ziehen kann.

### <a name="system-caption-buttons"></a>System Beschriftung

Das System reserviert die obere linke oder obere rechte Ecke des App-Fensters für die Beschriftungs Schaltflächen des Systems (zurück, minimieren, maximieren, schließen). Das System behält die Kontrolle über den Beschriftungs Steuerungs Bereich, um sicherzustellen, dass die minimale Funktionalität zum ziehen, minimieren, maximieren und Schließen des Fensters bereitgestellt wird. Das System zeichnet die Schaltfläche Schließen in der oberen rechten Ecke für Sprachen von links nach rechts und von rechts nach links in der linken oberen Ecke.

Die Abmessungen und die Position des Beschriftungs-Steuer Elements werden von der coreapplicationviewtitlebar-Klasse kommuniziert, sodass Sie Sie im Layout Ihrer Titelleisten-Benutzeroberfläche berücksichtigen können. Die Breite des reservierten Bereichs auf jeder Seite wird durch die Eigenschaften [systemoverlayleftinset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) oder [systemoverlayrightinset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) angegeben, und die Höhe wird durch die [height](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height) -Eigenschaft angegeben.

Sie können Inhalte unterhalb des Überschriften-Steuer Elements zeichnen, das durch diese Eigenschaften definiert ist, wie z. b. den App-Hintergrund. Sie sollten jedoch keine Benutzeroberfläche ablegen, mit der der Benutzer interagieren kann. Er empfängt keine Eingaben, weil Eingaben für die Beschriftungs Steuerelemente vom System behandelt werden.

Sie können das [layoutmetricschge](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) -Ereignis behandeln, um auf Änderungen in der Größe der Beschriftungs Schaltflächen zu reagieren. Dies kann beispielsweise der Fall sein, wenn die Schaltfläche "System zurück" angezeigt wird oder ausgeblendet ist. Behandeln Sie dieses Ereignis, um die Positionierung von Benutzeroberflächen Elementen zu überprüfen und zu aktualisieren, die von der Größe der Titelleiste abhängen.

In diesem Beispiel wird gezeigt, wie Sie das Layout Ihrer Titelleiste so anpassen, dass Änderungen wie das Anzeigen oder Ausblenden der System-zurück-Schaltfläche berücksichtigt werden. `AppTitleBar`, `LeftPaddingColumn` und `RightPaddingColumn` werden in der zuvor gezeigten XAML deklariert.

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

### <a name="interactive-content"></a>Interaktiver Inhalt

Sie können interaktive Steuerelemente wie Schaltflächen, Menüs oder ein Suchfeld im oberen Teil der APP platzieren, damit Sie in der Titelleiste angezeigt werden. Es gibt jedoch einige Regeln, die Sie befolgen müssen, um sicherzustellen, dass Ihre interaktiven Elemente Benutzereingaben erhalten.
- Sie müssen settitlebar aufrufen, um einen Bereich als dragfähigen Titelleisten Bereich zu definieren. Wenn Sie dies nicht tun, legt das System den standardmäßigen dragfähigen Bereich am oberen Rand der Seite fest. Das System verarbeitet dann alle Benutzereingaben in diesem Bereich und hindert Eingaben daran, Ihre Steuerelemente zu erreichen.
- Platzieren Sie die interaktiven Steuerelemente über dem oberen Rand des dragbaren Bereichs, der durch den Aufrufen von settitlebar (mit einer höheren z-Reihenfolge) definiert wird. Nehmen Sie keine untergeordneten Elemente des UIElement-Elements, das an settitlebar übergeben wird. Nachdem Sie ein Element an settitlebar übergeben haben, behandelt das System es wie die System Titelleiste und verarbeitet alle Zeiger Eingaben für dieses Element.

Hier hat das `TitleBarButton` -Element eine höhere z-Reihenfolge als `AppTitleBar` , sodass es Benutzereingaben erhält.

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

### <a name="transparency-in-caption-buttons"></a>Transparenz in Beschriftungs Schaltflächen

Wenn Sie "extendviewindetitlebar" auf " **true**" festlegen, können Sie den Hintergrund der Beschriftungs Schaltflächen transparent machen, damit der APP-Hintergrund angezeigt wird. In der Regel legen Sie für vollständige Transparenz den Hintergrund auf [Colors. transparent](/uwp/api/windows.ui.colors.Transparent) fest. Legen Sie für Teil Transparenz den Alphakanal für die [Farbe](/uwp/api/windows.ui.color) fest, auf die Sie die-Eigenschaft festlegen.

Diese applicationviewtitlebar-Eigenschaften können transparent sein:

- Buttonbackgroundcolor
- Buttonhoverbackgroundcolor
- Buttonpressedbackgroundcolor
- Buttoninactivebackgroundcolor

Die Hintergrundfarbe der Schaltfläche wird nicht auf die Schaltflächen "Schließen" und "gedrückt" angewendet. Die Schaltfläche Schließen verwendet immer die System definierte Farbe für diese Zustände.

Alle anderen Farbeigenschaften ignorieren weiterhin den Alphakanal. Wenn extendviewindetitlebar auf **false**festgelegt ist, wird der Alphakanal für alle applicationviewtitlebar-Farbeigenschaften immer ignoriert.

### <a name="full-screen-and-tablet-mode"></a>Vollbild-und Tablet-Modus

Wenn Ihre APP im _voll Bild_ Modus oder im _Tablet-Modus_ausgeführt wird, blendet das System die Steuer Schaltflächen Titelleiste und Beschriftung aus. Der Benutzer kann jedoch die Titelleiste aufrufen, um ihn als Überlagerung auf der Benutzeroberfläche der APP zu sehen.
Sie können das [coreapplicationviewtitlebar. isvisiblechanged](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) -Ereignis behandeln, um benachrichtigt zu werden, wenn die Titelleiste ausgeblendet oder aufgerufen wird. Sie können den benutzerdefinierten Titelleisten Inhalt bei Bedarf ein-oder ausblenden.

Dieses Beispiel zeigt, wie isvisiblechanged behandelt wird, um das zuvor gezeigte Element anzuzeigen und auszublenden `AppTitleBar` .

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
>Der _voll Bild_ Modus kann nur eingegeben werden, wenn er von ihrer App unterstützt wird. Weitere Informationen finden Sie unter [applicationview. isfullscreenmode](/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode) . Der [_Tablet-Modus_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) ist eine Benutzer Option auf unterstützter Hardware, sodass ein Benutzer eine beliebige App im Tablet-Modus ausführen kann.

## <a name="full-customization-example"></a>Beispiel für vollständige Anpassung

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

- Machen Sie es offensichtlich, wenn Ihr Fenster aktiv oder inaktiv ist. Ändern Sie mindestens die Farbe des Texts, der Symbole und der Schaltflächen in der Titelleiste.
- Definieren Sie am oberen Rand der APP-Canvas einen dragfähigen Bereich. Das Abgleichen der Platzierung von System Titelleisten erleichtert den Benutzern das Auffinden von.
- Definieren Sie einen dragfähigen Bereich, der der visuellen Titelleiste (sofern vorhanden) in der Canvas der APP entspricht.

## <a name="related-articles"></a>Verwandte Artikel

- [Acrylic](../style/acrylic.md)
- [Farbe](../style/color.md)
