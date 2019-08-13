---
description: Erfahren Sie, wie Sie die Typografie in Ihrer App verwenden, um Benutzern Inhalte leicht verständlich zu machen.
title: Typografie in UWP-Apps
ms.date: 04/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1d162fcf9a0f1023c58792e8c9f7a0e22fac4440
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867755"
---
# <a name="typography"></a>Typografie

![Herobild](images/header-typography.svg)

Typografie muss übersichtlich sein, da sie zur visuellen Darstellung von Sprache dient, um Informationen zu vermitteln. Ihr Stil darf diesem Ziel nie im Wege stehen. In diesem Artikel erläutern wir, wie Sie die Typografie in Ihre UWP-App formatieren, damit Benutzer Inhalte schnell und effizient verstehen.

## <a name="font"></a>Font

Verwenden Sie eine Schriftart in der gesamten Benutzeroberfläche Ihrer App. Es wird empfohlen, wenn möglich, die Standardschriftart für UWP-Apps **Segoe UI** zu verwenden. Sie wurde entwickelt, um eine optimale Lesbarkeit für Größe und Pixeldichte zu wahren, und bietet eine klare, ansprechende und offene Ästhetik, die den Inhalt des Systems ergänzt.

![Beispieltext für die Schriftart „Segoe UI“](images/type/segoe-sample.svg)

Weitere Informationen zum Anzeigen anderer Sprachen als Englisch oder um eine andere Schriftart für Ihre App auszuwählen finden Sie unter [Sprachen](#languages) und [Schriftarten](#fonts) für unsere empfohlenen Schriftarten für UWP-Apps.

:::row:::
    :::column:::
![Richtig](images/do.svg) Wählen Sie eine Schriftart für Ihre Benutzeroberfläche aus.
    :::column-end:::
    :::column:::
![Falsch](images/dont.svg) Mischen Sie nicht mehrere Schriftarten.
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>Größe und Skalierung

Schriftgrade in UWP-Apps werden automatisch auf allen Geräten skaliert. Mit dem Skalierungsalgorithmus wird sichergestellt, dass der Schriftgrad 24 Pixel auf einem 3 Meter entfernten Surface Hub genauso lesbar ist wie der Schriftgrad 24 Pixel auf einem 5-Zoll-Smartphone, das nur einige Zentimeter entfernt ist.

![Sichtabstände für verschiedene Geräte](images/type/scaling-chart.svg)

Aufgrund der Funktionsweise der Skalierung, entwerfen Sie in effektiven Pixeln, nicht in den tatsächlichen physischen Pixeln, und Sie müssen den Schriftgrad für unterschiedliche Bildschirmgrößen und Auflösungen nicht ändern.

:::row:::
    :::column:::
![Richtig](images/do.svg) Folgen Sie der Größenanpassung der UWP-[Typhierarchie](#type-ramp).
    :::column-end:::
    :::column:::
![Falsch](images/dont.svg) Verwenden Sie einen Schriftgrad kleiner als 12 Pixel.
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>Hierarchie

:::row:::
    :::column:::
Benutzer folgen beim Sichten einer Seite der visuellen Hierarchie: Überschriften fassen Inhalte zusammen, Textkörper enthalten weitere Details. Um eine klare visuelle Hierarchie in Ihrer App zu erstellen, folgen Sie der UWP-Typhierarchie.
    :::column-end:::
    :::column:::
![Textblock-Stile](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>Typhierarchie

Die UWP-Typhierarchie stellt wichtige Beziehungen zwischen den Schriftschnitten auf einer Seite her, damit der Benutzer den Inhalt einfach lesen kann. Alle Größen werden in effektiven Pixeln angegeben und sind für UWP-Apps optimiert, die auf allen Geräten ausgeführt werden.

![Typhierarchie](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>Verwenden der Typhierarchie

:::row:::
    :::column:::
Sie können auf Ebenen der Typhierarchie als [statische Ressourcen](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp) für XAML zugreifen. Die Stile folgen der Benennungskonvention `*TextBlockStyle`.
    :::column-end:::
    :::column:::
![Textblock-Stile](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

:::row:::
    :::column:::
![Richtig](images/do.svg) Verwenden Sie für die meisten Texte „Body“.

Verwenden Sie „Base“ für Titel mit begrenztem Platz.
    :::column-end:::
    :::column:::
![Falsch](images/dont.svg) Verwenden Sie „Caption“ nicht für die Hauptaktion oder für lange Zeichenfolgen.

Verwenden Sie „Header“ oder „Subheader“, wenn Text umgebrochen werden muss.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>Ausrichtung

Standardmäßig ist das [TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) links. In den meisten Fällen sorgt das Konzept „links bündig, rechts mit Flatterrand“ für eine konsistente Verankerung des Inhalts und für ein einheitliches Layout. Weitere Informationen für RTL-Sprachen finden Sie unter [Anpassen von Layout und Schriftarten zur Globalisierungsunterstützung](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

![Zeigt linksbündigen Text.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>Zeichenanzahl

:::row:::
    :::column:::
![Richtig](images/do.svg) Halten Sie sich zur besseren Lesbarkeit an 50 bis 60 Wörter pro Zeile.
    :::column-end:::
    :::column:::
![Falsch](images/dont.svg) Weniger als 20 oder mehr als 60 Zeichen pro Zeile sind schwer lesbar.
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>Beschnitt und Ellipsen

Wenn die Textmenge den verfügbaren Speicherplatz überschreitet, wird empfohlen, den Text zuzuschneiden, was dem Standardverhalten der meisten [UWP-Textsteuerelemente](../controls-and-patterns/text-controls.md) entspricht.

![Gerät mit abgeschnittenem Text](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![Richtig](images/do.svg) Schneiden Sie Text zu, oder brechen Sie ihn um, wenn mehrere Zeilen aktiviert sind.
    :::column-end:::
    :::column:::
![Falsch](images/dont.svg) Verwenden Sie für mehr Übersichtlichkeit Ellipsen.
    :::column-end:::
:::row-end:::

**Hinweis**: Bei Containern, die nicht klar definiert sind (also sich etwa nicht durch eine andere Hintergrundfarbe abheben) oder wenn ein Link zu mehr Text existiert, kann eine Ellipse verwendet werden.

## <a name="languages"></a>Sprachen 

Segoe UI ist unsere Schriftart für Englisch, für europäische Sprachen, Griechisch, Hebräisch, Armenisch, Georgisch und Arabisch. Lesen Sie die folgenden Empfehlungen für andere Sprachen.

### <a name="globalizinglocalizing-fonts"></a>Globalisierung/Lokalisierung von Schriftarten

Verwenden Sie die [LanguageFont-Schriftartenersetzungs-APIs](https://docs.microsoft.com/uwp/api/Windows.Globalization.Fonts.LanguageFont) für den programmgesteuerten Zugriff auf die Empfohlenen Einstellungen für Familie, Grad, Breite und Schnitt der Schriftart für eine spezielle Sprache. Das LanguageFont-Objekt ermöglicht den Zugriff auf die richtigen Schriftartinformationen für verschiedene Inhaltskategorien: UI-Kopfzeilen, Benachrichtigungen, Textkörper und Schriftarten für den Textkörper, die vom Benutzer bearbeitet werden können. Weitere Informationen finden Sie unter [Anpassen von Layout und Schriftarten zur Globalisierungsunterstützung](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

### <a name="fonts-for-non-latin-languages"></a>Schriftarten für nicht lateinische Sprachen

<table>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Anmerkungen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Normal, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für afrikanische Schriften (Äthiopisch, N'Ko, Osmanya, Tifinagh, Vai).</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Normal, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für nordamerikanische Schriften (Kanadische Silbenschrift, Cherokee).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">Normal, Semilight, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für südostasiatische Schriften (Buginesisch, Laotisch, Khmer, Thailändisch).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für Koreanisch.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Normal, fett, Light</td>
<td align="left">Benutzeroberflächen-Schriftart für Chinesisch (traditionell).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Normal, fett, Light</td>
<td align="left">Benutzeroberflächen-Schriftart für Chinesisch (vereinfacht).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für die Myanmar-Schrift.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Normal, Semilight, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für südasiatische Schriften (Bangla, Devanagari, Gujarati, Gurmukhi, Kannada, Malayalam, Odia, Ol Chiki, Singhalesisch, Sora Sompeng, Tamil, Telugu)</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Regular</td>
<td align="left">Eine ältere chinesische UI-Schriftart. </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Light, Semilight, normal, Semibold, fett</td>
<td align="left">Benutzeroberflächen-Schriftart für Japanisch.</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>Schriftarten

### <a name="sans-serif-fonts"></a>Serifenlose Schriftarten

Serifenlose Schriftarten eignen sich für Überschriften und UI-Elemente. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Anmerkungen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Normal, kursiv, fett, fett kursiv, schwarz</td>
<td align="left">Unterstützung für europäische und nahöstliche Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch, Armenisch und Hebräisch). Die Schriftbreite „Black“ unterstützt nur europäische Schriften.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Normal, kursiv, fett, fett kursiv, dünn, dünn kursiv</td>
<td align="left">Unterstützung für europäische und nahöstliche Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch und Hebräisch). Arabisch nur in gerader Schrift verfügbar.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Normal, kursiv, fett, fett kursiv</td>
<td>Schriftart mit fester Breite mit Unterstützung für europäische Schriften (Lateinisch, Griechisch und Kyrillisch).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Normal, kursiv, Light kursiv, Black kursiv, fett, fett kursiv, Light, Semilight, Semibold, Black</td>
<td>Benutzeroberflächen-Schriftart für europäische und nahöstliche Schriften (Arabisch, Armenisch, Kyrillisch, Georgisch, Griechisch, Hebräisch, Lateinisch) und auch Lisu-Schrift.</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Normal, Semilight, Light, fett, Semibold</td>
<td align="left">Open-Source-Schriftart, die metrisch kompatibel mit Segoe UI ist. Vorgesehen für Apps auf anderen Plattformen, auf denen Segoe UI nicht verfügbar ist. <a href="https://github.com/Microsoft/Selawik">Laden Sie Selawik über GitHub herunter.</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Serifenschriftarten

Mit Serifenschriftarten lassen sich größere Textmengen gut darstellen. 

<table>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Anmerkungen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Regular</td>
<td align="left">Serifenschriftart mit Unterstützung für europäischen Schriften (Lateinisch, Griechisch, Kyrillisch).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left">Serifenschriftart mit fester Breite und Unterstützung für europäische und nahöstliche Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch, Armenisch und Hebräisch).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgien</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left">Unterstützung für europäische Schriften (Lateinisch, Griechisch und Kyrillisch).</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Normal, kursiv, fett, fett kursiv</td>
<td align="left">Ältere Schriftart mit Unterstützung für europäische Schriften (Lateinisch, Griechisch, Kyrillisch, Arabisch, Armenisch, Hebräisch).</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>Symbole

<table>
<thead>
<tr class="header">
<th align="left">Schriftfamilie</th>
<th align="left">Stile</th>
<th align="left">Anmerkungen</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Regular</td>
<td align="left">Benutzeroberflächen-Schriftart für App-Symbole. Weitere Informationen finden Sie im Artikel <a href="segoe-ui-symbol-font.md">Segoe MDL2 Assets</a>.</td>
</tr>
<tr class="even">
<td align="left">Segoe UI-Emoji</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">Fallbackschriftart für Symbole</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>Verwandte Artikel

* [Textsteuerelemente](../controls-and-patterns/text-controls.md)
* [XAML-Designressourcen](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [XAML-Stile](../controls-and-patterns/xaml-styles.md)
* [Microsoft Typografie](https://docs.microsoft.com/typography/)
