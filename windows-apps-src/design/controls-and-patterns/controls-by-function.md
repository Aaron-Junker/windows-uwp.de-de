---
description: Stellt eine nach Funktionen geordnete Liste einiger Steuerelemente bereit, die Sie in Ihren Apps verwenden können.
title: Steuerelemente nach Funktion
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 36397e64215bfe4b57aac32e9eccc94182495688
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033303"
---
# <a name="controls-by-function"></a>Steuerelemente nach Funktion

Das XAML-Benutzeroberflächenframework für Windows bietet eine umfangreiche Bibliothek von Steuerelementen, welche die Entwicklung von Benutzeroberflächen unterstützen. Einige dieser Steuerelemente weisen eine visuelle Darstellung auf. Andere fungieren als Container für andere Steuerelemente oder Inhalte (z. B. Bilder und Medien). 

Laden Sie das [Beispiel für XAML-UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) herunter, um sich zahlreiche Windows-UI-Steuerelemente in Aktion anzusehen.

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/NavigationView">die App zu öffnen und NavigationView in Aktion zu sehen</a> .</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


Die folgende nach Funktionen geordnete Liste enthält die allgemeinen XAML-Steuerelemente, die Sie in Ihrer App verwenden können.

## <a name="appbars-and-commands"></a>App-Leisten und -Befehle

### <a name="app-bar"></a>App-Leiste
Eine Symbolleiste für anwendungsspezifische Befehle. Siehe Befehlsleiste.

Referenz: [AppBar](/uwp/api/Windows.UI.Xaml.Controls.AppBar) 

### <a name="app-bar-button"></a>App-Leistenschaltfläche
Eine Schaltfläche, die Befehle in Form einer App-Leiste anzeigt.

![Symbole für App-Leistenschaltflächen](images/controls/app-bar-buttons.png) 

Referenz: [AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [SymbolIcon](/uwp/api/Windows.UI.Xaml.Controls.SymbolIcon), [BitmapIcon](/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon), [FontIcon](/uwp/api/Windows.UI.Xaml.Controls.FontIcon), [PathIcon](/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 

Design und Vorgehensweise: [Leitfaden für App-Leiste und Befehlsleistensteuerelement](app-bars.md) 

Beispielcode: [Beispiel für XAML-Befehle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-separator"></a>Trennzeichen der App-Leiste
Trennt Befehlsgruppen in einer Befehlsleiste grafisch.

Referenz: [AppBarSeparator](/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 

Beispielcode: [Beispiel für XAML-Befehle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-toggle-button"></a>Umschaltfläche der App-Leiste
Eine Schaltfläche zum Wechseln zwischen den Befehlen in einer Befehlsleiste.

Referenz: [AppBarToggleButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 

Beispielcode: [Beispiel für XAML-Befehle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="command-bar"></a>Befehlsleiste
Eine spezielle App-Leiste zum Ändern der Größe von Schaltflächenelementen auf der App-Leiste.

![Befehlsleistensteuerelement](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
Referenz: [CommandBar](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 

Design und Vorgehensweise: [Leitfaden für App-Leiste und Befehlsleistensteuerelement](app-bars.md)

Beispielcode: [Beispiel für XAML-Befehle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="buttons"></a>Schaltflächen

### <a name="button"></a>Schaltfläche
Ein Steuerelement, das auf Benutzereingaben reagiert und ein **Click** -Ereignis auslöst.

![Standardschaltfläche](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

Referenz: [Schaltfläche](/uwp/api/Windows.UI.Xaml.Controls.Button) 

Design und Vorgehensweise: [Leitfaden für Schaltflächen-Steuerelement](buttons.md) 

### <a name="hyperlink"></a>Hyperlink
Siehe „Linkschaltfläche“.

### <a name="hyperlink-button"></a>Linkschaltfläche
Eine Schaltfläche, die als markierter Text dargestellt wird und den angegebenen URI in einem Browser öffnet.

![Linkschaltfläche](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="https://www.microsoft.com"/>
```

Referenz: [HyperlinkButton](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 

Design und Vorgehensweise: [Leitfaden für Hyperlinksteuerelement](hyperlinks.md)

### <a name="repeat-button"></a>Wiederholungsschaltfläche
Eine Schaltfläche, die ihr **Click** -Ereignis auslöst, das andauert, solange die Schaltfläche betätigt wird. 

![Ein Schaltflächen-Steuerelement zum Wiederholen](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

Referenz: [RepeatButton](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RepeatButton) 

Design und Vorgehensweise: [Leitfaden für Schaltflächen-Steuerelement](buttons.md) 

## <a name="collectiondata-controls"></a>Sammlungs-/Datensteuerelemente

### <a name="flip-view"></a>Flip-Ansicht
Ein Steuerelement, das eine Sammlung von Elementen darstellt, durch die der Benutzer jeweils einzeln blättern kann.

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

Referenz: [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView) 

Design und Vorgehensweise: [Leitfaden für Flip-Ansicht-Steuerelement](flipview.md) 

### <a name="grid-view"></a>Rasteransicht
Ein Steuerelement, das eine Sammlung von Elementen in Zeilen und Spalten darstellt, für die ein vertikaler Bildlauf durchgeführt werden kann.

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

Referenz: [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) 

Design und Vorgehensweise: [Listen](lists.md) 

Beispielcode: [ListView-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

### <a name="items-control"></a>Elementsteuerelement
Ein Steuerelement, das eine Sammlung von Elementen auf einer Benutzeroberfläche darstellt, die durch eine Datenvorlage angegeben wird. 

```xaml
<ItemsControl/>
```

Referenz: [ItemsControl](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 

### <a name="list-view"></a>Listenansicht
Ein Steuerelement, das eine Sammlung von Elementen in einer Liste darstellt, für die ein horizontaler Bildlauf durchgeführt werden kann.

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

Referenz: [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) 

Design und Vorgehensweise: [Listen](lists.md) 

Beispielcode: [ListView-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

## <a name="date-and-time-controls"></a>Datums- und Uhrzeitsteuerelemente

### <a name="calendar-date-picker"></a>Kalenderdatumsauswahl
Ein Steuerelement, mit dem Benutzer ein Datum über eine Kalender-Dropdownanzeige auswählen können.

![Kalenderdatumsauswahl mit offener Kalenderansicht](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

Referenz: [CalendarDatePicker](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker) 

Design und Vorgehensweise: [Kalender-, Datums- und Uhrzeitsteuerelemente](date-and-time.md)
 
### <a name="calendar-view"></a>Kalenderansicht
Eine konfigurierbare Kalenderanzeige, in der Benutzer ein einzelnes Datum oder mehrere Daten auswählen können.

```xaml
<CalendarView/>
```

Referenz: [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 

Design und Vorgehensweise: [Kalender-, Datums- und Uhrzeitsteuerelemente](date-and-time.md) 

### <a name="date-picker"></a>Datumsauswahl
Ein Steuerelement, mit dem ein Benutzer ein Datum auswählen kann.

![Datumsauswahlsteuerelement](images/controls_datepicker_expand.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

Referenz: [DatePicker](/uwp/api/Windows.UI.Xaml.Controls.DatePicker) 

Design und Vorgehensweise: [Kalender-, Datums- und Uhrzeitsteuerelemente](date-and-time.md)
 
### <a name="time-picker"></a>Zeitauswahl
Ein Steuerelement, mit dem ein Benutzer einen Zeitwert auswählen kann.

![TimePicker-Steuerelement](images/controls_timepicker_expand.png)

```xaml
<TimePicker Header="Arrival Time"/>
```

Referenz: [TimePicker](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) 

Design und Vorgehensweise: [Kalender-, Datums- und Uhrzeitsteuerelemente](date-and-time.md)

## <a name="flyouts"></a>Flyouts

### <a name="context-menu"></a>Kontextmenü
Siehe „Menü-Flyout“ und „Popupmenü“.

### <a name="flyout"></a>Flyout
Zeigt eine Meldung an, die einen Benutzereingriff erfordert. (Im Gegensatz zu einem Dialogfeld wird von einem Flyout kein separates Fenster erstellt und keine Benutzerinteraktion blockiert.)

![Flyout-Steuerelement](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

Referenz: [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout) 

Design und Vorgehensweise: [Flyouts](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>Menü-Flyout
Zeigt vorübergehend eine Liste der Befehle oder Optionen im Kontext der Benutzeraktion an.

![Menü-Flyoutsteuerelement](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

Referenz: [MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [MenuFlyoutSeparator](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutSeparator), [ToggleMenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.ToggleMenuFlyoutItem) 

Design und Vorgehensweise: [Menüs und Kontextmenüs](menus.md) 

Beispielcode: [Beispiel für XAML-Kontextmenü](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

### <a name="popup-menu"></a>Popupmenü
Ein benutzerdefiniertes Menü mit von Ihnen angegebenen Befehlen.

Referenz: [PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) 

Design und Vorgehensweise: [Dialogs](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>QuickInfo
Ein Popupfenster, das Informationen zu einem Element anzeigt. 
 
![QuickInfo-Steuerelement](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

Referenz: [ToolTip](/uwp/api/Windows.UI.Xaml.Controls.ToolTip), [ToolTipService](/uwp/api/Windows.UI.Xaml.Controls.ToolTipService) 

Design und Vorgehensweise: Richtlinien für QuickInfos 

## <a name="images"></a>Abbilder

### <a name="image"></a>Abbild
Ein Steuerelement, das ein Bild darstellt.

```xaml
<Image Source="Assets/Logo.png" />
```

Referenz: [Image](/uwp/api/Windows.UI.Xaml.Controls.Image) 

Design und Vorgehensweise: [Image und ImageBrush](images-imagebrushes.md) 

Beispielcode: [XAML-Steuerelementekatalog](/samples/microsoft/xaml-controls-gallery/xaml-controls-gallery/)

## <a name="graphics-and-ink"></a>Grafiken und Freihandstriche

### <a name="inkcanvas"></a>InkCanvas
Ein Steuerelement, das Freihandstriche empfängt und anzeigt.

```xaml
<InkCanvas/>
```

Referenz: [InkCanvas](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 

### <a name="shapes"></a>Formen
Verschiedene grafische Speichermodusobjekte, die als Ellipsen, Rechtecke, Linien, Bézierpfade usw. dargestellt werden können.

![Polygon](images/controls/shapes-polygon.png) 
![Pfad](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

Referenz: [Formen](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 

Ausführung [Zeichnen von Formen](./shapes.md) 

Beispielcode: [Beispiel für vektorbasierte XAML-Zeichnung](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20vector-based%20drawing%20sample%20(Windows%208))

## <a name="layout-controls"></a>Layoutsteuerelemente

### <a name="border"></a>Rahmen
Ein Containersteuerelement, das einen Rahmen, einen Hintergrund oder beides um ein anderes Objekt herum zeichnet.

![Ein Rahmen um zwei Rechtecke](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

Referenz: [Border](/uwp/api/Windows.UI.Xaml.Controls.Border)

### <a name="canvas"></a>Canvas
Ein Layoutpanel, das die absolute Positionierung untergeordneter Elemente relativ zur oberen linken Ecke der Canvas unterstützt.
 
![Canvas-Layoutpanel](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

Referenz: [Canvas](/uwp/api/Windows.UI.Xaml.Controls.Canvas)
 
### <a name="grid"></a>Raster
Ein Layoutpanel, das die Anordnung von untergeordneten Elementen in Zeilen und Spalten unterstützt.

![Raster-Layoutpanel](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

Referenz: [Grid](/uwp/api/Windows.UI.Xaml.Controls.Grid)
 
### <a name="panning-scroll-viewer"></a>Verschiebungs-Bildlaufanzeige
Siehe „Bildlaufanzeige“.

### <a name="relativepanel"></a>RelativePanel
Ein Bereich, in dem Sie untergeordnete Objekte relativ zueinander oder in Relation zum übergeordneten Objekt positionieren und ausrichten können.

![RelativePanel-Layoutpanel](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

Referenz: [RelativePanel](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)

### <a name="scroll-bar"></a>Bildlaufleiste
Siehe Bildlaufanzeige. (ScrollBar ist ein Element von ScrollViewer. Es wird normalerweise nicht als eigenständiges Steuerelement verwendet.)

Referenz: [ScrollBar](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar)
 
### <a name="scroll-viewer"></a>Bildlaufanzeige
Ein Containersteuerelement, mit dem der Benutzer Inhalte verschieben und vergrößern/verkleinern kann.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

Referenz: [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)

Design und Vorgehensweise: [Leitfaden für Steuerelemente für Bildlauf und Schwenken](scroll-controls.md) 

Beispielcode: [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208))

### <a name="stack-panel"></a>StackPanel
Ein Layoutpanel, das untergeordnete Elemente in einer einzelnen Zeile anordnet. Die Zeile kann horizontal oder vertikal ausgerichtet werden.

![StackPanel-Layoutsteuerelement](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

Referenz: [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
Ein Layoutpanel, das die Anordnung von untergeordneten Elementen in Zeilen und Spalten unterstützt. Jedes untergeordnete Element kann sich über mehrere Zeilen und Spalten erstrecken.

![Umbruchraster-Layoutpanel mit variabler Größe](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

Referenz: [VariableSizedWrapGrid](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid)

### <a name="viewbox"></a>Viewbox
Ein Containersteuerelement, das seinen Inhalt auf eine angegebene Größe skaliert.

![Viewbox-Steuerelement](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

Referenz: [Viewbox](/uwp/api/Windows.UI.Xaml.Controls.Viewbox)
 
### <a name="zooming-scroll-viewer"></a>Zoombildlaufanzeige
Siehe „Bildlaufanzeige“.

## <a name="media-controls"></a>Mediensteuerelemente

### <a name="audio"></a>Audio
Siehe „Medienelement“.

### <a name="media-element"></a>Medienelement
Ein Steuerelement, das Audio- und Videoinhalte wiedergibt.

```xaml
<MediaElement x:Name="myMediaElement"/>
```

Referenz: [MediaElement](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 

Design und Vorgehensweise: [Leitfaden für Medienelement-Steuerelement](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
Ein Steuerelement, das Wiedergabesteuerelemente für eine „MediaElement“-Klasse bereitstellt.

![Medienelement mit Transportsteuerelementen](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

Referenz: [MediaTransportControls](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 

Design und Vorgehensweise: [Leitfaden für Medienelement-Steuerelement](media-playback.md) 

Beispielcode: [Beispiel für Steuerelemente für den Medientransport](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls)

### <a name="video"></a>Video
Siehe „Medienelement“.

## <a name="navigation"></a>Navigation

### <a name="navigationview"></a>NavigationView

Ein anpassbarer Container und ein flexibles Navigationsmodell, das den linken Navigationsbereich, die obere Navigation und das Muster der Registerkarten implementiert.

Referenz: [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)

Design und Vorgehensweise: [Leitfaden für das NavigationView-Steuerelement](navigationview.md)

### <a name="splitview"></a>SplitView

Ein Containersteuerelement mit zwei Ansichten: einer Ansicht für den Hauptinhalt und einer weiteren Ansicht, die in der Regel für ein Navigationsmenü verwendet wird.

![Steuerelement für geteilte Ansicht](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

Referenz: [SplitView](/uwp/api/Windows.UI.Xaml.Controls.SplitView) 

Design und Vorgehensweise: [Leitfaden für das Steuerelement für geteilte Ansicht](split-view.md)

### <a name="web-view"></a>Webansicht

Ein Containersteuerelement, das Webinhalt hostet.

```xaml
<WebView x:Name="webView1" Source="https://developer.microsoft.com" 
         Height="400" Width="800"/>
```

Referenz: [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) 

Design und Vorgehensweise: Richtlinien für Webansichten 

Beispielcode: [Beispiel für XAML-WebView-Steuerelement](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208))

### <a name="semantic-zoom"></a>Semantischer Zoom

Ein Containersteuerelement, das es dem Benutzer ermöglicht, zwischen zwei Ansichten einer Sammlung zu zoomen.

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

Referenz: [SemanticZoom](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 

Design und Vorgehensweise: [Leitfaden für semantisches Zoomsteuerelement](semantic-zoom.md)

Beispielcode: [Beispiel für XAML-GridView-Gruppierung und -SemanticZoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20GridView%20grouping%20and%20SemanticZoom%20sample%20(Windows%208))

## <a name="progress-controls"></a>Statussteuerelemente

### <a name="progress-bar"></a>Statusleiste
Ein Steuerelement, das den Fortschritt durch Anzeigen einer Leiste angibt.

![Statusleistensteuerelement](images/controls/progress-bar-determinate.png)

Eine Fortschrittsleiste, die einen bestimmten Wert anzeigt.

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![Steuerelement für unbestimmte Statusanzeige](images/controls/progress-bar-indeterminate.gif)

Eine Fortschrittsleiste, die einen unbestimmten Fortschritt anzeigt.

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

Referenz: [ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) 

Design und Vorgehensweise: [Leitfaden für Statussteuerelemente](progress-controls.md) 

### <a name="progress-ring"></a>Statusring
Ein Steuerelement, das den Status durch Anzeigen eines Rings angibt. 

![Statusringsteuerelement](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

Referenz: [ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 

Design und Vorgehensweise: [Leitfaden für Statussteuerelemente](progress-controls.md) 

## <a name="text-controls"></a>Textsteuerelemente

### <a name="auto-suggest-box"></a>Feld mit automatischen Vorschlägen
Ein Texteingabefeld, das Text vorschlägt, während der Benutzer Zeichen eingibt.

![Feld mit automatischen Vorschlägen für die Suche](images/controls/auto-suggest-box.png) 

Referenz: [AutoSuggestBox](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)

Design und Vorgehensweise: [Textsteuerelemente](text-controls.md), [Richtlinien für Feldsteuerelement mit automatischen Vorschlägen](auto-suggest-box.md)

Beispielcode: [Beispiel für AutoSuggestBox-Migration](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

### <a name="multi-line-text-box"></a>Mehrzeiliges Textfeld
Siehe „Textfeld“.

### <a name="password-box"></a>Kennwortfeld
Ein Steuerelement für die Kennworteingabe.

 ![Ein Kennwortfeld](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

Referenz: [PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) 

Design und Vorgehensweise: [Textsteuerelemente](text-controls.md), [Richtlinien für Kennwortfelder](password-box.md) 

Beispielcode: [Beispiel für die XAML-Textanzeige](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208)), [Beispiel für die XAML-Textbearbeitung](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))

### <a name="rich-edit-box"></a>Rich-Edit-Feld
Ein Steuerelement, mit dem der Benutzer Rich-Text-Dokumente mit Inhalten wie formatiertem Text, Links und Bildern bearbeiten kann.

```xaml
<RichEditBox />
```

Referenz: [RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 

Design und Vorgehensweise: [Textsteuerelemente](text-controls.md), [Leitfaden für RichEditBox-Steuerelement](rich-edit-box.md)

Beispielcode: [Beispiel für XAML-Text](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))

### <a name="search-box"></a>Suchfeld
Siehe „Feld mit automatischen Vorschlägen“.

### <a name="single-line-text-box"></a>Einzeiliges Textfeld
Siehe „Textfeld“.

### <a name="static-textparagraph"></a>Statischer Text/Absatz
Siehe „Textblock“.

### <a name="text-block"></a>Textblock
Ein Steuerelement, das Text angezeigt.

![Textblocksteuerelement](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

Referenz: [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 

Design und Vorgehensweise: [Textsteuerelemente](text-controls.md), [Leitfaden für TextBlock-Steuerelement](text-block.md), [Richtlinie für Rich-Text-Blocksteuerelemente](rich-text-block.md)

Beispielcode: [Beispiel für XAML-Text](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))

### <a name="text-box"></a>Textfeld
Ein einzeiliges oder mehrzeiliges Nur-Text-Feld.

![Textfeldsteuerelement](images/controls/text-box.png)

```xaml
<TextBox x:Name="textBox1" Text="I am a Text Box."
         TextChanged="TextBox_TextChanged"/>
```

Referenz: [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 

Design und Vorgehensweise: [Textsteuerelemente](text-controls.md), [Leitfaden für TextBox-Steuerelement](text-box.md) 

Beispielcode: [Beispiel für XAML-Text](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))

## <a name="selection-controls"></a>Auswahlsteuerelemente

### <a name="check-box"></a>Kontrollkästchen
Ein Steuerelement, das der Benutzer aktivieren und deaktivieren kann.

![Die drei Zustände eines Kontrollkästchens](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

Referenz: [CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 

Design und Vorgehensweise: [Leitfaden für Kontrollkästchen-Steuerelement](checkbox.md) 

### <a name="combo-box"></a>Kombinationsfeld
Eine Dropdownliste mit Elementen, in der ein Benutzer eine Auswahl treffen kann.

![Offenes Kombinationsfeld](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

Referenz: [ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 

Design und Vorgehensweise: [Listen](lists.md) 

### <a name="list-box"></a>Listenfeld
Ein Steuerelement, das eine Inlineliste mit Elementen darstellt, aus der ein Benutzer eine Auswahl treffen kann. 

![Listenfeldsteuerelement](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>List item 1</x:String>
    <x:String>List item 2</x:String>
    <x:String>List item 3</x:String>
</ListBox>
```

Referenz: [ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 

Design und Vorgehensweise: [Listen](lists.md) 

### <a name="radio-button"></a>Optionsfeld
Ein Steuerelement, das es einem Benutzer ermöglicht, eine einzelne Option aus einer Gruppe von Optionen auszuwählen. Gruppierte Optionsfelder schließen sich gegenseitig aus.

![Optionsfeldsteuerelemente](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

Referenz: [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 

Design und Vorgehensweise: [Leitfaden für Optionsfeldsteuerelement](radio-button.md)
 
### <a name="slider"></a>Slider
Ein Steuerelement, über das der Benutzer aus einer Reihe von Werten auswählen kann, indem er ein Schiebereglersteuerelement über einen Bereich verschiebt.

![Schiebereglersteuerelement](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

Referenz: [Schieberegler](/uwp/api/Windows.UI.Xaml.Controls.Slider) 

Design und Vorgehensweise: [Leitfaden für Schiebereglersteuerelement](slider.md) 

### <a name="toggle-button"></a>Umschalter
Eine Schaltfläche, mit der zwischen zwei Zuständen gewechselt werden kann.

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

Referenz: [ToggleButton](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton)

Design und Vorgehensweise: [Leitfaden für Umschaltersteuerelement](toggles.md) 

### <a name="toggle-switch"></a>Umschalter
Ein Schalter, mit dem zwischen zwei Zuständen hin und her geschaltet werden kann.

![Umschaltersteuerelement](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

Referenz: [ToggleSwitch](/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) 

Design und Vorgehensweise: [Leitfaden für Umschaltersteuerelement](toggles.md)
