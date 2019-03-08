---
title: Erstellen eines Spiels für die universelle Windows-Plattform (UWP) mit DirectX
description: In diesen Tutorials lernen Sie, wie Sie ein einfaches Spiel für die universelle Windows-Plattform (UWP) mit DirectX und C++ erstellen.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX-Spielbeispiel, Spielbeispiel, Universelle Windows-Plattform (UWP), Direct3D 11-Spiel
ms.date: 12/01/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dc602e2dd29231c1e6554d7ef55e9666a373fa31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642865"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Erstellen eines einfachen Spiels für die universelle Windows-Plattform (UWP) mit DirectX

In diesen Tutorials lernen Sie, wie Sie ein einfaches Spiel für die universelle Windows-Plattform (UWP) mit DirectX und C++ erstellen. Wir befassen uns mit allen wichtigen Teilen eines Spiels. Hierzu zählen die Prozesse zum Laden von Ressourcen wie Grafiken und Gittern, das Erstellen einer Hauptschleife für das Spiel, das Implementieren einer einfachen Rendering-Pipeline sowie das Hinzufügen von Soundeffekten und Steuerelementen.

Wir machen Sie mit den Techniken und Überlegungen für die Entwicklung von UWP-Spielen vertraut. Sie bekommen von uns allerdings kein vollständiges Spiel geliefert. Stattdessen konzentrieren wir uns auf wichtige Konzepte für die Entwicklung von UWP-DirectX-Spielen und weisen auf Windows-Runtime-spezifische Aspekte im Zusammenhang mit diesen Konzepten hin.

## <a name="objective"></a>Ziel

Hier lernen Sie, wie Sie die grundlegenden Konzepte und Komponenten eines UWP-DirectX-Spiels einsetzen. Danach wird Ihnen auch die Entwicklung von UWP-Spielen mit DirectX leichter fallen.

## <a name="what-you-need-to-know-before-starting"></a>Wissenswertes


Für dieses Tutorial müssen Sie mit den folgenden Themen vertraut sein:

-   Microsoft C++ mit Windows-Runtime Language Erweiterungen (C++/CX). Dies ist ein Update für Microsoft C++, das die automatische Verweiszählung einführt. Außerdem handelt es sich hierbei um die Sprache für die Entwicklung eines UWP-Spiels mit DirectX 11.1 oder einer höheren Version.
-   Grundlegende Konzepte der linearen Algebra und der newtonschen Physik.
-   Grundlegende Terminologie für die Grafikprogrammierung.
-   Grundlegende Konzepte der Windows-Programmierung.
-   Grundlegende Kenntnisse der APIs von [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) und [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  <a name="direct3d-uwp-shooting-game-sample"></a>Beispiel für einen Direct3D UWP-Shooter


Dieses Beispiel implementiert einen einfachen Schießstand, an dem der Spieler aus der Ich-Perspektive mit Bällen auf bewegliche Ziele schießt. Wird ein Ziel getroffen, erhält der Spieler eine bestimmte Anzahl von Punkten. Der Spieler kann nacheinander sechs Level mit jeweils steigendem Schwierigkeitsgrad absolvieren. Am Ende der Level werden die Punkte zusammengezählt, und der Spieler erhält einen endgültigen Punktestand.

Das Beispiel veranschaulicht folgende Spielkonzepte:

-   Zusammenarbeit zwischen DirectX 11.1 und der Windows-Runtime
-   Dreidimensionale Ich-Perspektive und entsprechende Kamera
-   Stereoskopische 3D-Effekte
-   Kollisionserkennung zwischen Objekten in 3D
-   Behandlung von Spielereingaben per Maus, Toucheingabe und Xbox-Controller
-   Audiomixing und -wiedergabe
-   Einfacher Spielzustandsautomat

![Das Spielbeispiel in Aktion](images/simple-dx-game-overview.png)

| Thema | Beschreibung |
|-------|-------------|
|[Richten Sie das Spielprojekt](tutorial--setting-up-the-games-infrastructure.md) | Im ersten Schritt für die Erstellung Ihres Spiels richten Sie ein Projekt in Microsoft Visual Studio so ein, dass Sie möglichst wenig Aufwand mit der Bearbeitung der Codeinfrastruktur haben. Sie können sich eine Menge Zeit und Arbeit ersparen, wenn Sie die richtige Vorlage verwenden und das Projekt speziell für die Spieleentwicklung konfigurieren. Im Anschluss finden Sie die erforderlichen Einrichtungs- und Konfigurationsschritte für ein einfaches Spieleprojekt. |
| [Definieren von UWP-app-Framework des Spiels](tutorial--building-the-games-uwp-app-framework.md) | Erstellen Sie ein Framework, das die Interaktion eines UWP-DirectX-Spielobjekts mit Windows ermöglicht. Dazu gehören Windows-Runtime-Eigenschaften wie die Behandlung von Anhalte-/Fortsetzungsereignissen, Fensterfokus und Andocken.  |
| [Spielfluss management](tutorial-game-flow-management.md) | Definieren Sie den übergeordneten Zustandsautomat, um Spieler- und System-Interaktion zu ermöglichen. Hier erfahren Sie, wie UI mit dem Zustandsautomaten für das gesamte Spiel interagiert und wie Sie Ereignishandler für UWP-Spiele erstellen. |
| [Das Spiele Hauptobjekt definieren](tutorial--defining-the-main-game-loop.md) | Definieren Sie, wie das Spiel gespielt wird, indem Sie Regeln erstellen. |
| [Rendering-Framework I: Einführung zu rendern](tutorial--assembling-the-rendering-pipeline.md) | Erstellen eines Renderingframeworks zum Anzeigen von Grafiken. Der Abschnitt hat zwei Teile. Unter Einführung in das Rendern wird erläutert, wie die Szenenobjekte für die Anzeige auf dem Bildschirm dargestellt werden. |
| [Rendering-Framework II: Rendern von Spielen](tutorial-game-rendering.md) | Erfahren Sie im zweiten Teil des Themas Rendern mehr über das Vorbereiten der Daten, die vor der Wiedergabe erforderlich sind. |
| [Fügen Sie eine Benutzeroberfläche hinzu](tutorial--adding-a-user-interface.md) | Fügen Sie einfache Menüoptionen und Heads-up-Anzeigekomponenten hinzu, die dem Spieler Feedback bereitstellen. |
| [Hinzufügen von Steuerelementen](tutorial--adding-controls.md) | Fügen Sie dem Spiel Bewegungs-/Blicksteuerungen hinzu &mdash; grundlegende Fingereingabe, Maus und Gamecontroller-Steuerelemente. |
| [Hinzufügen von sound](tutorial--adding-sound.md) | Enthält Informationen zum Erstellen von Sounds für das Spiel mit [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) APIs. |
| [Erweitern Sie das Beispiel](tutorial-resources.md) | Ressourcen, um Ihre Kenntnisse der DirectX-Spieleentwicklung weiter zu vertiefen, einschließlich die Verwendung von XAML zum Erstellen von Überlagerungen. |