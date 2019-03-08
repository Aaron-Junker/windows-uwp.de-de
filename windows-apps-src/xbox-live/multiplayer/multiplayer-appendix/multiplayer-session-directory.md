---
title: Multiplayer-Sitzungsverzeichnis
description: Erfahren Sie, bis das Xbox Live Multiplayer-Sitzungsverzeichnis (MPSD).
ms.assetid: a9b029ea-4cc1-485a-8253-e1c74184f35e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Mpsd, Multiplayer-Sitzungsverzeichnis.
ms.localizationpriority: medium
ms.openlocfilehash: 1681fe59533ebaf0db197efb95ca46b828846f5d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662545"
---
# <a name="multiplayer-session-directory-mpsd"></a>Multiplayer Session Directory (MPSD)

Dieser Artikel enthält folgende Abschnitte:

* Übersicht über die MPSD
* MPSD Benachrichtigungsverarbeitung ändern, und Trennen von Erkennung
* MPSD-Handles für Sitzungen
* Synchronisierung von Aktualisierungen der Sitzung
* Aufrufen von MPSD
* Vorlagen für Ereignissitzungen MPSD
* Multiplayer-Sitzung-Explorer

## <a name="session-overview"></a>Übersicht über leistungssitzungen

### <a name="what-is-mpsd"></a>Was ist MPSD?

Multiplayer-Sitzungsverzeichnis (MPSD) ist ein Dienst ausgeführt wird, in der Xbox Live-Cloud, die des Spiels Multiplayer-Dateisystem-Metadaten über mehrere Clients zu zentralisieren. Es wird von umschlossen der **MultiplayerService Klasse**.

MPSD kann Titel für die wichtigsten Informationen für eine Gruppe von Benutzern die Verbindung freigeben. Dadurch wird sichergestellt, dass die Funktionalität der Sitzung synchronisiert und konsistent ist. Er koordiniert sich mit dem Betriebssystem-Shell und Konsole Einladungen senden/übernehmen und über die Spieler Karte verknüpft wird.


### <a name="mpsd-sessions"></a>MPSD-Sitzungen

Eine Sitzung MPSD wird dargestellt, durch die **MultiplayerSession Klasse** wie im Szenario, in denen eine oder mehrere Personen ein Spiels verwenden. Eine Sitzung wird von MPSD als sichere JSON-Dokument befinden, in der Xbox Live-Cloud gespeichert. Insbesondere weist eine Sitzung MPSD folgende Merkmale auf:   Erstellt und verwaltet wird vom Titel.

-   Verfügt über einen eindeutigen URI. Weitere Informationen finden Sie unter **Sitzung Directory URIs**.
-   Ermöglicht Verbindungen zwischen Benutzern, die als Mitglieder der Sitzung.
-   Speichert Daten für das Aktivieren von Game wiedergegeben werden, z. B. pro-Member-Attribute, Spiele Einstellungen bootstrapping und Gameserver Informationen.

MPSD unterstützt mehrere Variationen der Sitzung, für die Verwendung bei der Einrichtung von Multiplayer-Spiele. Jede Sitzung enthält Spiel Xbox-Benutzer-IDs (XUIDs) und Schützen von Gerätedaten Zuordnung-Adresse.

-   Spiele-Sitzung als das Muster für Spiele verwendet. Eine Spiele-Sitzung kann es sich um Peer-zu-Peer, Peer-zu-Host, Peer-zu-Server oder eine Mischung dieser Typen sein.
-   Ticket-Sitzung eine Helper-Sitzung verwendet, um den Status der eine Übereinstimmung bei der Vermittlung nachzuverfolgen. Dabei handelt es sich oft auch eine Sitzung Lobby, eine Spiele-Sitzung kann manchmal sein. Finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).
-   Zielsitzung eine Helper-Sitzung erstellt, bei der Vermittlung, um die Ausführung des übereinstimmenden Spiels darzustellen. Es ist fast immer auch eine Spiele-Sitzung. Finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).
-   Lobby-Sitzung eine Helper-Sitzung verwendet, um die eingeladenen Spieler zu ermöglichen, die darauf warten, Spiele Sitzung nicht beitreten. Vielzahl von Titeln sowohl eine Lobby Sitzung erstellen und eine Spiele-Sitzung. Weitere Informationen finden Sie unter **Verwalten von Spielern im Titel**.

## <a name="mpsd-change-notification-handling-and-disconnect-detection"></a>MPSD Benachrichtigungsverarbeitung ändern, und Trennen von Erkennung

MPSD kann Clients mithilfe des WebSockets der aktivitätsüberwachung in Echtzeit-Dienst eine Verbindung herstellen. Die Verbindung wird verwendet, um:

-   Senden Sie kurze Benachrichtigungen (Schulter Taps) bei Sitzung Änderungen anhand von Ereignisabonnements, die initiieren titles.
-   Benutzer Trennvorgänge zu erkennen.
-   Benutzer werden als inaktiv festlegen und diese anschließend wieder entfernt, in der Sitzung basierend Erkennung beim Trennen der Verbindung.


### <a name="making-user-connections"></a>Herstellen von Benutzerverbindungen

Die Bibliothek XSAPI verwaltet die Verbindung zwischen dem Client und dem MPSD. Der Titel ruft zuerst die **MultiplayerService.EnableMultiplayerSubscriptions Methode**. Diese Methode weist XSAPI, dass der Client eine aktivitätsüberwachung in Echtzeit-Verbindung für mehrere Spieler Zwecke verwendet werden soll. Anschließend, wenn der Titel der ersten Aufruf von macht die **MultiplayerService.WriteSessionAsync Methode** oder **MultiplayerService.WriteSessionByHandleAsync Methode**, mit dem aktuellen Benutzer aktivieren, um die Status, eine Verbindung erstellt und mit MPSD verknüpft.

| Hinweis                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------------------------|
| Zum Aktivieren von Benachrichtigungen der Sitzung und Trennvorgänge erkennen muss die sitzungsvorlage der ConnectionRequiredForActiveMembers auf "true" festgelegt. |

| Hinweis                                                                                                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| In früheren Versionen der XSAPI, Titel wird aufgerufen, die **RealTimeActivityService.ConnectAsync Methode** benutzerverbindungen auf den Dienst für die aktivitätsüberwachung in Echtzeit erstellen. Für die 2015 Multiplayer-Spiele diese Methode bewirkt nichts, und die Verbindung wird bei Bedarf erstellt. |

### <a name="subscribing-for-session-changes"></a>Abonnieren von Änderungen der Sitzung

MPSD verwendet "Schulter durch Tippen auf" als eine einfache Benachrichtigung, die Aktionen von Interesse geändert wurde. Der Titel sollte die geänderte Ressource aus, um zu bestimmen, die genaue Art der Änderung abrufen. Mit Abonnements, die aktiviert werden, kann der Titel für die Schulter auf Änderungen der Sitzung mit einem Aufruf von tippt Abonnieren der **MultiplayerSession.SetSessionChangeSubscription Methode**. Finden Sie unter [Vorgehensweise: Melden Sie sich für Änderungsbenachrichtigungen für MPSD Sitzung](multiplayer-how-tos.md).


### <a name="handling-shoulder-taps"></a>Behandeln von Schulter tippt

Wenn eine Änderung an einer Sitzung des Titels-Abonnement, für die jeweilige Sitzung übereinstimmt, benachrichtigt MPSD den Titel der die Änderung mithilfe der **RealTimeActivityService.MultiplayerSessionChanged Ereignis**. Der Titel sollte dann die Sitzung Abrufen der Sitzung mit der vorherigen zwischengespeicherte Ansicht die abgerufene Version vergleichen und Aktionen entsprechend.


### <a name="handling-notifications-of-connection-state-changes"></a>Behandeln von Benachrichtigungen über Änderungen des Verbindungsstatus

Der Titel kann auch über Änderungen in den Zustand der Verbindung mit MPSD benachrichtigt werden. Zwei Ereignisse signalisieren, diese Änderungen: ** RealTimeActivityService.MultiplayerSubscriptionsLost Ereignis **: wird ausgelöst, wenn des Titels-Verbindung mit dem Dienst für die aktivitätsüberwachung in Echtzeit MPSD verloren geht. Wenn dieses Ereignis auftritt, sollte der Titel der Multiplayer Herunterfahren.
-   ** Ereignis für RealTimeActivityService.ConnectionStateChanged ** – bei einer temporären Änderung in die Integrität des Titels-Verbindung mit dem Dienst für die aktivitätsüberwachung in Echtzeit ausgelöst. Der Titel ist nicht erforderlich, keinen Aktionen beim Empfang dieses Ereignisses, aber es kann hilfreich sein, das Ereignis zu Diagnosezwecken verwendet.


### <a name="disconnecting-clients"></a>Trennen der Clientverbindungen

Clients für den Titel trennen MPSD aus, wenn der Titel Benachrichtigungen mit einem Aufruf von deaktiviert die **RealTimeActivityService.DisableMultiplayerSubscriptions Methode**. Kurz nach dem Aufruf der **MultiplayerSubscriptionsLost** -Ereignis ausgelöst wird, der angibt, dass ein Client vom MPSD getrennt wurde.

| Hinweis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| In früheren Versionen Multiplayer-Titeln wird aufgerufen, die ** RealTimeActivityService.Disconnect-Methode ** um die aktivitätsüberwachung in Echtzeit-Dienst zu trennen. Für die 2015 Multiplayer-Spiele führt diese Methode keine Aktion. Die Trennung erfolgt automatisch nach dem **DisableMultiplayerSubscriptions** aufgerufen wird, treten keine Benutzer, der die WebSocket-Verbindung, z. B. ein Abonnement auf Vorhandensein eines aktivitätsüberwachung in Echtzeit. |


### <a name="disconnect-detection"></a>Trennen von Erkennung

MPSD verwendet die Funktion zum Erkennen Sie schnell herausfinden, wenn ein Benutzer nicht ordnungsgemäß getrennt wird getrennt. Eine nicht ordnungsgemäße trennen kann auftreten, wenn des Spielers Netzwerk ein Fehler auftritt oder stürzt ab, ein Titel. MPSD wechselt des Spielers, auf dem getrennten Status von "aktiv" inaktiv und benachrichtigt andere Sitzung-Member, der die Änderung nach Bedarf, basierend auf der Member-Abonnements für die Sitzung.

## <a name="mpsd-handles-to-sessions"></a>MPSD-Handles für Sitzungen

Ein MPSD Sitzungshandle handelt es sich um einen abstrakten und unveränderlich Verweis zu einer Sitzung, die auch zusätzliche typisierte Daten enthalten kann. Es ist ein Dateihandle ähnelt. Alle Handles haben ein Handle-ID (GUID) und einen vollständigen Sitzung Verweis bestehend aus der Dienstkonfiguration ID (SCID), sitzungsvorlage und Sitzungsnamen an. Ein Handle kann nicht aktualisiert werden, aber es erstellt, gelesen und gelöscht werden kann.

| Hinweis                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Ein Handle kann zu einer Sitzung verweisen, die nicht vorhanden ist. Erstellen ein Handle mit einem Sitzungsnamen nicht vorhandene bewirkt nicht, eine neue Sitzung erstellt werden. |


### <a name="handle-types"></a>Typen von Handles

2015 Multiplayer unterstützt, die in diesem Abschnitt beschriebenen Typen von Handles.

#### <a name="invite-handle"></a>Handle einladen

Ein Handle für die INVITE-Nachricht stellt eine Einladung (einladen) einer bestimmten Person dar. Typspezifischen Daten umfasst die Quelle Person, die Zielperson und beschreiben die INVITE-Nachricht, z. B. eine bestimmte Spielmodus Kontextzeichenfolge.

Ein Handle für die INVITE-Nachricht gewährt Lese-/ Schreibzugriff auf eine geöffnete Sitzung. Wenn die Sitzung geschlossen wird, wird das Handle schreibgeschützte Sitzung-Zugriff gewährt.

| Hinweis                                                |
|------------------------------------------------------------------|
| MPSD kann eine Einladung erstellen, auch wenn die Sitzung vollständige oder geschlossen ist. |


#### <a name="activity-handle"></a>Aktivität-Handle

Ein Handle für die Aktivität gibt an, welche Vorgänge ein Benutzer zum Zeitpunkt ausführt. Die Aktivität des Benutzers wird dargestellt, durch die **MultiplayerActivityDetails Klasse**.

| Hinweis                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ein Handle für die Aktivität kann auch explizit, aber nur durch den besitzenden Titel für einen bestimmten Benutzer gelöscht werden. Diese Löschung erfolgt mithilfe der **MultiplayerService.ClearActivityAsync Methode**. |


### <a name="creating-an-invite-handle"></a>Erstellen einen Handle für die INVITE-Nachricht

Zum Erstellen einer Einladung-Handles, die Title-Aufrufe der **MultiplayerService.SendInvitesAsync Methode**. Die-Methode sendet eine Einladung an die angegebenen Benutzer in Form von einer Benutzeroberfläche fest Toast, die die Empfänger auf fungieren kann, um die Einladung anzunehmen.


### <a name="creating-an-activity-handle"></a>Erstellen einen Handle für die Aktivität

Zum Erstellen einer Aktivität Handles, die Title-Aufrufe der **MultiplayerService.SetActivityAsync Methode**. MPSD wird die neue Handle-ID als gebundene Aktivität das Session-Element. Wenn eine vorherige gebundene Aktivität aufgetreten ist, löscht MPSD das entsprechende Handle. Wenn das aktive Element inaktiv oder die Sitzung verlässt, löscht MPSD das Handle gebundenen Aktivität an.


### <a name="using-handles"></a>Mithilfe von Handles

Der Titel verwendet Handles, wenn ein Benutzer eine Einladung (Einladung Handle) annimmt und ein Benutzer einen Freund aktuelle Aktivität (Aktivität Handle) verknüpft. In beiden Fällen müssen der Titel:

1.  Die Handle-ID aus dem Titel Aktivierungsparameter abrufen.
2.  Erstellen Sie ein lokales MPSD Session-Objekt, und fügen Sie ihn als aktiv.
3.  Schreiben Sie die Sitzung, in das entsprechende Handle übergeben.

## <a name="synchronization-of-session-updates"></a>Synchronisierung von Aktualisierungen der Sitzung

Da eine Sitzung handelt es sich um eine freigegebene Ressource, die erstellt oder aktualisiert werden kann von jedem Mitglied, ist das Potenzial für schreibkonflikten. Dies kann zu unerwarteten Ergebnissen führen, z. B. ein Titel kann die Änderungen durch einen anderen Titel unwissentlich überschreiben. MPSD Ansatz zum Beheben dieser Konflikte werden optimistische Parallelität und ein Lese-Änderungs-Schreib-Muster unterstützen.

Synchronisierung von Aktualisierungen MPSD Sitzung verwenden Sie zwei ähnliche allgemeine Implementierungsmuster:

-   Arbiter aktualisiert freigegebenen Teile der Sitzung. Wenn Ihre Implementierung der Arbiter einen einzelnen umfasst, können Sie vermeiden, mithilfe von synchronisierten Updates für die meisten Schreibvorgänge. Der Titel kann vermeiden, Synchronisierung für die (1) ein Update, das der Arbiter auf freigegebenen Teile der Sitzung ist, es sei denn, sie im Zusammenhang mit der Kommunikation die Arbiter Identität und (2) alle Updates, die ein Titel in den Bereich des Elements in der Sitzung zu machen.

| Hinweis                                                                                                                                                                                                                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Obwohl die oben genannten Updatetypen keine Synchronisierung erforderlich ist, es ist jedoch wichtig ist, um Updates zu synchronisieren der ** MultiplayerSessionProperties.HostDeviceToken Eigenschaft **. Diese Eigenschaft wird verwendet, um die Identität des Arbiter, z. B. im Rahmen der Migration der Arbiter zu kommunizieren. |

-   Alle Clients aktualisieren freigegebenen Teile der Sitzung. In diesem Fall müssen alle Updates auf freigegebenen Teile der Sitzung synchronisiert werden. Titel können jedoch weiterhin auf ihre eigenen Member Bereiche ohne Synchronisierung schreiben.


### <a name="session-update-synchronization-using-the-multiplayer-winrt-api"></a>Synchronisierung von Sitzung mithilfe der Multiplayer-WinRT-API

Die folgenden Multiplayer-WinRT-API-Methoden implementieren die vollständigen Parallelität:

-   **MultiplayerService.WriteSessionAsync-Methode**
-   **MultiplayerService.WriteSessionByHandleAsync-Methode**
-   **MultiplayerService.TryWriteSessionAsync-Methode**
-   **MultiplayerService.TryWriteSessionByHandleAsync-Methode**

Jede Write-Methode akzeptiert eine **MultiplayerSessionWriteMode Enumeration** Wert. Verwenden Sie übergibt den Wert SynchronizedUpdate macht der vollständigen Parallelität für Updates.

Andere Werte in der Enumeration Beheben potenzieller Konflikte bei der ersten Erstellung einer Sitzung. Jeder Schreibvorgang auf einen Teil der MPSD Sitzung, die möglicherweise durch einen anderen Titel geschrieben werden kann, muss ein synchronisiertes Update verwenden. Allerdings müssen nicht alle Schreibvorgänge geschützt werden.

Titel des versucht, das Objekt für die lokale Sitzung in mithilfe einer Sitzung Schreibmethoden MPSD schreiben und empfängt einen HTTP 412 /-Statuscode, sollten sie die lokale Kopie aktualisieren, durch das Ausgeben einer **MultiplayerService.GetCurrentSessionAsync Methode**Aufruf zum Abrufen der letzten Serverversion der der Sitzung, bevor erneut versucht, den Schreibvorgang. Andernfalls weiterhin, dass das Dokument für die lokale Sitzung die ungültigen Daten enthalten, und die Aufrufe zum Schreiben der Sitzungs weiterhin auftreten.

| Hinweis                                                                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wenn der Titel eines verwendet die **TryWrite\***  Methoden, die aktualisierte Sitzung wird im Fall von einem HTTP/412-Statuscode zurückgegeben. Dieses Verhalten erübrigen sich aufzurufende **GetCurrentSessionAsync**. |

Wenn der Titel einer Sitzung Write-Methode aufruft, wird möglicherweise eine aktualisierte Version der Sitzung zurückgegeben. Wenn es sich handelt, sollten der Titel der lokale zwischengespeicherte Kopie in die neue Version auf threadsichere Weise wechseln.


### <a name="session-update-synchronization-using-the-multiplayer-rest-api"></a>Synchronisierung von Sitzung die Multiplayer-REST-API

MPSD unterstützt optimistische Parallelität in der Sitzung-Updates über die REST-Funktionen, mit dem "If-Match"-HTTP-Header mit dem ETag-Einstellungen und dem Lese-Änderungs-Schreib-Muster. Das ETag in der Anforderung übergeben sollte dem entsprechen, die die MPSD mit der vorherigen read-Anforderung zurückgegeben.

## <a name="calling-mpsd"></a>Aufrufen von MPSD

Es gibt zwei Möglichkeiten für den Titel in MPSD zugreifen kann, um die Multiplayer-System und Vermittlung verwenden:
-   Empfohlen Verwenden Sie die Multiplayer-WinRT-API, die Klassen enthält, die als Wrapper für die RESTful-Funktionalität dienen. Finden Sie unter **Microsoft.Xbox.Services.Multiplayer Namespace**. Für SmartMatch Vermittlung, verwenden Sie die Vermittlung WinRT-API, dargestellt durch die **Microsoft.Xbox.Services.Matchmaking Namespace**.
-   Verwenden von direkten standard-HTTP-Aufrufe für die Multiplayer-Spiele und Vermittlung REST-APIs enthalten, die der **Xbox Live Services-Rest-Referenz**. Die entsprechenden URIs werden in beschrieben **Sitzung Directory URIs** (für Multiplayer-Spiele) und **Vermittlung URIs** (für die Vermittlung). Zugehörige JSON-Objekte werden in beschrieben die **JavaScript Object Notation (JSON)-Objektverweis**.


### <a name="using-the-multiplayer-winrt-api-to-call-mpsd"></a>Mithilfe der Multiplayer-WinRT-API-aufrufen, MPSD

Die empfohlene Methode zum Aufrufen von MPSD ist die Verwendung der Multiplayer-WinRT-API und die Vermittlung WinRT-API.

| Hinweis                                                                                             |
|---------------------------------------------------------------------------------------------------------------|
| Die xdk-Version-Beispiele wurden unter Verwendung der Multiplayer-Spiele und Vermittlung WinRT-APIs und die anderen Elemente von XSAPI geschrieben. |

Verwendung von Wrappercode für die zugrunde liegenden REST-Funktionen kann für einen eher traditionellen Ansatz für die Verwendung von clientseitigen API-Methoden ohne das HTTP-Datenverkehr für jeden Aufruf zu behandeln. Sowohl eine Binärdatei und eine Quelle für XSAPI werden mit der xdk-Version ausgeliefert. Verwenden die Binärdatei direkt, oder ändern Sie die Quelle, und integrieren sie den Titel nach Bedarf.


### <a name="using-the-multiplayer-rest-api-to-interact-with-mpsd"></a>Verwenden die Multiplayer-REST-API für die Interaktion mit MPSD

Der Titel oder einen Dienst, kann standard-HTTP-Aufrufe für die Multiplayer-REST-API und REST-API die Vermittlung verwenden. Wenn Sie REST-Funktionen direkt verwenden, die Aufrufer Probleme löschen, PUT, POST und GET-Aufrufe für das Sitzungsverzeichnis URIs für die meisten Aktionen. In einer PUT-Anforderung wird der Hauptteil der Anforderung in die vorhandene Sitzung zusammengeführt. Liegt keine vorhandener Sitzung, Hauptteil der Anforderung wird zum Erstellen einer neuen Sitzung, zusammen mit der sitzungsvorlage, die auf gespeicherten [Xbox-Entwickler-Portal (XDP)](https://xdp.xboxlive.com) oder [Partner Center](https://partner.microsoft.com/dashboard). Alle Felder sind optional, und nur Deltas müssen angegeben werden. Aus diesem Grund {} ist eine gültige PUT-Anforderung mit NULL Deltas.

Um eine hypothetische PUT-Anforderung auszuführen, die das Ergebnis der Zusammenführung ohne Auswirkungen auf die Kopie des Servers offizielle der Sitzung zurückgibt, können Sie die Abfragezeichenfolge anfügen "? Nocommit = True", die die PUT-Anforderung.

Die Anforderungen und Antworten für die Multiplayer-Spiele und Vermittlung-REST-API-Methoden sind JSON-Dokumente. Eine Anforderung Struktur Multiplayer-Sitzung finden Sie unter **MultiplayerSessionRequest (JSON)**. Eine Struktur zugeordnete Antwort sehen Sie in **MultiplayerSession (JSON)**. Die antwortstruktur die Member der Sitzung als verknüpfte Liste von frames und füllt in anderen schreibgeschützten Eigenschaften der Sitzung und seine Member.


### <a name="querying-for-sessions-and-session-templates-rest"></a>Abfragen von Sitzungen und Vorlagen für Ereignissitzungen (REST)

Ihre Titel können Sitzungsinformationen an der Dienstkonfiguration und die Vorlage sitzungslevel Abfragen. In diesem Thema wird erläutert, Abfragen, die die Multiplayer-REST-API verwenden.


#### <a name="query-for-basic-session-information"></a>Abfragen von Basissitzung-Informationen

Sie können Abfragen für grundlegende Sitzungsinformationen, die mit dem Sitzungsverzeichnis und Vermittlung URIs einrichten. Das Ergebnis einer Abfrage ist ein JSON-Array der Sitzung, die Verweise, einiger Sitzungsdaten Inline enthalten. Eine Abfrage ruft standardmäßig bis zu 100 privaten Sitzungen ab.

| Hinweis                                                          |
|----------------------------------------------------------------------------|
| Jede Abfrage muss entweder ein Schlüsselwortfilter, einen Filter XUID oder beides enthalten. |


#### <a name="query-for-session-templates"></a>Abfragen von Vorlagen für Ereignissitzungen

Verwenden Sie die GET-Methode zum Abrufen der Liste der Vorlagen für ereignissitzungen für die SCID als auch die Details einer bestimmten Sitzungs-Vorlage für eine der folgenden URIs:
-   **/serviceconfigs/{scid}/sessiontemplates**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}**


#### <a name="query-for-session-state"></a>Abfrage für den Sitzungszustand

Um für den Sitzungszustand abzufragen, verwenden Sie die GET-Methode für eine der folgenden URIs:
-   **/serviceconfigs/{scid}/sessions**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions**

## <a name="multiplayer-session-explorer"></a>Multiplayer-Sitzung-Explorer

Multiplayer-Sitzung Explorer ist ein Tool MPSD integriert sind, für das Durchsuchen von Sitzungen, Vorlagen für ereignissitzungen und Lokalisierung von Zeichenfolgen. Das Tool soll nur für Entwicklung Sandboxes verwendet werden.


### <a name="accessing-multiplayer-session-explorer"></a>Explorer für den Zugriff auf Multiplayer-Sitzung

| Hinweis                                                                                                      |
|------------------------------------------------------------------------------------------------------------------------|
| Um das Tool verwenden zu können, müssen Sie angemeldet sein. Die Browser-ist beschränkt, Sitzungen, die der angemeldete Benutzer als Mitglied haben. |

Öffnen Sie Internet Explorer auf Ihre Xbox One, drücken Sie zum Öffnen des Explorers für Multiplayer-Sitzung die **Ansicht** , und geben Sie <https://sessiondirectory.xboxlive.com/debug> in die **Adresse** Feld.

| Hinweis                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sie erhalten einen Statuscode "HTTP/404", wenn Sie versuchen, das Tool in der Sandbox RETAIL zugreifen. Weitere Informationen zu diesem Code finden Sie unter [Multiplayer-Sitzung Statuscodes](multiplayer-session-status-codes.md). |


### <a name="using-multiplayer-session-explorer"></a>Mit dem Explorer für Multiplayer-Sitzung


#### <a name="open-the-main-page"></a>Öffnen Sie den Hauptknoten Seite

1.  Auf die Hauptseite des Tools zugreifen. Sicherheitskontext (angemeldeten Benutzers und Sandbox) und eine Liste der Dienstkonfiguration IDs (SCIDs) in der Sandbox angezeigt.
2.  Drücken Sie die **Menü** Schaltfläche, um diese Seite, um die Startseite anzuheften, sodass Sie nicht den URI jedes Mal eingeben müssen.


#### <a name="display-available-sessions-and-templates"></a>Anzeigen von verfügbaren Sitzungen und Vorlagen

1.  Klicken Sie auf eine SCID im Tool eine Liste der Sitzungen in diesem SCID angezeigt, von denen der angemeldete Benutzer Mitglied ist.
2.  Sie können auf dieser Seite gleichen klicken Sie auf die SCID und Anzeigen von Vorlagen für ereignissitzungen und Lokalisierung von Zeichenfolgen in der Dienstkonfiguration für die SCID. Diese Elemente werden durch erfasst [XDP](https://xdp.xboxlive.com) oder [Partner Center](https://partner.microsoft.com/dashboard).


#### <a name="display-the-full-contents-of-a-session"></a>Zeigt den vollständigen Inhalt einer Sitzung

Multiplayer-Sitzung-Explorer klicken Sie auf einen Sitzungsnamen, um den gesamten Inhalt der entsprechenden Sitzung anzuzeigen.

Die Sitzung wie MPSD gezeigt unterscheiden von der Antwort auf eine GET-Standardmethode für die Sitzung des URI Gründen:

-   GET-Aufruf möglicherweise eine ältere vertragsversion in der X-Xbl-Contract-Version-Header verwenden. Sitzung-Explorer zeigt immer die-Sitzung mit die aktuelle vertragsversion.
-   Wenn eine Sitzung über GET, normalerweise angefordert wird Transformationen und Nebenwirkungen, z. B. ausgelöst werden können, ist die Timeouts abgelaufen. Sitzung Explorer zeigt eine Momentaufnahme der Sitzung, gespeichert werden, ohne dass Sie Logik, Transformationen oder Nebeneffekte ausgeführt.
-   Da das NextTimer JSON-Objekt-Feld zur gleichen Zeit wie die Nebeneffekte berechnet wird, ist es nicht auf MPSD Sitzungen vorhanden.

## <a name="see-also"></a>Siehe auch

[Übersicht über leistungssitzungen](mpsd-session-details.md)

[Statuscodes für Multiplayer-Sitzung](multiplayer-session-status-codes.md)

[So wird es gemacht: Aktualisieren Sie eine Multiplayer-Sitzung](multiplayer-how-tos.md)

[So wird es gemacht: Beitreten Sie zu einer Sitzungs MPSD bei Aktivierung der Titel](multiplayer-how-tos.md)

[So wird es gemacht: Melden Sie sich für Änderungsbenachrichtigungen für MPSD-Sitzung](multiplayer-how-tos.md)

[SmartMatch Vermittlung](smartmatch-matchmaking.md)