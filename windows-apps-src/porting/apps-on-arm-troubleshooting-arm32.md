---
title: Problembehandlung ARM32 UWP-apps
description: Häufige Probleme mit ARM32-Apps bei der Ausführung auf Arm und deren Behebung.
ms.date: 01/03/2019
ms.topic: article
keywords: Windows 10 s, immer verbunden, ARM32-apps auf arm, Windows 10 auf arm, Problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: 60c7a129844d69d18287ea7885a0acaf01f1f625
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171224"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Problembehandlung für Arm UWP-apps

Wenn Ihre ARM32-oder ARM64-UWP-App auf Arm nicht ordnungsgemäß funktioniert, finden Sie hier einige Anleitungen, die Ihnen helfen können.

>[!NOTE]
> Wenn Sie Ihre UWP-Anwendung erstellen möchten, um die ARM64-Plattform nativ zu erreichen, benötigen Sie Visual Studio 2017, Version 15,9 oder höher oder Visual Studio 2019. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


## <a name="common-issues"></a>Häufige Probleme
Im folgenden finden Sie einige häufige Probleme, die bei der Problembehandlung von ARM32 und ARM64-apps zu berücksichtigen sind.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Verwenden von nur Windows 10 Mobile-APIs auf ARM-basierten Prozessoren
Bei Arm-apps treten möglicherweise Probleme auf, wenn Mobile APIs verwendet werden (z. b. **hardwarebuttons**). Um dies zu vermeiden, können Sie dynamisch erkennen, ob Ihre APP unter Windows 10 Mobile ausgeführt wird, bevor Sie diese APIs aufrufen. Befolgen Sie die Anleitungen im Blogbeitrag [dynamisches erkennen von Features mit API-Verträgen](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Einschließen von Abhängigkeiten, die nicht von UWP-apps unter
Universelle Windows-Plattform-Apps (UWP), die nicht ordnungsgemäß mit Visual Studio und dem UWP SDK erstellt wurden, verfügen möglicherweise über Abhängigkeiten von Betriebssystemkomponenten, die für Arm-apps in einem ARM64-System nicht verfügbar sind. Beispiele für diese Abhängigkeiten sind:

- Es wird erwartet, dass Teile der .NET Framework verfügbar sind.
- Referenzieren von .NET-Komponenten von Drittanbietern, die nicht mit UWP kompatibel sind.

Diese Probleme können durch folgende Probleme gelöst werden: Entfernen der nicht verfügbaren Abhängigkeiten und Neuerstellen der APP mit den neuesten Microsoft Visual Studio und UWP SDK-Versionen. oder als letztes Mittel, wenn Sie die Arm-App aus dem Microsoft Store entfernen, sodass die x86-Version der APP (falls verfügbar) auf die PCs der Benutzer heruntergeladen wird.

Weitere Informationen zu .NET-APIs, die für UWP-apps verfügbar sind, finden Sie unter [.net für UWP-apps](/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Kompilieren einer APP mit einer älteren Version von Visual Studio und SDK
Wenn Probleme auftreten, stellen Sie sicher, dass Sie die neuesten Versionen von Microsoft Visual Studio und die Windows SDK verwenden, um Ihre APP zu kompilieren. Apps, die mit einer früheren Version von Visual Studio und dem SDK kompiliert wurden, haben möglicherweise Probleme, die in späteren Versionen behoben wurden.

## <a name="debugging"></a>Debuggen
Sie können vorhandene Tools zum Entwickeln von Apps für die ARM-Plattform verwenden. Im folgenden finden Sie einige hilfreiche Ressourcen.

- Visual Studio 15,5 Preview 1 und höher unterstützt die Ausführung von ARM32-Apps mithilfe des universellen Authentifizierungsmodus. Dadurch werden automatisch die erforderlichen remotedebuggingtools gestartet.
- Weitere Informationen zu Tools und Strategien für das Debuggen auf Arm finden Sie unter [Debugging auf ARM64](/windows-hardware/drivers/debugger/debugging-arm64) .