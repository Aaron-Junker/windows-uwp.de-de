---
title: Windows 10 auf ARM
description: Dieser Artikel bietet eine Übersicht darüber, wie Funktionen und Apps auf ARM ausgeführt werden, welche Einschränkungen bestehen und wo Sie weitere Informationen erhalten können.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, ARM, ARM64, x86-emulation
ms.localizationpriority: medium
ms.openlocfilehash: 47677cb2a9e8d62c76f10f932b142c4dba9752c6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640995"
---
# <a name="windows-10-on-arm"></a>Windows 10 auf ARM
Ursprünglich konnte Windows 10 (im Unterschied zu Windows 10 Mobile) nur auf PCs ausgeführt werden, die über x86- und x64-Prozessoren verfügen. Windows 10 Desktop (Pro und S-Editionen) kann jetzt auf Computern ausgeführt werden, die über ARM64 Prozessoren mit dem Fall Creators Update verfügen. Dank der stromsparenden Architektur der ARM-CPU weisen diese PCs eine Akkulaufzeit über einen ganzen Tag auf und erhalten Unterstützung für mobile Datennetzwerke. Diese PCs bieten eine hervorragende Anwendungskompatibilität und ermöglichen Ihnen, Ihre bestehenden x86 win32-Anwendungen unverändert auszuführen. z. B. Adobe Reader. Für weitere Informationen oder Demos sehen Sie sich das [Channel 9-Video für den Always Connected-PC](https://channel9.msdn.com/Events/Build/2017/P4171) an.

Wir verwenden den Begriff *ARM* hier als eine Kurzform für PCs, auf denen die Desktopversion von Windows 10 auf ARM64-Prozessoren (oft auch als *AArch64* bezeichnet) ausgeführt wird.  Wir verwenden den Begriff *ARM32"* hier als eine Kurzform für die 32-Bit-ARM-Architektur (in anderen Dokumentationen häufig *ARM* genannt).

## <a name="apps-and-experiences-on-arm"></a>Apps und Funktionen auf ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Integrierte Windows 10-Funktionen, Apps und Treiber
Die integrierten Windows 10-Funktionen wie z. B. Microsoft Edge, Cortana, Startmenü und Explorer sind alle nativ und werden als ARM64 (oder ARM32) ausgeführt. Dies beinhaltet auch alle Gerätetreiber wie z. B. Grafik, Netzwerk oder die Festplatte. Dies gewährleistet, dass Sie die beste Benutzererfahrung und Akkulaufzeit Ihres Geräts mit der vollen nativen Geschwindigkeit des Qualcomm Snapdragon-Prozessors erhalten.

### <a name="universal-windows-platform-uwp-apps"></a>Apps für die Universelle Windows-Plattform (UWP)
Windows 10 auf ARM ausgeführt wird, alle X86, ARM64 und ARM32 [UWP-apps](../get-started/universal-application-platform-guide.md) aus dem Microsoft Store. ARM32 und ARM64-apps nativ ausführen, ohne jegliche Emulation, while X86, die Ausführung von apps unter Emulation. Wenn Sie eine UWP-Entwickler sind, stellen Sie bitte sicher, dass Sie ein ARM-Paket für Ihre App übermitteln, da dies die beste Benutzererfahrung für das Gerät bietet. Weitere Informationen finden Sie unter [App-Paket-Architekturen](../packaging/device-architecture.md).

>[!NOTE]
> Um Ihrer UWP-Anwendung für die ARM64 systemintern Zielplattform erstellen zu können, müssen Sie Visual Studio 2017 Version 15.9 oder höher verfügen. Weitere Informationen finden Sie unter [in diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

>[!IMPORTANT]
> Wenn ein Benutzer eine UWP-App aus dem Microsoft Store herunterlädt, wird die ARM32-Version auf einem ARM64-Gerät installiert, sofern nicht nur eine x86-Version verfügbar ist. Weitere Informationen zu Architekturen finden Sie unter [App-Paket-Architekturen](../packaging/device-architecture.md).

### <a name="win32-apps"></a>Win32-Apps
Zusätzlich zur UWP-apps, Windows 10 auf ARM können auch Ausführen Ihrer X86 Win32 apps (z. B. Adobe Reader), in unveränderter Form zusammen mit guter Leistung und nahtloses Benutzererlebnis, wie Sie einem beliebigen PC. Diese X86 Win32-apps müssen nicht neu kompiliert für ARM und nicht einmal bewusst ist, die sie für ARM-Prozessor ausgeführt werden. Beachten Sie, dass 64-Bit-x64-Win32-Anwendungen nicht unterstützt werden, aber die überwiegende Mehrheit der Apps alle über x86-Versionen ihrer Apps verfügen. Wählen Sie aus der Benutzerperspektive einfach das 32-Bit-x86-Installationsprogramm für den PC mit Windows auf ARM aus.

## <a name="in-this-section"></a>Inhalt dieses Abschnitts
|Thema | Beschreibung |
|-----|-----|
|[Wie X86 Netzwerkemulation auf ARM funktioniert](apps-on-arm-x86-emulation.md)|Eine Übersicht mit detaillierten Informationen dazu, wie x86-Apps auf ARM emuliert werden.|
|[Problembehandlung bei X86 apps auf ARM](apps-on-arm-troubleshooting-x86.md)|Häufig auftretende Probleme mit x86-Apps bei der Ausführung auf ARM, und wie diese Probleme behoben werden können. |
|[Problembehandlung bei der ARM-apps auf ARM](apps-on-arm-troubleshooting-arm32.md)|Häufige Probleme mit ARM32 und ARM64-apps, bei der Ausführung auf ARM und zur Behebung dieser Probleme. |
|[Program Compatibility-Problembehandlung auf ARM](apps-on-arm-program-compat-troubleshooter.md)|Anleitung zum Anpassen der Kompatibilitätseinstellungen, wenn Ihre App auf ARM nicht korrekt funktioniert. |

## <a name="related-topics"></a>Verwandte Themen
|Thema | Beschreibung |
|-----|-----|
|[Erstellen von ARM64-Treiber mit WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Anleitung zum Erstellen eines ARM64-Treibers. |
| [Debuggen von X86 apps auf ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Anleitung für das Debuggen von x86-Apps auf ARM. |
