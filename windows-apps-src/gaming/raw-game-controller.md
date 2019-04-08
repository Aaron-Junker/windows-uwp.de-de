---
title: Unformatierter Gamecontroller
description: Mithilfe der Windows.Gaming.Input-APIs für unformatierte Gamecontroller können Sie die Eingaben von fast jedem Gamecontrollertyp lesen.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Spiele, Eingabe, unformatierter Gamecontroller
ms.localizationpriority: medium
ms.openlocfilehash: 7b5f4d49ad49cf9f9065fe17788456e9dd2a4a4e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644625"
---
# <a name="raw-game-controller"></a>Unformatierter Gamecontroller

Diese Seite beschreibt die Grundlagen der Programmierung für fast jeden Gamecontrollertyp mithilfe von [Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) und weiterer APIs für die Universelle Windows-Plattform (UWP).

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:

* Eine Liste der verbundenen unformatierten Gamecontroller und ihrer Benutzer zu erstellen
* Zu ermitteln, ob ein unformatierter Gamecontroller hinzugefügt oder entfernt wurde
* Die Funktionen eines unformatierten Gamecontrollers abzurufen
* Die Eingaben eines unformatierten Gamecontrollers zu lesen

## <a name="overview"></a>Übersicht

Ein unformatierter Gamecontroller bietet eine generische Darstellung mit den typischen allgemeinen Eingaben eines Gamecontrollers. Diese Eingaben sind als einfache Arrays von unbenannten Schaltflächen, Hebeln und Knöpfen dargestellt. Mit einem unformatierten Gamecontroller kann ein Kunde benutzerdefinierte Eingabe-Zuordnungen erstellen, unabhängig von seinem verwendeten Controllertyp.

Die [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)-Klasse ist eigentlich für Fälle gedacht, wo andere Eingabeklassen (wie  [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) usw.) nicht Ihren allgemeinen Anforderungen entsprechen. Diese Klasse ist geeignet, wenn Ihre Kunden voraussichtlich viele verschiedene Gamecontrollertypen verwenden.

## <a name="detect-and-track-raw-game-controllers"></a>Ermitteln und Nachverfolgen unformatierter Gamecontroller

Ermitteln und Nachverfolgen von unformatierten Gamecontrollern funktioniert auf genau die gleiche Weise wie für Gamepads, außer bei der [Unformatierte Gamecontroller](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)-Klasse anstelle der [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad)-Klasse. Weitere Informationen finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md).

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>Abrufen der Funktionen eines unformatierten Gamecontrollers

Wenn Sie einen für Sie interessanten unformatierten Gamecontroller gefunden haben, können Sie Informationen zu seinen Funktionen einholen. Sie können mit [RawGameController.ButtonCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount) die Anzahl der Schaltflächen des unformatierten Gamecontrollers, mit [RawGameController.AxisCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount) die Anzahl seiner analogen Achsen und mit [RawGameController.SwitchCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount) die Anzahl seiner Schalter abrufen. Außerdem können Sie mit [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_) den Typ eines Schalters abrufen.

Das folgende Beispiel ruft den Eingabezähler eines unformatierten Gamecontrollers ab:

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

Das folgende Beispiel bestimmt den Typ jedes Schalters:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>Lesen des unformatierten Gamecontrollers

Sobald Sie die Anzahl der Eingaben auf einem unformatierten Gamecontroller kennen, können Sie beginnen die Eingaben zu erfassen. Im Unterschied zu anderen Eingaben, die Sie vielleicht kennen, teilen unformatierte Gamecontroller keine Zustandsänderungen durch das Auslösen von Ereignissen mit. Sie müssen seinen Zustand daher regelmäßig durch _Abrufe_ auslesen.

### <a name="polling-the-raw-game-controller"></a>Abrufen des unformatierten Gamecontrollers

Jeder Abruf erstellt eine Momentaufnahme des unformatierten Gamecontrollers zu einem bestimmten Zeitpunkt. Dieser Ansatz zur Eingabeerfassung ist für die meisten Spiele geeignet, da deren Logik normalerweise als deterministische Schleife, nicht ereignisgesteuert ausgeführt wird. Außerdem lassen sich Spielbefehle aus erfassten Eingaben einfacher interpretieren als aus vielen einzelnen Eingaben, die im Laufe der Zeit erfasst wurden.

Sie erfassen einen unformatierten Gamecontroller durch den Aufruf [RawGameController.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___). Diese Funktion füllt Arrays für Schaltflächen, Schalter und Achsen, die den Zustand des unformatierten Gamecontrollers enthalten.

Das folgende Beispiel ruft den aktuellen Zustand eines unformatierten Gamecontrollers ab.

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

Da nicht für jeden Controllertyp garantiert werden kann, welche Position in den Arrays welchen Eingabewert enthält, müssen diese Werte durch die Methoden [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) und [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_) ermittelt werden.

**GetButtonLabel** gibt den Text oder das Symbol auf einer physischen Taste an, nicht ihre Funktion und sollte daher nur als Hilfe genutzt werden, falls Sie dem Spieler auf der Benutzeroberfläche zeigen möchten, welche Taste welche Funktion ausführt. **GetSwitchKind** gibt die Art eines Schalters an (d. h. wie viele Positionen er hat), nicht seinen Namen.

Da es keine standardisierte Methode gibt, um die Bezeichnungen von Achsen oder Schaltern abzurufen, müssen Sie diese durch Eingaben selbst ermitteln.

Wenn Sie einen bestimmten Controller unterstützen möchten, können Sie durch die Aufrufe [RawGameController.HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) und [RawGameController.HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) ermitteln, ob er unterstützt wird. Da die Position jeder Eingabe in einem Array für alle Controller mit gleicher **HardwareProductId** und **HardwareVendorId** gilt, müssen Sie nicht befürchten, dass Ihre Logik möglicherweise nicht zu den verschiedenen Controllern eines Typs passt.

Jeder Abruf des Zustands eines unformatierten Gamecontrollers gibt auch einen Zeitstempel zurück, der den genauen Zeitpunkt des Abrufs angibt. Der Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten vorheriger Ablesungen oder zum Zeitpunkt der Spielsimulation herzustellen.

### <a name="reading-the-buttons-and-switches"></a>Lesen von Schaltflächen und Tasten

Jede Schaltfläche eines unformatierten Gamecontrollers liefert einen digitalen Wert, der den gedrückten (unten) oder losgelassenen (oben) Zustand angibt. Schaltflächenwerte werden als einzelne boolesche Werte in einem Array dargestellt. Die Bezeichnung jeder Schaltfläche ermitteln Sie durch [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) und den Index des booleschen Werts im Array. Jeder Wert wird als [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) dargestellt.

Im folgenden Beispiel wird ermittelt, ob die Taste **XboxA** gedrückt ist:

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

In einigen Fällen möchten Sie vielleicht ermitteln, ob eine Taste von „gedrückt“ zu „nicht gedrückt“ bzw. umgekehrt wechselt, oder ob mehrere Tasten gedrückt bzw. nicht gedrückt oder in bestimmter Weise angeordnet sind&mdash;einige gedrückt, andere nicht. Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) sowie unter [Erkennen von komplexen Tastenanordnungen](input-practices-for-games.md#detecting-complex-button-arrangements).

Schalterwerte werden als Array mit [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition) angegeben. Diese Eigenschaft ist ein Bitfeld und wird deshalb bitweise maskiert, um die Richtung des Schalters zu isolieren.

Das folgende Beispiel ermittelt, ob sich alle Schalter in der Stellung „oben” befinden:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>Ablesen von analogen Eingaben (Sticks, Trigger, Drosselungen usw.)

Eine analoge Achse liefert Werte zwischen 0,0 und 1,0. Dazu gehören die einzelnen Dimensionen auf einem Stick, z. B. X und Y für Standardsticks, oder auch X-, Y- und Z-Achsen (für Rollen, Neigen und Schwenken) für Steuerknüppel.

Die Werte können jeden analogen Eingabetyp wie Trigger, Drosselungen usw. darstellen. Da für diese Werte keine Bezeichnungen angegeben werden, empfehlen wir Ihren Code auf verschiedenen Eingabegeräten zu testen, um sicherzustellen, dass er wie erwartet funktioniert.

Wenn sich der Stick in der Mittelposition befindet, ist der Wert auf allen Achsen ca. 0,5; es ist jedoch normal, wenn der genaue Wert variiert, auch zwischen nachfolgend erfassten Werten. Strategien zur Minimierung dieser Abweichung werden später in diesem Abschnitt erläutert.

Das folgende Beispiel zeigt, wie die analogen Werte eines Xbox-Controllers gelesen werden:

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

Sie bemerken, dass die gelesenen Werte nicht zuverlässig den neutralen Wert 0,5 liefern, wenn sich der Stick in der ruhenden Mittelstellung befindet, sondern verschiedene Näherungswerte bei 0,5, sobald der Stick bewegt wurde und in die Mittelstellung zurückkehrt. Zur Kompensierung dieser Abweichungen können Sie einen kleinen _inaktiven Bereich_ implementieren (also einen zu ignorierenden Wertebereich nahe der idealen Mittelposition).

Um inaktive Bereiche zu implementieren, können Sie beispielsweise festlegen, wie weit der Stick von der Mittelstellung bewegt wird, und die Werte unterhalb der gewählten Entfernung ignorieren. Die grobe Entfernung kann nach dem Satz des Pythagoras berechnet werden; diese Methode ist allerdings nicht exakt, da die Werte von Sticks nicht planar, sondern polar sind. Dadurch entsteht ein radialer inaktiver Bereich.

Das folgende Beispiel zeigt einen radialen inaktiven Bereich, berechnet nach dem Satz des Pythagoras:

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>Siehe auch

* [Geben Sie für Spiele](input-for-games.md)
* [Eingabe-Methoden für Spiele](input-practices-for-games.md)
* [Windows.Gaming.Input-namespace](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.RawGameController-Klasse](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)
* [Windows.Gaming.Input.IGameController-Schnittstelle](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows.Gaming.Input.IGameControllerBatteryInfo-Schnittstelle](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)