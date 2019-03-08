---
title: Verwenden Sie das Xbox Live-NuGet-Paket mit der xdk-Version
description: Erfahren Sie, wie Sie das Xbox Live-API-NuGet-Paket zu verwenden, um xdk-Version von Titeln zu entwickeln.
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, einen NuGet
ms.localizationpriority: medium
ms.openlocfilehash: c955ca42c09075e5125683588c335cfa47443f00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655035"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>Verwenden Sie das Xbox Live-API-NuGet-Paket, um xdk-Version Titel zu entwickeln.

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.  Stellen Sie sicher, dass Sie die aktuelle NuGet Package Manager installiert haben
1.  Überprüfen der aktuellen Version:
    - Klicken Sie auf der Menüleiste auf Extras -> Erweiterungen und Updates.
    - Suchen Sie auf der Registerkarte "installiert" nach `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  So aktualisieren Sie die aktuelle Version:
    - Klicken Sie auf der Menüleiste auf Extras -> Erweiterungen und Updates.
    - Klicken Sie unter der Updates -> Registerkarte "Visual Studio Gallery", wählen Sie `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2.  Verweis auf das Projekt hinzufügen
1.  Verweis auf das Projekt hinzufügen
    1.  Klicken Sie mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie "NuGet-Pakete verwalten"
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  Suchen Sie nach `Xbox Live` und wählen Sie das entsprechende Paket, und klicken Sie auf `Install`.
  - Die Xbox-API ist in Versionen, für die UWP und xdk-Version und für C++ und WinRT.  
  - Wählen Sie zwischen `Microsoft.Xbox.Live.SDK.*.UWP` und `Microsoft.Xbox.Live.SDK.*.XboxOneXDK`.  `XboxOneXDK` ist für ID@Xbox und verwaltete-Entwickler, die die Xbox One XDK verwenden.  `UWP` ist für die UWP Spiele für PC, die Xbox One oder Windows Phone ausgeführt werden können.  Erfahren Sie mehr über das Ausführen von UWP für Xbox One an [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - Wählen Sie zwischen `Microsoft.Xbox.Live.SDK.Cpp.*` und `Microsoft.Xbox.Live.SDK.WinRT.*`. `Cpp` ist für die Xbox Live-APIs mithilfe C++ Spiele-Engines.  `WinRT` ist für die Spiele-Engines, die mit C++, C#, oder der Xbox Live-APIs mit Javascript.  Wenn Sie WinRT mit einem C++-Modul verwenden, verwenden Sie C++ / CX Hüten (^) verwendet.  `Cpp` ist die empfohlene API für C++-Spiele-Engines verwendet.    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. Nach dem Akzeptieren der Lizenz TOS warten Sie, bis das Paket wurde erfolgreich hinzugefügt.  Dieses Protokoll in der Paket-Manager-Ausgabefenster sollte angezeigt werden:

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3.  Fügen Sie optional header
* Für `Microsoft.Xbox.Live.SDK.Cpp.*` -basierter Projekte `#include <xsapi\services.h>` in Ihrem Projekt die Quelle.
* Für `Microsoft.Xbox.Live.SDK.WinRT.*` -basierter Projekte, nicht erforderlich, alle Header enthalten.   
