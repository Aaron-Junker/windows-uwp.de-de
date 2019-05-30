---
title: Verwenden von systemgenerierten Werten
description: Systemgenerierte Werte werden von der Eingabe-Assemblerphase (IA) generiert (basierend auf der vom Benutzer bereitgestellten Eingabesemantik), um bestimmte Effizienzvorteile in Shader-Operationen zu ermöglichen.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- Verwenden von systemgenerierten Werten
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 87d4be69d9a7869f5331d30225e93a22ad9e959c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371254"
---
# <a name="span-iddirect3dconceptsusingsystem-generatedvaluesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>Verwenden von vom System generierte Werte


Systemgenerierte Werte werden von der [IEingabe-Assemblerphase (IA)](input-assembler-stage--ia-.md) generiert (basierend auf der vom Benutzer bereitgestellten Eingabe [semantics](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)), um bestimmte Effizienzvorteile in Shader-Operationen zu ermöglichen. Durch das Anhängen von Daten, wie beispielsweise der Instanz-ID (sichtbar für die [Vertexshaderphase (VS)](vertex-shader-stage--vs-.md)), einer Scheitelpunkt-ID (sichtbar für VS) oder einer Grundtypen-ID (sichtbar für die [Geometrieshaderphase (GS)](geometry-shader-stage--gs-.md)/[Pixelshaderphase (PS)](pixel-shader-stage--ps-.md)), kann eine nachfolgende Shaderphase nach diesen Systemwerten suchen, um die Verarbeitung in dieser Phase zu verbessern.

Beispielsweise kann die VS-Phase nach der Instanzen-ID suchen, um zusätzliche Daten pro Scheitelpunkt für den Shader zu erfassen, oder um andere Vorgänge auszuführen, oder die GS- und die PS-Phase können die Grundtyp-ID verwenden, um Daten pro Grundtyp in der gleichen Weise zu erfassen.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


Jede Shader-Phase identifiziert Scheitelpunkte anhand der Scheitelpunkt-ID. Dies ist eine 32-Bit-Ganzzahl ohne Vorzeichen, deren Standardwert 0 ist. Sie wird einem Scheitelpunkt zugewiesen, wenn der Grundtyp von der [Eingabe-Assembler (IA)-Phase](input-assembler-stage--ia-.md) verarbeitet wird. Fügen Sie die Scheitelpunkt-ID-Semantik der Anfügen der Shader-Eingabedeklaration hinzu, um die IA-Phase anzuweisen, eine ID für jeden Scheitelpunkt zu generieren.

Die IA-Phase fügt jedem Scheitelpunkt eine Scheitelpunkt-ID zur Verwendung durch Shader-Phasen hinzu. Die Scheitelpunkt-ID wird für jeden Draw-Aufruf um 1 erhöht. Über indizierte Draw-Aufrufe wird die Zählung wieder auf den Anfangswert zurückgesetzt. Wenn die Scheitelpunkt-IDs überlaufen (d. h. die Anzahl 2³² – 1 überschreitet), erfolgt ein Umbruch zu 0.

Bei allen Grundtypen ist den Scheitelpunkten eine Scheitelpunkt-ID zugeordnet (unabhängig von der Nachbarschaft der Scheitelpunkte).

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


Jede Shader-Phase identifiziert Grundtypen anhand der Grundtyp-ID. Dies ist eine 32-Bit-Ganzzahl ohne Vorzeichen, deren Standardwert 0 ist. Sie wird einem Grundtyp zugewiesen, wenn er von der [Eingabe-Assembler (IA)-Phase](input-assembler-stage--ia-.md) verarbeitet wird. Fügen Sie die Grundtyp-ID-Semantik der Anfügen der Shader-Eingabedeklaration hinzu, um die IA-Phase anzuweisen, eine Grundtyp-ID zu generieren.

Die IA-Phase fügt jedem Grundtyp eine Grundtyp-ID zu Verwendung durch die [Geometrie-Shader (GS)-Phase](geometry-shader-stage--gs-.md) oder die [Scheitelpunkt-Shader (VS)-Phase](vertex-shader-stage--vs-.md) (je nachdem, welche die erste aktive Phase nach der IA-Phase ist) hinzu. Für jeden indizierten Draw-Aufruf wird die Grundtyp-ID um 1 erhöht; wenn eine neu8e Instanz beginnt, wird die Grundtyp-ID jedoch auf 0 zurückgesetzt. Alle anderen Draw-Aufrufe ändern den Wert der Instanz-ID nicht. Wenn die Instanz-IDs überlaufen (d. h. die Anzahl 2³² – 1 überschreitet), erfolgt ein Umbruch zu 0.

Die [Pixel-Shader (PS)-Phase](pixel-shader-stage--ps-.md) hat keine separate Eingabe für eine Grundtyp-ID; allerdings verwendet eine Pixel-Shader-Eingabe, die eine Grundtyp-ID angibt, einen Konstanteninterpolationsmodus.

Die automatische Erzeugung einer Grundtyp-ID für benachbarte Grundtypen wird nicht unterstützt. Für Grundtypen mit Nachbarschaft, etwa einen Dreieckstreifen mit Nachbarschaft, wird nur für die inneren Grundtypen (die nicht-benachbarten Grundtypen) eine Grundtyp-ID geführt, ähnlich wie der Satz der Grundtypen in einem Dreieckstreifen ohne Nachbarschaft.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


Jede Shader-Phase identifiziert anhand einer Instanz-ID die Instanz der Geometrie, die derzeit verarbeitet wird. Dies ist eine 32-Bit-Ganzzahl ohne Vorzeichen, deren Standardwert 0 ist.

Die [Eingabe-Assembler (IA)-Phase](input-assembler-stage--ia-.md) fügt jedem Scheitelpunkt eine Instanz-ID hinzu, wenn die Scheitelpunkt-Shader-Eingabedeklaration die Instanz-ID-Semantik enthält. Für jeden indizierten Draw-Aufruf wird die Instanz-ID um 1 erhöht. Alle anderen Draw-Aufrufe ändern den Wert der Instanz-ID nicht. Wenn die Instanz-IDs überlaufen (d. h. die Anzahl 2³² – 1 überschreitet), erfolgt ein Umbruch zu 0.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


Die folgende Abbildung zeigt, wie einem instantiierten Dreieckstreifen in der [Eingabe-Assembler (IA)-Phase](input-assembler-stage--ia-.md) Systemwerte hinzugefügt werden.

![Illustration der Systemwerte für einen instantiierten Dreieckstreifen](images/d3d10-ia-example.png)

Diese Tabellen zeigen die Systemwerte, die für beide Instanzen des gleichen Dreieckstreifens generiert werden. Die erste Instanz (Instanz U) wird blau angezeigt, die zweite Instanz (Instanz V) wird grün angezeigt. Die durchgezogenen Linien verbinden die Scheitelpunkte in den Grundtypen, die gestrichelten Linien verbinden die benachbarten Scheitelpunkte.

Die folgenden Tabellen zeigen die vom System generierten Werte für die Instanz U.

| Scheitelpunktdaten    | C, U | D, U | E, U | F, U | G, U | H, U | I, U | J, U | K, U | L, U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

Dreieckstreifen-Instanz U hat 3 Dreieck-Grundtypen, mit den folgenden vom System generierten Werten:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

Die folgenden Tabellen zeigen die vom System generierten Werte für die Instanz V.

| Scheitelpunktdaten    | C, V | D, V | E, V | F, V | G, V | H, V | I, V | J, V | K, V | L, V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

Dreieckstreifen-Instanz V hat 3 Dreieck-Grundtypen, mit den folgenden vom System generierten Werten:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

Die [Eingabe-Assembler (IA)-Phase](input-assembler-stage--ia-.md) generiert die IDs (Scheitelpunkt, Grundtyp und Instanz); Beachten Sie, dass jeder Instanz eine eindeutige Instanz-ID zugeordnet ist Die Daten enden mit dem Streifenschnitt, der die einzelnen Instanzen des Dreieckstreifens voneinander trennt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Phase der Eingabe-Assembler (IA)](input-assembler-stage--ia-.md)

 

 




