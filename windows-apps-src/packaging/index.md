---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Packen von Apps
description: Dieser Abschnitt enthält Artikel verweist auf Artikel zum Packen von UWP-Apps (Universal Windows Platform, Universelle Windows-Plattform).
ms.date: 07/22/2019
ms.topic: article
keywords: Windows 10, UWP, packen
ms.localizationpriority: medium
ms.openlocfilehash: bb772007cd5c4391634f9df6ba0b6d2037a7b5de
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089476"
---
# <a name="packaging-apps"></a>Packen von Apps

Dieser Abschnitt enthält Artikel oder verweist auf Artikel zum Packen von UWP-Apps (Universal Windows Platform, Universelle Windows-Plattform) in MSIX- und AppX-App-Paketen für Bereitstellung und Installation. Einige dieser Links verweisen auf relevante Artikel in der [MSIX-Dokumentation](https://docs.microsoft.com/windows/msix/).

> [!NOTE]
> Das ursprüngliche App-Paketformat für UWP-Apps in Windows 10 war AppX. Ab Windows 10, Version 1809 wurde dieses Paketformat in MSIX umbenannt und erweitert, um alle Arten von Windows-Apps zu unterstützen, darunter beispielsweise .NET- und C++/Win32-Desktop-Apps. Die Unterstützung für MSIX wird auch auf frühere Windows-Versionen ausgedehnt. Weitere Informationen finden Sie in der [MSIX-Dokumentation ](https://docs.microsoft.com/windows/msix/).

| Thema | Beschreibung |
|-------|-------------|
| [Packen einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps) | Sie müssen ein App-Paket erstellen, damit Sie Ihre UWP-App (Universelle Windows-Plattform) vertreiben oder verkaufen können. |
| [Manuelles Packen von Apps](/windows/msix/package/manual-packaging-root) | Wenn Sie ein App-Paket erstellen und signieren möchten, für die Entwicklung der App aber nicht Visual Studio verwendet haben, müssen Sie die Tools zum manuellen Packen von Apps verwenden. |
| [App-Paketarchitekturen](/windows/msix/package/device-architecture) | Hier erfahren Sie mehr über die Prozessorarchitekturen, die beim Erstellen des App-Pakets verwendet werden müssen. |
| [Installieren von UWP-App-Streaming](/windows/msix/package/streaming-install) | Durch die Installation des App-Streamings können Sie angeben, welche Teile der App der Microsoft Store zuerst herunterladen soll. Wenn die wichtigen Dateien der App zuerst heruntergeladen werden, können Benutzer die App starten und mit dieser interagieren, während der Rest noch im Hintergrund heruntergeladen wird. |
| [Optionale Pakete und die Erstellung zugehöriger Sets](/windows/msix/package/optional-packages) | Optionale Pakete enthalten Inhalte, die in ein Hauptpaket integriert werden können. Diese sind nützlich für herunterladbare Inhalte (DLC), da große Apps so im Hinblick auf Größenbeschränkungen geteilt werden. Sie sind außerdem praktisch, wenn Sie zusätzliche Inhalte getrennt von der ursprünglichen App ausliefern möchten. |
| [Optionale Pakete mit ausführbarem Code](/windows/msix/package/optional-packages-with-executable-code) | Hier erfahren Sie, wie Sie Visual Studio verwenden, um ein optionales Paket mit ausführbarem Code zu erstellen. |
| [Installieren von Windows 10-Apps mit dem App-Installer](/windows/msix/app-installer/app-installer-root) | Mit dem App-Installer können Windows 10-Apps durch Doppelklicken auf das App-Paket installiert werden. |
| [Installieren von Apps mit dem Tool „WinAppDeployCmd.exe“](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Die Windows-Anwendungsbereitstellung (WinAppDeployCmd.exe) ist ein Befehlszeilentool, mit dem Sie eine UWP-App über einen Windows 10-Computer auf beliebigen Windows 10 Mobile-Geräten bereitstellen können. Dieses Tool ermöglicht die Bereitstellung eines App-Pakets, wenn das Windows 10 Mobile-Gerät über USB angeschlossen ist oder sich im selben Subnetz befindet, ohne dass Microsoft Visual Studio oder die Projektmappe für diese App erforderlich ist. Dieser Artikel beschreibt, wie UWP-Apps mit diesem Tool installiert werden. |
| [Einrichten automatisierter Builds für UWP-Apps](auto-build-package-uwp-apps.md) | Wenn Sie Ihre App als Teil eines automatisierten Buildprozesses packen möchten, erfahren Sie hier, wie Sie Visual Studio Team Services (VSTS) dazu verwenden können. |
| [Deklarationen von App-Funktionen](app-capability-declarations.md) | Im [Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) der App müssen Funktionen für den Zugriff auf bestimmte APIs oder Ressourcen deklariert werden, z.B. Bilder, Musik oder Geräte wie Kamera oder Mikrofon. |
| [Herunterladen und Installieren von Paketupdates aus dem Store](self-install-package-updates.md) | Ihre UWP-App kann programmgesteuert nach Paketupdates suchen und die Updates installieren. Ihre App kann auch Abfragen für Pakete ausführen, die in Partner Center als obligatorisch gekennzeichnet wurden, und Funktionen deaktivieren, bis das erforderliche Update installiert wurde.  |
