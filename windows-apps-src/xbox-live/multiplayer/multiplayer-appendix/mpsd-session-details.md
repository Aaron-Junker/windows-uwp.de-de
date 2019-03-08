---
title: Multiplayer-Sitzungsdetails
description: Informationen Sie zu den Details der Xbox Live Multiplayer-Sitzungen.
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine Multiplayer-2015 "Sitzung", Mpsd
ms.localizationpriority: medium
ms.openlocfilehash: 3eb92af2d478fa41150f375dd19ef347d2b6f2e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616085"
---
# <a name="mpsd-session-details"></a>MPSD-Sitzungsdetails

Dieser Artikel enthält folgende Abschnitte:
* Übersicht über leistungssitzungen
* Sitzung-Funktionen
* Sitzungsgröße
* Benutzer-Sitzungsstatus
* Sichtbarkeit und Joinability
* Sitzungstimeouts
* Mehrere angemeldeter Benutzer auf eine einzige Konsole
* Lebenszyklusverwaltung für Prozesse
* Die Bereinigung der inaktiven Sitzungen
* Sitzung Arbiter

## <a name="session-overview"></a>Übersicht über leistungssitzungen

Eine Sitzung Multiplayer-Sitzung-Verzeichnis (MPSD) wird verfügt über einen Sitzungsnamen und als eine Instanz einer Vorlage Sitzung, in dem ein JSON-Dokument mit Standardeinstellungen für die Sitzung handelt. Die Vorlage ist Teil einer Dienstkonfiguration mit einer Dienst-Konfiguration-ID (SCID), die eine GUID ist. Mit dieser Vorlage befinden sich [Xbox-Entwickler-Portal (XDP)](https://xdp.xboxlive.com) und [Partner Center](https://partner.microsoft.com/dashboard). Dienstkonfigurationen sind die Entwickler zugänglichen Ressourcen für die Erfassung, Management und Security-Richtlinie verwendet. Wenn eine Sitzung über MPSD zugegriffen wird, ist principal Autorisierung für die Dienstkonfiguration entsprechend die festlegen, indem der Entwickler über XDP oder Partner Center-Richtlinien ausgeführt. Sekundäre zugriffsprüfungen, z. B. Überprüfen der Mitgliedschaft Sitzung, werden auf Sitzungsebene ausgeführt, wenn die Sitzung geladen wird, nachdem der Zugriff auf die Dienstkonfiguration autorisiert ist.

In diesem Thema wird davon ausgegangen, dass Ihre Vorlage vertragsversion 107 verwendet, dies ist die Version, die von der aktuellen MPSD für Xbox One verwendet wird. Wenn Sie Vorlagen, basierend auf vertragsversion 105 (identisch mit 104) definiert haben, müssen Sie diese zur Unterstützung von Version 107 ändern. Anweisungen hierzu finden Sie unter [allgemeine Multiplayer-2015-Migrationsprobleme](common-issues-when-adapting-multiplayer.md).

| Wichtig                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Funktionen, die über eine Vorlage festgelegt ist kann nicht durch Schreibvorgänge in die MPSD geändert werden. Um die Werte ändern zu können, müssen Sie erstellen und übermitteln eine neue Vorlage mit den erforderlichen Änderungen. Alle Elemente, die nicht über eine Vorlage festgelegt werden können, MPSD durch Schreibvorgänge geändert werden. |

### <a name="session-reference"></a>Sitzung-Referenz

Jede Sitzung MPSD eindeutig verweist auf eine Sitzung-Referenz, dargestellt in der Multiplayer-WinRT-API durch die **MultiplayerSessionReference Klasse**. Der Verweis für die Sitzung enthält diese Zeichenfolgenwerte:

-   Konfiguration Dienstbezeichner (SCID)
-   Sitzungsname-Vorlage
-   Sitzungsname

Der Verweis für die Sitzung ordnet den URI zum Identifizieren von Sitzungen, wie unten dargestellt. In der folgenden Zuordnung Beispiel ist "Authority" sessiondirectory.xboxlive.com.

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>Elemente einer Sitzung

Jede Sitzung enthält Gruppen von Elementen, die erzwingen, Veränderlichkeit und Sicherheitsregeln, die von Session-Element, zusammen mit schreibgeschützten organisatorische Informationen (Metadaten) zu unterscheiden. In diesem Abschnitt wird beschrieben, die Gruppen der Sitzungselemente enthalten, in der JSON-Dateien so konfigurieren Sie die Sitzung, und klicken Sie in der JSON-Datei für die Vorlage, die Sie auswählen.

| Hinweis                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wenn Sie WinRT-Wrapper für eine HTTP/REST-Implementierung verwenden, müssen Ihre Sitzung und die Vorlage JSON-Objekte definieren, die genau die WinRT-Funktionen widerspiegeln. |

In jedem der Elementgruppen gibt es zwei interne Objekte:

-   Systemobjekte: Diese Objekte haben, ein festes Schema, das vom MPSD interpretiert und erzwungen wird. Sie werden überprüft und zusammengeführt. Da MPSD definiert und weiß, was sie bedeuten, kann es bei ihnen fungieren. Finden Sie die vollständige Definition der einzelnen Systemobjekte, die Verweise für die **MultiplayerSession Klasse** und die Verweise für die **Sitzung Directory URIs**.
-   Benutzerdefinierte Objekte – diese Objekte sind optional und haben kein Schema. Sie werden verwendet, um Metadaten bezüglich eines Multiplayer-Spiels zu speichern. Da MPSD diese Daten nicht interpretieren kann, ist es nicht einbezogen. Spieledaten oder gespeicherten Informationen sollten im Titel verwalteter Speicher (TMS) gespeichert werden. Weitere Informationen zu TMS finden Sie unter **Xbox Live Titel Storage**.

Hier ist ein Beispiel eines benutzerdefinierten JSON-Objekts:
```JSON
    "custom": {
      "myField1": true,
      "myField2": "string",
      "myField3": 5.5,
      "myField4": { "myObject": null },
      "myField5": [ "my", "array" ]
    }
```

#### <a name="session-constants"></a>Sitzungskonstanten

Sitzungskonstanten werden nur zum Zeitpunkt der Erstellung, durch den Ersteller des oder der Vorlage für die Sitzung festgelegt. Das /constants/system-Objekt dient zum Definieren von Konstanten für das Multiplayer-System, wie es über die MPSD bekannt ist. Der WinRT-Wrapper, der diesem Objekt zugeordnet wird dargestellt, durch die **MultiplayerSessionConstants Klasse**.

Das /constants/system-Objekt kann eine Anzahl von Elementen, z. B. eine Capabilities-Objekt, ein Objekt für die Metriken, eine ManagedInitialization (Vertrag Vorlagenversion 104/105) oder MemberInitialization (vertragsversion 107)-Objekt, ein PeerToPeerRequirements definieren. Objekt, ein PeerToHostRequirements-Objekt und ein MeasurementsServerAddresses-Objekt.


#### <a name="session-properties"></a>Eigenschaften von leistungssitzungen

Verwenden Sie das /properties/system-Objekt, um Eigenschaften von leistungssitzungen für MPSD zu definieren. Der WinRT-Wrapper, der diesem Objekt zugeordnet ist. die **MultiplayerSessionProperties Klasse**. Eigenschaften von leistungssitzungen sind jederzeit von der Sitzung Elemente geschrieben. Beispiele für Eigenschaften von leistungssitzungen im JSON-Format sind: JoinRestriction, InitializationSucceeded, und die Vermittlung-Objekt. Ein Beispiel für die Verwendung von diese Elementgruppe, finden Sie unter [Sitzungsinitialisierung Ziel und QoS](smartmatch-matchmaking.md).


#### <a name="member-constants"></a>Memberkonstanten

Legen Sie das Element Konstanten beim Beitritt für jedes Mitglied einer Sitzung. Das JSON-Objekt ist /members/ {Index} / Konstanten/System. Ist die WinRT-Wrapperklasse, die eine Sitzung-Element darstellt. die **MultiplayerSessionMember Klasse**.


## <a name="member-properties"></a>Elementeigenschaften

Elementeigenschaften sind nur von einer Sitzung-Element geschrieben. Sie in das /members/ {Index} / Eigenschaften/System-Objekt festgelegt werden, und die Elemente entsprechend dem **MultiplayerSessionMember Klasse**. Hier ist ein Beispiel:

```
    {
      // These flags control the member status and "activeTitle", and are mutually exclusive (it's an error to set both to true).
      // For each, false is the same as not present. The default status is "inactive", i.e. neither present.
      "ready": true,
      "active": false,

      // Base-64 blob, or not present. Empty string is the same as not present.
      "secureDeviceAddress": "ryY=",

      // During member initialization, if any members in the list fail, this member will also fail.
      // Can't be set on large sessions.
      "initializationGroup": [ 5 ],

      // List of the groups I'm in and the encounters I just had.
      // An encounter is a brief interaction with a group. When an encounter is reported, it counts as retroactively joining the group 30 seconds ago and just now leaving.
      // Group names use the session name validation rules (e.g. case-insensitive).
      // On large sessions, groups are used to report who played with who (rather than just session membership). Members
      // that are active in at least one group together at the same time are counted as playing together.
      // Empty lists are the same as no value specified.
      // The set of encounters is a point-in-time property, so it's immediately consumed and will never appear on a response.
      "groups": [ "team-buzz", "posse.99" ],
      "encounters": [ "CoffeeShop-757093D8-E41F-49D0-BB13-17A49B20C6B9" ],

      // Optional list of role preferences the player has specified for role-based game modes.
      // All role names have to match across all members in the session. Role weights are
      // defined from 0-100.
      "RolePreference": { "medic": 75, "sniper": 25, "assault": 50, "support": 100 },

      // QoS measurements by lower-case device token.
      // Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
      // Metrics can be omitted if they weren't successfully measured, i.e. the peer is unreachable.
      // If a "measurements" object is set, it can't contain an entry for the member's own address.
      "measurements": {
        "e69c43a8": {
          "bandwidthDown": 19342,  // Kilobits per second
          "bandwidthUp": 944,  // Kilobits per second
          "custom": { }
        }

      // QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
      // If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
      "serverMeasurements": {
        "server farm a": {
          "latency": 233  // Milliseconds
        }
      },

      // Subscriptions for shoulder taps on session changes. The "profile" indicates which session changes to tap as well as other properties of the registration like the min time between taps.
      // The subscription is named with a title-generated GUID that is also sent back with the tap as a context ID.
      // Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
      // To remove a subscription, set its context ID to null.
      // (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
      // Can't be set on large sessions.
      "subscriptions": {
        "961dc162-3a8c-4982-b58b-0347ed086bc9": {
          "profile": "party",  // Or "matchmaking", "initialization", "roster", "queuehost", or "queue"
          "onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
        },
        "709fef70-4638-4b94-905b-24cb02706eb5": null
      }
    }
```

#### <a name="server-elements"></a>Serverelemente

Server sind nicht--Benutzer, die verknüpft oder in eine Sitzung eingeladen wurden. Die zugehörigen JSON-Objekte sind/Server / {Servername} / Konstanten/und/Server / {Servername} / Eigenschaften /. Diese Objekte werden nur von Servern geschrieben.

| Hinweis                                                         |
|---------------------------------------------------------------------------|
| Die/Server / {Servername} / Konstanten/Systemobjekt wird derzeit nicht verwendet. |


### <a name="session-configuration"></a>Sitzungskonfiguration

Es gibt zwei Möglichkeiten, die Konfiguration von Sitzungen steuern:

-   Verwenden Sie Vorlagen für ereignissitzungen erfasst, die über XDP oder Partner Center.
-   Verwenden Sie die Aufrufe der Multiplayer-Spiele und Vermittlung WinRT-APIs oder REST-APIs. Sie müssen immer noch eine Vorlage verwenden, aber die Vorlage muss nicht die Werte enthalten, die Sie konfigurieren möchten. Beachten Sie, dass es sich bei Ihrem Titel die Konstanten, die bereits in der Vorlage festgelegten kann nicht überschrieben werden.

Ein separates JSON-Dokument wird bereitgestellt, um die Sitzung selbst zu definieren. Darüber hinaus muss der Entwickler alle WinRT-Wrapper-Funktionen für einen bestimmten Titel erforderlich implementieren. Der Inhalt der JSON-Dokumente und WinRT-Wrappercode wiedergeben müssen genau miteinander, und müssen die neueste Version der Vorlage Vertrag entsprechen.

Das Schema für eine Sitzung ist mit versionsverwaltung durch das mit der Sitzung-Version (Hauptversion) und die Protokoll-Revision (Nebenversion). Die Versionen werden kombiniert, in den Header X-Xbl-Contract-Version als "100 \* Haupt- und Nebenversion". Ein Titel Version 1.7 enthält beispielsweise die folgende Kopfzeile auf jeder REST-Anforderung, wenn die neueste Version der Vorlage Vertrag der 107: X-Xbl-Contract-Version: 107.

| Hinweis                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| Es wird für die meisten Titel (mit XSAPI) empfohlen, vertragsversion 105 und Sitzung Vorlagenversion 107 verwendet. |


### <a name="session-templates"></a>Vorlagen für ereignissitzungen

Jede sitzungsvorlage ist ein JSON-Dokument, die Teil der Dienstkonfiguration, die definiert, des Frameworks für die Sitzung erstellt wird, und stellt Konstanten bereit, für die neue Sitzung. Weitere Informationen finden Sie unter [Vorlagen für Ereignissitzungen MPSD](../service-configuration/session-templates.md).

## <a name="session-capabilities"></a>Sitzung-Funktionen

Funktionen sind die Konstanten in der Sitzung MPSD, die Verhalten konfigurieren, die die MPSD dieser Sitzung angewendet werden sollen. Können Sie am häufigsten XDP und Partner Center Funktionen in der sitzungsvorlage festgelegt. Sie sind im /constants/system/capabilities-Objekt festgelegt. Wenn keine Funktionen erforderlich sind, verwenden Sie eine leere Capabilities-Objekt.

| Hinweis                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| Titel fast nie ändern oder Sitzung-Funktionen, die mit der Multiplayer-WinRT-API oder die Vermittlung WinRT-API zugreifen. |

Sitzung Funktionen werden durch dargestellt die **MultiplayerSessionCapabilities Klasse**. Sie sind boolesche Werte, die angeben, was die Sitzung unterstützen kann:

-   Verbindung
-   Spiele
-   Größe
-   Verbindung ist erforderlich für aktive Mitglieder

Die **MultiplayerSessionConstants Klasse** definiert die folgenden Eigenschaften, die Sitzung Funktionen betreffen:

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| Hinweis                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| Wenn der Titel eine dynamischen Sitzung-Funktion definiert ist, wird die entsprechende Eigenschaft festgelegt, um für die Sitzungskonstanten "true". |

## <a name="session-size"></a>Sitzungsgröße

Die Größe einer Sitzung MPSD richtet sich nach der Anzahl der Elemente in dieser Sitzung.


### <a name="maximum-session-size"></a>Maximale Sitzungsgröße

Die maximale Größe einer Sitzung ist die maximale Anzahl der Sitzung-Elemente, die sie aufnehmen kann. Es wird dargestellt, durch die **MultiplayerSessionConstants.MaxMembersInSession Eigenschaft**. Die maximale Member ist das /constants/system-Objekt festgelegt werden.

Die maximale Sitzungsgröße zwischen 1 und 100 Sitzung Elementen ist; der Standardwert ist 100 betragen, wenn nicht festgelegt werden, bei der Erstellung. Wenn die erforderliche Größe mehr als 100 ist, wird die Sitzung wird eine "Groß" Sitzung aufgerufen und auf besondere Weise festgelegt ist.

Festlegen einer maximalen Größe für eine Sitzung kann einen geöffneten Slot angezeigt werden, wie voll während bestimmter Szenarien trennen. Z. B. wenn ein Player durch ein Netzwerk oder Stromausfall getrennt wird, wird die Verzögerung nicht sofort in der Sitzung wiedergegeben. Der Member ist festgelegt auf inaktiv, mit der Disconnect Erkennung-Funktion, die in beschriebenen [MPSD Änderung Benachrichtigung verarbeiten und Trennen von Erkennung](multiplayer-session-directory.md).

Im Gegensatz dazu einem Peermesh, die einen Takt verwendet, um eine Trennung zu erkennen häufig berücksichtigt eine Trennung der Verbindung innerhalb von zwei bis drei Sekunden und den Player-Slot direkt öffnen kann. Der Arbiter kann jedoch nicht andere Mitglieder entfernen.

### <a name="large-sessions"></a>Große Sitzungen

Eine große MPSD Sitzung kann bis zu 1000 Elemente besitzen, aber es bietet einige sitzungsfeatures deaktiviert werden, z. B. zum Abrufen einer Liste aller Member. Sitzung Largeness wird dargestellt, durch die **MultiplayerSessionCapabilities.Large Eigenschaft**. Diese Eigenschaft wird festgelegt, auf "true", um eine große Sitzung anzugeben, und die Funktion "große" wird angegeben, in dem /constants/system/capabilities-Objekt. 

## <a name="session-user-states"></a>Benutzer-Sitzungsstatus



MPSD definiert einen Benutzerzustand, der Status eines Benutzers zu einer Sitzung hinzugefügt wurde. Möglichen Zustände definieren, indem die **MultiplayerSessionStatus Enumeration**. Der Benutzer wird auch betrachtet, um den Status "verfügbar" zu erhalten, bevor Sie eine Sitzung hinzugefügt.

Die **MultiplayerSession.SetCurrentUserStatus Methode** können verwendet werden, um den Sitzungszustand für den Benutzer zu ändern. Diese Änderung kann für den REST von Einstellung /members/ {Index} / Eigenschaften/System ordnungsgemäß in das Spiel Sitzung JSON-Dokument vorgenommen werden.


### <a name="reserved-user-state"></a>Reservierte Benutzerzustand

Der Benutzer befindet sich im den reservierten Benutzerzustand, der Arbiter Fill einem geöffneten Steckplätze innerhalb der Sitzung ausgewählt wurde. In diesem Fall der Benutzer hat noch nicht offiziell akzeptiert die Einladung der Sitzung oder verknüpft die Sitzung, um eine Verbindung herstellen, mit Kollegen zu starten.


### <a name="active-user-state"></a>Aktiver Benutzer Zustand

Wenn ein Benutzer im Zustand aktiv ist, wurde der Titel die Sitzung hinzugefügt, im Auftrag des Benutzers, und der Benutzer nimmt in der Sitzung aktiv. Der Benutzer wird in diesem Zustand fortgesetzt, solange er das Spiel wiedergegeben wird.

Wenn Sie ein Titel zum ersten Mal gestartet wird, sollte ermitteln, ob der Benutzer bereits Mitglied aller Sitzungen, in der Regel wird durch Überprüfen des Sitzungszustands überprüft. Wenn der Benutzer eine Sitzung angehört, kann der Titel springen direkt in das Spiel und Festlegen von lokalen beteiligten Mitglieder Benutzerzustands aktiv.

Ein Benutzer sollte im aktiven Zustand bleiben, während Sie in der Sitzung wiedergeben. Wenn ein Benutzer die Sitzung über die in-Game-Benutzeroberfläche verlässt, muss der Benutzer entfernt werden, von der Sitzung mit dem **MultiplayerSession.Leave Methode**. Wenn der Benutzer nur vorübergehend sofort aus das Spiel, als wenn der Titel eingeschränkt wird, der Titel des Benutzers im aktiven Zustand für einen angemessenen Zeitraum berücksichtigen. Es empfiehlt sich des Benutzerstatus auf inaktiv zu ändern, wenn der Benutzer nicht nach einer Zeitperiode Titel angegeben zurückgegeben wurde.


### <a name="inactive-user-state"></a>Benutzer inaktiv

In den inaktiven Status wird der Benutzer derzeit nicht mit dem Spiel aktiviert wird, aber immer noch einen gespeicherte Slot in der Sitzung. Das heißt, ist der Benutzer "nicht aktiv" angezeigt.

Es ist die des Benutzers-Konsole, die Verantwortung für diese Benutzer zu Benutzer-Status Inaktiv festlegen, in der Sitzung hat. Diese Aktion kann nicht von der Arbiter ausgeführt werden. Beispielszenarien, in denen ein Benutzer in den inaktiven Status versetzt wird:

-   Der Titel empfängt ein Ereignis für das Anhalten.
-   Der Benutzer nicht aktiv war (keine Antwort Eingabe- oder Controller) für einen Zeitraum definiert, durch den Titel. Es wird zwei Minuten für eine wettbewerbsfähige Multiplayer-Spiele empfohlen.
-   Der Titel wurde im eingeschränkten Modus für mehr als zwei Minuten und für einen bestimmten Zeitraum definiert, durch den Titel. Dieses Zeitlimit eingeschränkten Modus ist die erwartete Zeitspanne, die für die ein Benutzer von den Titel, die über eine zugehörige app oder anderen Benutzererlebnis in Bezug auf den Titel sein kann.
-   Der Benutzer wurde nicht ordnungsgemäß aus der Sitzung getrennt. Finden Sie unter [MPSD Benachrichtigungsverarbeitung ändern, und Trennen von Erkennung](multiplayer-session-directory.md).

Wenn der Titel wird gestartet, und des Benutzerstatus für eine bestimmte Sitzung-Member auf inaktiv festgelegt ist, der Titel wurde angehalten, oder der Benutzer hat zu lange in der Sitzung inaktiv wurde. Da der Titel erneut gestartet wurde wird, ist der Hinweis darauf, dass der Benutzer zum Fortsetzen der game-Sitzungs zu der möchte er gehört. Wenn der Status eines Benutzers aktiv ist. beim Start der Titel, diese Situation ist wahrscheinlich aufgrund einer Trennung der Netzwerkverbindung oder ein anderes Szenario, bei denen war der Titel kann nicht den Benutzer auf inaktiv festgelegt werden, bevor er unterbrochen wird. In beiden Fällen sollte Ihre Titel versuchen, die der Benutzer das Spiel und anderen Benutzern, um die Wiedergabe fortgesetzt, erneut eine Verbindung herzustellen oder entfernen Sie den Benutzer aus der Sitzung.

### <a name="user-state-when-the-session-is-over"></a>Wird der Status bei der die Benutzersitzung über

Wenn die Sitzung abgeschlossen ist, wird Spiele nicht mehr unterstützt. Der Titel muss ermöglicht allen Benutzern das Entfernen selbst mithilfe der **MultiplayerSession.Leave Methode**. Die Sitzungsaktivitäten von Benutzern zugeordnet werden automatisch gelöscht, wenn sie die Sitzung verlassen.

## <a name="visibility-and-joinability"></a>Sichtbarkeit und Joinability

Zugriff auf eine Sitzung wird durch zwei Einstellungen auf der Ebene MPSD gesteuert: Sitzung Sichtbarkeit und Sitzung Joinability. Die Sichtbarkeit und Joinability Empfehlungen in diesem Thema gelten für die häufigsten Szenarien für den Titel. Titel sollte diesen Einstellungen folgen, wenn möglich, und sie sollten in der Title-Logik verwenden, um stellen die letzte, autoritative Feststellung, gibt an, ob ein neues Players Gruppe, in eine Sitzung aufgenommen wird.


### <a name="session-visibility"></a>Sichtbarkeit der Sitzung

Sichtbarkeit der Sitzung wird durch eine Konstante dargestellt, die zum Erstellen der Sitzung festgelegt wird. Es ist in der Regel in der sitzungsvorlage definiert, und bestimmt, welche Arten von Benutzern gelesen haben und Schreibzugriff auf eine Sitzung. Die möglichen Werte für die Sichtbarkeit der Sitzung werden definiert, durch die **MultiplayerSessionVisibility Enumeration**. Die Einstellungen, die für die Sichtbarkeit-Konstante, eine JSON-Datei zulässig sind, öffnen, sichtbare und private.


#### <a name="recommended-game-session-visibility-open"></a>Sichtbarkeit des Spiels Sitzung empfohlen: Öffnen

Offene game-Sitzungen erfordern keine Player-Reservierungen, die des einladungsprozesses vereinfacht. Nachdem eine Einladung gesendet wurde, aber nur Spuren Spieler lokal eingeladen haben, wird der Arbiter nicht Spieler in MPSD reserviert. Daher können Spieler direkt an der Arbiter und bestimmen, ob sie einer Sitzung beitreten soll, zurückgewiesen werden, oder warten soll, (wenn es sich um eine warten-Player unterstützt werden). Der Arbiter ist die endgültige Instanz reagiert und weist das neue Element zu behalten, oder lassen die Sitzung.

Die Verwendung von Spiele-Sitzung öffnen Sichtbarkeit erfordert den eingeladenen Player starten Sie einen Titel ein, und Verbinden mit dem Arbiter, bevor die endgültige Entscheidung vorgenommen wurde. Es ist akzeptabel, eine Fehlermeldung angezeigt, die dem Benutzer angezeigt, wenn eine Sitzung voll ist oder wenn eine INVITE-Nachricht zurückgewiesen wurde.

Um eine Verbindung mit dem Arbiter herzustellen, ist eine sicheres Gerät-Adresse erforderlich. Die **MultiplayerSessionProperties.HostDeviceToken Eigenschaft** wird verwendet, um herauszufinden, welche Sitzung-Element ist der aktuelle Arbiter einer Sitzung und die Adresse des Geräts ein eingeladener Player, für die Verbindung verwenden sollten sichern.

### <a name="session-joinability"></a>Sitzung Joinability

Sitzung Joinability bestimmt, welche Arten von Benutzern eine Sitzung beitreten können. Er kann dynamisch während einer Sitzung festgelegt werden. Die möglichen Werte für die Sitzung Joinability sind:

-   Keine (Standard) – Es gibt keine Einschränkungen auf, die der Sitzung teilnehmen kann.
-   Lokal, nur lokale Benutzer können die Sitzung beitreten.
-   Folgen – nur lokale Benutzer und Benutzer, die von anderen Membern Sitzung eingehalten werden können die Sitzung ohne Reservierung verknüpfen.

Der Arbiter einer Sitzung kann eine private Sitzung durch die Einstellung für Joinability erstellen. Joinability entweder lokal oder danach schränkt den Zugriff auf die Sitzung und macht sie privat.

Darüber hinaus die Arbiter sollten verfolgen Sitzung der, die Joinability auf der Hostebene, falls erforderlich, damit ältere Sitzung Einladungen zurückgewiesen werden kann. Z. B. ob jeder eingeladene Spieler nicht, zum Beitreten zu einer Sitzung angekommen sind, bis die Sitzung bereits voll ist, kann der Arbiter verknüpfte Player anweisen, dass die Sitzung wurde gesperrt, und sie die Sitzung automatisch zu verlassen müssen.

## <a name="session-timeouts"></a>Sitzungstimeouts

Sitzungen können von Timern und andere externe Ereignisse geändert werden. Sitzungstimeouts definieren die Zeiträume für die Sitzung Mitglieder in bestimmten Zuständen bleiben können, bevor sie automatisch inaktiv oder aus der Sitzung entfernt werden. MPSD unterstützt auch die Timeouts zum Verwalten der Sitzungslebensdauer.

| Hinweis                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Leerlauftimeout-Einstellungen werden in /constants/system/timeouts oder in die verwaltete Initialisierung-Objekt, um den Vertrag Vorlagenversion 104/105 vorgenommen. Für Version 107 oder höher können die Einstellungen einzeln im/Konstanten/System oder innerhalb des Objekts für die verwaltete Initialisierung erfolgen. |

Wenn ein Timer abläuft, wird MPSD nicht automatisch aktualisieren die Sitzung und der Arbiter auf einmal über Änderungen benachrichtigt. Die Sitzung und Timeout Zustände werden nur dann aktualisiert unmittelbar vor dem Lesen oder schreiben, dass die Anforderung gesendet wird. Sofortiges Update wird sichergestellt, dass die zurückgegebenen Daten die am häufigsten auf dem neuesten Stand ist.

| Hinweis                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| Sitzungstimeouts werden nicht gestapelt, und nur eine für einen Statusübergang für jedes Element der Sitzung auf dem Update angewendet wird. |


### <a name="currently-defined-timeouts"></a>Aktuell definierten Timeouts

In diesem Abschnitt wird beschrieben, die Timeouts, die derzeit von MPSD definiert sind. Alle Timeouts werden in Millisekunden angegeben. Der Wert 0 ist zulässig, und einen sofortigen Timeout angibt. Ein Timeout ohne Wert wird als unendlich betrachtet. Da die Timeouts über Standardwerte verfügen, sollten Sie explizit für ein unendliches Timeout null angeben.
#### <a name="evaluationtimeout"></a>evaluationTimeout

Dieses Timeout gibt die Zeitdauer für ein Element der Sitzung vornehmen und die Entscheidung für die Auswertung hochladen an. Wenn keine Entscheidung empfangen wird, zählt die Entscheidung, als Fehler. Dieses Timeout wird in die verwaltete Initialisierung-Objekt eingefügt.


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

Dieses Timeout wird für ein Element für die Sitzung festgelegt, die einer Sitzung beigetreten ist, aber derzeit nicht im Spiel aktiviert wird. Das Element wird aus der Sitzung nach 2 Stunden, wird standardmäßig entfernt.

| Hinweis                                                                      |
|----------------------------------------------------------------------------------------|
| Dieses Timeout wird das inaktive-Timeout für die Vorlage vertragsversion 104/105 festgelegt. |

In vielen Fällen empfiehlt das inaktive-Timeout auf 0, sodass jeder Benutzer, die in den inaktiven Zustand festgelegt ist, aus der Sitzung und dem entsprechenden Steckplatz zu löschenden sofort entfernt werden soll. Dieses Verhalten ist wünschenswert, dass die meisten wettbewerbsfähig Multiplayer-Spiele, damit Sie, wenn ein Benutzer inaktiv geschaltet oder einen inaktiven Zustand erreicht, ein neues Players schnell hinzugefügt werden kann. Für Co-op- oder andere Multiplayer-Entwürfe sollten Sie den Titel, um zuzulassen, dass Benutzer mehr Zeit, eine Verbindung herzustellen, der Titel nicht beteiligt sind, für einen Zeitraum oder getrennt werden. Beachten Sie, dass keine einfache Lösung, die alle Entwurfsszenarien geeignet ist.

#### <a name="jointimeout"></a>joinTimeout

Dieses Timeout gibt an, die Anzahl der Millisekunden, die ein Benutzer der Sitzung beizutreten. Reservierungen von Benutzern, die nicht hinzugefügt werden, werden entfernt. Dieses Timeout wird in die verwaltete Initialisierung-Objekt eingefügt.


#### <a name="measurementtimeout"></a>measurementTimeout

Dieses Timeout gibt die Zeitspanne, die eine Sitzung-Element zum Hochladen von Messungen wurde an. Ein Member, die ein Fehler, zum Hochladen von Messungen auftritt ist mit einem Grund für Fehler bei der "Timeout" markiert. Dieses Timeout wird in die verwaltete Initialisierung-Objekt eingefügt.

| Hinweis                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bei der Vermittlung wird ein 45-Sekunden-Timeout für die QoS-Messungen erzwungen. Aus diesem Grund empfehlen wir die Verwendung eines Timeouts von Messungen, die kleiner als oder gleich 30 Sekunden bei der Vermittlung ist. |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

Dieses Timeout wird für ein Element für die Sitzung festgelegt, hat die Sitzung verknüpft, und versucht, in das Spiel zu erhalten. Dies bedeutet normalerweise, dass die Shell den Benutzer, für den Titel hinzugefügt wurde und der Titel wird gestartet. Der Member aus der Sitzung entfernt und in den inaktiven Zustand nach 3 Minuten standardmäßig platziert.

| Hinweis                                                          |
|----------------------------------------------------------------------------|
| Dieses Timeout wird das bereit Timeout für die vertragsversion 104/105 festgelegt. |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

Dieses Timeout wird für ein Element für die Sitzung festgelegt, die für die Sitzung von einem anderen Benutzer hinzugefügt wurde, aber die Sitzung wurde noch nicht zusammengeführt. Die Reservierung wird gelöscht, und das Element wird als inaktiv betrachtet, wenn das Timeout abläuft. Der Standardwert ist 30 Sekunden.

| Hinweis                                                             |
|-------------------------------------------------------------------------------|
| Dieses Timeout wird der reservierte Timeout für die vertragsversion 104/105 festgelegt. |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

Dieses Timeout gibt die Anzahl der Millisekunden an, nachdem eine Sitzung leer ist, wenn er gelöscht wird. Der Standardwert ist 0.

| Hinweis                                                                 |
|-----------------------------------------------------------------------------------|
| Dieses Timeout wird das SessionEmpty-Timeout für die vertragsversion 104/105 festgelegt. |


### <a name="session-timeout-example"></a>Session Timeout-Beispiel

1.  Eine Sitzung wird mit vier Spielern gestartet.
2.  Zwei Spieler, A und B sind aufgrund eines Fehlers Power getrennt. Deren Status im Spiel bleibt aktiv.
3.  Die anderen zwei Spieler, C und D, beenden Sie ordnungsgemäß mit der **MultiplayerSession.Leave Methode**.
4.  Die Sitzung bleibt geöffnet, mit Spielern A und B nicht verbunden, aber immer noch im aktiven Zustand.
5.  Einige Tage später Player ein zurückgegeben und das Spiel startet.
6.  Spielern A überprüft, Sitzungen, die ein Mitglied ist (einen Lesevorgang ausführt) und sucht nach der verwaiste Sitzung vor einigen Tagen.
7.  Die Sitzung führt eine Präsenz-Überprüfung für die zwei Spieler, die immer noch in der Sitzung, (A und B) sind.
    1.  Da Player ein Titel ausgeführt wird, die Anwesenheit-Prüfung für Player ein erfolgreich ist und des Spielers, die in der Übereinstimmung Zustand aktiv bleibt.
    2.  Player-B wird den Titel nicht ausgeführt. Daher legt die Prüfung auf Vorhandensein Player B fehl, und der Dienst die Player-B-Status auf inaktiv fest. Beginnt an diesem Punkt das inaktive-Timeout für Player B.

8.  Player ein beendet die Sitzung ordnungsgemäß mit der **lassen** Methode.
9.  Das inaktive-Timeout läuft ab, für den Player B, die aus der Sitzung auf die nächste Lese- oder Schreibvorgang von jedem Benutzer entfernt wird.
10. Die Sitzung verfügt jetzt über 0 (null) Elemente und aus dem Dienst entfernt wird.

Wenn das inaktive-Timeout für die Sitzung auf 0, B der Player aufgrund der Zeitüberschreitung sofort nach der Überprüfung des Vorhandenseins festgelegt ist in Schritt 7a und wird wahrscheinlich durch das Schreiben der Sitzung entfernt. In diesem Fall wird die Sitzung wird geschlossen, ohne die Notwendigkeit eines zusätzlichen Lesen aus oder Schreiben in die Sitzung.


## <a name="multiple-signed-in-users-on-a-single-console"></a>Mehrere angemeldeter Benutzer auf eine einzige Konsole


Wenn mehrere Benutzer sind in der gleichen Konsole angemeldet ist möglich, dass einige Benutzer in einer Spiele-Sitzung sind, während andere Benutzer nicht in der Sitzung sind noch nicht in den aktuellen Titel aktiv sind. Spiele-Einladungen können auch empfangen und für mehrere Benutzer, wirkt sich auf der game-Sitzung Mitgliedschaft akzeptiert werden. Ein Titel muss diese Punkte, um alle Szenarios der Sitzung Mitgliedschaft ordnungsgemäß verarbeiten zu können.

In ein häufiges Szenario ein neues Player meldet sich an und wird aktiv in's Spiel und muss mit einer vorhandenen Spiels Sitzung hinzugefügt werden. Als sollte mit dem Erstellen einer neuen Sitzungs spielen, ein Titel nur einen Benutzer hinzufügen, wenn es während des Spiels geeignet ist.

Mit mehreren angemeldeten Benutzern können einen oder mehrere Benutzer auch Einladungen mit einer anderen Spiele Sitzung erhalten. Titel müssen nicht für solche Szenarien entworfen, in keiner bestimmten Weise. Sitzungsereignisse Status und die Member benachrichtigen Sie den Titel der Updates, Mitgliedschaft in der game-Sitzung und Benutzer.

Um mehrere angemeldeten Benutzern für einer onlinesitzung zu behandeln, abonniert der Titel für die Schulter für alle Benutzer, die über ein separates tippt **XboxLiveContext Klasse** Objekt für jeden Benutzer. Verwendet der Titel der **MultiplayerSession.ChangeNumber Eigenschaft** ermitteln Sie bestimmte Änderungen in der Sitzung, und ignorieren von doppelten Schulter tippt.


## <a name="process-lifecycle-management"></a>Lebenszyklusverwaltung für Prozesse


Genau wie einen Titel nicht Multiplayer-Spiele kann ein Titel in einer Sitzung Multiplayer-Titel anhalten und Beenden von prozesslebenszyklusereignisse auftreten. Die Sitzung des Arbiters sollten in regelmäßigen Abständen aus diesem Grund Sitzungszustand speichern. Für den Fall, dass der Arbiter angehalten wird, sollten die Spielzustands nach Bedarf, damit der Arbiter ein neuer Sitzungszustand wiederherstellen kann der Titel versuchen Arbiter-Migration und speichern. Es ist möglich für eine vollständige Multiplayer-Sitzung angehalten und später fortgesetzt werden, solange die Sitzung MPSD weiterhin gültig ist. Nur eines angegebenen Peers in der Regel der Hostcomputer des Spiels, sollten die globalen Spielzustands aktualisieren.


### <a name="storage-of-game-metadata"></a>Spiele-Metadaten

Ein Titel speichert Spiel Metadaten in der Sitzung MPSD. Spiele-Metadaten wird die erforderlichen Informationen zum Anzeigen der Sitzungsdaten, und aktivieren den Titel zu finden und verknüpfen die Spiele-Sitzung. Der Titel, die Player-spezifische Metadaten im Abschnitt über benutzerdefinierte Eigenschaften für die Sitzung Member, z. B. Farbe des Players, bevorzugten Player abgespielt Waffen für die Sitzung usw. gespeichert wird. Sitzungsweiten-Metadaten, z. B. aktuelle Zuordnung, werden im Bereich globale benutzerdefinierte Eigenschaften der MPSD Sitzung gespeichert.


### <a name="storage-of-game-state"></a>Speicherung von Spiel-Status

Spielzustands befindet sich in der Title-verwalteter Speicher (TMS), mit der **Titel Speicherdienst**. Storage mithilfe von diesem Speicherort können einen Titel ein, der Arbiter bedenkenlos Berechtigung zu migrieren. Finden Sie unter [Migration einen Arbiter](migrating-an-arbiter.md).

| Hinweis                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| Der Titel sollten nicht versuchen, um Spielzustands in TMS häufiger als einmal alle 5 Minuten, zu speichern, es sei denn, sie gesperrt wird. |

## <a name="cleanup-of-inactive-sessions"></a>Die Bereinigung der inaktiven Sitzungen

Wenn die SessionEmptyTimeout auf 0 festgelegt ist, wird eine Sitzung MPSD automatisch gelöscht, wenn der letzte Player die Sitzung verlässt. Gewusst wie: verhindern, dass eine nicht verwendete Sitzungen mit Spieler nach dem Absturz herstellen oder trennen finden Sie unter [MPSD Änderung Benachrichtigung verarbeiten und Trennen von Erkennung.](multiplayer-session-directory.md). Nicht ordnungsgemäße Behandlung von nicht verwendeten Sitzungen nach Absturz oder trennen kann zu Problemen führen, wenn ein Titel Sitzungen für einen Player abgefragt wird.

Wird empfohlen, um inaktive Sitzungen zu bereinigen, damit die Title-Abfrage alle Sitzungen für einen bestimmten Benutzer durch Aufrufen der **MultiplayerService.GetSessionsAsync Methode** und anschließende Evaluierung der Sitzungen. Wenn es eine veraltete Sitzung erkennt, ruft der Titel der **MultiplayerSession.Leave Methode** für alle lokalen Spieler in der Sitzung. Dieser Aufruf löscht schließlich die Anzahl der Elemente auf 0 und die Sitzungen bereinigt.

## <a name="session-arbiter"></a>Sitzung Arbiter


Einige Multiplayer-Methoden sollten nur von einem Client innerhalb einer Spiele-Sitzung aufgerufen werden. Dieser Client ist eine der Konsolen der Sitzung teilnimmt, wird aufgerufen, der Arbiter oder den Host. Wenn mindestens eine Sitzung in einem Spiel angehört, müssen die Sitzung einen Arbiter Joins in Bearbeitung zu überwachen.


### <a name="setting-the-arbiter"></a>Festlegen der Arbiter

Wenn es sich um eine Sitzung erstellt, legt der Client einer einzigen Konsole als der Arbiter. Finden Sie unter [Vorgehensweise: Legen Sie einen Arbiter für eine Sitzung MPSD](multiplayer-how-tos.md).


### <a name="saving-session-state"></a>Speichern des Sitzungsstatus

Siehe **Lebenszyklusverwaltung für Prozesse**, sollte der Arbiter Sitzungsstatus in regelmäßigen Abständen speichern. Der Arbiter ein neuer muss den Titel des Sitzungszustands im Fall von Arbiter-Migration wiederherstellen können. Weitere Informationen finden Sie unter [Migration einen Arbiter](multiplayer-how-tos.md).


### <a name="managing-game-session-members-and-joins-in-progress"></a>Verwalten von Spielen Sitzung Mitglieder und Verknüpfungen in Bearbeitung

Die wichtigste Rolle von der Sitzung Arbiter ist zum Verwalten von Benutzern, die in die Sitzung Spiele spielen. Dies schließt die Verarbeitung von game Einladungen, die Benachrichtigung warten Spieler und Arbeiten mit dem Spieler das Spiel beendet.


#### <a name="receiving-notifications"></a>Empfangen von Benachrichtigungen

Der Arbiter neue Spieler das Spiel mit beitreten möchte überwachen muss die **RealTimeActivityService.MultiplayerSessionChanged Ereignis**.


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>Suchen von Spieler, um leere Spiel Sitzung Slots füllen

Der Arbiter findet Spieler, um leere Spiel Sitzung Slots von einem dieser Vorgänge zu füllen:   Wenn der Titel einer Lobby-Sitzung oder einen anderen Mechanismus verwendet, um die verzögerte Verknüpfungen können, finden Sie neue Sitzung-Elemente, die diesen Mechanismus zu verwenden.
-   Erstellen Sie eine andere Übereinstimmung Ticket-Sitzung.

Siehe auch [Vorgehensweise: Geben Sie bei der Vermittlung geöffnete Sitzung Slots](multiplayer-how-tos.md).


#### <a name="handling-invited-session-members"></a>Behandlung von eingeladen Sitzung Mitglieder

Der Arbiter muss eingeladenen Sitzung Elemente zu überwachen und ein minimale Intervall zwischen Einladungen auf einen einzelnen Benutzer angewendet. Siehe auch [Vorgehensweise: Spiele-Einladungen senden](multiplayer-how-tos.md).
