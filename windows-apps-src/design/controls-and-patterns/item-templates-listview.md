---
Description: Elementvorlagen für Listenansicht
title: Elementvorlagen für Listenansicht
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, UWP, fluent
ms.openlocfilehash: 397c1d3a1502eaa352bf66b1bbf24e3fa39beff2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593055"
---
# <a name="item-templates-for-list-view"></a>Elementvorlagen für Listenansicht

Die Elementvorlagen in diesem Abschnitt können Sie mit einem [**ListView**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)-Steuerelement verwenden. Mithilfe dieser Vorlagen erhalten Sie das Erscheinungsbild gängiger App-Typen. 

Um die Datenbindung zu demonstrieren, binden Sie diese Vorlagen **ListViewItems** der Beispiel-Aufzeichnung-Klasse aus der [Übersicht über die Datenbindung](../../data-binding/data-binding-quickstart.md).

> [!NOTE] 
Wenn ein **DataTemplate** zurzeit mehrere Steuerelemente enthält (z. B. mehr als ein einzelner **TextBlock**), kommt der Name zur Verwendung durch Screenreader-Software von ToString(). Zur Vereinfachung können Sie stattdessen [**AutomationProperties.Name**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.automationproperties) auf dem Root-Element des **DataTemplate** setzen. Weitere Informationen zur Barrierefreiheit finden Sie unter [Zugänglichkeit im Überblick](../accessibility/accessibility-overview.md).

## <a name="single-line-list-item"></a>Einzeiliges Listenelement
Verwenden Sie diese Vorlage, um eine Elementliste mit einem Bild und einem einzeiligen Text anzuzeigen.

![Beispiel für eine einzelne Zeile Liste Element](images/listitems/singlelineexample.png)
![einzeiligen Listenelement](images/listitems/singlelineicon.png)
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
Verwenden Sie diese Vorlage, um eine Elementliste mit einem Bild und einem zweizeiligen Text anzuzeigen.

![Doppelte Linie Listenelement mit Symbol für Beispiel](images/listitems/doublelineexample.png) 
![doppelte Linie Listenelement mit Symbol](images/listitems/doublelineicon.png)

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
Verwenden Sie diese Vorlage, um eine Elementliste mit einem Bild und einem dreizeiligen Text anzuzeigen.

![Beispiel für Dreifache Linie Liste Element](images/listitems/triplelineexample.png)
![Dreifache Linie Listenelement](images/listitems/tripleline.png)

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
Verwenden Sie diese Vorlage, um eine Elementliste mit Text in Spalten anzuzeigen.

![Beispiel zu Tabellenlistenelement](images/listitems/tablelist.png)
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
- [ListView-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.listview)
- [Übersicht über die Datenbindung](../../data-binding/data-binding-quickstart.md)
- [Übersicht über die Accessibililty](../accessibility/accessibility-overview.md)
- [ListView und GridView-Beispiel (Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Miniaturbilder](../../files/thumbnails.md)