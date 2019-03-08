---
title: Einschränkungen von Apps und Oberflächen auf ARM
description: Problembehandlungsschritte für Apps, die auf ARM nicht ordnungsgemäß funktionieren.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, einschränkungen, windows 10 auf ARM
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/en-us/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: 42a2866642d11d1d97cc7b0b663486723499a5cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633455"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>Einschränkungen von Apps und Oberflächen auf ARM
Windows 10 auf ARM weist die folgenden notwendigen Einschränkungen auf:

- **Es werden nur ARM64-Treiber unterstützt**. Wie bei allen Architekturen, müssen Kernelmodustreiber, [User-Mode Driver Framework (UMDF)](https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/overview-of-the-umdf)-Treiber und Druckertreibern entsprechend der Architektur des Betriebssystems kompiliert werden. Während das ARM-Betriebssystem die Möglichkeit bietet, x86-Apps im Benutzermodus zu emulieren, werden Treiber, die für andere Architekturen (z. B. x64 oder x86) implementiert sind, derzeit nicht emuliert und werden daher auf dieser Plattform nicht unterstützt. Alle Apps, die mit ihren eigenen benutzerdefinierten Treibern funktionieren, müssten auf ARM64 portiert werden. In eingeschränkten Szenarien kann die App als x86 in einer Emulation ausgeführt werden, der Treiberteil der App muss jedoch auf ARM64 portiert werden. Weitere Informationen zum Kompilieren des Treibers für ARM64 finden Sie unter [Entwickeln von ARM64-Treibern mit WDK](/windows-hardware/drivers/develop/building-arm64-drivers).

- **x64-Apps werden nicht unterstützt**. Windows 10 auf ARM unterstützt keine Emulation von x64-Apps.

- **Bestimmte Spiele funktionieren nicht**. Spiele und Apps, die eine Version von OpenGL höher als 1.1 verwenden oder hardwarebeschleunigte OpenGL erfordern, funktionieren nicht. Darüber hinaus werden Spiele, die auf „Anti-Cheat”-Treiber angewiesen sind, auf dieser Plattform nicht unterstützt.

- **Apps, die die Windows-Benutzeroberfläche anpassen, funktionieren möglicherweise nicht ordnungsgemäß**. Systemeigene Komponenten des Betriebssystems können nicht systemeigene Komponenten nicht laden. Zu den Beispielen für Apps, die dies tun, gehören Eingabemethoden-Editoren (IMEs), Hilfstechnologien und Apps zur Cloudspeicherung. IMEs und Hilfstechnologien greifen oft für viele ihrer App-Funktionen in den Eingabestapel ein. Cloudspeicher-Apps verwenden häufig Shell-Erweiterungen (z. B. Symbole im Explorer und Zusätze zu Kontextmenüs). Ihre Shell-Erweiterungen können fehlschlagen, und wenn der Fehler nicht ordnungsgemäß behandelt wird, funktioniert die App möglicherweise überhaupt nicht.

- **Apps, die davon ausgehen, dass auf allen ARM-basierten Geräten eine mobile Version von Windows ausgeführt wird, funktionieren möglicherweise nicht ordnungsgemäß**. Apps, die diese Annahme treffen, können in der falschen Ausrichtung angezeigt werden, ein unerwartetes UI-Layout oder Rendering aufweisen oder nicht gestartet werden, wenn sie versuchen, nur mobile APIs aufzurufen, ohne zuvor die Vertragsverfügbarkeit getestet zu haben.

- **Die Windows-Hypervisor-Plattform wird auf ARM nicht unterstützt**. Das Ausführen von virtuellen Maschinen mit Hyper-V auf einem ARM-Gerät funktioniert nicht.

In der folgenden Tabelle sind einige allgemeine Probleme und Vorschläge zu ihrer Behebung aufgelistet.

|Problem|Lösung|
|-----|--------|
| Ihre App verwendet einen Treiber, der nicht für ARM entwickelt wurde. | Kompilieren Sie Ihren x86-Treiber neu zu ARM64. Siehe [Entwickeln von ARM64-Treibern mit WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers). |
| Ihre App ist nur für x64 verfügbar. | Wenn Sie für den Microsoft Store entwickeln, übermitteln Sie eine ARM-Version Ihrer App. Weitere Informationen finden Sie unter [App-Paketarchitekturen](../packaging/device-architecture.md). Wenn Sie ein Win32-Entwickler sind, verteilen Sie eine x86-Version Ihrer App. |
| Ihre App verwendet eine Version von OpenGL höher als 1.1 oder erfordert hardwarebeschleunigte OpenGL. | x86 Apps mit DirectX 9, DirectX 10, DirectX 11 und DirectX 12 funktionieren auf ARM. Weitere Informationen finden Sie unter [DirectX-Grafiken und -Spiele](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx). |
| Ihre x86-App funktioniert nicht wie erwartet. | Versuchen Sie es mit der Problembehandlung für die Programmkompatibilität, indem Sie die Schritte unter [Problembehandlung für die Programmkompatibilität auf ARM](apps-on-arm-program-compat-troubleshooter.md) befolgen. Weitere Schritte zur Problembehandlung finden Sie im Artikel [Problembehandlung bei x86 Apps auf ARM](apps-on-arm-troubleshooting-x86.md). |
| Ihre x86-App erkennt nicht, dass sie auf ARM ausgeführt wird. | Verwenden Sie [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) um festzustellen, ob Ihre App auf ARM ausgeführt wird. |
| Ihre UWP ARM32-App funktioniert nicht wie erwartet. | Unter [Problembehandlung bei ARM32-Apps auf ARM](apps-on-arm-troubleshooting-arm32.md) erfahren Sie, wie Ihre App auf ARM richtig einsetzen können. |
