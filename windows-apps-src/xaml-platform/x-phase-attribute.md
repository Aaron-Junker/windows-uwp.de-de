---
title: xPhase-Attribut
description: Verwenden Sie xPhase mit der xBind-Markuperweiterung zum inkrementellen Rendern von ListView- und GridView-Elementen und zur Verbesserung des Verschiebens.
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bf1372289afcc8649fff6c2ed56ad85aad46b76c
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997987"
---
# <a name="xphase-attribute"></a>x:Phase-Attribut


Verwenden Sie **x:Phase** mit der [Markup Erweiterung {x:Bind}](x-bind-markup-extension.md) , um [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) -und [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) -Elemente inkrementell zu Renderern und die schwenken zu verbessern. **x:Phase** bietet eine deklarative Möglichkeit, die gleiche Wirkung zu erzielen wie bei Verwendung des [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging)-Ereignisses zum manuellen Steuern des Renderns von Listenelementen. Weitere Informationen finden Sie auch unter [Inkrementelles Aktualisieren von ListView- und GridView-Elementen](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally).

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>XAML-Werte


| Begriff | BESCHREIBUNG |
|------|-------------|
| PhaseValue | Eine Zahl, die die Phase gibt an, in der das Element verarbeitet wird. Die Standardeinstellung ist 0. | 

## <a name="remarks"></a>Hinweise

Wenn eine Liste mit Toucheingabe oder mit dem Mausrad schnell verschoben wird, ist die Liste je nach Komplexität der Datenvorlage möglicherweise nicht in der Lage, Elemente schnell genug zu rendern, um mit der Geschwindigkeit des Bildlaufs Schritt zu halten. Dies gilt insbesondere für ein tragbares Gerät mit einer Strom effizienten CPU, wie z. b. einem Tablet.

Phasing ermöglicht das inkrementelle Rendern der Datenvorlage, sodass der Inhalt priorisiert werden kann und die wichtigsten Elemente zuerst gerendert werden. Dadurch kann die Liste beim schnellen Verschieben für jedes Element einige Inhalte anzeigen und mehr Elemente für jede Vorlage rendern, falls die Zeit dies zulässt.

## <a name="example"></a>Beispiel

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

Die Datenvorlage beschreibt vier Phasen:

1.  Stellt den DisplayName-Textblock dar. Alle Steuerelemente ohne Phase werden implizit als zur Phase 0 zugehörig betrachtet.
2.  Zeigt den prettyDate-Textblock.
3.  Zeigt die prettyFileSize- und prettyImageSize-Textblöcke.
4.  Zeigt das Bild.

Phasing ist eine Funktion von [{x:Bind}](x-bind-markup-extension.md), die aus [**ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase) abgeleitete Steuerelemente verwendet und die Elementvorlage für die Datenbindung inkrementell verarbeitet. Beim Rendern von Listenelementen stellt **ListViewBase** eine einzelne Phase für alle Elemente in der Ansicht dar, bevor der Wechsel zur nächsten Phase erfolgt. Das Rendern wird in zeitlich unterteilten Stapeln ausgeführt, damit die erforderliche Arbeit bei einem Listenbildlauf neu analysiert und nicht für Elemente ausgeführt werden kann, die nicht mehr sichtbar sind.

Das **x:Phase**-Attribut kann für jedes Element in einer Datenvorlage, die [{x:Bind}](x-bind-markup-extension.md) verwendet, angegeben werden. Wenn ein Element eine andere Phase als 0 aufweist, wird das Element aus der Ansicht ausgeblendet (über **Opacity**, nicht über **Visibility**), bis diese Phase verarbeitet wird und Bindungen aktualisiert werden. Wenn für ein von [**ListViewBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase) abgeleitetes Steuerelement ein Bildlauf durchgeführt wird, verwendet es die Elementvorlagen von Elementen, die nicht mehr auf dem Bildschirm sind, erneut, um die neu sichtbaren Elemente zu rendern. UI-Elemente in der Vorlage behalten ihre alten Werte bei, bis sie erneut an Daten gebunden werden. Phasing verursacht das Verzögern dieses Datenbindungsschritts, sodass Phasing die UI-Elemente für den Fall, dass sie veraltet sind, ausblenden muss.

Für jedes UI-Element darf nur eine Phase angegeben werden. Diese gilt dann für alle Bindungen des Elements. Wenn keine Phase angegeben ist, wird die Phase 0 angenommen.

Phasennummern müssen nicht fortlaufend sein und sind mit den Wert der [**ContainerContentChangingEventArgs.Phase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.phase) identisch. Das [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging)-Ereignis wird für jede Phase vor der Verarbeitung der **x:Phase**-Bindungen ausgelöst.

Phasing wirkt sich nur auf [{x:Bind}](x-bind-markup-extension.md) -Bindungen aus, nicht auf [{Binding}](binding-markup-extension.md)-Bindungen.

Phasing gilt nur, wenn die Elementvorlage mithilfe eines Steuerelements gerendert wird, das Phasing erkennt. Für Windows 10 bedeutet das [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) und [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView). Phasing gilt nicht für Datenvorlagen, die in anderen Elementsteuerelementen verwendet werden, oder für andere Szenarien, wie zum Beispiel die Abschnitte [**ContentTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) oder [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) – in diesen Fällen werden alle UI-Elemente auf einmal an Daten gebunden.

