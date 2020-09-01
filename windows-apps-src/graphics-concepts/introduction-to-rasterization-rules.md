---
title: Einführung in die Regeln für die Rasterung
description: Häufig Stimmen die für Scheitel Punkte angegebenen Punkte nicht exakt mit den Pixeln auf dem Bildschirm identisch. In diesem Fall wendet Direct3D Dreiecks rasterisierungsregeln an, um zu entscheiden, welche Pixel für ein bestimmtes Dreieck gelten.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- Einführung in die Regeln für die Rasterung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 38522be28280c0a08f6cb065e5dfb5c2f26642a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162794"
---
# <a name="introduction-to-rasterization-rules"></a>Einführung in die Regeln für die Rasterung


Häufig Stimmen die für Scheitel Punkte angegebenen Punkte nicht exakt mit den Pixeln auf dem Bildschirm identisch. In diesem Fall wendet Direct3D Dreiecks rasterisierungsregeln an, um zu entscheiden, welche Pixel für ein bestimmtes Dreieck gelten.

Dies ist eine vereinfachte Einführung in rasterisierungsregeln. Weitere Informationen finden Sie unter [rasterization Rules](rasterization-rules.md). Siehe auch die [Phase Rasterizer (RS)](rasterizer-stage--rs-.md).

## <a name="span-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>Dreiecks rasterisierungsregeln


Direct3D verwendet zum Auffüllen der Geometrie eine obere linke Füllungs Konvention. Dabei handelt es sich um dieselbe Konvention, die für Rechtecke in GDI und OpenGL verwendet wird. In Direct3D ist der Mittelpunkt des Pixels der entscheidende Punkt. Wenn sich das Zentrum in einem Dreieck befindet, ist das Pixel Teil des Dreiecks. Pixel Center sind an ganzzahligen Koordinaten.

Diese Beschreibung der von Direct3D verwendeten Dreiecks rasterisierungsregeln gilt nicht notwendigerweise für alle verfügbaren Hardware. Die Tests können kleinere Abweichungen in der Implementierung dieser Regeln aufdecken.

Die folgende Abbildung zeigt ein Rechteck, dessen linke obere Ecke bei (0,0) liegt und dessen untere rechte Ecke um (5, 5) liegt. Dieses Rechteck füllt 25 Pixel wie erwartet aus. Die Breite des Rechtecks wird als rechts minus Links definiert. Die Höhe ist als "Bottom minus Top" definiert.

![ein nummeriertes Quadrat, das in sechs Zeilen und Spalten aufgeteilt ist.](images/pixmap.png)

In der oberen linken Füllungs Konvention verweist *Top* auf die vertikale Position horizontaler spannen, und *left* bezieht sich auf die horizontale Position der Pixel innerhalb einer Spanne. Ein Edge darf kein oberer Rand sein, es sei denn, er ist horizontal. Im Allgemeinen haben die meisten Dreiecke nur den linken und rechten Rand. Die folgende Abbildung zeigt einen oberen und einen rechten Rand.

![ein nummeriertes Quadrat, das zwei Dreiecke enthält.](images/triedge.png)

Die obere linke Füllungs Konvention bestimmt die Aktion, die von Direct3D ausgeführt wird, wenn ein Dreieck den Mittelpunkt eines Pixels durchläuft. Die folgende Abbildung zeigt zwei Dreiecke: eins bei (0,0), (5, 0) und (5, 5) und das andere bei (0,0), (0, 0) und (5, 5). Das erste Dreieck in diesem Fall erhält 15 Pixel (schwarz dargestellt), während das zweite nur 10 Pixel (grau dargestellt) erhält, da der freigegebene Rand der linke Rand des ersten Dreiecks ist.

![ein nummeriertes Quadrat, das zwei Dreiecke anzeigt](images/twotris.png)

Wenn Sie ein Rechteck mit der linken oberen Ecke bei (0,5, 0,5) und der unteren rechten Ecke um (2,5, 4,5) definieren, befindet sich der Mittelpunkt dieses Rechtecks bei (1,5, 2,5). Wenn das Direct3D Raster-Element dieses Rechteck durchläuft, ist die Mitte der einzelnen Pixel eindeutig innerhalb der vier Dreiecke, und die obere linke Füllungs Konvention ist nicht erforderlich. Dies wird in der folgenden Abbildung veranschaulicht. Die Pixel im Rechteck sind entsprechend dem Dreieck gekennzeichnet, in dem Direct3D Sie enthält.

![ein nummeriertes Quadrat, das ein Rechteck enthält, das in vier Dreiecke aufgeteilt ist.](images/noambig.png)

Wenn Sie das Rechteck in der obigen Abbildung verschieben, sodass sich die obere linke Ecke bei (1,0, 1,0), der unteren rechten Ecke bei (3,0, 5,0) und dem zugehörigen Mittelpunkt bei (2,0, 3,0) befindet, wendet Direct3D die obere Füllungs Konvention an. Die meisten Pixel dieses Rechtecks verspannen den Rahmen zwischen zwei oder mehr Dreiecken, wie in der folgenden Abbildung dargestellt.

![ein nummeriertes Quadrat, das ein Rechteck enthält, das in vier Dreiecke aufgeteilt ist.](images/fillrule.png)

Bei beiden Rechtecke sind die gleichen Pixel betroffen, wie in der folgenden Abbildung dargestellt.

![Pixel, die von den vorangehenden zwei nummerierten Quadraten betroffen sind](images/samepix.png)

## <a name="span-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>Punkt-und Linien Regeln


Punkte werden genauso wie Punkt Sprite gerendert, die beide als Bildschirm ausgerichtete vierpunkte gerendert werden und somit denselben Regeln wie das Polygon Rendering entsprechen.

Zeilen Rendering-Regeln mit nicht-Antialiasing sind identisch mit denen für [GDI-Zeilen](/windows/desktop/gdi/lines).

## <a name="span-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Punkt Sprite-Regeln


Punkt Sprite und patchprimitiver werden so rasterisiert, als ob die primitiven zuerst in Dreiecke und die resultierenden Dreiecke in Dreiecke unterteilt wurden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

[Rasterizerphase (RS)](rasterizer-stage--rs-.md)

[Regeln für die Rasterung](rasterization-rules.md)

 

 