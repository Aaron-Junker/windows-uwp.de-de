---
title: Statusobjekte
description: Der Gerätestatus wird in Statusobjekten gruppiert, wodurch der Aufwand für Statusänderungen erheblich reduziert wird. Es gibt mehrere Statusobjekte und jedes dient der Initialisierung einer Statusgruppe für eine bestimmte Pipelinephase. Statusobjekte variieren je nach der Version von Direct3D.
ms.assetid: D998745C-2B75-4E59-9923-AD1A17A92E05
keywords:
- Statusobjekte
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3437119979073a5cec27948fc90f954e06c2fc93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596085"
---
# <a name="state-objects"></a>Statusobjekte


Der Gerätestatus wird in Statusobjekten gruppiert, wodurch der Aufwand für Statusänderungen erheblich reduziert wird. Es gibt mehrere Statusobjekte und jedes dient der Initialisierung einer Statusgruppe für eine bestimmte Pipelinephase. Statusobjekte variieren je nach der Version von Direct3D.

## <a name="span-idinputlayoutspanspan-idinputlayoutspanspan-idinputlayoutspaninput-layout-state"></a><span id="Input_Layout"></span><span id="input_layout"></span><span id="INPUT_LAYOUT"></span>Eingabe-Layout-Status


Diese Statusgruppe bestimmt, wie die [Eingabeassemblerphase (IA)](input-assembler-stage--ia-.md) Daten aus den Eingabepuffern liest und sie für die Verwendung durch den Vertex-Shader zusammensetzt. Dies umfasst Status wie die Anzahl der Elemente im Eingabepuffer und die Signatur der Eingabedaten. In der Eingabeassemblerphase (IA) werden Grundtypen aus dem Speicher in die Pipeline gestreamt.

## <a name="span-idrasterizerspanspan-idrasterizerspanspan-idrasterizerspanrasterizer-state"></a><span id="Rasterizer"></span><span id="rasterizer"></span><span id="RASTERIZER"></span>Status des Rasterizers


Diese Statusgruppe initialisiert die [Rasterizerphase (RS)](rasterizer-stage--rs-.md). Dieses Objekt enthält Status wie Modi zum Füllen oder Aussortieren, Aktivieren eines Schneiderechtecks zum Zuschneiden und Festlegen von Parametern für das Multisampling. In dieser Phase werden Grundtypen in Pixel gerastert, indem Vorgänge wie Zuschneiden durchgeführt und Viewports Grundtypen zugeordnet werden.

## <a name="span-iddepthstencilspanspan-iddepthstencilspanspan-iddepthstencilspandepth-stencil-state"></a><span id="DepthStencil"></span><span id="depthstencil"></span><span id="DEPTHSTENCIL"></span>Status der Tiefenschablone


Diese Statusgruppe initialisiert den Tiefenschablonenteil der [Ausgabezusammenführungsphase (OM)](output-merger-stage--om-.md). Genauer gesagt, initialisiert dieses Objekt Tiefen- und Schablonentests.

## <a name="span-idblendspanspan-idblendspanspan-idblendspanblend-state"></a><span id="Blend"></span><span id="blend"></span><span id="BLEND"></span>Blend-Status


Diese Statusgruppe initialisiert den Überblendungsteil der [Ausgabezusammenführungsphase (OM)](output-merger-stage--om-.md).

## <a name="span-idsamplerspanspan-idsamplerspanspan-idsamplerspansampler-state"></a><span id="Sampler"></span><span id="sampler"></span><span id="SAMPLER"></span>Samplerstatus


Diese Statusgruppe initialisiert ein Samplerobjekt. Ein Samplerobjekt wird von den Shaderphasen zum Filtern von Texturen im Speicher verwendet.

In Direct3D ist das Samplerobjekt nicht an eine bestimmte Textur gebunden, es beschreibt nur, wie die Filterung für angefügte Ressourcen erfolgt.

## <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Überlegungen zur Leistung


Die Verwendung von Statusobjekten in der API führt zu einer Reihe von Leistungsvorteilen. Dazu gehören: Überprüfen des Status bei der Objekterstellung, Aktivieren des Zwischenspeicherns von Statusobjekten in der Hardware und erhebliches Verringern der Statusmenge, die beim API-Aufruf für eine Statusfestlegung übergeben wird (durch Übergabe eines Handles an das Statusobjekt anstelle des Status).

Um diese Leistungsverbesserungen zu erzielen, sollten Sie Ihre Statusobjekte beim Start Ihrer Anwendung erstellen, und zwar vor der Renderschleife. Statusobjekte sind unveränderlich, d. h., nachdem sie erstellt wurden, können sie nicht mehr geändert werden. Sie müssen sie stattdessen löschen und neu erstellen.

Sie können mehrere Samplerobjekte mit verschiedenen Samplerstatuskombinationen erstellen. Das Ändern des Samplerstatus erfolgt dann durch Aufrufen der entsprechenden „Set”-API, die ein Handle für das Objekt (statt des Samplerstatus) übergibt. Der Aufwand für eine Statusänderung bei jedem Rendern von Frames wird dadurch deutlich reduziert, da die Anzahl der Aufrufe und die Menge an Daten erheblich verringert werden.

Alternativ können Sie das Effektsystem verwenden, das automatisch die effiziente Erstellung und Zerstörung von Statusobjekten für Ihre Anwendung verwaltet.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

[Ansichten](views.md)

 

 




