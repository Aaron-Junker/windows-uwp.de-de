---
title: Die Notwendigkeit für Streamingressourcen
description: Streaming-Ressourcen sind erforderlich, damit GPU-Speicher keine Bereiche speichert, auf die nicht zugegriffen werden kann, und die Hardware darüber informiert, wie Sie über angrenzende Kacheln filtern.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- Die Notwendigkeit für Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b2e95427be7cdae450e60317d13765bb9372e84b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173024"
---
# <a name="the-need-for-streaming-resources"></a>Die Notwendigkeit für Streamingressourcen


Streaming-Ressourcen sind erforderlich, damit GPU-Speicher keine Bereiche speichert, auf die nicht zugegriffen werden kann, und die Hardware darüber informiert, wie Sie über angrenzende Kacheln filtern.

## <a name="span-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>Streaming-Ressourcen oder Sparse-Texturen


*Streaming-Ressourcen* (als *gekachelte Ressourcen* in Direct3D 11 bezeichnet) sind große logische Ressourcen, die kleine Mengen an physischem Arbeitsspeicher verwenden.

Ein weiterer Name für Streamingressourcen sind *Sparse-Texturen*. "Sparse" vermittelt sowohl die gekachelte Natur der Ressourcen als auch den Hauptgrund dafür, dass Sie nicht alle gleichzeitig zugeordnet werden sollen. Tatsächlich könnte eine Anwendung eine streamingressource erstellen, in der keine Daten für alle Regionen und MIPS der Ressource absichtlich erstellt werden. Der Inhalt selbst könnte geringer sein, und die Zuordnung des Inhalts in GPU (Graphics Processing Unit)-Speicher zu einem bestimmten Zeitpunkt wäre eine Teilmenge dieser (noch stärker geringer).

## <a name="span-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>Ohne Tich werden Speicher Belegungen bei der Granularität der untergeordneten Quellen verwaltet.


In einem Grafiksystem (d. h. Betriebssystem, Anzeigetreiber und Grafikhardware) ohne Unterstützung für Streamingressourcen verwaltet das Grafiksystem alle Direct3D-Speicher Belegungen in der untergeordneten Quell Granularität.

Bei einem [Puffer](introduction-to-buffers.md)ist der gesamte Puffer die untergeordnete Quelle.

Bei einer [Textur](textures.md)(z. b. [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d)) ist jede MIP-Ebene eine untergeordnete Quelle. bei einem Textur Array (z. b. [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray)) ist jede MIP-Ebene in einem angegebenen Array Slice eine untergeordnete Quelle. Das Grafiksystem macht nur die Möglichkeit zur Verwaltung der Zuordnung von Zuordnungen in dieser unter Quell Granularität verfügbar. Im Kontext von Streamingressourcen bezieht sich "Mapping" darauf, dass Daten für die GPU sichtbar sind.

## <a name="span-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>Ohne Tick kann nicht auf einen kleinen Teil der MipMap-Kette zugreifen.


Angenommen, eine Anwendung weiß, dass ein bestimmter Renderingvorgang nur auf einen kleinen Teil der MipMap-Kette des Bilds zugreifen muss (möglicherweise nicht auf den vollständigen Bereich einer bestimmten MipMap). Im Idealfall könnte die APP das Grafiksystem über diesen Bedarf informieren. Das Grafiksystem würde dann nur dann sicherstellen, dass der benötigte Arbeitsspeicher auf der GPU ohne Paging in zu viel Arbeitsspeicher zugeordnet wird.

Tatsächlich kann das Grafiksystem ohne Unterstützung für Streamingressourcen nur über den Arbeitsspeicher informiert werden, der der GPU bei der Granularität der unter Ressourcen zugeordnet werden muss (z. b. ein Bereich von vollständigen MipMap-Ebenen, auf die zugegriffen werden kann). Es gibt auch keinen Anforderungs Fehler im Grafiksystem. Daher muss möglicherweise viel überschüssiger GPU-Speicher verwendet werden, um vollständige unter Ressourcen vor einem Rendering-Befehl, der auf einen beliebigen Teil des Arbeitsspeichers verweist, zugeordnet zu werden. Dies ist nur ein Problem, das die Verwendung von großen Speicher Belegungen in Direct3D ohne Unterstützung von Streamingressourcen erschwert.

## <a name="span-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>Software Auslagerung zum Aufteilen der Oberfläche in kleinere Kacheln


Software Paginierung kann verwendet werden, um die Oberfläche in Kacheln zu unterbrechen, die klein genug für die zu behandelnde Hardware sind.

Direct3D unterstützt [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) -Oberflächen mit bis zu 16384 Pixeln auf einer angegebenen Seite. Ein Bild, das 16384 breit und 4 Bytes pro Pixel ist, würde 1 GB Videospeicher beanspruchen (und das Hinzufügen 16384 von Mipmaps würde diesen Betrag verdoppeln). In der Praxis müssen in einem einzelnen Renderingvorgang nur selten auf alle 1 GB verwiesen werden.

Einige Spielentwickler modellieren die Gelände Flächen so groß wie 128 KB und 128 KB. Die Vorgehensweise zum Arbeiten mit vorhandenen GPUs besteht darin, die Oberfläche in Kacheln zu unterbrechen, die klein genug für die Verarbeitung sind. Die Anwendung muss herausfinden, welche Kacheln benötigt werden, und Sie in einen Zwischenspeicher zwischen den Texturen auf der GPU laden, einem Software-Paging-System.

Ein wesentlicher Nachteil dieses Ansatzes ist, dass die Hardware nicht weiß, was das Paging betrifft: Wenn ein Teil eines Images auf dem Bildschirm angezeigt werden muss, der Kacheln durchläuft, weiß die Hardware nicht, wie eine fixierte Funktion (d. h. effizient) gefiltert werden kann. Dies bedeutet, dass die Anwendung, die ihre eigene Software-tiult verwaltet, auf die manuelle Textur Filterung im Shader-Code (der sehr teuer wird, wenn ein guter qualitätsfilterter Filter erwünscht ist) und/oder das vergeuden von Speicher Erstellungs vorspielen bei Kacheln, die Daten aus benachbarten Kacheln enthalten, eine gewisse Unterstützung bieten kann.

## <a name="span-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>Erstellen einer Kachel Darstellung von Oberflächen Zuordnungen zu einer erstklassigen Funktion


Wenn eine gekachelte Darstellung von Oberflächen Zuordnungen eine erstklassige Funktion im Grafiksystem ist, könnte die Anwendung der Hardware mitteilen, welche Kacheln verfügbar gemacht werden sollen. Auf diese Weise verschwendet weniger GPU-Speicher die Speicherung von Oberflächen Bereichen, auf die die Anwendung keinen Zugriff hat, und die Hardware kann verstehen, wie Sie auf angrenzenden Kacheln filtern, um einige der Probleme zu beseitigen, die Entwickler haben, die Software-tichen selbst durchführen.

Um jedoch eine komplette Lösung bereitzustellen, müssen Sie sich mit der Tatsache befassen, dass unabhängig davon, ob die tik innerhalb einer Oberfläche unterstützt wird, die Oberfläche für die Oberfläche derzeit 16384 ist. Es ist ein Ansatz, nur die Hardware zu benötigen, um größere Textur Größen zu unterstützen. es gibt jedoch beträchtliche Kosten und/oder Kompromisse bei dieser Route.

Der Direct3D's-Textur Filter Pfad und der Renderingpfad sind bereits in Bezug auf die Genauigkeit bei der Unterstützung von 16K-Texturen mit den anderen Anforderungen erschöpft, wie z. b. das unterstützen von Viewport-Blöcken, die während des Renderings auf die Oberfläche fallen, oder das Eine Möglichkeit besteht darin, einen Kompromiss so zu definieren, dass die Textur-und Genauigkeits Größe in gewisser Weise erhöht wird, wenn die Textur Größe über 16K steigt. Auch bei dieser Gewährung sind möglicherweise zusätzliche Hardwarekosten in Bezug auf die Adressierungs Fähigkeit im gesamten Hardwaresystem erforderlich, um zu größeren Textur Größen zu wechseln.

## <a name="span-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>Problem mit großen Texturen: Genauigkeit für Standorte auf der Oberfläche


Ein Problem, das im Zusammenhang mit Texturen auftritt, liegt darin, dass die Texturkoordinaten mit einer Gleit Komma Zahl mit einfacher Genauigkeit (und die zugehörigen Interpolatoren zur Unterstützung der rasterisierung) nicht mehr präzise sind, um Orte auf der Oberfläche genau anzugeben. Die Textur Filterung von Jittery würde folgen. Eine teure Option wäre die Unterstützung von Interpolatoren mit doppelter Genauigkeit, die jedoch aufgrund einer angemessenen Alternative überschrieben werden könnte.

## <a name="span-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>Aktivieren mehrerer Ressourcen verschiedener Dimensionen für die Freigabe von Arbeitsspeicher


Ein weiteres Szenario, das durch Streaming-Ressourcen möglich wäre, besteht darin, dass mehrere Ressourcen mit unterschiedlichen Dimensionen/Formaten den gleichen Arbeitsspeicher gemeinsam verwenden. Manchmal haben Anwendungen exklusive Ressourcen Sätze, die bekanntermaßen nicht gleichzeitig verwendet werden sollen, oder Ressourcen, die nur für eine sehr kurze Verwendung erstellt und dann zerstört wurden, gefolgt von der Erstellung anderer Ressourcen. Eine Form der Generalität, die von "Streamingressourcen" abweichen kann, besteht darin, dass es möglich ist, dem Benutzer die Möglichkeit zu geben, mehrere verschiedene Ressourcen auf denselben (überlappenden) Speicher zu verweisen. Anders ausgedrückt: die Erstellung und Zerstörung von "Resources" (die eine Dimension/ein Format definieren usw.) kann von der Verwaltung des Arbeitsspeichers, der den Ressourcen zugrunde liegt, von der Sicht der Anwendung entkoppelt werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streamingressourcen](streaming-resources.md)

 

 