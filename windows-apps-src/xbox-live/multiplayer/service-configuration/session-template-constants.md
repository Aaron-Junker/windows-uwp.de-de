---
title: Sitzungsvorlagenkonstanten
description: Beschreibt die Systemkonstanten, die in Vorlagen für Xbox Live Multiplayer-Sitzung definiert.
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox one multiplayer, Vorlage "Sitzung"
ms.localizationpriority: medium
ms.openlocfilehash: 5bb4c6f39bab8779b5675a7b9c81b883965676c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646905"
---
# <a name="session-template-constants"></a>Sitzungsvorlagenkonstanten

Die folgenden Tabellen beschreiben die vordefinierte Elemente einer Vorlage Multiplayer-Sitzung mit der Sitzung Version 107.

## <a name="system"></a>System

System-Konstante  | Beschreibung | Gültige Werte | Standardwert
--|-- | -- | --
version | Die Version der Vorlage für die Sitzung. | 1 - n | keine
maxMembersCount | Die Anzahl der Gesamtdauer der Sitzung Member Slots für die Multiplayer-Aktivität unterstützt. | 1 – 100 für eine normale Sitzung 101 + für eine große Sitzung | 100
visibility | Der Sichtbarkeitszustand der Sitzung, das angibt, ob andere Benutzer finden Sie unter und/oder der Sitzung beitreten können. | Private "," angezeigt wird, öffnen | offen
inviteProtocol | Diese Konstante, um "Game" festlegen, können Gäste, die eine Toast-Benachrichtigung zu erhalten, wenn sie mit der Sitzung eingeladen werden. | Spiel "," Tournamentgame "," Chat "," gameparty | keine
reservedRemovalTimeout  | Das Timeout für eine Member-Reservierung in Millisekunden. Der Wert 0 gibt an, einem sofortigen Timeout. Wenn das Timeout auf null festgelegt ist, gilt es unendlich. | 0 - n, null | 30000
inactiveRemovalTimeout  | Das Timeout für ein Element, in Millisekunden als inaktiv betrachtet. Der Wert 0 gibt an, einem sofortigen Timeout. Wenn das Timeout auf null festgelegt ist, gilt es unendlich. | 0 - n, null | 0
readyRemovalTimeout | Das Timeout für einen Member bereit, in Millisekunden berücksichtigt werden. Der Wert 0 gibt an, einem sofortigen Timeout. Wenn das Timeout auf null festgelegt ist, gilt es unendlich. | 0 - n, null | 180000
sessionEmptyTimeout | Das Timeout bei einer leeren Sitzung wird in Millisekunden. Der Wert 0 gibt an, einem sofortigen Timeout. Wenn das Timeout auf null festgelegt ist, gilt es unendlich. | 0 - n, null | 0
[**Funktionen**](#capabilities) | Gibt an, das die Funktionen der Sitzung. Finden Sie im Abschnitt Funktionen. | n. a. | n. a.
[**Metriken**](#metrics) | Gibt einen Satz von Titel definiert servicequalitätsanforderungen, z. B. Latenz und Bandbreite Geschwindigkeit, die Elemente in der Sitzung erfüllen müssen.  | n. a. | n. a.
[**memberInitialization**](#memberInitialization) | Gibt die Timeouts und Initialisierung an, die erzwungen werden, wenn neue Mitglieder der Sitzung beizutreten. Member-Initialisierung unten wird angezeigt. | n. a. | n. a.
[**peerToPeerRequirements**](#peerToPeerRequirements) | Gibt an, das Netzwerk servicequalitätsanforderungen für Peer-to-Peer-Netz-Verbindungen. Finden Sie im Anforderungsabschnitt Peer-to-Peer. |n. a. | n. a.
[**peerToHostRequirements**](#peerToHostRequirements) | Gibt an, das Netzwerk servicequalitätsanforderungen für Peer-zu-Host-Verbindungen. Finden Sie in der Peers Host Anforderungen Abschnitt weiter unten. | n. a. | n. a.
[**measurementServerAddresses**](#measurementserveraddresses) | Gibt eine Auflistung von möglichen Rechenzentren, die verwendet werden, um QoS-Messungen zu ermitteln. Finden Sie im Abschnitt MeasurementServerAddresses. | n. a. | n. a.
[**cloudComputePackage**](#cloudComputePackage) | ? | n. a. | n. a.
[**arbitration**](#arbitration) | Gibt an, die Timeouts für Elemente Vermittlung führt Turniere zu übermitteln. Finden Sie im Abschnitt CloudComputePackage. | n. a. | n. a.
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | Gibt eine Liste der Titel, IDs, die Lesezugriff auf die Sitzung immer müssen. Finden Sie im Abschnitt BroadcastViewerTitleIds. | n. a. | n. a.
[**ownershipPolicies**](#ownershipPolicies) | Gibt die Richtlinien in Bezug auf die Sitzung den Besitz an. Finden Sie im Abschnitt OwnershipPolicies. | n. a. | n. a.


## <a name="capabilities"></a>capabilities
Funktionen sind boolesche Werte, die optional in der sitzungsvorlage festgelegt sind. Wenn keine Funktionen erforderlich sind, sollte ein leeres "Capabilities"-Objekt in der Vorlage sein, um zu verhindern, dass der Funktionen, die bei der sitzungserstellung, angegeben werden, wenn der Titel dynamische Sitzung Funktionen wünscht.

Funktion |  description | Gültige Werte | Standardwert
-- | -- | -- | -- |
unterbrochen werden. | Gibt an, wenn die Sitzung Peer-Verbindungen unterstützt. Wenn dieser Wert auf "false" festgelegt ist, aktivieren Sie dann die Sitzung kann kein Metriken verfügbar ist, und die Sitzung Mitglieder ihre SecureDeviceAddress können nicht festgelegt werden. Kann nicht für große Sitzungen festgelegt werden. | "true", "false" | false
suppressPresenceActivityCheck | True gibt an, deaktiviert die Vorhandensein überprüft. | "true", "false" | false
Spielverlauf | Gibt an, ob die Sitzung eigentliche Spiel, und nicht im Menü Setup /-Uhrzeit wie bei einem Lobby oder Vermittlung darstellt. Bei "true", ist die Sitzung im Modus für Spiele. | "true", "false" | false
groß | Gibt an, ob die Sitzung eine große-Sitzung (mehr als 100 Elemente). Große Sitzungen werden für die Verwendung mit Multiplayer-Manager nicht unterstützt. | "true", "false" | false
connectionRequiredForActiveMembers | Gibt an, wenn eine Verbindung in der Reihenfolge erforderlich ist, für ein Element aktiv sein. | "true", "false" | false
cloudCompute | Ermöglicht Clients, um anzufordern, dass eine Cloud-Compute-Instanz im Namen der Sitzung zugeordnet werden. | "true", "false" | false
autoPopulateServerCandidates | Automatisch berechnet, und legen Sie 'ServerConnectionStringCandidates' von "ServerMeasurements". Diese Funktion kann nicht für große Sitzungen festgelegt werden. | "true", "false" | false
userAuthorizationStyle | Gibt an, wenn die Sitzung Aufrufe von Plattformen ohne starke Titel Identity unterstützt. Diese Funktion kann nicht für große Sitzungen festgelegt werden.</br></br>Festlegen der `userAuthorizationStyle` Möglichkeit, `true` Standardwerte der `readRestriction` und `joinRestriction`Sitzungsverhalten `local` anstelle von `none`. Dies bedeutet, dass der Titel müssen bei der Suche verwenden oder übertragen Handles Spiele Sitzung nicht beitreten.| "true", "false" | false
crossplay | Gibt an, dass die Sitzung crossplay zwischen PC und Xbox One-Geräte unterstützt. | "true", "false" | false
broadcast | Gibt an, dass die Sitzung eine Übertragung darstellt. Der Name der Sitzung muss der von den Broadcaster Xuid sein. Erfordert die Funktion "große". | "true", "false" | false
Team | Gibt an, dass die Sitzung ein Turnier Team darstellt. Diese Funktion kann nicht auf 'large' oder "Spiel" Sitzungen festgelegt werden. | "true", "false" | false
Vermittlung | Gibt an, dass die Sitzung von einem Dienstprinzipal erstellt werden muss, die den "verteilen"-Eintrag hinzufügt. Kann nicht auf 'large' Sitzungen festgelegt werden, jedoch ist die "Gaming" erforderlich. | "true", "false" | false
hasOwners | Gibt an, dass die Sitzung eine Sicherheitsrichtlinie, die basierend auf bestimmten Mitgliedern, wird der Besitzer ist. | "true", "false" | false
durchsuchbare | Gibt an, dass die Sitzung eine zielsitzung ein Suchhandle kann. Wenn die Funktion 'UserAuthorizationStyle' festgelegt ist, kann die Funktion "searchable" festgelegt werden, wenn die Funktion "HasOwners" nicht festgelegt ist. | "true", "false" | false

Beispiel:

```json
"capabilities": {
    "connectivity": true,  
    "suppressPresenceActivityCheck": true,
    "gameplay": true,
    "large": true,
    "connectionRequiredForActiveMembers": true,
    "cloudCompute": true,
    "autoPopulateServerCandidates": true,
    "userAuthorizationStyle": true,
    "crossPlay": true,  
    "broadcast": true,  
    "team": true,   
    "arbitration": true,   
    "hasOwners": true,   
    "searchable": true  
},
```

## <a name="metrics"></a>Metriken
Wenn die `metrics` Eigenschaften sind nicht angegeben ist, wird standardmäßig die Werte, die erforderlich sind, um die Qualität der Anforderungen zu erfüllen.  
Wenn sie festgelegt sind, müssen die Werte sein ausreichen, um die Qualität der Anforderungen zu erfüllen.
Dieses Element ist nur gültig, wenn die Sitzung hat die `connectivity` Funktion festlegen.

Metrik | Beschreibung | Gültige Werte | Standardwert
-- | -- | -- | --
Wartezeit | | "true", "false" | finden Sie unter Beschreibung
bandwidthDown | | "true", "false" | finden Sie unter Beschreibung
bandwidthUp | | "true", "false" | finden Sie unter Beschreibung
Benutzerdefinierte | | "true", "false" | finden Sie unter Beschreibung

Beispiel:
```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true
},
```

## <a name="memberinitialization"></a>memberInitialization
Wenn eine `memberInitialization` Objekt festgelegt ist, wird die Sitzung erwartet, dass der Client-System oder Titel, Initialisierung, die nach der sitzungserstellung ausführen und/oder als neue Elemente an der Sitzung teilnehmen.  
Die Timeouts und Initialisierung Phasen werden automatisch nachverfolgt, von der Sitzung, einschließlich QoS Messungen aus, wenn von Metriken festgelegt sind.  
Diese Timeouts Überschreiben der Sitzungs Reservierung und können Timeouts für Elemente, die "InitializationEpisode" festgelegt.  
Kann nicht für große Sitzungen angegeben werden.

Element  | Beschreibung | Gültige Werte | Standardwert
-- | -- | -- | --
joinTimeout | Gibt die Anzahl der Millisekunden, die Mitglied der Sitzung beizutreten. Reservierungen von Benutzern, die nicht hinzugefügt werden, werden entfernt.</br>**Hinweis:** Der Standardzeitraum beträgt für Titel der normalen Ausführung ausreichend, jedoch kann dies um zu Timeouts zu verknüpfen, wenn Sie ein Titel während des Ausführungsflusses MPSD gedebuggt wird. Für diese Szenarien außer Kraft setzen, und der Standardwert für die Sitzung zu erhöhen.| 0 - n | 10000
measurementTimeout | Gibt die Anzahl der Millisekunden, die eine Sitzung-Element zum Hochladen von Messungen wurde. Ein Member, die ein Fehler, zum Hochladen von Messungen auftritt ist mit einem Grund für Fehler bei der "Timeout" markiert.  | 0 - n | 30000
evaluationTimeout | Gibt die Anzahl der Millisekunden, die eine externe Bewertung Messungen hochladen. | 0 -n | 5000
externalEvaluation | True gibt an, dass der Titel-Code die Auswertung führt, die ein Join auf QoS-Messungen basieren. Der Multiplayer-Dienst führt keine QoS-Logik, und der Titel ist verantwortlich für die Weiterentwicklung der Initialisierungsphase. Titel ist nicht in der Regel dadurch erforderlich. | "true", "false" | false
membersNeededToStart | Die Anzahl der Elemente, die zum Starten der Sitzung, für die Initialisierung Episode 0 (null) nur erforderlich. | 1 – maxMembersCount | 1

Beispiel:
```json
"memberInitialization": {
    "joinTimeout": 10000,  
    "measurementTimeout": 30000,  
    "evaluationTimeout": 5000,
    "externalEvaluation": false,
    "membersNeededToStart": 1
},
```


## <a name="peertopeerrequirements"></a>peerToPeerRequirements

Peer-zu-Peer-Netzwerk-Anforderungen | Beschreibung | Standardwert
-- | -- |--
latencyMaximum | Die maximale Wartezeit in Millisekunden zwischen zwei Clients. | 250
bandwidthMinimum | Die Mindestbandbreite in Kbit / s zwischen zwei Clients. | 10000

Beispiel:
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

Peer-zu netzwerkanforderungen des Hosts | Beschreibung | Gültige Werte | Standardwert
-- | -- | -- | --
latencyMaximum | Die maximale Wartezeit in Millisekunden, für die Peer-zu-hostverbindung. | | 250
bandwidthDownMinimum | Die Mindestbandbreite in Kilobits pro Sekunde für Informationen, die zwischen dem Host und dem Peer gesendet. | | 100000
bandwidthUpMinimum | Die Mindestbandbreite in Kilobits pro Sekunde für Informationen, die vom Peer an den Host gesendet werden soll. | | 1000
hostSelectionMetric | Gibt an, welche Metrik verwendet wird, um den Host auswählen. | Bandwidthup, Bandwidthdown, Bandbreite und Latenz | Wartezeit

Beispiel:
```json
"peerToHostRequirements": {
    "latencyMaximum": 250,
    "bandwidthDownMinimum": 100000,
    "bandwidthUpMinimum": 1000,  
    "hostSelectionMetric": "bandwidthup"
},
```

## <a name="measurementserveraddresses"></a>measurementServerAddresses
Der Satz von potenziellen Server-Verbindungszeichenfolgen, die ausgewertet werden soll. Die Verbindungszeichenfolgen müssen Kleinbuchstaben sein.
Kann nicht für große Sitzungen angegeben werden.

Die Verbindungszeichenfolgen werden im folgenden Format definiert:

`"<server name>" : {deviceAddress}`

Die Adresse des Geräts ist, in dem wie folgt beschrieben:

Server-Verbindungszeichenfolge | Beschreibung
-- | --
secureDeviceAddress | Die Base64-codierte sicheres Gerät-Adresse des Servers

Beispiel:
```json
"measurementServerAddresses": {
    "server farm a": {
        "secureDeviceAddress": "r5Y="
    },
    "datacenter b": {
        "secureDeviceAddress": "rwY="
    }
},
```

## <a name="cloudcomputepackage"></a>cloudComputePackage
Gibt an, dass die Eigenschaften der Cloud-compute-Pakets zugewiesen werden. Erfordert, dass die `cloudCompute` Funktion festgelegt ist.

Cloud Computing-Eigenschaft | Beschreibung
-- | -- | -- | --
titleId | Gibt an, den Titel, die ID der Cloud Computing Paket zugewiesen werden.
gsiSet | Gibt die GSI des Cloud-Compute-Pakets zugewiesen werden.
Variant | Gibt an, die Variante des des Cloud-Compute-Pakets zugewiesen werden.

Beispiel:
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>Vermittlung
Gibt an, die Timeouts für die Vermittlungsprozess. Erfordert, dass die `arbitration` Funktion festgelegt ist. Die Startzeit für die Vermittlung wird in einer Sitzung definiert die */servers/arbitration/constants/system/startTime* Element.

timeout | Beschreibung | Gültige Werte | default
-- | -- | -- | --
forfeitTimeout | Gibt die Zeitspanne in Millisekunden, nach der Startzeit der Vermittlung, muss noch festgelegt | 0 - n | 60000
arbitrationTimeout | Gibt an, die Zeit in Millisekunden, nach der Startzeit Vermittlung, die das Ergebnis der Vermittlung ein auftritt Timeout. Dieser Wert darf nicht kleiner als der `forfeitTimeout` Wert | 0 - n | 300000

Beispiel:
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

Gibt ein Array des Titels IDs der Titel, die immer über Lesezugriff auf die broadcast-Sitzung müssen.

Beispiel:
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
Gibt an, wie eine Sitzung zu behandeln, wenn der letzte Besitzer die Sitzung verlässt. Erfordert, dass die `hasOwners` Funktion festgelegt ist.

Besitz-Richtlinie | Beschreibung | Gültige Werte | default
-- | -- | -- | --
Migration | Gibt an, das Verhalten, das auftritt, wenn der letzte Besitzer die Sitzung verlässt. Wenn die Richtlinie für die Migration auf "Endsession" festgelegt ist, läuft die Sitzung. Wenn die Richtlinie für die Migration "absteigend" festgelegt ist, wählen Sie das Element mit dem frühesten Zeitpunkt der Verknüpfung zu den neuen Besitzer der Sitzung. | "ältesten", "Endsession" | "Endsession"

Beispiel:
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```
