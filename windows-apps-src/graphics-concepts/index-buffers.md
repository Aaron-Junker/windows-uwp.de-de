---
title: Indexpuffer
description: Indexpuffer sind Speicherpuffer mit Indexdaten, die Ganzzahl-Offsets in Vertexpuffer darstellen, und zum Rendern von Grundtypen verwendet werden.
ms.assetid: 14D3DEC5-CF74-488B-BE41-16BF5E3201BE
keywords:
- Indexpuffer
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 36d08006fa2f32812f97daef5135a98dce16c4e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594615"
---
# <a name="index-buffers"></a>Indexpuffer


*Indexpuffer* sind Speicherpuffer mit Indexdaten, die Ganzzahl-Offsets in Vertexpuffer darstellen, und zum Rendern von Grundtypen verwendet werden.

Indexpuffer sind Speicherpuffer mit Indexdaten. Indexdaten oder Indizes sind Ganzzahl-Offsets in Vertexpuffern, die zum Rendern von Grundtypen verwendet werden.

Ein Vertexpuffer enthält Scheitelpunkte; deshalb können Sie einen Vertexpuffer mit oder ohne indizierten Grundtypen zeichnen. Da ein Indexpuffer jedoch Indizes enthält, können Sie einen Indexpuffer nicht ohne entsprechenden Vertexpuffer verwenden.

## <a name="span-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanindex-buffer-description"></a><span id="Index_Buffer_Description"></span><span id="index_buffer_description"></span><span id="INDEX_BUFFER_DESCRIPTION"></span>Beschreibung für Index-Puffer


Ein Indexpuffer wird anhand seiner Fähigkeiten beschrieben, wie z. B. wo im Speicher er vorhanden ist, ob Lese- und Schreibberechtigungen unterstützt werden, und nach Typ und Anzahl der enthaltenen Indizes.

Die Beschreibungen von Indexpuffern geben Ihrer Anwendung Hinweise darauf, wie ein vorhandener Puffer erstellt wurde. Sie stellen eine leere Beschreibungsstruktur für das System bereit, die mit den Fähigkeiten eines vorher erstellten Indexpuffers gefüllt wird.

## <a name="span-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanindex-processing-requirements"></a><span id="Index_Processing_Requirements"></span><span id="index_processing_requirements"></span><span id="INDEX_PROCESSING_REQUIREMENTS"></span>Index-Verarbeitungsanforderungen


Die Leistung von Indexverarbeitungsvorgängen ist hauptsächlich davon abhängig, wo der Index im Speicher vorhanden ist, und welche Art von Renderinggerät verwendet wird. Anwendungen steuern die Speicherreservierung für Indexpuffer, wenn sie erstellt werden.

Die Anwendung kann direkt Indizes in einen Indexpuffer schreiben, der im treiberoptimierten Arbeitsspeicher reserviert ist. Mit dieser Technik wird später ein redundanter Kopiervorgang verhindert. Diese Technik funktioniert nicht optimal, wenn Ihre Anwendung Daten aus dem Indexpuffer zurückliest, da vom Host ausgeführte Lesevorgänge auf den treiberoptimierten Arbeitsspeicher sehr langsam sein können. Wenn Ihre Anwendung also während der Verarbeitung unregelmäßig Daten in oder aus dem Puffer lesen oder schreiben muss, ist ein Indexpuffer im Systemspeicher die bessere Wahl.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Vertex- und Indexpuffer](vertex-and-index-buffers.md)

 

 




