---
title: Auswählen einer Ressource
description: Eine Ressource ist eine von der 3D-Pipeline verwendete Datensammlung.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- Auswählen einer Ressource
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ccc99395dba2f2d1894db81fb48abb59f9a8ba4f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613395"
---
# <a name="choosing-a-resource"></a>Auswählen einer Ressource


Eine Ressource ist eine von der 3D-Pipeline verwendete Datensammlung. Das Erstellen von Ressourcen und das Definieren des Verhaltens ist der erste Schritt zur Programmierung Ihrer Anwendung. Dieses Handbuch enthält grundlegende Informationen zur Auswahl der von Ihrer Anwendung erforderlichen Ressourcen.

## <a name="span-ididentifybindingspanspan-ididentifybindingspanspan-ididentifybindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>Identifizieren Sie die Pipelinestufen, die Ressourcen benötigen.


Der erste Schritt ist die Auswahl der [Grafikpipeline](graphics-pipeline.md)-Phase (oder Phasen), die eine Ressource erfordert. Identifizieren Sie daher jede Phase, bei der Daten von einer Ressource gelesen werden und alle Phasen, bei der Daten auf eine Ressource geschrieben werden. Das Erkennen der Pipelinephasen, in denen Ressourcen verwendet werden, hilft dabei, festzulegen, welche APIs aufgerufen werden, um die Ressource an die Phase zu binden.

Diese Tabelle enthält die Ressourcentypen, die an die einzelnen Pipelinephasen gebunden werden können. Es wird ebenfalls angegeben, ob die Ressource als Eingabe oder als Ausgabe gebunden werden kann.

| Pipelinephase  | Eingabe/Ausgabe | Ressource               | Ressourcentyp                           |
|-----------------|--------|------------------------|-----------------------------------------|
| Eingabe-Assembler | Hinein     | Vertexpuffer          | Puffer                                  |
| Eingabe-Assembler | Hinein     | Indexpuffer           | Puffer                                  |
| Shaderstufen   | Hinein     | Shaderressourcenansicht    | Puffer, Texture1D, Texture2D, Texture3D |
| Shaderstufen   | Hinein     | Shaderkonstanten-Puffer | Puffer                                  |
| Streamausgabephase   | Heraus    | Puffer                 | Puffer                                  |
| Ausgabezusammenführungsphase   | Heraus    | Renderzielansicht     | Puffer, Texture1D, Texture2D, Texture3D |
| Ausgabezusammenführungsphase   | Heraus    | Ansicht/Tiefenschablone     | Texture1D, Texture2D                    |

 

## <a name="span-ididentifyusagespanspan-ididentifyusagespanspan-ididentifyusagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>Legen Sie wie jede Ressource verwendet werden


Nachdem Sie die Pipelinephasen ausgewählt haben, die Ihre Anwendung verwendet (und somit die Ressourcen, die für jede Phase erforderlich sind), ist der nächste Schritt die Verwendungsweise jeder Ressource, d. h. ob auf die Ressource von einer CPU oder GPU zugegriffen werden kann.

Die Hardware, auf der die App ausgeführt wird, muss mindestens eine CPU und eine GPU enthalten. Berücksichtigen Sie bei der Auswahl des Auslastungswerts, welcher Prozessortyp von der Ressource lesen oder darauf schreiben muss. Wählen Sie eine der folgenden Optionen aus.

| Ressourcennutzung | Kann aktualisiert werden am                    | Updatehäufigkeit |
|----------------|--------------------------------------|---------------------|
| Standard        | GPU                                  | selten        |
| Dynamisch        | CPU                                  | häufig          |
| Wird bereitgestellt        | GPU                                  | n. a.                 |
| Unveränderlich      | CPU (nur zum Zeitpunkt der Erstellung der Ressource) | n. a.                 |

 

„Standardverwendung” sollte für eine Ressource verwendet werden, die von der CPU voraussichtlich selten (weniger als einmal pro Frame) aktualisiert wird. Im Idealfall schreibt die CPU nie direkt auf eine Ressource mit Standardverwendung, um potenzielle Leistungseinbußen zu verhindern.

„Dynamische Verwendung” sollte für eine Ressource verwendet werden, die von der CPU relativ häufig (einmal oder mehrmals pro Frame) aktualisiert wird. Ein typisches Szenario für eine dynamische Ressource wäre die Erstellung dynamischer Vertex- und Indexpuffer, die während der Laufzeit mit Daten über die Geometrie ausgefüllt werden, die aus der Sicht des Benutzers für jedes Frame sichtbar sind. Diese Puffer werden verwendet, um nur die Geometrie dieses Frame für den Benutzer sichtbar zu machen.

„Staging-Nutzung” sollte verwendet werden, um Daten von einer Ressource auf eine andere zu kopieren. Ein typisches Szenario wäre das Kopieren von Daten von einer Ressource mit Standardnutzung (auf die die CPU nicht zugreifen kann) auf eine Ressource mit Staging-Nutzung (auf die die CPU zugreifen kann).

„Unveränderliche Ressourcen” sollten dann verwendet werden, wenn die Daten in der Ressource nie geändert werden.

Anders ausgedrückt handelt es sich hier darum, wie die Anwendung die Ressource verwendet.

| Wie die Anwendung die Ressource verwendet     | Ressourcennutzung       |
|---------------------------------------|----------------------|
| Einmal laden, nie wieder aktualisieren            | Unveränderlich oder Standard |
| Anwendung füllt Ressource wiederholt auf | Dynamisch              |
| Rendering auf Textur                     | Standard              |
| CPU-Zugriff auf GPU-Daten                | Wird bereitgestellt              |

 

Wenn Sie nicht sicher sind, welche Nutzung am besten ist, beginnen Sie mit der generell verwendeten Standardnutzung. Der Shaderkonstanten-Puffer ist der einzige Ressourcentyp, der immer mit der Standardnutzung verwendet werden sollte.

## <a name="span-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>Binden von Ressourcen an Pipelinestufen


Eine Ressource kann an mehr als eine Pipelinephase gebundenen sein, solange die bei der Erstellung der Ressource angegebenen Einschränkungen erfüllt werden. Diese Einschränkungen werden als Nutzungs-, Bindungs- oder CPU-Zugriffskennzeichen angegeben. Genauer gesagt kann eine Ressource gleichzeitig als Eingabe und Ausgabe gebunden werden, solange das Lesen und Schreiben eines Teils der Ressource zur gleichen Zeit ausgeführt werden kann.

Überlegen Sie beim Binden einer Ressource, wie die GPU und CPU auf die Ressource zugreifen werden. Ressourcen, die einem einzigen Zweck dienen (verwenden Sie nicht mehrere Kennzeichen für die Nutzung, Bindung und den CPU-Zugriff) bieten sehr wahrscheinlich eine bessere Leistung.

Betrachten Sie z. B. den Fall, in dem ein Renderziel mehrmals als Textur verwendet wird. Es ist unter Umständen schneller, zwei Ressourcen zu haben: ein Renderziel und eine Textur, die als Shaderressource verwendet wird. Jede Ressource würde nur ein Bindungskennzeichen mit dem Hinweis „Renderziel” oder „Shaderressource” verwenden. Die Daten würden von der Renderziel-Textur auf die Shader-Textur kopiert.

Das in diesem Beispiel dargestellte Verfahren kann die Leistung durch ein Isolieren des Renderziel-Schreibvorgangs vom Shader-Textur-Lesevorgang verbessern. Sie können dies nur durch das Implementieren beider Methoden und dem Messen des Leistungsunterschieds in Ihrer bestimmten Anwendung herausfinden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ressourcen](resources.md)

 

 




