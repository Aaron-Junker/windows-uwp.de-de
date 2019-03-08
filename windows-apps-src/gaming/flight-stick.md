---
title: Steuerknüppel
description: Verwenden Sie die Windows.Gaming.Input-Steuerknüppel-APIs, um Eingaben von Steuerknüppeln zu lesen.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp, Spiele, Eingabe, Steuerknüppel
ms.localizationpriority: medium
ms.openlocfilehash: 5eceb30c62f1e803397aff71d59b560c39736cf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609015"
---
# <a name="flight-stick"></a>Steuerknüppel

Auf dieser Seite werden die Grundlagen der Programmierung für Xbox One-zertifizierte Steuerknüppel mit [Windows.Gaming.Input.FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) und verwandten APIs für die Universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:

* Wie Sie eine Liste der verbundenen Steuerknüppel und ihrer Benutzer erstellen
* Wie Sie ermitteln, ob ein Steuerknüppel hinzugefügt oder entfernt wurde
* Wie Sie die Eingabe von einem oder mehreren Steuerknüppeln lesen
* Wie sich Steuerknüppel als UI-Navigationsgeräte verhalten

## <a name="overview"></a>Übersicht

Steuerknüppel sind Spieleingabegeräte, die das Gefühl eines Steuerknüppels reproduzieren, der in Flugzeugen oder Raumschiffen benutzt wird. Sie sind das perfekte Eingabegerät für die schnelle und genaue Steuerung von Fluggeräten. Steuerknüppel werden in Windows 10- und Xbox One-UWP-Apps durch den Namespace [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) unterstützt.

Xbox One-Steuerknüppel sind mit den folgenden Steuerelementen ausgestattet:

* Einem drehbaren analogen Joystick, der Rollen, Nicken und Gieren ausführen kann
* Einem analogen Schubregler
* Zwei Feuern-Tasten
* Einem digitalen 8-Wegeschalter
* **Ansicht**- und **Menü**-Tasten

> [!NOTE]
> Die **Ansicht**- und **Menü**-Tasten werden zur Unterstützung der Benutzeroberflächennavigation, nicht der Spielverlaufsbefehle verwendet. Daher kann auf diese Tasten nicht ohne Weiteres als Joystick-Tasten zugegriffen werden.

### <a name="ui-navigation"></a>Benutzeroberflächennavigation

Um den Aufwand für die Unterstützung unterschiedlicher Eingabegeräte für die Benutzeroberflächennavigation zu verringern und die Konsistenz zwischen Spielen und Geräten zu fördern, dienen die meisten _physischen_ Eingabegeräte gleichzeitig als getrennte _logische_ Eingabegeräte, die als [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md) bezeichnet werden. Der Benutzeroberflächen-Navigationscontroller stellt ein gemeinsames Vokabular für Benutzeroberflächen-Navigationsbefehle über verschiedene Eingabegeräte hinweg bereit.

Ein Steuerknüppel als Benutzeroberflächen-Navigationscontroller ordnet den [erforderlichen Satz](ui-navigation-controller.md#required-set) an Navigationsbefehlen dem Joystick und den Tasten **Ansicht**, **Menü**, **FirePrimary** und **FireSecondary** zu.

| Navigationsbefehl | Steuerknüppeleingabe                  |
| ------------------:| ----------------------------------- |
|                 Nach oben | Joystick nach oben                         |
|               Nach unten | Joystick nach-unten                       |
|               Nach links | Joystick nach links                       |
|              Nach rechts | Joystick nach rechts                      |
|               Ansicht | **Ansicht**-Taste                     |
|               Menü | **Menü**-Taste                     |
|             Annehmen | **FirePrimary**-Taste              |
|             Abbrechen | **FireSecondary**-Taste            |

Steuerknüppel ordnen keinen der [optionalen Sätze](ui-navigation-controller.md#optional-set) an Navigationsbefehlen zu.

## <a name="detect-and-track-flight-sticks"></a>Ermitteln und Nachverfolgen von Steuerknüppeln

Ermitteln und Nachverfolgen von Steuerknüppeln funktioniert auf genau die gleiche Weise wie für Gamepads, außer bei der [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick)-Klasse anstelle der [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad)-Klasse. Weitere Informationen finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md).

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

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

When a flight stick is added or removed, the [FlightStickAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

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

## <a name="reading-the-flight-stick"></a>Lesen von Steuerknüppeleingaben

Nachdem Sie den Steuerknüppel identifiziert haben, für den Sie sich interessieren, können Sie Eingaben von ihm erfassen. Im Gegensatz zu anderen Eingaben, die Sie möglicherweise kennen, teilen Steuerknüppel Zustandsänderungen nicht durch das Auslösen von Ereignissen mit. Stattdessen müssen Sie den aktuellen Status regelmäßig lesen, indem Sie ihn _abrufen_.

### <a name="polling-the-flight-stick"></a>Abrufen des Zustands von Steuerknüppeln

Beim Abrufen wird ein Snapshot des Steuerknüppels zu einem bestimmten Zeitpunkt erstellt. Dieser Ansatz zur Eingabeerfassung ist für die meisten Spiele geeignet, da deren Logik normalerweise als deterministische Schleife, nicht ereignisgesteuert ausgeführt wird. Außerdem lassen sich Spielbefehle aus erfassten Eingaben einfacher interpretieren als aus vielen einzelnen Eingaben, die im Laufe der Zeit erfasst wurden.

Sie rufen den Zustand eines Steuerknüppels ab, indem Sie [FlightStick.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick.GetCurrentReading) aufrufen. Diese Funktion gibt eine [FlightStickReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading)-Struktur zurück, die den Zustand des Steuerknüppels enthält.

Im folgenden Beispiel wird der aktuelle Zustand eines Steuerknüppels abgerufen.

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

Zusätzlich zum Zustand des Steuerknüppels enthält jede Ablesung einen Zeitstempel, der den genauen Zeitpunkt angibt, zu dem der Zustand abgerufen wurde. Der Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten vorheriger Ablesungen oder zum Zeitpunkt der Spielsimulation herzustellen.

### <a name="reading-the-joystick-and-throttle-input"></a>Lesen der Joystick- und Schubreglereingabe

Der Joystick liefert analoge Werte zwischen -1,0 und 1,0 in der X-, Y- und Z-Achse ( Rollen, Nicken bzw. Gieren). Beim Rollen entspricht der Wert -1,0 auf der X-Achse der äußerst linken Joystickposition und der Wert 1,0 der äußerst rechten Position. Beim Nicken entspricht der Wert -1,0 der untersten Joystickposition und der Wert 1,0 der obersten Position. Beim Gieren entspricht der Wert -1,0 der äußersten gegen den Uhrzeigersinn gedrehten Position und der Wert 1,0 der äußersten im Uhrzeigersinn gedrehten Position.

Auf allen Achsen ist der Wert ungefähr 0,0, wenn sich der Joystick in der Mittelstellung befindet, aber normalerwiese weicht der genaue Wert etwas davon ab, sogar in aufeinanderfolgenden Messwerten. Strategien zur Minimierung dieser Abweichung werden weiter unten in diesem Abschnitt behandelt.

Der Wert beim Rollen des Joysticks wird aus der [FlightStickReading.Roll](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Roll)-Eigenschaft gelesen, der Wert beim Nicken aus der [FlightStickReading.Pitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Pitch)-Eigenschaft und der Wert beim Gieren aus der [FlightStickReading.Yaw](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Yaw)-Eigenschaft:

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

Sie werden feststellen, dass die gelesenen Joystickwerte nicht zuverlässig einen neutralen 0,0-Wert liefern, wenn sich der Joystick in der Mittelstellung (und damit im Ruhezustand) befindet. Stattdessen erhalten Sie verschiedene Näherungswerte für 0,0, wann immer der Joystick bewegt wurde und wieder in die Mittelstellung zurückkehrt. Zur Kompensierung dieser Abweichungen können Sie einen kleinen _inaktiven Bereich_ implementieren (also einen zu ignorierenden Wertebereich nahe der idealen Mittelposition).

Zur Implementierung eines inaktiven Bereichs können Sie beispielsweise ermitteln, wie weit sich der Joystick von der Mittelposition entfernt hat, und dabei die Werte ignorieren, die eine bestimmte, von Ihnen gewählte Entfernung unterschreiten. Die grobe Entfernung kann mit dem Satz des Pythagoras berechnet werden. Die Berechnung ist allerdings nicht exakt, da die Joystickwerte im Grunde polar und nicht planar sind. Dadurch entsteht ein radialer inaktiver Bereich.

Das folgende Beispiel zeigt einen radialen inaktiven Bereich, berechnet nach dem Satz des Pythagoras:

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

### <a name="reading-the-buttons-and-hat-switch"></a>Lesen der Tasten und des Mehrwegeschalters

Jede der beiden Feuern-Tasten des Steuerknüppels liefert einen digitalen Wert, der angibt, ob die Taste gedrückt ist (unten) oder nicht gedrückt (oben) ist. Aus Effizienzgründen werden die Werte der Tasten nicht als einzelne boolesche Werte dargestellt, sondern in einem einzelnen Bitfeld zusammengefasst, das durch die Enumeration [FlightStickButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickbuttons) dargestellt wird. Darüber hinaus bietet der 8-Wegeschalter eine in einem einzelnen Bitfeld zusammengefasste Richtung, die durch die Enumeration [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition) dargestellt wird.

> [!NOTE]
> Steuerknüppel sind mit zusätzlichen Tasten für die Benutzeroberflächennavigation ausgestattet, zum Beispiel mit den Tasten **Ansicht** und **Menü**. Diese Tasten sind nicht in der Enumeration `FlightStickButtons` enthalten und können nur gelesen werden, wenn auf den Steuerknüppel als Benutzeroberflächen-Navigationsgerät zugegriffen wird. Weitere Informationen finden Sie unter [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md).

Die Tastenwerte werden aus der [FlightStickReading.Buttons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Buttons)-Eigenschaft gelesen. Da diese Eigenschaft ein Bitfeld ist, wird eine bitweise Maskierung verwendet, um den Wert der Taste zu isolieren, an der Sie interessiert sind, Die Taste ist gedrückt (unten), wenn das entsprechende Bit festgelegt ist. Andernfalls ist sie nicht gedrückt (oben).

Im folgenden Beispiel wird ermittelt, ob die Taste **FirePrimary** gedrückt ist:

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

Im folgenden Beispiel wird ermittelt, ob die Taste **FirePrimary** nicht gedrückt ist:

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

In einigen Fällen möchten Sie vielleicht ermitteln, ob eine Taste von „gedrückt“ zu „nicht gedrückt“ bzw. umgekehrt wechselt, oder ob mehrere Tasten gedrückt bzw. nicht gedrückt oder in bestimmter Weise angeordnet sind&mdash;einige gedrückt, andere nicht. Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) sowie unter [Erkennen von komplexen Tastenanordnungen](input-practices-for-games.md#detecting-complex-button-arrangements).

Der Wert des Mehrwegeschalters wird aus der [FlightStickReading.HatSwitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.HatSwitch)-Eigenschaft gelesen. Da diese Eigenschaft ebenfalls ein Bitfeld ist, wird erneut die bitweise Maskierung verwendet, um die Position des Mehrwegeschalters zu ermitteln.

Im folgenden Beispiel wird ermittelt, ob sich der Mehrwegeschalter in der oberen Stellung befindet:

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

Im folgenden Beispiel wird ermittelt, ob sich der Mehrwegeschalter in der Mittelstellung befindet:

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>Siehe auch

* [Windows.Gaming.Input.UINavigationController class](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController-Schnittstelle](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Eingabe-Methoden für Spiele](input-practices-for-games.md)