---
title: Ausgabezusammenführungsphase (OM)
description: Die Phase Output Merger (OM) kombiniert verschiedene Typen von Ausgabedaten (Pixel-Shader-Werte, tiefen-und Schablonen Informationen) mit dem Inhalt des Renderziels und der tiefen/Schablonen Puffer, um das abschließende Pipeline Ergebnis zu generieren.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- Ausgabezusammenführungsphase (OM)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6478eaa2dadac74c22e5959623f03547f18babfc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156314"
---
# <a name="output-merger-om-stage"></a>Ausgabezusammenführungsphase (OM)


Die Phase Output Merger (OM) kombiniert verschiedene Typen von Ausgabedaten (Pixel-Shader-Werte, tiefen-und Schablonen Informationen) mit dem Inhalt des Renderziels und der tiefen/Schablonen Puffer, um das abschließende Pipeline Ergebnis zu generieren.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Zweck und Verwendung


Bei der Ausgabe Zusammenführung (OM) handelt es sich um den letzten Schritt, um zu bestimmen, welche Pixel sichtbar sind (mit tiefen Schablone-Tests) und wie die endgültigen Pixel Farben gemischt werden.

Die OM-Phase generiert die endgültige gerenderte Pixelfarbe mithilfe einer Kombination aus folgendem:

-   Pipeline Status
-   Die Pixeldaten, die von den Pixel-Shadern generiert werden.
-   Der Inhalt der Renderziele
-   Der Inhalt der tiefen/Schablonen Puffer.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>Übersicht über das Mischen

Blending kombiniert einen oder mehrere Pixelwerte, um eine endgültige Pixelfarbe zu erstellen. Das folgende Diagramm zeigt den Prozess, der zum Mischen von Pixeldaten beteiligt ist.

![Diagramm zur Funktionsweise von Mischungs Daten](images/d3d10-blend-state.png)

Konzeptionell können Sie dieses Flussdiagramm visualisieren, das zweimal in der Ausgabe Zusammenführungs Phase implementiert wird: das erste kombiniert RGB-Daten, während gleichzeitig ein zweites Alpha Daten kombiniert. Informationen zur Verwendung der-API zum Erstellen und Festlegen des Blend-Zustands finden Sie unter [Konfigurieren von Mischungs Funktionen](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-blend-state).

Die Kombination von "Fixed-Function" kann für jedes Renderziel unabhängig aktiviert werden. Es gibt jedoch nur einen Satz von Blend-Steuerelementen, sodass die gleiche Blend auf alle Rendertargets mit aktivierter Mischung angewendet wird. Blend-Werte (einschließlich blendfactor) werden vor dem Mischen immer an den Bereich des renderzielformats gebunden. Die Klammer erfolgt pro Renderziel und respektiert den renderzieltyp. Die einzige Ausnahme sind die float16-, float11-oder float10-Formate, die nicht geklammert werden, sodass Blend-Vorgänge für diese Formate mit mindestens gleicher Genauigkeit/Bereich als Ausgabeformat durchgeführt werden können. Nan-und signed Nullen werden für alle Fälle (einschließlich 0,0 Blend-Gewichtungen) weitergegeben.

Wenn Sie sRGB-Renderziele verwenden, konvertiert die Common Language Runtime die renderzielfarbe in linearen Raum, bevor Sie gemischt funktioniert. Die Laufzeit konvertiert den endgültigen gemischten Wert wieder in den sRGB-Speicherplatz, bevor der Wert wieder in das Renderziel gespeichert wird.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>Farbmischung aus zwei Quellen

Diese Funktion ermöglicht der Ausgabe Zusammenführungs Phase das gleichzeitige verwenden beider Pixel-Shader-Ausgaben (o0 und O1) als Eingaben für einen Mischungs Vorgang mit dem einzelnen Renderziel an Slot 0. Gültige Blend-Vorgänge sind: Add, subtrahieren und revsubtract. Die Blend-Gleichung und die Ausgabe Schreib Maske geben an, welche Komponenten der Pixelshader ausgibt. Zusätzliche Komponenten werden ignoriert.

Das Schreiben in andere Pixel-Shader-Ausgaben (O2, O3 usw.) ist nicht definiert. Sie dürfen nicht in ein Renderziel schreiben, wenn es nicht an Slot 0 gebunden ist. Das Schreiben der otiefe ist während der Dual-Source-Farbmischung gültig.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>Übersicht über das Testen der tiefen Schablone

Ein tiefen Schablone-Puffer, der als Textur Ressource erstellt wird, kann sowohl Tiefendaten als auch Daten der Schablone enthalten. Die Tiefendaten werden verwendet, um zu bestimmen, welche Pixel der Kamera am nächsten liegen, und die Schablonen Daten werden verwendet, um die zu aktualisierenden Pixel zu maskieren. Letztendlich werden die Daten der tiefen-und Schablonen Werte von der Ausgabe-Fusion-Phase verwendet, um zu bestimmen, ob ein Pixel gezeichnet werden soll oder nicht. Das folgende Diagramm zeigt konzeptionell, wie tiefen Schablonen getestet werden.

![Diagramm der Funktionsweise von tiefen Schablone](images/d3d10-depth-stencil-test.png)

Informationen zum Konfigurieren von tiefen Schablone-Tests finden Sie unter [Konfigurieren der tiefen Schablone-Funktionalität](configuring-depth-stencil-functionality.md). Ein tiefen Schablone-Objekt kapselt den Zustand der tiefen Schablone. Eine Anwendung kann den Status der tiefen Schablone angeben, oder die OM-Phase verwendet die Standardwerte. Mischungs Vorgänge werden pro Pixel ausgeführt, wenn multisamplinggrad deaktiviert ist. Wenn die multisamplinggrad-Funktion aktiviert ist, erfolgt die Mischung pro Multisample-Basis.

Der Prozess der Verwendung des tiefen Puffers, um zu bestimmen, welches Pixel gezeichnet werden soll, wird als tiefen Pufferung bezeichnet, auch manchmal als z-Pufferung bezeichnet.

Sobald die tiefen Werte die Ausgabe-Fusion-Phase erreichen (unabhängig davon, ob Sie von der Interpolations-oder von einem Pixelshader stammt), werden Sie immer mit einem klammergen (z = min (Viewport. maxtiefe, Max (Viewport. mintiefe, z)) entsprechend dem Format/der Genauigkeit des tiefen Puffers, mithilfe von Gleit Komma Regeln. Nach dem Einspannen wird der tiefen Wert (mit depthfunc) mit dem vorhandenen tiefen Puffer Wert verglichen. Wenn kein tiefen Puffer gebunden ist, verläuft der tiefen Test immer.

Wenn im tiefen Puffer Format keine Schablonen Komponente vorhanden ist oder kein tiefen Puffer gebunden ist, wird der Schablone-Test immer weitergeleitet.

Es kann immer nur ein tiefen-/Schablone-Puffer aktiv sein. die Ansicht "Tiefe/Schablone" muss eine beliebige gebundene Ressourcen Ansicht (Größe und Größe) entsprechen. Dies bedeutet nicht, dass die Ressourcen Größe entsprechen muss, nur dass die Ansichts Größe Stimmen muss.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>Übersicht über die Beispiel Maske

Eine Beispiel Maske ist eine 32-Bit-Abdeckungs Maske, die bestimmt, welche Beispiele in aktiven Renderingzielen aktualisiert werden. Es ist nur eine Beispiel Maske zulässig. Die Zuordnung von Bits in einer Beispiel Maske zu den Beispielen in einer Ressource wird von einem Benutzer definiert. Für das Rendering von n-Stichproben werden die ersten n Bits (aus dem LSB) der Beispiel Maske verwendet (32 Bits ist die maximale Anzahl von Bits).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Der


In der Ausgabe Fusion (OM)-Phase wird die endgültige gerenderte Pixelfarbe mithilfe einer Kombination folgender Werte generiert:

-   Pipeline Status
-   Die Pixeldaten, die von den Pixel-Shadern generiert werden.
-   Der Inhalt der Renderziele
-   Der Inhalt der tiefen/Schablonen Puffer.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgeben


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>Ausgabe: Schreib Maske (Übersicht)

Verwenden Sie eine Ausgabe-/Schreibmaske, um (pro Komponente) zu steuern, welche Daten in ein Renderziel geschrieben werden können.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>Übersicht über mehrere Renderziele

Ein Pixelshader kann zum Rendern von mindestens 8 separaten renderzielen verwendet werden, von denen alle denselben Typ aufweisen müssen (buffer, Texture1D, Texture1DArray usw.). Außerdem müssen alle Renderziele in allen Dimensionen (Breite, Höhe, Tiefe, Array Größe, Anzahl von Stichproben) dieselbe Größe aufweisen. Jedes Renderziel weist möglicherweise ein anderes Datenformat auf.

Sie können eine beliebige Kombination von renderzielslots verwenden (bis zu 8). Eine Ressourcen Ansicht kann jedoch nicht gleichzeitig an mehrere renderzielslots gebunden werden. Eine Sicht kann wieder verwendet werden, solange die Ressourcen nicht gleichzeitig verwendet werden.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">Konfigurieren der Tiefenschablonenfunktionalität</a></p></td>
<td align="left"><p>In diesem Abschnitt werden die Schritte zum Einrichten des tiefen Schablone-Puffers und der Status der tiefen Schablone für die Ausgabe-Fusion-Phase behandelt.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 