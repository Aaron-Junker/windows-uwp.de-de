---
title: SmartMatch Matchmaking
description: Erfahren Sie, bis die Xbox Live SmartMatch Vermittlungsdienst für übereinstimmende Spieler eines Multiplayer-Spiels.
ms.assetid: ba0c1ecb-e928-4e86-9162-8cb456b697ff
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox one multiplayer, Vermittlung, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 6bc5f15e6fdb7d2f393daef8d21c134579610a9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647295"
---
# <a name="smartmatch-matchmaking"></a>SmartMatch Matchmaking

Dieser Artikel enthält die folgenden Abschnitte
* Einführung in die SmartMatch
* SmartMatch Laufzeitvorgängen
* Konfigurieren von SmartMatch für den Titel
* Team-Regeln definieren, der während der SmartMatch-Konfiguration
* Ziel-Sitzungsinitialisierung und QoS

## <a name="introduction-to-smartmatch"></a>Einführung in die SmartMatch

Die XSAPI bietet Vermittlungsdienst mit der Bezeichnung SmartMatch, die von umschlossen ist die [Multiplayer-Manager-API](../multiplayer-manager/multiplayer-manager-api-overview.md).  Erweiterte API-Verwendung finden Sie in der **MatchmakingService Klasse**, aber wenn Sie feststellen, Sie haben ein datenbankgesteuertes-Szenario, das nicht möglich, mithilfe des Managers Multiplayer-Spiele zu implementieren, geben Sie Feedback an uns über Ihre DAM.  Unabhängig davon, welche API, die Sie verwenden, müssen Sie die konzeptionelle Informationen in diesem Artikel gelten.

SmartMatch Vermittlung gruppiert Spieler basierend auf Benutzerinformationen und die Vermittlung-Anforderung für die Benutzer, die zusammen wiedergeben möchten. Vermittlung basiert auf Server, was bedeutet, dass die Benutzer geben Sie eine Anforderung an den Dienst, und sie werden später benachrichtigt, wenn eine Übereinstimmung gefunden wird. Wenn Sie einen Titel für Xbox One zu erstellen, können Sie SmartMatch wie beschrieben in [mithilfe SmartMatch Vermittlung](using-smartmatch-matchmaking.md). Alternativ können Sie Ihre eigenen Vermittlungsdienst Siehe [mit Ihren eigenen Vermittlungsdienst](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/multiplayer-and-networking/using-your-own-matchmaking-service). Beachten Sie, dass der Zugriff auf diesen Link, benötigen Sie eine [Partner Center](https://partner.microsoft.com/dashboard) Konto, das für die Xbox Live-Entwicklung aktiviert ist.

### <a name="about-smartmatch"></a>Informationen zu SmartMatch

Die Vermittlungsdienst arbeitet eng mit MPSD zur Vereinfachung der Vermittlung verwendet werden. Titel für die dies einfach Vermittlung im Hintergrund, z. B. während der Benutzer einzelnen-Player abgespielt wird dadurch in den Titel.

Einzelpersonen oder Gruppen, die Vermittlung eingeben möchten eine Match-Ticket-Sitzung erstellen und dann die Vermittlungsdienst Weitere Entwicklungen, mit denen zum Einrichten einer Übereinstimmung zu finden. Dies führt zur Erstellung eines temporären "Übereinstimmung Ticket" sich innerhalb der Vermittlungsdienst für eine bestimmte Zeitspanne befinden.

Wählt die Vermittlungsdienst Sitzungen zur Wiedergabe zusammen basierend auf Konfiguration, Statistiken, die für jeden Spieler und keine weiteren Informationen angegeben, die zum Zeitpunkt der Anforderung überein. Der Dienst erstellt anschließend eine Match-Ziel-Sitzung, die alle Spieler, die zugeordnet wurden enthält, und der Benutzer die Titel der Übereinstimmung benachrichtigt.

Wenn die zielsitzung bereit ist, führen Sie Titel QoS-Überprüfungen, um sicherzustellen, dass die Gruppe kann Zusammenspiel und anschließend Play beginnen, wenn alles gut ist. Während der QoS-Prozess und Matchmade Spiels Titeln Stand des Sitzungszustands in MPSD, und sie Benachrichtigungen von MPSD über Änderungen an der Sitzung. Solche Änderungen gehören Benutzer, die ein- und ausgehende, und Änderungen an der Sitzung Arbiter.


### <a name="match-ticket-session"></a>Match-Ticket-Sitzung

Eine Match-Ticket-Sitzung stellt dar, die Clients für die Spieler mit der eine Übereinstimmung vornehmen möchten. Es wird basierend auf einem Spiel oder auf eine Gruppe von fremden, die in einem Lobby zusammen sind oder auf anderen Titel-spezifische Gruppierungen der Spieler erstellt. In einigen Fällen möglicherweise die Ticket-Sitzung eine Spiele-Sitzung bereits in Bearbeitung befindlichen, die für mehrere Spieler sucht.


### <a name="match-ticket"></a>Match-Ticket

Senden eine Sitzung Ticket Vermittlung führt zur Erstellung eines Match-Tickets, die den Vermittlung Versuch nachverfolgt. Attribute in the-Ticket, z. B. Spiele Zuordnung oder Spielerebene, zusammen mit Attributen, die der Player in der Sitzung Ticket werden verwendet, um die Übereinstimmung zu ermitteln.
#### <a name="hoppers"></a>Hoppers

Hoppers sind logische stellen, an dem Übereinstimmung Tickets gesammelt werden. Nur die Tickets in der gleichen Hopper können zugeordnet werden. Ein Titel kann mehrere Hoppers verfügen. Beispielsweise können Sie durch ein Titel ein Hopper erstellen, für welche Spieler Fähigkeit das wichtigste Element für den Abgleich ist. Sie können einen anderen Hopper verwenden, in dem Spieler nur verglichen werden, wenn sie den gleichen herunterladbaren Inhalt erworben haben.


#### <a name="hopper-rules"></a>Hopper-Regeln

Hopper Regeln bieten die Definitionen der Kriterien, die dem Vermittlungsdienst verwendet für die Wahl der Spieler, um zu gruppieren. Es gibt zwei Arten von Regeln:   MUSS Regeln – für die Übereinstimmung Tickets kompatibel berücksichtigt werden müssen.
-   Regeln sollten – ein Match-Ticket, das eine Zuordnungsregel sitzungsauthentifizierungsmodul bevorzugt wird, die nicht der Fall ist.

In jedem dieser Kategorien gibt es mehrere spezifische Typen von Regeln. Weitere Informationen finden Sie Konfigurationsinformationen der Common Language Runtime-Vorgang in **SmartMatch Laufzeitvorgänge**.


### <a name="match-target-session"></a>Übereinstimmung Zielsitzung

Nach eine übereinstimmenden Gruppe gefunden wurde, wird der Dienst erstellt eine Match-Ziel-Sitzung, und stellen für alle Spieler der Ticket-Sitzungen, die miteinander verglichen werden reserviert.


## <a name="smartmatch-runtime-operations"></a>SmartMatch Laufzeitvorgängen



| Hinweis                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Informationen zur Verwendung von SmartMatch Vermittlung in Ihrem Titel finden Sie unter [mithilfe SmartMatch Vermittlung](using-smartmatch-matchmaking.md). |


### <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Erstellen eine Match-Ticket-Sitzung und ein Ticket für die Übereinstimmung

Ein Client, der als "Scout" der Vermittlung richtet eine Match-Ticket-Sitzung, um eine Gruppe von Spielern darzustellen, die Vermittlung eingeben möchten. Alle Spieler in dieser Gruppe beitreten zu der Sitzung mithilfe der MultiplayerSession.Join-Methode. Nachdem die Sitzung mit Spielern aufgefüllt ist, sendet der Titel die Sitzung auf dem Vermittlungsdienst mithilfe der MatchmakingService.CreateMatchTicketAsync-Methode, die eine Übereinstimmung Ticket erstellt, die der Sitzung darstellt.

| Hinweis                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der SmartMatch-Dienst aktualisiert das Feld "/servers/matchmaking/properties/system/status" in der Ticket-Sitzung auf "Suchen", während des Vorgangs CreateMatchTicketAsync. |

Die Antwort von der Erstellungsmethode für Übereinstimmung Ticket ist ein CreateMatchTicketResponse-Objekt. Diese Antwort enthält die Match-Ticket-ID, eine GUID, die verwendet werden kann, um Vermittlung abzubrechen, indem Sie das Ticket löschen. Darüber hinaus ist eine durchschnittliche Wartezeit für das Hopper in der Antwort und verwendet als Richtwert Player enthalten.

| Hinweis                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Es sei denn bestimmte Spiele Stat-Daten, die beibehalten werden müssen, empfiehlt es sich, dass Titel den PreserveSessionMode.Never Wert verwenden, wenn CreateMatchTicketAsync aufrufen. Verwenden PreserveSessionMode.Never ermöglicht schnellere Vermittlung und entfällt die Notwendigkeit, die Optionen der Benutzeroberfläche für das Hosten eines Spiels, die bis zur Suche nach einem Spiel im Gegensatz zu haben. |


### <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Die Sitzung und der Spieler festlegen Vermittlung Attribute

Beim Übermitteln der Ticket-Sitzungs, den Titel Sitzung festlegt und die Attribute "Player", verwendet der Vermittlungsdienst, um die Sitzung mit anderen Sitzungen zu gruppieren. Attribute können auf der Ebene Ticket oder auf der Ebene pro-Member angegeben werden.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Festlegen der Vermittlung Attribute auf der Ebene der Match-Ticket

Der Titel wird Attribute auf der Ebene des Match-Ticket in der TicketAttributesJson-Parameter der Methode MatchmakingService.CreateMatchTicketAsync übermittelt. Attribute, die auf der Ebene Ticket angegebene überschreiben die gleichen Attribute, die auf der Ebene pro-Member angegeben.


#### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Vermittlung-Attribute festlegen auf einzelne Mitglieder

Der Titel Gibt Attribute an einzelne Mitglieder für jedes Element in der Match-Ticket-Sitzung. Diese werden durch Aufrufen der MultiplayerSession.SetCurrentUserMemberCustomPropertyJson-Methode, die mit den Eigenschaftennamen des "MatchAttrs" festgelegt. Dieser Aufruf wird die Attribute in den /members/ {Index} / Eigenschaften/benutzerdefinierte/MatchAttrs Feld für jeden Spieler innerhalb der Sitzungs Ticket.

Der Prozess für die Vermittlung vereinfacht"" jedes einzelne Mitglieder Sie in ein einzelnes Ticket-Level-Attribut, auf der Grundlage der Flatten-Methode, die für dieses Attribut in die Xbox Live-Konfigurationsoberfläche für das Hopper angegeben. Diese Konfiguration auf [XDP](https://xdp.xboxlive.com) oder [Partner Center](https://partner.microsoft.com/dashboard).


### <a name="making-the-match"></a>Herstellen der Übereinstimmung

Mit der Ticket-Sitzung, und die Übereinstimmung Ticket einrichten, entspricht der Vermittlung-Dienst die dargestellten Ticket-Sitzung mit anderen Ticket-Sitzungen, die anderen Gruppen darstellt und erstellt oder identifiziert die Sitzung eine Match-Ziel. Der Dienst erstellt außerdem in der zielsitzung Reservierungen für die übereinstimmenden Spieler und markiert anschließend die Ticket-Sitzungen aus, wie abgeglichen. Anschließend benachrichtigt MPSD den Titel dieser Änderung in der Sitzung Ticket.

Jetzt muss der Titel der zielsitzung zu initialisieren, um sicherzustellen, dass genügend Player angezeigt, und führen Quality Service (QoS)-Überprüfungen, um sicherzustellen, dass sie erfolgreich eine Verbindung miteinander herstellen können. Wenn Initialisierung bzw. QoS fehlschlägt, markiert der Titel die Ticket-Sitzung auf Vermittlung erneut gesendet werden, damit eine andere Gruppe gefunden werden kann. Weitere Informationen zu diesem Prozess finden Sie unter **Sitzungsinitialisierung Ziel und QoS**.

Während der Match-Aktivität werden die folgenden Änderungen für die Sitzung in der JSON-Objekte erstellt:

-   /Servers/Matchmaking/Properties/System/Status Feld auf "gefunden".
-   /Servers/Matchmaking/Properties/System/targetSessionRef Feld festgelegt, Ziel-Sitzung.
-   /Members/ {Index} / Eigenschaften/benutzerdefinierte/MatchAttrs-Feld für jede Sitzung Ticket kopiert, auf die Member / {index} / Konstanten/benutzerdefinierte/MatchmakingResult/PlayerAttrs Feld.
-   Für jeden Spieler, ticket Attribute aus dem Feld TicketAttributes im Ticket Übereinstimmung in /members/ {Index} kopiert/Konstanten/benutzerdefinierte/MatchmakingResult/TicketAttrs Feld.


### <a name="maintaining-the-match-ticket"></a>Verwalten Sie das Ticket Übereinstimmung

Die Vermittlungsdienst wird eine Momentaufnahme der Ticket-Sitzung verwendet, die zum Zeitpunkt der Erstellung der Match-Ticket für die Sitzung. Wenn also alle Spieler teilzunehmen, oder lassen Sie die Ticket-Sitzung, muss der Titel verwenden die Vermittlungsdienst gelöscht und neu erstellt das Ticket Übereinstimmung.


### <a name="filling-spots-in-an-in-progress-game-session"></a>Füllen Plätzen zur Verfügung steht, in einer Spiele In Ausführung befindlichen-Sitzung

Ein Titel kann eine vorhandene Spiele Sitzung als Übereinstimmung Ticket Sitzung mehr Spieler, verknüpfen ein Spiel, das bereits ausgeführt wird, oder füllen eine Spiele Sitzung nach Abschluss einer und einige Links Player wiederverwenden. Dieser Prozess wird als "Abgleich" bezeichnet.

Eine Möglichkeit zum Ausführen Abgleich ist ein Ticket für die Übereinstimmung zu erstellen, die "die laufenden Sitzung durch den Aufruf beibehalten wird." **MatchmakingService.CreateMatchTicketAsync Methode** mit PreserveSession Festlegen des Parameters auf PreserveSessionMode.Always. Die Vermittlungsdienst wird sichergestellt, dass die vorhandene Sitzung, die während des Prozesses für die Vermittlung beibehalten und wird das resultierende zielsitzung.

Es gibt mögliche Nachteile dieser Muster, wie zwei Sitzungen mit Festlegung auf Always PreserveSession nicht werden können miteinander überein, da sie können nicht kombiniert werden. Dies kann zu sehr langsamen Abgleich Vermittlung führen. Um dieses Szenario zu vermeiden, sollten ein Titel PreserveSessionMode.Always nur verwenden, wenn die Beibehaltung des Spielzustands erforderlich ist. Otherwis PreserveSessionMode.Never festgelegt und die Beteiligten auf das neue TargetSession migriert werden soll, wenn eine Übereinstimmung gefunden wird.


### <a name="deleting-the-match-ticket"></a>Löschen das Ticket Übereinstimmung

So löschen Sie das Ticket Übereinstimmung den Titel ruft **MatchmakingService.DeleteMatchTicketAsync Methode**. Löschen des Tickets, das dem Vermittlungsdienst:

1.  Beendet die Vermittlung, für die Benutzer in der Sitzung Ticket.
2.  Updates "abgebrochen /servers/matchmaking/properties/system/status Felds in die Sitzung Ticket".


### <a name="matchmaking-for-games-using-xbox-live-compute"></a>Vermittlung für Spiele mithilfe von Xbox Live-Serverdienst

Im folgende Beispiel wird veranschaulicht, die auf hoher Ebene Vermittlung, die mithilfe von Live Compute Services. Ein ähnlicher Ansatz kann für Spiele, die auf Ressourcen von Drittanbietern gehosteten gelten.

1.  Der Client "Scout" erstellt eine Ticket-Sitzung, um der Gruppe darstellen. Diese Sitzung enthält eine Liste der möglichen Rechenzentren, in der Sitzungskonfiguration in /constants/system/measurementServerAddresses. Es geht entweder die Vorlage "Sitzung", wenn die Datacenter-Liste statisch ist, oder vom Client bei der sitzungserstellung nach Ihrem Erhalt von der Live-Serverdiensts. Diese Sitzung enthält auch GsiSetId GameVariantId und MaxAllowedPlayers Werte des TargetSessionConstants/benutzerdefinierte/GameServerPlatform-Objekts.
2.  Alle anderen Clients in der Gruppe an der Ticket-Sitzung teilnehmen.
3.  Alle Mitglieder der Gruppe, die die MeasurementServerAddresses Werte aus dem /constants/system-Objekt für die Ticket-Sitzung heruntergeladen wird. Jedes Element klicken Sie dann ein Ping an jede Adresse, die mit der API-Plattform und eine geordnete Liste der bevorzugten Rechenzentren auf die Sitzung hochladen, gemäß /members/ {Index} / Eigenschaften/System/ServerMeasurements.

| Hinweis                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der Titel kann festlegen und Abrufen von MeasurementServerAddresses Werte aus der Sitzung mit **MultiplayerSession.SetMeasurementServerAddresses Methode** und  **MultiplayerSessionConstants.MeasurementServerAddressesJson Eigenschaft**. |

4.  Die Clientaufrufe "Scout" **MatchmakingService.CreateMatchTicketAsync Methode**, und übergeben Sie einen Verweis auf die Ticket-Sitzung.

| Hinweis                                                                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wenn Sitzungsobjekte Ticket nicht übereinstimmende Konstanten verfügen, kann die CreateMatchTicket-Methode fehl. Dies kann durch Hinzufügen einer muss vermieden werden, um das Hopper-Regel und zu verhindern, dass die Übereinstimmung von Playern mit nicht übereinstimmenden Konstanten. |

5.  Wenn die **MatchTicketDetailsResponse.PreserveSession Eigenschaft** festgelegt ist nie, kopiert der Vermittlungsdienst Server Messungen aus jedem Member, in die interne Darstellung des Tickets, das vor dem Abflachen der erfasst in eine einzelne Sammlung für das Ticket und als Attribut für "spezielle" Ticket gespeichert.
6.  Wenn **MatchTicketDetailsResponse.PreserveSession Eigenschaft** nastaven NA hodnotu immer auf dem Server aus, die Messungen werden nicht verwendet. Stattdessen kopiert die Vermittlungsdienst /properties/system/matchmaking/serverConnectionString Wert für die Sitzung, in die interne Darstellung des Tickets, wie eine ServerMeasurements-Auflistung der Größe 1 mit einer Latenz von 0 (null).
7.  Die Vermittlungsdienst entspricht die Ticket-Sitzung mit anderen Benutzern, die andere Gruppen und nehmen den Server Messung von Sammlungen berücksichtigt darstellt. Wird versucht, die Gruppe mit anderen Gruppen übereinstimmen, die die gleichen Rechenzentren hoch bevorzugt.
8.  Nach eine übereinstimmenden Gruppe gefunden wurde, erstellt die Vermittlungsdienst oder identifiziert eine zielsitzung, und fügt alle Spieler der Ticket-Sitzungen, die miteinander verglichen werden. Der Dienst schreibt die endgültige vereinfachte Server-Messwerte für die passende Gruppe in /properties/system/serverConnectionStringCandidates. Klicken Sie dann schreibt er die ursprünglichen Server Messungen für jedes neue Element in der zielsitzung in /members/ {Index} / Konstanten/System/MatchmakingResult/ServerMeasurements.
9.  Alle Clients die Initialisierung für die zielsitzung, wie oben beschrieben ausführen. Jedoch, da die Clients Live-Server verbunden werden soll, führen sie keine QoS zum Bestätigen der Konnektivität aus.
10. Einige oder alle Clients Aufruf **GameServerPlatformService.AllocateClusterAsync Methode**. Erstens wird die Zuordnung, ausgelöst, während die anderen nur eine Bestätigung empfangen. Die Methode ruft die zielsitzung aus MPSD ab und wählt einen Dataceter basierend auf der Datacenter-Einstellungen in der zielsitzung und Last sowie andere Live Compute-spezifische Informationen zu kennen.
11. Alle Clients spielen.

## <a name="configuring-smartmatch-for-your-title"></a>Konfigurieren von SmartMatch für den Titel


### <a name="configuration-of-smartmatch-matchmaking-runtime-operations"></a>Konfiguration von Laufzeitvorgängen SmartMatch Vermittlung

Die gesamte Konfiguration SmartMatch Vermittlung erfolgt über die [Xbox-Entwickler-Portal (XDP)](https://xdp.xboxlive.com) oder [Partner Center](https://partner.microsoft.com/dashboard). Konfiguration verwendet die ServiceConfiguration -&gt;im Abschnitt Multiplayer-Spiele und Vermittlung eines Titels.


#### <a name="matchmaking-session-template-configuration"></a>Vorlage-Sitzungskonfiguration Vermittlung

Siehe [SmartMatch Vermittlung](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking), es gibt zwei Arten der Sitzung, die im Zusammenhang mit der Vermittlung, die Match-Ticket-Sitzung und der zielsitzung übereinstimmen. Im Grunde ist eine Ticket Sitzung die Eingabe für den Vermittlungsdienst, während die zielsitzung die Ausgabe ist. Wenn Sie Vorlagen für ereignissitzungen konfigurieren zu können, sollten Sie eine Vorlage für die einzelnen Sitzung erstellen.

Für eine Sitzung Ticket können Sie eine dedizierte Vorlage verwenden. Alternativ können Sie eine Vorlage für eine Lobby oder andere Sitzung nicht zur Verwendung für Spiele gedacht wiederverwenden.

| Wichtig                                                                                      |
|-------------------------------------------------------------------------------------------------------------|
| Die Ticket-Sitzung darf nicht von QoS-Überprüfungen aktiviert sein und muss nicht mit der Funktion "Gaming" markiert werden. |

Für eine zielsitzung müssen Sie eine Vorlage, die vorgesehen ist für Matchmade Spiels verwenden. Sie müssen Einstellungen, mit denen QoS-Überprüfungen zwischen Peers vor dem Start der Spiele und mit der Funktion "Gaming" markiert werden muss.

Mit die Benutzeroberfläche für XDP oder Partner Center-Konfiguration können Sie jede Sitzung eine oder mehrere Hoppers, jede mit Regeln zuordnen, die bestimmen, wie Sitzungen gemeinsam in diese Hopper abgeglichen werden. Weitere Informationen finden Sie grundlegende Hopper-Konfiguration für die Vermittlung.


#### <a name="basic-hopper-configuration-for-matchmaking"></a>Grundlegende Hopper-Konfiguration für die Vermittlung

Dieser Abschnitt definiert die Felder verwendet, um grundlegende Hopper Felder zu konfigurieren. Nach dieser Konfiguration müssen Sie die Hopper-Regeln konfigurieren, wie in der Konfiguration der Hopper Regeln beschrieben.

![](../../images/multiplayer/session_template_hopper_edit.png)


###### <a name="name"></a>Name

Der Name des der Hopper, die verwendet wird, wenn eine Sitzung mit Vermittlung zu übermitteln. Dieser Name muss als Parameter übergebenen Wert entsprechen den **CreateMatchTicketAsync** Methode während der Erstellung des Tickets übereinstimmen.


###### <a name="minmax-group-size"></a>Minimale/maximale Gruppengröße

Die minimale und maximale Größe für den Player-Gruppe, die von Sitzungen in der Hopper erstellt werden soll. Die Vermittlungsdienst versucht, eine übereinstimmende Gruppe zu erstellen, die so groß wie möglich, bis die maximale Gruppengröße ist. Allerdings wird eine Gruppe erstellt, wenn es genügend Spieler, um die Größe der minimalen Gruppe erfüllen zusammenstellen kann.


###### <a name="should-rule-expansion-cycles"></a>Erweiterung Zyklen sollten Regel werden.

Für eine sollte auszuschließen, versucht der Vermittlungsdienst, vergrößern Sie den Speicherplatz für die Suche und lockern die Regeln für die angegebene Vermittlung im Laufe der Zeit, wenn keine Übereinstimmung gefunden wird. Dieser Vorgang wird über mehrere Zyklen, wie die angegebene Verwendung das Feld sollte Regel Erweiterung Zyklen durchgeführt. Nach dem letzten Zyklus für die Erweiterung werden die Regeln sollten gelöscht, damit sie nicht mehr Tickets vom Abgleich verhindern. Allerdings sind diese weiterhin verwendet, um die beste Übereinstimmung zu ermitteln, wenn mehrere Tickets verfügbar sind. Nur die Anzahl und die QoS-Typen werden erweitert, bevor sie gelöscht werden. Siehe auch Hopper Konfigurationsregeln in diesem Thema.

Erhöhen des Werts sollte Regel Explansion Zyklen Einstellung Weitere Zyklen, damit bietet sollten Regel Erweiterung allerdings erhöht sich auch die Dauer der Vermittlung. Der Standardwert ist 3, dies ist in der Regel ausreichend, für die meisten Konfigurationen.

| Wichtig                                                                                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Erweiterung Zyklen treten in festen Intervallen von 5 Sekunden. Nach dem letzten Zyklus der Erweiterung werden alle Regeln mit "Sollte" nicht mehr für den Rest des Versuchs Vermittlung berücksichtigt. |

##### <a name="ranked-hopper"></a>Rang Hopper
Normalerweise SmartMatch blockierte Spieler verhindert, dass abgeglichen wird. Wenn Hopper Rangfolge aktiviert ist, wird diese Logik umgangen, um zu verhindern, dass die Spieler mit, dass dieses System um Akteure der größere Fähigkeiten zu vermeiden.

#### <a name="configuration-of-hopper-rules"></a>Konfiguration der Hopper-Regeln

Dieser Abschnitt definiert die Felder, die zum Konfigurieren von Regeln für eine Hopper verwendet.

###### <a name="common-rule-fields"></a>Allgemeine Felder

In diesem Abschnitt definierten Felder gelten für alle Hopper Regeln zur Verfügung.

**Regelname**

Der Anzeigename für die Regel zu Konfigurationszwecken angezeigt.

**Regeltyp**

Der Regeltyp. Optionen sind muss und sollte. Müssen die Regeln für die erfolgreiche Vermittlung. Können sollte Regeln gelockert und/oder entfernt, um eine Übereinstimmung gefunden werden. Weitere Informationen zu diesem Prozess finden Sie in der Konfiguration der sollten Regel Erweiterung.


**Datentyp**

Der Datentyp des Attributs der Vermittlung Regel. Mögliche Werte:   Anzahl. Geben Sie einen einfachen numerischen Wert für 32-Bit.
-   Eine Zeichenfolge. Geben Sie eine Unicode-Zeichenfolge von bis zu 128 Zeichen.
-   Auflistung. Geben Sie ein Array von Zeichenfolgen. Verwenden Sie diesen Wert, um herunterladbare Inhalte (DLC), Squad Mitgliedschaft oder Rolle-Einstellung für Spieler zu identifizieren.
-   Die Qualität des Diensts. Geben Sie einen benutzerdefinierten Datentyp zum Einschließen von Latenz QoS-Daten in der Vermittlung an. Nur eine solche Regel sollte pro Vermittlung Hopper verwendet werden.

| Hinweis |
|----------|
| Wenden Sie sich an Ihren Account Manager, Entwickler, wenn dieses Limit für den Titel problematisch ist. |

-   Gesamtwert. Geben Sie einen benutzerdefinierten Datentyp, der übermittelten Vermittlung Werte addiert. Sie können diesen Wert verwenden, um sicherzustellen, dass die resultierende Summe innerhalb eines bestimmten Bereichs oder einen genauen Wert.
-   Team. Geben Sie einen benutzerdefinierten Datentyp für die Teams, die der Spieler in der Vermittlung Anforderungen enthalten. Sie können diesen Wert verwenden, um zu vermeiden, Spieler innerhalb einer einzelnen Übereinstimmung Ticket zwischen mehreren Teams aufteilen.


###### <a name="data-type-specific-rule-fields"></a>Daten-spezifische Felder

Dieser Abschnitt definiert die Felder, mit denen Sie Regeln definieren, die in bestimmte Datentypen, aber nicht an andere Benutzer gelten. Die Benutzeroberfläche sollte in der Lage, um die Daten zu verdeutlichen, bestimmte Regeln gelten.

**Platzhalter**

Ein Wert, der angibt, ob das Attribut im Ticket Übereinstimmung ausgelassen werden kann. Wenn es ausgelassen wird, wird das Ticket kompatibel mit anderen Ticket, unabhängig vom Wert für dieses Attribut.


**Attributquelle**

Die Quelle des der Wert des Datentyps. Mögliche Quellen sind:
- Titel, die bereitgestellt werden. Der Wert wird in der Match-Ticket übermittelt.
-   Benutzer-Stat-Instanz. Der Wert wird automatisch vom Dienst UserStatistics abgerufen.


**Attributname**

Der Name der Quelle des Attributs. Es ist entweder den Namen der Eigenschaft im Ticket Übereinstimmung oder der Name des Benutzers Statistik.


**Standardwert**

Der Standardwert für den Datentyp, wenn kein Wert angegeben oder für die Vermittlung Anforderung verfügbar ist. Der Standardwert wird nicht angewendet, wenn das Feld "Platzhalter zulassen" aktiviert ist und kein Wert angegeben ist.


**Gewichtung**

Die Wichtigkeit der Regel. Die Gewichtung kann verwendet werden, um anzugeben, welche Regeln bei der Vermittlung und Regel Erweiterung priorisiert werden. Der Weight-Wert muss positiv sein und der Standardwert ist 1.


**Flatten-Methode**

Nur Anzahl Datentypen. Ein Wert, der angibt, wie mehrere Werte kombiniert werden, um eine Übereinstimmung zu erfüllen. Er bezieht sich auf mehrere Werte für verschiedene Spieler in einer einzelnen Übereinstimmung Ticket und über mehrere Tickets. Die möglichen Werte sind:
-  Min/Max. Verwenden Sie die minimalen oder maximalen Wert, der mehrere Werte aus verschiedenen Übereinstimmung Tickets.
-   Durchschnitt. Verwenden Sie die durchschnittlichen Wert von mehreren Werten aus verschiedenen Übereinstimmung Tickets.


**Max-Diff**

Nur Anzahl Datentypen. Die maximale numerische Abweichung zwischen den zwei verglichenen Werte, um eine Regel zu erfüllen. Für eine sollte auszuschließen, dieser Wert ist der Ausgangspunkt für die Regel-Erweiterung.


**Set-Vorgang**

Auflistung nur Datentypen. Der Vorgang, führen Sie auf die Übereinstimmung der Gruppe der Werte festlegen. Die möglichen Optionen sind:
- Schnittmenge. Übereinstimmung mit zwei Auflistungen, die basierend auf der Menge der Schnittmenge zwischen ihnen. Diese Einstellung führt in der für ähnliche oder identische Auflistungswerte.
-   Der Unterschied. Übereinstimmung mit zwei Auflistungen, die basierend auf der Menge der Unterschied zwischen ihnen.
-   Role-Einstellung. Übereinstimmung mit Sammlungen basierend auf den Einstellungen für die Rolle einen Player im Spiel Modi rollenbasierten.


**Ziel-Schnittmenge**

Teil der Konfiguration der Vorgang "festlegen". Die minimale Schnittmenge oder die maximale Differenz für zwei Auflistungen, bevor sie abgeglichen werden.


**Netzwerktopologie**

Die Qualität des Datentyps nur Dienst. Die Netzwerktopologie, die für QoS für verwendet wird. Mögliche Werte sind Peer-zu-Peer-Peer-zu-Host und Client/Server.


**Maximale maximale Wartezeit/Skalierung**  
Die Qualität des Datentyps nur Dienst. Die maximale Wartezeit für erfolgreiche Vermittlung innerhalb der angegebenen Netzwerktopologie. Dieser Wert wird behandelt, als einen Skalierungswert (im Gegensatz zu einer erforderlichen Latenz) mit einer Client / Server Quality of Service, wenn Regel sollte.


| Hinweis                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Darüber hinaus werden die Standardregeln für die Zuverlässigkeit auch auf eine Hopper angewendet. Diese Regeln können nicht entfernt werden und werden verwendet, um sicherzustellen, dass die ordnungsgemäße Verarbeitung der Zuverlässigkeit bei der Vermittlung. |


**Warten auf Rollen zulassen**

Sammlungsdaten für die Einstellungen der Rolle geben nur. Gibt an, wenn der Dienst für die Übereinstimmung das Ticket für die Vermittlung enthält, um alle verfügbaren Rollen zu füllen.


###### <a name="expansion-delta"></a>Expansion Delta

Der Wert, der angibt, wie viel klicken, um die übermittelten Regel für jede Erweiterung-Generierung zu lockern. Das Delta der Erweiterung wird zusätzlich zu den Wert für die maximale Differenz angewendet. Einzelheiten finden Sie in Beispiel 1 (Regel Expansion).

Sie können auch das Delta der Erweiterung verwenden, um mehrere numerische Werte mit verschiedenen Geschwindigkeiten zu erweitern. Dies ist nicht möglich, über die Erweiterung Zyklus Konfigurationseinstellung gilt für alle Regeln. Stattdessen ist der Ansatz decimal Erweiterung Werte, z. B. 0,4 verwenden. Eine Erweiterung tritt nur auf, wenn eine neue ganze Zahl erreicht wird, auch für die gleiche Anzahl von Zyklen für Erweiterung für eine andere Erweiterung Geschwindigkeit, wodurch.


###### <a name="qos-expansion-peer-to-peer-peer-to-host"></a>QoS-Erweiterung (Peer-zu-Peer, Peer-zu-Host)
Für die Qualität des Diensts typerweiterung für Peer-Spiele kann nicht das Delta Erweiterung konfiguriert werden. Stattdessen sollten Sie eine der folgenden Strategien für die Erweiterung verwenden.

<table>

<tr>

<tr>
<td>
<b>1. MaxLatency < 256.</b><p/><p/>

Erweiterung erfolgt am MaxLatency * Erweiterung Zyklus.<p/>
Z. B. wenn der anfängliche Wert 200 ist, wird in der ersten 200 verwendet Durchläufe ausführt, 400 im zweiten Zyklus, und so weiter.
</td>
</tr>

<tr>
<td>
<b>2. MaxLatency > oder gleich 256</b><p/><p/>

Erweiterung Skalierung linear von 50 auf MaxLatency - 256 verwendet werden.<p/>
Beispielsweise ist der Anfangswert 556, Skalieren des Werts linear von 50 zu 300 über die Anzahl der Zyklen. D. h. wenn sechs Zyklen ausgewählt wurden, wäre klicken Sie dann die Werte 50, 100, 150, 200, 250 und 300. Wenn fünf Zyklen ausgewählt wurden, wäre die Werte 50, 112,5, 175, 237.5 und 300.
</td>
</tr>

</tr>
</table>

##### <a name="qos-expansion-client-server"></a>QoS-Erweiterung (Client-Server)
Wenn Sie dedizierte Server verwenden, basiert die Erweiterung auf die relative Rangfolge. Nur die meisten bevorzugt, dass der Server in einem frühen Zeitpunkt Erweiterung Zyklen betrachtet werden. Im Laufe der Zeit werden andere, weniger bevorzugte Server verwendet werden. Um entsprechende Erweiterung müssen, wir einen ähnlichen Wert wie MaxLatency, maximale Skalierung bezeichnet. Diese maximale Skalierung sollte immer noch auf die größten zulässigen Ping-Zeit festgelegt werden — dieser Wert gibt jedoch einen relativen Maßstab wiedergegeben, für die anderen Server Ping-Zeiten, die von einem Player, im Gegensatz zu der Bereitstellung einer zwingende Voraussetzung für Pings bereitgestellt. Beachten Sie, dass Sie Server mit nicht akzeptablen Pings absichtlich ausschließen können, indem entfernen sie aus der Liste in der Anforderung.

#### <a name="example-1-rule-expansion"></a>Beispiel 1 (Regel-Erweiterung)

Spielerebene wird für die Vermittlung verwendet, und Player abgeglichen werden lose, basierend auf der Nähe ihrer Ebenen. Spieler mit den geringsten Unterschied zwischen ihrer Ebenen werden bevorzugt.

-   Player-Level-Regel
-   Regeltyp: SOLLTEN
-   Datentyp: Number
-   Max Diff: 1
-   Expansion Delta: 2
-   Flatten Sie-Methode: Durchschnitt

Standardmäßig ist der erforderliche Unterschied zwischen den Ebenen der Player kleiner oder gleich 1. Wenn innerhalb dieser Differenz Übereinstimmung gefunden wird, werden die Spieler abgeglichen. Wenn keine erste Übereinstimmung gefunden wird, wird der Player-Level-Wert 2 für jede Iteration (standardmäßig alle drei Iterationen) erweitert. Dieses Szenario führt eine Vermittlung Verhalten für einen Player auf 20, wie in der folgenden Tabelle gezeigt.

| Schritt                    | Wert für potenzielle Kandidaten für Übereinstimmung | Effektive Ebene Entfernung für Übereinstimmung |
|-----|
| Anfängliche übermittelte Wert | 19-21                                      | 1                                             |
| Erweiterung Zyklus 1       | 17-23                                      | 3                                             |
| Erweiterung Zyklus 2       | 15-25                                      | 5                                             |
| Erweiterung Zyklus 3       | 13-27                                      | 7                                             |

Die Erweiterung Zyklen fortfahren zu können, erhöht sich die effektive Ebene Entfernung für einen erfolgreichen Abgleich ohne Änderung des Werts Max. Differenz. Es wird nur der Ebenenwert Player gelockert.


#### <a name="example-2-collection-rule"></a>Beispiel 2 (Sammlungsregel)

Das Spiel gibt drei Arten von DLC, die für Spieler verfügbar sind. Mit dieser Regel Vermittlung wird auf "Nur DLC" Spiels Vermittlung angewendet, und ein Player sollte mindestens ein DLC als Matchmade mit anderen Spielern besitzen.

-   Player-DLC-Regel
-   Regeltyp: MUSS
-   Datentyp: Sammlung
-   Set-Vorgang: Schnittmenge
-   Ziel-Schnittmenge: 1

Spieler bewerten ihre DLCs und senden die Werte in der folgenden Tabelle in ihren Fahrkarten Übereinstimmung.

| Hinweis                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| In der folgenden Tabelle gibt der Auflistungswert der Besitz der DLC an. Es wird auf 1 festgelegt, wenn DLC für den Spieler und auf 0 ist dies nicht der vorliegen. |

| Player   | Wert der Auflistung | Übereinstimmung mit Spielern | Hinweis           |
|----------|------------------|----------------------|----------------|
| Player-1 | { "1", "1", "1"} | 3                    | Alle DLCs besitzt  |
| Player-2 | { "0", "0", "0"} | Keine                 | Besitzt keine DLC    |
| Player-3 | { "1", "0", "0"} | 1                    | Erste DLC besitzt |

Wenn die Schnittmenge Ziel im Beispiel auf 2 festgelegt ist, werden die Spieler 1 und 3 nicht zugeordnet werden, da die Schnittmenge zwischen ihnen nur 1 ist.

#### <a name="example-3-avoid-previous-players"></a>Beispiel 3 (Sie sollten vorherige Players)
Der Titel wird bevorzugt, dass ein Spiel mit dem Player vermeiden zuletzt wiedergegeben.
* Regeltyp: MUSS
* Datentyp: Sammlung
* Set-Vorgang: Unterschied
* Ziel-Schnittmenge: 0

## <a name="defining-team-rules-during-smartmatch-configuration"></a>Team-Regeln definieren, der während der SmartMatch-Konfiguration

### <a name="configuring-team-rules"></a>Konfigurieren von Team-Regeln

Informationen zum Einrichten der Team-Regel erstellen Sie zunächst eine für Ihre gewählte Konfiguration-Plattform (XDP oder Partner Center). Füllen Sie die teamgrößen, die Ihr Spiel geht davon aus, erstellen Sie anhand der Tickets in dieser Hopper übereinstimmen. Z. B. Wenn Ihr Spiel 4v4 erwartet, sollten Sie zwei Einträge erstellen erwartet eine Maximalgröße von 4 und einen anderen Namen. Es sind mindestens Teamgröße auch – verwenden Sie diese, wenn ein Spiel mit weniger Spieler in einem Team wiedergegeben werden kann. Andernfalls sollte die Mindest- und Höchstwerte der gleiche Wert sein.


#### <a name="using-team-rules"></a>Mithilfe von Team-Regeln

Sobald die Team-Regel konfiguriert ist, werden die Tickets in das Hopper von übereinstimmenden verhindert werden, wenn keine Möglichkeit besteht, ihre Gruppen in Teams anpassen, ohne eine Teilung verursacht. Die Regel wird der daraus resultierende Reservierung Team auf die zielsitzung unter Member/Konstanten/benutzerdefinierte/Matchmakingresult/InitialTeam geschrieben. Beachten Sie, dass dies einfach eine vorgeschlagene Zuordnung – der Titel ist möglicherweise durch das Neuanordnen der Spieler es bessere Spiele zu entwickeln, hervorrufen kann, während Sie weiterhin verhindert, dass Tickets in verschiedenen Teams aufgeteilt werden.

Wenn Sie ein Ticket für ein Spiel erstellt wird, die ausgeführt wird, wird der Team-Regel zusätzliche Informationen erforderlich. Nehmen wir an, z. B., dass 8 Spieler in einem 4v4 Spiels, wenn zwei Spieler Dropdown oder getrennt sind. Der Titel soll so füllen Sie diese leere Plätzen zur Verfügung steht, aber es kann nicht die Teams Umgestalten, während das Spiel gespielt wird.

Versucht, füllen Sie Spiele in Bearbeitung werden von Tickets mit der PreserveSession Feld auf "immer" dargestellt. In solchen Fällen da Teams Spieler, bereits zugewiesen wurden der Titel muss angeben, die aktuelle Team-Zuordnung, damit Übereinstimmung wissen, wie viele Leerstellen auf jedes Team geöffnet. Um die Namen der Teams geben ist jeden Spieler, jeden Spieler die Teamnamen in der game-Sitzung unter Member/me/Eigenschaften/System/Gruppen schreibt. Dieses Feld ist ein JArray.

Nachdem die oben aufgeführten Eigenschaften für die Spiele-Sitzung geschrieben werden, erstellt ein einziger Player ein Ticket für die Sitzung, in beim Suchen von mehr Spieler an. Wenn das Ticket erfüllt wird, wird Übereinstimmung erneut schreiben das vorgeschlagene alle Spieler, die join-Team in Member/Konstanten/benutzerdefinierte/Matchmakingresult/initialTeam


###### <a name="preferring-even-teams"></a>Es auch teams

Darüber hinaus werden Übereinstimmungen mit den größten Teams zuerst gestellt. Dies bedeutet, dass in einer hypothetischen 4v4 Hopper, Tickets von 4 Spieler abgeglichen werden zuerst, bis sich keine Tickets von 4 befinden. Tickets 3 fortgesetzt, und klicken Sie dann Singletons abrufen, wie sie möchten, und so weiter. Auf diese Weise werden Tickets ähnliche Größe in der Regel miteinander, wiedergegeben, wenn diese vorhanden und nicht verhindert hat, dass durch andere Regeln. Beachten Sie, dass dadurch die relativ starke Team regelrangfolge über andere Regeln. Nehmen wir beispielsweise an, dass Sie eine begrenzte Auffüllung, bestehend aus ein Ticket Größe 4 mit hoher skill(A), ein Ticket Größe 4 mit niedriger skill(B) und vier Tickets Größe 1 mit hoher skill(C-F) verfügen. Die Regel-Team führt dazu, dass abzugleichen, um es vorziehen, A und B mit übereinstimmt, im Gegensatz zu A, C, D, E und F.


###### <a name="should-variant"></a>Variant sollten

Muss der Regel wird verhindert, dass Tickets, die in allen Generationen aufteilen und ermöglicht die Sortierung der Prefer-sogar-Teams. Das sollte Regel ist identisch, bis die letzte Generierung – sobald das, Tickets aufgeteilt werden können, auch wenn die Sortierung der Prefer-sogar-Teams noch aktiv sind.

## <a name="target-session-initialization-and-qos"></a>Ziel-Sitzungsinitialisierung und QoS

Eine Gruppe von Spielern ergibt sich eine Übereinstimmung in eine zielsitzung SmartMatch Vermittlung, wie in beschrieben [SmartMatch Vermittlung](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking). Der Titel muss Schritte zu bestätigen, dass genügend Playern hinzugefügt haben, dass sie erfolgreich Verbindung miteinander herstellen können, wenn dies erforderlich. Dieser Prozess wird als Ziel sitzungsinitialisierung bezeichnet.

Für Spiele, die mithilfe von Peer-zu-Peer-Topologien werden ist ein wichtiger Aspekt der Ziel-sitzungsinitialisierung QoS-Messung und Bewertung. Zugehörigen Vorgänge sind das Maß an Latenz und Bandbreite zwischen einer Xbox-Konsolen (oder zwischen-Konsolen und -Servern) und die Auswertung der resultierenden Messungen bereit, um festzustellen, ob die Netzwerkverbindung zwischen Knoten in Ordnung ist.

Das folgende Flussdiagramm veranschaulicht, wie Sie die Initialisierung der zielsitzung und QoS-Vorgänge zu implementieren.

![](../../images/multiplayer/Multiplayer_2015_Matchmaking_QoS.png)

### <a name="managed-initialization"></a>Verwaltete Initialisierung

MPSD unterstützt eine Funktion namens "verwaltete Initialisierung" über die Initialisierung den Zielprozess-Sitzung über die Clients in einer Sitzung beteiligten koordiniert. MPSD automatisch verfolgt die Initialisierung-Phasen und die zugehörigen Timeouts für die Sitzung, und wertet die Konnektivität zwischen Clients, bei Bedarf. Verwaltete Initialisierung wird dargestellt, durch die **MultiplayerManagedInitialization Klasse**.

| Hinweis                                                                                                                  |
|------------------------------------------------------------------------------------------------------------------------------------|
| Es wird für den Titel SmartMatch Vermittlung verwenden, um die übernehmen dringend empfohlen, dass die Vorteile der MPSD Initialisierung Feature verwaltet. |


#### <a name="managed-initialization-episodes-and-stages"></a>Verwaltete Initialisierung folgen und Phasen

Eine zielsitzung ausgeführt wird, verwaltete Initialisierung jederzeit Vermittlung neue Spieler zur Sitzung hinzufügt. SmartMatch fügt Sitzung Member als Benutzerstatus reserviert, was bedeutet, dass jedes Element einen Slot verbraucht, aber die Sitzung wurde noch nicht zusammengeführt. Jede Gruppe von neuen Spieler löst eine neue Episode der Initialisierung.

Nach Abschluss der Initialisierung des Spielers entweder erfolgreich oder schlägt der Vorgang fehl. Ein Spieler, die Initialisierung erfolgreich kann mithilfe der zielsitzung wiedergeben. Ein Spieler, der ein Fehler auftritt, muss auf Vermittlung, die in einer anderen Sitzung abgeglichen werden erneut übermittelt werden. Für Fälle, in denen eine Sitzung ist an Vermittlung, mit, der *PreserveSession* Parametersatz immer, die bereits vorhandene Member der Sitzung ist keiner Initialisierung unterzogen, wie MPSD wird angenommen, sie ordnungsgemäß eingerichtet werden.

Jede Episode verwaltete Initialisierung besteht aus folgenden Stufen:

-   Verknüpfen von – Sitzung Mitglieder selbst schreiben, mit der Sitzung, deren Benutzerzustand aus reserviert aktiv zu verschieben und grundlegende Daten, z. B. sichere Geräteadresse hochladen.
-   Messung--für die Peer-Topologien Sitzung Mitglieder QoS miteinander zu messen und Hochladen der Ergebnisse in der Sitzung.
-   Auswertung--MPSD wertet die Ergebnisse der letzten beiden Phasen aus, und bestimmt, ob die Sitzung und/oder Elemente wurde erfolgreich initialisiert wurde.

Der Titel Code funktioniert in der Sitzung, um jeden Benutzer (und daher die Sitzung) über die Phasen verknüpfen und Messung zu gelangen. Klicken Sie dann entweder Starten der Wiedergabe oder wechseln Sie zurück zum Vermittlung, nachdem die auswertungsstufe erfolgreich war oder fehlgeschlagen ist.


### <a name="configuring-the-target-session-for-initialization"></a>Konfigurieren der Zielsitzung für die Initialisierung

Der Titel kann verwaltete Initialisierungsprozess Verwenden von Konstanten in der zielsitzung initialisierte konfigurieren. Diese Konstanten werden unter /constants/system in der sitzungsvorlage mit der Version 107, die empfohlene Vorlagenversion festgelegt. Es können zwei Arten von Konfigurationseinstellungen vorgenommen werden: Einstellungen, die durch die verwaltete Initialisierung wird als Ganzes zu konfigurieren und Einstellungen, die QoS-Anforderungen zu konfigurieren. Finden Sie unter [Vorlagen für Ereignissitzungen MPSD](multiplayer-session-directory.md) Beispiele für Vorlagen für ereignissitzungen für allgemeine Szenarien, Titel.

| Hinweis                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------|
| Die Messung Phase während der Initialisierung wird übersprungen, wenn QoS-Anforderungen nicht in der Ziel-Initialisierung Sitzungskonfiguration definiert sind. |


#### <a name="configuring-managed-initialization-as-a-whole"></a>Konfigurieren verwalteter-Initialisierung als Ganzes

Im folgenden sind die Felder aus, um verwaltete Initialisierung insgesamt steuern. Sie sind Teil des /constants/system/memberInitialization-Objekts:
- JoinTimeout. Gibt an, wie lange wird MPSD für jedes Element zu verknüpfen, nachdem die Folge der Initialisierung begonnen hat, wartet. Standardwert ist 10 Sekunden.
-   measurementTimeout. Gibt an, wie lange MPSD wartet für jedes Element QoS Maßeinheiten Ergebnisse, hochladen, nachdem die Messung-Stufe gestartet wurde. Standardwert ist 30000 Sekunden.
-   membersNeededToStart. Gibt die Anzahl der Elemente, die bei der Initialisierung für die ersten Initialisierung Folge erfolgreich ausgeführt werden kann, erfolgreich ausgeführt werden muss. Standardwert ist 1.

| Hinweis                                              |
|----------------------------------------------------------------|
| Wenn dieser Schwellenwert nicht erfüllt wird, nicht alle Mitglieder Initialisierung werden. |


#### <a name="configuring-qos-requirements"></a>Konfigurieren von QoS-Anforderungen

QoS ist nur während der Initialisierung erforderlich, wenn der Titel eine Peer-zu-Peer- oder Peer-zu-Host-Topologie verwendet. Jede Topologie wird mit einer Topologie-spezifische Konstante unter /constants/System/zugeordnet werden.
###### <a name="configuring-qos-requirements-for-peer-to-peer-topology"></a>Konfigurieren von QoS-Anforderungen für die Peer-zu-Peer-Topologie

| Hinweis                                                                                                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Es ist selten für Titel, um Einstellungen für QoS-Anforderungen für die Peer-zu-Peer-Topologie zu machen. Diese Einstellungen sind stark einschränkend und zu Problemen führen, für die Spieler mit strengen Netzwerkadressübersetzungen (NAT). |

Peer-zu-Peer-Topologie-QOS-Anforderungen werden in das PeerToPeerRequirements-Objekt festgelegt. Jeder Client muss für jeden anderen Client eine Verbindung herstellen können. Das Objekt hat die folgenden relevanten Felder:
-   LatencyMaximum. Gibt an, der maximalen Latenz zwischen zwei Clients.
-   bandwidthMinimum. Gibt die minimale Bandbreite zwischen zwei Clients.


###### <a name="configuring-qos-requirements-for-peer-to-host-topology"></a>Konfigurieren von QoS-Anforderungen für die Peer-zu-Host-Topologie

Peer-zu-Host-Topologie-QOS-Anforderungen werden in das PeerToHostRequirements-Objekt festgelegt. Jeder Client muss mit einem gemeinsamen Host herstellen. Wenn dieses Objekt konfiguriert und die Initialisierung erfolgreich ist, erstellt MPSD ursprüngliche Liste von Clients, die potenzielle Hosts, bekannt als die "Kandidaten des Hosts." Hier sind die Felder festlegen:
-   LatencyMaximum. Gibt an, der maximalen Latenz zwischen jedem Peer und dem Host.
-   bandwidthDownMinimum. Gibt die Mindestbandbreite downstream zwischen jedem Peer und dem Host an.
-   bandwidthUpMinimum. Gibt die Mindestbandbreite upstream zwischen jedem Peer und dem Host an.
-   hostSelectionMetric. Gibt an, die Metrik verwendet, um den Host auswählen.

## <a name="see-also"></a>Siehe auch

[Vorlagen für Ereignissitzungen MPSD](multiplayer-session-directory.md)

[SmartMatch Laufzeitvorgängen](/windows/uwp/xbox-live/multiplayer/multiplayer-appendix/smartmatch-matchmaking)
