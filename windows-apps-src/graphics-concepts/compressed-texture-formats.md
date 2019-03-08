---
title: Komprimierte Texturformate
description: Dieser Abschnitt enthält Informationen über die interne Organisation komprimierter Texturformate.
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords:
- Komprimierte Texturformate
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3171eba376911157a6ad2687fe3879df751615ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662195"
---
# <a name="compressed-texture-formats"></a>Komprimierte Texturformate


Dieser Abschnitt enthält Informationen über die interne Organisation komprimierter Texturformate. Diese Informationen sind nicht zur Verwendung von komprimierten Texturen notwendig, da Sie die Direct3D-Funktionen für eine Konvertierung in und von komprimierten Formate verwenden können. Sie sind jedoch hilfreich, wenn Sie direkt auf komprimierten Oberflächendaten arbeiten möchten.

Direct3D verwendet ein Komprimierungsformat, das Texturabbildungen in 4 x 4-Texel-Blöcke unterteilt. Wenn die Textur keine Transparenz enthält - wenn sie undurchsichtig ist - oder die Transparenz von einem 1-Bit-Alpha angegeben wird, stellt ein 8-Byte-Block den Texturabbildungsblock dar. Enthält die Texturabbildung transparente Texel, stellt ein 16-Byte-Block mittels eines Alpha-Kanals den Block dar.

Jede einzelne Textur muss angeben, dass die Daten als 64 oder 128 Bit pro Gruppe von 16 Texeln gespeichert werden. Wenn 64-Bit-Blöcke - im BC1-Format – für die Textur verwendet werden, ist es möglich, undurchsichtige und 1-Bit-Alpha-Kanal-Formate auf einer pro-Block-Basis innerhalb der gleichen Textur zu kombinieren. Das heißt, der Vergleich Ganzzahl ohne Vorzeichen als Maßeinheit Farbe\_0 und die Farbe\_1 eindeutig für jeden Block von 16 Texel durchgeführt wird.

Wenn 128-Bit-Blöcke verwendet werden, muss der Alphakanal entweder im expliziten ( BC2-Format) oder interpolierten Modus (BC3-Format) für die gesamte Textur angegeben werden. Was die Farbe anbetrifft, können beim aktiven interpolierten Modus entweder acht interpolierte Alphamodi oder sechs interpolierte Alphamodi auf einer nach Blöcken gestaffelten Basis verwendet werden. Erneut der Größe Vergleich Alpha\_0 und Alpha\_1 erfolgt eindeutig auf Basis von Blöcken.

Der Neigungswinkel für BCn-Formate wird in Bytes (nicht Blöcken) gemessen. Z. B. Wenn Sie eine Breite von 16 verfügen, dann müssen Pitch vier Blöcke (4\*8 für BC1, 4\*16 für BC2 oder bc3 ausgegeben).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Komprimierte des texturressourcen](compressed-texture-resources.md)

 

 




