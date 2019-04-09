---
title: Abstände und Größen
description: Die neue Fluent-Standard und Compact Steuerelementstile Vergewissern Sie sich eine vertraut Benutzeroberfläche unabhängig von Geräte- und Eingabe-Methode.
keywords: UWP, Windows 10, Steuerelemente, Größe, Density, Standard, compact
ms.date: 4/4/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7b74e3dc2ad047d9e52509b71ef00b829ad63a0d
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249452"
---
# <a name="control-size-and-density"></a>Größe des Steuerelements und Dichte

Verwenden Sie eine Kombination von Steuerelementgröße und Dichte, optimieren Ihre Anwendung für die universelle Windows-Plattform (UWP), und geben Sie eine Benutzeroberfläche, die den Anforderungen Ihrer app-Funktionalität und Interaktion am besten geeignet ist.

Standardmäßig werden die UWP-apps mit einer mit geringer Dichte gerendert (oder `Standard`) Layout. Seit WinUI 2.1, die eine hohe Dichte (oder `Compact`) Layoutoption aus, für die Informationen werden umfangreiche Benutzeroberfläche und spezielle Szenarien ähnlich, wird auch unterstützt. Dies kann über eine grundlegende Stilressource angegeben werden (Siehe Beispiele unten).

Bei der Funktionalität und Verhalten nicht geändert und bleibt konsistent, zwischen den beiden Optionen für Größe und Dichte, den Schriftgrad des Standard-Text wurde auf 14px für alle Steuerelemente auf die Supportoptionen für diese beiden Dichte aktualisiert. Diese Schriftgrad über Regionen und Geräte hinweg funktioniert und stellt sicher, dass Ihre Anwendung mit Lastenausgleich und wissen, für Benutzer bleibt.

## <a name="fluent-standard-sizing"></a>Fluent-Standard-größenanpassung

*Fluent-Standard-größenanpassung* wurde entwickelt, um ein Gleichgewicht zwischen Informationen Dichte Komfort für Benutzer bereitstellen. Richten Sie effektiv alle Elemente auf dem Bildschirm zu einem effektiven Pixeln von 40 x 40 (Epx)-Ziel, das basiert, können Sie UI-Elemente an ein Raster ausrichten und entsprechend skalieren zum Skalieren der Ebene von System.

**Standardmäßige Größe soll jeweils berührungs- und geben Zeiger zu berücksichtigen.**

> [!NOTE]
>Weitere Informationen zur effektiven Pixeln und Skalieren von Daten zu erhalten, finden Sie unter [Einführung in die UWP-app-Design](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Weitere Informationen zum System Ebene skalieren finden Sie unter [Ausrichtung, Margin und Padding](../layout/alignment-margin-padding.md).

Für das Windows 10 Oktober 2018 Update (Version 1809), der Standard, die Standardgröße für alle UWP-Steuerelemente wurde verringert, um die benutzerfreundlichkeit für alle Verwendungsszenarien zu erhöhen.

Die folgende Abbildung zeigt einige der das Steuerelement Änderungen am Layout, die mit dem Windows eingeführt wurden 10 Oktober 2018 aktualisieren. Insbesondere der Rand zwischen einem Header und dem oberen Rand eines Steuerelements aus 8epx verringert wurde, um 4epx und 44epx Raster an ein Raster 40epx geändert wurde.

![Beispiel für einen Standardsteuerelement](images/standarddensity.png)

*Beispiel für einen Standardsteuerelement*

Dieser nächste Abbildung zeigt die Änderungen, die zum Steuern von Größen für die Windows 10 Oktober 2018 aktualisieren. Insbesondere Ausrichtung am Raster 40epx.

![Standard-Eingabeereignisse-Beispiel](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent-Compact-größenanpassung

Compact größenanpassung ermöglicht Dichte, informationsreiche eine Gruppe von Steuerelementen und kann durch Folgendes:

- Durchsuchen große Mengen an Inhalt.
- Maximieren des sichtbaren Inhalts auf einer Seite.
- Navigation und Interaktion mit der Steuerelemente und Inhalte

**Compact größenanpassung dient in erster Linie zum Zeiger Eingabe zu berücksichtigen.**

### <a name="examples"></a>Beispiele

Kompakte Größe wird über eine spezielle Ressourcenverzeichnis implementiert, die in Ihrer Anwendung auf Seitenebene oder auf einem bestimmten Layout angegeben werden können. Das Ressourcenverzeichnis finden Sie in der [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) Nuget-Paket.

Die folgenden Beispiele zeigen wie die der `Compact` Stil für die Seite und ein einzelnes Grid-Steuerelement angewendet werden kann.

#### <a name="page-level"></a>Auf Seitenebene

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

## <a name="related-articles"></a>Verwandte Artikel

- [Richtlinien für die Touch-Ziele](../input/guidelines-for-targeting.md)
- [ResourceDictionary- und XAML-Ressourcenreferenzen](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [Ressourcenverzeichnis](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML-Formatvorlagen](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
