---
title: Mithilfe von XIM (C#)
description: Erfahren Sie, wie Sie mit der Xbox integrierte Multiplayer-Spiele (XIM) mit C#.
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox integrierte Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: 92ac7b9897b57de42fa56126b477f4db5b9b74dd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619435"
---
# <a name="using-xim-c"></a>Mithilfe von XIM (C#)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

Dies ist eine kurze exemplarische Vorgehensweise zur Verwendung des XIM C# API. Spiele-Entwickler, XIM über C++ zugreifen sollte [mithilfe XIM (C++)](using-xim.md).
- [Mithilfe von XIM (C#)](#using-xim-c)
    - [Erforderliche Komponenten](#prerequisites)
    - [Initialisieren und starten](#initialization-and-startup)
    - [Asynchrone Vorgänge und Verarbeitung von statusänderungen](#asynchronous-operations-and-processing-state-changes)
    - [Grundlegende IXimPlayer Behandlung](#basic-iximplayer-handling)
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

## <a name="initialization-and-startup"></a>Initialisieren und starten

Sie beginnen, interagieren mit der Bibliothek, durch die statischen Klasseneigenschaften XIM durch Ihre Xbox Live-Dienst Konfigurations-ID-Zeichenfolge zu initialisieren und den Titel des ID-Nummer. Wenn Sie auch die Xbox-API verwenden, es können bequem abzurufenden diese vom `Microsoft.Xbox.Services.XboxLiveAppConfiguration`. Im folgende Beispiel wird davon ausgegangen, dass die Werte bereits bzw. in der Variablen 'MyServiceConfigurationId' und "MyTitleId" befinden:

```cs
XboxIntegratedMultiplayer.ServiceConfigurationId = myServiceConfigurationId;
XboxIntegratedMultiplayer.TitleId = myTitleId;
```

Nach der Initialisierung die app sollten abrufen, die Xbox-Benutzer-ID-Zeichenfolgen für alle Benutzer derzeit auf dem lokalen Gerät, das teilnehmen, und übergeben sie einen `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` aufrufen. Im folgenden Beispielcode wird davon ausgegangen werden, ein einzelnen Benutzer verfügt über eine Absicht, spielen Ausdrücken Controller-Schaltfläche gedrückt, und die Xbox-Benutzer-ID-Zeichenfolge, die dem Benutzer zugeordneten bereits in eine "MyXuid"-Variable abgerufen wurde:

```cs
XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds(new List<string>() { myXuid });
```

Ein Aufruf von `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` sofort legt die Xbox-Benutzer-IDs verknüpft ist, mit der lokalen Benutzer, die mit dem Netzwerk XIM hinzugefügt werden soll. Diese Liste von Xbox-Benutzer-IDs werden in alle zukünftigen Netzwerkvorgänge verwendet werden, bis die Änderungen der Liste durch einen anderen Aufruf von `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`.

Im Fall, in denen kein Netzwerk XIM überhaupt noch vorhanden ist, ist im ersten Schritt mit einem Netzwerk XIM verschieben, um diese gestarteten Prozess zu erhalten. Wenn der Benutzer noch ein bestimmtes XIM Netzwerk Denken Sie daran nicht, ist es empfehlenswert, einfach in ein neues, leeres Netzwerk zu verschieben. Dadurch wird die Freunde des Benutzers, mit dem Netzwerk als eine Art "Lobby" aus dem sie miteinander zusammenarbeiten können, um ihre nächste Multiplayer-Aktivität (z. B. durch Eingabe von Vermittlung) zu wählen.

Im folgenden ist ein Beispiel zum Verschieben Sie nur die lokalen Benutzer, die zuvor hinzugefügte mit `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` mit einem leeren XIM-Netzwerk mit Platz für bis zu 8 gesamt Spieler:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringOnlyLocalPlayers);
```

Der Aufruf von `XboxIntegratedMultiplayer.MovetoNewNetwork()` initiiert einen asynchronen Vorgang "verschieben", der mit einem Statusänderungsereignis abgeschlossen ist, die Entwickler für regelmäßig verarbeitet werden sollen.

## <a name="asynchronous-operations-and-processing-state-changes"></a>Asynchrone Vorgänge und Verarbeitung von statusänderungen

Das Herzstück der XIM ist der app reguläre und häufige Aufrufe der `XboxIntegratedMultiplayer.GetStateChanges()` Methode. Diese Methode ist, wie XIM informiert wird, dass die app zur Verarbeitung von Updates für mehrere Spieler Zustand bereit ist, und wie XIM diese Updates durch Rückgabe ermöglicht eine `XimStateChangeCollection` -Objekt, das alle in der Warteschlange Updates enthält. `XboxIntegratedMultiplayer.GetStateChanges()` Dient zum schnell ausgeführt werden, sodass einzelbildweise Grafiken in der UI-Rendering-Schleife aufgerufen werden kann. Dadurch wird einen praktischer Ort für alle in der Warteschlange stehende Änderungen abrufen, ohne sich Gedanken Netzwerk Zeitangaben oder Multithread-Rückruf Komplexität nicht vorhersagbar sind. Die XIM-API ist für dieses Muster Singlethread-tatsächlich optimiert. Außerdem gewährleistet Sie seinen Status bleibt unverändert, während eine `XimStateChangeCollection` Objekt Prozess wird und noch nicht freigegeben wurde.

Die `XimStateChangeCollection` ist eine Sammlung von `IXimStateChange` Objekte.
Apps sollten Folgendes beachten:

1. Durchlaufen Sie das Array ein.
1. Überprüfen Sie die `IXimStateChange` für den spezifischeren Typ.
1. Umwandlung der `IXimStateChange` Typ an den entsprechenden Typ ausführlichere.
1. Behandeln Sie das Update nach Bedarf.

Wenn alle fertig gestellt das `IXimStateChange` Objekte, die derzeit verfügbaren, dieses Array übergeben werden an XIM auf die Ressourcen freigeben, indem `XimStateChangeCollection.Dispose()`. Die `using` Anweisung wird empfohlen, damit die Ressourcen auf jeden Fall nach der Verarbeitung gelöscht werden. Zum Beispiel:

```cs
using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
{
    foreach (var stateChange in stateChanges)
    {
        switch (stateChange.Type)
        {
            case XimStateChangeType.PlayerJoined:
                HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                break;

            case XimStateChangeType.PlayerLeft:
                HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                break;

            ...
        }
    }
}
```

Nun, da Sie Ihre grundlegenden Verarbeitungsschleife verfügen, können Sie die statusänderungen im Zusammenhang mit dem ersten behandeln `XboxIntegratedMultiplayer.MoveToNewNetwork()` Vorgang. Jeder XIM Netzwerk Move-Vorgang beginnt mit einem `XimMoveToNetworkStartingStateChange`. Wenn die Verschiebung aus irgendeinem Grund fehlschlägt, wird Ihre app bereitgestellt werden eine `XimNetworkExitedStateChange`, die den Fehlerbehandlungsmechanismus für ein asynchroner Fehler, die verhindern, dass Sie mit einem Netzwerk XIM verschieben oder trennt die Verbindung aus dem aktuellen XIM-Netzwerk mit häufiger Fehler ist. Andernfalls wird der Verschiebevorgang abgeschlossen, mit einem `XimMoveToNetworkSucceededStateChange` nachdem alle Zustände fertig gestellt wurde wurde, und alle Spieler erfolgreich mit dem Netzwerk XIM hinzugefügt wurden.

Wenn ein Benutzer nicht über die Plattform-Berechtigung für die Wiedergabe mit anderen Personen in einer Multiplayer-Sitzung verfügt, auf einem Gerät, das beim Erstellen oder nutzen Sie ein Netzwerk XIM XIM sendet eine `XimNetworkExitedStateChange` mit dem Grund `UserMultiplayerRestricted`. Dies geschieht, wenn die lokalen Benutzer mit festlegen `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` eine Xbox Live Multiplayer-Einschränkung aufweisen. Sie angemessene Behandlung der von der Benutzer das Problem informiert.

## <a name="basic-iximplayer-handling"></a>Grundlegende IXimPlayer Behandlung

Sofern das Beispiel für ein einzelner lokaler Benutzer mit einem neuen XIM Netzwerk verschieben erfolgreich ausgeführt wurde, wird Ihre app bereitgestellt werden eine `XimPlayerJoinedStateChange` für eine lokale `XimLocalPlayer` Objekt. Dieses Objekt bleibt gültig, bis das entsprechende `XimPlayerLeftStateChange` für sie bereitgestellt und über zurückgegeben wurde `XimStateChangeCollection.Dispose()`. Ihrer app erfolgt immer eine `XimPlayerLeftStateChange` für jede `XimPlayerJoinedStateChange`.

Sie können auch eine Liste mit allen abrufen `IXimPlayer` Objekte in der XIM Netzwerk zu einem beliebigen Zeitpunkt mit `XboxIntegratedMultiplayer.GetPlayers()`.

Die `IXimPlayer` Objekt hat viele nützliche Eigenschaften und Methoden, wie z. B. `IXimPlayer.Gamertag()` für den Player zu Anzeigezwecken Abrufen der aktuellen Zeichenfolge für die Xbox Live-Gamertag zugeordnet werden. Wenn die `IXimPlayer` wird lokal auf dem Gerät, IXimPlayer.Local gibt "true" zurück. Eine lokale `IXimPlayer` umgewandelt werden kann, um eine `XimLocalPlayer`, die über zusätzliche Methoden, die nur verfügbar für lokale Unternehmen verfügt.

Der wichtigste Status für Spieler ist natürlich nicht, die allgemeine Informationen, die XIM bekannt sind, aber was Ihre spezifische app nachverfolgt werden sollen. Da Sie wahrscheinlich Ihre eigenen Konstrukt für diese Nachverfolgungsinformationen verfügen, sollten Sie verknüpfen die `IXimPlayer` -Objekt in Ihren eigenen Player-Konstrukt-Objekt, damit jedes Mal, wenn XIM Berichte ein `IXimPlayer`, können Sie schnell den Status abrufen, ohne ausführen zu müssen eine die Suche durch Festlegen eines benutzerdefinierten Players Context-Objekt. Im folgende Beispiel wird angenommen, ein Objekt, das mit Ihrer privaten Status wird in der Variablen "MyPlayerStateObject" und der neu hinzugefügten `IXimPlayer` Objekt befindet sich in der Variablen "NewXimPlayer":

```cs
newXimPlayer.CustomPlayerContext = myPlayerStateObject;
```

Dadurch speichert das angegebene Objekt mit dem Playerobjekt lokal (wird nie übertragen über das Netzwerk für externe Geräte, in dem der Arbeitsspeicher gültig wäre nicht). Sie müssen dann immer wieder auf Ihr Objekt abrufen, indem benutzerdefinierten Kontext abrufen und ihn wieder auf Ihr Objekt wie im folgenden Beispiel umzuwandeln können:

```cs
myPlayerStateObject = (MyPlayerState)(newXimPlayer.CustomPlayerContext);
```

Sie können dieser benutzerdefinierten Player-Kontext zu einem beliebigen Zeitpunkt ändern.

## <a name="enabling-friends-to-join-and-inviting-them"></a>Aktivieren der join von Freunden und einladen

Für Datenschutz und Sicherheit alle neuen XIM Netzwerke werden automatisch konfiguriert, werden standardmäßig von lokalen Playern nur beigetreten sein, und es liegt an der app, die explizit andere können, sobald er fertig ist. Das folgende Beispiel zeigt, wie Sie mit `XboxIntegratedMultiplayer.NetworkConfiguration` zum Abrufen von der aktuellen Netzwerkkonfiguration und aktualisieren die Joinability mit `XboxIntegratedMultiplayer.SetNetworkConfiguration()` zu beginnen, sodass neue lokale Benutzer als Spieler hinzufügen sowie andere Benutzer, die eingeladen wurden oder werden, die " Folgen "(eine Xbox Live soziale Beziehung), von Playern bereits im Netzwerk XIM:

```cs
XimNetworkConfiguration currentConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
currentConfiguration.AllowedPlayerJoins = XimAllowedPlayerJoins.Local | XimAllowedPlayerJoins.Invited | XimAllowedPlayerJoins.Followed;
XboxIntegratedMultiplayer.SetNetworkConfiguration(currentConfiguration);
```

`XboxIntegratedMultiplayer.SetNetworkConfiguration()` führt asynchron aus. Wenn der vorherige Code-Beispiel-Aufruf abgeschlossen ist, eine `XimNetworkConfigurationChangedStateChange` wird bereitgestellt, um die app zu benachrichtigen, die der Joinability-Wert von seinem Standardwert geändert hat `XimAllowedPlayerJoins.None`. Sie können dann den neuen Wert durch Überprüfung des Werts der Abfragen `XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins`.

`XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins` kann überprüft werden, während das Gerät in einem Netzwerk XIM, um zu bestimmen, die Joinability des Netzwerks ist.

Sollte einen der lokalen Player, Einladungen für Remotebenutzer an mit diesem XIM Netzwerk senden möchten, kann die app Aufrufen `XimLocalPlayer.ShowInviteUI()` um die Einladung System Benutzeroberfläche zu starten. Der lokale Benutzer kann hier Personen auswählen, die sie einladen und Einladungen senden möchten.

```cs
XimLocalPlayer.ShowInviteUI();
```

Nachdem die oben genannte Anweisung ausgeführt wurde, wird die Einladung System Benutzeroberfläche angezeigt. Sobald der Benutzer hat die Einladungen gesendet (oder andernfalls beendet die Benutzeroberfläche), eine `xim_show_invite_ui_completed_state_change` bereitgestellt. Alternativ kann Ihre app direkt mit einer Einladung senden `XimLocalPlayer.InviteUsers()` ohne Einbindung der System-einladungs Benutzeroberfläche.

In beiden Fällen werden der Remotebenutzer eine Xbox Live einladungsnachrichten empfangen, wo sie angemeldet sind, und akzeptieren können. Dadurch wird Ihre app auf diesen Geräten gestartet, wenn es nicht bereits ausgeführt wird, und "Protocol aktivieren" mit der Ereignisargumente, die verwendet werden können, um mit diesem gleichen XIM-Netzwerk zu verschieben.

Finden Sie die Dokumentation zur Plattform für Weitere Informationen auf protokollaktivierung selbst.

Das folgende Beispiel zeigt, wie ein Aufruf `XboxIntegratedMultiplayer.ExtractProtocolActivationInformation()` bestimmt, ob die Ereignisargumente für XIM gelten. Dies setzt voraus, die unformatierte URI-Zeichenfolge aus abgerufenen `Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs` auf eine Variable "UriString":

```cs
XimProtocolActivationInformation protocolActivationInformation;
XboxIntegratedMultiplayer.ExtractProtocolActivationInformation(uriString, out protocolActivationInformation);
if (protocolActivationInformation != null)
{
    // It is a XIM activation.
}
```

Wenn sie einen aktivierten XIM und dann wird den lokalen Benutzer identifiziert, die in der 'LocalXboxUserId'-Eigenschaft des sicherstellen möchten die `XimProtocolActivationInformation` Objekt angemeldet ist und wird von den Benutzern angegeben, um `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`. Sie können dann initiieren, mit dem angegebenen XIM-Netzwerk mit einem Aufruf von verschieben `XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` unter Verwendung der gleichen URI-Zeichenfolge. Zum Beispiel:

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs(uriString);
```

Beachten Sie, die "gefolgt von remote-Benutzer" können Sie auch navigieren Sie in die Benutzeroberfläche des Systems zu Player-Karte des lokalen Benutzers und initiieren eine joinversuch selbst, ohne eine Einladung (vorausgesetzt, dass Sie solche Player-Joins wie oben gezeigt zugelassen haben). Diese Aktion wird außerdem Protokoll aktivieren Sie Ihre app einfach wie Einladungen und in die gleiche Weise behandelt werden.

Wechsel zu einem XIM-Netzwerk mithilfe von protokollaktivierung ist identisch mit der Umstellung auf ein neues XIM-Netzwerk, siehe [Initialisierungs- und Start](#initialization-and-startup). Der einzige Unterschied ist, wenn die Verschiebung erfolgreich ist, das Gerät verschieben sowohl lokale als auch einen Player bereitgestellt wird `XimPlayerJoinedStateChange` Objekte, die die entsprechenden Spieler darstellen. Natürlich ist das ursprüngliche Gerät, das bereits im Netzwerk XIM war wird nicht verschoben werden, aber sehen die Benutzer für das neue Gerät als Playern über zusätzliche hinzugefügt werden `XimPlayerJoinedStateChange` Objekte.

Nachdem Sie den spielerverlauf auf verschiedenen Geräten in einem Netzwerk XIM verknüpft werden, können sie multiplayer-Szenarien initiieren, kommunizieren über Sprache und Text automatisch und app-spezifische Nachrichten senden.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>Grundlegende Vermittlung und das Verschieben in ein anderes XIM-Netzwerk mit anderen Benutzern

Sie können die netzwerkerlebnis für eine Gruppe von Freunden weiter erweitern, durch das Verschieben der Spieler mit einem XIM-Netzwerk, das auch völlig fremden verfügt auch über. Fremden sind Gegnern von auf der ganzen Welt, die zusammen mit dem Xbox Live-Vermittlungsdienst basierend auf ähnlichen Interessen hochgefahren werden.

Eine einfache Möglichkeit, grundlegende Vermittlung mit XIM initiiert wird, durch den Aufruf `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` auf eines der Geräte mit einem gefüllten `MatchmakingConfiguration` Objekt, das die Spieler aus dem aktuellen XIM-Netzwerk mit.

Im folgende Beispiel startet eine Verschiebung mit einer Vermittlung-Konfiguration einrichten, insgesamt 8 Playern für eine ohne-Teams jeder gegen jeden Übereinstimmung zu finden. Wenn 8 gesamt Spieler nicht gefunden werden, kann das System 2 bis 7 Spieler noch zusammen übereinstimmen. Dieses Beispiel verwendet eine Konstante-app definiert der Typ "uint", mit dem Namen MYGAMEMODE_DEATHMATCH, die die Spiel-Modus zum Deaktivieren von Filtern darstellt. Der XIM Vermittlung werden nur dieses Netzwerk mit anderen Spielern gibt an, dass der gleiche Wert und schaltet entlang der Spieler alle Social eingebunden, aus dem aktuellen XIM-Netzwerk, bei der Bereitstellung des zweiten Parameters abgeglichen `XimPlayersToMove.BringExistingSocialPlayers` zu `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`:

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

Wie weiter oben verschiebt, dadurch ermöglichen Sie einen anfänglichen `MoveToNetworkStartingStateChange` auf allen Geräten und einem `MoveToNetworkSucceededStateChange` nach dem erfolgreichen der Verschiebung Abschluss. Da dies eine Verschiebung aus einem XIM-Netzwerk in ein anderes ist, ein Unterschied besteht darin, die bereits vorhandene `IXimPlayer` Objekte, die für lokale und remote Benutzer hinzugefügt werden soll, und diese bleibt für alle Spieler, die zusammen mit dem neuen XIM Netzwerk verschieben. Chat und Daten Kommunikation zwischen den Spieler weiterhin ununterbrochen weiterarbeiten, während Vermittlung ausgeführt wird.

Vermittlung kann abhängig von der Anzahl der potenziellen Player im Pool Vermittlung, die so genannte haben ein langwieriger Prozess sein `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`.

Ein `XimMatchmakingProgressUpdatedStateChange` erfolgt in regelmäßigen Abständen während der Operation, Sie und Ihre Benutzer, die mit dem aktuellen Status auf dem Laufenden zu halten. Wenn die Übereinstimmung gefunden wurde, werden zusätzliche Spieler das XIM-Netzwerk mit den normalen hinzugefügt `PlayerJoinedStateChange` und Abschluss der Verschiebung.

Wenn Sie die Multiplayer-Erfahrung mit diesen "Matchmade" Spieler fertig gestellt haben, können Sie den Prozess zum Verschieben in ein anderes XIM-Netzwerk mit einem anderen runde der Vermittlung wiederholen. Sehen Sie jeden Spieler, die über die vorherige verknüpft `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` Geben Sie den Vorgang eine `PlayerLeftStateChange` an, dass ihre `IXimPlayer` Objekte sind nicht mehr im gleichen Netzwerk XIM.

Wenn bei der Auswahl zum Initiieren der Vermittlung erneut mit `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, Angabe `XimPlayersToMove.BringExistingSocialPlayers`, den Player, die über soziale Netzwerke Einstiegspunkte miteinander verknüpft (`XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` oder `xXboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId`) bleibt während der neue Vermittlung ausgeführt wird. Sie können auch angeben `XimPlayersToMove.BringOnlyLocalPlayers` werden getrennt, Social verbundene remote-Playern, lassen nur die lokalen Spieler, um zu gewährleisten. In beiden Fällen wird eine andere Gruppe von fremden hinzugefügt werden, wenn der zweite Move-Vorgang abgeschlossen ist.

Sie haben auch die Möglichkeit, die nicht-Matchmade Spieler (oder nur lokal Spieler) verschieben zu einem vollständig neuen XIM Netzwerk entscheiden, ob die nächste Vermittlung Configuration/Multiplayer-Aktivität.

Im folgende Beispiel wird veranschaulicht, wie ein Gerät aufgerufen werden musste `XboxIntegratedMultiplayer.MoveToNewNetwork()` ein XIM-Netzwerk mit bis zu 8 Spieler, aber dieses Mal dauert die vorhandenen Playern Social eingebundenen auch:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringExistingSocialPlayers);
```

Ein `MoveToNetworkStartingStateChange` und `MoveToNetworkSucceededStateChange` wird für alle beteiligten Geräte bereitgestellt werden, zusammen mit einem `PlayerLeftStateChange` für die Matchmade, die zurückgelassen wurden. Auf diesen Geräten und sehen sie auf ähnliche Weise eine `PlayerLeftStateChange` für jeden Spieler, die aus dem Netzwerk verschoben werden.

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

Wenn die lokalen Benutzer haben Teilnahme an einem XIM Netzwerk häufig sie einfach wieder mit einem neuen XIM Netzwerk verschoben werden, die lokale Benutzer können Einladungen und Benutzer, um es zu verknüpfen, sodass sie die Koordination mit Freunden, um die nächste Aktivität zu finden. weiterhin können "gefolgt". Aber wenn des Benutzers ist vollständig mit Ihren Erfahrungen Multiplayer-Ihrer-app beginnen, die XIM verlassen möchten Netzwerk vollständig und in den Zustand zurück, als würde nur `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds` war aufgerufen wurde. Dies erfolgt mithilfe der `XboxIntegratedMultiplayer.LeaveNetwork()` Methode:

```cs
 XboxIntegratedMultiplayer.LeaveNetwork();
```

Diese Methode startet den Prozess des Trennens asynchron aus den anderen Teilnehmern ordnungsgemäß. Dies bewirkt, dass der remote-Geräte bereitgestellt werden eine `PlayerLeftStateChange` für jeden lokalen Spieler, die getrennt. Das lokale Gerät werden außerdem erfolgt eine `PlayerLeftStateChange` für jeden lokalen und Remotecomputern Spieler, die getrennt wurde. Wenn alle trennen Vorgänge fertig sind, eine endgültige `NetworkExitedStateChange` bereitgestellt.

Aufrufen von `XboxIntegratedMultiplayer.LeaveNetwork()` und wartet die `NetworkExitedStateChange` zum Beenden einer XIM Netzwerk ordnungsgemäß wird immer empfohlen bei einer `NetworkExitedStateChange` wurde noch nicht bereitgestellt.

## <a name="sending-and-receiving-messages"></a>Senden und Empfangen von Nachrichten

Führen die mühsame Arbeit Sichern der Kommunikationskanäle über das Internet herzustellen, sodass Sie keine Probleme mit der Netzwerkverbindung oder werden einige, aber nicht alle Spieler erreichen kümmern, XIM und zugrunde liegenden Komponenten. Wenn alle grundlegenden Peer-zu-Peer-Verbindung Probleme auftreten, ist der Wechsel zu einem Netzwerk XIM nicht erfolgreich.

Wenn ein Netzwerk XIM formatiert ist, und bestätigt mit `XimMoveToNetworkSucceededStateChange`, ist garantiert, dass alle Instanzen Ihrer app auf allen verbundenen Geräten über informiert werden alle `IXimPlayer` mit dem XIM-Netzwerk verbunden und kann Nachrichten an diese senden.

Wir veranschaulichen, wie zum Senden von Nachrichten über das Netzwerk XIM. Im folgende Beispiel wird davon ausgegangen, dass die Variable 'SendingPlayer' eine gültige lokale Player-Objekt ist. Im Beispiel wird "SendingPlayer'' s-Methode `SendDataToOtherPlayers()` eine Nachrichtenstruktur zu senden, die in der 'MsgBuffer" für alle Spieler (lokal oder remote) im Netzwerk XIM kopiert wurde. Beachten Sie, dass es für alle Spieler wechselt, da eine Liste der spezifischen Spieler nicht an die Methode übergeben wurde:

```cs
SendingPlayer.SendDataToOtherPlayers(msgBuffer, null, XimSendType.GuaranteedAndSequential);
```

Alle Empfänger der Nachricht bereitgestellt werden eine `XimPlayertoPlayerDataReceivedStateChange` , die eine Kopie der Daten enthält, sowie die `IXimPlayer` -Objekt, das es und eine Liste von gesendet der `IXimPlayer` Objekte, die lokal es empfangen.

Natürlich garantiert, geordnete Übermittlung ist praktisch, aber einen Typ ineffizient senden, kann auch sein, da XIM muss erneut, oder Nachrichten verzögern, wenn Pakete gelöscht oder durch das Internet ungeordnete. Achten Sie darauf, dass Sie für den Einsatz von den anderen Arten von Sendeports für Nachrichten, die Ihre app tolerieren kann, verlieren oder having außerhalb der Reihenfolge eintreffen. Da die Nachrichtendaten von einem Remotecomputer aus stammen, werden die bewährte Methode eindeutig Datenformaten, wie etwa das Packen von Multi-Byte-Werte in einer bestimmten Bytereihenfolge ("endiancharakteristik") definiert haben. Die app sollte auch die Daten, bevor auf ihn überprüfen.

XIM bietet Sicherheit auf Zeilenebene Netzwerk besteht keine Notwendigkeit für zusätzliche Verschlüsselung oder Signatur-Schemas. Es wird jedoch empfohlen immer "Defense-in-Depth" einsetzen, um Schutz vor versehentlichen Anwendungsfehler und verschiedene Versionen Ihrer Anwendung Protokoll Koexistenz ordnungsgemäß (während der Entwicklung, Inhaltsupdates usw.) behandeln können. Internetverbindungen von Benutzern können eingeschränkt und sich ständig ändernden Ressourcen sein. Wir empfehlen dringend die Verwendung von effizienten meldungsdatenformate und vermeiden Entwürfe, die alle UI-Frames zu senden.

## <a name="assessing-data-pathway-quality"></a>Bewerten der Datenqualität-Pfad

Erfahren Sie die aktuellen Qualität der datenüberwachungspfad zwischen zwei Spielern auszutauschen, indem Sie untersuchen die `XimPlayer.NetworkPathInformation` Eigenschaft. Erfahren Sie mehr über die aktuellen Qualität des Pfads zwischen zwei Spielern durch Überprüfen der `XimPlayer.NetworkPathInformation` Eigenschaft. Die Eigenschaft enthält die geschätzte Roundtrip-Latenz und wie viele Nachrichten weiterhin lokal für die Übertragung in die Groß-/Kleinschreibung in der Warteschlange befinden, dass die Unterstützung der Verbindung kann nicht mehr Daten übertragen werden für einen Moment.

Wenn Sie sehen, dass die Warteschlangen für einen bestimmten IXimPlayer sichern, sollten Sie die Rate reduzieren, an der Sie Daten senden.

Die `NetworkPathInformation.RoundTripLatency` Feld repräsentiert die Latenz der zugrunde liegenden Netzwerk und XIMs Geschätzte Wartezeit ohne queuing. Effektive Latenz nimmt zu, als `XimNetworkPathInformation.SendQueueSizeInMessages` wächst und XIM funktioniert durch die Warteschlange.

Wählen Sie einen angemessenen zeigen Sie auf starten, Aufrufe von Drosselung `SendDataToOtherPlayers` basierend auf Nutzung und die Anforderungen des Spiels. Jede Nachricht in der Sendewarteschlange stellt einen Anstieg bei der effektiven Netzwerkwartezeit dar.

Ein Wert in der Nähe XIMs maximal (derzeit 3500 Nachrichten) viel zu hohen bei den meisten Spielen und stellt wahrscheinlich einige Sekunden von Daten, die darauf warten, abhängig von der Rate der Aufruf gesendet werden `SendDataToOtherPlayers` und wie groß jede Datennutzlast ist. Wählen Sie stattdessen eine Zahl, die das Spiel latenzanforderungen sowie wie ganz nervös des Spiels berücksichtigt `SendDataToOtherPlayers` Aufrufmuster ist.

## <a name="sharing-data-using-player-custom-properties"></a>Freigeben von Daten mithilfe von benutzerdefinierten Eigenschaften der player

Die meisten app-Daten-Austausch auftreten, mit der `XimLocalPlayer.SendDataToOtherPlayers()` Methode beeinträchtigt, da die umfassendsten kontrollmöglichkeiten über, die er empfängt, und wenn, wie Paketverlust behandeln sollte, und so weiter. Es gibt jedoch noch Mal, wenn es nicht schön, auf dem Spieler basic "," nur selten ändernden Status über sich selbst mit minimalen anfällt für andere Benutzer freigeben, würde. Jeder Spieler möglicherweise z. B. eine feste Zeichenfolge, die ausgewählt werden, bevor Sie eingeben, dass alle Spieler verwenden, um deren Darstellung im Spiel Rendern multiplayer Zeichenmodell darstellt.

Für Daten, die nicht über einen Player sehr häufig ändern, bietet XIM Player über benutzerdefinierte Eigenschaften. Diese Eigenschaften bestehen aus einer app definierter Name und der Wert sind Null-terminierte Zeichenfolge-Paare, die angewendet werden kann, für den lokalen Player und, erhalten automatisch an alle Geräte weitergegeben, wenn sie geändert werden.

Eigenschaften von benutzerdefinierten Player haben weitere interne Synchronisierung Laufzeitaufwand als die `XimLocalPlayer.SendDataToOtherPlayers()` -Methode, sodass rasch ändernden Daten (d. h. spielerposition), weiterhin direkte sendet verwenden sollten.

Player, benutzerdefinierte Eigenschaften der Schlüssel-Wert-Paare verfügen, deren aktuelle Werte, die automatisch auf neue beteiligten Geräte bereitgestellt werden, wenn diese Geräte XIM Netzwerk verbinden, und finden Sie unter der Spieler hinzugefügt. Werte können festgelegt werden, durch den Aufruf `XimLocalPlayer.SetPlayerCustomProperty()` mit Namen und Wert der Zeichenfolgen, wie im folgenden Beispiel, eine Eigenschaft festlegt mit dem Namen "model", um den Wert "Brute" auf einem lokalen `IXIMPlayer` Objekt verweist die Variable "LocalPlayer":

```cs
localPlayer.SetPlayerCustomProperty("model","brute");
```

Bewirkt, dass Änderungen an den Eigenschaften der Spieler eine `XimPlayerCustomPropertiesChangedStateChange` bereitgestellt werden, um alle Geräte, die aufmerksam machen, die den Namen der Eigenschaften, die geändert wurden. Der Wert für einen bestimmten Namen auf Player, lokal oder Remote, abgerufen werden kann, mit `IXIMPlayer.GetPlayerCustomProperty()`. Im folgende Beispiel ruft den Wert für eine Eigenschaft mit dem Namen "model" aus einer `IXIMPlayer` verweist die Variable "XimPlayer":

```cs
string propertyValue = localPlayer.GetPlayerCustomProperty("model");
```

Festlegen eines neuen Werts für einen angegebenen Eigenschaftsnamen ersetzt alle vorhandenen Werte. Ein Eigenschaftenname festlegen, auf den Wert ein null-Wert-Zeichenfolgenzeiger behandelt identisch mit einem leeren Wert-Zeichenfolge, die die gleiche Weise wie die Eigenschaft an, dass noch nicht angegeben ist. Namen und Werte werden nicht von XIM interpretiert, daher verbleibt in der app, um den Inhalt der Zeichenfolge nach Bedarf zu überprüfen.

Eigenschaften von benutzerdefinierten Player sind immer zurückgesetzt werden, wenn von einem XIM-Netzwerk zu einer anderen wechseln. Jedoch erhalten neue Player, die das XIM Netzwerk einzudringen Eigenschaften der aktuellen benutzerdefinierten Player für alle Spieler, die über einige verfügen.

## <a name="sharing-data-using-network-custom-properties"></a>Freigeben von Daten mithilfe von benutzerdefinierten Eigenschaften des Netzwerks

Benutzerdefinierte Eigenschaften des Netzwerks dienen zur Vereinfachung Synchronisieren von Daten, die spezifisch für das XIM-Netzwerk, die nicht häufig ändern. Benutzerdefinierte Eigenschaften arbeiten genauso wie Network [benutzerdefinierte Eigenschaften der Player](#sharing-data-using-player-custom-properties), außer dass sie für das XIM Singleton-Objekt mit festgelegt sind `XboxIntegratedMultiplayer.SetNetworkCustomProperty()`. Im folgenden Beispiel wird eine "Zuordnung"-Eigenschaft den Wert "kundenbotschaften":

```cs
XboxIntegratedMultiplayer.SetNetworkCustomProperty("map", "stronghold");
```

Bewirkt, dass Änderungen an Eigenschaften des Netzwerks ein `NetworkCustomPropertiesChanged` bereitgestellt werden, um alle Geräte, die aufmerksam machen, die den Namen der Eigenschaften, die geändert wurden. Der Wert für einen bestimmten Namen abgerufen werden kann, mit `XboxIntegratedMultiplayer.GetNetworkCustomProperty()`, wie im folgenden Beispiel, das den Wert für eine benannte Eigenschaft "Zuordnung" abruft:

```cs
string property = XboxIntegratedMultiplayer.GetNetworkCustomProperty("map")
```

Genau wie [benutzerdefinierte Eigenschaften der Player](#sharing-data-using-player-custom-properties), eine neue Einstellung für ein angegebenen Eigenschaftsnamen, alle vorhandenen Werte ersetzt wird. Ein Eigenschaftenname festlegen, auf den Wert ein null-Wert-Zeichenfolgenzeiger behandelt identisch mit einem leeren Wert-Zeichenfolge, die die gleiche Weise wie die Eigenschaft an, dass noch nicht angegeben ist. Namen und Werte werden nicht von XIM interpretiert, daher verbleibt in der app, um den Inhalt der Zeichenfolge nach Bedarf zu überprüfen.

Neu erstellte XIM Netzwerke beginnen immer mit kein Netzwerk über benutzerdefinierte Eigenschaften festgelegt. Die aktuellen Werte der Network benutzerdefinierter Eigenschaften für dieses Netzwerk XIM erhalten jedoch neue Player, die ein vorhandenes XIM Netzwerk beitreten.

## <a name="matchmaking-using-per-player-skill"></a>Mithilfe der Fähigkeiten von pro-Player Vermittlung

Spieler ist vom gemeinsamen Interesse an einem bestimmten Spiel angegebene app-Modus eine gute Basis Strategie. Wenn der Pool der verfügbaren Spieler zunimmt, sollten Sie erwägen, auch übereinstimmende Spieler basierend auf ihren persönlichen Qualifikation oder Erfahrung mit Ihr Spiel, sodass erfahrene Spieler die Herausforderung, fehlerfreien Wettbewerb mit anderen clusterexperten, profitieren können, während neuere Spieler von vergrößert werden kann ein Wettbewerb mit anderen ähnlichen Funktionen.

Zu diesem Zweck zu starten, durch die Bereitstellung der Qualifikation für alle lokalen Spieler in den pro-Player-Rollen und Fähigkeiten Konfigurationsstruktur, die in Aufrufen von angegeben `XimLocalPlayer.SetRolesAndSkillConfiguration()` vor dem Start in einem XIM verschieben Netzwerk mithilfe der Vermittlung. Kenntnisstand ist ein app-spezifisches Konzept und die Anzahl ist nicht vom XIM, interpretiert, mit dem Unterschied, dass Vermittlung wird zunächst versuchen, die Spieler mit den gleichen Wert für die Fähigkeiten finden, und erweitern Sie dann in regelmäßigen Abständen die Suche in Schritten von +/-10, um zu versuchen, andere Spieler deklarieren Fähigkeiten finden die Werte innerhalb eines Bereichs auf dieser Fähigkeit. Im folgende Beispiel wird vorausgesetzt, dass die lokale `XimLocalPlayer` Objekt, dessen Variablen handelt es sich um "LocalPlayer", hat einen zugeordneten app-spezifische Fähigkeiten Ganzzahlwert einer lokalen oder Xbox Live-Speicher in eine Variable namens "PlayerSkillValue" abgerufen:

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Skill = playerSkillValue;
localPlayer.SetRolesAndSkillConfiguration(config);
```

Wenn dies abgeschlossen ist, werden alle Teilnehmer bereitgestellt werden eine `XimPlayerRolesAndSkillConfigurationChangedStateChange` dies `IXIMPlayer` pro-Player-Rollen und Fähigkeiten-Konfiguration geändert hat. Der neue Wert abgerufen werden kann, durch den Aufruf `IXIMPlayer.RolesAndSkillConfiguration`. Wenn alle Beteiligten Vermittlung von nicht-Null-Konfiguration angewendet haben, können Sie mit einem Wert "true", für die Vermittlung mit XIM-Netzwerk Verschieben der `RequirePlayerRolesAndSkillConfiguration` Feld der `XimMatchmakingConfiguration` Struktur angegeben, um `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`.

Das folgende Beispiel füllt eine Vermittlung-Konfiguration, um insgesamt 2 bis 8 Playern für eine ohne-Teams jeder gegen jeden gefunden. Dieses Beispiel verwendet außerdem eine app definierte-Konstante, die vom Typ ' Uint64 und mit der Bezeichnung MYGAMEMODE_DEATHMATCH, die die Spiel-Modus zum Filtern der darstellt. Dadurch wird die Vermittlung des Netzwerks XIM Spieler mit anderen Spielern, die die gleichen Werte angeben, sowie dass pro-Player Vermittlung Konfiguration entsprechend konfiguriert.

```cs
XimMatchmakingConfiguration matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.RequirePlayerRolesAndSkillConfiguration = true;
```

Wenn diese Struktur dient der `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, der Verschiebevorgang beginnt normalerweise so lange, wie Spieler verschieben aufgerufen haben `XimLocalPlayer.SetRolesAndSkillConfiguration()` mit einer nicht-Null `XimPlayerRolesAndSkillConfiguration` Objekt. Wenn ein Spieler nicht der Fall, dann der Prozess für die Vermittlung angehalten werden wird und alle Teilnehmer bereitgestellt werden eine `XimMatchmakingProgressUpdatedStateChange` mit einem `WaitingForPlayerRolesAndSkillConfiguration` Wert. Dazu Player, die anschließend am XIM Netzwerk über eine zuvor gesendete Einladung oder anderweitig social anzumelden (z. B. einen Aufruf von `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) vor dem Abschluss der Vermittlung. Wenn alle Beteiligten bereitgestellt haben ihre `XimPlayerRolesAndSkillConfiguration` Objekten, die Vermittlung wird fortgesetzt.

Vermittlung, die mithilfe von pro-Player-Fähigkeiten kann auch mit pro-Player Benutzerrolle Vermittlung, kombiniert werden, wie im nächsten Abschnitt erläutert. Wenn nur eine gewünscht ist, können Sie den Wert 0 für die anderen angeben. Dies ist, da alle Spieler, deklarieren sie haben eine `XimPlayerRolesAndSkillConfiguration` Fähigkeiten der Wert 0 werden immer übereinstimmen.

Nach der `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` oder einem beliebigen anderen XIM Netzwerk Verschieben der Vorgang abgeschlossen wurde, werden alle Spieler `XimPlayerRolesAndSkillConfiguration` Strukturen werden automatisch gelöscht werden, um einen null-Zeiger (mit einem zugehörigen `XimPlayerRolesAndSkillConfigurationChangedStateChange` Benachrichtigung). Wenn Sie mit einem anderen XIM-Netzwerk mit Vermittlung, die pro-Player-Konfiguration erfordert verschieben möchten, müssen Sie zum Aufrufen `XimLocalPlayer.SetRolesAndSkillConfiguration()` erneut mit der ein Objekt, das die aktuellste Informationen enthält.

## <a name="matchmaking-using-per-player-role"></a>Vermittlung mit pro-Player-Rolle

Eine andere Methode der Verwendung von pro-Player-Rollen und Konfiguration von Fähigkeiten zur Verbesserung der benutzererfahrung für die Vermittlung ist die Verwendung erforderlichen Player-Rollen. Dies ist am besten für Spiele, die angeben, dass auswählbare Zeichentypen, die verschiedene kooperativen empfehlen Stile wiedergeben. Diese Zeichentypen werden der nicht ändern Sie einfach die grafische Darstellung der in-Game- und, stattdessen die Gaming-Stil für den Spieler ändern. Benutzer bevorzugen möglicherweise als eine bestimmte Spezialisierung wiedergegeben. Jedoch, wenn Ihr Spiel ausgelegt ist, so, dass es funktionell nicht möglich, die Ziele, ohne mindestens einer Person, die jede Rolle erfüllen kann, ist manchmal es besser, solche Player zusammen zuerst als entsprechen alle Spieler zusammen dann müssen übereinstimmen handeln Sie Play Stile untereinander einmal gesammelt aus. Hierzu können Sie definieren von ein eindeutiger Bitflag jede Rolle in eines bestimmten Players angegeben werden `XimPlayerRolesAndSkillConfiguration` Struktur.

Im folgenden Beispiel wird einen app-spezifische Rolle-Wert, der vom Typ Byte und mit dem Namen MYROLEBITFLAG_HEALER, für die lokale `XimLocalPlayer` -Objekt, dessen Zeiger ist "LocalPlayer":

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Roles = MYROLEBITFLAG_HEALER;
localPlayer.SetRolesAndSkillConfiguration(config);
```

Wenn dies abgeschlossen ist, werden alle Teilnehmer bereitgestellt werden eine `XimPlayerRolesAndSkillConfigurationChangedStateChange` dies `IXimPlayer` pro-Player-Rollen und Fähigkeiten-Konfiguration geändert hat. Der neue Wert abgerufen werden kann, durch den Aufruf `IXimPlayer.RolesAndSkillConfiguration`.

Die globale `XimMatchmakingConfiguration` Struktur angegeben, um `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` sollte dann wurden alle erforderlichen Rollen Flags kombiniert mithilfe des bitweisen OR- und einem Wert "true", für die `RequirePlayerRolesAndSkillConfiguration` Feld.

Vermittlung mit pro-Player-Rolle kann auch mit Vermittlung Benutzer pro-Player können kombiniert werden. Wenn nur eine gewünscht ist, geben Sie den Wert 0 für die anderen. Dies ist, da alle Spieler, deklarieren sie haben eine `XimPlayerRolesAndSkillConfiguration` Fähigkeiten der Wert 0 wird immer übereinstimmen; und, wenn alle Bits auf 0 (null) in sind die `XimMatchmakingConfiguration.RequiredRoles` Feld, und klicken Sie dann keine Bits für die Rolle erforderlich sind, um eine Übereinstimmung herzustellen.

Nach der `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` oder einem beliebigen anderen XIM Netzwerk Verschieben der Vorgang abgeschlossen wurde, werden alle Spieler `XimPlayerRolesAndSkillConfiguration` Objekte werden automatisch auf null zurückgesetzt werden (mit einem zugehörigen `XimPlayerRolesAndSkillConfigurationChangedStateChange` Benachrichtigung). Wenn Sie mit einem anderen XIM-Netzwerk mit Vermittlung, die pro-Player-Konfiguration erfordert verschieben möchten, müssen Sie zum Aufrufen `XimLocalPlayer.SetRolesAndSkillConfiguration()` erneut mit der ein Objekt, das die aktuellste Informationen enthält.

## <a name="how-xim-works-with-player-teams"></a>Funktionsweise von XIM mit Player-teams

Multiplayer-Spiele spielen häufig umfasst die Spieler auf die gegensätzlichen Teams organisiert. XIM macht es einfach, Zuweisen von teams, die bei der Vermittlung durch Festlegen von `XimMatchmakingConfiguration.TeamConfiguration`. Im folgende Beispiel wird eine Verschiebung mit, dass die Vermittlung, die so konfiguriert, dass insgesamt 8-Player finden um auf beiden Teams von 4 zu platzieren, (obwohl es sich um eine Falls 4 nicht gefunden werden, 1 bis 3 auch akzeptabel sind in der Regel) initiiert. Dieses Beispiel verwendet außerdem eine app definierte-Konstante, die vom Typ "uint", und mit dem Namen MYGAMEMODE_CAPTURETHEFLAG, die die Spiel-Modus zum Filtern der darstellt.  Darüber hinaus wird die Konfiguration eingerichtet, entlang der Spieler alle Social eingebunden, aus dem aktuellen XIM-Netzwerk zu öffnen:

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 2;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 4;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

Wenn solche ein XIM verschieben Netzwerkvorgang abgeschlossen ist, werden der Spieler einen Team Indexwert 1 durch {n} für die angeforderte {n}-Teams zugewiesen werden. Team-Indexwert des Spielers abgerufen wird, über `IXIMPlayer.TeamIndex`. Bei Verwendung einer `XimMatchmakingConfiguration.TeamConfiguration` mit zwei oder mehr Teams Player werden nie ein Wert zugewiesen werden Team Index 0 (null), durch den Aufruf von `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`. Dies ist im Gegensatz dazu auf Player, die mit dem Netzwerk XIM mit alle anderen Konfigurationen oder Typ des Verschiebevorgangs hinzugefügt werden (z. B. über eine protokollaktivierung, die durch eine Einladung akzeptiert), besitzen, die immer einen Index mit der Team 0 (null). Es kann hilfreich sein, Team Index 0 als spezielle "nicht zugeordnete" Team behandeln sein.

Das folgende Beispiel ruft den Team-Index für eine XIM Player-Objekt in der Variablen "XimPlayer" gespeichert:

```cs
byte playerIndex = ximPlayer.TeamIndex;
```

Für die bevorzugte Benutzer (nicht zu erwähnen reduziert Verkaufschance für negative Player-Verhalten) die Xbox Live-Vermittlungsdienst wird nie Split-Playern, die mit einem Netzwerk XIM auf verschiedenen Teams zusammen verschoben werden.

Der Team-Indexwert, der ursprünglich vom Vermittlung zugewiesen ist, nur eine Empfehlung aus, und die app können Sie ändern, für lokale Spieler jederzeit `XimLocalPlayer.SetTeamIndex()`. Dies kann auch in XIM Netzwerken aufgerufen werden, die Vermittlung nicht zu verwenden. Im folgenden Beispiel wird ein XIM lokalen Player-Objekt in der Variablen "XimLocalPlayer" haben Sie einen neuen Team Indexwert eines gespeichert:

```cs
ximLocalPlayer.SetTeamIndex(1);
```

Alle Geräte werden darüber informiert, dass der Spieler einen neuen Team Indexwert aktiviert hat, wenn sie bereitgestellt haben eine `XimPlayerTeamIndexChangedStateChange` für die betreffende Spieler. Rufen Sie `XimLocalPlayer.SetTeamIndex()` zu einem beliebigen Zeitpunkt.

Vermittlung wertet pro Player erforderlichen Rollen unabhängig von den Teams. Aus diesem Grund ist es nicht empfohlen, um Teams und erforderlichen Rollen als Kriterium für gleichzeitige Vermittlung-Konfiguration zu verwenden, da die Teams nach Anzahl der Spieler, nicht von erfüllte Player Rollen ausgewogen ist.

## <a name="working-with-chat"></a>Arbeiten mit chat

Sprache und Text-Chat-Kommunikation werden automatisch für Spieler in einem Netzwerk XIM aktiviert. XIM verarbeitet die Interaktion mit gesamter Hardware für die Sprach-Kopfhörer und Mikrofon für Sie. Ihre app muss nicht viele Funktionen für Chat, jedoch verfügt es über die eine Anforderung in Bezug auf den Text-Chat: Unterstützung für Eingabe und Anzeige. Texteingabe ist erforderlich, da, sogar auf Plattformen oder Spiele Genres, die weit verbreitete physische Tastatur verwenden, in der Vergangenheit aufwiesen, Spieler das System, um die Sprachwiedergabe von Text hilfstechnologien verwenden konfigurieren. Auf ähnliche Weise ist Anzeigen von Text erforderlich, weil Spieler konfigurieren, dass das System, um die Sprache-zu-Text verwenden können.

Wenn Sie Text Mechanismen bedingt aktivieren möchten, können diese Einstellungen auf lokalen Playern erkannt werden, durch den Zugriff auf die `XimLocalPlayer.ChatTextToSpeechConversionPreferenceEnabled` und `XimLocalPlayer.ChatSpeechToTextConversionPreferenceEnabled` Feldern, und Sie möchten die Text-Mechanismen bedingt zu aktivieren. Allerdings empfiehlt es sich, dass Sie erwägen, Text eingeben und Anzeigen von Optionen, die immer verfügbar sind.

`Windows::Xbox::UI::Accessability` eine Xbox One Klasse wurde speziell für ein einfaches Rendering von Text mit in-Game-Chat mit dem Schwerpunkt auf hilfstechnologien Sprache-zu-Text bereitstellen.

Nachdem Sie die Texteingabe, die von einem physischen oder virtuellen Tastatur bereitgestellt haben, übergeben Sie die Zeichenfolge, die die `XimLocalPlayer.SendChatText()` Methode. Der folgende code zeigt, senden eine Beispiel hartcodierte Zeichenfolge aus einer lokalen `IXIMPlayer` Objekt verweist die Variable "LocalPlayer":

```cs
localPlayer.SendChatText(text);
```

Dieser Chat-Text wird an alle Beteiligten in das XIM-Netzwerk, das aus dem ursprünglichen lokalen Player als Chat Kommunikation empfangen kann übermittelt eine `XimChatTextReceivedStateChange`. Es kann auf Audiodaten von Spracherkennung umgewandelt werden soll, wenn TTS aktiviert ist.

Ihre app sollte eine Kopie jeder Textzeichenfolge, die empfangen und anzeigen sowie einige Identifizierung von der ursprünglichen Player für eine angemessene Zeit (oder in einem bildlauffähigen Fenster).

Es gibt auch einige bewährten Methoden bezüglich der Chat. Es wird empfohlen, dass an einer beliebigen Stelle Player, besonders in einer Liste von Gamertags wie z. B. eine spielstandsanzeige, angezeigt werden, dass auch stumm geschaltet/Vorträge Symbole als Feedback für den Benutzer angezeigt werden. Dies erfolgt durch den Zugriff auf `IXimPlayer.ChatIndicator` zum Abrufen einer `XboxIntegratedMultiplayer.XimPlayerChatIndicator` , die den aktuellen, sofortigen Status Chat für die betreffende Spieler darstellt.

Der gemeldete Wert ist vom `IXimPlayer.ChatIndicator` wird erwartet, dass Sie häufig als Player starten und beenden zu sprechen, z. B. ändern. Es soll Abrufen jedes UI-Frames daher-apps zu unterstützen.

> [!NOTE]
> Wenn ein lokaler Benutzer aufgrund der Einstellungen für ihre Geräte von Systemdiensten Kommunikation keine `XimLocalPlayer.ChatIndicator` gibt eine `XboxIntegratedMultiplayer.XimPlayerChatIndicator` mit dem Wert `XIM_PLAYER_CHAT_INDICATOR_PLATFORM_RESTRICTED`. Der Annahme aus, um die Anforderungen für die Plattform ist, dass die app, der angibt, einer plattformeinschränkung für Voice Chat oder messaging und eine Meldung für Benutzer, der angibt, des Problems ikonographie anzeigen. Eine Beispielnachricht, die empfohlen wird: "Leider sind Sie nicht zulässig, jetzt chatten."

## <a name="muting-players"></a>Stummschaltung des players

Ein anderes bewährtes Verfahren ist zur Unterstützung der stummschaltung Spieler. XIM übernimmt automatisch das System stummschaltung von Benutzern durch Player Karten initiiert, aber apps sollten unterstützen spezifische vorübergehende stummschaltung, die auf der game-Benutzeroberfläche über ausgeführt werden können die `IXimPlayer.ChatMuted` Eigenschaft. Im folgenden Beispiel zunächst eine stummschaltung `IXIMPlayer` Objekt verweist die Variable "RemotePlayer", damit keine Voice Chat hören ist und kein Textchat von ihm empfangen wird:

```cs
remotePlayer.ChatMuted = true;
```

Die muting wirksam wird ist sofort, und es keine Änderung XIM zugeordnet. Diese Änderung kann des Werts des der `IXimPlayer.ChatMuted` Eigenschaft auf "false". Im folgende Beispiel unmutes eine `IXIMPlayer` Objekt verweist die Variable "RemotePlayer":

```cs
remotePlayer.ChatMuted = false;
```

Deaktiviert bleiben wirksam, solange die `IXIMPlayer` vorhanden ist, einschließlich beim Wechsel zu einem neuen XIM-Netzwerk mit den Player. Es wird nicht beibehalten, wenn der Spieler verlässt und demselben Benutzer wieder vereint (als neues `IXIMPlayer` Instanz).

Spieler starten in der Regel im Zustand "unmuted". Wenn möchte, dass Ihre app ein Spieler in der stumm geschaltet sind Zustand Gründen Spiel zu starten, können sie festlegen `IXIMPlayer.ChatMuted` auf die `IXIMPlayer` Objekt vor dem Abschluss der Verarbeitung der zugeordneten `PlayerJoinedStateChange`, und XIM wird sichergestellt, es werden keine Zeitraum, in denen voicefonts audioeingaben aus der Player kann beteiligen.

Eine automatische stumm schalten Überprüfung basierend auf Player Reputation tritt auf, wenn es sich bei ein remote-Player im Netzwerk XIM verknüpft. Wenn der Spieler ein Flag in dem Ruf hat, ist der Spieler automatisch stumm geschaltet. Stummschaltung wirkt sich nur auf lokalen Status und daher weiterhin besteht, wenn ein Player über Netzwerke hinweg verschoben wird. Die automatische Bewertung zum Stummschalten-Prüfung wird einmal durchgeführt, und nicht neu ausgewertet werden erneut für solange die `IXIMPlayer` gültig bleibt.

## <a name="configuring-chat-targets-using-player-teams"></a>Konfigurieren die Spieler Teams über Chat-Ziele

Gemäß der [wie XIM funktioniert mit Player-Teams](#how-xim-works-with-player-teams) Abschnitt dieses Dokuments, die wahre Bedeutung von bestimmten Team Indexwert ist Aufgabe der app. XIM Interpretation nicht mit Ausnahme der Durchführung von Gleichheitsvergleichen in Bezug auf die Chat-Zielkonfiguration. Wenn die Chat-Zielkonfiguration gemeldet `XboxIntegratedMultiplayer.ChatTargets` befindet sich derzeit `XimChatTargets.SameTeamIndexOnly`, und klicken Sie dann alle angegebenen Player nur Chat-Kommunikation mit einem anderen ausgetauscht werden sollen, wenn die beiden den gleichen Wert, der von gemeldet haben `IXIMPlayer.TeamIndex` (und Datenschutz / Richtlinie ermöglichen es).

Konservative und Unterstützung, die Wettbewerber-Szenarios, neu erstellte XIM Netzwerke automatisch werden konfiguriert. Standardmäßig ist `XimChatTargets.SameTeamIndexOnly`. Chatten mit vanquished Gegner auf dem anderen Team kann jedoch wünschenswert, z. B. in einem nach der Spiel "Lobby" sein. Können Sie anweisen, XIM, wenden Sie sich an alle anderen Benutzer (wo Datenschutz und Richtlinie zulassen), durch den Aufruf erlauben% `XboxIntegratedMultiplayer.SetChatTargets()`. Im folgende Beispiel beginnt mit der Konfiguration alle Teilnehmer im Netzwerk XIM verwenden eine `XimChatTargets.AllPlayers` Wert:

```cs
XboxIntegratedMultiplayer.SetChatTargets(XimChatTargets.AllPlayers)
```

Alle Teilnehmer werden informiert, dass eine neue zieleinstellung für das gültig ist, wenn sie bereitgestellt sind eine `XimChatTargetsChangedStateChange`.

Wie bereits erwähnt, werden die meisten XIM Netzwerk verschieben anfänglich alle Spieler den Team-Indexwert von 0 (null) zuweisen. Dies bedeutet, dass eine Konfiguration des `XimChatTargets.SameTeamIndexOnly` ist wahrscheinlich nicht von Unterschieden `XimChatTargets.AllPlayers` standardmäßig. Player, die mit einem XIM-Netzwerk, die mithilfe der Vermittlung verschieben müssen jedoch unterschiedliche Indexwerte team, wenn die Vermittlung der Konfiguration `TeamMatchmakingMode` Wert deklariert zwei oder mehr Teams. Sie können auch aufrufen `XimLocalPlayer.SetTeamIndex()` zu einem beliebigen Zeitpunkt wie oben gezeigt. Wenn Ihre app nicht-NULL-Team Indexwerte über beide Methoden verwendet wird, vergessen Sie nicht zum Verwalten von der aktuellen Chat-Ziele, die entsprechend festlegen.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>Automatische Ausfüllen von Player-Slots ("Abgleich" Vermittlung)

Trennung von Gruppen von Spielern Aufrufen `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` gleichzeitig bietet der Xbox Live-Vermittlungsdienst die größte Flexibilität, um sie schnell in neuen, optimalen XIM Netzwerke zu organisieren. Allerdings möchten einige Gaming-Szenarien zu einem bestimmten XIM-Netzwerk intakt, und nur Zusammenarbeit zusätzliche Spieler, nur um freien Player Slots zu füllen. XIM unterstützt das Konfigurieren der Vermittlung, für die Ausführung im Modus oder "generellen", durch den Aufruf ausfüllen automatisch im Hintergrund `XboxIntegratedMultiplayer.SetNetworkConfiguration()` mit einem `XimNetworkConfiguration` , bei dem `XimAllowedPlayerJoins.Matchmade` flagssatz auf seine `XimNetworkConfiguration.AllowedPlayerJoins` Eigenschaft.

Im folgenden Beispiel wird die Vermittlung Abgleich, um zu versuchen, insgesamt 8 Playern für eine ohne-Teams jeder gegen jeden zu finden, (obwohl es sich um eine Falls 8 nicht gefunden werden, 2 bis 7 auch akzeptabel sind in der Regel), durch den Wert MYGAMEMODE_ definiert ein Spielmodus app-spezifischen Konstanten uint64_t verwenden DEATHMATCH, die nur mit anderen Spielern angeben dieser Wert entspricht:

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Matchmade;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Dadurch werden die vorhandenen XIM Netzwerk verfügbar, auf Geräten Aufrufen `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` auf normale Weise. Diese Geräte finden Sie unter kein Verhalten zu ändern. Die Teilnehmern im Netzwerk XIM generellen nicht verschoben, aber es erfolgt eine `XimNetworkConfigurationChangedStateChange` überprüft, die auf einschalten Abgleich als auch mehrere `XimMatchmakingProgressUpdatedStateChange` Benachrichtigungen, falls zutreffend. Alle Matchmade Player wird hinzugefügt werden, mit dem XIM-Netzwerk mit dem normalen `XimPlayerJoinedStateChange`.

In der Standardeinstellung Abgleich Vermittlung bleibt aktiviert auf unbestimmte Zeit, auch wenn es nicht mehr Spieler hinzufügen, verfügt das XIM Netzwerk bereits die maximale Anzahl der Spieler gemäß versucht die `XimNetworkConfiguration.TeamConfiguration` festlegen. Organisationsidentität kann deaktiviert werden, indem `XimNetworkConfiguration.AllowedPlayerJoins` nicht einschließlich `XimAllowedPlayerJoins.Matchmade`:

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins &= ~XimAllowedPlayerJoins.Matchmade;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Eine entsprechende `XimNetworkConfigurationChangedStateChange` erfolgt für alle Geräte, und nachdem dieser asynchrone Vorgang abgeschlossen ist, eine endgültige `XimMatchmakingProgressUpdatedStateChange` erfolgt mit `MatchmakingStatus.None` anzeigen, dass keine weiteren Matchmade-Player im Netzwerk XIM hinzugefügt werden.

Beim Aktivieren der Vermittlung der Abgleich mit einem `XimNetworkConfiguration.TeamConfiguration` festlegen, die zwei oder mehr Teams deklariert ist, alle vorhandene Playern müssen einen gültigen Teamprojektnamen-Index, der zwischen 1 und die Anzahl der Teams ist. Dies schließt die Spieler mit aufgerufen haben `XimLocalPlayer.SetTeamIndex()` , geben Sie einen benutzerdefinierten Wert oder die, die mittels einer Einladung wurde verknüpft werden oder über andere Social hat (z. B. einen Aufruf von `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) und mit einem Standardwert für die Index 0 hinzugefügt wurden. Wenn ein Spieler verfügt nicht über einen gültigen Teamprojektnamen-Index, und klicken Sie dann der Vermittlung Prozess angehalten und alle Teilnehmer bereitgestellt werden eine `XimMatchmakingProgressUpdatedStateChange` mit einem `MatchmakingStatus.WaitingForPlayerTeamIndex` Wert. Sobald alle Spieler bereitgestellt oder ihre Team-Indexwerte mit korrigiert haben `XimLocalPlayer.SetTeamIndex()`, Abgleich Vermittlung wird fortgesetzt. Weitere Informationen finden Sie der [wie XIM funktioniert mit Player-Teams](#how-xim-works-with-player-teams) Abschnitt dieses Dokuments.

Auf ähnliche Weise beim Aktivieren der Vermittlung der Abgleich mit einer `XimNetworkConfiguration` Struktur mit der `RequirePlayerRolesAndSkillConfiguration` Feld auf "true" festgelegt ist, und klicken Sie dann alle Spieler eine nicht-Null-pro-Player Vermittlung-Konfiguration angegeben ist müssen. Wenn ein Spieler nicht der Fall, dann der Prozess für die Vermittlung angehalten werden wird und alle Teilnehmer bereitgestellt werden eine `XimMatchmakingProgressUpdatedStateChange` mit einem `XimMatchMakingStatus.WaitingForPlayerRolesAndSkillConfiguration` Wert. Wenn alle Beteiligten bereitgestellt haben ihre `XimPlayerRolesAndSkillConfiguration` Objekten, die Vermittlung der Abgleich wird fortgesetzt. Weitere Informationen finden Sie der [Vermittlung, die mithilfe der Fähigkeiten von pro-Player](#matchmaking-using-per-player-skill) und [Vermittlung, die pro-Player-Rolle mit](#matchmaking-using-per-player-role) Abschnitten dieses Dokuments.

## <a name="querying-joinable-networks"></a>Abfragen von beigetreten Netzwerke

Zwar Vermittlung eine hervorragende Möglichkeit, Spieler schnell miteinander verbinden, ist manchmal es am besten, Spieler beigetreten Netzwerke, die mithilfe von benutzerdefinierten Suchkriterien ermitteln können, und wählen Sie das Netzwerk, die, das Sie verknüpfen möchten. Dies kann besonders vorteilhaft sein, wenn eine Spiele-Sitzung ein breites Spektrum an konfigurierbaren Spiel Regeln und Player-Einstellungen möglicherweise. Zu diesem Zweck ein vorhandenes Netzwerk muss zuerst erfolgen kann abgefragt werden durch Aktivieren der `XimAllowedPlayerJoins.Queried` Joinability und konfigurieren die Netzwerkinformationen für andere Personen verfügbar, außerhalb des Netzwerks durch einen Aufruf von `XboxIntegratedMultiplayer.SetNetworkConfiguration()`.

Im folgenden Beispiel wird `XimAllowedPlayerJoins.Queried` Joinability, legt Netzwerkkonfiguration mit einer Teamkonfiguration, die eine Summe von 1 bis 8 Spielern zusammen in 1-Team, ein Spielmodus app-spezifischen Konstanten uint64_t definiert durch den Wert GAME_MODE_BRAWL, ermöglicht eine Beschreibung "Cat" und "des Schaf Boxing Übereinstimmung mit", ein app-spezifische Zuordnung Index Konstanten uint32_t definiert durch den Wert MAP_KITCHEN und Spectatorallowed"Tags"Chatrequired","einfach"," enthält:

```cs
string[] tags = { "chatrequired", "easy", "spectatorallowed" };
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Queried;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = GAME_MODE_BRAWL;
networkConfiguration.Description = "Cat and sheep's boxing match";
networkConfiguration.MapIndex = MAP_KITCHEN;
networkConfiguration.SetTags(tags);
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Weitere Entwicklungen außerhalb des Netzwerks finden klicken Sie dann das Netzwerk durch Aufrufen von Xim::start_joinable_network_query() mit einem Satz von Filtern, die die Netzwerkinformationen auf dem vorherigen Aufruf von Xim::set_network_configuration() entsprechen. Im folgende Beispiel wird eine Abfrage für die beigetreten Netzwerk gestartet, mit der Spielmodus Filteroption, die nur für Netzwerke, die mit der app-spezifische Spielmodus definiert, nach Wert GAME_MODE_BRAWL abgefragt werden:

```cs
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Hier ist ein weiteres Beispiel, das Tag filtern können Abfragen für Netzwerke, die mit Tag "einfach" und "Spectatorallowed" in ihrer öffentlichen abfragbare Konfiguration verwendet:

```cs
string[] tagFilters = { "easy", "spectatorallowed" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Optionen für verschiedene Filter können auch kombiniert werden. Im folgenden Beispiel, das sowohl die Spielmodus Filteroption aus, und kennzeichnen verwendet Filtern Option aus, um eine Abfrage für Netzwerke, dass sowohl die Spielmodus app-spezifische Konstante GAME_MODE_BRAWL und Tag "einfach" zu starten:

```cs
string[] tagFilters = { "easy" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Wenn der Abfragevorgang erfolgreich ist, erhält die app eine Xim_start_joinable_network_query_completed_state_change aus der die app eine Liste der beigetreten werden kann Netzwerke abrufen kann. Die app wird außerdem kontinuierlich empfangen `XimJoinableNetworkUpdatedStateChange` für zusätzliche beigetreten Netzwerke oder Änderungen, die auf die zurückgegebene Liste von beigetreten Netzwerken auftreten, bis er entweder manuell oder automatisch beendet wird. Die Abfrage in Ausführung befindlichen kann manuell beendet werden, durch den Aufruf `XboxIntegratedMultiplayer.StopJoinableNetworkQuery()`. Es wird automatisch beendet beim Aufrufen von `XboxIntegratedMultiplayer.StartJoinableNetworkQuery()` um eine neue Abfrage zu starten.

Die app kann versuchen, ein Netzwerk in der Liste der beigetreten werden kann Netzwerke durch Aufrufen von verknüpfen `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation()`. Im folgende Beispiel wird davon ausgegangen, dass Sie ein Netzwerk auf die verwiesen wird durch "SelectedNetwork" die nicht durch eine Kennung gesichert ist (sodass es sich um eine leere Zeichenfolge an den zweiten Parameter übergeben wird) verknüpfen möchten:

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation(selectedNetwork, "");
```

Beim Aktivieren der Netzwerk-Abfrage mit einem `XimNetworkConfiguration.TeamConfiguration` , zwei oder mehr Teams deklariert, Spieler, die durch Aufrufen von XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation() wird eingebunden haben einen Standardwert für die Index 0.

> [!NOTE]
> Wenn die app verfügt über mehrere lokale Benutzer angegeben und ist ein Netzwerk mit weniger Platz als die Anzahl der lokalen Benutzer verknüpfen, kann die Verknüpfung weiterhin erfolgreich. Aber nur die zulässige Anzahl von lokale Benutzer kann mit dem Netzwerk.