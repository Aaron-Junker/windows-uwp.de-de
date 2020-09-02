---
title: Ebene 1
description: Erfahren Sie mehr über die allgemeinen und spezifischen Einschränkungen, die sich auf die Unterstützung von Ebene 1 für Streaming-Ressourcen Features
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- Ebene 1
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ff90c75e3f4aefa28ddad5528d39d2f2d541bb0
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304642"
---
# <a name="tier-1"></a>Ebene 1


In diesem Abschnitt wird die Unterstützung für Ebene 1 beschrieben.

## <a name="span-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>Allgemeine Einschränkungen der Ebene 1


-   Hardware auf Featureebene 11,0.
-   Keine Unterstützung für das Durchsuchen.
-   Keine Texture1D-oder Texture3D-Unterstützung.
-   Keine 2, 8 oder 16 Sample Multisampling Antialiasing (MSAA)-Unterstützung. Nur 4X ist erforderlich, außer keine 128 bpp-Formate.
-   Kein standardmäßiges Verhalten (das Layout in Kacheln mit 64 KB und die End-MIP-Komprimierung ist der Hardwarehersteller).
-   Einschränkungen für den Zugriff auf Kacheln, wenn doppelte Zuordnungen vorhanden sind. Siehe [Kachel Zugriffs Einschränkungen mit doppelten Zuordnungen](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>Spezifische Einschränkungen, die nur Ebene 1 betreffen


### <a name="span-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>Lesen/Schreiben in Streamingressourcen mit NULL Zuordnungen

Streaming-Ressourcen können **null** -Zuordnungen aufweisen, aber das Lesen aus Ihnen oder das Schreiben in Sie erzeugt nicht definierte Ergebnisse, einschließlich des entfernten Geräts. Anwendungen können dies umgehen, indem Sie alle leeren Bereiche einer einzelnen Dummy-Seite Mapping. Achten Sie darauf, dass Sie schreiben und auf eine Seite Renderen, die mehreren renderzielstandorten zugeordnet ist, da die Reihenfolge der Schreibvorgänge nicht definiert ist.

### <a name="span-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>Keine shaderanweisungen zum Spannen von Lod-und zugeordneten Status Feedback

Shaderanweisungen zum Einspannen von Lod und zugeordnetem Status Feedback sind nicht verfügbar. Weitere Informationen finden Sie unter [HLSL Streaming Resources](hlsl-streaming-resources-exposure.md).

### <a name="span-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>Ausrichtungs Einschränkungen für Standard Kachel Formen

Es ist nur sichergestellt, dass MIPS (beginnend mit dem feinsten), dessen Dimensionen alle Vielfachen der Standard Kachel Größe sind, die Standard Kachel Formen unterstützt und einzelne Kacheln willkürlich zugeordnet/nicht zugeordnet werden können. Die erste MipMap in einer Streaminganwendung, die eine beliebige Dimension, nicht das Vielfache der Standard Kachel Größe, zusammen mit allen gröbere-Mipmaps enthält, kann eine nicht standardmäßige tipeform aufweisen, die für diesen Satz von MIPS gleichzeitig (n für die Anwendung) angepasst wird. Diese N Kacheln werden als eine Einheit verpackt, die von der Anwendung zu einem beliebigen Zeitpunkt vollständig zugeordnet oder vollständig vollständig zugeordnet werden muss. die Zuordnungen der einzelnen N-Kacheln können sich jedoch an willkürlich nicht zusammenhängenden Positionen in einem Kachel Pool befinden.

### <a name="span-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>Array von Mipmaps, bei denen es sich nicht um ein Vielfaches der Standard Kachel Größe handelt

Streaming-Ressourcen mit Mipmaps, die kein Vielfaches der Standard Kachel Größe in allen Dimensionen sind, dürfen keine Array Größe größer als 1 aufweisen.

### <a name="span-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>Wechseln zwischen verweisenden Kacheln in einem Kachel Pool über einen Puffer und eine Textur Ressource

Um zwischen verweisenden Kacheln in einem Kachel Pool über eine [Puffer](introduction-to-buffers.md) Ressource zu wechseln, um über eine [Textur](introduction-to-textures.md) Ressource auf die gleichen Kacheln zu verweisen, oder umgekehrt müssen die letzten Aktualisierungen der Kachel Zuordnungen oder das Kopieren von Kachel Zuordnungen, die Zuordnungen für diese Kachel Pool Kacheln definieren, für dieselbe Ressourcen Dimension (Puffer und Textur) wie für die Ressourcen Dimension verwendet werden, die \* für den Zugriff auf die Kacheln verwendet werden. Andernfalls ist das Verhalten nicht definiert, einschließlich der Gefahr, dass das Gerät zurückgesetzt wird.

Beispielsweise ist es ungültig, die Kachel Zuordnungen so zu aktualisieren, dass Kachel Zuordnungen für einen Puffer definiert werden. Anschließend können Sie die Kachel Zuordnungen auf die gleichen Kacheln im Kachel Pool über eine [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) -Ressource aktualisieren und dann über den Puffer auf die Kacheln zugreifen. Die Umgehung von Aufgaben besteht darin, die Kachel Zuordnungen für eine Ressource neu zu definieren, wenn Sie Zwischenpuffer und Textur wechseln (oder umgekehrt), indem Sie Kacheln gemeinsam nutzen oder nur Kacheln in einem Kachel Pool Zwischenpuffer Ressourcen und Textur Ressourcen freigeben.

### <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Minimale/maximale Reduzierungs Filterung

Die minimale/maximale Reduzierungs Filterung wird nicht unterstützt. Weitere Informationen finden Sie unter [Streaming Resources Textur Sampling Features](streaming-resources-texture-sampling-features.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ebenen der Features von Streamingressourcen](streaming-resources-features-tiers.md)

 

 