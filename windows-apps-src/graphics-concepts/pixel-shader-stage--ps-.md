---
title: Pixelshaderphase (PS)
description: Die Pixelshaderphase (PS) empfängt interpolierte Daten für einen Grundtyp und generiert Pro-Pixel-Daten (z. B. die Farbe).
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- Pixelshaderphase (PS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 81b2bc5e78087b19d8829df4dab4b03e4db76467
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370978"
---
# <a name="pixel-shader-ps-stage"></a>Pixelshaderphase (PS)


Die Pixelshaderphase (PS) empfängt interpolierte Daten für einen Grundtyp und generiert Pro-Pixel-Daten (z. B. die Farbe).

Hierbei handelt es sich um eine programmierbare Shaderphase. Sie wird als abgerundeter Block im Diagramm [-Grafikpipeline](graphics-pipeline.md) angezeigt. Diese Shaderphase bietet eigene eindeutige Funktionen, die auf dem Shadermodell 4.0 [gemeinsamer Shaderkern](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) basieren.

Die Pixelshaderphase (PS) aktiviert umfassende Schattierungstechniken wie z. B. Beleuchtung pro Pixel und Nachbearbeitung. Ein Pixelshader ist ein Programm, das Konstantenvariablen, Texturdaten, interpolierte Vertex-Werte und andere Daten für Pro-Pixel-Ausgaben kombiniert. Die [Rasterizerphase (RS)](rasterizer-stage--rs-.md) ruft für jeden von einem Grundtyp abgedeckten Pixel einen Pixelshader auf, es ist jedoch möglich, einen **NULL**-Shader anzugeben, um zu vermeiden, dass ein Shader ausgeführt wird.

Wenn ein Multisampling für eine Textur durchgeführt wird, wird pro abgedecktem Pixel ein Pixelshader aufgerufen, während für jedes abgedeckte Multisample ein Tiefen- und Schablonentest ausgeführt wird. Beispiele, die den Tiefen- und Schablonentest bereits bestanden haben, werden mit der Pixelshader-Ausgabefarbe aktualisiert.

Die Pixelshader innewohnenden Funktionen erstellen oder verwenden unter Berücksichtigung von Bildschirmbereich x und y Mengenableitungen. Die Ableitungen werden am häufigsten zur Detaillierungsgrad-Berechnung für das Textursampling und bei der anisotropischen Filterung verwendet, wobei Beispiels entlang der Achse der Probe gesammelt werden. In der Regel führt eine Hardware-Implementierung einen Pixelshader auf mehreren Pixel (z. B. 2 x 2-Raster) gleichzeitig aus, damit die im Pixelshader berechneten Mengenableitungen entsprechend als Deltas der Werte am selben Ausführungspunkt in benachbarten Pixeln angenähert werden können.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>Eingaben


Wenn die Pipeline ohne einen Geometry-Shader konfiguriert wurde, ist ein Pixel-Shader auf 16 Eingaben mit je 32 Bits und 4 Komponenten beschränkt. Andernfalls kann ein Pixel-Shader bis zu 32 Eingaben mit je 32 Bits und 4 Komponente verarbeiten.

Zu den Eingabedaten für den Pixel-Shader gehören Vertexattribute, die mit oder ohne perspektivische Korrekturen interpoliert werden können oder als Konstanten pro Grundform behandelt werden können. Pixel-Shader-Eingaben werden anhand der Vertexattribute der zu rasternden Grundform interpoliert, basierend auf dem deklarierten Interpolationsmodus. Wenn eine Grundform vor der Rasterung gekappt wird, wird der Interpolationsmodus auch während des Clippingvorgangs Clipping berücksichtigt.

Vertexattribute werden an Pixelshader-Rechenzentrumsstandort interpoliert (oder ausgewertet). Die Interpolationsmodi des Pixeshader-Attributs werden in einer Eingabe-Registererklärungregister pro Element in einem [Argument](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-function-parameters) oder einer [Eingabestruktur](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-struct) deklariert. Attribute können linear werden oder mit Schwerpunkt-Sampling interpoliert werden. Weitere Informationen finden Sie im Abschnitt "Schwerpunkt-Sampling von Attributen beim Multisample-Antialiasing" [Regeln für die Rasterung](rasterization-rules.md). Die Bewertung des Schwerpunkts ist nur während des Multisampling relevant, um Fälle abzudecken, in denen zwar ein Pixel, aber nicht die Pixelmitte von einem Grundtyp abgedeckt wird. Die Bewertung des Schwerpunktes erfolgt so nah wie möglich an der (nicht abgedeckten) Pixelmitte.

Eingaben können auch mit einer [-Systemwertsemantik](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) deklariert werden, die einen Parameter kennzeichnet, der von anderen Pipelinephasen genutzt wird. Z. B. eine Pixelposition sollte mit PA markiert werden\_semantische Position. Die [Eingabe-Assembler (IA) Phase](input-assembler-stage--ia-.md) kann eine skalare für ein Pixel-Shader erstellen (SV mit\_PrimitiveID); die [des Rasterizers (RS)-Phase](rasterizer-stage--rs-.md) können auch eine skalare für ein Pixel-Shader (mithilfe von SVgenerieren\_ IsFrontFace).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>Ausgaben


Ein Pixel-Shader kann bis zu 8 Farben mit je 32 Bits und 4 Komponente ausgeben oder keine Farbe, wenn das Pixel verworfen wird. Registerkomponenten für die Pixel-Shader-Ausgabe müssen deklariert werden, bevor sie verwendet werden können. Jedes Register kann eine unterschiedliche Ausgabeschreibmaske besitzen.

Steuern Sie mit dem Zustand Tiefenschreibzugriff aktivieren (in der [Ausgabezusammenführungsphase](output-merger-stage--om-.md)), ob Tiefendaten in einen Tiefenpuffer geschrieben werden (oder verwenden Sie die Verwerfen-Anweisung, um Daten für das Pixel zu verwerfen). Ein Pixel-Shader kann auch einen Tiefenwert optional 32-Bit-, 1-Komponenten, Gleitkommazahlen, für Tiefe Tests ausgeben (PA mit\_semantischer Tiefe). Der Tiefenwert wird im oDepth-Register ausgegeben und ersetzt den interpolierten Tiefenwert für den Tiefentest (sofern Tiefentest aktiviert ist). Es gibt keine Möglichkeit, dynamisch zwischen der Verwendung der Tiefe mit fester Funktion oder oDepth-Shader zu wechseln.

Ein Pixelshader kann keinen Schablonenwert ausgeben.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




