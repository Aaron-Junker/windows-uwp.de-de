---
Description: Dieses Thema beschreibt die Verwendung von Kontaktgeometrie zur Bestimmung von Touchzielen sowie bewährte Methoden für Ziele in Windows-Runtime-Apps.
title: Zielgruppenadressierung
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8608c1ff607c76c3f121fe5ed5fded9098911c9d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172434"
---
# <a name="guidelines-for-touch-targets"></a>Richtlinien für Touch-Ziele

Alle interaktiven Benutzeroberflächen Elemente in Ihrer Windows-Anwendung müssen groß genug sein, damit Benutzer unabhängig vom Gerätetyp oder der Eingabemethode genau darauf zugreifen und diese verwenden können.

Die Unterstützung von Berührungs Eingaben (und die relativ ungenaue Natur des Berührungs Kontakt Bereichs) erfordert weitere Optimierungen in Bezug auf die Zielgröße und das Layout des Steuer Elements, da der größere, komplexere Satz von Eingabedaten, die vom Fingerabdruck-Digitalisierer gemeldet werden, verwendet wird, um das beabsichtigte (oder wahrscheinlichste) Ziel des Benutzers zu bestimmen

Alle UWP-Steuerelemente wurden mit standardmäßigen Berührungs Zielgrößen und-Layouts entworfen, mit denen Sie visuell ausgeglichene und ansprechende Apps erstellen können, die komfortabel und einfach zu verwenden sind.

In diesem Thema werden diese Standardverhalten beschrieben, damit Sie Ihre APP für maximale Nutzbarkeit mithilfe von Platt Form Steuerelementen und benutzerdefinierten Steuerelementen entwerfen können (falls Ihre APP Sie benötigt).

> **Wichtige APIs**: [**Windows. UI. Core**](/uwp/api/Windows.UI.Core), [**Windows. UI. Input**](/uwp/api/Windows.UI.Input), [**Windows. UI. XAML. Input**](/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Standard-Größenanpassung von Fluent

Die *Standard-Größenanpassung von Fluent* wurde entwickelt, um ein Gleichgewicht zwischen Informationsdichte und Benutzerfreundlichkeit zu schaffen. Effektiv werden alle Elemente auf dem Bildschirm auf einen Zielwert von 40 x 40 effektive Pixel (epx) ausgerichtet, wodurch UI-Elemente an einem Raster ausgerichtet und gemäß der Skalierung auf Systemebene entsprechend skaliert werden.

> [!NOTE]
> Weitere Informationen zu effektiven Pixeln und Skalierung finden Sie unter [Einführung in das Windows-App-Design](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Weitere Informationen zum Skalieren auf der Systemebene finden Sie unter [Ausrichtung, Rand, Abstand](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Compact-Größenanpassung von Fluent

Anwendungen können ein höheres Maß an Informationsdichte mit der *fließenden kompakten Größe*anzeigen. Bei der kompakten Größenanpassung werden Benutzeroberflächen Elemente an ein 32 x 32-EPX-Ziel angepasst, sodass Benutzeroberflächen Elemente an einem engeren Raster ausgerichtet und entsprechend der Skalierung auf Systemebene entsprechend skaliert werden können.

### <a name="examples"></a>Beispiele

Compact Sizing kann auf Seiten-oder Rasterebene angewendet werden.

### <a name="page-level"></a>Seitenebene

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

Legen Sie im Allgemeinen die Größe des touchziels auf einen quadratischen Bereich von 7,5 mm fest (40 x 40 Pixel auf einer 135-ppi-Anzeige auf einem 1.0 x-Skalierungs Plateau). UWP-Steuerelemente entsprechen in der Regel dem 7,5-mm-Berührungs Ziel (Dies kann je nach dem spezifischen Steuerelement und allen gängigen Verwendungs Mustern variieren). Weitere Details finden Sie unter [Steuerelement Größe und Dichte](../style/spacing.md) .

Sie können diese Empfehlungen für die Zielgröße an die Anforderungen des jeweiligen Szenarios anpassen. Folgende Punkte sollten berücksichtigt werden:

- Häufigkeit von Berührungen: Stellen Sie sich vor, dass Ziele, die wiederholt oder häufig gedrückt werden, die minimale Größe überschreiten.
- Fehler Folge: Ziele, die schwerwiegende Folgen haben, wenn Sie bei einem Fehler berührt werden, sollten größere Abstände aufweisen und am Rand des Inhalts Bereichs weiter platziert werden. Dies gilt insbesondere für Ziele, die häufig berührt werden.
- Position im Inhalts Bereich.
- Form Faktor und Bildschirmgröße.
- Der Finger Zustand.
- Touchscreen-Visualisierungen.

## <a name="related-articles"></a>Verwandte Artikel

- [Einführung in das Design von Windows-Apps](../basics/design-and-ui-intro.md)
- [Größe und Dichte des Steuerelements](../style/spacing.md)
- [Ausrichtung, Rand, Abstand](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Proben

- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Eingabe: Beispiel für Gerätefunktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Eingabe: Beispiel für Fingereingabe-Treffertests](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Eingabe: vereinfachtes Freihandbeispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Eingabe: Beispiel für Windows 8-Bewegungen](/samples/browse/?redirectedfrom=MSDN-samples)
* [Eingabe: Manipulationen und Gesten (Beispiel)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Beispiel für die DirectX-Fingereingabe](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))