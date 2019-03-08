---
title: Bereitstellen von Strichcodescanner-Profilen mit MDM
description: Strichcodescanner-Profile können mit einem MDM-Server bereitgestellt werden.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dbcaa683e2c7a2bb18d88fcba03e10fa951d4459
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626155"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Bereitstellen von Strichcodescanner-Profilen mit MDM

**Beachten Sie**  dieses Feature erfordert Windows 10 Mobile oder höher.

Strichcodescanner-Profile können mit einem MDM-Server bereitgestellt werden. Verwenden Sie zum Bereitstellen der Profile *OemProfile* in die [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) , platzieren Sie sie in der \\Daten\\SharedData\\OEM\\ Öffentliche\\Profilordner. Diese Scannerprofile können dann durch Treiberhersteller verwendet werden, um Einstellungen zu konfigurieren, die nicht auf der API-Oberfläche verfügbar sind.

Microsoft definiert die Einzelheiten eines Scannerprofils und seine Implementierung nicht.

## <a name="related-topics"></a>Verwandte Themen
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [Barcode Scanner-geräteunterstützung](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)