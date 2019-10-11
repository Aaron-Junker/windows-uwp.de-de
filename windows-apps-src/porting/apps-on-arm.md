---
title: Windows 10 auf ARM
description: Dieser Artikel bietet eine Übersicht darüber, wie Funktionen und Apps auf ARM ausgeführt werden, welche Einschränkungen bestehen und wo Sie weitere Informationen erhalten können.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, ARM, ARM64, x86-emulation
ms.localizationpriority: medium
ms.openlocfilehash: 7450b469f77fec4288ad6dff01ee7673affc8dd9
ms.sourcegitcommit: f3c1a81b50f4a372a15996ac71b3f408a8ee1409
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72237534"
---
# <a name="windows-10-on-arm"></a>Windows 10 auf ARM
Ursprünglich konnte Windows 10 (im Unterschied zu Windows 10 Mobile) nur auf PCs ausgeführt werden, die über x86- und x64-Prozessoren verfügen. Jetzt kann Windows 10 Desktop auf Computern ausgeführt werden, die von ARM64-Prozessoren mit dem Fall Creators Update oder neuer unter betrieben werden. Dank der stromsparenden Architektur der ARM-CPU weisen diese PCs eine Akkulaufzeit über einen ganzen Tag auf und erhalten Unterstützung für mobile Datennetzwerke. Diese PCs bieten eine hervorragende Anwendungskompatibilität und ermöglichen Ihnen, Ihre bestehenden x86 win32-Anwendungen unverändert auszuführen. Weitere Informationen oder eine Demo finden Sie im [Channel 9-Video für den stets verbundenen PC](https://channel9.msdn.com/Events/Build/2017/P4171).

Wir verwenden den Begriff *ARM* hier als eine Kurzform für PCs, auf denen die Desktopversion von Windows 10 auf ARM64-Prozessoren (oft auch als *AArch64* bezeichnet) ausgeführt wird.  Wir verwenden den Begriff *ARM32"* hier als eine Kurzform für die 32-Bit-ARM-Architektur (in anderen Dokumentationen häufig *ARM* genannt).

## <a name="apps-and-experiences-on-arm"></a>Apps und Funktionen auf ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Integrierte Windows 10-Funktionen, Apps und Treiber
Die integrierten Windows 10-Umgebungen, wie z. b. Edge, Cortana, Startmenü und Explorer, sind System eigen und werden als ARM64 ausgeführt. Dies umfasst auch alle Gerätetreiber, z. b. Grafiken, Netzwerke oder die Festplatte. Dadurch wird sichergestellt, dass das Gerät, das mit der vollständigen nativen Geschwindigkeit des Qualcomm-Snapdragon-Prozessors ausgeführt wird, die beste Benutzer Leistung und Akku Lebensdauer erreicht.

### <a name="universal-windows-platform-uwp-apps"></a>Apps für die Universelle Windows-Plattform (UWP)
Windows 10 auf Arm führt alle x86-, ARM32-und ARM64 [UWP-apps](../get-started/universal-application-platform-guide.md) aus dem Microsoft Store aus. ARM32-und ARM64-apps werden nativ ohne jegliche Emulation ausgeführt, während x86-apps unter Emulation ausgeführt werden. Wenn Sie eine UWP-Entwickler sind, stellen Sie bitte sicher, dass Sie ein ARM-Paket für Ihre App übermitteln, da dies die beste Benutzererfahrung für das Gerät bietet. Weitere Informationen finden Sie unter [App-Paket-Architekturen](/windows/msix/package/device-architecture).

>[!NOTE]
> Wenn Sie Ihre UWP-Anwendung erstellen möchten, um die ARM64-Plattform nativ zu erreichen, benötigen Sie Visual Studio 2017, Version 15,9 oder höher oder Visual Studio 2019. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Wenn ein Benutzer eine UWP-App aus dem Microsoft Store herunterlädt, wird die ARM32-Version auf einem ARM64-Gerät installiert, sofern nicht nur eine x86-Version verfügbar ist. Weitere Informationen zu Architekturen finden Sie unter [App-Paket-Architekturen](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Win32-Apps
Zusätzlich zu UWP-apps kann Windows 10 auf Arm auch Ihre x86-Win32-apps unverändert ausführen, mit einer guten Leistung und einer nahtlosen Benutzer Darstellung, wie bei jedem beliebigen PC. Diese x86-Win32-apps müssen für Arm nicht neu kompiliert werden und bemerken nicht einmal, dass Sie auf einem Arm-Prozessor ausgeführt werden. Beachten Sie, dass 64-Bit-x64-Win32-apps nicht unterstützt werden, aber für die meisten apps ist x86-Versionen verfügbar.  Wenn Sie die Auswahl der APP-Architektur gewählt haben, wählen Sie einfach die Version 32-Bit x86 aus, um die APP auf einem Windows 10 auf Arm-PC auszuführen.

## <a name="in-this-section"></a>Inhalt dieses Abschnitts
|Thema | Beschreibung |
|-----|-----|
|[Funktionsweise der x86-Emulation auf ARM](apps-on-arm-x86-emulation.md)|Eine Übersicht mit detaillierten Informationen dazu, wie x86-Apps auf ARM emuliert werden.|
|[Problembehandlung für x86-Apps auf ARM](apps-on-arm-troubleshooting-x86.md)|Häufig auftretende Probleme mit x86-Apps bei der Ausführung auf ARM, und wie diese Probleme behoben werden können. |
|[Problembehandlung bei Arm-apps auf Arm](apps-on-arm-troubleshooting-arm32.md)|Häufige Probleme mit ARM32-und ARM64-Apps bei der Ausführung auf Arm und deren Behebung. |
|[Problembehandlung für die Programmkompatibilität auf ARM](apps-on-arm-program-compat-troubleshooter.md)|Anleitung zum Anpassen der Kompatibilitätseinstellungen, wenn Ihre App auf ARM nicht korrekt funktioniert. |

## <a name="related-topics"></a>Verwandte Themen
|Thema | Beschreibung |
|-----|-----|
|[Entwickeln von ARM64-Treibern mit dem WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Anleitung zum Erstellen eines ARM64-Treibers. |
| [Debugging von x86-apps auf Arm](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Anleitung für das Debuggen von x86-Apps auf ARM. |
