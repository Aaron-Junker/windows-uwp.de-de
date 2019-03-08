---
title: Zuordnen des verfügbaren Speicherplatzes für Streamingressourcen
description: In diesem Abschnitt wird der virtuelle Adressraum angegeben, der für Streamingressourcen verfügbar ist.
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords:
- Zuordnen des verfügbaren Speicherplatzes für Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 35a591d805870df97ee03169b20e664316e094a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57652375"
---
# <a name="address-space-available-for-streaming-resources"></a>Zuordnen des verfügbaren Speicherplatzes für Streamingressourcen


In diesem Abschnitt wird der virtuelle Adressraum angegeben, der für Streamingressourcen verfügbar ist.

Bei 64-Bit-Betriebssystemen ist mindestens 40 Bit virtueller Adressraum (1 Terabyte) verfügbar.

Bei 32-Bit-Betriebssystemen beträgt der Adressraum 32 Bit (4 GB). Bei 32-Bit-Betriebssystemen kann das Erstellen einzelner Streamingressourcen zu Fehlern führen, wenn die Zuordnung des Adressraums (128 MB) mehr als 27 Bit beträgt. Dazu gehören alle ausgeblendeten Abstände im Adressraum, die die Hardware für Mipmaps, Abstände für aneinanderliegende Kacheln und möglicherweise Abstände für Oberflächengrößen mit Potenzen von 2 verwendet.

Auf Grafiksystemen mit einer separaten Seitentabelle für den Grafikprozessor (GPU) wird der Adressraum hauptsächlich für die von der Anwendung erstellten GPU-Ressourcen verwendet, auch wenn die vom Anzeigetreiber vorgenommene GPU-Zuordnungen genauso viel Platz verwenden.

Bei zukünftigen Systemen, bei denen die Seitentabelle gemeinsam von CPU und GPU genutzt wird, wird der verfügbare Adressraum gemeinsam von CPU und GPU-Zuordnungen in einem Prozess genutzt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Stream-Parameter für die Ressource](streaming-resource-creation-parameters.md)

 

 




