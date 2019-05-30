---
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: Dieser Artikel enthält eine Liste mit den DeviceInformation-Eigenschaften für Audiogeräte.
title: Audiogeräte-Informationseigenschaften
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 97b6bd0c3567c00902a9528d54e6417f41ac66ed
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359097"
---
# <a name="audio-device-information-properties"></a>Audiogeräte-Informationseigenschaften

Dieser Artikel enthält eine Liste mit den Geräteinformationseigenschaften für Audiogeräte. Unter Windows sind jedem Hardwaregerät [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Eigenschaften mit ausführlichen Geräteinformationen zugeordnet, auf die Sie zurückgreifen können, wenn Sie bestimmte Geräteinformationen benötigen oder eine Geräteauswahl erstellen möchten. Allgemeine Informationen zum Aufzählen von Geräten unter Windows finden Sie unter [Auflisten von Geräten](../devices-sensors/enumerate-devices.md) und [Geräteinformationseigenschaften](../devices-sensors/device-information-properties.md).


|Name|Typ|Beschreibung|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Double|Gibt die Empfindlichkeit des Mikrofons in Dezibel relativ zu Full-Scale-Einheiten (dB FS) an.|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRatioInDb**|Double|Gibt für das Mikrofon das Signal-Rausch-Verhältnis (SNR) in Dezibeleinheiten (dB) an.|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Boolesch|Gibt an, ob das Audiogerät die Verarbeitung von Sprache unterstützt.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Boolesch|Gibt an, ob das Audiogerät die Verarbeitung von Rohdaten unterstützt.|
|**System.Devices.MicrophoneArray.Geometry**|unsigned char[]|Geometriedaten für ein Mikrofonarray.|

## <a name="related-topics"></a>Verwandte Themen

* [Auflisten von Geräten](../devices-sensors/enumerate-devices.md)
* [Eigenschaften für Geräte](../devices-sensors/device-information-properties.md)
* [Erstellen Sie eine Geräteauswahl](../devices-sensors/build-a-device-selector.md)
* [Medienwiedergabe](media-playback.md)




