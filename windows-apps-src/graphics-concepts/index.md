---
title: Glossar für Direct3D-Grafiken
description: Hier finden Sie eine Definition der von Microsoft Direct3D verwendeten Grafikbegriffe.
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Glossar für Direct3D-Grafiken
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3cb6a2466ea201c9b5047f7eb159477a0d584429
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "63805639"
---
# <a name="direct3d-graphics-glossary"></a>Glossar für Direct3D-Grafiken


Hier werden Begriffe für Microsoft Direct3D-Grafiken definiert. In diesem Glossar werden allgemeine Begriffe für 3D-Computergrafiken definiert, die in der Spiele- und App-Entwicklung in Direct3D verwendet werden.

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
<td align="left"><p><a href="coordinate-systems-and-geometry.md">Koordinatensysteme und Geometrie</a></p></td>
<td align="left"><p>Für das Programmieren von Direct3D-Anwendungen muss der Anwender mit geometrischen 3D-Prinzipien vertraut sein. Dieser Abschnitt stellt die wichtigsten geometrischen Konzepte zum Erstellen von 3D-Szenen vor.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">Vertex- und Indexpuffer</a></p></td>
<td align="left"><p><em>Vertexpuffer</em> sind Speicherpuffer, die Scheitelpunktdaten enthalten. Scheitelpunkte in einem Scheitelpunktpuffer werden verarbeitet, um Transformationen sowie Beleuchtungs- und Zuschneidevorgänge auszuführen. <em>Indexpuffer</em> sind Speicherpuffer mit Indexdaten, die Ganzzahl-Offsets in Vertexpuffer darstellen und zum Rendern von Grundtypen verwendet werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">Geräte</a></p></td>
<td align="left"><p>Ein Direct3D-Gerät ist die Renderingkomponente von Direct3D. Ein Gerät kapselt und speichert den Renderstatus, führt Transformationen und Beleuchtungsvorgänge aus und rastert ein Bild auf einer Oberfläche.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">Beleuchtung</a></p></td>
<td align="left"><p>Beleuchtung wird verwendet, um Objekte in einer Szene zu beleuchten. Die Farbe eines Objektvertex basiert auf der aktuellen Texturzuordnung, den Vertexfarben und den Lichtquellen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">Tiefen- und Schablonenpuffer</a></p></td>
<td align="left"><p>Ein <em>Tiefenpuffer</em> speichert Tiefeninformationen, um zu steuern, welche Bereiche eines Polygons gerendert oder verborgen werden. Ein <em>Schablonenpuffer</em> wird zum Maskieren von Pixeln in einem Bild verwendet, um Spezialeffekte zu erzielen. Diese Effekte umfassen Zusammensetzungen, Rasterungen, Auflösungen, Ein- und Ausblendungen, Wischbewegungen, Umrisse und Silhouetten sowie das Feature „Zweiseitige Schablone“.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">Texturen</a></p></td>
<td align="left"><p>Texturen sind ein leistungsstarkes Tool, um mit dem Computer realistische 3D-Bilder zu erzeugen. Direct3D unterstützt einen umfangreichen Texturfunktionssatz und ermöglicht Entwicklern dadurch den einfachen Zugriff auf erweiterte Texturtechniken.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">Grafikpipeline</a></p></td>
<td align="left"><p>Die Direct3D-Grafikpipeline dient zum Erstellen von Grafiken für Echtzeitspiele. Von der Eingabe zur Ausgabe fließen Daten durch die einzelnen konfigurier- oder programmierbaren Phasen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">Ansichten</a></p></td>
<td align="left"><p>Der Begriff &quot;Ansicht&quot; wird für &quot;Daten im erforderlichen Format&quot; verwendet. In einer Konstantenpufferansicht (Constant Buffer View, CBV) werden beispielweise ordnungsgemäß formatierte konstante Pufferdaten dargestellt. Dieser Abschnitt beschreibt die gängigsten und hilfreichsten Ansichten.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">Compute-Pipeline</a></p></td>
<td align="left"><p>Die Direct3D-Compute-Pipeline wurde zur Verarbeitung von Berechnungen entwickelt, die meist parallel mit der Grafikpipeline ausgeführt werden können.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">Ressourcen</a></p></td>
<td align="left"><p>Eine Ressource ist ein Bereich im Speicher, auf den von der Direct3D-Pipeline zugegriffen werden kann. Damit die Pipeline effizient auf den Speicher zugreifen kann, müssen für die Pipeline bereitgestellte Daten (etwa Eingabegeometrie, Shaderressourcen und Texturen) in einer Ressource gespeichert werden. Es gibt zwei Arten von Ressourcen, aus denen alle Direct3D-Ressourcen abgeleitet sind: Puffer und Textur. Für jede Pipelinephase können bis zu 128 Ressourcen aktiv sein.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">Streamingressourcen</a></p></td>
<td align="left"><p><em>Streamingressourcen</em> sind umfangreiche logische Ressourcen, die wenig physischen Speicher belegen. Anstatt die gesamte umfangreiche Ressource zu übergeben, werden nur kleine Teile der Ressource nach Bedarf gestreamt. Streamingressourcen wurden vorher als <em>unterteilte Ressourcen</em> bezeichnet.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">Anhänge</a></p></td>
<td align="left"><p>Diese Abschnitte enthalten ausführliche technische Details.</p></td>
</tr>
</tbody>
</table>

 

 

 
