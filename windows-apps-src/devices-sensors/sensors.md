---
ms.assetid: 415F4107-0612-4235-9722-0F5E4E26F957
title: Sensoren
description: Mithilfe von Sensoren können Apps die Beziehung zwischen einem Gerät und der physischen Umgebung ermitteln. Sensoren können für die App die Richtung, Ausrichtung und Bewegung des Geräts erfassen.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c29673a0b41b4f071d438f0c03255c2d73831dc5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626795"
---
# <a name="sensors"></a>Sensoren



Mithilfe von Sensoren können Apps die Beziehung zwischen einem Gerät und der physischen Umgebung ermitteln. Sensoren können für die App die Richtung, Ausrichtung und Bewegung des Geräts erfassen. Diese Sensoren können Ihre Spiel-, Augmented Reality- oder Dienstprogramm-App hilfreicher und interaktiver machen, indem sie eine spezielle Eingabeform bereitstellen. So können z. B. durch Bewegen des Geräts die Zeichen auf dem Bildschirm angepasst werden, oder das Gerät kann als virtuelles Lenkrad verwendet werden.

Im Allgemeinen sollten Sie im Vorfeld entscheiden, ob die App ausschließlich auf Sensorsteuerung beruhen soll oder ob mit den Sensoren lediglich ein zusätzlicher Steuermechanismus bereitgestellt wird. Ein Rennspiel, bei dem ein Gerät als virtuelles Lenkrad genutzt wird, kann beispielsweise auch über eine GUI auf dem Bildschirm gesteuert werden. Die App funktioniert dann unabhängig von den Sensoren, die für das System zur Verfügung stehen. Andererseits könnte ein Murmel-Kipplabyrinth ausschließlich für die Funktion mit Systemen mit entsprechenden Sensoren geschrieben werden. Sie müssen die strategische Entscheidung treffen, ob ausschließlich Sensoren verwendet werden sollen. Dabei ist zu beachten, dass ein Ansatz mit Steuerung per Maus oder Touchfunktion eine bessere Kontrolle ermöglicht, was jedoch zu Lasten des Immersionseffekts geht.

| Thema                                                       | Beschreibung  |
|-------------------------------------------------------------|--------------|
| [Kalibrieren Sensoren](calibrate-sensors.md)                   | Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein. Falls für Ihr Gerät eine Kalibrierung erforderlich ist, kann die [<strong>MagnetometerAccuracy</strong>](https://msdn.microsoft.com/library/windows/apps/Dn297552)-Aufzählung helfen, die nächsten Schritte festzulegen. |
| [Sensor-Ausrichtung](sensor-orientation.md)                 | Sensordaten der [<strong>OrientationSensor</strong>](https://msdn.microsoft.com/library/windows/apps/BR206371)-Klassen sind durch ihre Referenzachsen definiert. Diese Achsen werden durch das Querformat des Geräts bestimmt und drehen sich mit dem Gerät, wenn es vom Benutzer gedreht wird. |
| [Verwenden Sie den Beschleunigungsmesser](use-the-accelerometer.md)           | Hier erfahren Sie, wie Sie mithilfe des Beschleunigungsmessers auf Benutzerbewegungen reagieren können. |
| [Verwenden des Kompasses](use-the-compass.md)                       | Hier erfahren Sie, wie Sie mithilfe des Kompasses die aktuelle Richtung ermitteln. |
| [Verwenden Sie die gyrometer](use-the-gyrometer.md)                   | Hier erfahren Sie, wie Sie mithilfe des Gyrometers Bewegungsänderungen des Benutzers erkennen. | 
| [Verwenden Sie die neigungsmesser](use-the-inclinometer.md)             | Hier erfahren Sie, wie Sie mithilfe des Neigungsmessers Werte für Nick-, Roll- und Gierwinkel ermitteln. |
| [Der lichtsensor verwenden](use-the-light-sensor.md)             | Hier erfahren Sie, wie Sie mithilfe des Umgebungslichtsensors veränderte Lichtverhältnisse erkennen. |
| [Verwenden der ausrichtungssensor](use-the-orientation-sensor.md) | Hier erfahren Sie, wie Sie mithilfe der Ausrichtungssensoren die Ausrichtung des Geräts ermitteln.|

## <a name="sensor-batching"></a>Sensorbatchverarbeitung

Einige Sensoren unterstützen das Konzept der Batchverarbeitung. Dies variiert je nach verfügbaren Sensoren. Wenn ein Sensor die Batchverarbeitung implementiert, sammelt er mehrere Datenpunkte in einem bestimmten Zeitintervall und überträgt dann alle Daten auf einmal. Dies unterscheidet sich vom normalen Verhalten, bei dem ein Sensor die Ergebnisse sofort beim Erfassen meldet. Im folgenden Diagramm sehen Sie, wie Daten erfasst und anschließend übermittelt werden. Zuerst ist die normale Übermittlung und anschließend die Batchübermittlung dargestellt.

![Sensorbatchsammlung](images/batchsample.png)

Der wichtigste Vorteil der Sensorbatchverarbeitung besteht in der längeren Akkulaufzeit. Wenn die Daten nicht sofort gesendet werden, wird dadurch Prozessorleistung gespart und die umgehende Verarbeitung der Daten verhindert. Teile des Systems können deaktiviert werden, bis sie benötigt werden. Dadurch wird Energie gespart.

Sie können durch Anpassen der Wartezeit beeinflussen, wie oft der Sensor Batches sendet. Beispiel: Der [**Beschleunigungsmesser**](https://msdn.microsoft.com/library/windows/apps/BR225687)-Sensor verfügt über die [**ReportLatency**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportlatency)-Eigenschaft. Wenn diese Eigenschaft für eine Anwendung festgelegt ist, sendet der Sensor Daten nach Ablauf der angegebenen Zeit. Sie können mithilfe der [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportinterval)-Eigenschaft steuern, wie viele Daten über einen bestimmten Zeitraum gesammelt werden.

Beim Festlegen der Wartezeit müssen mehrere Punkte berücksichtigt werden. Beispielsweise gilt für jeden Sensor eine [**MaxBatchSize**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.maxbatchsize.aspx), die basierend auf dem Sensor unterstützt wird. Dabei handelt es sich um die Anzahl der Ereignisse, die der Sensor zwischenspeichern kann, bevor er gezwungen ist, sie zu senden. Wenn Sie **MaxBatchSize** mit [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportinterval) multiplizieren, ergibt das den Höchstwert für [**ReportLatency**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.reportlatency). Wenn Sie einen höheren Wert als diesen angeben, wird die maximale Wartezeit verwendet, damit keine Daten verloren gehen. Darüber hinaus können mehrere Anwendungen eine gewünschte Wartezeit festlegen. Um die Anforderungen aller Anwendungen zu erfüllen, wird die kürzeste Wartezeit verwendet. Aufgrund dieser Tatsachen kann die in Ihrer Anwendung festgelegte Wartezeit von der beobachteten Wartezeit abweichen.

Falls ein Sensor die Batchberichterstellung verwendet, wird durch Aufruf von [**GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.accelerometer.getcurrentreading) der aktuelle Datenbatch gelöscht und eine neue Wartezeit gestartet.

## <a name="accelerometer"></a>Beschleunigungsmesser

Mit dem [**Beschleunigungsmesser**](https://msdn.microsoft.com/library/windows/apps/BR225687) -Sensor werden Schwerkraftwerte entlang der X-, Y- und Z-Achse des Geräts gemessen. Er eignet sich gut für einfache Anwendungen, die auf Bewegung basieren. Beachten Sie, dass für die Beschleunigungskraftwerte die Schwerkraft berücksichtigt wird. Wenn das Gerät für das **FaceUp**-Element den Wert [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399) auf einem Tisch aufweist, ermittelt der Beschleunigungsmesser für die Z-Achse den Wert -1 G. Mit Beschleunigungsmessern wird also nicht unbedingt nur die Koordinatenbeschleunigung (Veränderung der Geschwindigkeit) gemessen. Bei Verwendung eines Beschleunigungsmessers sollten Sie zwischen dem schwerkraftbezogenen Vektor der Schwerkraft und dem linearen Beschleunigungsvektor für die Bewegung unterscheiden. Beachten Sie, dass der Gravitationsvektor für ein stationäres Gerät auf den Wert 1 normalisiert werden sollte.

Die folgenden Diagramme veranschaulichen Folgendes:

-   V1 = Vektor 1 = Anziehungskraft aufgrund der Gravitation
-   V2 = Vektor 2 = -Z-Achse der Geräte-Chassis (zeigt aus der Bildschirmrückseite)
-   Θi = Neigungswinkel (Neigung) = Winkel zwischen –Z-Achse des Geräte-Chassis und Schwerkraftvektor

![Beschleunigungsmesser](images/accelerometer1.png)![Beschleunigungsmessung](images/accelerometer2.png)

Zu den Apps, für die der Beschleunigungsmessersensor verwendet werden kann, zählt beispielsweise ein Spiel, bei dem Sie durch Kippen des Geräts eine Murmel bewegen (Schwerkraftvektor). Die Funktionalität entspricht in etwa der [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766)-Funktionalität und kann auch mit dem Sensor mithilfe einer Kombination aus Nick- und Rollwinkel erzielt werden. Die Nutzung des Schwerkraftvektors eines Beschleunigungsmessers vereinfacht dies, da ein Vektor für das Kippen des Geräts vorhanden ist, der auf einfache Weise mathematisch manipuliert werden kann. Ein anderes Beispiel ist eine App, die das Geräusch eines Peitschenschlags generiert, wenn der Benutzer das Gerät schnell durch die Luft bewegt (linearer Beschleunigungsvektor).

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Beschleunigungsmesser](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer).

## <a name="activity-sensor"></a>Aktivitätssensor

Der [**Activity**](https://msdn.microsoft.com/library/windows/apps/Dn785096)-Sensor ermittelt den aktuellen Status des an den Sensor angeschlossenen Geräts. Dieser Sensor wird häufig in Fitness-Apps verwendet, um zu verfolgen, wenn ein Benutzer mit einem Gerät läuft oder geht. Eine Liste der möglichen Aktivitäten, die von dieser Sensor-API erkannt werden kann, finden Sie unter [**ActivityType**](https://msdn.microsoft.com/library/windows/apps/Dn785128).

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Aktivitätssensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ActivitySensor).

## <a name="altimeter"></a>Höhenmesser

Mit dem [**Höhenmesser**](https://msdn.microsoft.com/library/windows/apps/Dn858893) -Sensor gibt einen Wert zurück, der die Höhe des Sensors angibt. Dadurch können Sie Änderungen der Höhe in Metern über dem Meeresspiegel nachverfolgen. Ein Beispiel für eine App, die dies nutzen kann, ist eine Lauf-App, die Höhenänderungen verfolgt, um den Kalorienverbrauch zu berechnen. In diesem Fall können diese Sensordaten mit dem [**Activity**](https://msdn.microsoft.com/library/windows/apps/Dn785096)-Sensor kombiniert werden, um genauere Informationen bereitzustellen.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Höhenmesser](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Altimeter).

## <a name="barometer"></a>Barometer

Mit dem [**Barometer**](https://msdn.microsoft.com/library/windows/apps/Dn872405) -Sensor ermöglicht einer Anwendung das Abrufen barometrischer Messwerte. Eine Wetter-App kann diese Informationen verwenden, um den aktuellen Luftdruck anzugeben. Dies kann zur Bereitstellung ausführlicher Informationen und zur Vorhersage potenzieller Wetteränderungen genutzt werden.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für ein Barometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Barometer).

## <a name="compass"></a>Kompass

Mit dem [**Kompass**](https://msdn.microsoft.com/library/windows/apps/BR225705) -Sensor gibt eine 2D-Richtung unter Berücksichtigung des magnetischen Nordpols auf Grundlage der horizontalen Ebene der Erde zurück. Der Kompasssensor sollte nicht für die Bestimmung spezifischer Geräteausrichtung oder die Darstellung von Objekten im 3D-Raum verwendet werden. Geografische Objekte können eine natürliche Abweichung der Richtung verursachen, daher unterstützen einige Systeme sowohl [**HeadingMagneticNorth**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.compassreading.headingmagneticnorth.aspx) als auch [**HeadingTrueNorth**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.compassreading.headingtruenorth.aspx). Entscheiden Sie, welcher Pol von der App verwendet werden soll, aber bedenken Sie, dass nicht alle Systeme den korrekten Nordwert ausgeben. Gyrometer- und Magnetometer (ein Gerät zur Messung der magnetischen Stärke)-Sensoren vereinen ihre Daten, um die Kompassrichtung zu bestimmen, wodurch die Daten stabilisiert werden (Die magnetische Feldstärke ist aufgrund der Komponenten elektrischer Systeme sehr instabil).

![Kompasswerte im Hinblick auf den magnetischen Nordpol](images/compass.png)

Für Apps, in denen eine Kompassrose angezeigt oder nach einer Karte navigiert werden soll, wird dafür in der Regel der Kompasssensor eingesetzt.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Kompass](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass).

## <a name="gyrometer"></a>Gyrometer

Mit dem [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718) -Sensor misst Winkelgeschwindigkeiten entlang der X-, Y- und Z-Achse. Dies ist sehr nützlich bei einfachen bewegungsabhängigen Apps, bei denen es nicht um die Ausrichtung des Geräts geht, sondern darum, mit welchen unterschiedlichen Geschwindigkeiten sich das Gerät dreht. Die Leistung von Gyrometern kann durch ein Rauschen in den Daten oder einen systematischen Fehler entlang einer oder mehrerer Achsen beeinträchtigt werden. Sie sollten den Beschleunigungsmesser abfragen, um zu überprüfen, ob sich das Gerät bewegt. So können Sie ermitteln, ob für das Gyrometer ein Fehler vorliegt, und dies in der App dann entsprechend berücksichtigen.

![Gyrometer mit Neigen, Rollen und Schwenken](images/gyrometer.png)

Ein Beispiel für eine App, in der der Gyrometersensor verwendet werden kann, ist ein Spiel, bei dem ein Rouletterad der Drehbewegung des Geräts entsprechend schnell gedreht wird.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für ein Gyrometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Gyrometer).

## <a name="inclinometer"></a>Neigungsmesser

Mit dem [**Neigungssensor**](https://msdn.microsoft.com/library/windows/apps/BR225766) -Sensor gibt Gier-, Nick- und Rollwinkel des Geräts an und funktioniert optimal mit Apps, die die Stellung des Geräts im dreidimensionalen Raum berücksichtigen. Neige- und Rollwinkel werden anhand der Daten des Schwerkraftvektors des Beschleunigungsmessers und der Daten des Gyrometers berechnet. Der Schwenkwert wird durch die Daten des Magnetometers und des Gyrometers (entsprechend der Kompassrichtung) bestimmt. Neigungssensoren bieten erweiterte Ausrichtungsdaten auf eine leicht verständliche Art und Weise. Verwenden Sie Neigungsmesser, wenn Sie Geräteausrichtungswerte benötigen, aber die Sensordaten nicht verarbeiten möchten.

![Neigungsmesser mit Neigungs-, Roll- und Schwenkdaten](images/inclinometer.png)

Für Apps, bei denen die Ansicht der Ausrichtung des Geräts entsprechend geändert werden soll, kann der Neigungssensor verwendet werden. Eine App, bei der Nick-, Gier- und Rollwinkel eines Flugzeugs angezeigt werden, würde ebenfalls Neigungsmesserdaten verwenden.

Ein beispielimplementierung finden Sie im Beispiel neigungsmesser [ https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer).

## <a name="light-sensor"></a>Belichtungssensor

Der [**Light**](https://msdn.microsoft.com/library/windows/apps/BR225790)-Sensor kann das Umgebungslicht bestimmen, in dem sich der Sensor befindet. Dadurch kann die App ermitteln, wann sich die Einstellung des Umgebunglichts des Geräts geändert hat. Angenommen, ein Benutzer mit einem Slate-Gerät geht an einem sonnigen Tag aus einem geschlossenen Raum nach draußen. Eine intelligente Anwendung könnte diesen Wert verwenden, um den Kontrast zwischen dem Hintergrund und der dargestellten Schriftart zu erhöhen. Dadurch wäre der Inhalt auch in der helleren Umgebung draußen noch lesbar.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Belichtungssensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor).

## <a name="orientation-sensor"></a>Ausrichtungssensor

Die Geräteausrichtung wird durch die Quaternionen- und Drehungsmatrizes ausgedrückt. Der [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) bietet einen hohen Genauigkeitsgrad bei der Bestimmung der Position des Geräts im dreidimensionalen Raum unter Berücksichtigung der absoluten Richtung. Die **OrientationSensor**-Daten werden vom Beschleunigungsmesser, Gyrometer und Magnetometer abgeleitet. Auch die Neigungsmesser- und die Kompasssensorendaten können von den Quaternionenwerten abgeleitet werden. Quaternion- und Rotationsmatrizes sind für erweiterte mathematische Manipulationen geeignet und werden oft bei der grafischen Programmierung verwendet. Für Apps, die komplexe Manipulation verwenden, sollten die Ausrichtungssensoren verwendet werden, da viele Transformationen auf Quaternion- und Rotationsmatrizes beruhen.

![Ausrichtungssensordaten](images/orientation-sensor.png)

Der Ausrichtungssensor wird häufig in Augmented Reality-Apps verwendet, in denen auf Grundlage der Ausrichtung der Geräterückseite das Bild der Umgebung mit einer Grafik überlagert wird.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Ausrichtungssensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor).

## <a name="pedometer"></a>Schrittzähler

Mit dem [**Schrittzähler**](https://msdn.microsoft.com/library/windows/apps/Dn878203) -Sensor verfolgt die Anzahl der Schritte des Benutzers, der das verbundene Gerät trägt. Der Sensor ist so konfiguriert, dass die Anzahl der Schritte über einen bestimmten Zeitraum hinweg verfolgt wird. Einige Fitness-Apps verfolgen die Anzahl der ausgeführten Schritte, um dem Benutzer das Festlegen und Erreichen verschiedener Ziele zu erleichtern. Diese Informationen können dann erfasst und gespeichert werden, um den Fortschritt über einen Zeitraum anzuzeigen.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Schrittzähler](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Pedometer).

## <a name="proximity-sensor"></a>Näherungssensor

Der [**Proximity**](https://msdn.microsoft.com/library/windows/apps/Dn872427)-Sensor kann verwendet werden, um anzugeben, ob Objekte vom Sensor erkannt werden. Neben der Bestimmung, ob sich ein Objekt innerhalb des Erkennungsbereichs des Geräts befindet, kann der Näherungssensor auch den Abstand zum erkannten Objekt ermitteln. Ein Verwendungsbeispiel wäre eine Anwendung, die aus einem Standbyzustand aktiviert werden soll, wenn sich ein Benutzer bis zu einer bestimmten Entfernung nähert. Das Gerät könnte sich in einem Ruhezustand befinden, bis der Näherungssensor ein Objekt erkennt, woraufhin es in einen aktiveren Zustand versetzt wird.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen Näherungssensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ProximitySensor).

## <a name="simple-orientation"></a>Einfache Ausrichtung

Der [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/windows.devices.sensors.simpleorientationsensor.aspx) erkennt die aktuelle Quadrantenausrichtung des angegebenen Geräts, bzw. ob die Oberseite nach oben oder unten zeigt. Er verfügt über sechs mögliche [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)-Zustände (**NotRotated**, **Rotated90**, **Rotated180**, **Rotated270**, **FaceUp**, **FaceDown**).

Eine Reader-App, bei der die Anzeige abhängig von der Ausrichtung des Geräts in Relation zum Boden geändert wird, würde die Ausrichtung des Geräts anhand von SimpleOrientationSensor-Werten bestimmen.

Eine Beispiel für eine Implementierung finden Sie unter [Beispiel für einen einfachen Ausrichtungssensor](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor).
