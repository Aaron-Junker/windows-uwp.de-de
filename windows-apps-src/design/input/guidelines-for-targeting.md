---
Description: Dieses Thema beschreibt die Verwendung von Kontaktgeometrie zur Bestimmung von Touchzielen sowie bewährte Methoden für Ziele in Windows-Runtime-Apps.
title: Zielbestimmung
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 34f8d15b971cc9ed286471010a21d1b44b84af13
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363472"
---
# <a name="guidelines-for-touch-targets"></a>Richtlinien für die Touch-Ziele

Alle Elemente der interaktive Benutzeroberfläche in Ihrer Anwendung für die universelle Windows-Plattform (UWP) muss groß genug ist, damit Benutzer genau zugreifen und diese verwenden, unabhängig von Gerät-Typ oder die Eingabe-Methode.

Unterstützung von Touch-Eingabe (und welche relativ unpräzise die Touch-Bereich "Kontakt") eine weitere Optimierung im Hinblick auf Größe und Steuerelement Ziel-Layout erfordert, wie die umfangreicher und komplexere Satz von Eingabedaten, die durch den Touch Digitizer gemeldet verwendet wird, um zu bestimmen, die des Benutzers (oder wahrscheinlichsten) beabsichtigte Ziel.

Alle UWP-Steuerelemente wurden entworfen mit Standard-Touch-Ziel-Größen und Layouts, mit denen Sie zum Erstellen von visuell mit Lastenausgleich und ansprechende apps, die vertraut, einfach zu verwenden, und inspirieren vertrauen.

In diesem Thema beschreiben wir dieses Standardverhalten können Sie Ihre app für maximale benutzerfreundlichkeit mit Plattformsteuerelemente und benutzerdefinierte Steuerelemente (Ihre app diese erforderlich ist) entwerfen.

> **Wichtige APIs:** [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent-Standard-größenanpassung

*Fluent-Standard-größenanpassung* wurde entwickelt, um ein Gleichgewicht zwischen Informationen Dichte Komfort für Benutzer bereitstellen. Richten Sie effektiv alle Elemente auf dem Bildschirm zu einem effektiven Pixeln von 40 x 40 (Epx)-Ziel, das basiert, können Sie UI-Elemente an ein Raster ausrichten und entsprechend skalieren zum Skalieren der Ebene von System.

> [!NOTE]
>Weitere Informationen zur effektiven Pixeln und Skalieren von Daten zu erhalten, finden Sie unter [Einführung in die UWP-app-Design](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Weitere Informationen zum System Ebene skalieren finden Sie unter [Ausrichtung, Margin und Padding](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Fluent-Compact-größenanpassung

Anwendungen können anzeigen, ein höheres Maß an informationsdichte mit *Fluent Compact größenanpassung*. Compact größenanpassung richtet die UI-Elemente für ein 32 x 32 Epx-Ziel, können Sie UI-Elemente, die an einer engeren Raster und festen Dezimalstellen, die abhängig von Ihrer System Ebene Skalierung ausgerichtet.

### <a name="examples"></a>Beispiele

Compact größenanpassung kann auf der Seite oder Raster angewendet werden.

### <a name="page-level"></a>Auf Seitenebene

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>Rasterebene

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>Zielgröße

Legen Sie im Allgemeinen die Zielgröße Touch 7,5 mm quadratischen Bereich (40 x 40 Pixel auf einem 135 PPI-Bildschirm bei einem 1,0 x Plateau Skalierung). Richten UWP-Steuerelemente in der Regel mit 7,5 mm Ziel (Dies kann basierend auf das bestimmte Steuerelement und alle gängige Verwendungsmuster variieren). Finden Sie unter [Steuern der Größe und Dichte](../style/spacing.md) Weitere Details.

Sie können diese Empfehlungen für die Zielgröße an die Anforderungen des jeweiligen Szenarios anpassen. Hier sind einige Punkte zu berücksichtigen:

- Häufigkeit von Workflows – markieren Sie Ziele, die wiederholt oder häufig größer als die minimale Größe gedrückt werden.
- Fehler Folge - Ziele, die schwerwiegende Konsequenzen, wenn Fehler verwendete sollte größer Auffüllung und weiter von den Rand des Inhaltsbereichs platziert werden. Dies gilt insbesondere für Ziele, die häufig berührt werden.
- Die Position im Bereich.
- Bilden Sie Faktor und der Bildschirmgröße.
- Sicherheitsstatus der Finger.
- Berühren Sie Visualisierungen.

## <a name="related-articles"></a>Verwandte Artikel

- [Einführung in das UWP-App-Design](../basics/design-and-ui-intro.md)
- [Größe des Steuerelements und Dichte](../style/spacing.md)
- [Ausrichtung, Margin und padding](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Proben

- [Grundlegende Eingabebeispiel](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [Eingabebeispiel mit geringer Latenz](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [Beispiel für den Benutzerinteraktionsmodus](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: XAML-benutzerbeispiel Eingabeereignisse](https://go.microsoft.com/fwlink/p/?linkid=226855)
- [Eingabe: Funktionen-gerätebeispiel](https://go.microsoft.com/fwlink/p/?linkid=231530)
- [Eingabe: Beispiel zu Leistungstests in Touch Treffer](https://go.microsoft.com/fwlink/p/?linkid=231590)
- [XAML Bildlauf, schwenken und Zoomen Beispiel](https://go.microsoft.com/fwlink/p/?linkid=251717)
- [Eingabe: Vereinfachte Freihand-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=246570)
- [Eingabe: Beispiel für Windows 8-Gesten](https://go.microsoft.com/fwlink/p/?LinkId=264995)
- [Eingabe: Manipulationen und Beispiel für Bewegungen (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
- [DirectX-Touch-Eingabe-Beispiel](https://go.microsoft.com/fwlink/p/?LinkID=231627)
