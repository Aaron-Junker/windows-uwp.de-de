---
title: Portieren von Unity-Spielen für Xbox auf die UWP
description: UWP-Entwicklung für Unity-Spiele auf Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: fca3267a-0c0f-4872-8017-90384fb34215
ms.localizationpriority: medium
ms.openlocfilehash: 64a686aea24d23b5e999780eaa0eda661af3637f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611685"
---
# <a name="bringing-unity-games-to-uwp-on-xbox"></a>Portieren von Unity-Spielen für Xbox auf die UWP


In diesem ausführlichen Lernprogramm wird davon ausgegangen, dass Sie bereits über ein Spiel in Unity verfügen, das zum Erstellen und Bereitstellen bereit ist.

Beachten Sie auch die [Videoversion des Lernprogramms](https://www.youtube.com/watch?v=f0Ptvw7k-CE).

Möchten Sie eine Versionsverwaltung für Ihr Unity-UWP-Projekt verwenden? Siehe [Versionskontrolle für Ihr UWP-Projekt](development-lanes-unity-versioning.md).

## <a name="step-0-ensure-unity-is-installed-correctly"></a>Dies ist Schritt 0: Stellen Sie sicher, dass Unity ordnungsgemäß installiert ist

Beim Installieren von Unity müssen folgende Komponenten ausgewählt sein:

![Installationskomponenten von Unity](images/unity-install-components.png)

## <a name="step-1-building-the-uwp-solution"></a>Schritt 1: Die UWP-Lösung erstellen

Öffnen Sie in Ihrem Unity-Spielprojekt das Fenster mit den **Build-Einstellungen** unter **Datei -> Build-Einstellungen**, und wechseln Sie zum Menü mit den Microsoft Store-Optionen.

![Fenster mit Build-Einstellungen](images/build-settings.png)

Stellen Sie sicher, dass die **SDK**-Einstellung auf **Universal 10** gesetzt ist, und klicken Sie dann auf die Schaltfläche **Build**. Ein Datei-Explorer-Fenster wird geöffnet, in dem Sie nach einem Zielordner gefragt werden. Erstellen Sie neben dem Verzeichnis **Assets** Ihres Projekts den Ordner **UWP**, und wählen Sie diesen Ordner als Zielordner des Builds aus.

![Build-Zielordner](images/build-destination.png)

Unity hat eine neue Visual Studio-Lösung erstellt, die wir zum Bereitstellen Ihres UWP-Spiels verwenden.

![UWP-VS-Lösung](images/uwp-vs-solution.png)

## <a name="step-2-deploying-your-game"></a>Schritt 2: Ihr Spiel bereitstellen

Öffnen Sie die neu erstellte Lösung im Ordner **UWP**, und ändern Sie die Zielplattform zu **x64**.

![x64-Build-Plattform](images/x64-build-platform.png)

Nachdem Sie nun über eine Visual Studio-UWP-Lösung für Ihr Spiel verfügen, [befolgen Sie diese Schritte](getting-started.md), um Ihr Spiel erfolgreich auf Ihrer Xbox One-Konsole für den Einzelhandel bereitzustellen!

## <a name="step-3-modify-and-rebuild"></a>Schritt 3: Ändern und neu erstellen

Wenn etwas anderes als ein Skript geändert wird, muss das Projekt im Editor neu erstellt werden (siehe __Schritt 1__), damit die Änderungen in den UWP-Build Ihres Spiel übernommen werden.

## <a name="versioning-your-uwp-project"></a>Versionsverwaltung für Ihr UWP-Projekt

In bestimmten Situationen müssen Teile dieses neu generierten UWP-Verzeichnisses der Versionskontrolle hinzugefügt werden. Dies ist beispielsweise der Fall, wenn Sie dem UWP-Projekt eine neue Abhängigkeit (z. B. das Xbox Live SDK) hinzufügen.  Wir betrachten dieses Beispiel unter [Versionskontrolle für Ihr UWP-Projekt](development-lanes-unity-versioning.md) ausführlich.

## <a name="see-also"></a>Siehe auch
- [Vorhandene Spiele anbieten für Xbox](development-lanes-landing.md)
- [UWP auf Xbox One](index.md)
