---
title: Bereitstellen von Strichcodescanner-Profilen mit MDM
description: Strichcodescanner-Profile können mit einem MDM-Server bereitgestellt werden.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e92c4c715608f9ae36adb3a67beec8002083542f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370279"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Bereitstellen von Strichcodescanner-Profilen mit MDM

**Beachten Sie**  dieses Feature erfordert Windows 10 Mobile oder höher.

Strichcodescanner-Profile können mit einem MDM-Server bereitgestellt werden. Verwenden Sie zum Bereitstellen der Profile *OemProfile* in die [EnterpriseExtFileSystem CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp) , platzieren Sie sie in der \\Daten\\SharedData\\OEM\\ Öffentliche\\Profilordner. Diese Scannerprofile können dann durch Treiberhersteller verwendet werden, um Einstellungen zu konfigurieren, die nicht auf der API-Oberfläche verfügbar sind.

Microsoft definiert die Einzelheiten eines Scannerprofils und seine Implementierung nicht.

## <a name="related-topics"></a>Verwandte Themen
- [EnterpriseExtFileSystem CSP](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Barcode Scanner-geräteunterstützung](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)