---
title: Lichteigenschaften
description: Lichteigenschaften beschreiben die Art (Punktlicht, gerichtetes Licht, Spotlight), Dämpfung, Farbe, Richtung, Position und Reichweite einer Lichtquelle.
ms.assetid: E832C3FD-9921-41C4-87B8-056E16B61B77
keywords:
- Lichteigenschaften
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 86b8627461251a5d43762facc18c8a414a117fc9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604125"
---
# <a name="light-properties"></a>Lichteigenschaften


Lichteigenschaften beschreiben die Art (Punktlicht, gerichtetes Licht, Spotlight), Dämpfung, Farbe, Richtung, Position und Reichweite einer Lichtquelle. Abhängig von der verwendeten Lichtart, kann ein Licht über Eigenschaften für Dämpfung und Reichweite oder für Spotlight-Effekte verfügen. Nicht alle Lichtarten verwenden alle Eigenschaften.

Die Eigenschaften Position, Reichweite und Dämpfung definieren die Position eines Lichts im Weltraum und wie sich das abgestrahlte Licht über die Entfernung verhält.

## <a name="span-idlightattenuationspanspan-idlightattenuationspanspan-idlightattenuationspanlight-attenuation"></a><span id="Light_Attenuation"></span><span id="light_attenuation"></span><span id="LIGHT_ATTENUATION"></span>Lichtdämpfung


Die Dämpfung steuert, wie die Intensität eines Lichts gegenüber der maximalen Entfernung, angegeben durch die Eigenschaft „Reichweite“, abnimmt. Drei für Gleitkommawerte werden manchmal verwendet, um lichtdämpfung darzustellen: Attenuation0 Attenuation1 und Attenuation2. Diese Gleitkommawerte reichen von 0,0 bis unendlich und steuern die Lichtdämpfung. Einige Anwendungen stellen den Wert Dämpfung1 auf 1,0 und die anderen Werte, aufgrund der sich ändernden Lichtintensität bei 1 / D, wobei D die Entfernung der Lichtquelle zum Scheitelpunkt darstellt, auf 0,0 ein. Die maximale Lichtintensität befindet sich an der Quelle und nimmt nach 1 / (Leuchtweite) bei der Reichweite des Lichts ab.

Deshalb stellt eine Anwendung den Wert Dämpfung0 in der Regel auf 0,0, den Wert Dämpfung1 auf einen konstanten Wert und Dämpfung2 auf 0,0 ein. Variierende Lichteffekte können durch Änderung der Werte erreicht werden. Sie können die Dämpfungswerte kombinieren, um komplexere Dämpfungseffekte zu erzielen. Sie können die Werte auch außerhalb des Normalbereichs einstellen, um noch außergewöhnlichere Dämpfungseffekte zu erzielen. Negative Dämpfungswerte sind jedoch nicht zulässig. Siehe [Abschwächungs- und Spotlicht-Faktor](attenuation-and-spotlight-factor.md).

## <a name="span-idlightcolorspanspan-idlightcolorspanspan-idlightcolorspanlight-color"></a><span id="Light_Color"></span><span id="light_color"></span><span id="LIGHT_COLOR"></span>Helle Farbe


Licht in Direct3D strahlt drei Farben ab, die unabhängig voneinander in der Lichtberechnung des Systems verwendet werden: diffuse Farbe, Umgebungsfarbe und Glanzfarbe. Alle drei Farben sind im Direct3D-Lichtmodul integriert und interagieren mit einem Gegenwert des aktuellen Materials, um eine finale, für das Rendering verwendete Farbe zu erzeugen. Die diffuse Farbe interagiert mit der Eigenschaft „diffuse Reflektion“ des aktuellen Materials, die Glanzfarbe mit der Eigenschaft „Glanzreflektion“ des Materials usw. Einzelheiten darüber, wie Direct3D diese Farben anwendet, finden Sie unter [Beleuchtungsmathematik](mathematics-of-lighting.md).

In eine Direct3D-Anwendung gibt es in der Regel drei Farbwerte, Diffus, Umgebung und Glanz, die die abgestrahlte Farbe definieren.

Die Farbart, die dem System die größte Rechenleistung abverlangt, ist die diffuse Farbe. Die gängigste diffuse Farbe ist weiß ((R:1.0 G:1.0 B:1.0). Sie können Farben aber ganz nach ihren Anforderungen erstellen, um die gewünschten Effekte zu erzielen. Sie können beispielsweise rotes Licht für einen Kamin oder grünes Licht für eine Ampel verwenden.

Normalerweise stellen Sie die Lichtfarbenkomponenten auf Werte zwischen einschließlich 0,0 und 1,0 ein. Dies ist aber keine Anforderung. Sie können beispielsweise alle Komponenten auf 2,0 einstellen und damit ein Licht erzeugen, das „heller als weiß“ ist. Diese Art der Einstellung kann besonders hilfreich sein, wenn Sie die Dämpfungseinstellung nicht mit konstanten Werten verwenden.

Obwohl Direct3D die RGBA-Werte für Licht verwendet, wird die Alpha-Farbkomponente nicht verwendet.

Normalerweise werden Materialfarben für die Beleuchtung verwendet. Sie können jedoch einstellen, dass die Materialfarben, Emissiv, Umgebung, Diffus und Glanz, von diffusen oder Glanz-Scheitelpunktfarben überschrieben werden.

Der Alpha-/Transparenzwert kommt immer nur vom Alphakanal der diffusen Farbe.

Der Nebelwert kommt immer nur vom Alphakanal der Glanzfarbe.

## <a name="span-idlightdirectionspanspan-idlightdirectionspanspan-idlightdirectionspanlight-direction"></a><span id="Light_Direction"></span><span id="light_direction"></span><span id="LIGHT_DIRECTION"></span>Lichtrichtung


Die Eigenschaft „Richtung“ eines Lichts legt die Richtung des vom Objekt abgestrahlten Lichts im Weltraum fest. „Richtung“ wird nur für gerichtetes Licht und Spotlights verwendet, und wird mit einem Vektor beschrieben.

Stellen Sie die Lichtrichtung als Vektor ein. Richtungsvektoren werden als Entfernungen von einem logischen Ursprung beschrieben, ohne Berücksichtigung der Position eines Lichts in einer Szene. Deshalb hat ein gerade auf eine Szene gerichtetes Spotlight entlang der positiven Z-Achse einen Richtungsvektor von &lt;0,0,1&gt;, egal wo seine Position definiert wird. Ähnlich können Sie Sonnenlicht direkt auf eine Szene simulieren, indem Sie gerichtetes Licht mit der Richtung &lt;0,-1,0&gt; verwenden. Sie müssen kein Licht erstellen, das entlang der Koordinatenachsen leuchtet. Sie können Werte mischen und abstimmen, um Licht zu erzeugen, dass die interessanten Stellen ausleuchtet.

Obwohl Sie den Richtungsvektor eines Lichts nicht normalisieren müssen, sollten Sie immer sicherstellen, dass eine Größe angegeben ist. Mit anderen Worten, verwenden Sie niemals einen Richtungsvektor mit den Werten &lt;0,0,0&gt;.

## <a name="span-idlightpositionspanspan-idlightpositionspanspan-idlightpositionspanlight-position"></a><span id="Light_Position"></span><span id="light_position"></span><span id="LIGHT_POSITION"></span>Position der Lichtquelle


Die Lichtposition wird mithilfe einer Vektorstruktur beschrieben. Es wird angenommen, dass sich die X-, Y- und Z-Koordinaten im Weltraum befinden. Gerichtetes Licht ist die einzige Lichtart, für die die Eigenschaft „Position“ nicht verwendet wird.

## <a name="span-idlightrangespanspan-idlightrangespanspan-idlightrangespanlight-range"></a><span id="Light_Range"></span><span id="light_range"></span><span id="LIGHT_RANGE"></span>Light-Bereich


Mit der Eigenschaft „Reichweite“ des Lichts wird die Entfernung im Weltraum bestimmt, bei der Gitter in einer Szene nicht länger vom Licht beleuchtet werden, das von einem Objekt abgestrahlt wird. Für gerichtetes Licht wird die Eigenschaft „Reichweite“ nicht verwendet.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Lichter und Materialien](lights-and-materials.md)

 

 




