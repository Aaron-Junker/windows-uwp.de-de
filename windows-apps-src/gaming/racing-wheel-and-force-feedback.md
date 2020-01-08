---
title: Rennlenkräder und Kraftrückmeldung
description: Verwenden Sie die Windows.Gaming.Input-APIs für Rennlenkräder, um Funktionen zu erkennen und zu ermitteln sowie Kraftrückmeldungsbefehle zu lesen und an Rennlenkräder zu senden.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, Spiele, Rennlenkrad, Kraftrückmeldung
ms.localizationpriority: medium
ms.openlocfilehash: 12b28284fcc459a5d4f155ec7b5fc7cedc0ed8ca
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685173"
---
# <a name="racing-wheel-and-force-feedback"></a>Rennlenkräder und Kraftrückmeldung

Auf dieser Seite werden die Grundlagen der Programmierung für Xbox One-Rennräder mithilfe von [Windows. Gaming. Input. racingwheel][racingwheel] und zugehörigen APIs für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite lernen Sie:

* Wie Sie eine Liste der verbundenen Rennlenkräder und ihrer Benutzer erstellen
* Wie Sie feststellen, dass ein Rennlenkrad hinzugefügt oder entfernt wurde
* Wie Sie die Eingabe eines oder mehrerer Rennlenkräder auslesen
* Wie Sie Kraftrückmeldungsbefehle senden
* Wie sich Rennlenkräder als UI-Navigationsgerät verhalten

## <a name="racing-wheel-overview"></a>Übersicht über Rennlenkräder

Rennlenkräder sind Eingabegeräte, die Benutzern das Gefühl vermitteln, in einem echten Rennwagencockpit zu sitzen. Rennlenkräder sind das ideale Eingabegerät für Rennsportspiele im Arcade- und Simulationsstil, die Autos oder Trucks enthalten. Rennlenkräder werden in Windows 10- und Xbox One-UWP-Apps durch den Namespace [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) unterstützt.

Xbox One-Rennlenkräder werden zu verschiedenen Preisen angeboten. In der Regel verfügen Rennlenkräder über mehr und bessere Eingabe- und Kraftrückmeldungsfunktionen, je höher ihr Preis liegt. Alle Rennlenkräder besitzen ein analoges Lenkrad, analoge Gas- und Bremsensteuerelemente sowie einige Lenkradtasten. Einige Rennlenkräder sind zusätzlich mit analogen Kupplungs- und Handbremsensteuerelementen, einer analogen Gangschaltung sowie Kraftrückmeldungsfunktionen ausgestattet. Nicht alle Rennlenkräder besitzen die gleichen Features. Darüber hinaus unterscheiden sie sich möglicherweise hinsichtlich der Unterstützung für bestimmte Features. Beispielsweise können Lenkräder unterschiedliche Drehbereiche und Gangschaltungen eine unterschiedliche Zahl von Gängen unterstützen.

### <a name="device-capabilities"></a>Gerätefunktionen

Verschiedene Xbox One-Rennräder bieten unterschiedliche Sätze Optionaler Gerätefunktionen und unterschiedliche Unterstützungsstufen für diese Funktionen. Diese Abweichung zwischen einer einzelnen Art von Eingabegerät ist für die Geräte, die von der [Windows. Gaming. Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) -API unterstützt werden, eindeutig. Darüber hinaus unterstützen die meisten Geräte, denen Sie begegnen werden, zumindest einige optionale Funktionen oder andere Abweichungen. Daher ist es wichtig, die Funktionen der verbundenen Rennlenkräder einzeln zu ermitteln und die gesamte Bandbreite der für Ihr Spiel sinnvollen Funktionen zu unterstützen.

Weitere Informationen finden Sie unter [Ermitteln von Rennlenkradfunktionen](#determining-racing-wheel-capabilities).

### <a name="force-feedback"></a>Kraftrückmeldung

Einige Xbox One-Rennlenkräder stellen eine echte Kraftrückmeldung und nicht einfach nur Vibrationen bereit, d. h., sie können echte Kräfte auf eine Steuerungsachse wie das Lenkrad ausüben. Spiele verwenden diese Fähigkeit, um ein besseres Spielerlebnis zu vermitteln (_simulierte Unfallschäden_, „Gefühl für die Straße“) und die Anforderungen in Bezug auf gutes Fahren zu erhöhen.

Weitere Informationen finden Sie unter [Übersicht über die Kraftrückmeldung](#force-feedback-overview).

### <a name="ui-navigation"></a>Benutzeroberflächennavigation

Um den Aufwand für die Unterstützung unterschiedlicher Eingabegeräte für die Benutzeroberflächennavigation zu verringern und die Konsistenz zwischen Spielen und Geräten zu fördern, dienen die meisten _physischen_ Eingabegeräte gleichzeitig als getrennte _logische_ Eingabegeräte, die als [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md) bezeichnet werden. Der Benutzeroberflächen-Navigationscontroller stellt über verschiedene Eingabegeräte hinweg ein gemeinsames Vokabular für Benutzeroberflächen-Navigationsbefehle bereit.

Aufgrund der spezifischen Ausrichtung auf analoge Steuerungen und des Umfangs der Abweichungen zwischen den verschiedenen Rennlenkrädern verfügen diese in der Regel über Steuerkreuz, **Ansicht** **Menü** und die Tasten **A**, **B**, **X** und **Y**, die den Tasten eines [Gamepad](gamepad-and-vibration.md) ähnlich sind. Diese Tasten sind jedoch nicht für die Unterstützung von Befehlen im Spiel vorgesehen und können nicht direkt als Rennlenkradtasten verwendet werden.

Als Benutzeroberflächen-Navigationscontroller ordnen Rennlenkräder den [erforderlichen Satz](ui-navigation-controller.md#required-set) von Navigationsbefehlen dem linken Ministick, dem Steuerkreuz, der **Ansicht**, dem **Menü** sowie den Tasten **A** und **B** zu.

| Navigationsbefehl | Rennlenkradeingabe |
| ------------------:| ------------------ |
|                 Oben | Steuerkreuz nach oben           |
|               Unten | Steuerkreuz nach unten         |
|               Nach links | Steuerkreuz nach links         |
|              Nach rechts | Steuerkreuz nach rechts        |
|               „Ansicht” | Ansicht-Taste        |
|               Menü | Menü-Taste        |
|             Annehmen | A-TASTE           |
|             Abbrechen | B-TASTE           |

Darüber hinaus ordnen einige Rennlenkräder möglicherweise einige [optionale](ui-navigation-controller.md#optional-set) Navigationsbefehle anderen von ihnen unterstützten Eingaben zu. Die Befehlszuordnungen können sich jedoch von Gerät zu Gerät unterscheiden. Sie sollten die Unterstützung dieser Befehle ebenfalls in Betracht ziehen, dabei jedoch sicherstellen, dass diese Befehle für die Navigation in der Benutzeroberfläche Ihres Spiels nicht notwendig sind.

| Navigationsbefehl | Rennlenkradeingabe    |
| ------------------:| --------------------- |
|            Seite nach oben | _variiert_              |
|          Seite nach unten | _variiert_              |
|          Seite nach links | _variiert_              |
|         Seite nach rechts | _variiert_              |
|          Bildlauf nach oben | _variiert_              |
|        Bildlauf nach unten | _variiert_              |
|        Bildlauf nach links | _variiert_              |
|       Bildlauf nach rechts | _variiert_              |
|          Kontext 1 | X-Taste (_in der Regel_) |
|          Kontext 2 | Y-Taste (_in der Regel_) |
|          Kontext 3 | _variiert_              |
|          Kontext 4 | _variiert_              |

## <a name="detect-and-track-racing-wheels"></a>Erkennen und Nachverfolgen von Rennlenkrädern

Ermitteln und Nachverfolgen von Rennlenkrädern funktioniert auf genau die gleiche Weise wie für Gamepads, nur mit der [RacingWheel](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel)-Klasse anstelle der [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad)-Klasse. Weitere Informationen finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md).

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel) class provides a static property, [RacingWheels](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.racingwheels#Windows_Gaming_Input_RacingWheel_RacingWheels), which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.racingwheeladded) and [RacingWheelRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.racingwheelremoved) events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>Auslesen der Rennlenkradwerte

Nachdem Sie die Rennlenkräder identifiziert haben, für die Sie sich interessieren, können Sie Eingaben von diesen erfassen. Anders als im Fall anderer Eingaben, die Sie möglicherweise kennen, teilen Rennlenkräder Zustandsänderungen jedoch nicht durch das Auslösen von Ereignissen mit. Stattdessen müssen Sie die aktuellen Zustände regelmäßig _auslesen_ (Polling).

### <a name="polling-the-racing-wheel"></a>Abrufen von Rennlenkrad-Daten

Durch den Abruf wird eine Momentaufnahme des Rennlenkrads zu einem genau festgelegten Zeitpunkt erfasst. Dieser Ansatz zum Erfassen von Eingaben ist für die meisten Spiele geeignet, da deren Logik üblicherweise in einer deterministischen Schleife ausgeführt wird und nicht ereignisgesteuert ist. Es ist in der Regel auch einfacher, Befehle in Spielen anhand von Eingaben zu interpretieren, die alle gemeinsam erfasst werden, als anhand zahlreicher Eingaben, die im Laufe der Zeit erfasst werden.

Sie rufen Rennlenkradwerte durch Aufruf von [GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.getcurrentreading#Windows_Gaming_Input_RacingWheel_GetCurrentReading) ab. Diese Funktion gibt einen [RacingWheelReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelreading)-Wert zurück, der den Zustand des Rennlenkrads enthält.

Im folgenden Beispiel wird der aktuelle Zustand eines Rennlenkrads abgerufen.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

Zusätzlich zum Zustand des Rennlenkrads enthält jede Ablesung einen Zeitstempel, der den genauen Zeitpunkt angibt, an dem der Zustand abgerufen wurde. Dieser Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten früherer Abrufe oder Spielsimulationen herzustellen.

### <a name="determining-racing-wheel-capabilities"></a>Ermitteln der Rennlenkradfunktionalität

Viele Steuerelemente eines Rennlenkrads sind optional oder unterstützen verschiedene Varianten. Da dies auch für erforderliche Steuerelemente gilt, müssen Sie die Funktionalität jedes Rennlenkrads einzeln ermitteln, bevor Sie die Eingaben verarbeiten können, die mit den Ablesungen des Rennlenkrads erfasst wurden.

Optionale Steuerelemente sind Handbremse, Kupplung und Gangschaltung. Sie können ermitteln, ob ein verbundenes Rennlenkrad diese Steuerelemente unterstützt, indem Sie die Eigenschaften [HasHandbrake](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.hashandbrake#Windows_Gaming_Input_RacingWheel_HasHandbrake), [HasClutch](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.hasclutch#Windows_Gaming_Input_RacingWheel_HasClutch) bzw. [HasPatternShifter](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.haspatternshifter#Windows_Gaming_Input_RacingWheel_HasPatternShifter) des Rennlenkrads lesen. Das Steuerelement wird unterstützt, wenn der Wert der Eigenschaft **true** ist. Andernfalls wird es nicht unterstützt.

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

Die Steuerelemente, deren Funktionalität variieren kann, sind das Lenkrad und die Gangschaltung. Das Lenkrad kann hinsichtlich des Umfangs der physischen Drehung variieren, die durch das tatsächliche Lenkrad unterstützt wird. Die Gangschaltung kann hinsichtlich der Zahl der einzelnen Vorwärtsgänge variieren, die unterstützt werden. Sie können den größten, durch das tatsächliche Lenkrad unterstützten Drehwinkel ermitteln, indem Sie die Eigenschaft `MaxWheelAngle` des Rennlenkrads lesen. Dessen Wert entspricht dem maximal unterstützten physischen Winkel im Uhrzeigersinn, ausgedrückt in Grad (positive Gradangabe). Der gleiche Drehwinkel wird in der dem Uhrzeigersinn entgegengesetzten Richtung unterstützt (negative Gradangabe). Sie können den höchsten, durch die Gangschaltung unterstützten Vorwärtsgang ermitteln, indem Sie die Eigenschaft des Rennlenkrads lesen. Dessen Wert entspricht dem höchsten unterstützten Vorwärtsgang (einschließlich): Wenn der Wert 4 ist, unterstützt die Gangschaltung einen Rückwärtsgang, einen Leerlaufgang sowie einen ersten, zweiten, dritten und vierten Gang.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

Schließlich unterstützen einige Rennlenkräder die Kraftrückmeldung über das Lenkrad. Sie können ermitteln, ob ein verbundenes Rennlenkrad die Kraftrückmeldung unterstützt, indem Sie die Eigenschaft [WheelMotor](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) des Rennlenkrads lesen. Die Kraftrückmeldung wird unterstützt, wenn `WheelMotor` nicht **null** ist. Andernfalls wird sie nicht unterstützt.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

Weitere Informationen dazu, wie Sie die Kraftrückmeldungsfunktion der Rennlenkräder verwenden, die dies unterstützen, finden Sie unter [Übersicht über die Kraftrückmeldung](#force-feedback-overview).

### <a name="reading-the-buttons"></a>Auslesen der Tasten

Jede Rennlenkradtaste&mdash;die vier Richtungen des Steuerkreuzes, die Tasten **Vorheriger Gang** und **Nächster Gang** und 16 zusätzliche Tasten&mdash;stellt eine digitalen Wert bereit, der angibt, ob sie gedrückt (unten) oder freigegeben (oben) ist. Aus Effizienzgründen werden die Ablesewerte der Tasten nicht als einzelne boolesche Werte angezeigt, sondern in einem einzelnen Bitfeld zusammengefasst, dargestellt durch die Enumeration [RacingWheelButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelbuttons).

> [!NOTE]
> Rennlenkräder besitzen zusätzliche Tasten für die Benutzeroberflächennavigation, beispielsweise die Tasten **Ansicht** und **Menü**. Diese Tasten sind nicht in der Enumeration `RacingWheelButtons` enthalten und können nur gelesen werden, wenn auf das Rennlenkrad als Benutzeroberflächen-Navigationsgerät zugegriffen wird. Weitere Informationen finden Sie unter [Benutzeroberflächen-Navigationsgerät](ui-navigation-controller.md).

Die Tastenwerte werden aus der Eigenschaft `Buttons` der Struktur [RacingWheelReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelreading) gelesen. Da diese Eigenschaft ein Bitfeld ist, wird eine bitweise Maskierung verwendet, um den Wert der Taste zu isolieren, an der Sie interessiert sind, Die Taste ist gedrückt (unten), wenn das entsprechende Bit festgelegt ist. Andernfalls ist sie nicht gedrückt (oben).

Im folgenden Beispiel wird ermittelt, ob die Taste **Nächster Gang** gedrückt ist.

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

Im folgenden Beispiel wird ermittelt, ob die Taste „Nächster Gang“ freigegeben ist.

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

In einigen Fällen möchten Sie vielleicht ermitteln, ob eine Taste von „gedrückt“ zu „nicht gedrückt“ bzw. umgekehrt wechselt, oder ob mehrere Tasten gedrückt bzw. nicht gedrückt oder in bestimmter Weise angeordnet sind&mdash;einige gedrückt, andere nicht. Weitere Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) und [Erkennen von komplexen Tastenanordnungen](input-practices-for-games.md#detecting-complex-button-arrangements).

### <a name="reading-the-wheel"></a>Lesen der Lenkradwerte

Das Lenkrad ist ein erforderliches Steuerelement, das einen analogen Ablesewert zwischen -1,0 und +1,0 bereitstellt. Der Wert -1,0 entspricht der äußersten linken Lenkradposition. Der Wert +1,0 entspricht der äußersten rechten Lenkradposition. Der Wert für das Lenkrad wird aus der Eigenschaft `Wheel` der Struktur [RacingWheelReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelreading) gelesen.

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

Auch wenn die Lenkradwerte dem Grad der physischen Drehung des tatsächlichen Lenkrads entsprechen, abhängig vom durch das physische Rennlenkrad unterstützten Drehbereich, sollten Sie die Lenkradwerte nicht skalieren. Lenkräder, die einen größeren Drehbereich unterstützen, bieten lediglich eine größere Genauigkeit.

### <a name="reading-the-throttle-and-brake"></a>Auslesen der Gas- und Bremsensteuerelementwerte

Die Gas- und Bremsensteuerelemente sind erforderliche Steuerelemente, die jeweils analoge Ablesewerte zwischen 0,0 (vollständig freigegeben) und 1,0 (vollständig gedrückt) bereitstellen, dargestellt als Gleitkommawerte. Der Wert für das Gassteuerelement wird aus der Eigenschaft `Throttle` der Struktur [RacingWheelReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelreading) gelesen. Der Wert für das Bremsensteuerelement wird aus der Eigenschaft `Brake` gelesen.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>Lesen der Handbremsen- und Kupplungswerte

Die Handbremsen- und Kupplungssteuerelemente sind optionale Steuerelemente, die jeweils analoge Ablesewerte zwischen 0,0 (vollständig freigegeben) und 1,0 (vollständig gedrückt) bereitstellen, dargestellt als Gleitkommawerte. Der Wert für das Handbremsensteuerelement wird aus der Eigenschaft `Handbrake` der Struktur [RacingWheelReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelreading) gelesen. Der Wert für das Kupplungssteuerelement wird aus der Eigenschaft `Clutch` gelesen.

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>Lesen der Gangschaltungswerte

Die Gangschaltung ist ein optionales Steuerelement, das einen digitalen Wert zwischen -1 und [MaxPatternShifterGear](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.maxpatternshiftergear) bereitstellt, dargestellt als ganze Zahl mit Vorzeichen. Der Wert -1 bzw. 0 entspricht dem _Rückwärtsgang_ bzw. dem _Leerlaufgang_. Aufsteigende positive Werte entsprechen den jeweiligen höheren Vorwärtsgängen bis einschließlich **MaxPatternShifterGear**. Der Wert des mustershifter wird aus der Eigenschaft " [patternshibertergear](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelreading.patternshiftergear) " der " [racingwheelreading](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheelreading) "-Struktur gelesen.

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> Hinweis    Wenn unterstützt, ist die Gangschaltung zusätzlich zu den erforderlichen Tasten für den **vorherigen Gang** und den **nächsten Gang** vorhanden, die sich ebenfalls auf den aktuellen Gang des Autos des Spielers auswirken. Wenn beides vorhanden ist, besteht eine einfache Strategie für die Zusammenführung dieser Eingaben im Ignorieren von Gangschaltung (und Kupplung), wenn sich ein Spieler für ein Automatikgetriebe entscheidet, und im Ignorieren der Tasten für den **vorherigen Gang** und den **nächsten Gang**, wenn sich ein Spieler für ein manuelles Getriebe entscheidet (nur möglich, wenn das Rennlenkrad über ein Gangschaltungssteuerelement verfügt). Wenn dies für Ihr Spiel nicht geeignet ist, können Sie eine andere Strategie für die Zusammenführung implementieren.

## <a name="run-the-inputinterfacing-sample"></a>Ausführen des InputInterfacing-Beispiels

Das [InputInterfacingUWP-Beispiel _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) zeigt, wie Rennlenkräder und verschiedene Arten von Eingabegeräten zusammen verwendet werden und wie sich diese Eingabegeräte bei Verwendung als Benutzeroberflächen-Navigationscontroller verhalten.

## <a name="force-feedback-overview"></a>Übersicht über die Kraftrückmeldung

Viele Rennlenkräder besitzen eine Kraftrückmeldungsfunktion, um ein umfassenderes und stärker herausforderndes Fahrerlebnis bereitzustellen. Rennlenkräder, die die Kraftrückmeldung unterstützen, besitzen in der Regel einen einzelnen Motor, der entlang einer einzelnen Achse (der Achse der Lenkraddrehung) Kraft auf das Lenkrad ausübt. Das Erzwingen von Feedback wird in Windows 10-und Xbox One-UWP-Apps durch den [Windows. Gaming. Input. ForceFeedback](https://docs.microsoft.com/uwp/api/windows.gaming.input.forcefeedback) -Namespace unterstützt.

> [!NOTE]
> Die Kraftrückmeldungs-APIs können mehrere Krafteinwirkungsachsen unterstützen. Xbox One-Rennlenkräder unterstützen zurzeit jedoch nur die Lenkraddrehungsachse als Kraftrückmeldungsachse.

## <a name="using-force-feedback"></a>Verwenden der Kraftrückmeldung

In den folgenden Abschnitten werden die Grundlagen der Programmierung von Kraftrückmeldungseffekten für Xbox One-Rennlenkräder beschrieben. Die Rückmeldung wird mithilfe von Effekten angewendet, die zuerst in das Rückmeldungsgerät geladen und anschließend gestartet, angehalten, fortgesetzt und gestoppt werden können, ähnlich wie Soundeffekte. Sie müssen jedoch zunächst die Rückmeldungsfunktionalität des Rennlenkrads ermitteln.

### <a name="determining-force-feedback-capabilities"></a>Ermitteln der Kraftrückmeldungsfunktionalität

Sie können ermitteln, ob ein verbundenes Rennlenkrad die Kraftrückmeldung unterstützt, indem Sie die Eigenschaft [WheelMotor](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) des Rennlenkrads lesen. Die Kraftrückmeldung wird nicht unterstützt, wenn `WheelMotor`**null** ist. Andernfalls wird die Kraftrückmeldung unterstützt, und Sie können mit der Ermittlung der spezifischen Kraftrückmeldungsfunktionalität des Motors fortfahren, beispielsweise der Achsen, auf die Kraft ausgeübt werden kann.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>Laden der Kraftrückmeldungseffekte

Die Kraftrückmeldungseffekte werden in das Rückmeldungsgerät geladen, auf dem sie autonom nach den Regeln Ihres Spiels „gespielt“ werden. Eine Reihe grundlegender Effekte werden bereitgestellt. benutzerdefinierte Effekte können über eine Klasse erstellt werden, die die [iforcefeedbackeffect](https://docs.microsoft.com/uwp/api/windows.gaming.input.forcefeedback.iforcefeedbackeffect) -Schnittstelle implementiert.

| Effektklasse         | Effektbeschreibung                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | Ein Effekt, der als Reaktion auf den aktuellen Sensor im Gerät unterschiedliche Kraft anwendet. |
| ConstantForceEffect  | Ein Effekt, der eine konstante Kraft entlang eines Vektors anwendet.                                  |
| PeriodicForceEffect  | Ein Effekt, der entlang eines Vektors unterschiedliche Kraft anwendet, definiert durch eine Waveform.           |
| RampForceEffect      | Ein Effekt, der eine linear zunehmende/abnehmende Kraft entlang eines Vektors anwendet.          |

```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>Verwenden von Kraftrückmeldungseffekten

Nach dem Laden können alle Effekte synchron durch das Aufrufen von Funktionen in der Eigenschaft `WheelMotor` des Rennlenkrads oder einzeln durch das Aufrufen von Funktionen im Rückmeldungseffekt selbst gestartet, angehalten, fortgesetzt und gestoppt werden. In der Regel sollten Sie alle Effekte, die Sie verwenden möchten, vor Beginn des Spiels in das Rückmeldungsgerät laden und anschließend die jeweiligen `SetParameters`-Funktionen verwenden,, um die Effekte im Verlauf des Spiels zu aktualisieren.

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

Sie können das gesamte Kraftrückmeldungssystem auf einem bestimmten Rennlenkrad jederzeit asynchron aktivieren, deaktivieren oder zurücksetzen.

## <a name="see-also"></a>Weitere Informationen:

* [Windows.Gaming.Input.UINavigationController](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows. Gaming. Input. igamecontroller](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Eingabe Praktiken für Spiele](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[racingwheels]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheels.aspx
[racingwheeladded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheeladded.aspx
[racingwheelremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheelremoved.aspx
[haspatternshifter]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.haspatternshifter.aspx
[hashandbrake]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hashandbrake.aspx
[hasclutch]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hasclutch.aspx
[maxpatternshiftergear]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxpatternshiftergear.aspx
[maxwheelangle]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxwheelangle.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.getcurrentreading.aspx
[wheelmotor]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.wheelmotor.aspx
[racingwheelreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelreading.aspx
[racingwheelbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelbuttons.aspx
