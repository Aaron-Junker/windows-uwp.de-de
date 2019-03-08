---
title: Multiplayer-Anweisungen für Miningstrukturen
description: Beschreibt, wie häufige Aufgaben in Xbox Live Multiplayer-2015 implementiert.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.date: 08/29/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Multiplayer-2015
ms.localizationpriority: medium
ms.openlocfilehash: 20bf3486491e8173ce946a3a5dc2e4bc83305c83
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648635"
---
# <a name="multiplayer-how-tos"></a>Vorgehensweisen für Multiplayer

Dieses Thema enthält Informationen dazu, wie bestimmte Aufgaben im Zusammenhang mit mit Multiplayer-2015 implementiert.

* Melden Sie sich für änderungsbenachrichtigungen für MPSD-Sitzung
* Erstellen einer Sitzung für MPSD
* Einen Arbiter für eine Sitzung MPSD festlegen
* Verwalten der Title-Aktivierung
* Stellen Sie den Benutzer beigetreten werden kann
* Senden von Spieleinladungen
* Beitreten Sie zu einer spielen-Sitzungs aus einer Sitzung lobby
* Beitreten Sie zu einer Sitzungs MPSD bei Aktivierung der Titel
* Legen Sie die aktuelle Aktivität des Benutzers
* Aktualisieren Sie eine Sitzung MPSD
* Lassen Sie eine Sitzung MPSD
* Geben Sie bei der Vermittlung geöffnete Sitzung slots
* Erstellen Sie ein Ticket für die Übereinstimmung
* Match-Ticket-Status abrufen

## <a name="subscribe-for-mpsd-session-change-notifications"></a>Melden Sie sich für änderungsbenachrichtigungen für MPSD-Sitzung

| Hinweis                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abonnieren von Änderungen an eine Sitzung erfordert die zugeordneten Player in der Sitzung aktiv ist. Das Feld "ConnectionRequiredForActiveMembers" muss auch festgelegt werden, auf "true", in dem /constants/system/capabilities-Objekt, für die Sitzung. Dieses Feld ist in der Regel in der sitzungsvorlage festgelegt. Finden Sie unter [Vorlagen für Ereignissitzungen MPSD](multiplayer-session-directory.md). |



Um MPSD Sitzung änderungsbenachrichtigungen erhalten möchten, kann der Titel die folgende Vorgehensweise befolgen.

1.  Verwenden Sie den gleichen **XboxLiveContext Klasse** Objekt für alle Aufrufe vom gleichen Benutzer. Abonnements sind an die Lebensdauer dieses Objekts gebunden. Wenn mehrere lokale Benutzer vorhanden sind, verwenden Sie eine Separate **XboxLiveContext** Objekt für jeden Benutzer.
2.  Implementieren von Ereignishandlern für die **RealTimeActivityService.MultiplayerSessionChanged Ereignis** und **RealTimeActivityService.MultiplayerSubscriptionsLost Ereignis**.
3.  Wenn Sie Änderungen für mehrere Benutzer abonnieren, fügen Sie Code Ihre **MultiplayerSessionChanged** -Ereignishandler, um unnötige Arbeit zu vermeiden. Verwenden der **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Eigenschaft** und **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Eigenschaft**. Verwendung dieser Eigenschaften ermöglicht das Nachverfolgen der letzten Änderung, sichtbar und wird ignoriert, ältere Änderungen.
4.  Rufen Sie die **MultiplayerService.EnableMultiplayerSubscriptions Methode** um Abonnements zu ermöglichen.
5.  Erstellen Sie einen lokalen Session-Objekt und Join dieser Sitzung als aktiv.
6.  Aufrufe für jeden Benutzer auf die **MultiplayerSession.SetSessionChangeSubscription Methode**, Typ, für den benachrichtigt werden, übergeben die Sitzung zu ändern.
7.  Die Sitzung zu MPSD jetzt schreiben, wie in beschrieben **Vorgehensweise: Aktualisieren Sie eine Multiplayer-Sitzung**.

Das folgende Flussdiagramm veranschaulicht, wie Multplayer zu starten, indem das Abonnieren von Ereignissen in diesem Verfahren beschrieben wird.

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>Benachrichtigungen über doppelte Sitzungs-Analyse

Wenn mehrere Benutzer, die Benachrichtigungen abonniert werden, für die gleiche Sitzung vorhanden sind, löst jede Änderung an der Sitzung für jeden Benutzer durch Tippen auf Schulter. Werden alle bis auf eines der folgenden Duplikate ein. Weiterhin wird zwar empfohlen, ein Titel jeder Benutzer in einer Sitzung mit Benachrichtigungen abonnieren, sollten ein Titel Änderungen ignorieren, denen sie bereits über benachrichtigt wird; Sie können dazu die Verzweigung und ChangeNumber-Eigenschaft.

Um mehrere Schulter Taps erkennen zu können, sollten ein Titel ein:

-   Für jede **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Eigenschaft** Wert angezeigt, die neuesten Store **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Eigenschaft**.
-   Verfügt Schulter durch Tippen auf eine höhere **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Eigenschaft** als die letzte dafür gesehen **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch-Eigenschaft** , verarbeiten und aktualisieren Sie die neueste Version **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Eigenschaft**.
-   Wenn Schulter durch Tippen auf eine höhere keinen **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Eigenschaft** , **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Eigenschaft**, diesen Schritt überspringen. Diese Änderung wurde bereits verarbeitet.

| Hinweis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Eigenschaft** Werte müssen für die nachverfolgung von **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Eigenschaft**, nicht von der Sitzung. Es ist möglich, dass die **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch Eigenschaft** Wert ändern (und die **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber Eigenschaft**zurücksetzen) innerhalb der Lebensdauer einer Sitzung. |

## <a name="create-an-mpsd-session"></a>Erstellen einer Sitzung für MPSD


| Hinweis                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Standardmäßig wird eine MPSD-Sitzung erstellt, wenn das erste Element verknüpft. Wenn Ihre Logik Titel erwartet, den Titel vorhanden sein, oder beim Beitritt nicht vorhanden dass, können sie einen entsprechenden Write-Modus-Wert an die Write-Methode während des Updates für die Sitzung übergeben. |



Der Titel müssen Folgendes ein, um eine neue Sitzung zu erstellen:

1.  Erstellen Sie ein neues **XboxLiveContext Klasse** Objekt. Titel des erstellt dieses Objekt einmal, speichert ihn und verwendet ihn nach Bedarf in den Quellcode erneut. Insbesondere bei der Arbeit mit Sitzung Abonnements ist es erforderlich, genau die gleiche Kontext verwenden.
2.  Erstellen Sie ein neues **MultiplayerSession Klasse** Objekt, das die Sitzungsdaten vorzubereiten, das die MPSD benötigt, um eine neue Sitzung zu erstellen.
3.  Stellen Sie vor dem Schreiben von der Sitzungs auf MPSD erforderlichen Änderungen vor. Beispielsweise, wenn ein Element in der Sitzung mit einem Aufruf von einbinden **MultiplayerSession.Join Methode**, der Client fügt ausgeblendeten lokale Anforderungsdaten, die mitteilt, MPSD, nach dem Aufruf zum Aktualisieren der Sitzungs beizutreten.
4.  Wenn lokale Änderungen vorgenommen werden, Schreiben der Nachrichten in MPSD Siehe **Vorgehensweise: Aktualisieren Sie eine Multiplayer-Sitzung**.
5.  Erhalten Sie die neue **MultiplayerSession** -Sitzungsobjekts MPSD, mit der viele Felder ausgefüllt.
6.  Verwenden Sie das neue Sitzung-Objekt, das in Zukunft, und verwerfen Sie die alte Kopie, die eine versteckte Anforderung zum Erstellen einer neuen Sitzungs enthält.

### <a name="example"></a>Beispiel

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Partner Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>Einen Arbiter für eine Sitzung MPSD festlegen




Der Titel verwendet das folgende Verfahren, um ein Arbiter für eine Sitzung festgelegt werden, die bereits erstellt wurde.

| Hinweis                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gerätetoken für die Elemente (potenzielle Hosts) sind nicht verfügbar, bis die Elemente die Sitzung verknüpft und enthalten ihre Adressen sicheres Gerät haben. |

1.  Rufen Sie die gerätetoken für Kandidaten des Hosts aus der MPSD mithilfe der **MultiplayerSession.Members Eigenschaft**.

| Hinweis                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Wenn die Sitzung von SmartMatch Vermittlung erstellt wurde, können Ihre Clients die Kandidaten des Hosts verfügbar MPSD über die **MultiplayerSession.HostCandidates Eigenschaft**. |

2.  Wählen Sie den erforderlichen Host aus der Liste der Kandidaten des Hosts.
3.  Rufen Sie die **MultiplayerSession.SetHostDeviceToken Methode** das gerätetoken in den lokalen Cache, der die MPSD festgelegt. Wenn der Aufruf von Geräte-Token für den Host festgelegt erfolgreich ist, ersetzt das lokale gerätetoken des Hosts-Token.
4.  Wenn ein HTTP 412/Statuscode, beim Versuch empfangen wird, die Geräte-Token für den Host festgelegt, wird abzufragen Sie die Sitzungsdaten, und festzustellen Sie, ob das gerätetoken für den Host für die lokale Konsole ist. Wenn es sich nicht für die lokale Konsole ist, wurde als der Arbiter einer anderen Konsole festgelegt.

| Hinweis                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Der Client sollte den Statuscode "HTTP 412 /" getrennt von anderen HTTP-Codes, verarbeiten, da HTTP/412 nicht standardmäßigen Fehler angegeben. Weitere Informationen zu den Statuscode, finden Sie unter [Multiplayer-Sitzung Statuscodes](multiplayer-session-status-codes.md). |

5.  Aktualisieren Sie die Sitzung in MPSD, siehe **Vorgehensweise: Aktualisieren Sie eine Multiplayer-Sitzung**.

| Hinweis                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wenn Sie keine besseren Algorithmus verfügen, kann der Client einen greedy-Algorithmus implementieren, in dem einzelnen Kandidaten Host versucht, selbst als Host festgelegt, wenn niemand sonst sie noch nicht ausgeführt wurde. Weitere Informationen finden Sie unter [Sitzung Arbiter](mpsd-session-details.md). |

## <a name="manage-title-activation"></a>Verwalten der Title-Aktivierung

Xbox One löst die **CoreApplicationView.Activated Ereignis** während der protokollaktivierung. Dieses Ereignis wird im Kontext der multiplayer-API ausgelöst, wenn ein Benutzer eine Einladung akzeptiert oder einen anderen Benutzer verknüpft. Diese Aktionen lösen eine Aktivierung, die der Titel zu reagieren muss, indem Sie wie den verknüpften Benutzer Spiels mit dem Zielbenutzer machen.

| Hinweis                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| Titel des neuen Aktivierungsargumenten zu einem beliebigen Zeitpunkt erwarten, und sollte nie für Länge codiert werden. |

Der Titel muss die folgenden Hauptschritte zum Verarbeiten der Title-Aktivierung ausführen.

1.  Richten Sie einen Ereignishandler für die **CoreApplicationView.Activated Ereignis**. Dieser Handler wird jedes Mal, wenn protokollaktivierung auftritt, ausgelöst, selbst wenn der Titel bereits ausgeführt wird.
2.  Beginnen Sie bei der Aktivierung der Titel eine Sitzung, und melden Sie sich für änderungsbenachrichtigungen für die Sitzung. Finden Sie unter **Vorgehensweise: Melden Sie sich für Änderungsbenachrichtigungen für MPSD Sitzung**.
3.  Fügen Sie den Benutzer die Sitzung als aktiv. Finden Sie unter **Vorgehensweise: Beitreten zu eine Sitzung MPSD bei Aktivierung der Titel**.
4.  Legen Sie die Sitzung Lobby, als der Aktivität-Sitzung über das Profil Benutzeroberfläche verfügbar gemacht werden. Finden Sie unter **Vorgehensweise: Legen Sie die aktuelle Aktivität des Benutzers**.
5.  Fügen Sie den Benutzer die Spiele-Sitzung als aktiv. Der Benutzer kann jetzt eine Verbindung herstellen, um Peers und geben Sie Spiele oder Empfangsbereich.

Das folgende Flussdiagramm veranschaulicht das Verarbeiten der Title-Aktivierung.

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>Stellen Sie den Benutzer beigetreten werden kann

Damit wird den Benutzer beigetreten werden kann, muss der Titel der folgenden Schritte ausführen:

1.  Erstellen Sie ein Sitzungsobjekt, und ändern Sie die Attribute nach Bedarf.
2.  Fügen Sie den Benutzer die Sitzung als aktiv. Finden Sie unter **Vorgehensweise: Beitreten zu eine Sitzung MPSD bei Aktivierung der Titel**.
3.  Bestimmt, ob der Benutzer als der Arbiter Sitzung festgelegt wurde.
4.  Wenn der Benutzer nicht der Arbiter ist, wechseln Sie mit Schritt 7 fort.
5.  Wenn der Benutzer der Arbiter ist, rufen Sie die **MultiplayerSession.SetHostDeviceToken Methode**.
6.  Versuchen Sie, schreiben die Sitzung, die über einen Aufruf an die **MultiplayerService.TryWriteSessionAsync Methode**.
7.  Legen Sie die Sitzung, als die aktive Sitzung. Finden Sie unter **Vorgehensweise: Legen Sie die aktuelle Aktivität des Benutzers**.

Das folgende Flussdiagramm veranschaulicht die Schritte zum Ausführen, um einem Benutzer ermöglichen, andere Spieler während eines Spiels verbunden werden.

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>Senden von Spieleinladungen

Der Titel kann einen Player zum Spiel auf folgende Weise Einladungen senden aktivieren:

-   Senden Sie die Einladungen für die Sitzung Lobby.
-   Senden Sie, dass der Einladungen, die mithilfe der generischen Xbox-Plattform Benutzeroberfläche mit dem Verweis Spiel Sitzung laden.

Um, dass das Spiel für einen Player Einladungen zu senden, muss der Titel der folgenden Schritte ausführen:

1.  Stellen Sie die einladenden Spieler beigetreten werden kann. Finden Sie unter **Vorgehensweise: Stellen Sie den Benutzer beigetreten**.
2.  Ermitteln, ob der Einladungen, die über eine Lobby-Sitzung gesendet werden, oder verwenden Sie die INVITE-Nachricht-Benutzeroberfläche.
3.  Wenn die Sitzung Lobby verwenden zu können, senden Sie die Einladungen, die über einen Aufruf an **MultiplayerService.SendInvitesAsync Methode**. Diese Methode möglicherweise die Erstellung von eine Liste mit in-Game-Benutzeroberfläche die **SystemUI.ShowPeoplePickerAsync-Methode** oder **PartyChat.GetPartyChatViewAsync Methode**.
4.  Wenn die INVITE-Nachricht-Benutzeroberfläche verwenden, rufen Sie die **SystemUI.ShowSendGameInvitesAsync-Methode** die INVITE-Nachricht-Benutzeroberfläche angezeigt.
5.  Behandeln der **RealTimeActivityService.MultiplayerSessionChanged Ereignis** für den lokalen Spieler nach der die remote-Player-Joins.
6.  Implementieren Sie für den remote-Player Aktivierungscode Titel ein. Finden Sie unter **Vorgehensweise: Verwalten der Aktivierung der Titel**.

Das folgende Flussdiagramm veranschaulicht das Senden von Einladungen an.

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>Beitreten Sie zu einer spielen-Sitzungs aus einer Sitzung lobby

Gaming-Sitzungen auf Windows 10-Geräte müssen die `userAuthorizationStyle` legen Sie die Funktion auf **"true"** große Sitzungen stehen keine. Dies bedeutet, dass die `joinRestriction` Eigenschaft darf nicht sein, "none", was bedeutet, dass die Sitzung direkt öffentlich beigetreten werden kann.

Ein häufiges Szenario ist, erstellen Sie eine Sitzung Lobby zum Sammeln von Spieler, und klicken Sie dann die Spieler in einem Spiel oder Vermittlung Sitzung verschieben. Wenn jedoch die Gaming-Sitzung nicht öffentlich beigetreten werden kann ist, die Spiele-Clients werden nicht in der Lage, auf die Gaming-Sitzung teilnehmen, es sei denn, sie erfüllen die `joinRestriction` Einstellung, die in den meisten Fällen für dieses Szenario zu restriktiv ist.

Die Lösung ist ein Handle für die Übertragung verwenden, um der Sitzung Lobby und der game-Sitzung zu verknüpfen.  Der Titel dazu wie folgt vorgehen:

1. Wenn Sie auf die game-Sitzung erstellen, verwenden die `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` -API, um ein Handle für die Übertragung zu erstellen, die die Sitzung Lobby und die Spiele-Sitzung verknüpft.
2. Store die Übertragung behandeln GUID in der Sitzung Lobby anstelle der game Sitzung Sitzung Verweises.
3. Wenn möchte, dass der Titel verschieben Sie Elemente aus der Sitzung Lobby für die Spiele-Sitzung, würde jeder Client das Transfer-Handle aus der Sitzung Lobby verwenden, um das Spiel beitreten mithilfe der `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API.
4. MPSD sieht die Lobby-Sitzung, um sicherzustellen, dass die Elemente, bei dem Versuch, das Spiel beitreten, mit dem Handle für die Übertragung auch in der Sitzung Lobby sind.
5. Wenn die Elemente in der Sitzung Lobby sind, werden sie auf die game-Sitzung zugreifen.

## <a name="join-an-mpsd-session-from-a-title-activation"></a>Beitreten Sie zu einer Sitzungs MPSD bei Aktivierung der Titel

Wenn ein Benutzer beschließt, einen Freund Aktivität verknüpfen, oder übernehmen Sie eine INVITE-Nachricht mithilfe der Benutzeroberfläche der Xbox-Shell, wird der Titel mit Parametern aktiviert, die angeben, welche Sitzung des Benutzers beitreten möchten. Der Titel muss behandeln diese Aktivierung und der entsprechenden Sitzung des Benutzers hinzugefügt.

Hier sind die Schritte, die der Titel befolgen sollten:

1.  Implementieren Sie einen Ereignishandler für die **CoreApplicationView.Activated Ereignis**. Dieses Ereignis benachrichtigt der Aktivierungen für den Titel.
2.  Wenn der Handler ausgelöst wird, überprüfen Sie die **IActivatedEventArgs.Kind Eigenschaft**. Wenn sie Protokoll festgelegt ist, wandeln Sie die Ereignisargumente, um **ProtocolActivatedEventArgs Klasse**.
3.  Überprüfen Sie die **ProtocolActivatedEventArgs** Objekt. Wenn im URI angegeben die **ProtocolActivatedEventArgs.Uri Eigenschaft** entspricht entweder InviteHandleAccept (Einladung akzeptiert entspricht) oder ActivityHandleJoin (entsprechend in einen Join über die Benutzeroberfläche der Shell), analysiert die Abfragezeichenfolge des URIS, die als normale URI-Abfragezeichenfolge mit Schlüssel-Wert-Paaren formatiert ist, extrahieren die folgenden Felder:
    -   Für die Einladung akzeptiert ein:
        1.  Handle
        2.  invitedXuid
        3.  senderXuid
    -   Für einen Join:
        1.  Handle
        2.  joinerXuid
        3.  joineeXuid

4.  Starten Sie den Titel des Multiplayer-Code, mit dem Aufruf von aufzunehmen der **MultiplayerService.EnableMultiplayerSubscriptions Methode**.
5.  Erstellen Sie eine lokale **MultiplayerSession Klasse** Objekt, um mithilfe der **MultiplayerSession-Konstruktor (XboxLiveContext)**.
6.  Rufen Sie die **MultiplayerSession.Join-Methode (String, Boolean, Boolean)** zu der Sitzung beizutreten. Verwenden Sie die folgenden parametereinstellungen, sodass die Verknüpfung als aktiv festgelegt ist:
    -   *memberCustomConstantsJson* = null
    -   *InitializeRequested* = "false"
    -   *joinWithActiveStatus* = true

7.  Rufen Sie die **MultiplayerSession.SetSessionChangeSubscription Methode** auf die Schulter-getippt werden, wenn die Sitzung nach der Verknüpfung geändert wird.
8.  Rufen Sie die **MultiplayerService.WriteSessionByHandleAsync Methode**, mit dem Handle abgerufen werden, wie in Schritt 3 beschrieben. Jetzt wird der Benutzer ein Mitglied der Sitzung und kann Daten in der Sitzung für das Spiel die Verbindung.

## <a name="set-the-users-current-activity"></a>Legen Sie die aktuelle Aktivität des Benutzers

Aktuelle Aktivität des Benutzers wird im Xbox-Dashboard-Benutzeroberflächen für den Titel angezeigt. Aktivität für einen Benutzer kann über eine-Sitzung oder durch die Aktivierung der Titel festgelegt werden. Im zweiten Fall gibt der Benutzer eine Sitzung über Vermittlung, oder indem Sie ein Spiel zu starten.

| Hinweis                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Aktivität festgelegt, die über eine Sitzung kann gelöscht werden, durch einen Aufruf der **MultiplayerService.ClearActivityAsync Methode**. |

Festlegen eine Sitzung als der aktuellen Aktivität, die Title-Aufrufe der **MultiplayerService.SetActivityAsync Methode**, übergeben den Verweis für die Sitzung, für die Sitzung.

Zum Festlegen der aktuellen Aktivität durch die Aktivierung der Titel finden Sie unter **Vorgehensweise: Beitreten zu eine Sitzung MPSD bei Aktivierung der Titel**.

## <a name="update-an-mpsd-session"></a>Aktualisieren Sie eine Sitzung MPSD

| Hinweis                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wenn Ihre Titel eine vorhandene Sitzung mithilfe der multiplayer-API aktualisiert wird, denken Sie daran, dass es mit einer lokalen Kopie, arbeitet, bis der Aufruf zum Schreiben der Sitzungs. |

Um eine vorhandene Sitzung zu aktualisieren, müssen der Titel:

1.  Nehmen Sie die Änderungen an der aktuellen Sitzung nach Bedarf, z. B. durch Aufrufen der **MultiplayerSession.Leave Methode**.
2.  Wenn alle Änderungen vorgenommen werden, können schreiben Sie die lokalen Änderungen in MPSD, die mit einer der folgenden Methoden:

    -   **MultiplayerService.WriteSessionAsync-Methode**
    -   **MultiplayerService.WriteSessionByHandleAsync Methode**.
    -   **MultiplayerService.TryWriteSessionAsync-Methode**
    -   **MultiplayerService.TryWriteSessionByHandleAsync-Methode**

    Legen Sie den Schreibmodus auf **SynchronizedUpdate** Schreiben in einen freigegebenen Teil, die von anderen Titel auch ändern. Finden Sie unter [Sitzung Synchronisierungsupdates](multiplayer-session-directory.md) für Weitere Informationen.

    Die Write-Methode schreibt den Join mit dem Server und ruft Sie ab der aktuellen Sitzung, von dem anderen Mitgliedern der Sitzung und die sichere Geräteadressen (SDAs) ihre Konsolen zu ermitteln. Weitere Informationen zum Einrichten der Netzwerkverbindung zwischen dieser Konsolen finden Sie unter **Einführung in die Winsock für Xbox One**.

3.  Verwerfen Sie alte lokale Session-Objekt zu, und verwenden Sie die neu abgerufenen Session-Objekt, sodass zukünftige Aktionen für den letzten bekannten Sitzungszustand basieren.

## <a name="leave-an-mpsd-session"></a>Lassen Sie eine Sitzung MPSD

Damit einen Benutzer eine Sitzung verlassen können, muss der Titel Folgendes tun.

1.  Rufen Sie die **MultiplayerSession.Leave Methode** für die Spiele-Sitzung.
2.  Aktualisieren Sie die Spiele-Sitzung in MPSD, siehe **Vorgehensweise: Aktualisieren Sie eine Multiplayer-Sitzung**.
3.  Rufen Sie gegebenenfalls **lassen** Methode für die Lobby-Sitzung, und aktualisieren Sie diese Sitzung.
4.  Bei Bedarf für die Sitzung Lobby, fahren Sie die multiplayer-API durch Aufheben der Registrierung der **RealTimeActivityService.MultiplayerSubscriptionsLost Ereignis** und  **RealTimeActivityService.MultiplayerSessionChanged Ereignis**.

Das folgende Flussdiagramm veranschaulicht, wie auf die Sitzung beenden und heruntergefahren wird.

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>Geben Sie bei der Vermittlung geöffnete Sitzung slots

Um ein Ticket-Sitzung bei der Vermittlung öffnen Slots einzugeben, muss der Titel ähnlich der folgenden Schritte ausführen:

1.  Zugriff auf den aktuellen Sitzungsstatus für die Ticket-Sitzung, die bei der Vermittlung erstellt.
2.  Fügen Sie in der Sitzung Lobby verfügbaren Player für Spiele hinzu.
3.  Bestimmt, ob die Sitzung Ticket voll ist.
4.  Wenn die Sitzung voll ist, fahren Sie Spiele.
5.  Wenn die Sitzung noch nicht voll ist, erstellen Sie das Ticket Übereinstimmung, wie im **Vorgehensweise: Erstellen Sie ein Ticket Übereinstimmung**. Achten Sie darauf, dass Sie das Ticket durch Erstellen der *PreserveSession* -Parameter auf Always festgelegt.
6.  Fahren Sie mit der Vermittlung. Finden Sie unter [mit SmartMatch Vermittlung](using-smartmatch-matchmaking.md).

Das folgende Flussdiagramm veranschaulicht, wie geöffnete Sitzung Slots bei der Vermittlung ausgefüllt wird.

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>Erstellen Sie ein Ticket für die Übereinstimmung

Um ein Ticket für die Übereinstimmung zu erstellen, müssen die Vermittlung Scout ein:

1.  Rufen Sie die **MatchmakingService.CreateMatchTicketAsync Methode**, und übergeben Sie einen Verweis auf die Ticket-Sitzung. Die Methode liest die Ticket-Sitzung aus MPSD und Vermittlung, für die Benutzer in der Sitzung gestartet. Intern die Methode ruft die **POST (/serviceconfigs/ {scid} /hoppers/ {Hoppername})**.
2.  Legen Sie die *PreserveSession* Parameter nie ist die Vermittlungsdienst entsprechend die Elemente der Sitzung in eine neue Sitzung oder einer anderen vorhandenen Sitzung. Legen Sie die *PreserveSession* Parameter, um immer um den Titel in einer vorhandenen Spiele-Sitzung als Ticket Sitzung weiterhin Spiels zu ermöglichen. Die Vermittlungsdienst kann dann sicherstellen, dass die übermittelte Sitzung beibehalten wird und dieser Sitzung werden alle übereinstimmenden Playern hinzugefügt.

3.  Verwenden der **CreateMatchTicketResponse.EstimatedWaitTime Eigenschaft** zurückgegeben, der **CreateMatchTicketResponse-Klasse** Objekt zum Festlegen von Erwartungen der Benutzer der Vermittlung Zeit.
4.  Verwenden der **CreateMatchTicketResponse.MatchTicketId Eigenschaft** in das Antwortobjekt Vermittlung für die Sitzung bei Bedarf durch das Ticket löschen Abbrechen zurückgegeben. Ticket löschen verwendet die **MatchmakingService.DeleteMatchTicketAsync Methode**.

## <a name="get-match-ticket-status"></a>Match-Ticket-Status abrufen

Titel des tun Folgendes ein, um die Übereinstimmung Ticket-Status abrufen:

1.  Abrufen der **MultiplayerSession Klasse** Objekt für die Ticket-Sitzung.
2.  Verwenden der **MultiplayerSession.MatchmakingServer Eigenschaft** für den Zugriff auf die **MatchmakingServer Klasse** , das in der Vermittlung verwendet.
3.  Überprüfen Sie die **MatchmakingServer** Objekt, das den Status des Prozesses für die Vermittlung, die typische Wartezeit für die Sitzung, und die Ziel-Sitzung Referenz zu ermitteln, wenn eine Übereinstimmung gefunden wurde.
