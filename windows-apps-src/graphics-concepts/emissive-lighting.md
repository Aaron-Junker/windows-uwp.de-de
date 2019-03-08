---
title: Emissive Beleuchtung
description: Beim Emissive-Lighting handelt es sich um eine Beleuchtung, die von einem Objekt ausgeht (z. B. ein Glühen).
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- Emissive Beleuchtung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ba112e04518d3e1ee05e7ee8e23e633d4cf59748
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599175"
---
# <a name="emissive-lighting"></a>Emissive Beleuchtung


*Ausstrahlende Beleuchtung* ist Licht, das ein Objekt ausstrahlt; beispielsweise ein Leuchten. Durch das Ausstrahlen erscheint ein angezeigtes Objekt als selbstleuchtend. Die Ausstrahlung wirkt sich auf die Farbe des Objekts aus und kann beispielsweise ein dunkles Material heller erscheinen lassen und einen Teil der ausgestrahlten Farbe annehmen.

Das Emissive-lighting wird durch einen einzigen Faktor beschrieben.

Emissive-Lighting = Cₑ

Dabei gilt Folgendes:

| Parameter | Standardwert | Typ                                                                 | Beschreibung     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | Rot, Grün, Blau und Alphatransparenz (alle Gleitkommawerte) | Emissionsfarbe. |

 

Der Wert für Cₑ ist entweder Farbe 1 oder Farbe 2. Wenn die Vertex-Farbe nicht angegeben wird, wird das Emissive-Materialfarbe verwendet.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


In diesem Beispiel wird das Objekt durch die Farbe des Umgebungslichts der Szene und einer materiellen Umgebungsfarbe hervorgehoben.

Entsprechend der Gleichung ist die resultierende Farbe der Objekt-Vertizes die Materialfarbe.

Die folgende Abbildung zeigt die Materialfarbe (Grün). Die Emissive-Beleuchtung beleuchtet alle Objekt-Vertizes mit derselben Farbe. Es ist nicht von der Vertexnormale oder Lichteinfallsrichtung abhängig. Die Kugel sieht wie ein 2D-Kreis aus, da es keinen Unterschied in der Schattierung der Oberfläche des Objekts gibt.

![Abbildung einer grünen Kugel](images/lighte.jpg)

Die folgende Abbildung zeigt, wie sich die Emissive-Beleuchtung mit den anderen drei Beleuchtungstypen mischt. Auf der rechten Seite der Kugel ist eine Mischung aus grüne Emissive- und roter Ambient-Beleuchtung sichtbar. Auf der linken Seite der Kugel mischt sich die grüne Emissive-Beleuchtung mit einer Diffuse-Beleuchtung und erzeugt so einen roten Farbverlauf. Das Glanzlicht in der Mitte ist Weiß. Es sorgt für einen gelben Ring, denn der Wert der Glanz-Beleuchtung fällt stark ab. Dies sorgt dafür, dass die Werte für die Ambiente-, Diffuse- und Emissions-Beleuchtung gemischt werden. So entsteht das Gelb.

![Abbildung einer grüne Kugel mit Emissive-Beleuchtung](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Mathematik der Beleuchtung](mathematics-of-lighting.md)

 

 




