---
title: Multiplayer-Sitzungsvorlagen
description: Informationen Sie zu Vorlagen für Xbox Live Multiplayer-Sitzung.
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox one multiplayer, Vorlage "Sitzung"
ms.localizationpriority: medium
ms.openlocfilehash: 0bbe4f6a3afe2d39fb18b4d4bad13e2aa91d246e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599435"
---
# <a name="multiplayer-session-templates"></a>Multiplayer-Sitzungsvorlagen

Dieses Thema bietet eine kurze Übersicht über Vorlagen von Multiplayer-Sitzung und nennt einige Beispiele für Vorlagen, die Sie kopieren und ändern Sie für Ihre multiplayer-Sitzungen können.

Eine Vorlage Multiplayer-Sitzung ist eine Blaupause, die zum Erstellen einer Multiplayer-Sitzung verwendet wird. Alle Sitzungen müssen basierend auf einer vordefinierten Vorlage erstellt werden. Eine Vorlage definiert Konstanten, die die für jede andere Sitzung gleich sind, die aus der Vorlage erstellt wird. Nachdem ein Spiel eine Sitzung aus einer Vorlage erstellt, das Spiel hinzufügen kann und zusätzliche Daten für die Sitzung zu ändern, aber kann nicht geändert werden die Konstanten, die in der Vorlage definiert wurden.

 Weitere Informationen finden Sie unter [Übersicht über Leistungssitzungen](../multiplayer-appendix/mpsd-session-details.md).

Die Liste der Vorlagen für ereignissitzungen, die zu einer Dienst-Konfigurations-ID (SCID), sowie den Inhalt von Vorlagen bestimmte Sitzung gelten, kann von Multiplayer-Sitzung Verzeichnis (MPSD) abgerufen werden.


## <a name="about-session-templates"></a>Informationen zu Vorlagen für ereignissitzungen

Eine sitzungsvorlage verwendet das gleiche Format wie eine HTTP PUT-Anforderung zum Erstellen oder Ändern einer Sitzungs. Der Unterschied besteht darin, dass die Vorlage auf Konstanten (keine Mitglieder, Servern oder Eigenschaften) beschränkt ist. Die Konstanten können Sitzungskonstanten, einschließlich eines benutzerdefinierten Abschnitts und das gesamte Spektrum Systemkonstanten sein.

### <a name="session-template-versions"></a>Sitzung Vorlagenversionen

Die Vorlagen für ereignissitzungen definiert, die in diesem Thema werden mithilfe der Vorlage vertragsversion 107 erstellt. Wenn sie eine neue Vorlage zu erstellen, stellen Sie sicher, dass die Version des Vertrags 107 eingegeben wurde.

Wenn Sie XSAPI verwenden und die resultierenden Anforderungen im Debugger ansehen, bemerken Sie sich, dass die Anforderungen Vorlagenversion 105 Vertrag verwenden. MPSD werden"effektiv" diese Anfragen auf Version 107 zur Laufzeit.

> **Hinweis:** Es ist zulässig, verwenden eine anderen Vertrag-Version in der Anforderung aus, was in der sitzungsvorlage verwendet wird.

Bei Bedarf können Sie eine sitzungsvorlage von Version 104/105 auf Version 107 ändern. Anleitungen hierzu finden Sie unter Migrieren von Anweisungen in [häufige Probleme beim Anpassen der Titel für 2015 Multiplayer-Spiele](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md).


## <a name="session-template-default-values"></a>Standardwerte für Session-Vorlage

Jede Sitzung, die aus einer sitzungsvorlage erstellt, die als eine Kopie der Vorlage wird gestartet. Werte, die die Vorlage nicht enthalten ist, können bei der sitzungserstellung angegeben werden. In einigen Fällen werden Standardwerte bereitgestellt, wenn kein anderer Wert festgelegt ist. Beispielsweise ist der Standardsatz von Timeouts für vertragsversion 107:

```json
    {
      "constants": {
        "system": {
          "reservedRemovalTimeout": 30000,
          "inactiveRemovalTimeout": 0,
          "readyRemovalTimeout": 180000,
          "sessionEmptyTimeout": 0
        }
      }
    }
```
Sie können erzwingen, dass einen Wert, der die Festlegung bleiben durch Angabe von Null. Dies überschreibt alle Standard-Einstellungen und verhindert, dass den Wert, der bei der sitzungserstellung festgelegt wird. Fügen Sie zum Entfernen der leere Timeoutwert für Sitzungen dauerhaft fortgesetzt, auch ohne Elemente, sodass z. B. Folgendes an der sitzungsvorlage:
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **Wichtig:** Konstanten, die über eine Vorlage festgelegt werden können nicht durch Schreibvorgänge in MPSD geändert werden. Um die Werte ändern zu können, müssen Sie erstellen und übermitteln eine neue Vorlage für die erforderlichen Änderungen.


## <a name="example-session-templates"></a>Beispielvorlagen für die Sitzung
In diesem Abschnitt werden einige Beispiele für Vorlagen für ereignissitzungen für verschiedene Zwecke und Netzwerktopologien. . Überprüfen Sie jede Vorlage vor der Auswahl der am besten für den Client ein. Sie können dann kopieren und fügen Sie die Vorlage in der Dienstkonfiguration, potenziell, nachdem Sie die erforderliche Änderungen vorgenommen.

### <a name="standard-lobby-session"></a>Standard Lobby-Sitzung
Sie können die folgende Vorlage als startvorlage verwenden, um eine Sitzung Lobby für Ihr Spiel erstellen:

* Ändern der `maxMembersCount` Wert, der die maximale Anzahl von Playern geschützt, die Sie in der Sitzung Lobby unterstützen möchten.  
* Wenn Ihre Titel Spieler von verschiedenen Plattformen (z. B. eine Xbox One und einem PC) nicht unterstützt zusammen wiedergeben, können Sie entfernen die `crossPlay` Element.  
* Sie können die anderen Werte auch ändern, aber die folgenden Werte sind gute Werte für den Einstieg, wenn Sie nicht sicher sind, was Sie benötigen.


```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            },
        },
        "custom": {}
    }
}
```

### <a name="standard-game-session-without-matchmaking"></a>Standard-game-Sitzung ohne Vermittlung
Sie können die folgende Vorlage als ein Starter-Vorlage verwenden, um eine Spiele-Sitzung für Ihres Spiels zu erstellen, wenn Ihr Spiel enthält keine anonymen Vermittlung und nicht mehr als 100 Mitglieder erfordert.

Beachten Sie, dass nur neue angegeben, aus der Vorlage der standardmäßigen Lobby-Sitzung die folgenden Werte:
* `constants.system.inviteProtocol : "game"`
* `constants.system.capabilities.gameplay : true`

```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "inviteProtocol": "game",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "gameplay" : true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            }
        },
        "custom": {}
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>Fügen Sie Vermittlung hinzu eine sitzungsvorlage für die Spiele-, während den Multiplayer-Dienst, die Qualität der Dienst sucht verarbeiten lassen.

Sie können die folgende hinzufügen `memberInitialization` JSON-Element an die Gaming-Vorlage zum Hinzufügen von Unterstützung für die Vermittlung.

Wenn Sie Ihre SmartMatch Hopper erstellen, verwenden Sie diese Vorlage als die Sitzung Zielvorlage für Ihre Hopper.

```json
{
   "constants": {
        "system": {
            "memberInitialization": {
               "joinTimeout": 20000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        }
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>Fügen Sie Vermittlung, um eine sitzungsvorlage für die Spiele-, in der Qualität der Dienst sucht von einem verwalteten Rechenzentrum Titel behandelt werden.



```json
{
   "constants": {
        "system": {
            "peerToHostRequirements": {  
                "latencyMaximum": 250,
                "bandwidthDownMinimum": 256,
                "bandwidthUpMinimum": 256,
                "hostSelectionMetric": "latency"
            },
            "memberInitialization": {
               "joinTimeout": 15000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}
```

### <a name="basic-session-template-for-client-server-game-session"></a>Grundlegende sitzungsvorlage für Client / Server-game-Sitzung

Sie können diese Vorlage für einen Titel verwenden, die führt keine Peer-zu-Peer-Kommunikation und Xbox Live Compute nicht verwendet, aber stattdessen enthält Clients, die Verbindung mit eines gehosteten Servers von Drittanbietern.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true,
          },
        },
        "custom": {}
      }
    }
```

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>Lobby/SmartMatch Ticket sitzungsvorlage für Peer-basierte Netzwerke

Verwenden Sie diese Vorlage zum Erstellen einer Sitzung Lobby oder eine SmartMatch Ticket-Sitzung, die nur verwendet werden, um eine Gruppe von Playern an Vermittlung zu senden. Die Vorlage werden nicht verwendet werden, um eine Spiele-Sitzung konfigurieren. Es ist für die Verwendung durch Clients, die mit einer Peer-zu-Peer- oder Peer-zu-Host-Netzwerktopologie vorgesehen.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 10,
          "visibility": "open",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
          },
          "memberInitialization": {
            "membersNeededToStart": 1
          },
        },
        "custom": {}
      }
    }
```

### <a name="quality-of-service-qos-templates"></a>Quality Service (QoS)-Vorlagen

Wenn der Client die Vermittlung verwendet, und bewerten QoS, müssen Sie einige Konstanten für die sitzungsvorlage MPSD zur Koordinierung mit dem Client zum Verwalten von Benutzern, die die Sitzung beitreten informiert hinzufügen. Diese Koordination überprüft, ob die Qualität des Verbindungsstatus vor dem Informieren der Benutzer, die das Spiel gestartet werden kann. Im Fall von Client / Server-Spiele überprüft die Koordination Verbindungsqualität, bevor eine Gruppe von Spielern Vermittlung eintritt.

#### <a name="peer-to-host-game-session-template-with-qos"></a>Mit QoS-Spiel sitzungsvorlage für Peer-zu-host

Im folgende Beispiel stellt eine sitzungsvorlage für die Peer-zu-Host-game-mit QoS.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectivity": true,
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true
          },
          "memberInitialization": {
            "membersNeededToStart": 2
          },
          "peerToHostRequirements": {
            "latencyMaximum": 350,
            "bandwidthDownMinimum": 1000,
            "bandwidthUpMinimum": 1000,
            "hostSelectionMetric": "latency"
          }
        },
        "custom": { }
      }
    }
```

#### <a name="peer-to-peer-game-session-template-with-qos"></a>Peer-zu-Peer-Spiel sitzungsvorlage mit QoS

Folgendes ist ein Beispiel für eine sitzungsvorlage Peer-zu-Peer-game-mit QoS.
```json
    {
    "constants": {
      "system": {
        "version": 1,
        "maxMembersCount": 12,
        "visibility": "open",
        "inviteProtocol": "game",
        "capabilities": {
          "connectivity": true,
          "connectionRequiredForActiveMembers": true,
          "gameplay" : true
        },
        "memberInitialization": {
          "membersNeededToStart": 2
        },
        "peerToPeerRequirements": {
          "latencyMaximum": 250,
          "bandwidthMinimum": 10000
        }
      },
      "custom": { }
     }
    }
```

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>Client / Server (Xbox Live Compute) Lobby/Vermittlung sitzungsvorlage mit QoS

Verwenden Sie diese Vorlage, um eine Lobby oder ein datenbankgesteuertes-Sitzung mithilfe von QoS zu erstellen. Diese Vorlage dient nicht zum Konfigurieren einer spielen-Sitzungs verwendet werden.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "memberInitialization": {
            "membersNeededToStart": 1
          }
        },
        "custom": {}
      }
    }
```

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>Sitzungsvorlage für Crossplay zwischen Xbox One und Windows 10

Verwenden Sie diese Vorlage, um Crossplay Multiplayer-zwischen Xbox One und Windows 10 zu aktivieren. Die UserAuthorizationStyle Funktion ermöglicht den Zugriff auf Windows 10. Die optionale CrossPlay-Funktion gibt an, dass es sich bei Ihrem Titel Interaktionen wie z. B. Einladungen und Join-in-Bearbeitung zwischen Plattformen unterstützt.
```json
    {
      "constants": {
        "system": {
          "capabilities": {
            "crossPlay": true,
            "userAuthorizationStyle": true
          },
        },
        "custom": {}
      }
    }
```
