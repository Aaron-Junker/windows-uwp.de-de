---
title: Problembehandlung bei x86-Desktop Anwendungen
description: Erfahren Sie, wie Sie häufige Probleme mit einer x86-Desktop-App, die unter ARM64 ausgeführt wird, beheben und beheben, einschließlich Informationen zu Treibern, Shellerweiterungen und Debuggen.
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10 s, immer verbunden, x86-Emulation auf arm, Problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: 4dbb3c485d3f6ba3ba410e2a960162880b6f3660
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970268"
---
# <a name="troubleshooting-x86-desktop-apps"></a>Problembehandlung bei x86-Desktop Anwendungen
>[!IMPORTANT]
> Das ARM64 SDK ist jetzt als Teil von Visual Studio 15,8 Preview 1 verfügbar. Es wird empfohlen, dass Sie Ihre APP in ARM64 neu kompilieren, damit Ihre APP mit vollständiger nativer Geschwindigkeit ausgeführt wird. Weitere Informationen finden Sie in der [frühen Vorschau von Visual Studio-Unterstützung für Windows 10 auf der Arm-Entwicklung im](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) Blogbeitrag.

Wenn eine x86-Desktop-App nicht so funktioniert wie auf einem x86-Computer, finden Sie hier einige Anleitungen zur Problembehandlung.

|Problem|Lösung|
|-----|--------|
| Ihre APP basiert auf einem Treiber, der nicht für Arm konzipiert ist. | Kompilieren Sie Ihren x86-Treiber neu in ARM64. Weitere Informationen finden Sie unter [Building ARM64 Drivers with the WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers). |
| Ihre APP ist nur für x64 verfügbar. | Wenn Sie für Microsoft Store entwickeln, übermitteln Sie eine Arm-Version Ihrer APP. Weitere Informationen finden Sie unter [App-Paket Architekturen](/windows/msix/package/device-architecture). Wenn Sie ein Win32-Entwickler sind, empfehlen wir Ihnen, Ihre APP in ARM64 neu zu kompilieren. Weitere Informationen finden Sie [in der frühen Vorschau von Visual Studio-Unterstützung für Windows 10 in der Arm-Entwicklung](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). |
| Ihre APP verwendet eine OpenGL-Version, die höher als 1,1 ist oder Hardware beschleunigtes OpenGL erfordert. | Verwenden Sie den DirectX-Modus der APP, sofern verfügbar. x86-apps, die DirectX 9, DirectX 10, DirectX 11 und DirectX 12 verwenden, funktionieren auf Arm. Weitere Informationen finden Sie unter [DirectX-Grafiken und-Spiele](https://docs.microsoft.com/windows/desktop/directx). |
| Ihre x86-APP funktioniert nicht wie erwartet. | Verwenden Sie die Kompatibilitätsproblem Behandlung, indem Sie die Anweisungen unter [Programm Kompatibilitätsproblem Behandlung auf Arm](apps-on-arm-program-compat-troubleshooter.md)befolgen. Weitere Informationen zur Problembehandlung finden Sie im Artikel [Problembehandlung bei x86-apps auf Arm](apps-on-arm-troubleshooting-x86.md) . |

## <a name="best-practices-for-wow"></a>Bewährte Methoden für WoW
Ein häufiges Problem tritt auf, wenn eine APP erkennt, dass Sie unter wow ausgeführt wird, und dann annimmt, dass Sie sich auf einem x64-System befindet. Wenn Sie diese Annahme getroffen haben, kann die app folgende Aktionen ausführen:

- Versuchen Sie, die x64-Version von sich selbst zu installieren, die auf Arm nicht unterstützt wird.
- Überprüfen Sie die systemeigene Registrierungs Ansicht auf andere Software.
- Angenommen, ein 64-Bit-.NET Framework ist verfügbar.

Im Allgemeinen sollte eine APP keine Annahmen über das Host System treffen, wenn Sie für die Durchführung unter wow bestimmt wird. Vermeiden Sie so weit wie möglich die Interaktion mit nativen Komponenten des Betriebssystems.

Eine APP kann Registrierungsschlüssel in der nativen Registrierungs Ansicht platzieren oder basierend auf dem vorhanden sein von WoW Funktionen ausführen. Der ursprüngliche **IsWow64Process**  -Wert gibt nur an, ob die APP auf einem x64-Computer ausgeführt wird. Apps sollten jetzt [IsWow64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2) verwenden, um zu bestimmen, ob Sie auf einem System mit WoW-Unterstützung ausgeführt werden. 

## <a name="drivers"></a>Treiber 
Alle Kernelmodustreiber, der [Benutzermodus-Treiber Framework](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) -Treiber und die Druckertreiber müssen entsprechend der Architektur des Betriebssystems kompiliert werden. Wenn eine x86-App über einen Treiber verfügt, muss dieser Treiber für ARM64 neu kompiliert werden. Die x86-App kann unter der Emulation problemlos ausgeführt werden. der Treiber muss jedoch für ARM64 neu kompiliert werden, und jede APP-Umgebung, die vom Treiber abhängt, ist nicht verfügbar. Weitere Informationen zum Kompilieren des Treibers für ARM64 finden Sie unter [Erstellen von ARM64-Treibern mit dem WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers).

## <a name="shell-extensions"></a>-Shellerweiterungen 
Apps, die versuchen, Windows-Komponenten zu verbinden oder Ihre DLLs in Windows-Prozesse zu laden, müssen diese DLLs neu kompilieren, damit Sie der Architektur des Systems entsprechen. d. h. ARM64. Diese werden in der Regel von Eingabemethoden-Editoren (IMEs), Hilfstechnologien und Shellerweiterungs-Apps verwendet (z. b. zum Anzeigen von Cloud-Speicher Symbolen im Explorer oder einem Kontextmenü mit der rechten Maustaste). Weitere Informationen zum erneuten Kompilieren Ihrer Apps oder DLLs in ARM64 finden Sie in der [frühen Vorschau von Visual Studio-Unterstützung für Windows 10 auf Arm Development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) Blog Post. 

## <a name="debugging"></a>Debuggen
Wenn Sie das Verhalten Ihrer APP ausführlicher untersuchen möchten, finden Sie unter [Debugging auf Arm](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) Weitere Informationen zu Tools und Strategien für das Debuggen auf Arm.

## <a name="virtual-machines"></a>Virtual Machines
Die Windows Hypervisor-Plattform wird auf der Qualcomm Snapdragon 835 Mobile PC-Plattform nicht unterstützt. Daher können virtuelle Computer mit Hyper-V nicht ausgeführt werden. Wir investieren weiterhin in diese Technologien für zukünftige Qualcomm-Chipsätze. 

## <a name="dynamic-code-generation"></a>Dynamische Code Generierung
X86-Desktop-Apps werden auf ARM64 durch das System emuliert, das ARM64-Anweisungen zur Laufzeit erzeugt. Dies bedeutet, dass eine x86-Desktop-App das generieren oder Ändern dynamischer Code im Prozess verhindert, dass diese APP nicht als x86 auf ARM64 ausgeführt werden kann. 

Dies ist eine Sicherheits Entschärfung, die einige apps bei Ihrem Prozess mithilfe der [setprocessentschärationpolicy](https://docs.microsoft.com/windows/desktop/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy) -API mit dem- `ProcessDynamicCodePolicy` Flag aktivieren. Diese Entschärfungs Richtlinie muss deaktiviert werden, damit Sie auf ARM64 erfolgreich als x86-Prozess ausgeführt werden kann. 
