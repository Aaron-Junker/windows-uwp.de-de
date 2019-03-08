---
title: Einführung in die Regeln für die Rasterung
description: Häufig stimmen die als Scheitelpunkte angegebenen Punkte nicht exakt mit den Pixeln auf dem Bildschirm überein. Wenn dieser Fall eintritt, verwendet Direct3D Dreiecksrasterregeln, um zu entscheiden, welche Pixel für ein vorhandenes Dreieck gelten.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- Einführung in die Regeln für die Rasterung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d1907be029254d99be9e6158c93c179baea1fb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641085"
---
# <a name="introduction-to-rasterization-rules"></a>Einführung in die Regeln für die Rasterung


Häufig stimmen die als Scheitelpunkte angegebenen Punkte nicht exakt mit den Pixeln auf dem Bildschirm überein. Wenn dieser Fall eintritt, verwendet Direct3D Dreiecksrasterregeln, um zu entscheiden, welche Pixel für ein vorhandenes Dreieck gelten.

Es folgt eine vereinfachte Einführung in die Regeln für die Rasterung. Weitere Einzelheiten finden Sie unter [Regeln für die Rasterung](rasterization-rules.md). Siehe auch [Rasterizerphase (RS)](rasterizer-stage--rs-.md).

## <a name="span-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>Dreieck Rasterization Regeln


Direct3D verwendet für die Füllgeometrie eine Oben-Links-Füllkonvention. Es handelt sich hierbei um die gleiche Konvention, die in GDI und OpenGL für Rechtecke verwendet wird. In Direct3D ist der Mittelpunkt eines Pixels der entscheidende Punkt. Wenn der Mittelpunkt in einem Dreieck liegt, ist das Pixel Teil des Dreiecks. Die Mittelpunkte der Pixel liegen auf ganzzahligen Koordinaten.

Die Beschreibung der von Direct3D verwendeten Dreiecksrasterregeln gilt nicht automatisch für jede verfügbare Hardware. Beim Testen werden Sie unter Umständen minimale Abweichungen in der Implementierung dieser Regeln aufdecken.

In der folgenden Abbildung wird ein Rechteck dargestellt, dessen obere linke Ecke bei (0, 0) liegt und dessen untere Rechte Ecke bei (5, 5) liegt. Dieses Rechteck füllt, genau wie erwartet, 25 Pixel. Die Breite des Rechtecks wird durch „Rechts minus Links“ definiert. Die Höhe wird durch „Unterseite minus Oberseite“ definiert.

![ein nummeriertes Viereck, unterteilt in sechs Reihen und Spalten.](images/pixmap.png)

Gemäß der Oben-Links-Füllkonvention, bezieht sich *Oben* auf die vertikale Position der horizontalen Struktur und *Links* auf die horizontale Position des Pixels in einer Struktur. Bei einer Kante kann es sich nicht um eine Oberkante handeln, wenn sie nicht horizontal liegt. Grundsätzlich haben die meisten Dreiecke nur linke und rechte Kanten. In der folgenden Abbildung wird eine Oberkante und eine rechte Kante dargestellt.

![ein nummeriertes Viereck, in dem sich zwei Dreiecke befinden](images/triedge.png)

Die Oben-Links-Konvention bestimmt die von Direct3D unternommenen Aktionen, wenn ein Dreieck durch den Mittelpunkt eines Pixels verläuft. In der folgenden Abbildung werden zwei Dreiecke dargestellt. Eines liegt bei (0, 0), (5, 0) und (5, 5), das andere bei (0, 5), (0, 0) und (5, 5). Das erste Dreieck erhält in diesem Fall 15 Pixel (schwarz dargestellt), wobei das zweite Dreieck nur 10 Pixel (grau dargestellt) erhält, weil die gemeinsame Kante auch die linke Kante des ersten Dreiecks ist.

![ein nummeriertes Viereck mit zwei Dreiecken](images/twotris.png)

Wenn Sie ein Rechteck mit der unteren linken Ecke bei (0,5, 0,5) und der unteren rechten Ecke bei (2,5, 4,5) definieren, liegt der Mittelpunkt des Rechtecks bei (1,5, 2,5). Wenn der Direct3D-Rasterizer dieses Rechteck tesseliert, liegt der Mittelpunkt eines jeden Pixels eindeutig innerhalb jedem der vier Dreiecke, und die Oben-Links-Konvention ist nicht erforderlich. Die folgende Abbildung stellt dies dar. Die Pixel im Rechteck werden gemäß dem Dreieck bezeichnet, dem Direct3D sie zuordnet.

![ein nummeriertes Viereck, das ein Rechteck enthält, welches in vier Dreiecke unterteilt ist](images/noambig.png)

Wenn Sie das Rechteck aus der vorherigen Abbildung verschieben, so dass die linke obere Ecke bei (1,0, 1,0), die untere rechte Ecke bei (3,0, 5,0) und der Mittelpunkt bei (2,0, 3,0) liegen, wendet Direct3D die Oben-Links-Konvention an. Die meisten Pixel in diesem Rechteck überspannen die Grenze zwischen zwei oder mehr Dreiecken, wie in der folgenden Abbildung dargestellt.

![ein nummeriertes Viereck, das ein Rechteck enthält, welches in vier Dreiecke unterteilt ist](images/fillrule.png)

Für die beiden Rechtecke sind die gleichen Pixel betroffen, wie in folgender Abbildung dargestellt.

![Pixel, die für die beiden vorherigen nummerierten Vierecke gelten](images/samepix.png)

## <a name="span-idpointandlinerulesspanspan-idpointandlinerulesspanspan-idpointandlinerulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>Punkt- und Linienregeln


Punkte werden auf die gleiche Weise gerendert wie Punkt-Sprites, welche beide als am Bildschirm ausgerichtete Vierecke gerendert werden und deshalb den gleichen Regeln wie beim Polygon-Rendering unterliegen.

Die Rendering-Regeln für nicht geglättete Linien stimmen exakt mit denen für [GDI lines](https://msdn.microsoft.com/library/windows/desktop/dd145027) überein.

## <a name="span-idpointspriterulesspanspan-idpointspriterulesspanspan-idpointspriterulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Punkt Sprite-Regeln


Punkt-Sprites und Patch-Grundtypen sind gerastert, da die Grundtypen zuerst in Dreiecke tesseliert, und die daraus entstehenden Dreiecke dann gerastert werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

[Phase des Rasterizers (RS)](rasterizer-stage--rs-.md)

[Rasterization Regeln](rasterization-rules.md)

 

 




