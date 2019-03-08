---
description: Hier erfahren Sie, wie Sie Akzentfarben und Designs in Ihren UWP-Apps verwenden.
title: Farbe in UWP-Apps
ms.date: 04/7/2018
ms.topic: article
keywords: windows 10, UWP
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 49d891888e26b6ce4c9f94e92605eaf7d619b6f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654255"
---
# <a name="color"></a>Farbe

![Favoritenbild](images/header-color.svg)

Farbe bietet eine intuitive Möglichkeit, Informationen an Benutzer in Ihrer App zu übermitteln – sie kann Interaktivität anzuzeigen, Feedback auf Benutzeraktionen geben und Ihrer Benutzeroberfläche ein Gefühl von visueller Kontinuität vermitteln. 

In UWP-Apps werden die Farben in erster Linie durch Akzentfarbe und Design bestimmt. In diesem Artikel erläutern wir, wie Sie die Farbe in Ihrer App verwenden können, und wie Sie Akzentfarben und Designressourcen verwenden, um Ihre UWP-App in jedem beliebigen Design-Kontext verwendet zu können. 

## <a name="color-principles"></a>Farbprinzipien

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>Designs

UWP-Apps können ein helles oder dunkles Anwendungsdesign verwenden. Das Design wirkt sich auf die Farben des App-Hintergrunds, Text, Symbole und [Standardsteuerelemente](../controls-and-patterns/index.md) aus.

### <a name="light-theme"></a>Helles Design

![Helles Design](images/color/light-theme.svg)

### <a name="dark-theme"></a>Dunkles Design

![Dunkles Design](images/color/dark-theme.svg)

Das Design der UWP-App folgt standardmäßig den Design-Einstellungen des Benutzers aus den Windows-Einstellungen oder dem Standarddesign des Geräts (d. h. dunkel auf XBox). Allerdings können Sie das Design für Ihre UWP-App festlegen. 

### <a name="changing-the-theme"></a>Ändern des Designs

Sie können Designs einfach ändern, indem Sie die **RequestedTheme**-Eigenschaft in `App.xaml`-Datei ändern:

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

Das Entfernen der **RequestedTheme**-Eigenschaft bedeutet, dass die Anwendung die Systemeinstellungen des Benutzers verwendet.

Anwender können auch Designs mit hohem Kontrast verwenden, die eine kleine Palette von Farbkombinationen mit hohem Farbkontrast nutzen, durch die die Benutzeroberfläche leichter zu erkennen ist. In diesem Fall überschreibt das System Ihre RequestedTheme.

### <a name="testing-themes"></a>Testen von Designs

Wenn Sie kein Design für Ihre App anfordern, sollten Sie unbedingt Ihre App in hellem und dunklem Design testen, um sicherzustellen, dass Ihre App unter allen Umständen lesbar ist.

**Hinweis**: In Visual Studio ist die Standardeinstellung RequestedTheme Light, daher Sie ändern die RequestedTheme um beide zu testen müssen.

## <a name="theme-brushes"></a>Designpinsel

Allgemeine Steuerelemente verwenden automatisch [Designpinsel](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes), um den Kontrast für helle und dunkle Designs anzupassen.

Hier ist eine Abbildung, wie [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) die Designpinsel verwendet:

![Beispiel für Steuerelement-Designpinsel](images/color/theme-brushes.svg)

Die Designpinsel werden für folgende Zwecke verwendet:

- **Base** gilt für Text.
- **ALT** ist das Gegenteil von Base.
- **Chrome** richtet sich an die Elemente der obersten Ebene, z. B. Navigationsbereich oder Befehlsleisten.
- **List** gilt für Steuerelemente.

**Niedrig**/**Mittel**/**Hoch** beziehen sich auf die Intensität der Farben.

### <a name="using-theme-brushes"></a>Verwenden der Designpinsel

:::row:::
    :::column:::
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

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

Weitere Informationen zur Verwendung von Designpinseln in Ihrer App finden Sie unter [Designressourcen](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Akzentfarbe

Allgemeine Steuerelemente verwenden eine Akzentfarbe, um die Zustandsinformationen zu vermitteln. Standardmäßig ist die Akzentfarbe die `SystemAccentColor`, die der Benutzer in den Einstellungen auswählt. Sie können jedoch auch Akzentfarben entsprechend der Marke Ihrer App anpassen.

![Windows-Steuerelemente](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>Überschreiben der Akzentfarbe

Wenn Sie Ihre App-Akzentfarbe ändern möchten, platzieren Sie den folgenden Code in `app.xaml`.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>Auswählen einer Akzentfarbe

Wenn Sie eine benutzerdefinierte Akzentfarbe für Ihre App auswählen, stellen Sie sicher, dass Text und Hintergrund, die die Akzentfarbe verwenden, ausreichenden Kontrast für eine optimale Lesbarkeit haben. Um den Kontrast zu testen, können Sie das Farbauswahltool in den Windows-Einstellungen verwenden, oder Sie können diese [online Kontrast-Tools nutzen](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

![Benutzerdefinierter Akzentfarbwähler in den Windows-Einstellungen](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>Akzentfarbpalette

Ein Akzentfarbalgorithmus in der Windows-Shell generiert helle und dunkle Schattierungen der Akzentfarbe.

![Akzentfarbpalette](images/color/accent-color-palette.svg)

Diese Schattierungen sind als [Designressourcen](../controls-and-patterns/xaml-theme-resources.md) verfügbar:

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true --> Sie können auch zugreifen, die Unterscheidung Farbpalette programmgesteuert mit der [ **UISettings.GetColorValue** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) Methode und [ **UIColorType** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) die Enumeration.

Sie können die Akzentfarbpalette für Designfarben in Ihrer App verwenden. Hier ist ein Beispiel für die Verwendung der Akzentfarbpalette auf einer Schaltfläche.

![Akzentfarbpalette auf einer Schaltfläche](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

Wenn Sie farbigen Text auf farbigem Hintergrund verwenden, stellen Sie sicher, dass genügend Kontrast zwischen Text und Hintergrund vorhanden ist. Standardmäßig werden Hyperlinks oder Hypertext in der Akzentfarbe des Benutzers dargestellt. Wenden Sie Varianten der Akzentfarbe im Hintergrund an, sollten Sie eine Variante der ursprüngliche Akzentfarbe verwenden, um den Kontrast von farbigem Text auf farbigem Hintergrund zu optimieren.

Das folgende Diagramm zeigt ein Beispiel für die unterschiedlichen Hell/Dunkel-Töne der Akzentfarbe, und gibt an, wie der Farbtyp auf einer farbige Oberfläche angewendet werden kann.

![Farbe auf Farbe](images/color/color-on-color.png)

Weitere Informationen zum Verwenden der Stil-Steuerelemente finden Sie unter [XAML-Style](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>Farb-API

Es gibt verschiedene APIs, um Farbe auf Ihrer Anwendung hinzuzufügen. Zuerst kommt die [**Farb**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors)-Klasse, die eine umfangreiche Liste vordefinierter Farben implementiert. Auf diese kann automatisch mithilfe der XAML-Eigenschaften zugegriffen werden. Im folgenden Beispiel erstellen wir eine Schaltfläche und legen Sie Farbeigenschaften im Hintergrund und Vordergrund auf Mitglieder der **Farb**-Klasse fest.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

Sie können Ihre eigenen Farben aus RGB- und hex-Werten mithilfe der [**Farb**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color)-Struktur in XAML erstellen.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

Sie können auch die gleiche Farbe im Code mit der **FromArgb**-Methode erstellen.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

Die Buchstaben "Argb" bedeutet Alpha (Deckkraft), Rot, Grün und Blau, die vier Komponenten einer Farbe. Jedes Argument reicht von 0 bis 255. Sie können den ersten Wert auslassen, was eine standardmäßige Deckkraft von 255 oder 100 % undurchsichtig ergibt.

> [!Note]
> Wenn Sie C++ verwenden, müssen Sie Farben mit der [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper)-Klasse erstellen.

Am häufigsten wird für eine **Farbe** ein Argument für [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush) verwendet, das zum Zeichnen von UI-Elementen im Volltonfarbe verwendet werden kann. Diese Pinsel sind in der Regel durch das [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary) definiert, sodass sie für mehrere Elemente wiederverwendet werden können.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Weitere Informationen über die Verwendung der Pinsel finden Sie unter [XAML-Pinsel](brushes.md).

## <a name="scoping-system-colors"></a>Festlegen des Gültigkeitsbereichs von Systemfarben

Zusätzlich zum Definieren eigene Farben in Ihrer app, können Sie auch unsere systematized Farben, die gewünschten Regionen in der gesamten app beschränken, mit der **ColorSchemeResources** Tag. Mit dieser API können Sie nicht nur farblich zu markieren und Design eine große Gruppe von Steuerelementen durch das Festlegen von ein paar Eigenschaften, sondern auch Vorteile vielen anderen Systemen, die Sie erhalten würde nicht mit Ihrer eigenen benutzerdefinierten Farben manuell definieren normalerweise erhalten:

- Jede Farbe mit **ColorSchemeResources** wird keine Auswirkungen auf hoher Kontrast
  * D. h. Ihre app kann an mehrere Personen ohne zusätzliche Entwürfe oder Dev Kosten zugegriffen werden
- Können einfach Farben hell, dunkel oder für alle Bereiche geltende für beide Designs festgelegt durch Festlegen einer Eigenschaft für die API
- Set auf Farben **ColorSchemeResources** Kaskadieren auf alle Steuerelemente mit ähnlichen, die ebenfalls diese Systemfarbe verwenden
  * Dadurch wird sichergestellt, dass Sie eine konsistente farbdarstellung Geschichte über Ihre app hat und gleichzeitig das Aussehen Ihrer Marke
- Alle visuellen Zustände, Animationen und Deckkraft Variationen Auswirkungen Re-Vorlage ohne

### <a name="how-to-use-colorschemeresources"></a>Gewusst wie: Verwenden von ColorSchemeResources

ColorSchemeResources ist eine API, die mitteilt, dass das System, auf welche Ressourcen zu Where beschränkt. ColorSchemeResources nehmen muss ein [X: Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), die kann eine der drei Optionen zur Auswahl:
- Standard
  * Zeigt die Farbe Änderungen in beiden [Licht](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) und [dunkel](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme) Design
- Hell
  * Zeigt eine Farbe ändert sich nur in [Design "hell"](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 
- Dunkel
  * Zeigt eine Farbe ändert sich nur in [Design "dunkel"](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

Festlegen, X: Key wird sichergestellt, dass Sie Farben entsprechend zu, das System oder app-Design ändern, soll eine andere benutzerdefinierte Darstellung im entweder Design.

### <a name="how-to-apply-scoped-colors"></a>Wie Sie diesen Bereich Farben anwenden

Festlegen des Gültigkeitsbereichs von Ressourcen über die **ColorSchemeResources** -API in XAML können Sie Systemfarbe oder eines Pinsels an, die in unserer [Designressourcen](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) Bibliothek und anschließend erneut definieren, innerhalb des Bereichs einer Seite oder Container.

Wenn Sie zwei Systemfarben - definiert z. B. **SystemBaseLowColor** und **SystemBaseMediumLowColor** in einem Raster, und klicken Sie dann zwei Schaltflächen auf der Seite eingefügt: eine in diesem Raster und eine außerhalb:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default" 
        SystemBaseLowColor="LightGreen" 
        SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Sie erhalten **Button_A** mit den angewendeten neuen Farben, und **Button_B** suchen, wie unser System als Standardschaltfläche bleibt:

![Bereichsbezogene Systemfarben auf Schaltfläche "](images/color/scopedcolors_cyan_button.png)

Aber da unsere Systemfarben auf andere Steuerelemente zu kaskadieren festlegen **SystemBaseLowColor** und **SystemBaseMediumLowColor** wirkt sich auf mehr als nur die Schaltflächen. In diesem Fall Steuerelemente wie **ToggleButton**, **RadioButton** und **Schieberegler** wird zudem beeinträchtigt durch diese Farbe ändert, sollten die Steuerelemente im oben Exampl gesetzt werden die Raster-Bereich.
Wenn Sie eine Änderung Farbe beschränken möchten *auf einen einzelnen steuert nur* definieren Sie dazu **ColorSchemeResources** innerhalb des Steuerelements Ressourcen:

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorSchemeResources x:Key="Default" 
                SystemBaseLowColor="LightGreen" 
                SystemBaseMediumLowColor="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Sie haben im Grunde genau dasselbe wie vor, aber jetzt andere Steuerelemente, die dem Raster hinzugefügte keine neue ändert sich die Farbe. Dies ist, da diese Systemfarben zugeordnet sind **Button_A** nur.

### <a name="nesting-scoped-resources"></a>Bereich verschachteln, Ressourcen

Schachteln von Systemfarben ist auch möglich und erfolgt dies durch das Platzieren von **ColorSchemeResources** in der geschachtelten Elemente Ressourcen innerhalb des Markups Ihrer app-Layout:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorSchemeResources x:Key="Default"
                SystemBaseLowColor="Goldenrod"
                SystemBaseMediumLowColor="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

In diesem Beispiel **Button_A** ist erbende Farben definieren in **Grid_A**Ressourcen und **geschachtelten Schaltfläche** erbt Farben aus **Grid_B**Ressourcen. Durch Erweiterung, dies bedeutet, dass es sich bei allen anderen Steuerelementen innerhalb platziert **Grid_B** überprüft oder anwenden **Grid_B**Ressourcen zunächst vor dem Überprüfen oder Anwenden von **Grid_A**des Ressourcen und unserer Standardfarben schließlich übernommen wird, wenn nichts auf der Seite oder app-Ebene definiert ist.

Dies funktioniert für eine beliebige Anzahl von geschachtelten Elementen, deren Ressourcen Farbdefinitionen haben.

### <a name="scoping-with-a-resourcedictionary"></a>Festlegen des Gültigkeitsbereichs, mit einem ResourceDictionary

Sie sind nicht auf einen Container oder auf der Seite Ressourcen beschränkt, und Sie können auch diese Systemfarben in einem ResourceDictionary, die dann zusammengeführt werden kann in jedem Bereich die Möglichkeit, die Sie normalerweise ein Wörterbuch Zusammenführen würde definieren.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

Erstellen Sie zunächst ein ResourceDictionary. Legen Sie die **ColorPaletteResources** innerhalb der ThemeDictionaries und überschreiben Sie die gewünschten Systemfarben:

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>
            
                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF" 
                    AltHigh="#FF000000" 
                    AltLow="#FF000000"/>
                    
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

Auf der Seite, die das Layout enthält müssen Sie einfach Zusammenführen Sie dieses Wörterbuch in den gewünschten Bereich:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>
             
    <Button Content="Button_A"/>
</Grid>
```

Jetzt alle Ressourcen, Designs und benutzerdefinierte Farben können platziert werden in einem einzelnen **MyCustomTheme** Ressourcenverzeichnis und bei Bedarf ohne zusätzliche Überfrachtung im Markup Layout kümmern.

### <a name="other-ways-to-define-color-resources"></a>Weitere Informationen zum Definieren der Farbressourcen

ColorSchemeResources ermöglicht es auch für Systemfarben platziert werden sollen sowie die Definition direkt darin, als Wrapper und nicht in Zeile:

``` xaml
<ColorSchemeResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorSchemeResources>
```

## <a name="usability"></a>Nutzbarkeit

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Verwandte Artikel

- [XAML-Stile](../controls-and-patterns/xaml-styles.md)
- [XAML-Designressourcen](../controls-and-patterns/xaml-theme-resources.md)
