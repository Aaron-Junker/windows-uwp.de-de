---
title: Steuerknüppel
description: Verwenden Sie die Flight-Stick-APIs von Windows. Gaming. Input zum Lesen von Eingaben aus Flight Sticks.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Input, Flight Stick
ms.localizationpriority: medium
ms.openlocfilehash: b9b4353a2736feb7cdfde9871c29f61de52e0c9a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156424"
---
# <a name="flight-stick"></a>Steuerknüppel

Auf dieser Seite werden die Grundlagen der Programmierung für Xbox One-zertifizierte Flight-Sticks mithilfe von [Windows. Gaming. Input. Flightstick](/uwp/api/windows.gaming.input.flightstick) und zugehörigen APIs für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:

* Erfassen einer Liste mit verbundenen Flight-Sticks und ihren Benutzern
* erkennen, dass ein flugstick hinzugefügt oder entfernt wurde
* Lesen von Eingaben von mindestens einem flugsticks
* Verhalten von Flight-Sticks als Benutzeroberflächen-Navigationsgeräte

## <a name="overview"></a>Überblick

Bei Flight Sticks handelt es sich um Spiele Eingabegeräte, die für das Reproduzieren der Meinung von flugsticks, die sich im Cockpit einer Ebene oder des Netz Schiffs befinden würden. Sie sind das perfekte Eingabegerät für die schnelle und exakte Kontrolle des Flugs. Flight-Sticks werden in Windows 10-und Xbox One-UWP-Apps durch den [Windows. Gaming. Input](/uwp/api/windows.gaming.input) -Namespace unterstützt.

Xbox One-flugsticks sind mit den folgenden Steuerelementen ausgestattet:

* Ein twistable-Analog-Joystick, der Roll, Pitch und Yaw unterstützt.
* Eine analoge Drosselung
* Zwei Feuer Schaltflächen
* Ein 8-Wege-Digital hat-Switch
* **Anzeigen** und **Menü** Schaltflächen

> [!NOTE]
> Die Schaltflächen **Ansicht** und **Menü** werden zur Unterstützung der Navigation in der Benutzeroberfläche, nicht zum Aufrufen von-Befehlen verwendet

### <a name="ui-navigation"></a>Benutzeroberflächennavigation

Um die Unterstützung der verschiedenen Eingabegeräte für die Navigation in der Benutzeroberfläche zu vereinfachen und die Konsistenz zwischen spielen und Geräten zu fördern, fungieren die meisten _physischen_ Eingabegeräte gleichzeitig als separate _logische_ Eingabegeräte, die als [UI-Navigations Controller](ui-navigation-controller.md)bezeichnet werden. Der Benutzeroberflächen-Navigationscontroller stellt ein gemeinsames Vokabular für Benutzeroberflächen-Navigationsbefehle über verschiedene Eingabegeräte hinweg bereit.

Als Benutzeroberflächen-Navigations Controller ordnet ein flugstick die [erforderlichen](ui-navigation-controller.md#required-set) Navigations Befehle den Schaltflächen "Joystick" und " **Ansicht**", " **Menü**", " **fireprimary**" und " **firesecondary** " zu.

| Navigationsbefehl | Flight-Stick-Eingabe                  |
| ------------------:| ----------------------------------- |
|                 Nach oben | Joystick nach oben                         |
|               Nach unten | Joystick nach unten                       |
|               Left | Joystick nach links                       |
|              Right | Joystick nach rechts                      |
|               Sicht | **Anzeige** Schaltfläche                     |
|               Menü | **Menü** Schaltfläche                     |
|             Akzeptieren | **Fireprimary** -Schaltfläche              |
|             Abbrechen | **Firesecondary** -Schaltfläche            |

Flight-Sticks ordnen keinen der optionalen Navigations Befehls [Sätze](ui-navigation-controller.md#optional-set) zu.

## <a name="detect-and-track-flight-sticks"></a>Erkennen und Nachverfolgen von flugsticks

Das erkennen und Nachverfolgen von Flight Sticks funktioniert genauso wie für Gamepads, außer mit der [Flightstick](/uwp/api/windows.gaming.input.flightstick) -Klasse anstelle der [Gamepad](/uwp/api/Windows.Gaming.Input.Gamepad) -Klasse. Weitere Informationen finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md) .

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>Lesen des Flug Sticks

Nachdem Sie den gewünschten flugwert identifiziert haben, sind Sie bereit, Eingaben daraus zu erfassen. Anders als bei einigen anderen Arten von Eingaben, die möglicherweise verwendet werden, kommunizieren die Zustandsänderungen jedoch nicht durch das Durchsetzen von Ereignissen. Stattdessen müssen Sie den aktuellen Status regelmäßig lesen, indem Sie ihn _abrufen_.

### <a name="polling-the-flight-stick"></a>Abrufen des Flug Sticks

Bei der Abfrage wird eine Momentaufnahme des Flug Sticks zu einem bestimmten Zeitpunkt erfasst. Diese Vorgehensweise für die Eingabe Erfassung eignet sich gut für die meisten Spiele, da ihre Logik in der Regel in einer deterministischen Schleife ausgeführt wird, anstatt Sie als ereignisgesteuert zu betrachten. Es ist in der Regel einfacher, Spiel Befehle aus Eingaben zu interpretieren, die alle gleichzeitig gesammelt werden, als aus vielen einzelnen Eingaben, die im Laufe der Zeit gesammelt werden.

Sie rufen einen flugstick ab, indem Sie [Flightstick. getcurrentreading](/uwp/api/windows.gaming.input.flightstick.GetCurrentReading)aufrufen. Diese Funktion gibt ein [flightstickreading](/uwp/api/windows.gaming.input.flightstickreading) zurück, das den Zustand des Flug Sticks enthält.

Im folgenden Beispiel wird ein flugstick für seinen aktuellen Zustand abgefragt:

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

Zusätzlich zum Status des Flug Stapels enthält jede Lesevorgang einen Zeitstempel, der genau angibt, wann der Status abgerufen wurde. Der Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten vorheriger Ablesungen oder zum Zeitpunkt der Spielsimulation herzustellen.

### <a name="reading-the-joystick-and-throttle-input"></a>Lesen der Joystick-und Drosselungs Eingabe

Der Joystick bietet eine analoge Lesevorgang zwischen-1,0 und 1,0 in der X-, Y-und Z-Achse ("Roll", "Pitch" bzw. "Yaw"). Für "Roll" entspricht der Wert "-1,0" der am weitesten links gerichteten Joystick Position, während der Wert 1,0 der äußersten rechten Position entspricht. Bei der Tonhöhe entspricht der Wert-1,0 der untersten Joystick Position, während der Wert 1,0 der obersten Position entspricht. Bei einem Wert von "-1,0" entspricht der Wert "-" der am weitesten gegen den Uhrzeigersinn verdrehten Position, während der Wert 1,0 der am weitesten Uhrzeigersinn enden Position entspricht.

Bei allen Achsen ist der Wert ungefähr 0,0, wenn sich der Joystick in der Mittelpunkt Position befindet, aber es ist normal, dass der genaue Wert auch bei nachfolgenden Messungen variiert. Strategien zum mindern dieser Variation werden weiter unten in diesem Abschnitt erläutert.

Der Wert für den rollforwardrollup wird aus der [flightstickreading. Roll](/uwp/api/windows.gaming.input.flightstickreading.Roll) -Eigenschaft gelesen. der Wert der Tonhöhe wird aus der [flightstickreading. Pitch](/uwp/api/windows.gaming.input.flightstickreading.Pitch) -Eigenschaft gelesen, und der Wert der Yaw wird aus der [flightstickreading. Yaw](/uwp/api/windows.gaming.input.flightstickreading.Yaw) -Eigenschaft gelesen:

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

Wenn Sie die Joystick Werte lesen, werden Sie bemerken, dass Sie nicht zuverlässig ein neutrales Lesen von 0,0 ergeben, wenn sich der Joystick in der Mitte befindet. Stattdessen werden bei jedem Verschieben des Joysticks unterschiedliche Werte in der Nähe von 0,0 erzeugt. Zur Kompensierung dieser Abweichungen können Sie einen kleinen _inaktiven Bereich_ implementieren (also einen zu ignorierenden Wertebereich nahe der idealen Mittelposition).

Eine Möglichkeit, eine deadzone zu implementieren, besteht darin, zu bestimmen, wie weit der Joystick von der Mitte verschoben wurde, und die Messwerte zu ignorieren, die sich in der Nähe der von Ihnen gewählten Entfernung befinden. Sie können die Entfernung ungefähr so berechnen, &mdash; dass Sie nicht genau ist, da die Joystick-Messwerte im Grunde Polar sind, nicht planar, &mdash; sondern nur mit dem Pythagorean-Theorem. Dadurch entsteht ein radialer inaktiver Bereich.

Im folgenden Beispiel wird eine grundlegende radiale deadzone mit dem Pythagorean-Theorem veranschaulicht:

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>Lesen der Schaltflächen und des hat-Schalters

Jede der beiden Feuer Schaltflächen des flugsticks bietet ein digitales Lesematerial, das angibt, ob es gedrückt oder freigegeben wird (nach oben). Aus Effizienzgründen werden Schaltflächen Messwerte nicht als einzelne boolesche Werte dargestellt &mdash; , sondern alle in ein einzelnes Bitfeld gepackt, das durch die [flightstickbuttons](/uwp/api/windows.gaming.input.flightstickbuttons) -Enumeration dargestellt wird. Außerdem bietet der 8-Wege-hat-Schalter eine Richtung, die in ein einzelnes Bitfeld gepackt ist, das von der [gamecontrollerswitchposition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition) -Enumeration repräsentiert wird.

> [!NOTE]
> Flugsticks sind mit zusätzlichen Schaltflächen ausgestattet, die **für die Benutzer** Oberflächen Navigation verwendet **Menu** werden. Diese Schaltflächen sind nicht Teil der `FlightStickButtons` -Enumeration und können nur gelesen werden, indem Sie als Navigationsgerät für die Benutzeroberfläche auf den flugstick zugreifen. Weitere Informationen finden Sie unter [UI-Navigations Controller](ui-navigation-controller.md).

Die Schaltflächen Werte werden aus der [flightstickreading. Buttons](/uwp/api/windows.gaming.input.flightstickreading.Buttons) -Eigenschaft gelesen. Da diese Eigenschaft ein Bitfeld ist, wird eine bitweise Maskierung verwendet, um den Wert der Taste zu isolieren, an der Sie interessiert sind, Wenn das entsprechende Bit festgelegt ist, wird die Schaltfläche gedrückt (unten). Andernfalls wird Sie freigegeben (up).

Im folgenden Beispiel wird bestimmt, ob die **fireprimary** -Schaltfläche gedrückt wird:

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

Im folgenden Beispiel wird bestimmt, ob die **fireprimary** -Schaltfläche freigegeben wird:

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

Manchmal möchten Sie möglicherweise ermitteln, ob eine Schaltfläche von gedrückter zu freigegeben oder freigegeben ist, ob mehrere Schaltflächen gedrückt oder freigegeben sind oder ob ein Satz von Schaltflächen auf eine bestimmte Weise angeordnet &mdash; ist, nicht. Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) sowie unter [Erkennen von komplexen Tastenanordnungen](input-practices-for-games.md#detecting-complex-button-arrangements).

Der hat-Schalter Wert wird aus der [flightstickreading. hatswitch](/uwp/api/windows.gaming.input.flightstickreading.HatSwitch) -Eigenschaft gelesen. Da diese Eigenschaft auch ein Bitfeld ist, wird die bitweise Maskierung erneut verwendet, um die Position des hat-Schalters zu isolieren.

Im folgenden Beispiel wird ermittelt, ob sich der hat-Switch in der aufwärts-Position befindet:

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

Im folgenden Beispiel wird bestimmt, ob sich der hat-Switch in der Mitte befindet:

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>Weitere Informationen

* [Windows. Gaming. Input. UINavigationController-Klasse](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows. Gaming. Input. igamecontroller-Schnittstelle](/uwp/api/windows.gaming.input.igamecontroller)
* [Eingabemethoden für Spiele](input-practices-for-games.md)