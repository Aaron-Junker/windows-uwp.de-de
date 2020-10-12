---
title: Problembehandlung für die Programmkompatibilität auf ARM
description: Leitfaden zum Anpassen von Kompatibilitäts Einstellungen, wenn Ihre APP auf Arm nicht ordnungsgemäß funktioniert
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, immer verbunden, Kompatibilitätsproblem Behandlung, Windows auf Arm
ms.localizationpriority: medium
ms.openlocfilehash: 24ae6e7c12fde1dfbb9e5395b3fa3aa4901adb3d
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933021"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>Problembehandlung für die Programmkompatibilität auf ARM
Die Emulation zur Unterstützung von x86-Apps ist ein neues Feature, das für Windows 10 auf ARM64 erstellt wurde. Manchmal führt die Emulation Optimierungen aus, die nicht zu den besten Ergebnissen führen. Mit der Programm Kompatibilitätsproblem Behandlung können Sie die Emulations Einstellungen für Ihre x86-App umschalten, die Standard Optimierungen verringern und die Kompatibilität möglicherweise erhöhen.

## <a name="start-the-program-compatibility-troubleshooter"></a>Starten der Programm Kompatibilitätsproblem Behandlung
Sie starten die [Programm Kompatibilitäts](https://support.microsoft.com/help/15078/windows-make-older-programs-compatible) Problembehandlung manuell auf allen Windows 10-PCs: Klicken Sie mit der rechten Maustaste auf eine ausführbare Datei (exe-Datei), und wählen Sie **Kompatibilitätsprobleme beheben**. Dieser Bildschirm wird angezeigt.

![Screenshot der Kompatibilitäts Optionen für die Problembehandlung.](images/arm/Capture4.png)

Wenn Sie auf **Programm** zur Problembehandlung klicken, werden Ihnen die folgenden Optionen angezeigt.

![Screenshot der möglichen Probleme:](images/arm/Capture5.png)

Alle Optionen aktivieren die Einstellungen, die auf allen Windows 10-Desktop-PCs anwendbar sind und angewendet werden. Darüber hinaus wenden die ersten, zweiten und vierten Optionen die Emulations Einstellungen [Anwendungscache deaktivieren](#disable-app-cache) und [Hybrid Ausführungs Modus deaktivieren](#disable-hybrid-exec-mode) an.

## <a name="toggling-emulation-settings"></a>Umschalten von Emulations Einstellungen
> [!WARNING]
> Das Ändern von Emulations Einstellungen kann dazu führen, dass Ihre Anwendung unerwartet abstürzt oder überhaupt nicht gestartet wird.

Sie können Emulations Einstellungen umschalten, indem Sie mit der rechten Maustaste auf die ausführbare Datei klicken und **Eigenschaften**auswählen.

Auf Arm ist ein Abschnitt mit dem Namen **Windows 10 auf Arm** auf der Registerkarte **Kompatibilität** verfügbar. Klicken Sie auf **Emulations Einstellungen ändern** , um wie hier ein zweites Fenster zu starten.

![Screenshot: Ändern von Emulations Einstellungen](images/arm/Capture.png)

Dieses Fenster bietet zwei Möglichkeiten zum Ändern der Emulations Einstellungen. Sie können eine vordefinierte Gruppe von Emulations Einstellungen auswählen, oder Sie können auf die Option **Erweiterte Einstellungen verwenden** klicken, um die Auswahl einzelner Einstellungen zu aktivieren.

Die gruppierten Emulations Einstellungen reduzieren Leistungsoptimierungen zugunsten der Qualität. Unten sind einige gruppierte Einstellungen aufgeführt, die Sie auswählen können.

![Ändern der Emulations Einstellungen screenshot2](images/arm/Capture2.png)

Wählen Sie **Erweiterte Einstellungen verwenden** aus, um einzelne Einstellungen auszuwählen, wie in dieser Tabelle beschrieben.

| Emulations Einstellung | Ergebnis |
| ----------------- | ----------- |
| <p id="disable-app-cache">Anwendungscache deaktivieren</p> | Das Betriebssystem speichert kompilierte Code Blöcke zwischen, um den Emulations Aufwand bei nachfolgenden Ausführungen zu verringern. Diese Einstellung erfordert, dass der Emulator den gesamten app-Code zur Laufzeit neu kompiliert. |
| <p id="disable-hybrid-exec-mode">Hybrid Ausführungs Modus deaktivieren</p> | Kompilierte hybride portable ausführbare Dateien (CHPE), Binärdateien sind x86-kompatible Binärdateien, die systemeigenen ARM64-Code enthalten, um die Leistung zu verbessern, die jedoch möglicherweise nicht mit einigen Apps kompatibel sind Diese Einstellung erzwingt die Verwendung von reinen x86-Binärdateien. |
| Strikte selbst ändernde Code Unterstützung | Aktivieren Sie diese Option, um sicherzustellen, dass der selbst ändernde Code in der Emulation korrekt unterstützt wird. Die gängigsten selbst ändernden Code Szenarien werden durch das Standardverhalten des Emulators abgedeckt. Wenn Sie diese Option aktivieren, wird die Leistung des selbst veränderlichen Codes bei der Ausführung erheblich reduziert. |
| Deaktivieren der rwx-Seiten Leistungsoptimierung | Diese Optimierung verbessert die Leistung von Code auf lesbaren, beschreibbaren und ausführbaren Seiten (rwx), kann jedoch mit einigen apps nicht kompatibel sein. |

Sie können auch multikerneinstellungen auswählen, wie hier gezeigt.

![Screenshot mit Multi-Core-Einstellungen](images/arm/Capture3.png)

Mit diesen Einstellungen wird die Anzahl der Speicherbarrieren geändert, die zum Synchronisieren von Speicherzugriffen zwischen Kernen in apps während der Emulation verwendet werden. **Fast** ist der Standardmodus, aber mit den **strengen** und **sehr strengen** Optionen wird die Anzahl der Barrieren erhöht. Dadurch wird die APP verlangsamt, das Risiko von App-Fehlern wird jedoch verringert. Die **Single Core-** Option entfernt alle Barrieren, aber zwingt, dass alle App-Threads auf einem einzelnen Kern ausgeführt werden.

Wenn das Problem durch Ändern einer bestimmten Einstellung behoben wird, senden Sie eine e-Mail *woafeedback@microsoft.com* mit Details, damit wir Ihr Feedback einbeziehen können.