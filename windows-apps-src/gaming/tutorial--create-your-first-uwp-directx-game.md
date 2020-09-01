---
title: Erstellen eines Spiels für die universelle Windows-Plattform (UWP) mit DirectX
description: In diesen Tutorials erfahren Sie, wie Sie DirectX und [C++/WinRT](../cpp-and-winrt-apis/index.md) verwenden, um das Basic universelle Windows-Plattform (UWP)-Beispiel Spiel mit dem Namen **Simple3DGameDX**zu erstellen.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX-Beispiel Spiel, Beispiel Spiel, universelle Windows-Plattform (UWP), Direct3D 11-Spiel
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 284aa821cc58a49f45bed3b0d7e28c20f9d19ba1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163024"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Erstellen eines einfachen Spiels für die universelle Windows-Plattform (UWP) mit DirectX

In diesen Tutorials erfahren Sie, wie Sie DirectX und [C++/WinRT](../cpp-and-winrt-apis/index.md) verwenden, um das Basic universelle Windows-Plattform (UWP)-Beispiel Spiel mit dem Namen **Simple3DGameDX**zu erstellen. Das-Spiel ist in einer einfachen 3D-Galerie mit erster Person vorhanden.

> [!NOTE]
> Der Link, von dem aus Sie das **Simple3DGameDX** -Beispiel Spiel selbst herunterladen können, ist [Direct3D Sample Game](/samples/microsoft/windows-universal-samples/simple3dgamedx/). Der C++/WinRT-Quellcode befindet sich im Ordner mit dem Namen `cppwinrt` . Informationen zu anderen UWP-Beispiel-apps finden [Sie unter Beispiele für UWP](../get-started/get-app-samples.md)-apps.

In diesen Tutorials werden alle Hauptteile eines Spiels behandelt, einschließlich der Prozesse zum Laden von Assets wie z. b. Kunst und Meshes, Erstellen einer Hauptspiel Schleife, Implementieren einer einfachen Renderingpipeline und Hinzufügen von Sound und Steuerelementen.

Außerdem werden Techniken und Überlegungen zur Entwicklung von UWP-spielen angezeigt. Wir konzentrieren uns auf wichtige Konzepte der UWP DirectX-Spieleentwicklung und bezeichnen diese Konzepte durch Windows-Runtime-spezifische Überlegungen.

## <a name="objective"></a>Ziel

Um mehr über die grundlegenden Konzepte und Komponenten eines UWP DirectX-Spiels zu erfahren und die Entwicklung von UWP-spielen mit DirectX zu vertiefen.

## <a name="what-you-need-to-know"></a>Was Sie wissen müssen

Für dieses Tutorial müssen Sie mit diesen Themen vertraut sein.

- [C++/WinRT](../cpp-and-winrt-apis/index.md). C++/WinRT ist eine standardmäßige moderne C++ 17-sprach Projektion für Windows-Runtime (WinRT)-APIs, die als Header dateibasierte Bibliothek implementiert und für den erstklassigen Zugriff auf die modernen Windows-APIs konzipiert ist.
- Grundlegende Konzepte der linearen Algebra und der newtonschen Physik.
- Grundlegende Terminologie für die Grafikprogrammierung.
- Grundlegende Konzepte der Windows-Programmierung.
- Grundlegende Kenntnisse der APIs von [Direct2D](/windows/desktop/Direct2D/direct2d-portal) und [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11).

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Direct3D UWP-Shooting Gallery-Beispiel

Das **Simple3DGameDX** -Beispiel Spiel implementiert eine einfache 3D-Katalog Galerie, bei der der Spieler Bälle beim Verschieben von Zielen auslöst. Wird ein Ziel getroffen, erhält der Spieler eine bestimmte Anzahl von Punkten. Der Spieler kann nacheinander sechs Level mit jeweils steigendem Schwierigkeitsgrad absolvieren. Am Ende der Level werden die Punkte zusammengezählt, und der Spieler erhält einen endgültigen Punktestand.

Das Beispiel veranschaulicht diese Spielkonzepte.

- Zusammenarbeit zwischen DirectX 11.1 und der Windows-Runtime
- Dreidimensionale Ich-Perspektive und entsprechende Kamera
- Stereoskopische 3D-Effekte
- Konflikterkennung zwischen Objekten in 3D
- Behandeln von Player Eingaben für Maus-, Fingereingabe-und Xbox-Controller Steuerelemente
- Audiomixing und -wiedergabe
- Ein grundlegender Spielzustand: Computer

![Das Beispiel Spiel in Aktion](images/simple-dx-game-overview.png)

|Thema|BESCHREIBUNG|
|-------|-------------|
|[Einrichten des Spieleprojekts](tutorial--setting-up-the-games-infrastructure.md)|Der erste Schritt bei der Entwicklung Ihres Spiels ist das Einrichten eines Projekts in Microsoft Visual Studio. Nachdem Sie ein Projekt speziell für die Spieleentwicklung konfiguriert haben, können Sie es später als Vorlage wieder verwenden.|
|[Definieren des UWP-App-Frameworks für das Spiel](tutorial--building-the-games-uwp-app-framework.md)|Der erste Schritt beim Programmieren eines universelle Windows-Plattform-Spiels (UWP) ist das Entwickeln des Frameworks, mit dem das App-Objekt mit Windows interagieren kann.|
|[Spielablaufverwaltung](tutorial-game-flow-management.md)|Definieren Sie den Zustands Automaten auf hoher Ebene, um die Interaktion zwischen Player und System zu aktivieren. Erfahren Sie, wie die Benutzeroberfläche mit dem Zustands Automat des gesamten Spiels interagiert und wie Sie Ereignishandler für UWP-Spiele erstellen.|
|[Definieren des Hauptobjekts für das Spiel](tutorial--defining-the-main-game-loop.md)|Nun sehen wir uns die Details des Haupt Objekts des Beispiel Spiels an und wie die implementierten Regeln in Interaktionen mit der Spiel Welt übersetzt werden.|
|[Renderingframework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md)|Erfahren Sie, wie Sie die Renderingpipeline zum Anzeigen von Grafiken entwickeln. Einführung in das Rendering.|
|[Renderingframework II: Spiel Rendering](tutorial-game-rendering.md)|Erfahren Sie, wie Sie die Rendering-Pipeline zum Anzeigen von Grafiken zusammenstellen. Spiele Rendering, einrichten und Vorbereiten von Daten.|
|[Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md)|Erfahren Sie, wie Sie einem DirectX UWP-Spiel eine Überlagerung der 2D-Benutzeroberfläche hinzufügen.|
|[Hinzufügen von Steuerelementen](tutorial--adding-controls.md)|Nun sehen wir uns an, wie das Beispiel Spiel die Steuerelemente für das Verschieben von Steuerelementen in einem 3D-Spiel implementiert und wie einfache Steuerelemente für Finger Eingaben, Maus und Spielcontroller entwickelt werden.|
|[Hinzufügen von Sound](tutorial--adding-sound.md)|Entwickeln Sie eine einfache Sound-Engine mithilfe von XAudio2-APIs, um Spiele Musik und Soundeffekte wiederzugeben.|
|[Erweitern des Spielbeispiels](tutorial-resources.md)|Erfahren Sie, wie Sie eine XAML-Überlagerung für ein UWP DirectX-Spiel implementieren.|