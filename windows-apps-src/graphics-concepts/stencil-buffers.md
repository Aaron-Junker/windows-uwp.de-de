---
title: Schablonenpuffer
description: Mit einem Schablonenpuffer werden Pixel in einem Bild maskiert, um Spezialeffekte zu erzeugen.
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- Schablonenpuffer
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd59c1d32b4f09b58b7e78281e468fbb00a777d9
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291878"
---
# <a name="stencil-buffers"></a>Schablonenpuffer

Ein *Schablonenpuffer* wird verwendet, um die Pixel in einem Bild zu maskieren und Spezialeffekte zu erzeugen. Die Maske steuert, ob das Pixel gezeichnet wird oder nicht. Diese Spezialeffekte umfassen Zusammensetzungen, Auflösungen, Rasterungen, Ein- und Ausblendungen, Wischungen, Umrisse und Silhouetten sowie das Feature „zweiseitige Schablone”. Einige der häufigeren Effekte sind unten dargestellt.

Der Schablonenpuffer aktiviert oder deaktiviert das Zeichnen auf der Renderzieloberfläche pixelweise. Auf der Basisebene ermöglicht der Schablonenpuffer, dass Anwendungen Abschnitte des gerenderten Bilds maskieren, damit sie nicht angezeigt werden. Anwendungen verwenden Schablonenpuffer häufig für Spezialeffekte wie Auflösungen, Rasterungen und Umrisse.

Informationen des Schablonenpuffers sind in die Z-Pufferdaten eingebettet.

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>Funktionsweise von der Stencilpuffer

Direct3D führt einen pixelweisen Test der Inhalte des Schablonenpuffers durch. Für jedes Pixel in der Zieloberfläche wird ein Test mit dem entsprechenden Wert im Schablonenpuffer, einem Schablonenreferenzwert und einem Schablonenmaskenwert durchgeführt. Wenn der Test erfolgreich ist, führt Direct3D eine Aktion aus. Der Test wird in den folgenden Schritten ausausgeführt:

1.  Durchführen einer bitweisen AND-Operation des Schablonenreferenzwerts mit der Schablonenmaske.
2.  Durchführen einer bitweisen AND-Operation des Schablonenpufferwerts für das aktuelle Pixel mit der Schablonenmaske.
3.  Vergleichen des Ergebnisses von Schritt 1 mit dem Ergebnis von Schritt 2 anhand der Vergleichsfunktion.

Die oben aufgeführten Schritte sind in der folgenden Codezeile dargestellt:

```cpp
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef stellt den Schablonenreferenzwert dar.
-   StencilMask stellt den Wert der Schablonenmaske dar.
-   CompFunc ist die Vergleichsfunktion.
-   StencilBufferValue ist der Inhalt des Schablonenpuffers für das aktuelle Pixel.
-   Das kaufmännische Und-Zeichen (&) stellt die bitweise AND-Operation dar.

Wenn der Schablonentest erfolgreich ist, wird das aktuelle Pixel auf die Zieloberfläche geschrieben, und andernfalls ignoriert. Das Standardverhalten des Vergleichs ist, das Pixel unabhängig vom Ergebnis der bitweisen Operationen zu schreiben. Sie können dieses Verhalten ändern, indem Sie den Wert eines Aufzählungstyps ändern, um die gewünschte Vergleichsfunktion zu ermitteln.

Sie können in Ihrer Anwendung die Funktionsweise des Schablonenpuffers anpassen. In der Anwendung kann die Vergleichsfunktion, die Schablonenmaske und der Schablonenreferenzwert festgelegt werden. Es kann auch die Aktion gesteuert werden, die Direct3D im Falle eines erfolgreichen oder eines misslungenen Schablonentests ausführt.

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Zusammensetzung


Ihre Anwendung kann die Schablonenpuffer verwenden, um 2D- oder 3D-Bilder mit einer 3D-Szene zu kombinieren. Auf der Renderzieloberfläche wird mit einer Maske in dem Schablonenpuffer ein Bereich verdeckt. Gespeicherte 2D-Informationen, z. B. Text oder Bitmaps, können dann in den verdeckten Bereich geschrieben werden. Alternativ kann die Anwendung weitere 3D-Grundtypen auf den durch die Schablone maskierten Bereich der Renderzieloberfläche rendern. Es kann sogar eine ganze Szene gerendert werden.

In Spielen sind häufig mehrere 3D-Szenen zusammengesetzt. Zum Beispiel gibt es in Fahrspielen normalerweise einen Rückspiegel. Im Spiegel ist die Ansicht der 3D-Szene hinter dem Fahrer sichtbar. Das ist im Grunde eine zweite 3D-Szene, die mit der Sicht des Fahrers nach vorne zusammengesetzt ist.

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Decaling


Direct3D-Anwendungen steuern über die Rasterung, welche Pixel aus einem bestimmten Grundtypbild auf die Renderzieloberfläche gezeichnet werden. Anwendungen wenden Rasterungen für die Bilder der Grundtypen an, um komplanare Polygone richtig zu rendern.

Zum Beispiel sollten für die Darstellung von Reifenspuren und gelben Linien auf einer Straße die Reifenspuren direkt auf der Straße angezeigt werden. Die Z-Werte für die Reifenspuren und die Straße sind aber gleich. Der Tiefenpuffer kann deshalb eventuell keine klare Trennung zwischen den beiden erzeugen. Einige Pixel im Hintergrund-Grundtyp werden möglicherweise auf dem Vordergrund-Grundtyp (und umgekehrt) gerendert. Das resultierende Bild scheint von Frame zu Frame zu schimmern. Dieser Effekt wird *Z-Konflikt* oder *Flimmern* genannt.

Um dieses Problem zu beheben, verwenden Sie eine Schablone, mit der der Abschnitt des Hintergrund-Grundtyps maskiert wird, an dem die Rasterung angezeigt wird. Deaktivieren Sie die Z-Pufferung, und rendern Sie das Bild des Vordergrund-Grundtyps in den maskierten Bereich der Renderzieloberfläche.

Dieses Problem könnte auch durch mehrere Texturvermischungen behoben werden, aber dies würde die Anzahl der anderen Spezialeffekte, die Ihre Anwendung erzeugen kann, einschränken. Durch die Verwendung des Schablonenpuffers für Rasterungen werden Texturvermischungsphasen für andere Effekte freigegeben.

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>Szenenwechsel wird ausgeblendet und Kundenkarte


Immer mehr Anwendungen setzen Spezialeffekte ein, die häufig in Filmen und Videos verwendet werden, wie Auflösungen, Wischungen sowie Ein- und Ausblendungen.

Bei einer Auflösung wird ein Bild allmählich durch ein anderes in einer übergangslosen Framesequenz ersetzt. Direct3D stellt zwar Methoden für die Verwendung mehrerer Texturvermischungen bereit, um den gleichen Effekt zu erzielen, aber Anwendungen, die den Schablonenpuffer für Auflösungen verwenden, können die Texturvermischungsfunktionen bei einer Auflösung für andere Effekte nutzen.

Wenn Ihre Anwendung eine Auflösung durchführt, muss sie zwei verschiedene Bilder rendern. Die Anwendung steuert mit dem Schablonenpuffer, welche Pixel aus jedem Bild auf die Renderzieloberfläche gezeichnet werden. Sie können eine Reihe von Schablonenmasken definieren und sie in den Schablonenpuffer in aufeinanderfolgende Frames kopieren. Alternativ können Sie eine Basisschablonenmaske für den ersten Frame definieren und diese inkrementell ändern.

Am Anfang der Auflösung legt die Anwendung die Schablonenfunktion und die Schablonenmaske fest, sodass der Schablonentest für die meisten Pixel aus dem Startbild erfolgreich ist. Der Schablonentest für die meisten Pixel aus dem Endbild sollte nicht erfolgreich sein. In aufeinanderfolgenden Frames wird die Schablonenmaske aktualisiert, sodass der Test für immer weniger Pixel aus dem Startbild erfolgreich ist. Mit jedem weiteren Frame ist der Test für immer weniger Pixel aus dem Endbild nicht erfolgreich. Auf diese Weise kann die Anwendung mit jedem beliebigen Auflösungsmuster eine Auflösung durchführen.

Einblendungen oder Ausblendungen sind ein Sonderfälle der Auflösung. Beim Einblenden wird der Schablonenpuffer verwendet, um ein schwarzes oder weißes Bild in ein Rendering einer 3D-Szene aufzulösen. Ausblenden ist das Gegenteil. Die Anwendung beginnt mit einem Rendering einer 3D-Szene und löst sie in Schwarz oder Weiß auf. Ein- oder Ausblenden kann mit jedem beliebigen Muster, das Sie verwenden möchten, durchgeführt werden.

Direct3D-Anwendungen verwenden eine ähnliche Technik für Wischungen. Wenn eine Anwendung zum Beispiel eine Wischung von links nach rechts durchführt, gleitet das Endbild nach und nach von links nach rechts auf das Startbild. Wie bei einer Auflösung müssen Sie eine Reihe von Schablonenmasken definieren, die in aufeinanderfolgenden Frames in den Schablonenpuffer geladen werden, oder fortlaufend die erste Schablonenmaske ändern. Die Schablonenmasken werden verwendet, um das Schreiben von Pixeln aus dem Startbild zu deaktivieren und das Schreiben von Pixeln aus dem Endbild zu aktivieren.

Eine Wischung ist etwas komplexer als eine Auflösung, da die Anwendung Pixel aus dem Endbild in umgekehrter Reihenfolge der Wischung lesen muss. Das heißt, wenn sich die Wischung von links nach rechts bewegt, muss die Anwendung Pixel aus dem Endbild von rechts nach links lesen.

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>Beschreibt und Silhouetten


Sie können den Schablonenpuffer für abstraktere Effekte, wie Umrisse und Silhouetten, verwenden.

Wenn die Anwendung eine Schablonenmaske mit der gleichen, aber etwas kleineren Form auf das Bild eines Grundtyps anwendet, enthält das resultierende Bild nur den Umriss des Grundtyps. Die Anwendung kann dann den durch die Schablone maskierten Bereich des Bilds mit einer Volltonfarbe ausfüllen. Der Grundtyp erhält so ein geprägtes Aussehen.

Wenn die Schablonenmaske die gleiche Größe und Form wie der Grundtyp hat, den Sie rendern, enthält das resultierende Bild an der Stelle eine Lücke, an der sich der Grundtyp befinden sollte. Die Anwendung kann die Lücke mit Schwarz ausfüllen, um eine Silhouette des Grundtyps zu erstellen.

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>Zweiseitigen Schablone


Zum Zeichnen von Schatten mit dem Schablonenpuffer werden Schattenvolumen verwendet. Die Anwendung berechnet die Schattenvolumenumwandlung durch verdeckende Geometrie, indem die Ränder der Silhouette berechnet und aus dem Licht in eine Gruppe von 3D-Volumen extrudiert werden. Diese Volumen werden dann zweimal in den Schablonenpuffer gerendert.

Beim ersten Rendern werden die nach vorne gerichteten Polygone gezeichnet und die Werte im Schablonenpuffer erhöht. Beim zweiten Rendern werden die nach hinten gerichteten Polygone des Schattenvolumens gezeichnet und die Werte im Schablonenpuffer verringert. In der Regel heben sich alle erhöhten oder verringerten Werte gegenseitig auf. Da die Szene aber bereits mit normaler Geometrie gerendert wurde, ist der Z-Puffer-Test für einige Pixel beim Rendern des Schattenvolumens nicht erfolgreich. Werte, die im Schablonenpuffer verblieben sind, entsprechen den Pixeln, die sich im Schatten befinden. Dieser verbliebene Inhalt des Schablonenpuffers wird als Maske verwendet, um ein großes, allumfassendes schwarzes Viereck in der Szene per Alpha-Überblendung zu überlagern. Das Ergebnis der Verwendung des Schablonenpuffers als Maske besteht darin, dass Pixel, die im Schatten liegen, dunkler werden.

Dies bedeutet, dass die Schattengeometrie zweimal pro Lichtquelle gezeichnet wird, und somit der Vertexdurchsatz der GPU stark belastet wird. Das Feature „zweiseitige Schablone” wurde entwickelt, um diese Situation abzuschwächen. In diesen Ansatz gibt es zwei Gruppen von Schablonenzuständen, einen für die nach vorne gerichteten Dreiecke und den anderen für die nach hinten gerichteten Dreiecke. Auf diese Weise wird nur ein einziger Durchlauf pro Schattenvolumen und pro Licht gezeichnet.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Tiefen- und Schablonenpuffer](depth-and-stencil-buffers.md)

 

 




