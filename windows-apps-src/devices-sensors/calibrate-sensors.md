---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Kalibrieren von Sensoren
description: Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein.
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0ef2c1cfbe949e5f850a494e4f1ed9c79faf6050
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168574"
---
# <a name="calibrate-sensors"></a>Kalibrieren von Sensoren


**Wichtige APIs**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**Windows. Devices. Sensors. Custom**](/uwp/api/Windows.Devices.Sensors.Custom)

Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein. Falls für Ihr Gerät eine Kalibrierung erforderlich ist, kann die [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy)-Aufzählung helfen, die nächsten Schritte festzulegen.

## <a name="when-to-calibrate-the-magnetometer"></a>Zeitpunkt für die Kalibrierung des Magnetometers

Die [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy)-Aufzählung bietet vier Werte, mit deren Hilfe Sie bestimmen können, ob das Gerät, auf dem Ihre App ausgeführt wird, kalibriert werden muss. Wenn ein Gerät kalibriert werden muss, sollten Sie den Benutzer über die Notwendigkeit einer Kalibrierung informieren. Fordern Sie den Benutzer jedoch nicht zu häufig auf, eine Kalibrierung durchzuführen. Dies sollte nicht öfter als einmal alle 10 Minuten erfolgen.

| Wert           | BESCHREIBUNG    |
| ----------------- | ------------------- |
| **Unbekannt**     | Der Sensor Treiber konnte die aktuelle Genauigkeit nicht melden. Dies bedeutet nicht notwendigerweise, dass das Gerät falsch kalibriert ist. Es ist Aufgabe Ihrer App, die geeigneten Schritte festzulegen, wenn **Unknown** zurückgegeben wird. Falls Ihre App von exakten Sensorwerten abhängig ist, sollten Sie den Benutzer auffordern, das Gerät zu kalibrieren. |
| **Unzuverlässigen**  | Im Magnetometer ist zurzeit ein hoher Grad an Ungenauigkeit vorhanden. Wenn dieser Wert zuerst zurückgegeben wird, sollten Apps immer zu einer Kalibrierung durch den Benutzer auffordern. |
| **Ungefähr** | Die Daten sind für einige Anwendungen genau genug. Eine Virtual-Reality-App, die lediglich wissen muss, ob der Benutzer das Gerät nach oben/unten oder links/rechts bewegt hat, kann ohne Kalibrierung fortgesetzt werden. Apps, die einen absoluten Kurs benötigen, z. B. eine Navigations-App, die wissen muss, in welche Richtung Sie fahren, um Sie zu führen, müssen eine Kalibrierung anfordern. |
| **Hoch**        | Die Daten sind genau. Es ist keine Kalibrierung erforderlich. Dies gilt auch für Apps, die einen absoluten Kurs benötigen, wie Augmented-Reality- oder Navigations-Apps. |