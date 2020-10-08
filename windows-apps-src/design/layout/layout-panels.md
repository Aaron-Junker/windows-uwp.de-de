---
description: Erfahren Sie, wie Sie integrierte XAML-Layoutbereiche wie „RelativePanel“, „StackPanel“, „Grid“ und „Canvas“ verwenden, um Oberflächenelemente in Ihrer App anzuordnen und zu gruppieren.
title: Layoutpanels für Windows-Apps
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3fd8d57db92b8a59f32935586edfc247ec3c281f
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750456"
---
# <a name="layout-panels"></a>Layoutpanels

Layoutpanels sind Container und dienen zum Anordnen und Gruppieren von UI-Elementen in Ihrer App. Zu den integrierten XAML-Layoutpanels gehören [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel), [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel), [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) und [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas). Hier werden die einzelnen Panels beschrieben und es wird erläutert, wie Sie sie zum Layouten von XAML-UI-Elementen einsetzen.

Bei der Wahl eines Layoutpanels müssen mehrere Dinge berücksichtigt werden:
- Wie das Panel seine untergeordneten Elemente positioniert
- Wie das Panel die Größe seiner untergeordneten Elemente bestimmt
- Wie sich überlappende untergeordnete Elemente übereinander geschichtet werden (Z-Reihenfolge)
- Anzahl und Komplexität geschachtelter Panelelemente, die erforderlich sind, um das gewünschte Layout zu erstellen

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie auf Ihrem System die <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong>-App installiert ist, können Sie <a href="xamlcontrolsgallery:/item/RelativePanel">RelativePanel</a>, <a href="xamlcontrolsgallery:/item/StackPanel">StackPanel</a>, <a href="xamlcontrolsgallery:/item/Grid">Grid</a>, <a href="xamlcontrolsgallery:/item/VariableSizedWrapGrid">VariableSizedWrapGrid</a>, und <a href="xamlcontrolsgallery:/item/Canvas">Canvas</a> in Aktion erleben.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="panel-properties"></a>Paneleigenschaften

Bevor wir die einzelnen Panels besprechen, lassen Sie uns einige gemeinsame Eigenschaften aller Panels durchgehen. 

### <a name="panel-attached-properties"></a>Angefügte Paneleigenschaften

Die meisten XAML-Layoutpanels verwenden angefügte Eigenschaften, mit denen die untergeordneten Elemente das übergeordnete Panel darüber informieren, wie sie in der Benutzeroberfläche positioniert werden sollen. Angefügte Eigenschaften verwenden die Syntax *AttachedPropertyProvider.PropertyName*. Wenn in anderen Panels geschachtelte Panels vorhanden sind, werden angefügte Eigenschaften von UI-Elementen, die Layoutmerkmale für ein übergeordnetes Element angeben, nur vom direkt übergeordneten Panel interpretiert.

Hier sehen Sie ein Beispiel für das Festlegen der angefügten Eigenschaft [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) auf einem Schaltflächen-Steuerelement in XAML. Hiermit wird das übergeordnete Canvas-Element darüber informiert, dass das Button-Element 50 effektive Pixel vom linken Rand des Canvas-Elements entfernt positioniert werden soll.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Übersicht über angefügte Eigenschaften](../../xaml-platform/attached-properties-overview.md).

### <a name="panel-borders"></a>Panelrahmen

Die Panels „RelativePanel“, „StackPanel“ und „Grid“ definieren Rahmeneigenschaften, mit denen Sie einen Rahmen um das Panel zeichnen können, ohne dass ein zusätzliches umschließendes Rahmenelement erforderlich ist. Die Rahmeneigenschaften sind **BorderBrush**, **BorderThickness**, **CornerRadius** und **Padding**.

Hier sehen Sie ein Beispiel für das Festlegen von Rahmeneigenschaften für ein „Grid“.

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![Ein Raster mit Rahmen](images/layout-panel-grid-border.png)

Durch Verwendung der integrierten Rahmeneigenschaften kann die Anzahl der XAML-Elemente reduziert und dadurch die UI-Leistung Ihrer App verbessert werden. Weitere Informationen zu Layoutpanels und zur UI-Leistung finden Sie unter [Optimieren des XAML-Layouts](../../debug-test-perf/optimize-your-xaml-layout.md).

## <a name="relativepanel"></a>RelativePanel

[**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) ermöglicht Ihnen das Layout von UI-Elementen, indem Sie angeben, an welcher Position sie relativ zu anderen Elementen und dem Panel platziert werden sollen. Standardmäßig wird ein Element in der oberen linken Ecke des Panels positioniert. Sie können RelativePanel mit [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) und [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) verwenden, um die Benutzeroberfläche für unterschiedliche Fenstergrößen neu anzuordnen.

Die folgende Tabelle zeigt die angefügten Eigenschaften, mit denen Sie ein Element in Bezug auf andere Elemente ausrichten und positionieren können.

Panelausrichtung | Ausrichtung gleichgeordneter Elemente | Position gleichgeordneter Elemente
----------------|-------------------|-----------------
[**AlignTopWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithpanelproperty) | [**AlignTopWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithproperty) | [**Above**](/uwp/api/windows.ui.xaml.controls.relativepanel)  
[**AlignBottomWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithpanelproperty) | [**AlignBottomWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithproperty) | [**Below**](/uwp/api/windows.ui.xaml.controls.relativepanel.belowproperty)  
[**AlignLeftWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel) | [**AlignLeftWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.getalignleftwith) | [**LeftOf**](/uwp/api/windows.ui.xaml.controls.relativepanel.leftofproperty)  
[**AlignRightWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithpanelproperty) | [**AlignRightWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithproperty) | [**RightOf**](/uwp/api/windows.ui.xaml.controls.relativepanel.setrightof)  
[**AlignHorizontalCenterWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty) | [**AlignHorizontalCenterWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithproperty) | &nbsp;   
[**AlignVerticalCenterWithPanel**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanelproperty) | [**AlignVerticalCenterWith**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithproperty) | &nbsp;   

 
Dieser XAML-Code zeigt das Anordnen von Elementen in einem „RelativePanel“.

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Orange"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

Das Ergebnis sieht wie folgt aus. 

![RelativePanel](images/layout-panel-relative-panel.png)

Die folgenden Aspekte müssen in Bezug auf das Ändern der Rechteckgrößen beachtet werden:
- Das rote Rechteck erhält eine explizite Größe von 44 x 44. Es wird in der oberen linken Ecke des Panels platziert; dies ist die Standardposition.
- Das grüne Rechteck erhält eine explizite Höhe von 44. Die linke Seite dieses Rechtecks wird auf das rote Rechteck ausgerichtet und die rechte Seite auf das blaue Rechteck, das die Breite bestimmt.
- Das orangefarbene Rechteck erhält keine explizite Größe. Seine linke Seite wird auf das blaue Rechteck ausgerichtet. Die rechte und untere Kante werden auf die Kante des Panels ausgerichtet. Die Größe wird durch diese Ausrichtungen bestimmt und ändert sich zusammen mit dem Panel.

## <a name="stackpanel"></a>StackPanel

[**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) ist ein Layoutpanel, das untergeordnete Elemente in einer einzelnen Zeile anordnet. Die Zeile kann horizontal oder vertikal ausgerichtet werden. StackPanel wird normalerweise verwendet, um einen kleinen Teilbereich der Benutzeroberfläche auf einer Seite anzuordnen.

Mit der [**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation)-Eigenschaft können Sie die Richtung der untergeordneten Elemente angeben. Die Standardausrichtung ist [**Vertikal**](/uwp/api/Windows.UI.Xaml.Controls.Orientation).

Der folgende XAML-Code veranschaulicht das Erstellen eines vertikalen StackPanel von Elementen.

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Orange" Height="44"/>
</StackPanel>
```


Das Ergebnis sieht wie folgt aus.

![StackPanel](images/layout-panel-stack-panel.png)

Wird in einem StackPanel die Größe eines untergeordneten Elements nicht explizit festgelegt, wird das Element gestreckt, sodass es die zur Verfügung stehende Breite (oder Höhe, falls die Ausrichtung auf **Horizontal** festgelegt ist) ausfüllt. In diesem Beispiel ist die Breite der Rechtecke nicht festgelegt. Die Rechtecke werden erweitert, bis Sie die gesamte Breite von StackPanel ausfüllen.

## <a name="grid"></a>Raster

Das [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)-Panel unterstützt Fluid-Layouts und ermöglicht die Anordnung von Steuerelementen in mehrzeiligen und mehrspaltigen Layouts. Sie können die Zeilen und Spalten eines Rasterpanels mit den Eigenschaften [**RowDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) und [**ColumnDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.columndefinitions) angeben.

Mit den angefügten Eigenschaften [**Grid.Column**](/dotnet/api/system.windows.controls.grid.column) und [**Grid.Row**](/dotnet/api/system.windows.controls.grid.row) ordnen Sie Objekte in bestimmten Zellen an.

Wenn der Inhalt über mehrere Zeilen und Spalten angezeigt werden soll, können Sie die angefügten Eigenschaften [**Grid.RowSpan**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms605035(v=vs.95)) und [**Grid.ColumnSpan**](/dotnet/api/system.windows.controls.grid.columnspan) verwenden.

In diesem XAML-Beispiel wird veranschaulicht, wie Sie ein Rasterelement mit zwei Zeilen und zwei Spalten erstellen.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


Das Ergebnis sieht wie folgt aus.

![Raster](images/layout-panel-grid.png)

In diesem Beispiel funktioniert die Festlegung der Größe wie folgt: 
- Die zweite Zeile weist eine explizite Höhe von 44 effektiven Pixel auf. Standardmäßig füllt die Höhe der ersten Zeile den gesamten verbleibenden Platz.
- Die Breite der ersten Spalte ist auf **Auto** festgelegt, d. h. sie ist so breit, wie dies für ihre untergeordneten Elemente erforderlich ist. In diesem Fall ist die Spalte 44 effektive Pixel breit, um der Breite des roten Rechtecks zu entsprechen.
- Es sind keine anderen Größeneinschränkungen für die Rechtecke festgelegt, daher wird jedes gestreckt, um die Rasterzelle ausfüllen, in der es sich befindet.

Verteilen Sie den Platz innerhalb einer Spalte oder Zeile mit der automatischen Größenanpassung **Auto** oder per Größenanpassung mit Sternvariablen. Verwenden Sie die automatische Größenanpassung, damit die Größe von UI-Elementen entsprechend ihren Inhalten oder der Größe des übergeordneten Containers geändert wird. Sie können die automatische Größenanpassung auch für die Zeilen und Spalten eines Rasters verwenden. Um die automatische Größenanpassung zu verwenden, legen Sie für „Height“ und/oder „Width“ von UI-Elementen **Auto** fest.

Die proportionale Größenanpassung, die auch als *Größenanpassung mit Sternvariable* bezeichnet wird, wird zum gleichmäßigen Aufteilen des verfügbaren Platzes auf die Zeilen und Spalten eines Rasters verwendet. In XAML werden Sternwerte als \* ausgedrückt (bzw. *n*\* für gleichmäßige Größenanpassung mit Sternvariable). Möchten Sie also z. B. angeben, dass eine Spalte fünfmal breiter als die zweite Spalte eines zweispaltigen Layouts ist, verwenden Sie "5\*" und "\*" für die [**Width**](/uwp/api/windows.ui.xaml.controls.columndefinition.width)-Eigenschaften der [**ColumnDefinition**](/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition)-Elemente.

In diesem Beispiel wird die feste, automatische und proportionale Größenanpassung in einem [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) mit 4 Spalten kombiniert.

| Column | Festlegen der Größe | BESCHREIBUNG |
| ------ | ------ | ----------- |
Column_1 | **Auto** | Die Breite der Spalte wird entsprechend ihres Inhalts angepasst.
Column_2 | * | Nach dem Berechnen der Spalten mit automatischer Breite erhält diese Spalte einen Teil der verbleibenden Breite. Column_2 ist nur halb so breit wie Column_4.
Column_3 | **44** | Die Spalte ist 44 Pixel breit.
Column_4 | **2**\* | Nach dem Berechnen der Spalten mit automatischer Breite erhält diese Spalte einen Teil der verbleibenden Breite. Column_4 ist doppelt so breit wie Column_2.

Die standardmäßige Spaltenbreite beträgt "*", daher Sie müssen diesen Wert für die zweite Spalte nicht explizit festlegen.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

Im Visual Studio-XAML-Designer sieht das Ergebnis wie folgt aus.

![Ein Raster mit vier Spalten im Visual Studio-Designer](images/xaml-layout-grid-in-designer.png)

## <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid

[**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) ist ein Layoutpanel im Grid-Stil, bei dem Zeilen oder Spalten automatisch in eine neue Zeile oder Spalte umgebrochen werden, wenn der Wert [**MaximumRowsOrColumns**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns) erreicht wird. 

Mit der [**Orientation**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.orientation)-Eigenschaft wird angegeben, ob Elemente im Raster vor dem Umbrechen in Zeilen oder Spalten hinzugefügt werden. Die Standardausrichtung ist **Vertikal**, d. h., das Raster fügt Elemente von oben nach unten hinzu, bis eine Spalte voll ist, dann erfolgt ein Umbruch in eine neue Spalte. Wenn der Wert **Horizontal** lautet, fügt das Raster Elemente von links nach rechts hinzu und fügt dann einen Umbruch in eine neue Zeile ein.

Zellenabmessungen werden mit den Eigenschaften [**ItemHeight**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight) und [**ItemWidth**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth) angegeben. Jede Zelle hat die gleiche Größe. Wenn „ItemHeight“ oder „ItemWidth“ nicht angegeben wird, wird die Größe der erste Zelle ihrem Inhalt entsprechend festgelegt, und alle anderen Zellen haben die Größe der ersten Zelle.

Sie können die angefügten Eigenschaften [**VariableSizedWrapGrid.ColumnSpan**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid) und [**VariableSizedWrapGrid.RowSpan**](/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.getrowspan) verwenden, um anzugeben, wie viele benachbarte Zellen ein untergeordnetes Element füllen soll.

Hier sehen Sie die Verwendung eines VariableSizedWrapGrid-Elements in XAML.

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


Das Ergebnis sieht wie folgt aus.

![Umbruchraster mit variabler Größe](images/layout-panel-variable-size-wrap-grid.png)

In diesem Beispiel beträgt die maximale Anzahl von Zeilen in jeder Spalte 3. Die erste Spalte enthält nur zwei Elemente (das rote und das blaue Rechteck), da sich das blaue Rechteck über zwei Zeilen erstreckt. Das grüne Rechteck wird dann an den Anfang der nächsten Spalte umbrochen.

## <a name="canvas"></a>Canvas

Das [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas)-Panel positioniert seine untergeordneten Elemente mit festen Koordinatenpunkten und unterstützt keine Fluid-Layouts. Sie geben die Punkte in den einzelnen untergeordneten Elementen an, indem Sie für jedes Element die angefügten Eigenschaften [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) und [**Canvas.Top**](/dotnet/api/system.windows.controls.canvas.top) festlegen. Das übergeordnete Canvas liest während des [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange)-Durchlaufs des Layouts diese angehängten Eigenschaftswerte von seinen untergeordneten Elementen.

Objekte in einem Canvas-Panel können sich überlappen, wobei ein Objekt über einem anderen Objekt gezeichnet wird. Standardmäßig rendert das Canvas-Element untergeordnete Objekte in der Reihenfolge, in der sie deklariert werden, d. h., das letzte untergeordnete Element wird im Vordergrund gerendert (jedes Element verfügt über einen standardmäßigen Z-Index von 0). Dies ist genauso wie bei anderen integrierten Panels. Canvas unterstützt jedoch auch die angefügte Eigenschaft [**Canvas.ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)), die Sie für die einzelnen untergeordneten Elemente festlegen können. Sie können diese Eigenschaft im Code festlegen, um die Zeichnungsreihenfolge von Elementen zur Laufzeit zu ändern. Das Element mit dem höchsten Canvas.ZIndex-Wert wird zuletzt gezeichnet, also über allen anderen Elementen, die sich im gleichen Raum befinden oder sich auf irgendeine Weise überlappen. Beachten Sie, dass der Alpha-Wert (Transparenz) berücksichtigt wird. Auch wenn sich Elemente überlappen, werden die Inhalte in Überlappungsbereichen also ggf. vermischt, wenn das obere Element über einen Alpha-Wert verfügt, der nicht dem Maximalwert entspricht.

Das Canvas-Element legt die Größe seiner untergeordneten Elemente nicht fest. Jedes Element muss seine Größe selbst angeben.

Im Folgenden finden Sie ein Beispiel für ein Canvas-Element in XAML.

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

Das Ergebnis sieht wie folgt aus.

![Canvas](images/layout-panel-canvas.png)

Verwenden Sie das Canvas-Panel mit Bedacht. Es ist zwar in einigen Szenarien hilfreich, die Positionen von UI-Elementen genau steuern zu können, aber eine feste Positionierung eines Layoutpanels bewirkt, dass dieser Bereich der UI weniger gut an allgemeine Änderungen der Größe von App-Fenstern angepasst werden kann. Ursachen für Größenänderungen von App-Fenstern können Änderungen der Geräteausrichtung, Teilungen von App-Fenstern, Änderungen von Monitoren und verschiedene andere Benutzerszenarien sein.

## <a name="panels-for-itemscontrol"></a>Panels für ItemsControl

Es gibt verschiedene spezielle Panels, die nur als [**ItemsPanel**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) zum Anzeigen von Elementen in einem [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)-Element verwendet werden können. Dies sind [**ItemsStackPanel**](/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel), [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid), [**VirtualizingStackPanel**](/uwp/api/Windows.UI.Xaml.Controls.VirtualizingStackPanel) und [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid). Diese Panel können nicht für das allgemeine UI-Layout verwendet werden.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
