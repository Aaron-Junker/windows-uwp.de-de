---
title: Bereitstellen von Barcode Scanner-Profilen mit MDM
description: Barcode Scanner-Profile können mit einem MDM-Server bereitgestellt werden.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1bac497ec52dd0897af8c6c606bcdc041007c579
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173274"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Bereitstellen von Barcode Scanner-Profilen mit MDM

**Hinweis**    Für dieses Feature ist Windows 10 Mobile oder höher erforderlich.

Barcode Scanner-Profile können mit einem MDM-Server bereitgestellt werden. Verwenden Sie zum Bereitstellen der Profile *oemprofile* im [enterpricextfile System-CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp) , um Sie in den \\ Ordner " \\ Public shareddata \\ OEM \\ Public \\ profile" zu platzieren. Diese Scannerprofile können von Treiber Herstellern dann verwendet werden, um Einstellungen zu konfigurieren, die nicht über die API-Oberfläche verfügbar gemacht werden.

Microsoft definiert nicht die Besonderheiten eines Scanner-Profils oder seine Implementierung.

## <a name="related-topics"></a>Zugehörige Themen
- [EnterpriseExtFileSystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Geräte Unterstützung für Barcode Scanner](./pos-device-support.md#barcode-scanner)