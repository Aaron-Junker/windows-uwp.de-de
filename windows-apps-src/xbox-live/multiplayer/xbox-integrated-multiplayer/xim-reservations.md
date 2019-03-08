---
title: XIM Out-of-Band-Reservierungen
description: Beschreibt, wie Xbox integrierte Multiplayer-Spiele (XIM) als Lösung für dedizierte Chat über Out-of-Band-Reservierungen verwenden.
ms.assetid: 0ed26d19-defb-414d-a414-c4877bd0ed37
ms.date: 01/28/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Xbox integrierte Multiplayer-Spiele, Xim, chat
ms.localizationpriority: medium
ms.openlocfilehash: 82b38b8d0d94e4cccf501a101faee0e28a6b43c0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660835"
---
# <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>Verwenden von XIM als dedizierte Chatlösung über Out-of-Band-Reservierungen

Die meisten Apps verwenden XIM, um jeden Aspekt der Zusammenführung der Spieler zu verarbeiten. Bei „Xbox Integrated Multiplayer“ liegt der Schwerpunkt nicht zuletzt auf der Unterstützung allgemeiner End-to-End-Multiplayerszenarien. Einige apps, die ihre eigenen Multiplayer-Lösungen mithilfe von dedizierten Internetserver implementieren möchte jedoch auch die Vorteile der Einrichtung zuverlässigen, niedrige Latenz und kostengünstige Chat für Peer-zu-Peer-Kommunikation. XIM erkennt diese Anforderung und unterstützt einen Erweiterung-Modus nutzt XIMs vereinfachte Peer-Kommunikation erweitern, externe Player-Verwaltung, die außerhalb der XIM-API erfolgt. Anstatt in einem Netzwerk XIM über soziale Netzwerke bedeutet, dass oder Vermittlung, Spieler verschieben "Reservierungen", ausgetauscht garantierte Platzhalter für bestimmte Benutzer, die "Out-of-Band" über die app externe Player Rendezvous-Mechanismus.

Abgesehen von der Migration sind XIM-Netzwerke, die mithilfe von Out-of-Band-Reservierungen verwaltet wie jeder andere XIM identisch. Alle Kommunikationsfunktionen funktionieren identischer Weise. Allerdings sind Vermittlung und social-API-Ermittlungsmethoden unbedingt deaktiviert für XIM-Netzwerke, die mithilfe von Out-of-Band-Reservierungen, da sie mit der der app-externe-Implementierung in Konflikt stehen würde verwaltet. Sie können keine Einladungen von einem solchen XIM Netzwerk, z. B. senden.

>XIM ist optimiert, um eine einfache End-to-End-Lösung bereitzustellen. Daher möglicherweise nicht in allen komplexen Topologien und Szenarios perfekt für die Out-of-Band-Reservierungen. Wenn Sie Fragen, ob oder wie Sie XIMs Kommunikationsfunktionen nutzen haben, wenden Sie sich an Ihren Ansprechpartner bei Microsoft.

In den nachfolgenden Themen wird beschrieben, wie Out-of-Band-Reservierungen in XIM nutzen. Aufgrund der relativ wenige Unterschiede von "standard" XIM-Nutzung, die in vorherigen Abschnitten beschriebenen wird erörtert, abgekürzt. Vertrautheit mit [mithilfe XIM](using-xim.md) wird empfohlen.


## <a name="moving-to-a-new-out-of-band-reservation-xim-network"></a>Mit einem neuen Out-of-Band-Reservierung XIM Netzwerk verschieben

Um zu beginnen, mit der Out-of-Band-Reservierungen, muss eine der die erfassten Teilnehmer in ein neues XIM-Netzwerk, das in diesem Modus erstellt verschieben. Die Auswahl, welche Beteiligten, die Peer Gerät Ihnen überlassen ist. Möglicherweise verfügen Sie bereits ein Konzept von Spiele-Host oder Server, die eine gute Wahl zum Starten des Prozesses ist, aber dies ist nicht erforderlich. Es wird empfohlen, ein Gerät, das einen "Öffnen" Netzwerk-Zugriffstyp der schnellsten Verbindung Einrichtung erzielen meldet auswählen. Finden Sie unter den `Windows::Networking::XboxLive` Dokumentation zur Plattform für Weitere Informationen.

Wechsel zu einem XIM Netzwerk verwaltet wird, über die Out-of-Band-Reservierungen geschieht, indem XIM initialisieren, und deklarieren die gewünschten lokalen Xbox-Benutzer-IDs ein, wie in der exemplarischen Vorgehensweise Verwendung von standard XIM, aber statt eine Methode wie `xim::move_to_new_network()`, rufen Sie `xim::move_to_network_using_out_of_band_reservation()` mit einer Reservierung von NULL-Zeichenfolge. Zum Beispiel:

```cpp
 xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
 xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
 xim::singleton_instance().move_to_network_using_out_of_band_reservation(nullptr);
```

Der Standard `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, und `xim_move_to_network_succeeded_state_change` dann erfolgt im Laufe der Zeit bei der Verarbeitung der Änderungen des Ansichtszustands in der typischen `xim::start_processing_state_changes()` und `xim::finish_processing_state_changes()` Schleife. Zum Beispiel:

```cpp
 uint32_t stateChangeCount;
 xim_state_change_array stateChanges;
 xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
 for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
 {
     const xim_state_change * stateChange = stateChanges[stateChangeIndex];
     switch (stateChange->state_change_type)
     {
         case xim_state_change_type::player_joined:
         {
             MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change *>(stateChange));
             break;
         }

         case xim_state_change_type::player_left:
         {
             MyHandlePlayerLeft(static_cast<const xim_player_left_state_change *>(stateChange));*
             break;
         }

         ...
     }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Nach der vom erste Gerät verarbeitet die Änderungen des Ansichtszustands und hatte die Spieler erfolgreich verschoben wurde mit dem Netzwerk XIM Reservierungen für die zusätzliche Benutzer erstellen müssen.

## <a name="adding-players-to-a-xim-network-managed-using-out-of-band-reservations"></a>Hinzufügen von Spieler mit einem Netzwerk XIM verwaltet mithilfe der Out-of-Band-Reservierungen

XIM-Netzwerke, die mithilfe von Out-of-Band-Reservierungen immer verwaltet werden, melden den Wert `xim_allowed_player_joins::out_of_band_reservation` aus der `xim::allowed_player_joins()` Methode; sie sind für alle Spieler mit Ausnahme derjenigen mit reservierten Spots für ihre Xbox-Benutzer-IDs durch Aufrufen von geschlossen `xim::create_out_of_band_reservation()`. `xim::create_out_of_band_reservation()` ein Array von Benutzern akzeptiert, damit Sie diese Reservierungen für Ihre Spieler, extern erfasst alle gleichzeitig oder im Laufe der Zeit erstellen können. Darüber hinaus sind Benutzer, die bereits Player im Netzwerk XIM Teilnahme ignoriert, sodass Sie auch zusätzliche Xbox-Benutzer-IDs als vollständige Ersetzungssatz bereitstellen können oder als Delta geändert wird, je nachdem, was praktisch ist. Im folgende Beispiel wird davon ausgegangen, dass Sie bereits vollständig erfassten Gruppe von Benutzer-IDs von Xbox-Zeichenfolgenzeiger in ein Array variabler "XboxUserIds" haben mit "XboxUserIdCount" Anzahl der Elemente:

```cpp
 xim::singleton_instance().create_out_of_band_reservation(xboxUserIdCount, xboxUserIds);
```

Dies startet den asynchronen Prozess zum Erstellen einer Reservierungen für den angegebenen Benutzer-IDs von Xbox. Wenn der Vorgang abgeschlossen ist, XIM wird bereitgestellt. eine `xim_create_out_of_band_reservation_completed_state_change` , meldet Erfolg oder Fehler. Bei Erfolg wird eine Reservierung Zeichenfolge verfügbar, für Ihr System Diese Xbox-Benutzer-IDs bereitgestellt, um den Vorgang zu gemacht werden. Reservierung-Zeichenfolgen, die erfolgreich erstellt sind nur einem bestimmten Zeitraum gültig. Innerhalb dieser Zeit zurückgegeben `xim_create_out_of_band_reservation_completed_state_change`.

Eine Zeichenfolge gültige Reservierung kann Ihre externe "Out-of-Band"-Mechanismus verwendet, um die Spieler außerhalb XIM erfassen verteilen Sie die Zeichenfolge, die diese aufgelisteten verwendet werden. Z. B. Wenn Sie den Dienst Multiplayer-Sitzung-Verzeichnis (MPSD), Xbox Live verwenden, können Sie schreiben diese Zeichenfolge als eine benutzerdefinierte gemeinsamen Sitzung-Eigenschaft in der Sitzung-Dokument (Hinweis: die reservierte Zeichenfolge wird immer nur eine begrenzte Anzahl von Zeichen enthalten für die Verwendung im JSON-Format ohne Escapezeichen oder Base64-Codierung sicher).

Nach der anderen Benutzer ihre Reservierung Zeichenfolgen aus der Eigenschaft der gemeinsamen Sitzung Dokument abgerufen haben, können dann beginnen sie verschieben, die mit dem XIM-Netzwerk, das als Parameter verwenden `xim::move_to_network_using_out_of_band_reservation()`. Im folgende Beispiel wird davon ausgegangen, dass die reservierte Zeichenfolge auf eine Variable namens "ReservationString" extrahiert wurde.

```cpp
xim::singleton_instance().move_to_network_using_out_of_band_reservation(reservationString);

```

Die Move-Vorgängen führt asynchron genauso wie für das erste Gerät, das einen null-Zeiger für die reservierte Zeichenfolge angegeben. -Statusänderungen `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, und `xim_move_to_network_succeeded_state_change` generiert werden, durch das verschieben. Wenn die Verschiebung erfolgreich ist, werden sowohl lokale als auch Spieler hinzugefügt werden. Vorhandene Geräte im Netzwerk XIM erfolgt eine `xim_player_joined_state_change` für diese neuen Spieler. An diesem Punkt ist Sprache und Text-Chat-Kommunikation zwischen den Spieler auf diesen verschiedenen Geräten in diesem Netzwerk XIM automatisch aktiviert (wo Datenschutz und Richtlinie zulassen).

Sollte der Verschiebevorgang fehlschlagen, da eine ungültige reservierungsauftrags-Zeichenfolge, XIM zurück eine `xim_network_exited_state_change` mit dem Grund `xim_network_exit_reason::invalid_reservation`. Dies kann verschiedene Ursachen haben:

1. Der Titel aufrufen, versucht `xim::move_to_network_using_out_of_band_reservation()` auf dem Remotegerät vor der `xim_create_out_of_band_reservation_completed_state_change` wird zurückgegeben, auf dem Hostgerät
1. Der Titel aufrufen, versucht `xim::move_to_network_using_out_of_band_reservation()` auf dem Remotegerät, nachdem die Reservierung abgelaufen ist.
1. Der Titel aufrufen, versucht `xim::move_to_network_using_out_of_band_reservation()` auf dem Remotegerät mehrere Male während des Aufrufs nur `xim::create_out_of_band_reservation()` nach.

Unabhängig vom Erfolg oder Fehler werden Reservierungen von Move-Vorgängen genutzt werden, die diese verwenden. Aus diesem Grund nach jedem Versuch mit der Host generieren soll eine neue Reservierung Zeichenfolge durch den Aufruf `xim::create_out_of_band_reservation()` erneut aus.

## <a name="configuring-chat-targets-in-a-xim-network-managed-using-out-of-band-reservations"></a>Konfigurieren der Chat-Ziele in einem Netzwerk XIM verwaltet mit der Out-of-Band-Reservierungen

XIM-Netzwerke, die verwaltet werden, mithilfe der Out-of-Band-Reservierungen Verhalten sich identisch, mit herkömmlichen XIM-Netzwerken in Bezug auf die Chat-Kommunikation. Um wettbewerbsfähig zu unterstützen sind Szenarien, die neu erstellte XIM Netzwerke automatisch so konfiguriert, dass um nur Chat mit anderen Spielern zu unterstützen, die Mitglieder des gleichen Teams sind; d. h. der konfigurierte Wert von gemeldeten `xim::chat_targets()` standardmäßig `xim_chat_targets::same_team_index_only`. Dies kann geändert werden, um Benutzern gestatten, wenden Sie sich an alle anderen Benutzer (wo Datenschutz und Richtlinie zulassen), durch den Aufruf `xim::set_chat_targets()`. Im folgende Beispiel beginnt mit der Konfiguration jeder im Netzwerk XIM verwenden eine `xim_chat_targets::all_players` Wert:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Alle Teilnehmer werden darüber informiert, dass sie ein neues Ziel, die Einstellung in Kraft bei bereitgestellt sind eine `xim_chat_targets_changed_state_change`.

Jedes Player in XIM-Netzwerken mithilfe von Out-of-Band-Reservierungen verwaltet ist anfangs so konfiguriert, mit dem gleichen Team Indexwert, 0 (null). Dies bedeutet, dass eine Konfiguration des `xim_chat_targets::same_team_index_only` wird nicht von Unterschieden `xim_chat_targets::all_players` standardmäßig. Sie können jedoch des Spielers auf einem lokalen Team Index zu einem beliebigen Zeitpunkt ändern, durch den Aufruf `xim_player::xim_local::set_team_index()`. Im folgenden Beispiel wird einen Player-Zeiger "LocalPlayer", um einen neuen Team Indexwert eines zu erhalten:

```cpp
 localPlayer->local()->set_team_index(1);
```

Alle Geräte werden darüber informiert, dass der Spieler einen neues Team Indexwert wirksam hat, wenn sie eine Xim_player_team_index_changed_state_change für der Spieler angegeben sind. Wenn die Chat-Zielkonfiguration derzeit xim_chat_targets::same_team_index_only ist, klicken Sie dann beginnt Weitere Entwicklungen mit diesem gleichen Index der neuen Team hören-Sprache und Text-Chat (Datenschutz und sofern die Richtlinie dies zulässt) bereitgestellt wird von der Änderung Player und umgekehrt ist dies. Spieler mit den alten Team Index werden beendet, dieser Mitteilung Chat austauschen. Wenn die Chat-Zielkonfiguration derzeit xim_chat_targets::all_players ist, hat dann Team Index keine Auswirkungen auf, die mit denen chat können.

## <a name="removing-players-from-a-xim-network-managed-using-out-of-band-reservations"></a>Entfernen von Spielern über ein Netzwerk XIM verwaltet mit der Out-of-Band-Reservierungen

Sie verwalten die Liste der Spieler extern Verwendung von Out-of-Band-Reservierungen, natürlich müssen Sie möglicherweise Player aus dem Netzwerk XIM zu entfernen. Das übliche Verfahren ist für apps, spielen demselben Host nutzen, die verwendet wurde, erstellt die XIM Netzwerk und die nachfolgenden Reservierungen ursprünglich zur auch Player entfernen verwalten und Aufrufen `xim::kick_player()`. Dadurch werden die angegebenen Player und alle Spieler auf demselben Gerät aus dem Netzwerk XIM entfernt. Im folgende Beispiel wird davon ausgegangen, die app wurde festgestellt, dass eine Verbindung Entfernen der `xim_player` Objekt verweist die Variable "PlayerToRemove":

```cpp
xim::singleton_instance().kick_player(playerToRemove);
```

Die entsprechenden Player (oder -Playern,) erfolgt alle notwendigen `xim_player_left_state_change` für alle Spieler und `xim_network_exited_state_change` gibt an, dass sie über das Netzwerk gestartet wurde, haben. Sie können auch jeden Spieler Aufrufen `xim::leave_network()` um auf ihre eigenen für den gleichen Effekt zu beenden.

Denken Sie daran, dass XIM Peer-zu-Peer-Kommunikation, die ein Player im Netzwerk XIM aufgrund von environmental schwierigkeiten verlassen hat eine eigene Entscheidung zu einem beliebigen Zeitpunkt zu treffen kann. Ihre app sollte darauf vorbereitet sein verarbeitet eine "unverlangt" `xim_player_left_state_change` aus, und stimmen Sie etwaigen Unstimmigkeiten zwischen XIMs Status und Ihre externen Player Management das Schema für Ihre spezielle app geeignet.

## <a name="cleaning-up-a-xim-network-managed-using-out-of-band-reservations"></a>Bereinigen von einem Netzwerk XIM verwaltet mit der Out-of-Band-Reservierungen

Alle Player, die von der XIM bereits gestartet wurde noch nicht Netzwerk und in den Zustand zurückgegeben werden sollen, als würde nur möchten Xim::initialize() und `xim::set_intended_local_xbox_user_ids()` war aufgerufen wurde, kann dies dazu beginnen die `xim::leave_network()` Methode:

```cpp
xim::singleton_instance().leave_network();
```

Trennen die anderen Teilnehmer asynchron gestartet. Dies bewirkt, dass der remote-Geräte bereitgestellt werden eine `xim_player_left_state_change` für den lokalen Spieler und dem lokalen Gerät bereitgestellt wird eine `xim_player_left_state_change` für jeden Spieler, lokal oder remote. Nachdem alle ordnungsgemäßes Trennung abgeschlossen ist, eine endgültige `xim_network_exited_state_change` bereitgestellt. Die app kann dann aufrufen `xim::cleanup()` , alle Ressourcen freizugeben, und kehren Sie zu den nicht initialisierten Status zurück:

```cpp
 xim::singleton_instance().cleanup();
```

Aufrufen von `xim::leave_network()` und wartet die `xim_network_exited_state_change` zum Beenden einer XIM Netzwerk ordnungsgemäß wird immer empfohlen bei einer `xim_network_exited_state_change` wurde noch nicht bereitgestellt. Aufrufen von `xim::cleanup()` direkt kann dazu führen, dass Leistungsprobleme für die Kommunikation für die verbleibenden Teilnehmer, während sie Timeout-Nachrichten an das Gerät, die einfach erzwungen werden "verschwunden".
