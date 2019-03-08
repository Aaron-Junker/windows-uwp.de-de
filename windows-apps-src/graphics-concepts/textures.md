---
title: Texturen
description: Texturen sind ein leistungsstarkes Tool, um mit dem Computer realistische 3D-Bilder zu erzeugen. Direct3D unterstützt einen umfangreichen Texturfunktionssatz, und ermöglicht Entwicklern einfach auf erweiterte Texturtechniken zuzugreifen.
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords:
- Texturen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 81d77b262cc77c23d859cf76227a34bc72b15b96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593005"
---
# <a name="textures"></a>Texturen


Texturen sind ein leistungsstarkes Tool, um mit dem Computer realistische 3D-Bilder zu erzeugen. Direct3D unterstützt einen umfangreichen Texturfunktionssatz, und ermöglicht Entwicklern einfach auf erweiterte Texturtechniken zuzugreifen.

Für bessere Leistung sollten Sie dynamische Texturen verwenden. Eine dynamische Textur kann für jeden Frame gesperrt, beschrieben und entsperrt werden.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="introduction-to-textures.md">Einführung in Texturen</a></p></td>
<td align="left"><p>Eine Texturressource ist eine Datenstruktur zum Speichern von Texel, der die kleinste Einheit einer Textur darstellt, die gelesen oder geschrieben werden kann. Wird eine Textur von einem Shader gelesen, kann sie durch Textursampler gefiltert werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="basic-texturing-concepts.md">Mit Grundkonzepte</a></p></td>
<td align="left"><p>Obwohl frühe computergenerierte 3D-Bilder generell hoch entwickelt waren, sahen diese künstlich glänzend aus. Es fehlte ihnen Kennzeichen – wie etwa Abnutzungen, Risse, Fingerabdrücke und Flecken -, die 3D-Objekten eine realistische optische Komplexität verleihen. Texturen sind besonders für den verbesserten Realismus von computergenerierten 3D-Bilder beliebt.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-addressing-modes.md">Textur adressierungsmodi</a></p></td>
<td align="left"><p>Die Direct3D-Anwendung kann jedem Scheitelpunkt jedes Grundtyps Texturkoordinaten zuweisen. In der Regel liegen die u- und v-Texturkoordinaten, die Sie einem Scheitelpunkt zuweisen, im Bereich zwischen einschließlich 0,0 und 1,0. Durch die Zuweisung von Texturkoordinaten außerhalb dieses Bereichs können Sie jedoch bestimmte Textur-Spezialeffekte erzielen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering.md">Texturfilterung</a></p></td>
<td align="left"><p>Die Texturfilterung erzeugt eine Farbe für jedes Pixel im 2D-gerenderten Bild des Grundtyps, wenn dieser durch die Abbildung eines 3D-Grundtyps auf einem 2D-Bildschirm gerendert wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-resources.md">Texturressourcen</a></p></td>
<td align="left"><p>Texturen sind eine Art von Ressource, die zum Rendern verwendet wird.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-wrapping.md">Textur-Wrapper</a></p></td>
<td align="left"><p>Der Texturumbruch ändert die grundlegende Weise, in der Direct3D texturierte Vielecke rasterisiert, unter Verwendung der für jeden Scheitelpunkt angegebenen Texturkoordinaten. Beim Rastern eines Vielecks interpoliert das System zwischen den Texturkoordinaten an jedem der Scheitelpunkte des Vielecks, um die Texel zu bestimmen, die für jedes Pixel des Vielecks zu verwenden sind.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-blending.md">Textur blending</a></p></td>
<td align="left"><p>Direct3D kann bis zu acht Texturen auf Grundtypen in einem einzigen Durchlauf auf Grundtypen mischen. Die Verwendung von mehreren gemischten Texturen kann die Framerate einer Direct3D-Anwendung erheblich erhöhen. Eine Anwendung verwendet mehrere Texturmischungen zur Anwendung von Texturen, Schatten, glänzender Beleuchtung, diffuser Beleuchtung und anderer Spezialeffekte in einem einzigen Durchgang.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-mapping-with-textures.md">Einfache Zuordnung mit Texturen</a></p></td>
<td align="left"><p>Eine Lichtzuordnung ist eine Textur oder Texturgruppe, die Informationen über Licht in der 3D-Szene enthält. Lichtzuordnungen ordnen Licht- und Schattenbereiche Grundtypen zu. Multipass und das Mischen mehrerer Texturen ermöglichen Ihrer Anwendung, Szenen mit einer realistischeren Darstellung zu rendern als mit Schattierungstechniken.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compressed-texture-resources.md">Komprimierte des texturressourcen</a></p></td>
<td align="left"><p>Texturabbildungen sind digitale Bilder, die auf dreidimensionale Formen gezeichnete werden, um diesen mehr Details zu verleihen. Sie werden während der Rasterung in diesen Formen wiedergegeben. Der Prozess kann große Mengen des Systembuses und des Speichers verbrauchen. Um den von den Texturen verbrauchten Speicherplatz zu reduzieren, unterstützt Direct3D die Komprimierung von Texturoberflächen. Einige Direct3D-Geräte bieten eine systemeigene Unterstützung für komprimierte Texturoberflächen.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Schulungsleitfaden für Direct3D-Grafiken](index.md)

 

 




