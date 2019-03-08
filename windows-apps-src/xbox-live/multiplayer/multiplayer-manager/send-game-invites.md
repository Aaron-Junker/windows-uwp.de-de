---
title: Senden von Spieleinladungen
description: Erfahren Sie, wie Xbox Live-Multiplayer-Manager verwenden, um ein Spiele Einladungen senden Player zu ermöglichen.
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Spiele, Multiplayer-Manager "," Flussdiagramm "," spielen einladen
ms.localizationpriority: medium
ms.openlocfilehash: 85aa45558d1443638ba7dd50dbea8923125ef664
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645995"
---
# <a name="send-game-invites"></a>Senden von Spieleinladungen

Eine einfachere Multiplayer-Szenarien ist ein Spieler online Ihr Spiel mit Freunden spielen können. Dieses Szenario deckt die Schritte zum Senden von Einladungen an einen anderen Player, um Ihr Spiel zu verknüpfen.

Sobald Sie haben [initialisiert den Multiplayer-Manager](play-multiplayer-with-friends.md), und [erstellt eine Sitzung Lobby durch Hinzufügen von lokale Benutzer](play-multiplayer-with-friends.md), Sie müssen warten, bis Sie empfangen die `user_added` Ereignis aus, bevor Sie beginnen können, Einladungen zu senden.

Es gibt zwei Möglichkeiten, eine Einladung zu senden:

1. [Xbox-Plattform Einladung TCUI](#xbox-platform-invite-tcui)
2. [Titel implementiert die benutzerdefinierte Benutzeroberfläche](#title-implemented-custom-ui)

Sie können ein Flussdiagramm des Prozesses hier anzeigen: [Flussdiagramm – Send-Einladung an einen anderen Player](mpm-flowcharts/mpm-send-invites.md).

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>(1) Xbox-Plattform Einladung TCUI <a name="xbox-platform-invite-tcui">

| Methode | Ereignis wurde ausgelöst |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

Aufrufen von `invite_friends()` gelangen die standardmäßigen Xbox-UI für Freunde einladen. Dies zeigt eine Benutzeroberfläche, die der Player Freunde auswählen oder letzte Spieler auf dem Spiel einladen kann. Sobald die Player-Treffer bestätigen, sendet Multiplayer-Manager die Einladungen auf die ausgewählten Spieler.

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

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>(2) Titel implementiert die benutzerdefinierte Benutzeroberfläche<a name="title-implemented-custom-ui">

| Methode | Ereignis wurde ausgelöst |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

Titel des kann eine benutzerdefinierte TCUI implementieren, für die online-Freunde anzeigen und einladen. Spiele können die `invite_users()` Methode zum Senden von Einladungen, um eine Reihe von Personen, die durch ihre Xbox Live-Benutzer-ID definiert. Dies ist nützlich, wenn Sie eine eigene Benutzeroberfläche für in-Game-anstelle die Stock Xbox-UI verwenden möchten.

**Beispiel:**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**Funktionen von Multiplayer-Manager**

* Sendet die Einladung direkt auf die ausgewählten Spieler
