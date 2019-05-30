---
title: Ausgabezusammenführungsphase (OM)
description: Die Ausgabezusammenführungsphase (OM) kombiniert verschiedene Ausgabedaten (Pixelshaderwerte, Tiefen- und Schabloneninformationen) mit dem Inhalt des Renderziels und Tiefen-/Schablonenpuffern, um das endgültige Pipelineergebnis zu generieren.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- Ausgabezusammenführungsphase (OM)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 177d5a8fed47396fa694bd8fb88baea8d8b7bbb3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371187"
---
# <a name="output-merger-om-stage"></a>Ausgabezusammenführungsphase (OM)


Die Ausgabezusammenführungsphase (OM) kombiniert verschiedene Ausgabedaten (Pixelshaderwerte, Tiefen- und Schabloneninformationen) mit dem Inhalt des Renderziels und Tiefen-/Schablonenpuffern, um das endgültige Pipelineergebnis zu generieren.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Zweck und Verwendung


Die Ausgabezusammenführungsphase (OM) ist der letzte Schritt, in dem festgelegt werden kann, welche Pixel (mit Tiefen-Schablonen-Tests) angezeigt werden. Zudem können die endgültigen Pixelfarben vermischt werden.

Die OM-Phase generiert die endgültige gerenderte Pixelfarbe durch eine Kombination folgender Daten:

-   Pipelinestatus
-   Die von den Pixelshadern generierten Pixeldaten
-   Der Inhalt der Renderziele
-   Der Inhalt der Tiefe/Schablone-Puffer.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>Übersicht über die Blending

Bei der Vermischung werden eine oder mehrere Pixelwerte zum Erstellen einer endgültigen Pixelfarbe kombiniert. In der folgenden Abbildung wird der Prozess Vermischung von Pixeldaten dargestellt.

![Diagramm zur Funktionsweise der Vermischung von Daten](images/d3d10-blend-state.png)

Vom Konzept her können Sie dieses Flussdiagramm, das zweimal in die Ausgabezusammenführungsphase implementiert wird, visualisieren: das erster vermischt RGB-Daten, während das zweite gleichzeitig Alphadaten vermischt. Informationen zur Verwendung der APIs zum Erstellen und Festlegen des Blend-Status finden Sie unter [Vermischungsfunktionen konfigurieren](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-blend-state).

Die Vermischung mit fester Funktion kann unabhängig für jedes Renderziel aktiviert werden. Es gibt jedoch nur eine Gruppe von Steuerelementen für die Vermischung, sodass die gleiche Vermischung auf alle Renderziele mit aktivierter Vermischung angewandt wird. Die Vermischungswerte (einschließlich BlendFactor) werden vor der Vermischung immer auf den Bereich des Renderzielformats arretiert. Die Klammerung erfolgt pro Renderziel und unter Berücksichtigung des Renderzieltyps. Die einzige Ausnahme bilden float16-, float11- und float10-Formate, die nicht festgelegt werden, damit die Vermischungsvorgänge für diese Formate mit mindestens der gleichen Genauigkeit/dem gleichen Bereich die des Ausgabeformats erfolgen können. Die NaNs und signierten Nullen werden in allen Fällen (einschließlich 0.0 Blend-Schriftbreiten) weitergegeben.

Bei der Verwendung von sRGB-Renderzielen konvertiert die Laufzeit die Renderzielfarbe vor der Vermischung in linearen Speicherplatz. Die Laufzeit konvertiert den gemischten Endwert zurück in sRGB-Speicherplatz, bevor der Wert wieder im Renderziel gespeichert wird.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>Dual-Quellfarbe blending

Mit diesem Feature kann die Ausgabezusammenführungsphase beide Ausgaben des Pixelshaders (o0 und o1) gleichzeitig als Eingaben für einen Vorgang mit einem einzelnen Renderziel in Steckplatz 0 verwenden. Beispiele für gültige Vermischungsvorgänge sind: hinzufügen, subtrahieren und wieder hinzufügen. Die Vermischungsgleichung und die Maske Schreibausgabe geben an, welche Komponenten der Pixelshader ausgibt. Zusätzliche Komponenten werden ignoriert.

Schreiben in andere Ausgaben von Pixelshadern (o2, o3 usw.) ist nicht definiert; Sie können nur in Renderziele schreiben, die an Steckplatz 0 gebunden sind. Das Schreiben von oDepth ist während der Farbvermischung aus zwei Quellen gültig.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>Übersicht über tiefenschablone

Ein Tiefenschablonenpuffer, der als Texturressource erstellt wird, kann sowohl Tiefen- als auch Schablonendaten enthalten. Die Tiefedaten werden verwendet, um zu bestimmen, welche Pixel sich am nächsten bei der Kamera befinden, und die Schablonedaten werden verwendet, um zu maskieren, welche Pixel aktualisiert werden können. Schließlich werden sowohl die Tiefen- als auch die Schablonendatenwerte von der Ausgabezusammenführungsphase verwendet, um festzustellen, ob eine Pixel gezeichnet werden soll oder nicht. Das folgende Diagramm zeigt die Durchführung des Tiefen- und Schablonentests.

![Diagramm zur Funktionsweise des Tiefen- und Schablonentests](images/d3d10-depth-stencil-test.png)

Informationen zur Konfiguration des Tiefen- und Schablonentests finden Sie unter [Konfigurieren der Tiefenschablonenfunktionalität](configuring-depth-stencil-functionality.md). Ein Tiefenschablonenobjekt kapselt den Tiefenschablonenzustand. Eine Anwendung kann den Tiefenschablonenzustand spezifizieren oder die OM-Phase verwendet Standardwerte. Vermischensprozesse werden pro Pixel durchgeführt, wenn Multisampling deaktiviert ist. Wenn Multisampling aktiviert ist, wird die Vermischung pro Multisample durchgeführt.

Der Vorgang der Verwendung des Tiefenpuffers, um zu ermitteln, welcher Pixel gezeichnet werden soll, wird Tiefenpufferung oder manchmal auch Z-Pufferung genannt.

Sobald Tiefenwerte die Ausgabezusammenführungsphase erreichen (ob aus der Interpolation oder aus einem Pixelshader), werden sie immer gemäß des Formates/der Präzision des Tiefenpuffers und den Gleitkommaregeln, arretiert: z = min(Viewport.MaxDepth,max(Viewport.MinDepth,z)). Nach der Klammerung wird der Tiefenwert (mittels DepthFunc) mit dem vorhandenen Tiefenpufferwert verglichen. Wenn kein Tiefenpuffer gebunden ist, gilt der Tiefentest immer als bestanden.

Wenn im Tiefenpufferformat keine Schablonenkomponente vorhanden ist oder kein Tiefenpuffer gebunden ist, gilt der Schablonentest immer als bestanden.

Es kann jeweils nur ein Tiefen-/Schablonepuffer aktiv sein; gebundenen Ressourcenansichten müssen mit der Tiefen-/Schablonenansicht übereinstimmen (selbe Größe und Dimensionen). Dies bedeutet nicht, dass die Ressourcengröße übereinstimmen muss, sondern die Ansichtsgröße muss übereinstimmen.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>Übersicht über die Beispiel-Maske

Eine Beispielmaske ist eine Multi-Sampling-Deckungsmaske mit 32 Bit, deren Beispiele in aktiven Renderziele aktualisiert werden. Es ist nur eine Beispielmaske zulässig. Die Zuordnung der Bits in einer Beispielmaske zu den Beispielen einer Ressource wird von einem Benutzer festgelegt. Für das n-Sample-Rendering werden die ersten n-Bits der Beispielmaske (aus den unwichtigsten Bytes) verwendet (maximal 32 Bits).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


Die Ausgabezusammenführungsphase (OM) generiert die endgültige gerenderte Pixelfarbe durch die Kombination folgender Daten:

-   Pipelinestatus
-   Die von den Pixelshadern generierten Pixeldaten
-   Der Inhalt der Renderziele
-   Der Inhalt der Tiefe/Schablone-Puffer.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>Übersicht über die Write-Output mask

Verwenden Sie (pro Komponente) eine Ausgabe-Schreibmaske , um zu steuern, welche Daten in ein Renderziel geschrieben werden können.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>Übersicht über mehrere Render-Ziele

Mit einen Pixelshader können mindestens 8 separate Renderziele des gleichen Typs (Puffer, Texture1D, Texture1DArray usw.) gerendert werden. Darüber hinaus müssen alle Renderziele in allen Dimensionen (Breite, Höhe, Tiefe, Arraygröße, Beispielanzahlen) die gleiche Größe besitzen. Jedes Renderziel kann ein anderes Datenformat haben.

Sie können eine beliebige Kombination aus Renderziel-Steckplätzen (bis zu 8) verwenden. Eine Ressourcenansicht kann nicht jedoch an mehrere Renderziel-Steckplätze gleichzeitig gebunden werden. Eine Ansicht kann verwendet werden, solange die Ressourcen nicht gleichzeitig genutzt werden.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">Konfigurieren der Funktionalität der tiefenschablone</a></p></td>
<td align="left"><p>In diesem Abschnitt werden die Schritte zum Einrichten des Tiefenschablonenpuffers und der Tiefenschablonenphase für die Ausgabezusammenführungsphase behandelt.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




