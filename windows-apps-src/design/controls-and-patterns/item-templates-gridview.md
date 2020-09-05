---
description: Holen Sie sich Elementvorlagen, die Sie mit einem GridView-Steuerelement verwenden können, um Bilderkataloge, Bilder und Text sowie Bilder mit Textüberlagerungen anzuzeigen.
title: Elementvorlagen für Rasteransicht
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, UWP, Fluent
ms.openlocfilehash: 08849436f88dd9698f349f7fde64b51324212244
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172684"
---
# <a name="item-templates-for-grid-view"></a>Elementvorlagen für Rasteransicht

Die Elementvorlagen in diesem Abschnitt können Sie mit einem [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView)-Steuerelement verwenden. Mithilfe dieser Vorlagen erhalten Sie das Erscheinungsbild gängiger App-Typen.

Zur Veranschaulichung der Datenbindung werden über diese Vorlagen **GridViewItems** an die Recording-Beispielklasse aus der [Datenbindungsübersicht](../../data-binding/data-binding-quickstart.md) gebunden.

> [!NOTE] 
> Wenn ein **DataTemplate**-Element mehrere Steuerelemente enthält (z. B. mehr als einen einzelnen **TextBlock**), stammt der Name zur Verwendung durch Screenreader-Software derzeit von .ToString() für das Element. Zur Vereinfachung können Sie stattdessen [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties) im Stammelement von **DataTemplate** festlegen. Weitere Informationen zur Barrierefreiheit finden Sie unter [Übersicht über die Barrierefreiheit](../accessibility/accessibility-overview.md).

## <a name="icon-and-text"></a>Symbol und Text
Verwenden Sie diese Vorlagen, um eine Sammlung von Apps in einem Raster mit einem Symbol und Text anzuzeigen.

![Beispiel für Rasteransicht mit kleinem Symbol und Text](images/listitems/icontext.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="IconTextTemplate" x:DataType="local:Recording">
            <StackPanel Width="264" Height="48" Padding="12" Orientation="Horizontal" AutomationProperties.Name="{x:Bind CompositionName}">
                <SymbolIcon Symbol="Audio" VerticalAlignment="Top"/>
                <TextBlock Margin="12,0,0,0" Width="204" Text="{x:Bind CompositionName}"/>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal" HorizontalAlignment="Center"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

![Beispiel für Rasteransicht mit Symbol und zweizeiligem Text](images/listitems/icontext2.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="IconTextTemplate2" x:DataType="local:Recording">
            <StackPanel Width="264" Height="120" Padding="12" Orientation="Horizontal" AutomationProperties.Name="{x:Bind CompositionName}">
                <FontIcon Margin="0,6,0,0" FontSize="48" FontFamily="Segoe MDL2 Assets" FontWeight="Bold" Glyph="&#xE8D6;" VerticalAlignment="Top"/>
                <StackPanel Margin="16,1,0,0">
                    <TextBlock Width="176" Margin="0,0,0,2" TextWrapping="WrapWholeWords" TextTrimming="Clip" Text="{x:Bind CompositionName}"/>
                    <TextBlock Width="176" Height="48" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}" TextWrapping="WrapWholeWords" TextTrimming="Clip" Text="{x:Bind ArtistName}" />
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal" HorizontalAlignment="Center" Margin="40,0"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="image-gallery"></a>Bildergalerie
Verwenden Sie diese Vorlage, um eine Bildersammlung in einem Raster mit Mehrfachauswahlmodus anzuzeigen.

![Layout der GridView-Elemente](images/listitems/gridviewitems.png)
```xaml
<GridView SelectionMode="Multiple">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageGalleryDataTemplate">
            <Image Source="Placeholder.png" Height="180" Width="180" Stretch="UniformToFill"/>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="3" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```
## <a name="image-and-text"></a>Bild und Text
Verwenden Sie diese Vorlagen, um eine Mediensammlung mit darunter stehendem Text anzuzeigen.

![Beispiel für Rasteransicht mit quadratischen Bildern und Text](images/listitems/imageandtext.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageTextDataTemplate" x:DataType="local:Recording">
            <StackPanel Height="280" Width="180" Margin="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="180" Width="180" Stretch="UniformToFill"/>
                <StackPanel Margin="0,12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="10" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

![Beispiel für Rasteransicht mit rechteckigen Bildern und Text](images/listitems/imageandtext2.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageTextDataTemplate2" x:DataType="local:Recording">
            <StackPanel Height="280" Width="320" Margin="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="180" Width="320" Stretch="UniformToFill"/>
                <StackPanel Margin="0,12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="image-with-text-overlay"></a>Bild mit Textüberlagerung
Verwenden Sie diese Vorlage, um eine Mediensammlung mit Textüberlagerung anzuzeigen.

![Beispiel für Rasteransicht mit Bildern und Textüberlagerung](images/listitems/imageoverlay.png)
```xaml
<GridView ItemsSource="{x:Bind ViewModel.Recordings}">
    <GridView.ItemTemplate>
        <DataTemplate x:Name="ImageOverlayDataTemplate" x:DataType="local:Recording">
            <Grid Height="180" Width="320" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Stretch="UniformToFill"/>
                <StackPanel Orientation="Vertical" Height="60" VerticalAlignment="Bottom" Background="{ThemeResource SystemBaseLowColor}" Padding="12">
                    <TextBlock Text="{x:Bind CompositionName}"/>
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid MaximumRowsOrColumns="4" Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

## <a name="related-articles"></a>Verwandte Artikel
- [GridView-Klasse](/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [Übersicht über Datenbindung](../../data-binding/data-binding-quickstart.md)
- [Übersicht über die Barrierefreiheit](../accessibility/accessibility-overview.md)
- [Beispiel für ListView und GridView (Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Miniaturbilder](../../files/thumbnails.md)