---
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: Dieser Artikel enthält eine Liste mit den DeviceInformation-Eigenschaften für Audiogeräte.
title: Audiogeräte-Informationseigenschaften
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 443bcc3c0280aca85de31d8c9f3704302432cb76
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627175"
---
# <a name="audio-device-information-properties"></a>Audiogeräte-Informationseigenschaften

Dieser Artikel enthält eine Liste mit den Geräteinformationseigenschaften für Audiogeräte. Unter Windows sind jedem Hardwaregerät [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Eigenschaften mit ausführlichen Geräteinformationen zugeordnet, auf die Sie zurückgreifen können, wenn Sie bestimmte Geräteinformationen benötigen oder eine Geräteauswahl erstellen möchten. Allgemeine Informationen zum Aufzählen von Geräten unter Windows finden Sie unter [Auflisten von Geräten](../devices-sensors/enumerate-devices.md) und [Geräteinformationseigenschaften](../devices-sensors/device-information-properties.md).


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




