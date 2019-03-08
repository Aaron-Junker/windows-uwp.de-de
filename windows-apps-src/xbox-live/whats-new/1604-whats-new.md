---
title: Neuerungen für die Xbox Live SDK – April 2016
description: Neuerungen für die Xbox Live SDK – April 2016
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 9ce63a0174fa0c4158764b8bca2443d58d0aefd9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660765"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>Neuerungen für die Xbox Live SDK – April 2016

Informieren Sie sich die [– März 2016 Neuigkeiten](1603-whats-new.md) Artikel was in 1603 hinzugefügt wurde

## <a name="os-and-tool-support"></a>Unterstützung für Betriebssystem und Tools
Das Xbox Live-SDK unterstützt Windows 10 RTM [Version 10.0.10240] und Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="tournaments"></a>Turniere
- Das Xbox Live Turniere-Tool ist nun im SDK enthalten.  Sehen Sie es in das Toolverzeichnis, zusammen mit Informationen über das sie verwenden.
- Die APIs für Turniere sind auch verfügbar.  Finden Sie unter den Xbox::Services::Tournaments-namespace
- Dokumentation ist auch im Programmierhandbuch zur Verfügung.

## <a name="documentation"></a>Dokumentation
- [Melden Sie sich – Handbuch zur Problembehandlung](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md) Listen einige allgemeinen Strategien zum Debuggen von fehlgeschlagenen Anmeldungen, sowie Schritte zu befolgen basierend auf Fehlercode.
- Die [Marketplace](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content) Dokumente für Xbox One-Entwickler nur jetzt finden Sie in das Programmierhandbuch.  UWP-Entwickler sollten weiterhin in Partner Center-Dokumentation im Store zu informieren.
- Gibt es eine [xdk-Version zum UWP Portieren Anleitung](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) sehen Sie sich, wenn Sie einen Xbox One Titel in die universelle Windows-Plattform anbieten möchten.
- Finden Sie unter den [Fine-grained Ratenlimit](../using-xbox-live/best-practices/fine-grained-rate-limiting.md) Artikel eine Beschreibung wie diese für die verschiedenen Xbox Live-Dienst-Endpunkte und Szenarien, sowie Informationen dazu, was die Grenzwerte sind erzwungen werden.

## <a name="multiplayer-manager"></a>Multiplayer-Manager
Die [Multiplayer-Manager](../multiplayer/multiplayer-manager.md) ist nicht mehr in der experimentellen.  Wir haben Feedback von Entwicklern, die mit dieser API integriert und vorgenommen, einige der APIs mehr Konsistenz mit anderen.  Verwenden Sie den Multiplayer-Manager als Ausgangspunkt bei Ihrer Multiplayer-Entwicklung verwenden, da es sich um eine einfachere API bietet, der Großteil der Komplexität der 2015 Multiplayer-API für Sie verwaltet.

In den folgenden Abschnitten, haben wir einige der neuen Features für die API als auch eine kleine Anzahl von Änderungen aufgeführt.

#### <a name="completed-events"></a>Abgeschlossene Ereignisse
Alle APIs verfügen nun über eine entsprechende``` _competed``` Ereignis- und alle Ereignisse werden unabhängig vom Erfolg oder Fehler ausgelöst. Das vorherige Verhalten war, dass er nur bei einem Fehler für den Titel zu ergreifenden Maßnahmen ausgelöst. Und da Aufrufe in Batches enthalten sind, dies bedeutet, dass nach Abschluss des Vorgangs der Titel mehrere erhält ```_competed``` Ereignisse.

| API | Ereignis zurückgegeben |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

Das gleiche gilt für ```game_session()```.

#### <a name="application-defined-context"></a>Definierte Anwendungskontext
Sie können jetzt in einem optionalen anwendungsdefinierte Kontext für jede Set_ *-Methoden zum Korrelieren der Multiplayer-Ereignis an den Aufruf initiierenden übergeben.
Zum Beispiel

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

Kontext zurückgegeben würden dann zurück auf den Titel durch die ```_completed``` Ereignis.

#### <a name="breaking-changes"></a>Wichtige Änderungen

1.  Laden Sie beide APIs (```invite_friends``` & ```invite_users```) sind jetzt synchron. Nach Abschluss des Vorgangs wird ein Ereignis Invite_sent zurückgegeben.

2.  ```write_synchronized_properties_and_commit``` wurde umbenannt in ```set_synchronized_properties```. Nach Abschluss des Vorgangs wird eine ```session_synchronized_property_write_completed``` Ereignis.

3.  ```write_synchronized_host_and_commit``` wurde umbenannt in ```set_synchronized_host```. Nach Abschluss des Vorgangs wird eine ```synchronized_host_write_completed``` Ereignis.

4.  Auf ```lobby_session()```

  *Entfernt*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *Hinzugefügt*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  Auf ```game_session()```

  *Entfernt*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *Hinzugefügt*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  Ändern Sie unter den Ereignistypen:

  *Entfernt*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *Umbenannt*

  ```write_synchronized_properties_completed``` umbenannt in ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` umbenannt in ```synchronized_host_write_completed```
