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
ms.openlocfilehash: 9b1cac04405f18aaf3c8f39f9bfce2b965577807
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257941"
---
# <a name="guidelines-for-touch-targets"></a>Richtlinien für Berührungs Ziele

Alle interaktiven Benutzeroberflächen Elemente in ihrer universelle Windows-Plattform-Anwendung (UWP) müssen groß genug sein, damit Benutzer unabhängig vom Gerätetyp oder der Eingabemethode genau darauf zugreifen und diese verwenden können.

Die Unterstützung von Berührungs Eingaben (und die relativ ungenaue Natur des Berührungs Kontakt Bereichs) erfordert eine weitere Optimierung in Bezug auf die Zielgröße und das Steuerelement Layout, da der größere, komplexere Satz von Eingabedaten, die vom Fingerabdruck-Digitalisierer gemeldet werden, verwendet wird, um den das beabsichtigte (oder wahrscheinlichste) Ziel des Benutzers.

Alle UWP-Steuerelemente wurden mit standardmäßigen Berührungs Zielgrößen und-Layouts entworfen, mit denen Sie visuell ausgeglichene und ansprechende Apps erstellen können, die komfortabel und einfach zu verwenden sind.

In diesem Thema werden diese Standardverhalten beschrieben, damit Sie Ihre APP für maximale Nutzbarkeit mithilfe von Platt Form Steuerelementen und benutzerdefinierten Steuerelementen entwerfen können (falls Ihre APP Sie benötigt).

> **Wichtige APIs**: [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Standard-Größenanpassung von Fluent

Die *Standard-Größenanpassung von Fluent* wurde entwickelt, um ein Gleichgewicht zwischen Informationsdichte und Benutzerfreundlichkeit zu schaffen. Effektiv werden alle Elemente auf dem Bildschirm auf einen Zielwert von 40 x 40 effektive Pixel (epx) ausgerichtet, wodurch UI-Elemente an einem Raster ausgerichtet und gemäß der Skalierung auf Systemebene entsprechend skaliert werden.

> [!NOTE]
>Weitere Informationen zu effektiven Pixeln und Skalierung finden Sie unter [Einführung in das UWP-App-Design](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
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

Sie können diese Empfehlungen für die Zielgröße an die Anforderungen des jeweiligen Szenarios anpassen. Hier sind einige Punkte zu beachten:

- Häufigkeit von Berührungen: Stellen Sie sich vor, dass Ziele, die wiederholt oder häufig gedrückt werden, die minimale Größe überschreiten.
- Fehler Folge: Ziele, die schwerwiegende Folgen haben, wenn Sie bei einem Fehler berührt werden, sollten größere Abstände aufweisen und am Rand des Inhalts Bereichs weiter platziert werden. Dies gilt insbesondere für Ziele, die häufig berührt werden.
- Position im Inhalts Bereich.
- Form Faktor und Bildschirmgröße.
- Der Finger Zustand.
- Touchscreen-Visualisierungen.

## <a name="related-articles"></a>Verwandte Artikel

- [Einführung in das UWP-App-Design](../basics/design-and-ui-intro.md)
- [Steuerelement Größe und-Dichte](../style/spacing.md)
- [Ausrichtung, Rand, Abstand](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Beispiele

- [Beispiel für eine einfache Eingabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabe Beispiel mit niedriger Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für XAML-Benutzereingabe Ereignisse](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
- [Eingabe: Beispiel für Gerätefunktionen](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
- [Eingabe: Beispiel für Berührungs Treffer Tests](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
- [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
- [Eingabe: vereinfachtes Ink-Beispiel](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
- [Eingabe: Beispiel für Windows 8-Gesten](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Eingabe: Manipulationen und Gesten (C++) (Beispiel)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
- [Beispiel für DirectX-Fingereingabe](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
