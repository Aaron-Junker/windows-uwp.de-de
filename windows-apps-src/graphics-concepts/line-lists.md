---
title: Zeilenlisten
description: Bei einer Zeilenliste handelt es sich um eine Liste isolierter Zeilensegmente. Zeilenlisten sind hilfreich für Aufgaben wie das Hinzufügen von Schneeregen oder Starkregen zu einer 3D-Szene. Anwendungen erstellen eine Zeilenliste, indem Sie eine Reihe von Vertizes ausfüllen.
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- Zeilenlisten
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac66066a4140ace5905ff6bc52a7b1290341beea
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291585"
---
# <a name="line-lists"></a>Zeilenlisten

Bei einer Zeilenliste handelt es sich um eine Liste isolierter Zeilensegmente. Zeilenlisten sind hilfreich für Aufgaben wie das Hinzufügen von Schneeregen oder Starkregen zu einer 3D-Szene. Anwendungen erstellen eine Zeilenliste, indem Sie eine Reihe von Vertizes ausfüllen. Beachten Sie, dass die Anzahl der Vertizes in einer Zeilenliste eine gerade Zahl größer oder gleich 2 sein muss.

-   [Beispiel](#example)
-   [Verwandte Themen](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


Die folgende Abbildung zeigt eine gerenderte Zeilenliste.

![Abbildung einer Zeilenliste](images/linelst.png)

Sie können einer Zeilenliste Materialien und Texturen zuweisen. Die Farben des Materials oder der Textur erscheinen nur entlang der gezogenen Zeilen, nicht an einer beliebigen Stelle zwischen den Zeilen.

Der folgende Code zeigt, wie Vertizes für diese Zeilenliste erstellt werden.

```cpp
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

Im folgenden Codebeispiel wird veranschaulicht, wie Sie eine Zeilenliste in Direct3D rendern.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Primitive Typen](primitives.md)

 

 




