---
title: DirectX-Spielprojektvorlagen
description: Hier erhalten Sie Informationen zu den Vorlagen zum Erstellen eines DirectX-Spiels für die Universelle Windows-Plattform (UWP).
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Directx, Vorlagen
ms.localizationpriority: medium
ms.openlocfilehash: 5eb36b66cc067111e2749ebd51a05994a011ba01
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367474"
---
# <a name="directx-game-project-templates"></a>DirectX-Spielprojektvorlagen



Mit Vorlagen für DirectX und die universelle Windows-Plattform (UWP) können Sie schnell ein Projekt als Ausgangspunkt für Ihr Spiel erstellen.

## <a name="prerequisites"></a>Vorraussetzungen


Gehen Sie wie folgt vor, um das Projekt zu erstellen:

-   [Herunterladen von Microsoft Visual Studio 2015](https://www.visualstudio.com/vs-2015-product-editions). Visual Studio 2015 ist die Tools für die Grafikprogrammierung, z. B. Tools zum Debuggen. Eine Übersicht über DirectX-Grafik- und -Spielefeatures/-tools finden Sie unter [Visual Studio-Tools für die Entwicklung von DirectX-Spielen](set-up-visual-studio-for-game-development.md).

## <a name="choosing-a-template"></a>Auswählen einer Vorlage


Visual Studio 2015 enthält drei DirectX und UWP-Vorlagen:

-   DirectX 11-App (Universelle Windows-App): Mit der Vorlage "DirectX 11-App (Universelle Windows-App)" wird ein UWP-Projekt erstellt, das direkt in einem App-Fenster mit DirectX 11 gerendert wird.
-   DirectX 12-App (Universelle Windows-App): Mit der Vorlage "DirectX 12-App (Universelle Windows-App)" wird ein UWP-Projekt erstellt, das direkt in einem App-Fenster mit DirectX 12 gerendert wird.
-   DirectX 11- und XAML-App (Universelle Windows-App): Mit der Vorlage „DirectX 11- und XAML-App (Universelle Windows-App)“ wird ein UWP-Projekt erstellt, das direkt in einem XAML-Steuerelement mit DirectX 11 gerendert wird. Diese Vorlage verwendet ein [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)-Element und ermöglicht so die Verwendung von XAML-UI-Steuerelementen. Dies kann zwar das Hinzufügen von Benutzeroberflächenelementen vereinfachen, die Verwendung der XAML-Vorlage beeinträchtigt aber unter Umständen die Leistung.

Die Entscheidung für eine Vorlage hängt von der Leistung und von den gewünschten Technologien ab.

## <a name="template-structure"></a>Vorlagenstruktur


Die DirectX-UWP-Vorlagen enthalten die folgenden Dateien:

-   "pch.h" und "pch.cpp": Unterstützung für vorkompilierte Headerdateien.
-   Package.appxmanifest: Die Eigenschaften des Bereitstellungspakets für die App.
-   \*PFX - Zertifikate für die Anwendung.
-   Externe Abhängigkeiten: Links zu externen Dateien, die vom Projekt verwendet werden.
-   \*"Main.h" und \*"Main.cpp" - Methoden zum Verwalten von Anwendungsressourcen, Anwendungszustand zu aktualisieren und den Frame zu rendern.
-   "App.h" und "App.cpp": Haupteinstiegspunkt für die Anwendung. Verbindet die App mit der Windows-Shell und behandelt App-Lebenszyklusereignisse. Diese Dateien werden nur in den Vorlagen „DirectX 11-App (Universelle Windows-App)“ und „DirectX 12-App (Universelle Windows-App)“ angezeigt.
-   "App.Xaml", "App.Xaml.cpp" und "App.Xaml.h": Haupteinstiegspunkt für die Anwendung. Verbindet die App mit der Windows-Shell und behandelt App-Lebenszyklusereignisse. Diese Dateien werden nur in der Vorlage "DirectX 11- und XAML-App (Universelle Windows-App)" angezeigt.
-   "DirectXPage.xaml", "DirectXPage.xaml.cpp" und "DirectXPage.xaml.h": Eine Seite, die ein DirectX-SwapChainPanel hostet. Diese Dateien werden nur in der Vorlage "DirectX 11- und XAML-App (Universelle Windows-App)" angezeigt.
-   Inhalt
    -   Sample3DSceneRenderer.h und Sample3DSceneRenderer.cpp – Ein Beispielrenderer, der eine grundlegende Renderingpipeline instanziiert.
    -   SampleFpsTextRenderer.h und SampleFpsTextRenderer.cpp – Rendert den aktuellen FPS-Wert mit Direct2D und DirectWrite rechts unten auf dem Bildschirm. Diese Dateien werden nur in den Vorlagen „DirectX 11-App (Universelle Windows-App)“ und „DirectX 11- und XAML-App (Universelle Windows-App)“ angezeigt.
    -   SamplePixelShader.hlsl – Ein einfacher Beispiel-Pixelshader.
    -   SampleVertexShader.hlsl – Ein einfacher Beispiel-Vertex-Shader.
    -   ShaderStructures.h – Zum Senden von Daten an den Beispiel-Vertex-Shader verwendete Strukturen.
-   Allgemein
    -   StepTimer.h – Eine Hilfsklasse für das Animations- und Simulationstiming.
    -   DirectXHelper.h – Verschiedene Hilfsfunktionen.
    -   DeviceResources.h und Device Resources.cpp – Stellt eine Schnittstelle für eine Anwendung bereit, die Eigentümer von DeviceResources ist, um benachrichtigt zu werden, wenn das Gerät verloren geht oder erstellt wird.
    -   d3dx12.h – Enthält die D3DX12-Hilfsprogrammbibliothek. Diese Datei ist nur in der DirectX 12-App (Universelle Windows-App) vorhanden.
-   Ressourcen: Logo- und Splashscreen-Bilder, die von der Anwendung verwendet werden.

## <a name="next-steps"></a>Nächste Schritte


Erweitern Sie basierend auf diesen Grundkenntnissen Ihr Wissen im Bereich der Spielentwicklung und speziell die Fähigkeiten für die Entwicklung von Microsoft Store-Spielen.

Wenn Sie ein vorhandenes Spiel portieren, sind die folgenden Themen hilfreich:

-   [Portieren von OpenGL ES 2.0 zu Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Portieren von DirectX 9 zu universellen Windows-Plattform](porting-your-directx-9-game-to-windows-store.md)

Wenn Sie ein neues DirectX-Spiel erstellen, sind die folgenden Themen hilfreich.

-   [Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Entwickeln von Marble Maze, eine universelle Windows-Plattform-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)