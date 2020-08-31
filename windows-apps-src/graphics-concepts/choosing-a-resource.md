---
title: Auswählen einer Ressource
description: Erfahren Sie, wie Sie die besten Ressourcen ermitteln und auswählen, die an verschiedene Phasen der 3D-Grafik Pipeline in Ihrer Anwendung gebunden werden.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- Auswählen einer Ressource
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69527915988136854e201b82332c17f3e0134290
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094468"
---
# <a name="choosing-a-resource"></a>Auswählen einer Ressource


Eine Ressource ist eine Sammlung von Daten, die von der 3D-Pipeline verwendet wird. Das Erstellen von Ressourcen und das Definieren Ihres Verhaltens ist der erste Schritt beim Programmieren Ihrer Anwendung. Dieses Handbuch behandelt grundlegende Themen zum Auswählen der Ressourcen, die für Ihre Anwendung erforderlich sind.

## <a name="span-ididentify_bindingspanspan-ididentify_bindingspanspan-ididentify_bindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>Identifizieren von Pipeline Stufen, die Ressourcen benötigen


Der erste Schritt besteht darin, die [Grafik Pipeline](graphics-pipeline.md) Stufe (oder die Stufen) auszuwählen, die eine Ressource verwenden werden. Das heißt, jede Phase, mit der Daten aus einer Ressource gelesen werden, sowie die Phasen, die Daten in eine Ressource schreiben, werden identifiziert. Wenn Sie die Pipeline Stufen kennen, in denen die Ressourcen verwendet werden, bestimmt die APIs, die aufgerufen werden, um die Ressource an die Stufe zu binden.

In dieser Tabelle sind die Ressourcentypen aufgeführt, die an die einzelnen Pipeline Stufen gebunden werden können. Er gibt an, ob die Ressource als Eingabe oder Ausgabe gebunden werden kann.

| Pipeline Phase  | Ein/Aus | Ressource               | Ressourcentyp                           |
|-----------------|--------|------------------------|-----------------------------------------|
| Eingabe-Assembler | In     | Vertex-Puffer          | Buffer                                  |
| Eingabe-Assembler | In     | Indexpuffer           | Buffer                                  |
| Shaderphasen   | In     | Shader-resourceview    | Buffer, Texture1D, Texture2D, Texture3D |
| Shaderphasen   | In     | Shader-konstanter Puffer | Buffer                                  |
| Streamausgabe   | aus    | Buffer                 | Buffer                                  |
| Ausgabezusammenführung   | aus    | Renderzielansicht     | Buffer, Texture1D, Texture2D, Texture3D |
| Ausgabezusammenführung   | aus    | Ansicht "Tiefe/Schablone"     | Texture1D, Texture2D                    |

 

## <a name="span-ididentify_usagespanspan-ididentify_usagespanspan-ididentify_usagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>Ermitteln, wie die einzelnen Ressourcen verwendet werden


Nachdem Sie die Pipeline Stufen ausgewählt haben, die von der Anwendung verwendet werden (und damit die Ressourcen, die in den einzelnen Phasen benötigt werden), besteht der nächste Schritt darin, zu bestimmen, wie die einzelnen Ressourcen verwendet werden sollen, d. h. ob die CPU oder die GPU auf eine Ressource zugreifen kann.

Die Hardware, auf der Ihre Anwendung ausgeführt wird, hat mindestens mindestens eine CPU und eine GPU. Um einen Verwendungs Wert auszuwählen, berücksichtigen Sie den Prozessortyp, der von den folgenden Optionen gelesen oder in die Ressource geschrieben werden muss.

| Resource Usage | Kann aktualisiert werden von                    | Aktualisierungshäufigkeit |
|----------------|--------------------------------------|---------------------|
| Standard        | GPU                                  | selten        |
| Dynamisch        | CPU                                  | wieder          |
| Staging        | GPU                                  | –                 |
| Unveränderlich      | CPU (nur zum Zeitpunkt der Erstellung der Ressource) | –                 |

 

Die Standardverwendung sollte für eine Ressource verwendet werden, bei der erwartet wird, dass Sie von der CPU seltener aktualisiert wird (weniger als einmal pro Frame). Im Idealfall würde die CPU niemals direkt in eine Ressource mit Standardverwendung schreiben, um potenzielle Leistungseinbußen zu vermeiden.

Dynamische Verwendung sollte für eine Ressource verwendet werden, die die CPU relativ häufig (einmal oder mehr pro Frame) aktualisiert. Ein typisches Szenario für eine dynamische Ressource besteht darin, dynamische Vertex-und Index Puffer zu erstellen, die zur Laufzeit mit Daten über die Geometrie aufgefüllt werden, die aus der Sicht des Benutzers für jeden Frame sichtbar sind. Diese Puffer werden verwendet, um nur die Geometrie zu erzeugen, die für den Benutzer für diesen Frame sichtbar ist.

Die Verwendung von Staging sollte zum Kopieren von Daten in und aus anderen Ressourcen verwendet werden. Ein typisches Szenario ist das Kopieren von Daten in eine Ressource mit der Standard Auslastung (auf die die CPU nicht zugreifen kann) auf eine Ressource mit der stagingverwendung (auf die die CPU zugreifen kann).

Unveränderliche Ressourcen sollten verwendet werden, wenn sich die Daten in der Ressource nie ändern.

Eine andere Möglichkeit, dieselbe Idee zu betrachten, besteht darin, zu überprüfen, was eine Anwendung mit einer Ressource bewirkt.

| Verwendung der Ressource durch die Anwendung     | Resource Usage       |
|---------------------------------------|----------------------|
| Einmal laden und nie aktualisieren            | Unveränderlich oder Standard |
| Anwendung füllt Ressourcen wiederholt | Dynamisch              |
| In Textur Rendering                     | Standard              |
| CPU-Zugriff auf GPU-Daten                | Staging              |

 

Wenn Sie nicht sicher sind, welche Verwendung Sie auswählen, sollten Sie mit der standardmäßigen Verwendung beginnen, da es sich um den gängigsten Fall handelt. Ein Shader-Constant-Puffer ist der einzige Ressourcentyp, der immer die Standardverwendung aufweisen sollte.

## <a name="span-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>Binden von Ressourcen an Pipeline Stufen


Eine Ressource kann gleichzeitig an mehrere Pipeline Phasen gebunden werden, solange die beim Erstellen der Ressource angegebenen Einschränkungen erfüllt sind. Diese Einschränkungen werden als nutzungsflags, Bindungsflags oder CPU-Zugriffsflags angegeben. Genauer gesagt kann eine Ressource als Eingabe und als Ausgabe gleichzeitig gebunden werden, solange das Lesen und Schreiben eines Teils einer Ressource nicht gleichzeitig erfolgen kann.

Beim Binden einer Ressource sollten Sie sich Gedanken darüber machen, wie die GPU und die CPU auf die Ressource zugreifen. Ressourcen, die für einen einzigen Zweck entwickelt wurden (verwenden Sie nicht mehrere Nutzungs-, Bindungs-und CPU-Zugriffsflags), führen zu einer besseren Leistung.

Nehmen Sie beispielsweise an, dass die Groß-/Kleinschreibung eines Renderziels mehrmals verwendet wird. Es kann schneller sein, dass zwei Ressourcen vorhanden sind: ein Renderziel und eine Textur, die als Shaderressource verwendet wird. Jede Ressource verwendet nur ein Bindungsflag, das "Renderziel" oder "Shader-Ressource" angibt. Die Daten werden aus der renderzieltextur in die Shader-Textur kopiert.

Diese Vorgehensweise in diesem Beispiel kann die Leistung verbessern, indem Sie den Renderziel-Schreibvorgang aus dem Shader-Texture-Lesevorgang isolieren. Die einzige Möglichkeit besteht darin, beide Ansätze zu implementieren und den Leistungsunterschied in ihrer jeweiligen Anwendung zu messen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ressourcen](resources.md)

 

 




