---
title: Spiele-Chat-2-Übersicht
description: Erfahren Sie, wie Ihr Spiel Xbox Live-Game-Chat 2, eine aktualisierte Version des Spiels Chat mit Sprachkommunikation hinzugefügt.
ms.date: 10/20/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Spiele Chat, Spiele Chat 2, Sprachkommunikation
ms.localizationpriority: medium
ms.openlocfilehash: 9f013f8b80cc7bca367c3ef5cd2c0d1da86cc98c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615825"
---
# <a name="game-chat-2-overview"></a>Übersicht über die Spiele-Chat-2

Spiele-Chat-2 können Sie problemlos Sprache und Text-Chat-Kommunikation zu Ihrer app hinzufügen, während unter Beachtung der datenschutzeinstellungen für Ihre Spieler,, und erfüllen die Xbox-Anforderungen für ein Xbox-Spiele und Hub-Apps, die in Bezug auf Sprache und Text-Chat. Für Spieler, die Sprache-zu-Text oder Text-Sprach-Konvertierung über die erleichterte Bedienung - Spiel, Chat-Aufzeichnung-Einstellungen aktiviert haben führt Spiel Chat 2 transparent Übersetzungen Erstellung Chat SMS-Nachrichten darstellt, eingehende Sprach-Audio- und play Spracherkennung Audio für ausgehende Nachrichten für die Chat-Text, bzw. synthetisiert.

> [!NOTE]
> Wenn Sie für eine bestimmte API-Referenz suchen, finden Sie es in den herunterladbaren Xbox Live-API-kompilierten HTML-Hilfeseite (chm) Datei [hier](https://aka.ms/xboxliveuwpdocs).

- **Kommunikationsbeziehungen** -Spiel, Chat 2 bietet Ihnen eine präzise Steuerung, wie Ihre Spieler mit jeder anderen kommunizieren können. Anstatt von Teams oder Kanäle Spiel, Chat 2 explizite Beziehungen zwischen jedem Paar von Benutzern definiert werden muss. Spiele-Chat-2-kommunikationsbeziehungen unterstützt unidirektionale und bidirektionale Kommunikation zwischen eines beliebigen Paars von Spieler. Sprache und Text kommunikationsbeziehungen können unabhängig voneinander festgelegt werden.

- **Barrierefreiheit** -Spiel, Chat-2 unterstützt, Sprache-zu-Text und die Sprachsynthese. Wenn aktiviert, wird die Einstellung "Game-Chat-Aufzeichnung" Ihre Spieler, berücksichtigt und führt transparent Übersetzungen Erstellung Chat SMS-Nachrichten, die Audio für eingehende Sprach- und Play synthetisiert Spracherkennung Audio für ausgehende Chat-Text darstellt Spiel Chat 2 Nachrichten, bzw.

- **Xbox Live-Integration** -Spiel-Chat-2, die Xbox Live-Dienste verwendet, um die stellt sicher, dass die Einstellungen und Berechtigungen des Spielers berücksichtigt werden.

- **Voice-Erkennung Aktivität** -Spiel, Chat 2 führt die Erkennung von Sprachaktivität um zu bestimmen, wenn Audiodaten sprachaktivität enthält.

- **Automatische erhalten Sie die Kontrolle** -Spiel, Chat 2 führt automatische erhalten-Steuerelement, um die Abweichung eines Benutzers Mikrofon Ausgabe zu minimieren.

- **Codecs** -Spiel, Chat 2 codiert, Audiodaten, die remote-Instanzen der app bereitgestellt werden müssen. Auf der Xbox One werden diese Codierung (und Decodierung von der empfangenden Seite) hardwarebeschleunigt.

- `chat_manager::start/finish_processing_state_changes` -Das Paar von Methoden, der von der app jedes UI-Frames zum Durchführen asynchroner Vorgänge, zum Abrufen der Ergebnisse in Form von behandelt werden aufgerufen `game_chat_state_change` Strukturen, und klicken Sie dann auf, um die zugehörigen Ressourcen nach Abschluss freizugeben.

- `chat_manager::start/finish_processing_data_frames` -Das Paar von Methoden zum Spiel, Chat 2 in der app-Transportschicht eingebunden werden soll. Diese Methoden werden von der app jedes Einzelbild "Netzwerk" zum Abrufen und Verteilen von aufgerufen `game_chat_data_frame` Objekte für Instanzen der app auf Remotegeräten, und klicken Sie dann auf, um die zugehörigen Ressourcen nach Abschluss freizugeben.

- `chat_manager::process_incoming_data` – Die Methode verwendet, Daten zu Spiel, Chat 2 zu geben, die von einer Remoteinstanz von Spiel, Chat 2 über den app-Transportebene übermittelt wurde.

- **Real-Time-Audio-Manipulation** -Spiel Chat 2 ermöglicht der app, die sich in der Chat-audio-Pipeline einfügen, sodass möglicherweise überprüfen oder Bearbeiten von Audiodaten Chat. Finden Sie unter [in Echtzeit audio Manipulation](real-time-audio-manipulation.md) für Weitere Informationen.

Die app informiert die Bibliothek der Benutzer auf dem lokalen Gerät und Benutzer auf Remotegeräten, die miteinander sprechen erwartet werden. Die app, die Beziehungen zwischen den einzelnen Benutzer konfiguriert.

Nach der Konfiguration Spiel, Chat-2 Audio aus der lokalen Benutzer Microphone(s) abruft, führt die automatische Gewinn Kontrolle und stimme Aktivität Erkennung, codiert die Daten, und macht Audiodaten, die mit Remoteinstanzen des Spiels Chat 2 über übermittelt werden `chat_manager::start/finish_processing_data_frames`. Die app muss die Daten in bieten die `game_chat_state_change` Objekt, das die Remoteinstanzen von Spiel-Chat 2 in das gleiche Objekt angegeben. Nach dem Empfang der das, müssen remote-Instanzen der app übermitteln Sie Daten an eine eigene Instanz des Spiels Chat 2 über `chat_manager::process_incoming_data`. Spiel, Chat 2 die eingehenden Daten decodiert, und klicken Sie dann auf dem lokalen Gerät des Benutzers audio gerendert.

Spiel, Chat 2 erzwingt Xbox Live Anforderungen für Berechtigungen und Datenschutz. Audiodaten werden nicht von Benutzern, die Kommunikation von Berechtigungen nicht generiert werden, und Audiodaten von Benutzern, die über datenschutzeinstellungen blockiert werden nicht gerendert werden.

Informationen zum Einstieg finden Sie unter [mithilfe von Spiel-Chat 2](using-game-chat-2.md). Bei Verwendung von C#, finden Sie unter [mithilfe von Spiel-Chat 2 WinRT Projektionen](using-game-chat-2-winrt.md). Wenn Sie bereits über eine Spiel-Chat-Implementierung verfügen, verwenden Sie die [Spiel Chat-2-Migrationshandbuch](game-chat-2-migration.md) um Spiel Chat-Konzepte und Aufrufmuster Spiel, Chat 2 analoge zuzuordnen.

Informationen zu der Spiel Chat-2-API laden Sie die Xbox-API-Referenz: [XboxAPIs.zip](https://aka.ms/xboxliveuwpdocs)