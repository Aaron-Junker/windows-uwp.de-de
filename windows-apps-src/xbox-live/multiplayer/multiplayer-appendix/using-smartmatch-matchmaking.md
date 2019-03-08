---
title: Verwenden von SmartMatch Vermittlung
description: Erfahren Sie, wie Xbox Live SmartMatch verwenden, um Spieler eines Multiplayer-Spiels zu vergleichen.
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox one multiplayer, Vermittlung, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 89f33768efcd649987866fd0798c222aa97f7ff8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592095"
---
# <a name="using-smartmatch-matchmaking"></a>Verwenden von SmartMatch Vermittlung

Das folgende Flussdiagramm veranschaulicht den Prozess der SmartMatch Vermittlung.

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Erstellen eine Match-Ticket-Sitzung und ein Ticket für die Übereinstimmung

Vor dem Starten Vermittlung, richtet ein datenbankgesteuertes "Scout" eine Übereinstimmung Ticket-Sitzung auf die Gruppe von Personen darstellen, die Vermittlung zusammen eingeben möchten. Alle Benutzer in dieser Gruppe beitreten der Sitzung mit dem **MultiplayerSession.Join-Methode (String, Boolean, Boolean)**.

Sobald die Sitzung Ticket erstellt und mit Spielern aufgefüllt wurde, sendet der Titel die Sitzung in der Vermittlung-Dienst mit der **MatchmakingService.CreateMatchTicketAsync Methode**. Diese Methode erstellt ein Match-Ticket, das die Ticket-Sitzung darstellt, und aktualisiert das /servers/matchmaking/properties/system/status-Feld in der Sitzung Ticket "Suche". Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen Sie ein Ticket Übereinstimmung](multiplayer-how-tos.md).

Die Antwort von der Erstellungsmethode für Übereinstimmung Ticket ist eine **CreateMatchTicketResponse Klasse** Objekt. Die Antwort enthält die Match-Ticket-ID, eine GUID, die verwendet werden kann, kann Vermittlung Abbrechen, indem Sie das Ticket löschen verwendet werden. Die Antwort enthält auch eine durchschnittliche Wartezeit für das Hopper, die zum Festlegen von Erwartungen der Benutzer verwendet werden kann.


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Die Sitzung und der Spieler festlegen Vermittlung Attribute

Wenn Sie eine Sitzung mit Vermittlung zu übermitteln, kann der Titel Attribute festlegen, die dem Vermittlungsdienst verwendet, um die Sitzung mit anderen Sitzungen zu gruppieren. Attribute können auf der Ebene Ticket oder auf der Ebene pro-Member angegeben werden.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Festlegen der Vermittlung Attribute auf der Ebene der Match-Ticket

Der Titel übermittelt Attribute auf der Ebene des Match-Ticket in der *TicketAttributesJson* Parameter der **MatchmakingService.CreateMatchTicketAsync** Methode. Attribute, die auf der Ebene Ticket angegebene überschreiben die gleichen Attribute, die auf der Ebene pro-Member angegeben.


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Vermittlung-Attribute festlegen auf einzelne Mitglieder

Der Titel Gibt Attribute an einzelne Mitglieder für jedes Element in der Match-Ticket-Sitzung. Diese werden festgelegt, durch den Aufruf der **MultiplayerSession.SetCurrentUserMemberCustomPropertyJson Methode**, verwenden den Eigenschaftennamen des "MatchAttrs". Dieser Aufruf wird die Attribute in den /members/ {Index} / Eigenschaften/benutzerdefinierte/MatchAttrs Feld für jeden Spieler innerhalb der Sitzungs Ticket.

Der Prozess für die Vermittlung vereinfacht"" pro-Member jedes Sie in ein einzelnes Ticket-Level-Attribut, auf der Grundlage der Flatten-Methode, die für dieses Attribut in der Xbox Live-Konfiguration für das Hopper angegeben. Diese Konfiguration auf [XDP](https://xdp.xboxlive.com) oder [Partner Center](https://partner.microsoft.com/dashboard).


## <a name="making-the-match"></a>Herstellen der Übereinstimmung

Mit der Ticket-Sitzung, und die Übereinstimmung Ticket einrichten, entspricht der Vermittlung-Dienst die dargestellten Ticket-Sitzung mit anderen Ticket-Sitzungen, die anderen Gruppen darstellt und erstellt oder identifiziert die Sitzung eine Match-Ziel. Der Dienst erstellt außerdem in der zielsitzung Reservierungen für die übereinstimmenden Spieler und markiert anschließend die Ticket-Sitzungen aus, wie abgeglichen. MPSD benachrichtigt den Titel dieser Änderung in der Sitzung Ticket.

Jetzt muss der Titel dann Maßnahmen ergreifen, um die Initialisierung der zielsitzung, um sicherzustellen, dass genügend Player angezeigt, und führen Quality Service (QoS)-Überprüfungen, um sicherzustellen, dass sie erfolgreich eine Verbindung miteinander herstellen können. Wenn Initialisierung bzw. QoS fehlschlägt, markiert der Titel die Ticket-Sitzung für erneute Übermittlung zu Vermittlung, damit eine andere Gruppe gefunden werden kann. Weitere Informationen zu den Prozessen, finden Sie unter [Sitzungsinitialisierung Ziel und QoS](smartmatch-matchmaking.md).

Während der Match-Aktivität werden die folgenden Änderungen für die Sitzung in der JSON-Objekte erstellt:

-   /Servers/Matchmaking/Properties/System/Status Feld auf "gefunden"
-   /Servers/Matchmaking/Properties/System/targetSessionRef Feld auf der zielsitzung
-   /Members/ {Index} / Eigenschaften/benutzerdefinierte/MatchAttrs-Feld für jede Sitzung Ticket kopiert werden, um die /members/ {Index} / Konstanten/benutzerdefinierte/MatchmakingResult/PlayerAttrs Feld
-   Für jeden Spieler, ticket Attribute aus dem Feld TicketAttributes im Ticket Übereinstimmung in /members/ {Index} kopiert/Konstanten/benutzerdefinierte/MatchmakingResult/TicketAttrs Feld


## <a name="maintaining-the-match-ticket"></a>Verwalten Sie das Ticket Übereinstimmung

Die Vermittlungsdienst wird eine Momentaufnahme der Ticket-Sitzung verwendet, die zum Zeitpunkt der Erstellung der Match-Ticket für die Sitzung. Wenn also alle Spieler teilzunehmen, oder lassen Sie die Ticket-Sitzung, muss der Titel verwenden die Vermittlungsdienst gelöscht und neu erstellt das Ticket Übereinstimmung.


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>Wiederverwenden der Game-Sitzungs als Übereinstimmung Ticket-Sitzung

| Wichtig                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Es ist wichtig zu wissen, zwei Sitzungen mit *PreserveSession* auf Always festgelegt werden nicht miteinander überein, da sie können nicht kombiniert werden. Die Vermittlung-Ablauf, mit der Titel sollte diese Tatsache berücksichtigt werden. |

Ein Titel kann eine vorhandene Spiele Sitzung als Übereinstimmung Ticket Sitzung finden Sie weitere Spieler, ein Spiel zu verknüpfen, der bereits ausgeführt wird, wiederverwenden. Um dies zu ermöglichen, muss der Titel Übereinstimmung Ticket erstellen, durch den Aufruf **CreateMatchTicketAsync** mit der *PreserveSession* Parametersatz zu **PreserveSessionMode.Always** . Die Vermittlungsdienst wird sichergestellt, dass die vorhandene Sitzung verwendet wird, für das Ticket wird während des Prozesses für die Vermittlung beibehalten, und wird das resultierende zielsitzung.


## <a name="deleting-the-match-ticket"></a>Löschen das Ticket Übereinstimmung

So löschen Sie das Ticket Übereinstimmung, die Title-Aufrufe **MatchmakingService.DeleteMatchTicketAsync Methode**. Löschen des Tickets:

1.  Beendet die Vermittlung, für die Benutzer in der Sitzung Ticket.
2.  Updates "abgebrochen /servers/matchmaking/properties/system/status Felds in die Sitzung Ticket".


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>Ausführen der Vermittlung für Spiele mithilfe von Xbox Live-Serverdienst

Hier sind die allgemeinen Schritte, die zum Abrufen von einem Benutzer Matchmade in einem Spiel Xbox Live Compute-basierte ausgeführt werden. Ein ähnlicher Ablauf muss für Spiele, die von Drittanbietern gehosteten angewendet werden.
1.  Der Scout erstellt eine Ticket-Sitzung, um der Gruppe darstellen. Diese Sitzung enthält eine Liste der möglichen Rechenzentren, in der Sitzungskonfiguration in /constants/system/measurementServerAddresses. Sie stammen von entweder die Vorlage "Sitzung", wenn die Datacenter-Liste statisch ist, oder klicken Sie auf dem Client, der es sich bei der sitzungserstellung nach Erhalt es von Xbox Live Compute erste schreiben. Diese Sitzung enthält auch GsiSetId GameVariantId und MaxAllowedPlayers Werte des TargetSessionConstants/benutzerdefinierte/GameServerPlatform-Objekts.
2.  Alle anderen Clients in der Gruppe an der Ticket-Sitzung teilnehmen.
3.  Alle Mitglieder der Gruppe die MeasurementServerAddresses-Werte aus dem /constants/system-Objekt für die Ticket-Sitzung heruntergeladen, Pingen sie über die API-Plattform und eine geordnete Liste der bevorzugten Rechenzentren auf die Sitzung hochladen, gemäß /members/ {Index} /Properties/System/serverMeasurements.

| Hinweis                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der Titel kann festlegen und Abrufen von MeasurementServerAddresses Werte aus der Sitzung mit dem **MultiplayerSession.SetMeasurementServerAddresses** Methode und die  **MultiplayerSessionConstants.MeasurementServerAddressesJson Eigenschaft**. |

4.  Die Scout-Aufrufe **CreateMatchTicketAsync**, und übergeben Sie einen Verweis auf die Ticket-Sitzung.

| Hinweis                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wenn Sitzungsobjekte Ticket nicht übereinstimmende Konstanten verfügen, kann die Create Ticket-Methode nicht erfolgreich. Dies kann vermieden werden, durch das Hinzufügen einer muss mit dieser Regel werden die Hopper, um zu verhindern, Übereinstimmung mit der Spieler mit Konstanten falsch zugeordnet haben. |

Wenn die **MatchTicketDetailsResponse.PreserveSession Eigenschaft** festgelegt ist nie, kopiert der Vermittlungsdienst Server Messungen aus jedem Member, in die interne Darstellung des Tickets. Es vereinfacht die Server-Messungen der Elemente des Tickets, das in einer Einzelserver-Messungen-Sammlung für das Ticket, das in die interne Darstellung des Tickets als "spezielle" Ticket Attribut gespeichert.

Wenn **MatchTicketDetailsResponse.PreserveSession** nastaven NA hodnotu immer auf dem Server aus, die Messungen werden nicht verwendet. Stattdessen kopiert die Vermittlungsdienst /properties/system/matchmaking/serverConnectionString Wert für die Sitzung, in die interne Darstellung des Tickets, wie eine ServerMeasurements-Auflistung der Größe 1 mit einer Latenz von 0 (null).

5.  Die Vermittlungsdienst entspricht die Ticket-Sitzung mit anderen Benutzern, die andere Gruppen und nehmen den Server Messung von Sammlungen berücksichtigt darstellt. Wird versucht, die Gruppe mit anderen Gruppen übereinstimmen, die die gleichen Rechenzentren hoch bevorzugt.
6.  Nach eine übereinstimmenden Gruppe gefunden wurde, erstellt die Vermittlungsdienst oder identifiziert eine zielsitzung, und fügt alle Spieler der Ticket-Sitzungen, die miteinander verglichen werden. Der Dienst schreibt die endgültige vereinfachte Server-Messwerte für die passende Gruppe in /properties/system/serverConnectionStringCandidates. Schreibt er die ursprünglich übermittelten Server Messungen für jeden neu hinzugefügten Member in der zielsitzung in /members/ {Index} / Konstanten/System/MatchmakingResult/ServerMeasurements.
7.  Alle Clients die Initialisierung für die zielsitzung, wie oben beschrieben ausführen. Jedoch, da die Clients Xbox Live Compute verbunden werden soll, führen sie keine QoS zum Bestätigen der Konnektivität aus.
8.  Einige oder alle Clients Aufruf der **GameServerPlatformService.AllocateClusterAsync Methode**. Erstens wird die Zuordnung, ausgelöst, während die anderen nur eine Bestätigung empfangen. Die Methode ruft die zielsitzung aus MPSD ab und wählt ein Datencenter, die basierend auf der Datacenter-Einstellungen in der zielsitzung als auch Last und andere Informationen zu Xbox Live Compute-spezifische kennen.
9.  Alle Clients spielen.


## <a name="see-also"></a>Siehe auch

[SmartMatch Laufzeitvorgängen](smartmatch-matchmaking.md)

[SmartMatch Vermittlung](smartmatch-matchmaking.md)

**Microsoft.Xbox.Services.Matchmaking-Namespace**

**Microsoft.Xbox.Services.Multiplayer-Namespace**
