---
title: Problembehandlung bei ARM32 UWP-Apps
description: Häufig auftretende Probleme mit ARM32-Apps bei der Ausführung auf ARM, und wie diese Probleme behoben werden können.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, always connected, ARM32-Apps auf ARM, windows 10 auf ARM, problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: 6213c8c69695d160d4e6fa362aa7aa322a0326fd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683943"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Problembehandlung für Arm UWP-apps

Wenn Ihre ARM32-oder ARM64-UWP-App auf Arm nicht ordnungsgemäß funktioniert, finden Sie hier einige Anleitungen, die Ihnen helfen können.

>[!NOTE]
> Wenn Sie Ihre UWP-Anwendung erstellen möchten, um die ARM64-Plattform nativ zu erreichen, benötigen Sie Visual Studio 2017, Version 15,9 oder höher oder Visual Studio 2019. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


## <a name="common-issues"></a>Allgemeine Probleme
Im folgenden finden Sie einige häufige Probleme, die bei der Problembehandlung von ARM32 und ARM64-apps zu berücksichtigen sind.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Verwenden von Windows 10 Nur-Mobil-APIs auf ARM-basierten Prozessoren
Bei Arm-apps treten möglicherweise Probleme auf, wenn Mobile APIs verwendet werden (z. b. **hardwarebuttons**). Um dies zu verringern, können Sie dynamisch erkennen, ob Ihre App auf Windows 10 Mobile ausgeführt wird, bevor Sie diese APIs aufrufen. Befolgen Sie die Anleitungen im Blogbeitrag [Dynamisches Erkennen von Features mithilfe von API-Verträgen](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Einschließen von Abhängigkeiten, die von UWP-Apps nicht unterstützt werden
Universelle Windows-Plattform-Apps (UWP), die nicht ordnungsgemäß mit Visual Studio und dem UWP SDK erstellt wurden, verfügen möglicherweise über Abhängigkeiten von Betriebssystemkomponenten, die für Arm-apps in einem ARM64-System nicht verfügbar sind. Beispiele für diese Abhängigkeiten sind:

- Zu erwarten, dass Teile vom .NET Framework verfügbar sind.
- Auf .NET-Komponenten von Drittanbietern zu verweisen, die nicht mit UWP kompatibel sind.

Diese Probleme können durch folgende Probleme gelöst werden: Entfernen der nicht verfügbaren Abhängigkeiten und Neuerstellen der APP mit den neuesten Microsoft Visual Studio und UWP SDK-Versionen. oder als letztes Mittel, wenn Sie die Arm-App aus dem Microsoft Store entfernen, sodass die x86-Version der APP (falls verfügbar) auf die PCs der Benutzer heruntergeladen wird.

Weitere Informationen zu .NET-APIs, die für UWP-Apps verfügbar sind, finden Sie unter [.NET für UWP-Apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Kompilieren einer App mit einer älteren Version von Visual Studio und SDK
Wenn Probleme auftreten, stellen Sie sicher, dass Sie die neueste Version von Microsoft Visual Studio und Windows SDK zum Kompilieren Ihrer App verwenden. Apps, die mit einer früheren Version von Visual Studio und SDK kompiliert wurden, verursachen möglicherweise Probleme, die in späteren Versionen behoben wurden.

## <a name="debugging"></a>Debuggen
Sie können vorhandene Tools zum Entwickeln von Apps für die ARM-Plattform verwenden. Hier finden Sie einige hilfreiche Ressourcen.

- Visual Studio 15.5 Preview 1 und höher unterstützt das Ausführen von ARM32-Apps mithilfe des universellen Authentifizierungsmodus. Dies bootet automatisch die erforderlichen Remotedebuggingtools.
- Siehe [Debugging auf ARM64](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64), um mehr über Tools und Strategien zum Debuggen auf ARM zu erfahren.
