---
description: Bei Designressourcen in XAML handelt es sich um einen Satz von Ressourcen, die abhängig vom aktiven Systemdesign verschiedene Werte anwenden.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_theme\_resources
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML-Designressourcen
ms.assetid: 41B87DBF-E7A2-44E9-BEBA-AF6EEBABB81B
label: XAML theme resources
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6da668c005f95d51f6fb0588f60fdcf63aea4fd7
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034423"
---
# <a name="xaml-theme-resources"></a>XAML-Designressourcen

Bei Designressourcen in XAML handelt es sich um einen Satz von Ressourcen, die abhängig vom aktiven Systemdesign verschiedene Werte anwenden. Es gibt drei Designs, die vom XAML-Framework unterstützt werden: „Hell“, „Dunkel“ und „Hoher Kontrast“.

**Voraussetzungen** : In diesem Thema wird vorausgesetzt, dass Sie [ResourceDictionary- und XAML-Ressourcenreferenzen](resourcedictionary-and-xaml-resource-references.md) gelesen haben.

## <a name="theme-resources-v-static-resources"></a>Designressourcen im Vergleich zu statischen Ressourcen

Es gibt zwei XAML-Markuperweiterungen, die aus einem vorhandenen XAML-Ressourcenverzeichnis auf eine XAML-Ressource verweisen können: die [{StaticResource}-Markuperweiterung](../../xaml-platform/staticresource-markup-extension.md) und die [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md).

Die Auswertung einer [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) tritt beim Laden der App und anschließend bei jeder Änderung des Designs zur Laufzeit auf. Dies ist in der Regel das Ergebnis des Änderns der Geräteeinstellungen durch den Benutzer oder einer programmgesteuerten Änderung in der App, die das aktuelle Design ändert.

Für eine [{StaticResource}-Markuperweiterung](../../xaml-platform/staticresource-markup-extension.md) erfolgt eine Auswertung hingegen nur, wenn die XAML zuerst von der App geladen wird. Es findet keine Aktualisierung statt. Dies ähnelt dem Suchen und Ersetzen in XAML mit dem tatsächlichen Laufzeitwert beim Start der App.

## <a name="theme-resources-in-the-resource-dictionary-structure"></a>Designressourcen in der Ressourcenverzeichnisstruktur

Jede Designressource ist Teil der XAML-Datei „themeresources.xaml“. Zu Designzwecken steht „themeresources.xaml“ im Order „\\(Programme)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic“ einer Installation des Windows Software Development Kit (SDK) zur Verfügung. Die Ressourcenverzeichnisse in „themeresources.xaml“ werden auch in „generic.xaml“ im selben Verzeichnis reproduziert.

Die Windows-Runtime verwendet diese physischen Dateien nicht für die Runtime-Suche. Daher befinden sie sich in einem speziellen DesignTime-Ordner und werden nicht standardmäßig in Apps kopiert. Stattdessen sind die Ressourcenverzeichnisse als Teil der Windows-Runtime selbst im Speicher vorhanden, und die XAML-Ressource Ihrer App verweist auf Designressourcen (oder Systemressourcen), die dort zu Laufzeit aufgelöst werden.

## <a name="guidelines-for-custom-theme-resources"></a>Richtlinien für benutzerdefinierte Designressourcen

Halten Sie sich an die folgenden Richtlinien, wenn Sie Ihre eigenen benutzerdefinierten Designressourcen definieren und verwenden:

- Geben Sie zusätzlich zum Verzeichnis „HighContrast“ jeweils ein Designverzeichnis für „Light“ und „Dark“ an. Sie können zwar ein [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)-Element mit „Default“ als Schlüssel erstellen, es empfiehlt sich jedoch, explizit vorzugehen und stattdessen „Light“, „Dark“ und „HighContrast“ zu verwenden.

- Verwenden der [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) in: Formatvorlagen, Settern, Steuerelementvorlagen, Eigenschaften-Settern und Animationen.

- Verwenden Sie die [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) nicht in Ihren Ressourcendefinitionen in [ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries). Verwenden Sie stattdessen die [{StaticResource}-Markuperweiterung](../../xaml-platform/staticresource-markup-extension.md).

    AUSNAHME: Sie können die [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) verwenden, um auf Ressourcen zu verweisen, die in Bezug auf das App-Design in [ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) agnostisch sind. Beispiele für diese Ressourcen sind Akzentfarbenressourcen wie `SystemAccentColor` oder Systemfarbenressourcen, die normalerweise das Präfix „SystemColor“ haben, z. B. `SystemColorButtonFaceColor`.

> [!CAUTION]
> Wenn Sie diesen Richtlinien nicht folgen, kann ein unerwartetes Verhalten im Zusammenhang mit Designs in Ihrer App auftreten. Weitere Informationen finden Sie im Abschnitt [Problembehandlung für Designressourcen](#troubleshooting-theme-resources).

## <a name="the-xaml-color-ramp-and-theme-dependent-brushes"></a>Die XAML-Farbskala und designabhängige Pinsel

Die kombinierte Gruppe von Farben für die Designs „Light“, „Dark“ und „HighContrast“ bilden die *Windows-Farbskala* in XAML. Egal, ob Sie die Systemdesigns ändern oder ein Systemdesign auf Ihre eigenen XAML-Elemente anwenden möchten, ist es wichtig zu verstehen, wie die Farbressourcen strukturiert werden.

Weitere Informationen zur Anwendung von Farbe in Windows-Apps finden Sie unter [Farbe in Windows-Apps](../style/color.md).

### <a name="light-and-dark-theme-colors"></a>Helle und dunkle Designfarben

Das XAML-Framework stellt eine Gruppe von benannten [Color](/uwp/api/Windows.UI.Color)-Ressourcen mit Werten bereit, die speziell auf die Designs „Light“ und „Dark“ zugeschnitten sind. Die Schlüssel, die Sie zum Verweisen verwenden, haben folgendes Namensformat: `System[Simple Light/Dark Name]Color`.

Diese Tabelle enthält den Schlüssel, den einfachen Namen und die Zeichenfolgendarstellung der Farbe (mit dem \#aarrggbb-Format) für die Ressourcen „Light“ und „Dark“, die vom XAML-Framework bereitgestellt werden. Der Schlüssel wird verwendet, um auf die Ressource in einer App zu verweisen. Der einfache Name für hell/dunkel dient als Teil der Pinselbenennungskonvention, die später erläutert wird.

| Schlüssel                             | Einfacher Name für hell/dunkel | Leicht      | Dunkel       |
|---------------------------------|------------------------|------------|------------|
| SystemAltHighColor              | AltHigh                | \#FFFFFFFF | \#FF000000 |
| SystemAltLowColor               | AltLow                 | \#33FFFFFF | \#33000000 |
| SystemAltMediumColor            | AltMedium              | \#99FFFFFF | \#99000000 |
| SystemAltMediumHighColor        | AltMediumHigh          | \#CCFFFFFF | \#CC000000 |
| SystemAltMediumLowColor         | AltMediumLow           | \#66FFFFFF | \#66000000 |
| SystemBaseHighColor             | BaseHigh               | \#FF000000 | \#FFFFFFFF |
| SystemBaseLowColor              | BaseLow                | \#33000000 | \#33FFFFFF |
| SystemBaseMediumColor           | BaseMedium             | \#99000000 | \#99FFFFFF |
| SystemBaseMediumHighColor       | BaseMediumHigh         | \#CC000000 | \#CCFFFFFF |
| SystemBaseMediumLowColor        | BaseMediumLow          | \#66000000 | \#66FFFFFF |
| SystemChromeAltLowColor         | ChromeAltLow           | \#FF171717 | \#FFF2F2F2 |
| SystemChromeBlackHighColor      | ChromeBlackHigh        | \#FF000000 | \#FF000000 |
| SystemChromeBlackLowColor       | ChromeBlackLow         | \#33000000 | \#33000000 |
| SystemChromeBlackMediumLowColor | ChromeBlackMediumLow   | \#66000000 | \#66000000 |
| SystemChromeBlackMediumColor    | ChromeBlackMedium      | \#CC000000 | \#CC000000 |
| SystemChromeDisabledHighColor   | ChromeDisabledHigh     | \#FFCCCCCC | \#FF333333 |
| SystemChromeDisabledLowColor    | ChromeDisabledLow      | \#FF7A7A7A | \#FF858585 |
| SystemChromeHighColor           | ChromeHigh             | \#FFCCCCCC | \#FF767676 |
| SystemChromeLowColor            | ChromeLow              | \#FFF2F2F2 | \#FF171717 |
| SystemChromeMediumColor         | ChromeMedium           | \#FFE6E6E6 | \#FF1F1F1F |
| SystemChromeMediumLowColor      | ChromeMediumLow        | \#FFF2F2F2 | \#FF2B2B2B |
| SystemChromeWhiteColor          | ChromeWhite            | \#FFFFFFFF | \#FFFFFFFF |
| SystemListLowColor              | ListLow                | \#19000000 | \#19FFFFFF |
| SystemListMediumColor           | ListMedium             | \#33000000 | \#33FFFFFF |

:::row:::
    :::column:::
        #### <a name="light-theme"></a>Helles Design
    :::column-end:::
    :::column:::
        #### <a name="dark-theme"></a>Dunkles Design
    :::column-end:::
:::row-end:::

#### <a name="base"></a>Basis

:::row:::
    :::column:::
        ![Das helle Basisdesign](images/themes/light-base.png)
    :::column-end:::
    :::column:::
        ![Das dunkle Basisdesign](images/themes/dark-base.png)
    :::column-end:::
:::row-end:::

#### <a name="alt"></a>ALT-Taste

:::row:::
    :::column:::
        ![Das helle Alternativdesign](images/themes/light-alt.png)
    :::column-end:::
    :::column:::
        ![Das dunkle Alternativdesign](images/themes/dark-alt.png)
    :::column-end:::
:::row-end:::

#### <a name="list"></a>List

:::row:::
    :::column:::
        ![Das helle Listendesign](images/themes/light-list.png)
    :::column-end:::
    :::column:::
        ![Das dunkle Listendesign](images/themes/dark-list.png)
    :::column-end:::
:::row-end:::

#### <a name="chrome"></a>Chrom

:::row:::
    :::column:::
        ![Das helle Chromdesign](images/themes/light-chrome.png)
    :::column-end:::
    :::column:::
        ![Das dunkle Chromdesign](images/themes/dark-chrome.png)
    :::column-end:::
:::row-end:::

### <a name="windows-system-high-contrast-colors"></a>Windows-Systemfarben mit hohem Kontrast

Zusätzlich zu den Ressourcen, die vom XAML-Framework bereitgestellt werden, gibt es eine Reihe von Farbwerten, die von der Windows-Systempalette abgeleitet werden. Diese Farben werden nicht speziell für die Windows-Runtime- oder Windows-Apps verwendet. Viele der XAML-[Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)-Ressourcen verwenden diese Farben jedoch, wenn das System mit dem Design „HighContrast“ betrieben wird (und die App ausgeführt wird). Das XAML-Framework stellt diese systemweiten Farben als Ressourcen mit Schlüssel bereit. Die Schlüssel folgen diesem Benennungsformat: `SystemColor[name]Color`.

Diese Tabelle enthält die systemweiten Farben, die XAML als Ressourcenobjekte aus der Windows-Systempalette abgeleitet bereitstellt. In der Spalte „Name der erleichterten Bedienung“ wird gezeigt, wie Farben in der Windows-Einstellungs-UI beschriftet werden. In der Spalte „Einfacher Name für HighContrast“ wird mit einem Wort beschrieben, wie die Farbe für die allgemeinen XAML-Steuerelemente angewendet wird. Sie wird als Teil der Pinselbenennungskonvention verwendet, die später erläutert wird. Die in der Spalte „Anfänglicher Standardwert“ aufgeführten Werte erhalten Sie, wenn das System nicht mit hohem Kontrast ausgeführt wird.

| Schlüssel                           | Name der erleichterten Bedienung            | Einfacher Name für HighContrast | Anfänglicher Standardwert |
|-------------------------------|--------------------------------|--------------------------|-----------------|
| SystemColorButtonFaceColor    | **Text der Schaltfläche** (Hintergrund)   | Hintergrund               | \#FFF0F0F0      |
| SystemColorButtonTextColor    | **Text der Schaltfläche** (Vordergrund)   | Vordergrund               | \#FF000000      |
| SystemColorGrayTextColor      | **Deaktivierter Text**              | Deaktiviert                 | \#FF6D6D6D      |
| SystemColorHighlightColor     | **Ausgewählter Text** (Hintergrund) | Markierung                | \#FF3399FF      |
| SystemColorHighlightTextColor | **Ausgewählter Text** (Vordergrund) | HighlightAlt             | \#FFFFFFFF      |
| SystemColorHotlightColor      | **Hyperlinks**                 | Hyperlink                | \#FF0066CC      |
| SystemColorWindowColor        | **Hintergrund**                 | PageBackground           | \#FFFFFFFF      |
| SystemColorWindowTextColor    | **Text**                       | PageText                 | \#FF000000      |

Windows bietet verschiedene kontrastreiche Designs und ermöglicht es dem Benutzer, für ihre Einstellungen für hohen Kontrast durch das Center für erleichterte Bedienung die spezifischen Farben festzulegen, wie hier gezeigt wird. Daher ist es nicht möglich, eine definitive Liste der Farbwerte mit hohem Kontrast bereitzustellen.

![Die Windows-Einstellungs-UI für hohen Kontrast](images/high-contrast-settings.png)

Weitere Informationen zum Unterstützen von Designs mit hohem Kontrast finden Sie unter [Designs mit hohem Kontrast](../accessibility/high-contrast-themes.md).

### <a name="system-accent-color"></a>Systemakzentfarbe

Neben der Systemdesignfarben mit hohem Kontrast wird die Akzentfarbe des Systems als spezielle Ressource mit dem Schlüssel `SystemAccentColor` bereitgestellt. Zur Laufzeit ruft diese Ressource die Farbe ab, die der Benutzer als Akzentfarbe in den Windows-Einstellungen zur Personalisierung angegeben hat.

> [!NOTE]
> Die Systemfarbenressourcen können zwar überschrieben werden, es empfiehlt sich jedoch, die Farbauswahl des Benutzers, insbesondere für Einstellungen für hohen Kontrast, zu respektieren.

### <a name="theme-dependent-brushes"></a>Designabhängige Pinsel

Die in den vorherigen Abschnitten gezeigten Farbressourcen dienen zum Festlegen der [Color](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)-Eigenschaft von [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)-Ressourcen in den Systemdesign-Ressourcenwörterbüchern. Sie verwenden die Pinselressourcen, um die Farbe auf XAML-Elemente anzuwenden. Die Schlüssel für die Pinselressourcen folgen diesem Benennungsformat: `SystemControl[Simple HighContrast name][Simple light/dark name]Brush`. Beispiel: `SystemControlBackgroundAltHighBrush`.

Sehen wir uns an, wie der Farbwert für diesen Pinsel zur Laufzeit bestimmt wird. In den Ressourcenverzeichnissen „Light“ und „Dark“ wird dieser Pinsel wie folgt definiert:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{StaticResource SystemAltHighColor}"/>`

Im Ressourcenverzeichnis „HighContrast“ wird dieser Pinsel wie folgt definiert:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>`

Wenn dieser Pinsel auf ein XAML-Element angewendet wird, wird die Farbe zur Laufzeit durch das aktuelle Design bestimmt, wie in dieser Tabelle zu sehen ist.

| Design        | Einfacher Farbname | Farbressource             | Laufzeitwert                                              |
|--------------|-------------------|----------------------------|------------------------------------------------------------|
| Leicht        | AltHigh           | SystemAltHighColor         | \#FFFFFFFF                                                 |
| Dunkel         | AltHigh           | SystemAltHighColor         | \#FF000000                                                 |
| HighContrast | Hintergrund        | SystemColorButtonFaceColor | Die in den Einstellungen für den Hintergrund der Schaltfläche angegebene Farbe. |

Sie können das `SystemControl[Simple HighContrast name][Simple light/dark name]Brush`-Benennungsschema verwenden, um zu bestimmen, welcher Pinsel auf Ihre eigenen XAML-Elemente angewendet wird.

<!--
For many examples of how the brushes are used in the XAML control templates, see the [Default control styles and templates](default-control-styles-and-templates.md).
-->

> [!NOTE]
> Nicht jede Kombination aus \[*Einfacher Name für HighContrast*\]\[*Einfacher Name für hell/dunkel*\] wird als Pinselressource bereitgestellt.

## <a name="the-xaml-type-ramp"></a>Die XAML-Typhierarchie

Die Datei „themeresources.xaml“ definiert verschiedene Ressourcen, die eine [Formatvorlage](/uwp/api/Windows.UI.Xaml.Style) definieren, die Sie auf Textcontainer in Ihrer UI speziell für [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) oder [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) anwenden können. Dies sind nicht die impliziten Standardstile. Sie werden bereitgestellt, um Ihnen das Erstellen von XAML-UI-Definitionen zu erleichtern, die der in [Richtlinien für Schriftarten](../style/typography.md) dokumentierten *Windows-Typhierarchie* entsprechen.

Diese Stile sind für Textattribute gedacht, die Sie auf den gesamten Textcontainer anwenden möchten. Wenn Sie Formatvorlagen nur auf Textabschnitte anwenden möchten, legen Sie Attribute für die Textelemente im Container fest, zum Beispiel für [Run](/uwp/api/Windows.UI.Xaml.Documents.Run) in [TextBlock.Inlines](/uwp/api/windows.ui.xaml.controls.textblock.inlines) oder für einen [Absatz](/uwp/api/Windows.UI.Xaml.Documents.Paragraph) in [RichTextBlock.Blocks](/uwp/api/windows.ui.xaml.controls.richtextblock.blocks).

Die Formatvorlagen sehen bei Anwendung auf einen [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) wie folgt aus:

![Textblock-Stile](../style/images/type/text-block-type-ramp.svg)

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

Eine Anleitung zur Verwendung der Windows-Typhierarchie in Ihrer App finden Sie unter [Typografie in Windows-Apps](../style/typography.md).

### <a name="basetextblockstyle"></a>BaseTextBlockStyle

**TargetType** : [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

Stellt die gemeinsamen Eigenschaften für alle anderen [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Containerformatvorlagen bereit.

```XAML
<!-- Usage -->
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
</Style>
```

### <a name="headertextblockstyle"></a>HeaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="HeaderTextBlockStyle" TargetType="TextBlock"
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="46"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subheadertextblockstyle"></a>SubheaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubheaderTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="34"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="titletextblockstyle"></a>TitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="TitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="SemiLight"/>
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subtitletextblockstyle"></a>SubtitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubtitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodytextblockstyle"></a>BodyTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BodyTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="14"/>
</Style>
```

### <a name="captiontextblockstyle"></a>CaptionTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="CaptionTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="12"/>
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

### <a name="baserichtextblockstyle"></a>BaseRichTextBlockStyle

**TargetType** : [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)

Stellt die gemeinsamen Eigenschaften für alle anderen [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)-Containerformatvorlagen bereit.

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BaseRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BaseRichTextBlockStyle" TargetType="RichTextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodyrichtextblockstyle"></a>BodyRichTextBlockStyle

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BodyRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BodyRichTextBlockStyle" TargetType="RichTextBlock" BasedOn="{StaticResource BaseRichTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

**Hinweis** :  Die [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)-Formatvorlagen verfügen nicht über alle Texthierarchie-Formatvorlagen von [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock). Dies liegt hauptsächlich daran, dass das blockbasierte Dokumentobjektmodell für **RichTextBlock** das Festlegen von Attributen für die einzelnen Textelemente erleichtert. Außerdem entsteht, wenn Sie [TextBlock.Text](/uwp/api/windows.ui.xaml.controls.textblock.text) mit der XAML-Inhaltseigenschaft festlegen, eine Situation, in der kein zu formatierendes Textelement vorhanden ist und Sie daher den Container formatieren müssen. Das ist für **RichTextBlock** kein Problem, da sein Textinhalt immer in spezifischen Textelementen wie [Absatz](/uwp/api/Windows.UI.Xaml.Documents.Paragraph) enthalten sein muss, in denen Sie XAML-Stile für die Kopfzeile, die Seitenunterüberschrift und ähnliche Texthierarchiedefinitionen festlegen können.

## <a name="miscellaneous-named-styles"></a>Sonstige benannte Stile

Es gibt eine Reihe von weiteren [Formatvorlagen](/uwp/api/Windows.UI.Xaml.Style)-Definitionen mit Schlüsseln, die Sie auf ein [Schaltflächen](/uwp/api/Windows.UI.Xaml.Controls.Button)-Element anders als dessen implizite Standardformatvorlage anwenden können.

### <a name="textblockbuttonstyle"></a>TextBlockButtonStyle

**TargetType** : [ButtonBase](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase)

Wendet diese Formatvorlage auf eine [Schaltfläche](/uwp/api/Windows.UI.Xaml.Controls.Button) an, wenn Sie Text darstellen müssen, auf den ein Benutzer klicken, kann, um eine Aktion auszulösen. Der Text wird mithilfe der aktuellen Akzentfarbe formatiert, um ihn als interaktiv hervorzuheben, und weist Fokusrechtecke auf, die gut für Text geeignet sind. Im Gegensatz zur impliziten Formatvorlage eines [HyperlinkButton](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)-Elements unterstreicht der **TextBlockButtonStyle** den Text nicht.

Die Vorlage formatiert außerdem den angezeigten Text für die Verwendung von **SystemControlHyperlinkBaseMediumBrush** (für den Zustand „PointerOver“), **SystemControlHighlightBaseMediumLowBrush** (für den Zustand „Pressed“) und **SystemControlDisabledBaseLowBrush** (für den Zustand „Disabled“).

Hier ist ein [Schaltflächen](/uwp/api/Windows.UI.Xaml.Controls.Button)-Element mit darauf angewendeter **TextBlockButtonStyle** -Ressource.

```XAML
<Button Content="Clickable text" Style="{StaticResource TextBlockButtonStyle}"
        Click="Button_Click"/>
```

Er sieht ungefähr so aus:

![Eine Schaltfläche, die wie Text aussieht](images/styles-textblock-button-style.png)

### <a name="navigationbackbuttonnormalstyle"></a>NavigationBackButtonNormalStyle

**TargetType** : [Schaltfläche](/uwp/api/Windows.UI.Xaml.Controls.Button)

Diese [Formatvorlage](/uwp/api/Windows.UI.Xaml.Style) stellt eine vollständige Vorlage für ein [Schaltflächen](/uwp/api/Windows.UI.Xaml.Controls.Button)-Element bereit, bei der es sich um die Navigationsschaltfläche „Zurück“ für eine Navigations-App handeln kann. Die Standardgröße ist 40 x 40 Pixel. Um die Formatvorlage anzupassen, können Sie die Eigenschaften [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [FontSize](/uwp/api/windows.ui.xaml.controls.control.fontsize) und andere für Ihr **Schaltflächen** -Element explizit festlegen oder eine abgeleitete Formatvorlage mithilfe von [BasedOn](/uwp/api/windows.ui.xaml.style.basedon) erstellen.

Hier ist ein [Schaltflächen](/uwp/api/Windows.UI.Xaml.Controls.Button)-Element mit der angewendeten **NavigationBackButtonNormalStyle** -Ressource.

```XAML
<Button Style="{StaticResource NavigationBackButtonNormalStyle}" />
```

Er sieht ungefähr so aus:

![Eine als Zurückschaltfläche formatierte Schaltfläche](images/styles-back-button-normal.png)

### <a name="navigationbackbuttonsmallstyle"></a>NavigationBackButtonSmallStyle

**TargetType** : [Schaltfläche](/uwp/api/Windows.UI.Xaml.Controls.Button)

Diese [Formatvorlage](/uwp/api/Windows.UI.Xaml.Style) stellt eine vollständige Vorlage für ein [Schaltflächen](/uwp/api/Windows.UI.Xaml.Controls.Button)-Element bereit, bei der es sich um die Navigationsschaltfläche „Zurück“ für eine Navigations-App handeln kann. Sie ähnelt **NavigationBackButtonNormalStyle** , aber die Größe beträgt 30x30 Pixel.

Hier sehen Sie eine [Schaltfläche](/uwp/api/Windows.UI.Xaml.Controls.Button) mit darauf angewendeter **NavigationBackButtonSmallStyle** -Ressource.

```XAML
<Button Style="{StaticResource NavigationBackButtonSmallStyle}" />
```

## <a name="troubleshooting-theme-resources"></a>Problembehandlung für Designressourcen

Wenn Sie den [Richtlinien für die Verwendung von Designressourcen](#guidelines-for-custom-theme-resources) nicht folgen, kann unerwartetes Verhalten im Zusammenhang mit Designs in Ihrer App auftreten.

Wenn Sie beispielsweise ein Flyout mit hellem Design öffnen, ändern sich auch Teile der App mit dem dunklen Design so, als wären sie im hellen Design. Wenn Sie zu einer Seite mit hellem Design und dann zurück navigieren, sieht die ursprüngliche Seite mit dunklem Design (oder Teile davon) so aus, als wäre sie im hellen Design.

Diese Arten von Problemen treten in der Regel auf, wenn Sie ein Standard-Design und ein HighContrast-Design für die Unterstützung von Szenarien mit hohem Kontrast bereitstellen und dann Light- und Dark-Designs in verschiedenen Teilen der App verwenden.

Betrachten Sie beispielsweise diese Definition eines Designwörterbuchs:

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Intuitiv sieht sie richtig aus. Sie möchten die Farbe ändern, auf die durch `myBrush` mit hohem Kontrast verwiesen wird. Außerhalb des hohen Kontrasts vertrauen Sie aber auf die [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md), um sicherzustellen, dass `myBrush` auf die richtige Farbe für das Design verweist. Wenn Ihre App nie [FrameworkElement.RequestedTheme](/uwp/api/windows.ui.xaml.frameworkelement.requestedtheme) für Elemente in der visuellen Struktur festgelegt hat, funktioniert dies in der Regel wie erwartet. Allerdings treten Probleme in Ihrer App auf, sobald Sie das Design verschiedener Teile der visuellen Struktur ändern.

Das Problem tritt auf, da Pinsel im Gegensatz zu den meisten anderen XAML-Typen freigegebene Ressourcen sind. Wenn Sie zwei Elemente in XAML-Unterstrukturen mit verschiedenen Designs haben, die auf die gleiche Pinselressource verweisen, und das Framework für jede Teilstruktur eine Aktualisierung der Ausdrücke der [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) durchführt, werden Änderungen an der freigegebenen Pinselressource in der anderen untergeordneten Struktur wiedergegeben, was nicht dem gewünschten Ergebnis entspricht.

Ersetzen Sie zum Beheben dieses Problems das Standard-Wörterbuch durch separate Designwörterbücher für die Light- und Dark-Designs zusätzlich zu „HighContrast“:

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Allerdings treten weiterhin Probleme auf, wenn auf eine dieser Ressourcen in geerbten Eigenschaften wie [Foreground](/uwp/api/windows.ui.xaml.controls.control.foreground) verwiesen wird. Die benutzerdefinierte Steuerelementvorlage gibt möglicherweise die Vordergrundfarbe für ein Element mit der [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) an, aber wenn das Framework den geerbten Wert an die untergeordneten Elemente weitergibt, stellt es einen direkten Verweis auf die Ressource bereit, die durch den Ausdruck der {ThemeResource}-Markuperweiterung aufgelöst wurde. Dies führt zu Problemen, wenn das Framework Designänderungen verarbeitet, während es die visuelle Struktur des Steuerelements durchläuft. Es wertet den Ausdruck der {ThemeResource}-Markuperweiterung neu aus, um eine neue Pinselressource abzurufen, gibt diesen Verweis aber noch nicht an die untergeordneten Elemente des Steuerelements weiter; dies geschieht später, zum Beispiel beim nächsten Messwertdurchlauf.

Nach dem Durchlaufen der visuellen Struktur des Steuerelements in Reaktion auf eine Designänderung durchläuft das Framework daher die untergeordneten Elemente und aktualisiert Ausdrücke der [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) für sie oder für Objekte, die für ihre Eigenschaften festgelegt sind. Hier tritt das Problem auf. Das Framework durchläuft die Pinselressource, und da die Farbe mit einer {ThemeResource}-Markuperweiterung angegeben wird, erfolgt eine erneute Auswertung.

An dieser Stelle hat das Framework Ihr Designverzeichnis anscheinend „verunreinigt“, da es jetzt eine Ressource aus einem Wörterbuch enthält, für die die Farbe durch ein anderes Wörterbuch festgelegt wird.

Um dieses Problem zu beheben, verwenden Sie die [{StaticResource}-Markuperweiterung](../../xaml-platform/staticresource-markup-extension.md) anstatt der [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md). Mit den angewendeten Richtlinien sehen die Designverzeichnisse wie folgt aus:

```XAML
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Beachten Sie, dass die [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) weiterhin im HighContrast-Wörterbuch anstelle der [{StaticResource}-Markuperweiterung](../../xaml-platform/staticresource-markup-extension.md) verwendet wird. Diese Situation fällt unter die Ausnahme, die weiter oben in den Richtlinien angegeben wurde. Die meisten Pinselwerte für das HighContrast-Design verwenden eine Farbauswahl, die global vom System gesteuert wird, aber für XAML als Ressource mit einem speziellen Namen („SystemColor“ ist dem Namen vorangestellt) verfügbar gemacht wird. Das System ermöglicht es den Benutzern, im Center für erleichterte Bedienung die spezifischen Farben festzulegen, die für ihre Einstellungen für hohen Kontrast verwendet werden sollen. Diese Farbauswahl gilt für die speziell benannten Ressourcen. Das XAML-Framework verwendet das gleiche Designänderungsereignis, um auch diese Pinsel zu aktualisieren, wenn es erkennt, dass sie auf Systemebene geändert wurden. Darum wird hier die {ThemeResource}-Markuperweiterung verwendet.
