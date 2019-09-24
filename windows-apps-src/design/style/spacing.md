---
title: Abstände und Größen
description: Die neuen Fluent-Steuerelementstilarten Standard und Compact stellen unabhängig von Gerät und Eingabemethode eine vertraute Benutzeroberfläche sicher.
keywords: UWP, Windows 10, Steuerelemente, Größe, Dichte, Standard, Compact
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 1c535e4ea4ad3c93acb048de2050d5ae7a9c2c2b
ms.sourcegitcommit: bf95c8b29145a224957a940512394e6aa97cb90f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71061938"
---
# <a name="control-size-and-density"></a>Größe und Dichte des Steuerelements

Optimieren Sie Ihre UWP (Universal Windows Platform)-Anwendung anhand einer Kombination von Steuerelementgröße und -dichte, und stellen Sie eine Benutzererfahrung bereit, die für die Funktions- und Interaktionsanforderungen der App am besten geeignet ist.

Standardmäßig werden UWP-Apps mit einem Layout mit geringer Dichte (bzw. `Standard`) gerendert. Ab WinUI 2.1 wird jedoch auch eine Layoutoption mit hoher Dichte (bzw. `Compact`) für Benutzeroberflächen mit vielfältigen Informationen oder ähnlich spezialisierte Szenarien unterstützt. Dies kann über eine grundlegende Stilressource angegeben werden (siehe Beispiele unten).

Funktionalität und Verhalten haben sich nicht geändert und sind weiterhin einheitlich für die beiden Größen- und Dichteoptionen, der Standardschriftgrad für Text wurde jedoch für alle Steuerelemente auf 14 px aktualisiert, damit beide Dichteoptionen unterstützt werden. Dieser Schriftgrad funktioniert in allen Regionen und auf allen Geräten; damit wird sichergestellt, dass Ihre Anwendung ausgewogen und benutzerfreundlich bleibt.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/Compact Sizing">die App zu öffnen und die Änderung in eine kompakte Größe in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-standard-sizing"></a>Standard-Größenanpassung von Fluent

Die *Standard-Größenanpassung von Fluent* wurde entwickelt, um ein Gleichgewicht zwischen Informationsdichte und Benutzerfreundlichkeit zu schaffen. Effektiv werden alle Elemente auf dem Bildschirm auf einen Zielwert von 40 x 40 effektive Pixel (epx) ausgerichtet, wodurch UI-Elemente an einem Raster ausgerichtet und gemäß der Skalierung auf Systemebene entsprechend skaliert werden.

**Die Standard-Größenanpassung wurde sowohl auf Touch- als auch Zeigereingaben ausgelegt.**

> [!NOTE]
>Weitere Informationen zu effektiven Pixeln und Skalierung finden Sie unter [Einführung in das UWP-App-Design](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Weitere Informationen zum Skalieren auf der Systemebene finden Sie unter [Ausrichtung, Rand, Abstand](../layout/alignment-margin-padding.md).

Für das Windows 10 October 2018 Update (Version 1809) wurde die Standard-Größe für alle UWP-Steuerelemente verringert, um die Benutzerfreundlichkeit in allen Verwendungsszenarien zu verbessern.

In der folgenden Abbildung werden einige der Layoutänderungen für Steuerelemente veranschaulicht, die mit dem Windows 10 October 2018 Update eingeführt wurden. Konkret wurde der Rand zwischen einer Überschrift und der Oberkante eines Steuerelements von 8epx auf 4epx verringert, und das 44epx-Raster wurde in ein 40epx-Raster geändert.

![Beispiel eines Standard-Layouts für Steuerelemente](images/standarddensity.png)

*Beispiel eines Standard-Layouts für Steuerelemente*

Die nächste Abbildung veranschaulicht die Änderungen in Bezug auf Steuerelementgrößen, die mit dem Windows 10 October 2018 Update eingeführt wurden. Hier sehen Sie die Ausrichtung am 40epx-Raster.

![Beispiel für Standard-Befehle](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Compact-Größenanpassung von Fluent

Die Compact-Größenanpassung ermöglicht dichte Gruppen von Steuerelementen mit vielfältigen Informationen; sie bietet Vorteile für folgende Vorgänge:

- Durchsuchen großer Inhaltsmengen.
- Maximieren des sichtbaren Inhalts auf einer Seite.
- Navigation durch und Interaktion mit Steuerelementen und Inhalten

**Die Compact-Größenanpassung ist hauptsächlich auf Zeigereingaben ausgelegt.**

### <a name="examples"></a>Beispiele

Die Compact-Größenanpassung wird über ein spezielles Ressourcenverzeichnis implementiert, das in Ihrer Anwendung entweder auf der Seitenebene oder in einem bestimmten Layout angegeben werden kann. Das Ressourcenverzeichnis finden Sie im [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/)-Nuget-Paket.

In den folgenden Beispielen wird erläutert, wie der `Compact`-Stil auf die Seite und auf ein einzelnes Grid-Steuerelement angewendet wird.

#### <a name="page-level"></a>Seitenebene

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>Rasterebene

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Richtlinien für Touch-Ziele](../input/guidelines-for-targeting.md)
- [ResourceDictionary- und XAML-Ressourcenreferenzen](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [Ressourcenverzeichnis](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML-Stile](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
