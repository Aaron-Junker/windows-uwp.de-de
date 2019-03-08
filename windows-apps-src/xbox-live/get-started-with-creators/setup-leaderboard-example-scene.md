---
title: Verwenden Sie die Szene Leaderboard-Beispiels in Unity
description: Zeigt die Schritte zur Einrichtung ordnungsgemäß, bis der Unity-Szene Bestenliste
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Unity, Bestenlisten
ms.openlocfilehash: d931a0fdcbdec5dd9deb53876a19e1b475afcb93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589745"
---
# <a name="the-leaderboard-example-scene-in-unity"></a>Der Leaderboard-Beispiel-Szene in Unity

[Die Unity-Plug-Ins](https://github.com/Microsoft/xbox-live-unity-plugin) umfasst eine Reihe von Beispiel-Szenen, die Xbox Live-Dienste für Unity-Entwickler zu veranschaulichen. Eine solche Szene ist der Leaderboard-Beispiel-Szene. Im folgenden Screenshot sehen Sie sich, dass die Leaderboard-Beispiel-Szene einfach ein Panel-Anmeldung für den Xbox Live-Benutzer und ein Bereich, enthalten die Bestenliste angezeigt. Wenn Sie die Wiedergabe an diesem Punkt erreichen, ohne in diese Szene finden Sie im Bereich-Anmeldung mit falschen Benutzerdaten aufgefüllt wird, aber die Bestenliste lädt keine Informationen. Um diese Beispiel-Szene, um eine tatsächliche Bestenliste zu laden, Sie einige Ergänzungen durchführen müssen, zu erhalten.

![Screenshot der Bestenliste](../images/unity/leaderboard-scene-1804.JPG)

## <a name="prerequisites"></a>Voraussetzungen

Leaderboards in Xbox Live basieren auf die Statistiken in den Xbox Live-Statistik-Dienst. Bevor Sie die Bestenliste mit Daten auffüllen können, die Sie einige Statistiken Ihrer Testkonten zugeordnet haben müssen. Wenn Sie nicht bereits Statistiken auf den Titel hinzugefügt haben Sie erfahren, wie durch Lesen [Player-Statistiken und Bestenlisten hinzufügen, um Ihr Unity-Projekt](add-stats-and-leaderboards-in-unity.md). Kehren Sie nach Durchführen der Aktionen im Abschnitt "Stats" des Artikels hierher zurück zum Anzeigen des Status in der Beispiel-Leaderboard-Szene.

## <a name="the-leaderboard-inspector"></a>Der Bestenlisten Inspektor

Das Leaderboard-Prefab bietet eine Reihe von Einstellungen, die im Abschnitt "Inspector" für die Bestenliste mit der Skriptkomponente, wie z. B. die Benutzeroberfläche geändert werden können *Design*, der Spieler die Bestenliste, Xbox-Controller-Einstellungen, zugeordnet und andere Leaderboard-Einstellungen. Sie sehen, dass die Leaderboard-Einstellungen in ein paar unterschiedliche Abschnitte in der folgenden Inspektor aufgeteilt werden.

![Bildschirmabbildung von Bestenlisten Inspektor](../images/unity/leaderboard_script_inspector.JPG)

### <a name="theme-and-display-settings"></a>Einstellungen für Design und der Anzeige

Dieser Abschnitt enthält eine Einstellung *Design*. Dies ist eine einfache Dropdownliste unten die dunkle oder helle Design für Ihre Leaderboard-prefab verwenden können. Dies ändert die Hintergrundfarbe, Schriftart und Bildfarben den prefabs. Der Effekt ist leicht erkennen, wenn Sie die Szene in der Unity-Player wiedergeben.

![Design "hell"](../images/unity/leaderboard_light_theme.JPG) ![Design "dunkel"](../images/unity/leaderboard_dark_theme.JPG)

### <a name="stat-configuration"></a>STAT-Konfiguration

In diesem Abschnitt können Sie bestimmen, welche Art von Daten einbezogen werden sollen, wenn die Bestenliste mit den Zeilen gefüllt ist.

- **Anzahl der Spieler** -dieser Wert gibt die Player die Bestenliste zugeordnet ist.
- **STAT** -der Statistik, die zum Auffüllen der Leaderboard-Daten verwendet. Dies ist erforderlich, damit das Leaderboard-Prefab zum Laden von Daten.
- **Leaderboard-Typ** -dieses Dropdown-Menü wendet einen Filter auf das Leaderboard-Daten geladen. Wenn *Global* ausgewählt ist die Bestenliste werden nicht gefiltert und zeigt, dass jeder Spieler mit einem Wert für den ausgewählten Status. Wenn *Freunde* ausgewählt ist die Bestenliste werden, nur anzeigen Spieler auf die Bestenliste, die auch in der Freundesliste Ihrer werden gefiltert. Wenn *bevorzugten* ausgewählt ist die Bestenliste wird gefiltert, um nur anzeigen Spieler auf die Bestenliste, die auch in der Favoritenliste aufgeführt sind.
- **Anzahl der Einträge** -einen Schieberegler mit einem Bereich von 1 bis 100, die bestimmt, wie viele Zeilen aus der Bestenliste zu einem Zeitpunkt zurückgegeben werden. Die Anzahl festgelegten hier bestimmt die Anzahl der Leaderboard-Zeilen pro Seite angezeigt wird.

### <a name="controller-configuration"></a>Controller-Konfiguration

Das Leaderboard-Prefab kann Entwickler auf einfache Weise Konfigurieren von Xbox-Controller verwenden. Der Controller-Konfigurationsabschnitt der prefabs können Sie aktivieren, und wählen Sie die Schaltflächen, die das Leaderboard-Prefab steuern.

- **Aktivieren der Controller Eingabe** -eine Schaltfläche einfaches Optionsfeld, ein-/ausschalten. Wenn diese Option aktiviert ist können Sie einen Xbox-Controller für die Interaktion mit der Prefab verwenden. Für Domänencontroller-Unterstützung erforderlich sind.
- **Anzahl der Joystick** -legt fest, welcher Controller mit diesem Leaderboard-prefab interagieren kann.
- **Nächste Seite-Schaltfläche Controller** -Dropdownmenü, welche Steuerelemente, auf welche Schaltfläche die nächste Seite der Leaderboard-Zeilen lädt.
- **Vorherige Seite-Schaltfläche Controller** -Dropdownmenü, welche Steuerelemente, auf welche Schaltfläche die vorherige Seite des Leaderboard-Zeilen lädt.
- **Schaltfläche "Weiter View Controller"** -Schaltet den Ansichtstyp zwischen **alle** und **nächste mich**.
- **Schaltfläche "Vorheriges View Controller"** -Schaltet den Ansichtstyp zwischen **alle** und **nächste mich**.
- **Vertikalen Bildlauf Eingabe Achse** -Zeichenfolge, die festlegt, welche Achse Controller mit Bildlauf zugeordnet ist.
- **Führen Sie einen Bildlauf Geschwindigkeit Multiplikator** -Controller Scroll Geschwindigkeit bestimmt.

> [!NOTE]
> Ändern die Schaltflächen zum Ändern der Seite oder Ansicht, der die Bestenliste ändert sich auch auf das Bild der Schaltfläche verwendet wird, für die einzelnen Funktionen des die Bestenliste zugeordnet. Sie müssen sich nicht zum Ändern der Abschnitt Verweise auf die Benutzeroberfläche manuell, um das Bild, um die Schaltfläche mit den übereinstimmen.

### <a name="ui-references"></a>Verweise auf die Benutzeroberfläche

In diesem Abschnitt werden Images und allgemeine Zusammensetzung der Leaderboard-Prefab gesteuert. In diesem Abschnitt muss nicht für die erfolgreiche Verwendung der Bestenliste prefabs geändert werden. Jedoch unter Umständen müssen Sie Anpassungen vor, um diesen Abschnitt, um das Aussehen der prefabs für Ihre eigenen Zwecke anpassen.

## <a name="populating-the-unity-player-leaderboard-with-fake-data"></a>Füllen die Unity-Player-Bestenliste (mit gefälschten Daten)

Um die Unity-Player-Bestenliste mit Daten aufzufüllen, müssen Sie eine Statistik der Leaderboard-Prefab hinzufügen. Anzeigen der Bestenliste prefabs im Inspektor wird ersichtlich, dass sie ein Objekt des Typs nutzen kann `Stat Base` als ein öffentlicher Parameter in das Skript. Sie können eine der Ziehen der `State Base` geben prefabs (Vorlagen) `IntegerStat`, `DoubleStat`, oder `StringStat` aus dem Ordner "Prefabs" von der Xbox Live Unity-Plug-in und platzieren Sie sie an dieser Stelle im Leaderboard prefab.

![Drag & Drop-Stat Prefabs](../images/unity/stat-to-leaderbaord-drag.gif)

Spielen Sie die Szene jetzt in Unity, und sehen Sie, dass die Bestenliste mit gefälschten Daten, wie unten aufgefüllt wird.

![Screenshot der gefälschten Daten aufgefüllt-Bestenliste](../images/unity/leaderboard-fake-data-1804.JPG)

## <a name="populating-a-visual-studio-built-project-with-real-data"></a>Auffüllen eines Visual Studio-Projekt mit echten Daten erstellt

Um die Bestenliste mit echten Daten für den Titel aufzufüllen, Sie Ihr Spiel auf Sie Computer lokal erstellen müssen. Sie benötigen einen lokalen Build, da im Unity-Editor keinen Zugriff auf Xbox Live. Neben der Erstellung von Ihrem Projekt lokal ausführen, müssen Sie die Stat in Ihre Bestenliste zum Status zu konfigurieren, die initialisiert wird und Werte für den Titel. Um eine Statistik zu Ihrem Bestenliste zuzuordnen müssen Sie so ändern Sie die ID und Anzeigename des Stat-Objekts in der Leaderboard-Prefab. Die ID muss der Status im konfigurierten entsprechen [Partner Center](https://partner.microsoft.com/dashboard). Nachdem Sie dies getan haben, erstellen Sie das Projekt, unter der [Abschnitt des Fensters konfigurieren, die Xbox Live in Unity-Artikel "build"](configure-xbox-live-in-unity.md#build-and-test-the-project). Dieses Projekt als x X64 ausführen ermöglicht erstellen, die auf dem lokalen Computer Ihnen die Anmeldung mit einem echten Gamertag und füllen Sie die Bestenliste mit echten Daten.