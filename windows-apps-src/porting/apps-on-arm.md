---
title: Windows 10 auf ARM
description: Dieser Artikel bietet eine Übersicht darüber, wie Benutzererfahrungen und Apps auf ARM ausgeführt werden, welche Einschränkungen bestehen und wo Sie mehr erfahren können.
ms.date: 05/22/2020
ms.topic: article
keywords: Windows 10 s, immer verbunden, ARM, ARM64, x86-Emulation
ms.localizationpriority: medium
ms.openlocfilehash: ff763ea543e8dd6592e1f8502438a684ec38be16
ms.sourcegitcommit: 6cd970686d1ea7176b7e6651f349a14551709820
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559369"
---
# <a name="windows-10-on-arm"></a>Windows 10 auf ARM
Ursprünglich Windows 10 (wie von Windows 10 Mobile unterschieden) nur auf PCs ausgeführt, die von x86- und x64-Prozessoren unterstützt wurden. Jetzt kann Windows 10 Desktop auf Computern ausgeführt werden, die von ARM64-Prozessoren mit dem Fall Creators Update oder einer neueren Version unterstützt werden. Da die ARM-CPU-Architektur energiesparend ist, können diese PCs über die ganze Akkulaufzeit verfügen und mobile Datennetzwerke unterstützen. Diese PCs bieten eine hervorragende Anwendungskompatibilität und ermöglichen es Ihnen, Ihre vorhandenen x86 win32-Anwendungen unverändert ausführen zu können. Weitere Informationen oder eine Demo finden Sie im [Channel 9-Video für den Always Connected PC.](https://channel9.msdn.com/Events/Build/2017/P4171)

Wir verwenden den Begriff *ARM* hier als Kurzfassung für PCs, auf denen die Desktopversion von Windows 10 auf ARM64-Prozessoren (auch als *AArch64* bezeichnet) ausgeführt wird.  Wir verwenden hier den Begriff *ARM32* als Kurzbegriff für die 32-Bit-ARM-Architektur (in der anderen Dokumentation häufig *als ARM* bezeichnet).

## <a name="apps-and-experiences-on-arm"></a>Apps und Erfahrungen auf ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Integrierte Windows 10, Apps und Treiber
Die integrierten Windows 10 wie Edge, Cortana, Startmenü und Explorer sind nativ und werden als ARM64 ausgeführt. Dies schließt auch alle Gerätetreiber ein, z. B. Grafiken, Netzwerke oder die Festplatte. Dadurch wird sichergestellt, dass Sie die beste Benutzererfahrung und Akkulaufzeit für Ihr Gerät mit der vollständigen nativen Geschwindigkeit des Prozessors Von Qualcomm Prozessor erhalten.

### <a name="universal-windows-platform-uwp-apps"></a>Universelle Windows-Plattform-Apps (UWP)
Windows 10 auf ARM werden alle x86-, ARM32- und [ARM64-UWP-Apps](../get-started/universal-application-platform-guide.md) aus dem Microsoft Store. ARM32- und ARM64-Apps werden nativ ohne Emulation ausgeführt, während x86-Apps unter Emulation ausgeführt werden. Wenn Sie UWP-Entwickler sind, stellen Sie sicher, dass Sie ein ARM-Paket für Ihre App übermitteln, da dies die beste Benutzererfahrung für das Gerät bietet. Weitere Informationen finden Sie unter [App-Paketarchitekturen.](/windows/msix/package/device-architecture)

>[!NOTE]
> Um Ihre UWP-Anwendung nativ für die ARM64-Plattform zu erstellen, benötigen Sie Visual Studio 2017 Version 15.9 oder höher oder Visual Studio 2019. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Windows 10 auf ARM unterstützt X86-, ARM32- und ARM64-UWP-Apps aus dem Store auf ARM64-Geräten. Wenn ein Benutzer Ihre UWP-App auf ein ARM64-Gerät herunterlädt, installiert das Betriebssystem automatisch die optimale Verfügbare Version Ihrer App. Wenn Sie x86-, ARM32- und ARM64-Versionen Ihrer App an den Store übermitteln, installiert das Betriebssystem automatisch die ARM64-Version Ihrer App. Wenn Sie nur x86- und ARM32-Versionen Ihrer App übermitteln, installiert das Betriebssystem die ARM32-Version. Wenn Sie nur die x86-Version Ihrer App übermitteln, installiert das Betriebssystem diese Version und führt sie unter Emulation aus. Weitere Informationen zu Architekturen finden Sie unter [App-Paketarchitekturen.](/windows/msix/package/device-architecture)

### <a name="win32-apps"></a>Win32-Apps
Zusätzlich zu UWP-Apps können Windows 10 auf ARM ihre x86 Win32-Apps unverändert ausführen, mit guter Leistung und einer nahtlosen Benutzeroberfläche, genau wie jeder pc. Diese x86 Win32-Apps müssen nicht für ARM neu kompiliert werden und erkennen nicht einmal, dass sie auf einem ARM-Prozessor ausgeführt werden.

### <a name="x86-64-apps"></a>x86-64-Apps
Die anfängliche Unterstützung für x86-64-Anwendungen wurde in Build 21277 hinzugefügt und wird derzeit weiterentwickelt. Wenn die x64 Win32-Version einer App nicht funktioniert, sind für die meisten Apps auch x86-Versionen verfügbar. Wenn Sie die App-Architektur auswählen, wählen Sie einfach die 32-Bit x86-Version aus, um die 32-Bit-Version der App auf einem Windows 10 auf dem ARM-PC auszuführen.

## <a name="downloads"></a>Downloads

Visual Studio 2019 bietet mehrere Toolsdownloads für Windows 10 auf ARM. Benutzer, die weiterhin Visual Studio 2017 verwenden, können das Installationsprogramm verwenden, um vergleichbare Tools und Pakete zu suchen und zu installieren. Beachten Sie, dass Sie Visual Studio 2019 verwenden müssen, um diese Schritte ausführen zu können.

### <a name="visual-c-redistributable"></a>Visual C++ Redistributable

Das Visual C++ Redist-Paket ist für ARM-Apps verfügbar. Navigieren Sie [zur Seite Visual Studio Downloads,](https://visualstudio.microsoft.com/downloads/) scrollen Sie nach unten zu **Alle Downloads,** öffnen Sie **Andere Tools und Frameworks, und** navigieren Sie dann zum Eintrag Microsoft Visual C++ **Redistributable for Visual Studio 2019.** Wählen Sie das **Optionsfeld ARM64** und dann **Herunterladen aus.**

### <a name="remote-tools"></a>-Remotetools

Remotetools für Visual Studio sind für ARM-Apps verfügbar. Navigieren Sie [Visual Studio](https://visualstudio.microsoft.com/downloads/) Downloadseite nach unten zu Alle **Downloads,** öffnen Sie Tools für **Visual Studio 2019,** und navigieren Sie dann zum Eintrag Remotetools für Visual Studio **2019.** Wählen Sie das Optionsfeld **ARM64* und dann **Herunterladen aus.**


## <a name="in-this-section"></a>In diesem Abschnitt
|Thema | BESCHREIBUNG |
|-----|-----|
|[Funktionsweise der x86-Emulation auf ARM](apps-on-arm-x86-emulation.md)|Eine Übersicht, in der erläutert wird, wie x86-Apps auf ARM emuliert werden.|
|[Problembehandlung für x86-Apps auf ARM](apps-on-arm-troubleshooting-x86.md)|Häufige Probleme mit x86-Apps bei der Ausführung auf ARM und deren Behebung. |
|[Problembehandlung für ARM-Apps in ARM](apps-on-arm-troubleshooting-arm32.md)|Häufige Probleme mit ARM32- und ARM64-Apps bei der Ausführung auf ARM und deren Behebung. |
|[Problembehandlung für die Programmkompatibilität auf ARM](apps-on-arm-program-compat-troubleshooter.md)|Anleitung zum Anpassen von Kompatibilitätseinstellungen, wenn Ihre App auf ARM nicht ordnungsgemäß funktioniert. |

## <a name="related-topics"></a>Zugehörige Themen
|Thema | BESCHREIBUNG |
|-----|-----|
|[Entwickeln von ARM64-Treibern mit dem WDK](/windows-hardware/drivers/develop/building-arm64-drivers)|Anweisungen zum Erstellen eines ARM64-Treibers. |
| [Debuggen von x86-Apps auf ARM](/windows-hardware/drivers/debugger/debugging-arm64) | Anleitung zum Debuggen von x86-Apps auf ARM. |
