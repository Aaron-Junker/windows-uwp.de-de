---
title: Melden Sie sich mit den PlayerAuthentication Prefabs
description: Übersicht über die Unity-Plug-in-PlayerAuthentication prefabs
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, unity
ms.openlocfilehash: ea161ff801e2004569d88d53c78ae963e91b4ce6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605735"
---
# <a name="easy-sign-in-with-the-playerauthentication-prefab"></a>Einfache melden Sie sich mit den PlayerAuthentication Prefabs

Die PlayerAuthentication-Prefab ist die einfachste Möglichkeit zum Titel des Xbox Live-Authentifizierung hinzugefügt. Es dauert nur drei einfache Schritten, die von der eine neue Szene zu einer Anmeldeseite zu gelangen.

1. Ziehen Sie das Prefab PlayerAuthentication in der Szene
2. Ziehen Sie ein XboxLiveServices Prefab in der Szene
3. Hinzufügen einer EventSystem der Szene (technisch gesehen der PlayerAuthentication wird für Sie erstellen, wenn ein EventSystem nicht vorhanden ist, aber das Hinzufügen von sie guten Gewohnheit ist.)

Und das ist es. Sie können jetzt ein Spieler in XboxLive durch Klicken auf das Prefab PlayerAuthentication in Ihrer Szene in der Titelleiste Ihres anmelden. Testen Ihre Szene in Unity durch Klicken auf die Wiedergabeschaltfläche führt dazu, dass Ihre Prefab gefälschte Daten zu generieren, da die Unity-Player an den Xbox Live-Dienst keine Verbindung herstellen kann. Um eine echte Anmeldung anzuzeigen, Sie das Projekt in Visual Studio lokal zu erstellen müssen. Wenn in der Titelleiste Ihres konfiguriert wurde, Partner Center, und Sie müssen ein Microsoft-Konto/Gamertag für die Anmeldung bei Ihrem Titels autorisiert und können dann Anmelden eines Ihrer autorisierten Konten in einem Visual Studio-Build können.

Die PlayerAuthentication-Prefab-Skript weist auf ein paar Einstellungen, die Sie aus der Sicht im Inspektor bearbeiten können.

![Screenshot der PlayerAuthentication-Inspektor](../images/unity/playerauthentication_prefab_inspector.JPG)

* Anzahl der Spieler - bestimmt den Player, der den Zugriffsbereich anmelden verknüpft ist
* Design – ändert das Farbschema für das Panel-Anmeldung für, wenn ein Benutzer angemeldet oder abgemeldet. Diese Einstellung hat eine Option helle oder dunkel.
* Aktivieren Sie Controller-Eingabe: dieses Kontrollkästchen ermöglicht das für Spieler einen Xbox-Controller zum an- und Abmelden verwenden das Prefab PlayerAuthentication verwenden.
* Anzahl der Joystick - bestimmt, der Controller, der unsere Out anmelden kann mithilfe der Prefab.
* Melden Sie sich die Schaltfläche "- und eine Dropdownliste können Sie auswählen, welche Schaltfläche auf eine Xbox-Controller ein Benutzer anmeldet.
* Melden Sie sich die Schaltfläche "- und eine Dropdownliste können Sie auswählen, auf welche Schaltfläche auf eine Xbox-Controller, ein Benutzer anmeldet.

## <a name="multiplayer-scenario"></a>Multiplayer-Szenario

Zusätzlich zur Anmeldung einen Spieler können Sie auch mehrere PlayerAuthentication prefabs (Vorlagen) verwenden, lokales Multiplayer-Spiele für Xbox One-Konsole Titel zu implementieren. Durch Hinzufügen mehrerer Instanzen der prefabs aus, und ändern das Player-Anzahl-Attribut der einzelnen, können Sie den Titel in mehrere Benutzer anmelden.

> [!WARNING]
> Anmelden bei mehreren Gamertags ist für Windows 10-PCs nicht zulässig. Um mehrere Benutzer anmelden, müssen Sie Ihr Spiel auf einem einer Xbox-Konsole zu testen.

Erstellen eine Szene, die Multiplayer-ermöglicht ist nur geringfügig schwieriger zu verwenden das Prefab PlayerAuthentication.

1. Ziehen Sie eine Instanz der PlayerAuthentication prefabs in der Szene
2. Überprüfen Sie die **aktivieren Controller Eingabe** das Prefab Inspektor im Feld.
3. Stellen Sie sicher, dass die **Player Anzahl** und **Joystick Anzahl** auf 1 festgelegt.
4. Weisen Sie die **In Schaltfläche Anmelden** aus der Dropdown-Menü.
5. Weisen Sie die **Out Schaltfläche Anmelden** aus der Dropdown-Menü.
6. Ziehen Sie eine *zweite* Instanz die PlayerAuthentication prefabs auf die Szene.
7. Überprüfen Sie die **aktivieren Controller Eingabe** das Prefab Inspektor im Feld.
8. Stellen Sie sicher, dass die **Player Anzahl** und **Joystick Anzahl** auf 2 festgelegt werden.
9. Weisen Sie die **In Schaltfläche Anmelden** aus der Dropdown-Menü.
10. Weisen Sie die **Out Schaltfläche Anmelden** aus der Dropdown-Menü.
11. Ziehen Sie ein XboxLiveServices Prefab in der Szene
12. Fügen Sie in die Szene ein EventSystem

Prüfen Sie, dass die prefabs (Vorlagen) ordnungsgemäß, indem drücken Play im Unity-Player funktioniert und durch Klicken auf den prefabs (Vorlagen). Sie gibt falsche Daten zurück, die Unity-Player, Xbox Live connect kann nicht erwartet wird. Mit zwei Instanzen der PlayerAuthentication prefabs so konfiguriert, dass verschiedene Spieler und Joysticks, die Sie Ihr Spiel in Visual Studio dies erstellen möchten können sie ordnungsgemäß auf eine Xbox-Konsole getestet werden. Nachdem Ihr Spiel erstellt wurde, öffnen Sie die Projektmappendatei in Visual Studio, die Sie benötigen, um Unterstützung für mehrere Benutzer für Ihr Spiel zu aktivieren.
Gehen Sie dazu wie folgt vor:

1. Suchen Sie im Projektmappen-Explorer für die Datei "Package.appxmanifest.xml"
2. Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie Code anzeigen
3. Unter den `<Properties><\/Properties>` Abschnitt, fügen Sie die folgende Zeile hinzu: "< Uap:SupportedUsers > mehrere <\/Uap:SupportedUsers >.
4. Stellen Sie das Spiel auf der Xbox durch Starten eines remote Debuggen-Builds in Visual Studio bereit. Finden Sie Anweisungen zum Einrichten Ihrer Titels auf eine Xbox in die [Ihrer UWP auf Xbox-Entwicklungsumgebung einrichten](../../xbox-apps/development-environment-setup.md) Artikel.

> [!NOTE]
> Der Teil der Konfiguration wurde geändert sieht möglicherweise wie es mehrere Spieler ermöglicht, aber es weiterhin erforderlich ist ist, Ihr Spiel einzelner Player für Szenarien mit.

Nachdem Sie die PlayerAuthentication prefab haben arbeiten Sie weitere Informationen zu scripting melden Sie sich mit den Artikel [melden Sie sich mit der Manager-Anmeldung in Unity](sign-in-manager.md).