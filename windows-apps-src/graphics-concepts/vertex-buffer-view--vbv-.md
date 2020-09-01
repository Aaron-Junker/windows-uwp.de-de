---
title: Vertexpuffer- (VBV) und Indexpufferansicht (IBV)
description: Erfahren Sie mehr über die Vertex-Puffer Ansicht (VBV) und die Index Puffer Ansicht (IBV), die Daten und ganzzahlige Indizes für Vertices in Direct3D Rendering enthalten.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- Vertex-Puffer Ansicht (VBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a616f2bad8f478b2d20e96b183ba944950fef8a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156094"
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>Vertexpuffer- (VBV) und Indexpufferansicht (IBV)


Ein Vertex-Puffer enthält Daten für eine Liste von Vertices. Die Daten für jeden Scheitelpunkt können Position, Farbe, normaler Vektor, Texturkoordinaten usw. enthalten. Ein Index Puffer enthält ganzzahlige Indizes (Offsets) in einem Scheitelpunkt Puffer und wird verwendet, um ein Objekt zu definieren und zu Rendering, das aus einer Teilmenge der vollständigen Liste von Vertices besteht.

Die Definition eines einzelnen Scheitel Punkts liegt häufig in der Definition der zu definierenden Anwendung vor, wie z. b.:

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

Die Definition von CustomVertex wird dann beim Erstellen von Scheitelpunkt Puffern an den Grafiktreiber weitergegeben.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ansichten](views.md)

 

 




