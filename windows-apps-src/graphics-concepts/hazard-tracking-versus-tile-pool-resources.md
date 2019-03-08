---
title: Gefahrennachverfolgung im Vergleich mit den Kachelpoolressourcen
description: Bei Nicht-Streaming-Ressourcen kann Direct3D bestimmte Gefahrenbedingungen während der Wiedergabe verhindern, da aber die Nachverfolgung von Gefahren auf einer Kachelebene für Streaming-Ressourcen erfolgen würde, kann das Nachverfolgen von Gefahrenbedingungen während der Wiedergabe von Streaming-Ressourcen möglicherweise zu teuer sein.
ms.assetid: 8B0C73D3-3F77-41E8-B17D-C595DEE39E49
keywords:
- Gefahrennachverfolgung im Vergleich mit den Kachelpoolressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4dec176206aacb946bfd65341c483d8ba61558ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629745"
---
# <a name="hazard-tracking-versus-tile-pool-resources"></a>Gefahrennachverfolgung im Vergleich mit den Kachelpoolressourcen


Bei Nicht-Streaming-Ressourcen kann Direct3D bestimmte Gefahrenbedingungen während der Wiedergabe verhindern, da aber die Nachverfolgung von Gefahren auf einer Kachelebene für Streaming-Ressourcen erfolgen würde, kann das Nachverfolgen von Gefahrenbedingungen während der Wiedergabe von Streaming-Ressourcen möglicherweise zu teuer sein.

Beispielsweise erlaubt die Runtime für Nicht-Streamingressourcen nicht, dass eine Unterressource gleichzeitig als Eingabe (z. B. als Shaderressourcenansicht) und als Ausgabe (z. B. als Renderzielansicht) gebunden wird. In einem solchen Fall würde die Runtime die Bindung der Eingabe aufheben. Dieser Nachverfolgungsaufwand in der Runtime ist gering und erfolgt auf der Ebene von Unterressourcen. Einer der Vorteile dieses durch die Nachverfolgung bedingten Mehraufwands ist die Minimierung der Wahrscheinlichkeit, dass Apps versehentlich von der Ausführungsreihenfolge der Hardwareshader abhängig sind. Die Ausführungsreihenfolge der Hardwareshader kann variieren – wenn nicht für eine bestimmte GPU (Graphics Processing Unit), so doch bestimmt für verschiedene GPUs.

Nachzuverfolgen, wie Ressourcen gebunden sind, kann für Streamingressourcen zu aufwändig sein, da die Überwachung auf Tile-Ebene erfolgt. Und es können weitere Probleme auftreten. Möglichweise müssen Versuche unterbunden werden, in eine Render Target View zu rendern, wenn eine Tile gleichzeitig mehreren Bereichen der Oberfläche zugeordnet ist. Wenn diese Konfliktnachverfolgung pro Tile für die Runtime zu aufwändig ist, wäre das eine Option für die Debug-Ebene.

Eine Anwendung, die mit einem Schreib-oder Lesevorgang auf eine Streamingressource zugreift, die den Tile-Pool-Speicher referenziert, der auch von anderen Streamingressourcen in anschließenden Schreib- oder Lesevorgängen referenziert wird, muss den Grafiktreiber darüber informieren, dass sie erwartet, den ersten Vorgang abschließen zu können, bevor die folgenden Vorgänge beginnen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Zuordnungen sind in einem Pool für die Kachel "](mappings-are-into-a-tile-pool.md)

 

 




