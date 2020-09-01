---
title: Ebene 2
description: Die Unterstützung der Ebene 2 für Streaming-Ressourcen fügt Funktionen über Ebene 1 hinaus hinzu, z. b. die Gewährleistung der nicht gepackten Textur MipMap, wenn die Größe mindestens eine Standard Kachel Form ist shaderanweisungen zum Spannen der Detailebene (LOD) und zum Abrufen des Status über den shadervorgang. beim Lesen von mit NULL zugeordneten Kacheln wird der Stichproben Wert auch als 0 (null) behandelt.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- Ebene 2
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62fbbf3f21d614739918478b58394e51dbe577f9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175164"
---
# <a name="tier-2"></a>Ebene 2


Die Unterstützung der Ebene 2 für Streaming-Ressourcen fügt Funktionen über Ebene 1 hinaus hinzu, z. b. die Gewährleistung der nicht gepackten Textur MipMap, wenn die Größe mindestens eine Standard Kachel Form ist shaderanweisungen zum Spannen der Detailebene (LOD) und zum Abrufen des Status über den shadervorgang. beim Lesen von mit NULL zugeordneten Kacheln wird der Stichproben Wert auch als 0 (null) behandelt.

## <a name="span-idtier_2_general_supportspanspan-idtier_2_general_supportspanspan-idtier_2_general_supportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>Allgemeiner Support der Ebene 2


Die Unterstützung der Ebene 2 umfasst Folgendes:

-   Hardware auf Featureebene 11,1.
-   Alle Features der vorherigen Ebene (ohne Einschränkungen der [Ebene 1](tier-1.md) ) sowie die Ergänzungen der folgenden Elemente:
-   Shaderanweisungen zum Spannen von Lod und zugeordneten Status Feedback sind verfügbar. Weitere Informationen finden Sie unter [HLSL Streaming Resources](hlsl-streaming-resources-exposure.md).

Zusätzlich gibt es einige spezielle Support Probleme, die Folgen.

## <a name="span-idnon-mapped_tilesspanspan-idnon-mapped_tilesspanspan-idnon-mapped_tilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>Nicht zugeordnete Kacheln


Lesevorgänge von nicht zugeordneten Kacheln geben 0 (null) in allen nicht fehlenden Komponenten des Formats und die Standardeinstellung für fehlende Komponenten zurück.

Schreibvorgänge in nicht zugeordnete Kacheln werden nicht mehr in den Arbeitsspeicher aufgenommen, werden jedoch möglicherweise in Caches gespeichert, bei denen nachfolgende Lesevorgänge in die gleiche Adresse möglicherweise nicht abgerufen werden.

## <a name="span-idtexture_filteringspanspan-idtexture_filteringspanspan-idtexture_filteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>Textur Filterung


Bei der Textur Filterung mit einem Speicherplatz, der auf Kacheln von **null** und nicht**null** liegt, wird 0 (mit Standardwerten für fehlende Format Komponenten) für texeln auf **null** -Kacheln in den Gesamt Filter Vorgang einbezogen. Einige frühe Hardware erfüllt diese Anforderung nicht und gibt 0 (mit Standardwerten für fehlende Format Komponenten) für das vollständige Filterergebnis zurück, wenn eine texeln (mit einer Gewichtung ungleich null) auf eine **null** -Kachel fallen. Keine andere Hardware darf die Anforderung zum Einbeziehen aller (ungleich NULL gewichteten) texeln in den Filter Vorgang übersehen.

**Null** -texuszugriffe bewirken, dass der [**checkaccessfullymapping**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) -Vorgang für ein Textur Lesevorgang den Wert "false" zurückgibt. Dies ist unabhängig davon, wie das Ergebnis des Textur Zugriffs im Shader im Shader maskiert werden kann und wie viele Komponenten im Textur Format vorliegen (die Kombination aus, die den Anschein hat, dass auf die Textur nicht zugegriffen werden muss).

## <a name="span-idalignment_constraintsspanspan-idalignment_constraintsspanspan-idalignment_constraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>Ausrichtungs Einschränkungen


Ausrichtungs Einschränkungen für Standard Kachel Formen: bei MipMaps, bei denen mindestens eine Standard Kachel in allen Dimensionen ausgefüllt wird, wird garantiert die Standard-tiult verwendet, wobei der Rest als **Einheit als Einheit** in n Kacheln (der der Anwendung gemeldet wird) als gepackt betrachtet wird. Die Anwendung kann die N-Kacheln willkürlich nicht zusammenhängenden Positionen in einem Kachel Pool zuordnen, muss jedoch entweder alle oder keine der gepackten Kacheln zuordnen. Die MIP-Verpackung ist ein eindeutiger Satz von gepackten Kacheln pro Array Slice.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Minimale/maximale Reduzierungs Filterung


Die minimale/maximale Reduzierungs Filterung wird unterstützt. Weitere Informationen finden Sie unter [Streaming Resources Textur Sampling Features](streaming-resources-texture-sampling-features.md).

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>Einschränken


Das Streamen von Ressourcen mit allen Mipmaps, die kleiner als die Standard Kachel Größe in einer Dimension sind, darf nicht größer als 1 sein.

Einschränkungen für den Zugriff auf Kacheln, wenn doppelte Zuordnungen vorhanden sind, werden weiterhin angewendet. Siehe [Kachel Zugriffs Einschränkungen mit doppelten Zuordnungen](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ebenen der Features von Streamingressourcen](streaming-resources-features-tiers.md)

 

 