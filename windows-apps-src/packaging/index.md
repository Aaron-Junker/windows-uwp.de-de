---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Verpacken von Apps
description: Dieser Abschnitt enthält Artikel oder Links zum Verpacken von UWP (Universelle Windows-Plattform)-Apps.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP, Verpacken
ms.localizationpriority: medium
ms.openlocfilehash: 04736c9ac4de5adf162d32191ff30f7a981d6a6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582867"
---
# <a name="packaging-apps"></a>Verpacken von Apps


## <a name="purpose"></a>Zweck

Dieser Abschnitt enthält Artikel oder Links zum Verpacken von UWP (Universelle Windows-Plattform)-Apps.

| Thema | Beschreibung |
|-------|-------------|
| [Verpacken einer UWP-App mit Visual Studio](packaging-uwp-apps.md) | Sie müssen ein App-Paket erstellen, damit Sie Ihre UWP-App (Universelle Windows-Plattform) vertreiben oder verkaufen können. |
| [Manuelles Verpacken von Apps](manual-packaging-root.md) | Wenn Sie ein App-Paket erstellen und signieren möchten, für die Entwicklung der App aber nicht Visual Studio verwendet haben, müssen Sie die Tools für die manuelle App-Verpackung verwenden. |
| [App-Paketarchitekturen](device-architecture.md) | Hier erfahren Sie mehr über die Prozessorarchitekturen, die beim Erstellen des UWP-App-Pakets verwendet werden sollten. |
| [Installieren von UWP-App-Streaming](streaming-install.md) | Durch die Installation des UWP-App-Streamings (Universelle Windows-Plattform) können Sie angeben, welche Teile der App der Microsoft Store zuerst herunterladen soll. Wenn die wichtigen Dateien der App zuerst heruntergeladen werden, können Benutzer die App starten und mit dieser interagieren, während der Rest noch im Hintergrund heruntergeladen wird. |
| [Optionale Pakete und die Erstellung zugehöriger Sets](optional-packages.md) | Optionale Pakete enthalten Inhalte, die in ein Hauptpaket integriert werden können. Diese sind nützlich für herunterladbare Inhalte (DLC), da große Apps so im Hinblick auf Größenbeschränkungen geteilt werden. Sie sind außerdem praktisch, wenn Sie zusätzliche Inhalte getrennt von der ursprünglichen App ausliefern möchten. |
| [Optionale Pakete mit ausführbarem Code](optional-packages-with-executable-code.md) | Hier erfahren Sie, wie Sie Visual Studio verwenden, um ein optionales Paket mit ausführbarem Code zu erstellen. |
| [Installieren von UWP-Apps mit dem App-Installer](appinstaller-root.md) | Mit dem App-Installer können UWP-Apps durch Doppelklicken auf das App-Paket installiert werden. |
| [Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Die Windows-Anwendungsbereitstellung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit dem Sie eine UWP-App über einen Windows 10-Computer auf beliebigen Windows 10 Mobile-Geräten bereitstellen können. Dieses Tool ermöglicht die Bereitstellung eines APPX-Pakets, wenn das Windows 10 Mobile-Gerät über USB angeschlossen ist oder sich im selben Subnetz befindet, ohne dass Microsoft Visual Studio oder die Projektmappe für diese App erforderlich ist. Dieser Artikel beschreibt, wie UWP-Apps mit diesem Tool installiert werden. |
| [Einrichten automatisierter Builds für UWP-Apps](auto-build-package-uwp-apps.md) | Wenn Sie Ihre App als Teil eines automatisierten Buildprozesses packen möchten, erfahren Sie hier, wie Sie Visual Studio Team Services (VSTS) dazu verwenden können. |
| [Deklarationen von App-Funktionen](app-capability-declarations.md) | Funktionen müssen im [Paketmanifest](https://msdn.microsoft.com/library/windows/apps/BR211474) der UWP-App für den Zugriff auf bestimmte APIs oder Ressourcen deklariert werden, z. B. Bilder, Musik oder Geräte wie die Kamera oder das Mikrofon. |
| [Herunterladen und Installieren von Paketupdates aus dem Store](self-install-package-updates.md) | Ihre UWP-App kann programmgesteuert nach Paketupdates suchen und die Updates installieren. Ihre App kann auch Abfragen für Pakete ausführen, die in Partner Center als obligatorisch gekennzeichnet wurden, und Funktionen deaktivieren, bis das erforderliche Update installiert wurde.  |
