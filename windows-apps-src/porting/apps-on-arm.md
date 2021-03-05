---
title: Windows 10 auf ARM
description: Dieser Artikel bietet einen Überblick über die Art und Weise, wie Umgebungen und Apps auf Arm ausgeführt werden, welche Einschränkungen es gibt und wo Sie weitere Informationen finden.
ms.date: 05/22/2020
ms.topic: article
keywords: Windows 10 s, immer verbunden, arm, ARM64, x86-Emulation
ms.localizationpriority: medium
ms.openlocfilehash: abb6e891d1f23ae94d61732d70af6bc3babcb07f
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102192932"
---
# <a name="windows-10-on-arm"></a>Windows 10 auf ARM
Ursprünglich konnte Windows 10 (wie von Windows 10 Mobile unterschieden) nur auf PCs ausgeführt werden, die von x86-und x64-Prozessoren unter betrieben wurden. Jetzt kann Windows 10 Desktop auf Computern ausgeführt werden, die von ARM64-Prozessoren mit dem Fall Creators Update oder neuer unter betrieben werden. Mit dem Energiesparmodus der ARM-CPU-Architektur können diese PCs die gesamte Akku Lebensdauer und Unterstützung für mobile Daten Netzwerke aufweisen. Diese PCs stellen eine hervorragend vorhandene Anwendungs Kompatibilität bereit und ermöglichen es Ihnen, vorhandene x86-Win32-Anwendungen unverändert auszuführen. Weitere Informationen oder eine Demo finden Sie im [Channel 9-Video für den stets verbundenen PC](https://channel9.msdn.com/Events/Build/2017/P4171).

Wir verwenden den Begriff " *Arm* here" als Kurzform für PCs, auf denen die Desktop Version von Windows 10 auf ARM64 (auch als " *AArch64*" bezeichnet) ausgeführt wird.  Wir verwenden den Begriff *ARM32* hier als Kurzform für die 32-Bit-ARM-Architektur (in der Regel *Arm* in anderer Dokumentation genannt).

## <a name="apps-and-experiences-on-arm"></a>Apps und Erfahrungen auf Arm

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Integrierte Windows 10-Umgebungen,-Apps und-Treiber
Die integrierten Windows 10-Umgebungen, wie z. b. Edge, Cortana, Startmenü und Explorer, sind System eigen und werden als ARM64 ausgeführt. Dies umfasst auch alle Gerätetreiber, z. b. Grafiken, Netzwerke oder die Festplatte. Dadurch wird sichergestellt, dass das Gerät, das mit der vollständigen nativen Geschwindigkeit des Qualcomm-Snapdragon-Prozessors ausgeführt wird, die beste Benutzer Leistung und Akku Lebensdauer erreicht.

### <a name="universal-windows-platform-uwp-apps"></a>Universelle Windows-Plattform-Apps (UWP)
Windows 10 auf Arm führt alle x86-, ARM32-und ARM64 [UWP-apps](../get-started/universal-application-platform-guide.md) aus dem Microsoft Store aus. ARM32-und ARM64-apps werden nativ ohne jegliche Emulation ausgeführt, während x86-apps unter Emulation ausgeführt werden. Wenn Sie ein UWP-Entwickler sind, stellen Sie sicher, dass Sie ein Arm-Paket für Ihre APP übermitteln, da dadurch die beste Benutzer Leistung für das Gerät bereitgestellt wird. Weitere Informationen finden Sie unter [App-Paket Architekturen](/windows/msix/package/device-architecture).

>[!NOTE]
> Wenn Sie Ihre UWP-Anwendung erstellen möchten, um die ARM64-Plattform nativ zu erreichen, benötigen Sie Visual Studio 2017, Version 15,9 oder höher oder Visual Studio 2019. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Windows 10 auf Arm unterstützt x86-, ARM32-und ARM64-UWP-Apps aus Store auf ARM64-Geräten. Wenn ein Benutzer die UWP-App auf einem ARM64-Gerät herunterlädt, installiert das Betriebssystem automatisch die optimale Version der APP, die verfügbar ist. Wenn Sie x86-, ARM32-und ARM64-Versionen Ihrer APP an den Store übermitteln, installiert das Betriebssystem automatisch die ARM64-Version Ihrer APP. Wenn Sie nur x86-und ARM32-Versionen Ihrer APP übermitteln, installiert das Betriebssystem die ARM32-Version. Wenn Sie nur die x86-Version der APP übermitteln, installiert das Betriebssystem diese Version und führt Sie unter Emulation aus. Weitere Informationen zu Architekturen finden Sie unter [App-Paket Architekturen](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Win32-Apps
Zusätzlich zu UWP-apps kann Windows 10 auf Arm auch Ihre x86-Win32-apps unverändert ausführen, mit einer guten Leistung und einer nahtlosen Benutzer Darstellung, wie bei jedem beliebigen PC. Diese x86-Win32-apps müssen für Arm nicht neu kompiliert werden und bemerken nicht einmal, dass Sie auf einem Arm-Prozessor ausgeführt werden. Beachten Sie, dass 64-Bit-x64-Win32-apps nicht unterstützt werden, aber für die meisten apps ist x86-Versionen verfügbar.  Wenn Sie die Auswahl der APP-Architektur gewählt haben, wählen Sie einfach die Version 32-Bit x86 aus, um die APP auf einem Windows 10 auf Arm-PC auszuführen.

## <a name="downloads"></a>Downloads

Visual Studio 2019 bietet verschiedene Tools zum Herunterladen von Tools für Windows 10 auf Arm. Benutzer, die weiterhin Visual Studio 2017 verwenden, können das Installationsprogramm verwenden, um vergleichbare Tools und Pakete zu suchen und zu installieren. Beachten Sie, dass Sie Visual Studio 2019 verwenden müssen, um diese Schritte ausführen zu können.

### <a name="visual-c-redistributable"></a>Visual C++ Redistributable

Das Visual C++ Redist-Paket ist für Arm-apps verfügbar. Besuchen Sie die [Visual Studio-Downloadseite](https://visualstudio.microsoft.com/downloads/) , Scrollen Sie nach unten zu **alle Downloads**, öffnen Sie **andere Tools und Frameworks**, und navigieren Sie dann zum Eintrag **Microsoft Visual C++ Redistributable für Visual Studio 2019** . Aktivieren Sie das Optionsfeld **ARM64** , und **Laden Sie dann herunter**.

### <a name="remote-tools"></a>-Remotetools

Remotetools für Visual Studio sind für Arm-apps verfügbar. Besuchen Sie die [Visual Studio-Downloadseite](https://visualstudio.microsoft.com/downloads/) , Scrollen Sie nach unten zu **alle Downloads**, öffnen Sie **Tools für Visual Studio 2019**, und navigieren Sie dann zum Eintrag **Remotetools für Visual Studio 2019** . Wählen Sie das Optionsfeld **ARM64* und dann **herunterladen** aus.


## <a name="in-this-section"></a>In diesem Abschnitt
|Thema | BESCHREIBUNG |
|-----|-----|
|[Funktionsweise der x86-Emulation auf ARM](apps-on-arm-x86-emulation.md)|In dieser Übersicht wird erläutert, wie x86-apps auf Arm emuliert werden.|
|[Problembehandlung für x86-Apps auf ARM](apps-on-arm-troubleshooting-x86.md)|Häufige Probleme mit x86-Apps bei der Ausführung auf Arm und deren Behebung. |
|[Problembehandlung bei Arm-apps auf Arm](apps-on-arm-troubleshooting-arm32.md)|Häufige Probleme mit ARM32-und ARM64-Apps bei der Ausführung auf Arm und deren Behebung. |
|[Problembehandlung für die Programmkompatibilität auf ARM](apps-on-arm-program-compat-troubleshooter.md)|Leitfaden zum Anpassen von Kompatibilitäts Einstellungen, wenn Ihre APP auf Arm nicht ordnungsgemäß funktioniert. |

## <a name="related-topics"></a>Zugehörige Themen
|Thema | BESCHREIBUNG |
|-----|-----|
|[Entwickeln von ARM64-Treibern mit dem WDK](/windows-hardware/drivers/develop/building-arm64-drivers)|Anweisungen zum Entwickeln eines ARM64-Treibers. |
| [Debugging von x86-apps auf Arm](/windows-hardware/drivers/debugger/debugging-arm64) | Leitfaden zum Debuggen von x86-apps auf Arm. |
