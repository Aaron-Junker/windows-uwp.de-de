---
title: Pixelshaderphase (PS)
description: Die Pixel-Shader-Stufe (PS) empfängt interpoliert Daten für einen primitiven und generiert Pixel-Daten wie z. b. Color.
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- Pixelshaderphase (PS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d35fa06ac415b1d4197c1b1bcbf101f1d4cf9b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156284"
---
# <a name="pixel-shader-ps-stage"></a>Pixelshaderphase (PS)


Die Pixel-Shader-Stufe (PS) empfängt interpoliert Daten für einen primitiven und generiert Pixel-Daten wie z. b. Color.

Dies ist eine programmierbare Shader-Stufe. Sie wird im [Grafik Pipeline](graphics-pipeline.md) Diagramm als abgerundeter Block angezeigt. Diese Shader-Phase bietet eine eigene, einzigartige Funktionalität, die auf dem Shader-Modell 4,0 [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)basiert.

Die Pixel-Shader (PS)-Phase ermöglicht umfassende Schattierungs Techniken wie z. b. pro Pixel-Beleuchtung und Nachbearbeitung. Ein Pixelshader ist ein Programm, das Konstante Variablen, Textur Daten, interpoliert pro Scheitelpunkt Werte und andere Daten kombiniert, um pro-Pixel-Ausgaben zu liefern. Die [Phase Rasterizer (RS)](rasterizer-stage--rs-.md) Ruft für jedes Pixel, das von einem primitiven abgedeckt wird, einen Pixel-Shader auf. es ist jedoch möglich, einen **null** -Shader anzugeben, um das Ausführen eines Shaders zu vermeiden.

Beim multisamplinggrad einer Textur wird ein Pixelshader einmal pro abgedecktem Pixel aufgerufen, während bei jedem behandelten Multisample ein tiefen-/Schablone-Test erfolgt. Beispiele, die den tiefen-/Schablone-Test bestehen, werden mit der Pixel-Shader-Ausgabe Farbe aktualisiert.

Die intrinsischen Funktionen des Pixel-Shaders liefern oder verwenden Ableitungen von Mengen in Bezug auf den Bildschirmbereich x und y. Die häufigste Verwendung von Ableitungen ist die Berechnung von Detail Berechnungen für die Textur Stichprobe und im Fall von anisotrope Filterung, indem Beispiele entlang der Achse von Anisotropie ausgewählt werden. In der Regel führt eine Hardware Implementierung einen Pixel-Shader auf mehreren Pixeln (z. b. ein 2 x 2-Raster) gleichzeitig aus, sodass Ableitungen von Mengen, die im Pixelshader berechnet werden, als Delta der Werte zum gleichen Zeitpunkt der Ausführung in angrenzenden Pixeln angleichen können.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>Ungs


Wenn die Pipeline ohne einen Geometry-Shader konfiguriert ist, ist ein Pixelshader auf 16, 32-Bit, 4-komponenteneingaben beschränkt. Andernfalls kann ein Pixelshader bis zu 32, 32-Bit, 4-komponenteneingaben annehmen.

Die Pixel-Shader-Eingabedaten enthalten Scheitelpunkt Attribute (die mit oder ohne Perspektiven Korrektur interpoliert werden können) oder als primitive Konstanten behandelt werden können. Pixel-shadereingaben werden aus den Vertex-Attributen des primitiven, das auf der Grundlage des deklarierten Interpolations Modus basiert, interpoliert. Wenn ein primitiver vor der rasterisierung abgeschnitten wird, wird der Interpolations Modus während des clippingprozesses ebenfalls berücksichtigt.

Vertex-Attribute werden an Pixel-Shader-Mittelpunkt Positionen interpoliert (oder ausgewertet). Pixelshader-Attribut Interpolations Modi werden in einer Eingabe Register Deklaration in einer pro-Element-Basis in einem [Argument](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-function-parameters) oder einer [Eingabe Struktur](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-struct)deklariert. Attribute können linear oder mit Schwerpunkt-Sampling interpoliert werden. Weitere Informationen finden Sie im Abschnitt "Centroid-Sampling von Attributen bei Multisample-Antialiasing" in [rasterisierungsregeln](rasterization-rules.md). Die Centroid-Auswertung ist nur bei der multisamplinggrad relevant, um Fälle abzudecken, in denen ein Pixel durch ein primitiv abgedeckt ist, ein Pixel Center jedoch möglicherweise nicht ist. die Schwerpunkt Auswertung erfolgt so nah wie möglich im (nicht abgedeckten) Pixel Center.

Eingaben können auch mit einer [System Wert Semantik](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)deklariert werden, die einen Parameter markiert, der von anderen Pipeline Stufen verwendet wird. Beispielsweise sollte eine Pixelposition mit der SV- \_ Positions Semantik gekennzeichnet werden. In der [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md) kann ein Skalar für einen PixelShader (mit \_ der SV primitiveid) erstellt werden. die [Phase Rasterizer (RS)](rasterizer-stage--rs-.md) kann auch einen Skalarwert für einen PixelShader generieren (mithilfe von SV \_ isfrontface).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>Ausgaben


Ein Pixelshader kann bis zu 8, 32-Bit, 4-Komponentenfarben oder keine Farbe ausgeben, wenn das Pixel verworfen wird. Die Pixel-Shader-Ausgabe Register Komponenten müssen deklariert werden, bevor Sie verwendet werden können. jedem Register ist eine eindeutige Ausgabe-/Schreibmaske gestattet.

Verwenden Sie den Status "Tiefe-Write-Enable" (in der [Ausgabe Zusammenführung (OM)](output-merger-stage--om-.md)), um zu steuern, ob die Tiefendaten in einen tiefen Puffer geschrieben werden (oder verwenden Sie die Verwerfungs Anweisung, um Daten für dieses Pixel zu verwerfen). Ein Pixelshader kann auch einen optionalen 32-Bit-, 1-Component-, Gleit Komma-und Tiefen Wert für tiefen Tests ausgeben (mit der SV- \_ tiefen Semantik). Der tiefen Wert wird im otiefe-Register ausgegeben und ersetzt den interinterpolated-tiefen Wert für tiefen Tests (vorausgesetzt, die tiefen Tests sind aktiviert). Es gibt keine Möglichkeit, dynamisch zwischen der Verwendung der tiefen Tiefe oder der Shader-otiefe zu wechseln.

Ein Pixelshader kann einen Schablonen Wert nicht ausgeben.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 