---
title: Vorgänge für Streamingressourcen
description: Dieser Abschnitt enthält Vorgänge, die Sie auf Streaming-Ressourcen ausführen können.
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords:
- Vorgänge für Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 922798fad97754421541297a5434a81e9c660b2b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655515"
---
# <a name="operations-available-on-streaming-resources"></a>Vorgänge für Streamingressourcen


Dieser Abschnitt enthält Vorgänge, die Sie auf Streaming-Ressourcen ausführen können.

-   Aktualisieren von Kachelzuordnungen, die "ungültig" zurückgeben, und Kopiere von Kachelzuordnungen, die "ungültig" zurückgeben - diese Vorgänge verweisen Kachelspeicherorte in einer Streaming-Ressource auf Speicherorte in Kachelpools oder auf NULL oder auf beide. Diese Vorgänge können eine getrennte Teilmenge der Kachelverweise aktualisieren.
-   Kopieren und Aktualisieren von Vorgängen - Alle API, die Daten in eine und aus einer Standard-Pooloberfläche kopieren können, arbeiten für Streaming-Ressourcen. Lesen von nicht zugeordneten Kacheln erzeugt 0 und Einträge in nicht zugeordnete Kacheln werden gelöscht.
-   Kopieren von Kacheln und Aktualisieren von Kachelvorgängen - diese Vorgänge dienen dem Kopieren von Kacheln mit einer Granularität von 64 KB in eine und aus einer Streaming-Ressource und Puffer-Ressource in einem kanonischen Speicherlayout. Der Bildschirmtreiber und die Hardware durchmischen den Speicher so, wie dies für die Streaming-Ressource erforderlich ist.
-   Direct3D-Rohrbindungen und Anzeigen von Kreationen/Bindungen, die auf Nicht-Streaming-Ressourcen funktionieren, würden auch auf Streaming-Ressourcen funktionieren.

Kachelsteuerelemente sind auf unmittelbare oder verzögerte Kontexte verfügbar (wie Aktualisierungen auf typische Ressourcen) und wirken sich nach der Ausführung auf nachfolgende Zugriffe auf die Kacheln aus (nicht zuvor eingereichte Vorgänge).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Erstellen von streaming-Ressourcen](creating-streaming-resources.md)

 

 




