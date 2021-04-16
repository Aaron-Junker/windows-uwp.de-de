---
ms.openlocfilehash: e4f09efa1bd6d6884e9a32b46aff1bf586a16e60
ms.sourcegitcommit: 6cd970686d1ea7176b7e6651f349a14551709820
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559398"
---
> [!IMPORTANT]
> .NET Framework Desktop-Apps, die weiterhin packages.config verwenden, müssen zu PackageReference migriert werden, da andernfalls nicht ordnungsgemäß auf die Windows 10 SDKs verwiesen wird. Klicken Sie in Ihrem Projekt mit der rechten Maustaste auf "Verweise", und klicken Sie auf "Migrate packages.config to PackageReference".
> 
> WPF-Apps für .NET Core 3.0 müssen auf .NET Core 3.1 aktualisiert werden, andernfalls fehlen die APIs.
> 
> .NET 5-Apps müssen [eines der Windows 10 TFMs verwenden,](https://docs.microsoft.com/dotnet/standard/frameworks#how-to-specify-a-target-framework)andernfalls fehlen die Popup-APIs zum Senden und Verwalten von APIs wie `Show()` . Legen Sie tfm auf `net5.0-windows10.0.17763.0` oder höher fest.
