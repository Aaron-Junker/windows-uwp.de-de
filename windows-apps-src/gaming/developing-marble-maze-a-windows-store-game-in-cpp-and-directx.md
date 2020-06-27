---
title: Entwickeln von *Marble Maze* &mdash; a universelle Windows-Plattform (UWP)-Spiel, das mit C++ for DirectX erstellt wurde
description: In diesem Abschnitt der Dokumentation wird die Verwendung von DirectX und C++ zum Erstellen eines 3D-universelle Windows-Plattform (UWP)-Spiels beschrieben.
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Sample, DirectX, 3D-
ms.localizationpriority: medium
ms.openlocfilehash: d2c0c630090c178a54a0452ab3cc430ffee4a176
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409499"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>Entwickeln von *Marble Maze* &mdash; a universelle Windows-Plattform (UWP)-Spiel, das mit C++ for DirectX erstellt wurde

In diesem Thema wird die Verwendung von DirectX und C++ zum Erstellen eines 3D-universelle Windows-Plattform (UWP)-Spiels beschrieben. Das Spiel, das als *Marble Maze*bezeichnet wird, umfasst mehrere Formfaktoren, wie z. b. Tablets, herkömmliche Desktop-PCs und Laptop-PCs.

> [!NOTE]
> Informationen zum Herunterladen des *Marble Maze* -Quellcodes finden Sie im [Beispiel auf GitHub](https://github.com/microsoft/Windows-appsample-marble-maze).

> [!IMPORTANT]
> In *Marble Maze* werden Entwurfsmuster veranschaulicht, die als bewährte Methoden für das Erstellen von UWP-spielen angesehen werden. Viele der Implementierungsdetails können Sie an Ihre eigenen Vorgehensweisen und die konkreten Anforderungen des von Ihnen entwickelten Spiels anpassen. Wenn es Ihren Anforderungen besser entgegenkommt, können Sie natürlich auch andere Verfahren oder Bibliotheken verwenden. (Stellen Sie jedoch immer sicher, dass Ihr Code das [zertifizierungskit für Windows-apps](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)übergibt.) Wenn wir eine Implementierung für eine erfolgreiche Spieleentwicklung in Erwägung gezogen haben, wird diese in dieser Dokumentation hervorgehoben.

## <a name="introducing-marble-maze"></a>Einführung in *Marble Maze*

Wir haben uns für das *Marble Maze* entschieden, da es relativ einfach ist, aber dennoch die Breite der Features veranschaulicht, die in den meisten spielen gefunden werden. Die Verwendung von Grafiken kann ebenso erläutert werden wie die Verarbeitung von Eingaben und Audiodaten. Außerdem kann die Spielmechanik gezeigt werden, etwa Regeln und Ziele.

*Marble Maze* ähnelt dem Table-Top-Labyrinth-Spiel, das in der Regel aus einem Feld erstellt wird, das Löcher und ein Stahl-oder Glas Marmor enthält. Das Ziel von *Marble Maze* ist das gleiche wie die Tabelle-Top-Version: kippen Sie den Maze so wenig Zeit wie möglich, um den Marmor von Anfang bis zum Ende des Maze zu leiten, ohne den Marmor in eine der Lücken zu versetzen. *Marble Maze* fügt das Konzept von Prüfpunkten hinzu. Wenn die Murmel in ein Loch fällt, beginnt das Spiel am letzten Kontrollpunkt neu, den die Murmel passiert hat.

*Marble Maze* bietet mehrere Möglichkeiten für einen Benutzer, mit dem Spiel Board zu interagieren. Wenn Sie über ein Gerät verfügen, das Toucheingaben oder einen Beschleunigungsmesser unterstützt, können Sie das Spielbrett mit diesen Geräten bewegen. Sie können auch einen Xbox One-Controller oder eine Maus verwenden, um Game Play zu steuern.

![Screenshot des Marble Maze-Spiels](images/marblemaze-2.png)

## <a name="prerequisites"></a>Voraussetzungen

-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   Kenntnisse in der C++-Programmierung
-   Erfahrung mit DirectX und der Terminologie von DirectX
-   Grundkenntnisse in COM

## <a name="who-should-read-this"></a>Zielgruppe dieses Artikels

Wenn Sie an der Erstellung von 3D-spielen oder anderen Grafik intensiven Anwendungen für Windows 10 interessiert sind, können Sie dies tun. Wir hoffen, dass Ihnen die in dieser Dokumentation skizzierten Prinzipien und praktischen Vorgehensweisen dabei helfen, eigene UWP-Spiele zu erstellen. Den größten Nutzen aus dieser Dokumentation werden Sie ziehen, wenn Sie bereits über Erfahrungen mit C++ und DirectX verfügen oder sich sehr dafür interessieren. Wenn Sie nicht über die Verwendung von DirectX verfügen, können Sie dennoch von Vorteil sein, wenn Sie mit ähnlichen 3D-Grafik Programmierungs Umgebungen arbeiten.

Das Dokument Exemplarische Vorgehensweise [: Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-uwp-directx-game.md) beschreibt ein weiteres Beispiel, das ein einfaches 3D-Erstellungs Spiel mithilfe von DirectX und C++ implementiert.

## <a name="what-this-documentation-covers"></a>In dieser Dokumentation behandelte Themen

In dieser Dokumentation wird Folgendes beschrieben:

-   Erstellen eines UWP-Spiels mit der Windows-Runtime-API und DirectX
-   Verwenden Sie [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) und [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) , um mit visuellen Inhalten wie Modellen, Texturen, Scheitel Punkten und Pixel-Shadern und 2D-Überlagerungen zu arbeiten.
-   Integrieren Sie Eingabe Mechanismen wie Fingerabdruck, Beschleunigungsmesser und den Xbox One-Controller.
-   Integrieren von Musik und Soundeffekten mit [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal)

## <a name="what-this-documentation-does-not-cover"></a>In dieser Dokumentation nicht behandelte Themen

Folgende Aspekte der Spieleentwicklung werden in dieser Dokumentation nicht behandelt. Im Anschluss an diese Aspekte folgen zusätzliche Ressourcen, in denen sie behandelt werden.

-   Entwurfs Prinzipien für 3D-Spiele.
-   Grundlagen der C++- und DirectX-Programmierung
-   Entwerfen von Ressourcen wie Texturen, Modellen oder Audio
-   Beheben von Verhaltens- oder Leistungsproblemen in Spielen
-   Vorbereiten von Spielen für andere Teile der Welt
-   Zertifizieren und Veröffentlichen des Spiels für die Microsoft Store.

*Marble Maze* verwendet auch die [directxmath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) -Bibliothek für die Arbeit mit 3D-Geometrie und die Durchführung von Physik Berechnungen, wie z. b. Kollisionen. DirectXMath wird in diesem Abschnitt nicht eingehend beschrieben. Ausführliche Informationen zur Verwendung von directxmath in *Marble Maze* finden Sie im Quellcode.

Obwohl *Marble Maze* viele wiederverwendbare Komponenten bereitstellt, ist es kein ganzes Spiel Entwicklungs Framework. Wenn wir eine *Marble Maze* -Komponente in Ihrem Spiel als wiederverwendbar betrachtet haben, wird Sie in der Dokumentation hervorgehoben.

## <a name="next-steps"></a>Nächste Schritte

Wir empfehlen Ihnen, mit den [Grundlagen von Marble Maze-Beispielen](marble-maze-sample-fundamentals.md) zu beginnen, um mehr über die *Marmor Maze* -Struktur und einige der Codierungs-und Stilrichtlinien zu erfahren, auf die der Quellcode für *Marble Maze* folgt Die folgende Tabelle skizziert die Dokumente in diesem Abschnitt, sodass Sie einen genaueren Überblick über diese gewinnen.

## <a name="in-this-section"></a>In diesem Abschnitt

| Titel                                                                                                                    | Beschreibung                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Grundlagen am Beispiel von Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Bietet einen Überblick über die Struktur des Spiels und einige Richtlinien zu Codierung und Stil im Marble Maze-Quellcode.                                                                                                                                 |
| [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md)                                               | Beschreibt, wie der *Marble Maze* -Anwendungscode strukturiert ist und wie sich die Struktur einer DirectX UWP-APP von der Struktur einer herkömmlichen Desktop Anwendung unterscheidet.                                                                                    |
| [Hinzufügen von visuellem Inhalt zum Marble Maze-Beispiel](adding-visual-content-to-the-marble-maze-sample.md)                   | Beschreibt einige der zentralen praktischen Verfahren, die bei der Arbeit mit Direct3D und Direct2D zu beachten sind. Außerdem wird beschrieben, wie *Marble Maze* diese Verfahren für visuelle Inhalte anwendet.                                                                           |
| [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Beschreibt, wie *Marble Maze* mit Accelerometer-, Touchscreen-und Xbox One-Controller Eingaben funktioniert, damit Benutzer in Menüs navigieren und mit der Spiel Platine interagieren können. Beschreibt außerdem einige bewährte Methoden, die bei der Arbeit mit Eingaben beachtet werden sollten. |
| [Hinzufügen von Audio zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md)                                     | Beschreibt, wie *Marble Maze* mit Audio arbeitet, um der Spiel Darstellung Musik und Soundeffekte hinzuzufügen.                                                                                                                                                  |
