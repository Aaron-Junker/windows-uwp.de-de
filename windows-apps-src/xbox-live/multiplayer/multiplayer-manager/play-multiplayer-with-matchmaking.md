---
title: Wiedergeben eines Multiplayer-Spiels mit SmartMatch
description: Erfahren Sie, wie Xbox Live-Multiplayer-Manager verwenden, um ein Spieler ein Multiplayer-Spiels mit SmartMatch suchen zu ermöglichen.
ms.assetid: f9001364-214f-4ba0-a0a6-0f3be0b2f523
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Spiele, Multiplayer-Manager "," Flussdiagramm "," Smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 0c0ba897f23eb690c3044b00cdb4ce3bea975e71
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655955"
---
# <a name="find-a-multiplayer-game-by-using-smartmatch"></a>Suchen eines Multiplayer-Spiels mit SmartMatch

In einigen Fällen kann ein Spieler genug Freunde online haben keinen zu einem Spiel soll, oder sie möchten nur gegen zufälliges Personen online zu spielen. Sie können den Xbox Live SmartMatch-Dienst verwenden, finden Sie weitere Entwicklungen Xbox Live

Diese Seite behandelt die grundlegenden Schritte SmartMatch Vermittlung mit Multiplayer-Manager implementiert werden müssen.

## <a name="find-a-match"></a>Keine Übereinstimmung gefunden

Die Multiplayer-Manager verwenden, um eine Einladung zu senden, Freund des Benutzers enthält, und klicken Sie dann diese Friend verknüpfen Sie das Spiel in Bearbeitung sind vier Schritte erforderlich:

1. [Initialisieren Sie Multiplayer-Manager](#initialize-multiplayer-manager)
2. [Erstellen Sie die Lobby-Sitzung, indem Sie lokale Benutzer hinzufügen](#create-lobby)
3. [Einladungen an Freunde senden](#send-invites)
4. [Akzeptieren der Einladungen](#accept-invites)
5. [Keine Übereinstimmung gefunden](#find-match)

Die Schritte 1, 2, 3 und 5 werden auf dem Gerät ausführen die INVITE-Nachricht ausgeführt.  Schritt 4 wird in der Regel auf des Teilnehmers Computer app-Start über protokollaktivierung initiiert werden.

Sie können ein Flussdiagramm des Prozesses hier anzeigen: [Flussdiagramm: Wiedergabe eines Multiplayer-Spiels mit SmartMatch Vermittlung](mpm-flowcharts/mpm-play-with-smartmatch-matchmaking.md).

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>(1) Multiplayer-Manager wird initialisiert. <a name="initialize-multiplayer-manager">

| Konferenz | Ereignis wurde ausgelöst |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | n. a. |

Das Sitzungsobjekt Lobby wird automatisch erstellt, beim Initialisieren des Multiplayer-Managers, vorausgesetzt, dass eine gültige Sitzungs-Vorlagenname (in der Dienstkonfiguration konfiguriert) angegeben wird. Beachten Sie, dass dies nicht die Lobby Sitzung-Instanz für den Dienst erstellt wird.

**Beispiel:**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

mpInstance->initialize(lobbySessionTemplateName);
```


### <a name="2-create-the-lobby-session-by-adding-local-usersa-namecreate-lobby"></a>(2) erstellen Sie die Lobby-Sitzung, indem Sie lokale Benutzer hinzufügen<a name="create-lobby">

| Methode | Ereignis wurde ausgelöst |
|-----|----------------|
| `multiplayer_lobby_session::add_local_user()` | `user_added_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Hier fügen den lokal angemeldeten Benutzer mit der Sitzung Lobby Xbox Live. Wenn der erste Benutzer hinzugefügt, wird eine neue Lobby gehostet. Für alle anderen Benutzer ist werden sie der vorhandenen Lobby als sekundäre Benutzer hinzugefügt werden. Diese API wird auch in der Shell für Freunde zum Beitritt Empfangsbereich ankündigen. Sie können Einladungen zu senden, Lobby Eigenschaften festlegen, Lobby-Member über lobby() zugreifen, nur verwendet werden, nachdem Sie den lokalen Benutzer hinzugefügt haben.

Nach dem Hinzufügen der Lobby empfiehlt Microsoft die Verbindungsadresse für das lokale Element sowie alle benutzerdefinierten Eigenschaften für das Element festlegen.

Sie müssen diesen Vorgang wiederholen, für alle lokalen Benutzer angemeldet.

**Beispiel: (einzelne lokaler Benutzer)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

auto result = mpInstance->lobby_session()->add_local_user(xboxLiveContext->user());
if (result.err())
{
  // handle error
}

// Set member connection address
string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

// Set custom member properties
mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
```

**Beispiel: (mehrere lokale Benutzer)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();
string_t connectionAddress = L"1.1.1.1";

for (User^ user : User::Users)
{
  if( user->IsSignedIn )
  {
    auto result = mpInstance.lobby_session()->add_local_user(user);
    if (result.err())
    {
      // handle error
    }

    // Set member connection address
    mpInstance->lobby_session()->set_local_member_connection_address(
        xboxLiveContext->user(), connectionAddress);

    // Set custom member properties
    mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
  }
}

```


Die Änderungen werden auf dem nächsten Batch `do_work()` aufrufen.  
Multiplayer-Manager wird eine `user_added` Ereignis jedes Mal ein Benutzer mit der Sitzung Lobby hinzugefügt. Es wird empfohlen, Sie untersuchen den Fehlercode des Ereignisses, um festzustellen, ob dieser Benutzer wurde erfolgreich hinzugefügt wurde. Im Fall eines Fehlers wird eine Fehlermeldung mit die Ursache des Fehlers angegeben werden.

**Funktionen von Multiplayer-Manager**

* Registrieren Sie Real-Time-Aktivität und Multiplayer-Abonnements beim Xbox Live Multiplayer-Dienst.
* Erstellen Sie Lobby-Sitzung
* Verknüpfen Sie alle lokalen Spieler als aktiv
* SDA hochladen
* Set-Elementeigenschaften
* Registrieren Sie sich für Ereignisse der Sitzung ändern
* Lobby Sitzung als aktive Sitzung festlegen

### <a name="3-send-invites-to-friends-optional-a-namesend-invites"></a>(3) senden Einladungen an Freunde (optional) <a name="send-invites">

| Methode | Ereignis wurde ausgelöst |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

Als Nächstes sollten Sie die standardmäßigen Xbox UI für Freunde einladen angezeigt. Dies zeigt eine Benutzeroberfläche, die der Player Freunde auswählen oder letzte Spieler auf dem Spiel einladen kann. Sobald die Player-Treffer bestätigen, sendet Multiplayer-Manager die Einladungen auf die ausgewählten Spieler.

Spiele können Sie auch die `invite_users()` Methode zum Senden von Einladungen, um eine Reihe von Personen, die durch ihre Xbox Live-Benutzer-ID definiert. Dies ist nützlich, wenn Sie eine eigene Benutzeroberfläche für in-Game-anstelle die Stock Xbox-UI verwenden möchten.

**Beispiel:**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**Funktionen von Multiplayer-Manager**

* Bringt Sie das Xbox Titel Stock aufrufbare UI (TCUI)
* Sendet die Einladung direkt auf die ausgewählten Spieler

### <a name="4-accept-invites-optional-a-nameaccept-invites"></a>(4) akzeptieren der Einladungen (optional) <a name="accept-invites">

| Methode | Ereignis wurde ausgelöst |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Wenn ein eingeladener-Player eine Spiele Einladung akzeptiert oder ein Spiel Freunde über eine Shell-Benutzeroberfläche eingebunden, wird das Spiel auf ihrem Gerät mithilfe von protokollaktivierung gestartet. Einmal das Spiel startet, Multiplayer-Manager die Ereignisargumente Protokoll aktiviert können um Empfangsbereich zu verknüpfen. Optional, falls Sie die lokalen Benutzer über lobby_session()::add_local_user() hinzugefügt haben, können Sie in der Liste der Benutzer über die join_lobby() API übergeben. Wenn der eingeladene Benutzer nicht über Lobby_session()::add_local_user() oder über join_lobby(), hinzugefügt wird und dann join_lobby() fehl und geben Sie die invited_xbox_user_id(), die die INVITE-Nachricht als Teil der join_lobby_completed_event_args() gesendet wurde, für die.

Nach dem Hinzufügen der Lobby empfiehlt Microsoft die Verbindungsadresse für das lokale Element sowie alle benutzerdefinierten Eigenschaften für das Element festlegen. Sie können auch den Host über Set_synchronized_host festlegen, wenn es nicht vorhanden ist.

Abschließend werden der Multiplayer-Manager Join der Benutzer in die Spiele-Sitzung automatisch, wenn es sich bei einem Spiel wird bereits ausgeführt und Platz für den eingeladenen. Der Titel werden über das Join_game_completed-Ereignis, das bereitgestellt wird, ein entsprechender Fehlercode und eine Meldung benachrichtigt.

**Beispiel:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

```

Fehler "oder" Erfolg erfolgt über die `join_lobby_completed` Ereignis

**Funktionen von Multiplayer-Manager**

* Registrieren von RTA & Multiplayer-Abonnements
* Lobby Sitzung beitreten
 * Vorhandene Lobby Status bereinigen
 * Verknüpfen Sie alle lokalen Spieler als aktiv
 * SDA hochladen
 * Set-Elementeigenschaften
* Registrieren Sie sich für Ereignisse der Sitzung ändern
* Lobby Sitzung als aktive Sitzung festlegen
* Spiel Sitzung beitreten (falls vorhanden)
 * Die Übertragung Handle verwendet


### <a name="5-find-match-a-namefind-match"></a>(5) Übereinstimmung gefunden <a name="find-match">

| Konferenz | Ereignis wurde ausgelöst |
|-----|----------------|
| `multiplayer_manager::find_match()` | Viele Ereignisse, wie in beschrieben ```match_status``` (z. B.: ```searching```, ```found```, ```measuring```usw.) |

Nachdem Einladungen, wenn vorhanden, angenommen wurden, und der Host ist mit der das Spiel zu spielen beginnen können SmartMatch entweder suchen, die ein existierendes Spiel, die genügend öffnen Spieler hat für alle Elemente in der Sitzung Lobby durch Aufrufen von Slots `find_match()`, oder erstellen Sie eine neue Spiele Se Sitzung, die alle Elemente aus dem Lobby Sitzung und füllen Sie offene Stellen mit anderen Personen, die für eine Übereinstimmung des gleichen Typs Spiele, durch den Aufruf enthält `join_game_from_lobby()` folgen `set_auto_fill_members_during_matchmaking()`.

Bevor Sie aufrufen können `find_match()`, müssen Sie zunächst die Hoppers konfigurieren, in der Dienstkonfiguration. Eine Hopper definiert die Regeln, die smartmatch verwendet wird, entsprechend der Spieler.

**Beispiel:**

```cpp
auto result = mpInstance.find_match(HOPPER_NAME);
if (result.err())
{
  // handle error
}
```

**Funktionen von Multiplayer-Manager**

* Erstellen Sie ein Ticket für die Übereinstimmung
* Behandeln Sie alle QoS-Stufen
* Die Liste bearbeitet
 * Erneutes Senden (falls erforderlich)
* Joins Ziel Game-Sitzung
* Spiel über Lobby-Sitzung ankündigen
