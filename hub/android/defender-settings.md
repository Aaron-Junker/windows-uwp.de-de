---
title: Leistungsgeschwindigkeit durch Aktualisieren von Defender-Einstellungen verbessern
description: Eine Anleitung zur Verbesserung der Gesamt Leistungsgeschwindigkeit und Buildzeiten durch Aktualisieren von Windows Defender-Einstellungen, um die Überprüfung der angegebenen Dateitypen auszuschließen.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Windows Defender, Einstellungen, Konfiguration, Ausschlüsse,% User Profile%, devenv.exe, Leistung, Geschwindigkeit, Build, gradle
ms.date: 04/28/2020
ms.openlocfilehash: e762e595bf2b8aa9b8420d41f68d2890e7d5dfe0
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823204"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>Aktualisieren der Windows Defender-Einstellungen zur Verbesserung der Leistung

In dieser Anleitung wird erläutert, wie Sie die Ausschlüsse in Ihren Windows Defender-Sicherheitseinstellungen einrichten, um die Buildzeiten und die Gesamt Leistungsgeschwindigkeit Ihres Windows-Computers zu verbessern.

## <a name="windows-defender-overview"></a>Übersicht über Windows Defender

In Windows 10, Version 1703 und höher, ist die [Windows Defender Antivirus](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus) -App Teil der Windows-Sicherheit. Windows Defender zielt darauf ab, den PC mit integrierter Echtzeitschutz vor Viren, Ransomware, Spyware und anderen Sicherheitsbedrohungen sicher zu halten.

Der Echtzeitschutz von Windows Defender verlangsamt **jedoch** auch den Dateisystem Zugriff und die Buildgeschwindigkeit bei der Entwicklung von Android-Apps erheblich.

Während des Android-Buildprozesses werden viele Dateien auf Ihrem Computer erstellt. Wenn die antivirenüberprüfung in Echtzeit aktiviert ist, wird der Buildprozess jedes Mal angehalten, wenn eine neue Datei erstellt wird, während das Antivirenprogramm diese Datei scannt.

Glücklicherweise verfügt Windows Defender über die Möglichkeit, Dateien, Projekt Verzeichnisse oder Dateitypen, von denen Sie wissen, dass Sie sicher sind, aus dem Antivirenscan Prozess auszuschließen.

> [!WARNING]
> Um sicherzustellen, dass der Computer vor Schadsoftware geschützt ist, sollten Sie die Echtzeitüberprüfung oder Ihre Windows Defender Antivirus-Software nicht vollständig deaktivieren.

## <a name="add-exclusions-to-windows-defender"></a>Hinzufügen von Ausschlüssen zu Windows Defender

Um die Android-Buildgeschwindigkeit zu verbessern, fügen Sie Ausschlüsse in der [Windows Defender-Security Center](windowsdefender://) hinzu:

1. Windows-Menü **Start** Schaltfläche auswählen
2. **Windows-Sicherheit** eingeben
3. Auswählen von **Viren-und Bedrohungsschutz**
4. Wählen Sie unter **Viren & Bedrohungsschutz Einstellungen** die Option **Einstellungen verwalten** .
5. Führen Sie einen **Bildlauf zur Überschrift aus** , und klicken Sie auf **Ausschlüsse**
6. Wählen Sie **+ Ausschluss hinzufügen** aus. Sie müssen dann auswählen, ob der Ausschluss, den Sie hinzufügen möchten, eine **Datei**, ein **Ordner**, ein **Dateityp** oder ein **Prozess** ist.

![Bildschirm Abbildung von Windows Defender Add-Ausschluss](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>Empfohlene Ausschlüsse

In der folgenden Liste wird der Standard Speicherort jedes Android Studio Verzeichnisses angezeigt, das empfohlen wird, als Ausschluss von Windows Defender Echt Zeit Scans hinzuzufügen:

- Gradle-Cache: `%USERPROFILE%\.gradle`
- Android Studio Projekte: `%USERPROFILE%\AndroidStudioProjects`
- Android SDK: `%USERPROFILE%\AppData\Local\Android\SDK`
- Systemdateien Android Studio: `%USERPROFILE%\.AndroidStudio<version>\system`

Diese Verzeichnis Speicherorte gelten möglicherweise nicht für Ihr Projekt, wenn Sie die Standard Speicherorte nicht verwendet haben, die von Android Studio festgelegt wurden, oder wenn Sie ein Projekt von GitHub heruntergeladen haben (z.b.). Fügen Sie ggf. einen Ausschluss zum Verzeichnis Ihres aktuellen Android-Entwicklungsprojekts hinzu.

Weitere Ausnahmen, die Sie berücksichtigen sollten, sind u. a.:

- Visual Studio-Entwicklungsumgebung: `devenv.exe`
- Visual Studio-Buildprozess: `msbuild.exe`
- Jetbrains-Verzeichnis: `%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

Weitere Informationen zum Hinzufügen von Antiviren-Überprüfungs Ausschlüssen, einschließlich Informationen zum Anpassen von Verzeichnis Standorten für Gruppenrichtlinie gesteuerte Umgebungen, finden Sie im Abschnitt " [Antivirus Impact](https://developer.android.com/studio/intro/studio-config#antivirus-impact) " in der Android Studio-Dokumentation.

> [!Note]
> Daniel kloodle hat ein GitHub-Repository mit empfohlenen Skripts eingerichtet, um [Windows Defender-Ausschlüsse für Visual Studio 2017](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10)hinzuzufügen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Entwickeln Sie Dual-Screen-Apps für Android, und erhalten Sie das Surface Duo-Geräte-SDK](/dual-screen/android/)

- [Windows Defender-Ausschlüsse zum Verbessern der Leistung hinzufügen](./defender-settings.md)

- [Virtualisierungsunterstützung zur Verbesserung der emulatorleistung aktivieren](./emulator.md#enable-virtualization-support)