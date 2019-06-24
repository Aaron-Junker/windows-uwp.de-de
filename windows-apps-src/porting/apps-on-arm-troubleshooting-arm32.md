---
title: Problembehandlung bei ARM32 UWP-Apps
description: Häufig auftretende Probleme mit ARM32-Apps bei der Ausführung auf ARM, und wie diese Probleme behoben werden können.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, always connected, ARM32-Apps auf ARM, windows 10 auf ARM, problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: b93a4a3bdf4a6abd12a0df8bfc1871cb38647f63
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319753"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Problembehandlung bei ARM UWP-apps

Wenn Ihre ARM32 oder ARM64-UWP-app nicht ordnungsgemäß auf ARM funktioniert, ist hier einige Hinweise, die helfen können.

>[!NOTE]
> Um Ihrer UWP-Anwendung für die ARM64 systemintern Zielplattform erstellen zu können, müssen Sie Visual Studio 2017 Version 15.9 oder höher verfügen. Weitere Informationen finden Sie unter [in diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

## <a name="common-issues"></a>Allgemeine Probleme
Hier sind einige häufig auftretende Probleme zu bedenken, bei der Problembehandlung ARM32 und ARM64-apps.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Verwenden von Windows 10 Nur-Mobil-APIs auf ARM-basierten Prozessoren
ARM-apps möglicherweise treten Probleme auf, wenn nur Mobile-APIs verwenden (z. B. **ob "hardwarebuttons"** ). Um dies zu verringern, können Sie dynamisch erkennen, ob Ihre App auf Windows 10 Mobile ausgeführt wird, bevor Sie diese APIs aufrufen. Befolgen Sie die Anleitungen im Blogbeitrag [Dynamisches Erkennen von Features mithilfe von API-Verträgen](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Einschließen von Abhängigkeiten, die von UWP-Apps nicht unterstützt werden
Universelle Windows-Plattform (UWP)-apps, die ordnungsgemäß mit Visual Studio und die UWP-SDK erstellt werden nicht möglicherweise Abhängigkeiten auf Betriebssystemkomponenten, die nicht auf ARM-apps, die auf einem ARM64 System zur Verfügung stehen. Beispiele für diese Abhängigkeiten sind:

- Zu erwarten, dass Teile vom .NET Framework verfügbar sind.
- Auf .NET-Komponenten von Drittanbietern zu verweisen, die nicht mit UWP kompatibel sind.

Durch diese Probleme gelöst werden können: entfernen die Abhängigkeiten nicht verfügbar ist und die Neuentwicklung der app mit den neuesten Versionen von Microsoft Visual Studio und UWP-SDK; oder als letztes Mittel, entfernen die ARM-app aus dem Microsoft Store, damit die X86 Version der app (falls vorhanden) wird auf PCs von Benutzern heruntergeladen.

Weitere Informationen zu .NET-APIs, die für UWP-Apps verfügbar sind, finden Sie unter [.NET für UWP-Apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Kompilieren einer App mit einer älteren Version von Visual Studio und SDK
Wenn Probleme auftreten, stellen Sie sicher, dass Sie die neueste Version von Microsoft Visual Studio und Windows SDK zum Kompilieren Ihrer App verwenden. Apps, die mit einer früheren Version von Visual Studio und SDK kompiliert wurden, verursachen möglicherweise Probleme, die in späteren Versionen behoben wurden.

## <a name="debugging"></a>Debuggen
Sie können vorhandene Tools verwenden, für die Entwicklung von apps für die ARM-Plattform. Hier finden Sie einige hilfreiche Ressourcen.

- Visual Studio 15.5 Preview 1 und höher unterstützt das Ausführen von ARM32-Apps mithilfe des universellen Authentifizierungsmodus. Dies bootet automatisch die erforderlichen Remotedebuggingtools.
- Siehe [Debugging auf ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64), um mehr über Tools und Strategien zum Debuggen auf ARM zu erfahren.
