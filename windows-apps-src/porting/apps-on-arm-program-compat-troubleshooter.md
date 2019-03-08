---
title: Problembehandlung für die Programmkompatibilität auf ARM
description: Anleitung zum Anpassen der Kompatibilitätseinstellungen, wenn Ihre App auf ARM nicht korrekt funktioniert
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, problembehandlung für die programmkompatibilität, windows auf ARM
ms.localizationpriority: medium
ms.openlocfilehash: 763b00a5790274d81b6daa2838ef926936e458db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647615"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>Problembehandlung für die Programmkompatibilität auf ARM
Emulation zur Unterstützung von x86-Anwendungen ist ein neues Feature, das für Windows 10 auf ARM64 erstellt wurde. Manchmal führt die Emulation Optimierungen durch, die nicht zu den besten Ergebnissen führen. Sie können die Problembehandlung für die Programmkompatibilität verwenden, um die Emulationseinstellungen für Ihre x86-App umzuschalten, wodurch die Standardoptimierungen reduziert und die Kompatibilität möglicherweise erhöht wird.

## <a name="start-the-program-compatibility-troubleshooter"></a>Starten der Problembehandlung für die Programmkompatibilität
Die [Problembehandlung für die Programmkompatibilität](https://support.microsoft.com/en-us/help/15078/windows-make-older-programs-compatible) wird manuell auf allen Windows 10 PCs auf die gleiche Weise gestartet: Klicken Sie mit der rechten Maustaste auf eine ausführbare Datei (.exe) und wählen Sie **Behebung von Kompatibilitätsproblemen** aus. Der folgende Bildschirm wird angezeigt.

![Screenshot der Option „Behebung von Kompatibilitätsproblemen”](images/arm/Capture4.png)

Wenn Sie auf **Programm zur Problembehandlung** klicken, werden die folgenden Optionen angezeigt.

![Screenshot der Option „Behebung von Kompatibilitätsproblemen”](images/arm/Capture5.png)

Alle Optionen aktivieren die Einstellungen, die für alle Windows 10-Desktop-PCs gelten und angewendet werden. Darüber hinaus verwendet die erste, zweite und vierte Option die Emulationseinstellungen [Deaktivieren der Anwendungscache](#disable-app-cache) und [Deaktivieren des Hybrid-Ausführungsmodus](#disable-hybrid-exec-mode).

## <a name="toggling-emulation-settings"></a>Umschalten der Emulationseinstellungen
> [!WARNING]
> Das Ändern der Emulationseinstellungen führt möglicherweise dazu, dass Ihre Anwendung unerwartet abstürzt oder überhaupt nicht gestartet wird.

Sie können die Emulationseinstellungen umschalten, indem Sie mit der rechten Maustaste auf die ausführbare Datei klicken und **Eigenschaften** auswählen.

Auf ARM, ein Abschnitt mit dem Titel **Windows 10 auf ARM** stehen in der **Kompatibilität** Registerkarte. Klicken Sie auf **ändern emulationseinstellungen** um ein zweites Fenster wie hier zu starten.

![Screenshot von „Ändern der Emulationseinstellungen”](images/arm/Capture.png)

Dieses Fenster bietet zwei Möglichkeiten zum Ändern der Emulationseinstellungen. Sie können eine vordefinierte Gruppe von Emulationseinstellungen auswählen, oder Sie können auf die Option **Erweiterte Einstellungen verwenden** klicken, um die einzelnen Einstellungen auszuwählen.

Die gruppierten Emulationseinstellungen reduzieren Leistungsoptimierungen zugunsten der Qualität. Im Folgenden sind einige gruppierten Einstellungen aufgelistet, die Sie auswählen können.

![Screenshot 2 von „Ändern der Emulationseinstellungen”](images/arm/Capture2.png)

Wählen Sie **Erweiterte Einstellungen verwenden** aus, um individuelle Einstellungen auszuwählen, wie in dieser Tabelle beschrieben.

| Emulationseinstellung | Ergebnis |
| ----------------- | ----------- |
| <p id="disable-app-cache">Deaktivieren der Anwendungscache</p> | Vom Betriebssystem werden kompilierte Codeblöcke zwischengespeichert, um den Emulationsaufwand bei nachfolgenden Ausführungen zu reduzieren. Diese Einstellung erfordert, dass der Emulator den gesamten App-Code zur Laufzeit neu kompiliert. |
| <p id="disable-hybrid-exec-mode">Deaktivieren des Hybrid-Ausführungsmodus</p> | Kompilierte hybride portierbare ausführbare (CHPE) Binärdateien sind x86-kompatible Binärdateien, die nativen ARM64-Code enthalten, um die Leistung zu verbessern, aber mit einigen Apps möglicherweise nicht kompatibel sind. Diese Einstellung erzwingt die Verwendung von nur-x86-Binärdateien. |
| Strenge selbstmodifizierende Codeunterstützung | Aktivieren Sie diese Option, um sicherzustellen, dass jeder selbstsmodifizierende Code bei der Emulation korrekt unterstützt wird. Die häufigsten selbstmodifizierenden Codeszenarien werden vom Standardverhalten des Emulators abgedeckt. Durch das Aktivieren dieser Option wird die Leistung des selbstmodifizierenden Codes während der Ausführung erheblich reduziert. |
| Deaktivieren der RWX-Seitenleistungsoptimierung | Diese Optimierung verbessert die Leistung des Codes auf lesbaren, schreibbaren und ausführbaren (RWX)-Seiten, ist jedoch mit einigen Apps möglicherweise nicht kompatibel. |

Sie können auch Multi-Core-Einstellungen auswählen, wie hier gezeigt.

![Screenshot von „Multi-Core-Einstellungen”](images/arm/Capture3.png)

Diese Einstellungen ändern die Anzahl der Speicherbarrieren, die zum Synchronisieren der Speicherzugriffe zwischen Kernen in Apps während der Emulation verwendet werden. Der Standardmodus ist **schnell**, jedoch steigern die Optionen **streng** und **sehr streng** die Anzahl der Barrieren. Dies verlangsamt die App, reduziert aber das Risiko von App-Fehlern. Die Option **single-core** beseitigt alle Barrieren, erzwingt jedoch die Ausführung aller App-Threads auf einem einzigen Kern.

Wenn das Ändern einer bestimmten Einstellung Ihr Problem behebt, senden Sie eine E-Mail mit Details an *woafeedback@microsoft.com*, damit wir Ihr Feedback einbinden können.