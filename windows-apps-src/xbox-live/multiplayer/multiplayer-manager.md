---
title: Multiplayer-Manager
description: Informationen Sie zu Multiplayer-Manager, eine allgemeine API macht es einfacher zu implementieren, Multiplayer-Xbox Live.
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 99159b5d126c671b07d37e20f1bcd61452c7d670
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594715"
---
# <a name="multiplayer-manager"></a>Multiplayer-Manager

Xbox Live bietet umfangreiche Unterstützung für das Hinzufügen von Multiplayer-Funktionalität zu Ihrem Titel, Ihr Spiel Xbox Live-Mitglieder, die auf der ganzen Welt eine Verbindung herstellen können.  Dies schließt die umfassenden Vermittlung Szenarien, die Möglichkeit, dass ein Player einen Freund-Spiel in Bearbeitung und vieles mehr zu verknüpfen. Mithilfe der Multiplayer-2015-APIs direkt implementieren Xbox Live Multiplayer-kann jedoch eine komplexe Aufgabe sein, dass ein hohes Maß an Entwurf und testen, um sicherzustellen, dass Sie die bewährten Methoden folgen zertifizierungsanforderungen erfüllen.

Multiplayer-Manager vereinfacht das Multiplayer-Funktionen Ihres Spiels durch die Verwaltung von Sitzungen und Vermittlung, und bietet einen Zustand hinzu, und ereignisbasierter Programmiermodell. Es ist ein Satz von APIs, macht es einfach, multiplayer-Szenarien für Ihre Xbox Live zu implementieren spielen. Es bietet es sich um eine API, die für allgemeine Multiplayer-Szenarien wie die Wiedergabe von Multiplayer-Spiele mit Freunden ausgerichtet wird, Behandlung Spielen eingeladen wird, behandeln, beteiligen Sie sich die Status- und Vermittlung und vieles mehr. Es unterstützt mehrere lokale Benutzer und erleichtert Ihre Titel in Multiplayer-Sitzungsverzeichnis integrieren, wenn Sie eine Drittanbieter-Vermittlungsdienst verwenden. Viele dieser Szenarios können mit nur wenigen API-Aufrufe erreicht werden.

## <a name="key-features"></a>Hauptmerkmale
Dies sind die wichtigsten Funktionen der Multiplayer-Manager-API:

* Einfache sitzungsverwaltung und Xbox Live Vermittlung
* Status und das Ereignis basiert Programmiermodell.
* Stellt sicher Xbox Live-Best Practices sowie Multiplayer-XR-kompatibel.
* Unterstützt sowohl für Xbox One XDK-als auch für UWP-Titel an.
* Implementiert [Multiplayer-2015 Flussdiagramme](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx).
* Funktioniert zusammen mit der herkömmlichen Multiplayer-2015-APIs.

>**Wichtige** -Ihres Spiels muss weiterhin die erforderlichen Ereignisse für online Multiplayer-Spiele implementieren, um zertifiziert wurde.

## <a name="overview"></a>Übersicht
Multiplayer-Manager ist in einigen grundlegenden Begriffen ausgerichtet:
* `lobby_session` : Eine permanente Sitzung, die zum Verwalten von Benutzern verwendet werden, die lokal auf diesem Gerät und Freunde eingeladen, die zusammen wiedergeben möchten. Die Gruppe kann mehrere rundet, Karten, Ebenen, usw., für das Spiel spielen und die Sitzung Lobby verfolgt diese Kerngruppe von Freunden (einschließlich der Spieler, die lokal auf dem Gerät). In der Regel wird diese Gruppe gebildet, während der Host durch die Menüs Navigation kann, chatten mit der Mitglieder entscheiden, welche Spielmodus sie wiedergeben möchten.

* `game_session` : Verfolgt-Playern, die eine bestimmte Instanz des Spiels spielen. Z. B. ein Race, Zuordnung oder Ebene. Sie können eine neue Spiele-Sitzung über erstellen `join_game_from_lobby` , die die Elemente in der Sitzung Lobby enthält.  Wenn ein Element eine Einladung akzeptiert, werden sie (sofern genügend Platz vorhanden ist) der Lobby und der game-Sitzung hinzugefügt werden. Zusätzliche Spieler können für die Spiele-Sitzung hinzugefügt werden, wenn Vermittlung aktiviert ist, aber diese zusätzliche Spieler werden nicht in der Sitzung Lobby eingefügt. Dies bedeutet, dass sobald das Spiel ist beendet, Spieler in der Sitzung Lobby weiterhin zusammen, während die zusätzlichen Spieler aus Vermittlung nicht der Fall.

* `multiplayer_member` : Stellt einen einzelnen Benutzer auf einem lokalen oder Remotecomputer Gerät angemeldet.

* `do_work` : Stellt sicher, ordnungsgemäße Spielzustands Updates zwischen den Titel und Xbox Live Multiplayer-Dienst verwaltet werden. Um optimale Leistung sicherzustellen, muss die do_work() häufig, wie z. B. einmal pro Frame aufgerufen werden. Sie erhalten eine Liste mit `multiplayer_event` Rückrufereignisse für das Spiel zu behandeln.

## <a name="state-machine"></a>Zustandsautomat
Die `do_work()` Aufruf ist erforderlich, um sicherzustellen, Ihren Zustand wird aktuell gehalten.  Für Multiplayer-Manager seine Arbeit erledigen, die Entwickler müssen Aufrufen der `do_work()` regelmäßig-Methode. Die zuverlässigste Möglichkeit hierzu ist mindestens ein Mal pro Frame aufgerufen. `do_work()` Wenn es keine weiteren Schritte erforderlich ist, hat, sodass es kein Grund zur Sorge besteht über sie zu häufig aufgerufen wird zurückgegeben schnell.

Alle Objekte, die von der Multiplayer-Manager-API zurückgegebenen sollte nicht threadsicher betrachtet werden. Allerdings bietet er Ihnen Steuerelement Synchronisierung Threads, wenn Sie es von mehreren Threads aus aufrufen. Die Bibliothek verfügt über einen internen multithreading Schutz, aber Sie müssen dennoch implementieren Ihre eigenen Sperren, wenn Sie einen Thread den Zugriff auf alle Werte – z. B. benötigen die members() Liste durchlaufen, während ein anderer Thread aufgerufen werden kann `do_work()`.

Multiplayer-Manager verwaltet ein statusbasierte Modell die Sitzungen im Hintergrund aktualisiert, wie Spieler Join-, lassen oder werden, wenn Sitzungen aktualisiert werden. Um dabei zu helfen, Probleme der Threadsynchronisierung zu zwischen der UI-Thread und Ihr Spiel Thread zu vermeiden, Multiplayer-Manager wird nicht aktualisiert den sichtbare Zustand der app-Sitzungen bis zum Aufruf der `do_work()` Methode. Normalerweise würden Sie empfangen von Benachrichtigungen zu Ereignisse wie Änderungen der Sitzung in einem Hintergrundthread und müssen Sie es mit einem UI-Thread zum Anzeigen dieser Änderungen zu synchronisieren. Mit Multiplayer-Manager wird diese Aufgabe für Sie hinter den Kulissen verwendet.  Rufen Sie `do_work()` auf den Hauptthread zum Zeitpunkt Ihrer Wahl, um der letzten Momentaufnahme des Zustands der Multiplayer erhalten Manager das Puffern für Sie hinter den Kulissen.

## <a name="events-and-notifications"></a>Ereignisse und Benachrichtigungen
Multiplayer-Manager definiert eine Reihe von wichtigen Ereignissen (finden Sie unter `multiplayer_event_type`) und benachrichtigt den Titel, über die `do_work()` Methode, wenn sie auftreten. Ereignisse, z. B. remote Spieler beitreten oder diese verlassen, Elementeigenschaften, die ändern, oder der Sitzungszustand ändern. Alle Multiplayer-Manager-APIs sind synchron. Die `do_work()` Methode gibt eine Liste der Ereignisse aus, wenn diese asynchronen Vorgänge abgeschlossen sind. Titel des sollten diese Ereignisse behandeln, Angaben je nach Bedarf. Informieren Sie sich die `multiplayer_event` Klasse Dokumentation.

Für jedes Ereignis, je nach Ereignistyp müssen Sie eine Umwandlung der `event_args` auf die entsprechende Ereignis Arg-Klasse, um die Ereigniseigenschaften zu erhalten. Das folgende Beispiel veranschaulicht die Verwendung `do_work()` zum Behandeln von Ereignissen:

```cpp
auto eventQueue = mpInstance.do_work();
for (auto& event : eventQueue)
{
    switch (event.event_type())
    {
      case multiplayer_event_type::member_joined:
      {
        auto memberJoinedArgs =  std::dynamic_pointer_cast<member_joined_event_args>(event.event_args());
        HandleMemberJoined(memberJoinedArgs);
        break;
      }
      case multiplayer_event_type::session_property_changed:
      {
        auto sessionPropertyChangedArgs =  std::dynamic_pointer_cast<session_property_changed_event_args>(event.event_args());
        HandlePropertiesChanged(sessionPropertyChangedArgs);
        break;
      }
      . . .
    }
}

```

## <a name="scenarios"></a>Szenarien

In diesem Abschnitt erläutern wir einige häufige Szenarien sowie die APIs, die Sie in jedem Szenario aufgerufen werden musste.  Einige Informationen an, was hinter den Kulissen Multiplayer-Manager ausführt, wird ebenfalls bereitgestellt.

* [Experimentieren Sie mit Freunden](multiplayer-manager/play-multiplayer-with-friends.md)
* [Keine Übereinstimmung gefunden](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [Spiele-Einladungen senden](multiplayer-manager/send-game-invites.md)
* [Behandeln von protokollaktivierung](multiplayer-manager/handle-protocol-activation.md)

Ein allgemeinen Überblick über die API finden Sie unter [Multiplayer-Manager-API-Übersicht](multiplayer-manager/multiplayer-manager-api-overview.md).

## <a name="what-multiplayer-manager-does-not-do"></a>Was Multiplayer-Manager nicht tun wird
Multiplayer-Manager ist es viel einfacher, multiplayer-Szenarien zu implementieren und einige der Daten vom Entwickler abstrahiert, gibt es einige Dinge, die sie nicht behandelt, die oder ist nicht für am besten geeignet.

* Persistente Onlineserver Spiele, z. B. MMOs oder anderen Spielen-Typen, die große Sitzungen (mehr als 100 Spieler in einer Sitzung) erforderlich sind.
* Server in Server-sitzungsverwaltung

>Multiplayer-Manager ist nicht an eine beliebige Technologie bestimmten Netzwerk gebunden und können auf jeder Ebene des Netzwerks.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich die C++- oder WinRT *Multiplayer* ein funktionsfähiges Beispiel für die API-Beispiel.

Die API-Dokumentation finden Sie in den C++- oder WinRT-Handbüchern im Microsoft::Xbox::Services::Multiplayer::Manager-Namespace.  Sie sehen auch die `multiplayer_manager.h` Header.

Wenn Sie, Feedback Fragen oder mithilfe des Multiplayer-Managers Probleme auftreten, wenden Sie sich an Ihre Mutter, oder Posten Sie einen Unterstützungsthread in den Foren unter [ https://forums.xboxlive.com ](https://forums.xboxlive.com).
