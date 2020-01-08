---
title: Bereitstellen von Strichcodescanner-Profilen mit MDM
description: Strichcodescanner-Profile können mit einem MDM-Server bereitgestellt werden.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f537833385582678b215804cac9a16002618c7e4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684825"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Bereitstellen von Strichcodescanner-Profilen mit MDM

**Hinweis**  für dieses Feature ist Windows 10 Mobile oder höher erforderlich.

Strichcodescanner-Profile können mit einem MDM-Server bereitgestellt werden. Verwenden Sie zum Bereitstellen der Profile *oemprofile* im [enterpricextfile System-CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp) , um Sie im Ordner \\Data\\shareddata\\OEM\\Public\\Profile zu platzieren. Diese Scannerprofile können dann durch Treiberhersteller verwendet werden, um Einstellungen zu konfigurieren, die nicht auf der API-Oberfläche verfügbar sind.

Microsoft definiert die Einzelheiten eines Scannerprofils und seine Implementierung nicht.

## <a name="related-topics"></a>Zugehörige Themen
- [Enterpricextfile System-CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Geräte Unterstützung für Barcode Scanner](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)