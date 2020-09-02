---
title: Umgebungslicht
description: Erfahren Sie, wie Umgebungsbeleuchtung eine konstante Beleuchtung für eine Szene bietet, und erfahren Sie, wie Sie Ambient-Beleuchtung in Direct3D mithilfe von C++ festlegen.
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- Umgebungslicht
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c21a674b0961836752c879bcea681b568f31053c
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304462"
---
# <a name="ambient-lighting"></a>Umgebungslicht

Ambient-Beleuchtung bietet eine konstante Beleuchtung für eine Szene. Alle Objekt Scheitel Punkte werden gleich angezeigt, da Sie nicht von anderen Beleuchtungs Faktoren abhängen, wie z. b. Vertex-normalen, Lichtrichtung, Lichtposition, Bereich oder Dämpfung. Ambient-Beleuchtung ist in allen Richtungen konstant und färbt alle Pixel eines Objekts gleich. Es ist schnell zu berechnen, die Objekte werden jedoch flach und unrealistisch angezeigt.

Ambient-Beleuchtung ist die schnellste Art von Beleuchtung, liefert aber die wenigsten realistischen Ergebnisse. Direct3D enthält eine einzelne globale Ambient-Light-Eigenschaft, die Sie verwenden können, ohne dass ein Licht erstellt werden kann. Alternativ können Sie ein beliebiges helles Objekt festlegen, um Umgebungsbeleuchtung bereitzustellen.

Die Umgebungsbeleuchtung für eine Szene wird durch die folgende Gleichung beschrieben.

Ambient lighting = c ₐ \* \[ g ₐ + Sum (Atten<sub>i</sub> \* Spot<sub>i</sub> \* L<sub>AI</sub>)\]

Hierbei gilt:

| Parameter         | Standardwert | type          | BESCHREIBUNG                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| C ₐ                | (0, 0, 0, 0)     | D3DCOLORVALUE | Material Ambient-Farbe                                                                                            |
| G ₐ                | (0, 0, 0, 0)     | D3DCOLORVALUE | Globale Ambient-Farbe                                                                                              |
| Atten<sub>i</sub> | (0, 0, 0, 0)     | D3DCOLORVALUE | Helle Dämpfung des ITH-Lichts. Siehe [Dämpfung und Spotlight-Faktor](attenuation-and-spotlight-factor.md). |
| Stelle<sub>ich</sub>  | (0, 0, 0, 0)     | D3DVECTOR     | Der Spotlight-Faktor des ITH-Lichts. Siehe [Dämpfung und Spotlight-Faktor](attenuation-and-spotlight-factor.md).  |
| Sum               | Nicht zutreffend           | Nicht zutreffend           | Summe der Umgebungsbeleuchtung                                                                                          |
| L<sub>AI</sub>    | (0, 0, 0, 0)     | D3DVECTOR     | Helle Ambient-Farbe des ITH-Lichts                                                                              |

 

Der Wert für c ₐ lautet wie folgt:

-   Vertex color1, wenn AmbientMaterialSource = D3DMCS \_ color1 und die erste Scheitelpunkt Farbe in der Vertex-Deklaration angegeben wird.
-   Vertex color2, wenn AmbientMaterialSource = D3DMCS \_ color2 und die zweite Scheitelpunkt Farbe in der Scheitelpunkt Deklaration angegeben wird.
-   Material Ambient-Farbe.

**Hinweis**    Wenn eine der beiden Optionen "AmbientMaterialSource" verwendet wird und die Scheitelpunkt Farbe nicht angegeben wird, wird die Umgebungs Farbe für das Material verwendet.

 

Verwenden Sie setmaterial, um die Material Ambient-Farbe zu verwenden, wie im folgenden Beispielcode gezeigt.

G ₐ ist die globale Umgebungs Farbe. Sie wird mithilfe von setrenderstate (D3DRS \_ AMBIENT) festgelegt. In einer Direct3D-Szene gibt es eine globale Umgebungs Farbe. Dieser Parameter ist keinem Direct3D Light-Objekt zugeordnet.

L<sub>AI</sub> ist die Ambiente-Farbe des ITH-Lichts in der Szene. Jedes Direct3D Light verfügt über eine Reihe von Eigenschaften, von denen eine die Umgebungs Farbe ist. Der Begriff Sum (L<sub>AI</sub>) ist eine Summe aller Ambient-Farben in der Szene.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


In diesem Beispiel wird das-Objekt mithilfe der Szene Ambient Light und einer Material Ambient-Farbe gefärbt.

```cpp
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

Gemäß der Gleichung ist die resultierende Farbe für die Objekt Vertices eine Kombination aus der Material Farbe und der hellen Farbe.

Die beiden folgenden Abbildungen zeigen die Material Farbe, die grau ist, und die helle Farbe, die hell rot ist.

![Abbildung einer grauen Kugel](images/amb1.jpg)![Abbildung einer roten Kugel](images/lightred.jpg)

Die resultierende Szene wird in der folgenden Abbildung dargestellt. Das einzige Objekt in der Szene ist eine Kugel. Ambient Light beleuchtet alle Objekt Scheitel Punkte mit der gleichen Farbe. Er ist nicht abhängig von der Scheitelpunkt normalen oder der Lichtrichtung. Folglich sieht die Kugel wie ein 2D-Kreis aus, da es keinen Unterschied gibt, um die Oberfläche des Objekts zu schattieren.

![Abbildung einer Kugel mit Umgebungsbeleuchtung](images/lighta.jpg)

Um Objekten einen realistischeren Einblick zu verschaffen, wenden Sie zusätzlich zur Umgebungsbeleuchtung diffuse oder Glanzlichter an.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Beleuchtungsmathematik](mathematics-of-lighting.md)

 

 




