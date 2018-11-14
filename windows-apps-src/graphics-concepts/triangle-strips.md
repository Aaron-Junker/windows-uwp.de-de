---
title: Dreieckstreifen
description: Ein Dreiecksstrip ist eine Reihe verbundener Dreiecke. Da die Dreiecke verbunden sind, muss die Anwendung nicht alle drei Scheitelpunkte für jedes Dreieck wiederholt angeben.
ms.assetid: BACC74C5-14E5-4ECC-9139-C2FD1808DB82
keywords:
- Dreieckstreifen
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f60f0f65868d4dec67bf77a329d4b952c20ec44a
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "6649197"
---
# <a name="triangle-strips"></a>Dreieckstreifen


Ein Dreiecksstrip ist eine Reihe verbundener Dreiecke. Da die Dreiecke verbunden sind, muss die Anwendung nicht alle drei Scheitelpunkte für jedes Dreieck wiederholt angeben. So benötigen Sie beispielsweise nur sieben Scheitelpunkte zur Definition des folgenden Dreieckstreifens.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


![Illustration eins Dreieckstreifens mit sieben Scheitelpunkten](images/tristrip.png)

Das System verwendet die Scheitelpunkte v1, v2 und v3 zum Zeichnen des ersten Dreiecks, v2, v4 und v3 für das zweite v3, v4 und v5 für das dritte. v4, v6 und v5 für das vierte Dreieck und so weiter. Beachten Sie, dass die Scheitelpunkte des zweiten und vierten Dreiecke die Reihenfolge verletzen. Dies ist notwendig, um sicherzustellen, dass alle Dreiecke im Uhrzeigersinn gezeichnet werden.

Die meisten Objekte in 3D-Szenen bestehen aus Dreieckstreifen. Der Grund dafür ist, dass mit Dreieckstreifen komplexe Objekte so zusammengesetzt werden können, dass Speicherplatz und Verarbeitungszeit dabei effizient genutzt werden.

Die folgende Abbildungzeigt einen gerenderte Dreieckstreifen.

![Illustration eines gerenderten Dreieckstreifens](images/tstrip2.png)

Der folgende Code zeigt, wie Scheitelpunkte für diesen Dreieckstreifen erstellt werden.

```
struct CUSTOMVERTEX
{
float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

Im folgenden Codebeispiel wird veranschaulicht, wie Sie diesen Dreieckstreifen in Direct3D rendern.

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLESTRIP, 0, 4);
```

## <a name="span-idpolygonsspanspan-idpolygonsspanspan-idpolygonsspanpolygons"></a><span id="Polygons"></span><span id="polygons"></span><span id="POLYGONS"></span>Vielecke


Häufig werden Dreieckstreifen verwendet, um Vielecke zu erstellen. Ein Vieleck ist eine geschlossene 3D-Figur, die von mindestens drei Scheitelpunkten begrenzt wird. Das einfachste Vieleck ist ein Dreieck. Microsoft Direct3D verwendet Dreiecke zur Erstellung der meisten Vielecke, da alle drei Scheitelpunkte in einem Dreieck garantiert auf der gleichen Ebene liegen. Das Rendern von nicht-planaren Scheitelpunkten ist ineffizient. Sie können Dreiecke zu großen, komplexen Vielecken und Gittern kombinieren.

Die folgende Abbildung zeigt einen Würfel. Je zwei Dreiecke bilden die einzelnen Flächen des Würfels. Der gesamte Satz von Dreiecken bildet einen kubischen Grundtyp. Sie können Texturen auf den Oberflächen von Grundtypen anwenden, um dafür zu sorgen, dass sie wie eine einzige massive Form wirken. Weitere Informationen finden Sie unter [Texturen](textures.md).

![Illustration eines Würfels mit zwei Dreiecken auf jeder Seite](images/cube3d.png)

Sie können mit Dreiecken auch Grundtypen erstellen, deren Oberflächen wie glatte Kurven aussehen. Die folgende Abbildungzeigt die Simulation einer Kugel mithilfe von Dreiecken. Nach der Anwendung eines Materials kann die Kugel beim Rendern so gestaltet werden, dass sie rund wirkt.

![Illustration einer mithilfe von Dreiecken simulierten Kugel](images/sphere3d.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grundtypen](primitives.md)

 

 




