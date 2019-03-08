---
title: Lichtzuordnung mit Texturen
description: Eine Lichtzuordnung ist eine Textur oder Texturgruppe, die Informationen über Licht in der 3D-Szene enthält.
ms.assetid: 5C7518D2-AC92-4A97-B7AF-4469D213D7BD
keywords:
- Lichtzuordnung mit Texturen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b5d245247d33f3c04839620615f2778ef7dfb59
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660025"
---
# <a name="light-mapping-with-textures"></a>Lichtzuordnung mit Texturen


Eine Lichtzuordnung ist eine Textur oder Texturgruppe, die Informationen über Licht in der 3D-Szene enthält. Lichtzuordnungen ordnen Licht- und Schattenbereiche Grundtypen zu. Multipass und das Mischen mehrerer Texturen ermöglichen Ihrer Anwendung, Szenen mit einer realistischeren Darstellung zu rendern als mit Schattierungstechniken.

Damit eine Anwendung eine 3D-Szene realistisch rendern kann, muss der Effekt von Lichtquellen in der Darstellung der Szene berücksichtigt werden. Obwohl Techniken wie Flat und Gouraud Shading in dieser Hinsicht wertvolle Tools darstellen, sind sie für Ihre Anforderungen möglicherweise unzureichend. Direct3D unterstützt auch Multipass und das Mischen mehrerer Texturen. Mit diesen Funktionen kann Ihre Anwendung Szenen in einer realistischeren Darstellung rendern als nur mit Shading-Techniken gerenderte Szenen. Durch die Anwendung einer oder mehrerer Lichtzuordnungen kann Ihre Anwendung Licht- und Schattenbereiche ihren Grundtypen zuordnen.

Eine Lichtzuordnung ist eine Textur oder Texturgruppe, die Informationen über Licht in der 3D-Szene enthält. Sie können Lichtinformationen in den Alpha-Werten der Lichtzuordnung, in den Farbwerten oder in beiden speichern.

Wenn Sie die Lichtzuordnung mithilfe eine mehrstufigen Texturverblendung implementieren, sollte Ihre Anwendung die Lichtzuordnung zu seinen Grundtypen beim ersten Durchlauf rendern. Im zweiten Durchlauf sollte sie die Basistextur rendern. Die Ausnahme bildet die Glanzlichtzuordnung. Rendern Sie in diesem Fall zuerst die Basistextur und fügen Sie dann die Lichtzuordnung hinzu.

Die mehrstufige Texturverblendung ermöglicht Ihrer Anwendung, die Lichtzuordnung und die Basistextur in einem Durchlauf zu rendern. Wenn die Benutzerhardware für mehrstufige Texturverblendung geeignet ist, sollte Ihre Anwendung diesen Vorteil für die Lichtzuordnung nutzen. Damit verbessern Sie die Leistung Ihrer Anwendung erheblich.

Durch die Verwendung von Lichtzuordnungen kann eine Direct3D-Anwendung beim Rendern von Grundtypen zahlreiche Lichteffekte erreichen. Sie kann nicht nur monochrome und farbige Lichter in einer Szene zuordnen, sondern auch Details hinzufügen, wie beispielsweise Glanzlicht oder diffuses Licht.

Informationen zur Verwendung der Texturverblendung in Direct3D zum Ausführen einer Lichtzuordnung finden Sie in folgenden Themen.

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
<td align="left"><p><a href="monochrome-light-maps.md">Monochrome Licht zugeordnet wird.</a></p></td>
<td align="left"><p>Die einfarbige Lichtzuordnung ermöglicht älteren Adaptern das Ausführen der Vermischung von mehrlagigen Texturen, wenn eine ältere 3D-Beschleunigungsplatine die Texturvermischung mit dem Alphawert des Zielpixels nicht unterstützt.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="color-light-maps.md">Helle Farbe-Zuordnungen</a></p></td>
<td align="left"><p>Eine farbige Lichtzuordnung verwendet für die Beleuchtungsinformationen RGB-Daten in der Lichtzuordnung. Eine Anwendung rendert 3D-Szenen in der Regel realistischer, wenn farbige Lichtzuordnungen verwendet werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-light-maps.md">Glänzende Licht zugeordnet wird.</a></p></td>
<td align="left"><p>Glänzende Objekte aus hochreflektieRendern Materialien erhalten durch das Beleuchten mit einer Lichtquelle Glanzlichter. Manchmal erhalten Sie genauere Glanzlichter, wenn Sie eine Glanzlichtzuordnung zu den Grundtypen anwenden, statt die vom Beleuchtungsmodul erzeugten Glanzlichter zu verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-light-maps.md">Diffuses Licht Zuordnungen</a></p></td>
<td align="left"><p>Matte Oberflächen haben eine diffuse Lichtreflektion. Die Helligkeit von diffusem Licht ist von der Entfernung zur Lichtquelle und dem Winkel zwischen der Oberflächennormale und dem Richtungsvektor der Lichtquelle abhängig. Texturlichtzuordnungen können komplexe diffuse Beleuchtung simulieren.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)

 

 




