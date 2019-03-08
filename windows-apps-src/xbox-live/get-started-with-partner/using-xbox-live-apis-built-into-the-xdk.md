---
title: Xbox Live-APIs verwenden, integriert der xdk-Version
description: Erfahren Sie, wie die integrierten Xbox Live-APIs in Ihrem Projekt Xbox Developer Kit (xdk-Version) zu verwenden.
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c762dd8abbbc80948d232610e4123b6e4893936d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598405"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>Xbox Live-APIs verwenden, integriert der xdk-Version

1. Klicken Sie mit der rechten Maustaste auf das Projekt in Visual Studio, und wählen Sie "Referenzen".
1. Wählen Sie "Neuen Verweis hinzufügen"
1. Klicken Sie auf "Durango. <build number>" und "Erweiterungen" im linken Bereich
1. Wählen Sie in der Mitte entweder aus:
- Wenn Sie die WinRT-XSAPI-API verwenden möchten, wählen Sie "Xbox-Dienste-API"
- Wenn Sie die C++-XSAPI-API verwenden möchten, wählen Sie "Xbox-Dienste-API-Cpp"
1. Auf "OK" klicken

Hinweis: Wenn das Buildsystem Props-Dateien nicht unterstützt, müssen Sie manuell die Präprozessordefinitionen und -Bibliotheken wie in hinzufügen `%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`
