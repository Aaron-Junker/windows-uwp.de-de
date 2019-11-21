---
Description: Dieses Thema beschreibt die Verwendung von Kontaktgeometrie zur Bestimmung von Touchzielen sowie bewährte Methoden für Ziele in Windows-Runtime-Apps.
title: Zielbestimmung
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cac04405f18aaf3c8f39f9bfce2b965577807
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257941"
---
# <a name="guidelines-for-touch-targets"></a>Guidelines for touch targets

All interactive UI elements in your Universal Windows Platform (UWP) application must be large enough for users to accurately access and use, regardless of device type or input method.

Supporting touch input (and the relatively imprecise nature of the touch contact area) requires further optimization with respect to target size and control layout as the larger, more complex set of input data reported by the touch digitizer is used to determine the user's intended (or most likely) target.

All UWP controls have been designed with default touch target sizes and layouts that enable you to build visually balanced and appealing apps that are comfortable, easy to use, and inspire confidence.

In this topic, we describe these default behaviors so you can design your app for maximum usability using both platform controls and custom controls (should your app require them).

> **Wichtige APIs**: [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Standard-Größenanpassung von Fluent

Die *Standard-Größenanpassung von Fluent* wurde entwickelt, um ein Gleichgewicht zwischen Informationsdichte und Benutzerfreundlichkeit zu schaffen. Effektiv werden alle Elemente auf dem Bildschirm auf einen Zielwert von 40 x 40 effektive Pixel (epx) ausgerichtet, wodurch UI-Elemente an einem Raster ausgerichtet und gemäß der Skalierung auf Systemebene entsprechend skaliert werden.

> [!NOTE]
>Weitere Informationen zu effektiven Pixeln und Skalierung finden Sie unter [Einführung in das UWP-App-Design](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Weitere Informationen zum Skalieren auf der Systemebene finden Sie unter [Ausrichtung, Rand, Abstand](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Compact-Größenanpassung von Fluent

Applications can display a higher level of information density with *Fluent Compact sizing*. Compact sizing aligns UI elements to a 32x32 epx target, which lets UI elements to align to a tighter grid and scale appropriately based on system level scaling.

### <a name="examples"></a>Beispiele

Compact sizing can be applied at the page or grid level.

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

## <a name="target-size"></a>Target size

In general, set your touch target size to 7.5mm square range (40x40 pixels on a 135 PPI display at a 1.0x scaling plateau). Typically, UWP controls align with 7.5mm touch target (this can vary based on the specific control and any common usage patterns). See [Control size and density](../style/spacing.md) for more detail.

Sie können diese Empfehlungen für die Zielgröße an die Anforderungen des jeweiligen Szenarios anpassen. Here are some things to consider:

- Frequency of Touches - consider making targets that are repeatedly or frequently pressed larger than the minimum size.
- Error Consequence - targets that have severe consequences if touched in error should have greater padding and be placed further from the edge of the content area. Dies gilt insbesondere für Ziele, die häufig berührt werden.
- Position in the content area.
- Form factor and screen size.
- Finger posture.
- Touch visualizations.

## <a name="related-articles"></a>Verwandte Artikel

- [Einführung in das UWP-App-Design](../basics/design-and-ui-intro.md)
- [Control size and density](../style/spacing.md)
- [Ausrichtung, Rand, Abstand](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Beispiele

- [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Input: XAML user input events sample](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
- [Input: Device capabilities sample](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
- [Input: Touch hit testing sample](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
- [XAML scrolling, panning, and zooming sample](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
- [Input: Simplified ink sample](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
- [Input: Windows 8 gestures sample](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Input: Manipulations and gestures (C++) sample](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
- [DirectX touch input sample](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
