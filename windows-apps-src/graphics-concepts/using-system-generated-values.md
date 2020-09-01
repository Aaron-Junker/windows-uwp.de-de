---
title: Verwenden von systemgenerierten Werten
description: Vom System generierte Werte werden von der Eingabe Assembler-Stufe (IA) generiert (basierend auf der vom Benutzer bereitgestellten Eingabe Semantik), um bestimmte Effizienz bei shadervorgängen zu ermöglichen.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- Verwenden von systemgenerierten Werten
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7217b52c6e9f9882997649c5f843eb119d741e0b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156154"
---
# <a name="span-iddirect3dconceptsusing_system-generated_valuesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>Verwenden von systemgenerierten Werten


Vom System generierte Werte werden von der [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md) generiert (basierend auf der vom Benutzer bereitgestellten Eingabe [Semantik](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)), um bestimmte Effizienz bei shadervorgängen zu ermöglichen. Durch das Anfügen von Daten, z. b. eine Instanz-ID (sichtbar für die [Vertex-Shader-Phase](vertex-shader-stage--vs-.md)), eine Scheitelpunkt-ID (sichtbar für vs) oder eine primitive ID (sichtbar für [Geometrie-Shader (GS) Stage](geometry-shader-stage--gs-.md) / [Pixel Shader (PS)](pixel-shader-stage--ps-.md)), kann eine nachfolgende Shader-Phase nach diesen System Werten suchen, um die Verarbeitung in

Beispielsweise kann in der vs-Phase nach der Instanz-ID gesucht werden, um zusätzliche pro-Vertex-Daten für den Shader zu erfassen oder um andere Vorgänge auszuführen. in den GS-und PS-Phasen kann die primitive ID verwendet werden, um einzelne Daten auf die gleiche Weise zu erfassen.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>Vertexid


Eine Scheitelpunkt-ID wird von jeder Shader-Stufe verwendet, um jeden Scheitelpunkt zu identifizieren. Dabei handelt es sich um eine 32-Bit-Ganzzahl ohne Vorzeichen, deren Standardwert 0 ist. Sie wird einem Scheitelpunkt zugewiesen, wenn die primitive durch die [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md)verarbeitet wird. Fügen Sie die Vertex-ID-Semantik an die shadereingabedeklaration an, um die IA-Phase anzuweisen, eine pro-Vertex-ID

Die IA fügt jedem Scheitelpunkt eine Scheitelpunkt-ID zur Verwendung in Shader-Stufen hinzu. Für jeden zeichnen-Befehl wird die Vertex-ID um 1 erhöht. Über indizierte zeichnen-Aufrufe wird die Anzahl zurück auf den Startwert zurückgesetzt. Wenn die Scheitelpunkt-ID überläuft (überschreitet 2 ³ ² – 1), wird Sie auf 0 (null) umschlossen.

Für alle primitiven Typen sind Scheitel Punkte eine Scheitelpunkt-ID zugeordnet (unabhängig von der Verwendung).

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>Primitiveid


Eine primitive ID wird von jeder Shader-Stufe verwendet, um die einzelnen primitiven zu identifizieren. Dabei handelt es sich um eine 32-Bit-Ganzzahl ohne Vorzeichen, deren Standardwert 0 ist. Sie wird einem primitiven zugewiesen, wenn die primitive durch die [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md)verarbeitet wird. Fügen Sie die primitive-ID-Semantik an die Shader-Eingabe Deklaration an, um die IA-Phase zu informieren und eine primitive ID zu generieren.

In der IA-Phase wird jedem primitiven eine primitive ID für die Verwendung durch die [GS-Stufe (Geometry-Shader)](geometry-shader-stage--gs-.md) oder die [Vertex Shader (VS)](vertex-shader-stage--vs-.md) -Phase hinzugefügt (je nachdem, was die erste Phase ist, die nach der IA-Phase aktiv ist). Für jeden indizierten Draw-Aufruf wird die primitive ID um 1 erhöht. die primitive ID wird jedoch immer auf 0 zurückgesetzt, wenn eine neue Instanz beginnt. Alle anderen Draw-Aufrufe ändern den Wert der Instanz-ID nicht. Wenn die Instanz-ID überläuft (überschreitet 2 ³ ² – 1), wird Sie auf 0 (null) umschlossen.

Die [Pixel-Shader-Stufe (PS)](pixel-shader-stage--ps-.md) besitzt keine separate Eingabe für eine primitive ID. Allerdings wird für jede Pixel-Shader-Eingabe, die eine primitive ID angibt, ein konstanter Interpolations Modus verwendet.

Es gibt keine Unterstützung für das automatische Erstellen einer primitiven ID für benachbarte primitive. Bei primitiven Typen, wie z. b. einem Dreiecks Streifen mit der Verwendung, wird eine primitive ID nur für die inneren primitiven (die nicht angrenzenden primitiven) beibehalten, genauso wie der Satz von primitiven in einem Dreiecks Band ohne jegliche Notwendigkeit.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceId


Eine Instanz-ID wird von jeder Shader-Phase verwendet, um die Instanz der Geometrie zu identifizieren, die gerade verarbeitet wird. Dabei handelt es sich um eine 32-Bit-Ganzzahl ohne Vorzeichen, deren Standardwert 0 ist.

In der [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md) wird jedem Scheitelpunkt eine Instanz-ID hinzugefügt, wenn die Vertexshader-Eingabe Deklaration die Semantik der Instanz-ID enthält. Für jeden indizierten zeichnen-Befehl wird die Instanz-ID um 1 erhöht. Alle anderen Draw-Aufrufe ändern den Wert der Instanz-ID nicht. Wenn die Instanz-ID überläuft (überschreitet 2 ³ ² – 1), wird Sie auf 0 (null) umschlossen.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


In der folgenden Abbildung wird gezeigt, wie System Werte an einen in der [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md)angefügten Dreiecks Streifen angefügt werden.

![Abbildung der System Werte für einen instanziierten Dreiecks Streifen](images/d3d10-ia-example.png)

In diesen Tabellen werden die für beide Instanzen desselben Dreiecks Streifens generierten System Werte angezeigt. Die erste Instanz (Instanz U) ist blau dargestellt, die zweite Instanz (Instanz V) ist grün dargestellt. Die durch gestrichelten Linien verbinden die Scheitel Punkte in den primitiven, die gestrichelten Linien verbinden die angrenzenden Scheitel Punkte.

In den folgenden Tabellen sind die vom System generierten Werte für die Instanz U aufgeführt.

| Scheitelpunkt Daten    | C, U | D, U | E, U | F, U | G, U | H, U | I, U | J, U | K, U | L, U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **Vertexid**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

Die Dreiecks Bereichs Instanz U hat drei Dreiecks primitive mit den folgenden vom System generierten Werten:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **Primitiveid** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

In den folgenden Tabellen sind die vom System generierten Werte für die Instanz V aufgeführt.

| Scheitelpunkt Daten    | C, V | D, V | E, V | F, V | G, V | H, V | I, V | J, V | K, V | L, V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **Vertexid**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

Die Dreiecks Bereichs Instanz V verfügt über 3 Dreiecks primitive mit den folgenden vom System generierten Werten:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **Primitiveid** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

In der [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md) werden die IDs (Vertex, primitiv und Instanz) generiert; Beachten Sie auch, dass jeder Instanz eine eindeutige Instanz-ID zugewiesen wird. Die Daten enden mit dem Strip-Cut, der die einzelnen Instanzen des Dreiecks Streifens trennt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Eingabeassemblerphase (IA)](input-assembler-stage--ia-.md)

 

 