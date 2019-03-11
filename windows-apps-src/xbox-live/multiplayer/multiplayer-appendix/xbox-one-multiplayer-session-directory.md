---
title: Xbox One Multiplayer Session Directory
description: Informationen Sie zum Erstellen von Multiplayer-Sitzungen mit dem Xbox Live Mutliplayer Sitzung Verzeichnis (MPSD)-Dienst.
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 52b363492d1cd17ae54ae5d23d04371c5adf73ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606815"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Xbox One Multiplayer Session Directory

Dieses Thema enthält eine Übersicht über Multiplayer-Sitzung erstellen, die mithilfe des neuen Xbox eine Multiplayer-Sitzung Verzeichnis (MPSD)-Diensts. Das Dokument wird eignet sich hauptsächlich für Entwickler für Xbox One Titel umgeleitet, deren Sitzung Vorlagen direkt auf den Xbox Entwicklung Portal (XDP) sendet. Der MPSD-Dienst kann auch mit Partner Center konfiguriert werden, jedoch nicht in diesem Artikel konzentriert. Es soll sie mit den Begriffen und Konzepten MPSD-Konfiguration, Nutzung, zugeordneten und Problembehandlung von Multiplayer-Sitzungen vertraut zu machen.

## <a name="revision-summary"></a>Zusammenfassung Revision

Die clientseitige XSAPI (Xbox-Dienste-API)-Clientbibliotheken verwenden zurzeit vertragsversion 104, bei der Kommunikation mit den Diensten MPSD. Diese Version des Dokuments aktualisiert die Sitzung Vorlagenschema zum Vertrag Version 107, d.h., dass die neueste Contract-Version, die von den Diensten MPSD unterstützt. Die Änderungen zwischen Version 104 und 107 werden in den folgenden Abschnitt zusammengefasst [Vertrag schemaaktualisierung](#_Contract_schema_update).

Beachten Sie, dass Vorlagen, die für die vertragsversion 104 geschrieben wurden aktualisiert werden, wenn sie als 107 erneut veröffentlicht werden müssen. Allerdings sind die Dienste abwärtskompatibel, daher Sie weiterhin können mit den aktuellen Bibliotheken, die aktualisiert wird, in einer zukünftigen Version von xdk-Version.

Die frühere Version dieses Dokuments aktualisiert Informationen zu Sitzungen und einen neuen Abschnitt zu Xbox Live-Dienst-APIs und Aufrufe der RESTful-Dienst hinzugefügt. Darüber hinaus die Tabelle finden Sie in der Abfrage für Sitzungszustandsinformationen Abschnitt wurde aktualisiert und Abschnitt Quality of Service (QoS)-Vorlagen wurde überarbeitet.

## <a name="introduction"></a>Einführung

Für Xbox One eine Multiplayer-Sitzung ist ein Schutz von Dokumenten, die in der Cloud auf die Multiplayer-Sitzung Verzeichnis (MPSD) befindet, und dieses Dokument stellt eine Gruppe von Personen, die ein Spiel spielen. Insbesondere verwenden Multiplayer-Sitzungen folgenden Qualitäten:

-   Jede Sitzung hat einen eindeutigen URI.

-   Das Dokument für die Sitzung enthält die derzeitigen Mitglieder game-Einstellungen, die bootstrapping von Daten und Informationen zum Spiel Server ist.

-   Titel erstellen und verwalten ihre eigenen Sitzungen.

-   Eine Sitzung können die Konnektivität zwischen den Mitgliedern.

Das Sitzungsverzeichnis Multiplayer zentralisiert Spiel sitzungsmetadaten auf allen Clients. Sie erhalten Sie grundlegende Informationen zu einer Sitzung zur Einrichtung der sicheren gerätezuordnungen zwischen-Clients erforderlich. Darüber hinaus grundlegende First-Boot-Metadaten für einen Client Verbindung mit das Spiel, bevor die Weitergabe von spezifischeren Spieledaten gestartet wird. (Process Lifetime Management, PLM) und die programmumschaltung Art von Anwendungen auf der Xbox One gewährleistet die MPSD, dass Clients die richtige Informationen für die Kontaktaufnahme mit Kollegen und Servern, die im Rahmen der aktiven game-Sitzung und Koordinaten identifiziert werden mit dem Shell und Konsole Betriebssystem zu reservieren, aktivieren und den Player-Lebensdauer für eine Spiele-Sitzung verwalten.

## <a name="terminology-used-in-this-document"></a>In diesem Dokument verwendete Terminologie

| Begriff                 | Definition                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Multiplayer-Sitzung  | Ein Schutz von Dokumenten, die in der Xbox Live-Cloud befinden, und stellt eine Gruppe von Benutzern (oder) miteinander verbunden werden, während der Wiedergabe eines Titels auf der Xbox One. Alle Aspekte von Multiplayer – z. B. Vermittlung, Parteien, Join-in-Bearbeitung und So weiter – nutzen Sie die Multiplayer-Sitzung. |
| Spiele-Sitzung         | Dies ist der tatsächliche Spiel Sitzung in der MPSD, in dem Benutzer zusammen spielen verfügbar gemacht werden. Alle multiplayer-Szenarien schließlich letztendlich in einer Spiele-Sitzung.                                                                                                                               |
| Ticket-Sitzung überein | Dies ist eine Sitzung, die zum Nachverfolgen von ticketübermittlung über Übereinstimmung bei der Vermittlung verwendet.                                                                                                                                                                                                                 |
| Inaktive player      | Ein Spieler, die in den inaktiven Zustand innerhalb der Sitzung festgelegt wurde. Der Titel wird einen Benutzer auf die inaktiv, Status, wenn das Spiel angehalten, eingeschränkt wird, oder andernfalls inaktiv, gemäß den Titel.                                                                                     |

## <a name="the-multiplayer-session-directory"></a>Das Verzeichnis Multiplayer-Sitzung

Die MPSD vereinfacht und Titel Informationen zwischen online Spieler koordinieren kann. Es können verschiedene Typen von Sitzungen, die erstellt, um verschiedene Aufgaben von Multiplayer-Funktion sein. Die folgende Tabelle zeigt die Unterschiede wie solche Aufgaben durchgeführt wurden auf der Xbox 360 im Vergleich zu, wie sie auf der Xbox One ausgeführt werden.

| Funktion "oder" task                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Abrufen von Sitzungsinformationen für Spiele**     | **XSessionGetDetails**, **XSessionSearchByID**, oder verfolgen Sie selbst.                                              | Fordern Sie die Sitzungsinformationen aus dem Dienst.                                                          |
| **Migrieren von host**                     | Wenn benötigt, ruft der Titel **XSessionMigrateHost.**                                                           | Müssen Sie nicht auf eine neue Sitzung erstellen, weisen Sie einen neuen Host für "SessionId".                                  |
| **Verwalten von mehreren Player-Sitzungen**  | Kompliziert, um mehr als eine Sitzung gleichzeitig zu verarbeiten. Z. B. **XNetReplaceKey** im Vergleich zu **XNetUnregisterKey**. | Dienstbasierte Sitzung vereinfacht die Verwaltung von einer Sitzung aufgeräumter und erleichtert es, mehrere Sitzungen zu behandeln.    |
| **Behandeln Sie SSO-Überwachung von Timeouts und trennt die Verbindung** | Behandeln müssen, die Verbindung trennt und Abmeldung anders mit **XCloseHandle** oder **XSessionDelete**bzw. | Zentralisierter Dienst vereinfacht die SSO-Überwachung von Timeouts und getrennt behandeln und Timeouts in die Spiele-Konfiguration festgelegt werden. |

### <a name="session-patterns"></a>Sitzung-Muster

-   Spiele-Sitzungen

    -   Die Sitzung mit HomeRuns XUIDs sicheres Gerät Adressdaten und Status der Eigenschaft. Dies ist als die tatsächliche Gaming-Sitzung betrachtet.

    -   Hierbei kann es sich um Peer-zu-Peer, Peer-zu-Host, Peer-zu-Server oder Hybrid sein.

-   Match-Ticket-Sitzungen

    -   Eine Sitzung, die an die Vermittlung finden Sie weitere Entwicklungen mit Spielen übermittelt wird. Beachten Sie, dass eine Spiele-Sitzung auch ein Ticket-Sitzung aus, wenn das Spiel für mehrere Spieler sucht.

-   Server-Sitzungen

    -   Spiele-Sitzungen erstellt und Xbox Live Compute-Verarbeitung verwendet.

Abbildung 1 zeigt die Verwendung von MPSD Sitzungen, bei denen die blauen Felder darstellen, MPSD Sitzungen, und der roten Feldern werden die Dienste, die sie verwenden.

Abbildung 1. MPSD Sitzung verwenden.

## <a name="mpsd-sessions"></a>MPSD-Sitzungen

Sitzungen verwalten zwei Listen mit verknüpften Entitäten:

1.  Mitglieder (Benutzer), die verknüpft oder in eine Sitzung eingeladen wurden.

2.  Server (Cloud-Servern oder dedizierte Server), die die Sitzung hinzugefügt wurden.

Jede Entität verfügt über:

1.  Konstante Werte, die zum Zeitpunkt der Erstellung festgelegt werden.

2.  Veränderliche Eigenschaften.

3.  Schreibgeschützte Metadaten.

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>Xbox Live-Dienst-APIs und RESTful-Dienst-Aufrufe

Es gibt zwei Möglichkeiten für die Kommunikation mit einer Xbox-Sitzungen und Vermittlung Dienste. Die erste Möglichkeit ist die Verwendung von standard-HTTPS-Aufrufe, die RESTful Xbox Live Services-URIs. Dies ermöglicht Flexibilität der Titel in aufrufen und die Kommunikation mit diesen Diensten abhängig von ihrer Server und der game-Konfigurationen. Eine Liste dieser URIs befinden sich die [Xbox eine Development Kit (xdk-Version)-Dokumentation](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) unter "Xbox Live Services RESTful Reference." [1]

Die zweite Methode ist die Verwendung der Xbox Live-Dienst-APIs, die als Wrapper für die RESTful-Dienst-URIs fungieren. Diese ermöglichen eine eher traditionellen Ansatz für die Verwendung von APIs in Code ohne HTTPS-Datenverkehr für jeden Aufruf behandeln zu müssen. Der Quellcode für die Xbox Live-Dienst-APIs kann wird mit der Xbox (xdk Development Kit-Version) zur Verfügung gestellt und geändert und in der Titelleiste Ihres integriert werden, je nach Bedarf. Die Beispiele wurden mit der Xbox Live-APIs. Weitere Informationen zu den Xbox Live Services-APIs finden Sie in der Xbox One [xdk-Version Dokumentation](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) unter "Xbox Live Services Reference". [2]

### <a name="mpsd-sessions-and-templates"></a>MPSD Sitzungen und Vorlagen

MPSD Sitzungen werden erstellt, mit Vorlagen, die über die Xbox-Entwicklung-Portal (XDP) erfasst. Vorlagen sind JSON-Dokumente, die definieren, das Framework für die Sitzung erstellt wird. Die Vorlage stellt Konstanten für die neue Sitzung.

Der folgende Ausschnitt der [Player Rendezvous-Codebeispiel](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) ist ein Beispiel für die Vorlage Config.

```json
// Used for creating the session that you then pass into your match ticket request. This *should* not have any QoS requirements.
MatchTicketSession (Contract Version: 107)
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

// This is the new game session that is returned after you’ve been matched.
// Note: This is used for in-game QoS. For out-of-game QoS, you will need P2P/HTP requirements.
GameSession (Contract Version:107)
{
    "constants": {
        "system": {
            "maxMembersCount": 12,
            "capabilities": {"connectivity": true }
        },
        "memberInitialization": {
         "joinTimeout": 20000,
         "measurementTimeout": 15000,
         "externalEvaluation": false,
         "membersNeededToStart": 4
         },

   "custom": {}
  }
}
```

Die Übereinstimmung Ticket Sitzung sollte verwendet werden, mit einer Spiele sitzungsvorlage einrichten, die mit QoS-Timeout-Werten in der **MemberInitialization** Objekt.

Abbildung 2. Beispiel-Hopper.

![](../../images/whitepapers/mpsd_image1.png)

Die folgende Codeauszug ist ein Beispiel für eine sitzungsvorlage für Peer-zu-Peer-game-(Titel verwalteten QoS).

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}

```

Dies ist ein Beispiel für einen Peer-zu-Server-Sitzung und RAW-Vorlage.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {}
        },
        "custom": {}
    }
}
```

Der folgende Code zeigt ein Beispiel für eine Übereinstimmung Ticket Sitzung-Vorlage, die für die intelligente entsprechen verwendet wird.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

```

### <a name="checking-which-templates-are-configured-for-your-scid"></a>Überprüfen die Vorlagen für Ihre SCID konfiguriert sind

#### <a name="session-templates"></a>Vorlagen für ereignissitzungen

Die Liste der Vorlagen für ereignissitzungen, die innerhalb einer SCID vorhanden sind, sowie die Details einer bestimmten Sitzungs-Vorlage können aus MPSD abgerufen werden:

```
GET /serviceconfigs/{scid}/sessiontemplates

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}
```

#### <a name="query-for-session-state-information"></a>Abfragen von Statusinformationen von Sitzungen

Sitzungen können mit der Konfiguration und der angegebenen Sitzung Vorlage Servicelevel abgefragt werden:

```
GET /serviceconfigs/{scid}/sessions

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}/sessions
```

Standardmäßig wird bis zu 100 privaten Sitzungen zurückgegeben. Die Abfrage kann mithilfe dieser Abfragezeichenfolgen-Parameter geändert werden:

| Abfragezeichenfolge             | Auswirkung                                                                                                         | Beschreibung                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Schlüsselwort "Foo" =              | Einschließen Sie nur Sitzungen, die das Schlüsselwort "Foo".                                                             |                                                                                                     |
| XUID = 123                 | Schließen Sie nur Sitzungen, denen der Benutzer "123" ein.                                                               | Standardmäßig muss der Benutzer in der Sitzung einzuschließenden aktiv sein.                                  |
| *reservations*=**true** | Gehören Sie Sitzungen für die der Benutzer wird als ein reserviertes Player festgelegt, aber nicht um einen aktiven Player werden verknüpft. | Nur, wenn Sie Ihren eigenen Sitzungen Abfragen oder beim Abfragen eines bestimmten Benutzers Sitzungen Server-zu-Server. |
| *inactive*=**true**      | Zählen Sie die Sitzungen, die der Benutzer hat akzeptiert, jedoch ist nicht aktiv spielen.                                     | Anzahl der Sitzungen, in denen der Benutzer "ready", aber nicht "aktiv" ist, als inaktiv.                           |
| *private*=**true**       | Gehören Sie private Sitzungen an.                                                                                      | Nur, wenn Sie Ihren eigenen Sitzungen Abfragen oder beim Abfragen von Server-zu-Server.                            |
| *visibility*=open        | Einschließen Sie nur Sitzungen, die "Öffnen".                                                                         | Wenn auf "Privat" Festlegen der "private = True" Richtlinie muss auch festgelegt werden.                                 |
| *dauern*= 5                 | Geben Sie bis zu fünf Sitzungen zurück.                                                                                    | Muss zwischen 0 und 100 liegen.                                                                          |

Das Ergebnis ist ein JSON-Array von Sitzung verweisen. Einiger Sitzungsdaten ist Inlineschemainformationen enthält.

**Beachten Sie** jeder Abfrage muss entweder ein Schlüsselwortfilter, einen Filter XUID oder beides enthalten.

Einstellung *private* (private Sitzungen zurückgegeben) oder *Reservierungen* (womit Sitzungen den Benutzer noch nicht verknüpft), **"true"** muss der Aufrufer auf Serverebene den Zugriff für die Sitzung oder des Aufrufers XUID Anspruch auf dem XUID Filter im URI entsprechen. Andernfalls wird "403 Forbidden" zurückgegeben (oder nicht tatsächlich eine solche Sitzung vorhanden sind).

Der folgende Code-Ausschnitt zeigt ein Beispiel einer Abfrageantwort.

```
{
"results": [ {
"xuid": "9876",  // If the session was found from a xuid, that xuid.
"startTime": "2009-06-15T13:45:30.0900000Z",
"sessionRef": {
  "scid": "foo",
  "templateName": "bar",
  "name": "session-seven"
},
"accepted": 4,        // Approximate number of non-reserved members.
"status": "active",   // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
"visibility": "open", // or "private", "visible", or "full"
"myTurn": true,       // Not present is the same as 'false'. Only present if the session was found using a xuid.
"keywords": [ "one", "two" ]
} ]
}

```

## <a name="session-template-attributes"></a>Sitzung Attribute

<div id="_Contract_schema_update"/>

## <a name="contract-schema-update"></a>Vertrag schemaaktualisierung

Wie am Anfang des in diesem Dokument erwähnt, ist die aktuelle Sitzung Vertrag Vorlagenversion 107, der einige Änderungen am Schema von der früheren Version der 104 eingeführt wurde. Vorlagen, die für die vertragsversion 104 geschrieben wurden, müssen aktualisiert werden, wenn sie als 107 erneut veröffentlicht werden. Folgendes ist eine Zusammenfassung der Änderungen.

-   **/Constants/System/managedInitialization** wird umbenannt in **/constants/system/memberInitialization**.

    -   Die **AutoEvaluate** Feld wird umbenannt in **ExternalEvaluation** und Polarität ändert sich sein, außer dass **"false"** bleibt die Standardeinstellung.

    -   Der Standardwert von **MembersNeededToStart** von 2 in 1 geändert wird.

    -   Der Standardwert von **JoinTimeout** ändert sich von 5 Sekunden in 10 Sekunden.

    -   Die **MeasurementTimeout** ändert sich der Standardwert von 5 Sekunden auf 30 Sekunden.

-   **/Constants/System/Timeouts** wird entfernt, und die Timeouts umbenannt werden, und unter verschoben **/Konstanten/System**.

    -   Die **reservierte** Timeout wird **ReservedRemovalTimeout**.

    -   Die **inaktiv** Timeout wird **InactiveRemovalTimeout** und der neue Standardwert ist 0, anstelle von 2 Stunden.

    -   Die **bereit** Timeout wird **ReadyRemovalTimeout**.

    -   Die **SessionEmpty** Timeout wird **SessionEmptyTimeout**.

-   **/Constants/System/Capabilities/Gameplay** muss angegeben werden, als **"true"** für Sitzungen, die eigentliche Spiel (im Gegensatz zu Helper-Sitzungen, z. B. Übereinstimmung und Lobby Sitzungen) darstellen, um aktuelle Spieler zu ermöglichen und Ruf zu melden.

### <a name="system-objects"></a>Systemobjekte

Jede Systemobjekte in der Sitzung Dokument verfügt über ein festes Schema, das vom MPSD interpretiert und erzwungen.

Im Text der PUT-Anforderungen werden die Systemobjekte überprüft und zusammengeführt werden, wie die benutzerdefinierten Objekte. Aber im Gegensatz zu benutzerdefinierten Objekten, nachdem sie das System zusammengeführt werden, Objekte weiter überprüft und eine Aktion ausgeführt werden, basierend auf diesen Schemas.

**/constants/system**

```json
{
"version": 1,  //Document Version (FAL release version 1, service contract 107)
"maxMembersCount": 100,  // Defaults to 100 if not set on creation. Must be between 1 and 100.
"visibility": "private",  // Or "visible" or "open", defaults to "open" if not set on creation.

"initiators": [ "1234" ],  // If specified on a new session, the creators xuid must be in the list (or the creator must be a server).
"inviteProtocol": "party",  // Optional URI scheme of the launch URI for invite toasts.

"reservedRemovalTimeout": 10000, // Default is 30 seconds. Member is removed from the session.
"inactiveRemovalTimeout": 0, // Default is zero. Member is removed from the session.
"readyRemovalTimeout": 60000, // Default is three minutes. Member is removed from the session.
"sessionEmptyTimeout": 0, // Default is zero. Session is deleted.

// Capabilities are boolean values that are optionally set in the session template. If no capabilities are needed, an empty "capabilities" object should be in the template in order to prevent capabilities from being specified on session creation, unless the title desires dynamic session capabilities.
"capabilities": {
"clientMatchmaking": true,
"connectivity": true, // Cannot be set if ‘large’ is specified.
     "suppressPresenceActivityCheck": false,
     "gameplay": false,
     "large": false
},
/* If a "memberInitialization" object is set, the session expects the client system or title to perform initialization following session creation and/or as new members join the session. The timeouts and initialization stages are automatically tracked by the session, including QoS measurements if any metrics are set. These timeouts override the session's reservation and ready timeouts for members that have 'initializationEpisode' set. */
"memberInitialization": {
"joinTimeout": 20000,  // Milliseconds. Unspecified counts as 10 seconds.
"externalEvaluation": false,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 00 and the session is created with no members to initialize, episode 1 is skipped..
},
```

**/properties/system**

```
{
// Optional array of case-insensitive strings. Cannot be set if the session's visibility is "private".
"keywords": [ "hello" ],

// Array of integer indices of members whose turn it is. Defaults to empty. Can't be set (and doesn't appear) on large sessions.
"turn": [ 0 ],

/* Restricts who can join "open" sessions. (Has no effect on reservations, which means it has no impact on "private" and "visible" sessions.) Defaults to "none". On large sessions, "none" is the only valid value.
If "local", only users whose token's DeviceId matches someone else already in the session and "active": true.
If "followed", only local users (as defined above) and users who are followed by an existing (not reserved) member of the session can join without a reservation. */
"joinRestriction": "none",

// Device token of the host. Must match the "deviceToken" of at least one member, otherwise this field is deleted.
// If "peerToHostRequirements" is set and "host" is set, the measurement stage assumes the given host is the correct host and only measures metrics to that host.
// Can't be set on large sessions.
"host": "99e4c701",

// Can only be set while "initializing/stage" is "evaluating". True indicates success, and false indicates failure. Once set, "initializing/stage" is immediately updated, and this field is removed.
"initializationSucceeded": true,

/* The ordered list of case-insensitive connection strings that the session could use to connect to a game server. Generally titles should use the first on the list, but sophisticated titles could use a custom mechanism (e.g. Thunderhead) for choosing one of the others (e.g. based on load). */
"serverConnectionStringCandidates": [ "datacenter b", "serverfarm a" ],

"matchmaking": {
    "targetSessionConstants": { },
    // Force a specific connection string to be used (useful in preserveSession=always cases).
    "serverConnectionString": "datacenter b",
},

// True if the match that was found didn't work out and needs to be resubmitted. Set to false to signal that the match did work, and the matchmaking service can release the session.
"matchmakingResubmit": true
}

```

### <a name="timeouts"></a>Timeouts

Sitzungen können von Timern und andere externe Ereignisse geändert werden. Die **Timeouts** Objekt im MPSD bietet einen einfachen Mechanismus zum Verwalten von Sitzungsdauer und Member.

Die **NextTimer** Feld der Sitzung bietet, Zeitpunkt der der nächste Timer. Clients können diese Informationen verwenden, um ein gutes Intervall für die der nächsten Abfrage auszuwählen. Dieser Wert wird auch zurückgegeben, der **Expires** HTTP-Header.

Timeouts werden in Millisekunden angegeben. 0 (null) ist zulässig, und gibt an, dass das Timeout sofortige werden soll. Wenn ein angegebenes Timeout nicht angegeben ist, gilt es unendlich. Da die Timeouts über Standardwerte verfügen, sollten die Vorlage "Sitzung" explizit "null" für ein unendliches Timeout angeben.

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

Die **/constants/system/sessionEmptyTimeout** Wert konfiguriert der Anzahl der Millisekunden, nachdem eine Sitzung leer ist, dass es gelöscht. Der Standardwert ist 0 (null), was bedeutet, dass die Sitzung sofort gelöscht wird. Wenn der Wert nicht angegeben ist, werden auf unbestimmte Zeit leere Sitzungen befinden, auf.

#### <a name="member-timeouts"></a>Member-timeouts

Die drei anderen Timeouts in **/Konstanten/System** steuern Sie die Zeitspanne, die ein Element kann in einem bestimmten Zustand bleiben:

-   **reservedRemovalTimeout**

    -   Die Reservierung wird gelöscht, wenn das Timeout abläuft. Der Standardwert ist 30 Sekunden.

-   **inactiveRemovalTimeout**

    -   Ein inaktives Element wird aus der Sitzung nach zwei Stunden standardmäßig entfernt.

-   **readyRemovalTimeout**

    -   Mitglieder, die "bereit" sind, werden nach drei Minuten in den inaktiven Zustand wiederherstellen, wird standardmäßig.

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>Memberinitialisierung in der Sitzung Dokumente

### <a name="member-initialization"></a>Memberinitialisierung


Die **MemberInitialization** Objekt steuert die Initialisierung nach Erstellen der Sitzung bzw. als neue Elemente Phasen an der Sitzung teilnehmen. Die Timeouts und Initialisierung Phasen werden automatisch nachverfolgt, von der Sitzung, einschließlich QoS Messungen aus, wenn von Metriken festgelegt sind.

Diese Timeouts Überschreiben der Sitzungs Reservierung und können Timeouts für Elemente, die die **InitializationEpisode** Eigenschaftensatz.

Beispiel:

```
"memberInitialization": {
        "joinTimeout": 5000,
        "measurementTimeout": 5000,
        "evaluationTimeout": 5000,  // only specify for external evaluation
        "externalEvaluation": true,
       "membersNeededToStart": 2
    },
```


![](../../images/whitepapers/mpsd_image2.png)

**Abbildung 3. Ablauf der Member-Initialisierung.**

Jede der drei Phasen der Memberinitialisierung kann einen Timeout beendet:

1.  **joiningTimeout**

    -   Die Zeitspanne in Millisekunden, die Benutzer der Sitzung beizutreten müssen. Reservierungen von Membern, die nicht hinzugefügt werden, werden entfernt.

2.  **measuringTimeout**

    -   Die Zeitspanne, die Elemente enthalten, die Messungen hochladen. Elemente, die Fehler beim Hochladen des Messungen mit gekennzeichnet sind eine *FailureReason* für "Timeout".

3.  **evaluationTimeout**

    -   Die Zeitspanne für ein Element und die Entscheidung für die Auswertung hochladen. Wenn keine Entscheidung empfangen wird, zählt es als Fehler.

**externalEvaluation**

-   MPSD möglich, eine automatische QoS-Auswertung basierend auf Anforderungen, die in der sitzungsvorlage bereitgestellt. Die Auswertung erfolgt, wenn ExternalEvaluation auf "false" festgelegt ist. **ExternalEvaluation** muss **"true"** bei einer **EvaluationTimeout** festgelegt ist. Wenn zwei Peer-zu-Peer- oder Peer-zu-Host-Anforderungen vorliegen, sollten Sie immer noch festlegen **ExternalEvaluation** zu **"false"** um die Sitzung automatisch die Initialisierung abgeschlossen haben.

**membersNeededToStart**

-   Dies ist die minimale Anzahl von der Members-Reservierungen, die mit "Initialize" vorhanden sein müssen: "true", und übergeben Sie QoS für die automatische Auswertung erfolgreich ausgeführt werden kann.

### <a name="initialization-episodes"></a>Initialisierung Episoden


Wenn die **MemberInitialization** Objekt festgelegt ist, wird MPSD werden die Phasen der Initialisierung von Episode fortgesetzt. Die ersten Folge wird gestartet, wenn die Sitzung erstellt wird, und alle Phasen in der Vorlage definiert umfasst.

Alle neuen Member, die eingeladen und verknüpft werden, während eine Folge bereits ausgeführt wird, werden für der nächsten Folge markiert werden. Der Status einer Episode oder **MemberInitialization** im Allgemeinen können vom Abrufen der **initialisieren** Objekt der Sitzung.

**Beachten Sie** Beachten Sie, dieses Objekt entfernt wird, nachdem die Initialisierung abgeschlossen ist.

Beispiel:

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

Die Phase geht Beitritt zu messen, auswerten. Wenn eine Folge ein Fehler auftritt, Phase nastaven NA hodnotu *Fehler* und die Sitzung kann nicht initialisiert werden. Wenn eine Initialisierung Episode abgeschlossen ist, andernfalls die **initialisieren** Objekt entfernt wird.

Initialisierungsfehler können auch für jedes Element nachverfolgt werden. Sie sind festgelegt, wenn aus der Verknüpfung oder Messen Phase übergeht, wenn dieses Element nicht erfolgreich abgeschlossen.

Beispiel:
```
"initializationFailure": "latency",
```
In der Rangfolge, konnte auf der Wert für dieses Attribut festgelegt werden *Timeout, Latenz, Bandwidthdown, Bandwidthup, Netzwerk, eine Gruppe*, oder *Episode*. Die Netzwerk-Wert bedeutet, dass die Netzwerkkonfiguration und/oder Bedingungen (z. B. in Konflikt stehende Netzwerkadressübersetzung \[NAT\]) verhindert, dass die QoS-Metriken, die gemessen werden. Ist der einzig mögliche Wert am Ende der zu verknüpfenden *Gruppe*. (Bei einem Timeout beitreten, wird die Reservierung entfernt.)

Wenn **MemberInitialization** festgelegt ist, und das Element wurde hinzugefügt, mit der "Initialize": "true", dies festgelegt ist, die Initialisierung-Folge, die das Element im einbezogen werden. Ein Wert von 1 wird verwendet, für die Elemente, die zu einer neuen Sitzung zum Zeitpunkt seiner Erstellung hinzugefügt und es wird entfernt, wenn die Initialisierung Folge endet.
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>Ticket-Sitzung überein

Wenn eine Sitzung MPSD als Übereinstimmung Ticket-Sitzung verwendet wird, werden einige spezielle Sitzungseigenschaften und Konstanten.

**/members/{index}/constants/system**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

Wenn die Vermittlungsdienst Benutzer zu einer Sitzung hinzufügt, es bietet einige um wie und warum sie in der Sitzung in Übereinstimmungen gefunden wurden die **MatchmakingResult** Feld.

```
"matchmakingResult": {
```

Dies ist eine Kopie eines Benutzers **ServerMeasurements** aus der Vermittlung-Sitzung.

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>Quality Service (QoS)-Vorlagen

Für Spiele Sitzung Vorlagen können Werte hinzugefügt werden, die MPSD zur Koordinierung mit der Netzwerkschicht und die Konsole sozialen Plattform informieren. Der Zweck der diese Koordination wird um die Qualität des Verbindungsstatus zu überprüfen, bevor Sie eine Sitzung erstellt wird und ein Benutzer informiert wird, dass das Spiel eingebunden ist.

Der folgende Code-Ausschnitt ist ein Beispiel für eine Peer-zu-Host sitzungsvorlage Spiele mit QoS:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToHostRequirements": {
                "latencyMaximum": 350,
                "bandwidthDownMinimum": 1000,
                "bandwidthUpMinimum": 100,
                "hostSelectionMetric": "latency"
            }
        },
        "custom": {}
    }
}
```

Dieser Code-Ausschnitt ist ein Beispiel einer Peer-zu-Peer-sitzungsvorlage Spiele mit QoS:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToPeerRequirements": {
                "latencyMaximum": 250,
                "bandwidthMinimum": 10000
            }
        },
        "custom": {}
    }
}

```

### <a name="qos-session-template-attributes"></a>QoS-Sitzung Attribute

Wenn eine **MemberInitialization** Objekt festgelegt ist, wird die Sitzung erwartet, dass der Client-System oder Titel, Initialisierung, die nach der sitzungserstellung ausführen und/oder als neue Elemente an der Sitzung teilnehmen.

Die Timeouts und Initialisierung Phasen werden automatisch nachverfolgt, von der Sitzung, einschließlich QoS Messungen aus, wenn von Metriken festgelegt sind.

Diese Timeouts Überschreiben der Sitzungs Reservierung und können Timeouts für Elemente, die die **InitializationEpisode** Eigenschaftensatz.

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

Eine Spiele-Sitzung mit QoS ist die konnektivitätsfunktion erforderlich. Wenn keine Metriken angegeben sind, werden standardmäßig wie benötigt werden, um die QoS-Anforderungen zu erfüllen. Wenn sie festgelegt sind, müssen sie ausreichend, um die QoS-Anforderungen erfüllt sein.

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

Die folgenden Grenzwerte gelten für jede paarweisen Verbindung für alle Elemente in einer Sitzung:

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

Die folgenden Grenzwerte gelten für jede Verbindung aus ein Kandidat für die Host:

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

Die folgenden potenziellen Serververbindung, die Zeichenfolgen sein ausgewertet (Beachten Sie, dass die Verbindungszeichenfolgen müssen kleingeschrieben werden):

```json
"measurementServerAddresses": {
        "east.thunderhead.azure.com": {
            "secureDeviceAddress": "r5Y="  // Base-64 encoded secure-device-address
        },
        "west.thunderhead.azure.com": {
            "secureDeviceAddress": "rwY="
        }
    }
}

```

**members/{index}/properties/system**

Diese Flags steuern, die den Elementstatus und **ActiveTitle**, und sie sind sich gegenseitig ausschließende (es ist ein Fehler, sowohl auf **"true"**). Für jedes **"false"** ist identisch mit "nicht vorhanden." Der Standardstatus ist "inaktiv", d. h. keine Ziele vorhanden sind.

```json
"ready": true,
"active": false,

// Base-64 blob, or not present. Empty-string is the same as not present.
// 'capabilities/connectivity' must be enabled in order for this to be set.
"secureDeviceAddress": "ryY=",

// List of members in my group, by index. Each element must be an integer 0 <= n < 'membersInfo/next'.  
// During member initialization, if any members in the list fail, this member will also fail.
"group": [ 5 ],

// QoS measurements by lower-case device token.
// Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
// Metrics can me omitted if they weren't successfully measured, i.e. the peer is unreachable.
// If a "measurements" object is set, it can't contain an entry for the member's own address.
"measurements": {
"e69c43a8": {
"latency": 5953,  // Milliseconds
"bandwidthDown": 19342,  // Kilobits per second
"bandwidthUp": 944,  // Kilobits per second
"custom": { }
     }
},

// QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
// If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
    "serverMeasurements": {
        "east.thunderhead.azure.com": {
            "latency": 233  // Milliseconds
        }
    },

// Subscriptions for shoulder taps on session changes. The 'profile' indicates which session changes to tap as well as other properties of the registration like the min time between taps.
// The subscription is named with a client-generated GUID that is also sent back with the tap as a context ID.
// Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
// To remove a subscription, set its context ID to null.
// (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
"subscriptions": {
"961dc162-3a8c-4982-b58b-0347ed086bc9": {
"profile": "party",  // Or "matchmaking", "initialization", "roster", "queueHost", or "queue"
"onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
},
"709fef70-4638-4b94-905b-24cb02706eb5": null
}
}

```

## <a name="qos-phase-and-session-initialization-details"></a>QoS-Phase und der angegebenen Sitzung Initialisierung-details

MPSD überwacht und QoS-Ergebnisse für das Erstellen von Spielen zu melden, nachdem die Vorlage Memberinitialisierung abgeschlossen wurde. Wird der Status dieses Vorgangs dargestellt werden, durch eine *initialisieren* -Objekt im Dokument Sitzung wie in beschrieben die [Memberinitialisierung](#_Member_initialization_in) weiter oben. Die *initialisieren* Objekt verfügt über eine *Phase* -Attribut, das die aktuelle Phase der Initialisierung darstellt. Die Phase wechselt von *verknüpfen* zu *messen* zu *auswerten*.

-   Wenn Episode initialisieren \#1 ein Fehler auftritt, und klicken Sie dann die Stufe festgelegt ist, um *Fehler* und die Sitzung kann nicht initialisiert werden. Nach Abschluss eine Initialisierung Episode wird, das Objekt "Initialisieren" entfernt. Wenn **ExternalEvaluation** nastaven NA hodnotu **"false"**, wird die Stufe Auswertung wird übersprungen. Wenn weder **Metriken** noch **MeasurementServerAddresses** festgelegt ist, wird die messen Phase wird übersprungen.

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   Kandidaten des Hosts sind gerätetoken in Reihenfolge ihrer Priorität aufgelistet. Werden festgelegt, nachdem die *messen* Phase der Initialisierung Episode \#1, wenn **PeerToHostRequirements** festgelegt ist und **/properties/system/host** ist nicht festgelegt. Sie werden gelöscht, wenn es eine **/properties/system/host** Objekt festgelegt ist.

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   Die *Gamertag* Attribut wird nur festgelegt werden, wenn das Element akzeptiert hat und ein Anspruch Gamertag für diesen Member gefunden wurde.

```json
"gamertag": "stacy",
```

-   Die *"devicetoken"* -Attribut festgelegt ist, wenn das Element eine sichere Geräteadresse hochlädt. Es ist eine Groß-/Kleinschreibung Zeichenfolge, die für die Durchführung von Gleichheitsvergleichen verwendet werden kann.

```json
"deviceToken": "9f4032ba7",
```

-   Die *reservierte* Wert entfernt wird, nachdem der Benutzer seine erste PUT-Anforderung an das Dokument für die Sitzung ausgeführt wird. Wenn der Spieler reserviert sind, bedeutet, dass, sie, die Spiele-Sitzung eingeladen wurden, aber keines von beiden zugestimmt haben noch ihre Verbindungen ausgewertet werden hatte.

```json
"reserved": true,
```

-   Wenn das Element aktiv ist, ist *ActiveTitleId* ist der Titel in der das Element aktiv, in Dezimalstellen ist.

```json
"activeTitleId": "8397267",
```

-   Dieses Attribut verweist auf die Zeit, dass der Benutzer die Sitzung verknüpft. Wenn *reservierte* ist **"true"**, klicken Sie dann *JoinTime* werden die Zeit, an dem die Reservierung vorgenommen wurde.

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   Vorhanden Sie, wenn dieses Element im Array Eigenschaften/System/aktivieren, andernfalls nicht.

```json
"turn": true,
```

-   Die *InitializationFailure* -Attribut für das Memberobjekt beim Übergang aus der Verknüpfung oder Phase messen, wenn das Element erfolgreich die Phase noch nicht festgelegt ist. In der Rangfolge, es sollte festgelegt werden auf *Timeout*, *Latenz*, *Bandwidthdown*, *Bandwidthup*, *Netzwerk* , *Gruppe*, oder *Episode*.
    Die *Netzwerk* Wert bedeutet, dass die Netzwerkkonfiguration und/oder Bedingungen (z. B. in Konflikt stehende Netzwerkadressübersetzungen \[NATs\]) verhindert, dass die QoS-Metriken, die gemessen werden. Ist der einzig mögliche Wert am Ende der zu verknüpfenden *Gruppe*. (Bei einem Timeout beitreten, wird die Reservierung entfernt.) Die *Episode* Wert wird festgelegt, nach einem fehlgeschlagenen "Evaluation"-Phase auf allen Mitgliedern des Episode Initialisierung, bei denen nicht während der beitreten oder diese messen.

```json
"initializationFailure": "latency",
```

-   Wenn **MemberInitialization** festgelegt ist, und das Element wurde hinzugefügt, mit der "Initialize": "true", dies festgelegt ist, die Initialisierung-Folge, die das Element im einbezogen werden. Der Wert 1 wird verwendet, für die Elemente hinzugefügt, um eine neue Sitzung zu erstellen. Entfernt, wenn die Initialisierung Folge endet.

```json
"initializationEpisode": 1,
```

-   Die *Weiter* Attribut stellt den Indexwert, der das nächste Element in der Sitzung. Sie werden den gleichen Wert wie die *Weiter* -Attribut in der **MembersInfo** -Objekt unten, wenn es keine weiteren Elemente sind hinzufügen.

```json
            "next": 4
        },
        "4": { "next": 5 /* etc */ }
    },
    "membersInfo": {
        "first": 1,  // The first member's index.
        "next": 5,  // The index that the next member added will get.
        "count": 2,  // The number of members.
        "accepted": 1  // The number of members that are no longer 'pending'.
    },
    "servers": {
        "name": {
            "constants": { /* Property Bag */ },
            "properties": { /* Property Bag */ }
        }
    }
}

```

## <a name="xbox-cloud-compute-session"></a>Xbox Cloud Computing-Sitzung

Eine Sitzung für Xbox Cloud Computing-enthält die geordnete Liste der Groß-/Kleinschreibung Verbindungszeichenfolgen, die die Sitzung, zum einen Gameserver herstellen verwenden können. Im Allgemeinen Titel sollte die erste Zeichenfolge in der Liste verwenden, aber anspruchsvolle Titel können einen benutzerdefinierten Mechanismus (z. B. Xbox Live Compute) für die Auswahl einer der anderen (z. B. basierend auf Laden) verwenden.

```json
    "serverConnectionStringCandidates": [ "west.thunderhead.azure.com", "east.thunderhead.azure.com" ],

    "matchmaking": {
        "clientResult": {  // Requires the clientMatchmaking property.
            "status": "searching",  // Or "expired", "found", "failed", or "canceled".
            "statusDetails": "Description",  // Default is empty string.
            "typicalWait": 30,  // The expected number of seconds waiting as a non-negative integer.
            "targetSessionRef": {
                "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
                "templateName": "capture-the-flag",
                "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
            }
        },

        "targetSessionConstants": { },

        // Force a specific connection string to be used (useful in preserveSession=always cases).
        "serverConnectionString": "west.thunderhead.azure.com",

        // True if the match that was found didn't work out and needs to be resubmitted. Set to false
        // to signal that the match did work, and the matchmaking service can release the session.
        "resubmit": true
    }
}

```

** / Server / {Servername} / Eigenschaften/System **

```json
{
    "lockId": "opaque56789",  // If set, a matchmaking service is servicing this session.
    "status": "searching",  // Or "expired", "found", "failed", or "canceled". Optional.
    "statusDetails": "Description",  // Optional free-form text. Default is empty string.
    "typicalWait": 30,  // Optional. The expected number of seconds waiting as a non-negative integer.
    "targetSessionRef": {  // Optional.
        "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
        "templateName": "capture-the-flag",
        "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
    }
}

```

## <a name="raw-session-and-custom-title-properties"></a>RAW-Sitzung und benutzerdefinierte Eigenschaften

Eine Sitzung kann verwendet werden, um benutzerdefinierte organisatorische Informationen (Metadaten) für ein Multiplayer-Spiel zu speichern. Spieledaten oder gespeicherten Informationen sollten im CTS-gespeichert werden.

### <a name="property-bags"></a>Eigenschaftenbehälter

Jedes der oben genannten Objekte als eine Eigenschaftensammlung besteht aus zwei optionale innere Objekte, System- und benutzerdefinierten markiert.

Die benutzerdefinierten Objekte können beliebiger JSON-Code enthalten.

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>Aktives Mitglied decay

Aktive Mitglieder werden automatisch inaktiv gekennzeichnet, wenn MPSD erkennt, dass der Benutzer nicht mehr im Titel beteiligt ist. Dies kann z. B. vorkommen, wenn das Vorhandensein der Datensatz, der Benutzer ein Timeout. Dieser Mechanismus ist nur ein Sicherheitsnetz; d. h. Titel sind erforderlich, damit Sie proaktiv Member als inaktiv markieren (oder entfernen sie aus der Sitzung) anmelden, die "Out" oder andernfalls zu lösen, wenn die Elemente lassen, oder wechseln von den Titel,.

## <a name="faq-and-troubleshooting"></a>Häufig gestellte Fragen und Problembehandlung

### <a name="how-do-i-call-mpsd-"></a>Wie rufe ich MPSD?

Unter Verwendung der Zertifikatauthentifizierung: Client-sessiondirectory.xboxlive.com

Beispiel:

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

Mithilfe der XToken-Authentifizierung: sessiondirectory.xboxlive.com

Beispiel:

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>Wie finde ich, welche SCID sitzungsvorlage und Sandbox verwenden?

Diese Informationen finden Sie auf der Website XDP für den Titel. Wenn Sie noch nicht über Zugriff auf XDP verfügen wenden Sie sich an Ihren Developer Account Manager, die beim Abrufen von Informationen für Sie unterstützen kann.

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>Gibt es ein Beispiel für einen Anforderungstext, die ich vergleichen können eigene auf?


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>Beim Aufrufen von MPSD, erhalte ich einen 404-Fehler.

Sammeln Sie Fiddler-ablaufverfolgungen, um weitere Informationen erhalten und führen Sie Folgendes ein:

1.  Überprüfen Sie die Nachricht als Teil des Texts HttpResponse für allgemeine 404-Meldungen zurückgegeben:

    -   *Der angeforderte Dienstkonfiguration ist entweder ungültig oder nicht konfiguriert für Sitzungen*. Stellen Sie sicher, dass die verwendeten SCID korrekt ist.

    -   *Die angeforderte Sitzung wurde nicht gefunden*. Stellen Sie sicher, dass die Sitzung erstellt und die sitzungsvorlage richtig, ist bevor Sie sie abrufen. Sie können eine Sitzung mit einem PUT-Aufruf erstellen.

2.  Überprüfen Sie den URI, die Sie verwenden.

3.  Starten Sie die Konsole neu, und/oder versuchen Sie es mit einem neuen Benutzer.

4.  Suchen Sie die [Entertainment-Entwicklerforen](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) für den Fehlercode oder eine andere möglichen Lösungen.

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>Ich erhalte Fehler 403 beim Aufrufen von MPSD.

Dies ist normalerweise eine Berechtigungen oder Bereich-Problem. Sammeln Sie Fiddler-ablaufverfolgungen, um weitere Informationen erhalten und führen Sie Folgendes ein:

1.  Meldungen der laufzeitüberprüfung, die als Teil des Texts HttpResponse für allgemeine 403 Nachrichten zurückgegeben werden:

-   * Der angeforderten Dienstkonfiguration kann nicht zugegriffen werden. *

    -   Stellen Sie sicher, dass Sie ein Konto verwenden, die Zugriff auf die Sandbox hat.

    -   Stellen Sie sicher, dass Sie in der richtigen Sandbox sind.

    -   Wenn Sie die Zertifikatauthentifizierung verwenden und dieser Fehler angezeigt, wenden Sie sich an Ihre DAM.

-   *Die angeforderte Sitzung kann nicht zugegriffen werden. Private-Sitzungen können nur von Mitgliedern der Sitzung gelesen werden.*

    -   Sie versuchen, eine Sitzung zuzugreifen, die Sichtbarkeit des "Private". Nur Mitglieder in der Sitzung können das Dokument Sitzung lesen.

-   *Hauptteil der Anforderung kann nicht vorhandenen Memberverweise enthalten, es sei denn, der Prinzipal der Authentifizierung ein Servers umfasst.*

    -   Sie können nicht ein anderer Benutzer eine Sitzung im Auftrag eines Benutzers hinzufügen. Sie können nur einladen. Legen Sie den Index auf "reservieren\_&lt;Anzahl&gt;" einen Player einladen.

### <a name="i-am-getting-a-412-precondition-failed-error"></a>Ich erhalte einen 412-Vorbedingung nicht erfüllt-Fehler.

Dadurch wird die 412 Precondition Failed zurückgegeben, wenn die Sitzung bereits vorhanden ist:

> PUT Serviceconfigs/00000000-0000-0000-0000-000000000000/Sessiontemplates/Quick/Sitzungen/Foo HTTP/1.1-Content-Type: Application/Json-If-None-Match: \*

Dadurch wird die 412 Precondition Failed zurückgegeben, wenn das Etag für die Sitzung nicht den If-Match-Header entspricht:

> PUT Serviceconfigs/00000000-0000-0000-0000-000000000000/Sessiontemplates/Quick/Sitzungen/Foo HTTP/1.1-Content-Type: Application/Json-If - Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>Ich erhalte Fehler, z. B. 405, 409, 503 und aufrufenden MPSD 400when.

Sammeln Fiddler-ablaufverfolgungen können weitere Informationen zu erhalten und überprüfen Sie die Nachrichten, die als Teil des Texts HttpResponse zurückgegeben werden. Daraufhin sollte Ihnen genügend Informationen zum Identifizieren und Beheben des Fehlers oder suchen Sie die [Entertainment-Entwicklerforen](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) nach möglichen Lösungen.

Sie können den Text der Antwort auch abrufen, bei Verwendung der Xbox Live-APIs durch Festlegen der **DiagnosticsTraceLevel** Eigenschaft Fehler wird ausgegeben, die Informationen in die Debugausgabe, oder Sie können die  **XboxLiveContextSettings.ServiceCallRouted** Ereignis wie in mehrere Beispiele zur Ausgabe in der Titelleiste Ihres Benutzeroberfläche gezeigt.

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>Was kann oder sollte werden geändert in den Vorlagen für mein Titel?

Vorlagen für ereignissitzungen sind nicht standardmäßig, aber es sind mehrere eine wirklich. Sie können nicht jedoch die Konstanten, die bereits in den Vorlagen festgelegt überschreiben, auch wenn Sie diese hinzufügen können.

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>Ich erhalte einen Fehler, der besagt, dass meine Sitzung nicht initialisiert wurde.

Bei der Memberinitialisierung in der Vorlage (Spiel, Partei und Übereinstimmung Ticket-Szenarien, in der Regel) vorhanden ist, müssen Sie sicherstellen, dass "Initialisieren = True" für ausreichend Reservierungen Members (Elemente, die erforderlich sind) festgelegt ist, übergeben QoS zur Behebung dieses Problems.

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>Meine Sitzung wird nicht erstellt, und ich erhalte einen Fehler "HTTP 204".

Dies gibt an, dass keine Benutzer, die der Sitzung, die bei der Erstellung hinzugefügt wurden. Wenn für eine Sitzung zum Zeitpunkt der Erstellung kein Benutzer vorhanden sind, wird die Sitzung nicht erstellt werden. Stellen Sie sicher, dass Sie eine Sitzung, bei der Erstellung mindestens einen Player hinzufügen. Erhalten Sie für dedizierte Server-Szenarien einen Player, wer versucht, eine Übereinstimmung zu erstellen, oder erstellen Sie eine Übereinstimmung, und stellen, dass Benutzer den ersten Player in entsprechen muss. Alternativ können Sie ändern oder entfernen Sie die **SessionEmptyTimeout**.

### <a name="when-should-i-poll-the-mpsd"></a>Wann sollte ich die MPSD abrufen?

Vermeiden Sie, ob Sie MPSD Sitzungen abrufen. Auf einer hohen Ebene können Sie dazu Ihren Code auf eine Weise, die die MPSD Sitzung nur für die anfängliche Einrichtung der Netzwerkkonnektivität für jeden Client, und klicken Sie zum Wiederherstellen der Netzwerkstatus für einen Client oder -Clients, die die Konnektivität verloren nutzt entwerfen. Es ist auch wichtig, nutzen MPSDs Etag-basierte Synchronisierungsprimitiven, die nicht erforderlich, aktualisieren Sie den Sitzungszustand, um Racebedingungen zu beheben.

Eine allgemeine Anwendung dieser Prinzipien ist, wenn Sie einen Satz von N-Clients verfügen, die alle miteinander verbinden und in einem Peer-zu-Peer-Netz wiedergeben. Anstatt die Sitzung Abfragen regelmäßig neue Elemente, kann jedes Element an der Sitzung teilnehmen, auf die Elemente bereits in der Sitzung herstellen und wird davon ausgegangen, dass alle späteren mühsamer dasselbe tun werden. Finden Sie in den Beispielen Chat und Player Rendezvous Beispiele für die Implementierung.

Es gibt einige Fälle, in dem Abruf für kurze Zeiträume erforderlich sein kann; Wenn Sie glauben, dass Sie dies tun müssen, wenden Sie sich an Ihren Account Manager, Entwickler.
