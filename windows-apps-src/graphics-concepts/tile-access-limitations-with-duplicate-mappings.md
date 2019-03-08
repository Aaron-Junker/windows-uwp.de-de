---
title: Kachelzugriffseinschränkungen bei doppelten Zuordnungen
description: Bei doppelten Zuordnungen gibt es Kachelzugriffseinschränkungen, wie z. B. beim Kopieren von Streamingressourcen mit Quellen- und Zielüberlappung oder beim Rendern von Kacheln innerhalb des Bereichs Rendern freigegeben.
ms.assetid: 6E40B1DC-BCF1-4B09-82A8-7B2D9B209A61
keywords:
- Kachelzugriffseinschränkungen bei doppelten Zuordnungen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d5563a9909ba3d6cb3deaae43bcf9e55b4b2c880
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607965"
---
# <a name="tile-access-limitations-with-duplicate-mappings"></a>Kachelzugriffseinschränkungen bei doppelten Zuordnungen


Bei doppelten Zuordnungen gibt es Kachelzugriffseinschränkungen, wie z. B. beim Kopieren von Streamingressourcen mit Quellen- und Zielüberlappung oder beim Rendern von Kacheln innerhalb des Bereichs Rendern freigegeben.

## <a name="span-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspancopying-streaming-resources-with-overlapping-source-and-destination"></a><span id="Copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="COPYING_STREAMING_RESOURCES_WITH_OVERLAPPING_SOURCE_AND_DESTINATION"></span>Kopieren die streaming-Ressourcen mit sich überschneidenden Quelle und Ziel


Wenn Kacheln in den Bereich für Quelle und Ziel einer Kopie\* Vorgang haben Zuordnungen in der Kopierbereich, der überlappende haben würde, auch wenn beide Ressourcen keine Ressourcen und die Kopie streaming wurden dupliziert\* -Vorgang unterstützt überlappende kopiert, die Kopie\* Vorgang verhält (wie bei die Quelle in ein temporäres Verzeichnis kopiert wird, vor dem Wechsel in das Ziel) einwandfrei. Wenn die Überlappung nicht offensichtlich ist (bspw. sind die Quell- und Zielressourcen unterschiedlich, teilen jedoch Zuordnungen oder Zuordnungen werden über eine bestimmte Oberfläche dupliziert), sind Ergebnisse des Kopiervorgangs auf den Kacheln, die freigegeben werden, nicht definiert.

## <a name="span-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspancopying-to-streaming-resource-with-duplicated-tiles-in-destination-area"></a><span id="Copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="COPYING_TO_STREAMING_RESOURCE_WITH_DUPLICATED_TILES_IN_DESTINATION_AREA"></span>In der streaming-Ressource mit doppelt vorhandenen Kacheln im Zielbereich kopiert


Das Kopieren auf eine Streamingressource mit duplizierten Kacheln im Zielbereich erzeugt nicht definierte Ergebnisse in diesen Kacheln, es sei denn, die Daten selbst sind identisch. Verschiedene Kacheln können die Kacheln in einer anderen Reihenfolge schreiben.

## <a name="span-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanuav-accesses-to-duplicate-tiles-mappings"></a><span id="UAV_accesses_to_duplicate_tiles_mappings"></span><span id="uav_accesses_to_duplicate_tiles_mappings"></span><span id="UAV_ACCESSES_TO_DUPLICATE_TILES_MAPPINGS"></span>UAV greift auf doppelte Kacheln Zuordnungen auf.


Nehmen wir an, dass eine unsortierte Access-Ansicht (UAV) auf einer Streamingressource doppelte Kachelzuordnungen in ihrem Gebiet oder mit anderen Ressourcen, die an die Pipeline gebunden sind, hat. Die Sortierung der Zugriffe auf diese dupliziert Kacheln ist nicht definiert, wenn sie durch verschiedene Threads durchgeführt wird. Jegliche Sortierung des Speicherzugriffs auf UAVs ist allgemein nicht sortiert.

## <a name="span-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanrendering-after-tile-mapping-changes-or-content-updates-from-outside-mappings"></a><span id="Rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="RENDERING_AFTER_TILE_MAPPING_CHANGES_OR_CONTENT_UPDATES_FROM_OUTSIDE_MAPPINGS"></span>Rendering nach Kachel Zuordnung Änderungen oder Updates von Zuordnungen von externen Inhalten


Wenn ein streaming der Ressource Kachel Zuordnungen wurden geändert, oder Inhalte in zugeordneten gekachelten poolkacheln über eine andere streaming Ressource Zuordnungen und die streaming-Ressource geändert haben, ist Ansicht des Renderziels über gerendert werden soll oder Ansicht der tiefenschablone, die Anwendung muss Deaktivieren Sie (mit der fixed-Funktion Clear-APIs) oder vollständig kopieren Sie mithilfe einer Kopie\*/Update\* APIs, wenn die Kacheln, die innerhalb des Bereichs geändert haben (zugeordnet oder nicht) gerendert wird.

Scheitern der Anwendung beim Deaktivierungs- oder Kopiervorgang führt in diesen Fällen dazu, dass die Hardware-Optimierungsstrukturen für die angegebene Renderzielansicht oder Tiefenschablonen-Ansicht veralten. Außerdem führt es zu Garbage-Renderergebnisse und zu Inkonsistenzen in verschiedener Hardware. Diese ausgeblendeten Optimierungsdatenstrukturen, die von der Hardware verwendet werden, befinden sich möglicherweise lokal auf einzelne Zuordnungen und sind nicht für Zuordnungen zum gleichen Arbeitsspeicher sichtbar.

Wenn Sie eine Ressourcenansicht (durch Festlegen aller Elemente in einer Ressourcenansicht auf einen Wert) deaktivieren, können Sie Renderzielansichten mithilfe von Rechtecken deaktivieren. Für Hardware, das Streamingressourcen unterstützt, muss das Deaktivieren einer Ressourcenansicht auch das Deaktivieren von Tiefenschablonenansichten ausschließlich für Oberflächen (ohne Schablone) mithilfe von Rechtecken unterstützen. Dieser Vorgang ermöglicht es der Anwendung, nur den erforderlichen Bereich einer Oberfläche zu deaktivieren.

Wenn eine Anwendung den bestehenden Speicherinhalt der Bereiche in einer Streamingressource beibehalten muss, in denen Zuordnungen geändert wurden, muss diese Anwendungen die klare Anforderung umgehen. Die Anwendung kann die Anforderung erfolgreich umgehen, indem der Inhalt zunächst in den Bereichen gespeichert wird, in denen Kachelzuordnungen (durch das Kopieren in eine temporäre Oberfläche) geändert wurden, der erforderliche Deaktivierungsbefehl gegeben wird und der Inhalt zurück kopiert wird. Während dies die Aufgabe des Beibehalten von Oberflächeninhalten für das inkrementelle Rendern ausführen würde, besteht der Nachteil darin, dass nachfolgende Renderingleistung auf der Oberfläche beeinträchtigt werden kann, da Renderoptimierungen möglicherweise verloren gehen.

Wenn eine Kachel gleichzeitig in mehreren Streamingressourcen zugeordnet ist und der Kachelinhalt auf irgendeine Weise (Rendern, Kopieren und so weiter) über eine der Streamingressourcen geändert wurden, und die gleiche Kachel über eine andere Streamingressource gerendert wird, muss zunächst wie zuvor beschrieben die Kachel gelöscht werden.

## <a name="span-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanrendering-to-tiles-shared-outside-render-area"></a><span id="Rendering_to_tiles_shared_outside_render_area"></span><span id="rendering_to_tiles_shared_outside_render_area"></span><span id="RENDERING_TO_TILES_SHARED_OUTSIDE_RENDER_AREA"></span>Rendering an Kacheln, die außerhalb von freigegebenen Bereich zu rendern


Nehmen Sie an, dass ein Bereich in einer Streamingressource gerendert wird und die Poolkacheln, die vom Renderbereich verwiesen werden, auch von außerhalb des Renderbereichs zugeordnet werden (auch über andere Streamingressourcen, zur gleichen Zeit oder nicht). Die Daten, die für diese Kacheln gerendert werden, werden möglicherweise nicht richtig über die anderen Zuordnungen angezeigt, obwohl das zugrunde liegende Speicherlayout kompatibel ist. Dies ergibt sich aus der Tatsache, dass die Optimierungsdatenstrukturen, die manche Hardware-Typen verwenden, auf einzelne lokale Zuordnungen für darstellbare Oberflächen sein können und möglicherweise für andere Zuordnungen an demselben Speicherort nicht angezeigt werden.

Sie können diese Einschränkung umgehen, indem Sie von der gerenderten Zuordnung auf alle anderen Zuordnungen des gleichen Speichers kopieren, auf dem zugegriffen werden kann (oder indem Sie den Arbeitsspeicher löschen oder andere Daten auf den Speicher kopieren, wenn der alte Inhalt nicht mehr benötigt wird). Während diese Umgehung redundant erscheint, ermöglicht es allen anderen Zuordnungen des gleichen Speichers den ordnungsgemäßen Zugriff auf seine Inhalte. Zumindest bleiben die Speichereinsparungen mit nur einem einzelnen physischen Speichersicherung erhalten.

Außerdem, wenn Sie zwischen verschiedenen Streamingressourcen wechseln, die Zuordnungen teilen (es sei denn, es sind nur lesbare), müssen Sie eine Einschränkung der Datenzugriffsreihenfolge (eine Barriere) zwischen Ressourcen mit mehreren Kacheln und zwischen den Switches angeben.

## <a name="span-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanrendering-to-tiles-shared-within-render-area"></a><span id="Rendering_to_tiles_shared_within_render_area"></span><span id="rendering_to_tiles_shared_within_render_area"></span><span id="RENDERING_TO_TILES_SHARED_WITHIN_RENDER_AREA"></span>Rendern in das freigegebene innerhalb Bereichs, der Rendering Kacheln


Wenn ein Bereich in einer Streamingressource gerendert zu und innerhalb des Renderbereichs gerendert wird und mehrere Kacheln am selben Speicherort des Kachelpools zugeordnet werden, sind die Renderingergebnisse auf diesen Kacheln nicht definiert.

## <a name="span-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspandata-compatibility-across-streaming-resources-sharing-tiles"></a><span id="Data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="DATA_COMPATIBILITY_ACROSS_STREAMING_RESOURCES_SHARING_TILES"></span>Kompatibilität der Daten für das streaming von Ressourcen freigeben von Kacheln


Nehmen wir an, dass mehrere Streamingressourcen Zuordnungen auf die gleichen Speicherorte der Kachelpools haben und jede Ressource dazu verwendet wird, um auf dieselben Daten zuzugreifen. Dieses Szenario gilt nur, wenn die anderen Regeln zum Vermeiden von Problemen mit Hardwareoptimierungsstrukturen vermieden werden, entsprechende Hindernisse angegeben werden (Angeben einer Einschränkung der Datenzugriffsreihenfolge zwischen Ressourcen mit mehreren Kacheln) und die Streamingressourcen miteinander kompatibel sind.

Die zweite Option wird hier in Bezug auf die Bedeutung für die Streamingressourcen beschrieben, die Kacheln freigeben, um inkompatibel zu sein. Die Inkompatibilitätsbedingungen für den Zugriff auf dieselben Daten über doppelte Kachelzuordnungen ergeben sich aus der Verwendung der verschiedenen Oberflächenabmessungen oder des Formats, oder aus Differenzen der Bindungskennzeichen für Renderziele oder Tiefenschablonen auf den Ressourcen. Das Schreiben im Speicher mit einem Zuordnungstyp erzeugt undefinierte Ergebnisse, wenn Sie anschließend über eine Zuordnung von einer nicht kompatiblen Ressource lesen oder rendern.

Wenn die anderen freigegebenen Ressourcenzuordnungen zuerst mit neuen Daten initialisiert werden (Wiederverwendung des Speichers für einen anderen Zweck), ist der nachfolgende Lese- oder Rendervorgang geeignet, da Daten nicht über inkompatible Interpretationen übertragen werden. Beim Wechseln zwischen den Zugriffen auf inkompatible Zuordnungen wie hier müssen Sie jedoch Barrieren angeben (Angeben einer Einschränkung der Datenzugriffsreihenfolge zwischen Ressourcen mit mehreren Kacheln).

Wenn das Bindungskennzeichen für das Renderziel oder die Tiefenschablone nicht auf eine der Ressourcen festgelegt ist, die miteinander Zuordnungen teilen, gibt es sehr viel weniger Einschränkungen. Solange die Format- und Surface-Typen (z. B. Texture2D) identisch sind, können Kacheln freigegeben werden. Verschiedene Formate, die kompatibel sind Fälle, z. B. BC\* Oberflächen und die entsprechende Größe nicht komprimierte 32-Bit- oder 16 Bit pro Komponentenformat, wie z. B. BC6H und R32G32B32A32. Viele 32-Bit pro Element in Formaten möglich Alias R32 Entwickelt bei\_ \* auch (R10G10B10A2\_\*, R8G8B8A8\_\*, B8G8R8A8\_\*, B8G8R8X8\_ \*, R16G16\_\*); dieser Vorgang wurde immer zulässig, für nicht-streaming-Ressourcen.

Das Freigeben zwischen verpackten und nicht verpackten Kacheln ist geeignet, wenn die Formate kompatibel und die Kacheln mit Volltonfarbe gefüllt sind.

Wenn die Zuordnungen der Ressourcenfreigabekachel keine Ähnlichkeiten aufweisen, abgesehen davon, dass keine Bindungskennzeichen für Renderziel oder Tiefenschablone festgelegt wurden, kann nur mit 0 gefüllter Speicher problemlos geteilt werden. Die Zuordnung wird als das angezeigt, was aus 0 für die Definition des gegebenen Ressourcenformats (in der Regel nur 0) dekodiert wird.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipeline-Zugriff auf Ressourcen streaming](pipeline-access-to-streaming-resources.md)

 

 




