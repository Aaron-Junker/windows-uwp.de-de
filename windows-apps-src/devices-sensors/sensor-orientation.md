---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: Sensorausrichtung
description: Sensordaten der Klassen Accelerometer, Gyrometer, Compass, Inclinometer und OrientationSensor sind durch ihre Referenzachsen definiert. Diese Achsen werden durch das Querformat des Geräts bestimmt und drehen sich mit dem Gerät, wenn es vom Benutzer gedreht wird.
ms.date: 05/24/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a4c7f1ad75e1e0544486049f9bd721d8a82edf03
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "8713242"
---
# <a name="sensor-orientation"></a>Sensorausrichtung


**Wichtige APIs**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Sensordaten der Klassen [**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687), [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718), [**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705), [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) und [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) sind durch ihre Referenzachsen definiert. Diese Achsen werden durch den Referenzrahmen des Geräts bestimmt und drehen sich mit dem Gerät, wenn es vom Benutzer gedreht wird. Falls Ihre App die automatische Drehung unterstützt und sie sich selbst entsprechend der Drehung des Geräts neu ausrichtet, müssen Sie die Sensordaten für die Drehung anpassen, bevor Sie sie verwenden.

## <a name="display-orientation-vs-device-orientation"></a>Bildschirmausrichtung und Geräteausrichtung

Um die Referenzachse für Sensoren begreifen zu können, muss zwischen Bildschirmausrichtung und Geräteausrichtung unterschieden werden. Bei der Bildschirmausrichtung handelt es sich um die Richtung, in der Text und Bilder auf dem Bildschirm angezeigt werden, wohingegen es sich bei der Geräteausrichtung um die physische Position des Geräts handelt. Im folgenden Bild sind sowohl das Gerät als auch die Anzeige im **Querformat** ausgerichtet. (Beachten Sie, dass die abgebildeten Sensorachsen nur für Geräte gelten, die für das Querformat ausgelegt sind).

![Bildschirm- und Geräteausrichtung im Querformat](images/sensor-orientation-a.PNG)

Im folgenden Bild sind sowohl Bildschirm- als auch Geräteausrichtung im **LandscapeFlipped**-Format.

![Bildschirm- und Geräteausrichtung im LandscapeFlipped-Format](images/sensor-orientation-b.PNG)

Das nächste Bild zeigt die Anzeigeausrichtung im Querformat und die Geräteausrichtung im LandscapeFlipped-Format.

![Bildschirmausrichtung im Querformat und Geräteausrichtung im LandscapeFlipped-Format](images/sensor-orientation-c.PNG)

Sie können die Ausrichtungswerte mithilfe der [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Dn264258)-Klasse abfragen, indem Sie die [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.getforcurrentview.aspx)-Methode mit der [**CurrentOrientation**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.currentorientation.aspx)-Eigenschaft verwenden. Anschließend können Sie durch einen Vergleich mit der [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/BR226142)-Enumeration eine Logik erstellen. Bedenken Sie, dass Sie für jede unterstützte Ausrichtung eine Konvertierung der Referenzachsen in die jeweilige Ausrichtung unterstützen müssen.

## <a name="landscape-first-vs-portrait-first-devices"></a>Für Querformat und für Hochformat ausgelegte Geräte

Hersteller bieten Geräte an, die sowohl für das Quer- als auch das Hochformat ausgelegt sind. Der Referenzrahmen weicht zwischen Geräten ab, die für das Querformat (z.B. Desktops und Laptops) und das Hochformat (z.B. Smartphones und einige Tablets) ausgelegt sind. Die folgende Tabelle enthält die Gerätesensorachsen für Geräte, die jeweils für Hoch- oder Querformat ausgelegt sind.

| Ausrichtung | Für Querformat ausgelegt | Für Hochformat ausgelegt |
|-------------|-----------------|----------------|
| **Querformat** | ![Querformatgerät im Querformat](images/sensor-orientation-0.PNG) | ![Hochformatgerät im Querformat](images/sensor-orientation-1.PNG) |
| **Hochformat** | ![Querformatgerät im Hochformat](images/sensor-orientation-2.PNG) | ![Hochformatgerät im Hochformat](images/sensor-orientation-3.PNG) |
| **LandscapeFlipped** | ![Querformatgerät in LandscapeFlipped-Ausrichtung](images/sensor-orientation-4.PNG) | ![Hochformatgerät in LandscapeFlipped-Ausrichtung](images/sensor-orientation-5.PNG) | 
| **PortraitFlipped** | ![Querformatgerät in PortraitFlipped-Ausrichtung](images/sensor-orientation-6.PNG)| ![Hochformatgerät in PortraitFlipped-Ausrichtung](images/sensor-orientation-7.PNG) |

## <a name="devices-broadcasting-display-and-headless-devices"></a>Geräte, die die Anzeige übertragen, und monitorlose Geräte

Manche Geräte können die Anzeige auf ein anderes Gerät übertragen. Sie können z.B. ein Tablet verwenden und die Anzeige auf einen Projektor im Querformat übertragen. In diesem Szenario müssen Sie bedenken, dass die Geräteausrichtung auf dem ursprünglichen Gerät basiert, und nicht auf dem Gerät, das die Anzeige darstellt. Ein Beschleunigungsmesser würde daher Daten für das Tablet melden.

Außerdem verfügen einige Geräte nicht über eine Anzeige. Die Standardausrichtung für diese Geräte ist das Hochformat.

## <a name="display-orientation-and-compass-heading"></a>Bildschirmausrichtung und Kompassrichtung


Die Kompassrichtung hängt von den Referenzachsen ab und ändert sich daher mit der Geräteausrichtung. Sie kompensieren dies entsprechend den Angaben in der folgenden Tabelle (unter der Annahme, dass der Benutzer nach Norden ausgerichtet ist).

| Bildschirmausrichtung | Referenzachse für Kompassrichtung | API-Kompassrichtung bei Ausrichtung nach Norden (auf Querformat ausgelegt) | API-Kompassrichtung bei Ausrichtung nach Norden (auf Hochformat ausgelegt) |Kompensation der Kompassrichtung (auf Querformat ausgelegt) | Kompensation der Kompassrichtung (auf Hochformat ausgelegt) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Querformat           | -Z | 0   | 270 | Richtung               | (Richtung + 90) % 360  |
| Hochformat            |  Y | 90  | 0   | (Richtung + 270) % 360 |  Richtung              |
| LandscapeFlipped    |  Z | 180 | 90  | (Richtung + 180) % 360 | (Richtung + 270) % 360 |
| PortraitFlipped     |  Y | 270 | 180 | (Richtung + 90) % 360  | (Richtung + 180) % 360 |

Ändern Sie die Kompassrichtung gemäß den Angaben in der Tabelle, um die Richtung korrekt anzuzeigen. Der folgende Codeausschnitt veranschaulicht die Vorgehensweise.

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;        
    double displayOffset;
    
    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    
    switch (displayInfo.CurrentOrientation) 
    { 
        case DisplayOrientations.Landscape: 
            displayOffset = 0; 
            break;
        case DisplayOrientations.Portrait: 
            displayOffset = 270; 
            break; 
        case DisplayOrientations.LandscapeFlipped: 
            displayOffset = 180; 
            break; 
        case DisplayOrientations.PortraitFlipped: 
            displayOffset = 90; 
            break; 
     } 
    

    double displayCompensatedHeading = (heading + displayOffset) % 360;
    
    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>Bildschirmausrichtung mit Beschleunigungsmesser und Gyrometer

Die folgende Tabelle zeigt, wie Beschleunigungsmesser- und Gyrometerdaten für die Bildschirmausrichtung konvertiert werden.

| Referenzachsen        |  X |  Y | Z |
|-----------------------|----|----|---|
| **Querformat**         |  X |  Y | Z |
| **Hochformat**          |  Y | -X | Z |
| **LandscapeFlipped**  | -X | -Y | Z |
| **PortraitFlipped**   | -Y |  X | Z |

Das folgende Codebeispiel wendet diese Konvertierungen auf das Gyrometer an.

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  
    
    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation) 
    { 
        case DisplayOrientations.Landscape: 
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait: 
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break; 
        case DisplayOrientations.LandscapeFlipped: 
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break; 
        case DisplayOrientations.PortraitFlipped: 
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break; 
     } 
    
    
    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>Bildschirmausrichtung und Geräteausrichtung

Die [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371)-Daten müssen auf andere Weise geändert werden. Stellen Sie sich diese unterschiedlichen Ausrichtungen als Drehungen um die Z-Achse entgegen dem Uhrzeigersinn vor. Folglich müssen wir die Drehung umkehren, um wieder die Ausrichtung des Benutzers zu erhalten. Für Quaterniondaten können wir anhand der eulerschen Formel eine Drehung mit einer Referenzquaternion definieren, und außerdem können wir eine Referenzdrehungsmatrix verwenden.

![Eulersche Formel](images/eulers-formula.png)

Um die gewünschte relative Ausrichtung zu erhalten, multiplizieren Sie das Referenzobjekt mit dem absoluten Objekt. Beachten Sie, dass diese Berechnung nicht kommutativ ist.

![Multiplizieren des Referenzobjekts mit dem absoluten Objekt](images/orientation-formula.png)

Im vorangehenden Ausdruck wird das absolute Objekt von den Sensordaten zurückgegeben.


| Bildschirmausrichtung  | Drehung gegen den Uhrzeigersinn um Z | Referenzquaternion (Drehung in umgekehrter Richtung) | Referenzdrehungsmatrix (Drehung in umgekehrter Richtung) | 
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Querformat**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Hochformat**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1 0 0<br/> 0 0 1]             |

