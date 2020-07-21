---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: Sensorausrichtung
description: Sensordaten der Klassen Accelerometer, Gyrometer, Compass, Inclinometer und OrientationSensor sind durch ihre Referenzachsen definiert. Diese Achsen werden durch das Querformat des Geräts bestimmt und drehen sich mit dem Gerät, wenn es vom Benutzer gedreht wird.
ms.date: 07/03/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b61b7bcd18419ec9be719b5f565e5503953be7c3
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493469"
---
# <a name="sensor-orientation"></a>Sensorausrichtung

Sensordaten der Klassen [**Accelerometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer), [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer), [**Compass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Compass), [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) und [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) sind durch ihre Referenzachsen definiert. Diese Achsen werden durch den Verweis Rahmen des Geräts definiert und drehen sich mit dem Gerät, während der Benutzer Sie einschalten. Falls Ihre App die automatische Drehung unterstützt und sie sich selbst entsprechend der Drehung des Geräts neu ausrichtet, müssen Sie die Sensordaten für die Drehung anpassen, bevor Sie sie verwenden.

### <a name="important-apis"></a>Wichtige APIs

- [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
- [**Windows. Devices. Sensors. Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>Bildschirmausrichtung und Geräteausrichtung

Um die Referenzachse für Sensoren begreifen zu können, muss zwischen Bildschirmausrichtung und Geräteausrichtung unterschieden werden. Bei der Bildschirmausrichtung handelt es sich um die Richtung, in der Text und Bilder auf dem Bildschirm angezeigt werden, wohingegen es sich bei der Geräteausrichtung um die physische Position des Geräts handelt.

> [!NOTE]
> Die positive z-Achse erweitert den Bildschirm des Geräts, wie in der folgenden Abbildung dargestellt.
> :::image type="content" source="images/sensor-orientation-zaxis-1-small.png" alt-text="Z-Achse für Laptop":::

In den folgenden Diagrammen befinden sich sowohl das Gerät als auch die Anzeige Ausrichtung im [quer](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) Format (die angezeigten Sensor Achsen sind spezifisch für Querformat).


Dieses Diagramm zeigt sowohl die Anzeige als auch die Geräte Ausrichtung im [quer](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)Format an.

:::image type="content" source="images/sensor-orientation-a-small.jpg" alt-text="Bildschirm- und Geräteausrichtung im Querformat":::

Das nächste Diagramm zeigt sowohl die Anzeige als auch die Geräte Ausrichtung in " [landscapeflipped](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)" an.

:::image type="content" source="images/sensor-orientation-b-small.jpg" alt-text="Bildschirm- und Geräteausrichtung im LandscapeFlipped-Format":::

In diesem abschließenden Diagramm wird die Anzeige Ausrichtung im Querformat dargestellt, während die Geräte Ausrichtung [Landschafts](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)basiert ist.

:::image type="content" source="images/sensor-orientation-c-small.jpg" alt-text="Bildschirmausrichtung im Querformat und Geräteausrichtung im LandscapeFlipped-Format":::

Sie können die Ausrichtungswerte mithilfe der [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation)-Klasse abfragen, indem Sie die [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.getforcurrentview)-Methode mit der [**CurrentOrientation**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.currentorientation)-Eigenschaft verwenden. Anschließend können Sie durch einen Vergleich mit der [**DisplayOrientations**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations)-Enumeration eine Logik erstellen. Bedenken Sie, dass Sie für jede unterstützte Ausrichtung eine Konvertierung der Referenzachsen in die jeweilige Ausrichtung unterstützen müssen.

## <a name="landscape-first-vs-portrait-first-devices"></a>Für Querformat und für Hochformat ausgelegte Geräte

Hersteller bieten Geräte an, die sowohl für das Quer- als auch das Hochformat ausgelegt sind. Der Referenzrahmen weicht zwischen Geräten ab, die für das Querformat (z. B. Desktops und Laptops) und das Hochformat (z. B. Smartphones und einige Tablets) ausgelegt sind. Die folgende Tabelle enthält die Gerätesensorachsen für Geräte, die jeweils für Hoch- oder Querformat ausgelegt sind.

| Ausrichtung | Für Querformat ausgelegt | Für Hochformat ausgelegt |
|-------------|-----------------|----------------|
| **Querformat** | :::image type="content" source="images/sensor-orientation-0-small.jpg" alt-text="Querformatgerät im Querformat"::: | :::image type="content" source="images/sensor-orientation-1-small.jpg" alt-text="Hochformatgerät im Querformat"::: |
| **Hochformat** | :::image type="content" source="images/sensor-orientation-2-small.jpg" alt-text="Querformatgerät im Hochformat"::: | :::image type="content" source="images/sensor-orientation-3-small.jpg" alt-text="Hochformatgerät im Hochformat"::: |
| **LandscapeFlipped** | :::image type="content" source="images/sensor-orientation-4-small.jpg" alt-text="Querformatgerät in LandscapeFlipped-Ausrichtung"::: | :::image type="content" source="images/sensor-orientation-5-small.jpg" alt-text="Hochformatgerät in LandscapeFlipped-Ausrichtung":::
| **PortraitFlipped** | :::image type="content" source="images/sensor-orientation-6-small.jpg" alt-text="Querformatgerät in PortraitFlipped-Ausrichtung"::: | :::image type="content" source="images/sensor-orientation-7-small.jpg" alt-text="Hochformatgerät in PortraitFlipped-Ausrichtung"::: |

## <a name="devices-broadcasting-display-and-headless-devices"></a>Geräte, die die Anzeige übertragen, und monitorlose Geräte

Manche Geräte können die Anzeige auf ein anderes Gerät übertragen. Sie können z. B. ein Tablet verwenden und die Anzeige auf einen Projektor im Querformat übertragen. In diesem Szenario müssen Sie bedenken, dass die Geräteausrichtung auf dem ursprünglichen Gerät basiert, und nicht auf dem Gerät, das die Anzeige darstellt. Ein Beschleunigungsmesser würde daher Daten für das Tablet melden.

Außerdem verfügen einige Geräte nicht über eine Anzeige. Die Standardausrichtung für diese Geräte ist das Hochformat.

## <a name="display-orientation-and-compass-heading"></a>Bildschirmausrichtung und Kompassrichtung

Die Kompassrichtung hängt von den Referenzachsen ab und ändert sich daher mit der Geräteausrichtung. Sie kompensieren dies entsprechend den Angaben in der folgenden Tabelle (unter der Annahme, dass der Benutzer nach Norden ausgerichtet ist).

| Bildschirmausrichtung | Referenzachse für Kompassrichtung | API-Kompass-Überschrift, wenn nach Norden (Querformat) | API-Kompass-Überschrift, wenn nach Norden (Hochformat) |Kompensierung der Kompass Überschrift (Querformat zuerst) | Kompensierung der Kompass Überschrift (Hochformat-zuerst) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Querformat           | -Z | 0   | 270 | Richtung               | (Richtung + 90) % 360  |
| Hochformat            |  J | 90  | 0   | (Richtung + 270) % 360 |  Richtung              |
| LandscapeFlipped    |  Z | 180 | 90  | (Richtung + 180) % 360 | (Richtung + 270) % 360 |
| PortraitFlipped     |  J | 270 | 180 | (Richtung + 90) % 360  | (Richtung + 180) % 360 |

Ändern Sie die Kompassrichtung entsprechend den Angaben in der Tabelle, um die Richtung korrekt anzuzeigen. Der folgende Codeausschnitt veranschaulicht die Vorgehensweise.

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

| Referenzachsen        |  X |  J | Z |
|-----------------------|----|----|---|
| **Querformat**         |  X |  J | Z |
| **Hochformat**          |  J | -X | Z |
| **LandscapeFlipped**  | -X | -y | Z |
| **PortraitFlipped**   | -y |  X | Z |

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

Die [**OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor)-Daten müssen auf andere Weise geändert werden. Betrachten Sie diese verschiedenen Ausrichtungen als Drehungen gegen den Uhrzeigersinn zur z-Achse, sodass wir die Drehung umkehren müssen, um die Ausrichtung des Benutzers zurückzusetzen. Für Quaterniondaten können wir anhand der eulerschen Formel eine Drehung mit einer Referenzquaternion definieren, und außerdem können wir eine Referenzdrehungsmatrix verwenden.

:::image type="content" source="images/eulers-formula.png" alt-text="Eulersche Formel":::

Um die gewünschte relative Ausrichtung zu erhalten, multiplizieren Sie das Referenzobjekt mit dem absoluten Objekt. Beachten Sie, dass diese Berechnung nicht kommutativ ist.

:::image type="content" source="images/orientation-formula.png" alt-text="Multiplizieren des Referenzobjekts mit dem absoluten Objekt":::

Im vorangehenden Ausdruck wird das absolute Objekt von den Sensordaten zurückgegeben.

| Bildschirmausrichtung  | Drehung gegen den Uhrzeigersinn um Z | Referenzquaternion (Drehung in umgekehrter Richtung) | Referenzdrehungsmatrix (Drehung in umgekehrter Richtung) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Querformat**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Hochformat**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1 0 0<br/> 0 0 1]             |

## <a name="see-also"></a>Weitere Informationen

[Integrieren von Bewegungs- und Ausrichtungssensoren](https://docs.microsoft.com/windows-hardware/design/whitepapers/integrating-motion-and-orientation-sensors)
