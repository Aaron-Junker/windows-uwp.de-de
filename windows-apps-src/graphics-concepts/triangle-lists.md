---
title: Dreieckslisten
description: "Eine Dreiecksliste ist eine Liste isolierter Dreiecke. Die isolierten Dreiecke können sich nahe beieinander befinden oder nicht. Eine Dreiecksliste muss mindestens drei Scheitelpunkte enthalten, und die Gesamtzahl der Scheitelpunkte muss durch drei teilbar sein."
ms.assetid: BC50D532-9E9C-4AAE-B466-9E8C4AD1862A
keywords:
- Dreieckslisten
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3f04e1b830b2d82b0ddcc9ae08186680df404ea3
ms.lasthandoff: 02/07/2017

---

# <a name="triangle-lists"></a>Dreieckslisten


Eine Dreiecksliste ist eine Liste isolierter Dreiecke. Die isolierten Dreiecke können sich nahe beieinander befinden oder nicht. Eine Dreiecksliste muss mindestens drei Scheitelpunkte enthalten, und die Gesamtzahl der Scheitelpunkte muss durch drei teilbar sein.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


Verwenden Sie Dreieckslisten zur Erstellung eines Objekts, das aus nicht zusammenhängenden Teilen besteht. Beispielsweise ist eine Möglichkeit zur Erstellung einer Kraftfeldwand in einem 3D-Spiel eine umfangreiche Liste mit kleinen, nicht verbundenen Dreiecken. Wenden Sie dann ein Material und eine Struktur an, die den Eindruck erweckt, dass sie Licht zu der Dreiecksliste aussendet. Jedes Dreieck in der Wand leuchtet jetzt auf. Die Szene hinter der Wand wird teilweise durch die Lücken zwischen den Dreiecken sichtbar, was ein Spieler beim betrachten eines Kraftfelds auch erwartet.

Dreieckslisten sind auch nützlich für das Erstellen von Grundtypen, die scharfe Kanten haben und mit Gouraud-Schattierung versehen sind. Vgl. [Seiten- und Scheitelpunkt-Normalvektoren](face-and-vertex-normal-vectors.md).

Die folgende Abbildung zeigt eine gerenderte Dreiecksliste.

![Illustration einer gerenderten Dreiecksliste](images/trilist.png)

Der folgende Code zeigt, wie Scheitelpunkte für diese Dreiecksliste erstellt werden.

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

Im folgenden Codebeispiel wird veranschaulicht, wie Sie diese Dreiecksliste in Direct3D rendern.

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLELIST, 0, 2 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grundtypen](primitives.md)

 

 





