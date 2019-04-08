---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Kalibrieren von Sensoren
description: Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein.
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7a93d59a00630c240e74049a9fd98d50f285b0dd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634675"
---
# <a name="calibrate-sensors"></a>Kalibrieren von Sensoren


**Wichtige APIs**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein. Falls für Ihr Gerät eine Kalibrierung erforderlich ist, kann die [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552)-Aufzählung helfen, die nächsten Schritte festzulegen.

## <a name="when-to-calibrate-the-magnetometer"></a>Zeitpunkt für die Kalibrierung des Magnetometers

Die [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552)-Aufzählung bietet vier Werte, mit deren Hilfe Sie bestimmen können, ob das Gerät, auf dem Ihre App ausgeführt wird, kalibriert werden muss. Wenn ein Gerät kalibriert werden muss, sollten Sie den Benutzer über die Notwendigkeit einer Kalibrierung informieren. Fordern Sie den Benutzer jedoch nicht zu häufig auf, eine Kalibrierung durchzuführen. Dies sollte nicht öfter als einmal alle 10 Minuten erfolgen.

| Wert           | Beschreibung    |
| ----------------- | ------------------- |
| **Unbekannt**     | Der Sensortreiber konnte die aktuelle Genauigkeit nicht ermitteln. Dies bedeutet nicht notwendigerweise, dass das Gerät falsch kalibriert ist. Es ist Aufgabe Ihrer App, die geeigneten Schritte festzulegen, wenn **Unknown** zurückgegeben wird. Falls Ihre App von exakten Sensorwerten abhängig ist, sollten Sie den Benutzer auffordern, das Gerät zu kalibrieren. |
| **Unzuverlässige**  | Das Magnetometer ist aktuell hochgradig ungenau. Wenn dieser Wert zuerst zurückgegeben wird, sollten Apps immer zu einer Kalibrierung durch den Benutzer auffordern. |
| **Ungefähre** | Die Daten sind für bestimmt Anwendungen genau genug. Eine Virtual-Reality-App, die lediglich wissen muss, ob der Benutzer das Gerät nach oben/unten oder links/rechts bewegt hat, kann ohne Kalibrierung fortgesetzt werden. Apps, die einen absoluten Kurs benötigen, z. B. eine Navigations-App, die wissen muss, in welche Richtung Sie fahren, um Sie zu führen, müssen eine Kalibrierung anfordern. |
| **Hohe**        | Die Daten sind genau. Es ist keine Kalibrierung erforderlich. Dies gilt auch für Apps, die einen absoluten Kurs benötigen, wie Augmented-Reality- oder Navigations-Apps. |