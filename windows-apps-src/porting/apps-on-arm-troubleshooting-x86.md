---
title: Problembehandlung bei x86-Desktop-Apps
description: Häufig auftretende Probleme mit x86-Apps bei der Ausführung auf ARM, und wie diese Probleme behoben werden können.
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, always connected, x86-emulation auf ARM, problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: 480d9cd6508b149e0d4966bae8835c05d30db7af
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682745"
---
# <a name="troubleshooting-x86-desktop-apps"></a>Problembehandlung bei x86-Desktop-Apps
>[!IMPORTANT]
> Das ARM64-SDK ist jetzt als Teil von Visual Studio 15.8 Preview 1 verfügbar. Wir empfehlen, die App für ARM64 erneut zu kompilieren, damit sie mit der höchstmöglichen systemeigenen Geschwindigkeit ausgeführt wird. Weitere Informationen finden Sie im Blogbeitrag [Early preview of Visual Studio support for Windows 10 on ARM development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/).

Für den Fall, dass eine x86-Desktop-App nicht wie auf einem x86-Computer funktioniert, finden Sie hier einige Anleitungen, die Ihnen bei der Problembehandlung helfen können.

|Problem|Lösung|
|-----|--------|
| Ihre App verwendet einen Treiber, der nicht für ARM entwickelt wurde. | Kompilieren Sie Ihren x86-Treiber neu zu ARM64. Siehe [Entwickeln von ARM64-Treibern mit WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers). |
| Ihre App ist nur für x64 verfügbar. | Wenn Sie für den Microsoft Store entwickeln, übermitteln Sie eine ARM-Version Ihrer App. Weitere Informationen finden Sie unter [App-Paketarchitekturen](/windows/msix/package/device-architecture). Wenn Sie ein Win32-Entwickler sind, empfehlen wie, dass Sie Ihre App für ARM64 neu kompilieren. Weitere Informationen finden Sie unter [Early preview of Visual Studio support for Windows 10 on ARM development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). |
| Ihre App verwendet eine Version von OpenGL höher als 1.1 oder erfordert hardwarebeschleunigte OpenGL. | Verwenden Sie den DirectX-Modus der App, wenn er verfügbar ist. x86 Apps mit DirectX 9, DirectX 10, DirectX 11 und DirectX 12 funktionieren auf ARM. Weitere Informationen finden Sie unter [DirectX-Grafiken und -Spiele](https://docs.microsoft.com/windows/desktop/directx). |
| Ihre x86-App funktioniert nicht wie erwartet. | Versuchen Sie es mit der Problembehandlung für die Programmkompatibilität, indem Sie die Schritte unter [Problembehandlung für die Programmkompatibilität auf ARM](apps-on-arm-program-compat-troubleshooter.md) befolgen. Weitere Schritte zur Problembehandlung finden Sie im Artikel [Problembehandlung bei x86 Apps auf ARM](apps-on-arm-troubleshooting-x86.md). |

## <a name="best-practices-for-wow"></a>Bewährte Methoden für WOW
Eine allgemeines Problem tritt auf, wenn eine App ermittelt, dass sie unter WOW ausgeführt wird, und daraufhin annimmt, dass sie sich auf einem x64-System befindet. Nach dieser Annahme kann die App Folgendes tun:

- Versuchen, die x64-Version selbst zu installieren, die auf ARM nicht unterstützt wird.
- In der nativen Registrierungsansicht nach anderer Software suchen.
- Annehmen, dass ein 64-Bit-.NET Framework verfügbar ist.

Im Allgemeinen sollte eine App keine Annahmen über das Hostsystem machen, wenn festgestellt wird, dass es unter WOW läuft. Interaktionen mit nativen Komponenten des Betriebssystems so weit wie möglich vermeiden.

Eine App kann Registrierungsschlüssel unter der nativen Registrierungsansicht platzieren oder Funktionen je nach dem Vorhandensein von WOW ausführen. Der ursprüngliche **IsWow64Process** gibt nur an, ob die App auf einem x64-Computer ausgeführt wird. Apps sollten jetzt [IsWow64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2) verwenden, um zu bestimmen, ob sie auf einem System mit WOW-Unterstützung ausgeführt werden. 

## <a name="drivers"></a>Treiber 
Alle Kernelmodustreiber, [User-Mode Driver Framework (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf)-Treiber und Druckertreiber müssen entsprechend der Architektur des Betriebssystems kompiliert werden. Wenn eine x86-Anwendung über einen Treiber verfügt, muss dieser Treiber für ARM64 neu kompiliert werden. Die x86-App funktioniert möglicherweise problemlos unter Emulation, ihr Treiber muss jedoch für ARM64 neu kompiliert werden, und App-Funktionen, die von diesem Treiber abhängen, stehen nicht zur Verfügung. Weitere Informationen zum Kompilieren des Treibers für ARM64 finden Sie unter [Entwickeln von ARM64-Treibern mit WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers).

## <a name="shell-extensions"></a>-Shellerweiterungen 
Apps, die versuchen, Windows-Komponenten zu verknüpfen oder ihre DLLs in Windows-Prozesse zu laden, müssen diese DLLs neu kompilieren, damit sie der Architektur des Systems entsprechen, d.h. ARM64. In der Regel werden diese von Eingabemethoden-Editoren (IMEs), Hilfstechnologien und Shell-Erweiterungs-Apps verwendet (z. B. um Cloud-Speichersymbole in Explorer oder einem Kontextmenü anzuzeigen). Weitere Informationen zum Neukompilieren von Apps oder DLLs für ARM64 finden Sie im Blogbeitrag der [Early preview of Visual Studio support for Windows 10 on ARM development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). 

## <a name="debugging"></a>Debuggen
Wenn Sie das Verhalten Ihrer App genauer untersuchen möchten, lesen Sie den Artikel [Debugging on ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64). Darin erfahren Sie Details über Tools und Strategien zum Debuggen unter ARM.

## <a name="virtual-machines"></a>Virtuelle Computer
Die Windows-Hypervisor-Plattform wird auf der Qualcomm Snapdragon 835 Mobile PC-Plattform nicht unterstützt. Daher funktioniert das Ausführen von virtuellen Computern mit Hyper-V nicht. Wir investieren weiterhin in diese Technologien auf zukünftigen Qualcomm-Chipsätzen. 

## <a name="dynamic-code-generation"></a>Dynamische Code Generierung
X86-Desktop-Apps werden auf ARM64 durch das System emuliert, das ARM64-Anweisungen zur Laufzeit erzeugt. Dies bedeutet, dass eine x86-Desktop-App das generieren oder Ändern dynamischer Code im Prozess verhindert, dass diese APP nicht als x86 auf ARM64 ausgeführt werden kann. 

Dies ist eine Sicherheits Entschärfung, die einige apps bei Ihrem Prozess mithilfe der [setprocessentschärationpolicy](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy) -API mit dem `ProcessDynamicCodePolicy` -Flag aktivieren. Diese Entschärfungs Richtlinie muss deaktiviert werden, damit Sie auf ARM64 erfolgreich als x86-Prozess ausgeführt werden kann. 
