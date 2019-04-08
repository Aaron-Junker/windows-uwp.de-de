---
title: Die Notwendigkeit für Streamingressourcen
description: Streamingressourcen sind erforderlich, damit der GPU-Speicher nicht unnötig durch die Speicherung von Oberflächenbereichen belegt wird, auf die nicht zugegriffen wird, und um der Hardware mitzuteilen, wie angrenzende Kacheln gefiltert werden sollen.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- Die Notwendigkeit für Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e0354b0e727e84d562bf63779e74be72f87198f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632175"
---
# <a name="the-need-for-streaming-resources"></a>Die Notwendigkeit für Streamingressourcen


Streamingressourcen sind erforderlich, damit der GPU-Speicher nicht unnötig durch die Speicherung von Oberflächenbereichen belegt wird, auf die nicht zugegriffen wird, und um der Hardware mitzuteilen, wie angrenzende Kacheln gefiltert werden sollen.

## <a name="span-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>Streaming von Ressourcen oder mit geringer Dichte Texturen


*Streamingressourcen* ( in Direct3D 11 als *unterteilte Ressourcen* bezeichnet) sind große logische Ressourcen, die wenig physischen Speicher verwenden.

Eine andere Bezeichnung für Streamingressourcen ist *Texturen geringer Dichte*. „Geringe Dichte“ bezeichnet sowohl die Art der Ressourcen als auch den vielleicht wichtigsten Grund für die Kachelanordnung der Ressourcen – dass nicht alle gleichzeitig zugeordnet werden sollen. Eine Anwendung kann tatsächlich eine Streamingressource erstellen, in der absichtlich keine Daten für alle Regionen + Mips der Ressource erstellt werden. Daher kann der Inhalt selbst über eine geringe Datendichte verfügen, und die Zuordnung des Inhalts im Arbeitsspeicher des Grafikprozessors (GPU) wäre zu einem bestimmten Zeitpunkt eine Teilmenge davon (sogar mit noch geringerer Datendichte).

## <a name="span-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>Ohne Tiling, werden speicherbelegungen Subresource Granularität verwaltet


In einem Grafiksystem (d. h. einem Betriebssystem, einem Bildschirmtreiber und Grafikhardware) ohne Unterstützung von Streamingressourcen verwaltet das Grafiksystem alle Direct3D-Speicherzuordnung mit der Präzision von Unterressourcen.

Für einen [Puffer](introduction-to-buffers.md) ist der gesamte Puffer die Unterressource.

Für eine [Textur](textures.md) (z. B. [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525)) ist jede Mip-Ebene eine Unterressource; für ein Textur-Array (z. B. [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526)) ist jede Mip-Ebene an einem bestimmten Arraysegment eine Unterressource. Das Grafiksystem weist nur die Möglichkeit zur Verwaltung der Zuordnung von Vergaben bei dieser Unterressourcen-Granularität auf. Im Kontext von Streamingressourcen bezieht sich der Begriff „Zuordnung” auf die Sichtbarmachung der Daten für den Grafikprozessor.

## <a name="span-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>Ohne Tiling, kann nicht nur einen kleinen Teil der Mipmap-Kette zugegriffen.


Angenommen, einer Anwendung ist bekannt, dass ein bestimmter Renderingvorgang nur auf einen kleinen Teil eines Image-Mipmaps zugreifen muss (möglicherweise sogar nicht einmal auf den gesamten Bereich eines bestimmten Mipmaps). Im Idealfall könnte die App das Grafiksystem über diese Anforderung informieren. Das Grafiksystem würde dann nur sicherstellen, dass der benötigte Speicher dem Grafikprozessor zugeordnet wird, ohne dass zu viel Speicher eingelagert wird.

In der Praxis kann das Grafiksystem ohne Streamingressourcenunterstützung nur über den Speicher informiert werden, der auf dem Grafikprozessor bei Unterressourcengranularität (z. B. eine Reihe vollständiger Mipmap-Ebenen, auf die zugegriffen werden kann) zugeordnet werden muss. Es gibt im Grafiksystem auch keine bedarfsgesteuerte Fehlersuche; somit muss möglicherweise ein Großteil des überflüssigen Grafikprozessor-Speichers verwendet werden, um vollständige Unterressourcenzuordnungen zu erstellen, bevor ein Renderingbefehl, der auf einen Teil des Speichers verweist, ausgeführt wird. Dies ist nur eines der Probleme, die die Verwendung von großen Speicherzuordnungen in Direct3D ohne Streamingunterstützung erschwert.

## <a name="span-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>Software-Auslagerung auf die Oberfläche in kleinere Kacheln unterteilen


Die Softwareauslagerung kann für die Unterteilung der Oberfläche in Kacheln verwendet werden, die klein genug sind, um von der Hardware behandelt zu werden.

Direct3D unterstützt [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) Oberflächen mit bis zu 16384 Pixeln auf einer Seite. Ein Bild, das 16384 breit und 16384 hoch ist und 4 Byte pro Pixel aufweist, würde 1 GB Videospeicher verbrauchen (und das Hinzufügen von Mipmaps würde diese Menge verdoppeln). In der Praxis müssten die gesamten 1 GB nur selten in einem einzelnen Renderingvorgang referenziert werden.

Einige Spieleentwickler modellieren Geländeoberflächen mit Größen von bis zu 128 KB x 128 KB. Auf vorhandenen Grafikprozessoren erreichen sie dies, indem sie die Oberfläche in Kacheln unterteilen, die für die Hardware klein genug sind. Die Anwendung muss herausfinden, welche der Kacheln erforderlich sein könnten und diese in einen Zwischenspeicher von Texturen auf dem Grafikprozessor zu laden – ein Softwareauslagerungssystem.

Ein erheblicher Nachteil dieses Ansatzes stammt aus der Hardware, ohne zu wissen nichts über die Paging, das vor sich geht: Wenn ein Teil eines Images muss auf dem Bildschirm angezeigt werden, die Kacheln überspannt, die Hardware weiß nicht, wie Sie die fixed-Funktion ausführen (d. h. effiziente) filtern kachelübergreifend. Dies bedeutet, dass die Anwendung, die ihre eigene Software-Kachelunterteilung handhabt, zur manuellen Texturfilterung im Shader-Code greifen muss (dies kann sehr teuer werden, wenn ein hochwertiger anisotropischer Filter gewünscht wird); weiterhin wird möglicherweise Speicherplatz für die Erstellung von Rinnen rund um die Kacheln verschwendet, die Daten benachbarter Kacheln enthalten, damit die Hardwarefilterung mit festen Funktionen weiterhin nützlich sein kann.

## <a name="span-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>Making gekachelt Darstellung der Oberfläche Zuordnungen als vorrangige Funktion


Wenn eine gekachelte Repräsentation von Oberflächenzuordnungen ein First-Class-Feature des Grafiksystems ist, kann die Anwendung der Hardware mitteilen, welche Kacheln verfügbar gemacht werden sollen. Auf diese Weise wird weniger Grafikprozessorspeicher für die Speicherung von Oberflächenregionen verschwendet, von denen die Anwendung weiß, dass auf sie nicht zugegriffen wird, und die Hardware kann die Filterung über benachbarte Kacheln durchführen und so das Leben für Entwickler vereinfachen, die die Software-Kachelunterteilung selbst durchführen.

Für eine vollständige Lösung ist es jedoch erforderlich, die Tatsache zu berücksichtigen, dass die maximale Oberflächengröße unabhängig davon, ob auf einer Oberfläche Kacheln unterstützt werden, derzeit bei 16384 liegt – weit von den mehr als 128 K entfernt, die Anwendungen wirklich verlangen. Eine Möglichkeit dafür besteht darin, zu verlangen, dass die Hardware größere Texturen unterstützt, dies ist aber mit erheblichen Kosten und/oder Nachteilen verbunden.

Der Texturfilter- und der Renderingpfad von Direct3D sind bereits hinsichtlich der präzisen Unterstützung von 16 K-Texturen zusammen mit den anderen Anforderungen ausgelastet, wie etwa der Unterstützung von Viewport-Erweiterungen über die Oberfläche hinaus beim Rendern oder der Unterstützung des Texturumbruchs über den Rand der Oberfläche hinaus beim Filtern. Eine Möglichkeit besteht darin, einen Kompromiss zu definieren, der etwa so aussieht, dass die Texturgröße über 16 K hinaus wächst, während dann Funktionalität und oder Präzision in bestimmtem Maße beeinträchtigt werden. Aber selbst bei einem solchen Kompromiss fallen möglicherweise höhere Hardwarekosten für Adressierungsfunktionen im gesamten Hardwaresystem an, um größere Texturen zu ermöglichen.

## <a name="span-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>Problem mit großen Texturen: Genauigkeit für Speicherorte auf der Oberfläche


Eines der Probleme mit großen Texturen besteht darin, dass Fließkomma-Texturkoordinaten mit eindeutiger Präzision (und die zugehörigen Interpolatoren für die Unterstützung der Rasterisierung) nicht präzise genug sind, um Orte auf der Oberfläche korrekt angeben zu können. Dies führt zu einer „flackernden“ Texturfilterung. Eine kostspielige Option würde darin bestehen, die Unterstützung von Interpolatoren mit doppelter Präzision zu fordern, dies wäre jedoch ein zu großer Aufwand angesichts einer sinnvollen Alternative.

## <a name="span-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>Aktivieren mehrere Ressourcen aus verschiedenen Dimensionen um Arbeitsspeicher freizugeben


Ein weiteres Szenario für Streamingressourcen ist die Aktivierung mehrerer Ressourcen unterschiedlicher Abmessungen/Formate für die gemeinsame Nutzung desselben Speichers. Manchmal verfügen Anwendungen über exklusive Sätze von Ressourcen, von denen bekannt ist, dass sie nie zur selben Zeit verwendet werden müssen, oder über Ressourcen, die nur sehr kurzzeitig verwendet und dann zerstört werden, worauf dann andere Ressourcen erstellt werden. Eine allgemeine Tatsache außerhalb des Bereiches von „Streamingressourcen“ ist, dass es möglich ist, Benutzern zu gestatten, auf mehrere Ressourcen im selben (überlappenden) Speicher zu verweisen. Anders ausgedrückt: Die Erstellung und Zerstörung von „Ressourcen“ (die eine Abmessung, ein Format o. dgl. definieren) kann von der Verwaltung des Speichers getrennt werden, die den Ressourcen aus Sicht der Anwendung unterliegt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streamingressourcen](streaming-resources.md)

 

 




