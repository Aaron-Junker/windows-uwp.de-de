---
Description: Elementvorlagen für Listenansicht
title: Elementvorlagen für Listenansicht
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, UWP, Fluent
ms.openlocfilehash: 9328c3f156acd13fd8947e01e924bf0d6849c0a6
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684405"
---
# <a name="item-templates-for-list-view"></a>Elementvorlagen für Listenansicht

Die Elementvorlagen in diesem Abschnitt können Sie mit einem [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)-Steuerelement verwenden. Mithilfe dieser Vorlagen erhalten Sie das Erscheinungsbild gängiger App-Typen. 

Zur Veranschaulichung der Datenbindung werden über diese Vorlagen **ListViewItems** an die Recording-Beispielklasse aus der [Datenbindungsübersicht](../../data-binding/data-binding-quickstart.md) gebunden.

> [!NOTE] 
> Wenn ein **DataTemplate**-Element mehrere Steuerelemente enthält (z. B. mehr als einen einzelnen **TextBlock**), stammt der Name zur Verwendung durch Screenreader-Software derzeit von .ToString() für das Element. Zur Vereinfachung können Sie stattdessen [**AutomationProperties.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties) im Stammelement von **DataTemplate** festlegen. Weitere Informationen zur Barrierefreiheit finden Sie unter [Übersicht über die Barrierefreiheit](../accessibility/accessibility-overview.md).

## <a name="single-line-list-item"></a>Einzeiliges Listenelement
Verwenden Sie diese Vorlage, um eine Liste von Elementen mit einem Bild und einzeiligem Text anzuzeigen.

![Beispiel für einzeiliges Listenelement](images/listitems/singlelineexample.png)
![Einzeiliges Listenelement](images/listitems/singlelineicon.png)
```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="SingleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="44" Padding="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="16" Width="16" VerticalAlignment="Center"/>
                <TextBlock Text="{x:Bind CompositionName}" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" Margin="12,0,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="double-line-list-item"></a>Zweizeiliges Listenelement 
Verwenden Sie diese Vorlage, um eine Liste von Elementen mit einem Bild und zweizeiligem Text anzuzeigen.

![Beispiel für zweizeiliges Listenelement mit Symbol](images/listitems/doublelineexample.png) 
![Zweizeiliges Listenelement mit Symbol](images/listitems/doublelineicon.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="triple-line-list-item"></a>Dreizeiliges Listenelement
Verwenden Sie diese Vorlage, um eine Liste von Elementen mit dreizeiligem Text anzuzeigen.

![Beispiel für dreizeiliges Listenelement](images/listitems/triplelineexample.png)
![Dreizeiliges Listenelement](images/listitems/tripleline.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TripleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Height="84" Padding="20" AutomationProperties.Name="{x:Bind CompositionName}">
                <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".8" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ReleaseDateTime}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".6" Margin="0,4,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="table-list-item"></a>Tabellenlistenelement
Verwenden Sie diese Vorlage, um eine Liste von Elementen mit Text in definierten Spalten anzuzeigen.

![Beispiel für Tabellenlistenelement](images/listitems/tablelist.png)
```xaml
<ListView  ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.HeaderTemplate>
        <DataTemplate>
            <Grid Padding="12" Background="{ThemeResource SystemBaseLowColor}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="408"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Composition" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="1" Text="Artist" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="2" Text="Release Date" Style="{ThemeResource CaptionTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.HeaderTemplate>
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TableDataTemplate" x:DataType="local:Recording">
            <Grid Height="48" AutomationProperties.Name="{x:Bind CompositionName}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="48"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <Ellipse Height="32" Width="32" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <TextBlock Grid.Column="1" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Text="{x:Bind CompositionName}" />
                <TextBlock Grid.Column="2" VerticalAlignment="Center" Text="{x:Bind ArtistName}"/>
                <TextBlock Grid.Column="3" VerticalAlignment="Center" Text="{x:Bind ReleaseDateTime}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="related-articles"></a>Verwandte Artikel
- [ListView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [Übersicht über Datenbindung](../../data-binding/data-binding-quickstart.md)
- [Übersicht über die Barrierefreiheit](../accessibility/accessibility-overview.md)
- [Beispiel für ListView und GridView (Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Miniaturbilder](../../files/thumbnails.md)
