---
title: Regeln für die Rasterung
description: Die Regeln für die Rasterung legen fest, wie Vektordaten Rasterdaten zugeordnet werden.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- Regeln für die Rasterung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28376f6e96cc490c7c44f4ed60093591e873300e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371005"
---
# <a name="rasterization-rules"></a>Regeln für die Rasterung


Die Regeln für die Rasterung legen fest, wie Vektordaten Rasterdaten zugeordnet werden. Rasterdaten werden als Ganzzahl-Positionen angedockt, die dann ausgewählt und gekürzt werden (um eine minimale Anzahl der Pixel zu ergeben). Attribute pro Pixel werden interpoliert (von Attributen pro Scheitelpunkt), bevor diese an einen Pixel-Shader weitergeleitet werden.

Es gibt verschiedene Arten von Regeln, die vom Typ des Grundtyps abhängen, der zugeordnet wird, und davon, ob die Daten Multisampling verwenden, um Aliase zu reduzieren. Die folgenden Abbildungen veranschaulichen, wie die Ausnahmefälle behandelt werden.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>Dreieck Rasterization Regeln (ohne Multisampling)


Jedes Pixelzentrum innerhalb eines Dreiecks wird gezeichnet. Ein Pixel wird als innerhalb angenommen, wenn es die Links-oben-Regel erfüllt. Die Links-oben-Regel besagt, dass ein Pixelzentrum als innerhalb eines Dreiecks definiert wird, wenn es sich auf dem oberen oder linken Rand eines Dreiecks befindet.

Dabei gilt Folgendes:

-   Ein oberer Rand ist ein Rand, der genau horizontal und oberhalb der anderen Ränder liegt.
-   Ein linker Rand ist ein Rand, der nicht genau horizontal liegt und sich auf der linken Seite des Dreiecks befindet. Ein Dreieck kann einen oder zwei linke Rändern aufweisen.

Die Oben-links-Regel stellt sicher, dass angrenzende Dreiecke ein Mal gezeichnet werden.

Diese Abbildung zeigt Beispiele für Pixel, die gezeichnet werden, weil sie innerhalb eines Dreiecks liegen oder die Oben-links-Regel erfüllen.

![Beispiele für die Oben-Links-Dreiecksrasterung](images/d3d10-rasterrulestriangle.png)

Die hell- und dunkelgrauen Abdeckungen der Pixel zeigen sie als Pixelgruppen an, um anzugeben, in welchem Dreieck sie sich befinden.

## <a name="span-idline1spanspan-idline1spanspan-idline1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>Zeile Rasterization Regeln (ohne Multisampling Alias)


Regeln für die Linienrasterung verwenden einen rautenförmigen Testbereich, um zu bestimmen, ob eine Linie einen Pixel abdeckt. Für X-Major-Linien (Linien mit -1 &lt;= Neigung &lt;= +1) umfasst der rautenförmige Testbereich (ausgefüllt angezeigt) den unteren linken Rand, den unteren rechten Rand und die untere Ecke. Die Raute umfasst nicht (gepunktet angezeigt) den oberen linken Rand, den oberen rechten Rand, die obere Ecke, die linke Ecke und die rechte Ecke. Eine Y-Major-Linie ist jede Linie, die keine X-Major-Linie ist. Der rautenförmige Testbereich ist mit dem für die X-Major-Linie beschriebenen identisch, mit der Ausnahme, dass die rechte Ecke ebenfalls enthalten ist.

Im rautenförmigen Bereich deckt eine Linie einen Pixel ab, wenn die Linie den rautenförmigen Testbereich des Pixels verlässt, wenn sie vom Start in Richtung Ende daran entlang verläuft. Ein Linienstreifen verhält sich ebenfalls so, wenn er als Abfolge von Linien gezeichnet wird.

Die folgende Abbildung zeigt einige Beispiele.

![Beispiele für eine Linienrasterung mit Aliasing](images/d3d10-rasterrulesline.png)

## <a name="span-idline2spanspan-idline2spanspan-idline2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>Zeile Rasterization Regeln (ohne Multisampling geglättet)


Eine Linie mit Antialiasing ist wie ein Rechteck gerastert (mit Breite = 1). Das Rechteck überschneidet sich mit einem Renderziel und erzeugt Pro-Pixel-Abdeckungswerte, die in Pixel-Shader-Ausgabe-Alphakomponenten multipliziert werden. Es wird kein Antialiasing beim Zeichnen von Linien auf einem Renderziel mit Multisampling durchgeführt.

Es gilt, dass es keine einzelne "beste" Methode zum Durchführen eines Linienrenderings mit Antialiasing gibt. Direct3D verwendet als Richtlinie die in der folgenden Abbildung dargestellte Methode. Diese Methode wurde empirisch abgeleitet ist und zeigt eine Reihe von visuellen Eigenschaften, die als wünschenswert angesehen werden.

Hardware muss diesem Algorithmus nicht exakt entsprechen. Tests für diesen Verweis sollten "angemessene" Toleranzen haben, die auf einigen der weiter unten aufgelisteten Prinzipien basieren, sodass verschiedene Hardwareimplementierungen und Filter-Kernel-Größen ermöglicht werden. Diese bei der Hardwareimplementierung zulässige Flexibilität kann jedoch nicht über Direct3D zu Anwendungen kommuniziert werden. Es können lediglich Linien gezeichnet und ihr Aussehen beobachten/gemessen werden.

![Beispiele für eine Linienrasterung mit Antialiasing](images/d3d10-rasterruleslineaa.png)

Dieser Algorithmus generiert relativ weiche Linien mit einheitlicher Intensität und mit minimalen ausgefransten Rändern sowie minimalem Braiding. Moiremuster für abschließende Linien werden minimiert. Es gibt eine gute Abdeckung für Verbindungen zwischen Liniensegmenten, die End-to-End platziert werden.

Der Filter-Kernel ist ein angemessener Kompromiss zwischen der Weichzeichnung des Rands und den Intensitätsänderungen, die durch Gammakorrekturen verursacht werden. Der Abdeckungswert wird in Pixel-Shader o0.a (srcAlpha) durch die folgende Formel von der [OM-Phase (Output Merger)](output-merger-stage--om-.md) multipliziert :

SrcColor \* SrcAlpha + DestColor \* (1-SrcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>Zeigen Sie die von Rasterungsregeln (ohne Multisampling)


Ein Punkt wird so interpretiert, als würde er aus zwei Dreiecken in einem Z-Muster bestehen, die Regeln für die Dreiecksrasterung verwenden. Die Koordinate identifiziert die Mitte eines einen Pixel breiten Quadrats. Für Punkte gibt es kein Culling.

Die folgende Abbildung zeigt einige Beispiele.

![Beispiele für Punktrasterung](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>Für Multisampling von Anti-Aliasing-Rasterungsregeln


Multisample-Antialiasing (MSAA) reduziert Geometrie-Aliasing mit Pixelabdeckung und Tiefenschablonentests an mehreren untergeordneten Beispielpositionen. Zum Verbessern der Leistung werden Pro-Pixel-Berechnungen einmal für jedes abgedeckte Pixel ausgeführt, indem Shaderausgaben über abgedeckte Teilpixel hinweg gemeinsam genutzt werden. Multisample-Antialiasing reduziert nicht das Oberflächen-Aliasing. Beispielpositionen und Wiederherstellungsfunktionen hängen von der Hardwareimplementierung ab.

Die folgende Abbildung zeigt einige Beispiele.

![Beispiele für Multisample-Antialiasing-Rasterung](images/d3d10-rasterrulesmsaa.png)

Die Anzahl der Beispielpositionen ist vom Multisample-Modus abhängig. Vertexattribute werden in der Pixelmitte interpoliert, da dort der Pixel-Shader aufgerufen wird (dies wird zur Extrapolation, wenn die Mitte nicht abgedeckt ist). Attribute können im Pixel-Shader als mittig gesampelt markiert werden, sodass nicht abgedeckte Pixel das Attribut am Schnittpunkt zwischen dem Bereich und dem Grundtyp des Pixels interpolieren.

Ein Pixel-Shader wird für jeden 2x2-Pixelbereich ausgeführt, um abgeleitete Berechnungen (die x- und y-Deltas verwenden) zu unterstützen. Das heißt, dass Shader-Aufrufe mehr als gezeigt auftreten, um mindestens 2x2 Quanten (was unabhängig vom Multisampling ist) auszufüllen. Das Shader-Ergebnis wird für jedes abgedeckte Beispiel geschrieben, das den Pro-Beispiel-Tiefenschablonentest besteht.

Regeln für die Grundtypenrasterung werden normalerweise durch Multisample-Antialiasing nicht geändert. Ausnahmen:

-   Für ein Dreieck wird ein Abdeckungstest für jede Beispielposition (nicht für eine Pixelmitte) ausgeführt. Wenn mehr als eine Beispielposition abgedeckt wird, wird ein Pixel-Shader einmal mit Attributen ausgeführt, die in der Pixelmitte interpoliert werden. Das Ergebnis wird für jede abgedeckte Beispielposition im Pixel, der den Tiefenschablonentest besteht, gespeichert (repliziert).

    Eine Linie wird als ein Rechteck, das aus zwei Dreiecken besteht, mit einer Linienbreite von 1,4 behandelt.

-   Für einen Punkt wird ein Abdeckungstest für jede Beispielposition (nicht für eine Pixelmitte) ausgeführt.

Multisampling-Formate können in Renderzielen verwendet werden, die wieder in Shader mithilfe von [load](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load) gelesen werden können, da für einzelne Beispiele, auf die der Shader zugreift, keine Lösung erforderlich ist. Tiefenformate werden für Multisample-Ressourcen nicht unterstützt. Daher sind Tiefenformate auf Renderziele beschränkt.

Formate ohne Typ unterstützen Multisampling, um einer Ressourcenansicht das Interpretieren von Daten auf unterschiedliche Weise zu ermöglichen. Beispielsweise erstellen Sie konnte eine für Multisampling-Ressource mit R8G8B8A8\_TYPELESS, rendern Sie über eine renderzielansicht Ressource mit einer R8G8B8A8\_UINT zu formatieren, und beheben Sie den Inhalt in eine andere Ressource mit einer R8G8B8A8\_UNORM-Datenformat.

### <a name="span-idhardwaresupportspanspan-idhardwaresupportspanspan-idhardwaresupportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>Hardwaresupport

Die API meldet Hardwareunterstützung für das Multisampling durch die Anzahl der Qualitätsstufen. Die Qualitätsstufe 0 bedeutet beispielsweise, dass die Hardware Multisampling (in einem bestimmten Format und auf einer bestimmten Qualitätsstufe) nicht unterstützt. Die Qualitätsstufe 3 bedeutet, dass die Hardware drei verschiedene Beispiellayouts und/oder -algorithmen unterstützt. Sie können auch Folgendes annehmen:

-   Jedes Format, das Multisampling unterstützt, unterstützt die gleiche Anzahl von Qualitätsstufen für jedes Format in dieser Familie.
-   Jedes Format, das Multisampling unterstützt und verfügt über die \_UNORM, \_SRGB, \_SNORM oder \_"float" formatiert auch Auflösung unterstützt.

### <a name="span-idcentroidsamplingspanspan-idcentroidsamplingspanspan-idcentroidsamplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>Schwerpunkt Stichproben von Attributen, wenn für Multisampling Antialiasing

Standardmäßig werden Vertexattribute während des Multisample-Antialiasing zu einer Pixelmitte interpoliert. Wenn die Pixelmitte nicht abgedeckt ist, werden die Attribute zu einer Pixelmitte extrapoliert. Eine Pixel-Shader-Eingabe, die die Schwerpunktsemantik enthält (vorausgesetzt, dass das Pixel nicht vollständig abgedeckt ist), wird an einer beliebigen Stelle innerhalb des abgedeckten Bereichs des Pixels gesampelt, möglicherweise an einer der abgedeckten Beispielpositionen. Eine Beispielmaske (angegeben durch den Rasterizer-Zustand) wird vor der Schwerpunktberechnung angewendet. Aus diesem Grund wird ein Beispiel außerhalb der Maske nicht als Schwerpunktposition verwendet.

Der Referenz-Rasterizer wählt eine Beispielposition für das Schwerpunkt-Sampling ähnlich wie folgt aus:

-   Die Beispielmaske lässt alle Beispiele zu. Verwenden Sie eine Pixelmitte, falls das Pixel abgedeckt ist oder falls keines der Beispiele abgedeckt ist. Andernfalls wird das erste abgedeckte Beispiel ausgewählt, beginnend in der Pixelmitte und von dort nach außen.
-   Die Beispielmaske deaktiviert alle Beispiele außer eins (ein häufiges Szenario). Eine Anwendung kann Multipass-Supersampling durch Durchlaufen von Einzelbit-Beispielmaskenwerte und erneutes Rendern der Szene für jedes Beispiel mithilfe des Schwerpunkt-Samplings implementieren. Dies würde erfordern, dass eine Anwendung Ableitungen anpasst, um entsprechend detailliertere Textur-Mips für die höhere Textur-Samplingdichte auszuwählen.

### <a name="span-idderivativecalculationsspanspan-idderivativecalculationsspanspan-idderivativecalculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>Ableitung Berechnungen beim Multisampling

Pixel-Shader werden immer mindestens mithilfe eines 2x2-Pixelbereichs ausgeführt, um abgeleitete Berechnungen zu unterstützen, die mit Deltas zwischen Daten aus benachbarten Pixeln berechnet werden (unter der Annahme, dass die Daten in jedem Pixel mit horizontalem oder vertikalem Einheitenabstand gesampelt wurden). Multisampling hat darauf keine Auswirkungen.

Wenn Ableitungen für ein Attribut angefordert werden, das mir Schwerpunkt gesampelt wurde, wird die Berechnung der Hardware nicht angepasst, was zu ungenauen Ableitungen führen kann. Ein Shader erwartet einen Einheitenvektor im Renderzielraum, erhält jedoch unter Umständen einen Vektor ohne Einheit mit Bezug auf einen anderen Vektorraum. Daher liegt es in der Verantwortung der Anwendung, Vorsicht walten zu lassen, wenn Ableitungen von Attributen angefordert werden, die mit Schwerpunkt gesampelt werden.

Tatsächlich empfiehlt es sich, Ableitungen und Schwerpunkt-Sampling nicht miteinander zu kombinieren. Schwerpunkt-Sampling ist in Situationen hilfreich, in denen es wichtig ist, das interpolierte Attribute eines Grundtyps nicht extrapoliert werden, was aber Nachteile mit sich bringt, z. B. dass Attribute anscheinend springen, wenn der Rand eines Grundtyps einen Pixel kreuzt (anstatt sich kontinuierlich zu ändern) oder dass Ableitungen nicht von Textur-Sampling-Vorgängen verwendet werden können, die LOD ableiten.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Anhänge](appendix.md)

[Phase des Rasterizers (RS)](rasterizer-stage--rs-.md)

 

 




