---
title: Portieren von Unity-Spielen für Xbox auf die UWP
description: Erfahren Sie, wie Sie ein Unity-Spiel aus einer universelle Windows-Plattform (UWP) Visual Studio-Projekt Mappe in Xbox One erstellen und bereitstellen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: fca3267a-0c0f-4872-8017-90384fb34215
ms.localizationpriority: medium
ms.openlocfilehash: 5b985372c52711dd7b6b4f865ab5164e25c88e0c
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238325"
---
# <a name="bringing-unity-games-to-uwp-on-xbox"></a>Portieren von Unity-Spielen für Xbox auf die UWP


In diesem ausführlichen Lernprogramm wird davon ausgegangen, dass Sie bereits über ein Spiel in Unity verfügen, das zum Erstellen und Bereitstellen bereit ist.

Beachten Sie auch die [Videoversion des Lernprogramms](https://www.youtube.com/watch?v=f0Ptvw7k-CE).

Möchten Sie eine Versionsverwaltung für Ihr Unity-UWP-Projekt verwenden? Siehe [Versionskontrolle für Ihr UWP-Projekt](development-lanes-unity-versioning.md).

## <a name="step-0-ensure-unity-is-installed-correctly"></a>Schritt 0: Sicherstellen, dass Unity richtig installiert wurde

Beim Installieren von Unity müssen folgende Komponenten ausgewählt sein:

![Installationskomponenten von Unity](images/unity-install-components.png)

## <a name="step-1-building-the-uwp-solution"></a>Schritt 1: Erstellen der UWP-Lösung

Öffnen Sie in Ihrem Unity-Spiel Projekt die Fenster mit den **Buildeinstellungen** unter **Datei > Buildeinstellungen**, und wechseln Sie zum Menü Microsoft Store Optionen.

![Fenster mit Build-Einstellungen](images/build-settings.png)

Stellen Sie sicher, dass die **SDK**-Einstellung auf **Universal 10** gesetzt ist, und klicken Sie dann auf die Schaltfläche **Build**. Ein Datei-Explorer-Fenster wird geöffnet, in dem Sie nach einem Zielordner gefragt werden. Erstellen Sie neben dem Verzeichnis **Assets** Ihres Projekts den Ordner **UWP**, und wählen Sie diesen Ordner als Zielordner des Builds aus.

![Build-Zielordner](images/build-destination.png)

Unity hat eine neue Visual Studio-Lösung erstellt, die wir zum Bereitstellen Ihres UWP-Spiels verwenden.

![UWP-VS-Lösung](images/uwp-vs-solution.png)

## <a name="step-2-deploying-your-game"></a>Schritt 2: Bereitstellen des Spiels

Öffnen Sie die neu erstellte Lösung im Ordner **UWP**, und ändern Sie die Zielplattform zu **x64**.

![x64-Build-Plattform](images/x64-build-platform.png)

Nachdem Sie nun über eine Visual Studio-UWP-Lösung für Ihr Spiel verfügen, [befolgen Sie diese Schritte](getting-started.md), um Ihr Spiel erfolgreich auf Ihrer Xbox One-Konsole für den Einzelhandel bereitzustellen!

## <a name="step-3-modify-and-rebuild"></a>Schritt 3: Ändern und Neuerstellen

Wenn etwas anderes als ein Skript geändert wird, muss das Projekt im Editor neu erstellt werden (siehe __Schritt 1__), damit die Änderungen in den UWP-Build Ihres Spiel übernommen werden.

## <a name="versioning-your-uwp-project"></a>Versionsverwaltung für Ihr UWP-Projekt

Es gibt einige häufige Situationen, in denen es erforderlich ist, Teile dieses neu generierten UWP-Verzeichnisses der Versionskontrolle hinzuzufügen. Dies ist beispielsweise der Fall, wenn Sie dem UWP-Projekt eine neue Abhängigkeit (z. B. das Xbox Live SDK) hinzufügen.  Wir betrachten dieses Beispiel unter [Versionskontrolle für Ihr UWP-Projekt](development-lanes-unity-versioning.md) ausführlich.

## <a name="see-also"></a>Weitere Informationen
- [Portieren vorhandener Spiele zu Xbox](development-lanes-landing.md)
- [UWP auf Xbox One](index.md)
