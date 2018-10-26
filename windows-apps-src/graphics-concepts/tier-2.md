---
title: Ebene2
description: Der Support der Ebene2 für Streamingressourcen fügt über die Ebene1 hinausgehende Funktionen hinzu, z.B. Garantieren eines nicht gepackten Textur-Mipmap, wenn die Größe mindestens eine Standardkachelform beträgt, Shaderanweisungen zur Klammerung der Detailebene (Level-of-Detail, LOD) und zum Abrufen des Status des Shadervorgangs sowie das Lesen aus NULL-zugeordneten Kacheln behandeln, deren Samplingwert null ergab.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- Ebene2
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fac8780995231ce56d1264ea8a1a5cb52fd9a3d0
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "5557429"
---
# <a name="tier-2"></a>Ebene2


Der Support der Ebene2 für Streamingressourcen fügt über die Ebene1 hinausgehende Funktionen hinzu, z.B. Garantieren eines nicht gepackten Textur-Mipmap, wenn die Größe mindestens eine Standardkachelform beträgt, Shaderanweisungen zur Klammerung der Detailebene (Level-of-Detail, LOD) und zum Abrufen des Status des Shadervorgangs sowie das Lesen aus NULL-zugeordneten Kacheln behandeln, deren Samplingwert null ergab.

## <a name="span-idtier2generalsupportspanspan-idtier2generalsupportspanspan-idtier2generalsupportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>Allgemeiner Support der Ebene 2


Der Support der Ebene 2 umfasst Folgendes:

-   Hardware mindestens auf Funktionsebene11.1.
-   Alle Funktionen von der vorherigen Ebene (ohne [Ebene 1](tier-1.md) bestimmte Einschränkungen) sowie die Hinzufügungen in den folgenden Elementen:
-   Shader-Anweisungen für Klammerung-LOD und Feedback über den zugeordneten Status verfügbar. Siehe [Belichtung von HLSL-Streamingressourcen](hlsl-streaming-resources-exposure.md)

Zusätzlich zu diesen gibt es einige bestimmte nachfolgende Supportfragen.

## <a name="span-idnon-mappedtilesspanspan-idnon-mappedtilesspanspan-idnon-mappedtilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>Nicht zugeordnete Kacheln


Lesevorgänge von nicht zugeordneten Kacheln geben den Wert 0 in allen nicht fehlenden Komponenten des Formats und den Standardwert für fehlende Komponenten zurück.

Schreibvorgänge auf nicht zugeordnete Kacheln werden beim Wechsel in den Speicher angehalten. Sie könnten jedoch unter Umständen in Anwendungscaches enden, die möglicherweise von nachfolgenden Lesevorgängen auf die gleiche Adresse erfasst werden.

## <a name="span-idtexturefilteringspanspan-idtexturefilteringspanspan-idtexturefilteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>Texturfilterung


Filtern mit einem Abbild, welches die **NULL** und nicht-**NULL** Kacheln überspannt und 0 (standardmäßig nach fehlenden Format) für Texel auf **NULL** Kacheln in der gesamten Ausführung beiträgt. Ältere Hardware erfüllt diese Anforderung nicht und gibt 0 (mit standardmäßig nach fehlenden Format) für das vollständige Filterergebnis zurück, wenn jegliche Texel (mit ungleich NULL Breite) sich auf einer **NULL** Kachel befinden. Jede andere Hardware muss die Anforderung erfüllen, alle Texel (mit einer Gewichtung ungleich NULL) in den Filtervorgang einzubeziehen.

**NULL** Texelzugriffe haben zur Folge, dass durch den [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) Vorgang der Feedbackstatus des Texturlesevorgangs mit „false” zurückgegeben wird. Dies ist unabhängig davon, wie die Schreibmaske des Texturzugriffsergebnisses im Shader erfolgt, und wie viele Komponenten im Texturformat vorhanden sind (deren Kombination möglicherweise den Eindruck vermittelt, dass auf die Textur nicht zugegriffen werden muss).

## <a name="span-idalignmentconstraintsspanspan-idalignmentconstraintsspanspan-idalignmentconstraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>Ausrichtungseinschränkungen


Ausrichtungseinschränkungen für Standardkachelformen: Mipmaps, die mindestens eine Standardkachel in allen Abmessungen ausfüllen, ist die Verwendung der Standardkacheln gewährleistet. Der Rest wird als eine **Einheit** in N-Kacheln (N an die Anwendung gemeldet) betrachtet. Die Anwendung kann die N-Kacheln in willkürlich getrennte Orte in einem Kachelpool zuordnen. Sie muss jedoch entweder oder keine der gepackten Kacheln zuordnen. Die Mip-Verpackung ist ein einzigartiger Satz von gepackten Kacheln pro Array-Segment.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Min/Max-Reduzierungsfilterung


Min/Max-Reduzierungsfilterung wird unterstützt. Siehe [Textursampling-Features für Streamingressourcen](streaming-resources-texture-sampling-features.md)

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>Einschränkungen


Streamingressourcen mit Mipmaps, die in jeglicher Dimension kleiner sind als die Standardkachelgröße, dürfen keine Arraygröße größer als 1 aufweisen.

Die Einschränkungen der Kachelzugriffe gelten weiterhin, wenn doppelte Zuordnungen vorhanden sind. Siehe [Kachelzugriffseinschränkungen bei doppelten Zuordnungen](tile-access-limitations-with-duplicate-mappings.md)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ebenen der Features von Streamingressourcen](streaming-resources-features-tiers.md)

 

 




