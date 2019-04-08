---
title: Bilineare Texturfilterung
description: Die bilineare Filterung berechnet den gewichteten Mittelwert der 4 Texel, die dem Sampling-Punkt am nächsten liegen.
ms.assetid: 0851AD28-8246-4547-A663-47884DDDFC3E
keywords:
- Bilineare Texturfilterung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 437650883b4782ca02c0daf24cc8ebed01d954f6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651255"
---
# <a name="bilinear-texture-filtering"></a>Bilineare Texturfilterung


Die *bilineare Filterung* berechnet den gewichteten Durchschnitt der 4 Texel, die dem Sampling-Punkt am nächsten liegen. Diese Filtermethode ist präziser und gängiger als das Filtern am nächstgelegenen Punkt. Dieser Ansatz ist effizient, da er in moderner Grafikhardware implementiert wird.


## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


Texturen werden immer linear behandelt, von (0,0, 0,0) in der oberen linken Ecke bis (1,0, 1,0) in der unteren rechten Ecke. Die lineare Texturadressierung wird in der folgenden Abbildung dargestellt.

![Abbildung einer 4 x 4-Textur mit einfarbigen Farbblöcken](images/bilinear-fig7a.png)

Texturen werden in der Regel dargestellt, als ob sie solide Farbblöcke bestehen wurden allerdings handelt es sich tatsächlich korrekter vorstellen Texturen die gleiche Weise sollten Sie die Raster-Anzeige vorstellen: Jede Texel wird in der genauen Mitte einer Zelle, in der definiert, wie in der folgenden Abbildung dargestellt.

![Abbildung einer 4 × 4-Textur mit in der Mitte der Rasterzelle definierten Texels](images/bilinear-fig7b.png)

Wenn Sie den Texturen-Sampler nach der Farbe dieser Textur in UV-Koordinaten fragen, (0,375, 0,375) erhalten Sie die Volltonfarbe Rot (255, 0, 0). Das ergibt daher Sinn, da die Mitte der roten Texel-Zelle bei UV (0,375, 0,375) liegt. Was aber, wenn Sie den Sampler nach der Texturfarbe auf UV (0,25, 0,25) fragen? Dies ist nicht einfach, da der Punkt auf UV (0,25, 0,25) genau an der Grenze von 4 Texeln liegt.

Die einfachste Methode ist hier, dass der Sampler die Farbe des nächstliegenden Texel zurück gibt. Dies wird als Punktfilterung bezeichnet (Infos finden Sie unter [Sampling am nächstgelegenen Punkt](nearest-point-sampling.md)). Dies ist generell aufgrund von körnigen oder „eckigen” Ergebnisse unerwünscht. Beim Filtern am nächstgelegenen Punkt stellen Punktsamples der Textur bei UV (0,25, 0,25) ein ganz anderes Problem dar: es gibt vier Texel mit dem gleichen Abstand vom Samplingpunkt, daher gibt es kein einziges naheliegendes Texel. Eines dieser vier Texel wird als die zurückgegebene Farbe ausgewählt, doch die Auswahl hängt der Abrundung der Koordinate ab, was störende Artefakte einbringen kann (lesen Sie den Artikel „Sampling am nächstgelegenen Punkt” in SDK).

Eine etwas genauere und häufiger verwendete Filtermethode ist das Berechnen des gewichteten Mittelwerts der 4 Texel, die dem Samplingpunkt am nächsten liegen. Dies wird als *Bilineare Filterung* bezeichnet. Der zusätzliche Rechenaufwand für die bilineare Filterung ist in der Regel unerheblich, da diese Routine in moderner Grafikhardware verwendet wird. Hier sind die Farben, die wir an verschiedenen Sampling-Punkten mit bilinearer Filterung erhalten:

```
UV: (0.5, 0.5)
```

Dieser Punkt liegt genau an der Grenze zwischen roten, grünen, blauen und weißen Texel. Die vom Sampler zurückgegebene Farbe ist Grau:

```
  0.25 * (255, 0, 0)
  0.25 * (0, 255, 0) 
  0.25 * (0, 0, 255) 
## + 0.25 * (255, 255, 255) 
------------------------
= (128, 128, 128)
```

```
UV: (0.5, 0.375)
```

Dieser Punkt liegt im Mittelwert des Grenzpunkts zwischen roten und grünen Texel. Die vom Sampler zurückgegebene Farbe ist gelb-grau (beachten Sie, dass der blaue und weiße Texel auf 0 skaliert werden):

```
  0.5 * (255, 0, 0)
  0.5 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (128, 128, 0)
```

```
UV: (0.375, 0.375)
```

Dies ist die Adresse des rote Texels in der zurückgegebene Farbe (alle anderen Texel in der Filterberechnung werden auf 0 angepasst):

```
  1.0 * (255, 0, 0)
  0.0 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (255, 0, 0)
```

Vergleichen Sie diese Berechnungen mit der folgenden Abbildung, auf der angezeigt wird, was geschieht, wenn die bilineare Filterberechnung bei jeder Textur auf der 4 x 4-Textur durchgeführt wird.

![Abbildung einer 4 x 4-Textur mit bilinearer Filterung, die bei jeder Textur-Adresse ausgeführt wird](images/bilinear-fig7c.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturfilterung](texture-filtering.md)

 

 




