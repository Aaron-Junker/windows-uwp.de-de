---
title: Rennlenkräder und Kraftrückmeldung
description: Verwenden Sie die Windows.Gaming.Input-APIs für Rennlenkräder, um Funktionen zu erkennen und zu ermitteln sowie Kraftrückmeldungsbefehle zu lesen und an Rennlenkräder zu senden.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, Games, Rennrad, Erzwingen von Feedback
ms.localizationpriority: medium
ms.openlocfilehash: 0d92fe45a008ee0c3864355faf8133ab8e1ebefb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163064"
---
# <a name="racing-wheel-and-force-feedback"></a>Rennlenkräder und Kraftrückmeldung

Auf dieser Seite werden die Grundlagen der Programmierung für Xbox One-Rennräder mithilfe von [Windows. Gaming. Input. racingwheel][racingwheel] und zugehörigen APIs für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:

* Erfassen einer Liste mit verbundenen Rennrädern und ihren Benutzern
* erkennen, dass ein Rennrad hinzugefügt oder entfernt wurde
* Lesen von Eingaben von einem oder mehreren Rennrädern
* Wie Sie Kraftrückmeldungsbefehle senden
* Verhalten von Rennrädern als UI-Navigationsgeräte

## <a name="racing-wheel-overview"></a>Übersicht über Rennlenkräder

Rennlenkräder sind Eingabegeräte, die Benutzern das Gefühl vermitteln, in einem echten Rennwagencockpit zu sitzen. Rennlenkräder sind das ideale Eingabegerät für Rennsportspiele im Arcade- und Simulationsstil, die Autos oder Trucks enthalten. Rennlenkräder werden in Windows 10- und Xbox One-UWP-Apps durch den Namespace [Windows.Gaming.Input](/uwp/api/windows.gaming.input) unterstützt.

Xbox One-Rennräder werden zu einer Vielzahl von Preispunkten angeboten, die im Allgemeinen mehr und bessere Eingabe-und Erzwingungs Funktionen haben, wenn Ihre Preispunkte steigen. Alle Rennwagen sind mit einem analogen steuerungrad, einer analogen Drosselung und einem Brems Steuerelement und einigen Schaltflächen ausgestattet. Einige Rennwagen sind zusätzlich mit analogen Kupplungs-und Hand Shake Steuerelementen, mustershifters und Funktionen zum Erzwingen von Feedback ausgestattet. Nicht alle Rennwagen sind mit denselben Features ausgestattet und können auch Unterstützung für bestimmte Features unterstützen, z. b. können die Steuerelemente &mdash; verschiedene Bereiche der Drehung unterstützen, und mustershifter unterstützen möglicherweise eine unterschiedliche Anzahl von Gängen.

### <a name="device-capabilities"></a>Gerätefunktionen

Verschiedene Xbox One-Rennräder bieten unterschiedliche Sätze Optionaler Gerätefunktionen und unterschiedliche Unterstützungsstufen für diese Funktionen. Diese Abweichung zwischen einer einzelnen Art von Eingabegerät ist für die Geräte, die von der [Windows. Gaming. Input](/uwp/api/windows.gaming.input) -API unterstützt werden, eindeutig. Darüber hinaus unterstützen die meisten Geräte, denen Sie begegnen werden, zumindest einige optionale Funktionen oder andere Abweichungen. Aus diesem Grund ist es wichtig, die Funktionen jedes verbundenen Renn Rades einzeln zu bestimmen und die vollständige Variation der Funktionen zu unterstützen, die für Ihr Spiel sinnvoll sind.

Weitere Informationen finden Sie unter [Ermitteln von Rennlenkradfunktionen](#determining-racing-wheel-capabilities).

### <a name="force-feedback"></a>Kraftrückmeldung

Einige Xbox One-Rennräder bieten echte Rückmeldungen, d &mdash; . h., Sie können tatsächliche Erzwingungen auf eine Steuerungs Achse anwenden, z. b. das Mausrad, &mdash; nicht nur einfache Vibrationen. Spiele verwenden diese Möglichkeit, um einen besseren Einblick zu schaffen (_simulierte Absturz Schäden_, "Fahrverhalten") und die Herausforderung, eine gute Leistung zu erzielen.

Weitere Informationen finden Sie unter [Übersicht über die Kraftrückmeldung](#force-feedback-overview).

### <a name="ui-navigation"></a>Benutzeroberflächennavigation

Um den Aufwand für die Unterstützung unterschiedlicher Eingabegeräte für die Benutzeroberflächennavigation zu verringern und die Konsistenz zwischen Spielen und Geräten zu fördern, dienen die meisten _physischen_ Eingabegeräte gleichzeitig als getrennte _logische_ Eingabegeräte, die als [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md) bezeichnet werden. Der Benutzeroberflächen-Navigationscontroller stellt ein gemeinsames Vokabular für Benutzeroberflächen-Navigationsbefehle über verschiedene Eingabegeräte hinweg bereit.

Aufgrund des einzigartigen Schwerpunkts auf die analogen Steuerelemente und den Grad der Variation zwischen verschiedenen Rennrädern sind Sie in der Regel mit einer digitalen D-Pad-, **Ansichts** **-,** **Menü**-, **B**-, **X**-und **Y** -Schaltfläche ausgestattet, die denen eines [Gamepad](gamepad-and-vibration.md)ähnelt. Diese Schaltflächen sind nicht zur Unterstützung von Spiel Befehlen gedacht und können nicht ohne weiteres als Mausrad-Schaltflächen aufgerufen werden

Bei einem UI-Navigations Controller werden die erforderlichen Navigations Befehle auf den Schalt [Flächen](ui-navigation-controller.md#required-set) Linker, D-Pad, **Ansicht**, **Menü**, **a**und **B** angezeigt.

| Navigationsbefehl | Rennlenkradeingabe |
| ------------------:| ------------------ |
|                 Nach oben | Steuerkreuz nach oben           |
|               Nach unten | Steuerkreuz nach unten         |
|               Left | Steuerkreuz nach links         |
|              Right | Steuerkreuz nach rechts        |
|               Sicht | Ansicht-Taste        |
|               Menü | Menü-Taste        |
|             Akzeptieren | A-Taste           |
|             Abbrechen | B-Taste           |

Darüber hinaus ordnen einige Rennlenkräder möglicherweise einige [optionale](ui-navigation-controller.md#optional-set) Navigationsbefehle anderen von ihnen unterstützten Eingaben zu. Die Befehlszuordnungen können sich jedoch von Gerät zu Gerät unterscheiden. Sie sollten die Unterstützung dieser Befehle ebenfalls in Betracht ziehen, dabei jedoch sicherstellen, dass diese Befehle für die Navigation in der Benutzeroberfläche Ihres Spiels nicht notwendig sind.

| Navigationsbefehl | Rennlenkradeingabe    |
| ------------------:| --------------------- |
|            BILD-AUF | _variiert_              |
|          BILD-AB | _variiert_              |
|          Seite nach links | _variiert_              |
|         Seite nach rechts | _variiert_              |
|          Bildlauf nach oben | _variiert_              |
|        Bildlauf nach unten | _variiert_              |
|        Bildlauf nach links | _variiert_              |
|       Bildlauf nach rechts | _variiert_              |
|          Kontext 1 | X-Schaltfläche (_häufig_) |
|          Kontext 2 | Y-Taste (_in der Regel_) |
|          Kontext 3 | _variiert_              |
|          Kontext 4 | _variiert_              |

## <a name="detect-and-track-racing-wheels"></a>Erkennen und Nachverfolgen von Rennlenkrädern

Das erkennen und Nachverfolgen von Rennrädern funktioniert genauso wie bei Gamepads, außer bei der Klasse " [racingwheel](/uwp/api/windows.gaming.input.racingwheel) " anstelle der [Gamepad](/uwp/api/Windows.Gaming.Input.Gamepad) -Klasse. Weitere Informationen finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md) .

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) class provides a static property, [RacingWheels](/uwp/api/windows.gaming.input.racingwheel.racingwheels#Windows_Gaming_Input_RacingWheel_RacingWheels), which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

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

When a racing wheel is added or removed the [RacingWheelAdded](/uwp/api/windows.gaming.input.racingwheel.racingwheeladded) and [RacingWheelRemoved](/uwp/api/windows.gaming.input.racingwheel.racingwheelremoved) events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

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

## <a name="reading-the-racing-wheel"></a>Lesen der Rennlenkradwerte

Nachdem Sie die für Sie interessanten Rennräder identifiziert haben, sind Sie bereit, Eingaben von Ihnen zu erfassen. Anders als im Fall anderer Eingaben, die Sie möglicherweise kennen, teilen Rennlenkräder Zustandsänderungen jedoch nicht durch das Auslösen von Ereignissen mit. Stattdessen nehmen Sie regelmäßige Messungen Ihrer aktuellen Zustände vor, indem Sie Sie _Abfragen_ .

### <a name="polling-the-racing-wheel"></a>Abrufen von Rennlenkrädern

Durch den Abruf wird eine Momentaufnahme des Rennlenkrads zu einem genau festgelegten Zeitpunkt erfasst. Diese Vorgehensweise für die Eingabe Erfassung eignet sich gut für die meisten Spiele, da ihre Logik in der Regel in einer deterministischen Schleife ausgeführt wird, anstatt Sie als ereignisgesteuert zu betrachten. Es ist in der Regel einfacher, Spiel Befehle aus Eingaben zu interpretieren, die alle gleichzeitig gesammelt werden, als aus vielen einzelnen Eingaben, die im Laufe der Zeit gesammelt werden.

Sie rufen ein Rennrad durch Aufrufen von [getcurrentreading](/uwp/api/windows.gaming.input.racingwheel.getcurrentreading#Windows_Gaming_Input_RacingWheel_GetCurrentReading); ab. Diese Funktion gibt ein [racingwheelreading](/uwp/api/windows.gaming.input.racingwheelreading) zurück, das den Zustand des Renn Rades enthält.

Im folgenden Beispiel wird der aktuelle Zustand eines Rennlenkrads abgerufen.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

Zusätzlich zum Zustand des Rennlenkrads enthält jede Ablesung einen Zeitstempel, der den genauen Zeitpunkt angibt, an dem der Zustand abgerufen wurde. Der Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten vorheriger Ablesungen oder zum Zeitpunkt der Spielsimulation herzustellen.

### <a name="determining-racing-wheel-capabilities"></a>Ermitteln der Rennlenkradfunktionalität

Viele Steuerelemente eines Rennlenkrads sind optional oder unterstützen verschiedene Varianten. Da dies auch für erforderliche Steuerelemente gilt, müssen Sie die Funktionalität jedes Rennlenkrads einzeln ermitteln, bevor Sie die Eingaben verarbeiten können, die mit den Ablesungen des Rennlenkrads erfasst wurden.

Optionale Steuerelemente sind Handbremse, Kupplung und Gangschaltung. Sie können ermitteln, ob ein verbundenes Rennlenkrad diese Steuerelemente unterstützt, indem Sie die Eigenschaften [HasHandbrake](/uwp/api/windows.gaming.input.racingwheel.hashandbrake#Windows_Gaming_Input_RacingWheel_HasHandbrake), [HasClutch](/uwp/api/windows.gaming.input.racingwheel.hasclutch#Windows_Gaming_Input_RacingWheel_HasClutch) bzw. [HasPatternShifter](/uwp/api/windows.gaming.input.racingwheel.haspatternshifter#Windows_Gaming_Input_RacingWheel_HasPatternShifter) des Rennlenkrads lesen. Das-Steuerelement wird unterstützt, wenn der Wert der-Eigenschaft **true**ist. Andernfalls wird Sie nicht unterstützt.

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

Die Steuerelemente, deren Funktionalität variieren kann, sind das Lenkrad und die Gangschaltung. Das Lenkrad kann hinsichtlich des Umfangs der physischen Drehung variieren, die durch das tatsächliche Lenkrad unterstützt wird. Die Gangschaltung kann hinsichtlich der Zahl der einzelnen Vorwärtsgänge variieren, die unterstützt werden. Sie können den größten, durch das tatsächliche Lenkrad unterstützten Drehwinkel ermitteln, indem Sie die Eigenschaft `MaxWheelAngle` des Rennlenkrads lesen. Dessen Wert entspricht dem maximal unterstützten physischen Winkel im Uhrzeigersinn, ausgedrückt in Grad (positive Gradangabe). Der gleiche Drehwinkel wird in der dem Uhrzeigersinn entgegengesetzten Richtung unterstützt (negative Gradangabe). Sie können das größte Forward-Zahnrad festlegen, das der mustershifter unterstützt, indem Sie die- `MaxPatternShifterGear` Eigenschaft des-baurades lesen. sein Wert ist die höchste unterstützte Vorwärtsbewegung, d. h &mdash; ., wenn der Wert 4 ist, unterstützt der mustershifter umgekehrte, neutrale, erste, zweite, dritte und vierte Gänge.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

Schließlich unterstützen einige Rennlenkräder die Kraftrückmeldung über das Lenkrad. Sie können ermitteln, ob ein verbundenes Rennlenkrad die Kraftrückmeldung unterstützt, indem Sie die Eigenschaft [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) des Rennlenkrads lesen. Das Erzwingen von Feedback wird unterstützt `WheelMotor` , wenn nicht **null**ist; andernfalls wird es nicht unterstützt.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

Weitere Informationen dazu, wie Sie die Kraftrückmeldungsfunktion der Rennlenkräder verwenden, die dies unterstützen, finden Sie unter [Übersicht über die Kraftrückmeldung](#force-feedback-overview).

### <a name="reading-the-buttons"></a>Lesen der Tastenwerte

Jedes der Mausrad &mdash; Schaltflächen, die vier Richtungen des D-Pads, die **vorherigen Zahnrad** -und **nächsten Zahnrad** Schaltflächen und 16 zusätzliche Schaltflächen, &mdash; bietet ein digitales Lesematerial, das angibt, ob es gedrückt oder freigegeben wird (nach oben). Aus Effizienzgründen werden die Ablesewerte der Tasten nicht als einzelne boolesche Werte angezeigt, sondern in einem einzelnen Bitfeld zusammengefasst, dargestellt durch die Enumeration [RacingWheelButtons](/uwp/api/windows.gaming.input.racingwheelbuttons).

> [!NOTE]
> Starträder sind mit zusätzlichen Schaltflächen für die Benutzeroberflächen Navigation ausgestattet, z. b. die **Ansicht** und die **Menü** Schaltflächen. Diese Tasten sind nicht in der Enumeration `RacingWheelButtons` enthalten und können nur gelesen werden, wenn auf das Rennlenkrad als Benutzeroberflächen-Navigationsgerät zugegriffen wird. Weitere Informationen finden Sie unter [Benutzeroberflächen-Navigationsgerät](ui-navigation-controller.md).

Die Tastenwerte werden aus der Eigenschaft `Buttons` der Struktur [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) gelesen. Da diese Eigenschaft ein Bitfeld ist, wird eine bitweise Maskierung verwendet, um den Wert der Taste zu isolieren, an der Sie interessiert sind, Wenn das entsprechende Bit festgelegt ist, wird die Schaltfläche gedrückt (unten). Andernfalls wird Sie freigegeben (up).

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

Manchmal möchten Sie möglicherweise ermitteln, ob eine Schaltfläche von gedrückter zu freigegeben oder freigegeben ist, ob mehrere Schaltflächen gedrückt oder freigegeben sind oder ob ein Satz von Schaltflächen auf eine bestimmte Weise angeordnet &mdash; ist, nicht. Weitere Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) und [Erkennen von komplexen Tastenanordungen](input-practices-for-games.md#detecting-complex-button-arrangements).

### <a name="reading-the-wheel"></a>Lesen der Lenkradwerte

Das Lenkrad ist ein erforderliches Steuerelement, das einen analogen Ablesewert zwischen -1,0 und +1,0 bereitstellt. Der Wert -1,0 entspricht der äußersten linken Lenkradposition. Der Wert +1,0 entspricht der äußersten rechten Lenkradposition. Der Wert für das Lenkrad wird aus der Eigenschaft `Wheel` der Struktur [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) gelesen.

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

Auch wenn die Lenkradwerte dem Grad der physischen Drehung des tatsächlichen Lenkrads entsprechen, abhängig vom durch das physische Rennlenkrad unterstützten Drehbereich, sollten Sie die Lenkradwerte nicht skalieren. Lenkräder, die einen größeren Drehbereich unterstützen, bieten lediglich eine größere Genauigkeit.

### <a name="reading-the-throttle-and-brake"></a>Lesen der Gas- und Bremsensteuerelementwerte

Die Drosselung und die Bremse sind erforderliche Steuerelemente, die jeweils analoge Messwerte zwischen 0,0 (vollständig freigegeben) und 1,0 (vollständig gedrückt) bereitstellen, die als Gleit Komma Werte dargestellt werden. Der Wert für das Gassteuerelement wird aus der Eigenschaft `Throttle` der Struktur [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) gelesen. Der Wert für das Bremsensteuerelement wird aus der Eigenschaft `Brake` gelesen.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>Lesen der Handbremsen- und Kupplungswerte

Die Handbremsen- und Kupplungssteuerelemente sind optionale Steuerelemente, die jeweils analoge Ablesewerte zwischen 0,0 (vollständig freigegeben) und 1,0 (vollständig gedrückt) bereitstellen, dargestellt als Gleitkommawerte. Der Wert für das Handbremsensteuerelement wird aus der Eigenschaft `Handbrake` der Struktur [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) gelesen. Der Wert für das Kupplungssteuerelement wird aus der Eigenschaft `Clutch` gelesen.

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

Die Gangschaltung ist ein optionales Steuerelement, das einen digitalen Wert zwischen -1 und [MaxPatternShifterGear](/uwp/api/windows.gaming.input.racingwheel.maxpatternshiftergear) bereitstellt, dargestellt als ganze Zahl mit Vorzeichen. Der Wert-1 oder 0 entspricht den _umgekehrten_ bzw. _neutralen_ Gängen. zunehmend positive Werte entsprechen den größeren vorwärts Gängen bis zu **maxpatternshiftergear**(einschließlich). Der Wert des mustershifter wird aus der Eigenschaft " [patternshibertergear](/uwp/api/windows.gaming.input.racingwheelreading.patternshiftergear) " der " [racingwheelreading](/uwp/api/windows.gaming.input.racingwheelreading) "-Struktur gelesen.

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> Der Muster-Shifter, sofern er unterstützt wird, ist neben den erforderlichen **vorherigen** Zahnrad-und **nächsten Zahnrad** Schaltflächen vorhanden, die sich auch auf den aktuellen Gang des Players des Players auswirken. Eine einfache Strategie zum vereinheitlichen dieser Eingaben, bei denen beide vorhanden sind, besteht darin, den mustershifter (und die-Kupplung) zu ignorieren, wenn ein Player eine automatische Übertragung für das Auto auswählt und die **Schalt** Flächen "zurück" und "weiter" ignoriert, wenn ein Spieler eine manuelle Übertragung für das Fahrzeug auswählt, **Wenn das** Rennrad mit einem Muster Shifter Wenn dies für Ihr Spiel nicht geeignet ist, können Sie eine andere Strategie für die Zusammenführung implementieren.

## <a name="run-the-inputinterfacing-sample"></a>Ausführen des InputInterfacing-Beispiels

Das [InputInterfacingUWP-Beispiel _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) zeigt, wie Rennlenkräder und verschiedene Arten von Eingabegeräten zusammen verwendet werden und wie sich diese Eingabegeräte bei Verwendung als Benutzeroberflächen-Navigationscontroller verhalten.

## <a name="force-feedback-overview"></a>Übersicht über die Kraftrückmeldung

Viele Rennwagen verfügen über eine Funktion zum Erzwingen von Feedback, um eine immersive und schwierigere Fahrleistung zu bieten. Rennlenkräder, die die Kraftrückmeldung unterstützen, besitzen in der Regel einen einzelnen Motor, der entlang einer einzelnen Achse (der Achse der Lenkraddrehung) Kraft auf das Lenkrad ausübt. Das Erzwingen von Feedback wird in Windows 10-und Xbox One-UWP-Apps durch den [Windows. Gaming. Input. ForceFeedback](/uwp/api/windows.gaming.input.forcefeedback) -Namespace unterstützt.

> [!NOTE]
> Die APIs für das Erzwingen von Feedback können mehrere Achsen von Force unterstützen, aber kein Xbox One-Rennrad unterstützt derzeit keine Feedback Achse außer der Radrotation.

## <a name="using-force-feedback"></a>Verwenden der Kraftrückmeldung

In den folgenden Abschnitten werden die Grundlagen der Programmierung von Kraftrückmeldungseffekten für Xbox One-Rennlenkräder beschrieben. Die Rückmeldung wird mithilfe von Effekten angewendet, die zuerst in das Rückmeldungsgerät geladen und anschließend gestartet, angehalten, fortgesetzt und gestoppt werden können, ähnlich wie Soundeffekte. Sie müssen jedoch zunächst die Rückmeldungsfunktionalität des Rennlenkrads ermitteln.

### <a name="determining-force-feedback-capabilities"></a>Ermitteln der Kraftrückmeldungsfunktionalität

Sie können ermitteln, ob ein verbundenes Rennlenkrad die Kraftrückmeldung unterstützt, indem Sie die Eigenschaft [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) des Rennlenkrads lesen. Das Erzwingen von Feedback wird nicht unterstützt, wenn `WheelMotor` **null**ist. andernfalls wird das Erzwingen von Feedback unterstützt, und Sie können mit der Ermittlung der spezifischen Feedback Funktionen des Motors fortfahren, wie z. b. der Achsen, die sich

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

Die Kraftrückmeldungseffekte werden in das Rückmeldungsgerät geladen, auf dem sie autonom nach den Regeln Ihres Spiels „gespielt“ werden. Eine Reihe grundlegender Effekte werden bereitgestellt. benutzerdefinierte Effekte können über eine Klasse erstellt werden, die die [iforcefeedbackeffect](/uwp/api/windows.gaming.input.forcefeedback.iforcefeedbackeffect) -Schnittstelle implementiert.

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

## <a name="see-also"></a>Weitere Informationen

* [Windows.Gaming.Input.UINavigationController](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Eingabemethoden für Spiele](input-practices-for-games.md)

[Windows.Gaming.Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[racingwheel]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheels]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheeladded]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelremoved]: /uwp/api/Windows.Gaming.Input.RacingWheel
[haspatternshifter]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hashandbrake]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hasclutch]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxpatternshiftergear]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxwheelangle]: /uwp/api/Windows.Gaming.Input.RacingWheel
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.RacingWheel
[wheelmotor]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelreading]: /uwp/api/Windows.Gaming.Input.RacingWheelReading
[racingwheelbuttons]: /uwp/api/Windows.Gaming.Input.RacingWheelButtons