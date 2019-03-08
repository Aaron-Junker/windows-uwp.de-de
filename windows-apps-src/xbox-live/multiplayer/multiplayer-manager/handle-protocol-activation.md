---
title: Behandeln der Protokollaktivierung
description: Erfahren Sie, wie Xbox Live-Multiplayer-Manager verwenden, um protokollaktivierung zu behandeln.
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Manager, protokollaktivierung
ms.localizationpriority: medium
ms.openlocfilehash: 0b5dead742e18bbf5f3e9c271109352ae48e8fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655835"
---
# <a name="handle-protocol-activation"></a>Behandeln der Protokollaktivierung

Es ist ein protokollaktivierung auf, wenn das System automatisch ein Spiel in Reaktion auf eine andere Aktion in der Regel gestartet, wenn ein Player eine Spiele Einladung über einen anderen Player akzeptiert.

Titel des kann Protokoll aktiviert werden, auf folgende Weise abrufen:

* Wenn ein Benutzer eine Spiele-Einladung akzeptiert
* Wenn ein Benutzer "Game verknüpfen" aus des Spielers-Spielerkarte auswählt.

Dieses Szenario wird beschrieben, wie die protokollaktivierung behandelt wird, wenn der Titel des gestartet wird, und verknüpfen die Lobby und -Spiel (falls vorhanden).

Sie können ein Flussdiagramm des Prozesses hier anzeigen: [Flussdiagramm – Handle Protokoll Aktivierung Player](mpm-flowcharts/mpm-on-protocol-activation.md).

| Methode | Ereignis wurde ausgelöst |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Wenn ein Player eine Spiele-Einladung akzeptiert oder einen Freund-Spiel mithilfe des Spielers-Spielerkarte verknüpft, wird das Spiel auf ihrem Gerät mithilfe von protokollaktivierung gestartet. Einmal das Spiel startet, Multiplayer-Manager die Ereignisargumente Protokoll aktiviert können um Empfangsbereich zu verknüpfen. Optional, wenn Sie noch nicht über die lokalen Benutzer hinzugefügt `lobby_session()::add_local_user()`, können Sie in der Liste der Benutzer über übergeben die `join_lobby()` API. Wenn der eingeladene Benutzer nicht hinzugefügt wird oder wenn die INVITE-Nachricht für einen anderen Benutzer als der Benutzer war, die hinzugefügt wurde, `join_lobby()` fehl und geben Sie die `invited_xbox_user_id()` , die die Einladung wurde gesendet für als Teil der `join_lobby_completed_event_args`.

Nach dem Hinzufügen der Lobby empfiehlt Microsoft die Verbindungsadresse für das lokale Element sowie alle benutzerdefinierten Eigenschaften für das Element festlegen. Sie können auch festlegen, den Host über `set_synchronized_host` , wenn es nicht vorhanden ist.

Abschließend werden der Multiplayer-Manager Join der Benutzer in die Spiele-Sitzung automatisch, wenn es sich bei einem Spiel wird bereits ausgeführt und Platz für den eingeladenen. Titel werden benachrichtigt, über die `join_game_completed` Ereignis bereitstellen und ein entsprechender Fehlercode und die Nachricht.

**Beispiel:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args, users);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);
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
