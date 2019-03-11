---
title: Multiplayer-Manager-API-Übersicht
description: Erfahren Sie mehr über die Xbox Live-Multiplayer-Manager-API.
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Spiele, Multiplayer-Manager
ms.localizationpriority: medium
ms.openlocfilehash: 7838de6845bc6c49acf649c0e859cd0d7020490f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628025"
---
# <a name="multiplayer-manager-api-overview"></a>Multiplayer-Manager-API – Übersicht

Diese Seite beschreibt einen umfassenden Überblick über die Multiplayer-Manager-API und deren Verwendung in einem Spiel. Sie ruft die wichtigsten Klassen und Methoden in der API. Weitere API-Informationen finden Sie die Referenzdokumentation. Beispiele für diese APIs in einer Anwendung verwenden finden Sie im Multiplayer-Beispiel.

## <a name="namespace"></a>Namespace
Die Multiplayer-Manager-Klassen befinden sich die folgenden Namespace:

| language | Namespace |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

Gibt es beschreibt folgende Listen die wichtigsten Klassen, die Sie kennen sollten:

* [Multiplayer-Manager-Klasse](#multiplayer-manager-class)
* [Multiplayer-Ereignisklasse](#multiplayer-event-class)
* [Multiplayer-Member-Klasse](#multiplayer-member-class)
* [Multiplayer-Lobby Sitzungsklasse](#multiplayer-lobby-session-class)
* [Multiplayer-Spiels Sitzungsklasse](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>Multiplayer-Manager-Klasse <a name="multiplayer-manager-class">

| language | Klasse |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

Diese Klasse ist die primäre Klasse für die Interaktion mit Multiplayer-Manager. Es ist eine Singleton-Klasse, daher müssen Sie nur zum Erstellen und initialisieren einmal die Klasse.
Diese Klasse enthält eine einzelne Lobby Session-Objekt und ein einzelnes Spiel Session-Objekt.

Sie müssen mindestens Aufrufen der `initialize()` und `do_work()` Methoden in dieser Klasse in der Reihenfolge für Multiplayer-Manager-Funktion.

In den folgenden Tabellen werden einige, aber nicht alle, desto mehr häufig Methoden und Eigenschaften für diese Klasse verwendet. Eine vollständige Liste von Klassenmembern finden Sie in der Referenz.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Methoden** | | |
| Initialize() | Initialize() | Multiplayer-Manager wird initialisiert. Sie müssen diese Methode aufrufen, bevor mit Multiplayer-Manager. |
| DoWork() | do_work() | Aktualisiert den Sitzungsstatus der app angezeigt. Sie sollten diese Methode sich mindestens einmal pro Frame aufrufen und Ihr Spiel sollte die Multiplayer-Ereignisse behandelt, die von der-Methode zurückgegeben werden. |
| JoinLobby() | join_lobby() | Bietet eine Möglichkeit, Joins eine Friend Lobby Sitzung entweder über ein, die eindeutig Empfangsbereich HandleId einbinden möchten oder wenn der Benutzer eine Einladung, die bewirkt, dass den Titel zum Abrufen von Protokoll aktiviert akzeptiert. |
| JoinGameFromLobby() | join_game_from_lobby() | Nehmen Sie Empfangsbereich des Spiels-Sitzung Teil, wenn eine solche vorhanden ist und genügend Platz vorhanden ist. Wenn die Sitzung nicht vorhanden ist, wird eine neue Sitzung für die Spiele mit den vorhandenen Lobby Mitgliedern. Dadurch werden keine vorhandenen Lobby Sitzungseigenschaften über der game-Sitzung migriert. Nach der Verknüpfung können Sie festlegen, die Eigenschaften oder den Host über `game_session()::set_synchronized_*` APIs. Der Titel ist erforderlich, um diese API auf allen Clients aufzurufen, die das Spiel beitreten möchten.|
| JoinGame() | join_game() | Verknüpft eine vorhandene Spiele Sitzung erhalten einen global eindeutigen Sitzungsnamen, finden Sie in der Regel durch die Vermittlung Diensten von Drittanbietern. Sie können in der Liste der Xbox-Benutzer-IDs übergeben, die Sie Teils des Spiels werden möchten.|
| FindMatch() | find_match() | Verwenden Sie zum Suchen und verknüpfen ein Spiel Vermittlung Xbox Live. |
| LeaveGame() | leave_game() | Bewirkt, dass ein Spiel, und gibt das Element und alle lokalen Elemente Empfangsbereich zurück. |
| **Eigenschaften** | | |
| LobbySession | lobby_session() | Ein Handle für ein Objekt, das die Sitzung Lobby darstellt. |
| GameSession |  game_session() | Ein Handle für ein Objekt, die Spiele Sitzung darstellt. |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>Multiplayer-Ereignisklasse <a name="multiplayer-event-class">

| language | Klasse |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

Beim Aufruf `do_work()`, Multiplayer-Manager gibt eine Liste der Ereignisse, die die Änderungen an die Sitzungen darstellen, da Sie dem letzten Aufruf `do_work()`. Diesen Ereignissen gehören Änderungen an, wie z. B. ein Member eine Sitzung beigetreten ist, ein Member eine-Sitzung verlassen hat, Elementeigenschaften wurden geändert, die Host-Client hat sich geändert, usw.

Eine Liste aller möglichen Ereignistypen, finden Sie unter den `multiplayer_event_type` Enumeration.

Jede zurückgegebene `multiplayer_event` enthält ein `event_args`, das Sie in der entsprechenden Event_args-Klasse für den Ereignistyp umwandeln müssen. Z. B. wenn die `event_type` ist `member_joined`, dann würden Sie eine Umwandlung der `event_args` Verweis auf eine `member_joined_event_args` Klasse.

Ihr Spiel sollte jedes der Ereignisse bei Bedarf nach dem Aufruf behandeln `do_work()`.

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>Multiplayer-Member-Klasse <a name="multiplayer-member-class">

| language | Klasse |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

Diese Klasse stellt einen Player in eine Lobby oder Spiele-Sitzung. Die Klasse enthält Eigenschaften, die über den Member, einschließlich der XboxUserID für den Player, die Netzwerkadresse für die Verbindung für den Spieler, benutzerdefinierte Eigenschaften für jeden Spieler und vieles mehr.

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>Multiplayer-Lobby Sitzungsklasse <a name="multiplayer-lobby-session-class">

| language | Klasse |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

Eine permanente Sitzung, die zum Verwalten von Benutzern verwendet werden, die lokal auf diesem Gerät und Freunde eingeladen, die zusammen wiedergeben möchten. Eine Sitzung Lobby muss mindestens ein Mitglied in der Reihenfolge für Multiplayer-Manager, um Multiplayer-Aktionen enthalten. Sie können zunächst eine neue Lobby-Sitzung erstellen, durch Aufrufen der `add_local_user()` Methode.

In den folgenden Tabellen werden einige, aber nicht alle, desto mehr häufig Methoden und Eigenschaften für diese Klasse verwendet. Eine vollständige Liste von Klassenmembern finden Sie in der Referenz.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Methoden** | | |
| AddLocalUser() | add_local_user() | Fügt einen lokalen Benutzer (einen Player, die sich auf dem lokalen Gerät angemeldet ist) die Lobby Sitzung hinzu. Ist dies das erste Element, das eine Sitzung Lobby hinzugefügt, erstellt dann eine neue Lobby-Sitzung. |
| RemoveLocalUser() | remove_local_user() | Entfernt ein angegebenes Element aus dem Lobby und Spiele-Sitzung. |
| InviteFriends() | invite_friends() | Öffnet eine standardmäßige Xbox Live-Benutzeroberfläche, die den Spieler Personen aus der Freundesliste auswählen können, und lädt dann die Spieler, um das Spiel. |
| InviteUsers() | invite_users() | Einladungen, die angegebene Xbox Live-Benutzer auf das Spiel. |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | Legt die Netzwerkadresse für das lokale Element fest. Diese Netzwerkadresse können Spiele die Netzwerkkommunikation zwischen Elementen herzustellen. |
| SetLocalMemberProperties() | set_local_member_properties() | Legt eine benutzerdefinierte Eigenschaft für ein lokales Element fest. Die Eigenschaft wird in einer JSON-Zeichenfolge gespeichert. |
| DeleteLocalMemberProperties() | delete_local_member_properties() | Entfernt eine benutzerdefinierte Eigenschaft für ein lokales Element. |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Legt eine benutzerdefinierte Eigenschaft für die Sitzung Lobby fest. Die Eigenschaft wird in einer JSON-Zeichenfolge gespeichert. Wenn die Eigenschaft zwischen den Geräten gemeinsam genutzt wird und über mehrere Geräte gleichzeitig aktualisiert werden kann, verwenden Sie die synchronisierte Version der Methode. |
| IsHost() | is_host() | Gibt an, wenn das aktuelle Gerät als Lobby Host fungiert. |
| SetSynchronizedHost() | set_synchronized_host() | Legt den Host des Empfangsbereich fest. |
| **Eigenschaften** | | |
| LocalMembers | local_members() | Die Auflistung der Elemente, die auf dem lokalen Gerät angemeldet sind. |
| Mitglieder | members() | Die Auflistung der Elemente, die in der Sitzung Lobby sind. |
| Eigenschaften | properties() | Ein jsonobjekt, das eine Auflistung von Eigenschaften für die Sitzung Lobby darstellt. |
| Host | host() | Der Host-Member für Empfangsbereich. |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>Multiplayer-Spiels Sitzungsklasse <a name="multiplayer-game-session-class">

| language | Klasse |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

Die Spiele-Sitzung stellt die Gruppe von Xbox Live-Elementen, die in einer Instanz von tatsächlichen Spiels berücksichtigt werden. Dies kann Spieler einschließen, die sich über Vermittlung Dienste zugeordnet wurde.

Um eine neue Spiele-Sitzung zu starten, die Mitglieder von enthält die `lobby_session`, rufen Sie `multiplayer_manager::join_game_from_lobby()`. Wenn Sie die Vermittlung Xbox Live verwenden möchten, können Sie aufrufen `multiplayer_manager::find_match()`. Wenn Sie eine Drittanbieter-Vermittlungsdienst verwenden, können Sie aufrufen `multiplayer_manager::join_game()`.

In den folgenden Tabellen werden einige, aber nicht alle, desto mehr häufig Methoden und Eigenschaften für diese Klasse verwendet. Eine vollständige Liste von Klassenmembern finden Sie in der Referenz.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Methoden** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Legt eine benutzerdefinierte Eigenschaft für die Spiele-Sitzung fest. Die Eigenschaft wird in einer JSON-Zeichenfolge gespeichert. Wenn die Eigenschaft zwischen den Geräten gemeinsam genutzt wird und über mehrere Geräte gleichzeitig aktualisiert werden kann, verwenden Sie die synchronisierte Version der Methode. |
| IsHost() | is_host() | Gibt an, wenn das aktuelle Gerät als dem Spiel Host fungiert. |
| SetSynchronizedHost() | set_synchronized_host() | Legt fest, den Host des Spiels. |
| **Eigenschaften** | | |
| Mitglieder | members() | Die Auflistung der Elemente, die in der game-Sitzung sind. |
| Eigenschaften | properties() | Ein JSON-Objekt, das eine Auflistung von Eigenschaften für die Spiele-Sitzung darstellt. |
| Host | host() | Der Host-Member für das Spiel. |
