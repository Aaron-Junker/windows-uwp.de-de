---
title: Zuordnungen in einen Kachelpool
description: Wenn eine Ressource als streamingressource erstellt wird, stammen die Kacheln, aus denen die Ressource besteht, aus dem Verweis auf Positionen in einem Kachel Pool. Ein Kachel Pool ist ein Arbeitsspeicher Pool (gestützt durch eine oder mehrere Zuordnungen im Hintergrund, die von der Anwendung nicht gesehen werden).
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- Zuordnungen in einen Kachelpool
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b021fc47eee6063761635422a780bb5f9f341f4d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171914"
---
# <a name="mappings-are-into-a-tile-pool"></a>Zuordnungen in einen Kachelpool


Wenn eine Ressource als streamingressource erstellt wird, stammen die Kacheln, aus denen die Ressource besteht, aus dem Verweis auf Positionen in einem Kachel Pool. Ein Kachel Pool ist ein Arbeitsspeicher Pool (gestützt durch eine oder mehrere Zuordnungen im Hintergrund, die von der Anwendung nicht gesehen werden). Das Betriebssystem und der Anzeigetreiber verwalten diesen Speicherpool, und der Speicherbedarf wird von einer Anwendung leicht verstanden. Streaming-Ressourcen ordnen 64 KB-Regionen zu, indem Sie auf Standorte in einem Kachel Pool zeigen. Ein Fehler dieses Setups besteht darin, dass mehrere Ressourcen die gleichen Kacheln freigeben und wieder verwenden können und dass die gleichen Kacheln bei Bedarf an unterschiedlichen Positionen in einer Ressource wieder verwendet werden können.

Die Kosten für die Flexibilität beim Auffüllen der Kacheln für eine Ressource aus einem Kachel Pool besteht darin, dass die Ressource die Zuordnung und Verwaltung der Kacheln im Kachel Pool die für die Ressource benötigten Kacheln definieren und verwalten muss. Kachel Zuordnungen können geändert werden. Außerdem müssen nicht alle Kacheln in einer Ressource gleichzeitig zugeordnet werden. eine Ressource kann **null** -Zuordnungen aufweisen. Eine **null** -Zuordnung definiert eine Kachel als nicht verfügbar aus der Sicht der Ressource, auf die Sie zugreift.

Es können mehrere Kachel Pools erstellt werden, und beliebig viele Streamingressourcen können einem bestimmten Kachel Pool gleichzeitig zugeordnet werden. Kachel Pools können auch vergrößert oder verkleinert werden. Weitere Informationen finden Sie unter [Ändern der Größe des Kachel Pools](tile-pool-resizing.md). Eine Einschränkung, die zur Vereinfachung der Anzeigetreiber-und Lauf Zeit Implementierung vorhanden ist, besteht darin, dass eine angegebene streamingressource nur Zuordnungen zu höchstens einem Kachel Pool aufweisen kann (im Gegensatz zu gleichzeitigen Zuordnung zu mehreren Kachel Pools).

Die Menge an Speicher, die einer streamingressource selbst (d. h. dem unabhängigen Kachel Pool Speicher) zugeordnet ist, ist ungefähr proportional zur Anzahl der Kacheln, die tatsächlich zu einem bestimmten Zeitpunkt dem Pool zugeordnet sind. In der Hardware wird dieser Fakt so skaliert, dass der Speicherbedarf für den Seitentabellen Speicher ungefähr mit der Menge der zugeordneten Kacheln skaliert wird (z. b. Wenn ein mehrstufiges Seitentabellen Schema nach Bedarf verwendet wird).

Der Kachel Pool ist eine vollständige Software Abstraktion, mit der Direct3D-Anwendungen die Seitentabellen auf der GPU (Graphics Processing Unit) effektiv programmieren können, ohne die Implementierungsdetails auf niedriger Ebene kennen zu müssen (oder direkt mit Zeiger Adressen umzugehen). Kachel Pools wenden keine weiteren Dereferenzierungsebenen auf Hardware an. Optimierungen einer Seiten Tabelle auf einer einzelnen Ebene, die Konstrukte wie Seiten Verzeichnisse verwenden, sind unabhängig vom Kachel Pool Konzept.

Wir untersuchen den Speicher, der für die Seiten Tabelle im ungünstigsten Fall erforderlich ist (in der Praxis ist es jedoch nur erforderlich, dass der Speicher ungefähr proportional zu den zugeordneten zugeordnet ist).

Angenommen, jeder Seitentabellen Eintrag ist 64 Bits.

Für den schlechtesten Fall der Seitentabellen Größe, die für eine einzelne Oberfläche getroffen wurde, wenn die Ressourcen Grenzwerte in Direct3D 11 liegen, wird angenommen, dass eine streamingressource mit einem 128-Bit-pro-Element-Format (z. b. ein RGBA-float) erstellt wird. eine Kachel mit 64 KB enthält also nur 4096 Pixel. Die maximal unterstützte [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) -Größe von 16384 \* 16384 \* 2048 (aber nur mit einer einzigen MipMap) erfordert ungefähr 1 GB Speicher in der Seiten Tabelle, wenn vollständig aufgefüllt (ohne Mipmaps) mithilfe von 64-Bit-Tabellen Einträgen. Durch das Hinzufügen von Mipmaps wird der vollständig zugeordnete Seitentabellen Speicher (ungünstigste Fall) um ungefähr eine dritte Größe von ungefähr 1,3 GB vergrößert.

Dieser Fall ermöglicht den Zugriff auf ungefähr 10,6 TB adressierbaren Arbeitsspeicher. Möglicherweise gibt es jedoch eine Beschränkung für die Menge des adressierbaren Speichers, wodurch diese Beträge reduziert werden, vielleicht um den Terabytebereich.

Ein weiterer Fall, der berücksichtigt werden muss, ist eine einzelne [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) -streamingressource von 16384 \* 16384 mit einem 32-Bit-pro-Element-Format, einschließlich Mipmaps. Der Speicherplatz, der in einer vollständig aufgefüllten Seiten Tabelle benötigt wird, beträgt ungefähr 170 KB mit 64-Bit-Tabellen Einträgen.

Sehen Sie sich abschließend ein Beispiel mit einem BC-Format an, z. b. bzw bc7 mit 128 Bits pro Kachel mit 4 x 4 Pixeln. Das ist ein Byte pro Pixel. Ein [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) von 16384 \* 16384 \* 2048 einschließlich Mipmaps erfordert ungefähr 85 MB, um diesen Speicher vollständig in einer Seiten Tabelle aufzufüllen. Das ist nicht schlecht, wenn Sie dies in Erwägung ziehen, kann eine streamingressource 550 gigapixels (in diesem Fall 512 GB Arbeitsspeicher) umfassen.

In der Praxis würden diese vollständigen Zuordnungen nicht in naher Nähe definiert werden, da die Menge des verfügbaren physischen Speichers nicht zulässt, dass eine Menge an einem bestimmten Punkt zugeordnet wird. Bei einem Kachel Pool können sich Anwendungen jedoch für die Wiederverwendung von Kacheln entscheiden (als einfaches Beispiel für die Wiederverwendung einer "schwarzen" farbigen Kachel für große schwarze Bereiche in einem Bild), indem der Kachel Pool (d. h. Seitentabellen Zuordnungen) als Tool für die Speicher Komprimierung verwendet wird.

Der anfängliche Inhalt der Seiten Tabelle ist für alle Einträge **null** . Anwendungen können auch keine anfänglichen Daten für den Speicherinhalt der Oberfläche übergeben, da Sie ohne Arbeitsspeicher Unterstützung gestartet werden.

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
<td align="left"><p><a href="tile-pool-creation.md">Erstellung eines Kachelpools</a></p></td>
<td align="left"><p>Anwendungen können einen oder mehrere Kachel Pools pro Direct3D-Gerät erstellen. Die Gesamtgröße jedes Kachel Pools ist auf die Ressourcen Größenbeschränkung von Direct3D 11 beschränkt, was ungefähr 1/4 von GPU-RAM liegt.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-pool-resizing.md">Ändern der Größe des Kachelpools</a></p></td>
<td align="left"><p>Ändern Sie die Größe eines Kachel Pools, um einen Kachel Pool zu vergrößern, wenn die Anwendung für die streamingressourcenzuordnung einen größeren Workingset benötigt oder wenn weniger Speicherplatz benötigt wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="hazard-tracking-versus-tile-pool-resources.md">Gefahrennachverfolgung im Vergleich mit den Kachelpoolressourcen</a></p></td>
<td align="left"><p>Für Ressourcen, die keine Streaming-Ressourcen sind, kann Direct3D bestimmte Gefahren Bedingungen während des Renderings verhindern, aber da die Gefahren Verfolgung auf Kachel Ebene für Streamingressourcen erfolgen würde, kann das Nachverfolgen von Gefahren Zuständen beim Rendern von Streamingressourcen zu teuer sein</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Erstellen von Streamingressourcen](creating-streaming-resources.md)

 

 