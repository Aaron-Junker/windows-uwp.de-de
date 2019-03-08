---
title: Einen Arbiter migrieren
description: Erfahren Sie, wie einen Arbiter in Xbox Live Multiplayer-2015 migrieren
ms.assetid: 9fb5d2c0-d548-4a22-b64e-6b215f78d22e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Arbiter, Multiplayer-2015
ms.localizationpriority: medium
ms.openlocfilehash: 7e250841fb0731a903c0e9c5bdeeecd42f07ac84
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629705"
---
# <a name="migrating-an-arbiter"></a>Einen Arbiter migrieren

Irgendwann während einer Sitzung abgeschlossen müssen Sie einen neuen Arbiter mit Arbiter-Migration zu wählen. Es gibt zwei Arten der Migration:

-   **Ordnungsgemäßes Arbiter-migration**
-   **Failover-Arbiter-migration**

Das folgende Flussdiagramm veranschaulicht, wie einen Arbiter migriert wird.

![](../../images/multiplayer/Multiplayer_2015_HostMigration.png)

## <a name="graceful-arbiter-migration"></a>Ordnungsgemäßes Arbiter-Migration

Bei ordnungsgemäßen Arbiter Migration kann der Arbiter einen neuen der ausgehenden Arbiter Unterstützung bei der Migrationsaufgabe und ermitteln. Dieser Migrationstyp wird die Einstellung von einem Arbiter aus, wie in beschrieben [Vorgehensweise: Legen Sie einen Arbiter für eine Sitzung MPSD](multiplayer-how-tos.md).


## <a name="failover-arbiter-migration"></a>Failover-Arbiter-Migration

Bei der Migration des Arbiters Failover Verbindung mit der vorherigen Arbiter verloren und die verbleibenden Peers müssen einen neuen Arbiter für die Sitzung bestimmen. Failover Arbiter Migration auch legt den Host des gerätetokens und HTTP/412-Statuscodes, die nur als ordnungsgemäße Arbiter-Migration wird verarbeitet. Es gibt jedoch mehrere Ansätze zur Auswahl der Arbiters eines neuen während einer Migrations der Failover-Arbiter.
## <a name="select-arbiter-using-the-host-candidate-list"></a>Wählen Sie die Arbiter, die mit der Host-Kandidatenliste

Sie können konfigurieren, dass MPSD zum Bereitstellen einer geordnete Host Kandidaten-Liste, die basierend auf Vermittlung QoS-Metriken, die während bestimmter Vorgänge gemessen werden. Der Client kann diese Liste verwenden, um einen neuen Arbiter zu bestimmen. Um diese Liste während der Migration der Arbiter nutzen zu können, können jeden Peer:

1.  Identifizieren Sie die der vorherigen Arbiter Listenposition.
2.  Bewerten Sie die Konsole Weitere, in der Liste.
3.  Wenn es sich bei der Konsole für die lokale Konsole ist, verwenden sie als neue der Arbiter.
4.  Wenn die Konsole nicht mehr vorhanden, in der Multiplayer-Sitzung ist oder Verbindung von den Peers getrennt wurde, fahren Sie mit der nächste Kandidat in der Liste aus, und bewerten sie, wie in den vorherigen Schritten.
5.  Wenn das Ende der Liste mit keine neuen Arbiter ausgewählt zu erreichen, verwenden Sie einen greedy-Ansatz für die Arbiter-Auswahl, die Verbindung unterbrochen werden kann. Finden Sie unter "Verwenden der gierige Arbiter-Auswahl".

| Hinweis                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Es wird nicht empfohlen, eine Host Kandidatenliste im Spiel nach Vermittlung durch explizite im Titel QoS-Tests erstellen. Wenn dieser Mechanismus unbedingt erforderlich ist, müssen Sie den Client, der das Host-Geräte-Token anstelle von Benutzerinformationen, z. B. Xbox Benutzer-ID zu verwenden, um Arbiter Kandidaten zu bestimmen. |


### <a name="select-arbiter-using-peer-voting"></a>Wählen Sie die Arbiter mit Peer abstimmen

Wenn vollständige Konnektivität unter allen Peers vorhanden ist, können sie die Peer-Nachrichten verwenden, stimmen, und wählen einen neuen Arbiter. Der neue Arbiter aktualisiert dann das gerätetoken für den Host für die Sitzung, die mithilfe eines synchronisierten Updates. Finden Sie unter [Vorgehensweise: Aktualisieren Sie eine Multiplayer-Sitzung](multiplayer-how-tos.md).


### <a name="use-greedy-arbiter-selection"></a>Verwenden Sie gierigen Arbiter-Auswahl

Mitunter keine Kandidatenliste Host verfügbar ist, oder Konnektivität QoS ist nicht erforderlich, z. B. für reine Arbiter-Zuständigkeiten. In diesen Fällen kann der Client eine gierige Arbiter-Auswahl-Methode verwenden. In diesem Fall sollte ein Peer der neuen Arbiter festgelegt, sobald es feststellt, dass der ursprüngliche Arbiter die game-Sitzung verlassen hat, von gemeldeten der **MultiplayerSessionChanged** Ereignis. Alle anderen Peers finden Sie einen HTTP 412/Statuscode beim Versuch, die Geräte-Token für den Host festgelegt vorausgesetzt, dass an diesem Punkt keine weiteren Änderungen an der Sitzung vorgenommen werden. Bei der Auswahl der neuen Arbiter ist nur ein Peer erfolgreich.


## <a name="see-also"></a>Siehe auch

[Sitzungsdetails MPSD](mpsd-session-details.md)
