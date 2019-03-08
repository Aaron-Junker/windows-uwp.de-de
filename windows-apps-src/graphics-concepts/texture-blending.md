---
title: Texturvermischung
description: Direct3D kann bis zu acht Texturen auf Grundtypen in einem einzigen Durchlauf auf Grundtypen mischen.
ms.assetid: 9AD388FA-B2B9-44A9-B73E-EDBD7357ACFB
keywords:
- Texturvermischung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c40c7d3bd080bd927fc52cb7f740e1dc4a6358c0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620805"
---
# <a name="texture-blending"></a>Texturvermischung


Direct3D kann bis zu acht Texturen auf Grundtypen in einem einzigen Durchlauf auf Grundtypen mischen. Die Verwendung von mehreren gemischten Texturen kann die Framerate einer Direct3D-Anwendung erheblich erhöhen. Eine Anwendung verwendet mehrere Texturmischungen zur Anwendung von Texturen, Schatten, glänzender Beleuchtung, diffuser Beleuchtung und anderer Spezialeffekte in einem einzigen Durchgang.

Um die Texturmischung verwenden zu können, muss Ihre Anwendung zuerst prüfen, ob dies von der Hardware des Benutzers unterstützt wird.

## <a name="span-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespantexture-stages-and-the-texture-blending-cascade"></a><span id="Texture-Stages-and-the-Texture-Blending-Cascade"></span><span id="texture-stages-and-the-texture-blending-cascade"></span><span id="TEXTURE-STAGES-AND-THE-TEXTURE-BLENDING-CASCADE"></span>Textur-Phasen und die Kombination von Cascade Textur


Direct3D unterstützt die mehrfache Texturmischung in einem Durchgang durch die Verwendung von Texturphasen. Eine Texturphase nimmt zwei Argumente und führt darauf einen Mischvorgang aus; das Ergebnis wird dann zur weiteren Verarbeitung oder zur Rasterisierung weitergegeben. Eine Texturphase kann wie im folgenden Diagramm gezeigt visualisiert werden.

![Diagramm einer Texturphase](images/texstg.png)

Wie das obige Diagramm zeigt, mischen Texturphasen zwei Argumente unter Verwendung eines angegebenen Operators. Häufig ausgeführte Vorgänge sind u. a. die einfache Modulation oder Hinzufügung der Farb- oder Alphakomponenten der Argumente, insgesamt werden jedoch mehr als zwei Dutzend Vorgänge unterstützt. Die Argumente für eine Phase können eine zugeordnete Textur, der iterierte Farb- oder Alphawert (iteriert im Rahmen der Gouraud-Schattierung), ein beliebiger Farb- oder Alphawert oder das Ergebnis der vorherigen Texturphase sein.

**Beachten Sie**    Direct3D unterscheidet, Farbe, die aus der alpha-blending blending. Anwendungen stellen Mischvorgänge und Argumente für Farbe und Alpha einzeln ein, und die Ergebnisse dieser Einstellungen sind voneinander unabhängig.

 

Die Kombination der von mehreren Mischphasen verwendeten Argumente und Vorgänge definiert eine einfache, ablaufbasierte Mischungssprache. Die Ergebnisse werden von einer Phase an die nächste übergeben, von dieser Phase wieder an die nächste und so weiter. Dieses Konzept der Weitergabe der Ergebnisse von Phase zu Phase bis zur schließlichen Rasterisierung auf einem Vieleck wird oft als „Texturmischungskaskade“ bezeichnet. Das folgende Diagramm zeigt, wie einzelne Texturphasen die Texturmischungskaskade bilden.

![Diagramm der Texturphasen in einer Texturmischungskaskade](images/tcascade.png)

Jede Phase in einem Gerät hat einen nullbasierten Index. Direct3D erlaubt bis zu acht Mischungsphasen; Sie sollten jedoch stets die Möglichkeiten des Geräts prüfen, um festzustellen, wie viele Phasen die aktuell verwendete Hardware unterstützt. Die erste Mischungsphase befindet sich bei Index 0, die zweite bei Index 1 und so weiter, bis Index 7. Das System mischt Phasen in aufsteigender Reihenfolge der Indizes.

Verwenden Sie nur die Anzahl der Phasen, die Sie benötigen. die nicht verwendeten Mischungsphasen sind standardmäßig deaktiviert. Wenn Ihre Anwendung nur die ersten beiden Phasen verwendet, müssen daher nur Vorgänge und Argumente für Phase 0 und 1 festgelegt werden. Das System mischt die beiden Phasen und berücksichtigt die deaktivierten Phasen nicht.

Wenn Ihre Anwendung für unterschiedliche Situationen unterschiedlich viele Phasen verwendet – z. B. vier Phasen für einige Objekte und nur zwei für andere -, müssen Sie nicht alle vorher verwendeten Phasen explizit deaktivieren. Eine Möglichkeit besteht darin, den Farbvorgang für die erste nicht verwendete Phase zu deaktivieren; dann werden alle Phasen mit einem höheren Index nicht angewendet. Eine weitere Möglichkeit ist, die Texturzuordnung ganz zu deaktivieren, indem Sie den Farbvorgang für die erste Texturphase (Phase 0) in den deaktivierten Zustand versetzen.

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
<td align="left"><p><a href="blending-stages.md">Kombination von Phasen</a></p></td>
<td align="left"><p>Eine Vermischungsstufe ist ein Satz von Texturvorgängen und ihren Argumenten, die festlegen, wie Texturen vermischt werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multipass-texture-blending.md">Kombination von Renderingdurchläufen Textur</a></p></td>
<td align="left"><p>Direct3D-Apps können durch die Anwendung verschiedener Texturen auf eine Primitive im Laufe von mehreren Berechnungs- und Ausgabedurchläufen zahlreiche Spezialeffekte erzielen. Der allgemeine Begriff dafür ist <em>Mehrstufige Texturmischung</em>. Eine typische Anwendung für die mehrstufige Texturvermischung liegt in der Nachbildung der Effekte von komplexen Beleuchtungs- und Verschattungsmodellen durch die Anwendung mehrerer Farben aus mehreren unterschiedlichen Texturen. Eine solche Anwendung heißt <em>Lichtzuordnung</em>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)

 

 




