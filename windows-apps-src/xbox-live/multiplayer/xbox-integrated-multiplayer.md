---
title: Xbox-integrierten Multiplayer-Spiele
description: Erfahren Sie mehr über Xbox Integrated Multiplayer (XIM), eine All-in-One-Lösung für Multiplayer/Netzwerk/Chats für Xbox Live-Spiele.
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Xbox integrierte Multiplayer-Spiele
localizationpriority: medium
ms.openlocfilehash: aa82b1042f710d81d83b98767802c7538fd53fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641775"
---
# <a name="xbox-integrated-multiplayer-xim"></a>Xbox Integrated Multiplayer (XIM)

- [Übersicht über die](#overview)
- [Konzepte](#concepts)
- [Features](#features)
- [Beziehung zu anderen Modulen](#relationship-to-other-modules)

## <a name="overview"></a>Übersicht

Xbox Integrated Multiplayer (XIM) ist eine eigenständige Schnittstelle zum einfachen Hinzufügen von Echtzeitfunktionen für Multiplayernetzwerke und Chatkommunikation zu Ihrem Spiel, die auf den leistungsstarken Xbox Live-Diensten aufbaut. Die Schnittstelle XIM erfordert kein Projekt auswählen, die zwischen dem Kompilieren mit C++ / CX im Vergleich zu herkömmlichen C++; Es kann entweder mit verwendet werden. Die Implementierung auslösen nicht auch Ausnahmen, als Mittel zum nicht schwerwiegenden Fehler, die Berichterstattung, damit Sie es einfach aus Projekten ausnahmefreie nutzen darf, wenn bevorzugt.

Informationen zu den ersten Schritten finden Sie unter [Verwenden von XIM](xbox-integrated-multiplayer/using-xim.md). Bei Verwendung von C#, finden Sie unter [mithilfe XIM C# ](xbox-integrated-multiplayer/using-xim-cs.md).

## <a name="concepts"></a>Konzepte

XIM wird in einigen grundlegenden Begriffen orientiert:

- XIM Network – eine logische Darstellung einer Reihe von miteinander verbundenen Benutzer, die Teilnahme an einer bestimmten Multiplayer-Erfahrung als auch Grundzustand, die dieser Sammlung beschreibt. Die Teilnehmer können sich jeweils nur in einem XIM-Netzwerk befinden, Sie können jedoch problemlos von einem konzeptionellen XIM-Netzwerk in ein anderes wechseln.
- Vermittlung - optional Vorgang zum Ermitteln zusätzlicher remote Spieler mit ähnlichen Interessen und Fähigkeiten in einem Netzwerk XIM teilnehmen, ohne dass einer vorhandenen Beziehung auf soziale Netzwerke.
- Abfragen – optional Vorgang zum Ermitteln von XIM Netzwerke, ohne dass einer vorhandenen social-Beziehung zwischen Teilnehmern.
- `xim_player` – Ein Objekt, das einen einzelnen menschlichen Benutzer angemeldet, die auf einem Gerät lokal oder remote und Teilnahme an einem Netzwerk XIM darstellt. Ein einzelner physischer Benutzer, der dem gleichen XIM-Netzwerk beitritt, es verlässt und wieder beitritt wird als zwei separate Spielerinstanzen angesehen.
- `xim_state_change` – Eine Struktur, die eine Benachrichtigung auf dem lokalen Gerät in Bezug auf eine asynchrone Änderung in einem bestimmten Punkt des Netzwerks XIM darstellt.
- `xim::start_processing_state_changes` und `xim::finish_processing_state_changes` -das Paar von Methoden, der von der app jedes UI-Frames zum Durchführen asynchroner Vorgänge, zum Abrufen der Ergebnisse in Form von behandelt werden aufgerufen `xim_state_change` Strukturen, und klicken Sie dann auf, um die zugehörigen Ressourcen nach Abschluss freizugeben.

Auf einer sehr hohen Ebene verwendet der spielanwendung die XIM-Bibliothek so konfigurieren Sie eine Gruppe von Benutzern angemeldet, auf dem lokalen Gerät um als neuen Spieler in einem Netzwerk XIM verschoben werden. Die app ruft `xim::start_processing_state_changes` und `xim::finish_processing_state_changes` jedes UI-Frames. Wenn app-Instanzen auf Remotegeräten ihrer Benutzer in einem Netzwerk XIM hinzufügen, wird jede beteiligte Instanz bereitgestellt `xim_state_change` Updates, die beschreiben, die lokal und Remote `xim_player`s, XIM Network beitreten. Wenn ein Spieler seine Teilnahme am XIM-Netzwerk beendet (selbst ausgelöst oder aufgrund von Netzwerkverbindungsproblemen), werden `xim_state_change`-Updates an alle App-Instanzen bereitgestellt, dass der `xim_player` das Netzwerk verlassen hat.

Eine App kann auf verschiedene Weisen das XIM-Netzwerk erkennen, an dem teilgenommen werden soll. Häufig starten die Apps, indem sie lokale Benutzer in ein neues Netzwerk automatisch verschieben, das den Freunden des Benutzers zur Verfügung steht, wobei die lokalen Benutzer Einladungen senden können oder das XIM-Netzwerk als Teilnahmeaktivität anzeigen lassen können (z. B. über Spielerkarten). Nachdem diese über Social Media ermittelten Benutzer bereit sind, kann die App den „Spielersuche“-Prozess von Xbox Live initiieren und alle Spieler in ein neues XIM-Netzwerk verschieben, das auch zusätzliche „abgestimmte“ Remotespieler für Teams und nach Bedarf Gegnerteams enthält. Nach Abschluss dieses Multiplayererlebnisses können die App-Instanzen ihre lokalen Spieler – und optional auch die ursprünglichen Remotespieler vor der Spielersuche – in ein neues privates XIM-Netzwerk oder in ein anderes zufällig über die Spielersuche gefundenes XIM-Netzwerk verschieben. Sprach- und Text-Chat bleiben die ganze Zeit verfügbar. Dieses einfache Verschieben von Spielern zwischen XIM-Netzwerken ist ein zentraler Bestandteil der API und reflektiert die heutigen Erwartungen an gute und hochentwickelte Social Gaming-Erfahrungen.

Im Gegensatz zu einem Client / Server-Modell ist ein XIM-Netzwerk logisch ein vollständig verbundenen Mesh von Peer-Geräten. Wie im Abschnitt dieses Dokuments beschrieben wird, kann alle Player direkt an eine beliebige andere über die API senden. Alle Methoden, die Auswirkungen auf den Status des Netzwerks XIM als Ganzes können von jedem beteiligten Gerät aufgerufen werden.

XIM verwendet einfache Last-Write-Wins-konfliktlösung, wenn die app ansonsten nicht verhindern, dass mehr als ein Teilnehmer, die den gleichen XIM Netzwerkstatus gleichzeitig effektiv ändern. Dies bedeutet, dass es sich bei XIM alle Rolle Konzepte für "Host" oder "Server" vorgibt. XIM keine apps verwenden eigene Konzepte, z. B. die Unterstützung für die Migration von app-definierte Rollen an einen anderen Teilnehmer, wenn ein Player einen XIM Netzwerk verlässt, auch einschränken.

## <a name="features"></a>Features

- Spiel bereitstellt, mit der Sprache und Text-Chat-Kommunikation, die berücksichtigt und berücksichtigt die Player-datenschutzeinstellungen

    Sprach- und Text-Chat-Kommunikation wird automatisch auch für alle Spieler bereitgestellt, sofern die Einstellungen für Datenschutz und die App-Konfiguration dies zulassen. Für Spieler, bei denen Sprache-zu-Text- oder Text-zu-Sprache-Konvertierung aktiviert ist, führt XIM diese Übersetzung transparent aus, sodass Chat-Textnachrichten gesendet werden, die die eingehenden Sprachaudio-Nachrichten darstellen, sowie Sprachsynthese-Audio für ausgehende Chat-Textnachrichten.

- Ermöglicht das Spiel um einen eigenen spezifischen Datennachrichten zu senden

    Innerhalb des XIM-Netzwerks kann die App eigene spielspezifische Datennachrichten senden, z. B. Avatar-Bewegungsupdates. Alle empfangenen Nachrichten werden an die App als ein `xim_state_change` übermittelt, der die gewünschte Quelle und die lokalen Ziele anzeigt.

- Fungiert als ein dedizierter Chat-Lösung über die Out-of-Band-Reservierungen

    Eine ausführliche Dokumentation zur Verwendung von XIM über Out-of-Band-Reservierungen finden Sie unter [XIM-Reservierungen](xbox-integrated-multiplayer/xim-reservations.md).

- Ausnahmefreie und kann verwendet werden, mit beiden C++ / CX oder herkömmlichen C++

## <a name="relationship-to-other-modules"></a>Beziehung zu anderen Modulen

XIM dient zur Bereitstellung einer komfortablen All-in-One-Schnittstelle für Spiele mit grundlegenden Multiplayeranforderungen. XIM kapselt die Funktionalität mehrerer Module – insbesondere des `multiplayer_manager`-Moduls der Xbox Services API (XSAPI), der `xbox::services::game_chat_2` -Bibliothek und der sicheren Multiplayer-Netzwerke von `Windows::Networking::XboxLive` – in einer einzelnen optimierten API. Dadurch wird die typische Menge an Code, Aufgaben und Konzepten beim Erstellen von Multiplayerspielen reduziert, bei denen nicht absolut maximale Flexibilität oder Kontrolle erforderlich ist. Apps, deren Anforderungen nicht mit den vereinfachenden Annahmen der XIMs übereinstimmen, sollten diese Komponenten stattdessen jedoch direkt verwenden.

Auch wenn XIM dazu gedacht ist, den Verwaltungsbedarf für Elemente wie Multiplayer Session Directory (MPSD)-Sitzungsdokumente oder den Netzwerktransport über die zugrunde liegenden Komponenten zu vermeiden, schließt dies nicht die gleichzeitige oder Side-by-Side-Nutzung als Teil einer separaten Spielergruppe oder eines Kommunikationsnetzwerks aus. In diesem Fall liegt es in der Verantwortung der App, eine kooperative Netzwerkressourcenauslastung zwischen XIM und den eigenen Mechanismen sicherzustellen. XIM unterstützt derzeit „Out-of-Band-Reservierungen“, um die Verwendung als dedizierte Chat-Lösung zu erleichtern, deren Benutzerliste ausschließlich durch externe Eingabe gesteuert wird.

Xbox Live bietet viele weitere Features, die nützlich für Multiplayerspiele, jedoch weniger direkt an der Einrichtung von Multiplayerchat und Netzwerkkommunikation beteiligt sind, und die daher nicht in diesem Modul einbezogen werden. Apps sollten auf der Xbox Services API (XSAPI) für Player Erfolge, Bestenlisten, Speicher und vieles mehr zu suchen.
