---
description: Hier erfahren Sie, wie Sie Akzentfarben und Designs in Ihren UWP-Apps verwenden.
title: Farbe in UWP-Apps
ms.date: 04/07/2019
ms.topic: article
keywords: Windows 10, UWP
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e5cd8ecafd3557719e70c50890da4c3eade18f52
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714136"
---
# <a name="color"></a>Farbe

![Herobild](images/header-color.svg)

Farbe bietet eine intuitive Möglichkeit zur Übermittlung von Informationen an Benutzer in Ihrer App: Mit ihr kann Interaktivität angezeigt, Feedback auf Benutzeraktionen gegeben und Ihrer Benutzeroberfläche ein Gefühl von visueller Kontinuität verliehen werden.

In UWP-Apps werden Farben in erster Linie durch Akzentfarbe und Design bestimmt. In diesem Artikel erläutern wir, wie Sie Farbe in Ihrer App einsetzen können und wie Sie mit Akzentfarben und Designressourcen dafür sorgen, dass Ihre UWP-App in jedem beliebigen Designkontext verwendet werden kann.

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

UWP-Apps können ein helles oder dunkles Anwendungsdesign verwenden. Das Design wirkt sich auf die Farben des App-Hintergrunds, Text, Symbole und [allgemeine Steuerelemente](../controls-and-patterns/index.md) aus.

### <a name="light-theme"></a>Helles Design

![helles Design](images/color/light-theme.svg)

### <a name="dark-theme"></a>Dunkles Design

![dunkles Design](images/color/dark-theme.svg)

Das Design Ihrer UWP-App folgt standardmäßig den Designeinstellungen des Benutzers aus den Windows-Einstellungen oder dem Standarddesign des Geräts (d.h. dunkel auf XBox). Sie können jedoch das Design für Ihre UWP-App festlegen.

### <a name="changing-the-theme"></a>Ändern des Designs

Sie können Designs ändern, indem Sie die **RequestedTheme**-Eigenschaft in Ihrer Datei `App.xaml` ändern.

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

Das Entfernen der **RequestedTheme**-Eigenschaft bedeutet, dass Ihre App die Systemeinstellungen des Benutzers verwendet.

Benutzer können auch das Design mit hohem Kontrast verwenden, das eine kleine Palette von Kontrastfarben nutzt, durch die die Benutzeroberfläche leichter zu erkennen ist. In diesem Fall setzt das System Ihren Wert für „RequestedTheme“ außer Kraft.

### <a name="testing-themes"></a>Testen von Designs

Wenn Sie kein Design für Ihre App anfordern, sollten Sie die App unbedingt in hellem und dunklem Design testen, um sicherzustellen, dass sie unter allen Bedingungen lesbar ist.

**Hinweis**: In Visual Studio ist „RequestedTheme“ standardmäßig „Light“ (Hell). Daher müssen Sie den Wert für „RequestedTheme“ ändern, um beide Designs zu testen.

## <a name="theme-brushes"></a>Designpinsel

Allgemeine Steuerelemente verwenden automatisch [Designpinsel](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes), um den Kontrast für helle und dunkle Designs anzupassen.

Hier zeigt eine Abbildung, wie das Steuerelement [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) Designpinsel verwendet:

![Beispiel für ein Steuerelement mit Designpinseln](images/color/theme-brushes.svg)

Die Designpinsel werden für folgende Zwecke verwendet:

- **Base** gilt für Text.
- **Alt** ist das Gegenteil von Base.
- **Chrome** gilt für Elemente der obersten Ebene, z.B. Navigationsbereiche oder Befehlsleisten.
- **List** gilt für Steuerelemente.

**Low**/**Medium**/**High** (Niedrig/Mittel/Hoch) bezieht sich auf die Intensität der Farbe.

### <a name="using-theme-brushes"></a>Verwenden von Designpinseln

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

Allgemeine Steuerelemente verwenden eine Akzentfarbe, um Zustandsinformationen zu vermitteln. Standardmäßig ist die Akzentfarbe der Wert für `SystemAccentColor`, den der Benutzer in den Einstellungen auswählt. Sie können die Akzentfarbe für Ihre App jedoch auch entsprechend Ihrer Marke anpassen.

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

Wenn Sie die Akzentfarbe Ihrer App ändern möchten, fügen Sie den folgenden Code in `app.xaml` ein.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>Auswählen einer Akzentfarbe

Wenn Sie eine benutzerdefinierte Akzentfarbe für Ihre App auswählen, stellen Sie sicher, dass Text und Hintergrund, die die Akzentfarbe verwenden, ausreichenden Kontrast für eine optimale Lesbarkeit aufweisen. Um den Kontrast zu testen, können Sie das Farbauswahltool in den Windows-Einstellungen verwenden, oder Sie können diese [Online-Kontrast-Tools nutzen](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

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

<!-- check this is true -->
Sie können die Akzentfarbpalette auch programmgesteuert mit der [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_)-Methode und der [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType)-Enumeration aufrufen.

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

Wenn Sie farbigen Text auf einem farbigem Hintergrund verwenden, stellen Sie sicher, dass genügend Kontrast zwischen Text und Hintergrund vorhanden ist. Standardmäßig werden Hyperlinks oder Hypertext in der Akzentfarbe des Benutzers dargestellt. Wenn Sie Varianten der Akzentfarbe auf den Hintergrund anwenden, sollten Sie eine Variante der ursprünglichen Akzentfarbe verwenden, um den Kontrast von farbigem Text auf einem farbigen Hintergrund zu optimieren.

Das folgende Diagramm zeigt ein Beispiel für die verschiedenen Hell/Dunkel-Töne der Akzentfarbe und gibt an, wie der Farbtyp auf einer farbigen Oberfläche angewendet werden kann.

![Farbe auf Farbe](images/color/color-on-color.png)

Weitere Informationen zum Formatieren von Steuerelementen finden Sie unter [XAML-Stile](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>Color-API

Es gibt verschiedene APIs, mit denen Sie Ihrer App Farbe hinzufügen können. Dies ist zunächst die [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors)-Klasse, die eine umfangreiche Liste vordefinierter Farben implementiert. Darauf kann mithilfe der XAML-Eigenschaften automatisch zugegriffen werden. Im folgenden Beispiel erstellen wir eine Schaltfläche und legen die Farbeigenschaften für Hintergrund und Vordergrund auf Mitglieder der **Colors**-Klasse fest.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

Sie können Ihre eigenen Farben aus RGB- und Hex-Werten mithilfe der [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color)-Struktur in XAML erstellen.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

Sie können auch dieselbe Farbe im Code mit der **FromArgb**-Methode erstellen.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

Die Buchstaben "Argb" stehen für „Alpha“ (Deckkraft), „Rot“, „Grün“ und „Blau“ – die vier Komponenten einer Farbe. Jedes Argument kann zwischen 0 und 255 liegen. Sie können den ersten Wert wahlweise weglassen und erhalten so eine standardmäßige Deckkraft von 255 oder 100 % undurchsichtig.

> [!Note]
> Wenn Sie C++ verwenden, müssen Sie Farben mit der [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper)-Klasse erstellen.

Am häufigsten wird **Color** als ein Argument für den Pinsel [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush) verwendet, der zum Zeichnen von UI-Elementen in einer Volltonfarbe verwendet werden kann. Diese Pinsel werden in der Regel in einem [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary) definiert, sodass sie für mehrere Elemente wiederverwendet werden können.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Weitere Informationen zur Verwendung von Pinseln finden Sie unter [XAML-Pinsel](brushes.md).

## <a name="scoping-system-colors"></a>Festlegen des Gültigkeitsbereichs von Systemfarben

Zusätzlich zum Definieren Ihrer eigenen Farben in Ihrer App können Sie auch mithilfe des Tags **ColorPaletteResources** den Gültigkeitsbereich für unsere Systemfarben in der gesamten App auf gewünschte Regionen festlegen. Über diese API können Sie nicht nur großen Gruppen von Steuerelementen gleichzeitig Farben und Designs zuweisen, indem Sie einige wenige Eigenschaften festlegen, sondern Sie können auch viele andere Systemvorteile nutzen, die Sie beim manuellen Definieren Ihrer eigenen benutzerdefinierten Farben normalerweise nicht erhalten würden:

- Eine mit **ColorPaletteResources** festgelegte Farbe hat keine Auswirkung auf „Hoher Kontrast“.
  * Dies bedeutet, dass mehr Personen ohne zusätzliche Design- oder Dev-Kosten auf Ihre App zugreifen können.
- Sie können Farben in beiden Designs ganz einfach auf „Hell“, „Dunkel“ oder „Pervasive“ festlegen, indem Sie in der API eine einzige Eigenschaft festlegen.
- Farben, die unter **ColorPaletteResources** festgelegt wurden, werden auf alle ähnlichen Steuerelemente, die diese Systemfarben ebenfalls verwenden, nach unten kaskadiert.
  * Dadurch wird sichergestellt, dass Ihre gesamte App eine einheitliche Farbdarstellung aufweist und gleichzeitig das Erscheinungsbild Ihrer Marke beibehalten wird.
- Wirkt sich auf alle visuellen Zustände, Animationen und Deckkraftvarianten aus, ohne dass eine neue Vorlage erforderlich ist.

### <a name="how-to-use-colorpaletteresources"></a>Verwenden von „ColorPaletteResources“

„ColorPaletteResources“ ist eine API, die das System informiert, für welche Ressourcen wo ein Gültigkeitsbereich festgelegt wird. Für „ColorPaletteResources“ muss ein [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) angegeben werden, der einer der drei folgenden Werte sein kann:
- Standard
  * Zeigt Ihre Farbänderungen im Design [Hell](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) und [Dunkel](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme).
- Hell
  * Zeigt Ihre Farbänderungen nur im [Design „Hell“](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme).
- Dunkel
  * Zeigt Ihre Farbänderungen nur im [Design „Dunkel“](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme).

Durch Festlegen eines Werts für „x:Key“ wird sichergestellt, dass Ihre Farben sich entsprechend dem System oder dem App-Design ändern, wenn eines der Designs ein anderes benutzerdefiniertes Erscheinungsbild haben soll.

### <a name="how-to-apply-scoped-colors"></a>Anwenden von bereichsbezogenen Farben

Durch das Festlegen von Gültigkeitsbereichen über die **ColorPaletteResources**-API in XAML können Sie eine beliebige Systemfarbe oder einen beliebigen Pinsel aus unserer Bibliothek [Designressourcen](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) verwenden und sie bzw. ihn innerhalb des Bereichs einer Seite oder eines Containers neu definieren.

Wenn Sie beispielsweise zwei Systemfarben – **BaseLow** und **BaseMediumLow** – in einem Raster definiert und dann zwei Schaltflächen auf Ihrer Seite platziert haben, eine in diesem Raster und eine außerhalb:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Würden Sie **Button_A** mit den angewendeten neuen Farben erhalten, und **Button_B** würde weiterhin wie die Standardschaltfläche unseres Systems aussehen:

![Bereichsbezogene Systemfarben auf Schaltfläche](images/color/scopedcolors_cyan_button.png)

Da aber alle unsere Systemfarben auch auf andere Steuerelemente nach unten kaskadiert werden, wird sich das Festlegen von Werten für **BaseLow** und **BaseMediumLow** auf mehr als nur Schaltflächen auswirken. In diesem Fall wirken sich diese Änderungen der Systemfarbe auch auf Steuerelemente wie **ToggleButton**, **RadioButton** und **Slider** aus, wenn diese Steuerelemente in den Rasterbereich des vorstehenden Beispiels eingefügt werden.
Wenn Sie einen Bereich für eine Systemfarbe festlegen möchten, wechseln Sie *zu einem einzigen Steuerelement*, indem Sie **ColorPaletteResources** innerhalb der Ressourcen dieses Steuerelements definieren:

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Das Ergebnis entspricht im Wesentlichen exakt dem vorstehend gezeigten Beispiel, doch jetzt übernehmen alle anderen Steuerelemente, die dem Raster hinzugefügt werden, die Farbänderungen nicht. Der Grund: Diese Systemfarben sind nur auf den Bereich **Button_A** bezogen.

### <a name="nesting-scoped-resources"></a>Schachteln von bereichsbezogenen Ressourcen

Es ist auch möglich, Systemfarben zu schachteln. Dazu fügen Sie **ColorPaletteResources** im Markup Ihres App-Layouts in den Ressourcen der geschachtelten Elemente ein:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

In diesem Beispiel erbt **Button_A** Farben, die in den Ressourcen von **Grid_A** definiert wurden, und **Nested Button** erbt Farben aus den Ressourcen von **Grid_B**. Weiter gedacht bedeutet dies, dass alle anderen in **Grid_B** platzieren Steuerelemente zuerst die Ressourcen von **Grid_B** überprüfen oder anwenden, bevor sie die Ressourcen von **Grid_A** überprüfen oder anwenden, und dass sie zuletzt unsere Standardfarben anwenden, wenn auf der Seiten- oder App-Ebene nichts definiert wurde.

Dies funktioniert bei einer beliebigen Anzahl von geschachtelten Elementen, für deren Ressourcen es Farbdefinitionen gibt.

### <a name="scoping-with-a-resourcedictionary"></a>Festlegen des Gültigkeitsbereichs mit einem ResourceDictionary

Ihre Möglichkeiten sind nicht auf die Ressourcen für einen Container oder eine Seite beschränkt, und Sie können diese Systemfarben auch in einem ResourceDictionary definieren und es anschließend in jedem beliebigen Bereich genauso zusammenführen, wie Sie ein Wörterbuch normalerweise zusammenführen würden.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

Zuerst erstellen Sie ein ResourceDictionary. Dann fügen Sie die **ColorPaletteResources** in den „ThemeDictionaries“ ein und setzen die gewünschten Systemfarben außer Kraft:

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

Auf der Seite mit Ihrem Layout führen Sie dieses Wörterbuch einfach in dem gewünschten Bereich zusammen:

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

Jetzt können alle Ressourcen, Designs und benutzerdefinierten Farben in einem einzigen **MyCustomTheme**-Ressourcenverzeichnis platziert und ihnen ein Gültigkeitsbereich nach Bedarf hinzugefügt werden, ohne dass Sie sich Gedanken um zusätzlichen Clutter im Markup Ihres Layouts machen müssen.

### <a name="other-ways-to-define-color-resources"></a>Andere Möglichkeiten zum Definieren von Farbressourcen

„ColorPaletteResources“ ermöglicht es auch, dass Systemfarben darin platziert und dort direkt als Wrapper (statt inline) definiert werden:

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>Nutzbarkeit

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessibility should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the usability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the usability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Verwandte Artikel

- [XAML-Stile](../controls-and-patterns/xaml-styles.md)
- [XAML-Designressourcen](../controls-and-patterns/xaml-theme-resources.md)
