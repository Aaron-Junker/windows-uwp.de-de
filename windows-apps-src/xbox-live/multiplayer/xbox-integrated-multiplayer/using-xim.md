---
title: Verwenden von XIM (C++)
description: Erfahren Sie, wie Sie Xbox integrierte Multiplayer-Spiele (XIM) mit C++ verwenden.
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Xbox eine Xbox integrierte Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: 798d1a1bc738cbdc7bb2b3fb34076f0897fc76ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644055"
---
# <a name="using-xim-c"></a>Verwenden von XIM (C++)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

Dies ist eine kurze exemplarische Vorgehensweise zur Verwendung XIMs C++-API. Entwickler XIM über den Zugriff auf, die von Spielen C# sollte [XIM verwenden (C#)](using-xim-cs.md).

- [Verwenden von XIM (C++)](#using-xim-c)
    - [Erforderliche Komponenten](#prerequisites)
    - [Initialisieren und starten](#initialization-and-startup)
    - [Asynchrone Vorgänge und Verarbeitung von statusänderungen](#asynchronous-operations-and-processing-state-changes)
    - [Behandeln von XIM Spielern Objekte](#handling-xim-player-objects)
    - [Aktivieren der join von Freunden und einladen](#enabling-friends-to-join-and-inviting-them)
    - [Grundlegende Vermittlung und das Verschieben in ein anderes XIM-Netzwerk mit anderen Benutzern](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [Deaktivieren die Vermittlung von NAT-Regel zum Debuggen](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [Verlassen eines Netzwerks XIM und bereinigen](#leaving-a-xim-network-and-cleaning-up)
    - [Senden und Empfangen von Nachrichten](#sending-and-receiving-messages)
    - [Bewerten der Datenqualität-Pfad](#assessing-data-pathway-quality)
    - [Freigeben von Daten mithilfe von benutzerdefinierten Eigenschaften der player](#sharing-data-using-player-custom-properties)
    - [Freigeben von Daten mithilfe von benutzerdefinierten Eigenschaften des Netzwerks](#sharing-data-using-network-custom-properties)
    - [Mithilfe der Fähigkeiten von pro-Player Vermittlung](#matchmaking-using-per-player-skill)
    - [Vermittlung mit pro-Player-Rolle](#matchmaking-using-per-player-role)
    - [Funktionsweise von XIM mit Player-teams](#how-xim-works-with-player-teams)
    - [Arbeiten mit chat](#working-with-chat)
    - [Stummschaltung des players](#muting-players)
    - [Konfigurieren die Spieler Teams über Chat-Ziele](#configuring-chat-targets-using-player-teams)
    - [Automatische Ausfüllen von Player-Slots ("Abgleich" Vermittlung)](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [Abfragen von beigetreten Netzwerke](#querying-joinable-networks)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Codierung mit XIM beginnen, gibt es zwei Voraussetzungen.

1. Müssen konfiguriert sein, Ihrer app AppXManifest mit standard Multiplayer-Netzwerkfunktionen, und Sie müssen den manifest Netzwerkteil aus, um den erforderlichen Verkehr von XIM verwendeten Vorlagen deklarieren konfiguriert haben.

    AppXManifest-Funktionen und Netzwerk-Manifeste werden in den jeweiligen Abschnitten der Plattformdokumentation ausführlicher beschrieben; der typische XIM-spezifische XML-Code einfügen, finden Sie auf [XIM Projektkonfiguration](xim-manifest.md).

1. Sie benötigen zwei Identität Anwendungsinformationen verfügbar:

    * Die zugewiesenen Titel Xbox Live-ID.
    * Die Konfigurations-ID als Teil der Bereitstellung Ihrer Anwendung für den Zugriff auf den Xbox Live-Dienst bereitgestellt.

    Finden Sie in Ihren Ansprechpartner bei Microsoft Weitere Informationen zum Anfordern dieser. Diese Informationen werden während der Initialisierung verwendet werden.

Kompilieren XIM ist erforderlich, einschließlich des primären Replikats `XboxIntegratedMultiplayer.h` Header. Um ordnungsgemäß zu verknüpfen, das Projekt muss auch umfassen `XboxIntegratedMultiplayerImpl.h` in mindestens einer Kompilierungseinheit (Allgemeine vorkompilierten Headers wird empfohlen, da diese Funktion stubimplementierungen kurz und leicht für den Compiler an, wie "Inline" generiert werden).

Siehe die [XIM Übersicht über](../xbox-integrated-multiplayer.md), die XIM-Schnittstelle erfordert kein Projekt auswählen, die zwischen dem Kompilieren mit C++ / CX im Vergleich zu herkömmlichen C++; kann entweder mit verwendet werden. Die Implementierung auslösen nicht auch Ausnahmen, als Mittel zum nicht schwerwiegenden Fehler, die Berichterstattung, damit Sie es aus ausnahmefreie Projekten nutzen können, wenn Sie dies bevorzugen.

## <a name="initialization-and-startup"></a>Initialisieren und starten

Sie beginnen mit der Bibliothek interagieren, mit der Initialisierung des XIM Objekt Singletons durch Ihre Xbox Live-Dienst-Konfiguration-ID Zeichenfolge und Titel ID-Nummer. Wenn Sie auch die Xbox-API verwenden, es können bequem abzurufenden diese vom `xbox::services::xbox_live_app_config`. Im folgende Beispiel wird davon ausgegangen, dass die Werte bereits bzw. in der Variablen 'MyServiceConfigurationId' und "MyTitleId" befinden:

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

Nach der Initialisierung die app sollten abrufen, die Xbox-Benutzer-ID-Zeichenfolgen für alle Benutzer derzeit auf dem lokalen Gerät, das teilnehmen, und übergeben sie einen `xim::set_intended_local_xbox_user_ids()` aufrufen. Im folgenden Beispielcode wird davon ausgegangen werden, ein einzelnen Benutzer verfügt über eine Absicht, spielen Ausdrücken Controller-Schaltfläche gedrückt, und die Xbox-Benutzer-ID-Zeichenfolge, die dem Benutzer zugeordneten bereits in eine "MyXuid"-Variable abgerufen wurde:

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

Ein Aufruf von `xim::set_intended_local_xbox_user_ids()` sofort legt die Xbox-Benutzer-IDs verknüpft ist, mit der lokalen Benutzer, die mit dem Netzwerk XIM hinzugefügt werden soll. Diese Liste von Xbox-Benutzer-IDs werden in alle zukünftigen Netzwerkvorgänge verwendet werden, bis die Änderungen der Liste durch einen anderen Aufruf von `xim::set_intended_local_xbox_user_ids()`.

Im Fall, in denen kein Netzwerk XIM überhaupt noch vorhanden ist, ist im ersten Schritt mit einem Netzwerk XIM verschieben, um diese gestarteten Prozess zu erhalten. Wenn der Benutzer noch ein bestimmtes XIM Netzwerk Denken Sie daran nicht, ist es empfehlenswert, einfach in ein neues, leeres Netzwerk zu verschieben. Dadurch wird die Freunde des Benutzers, mit dem Netzwerk als eine Art "Lobby" aus dem sie miteinander zusammenarbeiten können, um ihre nächste Multiplayer-Aktivität (z. B. durch Eingabe von Vermittlung) zu wählen.

Im folgenden ist ein Beispiel zum Verschieben Sie nur die lokalen Benutzer, die zuvor hinzugefügte mit `xim::set_intended_local_xbox_user_ids()` mit einem leeren XIM-Netzwerk mit Platz für bis zu 8 gesamt Spieler:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

Der Aufruf von `xim::move_to_new_network()` initiiert einen asynchronen Vorgang "verschieben", der mit einem Statusänderungsereignis abgeschlossen ist, die Entwickler für regelmäßig verarbeitet werden sollen.

## <a name="asynchronous-operations-and-processing-state-changes"></a>Asynchrone Vorgänge und Verarbeitung von statusänderungen

Das Herzstück der XIM ist der app reguläre und häufige Aufrufe der `xim::start_processing_state_changes()` und `xim::finish_processing_state_changes()` Paar von Methoden. Diese Methoden sind wie XIM informiert wird, dass die app zur Verarbeitung von Updates für mehrere Spieler Zustand bereit ist, und wie XIM diese Aktualisierungen bereitstellt. Sie sind dazu konzipiert, schnell ausgeführt werden, sodass sie jedes Einzelbild Grafiken in der UI-Rendering-Schleife aufgerufen werden können. Dadurch wird einen praktischer Ort für alle in der Warteschlange stehende Änderungen abrufen, ohne sich Gedanken Netzwerk Zeitangaben oder Multithread-Rückruf Komplexität nicht vorhersagbar sind. Die XIM-API ist für dieses Muster Singlethread-tatsächlich optimiert. Es wird sichergestellt, dass es sich bei seinem Zustand außerhalb dieser beiden Funktionen, unverändert bleiben wird, damit Sie sie direkt und effizient verwenden können.

Aus demselben Grund, alle Objekte, die von der XIM-API zurückgegebenen sollten *nicht* threadsicher betrachtet werden. Die Bibliothek verfügt über einen internen multithreading Schutz, jedoch müssen Sie dennoch implementieren Ihre eigenen Sperren, wenn Sie einen beliebigen Thread für den Zugriff auf XIMs Werte benötigen. Z. B. bei einem Thread durchlaufen müssen die `xim::players()` auflisten, während ein anderer Thread entweder aufrufen konnte `xim::start_processing_state_changes()` oder `xim::finish_processing_state_changes()` und ändern die Liste der Spieler zugeordneten Arbeitsspeicher.

Wenn `xim::start_processing_state_changes()` wird aufgerufen, werden alle in der Warteschlange Updates in ein Array von gemeldet `xim_state_change` Zeiger zu strukturieren.

Apps sollten Folgendes beachten:

1. Durchlaufen Sie das Array ein.
1. Überprüfen Sie die grundlegende Struktur für die spezifischeren Typ.
1. Wandeln Sie den Strukturtyp der Basis der den entsprechenden Typ ausführlichere.
1. Behandeln Sie das Update nach Bedarf.

Wenn alle fertig gestellt das `xim_state_change` Strukturen, die derzeit verfügbaren, dieses Array übergeben werden an XIM auf die Ressourcen freigeben, indem `xim::finish_processing_state_changes()`. Zum Beispiel:

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
           MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change*>(stateChange));
           break;
        }

       case xim_state_change_type::player_left:
       {
           MyHandlePlayerLeft(static_cast<const xim_player_left_state_change*>(stateChange));
           break;
       }

       ...
    }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Nun, da Sie Ihre grundlegenden Verarbeitungsschleife verfügen, können Sie die statusänderungen im Zusammenhang mit dem ersten behandeln `xim::move_to_new_network()` Vorgang. Jeder XIM Netzwerk Move-Vorgang beginnt mit einem `xim_move_to_network_starting_state_change`. Wenn die Verschiebung aus irgendeinem Grund fehlschlägt, wird Ihre app bereitgestellt werden eine `xim_network_exited_state_change`, die den Fehlerbehandlungsmechanismus für ein asynchroner Fehler, die verhindern, dass Sie mit einem Netzwerk XIM verschieben oder trennt die Verbindung aus dem aktuellen XIM-Netzwerk mit häufiger Fehler ist. Andernfalls wird der Verschiebevorgang abgeschlossen, mit einem `xim_move_to_network_succeeded_state_change` nachdem alle Zustände fertig gestellt wurde wurde, und alle Spieler erfolgreich mit dem Netzwerk XIM hinzugefügt wurden.

Wenn ein Benutzer nicht über die Plattform-Berechtigung für die Wiedergabe mit anderen Personen in einer Multiplayer-Sitzung verfügt, auf einem Gerät, das beim Erstellen oder nutzen Sie ein Netzwerk XIM XIM sendet eine `xim_network_exited_state_change` mit dem Grund `xim_network_exit_reason::user_multiplayer_restricted`. Dies geschieht, wenn die lokalen Benutzer mit festlegen `xim::set_intended_local_xbox_user_ids()` eine Xbox Live Multiplayer-Einschränkung aufweisen. Xbox ERA-Apps, die mithilfe von XIM sollten entweder Aufrufen `Store::Product::CheckPrivilegeAsync` von lokalen Playern, bevor Sie versuchen, auf "true" in einem Netzwerk XIM mit AttemptResolution verschieben oder zu diesem Zweck als Reaktion auf den Empfang `xim_network_exit_reason::user_multiplayer_restricted` als einen Grund für ein zurückgegebenes `xim_network_exited_state_change`.

## <a name="handling-xim-player-objects"></a>Behandeln von XIM Spielern Objekte

Sofern das Beispiel für ein einzelner lokaler Benutzer mit einem neuen XIM Netzwerk verschieben erfolgreich ausgeführt wurde, wird Ihre app bereitgestellt werden eine `xim_player_joined_state_change` für eine lokale `xim_player` Objekt. Diese Objektzeiger wird so lange gültig bleiben, solange die Player-Instanz selbst gültig ist. Die Player-Instanz wird ungültig, wenn die entsprechende `xim_player_left_state_change` für die betreffende Spieler Instanz über einen Aufruf zurückgegeben wurde `xim::finish_processing_state_changes()`. Ihrer app erfolgt immer eine `xim_player_left_state_change` für jede `xim_player_joined_state_change`.

Sie können auch ein Array aller abrufen `xim_player` Objekte in der XIM Netzwerk zu einem beliebigen Zeitpunkt mit `xim::get_players()`.

Die `xim_player` Objekt verfügt über viele nützliche Methoden, z. B. `xim_player::gamertag()` für den Player zu Anzeigezwecken Abrufen der aktuellen Zeichenfolge für die Xbox Live-Gamertag zugeordnet werden. Wenn die `xim_player` lokal auf dem Gerät ist, wird es auch eine Wert ungleich Null gemeldet werden `xim_player::xim_local` Objektzeiger aus `xim_player::local()`, die über zusätzliche Methoden, die nur verfügbar für lokale Unternehmen verfügt.

Der wichtigste Status für Spieler ist natürlich nicht, die allgemeine Informationen, die XIM bekannt sind, aber was Ihre spezifische app nachverfolgt werden sollen. Da Sie wahrscheinlich Ihre eigenen Konstrukt für diese Nachverfolgungsinformationen verfügen, sollten Sie verknüpfen die `xim_player` -Objekt in Ihren eigenen Player-Konstrukt-Objekt, damit jedes Mal, wenn XIM Berichte ein `xim_player`, können Sie schnell den Status abrufen, ohne zu füllen und Suchen Sie einen benutzerdefinierten Player Kontextzeiger. Im folgende Beispiel wird angenommen, ein Zeiger auf den privaten Zustand ist in der Variablen "MyPlayerStateObject" und der neu hinzugefügten `xim_player` Objekt befindet sich in der Variablen "NewXimPlayer":

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

Dies spart Wert des angegebenen Zeigers mit lokal das Player-Objekt (wird nie übertragen über das Netzwerk für externe Geräte, in dem der Arbeitsspeicher gültig wäre nicht). Sie müssen dann immer wieder auf Ihr Objekt abrufen, indem benutzerdefinierten Kontext abrufen und ihn wieder auf Ihr Objekt wie im folgenden Beispiel umzuwandeln können:

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

Sie können ändern, dass der benutzerdefinierte Player-Kontextzeiger zugewiesen eine `xim_player` zu einem beliebigen Zeitpunkt.

## <a name="enabling-friends-to-join-and-inviting-them"></a>Aktivieren der join von Freunden und einladen

Für Datenschutz und Sicherheit alle neuen XIM Netzwerke werden automatisch konfiguriert, werden standardmäßig von lokalen Playern nur beigetreten sein, und es liegt an der app, die explizit andere können, sobald er fertig ist. Das folgende Beispiel zeigt, wie Sie mit `xim::network_configuration()` zum Abrufen von der aktuellen Netzwerkkonfiguration und aktualisieren die Joinability mit `xim::set_network_configuration()` zu beginnen, sodass neue lokale Benutzer als Spieler hinzufügen sowie andere Benutzer, die eingeladen wurden oder werden, die " Folgen "(eine Xbox Live soziale Beziehung), von Playern bereits im Netzwerk XIM:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` führt Asynchronoulsy an. Wenn der vorherige Code-Beispiel-Aufruf abgeschlossen ist, eine `xim_network_configuration_changed_state_change` wird bereitgestellt, um die app zu benachrichtigen, die der Joinability-Wert von seinem Standardwert geändert hat `xim_allowed_player_joins::none`. Sie können dann für den neuen Wert Abfragen, indem Sie überprüfen die `allowed_player_joins` Eigenschaft der `xim_network_configuration` zurückgegebenes `xim::network_configuration()`. 

Die `allowed_player_joins` kann überprüft werden, während das Gerät in einem Netzwerk XIM, um zu bestimmen, die Joinability des Netzwerks ist.

Sollte einen der lokalen Player, Einladungen für Remotebenutzer an mit diesem XIM Netzwerk senden möchten, kann die app Aufrufen `xim_player::xim_local::show_invite_ui()` um die Einladung System Benutzeroberfläche zu starten. Der lokale Benutzer kann hier Personen auswählen, die sie einladen und Einladungen senden möchten. Im folgenden Beispiel wird veranschaulicht, wie dies funktioniert, und setzt voraus, dass die Variable "XimPlayer" mit einer gültigen lokalen zeigt `xim_player`:

```cpp
ximPlayer->local()->show_invite_ui();
```

Nachdem die oben genannte Anweisung ausgeführt wurde, wird die Einladung System Benutzeroberfläche angezeigt. Sobald der Benutzer hat die Einladungen gesendet (oder andernfalls beendet die Benutzeroberfläche), eine `xim_show_invite_ui_completed_state_change` finden Sie auf dem nächsten Aufruf von `xim::start_processing_state_changes()`. Alternativ kann Ihre app direkt mit einer Einladung senden `xim_player::xim_local::invite_users()` ohne Einbindung der System-einladungs Benutzeroberfläche.

In beiden Fällen werden Remotebenutzer einladungsnachrichten Xbox Live erhalten, wo sie angemeldet sind, die sie auswählen können, um anzunehmen oder abzulehnen. Wenn sie akzeptiert haben, initiiert eine protokollaktivierung Ihrer app auf diesen Geräten, wenn es nicht bereits ausgeführt wird, und die protokollaktivierung zu Ihrer app mit den Ereignis-Argumenten, die verwendet werden können, um mit dem entsprechenden XIM-Netzwerk zu verschieben, übermittelt werden.

Finden Sie die Dokumentation zur Plattform für Weitere Informationen auf protokollaktivierung selbst.

Das folgende Beispiel zeigt, wie ein Aufruf `xim::extract_protocol_activation_information()` bestimmt, ob die Ereignisargumente für XIM gelten. Dies setzt voraus, die unformatierte URI-Zeichenfolge aus abgerufenen `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` auf eine Variable "UriString":

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

Wenn sie einen aktivierten XIM und dann wird den lokalen Benutzer identifiziert, die im Feld "Local_xbox_user_id" ein, der die aufgefüllte sicherstellen möchten `xim_protocol_activation_information` Struktur angemeldet ist und wird von den Benutzern angegeben, um `xim::set_intended_local_xbox_user_ids()`. Sie können dann initiieren, mit dem angegebenen XIM-Netzwerk mit einem Aufruf von verschieben `xim::move_to_network_using_protocol_activated_event_args()` unter Verwendung der gleichen URI-Zeichenfolge. Zum Beispiel:

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

Beachten Sie, die "gefolgt von remote-Benutzer" können Sie auch navigieren Sie in die Benutzeroberfläche des Systems zu Player-Karte des lokalen Benutzers und initiieren eine joinversuch selbst, ohne eine Einladung (vorausgesetzt, dass Sie solche Player-Joins wie oben gezeigt zugelassen haben). Diese Aktion wird außerdem Protokoll aktivieren Sie Ihre app einfach wie Einladungen und in die gleiche Weise behandelt werden.

Wechsel zu einem XIM-Netzwerk mithilfe von protokollaktivierung ist identisch mit der Umstellung auf ein neues XIM-Netzwerk, siehe [Initialisierungs- und Start](#initialization-and-startup). Der einzige Unterschied ist, wenn die Verschiebung erfolgreich ist, das Gerät verschieben sowohl lokale als auch einen Player bereitgestellt wird `xim_player_joined_state_change` Strukturen, die alle anwendbaren Spieler darstellen. Natürlich ist das ursprüngliche Gerät, das bereits im Netzwerk XIM war wird nicht verschoben werden, aber sehen die Benutzer für das neue Gerät als Playern über zusätzliche hinzugefügt werden `xim_player_joined_state_change` Strukturen.

Nachdem Sie den spielerverlauf auf verschiedenen Geräten in einem Netzwerk XIM verknüpft werden, können sie multiplayer-Szenarien initiieren, kommunizieren über Sprache und Text automatisch und app-spezifische Nachrichten senden.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>Grundlegende Vermittlung und das Verschieben in ein anderes XIM-Netzwerk mit anderen Benutzern

Sie können die netzwerkerlebnis für eine Gruppe von Freunden weiter erweitern, durch das Verschieben der Spieler mit einem XIM-Netzwerk, das auch völlig fremden verfügt auch über. Fremden sind Gegnern von auf der ganzen Welt, die zusammen mit dem Xbox Live-Vermittlungsdienst basierend auf ähnlichen Interessen hochgefahren werden.

Eine einfache Möglichkeit, grundlegende Vermittlung mit XIM initiiert wird, durch den Aufruf `xim::move_to_network_using_matchmaking()` auf eines der Geräte mit einem gefüllten `xim_matchmaking_configuration` Struktur, die Spieler aus dem aktuellen XIM-Netzwerk mit.

Im folgende Beispiel startet eine Verschiebung mit einer Vermittlung-Konfiguration einrichten, insgesamt 8 Playern für eine ohne-Teams jeder gegen jeden Übereinstimmung zu finden. Wenn 8 gesamt Spieler nicht gefunden werden, kann das System 2 bis 7 Spieler noch zusammen übereinstimmen. Dieses Beispiel verwendet eine Konstante-app definiert der Typ uint64_t, mit dem Namen MYGAMEMODE_DEATHMATCH, die die Spiel-Modus zum Deaktivieren von Filtern darstellt. Der XIM Vermittlung werden nur dieses Netzwerk mit anderen Spielern gibt an, dass der gleiche Wert und schaltet entlang der Spieler alle Social eingebunden, aus dem aktuellen XIM-Netzwerk, bei der Bereitstellung des zweiten Parameters abgeglichen `xim_players_to_move::bring_existing_social_players` zu `xim::move_to_network_using_matchmaking()`:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Wie weiter oben verschiebt, bietet dieser Prozess für die Vermittlung ein ursprüngliches `xim_move_to_network_starting_state_change` auf allen Geräten und einem `xim_move_to_network_succeeded_state_change` nach dem erfolgreichen der Verschiebung Abschluss. Da dies eine Verschiebung aus einem XIM-Netzwerk in ein anderes ist, ein Unterschied besteht darin, die bereits vorhandene `xim_player` Objekte, die für lokale und remote Benutzer hinzugefügt werden soll, und diese bleibt für alle Spieler, die zusammen mit dem neuen XIM Netzwerk verschieben. Chat und Daten Kommunikation zwischen den Spieler weiterhin ununterbrochen weiterarbeiten, während Vermittlung ausgeführt wird.

Vermittlung kann abhängig von der Anzahl der potenziellen Player im Pool Vermittlung, die so genannte haben ein langwieriger Prozess sein `xim::move_to_network_using_matchmaking()`.

Ein `xim_matchmaking_progress_updated_state_change` erfolgt in regelmäßigen Abständen während der Operation, Sie und Ihre Benutzer, die mit dem aktuellen Status auf dem Laufenden zu halten. Wenn die Übereinstimmung gefunden wurde, werden zusätzliche Spieler das XIM-Netzwerk mit den normalen hinzugefügt `xim_player_joined_state_change` und Abschluss der Verschiebung.

Wenn Sie die Multiplayer-Erfahrung mit diesen "Matchmade" Spieler fertig gestellt haben, können Sie den Prozess zum Verschieben in ein anderes XIM-Netzwerk mit einem anderen runde der Vermittlung wiederholen. Sehen Sie jeden Spieler, die über die vorherige verknüpft `xim::move_to_network_using_matchmaking()` Geben Sie den Vorgang eine `xim_player_left_state_change` an, dass ihre `xim_player` Objekte sind nicht mehr im gleichen Netzwerk XIM.

Wenn bei der Auswahl zum Initiieren der Vermittlung erneut mit `xim::move_to_network_using_matchmaking()`, Angabe `xim_players_to_move::bring_existing_social_players`, den Player, die über soziale Netzwerke Einstiegspunkte miteinander verknüpft (`xim::move_to_network_using_protocol_activated_event_args()` oder `xim::move_to_network_using_joinable_xbox_user_id()`) bleibt während der neue Vermittlung ausgeführt wird. Sie können auch angeben `xim_players_to_move::bring_only_local_players` werden getrennt, Social verbundene remote-Playern, lassen nur die lokalen Spieler, um zu gewährleisten. In beiden Fällen wird eine andere Gruppe von fremden hinzugefügt werden, wenn der zweite Move-Vorgang abgeschlossen ist.

Sie haben auch die Möglichkeit, die nicht-Matchmade Spieler (oder nur lokal Spieler) verschieben zu einem vollständig neuen XIM Netzwerk entscheiden, ob die nächste Vermittlung Configuration/Multiplayer-Aktivität. Im folgende Beispiel wird veranschaulicht, wie ein Gerät aufgerufen werden musste `xim::move_to_new_network()` ein XIM-Netzwerk mit bis zu 8 Spieler, aber dieses Mal dauert die vorhandenen Playern Social eingebundenen auch:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

Ein `xim_move_to_network_starting_state_change` und `xim_move_to_network_succeeded_state_change` wird für alle beteiligten Geräte bereitgestellt werden, zusammen mit einem `xim_player_left_state_change` für Matchmade Spieler, die zurückgelassen wurden. Auf diesen Geräten und sehen sie auf ähnliche Weise eine `xim_player_left_state_change` für jeden Spieler, die aus dem Netzwerk verschoben werden.

Sie können weiterhin, auf die oben beschriebenen Weise so oft wie gewünscht aus XIM Netzwerk XIM Netzwerk verschieben.

Für die Leistung zu erzielen versucht der Xbox Live-Dienst nicht entsprechend Gruppen der Spieler auf Geräten, die wahrscheinlich keine direkten Peer-zu-Peer-Verbindungen hergestellt werden kann. Wenn Sie in einer Umgebung entwickeln, die nicht ordnungsgemäß für die standardmäßige Xbox Live Multiplayer-Unterstützung konfiguriert ist, kann die Umstellung auf neuen Netzwerks Vermittlung-Vorgang mit unbestimmte Zeit fortgesetzt ohne erfolgreiche übereinstimmende, selbst wenn Sie sicher, dass Sie haben ausreichende Spieler erfüllt die Kriterien für die Vermittlung, die alle verschieben und alle Geräte in der gleichen lokalen Umgebung sind. Achten Sie darauf, dass zum Ausführen der Multiplayer-Konnektivitätstest in die Netzwerkeinstellungen Bereich/Xbox-Anwendung und führen die Empfehlungen, wenn es Probleme, vor allem in Bezug auf eine NAT"Strict" meldet.

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>Deaktivieren die Vermittlung von NAT-Regel zum Debuggen

Wenn vom Netzwerkadministrator der erforderlichen Umgebung Änderungen an XIMs NAT-Regeln unterstützen kann, können Sie Ihre Tests auf Xbox One Development-Kits, durch Konfigurieren von XIM damit können die passende "Strict NAT" Geräte ohne mindestens eine "Open Vermittlung zulassen. NAT"-Gerät. Dies erfolgt durch Ablegen einer Datei namens "Xim_disable_matchmaking_nat_rule" (Inhalt nicht von Bedeutung sind) im Stammverzeichnis des Laufwerks "Titel Grund auf neu erstellen" auf alle Xbox One-Konsolen. Ein ist Beispiel dafür die Folgendes an einer Eingabeaufforderung xdk-Version vor dem Starten der app, und Ersetzen Sie dabei den Platzhalter "{Console_name_or_ip_address}" für jede Konsole nach Bedarf ausführen:

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> Diese Entwicklung problemumgehung ist derzeit nur für Xbox One-Ressource-Anwendungen verfügbar und wird nicht für universelle Windows-Anwendungen unterstützt. Auch Hinweis: Konsolen, die Verwendung dieser Einstellung werden nie mit Geräten entspricht, die auch diese Datei vorhanden ist, unabhängig von der Netzwerkumgebung, haben also werden Sie sicher, dass zum Hinzufügen und entfernen Sie die Datei überall.

## <a name="leaving-a-xim-network-and-cleaning-up"></a>Verlassen eines Netzwerks XIM und bereinigen

Wenn lokale Benutzer haben Teilnahme an einem XIM Netzwerk häufig sie einfach in ein neues XIM Netzwerk verschoben werden, die lokalen Benutzer ermöglicht, eingeladene Benutzer, und "folgen" Benutzer hinzufügen, damit sie mit ihren Freunden, bei der Suche nach der nächsten Aktivität koordinieren fortgesetzt werden können. Aber wenn des Benutzers ist vollständig mit Ihren Erfahrungen Multiplayer-Ihrer-app beginnen, die XIM verlassen möchten Netzwerk vollständig und in den Zustand zurück, als würde nur `xim::initialize()` und `xim::set_intended_local_xbox_user_ids()` war aufgerufen wurde. Dies erfolgt mithilfe der `xim::leave_network()` Methode:

```cpp
xim::singleton_instance().leave_network();
```

Diese Methode startet den Prozess des Trennens asynchron aus den anderen Teilnehmern ordnungsgemäß. Dies bewirkt, dass der remote-Geräte bereitgestellt werden eine `xim_player_left_state_change` für jeden lokalen Spieler, die getrennt. Das lokale Gerät werden außerdem erfolgt eine `xim_player_left_state_change` für jeden lokalen und Remotecomputern Spieler, die getrennt wurde. Wenn alle trennen Vorgänge fertig sind, eine endgültige `xim_network_exited_state_change` bereitgestellt. Die app kann dann aufrufen "xim::cleanup()'', alle Ressourcen freizugeben, und kehren Sie zu den nicht initialisierten Status zurück:

```cpp
xim::singleton_instance().cleanup();
```

Aufrufen von `xim::leave_network()` und wartet die `xim_network_exited_state_change` zum Beenden einer XIM Netzwerk ordnungsgemäß wird immer empfohlen bei einer `xim_network_exited_state_change` wurde noch nicht bereitgestellt.

Wenn `xim::cleanup()` wird aufgerufen, direkt ohne `xim::leave_network()` aufgerufen wird, und klicken Sie dann die Kommunikation von Leistungsproblemen für die verbleibenden Teilnehmer erfolgt, da sie gezwungen werden, nicht zustellbare Nachrichten an das Gerät, das einfach "verschwunden" zu senden, indem Aufrufen von `xim::cleanup()`.

## <a name="sending-and-receiving-messages"></a>Senden und Empfangen von Nachrichten

Führen die mühsame Arbeit Sichern der Kommunikationskanäle über das Internet herzustellen, sodass Sie keine Probleme mit der Netzwerkverbindung oder werden einige, aber nicht alle Spieler erreichen kümmern, XIM und zugrunde liegenden Komponenten. Wenn alle grundlegenden Peer-zu-Peer-Verbindung Probleme auftreten, ist der Wechsel zu einem Netzwerk XIM nicht erfolgreich.

Wenn ein Netzwerk XIM formatiert ist, und bestätigt mit `xim_move_to_network_succeeded_state_change`, ist garantiert, dass alle Instanzen Ihrer app auf allen verbundenen Geräten über informiert werden alle `xim_player` mit dem XIM-Netzwerk verbunden und kann Nachrichten an diese senden.

Wir veranschaulichen, wie zum Senden von Nachrichten über das Netzwerk XIM. Im folgende Beispiel wird davon ausgegangen, dass die Variable "SendingPlayer" ein Zeiger auf eine gültige lokale Player-Objekt ist. Ruft "SendingPlayer" `local()` und ruft `send_data_to_other_players()` der Nachrichtenstruktur alle Spieler (lokal und remote) mit garantierter, geordnete Übermittlung "MsgData" an. Beachten Sie, dass es für alle Spieler wechselt, da eine Liste der spezifischen Spieler nicht an die Methode übergeben wurde:

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

Nachrichten können übermittelt werden, um alle Spieler mit `send_data_to_other_players()` durch ein Array von bestimmten Spieler nicht an die Methode übergeben.

Alle Empfänger der Nachricht dienen einem `xim_player_to_player_data_received_state_change` , enthält einen Zeiger auf eine Kopie der Daten, sowie Zeiger auf die entsprechende Xim_player Objekt, die sie gesendet, und erhalten sie lokal.

Natürlich garantiert, geordnete Übermittlung ist praktisch, aber einen Typ ineffizient senden, kann auch sein, da XIM muss erneut, oder Nachrichten verzögern, wenn Pakete gelöscht oder durch das Internet ungeordnete. Achten Sie darauf, dass Sie für den Einsatz von den anderen Arten von Sendeports für Nachrichten, die Ihre app tolerieren kann, verlieren oder having außerhalb der Reihenfolge eintreffen. Da die Nachrichtendaten von einem Remotecomputer aus stammen, werden die bewährte Methode eindeutig Datenformaten, wie etwa das Packen von Multi-Byte-Werte in einer bestimmten Bytereihenfolge ("endiancharakteristik") definiert haben. Die app sollte auch die Daten, bevor auf ihn überprüfen.

XIM bietet Sicherheit auf Zeilenebene Netzwerk besteht keine Notwendigkeit für zusätzliche Verschlüsselung oder Signatur-Schemas. Es wird jedoch empfohlen immer "Defense-in-Depth" einsetzen, um Schutz vor versehentlichen Anwendungsfehler und verschiedene Versionen Ihrer Anwendung Protokoll Koexistenz ordnungsgemäß (während der Entwicklung, Inhaltsupdates usw.) behandeln können. Internetverbindungen von Benutzern können eingeschränkt und sich ständig ändernden Ressourcen sein. Wir empfehlen dringend die Verwendung von effizienten meldungsdatenformate und vermeiden Entwürfe, die alle UI-Frames zu senden.

## <a name="assessing-data-pathway-quality"></a>Bewerten der Datenqualität-Pfad

Rufen Sie Informationen über die aktuellen Qualität der datenüberwachungspfad zwischen zwei Spielern auszutauschen, die `xim_player::network_path_information()` Methode. Im folgende Beispiel ruft einen Zeiger auf die `xim_network_path_information` -Struktur für eine `xim_player` Zeiger, die in der Variablen "RemotePlayer" enthalten:

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

Die zurückgegebene Struktur enthält die geschätzte Roundtrip-Latenz und wie viele Nachrichten weiterhin lokal für die Übertragung in die Groß-/Kleinschreibung in der Warteschlange befinden, dass die Unterstützung der Verbindung kann nicht mehr Daten übertragen werden für einen Moment.

Wenn Sie sehen, dass die Warteschlangen für einen bestimmten "Xim_player" Sichern, sollten Sie die Rate reduzieren, an der Sie Daten senden.

Die `xim_network_path_information::round_trip_latency_in_milliseconds` Feld repräsentiert die Latenz der zugrunde liegenden Netzwerk und XIMs Geschätzte Wartezeit ohne queuing. Effektive Latenz nimmt zu, als `xim_network_path_information::send_queue_size_in_messages` wächst und XIM funktioniert durch die Warteschlange.

Wählen Sie einen angemessenen zeigen Sie auf starten, Aufrufe von Drosselung `send_data_to_other_players` basierend auf Nutzung und die Anforderungen des Spiels. Jede Nachricht in der Sendewarteschlange stellt einen Anstieg bei der effektiven Netzwerkwartezeit dar.

Ein Wert in der Nähe XIMs maximal (derzeit 3500 Nachrichten) viel zu hohen bei den meisten Spielen und stellt wahrscheinlich einige Sekunden von Daten, die darauf warten, abhängig von der Rate der Aufruf gesendet werden `send_data_to_other_players` und wie groß jede Datennutzlast ist. Wählen Sie stattdessen eine Zahl, die das Spiel latenzanforderungen sowie wie ganz nervös des Spiels berücksichtigt `send_data_to_other_players` Aufrufmuster ist.

## <a name="sharing-data-using-player-custom-properties"></a>Freigeben von Daten mithilfe von benutzerdefinierten Eigenschaften der player

Die meisten app-Daten-Austausch auftreten, mit der `xim_player::xim_local::send_data_to_other_players()` Methode beeinträchtigt, da die umfassendsten kontrollmöglichkeiten über, die Daten empfängt, wenn sie diese Daten erhalten soll und wie das System Paketverlust behandeln soll. Es gibt jedoch noch Mal, wenn es nicht schön, auf dem Spieler basic "," nur selten ändernden Status über sich selbst mit minimalen anfällt für andere Benutzer freigeben, würde. Jeder Spieler möglicherweise z. B. eine feste Zeichenfolge, die ausgewählt werden, bevor Sie eingeben, dass alle Spieler verwenden, um deren Darstellung im Spiel Rendern multiplayer Zeichenmodell darstellt.

Für Daten, die nicht über einen Player sehr häufig ändern, bietet XIM Player über benutzerdefinierte Eigenschaften. Diese Eigenschaften bestehen aus einer app definierter Name und der Wert sind Null-terminierte Zeichenfolge-Paare, die angewendet werden kann, für den lokalen Player und, erhalten automatisch an alle Geräte weitergegeben, wenn sie geändert werden.

Eigenschaften von benutzerdefinierten Player haben weitere interne Synchronisierung Laufzeitaufwand als die `xim_player::xim_local::send_data_to_other_players()` -Methode, sodass rasch ändernden Daten (d. h. spielerposition), weiterhin direkte sendet verwenden sollten.

Player, benutzerdefinierte Eigenschaften der Schlüssel-Wert-Paare verfügen, deren aktuelle Werte, die automatisch auf neue beteiligten Geräte bereitgestellt werden, wenn diese Geräte XIM Netzwerk verbinden, und finden Sie unter der Spieler hinzugefügt. Werte können festgelegt werden, durch den Aufruf `xim_player::xim_local::set_player_custom_property()` mit Name-Wert-Zeichenfolgen. Im folgenden Beispiel legen wir eine Eigenschaft mit dem Namen "model", um den Wert "Brute" auf einem lokalen `xim_player` Objekt verweist die Variable "LocalPlayer":

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

Bewirkt, dass Änderungen an den Eigenschaften der Spieler eine `xim_player_custom_properties_changed_state_change` bereitgestellt werden, um alle Geräte, die aufmerksam machen, die den Namen der Eigenschaften, die geändert wurden. Der Wert für einen bestimmten Namen auf Player, lokal oder Remote, abgerufen werden kann, mit `xim_player::get_player_custom_property()`. Im folgende Beispiel ruft den Wert für eine Eigenschaft mit dem Namen "model" aus einer `xim_player` verweist die Variable "XimPlayer":

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

Festlegen eines neuen Werts für einen angegebenen Eigenschaftsnamen ersetzt alle vorhandenen Werte. Ein Eigenschaftenname festlegen, auf den Wert ein null-Wert-Zeichenfolgenzeiger behandelt identisch mit einem leeren Wert-Zeichenfolge, die die gleiche Weise wie die Eigenschaft an, dass noch nicht angegeben ist. Namen und Werte werden nicht von XIM interpretiert, daher verbleibt in der app, um den Inhalt der Zeichenfolge nach Bedarf zu überprüfen.

Eigenschaften von benutzerdefinierten Player sind immer zurückgesetzt werden, wenn von einem XIM-Netzwerk zu einer anderen wechseln. Jedoch erhalten neue Player, die das XIM Netzwerk einzudringen Eigenschaften der aktuellen benutzerdefinierten Player für alle Spieler, die über einige verfügen.

## <a name="sharing-data-using-network-custom-properties"></a>Freigeben von Daten mithilfe von benutzerdefinierten Eigenschaften des Netzwerks

Benutzerdefinierte Eigenschaften des Netzwerks dienen zur Vereinfachung Synchronisieren von Daten, die spezifisch für das XIM-Netzwerk, die nicht häufig ändern. Benutzerdefinierte Eigenschaften arbeiten genauso wie Network [benutzerdefinierte Eigenschaften der Player](#sharing-data-using-player-custom-properties), außer dass sie für das XIM Singleton-Objekt mit festgelegt sind `xim::set_network_custom_property()`. Im folgenden Beispiel wird eine "Zuordnung"-Eigenschaft den Wert "kundenbotschaften":

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

Bewirkt, dass Änderungen an Eigenschaften des Netzwerks ein `xim_network_custom_properties_changed_state_change` bereitgestellt werden, um alle Geräte, die aufmerksam machen, die den Namen der Eigenschaften, die geändert wurden. Der Wert für einen bestimmten Namen abgerufen werden kann, mit `xim::get_network_custom_property()`, wie im folgenden Beispiel, das den Wert für eine benannte Eigenschaft "Zuordnung" abruft:

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

Genau wie [benutzerdefinierte Eigenschaften der Player](#sharing-data-using-player-custom-properties), eine neue Einstellung für ein angegebenen Eigenschaftsnamen, alle vorhandenen Werte ersetzt wird. Ein Eigenschaftenname festlegen, auf den Wert ein null-Wert-Zeichenfolgenzeiger behandelt identisch mit einem leeren Wert-Zeichenfolge, die die gleiche Weise wie die Eigenschaft an, dass noch nicht angegeben ist. Namen und Werte werden nicht von XIM interpretiert, daher verbleibt in der app, um den Inhalt der Zeichenfolge nach Bedarf zu überprüfen.

Neu erstellte XIM Netzwerke beginnen immer mit kein Netzwerk über benutzerdefinierte Eigenschaften festgelegt. Die aktuellen Werte der Network benutzerdefinierter Eigenschaften für dieses Netzwerk XIM erhalten jedoch neue Player, die ein vorhandenes XIM Netzwerk beitreten.

## <a name="matchmaking-using-per-player-skill"></a>Mithilfe der Fähigkeiten von pro-Player Vermittlung

Spieler ist vom gemeinsamen Interesse an einem bestimmten Spiel angegebene app-Modus eine gute Basis Strategie. Wenn der Pool der verfügbaren Spieler zunimmt, sollten Sie erwägen, auch übereinstimmende Spieler basierend auf ihren persönlichen Qualifikation oder Erfahrung mit Ihr Spiel, sodass erfahrene Spieler die Herausforderung, fehlerfreien Wettbewerb mit anderen clusterexperten, profitieren können, während neuere Spieler von vergrößert werden kann ein Wettbewerb mit anderen ähnlichen Funktionen.

Zu diesem Zweck zu starten, durch die Bereitstellung der Qualifikation für alle lokalen Spieler in den pro-Player-Rollen und Fähigkeiten Konfigurationsstruktur, die in Aufrufen von angegeben `xim_player::xim_local::set_roles_and_skill_configuration()` vor dem Start in einem XIM verschieben Netzwerk mithilfe der Vermittlung. Kenntnisstand ist ein app-spezifisches Konzept und die Anzahl ist nicht vom XIM, interpretiert, mit dem Unterschied, dass Vermittlung wird zunächst versuchen, die Spieler mit den gleichen Wert für die Fähigkeiten finden, und erweitern Sie dann in regelmäßigen Abständen die Suche in Schritten von +/-10, um zu versuchen, andere Spieler deklarieren Fähigkeiten finden die Werte innerhalb eines Bereichs auf dieser Fähigkeit. Im folgende Beispiel wird vorausgesetzt, dass die lokale `xim_player` -Objekt, dessen Zeiger ist "LocalPlayer", weist einen zugeordneten app-spezifische uint32_t-Fähigkeiten-Wert einer lokalen oder Xbox Live-Speicher in eine Variable namens "PlayerSkillValue" abgerufen:

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Wenn dies abgeschlossen ist, werden alle Teilnehmer bereitgestellt werden eine `xim_player_roles_and_skill_configuration_changed_state_change` dies `xim_player` pro-Player-Rollen und Fähigkeiten-Konfiguration geändert hat. Der neue Wert abgerufen werden kann, durch den Aufruf `xim_player::roles_and_skill_configuration()`. Wenn alle Beteiligten ungleich Null von Rollen und Fähigkeiten Konfiguration angewendet haben, können Sie mit einem Wert "true", für die Vermittlung mit XIM-Netzwerk Verschieben der `require_player_roles_and_skill_configuration` Feld der `xim_matchmaking_configuration` Struktur angegeben, um `xim::move_to_network_using_matchmaking()`. Das folgende Beispiel füllt eine datenbankgesteuertes-Konfiguration, die insgesamt 2 bis 8-Player für eine ohne-Teams jeder gegen jeden, die ein Spielmodus app-spezifischen Konstanten uint64_t definiert durch den Wert MYGAMEMODE_DEATHMATCH, die nur entsprechen, werden mit anderen Spielern über gefunden wird Gibt an, dass der gleiche Wert, und dieser erfordert die pro-Player-Rollen und Fähigkeiten-Konfiguration:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Wenn diese Struktur dient der `xim::move_to_network_using_matchmaking()`, der Verschiebevorgang beginnt normalerweise so lange, wie Spieler verschieben aufgerufen haben `xim_player::xim_local::set_roles_and_skill_configuration()` mit einer nicht-Null `xim_player_roles_and_skill_configuration` Zeiger. Wenn ein Spieler nicht der Fall, dann der Prozess für die Vermittlung angehalten werden wird und alle Teilnehmer bereitgestellt werden eine `xim_matchmaking_progress_updated_state_change` mit einem `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` Wert. Dies schließt Player, die anschließend dem XIM Netzwerk über eine zuvor gesendete Einladung oder auf andere Weise beitreten (z. B. einen Aufruf von `xim::move_to_network_using_joinable_xbox_user_id()`) vor dem Abschluss der Vermittlung. Wenn alle Beteiligten bereitgestellt haben ihre `xim_player_roles_and_skill_configuration` Strukturen Vermittlung wird fortgesetzt.

Vermittlung, die mithilfe von pro-Player-Fähigkeiten kann auch mit pro-Player Benutzerrolle Vermittlung, kombiniert werden, wie im nächsten Abschnitt erläutert. Wenn nur eine gewünscht ist, können Sie den Wert 0 für die anderen angeben. Dies ist, da alle Spieler, deklarieren sie haben eine `xim_player_roles_and_skill_configuration` Fähigkeiten der Wert 0 werden immer übereinstimmen.

Nach der `xim::move_to_network_using_matchmaking()` oder einem beliebigen anderen XIM Netzwerk Verschieben der Vorgang abgeschlossen wurde, werden alle Spieler `xim_player_roles_and_skill_configuration` Strukturen werden automatisch gelöscht werden, um einen null-Zeiger (mit einem zugehörigen `xim_player_roles_and_skill_configuration_changed_state_change` Benachrichtigung). Wenn Sie mit einem anderen XIM-Netzwerk mit Vermittlung, die pro-Player-Konfiguration erfordert verschieben möchten, müssen Sie zum Aufrufen `xim_player::xim_local::set_roles_and_skill_configuration()` erneut mit einem neuen Struktur-Zeiger, die mit den neuesten Informationen.

## <a name="matchmaking-using-per-player-role"></a>Vermittlung mit pro-Player-Rolle

Eine andere Methode der Verwendung von pro-Player-Rollen und Konfiguration von Fähigkeiten zur Verbesserung der benutzererfahrung für die Vermittlung ist die Verwendung erforderlichen Player-Rollen. Dies ist am besten für Spiele, die auswählbar Zeichentypen bereitstellen, die andere kooperative Play Stile zu empfehlen. Typen, die keine grafische Darstellung im Spiel einfach ändern, aber komplementäre, beeindruckende Attribute wie z. B. defensive "Healers" im Vergleich zu Detail "Melee" offensiven im Vergleich zu entfernten "Bereich" steuern angreifen, also unterstützt. Benutzer persönlichkeiten bedeutet, dass es sich bei dieser bevorzugt, als eine bestimmte Spezialisierung wiedergeben können. Aber wenn Ihr Spiel ausgelegt ist, so, dass es funktionell nicht möglich, die Ziele, ohne mindestens einer Person, die jede Rolle erfüllen kann, manchmal ist es besser, solche Player zusammen zuerst als entsprechen alle Spieler zusammen klicken Sie dann zum Aushandeln der müssen übereinstimmen Spielen Sie Stile untereinander einmal gesammelt. Hierzu können Sie definieren von ein eindeutiger Bitflag jede Rolle in eines bestimmten Players angegeben werden `xim_player_roles_and_skill_configuration` Struktur.

Im folgenden Beispiel wird einen app-spezifische MYROLEBITFLAG_HEALER uint8_t Rolle Wert für das lokale Xim_player-Objekt, dessen Zeiger "LocalPlayer" ist:

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Wenn dies abgeschlossen ist, werden alle Teilnehmer bereitgestellt werden eine `xim_player_roles_and_skill_configuration_changed_state_change` dies `xim_player` dessen pro-Player-Rollenkonfiguration geändert hat. Der neue Wert abgerufen werden kann, durch den Aufruf `xim_player::roles_and_skill_configuration()`.

Die globale `xim_matchmaking_configuration` Struktur angegeben, um `xim::move_to_network_using_matchmaking()` sollte haben alle erforderlichen Rollen Flags kombiniert mit der bitweisen OR- und einem Wert "true" für das Feld "Require_player_matchmaking_configuration".

Das folgende Beispiel füllt eine datenbankgesteuertes-Konfiguration, die insgesamt 3 Playern für eine ohne-Teams jeder gegen jeden gefunden wird. Dieses Beispiel verwendet außerdem eine app definierte-Konstante, die ist vom Typ uint64_t und Namen der MYGAMEMODE_COOPERATIVE, die die Spiel-Modus zum Filtern der darstellt. Darüber hinaus wird die Konfiguration eingerichtet erforderlich ist, dass pro-Player-Rollen und Fähigkeiten-Konfiguration, in denen mindestens ein einziger Player für jede app-spezifische uint8_t-Rollen ausführt auf dem bitweisen OR-zusammen und in der Konfiguration (MYROLEBITFLAG_HEALER, MYROLEBITFLAG_ aufgenommen werden konnten MELEE und MYROLEBITFLAG_RANGE):

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Wenn diese Struktur dient der `xim::move_to_network_using_matchmaking()`, der Verschiebevorgang wird gestartet, wie oben beschrieben.

Vermittlung mit pro-Player-Rolle kann auch mit Vermittlung Benutzer pro-Player können kombiniert werden. Wenn nur eine gewünscht ist, geben Sie den Wert 0 für die anderen. Dies ist, da alle Spieler, deklarieren sie haben eine `xim_player_roles_and_skill_configuration` Fähigkeiten der Wert 0 wird immer übereinstimmen; und, wenn alle Bits auf 0 (null) in sind die `xim_matchmaking_configuration` Required_roles-Feld, und klicken Sie dann keine Bits für die Rolle erforderlich sind, um eine Übereinstimmung herzustellen.

Nach der `xim::move_to_network_using_matchmaking()` oder einem beliebigen anderen XIM Netzwerk Verschieben der Vorgang abgeschlossen wurde, werden alle Spieler `xim_player_roles_and_skill_configuration` Strukturen werden automatisch gelöscht werden, um einen null-Zeiger (mit einem zugehörigen `xim_player_roles_and_skill_configuration_changed_state_change` Benachrichtigung). Wenn Sie mit einem anderen XIM-Netzwerk mit Vermittlung, die pro-Player-Konfiguration erfordert verschieben möchten, müssen Sie zum Aufrufen `xim_player::xim_local::set_roles_and_skill_configuration()` erneut mit einem neuen Struktur-Zeiger, die mit den neuesten Informationen.

## <a name="how-xim-works-with-player-teams"></a>Funktionsweise von XIM mit Player-teams

Multiplayer-Spiele spielen häufig umfasst die Spieler auf die gegensätzlichen Teams organisiert. XIM macht es einfach, Zuweisen von teams, die bei der Vermittlung durch Festlegen von `xim_team_configuration`. Im folgende Beispiel wird eine Verschiebung mit Vermittlung, die so konfiguriert, dass insgesamt 8-Player finden, um auf zwei Teams gleich 4 zu platzieren, (obwohl es sich um eine Falls 4 nicht gefunden werden, 1 bis 3 auch akzeptabel sind in der Regel) initiiert, durch den Wert definiert ein Spielmodus app-spezifischen Konstanten uint64_t verwenden MYGAMEMODE_CAPTURETHEFLAG, die nur mit anderen Spielern, denselben Wert angeben und zum Zusammenführen von alle Social eingebundenen Spieler aus dem aktuellen XIM Netzwerk entsprechen:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Wenn solche ein XIM verschieben Netzwerkvorgang abgeschlossen ist, werden der Spieler einen Team Indexwert 1 durch {n} für die angeforderte {n}-Teams zugewiesen werden. Die wahre Bedeutung von bestimmten Team Indexwert ist Aufgabe der app. Team-Indexwert des Spielers abgerufen wird, über `xim_player::team_index()`. Bei Verwendung einer `xim_team_configuration` mit zwei oder mehr Teams Player werden nie ein Wert zugewiesen werden Team Index 0 (null), durch den Aufruf von `xim::move_to_network_using_matchmaking()`. Dies ist im Gegensatz dazu auf Player, die mit dem Netzwerk XIM mit alle anderen Konfigurationen oder Typ des Verschiebevorgangs hinzugefügt werden (z. B. über eine protokollaktivierung, die durch eine Einladung akzeptiert), besitzen, die immer einen Index mit der Team 0 (null). Es kann hilfreich sein, Team Index 0 als spezielle "nicht zugeordnete" Team behandeln sein.

Im folgende Beispiel wird den Team-Index für ein Xim_player-Objekt, dessen Zeiger in der Variablen "XimPlayer" wird, abgerufen:

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

Für die bevorzugte Benutzer (nicht zu erwähnen reduziert Verkaufschance für negative Player-Verhalten) die Xbox Live-Vermittlungsdienst wird nie Split-Playern, die mit einem Netzwerk XIM auf verschiedenen Teams zusammen verschoben werden.

Der Team-Indexwert, der ursprünglich vom Vermittlung zugewiesen ist, nur eine Empfehlung aus, und die app können Sie ändern, für lokale Spieler jederzeit `xim_player::xim_local::set_team_index()`. Dies kann auch in XIM Netzwerken aufgerufen werden, die Vermittlung nicht zu verwenden. Im folgenden Beispiel wird einen Player-Zeiger "LocalPlayer", um einen neuen Team Indexwert eines zu erhalten:

```cpp
localPlayer->local()->set_team_index(1);
```

Alle Geräte werden darüber informiert, dass der Spieler einen neuen Team Indexwert aktiviert hat, wenn sie bereitgestellt haben eine `xim_player_team_index_changed_state_change` für die betreffende Spieler. Rufen Sie `xim_player::xim_local::set_team_index()` zu einem beliebigen Zeitpunkt.

Vermittlung wertet pro Player erforderlichen Rollen unabhängig von den Teams. Aus diesem Grund ist es nicht empfohlen, um Teams und erforderlichen Rollen als Kriterium für gleichzeitige Vermittlung-Konfiguration zu verwenden, da die Teams nach Anzahl der Spieler, nicht von erfüllte Player Rollen ausgewogen ist.

## <a name="working-with-chat"></a>Arbeiten mit chat

Sprache und Text-Chat-Kommunikation werden automatisch für Spieler in einem Netzwerk XIM aktiviert. XIM verarbeitet die Interaktion mit gesamter Hardware für die Sprach-Kopfhörer und Mikrofon für Sie. Ihre app muss nicht viele Funktionen für Chat, jedoch verfügt es über die eine Anforderung in Bezug auf den Text-Chat: Unterstützung für Eingabe und Anzeige. Texteingabe ist erforderlich, da, sogar auf Plattformen oder Spiele Genres, die weit verbreitete physische Tastatur verwenden, in der Vergangenheit aufwiesen, Spieler das System, um die Sprachwiedergabe von Text hilfstechnologien verwenden konfigurieren. Auf ähnliche Weise ist Anzeigen von Text erforderlich, weil Spieler konfigurieren, dass das System, um die Sprache-zu-Text verwenden können.

Diese Einstellungen können auf lokale Unternehmen erkannt werden, durch den Aufruf `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` und `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()`, und Sie möchten die Text-Mechanismen bedingt zu aktivieren. Allerdings empfiehlt es sich, dass Sie erwägen, Text eingeben und Anzeigen von Optionen, die immer verfügbar sind.

 `Windows::Xbox::UI::Accessability` eine Xbox One Klasse wurde speziell für ein einfaches Rendering von Text mit in-Game-Chat mit dem Schwerpunkt auf hilfstechnologien Sprache-zu-Text bereitstellen.

Nachdem Sie die Texteingabe, die von einem physischen oder virtuellen Tastatur bereitgestellt haben, übergeben Sie die Zeichenfolge, die die `xim_player::xim_local::send_chat_text()` Methode. Der folgende code zeigt, senden eine Beispiel hartcodierte Zeichenfolge aus einer lokalen `xim_player` Objekt verweist die Variable "LocalPlayer":

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

Dieser Chat-Text wird an alle Beteiligten in das Netzwerk XIM übermittelt, die Chat-Kommunikation aus dem ursprünglichen lokalen Player empfangen kann. Es kann auf Audiodaten von Sprache synthetisiert werden, und es möglicherweise als bereitgestellt werden eine `xim_chat_text_received_state_change`.

Ihre app sollte eine Kopie jeder Textzeichenfolge, die empfangen und anzeigen sowie einige Identifizierung von der ursprünglichen Player für eine angemessene Zeit (oder in einem bildlauffähigen Fenster).

Es gibt auch einige bewährten Methoden bezüglich der Chat. Es wird empfohlen, dass an einer beliebigen Stelle Player, besonders in einer Liste von Gamertags wie z. B. eine spielstandsanzeige, angezeigt werden, dass auch stumm geschaltet/Vorträge Symbole als Feedback für den Benutzer angezeigt werden. Dies erfolgt durch Aufrufen von `xim_player::chat_indicator()` zum Abrufen einer `xim_player_chat_indicator` , die den aktuellen, sofortigen Status Chat für die betreffende Spieler darstellt. Das folgende Beispiel zeigt das Abrufen der Indikatorwert für den ein `xim_player` Objekt verweist die Variable "XimPlayer", um zu bestimmen, einen bestimmtes Symbol Konstantenwert zuweisen zu einer Variablen "IconToShow":

```cpp
switch (ximPlayer->chat_indicator())
{
   case xim_player_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

Der gemeldete Wert ist vom `xim_player::chat_indicator()` wird erwartet, dass Sie häufig als Player starten und beenden zu sprechen, z. B. ändern. Es soll Abrufen jedes UI-Frames daher-apps zu unterstützen.

> [!NOTE]
> Wenn ein lokaler Benutzer aufgrund der Einstellungen für ihre Geräte von Systemdiensten Kommunikation keine `xim_player::chat_indicator()` zurück `xim_player_chat_indicator::platform_restricted`. Der Annahme aus, um die Anforderungen für die Plattform ist, dass die app, der angibt, einer plattformeinschränkung für Voice Chat oder messaging und eine Meldung für Benutzer, der angibt, des Problems ikonographie anzeigen. Eine Beispielnachricht, die empfohlen wird: "Leider sind Sie nicht zulässig, jetzt chatten."

## <a name="muting-players"></a>Stummschaltung des players

Ein anderes bewährtes Verfahren ist zur Unterstützung der stummschaltung Spieler. XIM übernimmt automatisch das System stummschaltung von Benutzern durch Player Karten initiiert, aber apps sollten unterstützen spezifische vorübergehende stummschaltung, die auf der game-Benutzeroberfläche über ausgeführt werden können die `xim_player::set_chat_muted()` Methode. Im folgenden Beispiel zunächst eine stummschaltung `xim_player` Objekt verweist die Variable "RemotePlayer", damit keine Voice Chat hören ist und kein Textchat von ihm empfangen wird:

```cpp
remotePlayer->set_chat_muted(true);
```

Dieser Schalter nicht sofort in Kraft, und es gibt keine `xim_state_change` zugeordnet. Es kann rückgängig zu machen, durch den Aufruf `xim_player::set_chat_muted()` erneut mit dem Wert "false". Im folgende Beispiel unmutes eine `xim_player` Objekt verweist die Variable "RemotePlayer":

```cpp
remotePlayer->set_chat_muted(false);
```

Deaktiviert bleiben wirksam, solange die `xim_player` vorhanden ist, einschließlich beim Wechsel zu einem neuen XIM-Netzwerk mit den Player. Es wird nicht beibehalten, wenn der Spieler verlässt und demselben Benutzer wieder vereint (als neues `xim_player` Instanz).

Spieler starten in der Regel im Zustand "unmuted". Wenn möchte, dass Ihre app ein Spieler in der stumm geschaltet sind Zustand Gründen Spiel zu starten, können sie aufrufen `xim_player::set_chat_muted()` auf die `xim_player` Objekt vor dem Abschluss der Verarbeitung der zugeordneten `xim_player_joined_state_change`, und XIM wird sichergestellt, es werden keine Zeitraum, in denen voice-Audio vor der Spieler kann gehört.

Eine automatische stumm schalten Überprüfung basierend auf Player Reputation tritt auf, wenn es sich bei ein remote-Player im Netzwerk XIM verknüpft. Wenn der Spieler ein Flag in dem Ruf hat, ist der Spieler automatisch stumm geschaltet. Stummschaltung wirkt sich nur auf lokalen Status und daher weiterhin besteht, wenn ein Player über Netzwerke hinweg verschoben wird. Die automatische Bewertung zum Stummschalten-Prüfung wird einmal durchgeführt, und nicht neu ausgewertet werden erneut für solange die `xim_player` gültig bleibt.

## <a name="configuring-chat-targets-using-player-teams"></a>Konfigurieren die Spieler Teams über Chat-Ziele

Gemäß der [wie XIM funktioniert mit Player-Teams](#how-xim-works-with-player-teams) Abschnitt dieses Dokuments, die wahre Bedeutung von bestimmten Team Indexwert ist Aufgabe der app. XIM Interpretation nicht mit Ausnahme der Durchführung von Gleichheitsvergleichen in Bezug auf die Chat-Zielkonfiguration. Wenn die Chat-Zielkonfiguration gemeldet `xim::chat_targets()` befindet sich derzeit `xim_chat_targets::same_team_index_only`, und klicken Sie dann alle angegebenen Player nur Chat-Kommunikation mit einem anderen ausgetauscht werden sollen, wenn die beiden den gleichen Wert, der von gemeldet haben `xim_player::team_index()` (und Datenschutz / Richtlinie ermöglichen es).

Konservative und Unterstützung, die Wettbewerber-Szenarios, neu erstellte XIM Netzwerke automatisch werden konfiguriert. Standardmäßig ist `xim_chat_targets::same_team_index_only`. Chatten mit vanquished Gegner auf dem anderen Team kann jedoch wünschenswert, z. B. in einem nach der Spiel "Lobby" sein. Können Sie anweisen, XIM, wenden Sie sich an alle anderen Benutzer (wo Datenschutz und Richtlinie zulassen), durch den Aufruf erlauben% `xim::set_chat_targets()`. Im folgende Beispiel beginnt mit der Konfiguration alle Teilnehmer im Netzwerk XIM verwenden eine `xim_chat_targets::all_players` Wert:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Alle Teilnehmer werden informiert, dass eine neue zieleinstellung für das gültig ist, wenn sie bereitgestellt sind eine `xim_chat_targets_changed_state_change`.

Wie bereits erwähnt, werden die meisten XIM Netzwerk verschieben anfänglich alle Spieler den Team-Indexwert von 0 (null) zuweisen. Dies bedeutet, dass eine Konfiguration des `xim_chat_targets::same_team_index_only` ist wahrscheinlich nicht von Unterschieden `xim_chat_targets::all_players` standardmäßig. Player, die mit einem XIM-Netzwerk, die mithilfe der Vermittlung verschieben müssen jedoch unterschiedliche Indexwerte team, wenn die Vermittlung der Konfiguration `xim_team_matchmaking_mode` Wert deklariert zwei oder mehr Teams. Sie können auch aufrufen `xim_player::xim_local::set_team_index()` zu einem beliebigen Zeitpunkt wie oben gezeigt. Wenn Ihre app nicht-NULL-Team Indexwerte über beide Methoden verwendet wird, vergessen Sie nicht zum Verwalten von der aktuellen Chat-Ziele, die entsprechend festlegen.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>Automatische Ausfüllen von Player-Slots ("Abgleich" Vermittlung)

Trennung von Gruppen von Spielern Aufrufen `xim::move_to_network_using_matchmaking()` gleichzeitig bietet der Xbox Live-Vermittlungsdienst die größte Flexibilität, um sie schnell in neuen, optimalen XIM Netzwerke zu organisieren. Allerdings möchten einige Gaming-Szenarien zu einem bestimmten XIM-Netzwerk intakt, und nur Zusammenarbeit zusätzliche Spieler, nur um freien Player Slots zu füllen. XIM unterstützt das Konfigurieren der Vermittlung, für die Ausführung im Modus oder "generellen", durch den Aufruf ausfüllen automatisch im Hintergrund `xim::set_network_configuration()` mit einem `xim_network_configuration` , bei dem `xim_allowed_player_joins::matchmade` flagssatz auf seine `xim_network_configuration::allowed_player_joins` Eigenschaft.

Im folgenden Beispiel wird die Vermittlung Abgleich, um zu versuchen, insgesamt 8 Playern für eine ohne-Teams jeder gegen jeden zu finden, (obwohl es sich um eine Falls 8 nicht gefunden werden, 2 bis 7 auch akzeptabel sind in der Regel), durch den Wert MYGAMEMODE_ definiert ein Spielmodus app-spezifischen Konstanten uint64_t verwenden DEATHMATCH, die nur mit anderen Spielern angeben dieser Wert entspricht:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Dadurch werden die vorhandenen XIM Netzwerk verfügbar, auf Geräten Aufrufen `xim::move_to_network_using_matchmaking()` auf normale Weise. Diese Geräte finden Sie unter kein Verhalten zu ändern. Die Teilnehmern im Netzwerk XIM generellen nicht verschoben, aber es erfolgt eine `xim_network_configuration_changed_state_change` überprüft, die auf einschalten Abgleich als auch mehrere `xim_matchmaking_progress_updated_state_change` Benachrichtigungen, falls zutreffend. Alle Matchmade Player wird hinzugefügt werden, mit dem XIM-Netzwerk mit dem normalen `xim_player_joined_state_change`.

Standardmäßig bleibt Vermittlung der Abgleich ausgeführt auf unbestimmte Zeit, auch wenn es nicht mehr Spieler hinzufügen, wenn das Netzwerk XIM bereits die maximale Anzahl der Spieler, die von der Einstellung für Xim_team_configuration angegeben hat versucht. Organisationsidentität kann durch Einstellung Xim_allowed_player_joins Matchmade nicht erlauben deaktiviert werden. Das folgende Beispiel deaktiviert die generellen deaktivieren Sie das Flag xim_allowed_player_joins::matchmade während alle anderen vorhandenen Flags und Netzwerk-Konfigurationseinstellungen beibehalten.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Eine entsprechende `xim_network_configuration_changed_state_change` erfolgt für alle Geräte, und nachdem dieser asynchrone Vorgang abgeschlossen ist, eine endgültige `xim_matchmaking_progress_updated_state_change` erfolgt mit `xim_matchmaking_status::none` anzeigen, dass keine weiteren Matchmade-Player im Netzwerk XIM hinzugefügt werden.

Beim Aktivieren der Vermittlung der Abgleich mit einem `xim_team_configuration` festlegen, die zwei oder mehr Teams deklariert ist, alle vorhandene Playern müssen einen gültigen Teamprojektnamen-Index, der zwischen 1 und die Anzahl der Teams ist. Dies schließt die Spieler mit aufgerufen haben `xim_player::xim_local::set_team_index()` , geben Sie einen benutzerdefinierten Wert oder die, die mittels einer Einladung wurde verknüpft werden oder über andere Social hat (z. B. einen Aufruf von `xim::move_to_network_using_joinable_xbox_user_id`) und mit einem Standardwert für die Index 0 hinzugefügt wurden. Wenn ein Spieler verfügt nicht über einen gültigen Teamprojektnamen-Index, und klicken Sie dann der Vermittlung Prozess angehalten und alle Teilnehmer bereitgestellt werden eine `xim_matchmaking_progress_updated_state_change` mit einem `xim_matchmaking_status::waiting_for_player_team_index` Wert. Sobald alle Spieler bereitgestellt oder ihre Team-Indexwerte mit korrigiert haben `xim_player::xim_local::set_team_index()`, Abgleich Vermittlung wird fortgesetzt. Weitere Informationen finden Sie der [wie XIM funktioniert mit Player-Teams](#how-xim-works-with-player-teams) Abschnitt dieses Dokuments.

Auf ähnliche Weise beim Aktivieren der Vermittlung der Abgleich mit einer `xim_network_configuration` Struktur mit der `require_player_roles_and_skill_configuration` Festlegung auf "true" für Rollen oder die Fähigkeit, Feld, und klicken Sie dann alle Spieler eine nicht-Null-pro-Player Vermittlung-Konfiguration angegeben ist müssen. Wenn ein Spieler nicht der Fall, dann der Prozess für die Vermittlung angehalten werden wird und alle Teilnehmer bereitgestellt werden eine `xim_matchmaking_progress_updated_state_change` mit einem `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` Wert. Wenn alle Beteiligten bereitgestellt haben ihre `xim_player_roles_and_skill_configuration` Strukturen Abgleich Vermittlung wird fortgesetzt. Weitere Informationen finden Sie der [Vermittlung, die mithilfe der Fähigkeiten von pro-Player](#matchmaking-using-per-player-skill) und [Vermittlung, die pro-Player-Rolle mit](#matchmaking-using-per-player-role) Abschnitten dieses Dokuments.

## <a name="querying-joinable-networks"></a>Abfragen von beigetreten Netzwerke

Zwar Vermittlung eine hervorragende Möglichkeit, Spieler schnell miteinander verbinden, ist manchmal es am besten, Spieler beigetreten Netzwerke, die mithilfe von benutzerdefinierten Suchkriterien ermitteln können, und wählen Sie das Netzwerk, die, das Sie verknüpfen möchten. Dies kann besonders vorteilhaft sein, wenn eine Spiele-Sitzung ein breites Spektrum an konfigurierbaren Spiel Regeln und Player-Einstellungen möglicherweise. Zu diesem Zweck ein vorhandenes Netzwerk muss zuerst erfolgen kann abgefragt werden durch Aktivieren der `xim_allowed_player_joins::queried` Joinability und konfigurieren die Netzwerkinformationen für andere Personen verfügbar, außerhalb des Netzwerks durch einen Aufruf von `xim::set_network_configuration()`.

Im folgenden Beispiel wird `xim_allowed_player_joins::queried` Joinability, legt Netzwerkkonfiguration mit einer Teamkonfiguration, die eine Summe von 1 bis 8 Spielern zusammen in 1-Team, ein Spielmodus app-spezifischen Konstanten uint64_t definiert durch den Wert GAME_MODE_BRAWL, ermöglicht eine Beschreibung "Cat" und "des Schaf Boxing Übereinstimmung mit", ein app-spezifische Zuordnung Index Konstanten uint32_t definiert durch den Wert MAP_KITCHEN und Spectatorallowed"Tags"Chatrequired","einfach"," enthält:

```cpp
PCWSTR tags[] = { L"chatrequired", L"easy", L"spectatorallowed" };
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::queried;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 1;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = GAME_MODE_BRAWL;
networkConfiguration.description = L"cat and sheep's boxing match";
networkConfiguration.map_index = MAP_KITCHEN;
networkConfiguration.tag_count = _countof(tags);
networkConfiguration.tags = tags;

xim::set_network_configuration(&networkConfiguration);
```

Weitere Entwicklungen außerhalb des Netzwerks können dann das Netzwerk durch den Aufruf finden `xim::start_joinable_network_query()` mit einem Satz von Filtern, die die Informationen in der vorherigen entsprechen `xim::set_network_configuration()` aufrufen. Im folgende Beispiel wird eine Abfrage für die beigetreten Netzwerk gestartet, mit der Spielmodus Filteroption, die nur für Netzwerke, die mit der app-spezifische Spielmodus definiert, nach Wert GAME_MODE_BRAWL abgefragt werden:

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

Hier ist ein weiteres Beispiel, das Tag filtern können Abfragen für Netzwerke, die mit Tag "einfach" und "Spectatorallowed" in ihrer öffentlichen abfragbare Konfiguration verwendet:

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

Optionen für verschiedene Filter können auch kombiniert werden. Im folgenden Beispiel, das sowohl die Spielmodus Filteroption aus, und kennzeichnen verwendet Filtern Option aus, um eine Abfrage für Netzwerke, dass sowohl die Spielmodus app-spezifische Konstante GAME_MODE_BRAWL und Tag "einfach" zu starten:

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

Wenn der Abfragevorgang erfolgreich ist, erhält die app eine `xim_start_joinable_network_query_completed_state_change` aus der die app eine Liste der beigetreten werden kann Netzwerke abrufen kann. Die app wird außerdem kontinuierlich empfangen `xim_joinable_network_query_updated_state_change` für zusätzliche beigetreten Netzwerke oder Änderungen, die auf die zurückgegebene Liste von beigetreten Netzwerken auftreten, bis er entweder manuell oder automatisch beendet wird. Die Abfrage in Ausführung befindlichen kann manuell beendet werden, durch den Aufruf `xim::stop_joinable_network_query()`. Es wird automatisch beendet beim Aufrufen von `xim::start_joinable_network_query()` um eine neue Abfrage zu starten.

Die app kann versuchen, ein Netzwerk in der Liste der beigetreten werden kann Netzwerke durch Aufrufen von verknüpfen `xim::move_to_network_using_joinable_network_information()`. Im folgende Beispiel wird angenommen, Sie verknüpfen möchten eine `xim_joinable_network_information` , die durch Zeiger "SelectedNetwork" nicht durch eine Kennung gesichert ist (also "nullptr" an den zweiten Parameter übergeben werden):

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

Beim Aktivieren der Netzwerk-Abfrage mit einem Xim_team_configuration, der zwei oder mehr Teams deklariert, Playern hinzugefügt, durch den Aufruf `xim::move_to_network_using_joinable_network_information()` erhalten einen Standardwert für die Index 0.

> [!NOTE]
> Wenn die app verfügt über mehrere lokale Benutzer angegeben und ist ein Netzwerk mit weniger Platz als die Anzahl der lokalen Benutzer verknüpfen, kann die Verknüpfung weiterhin erfolgreich. Aber nur die zulässige Anzahl von lokale Benutzer kann mit dem Netzwerk.