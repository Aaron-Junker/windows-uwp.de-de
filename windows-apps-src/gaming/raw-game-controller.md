---
title: Unformatierter Gamecontroller
description: Verwenden Sie die Windows. Gaming. Input RAW Game Controller-APIs, um Eingaben aus fast allen Arten von Spiel Controllern zu lesen.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Input, RAW Game Controller
ms.localizationpriority: medium
ms.openlocfilehash: d21b411965da874cfb324fc2ee867e39bdcd0ded
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171984"
---
# <a name="raw-game-controller"></a>Unformatierter Gamecontroller

Auf dieser Seite werden die Grundlagen der Programmierung für nahezu jede Art von Game Controller unter Verwendung von [Windows. Gaming. Input. rawgamecontroller](/uwp/api/windows.gaming.input.rawgamecontroller) und zugehörigen APIs für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:

* Erfassen einer Liste verbundener Rollierer Spiele Controller und ihrer Benutzer
* erkennen, dass ein unformatierten Spielcontroller hinzugefügt oder entfernt wurde
* So erhalten Sie die Funktionen eines roamingspielcontrollers
* Lesen von Eingaben von einem unformatierten Spielcontroller

## <a name="overview"></a>Übersicht

Ein unformatierte Spielcontroller ist eine generische Darstellung eines Spiel Controllers, wobei Eingaben auf vielen unterschiedlichen Arten von gängigen Spiel Controllern gefunden werden. Diese Eingaben werden als einfache Arrays unbenannter Schaltflächen, Switches und Achsen verfügbar gemacht. Mithilfe eines unformatierten Spiel Controllers können Sie es Kunden ermöglichen, benutzerdefinierte Eingabe Zuordnungen zu erstellen, unabhängig davon, welche Art von Controller Sie verwenden.

Die [rawgamecontroller](/uwp/api/windows.gaming.input.rawgamecontroller) -Klasse ist wirklich für Szenarien gedacht, in denen die anderen Eingabe Klassen ("[arcadestick](/uwp/api/windows.gaming.input.arcadestick)", " [Flightstick](/uwp/api/windows.gaming.input.flightstick)" usw.) nicht Ihren Anforderungen entsprechen &mdash; , wenn Sie etwas allgemeineres tun möchten, in dem die Kunden viele verschiedene Arten von Spiel Controllern verwenden werden, dann ist diese Klasse für Sie geeignet.

## <a name="detect-and-track-raw-game-controllers"></a>Unformatierte Spiele Controller erkennen und Nachverfolgen

Das erkennen und Nachverfolgen von rohspiel Controllern funktioniert genauso wie für Gamepads, außer mit der [rawgamecontroller](/uwp/api/windows.gaming.input.rawgamecontroller) -Klasse anstelle der [Gamepad](/uwp/api/Windows.Gaming.Input.Gamepad) -Klasse. Weitere Informationen finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md) .

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

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

When a raw game controller is added or removed, the [RawGameControllerAdded](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

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

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>Funktionen eines roamingspielcontrollers

Nachdem Sie den roamingspielcontroller identifiziert haben, an dem Sie interessiert sind, können Sie Informationen zu den Funktionen des Controllers sammeln. Sie können die Anzahl der Schaltflächen auf dem RAW Game-Controller mit [rawgamecontroller. ButtonCount](/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount), die Anzahl der analogen Achsen mit [rawgamecontroller. axiscount](/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount)und die Anzahl der Switches mit [rawgamecontroller. switchcount](/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount)erhalten. Darüber hinaus können Sie den Typ eines Schalters mithilfe von [rawgamecontroller. getwitchkind](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_)erhalten.

Im folgenden Beispiel wird die Eingabe Anzahl eines rohspiel Controllers abgerufen:

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

Im folgenden Beispiel wird der Typ der einzelnen Schalter bestimmt:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>Lesen des rohspiel Controllers

Nachdem Sie die Anzahl der Eingaben für einen rohspiel-Controller kennen, können Sie die Eingaben von dem Controller erfassen. Anders als bei einigen anderen Arten von Eingaben, die Sie möglicherweise verwenden, kommuniziert ein unformatierten Spielcontroller jedoch nicht durch das Durchsetzen von Ereignissen. Stattdessen nehmen Sie regelmäßige Messwerte Ihres aktuellen Zustands vor, indem Sie ihn _Abfragen_ .

### <a name="polling-the-raw-game-controller"></a>Abrufen des rohspiel Controllers

Bei der Abfrage wird eine Momentaufnahme des rohspiel Controllers zu einem bestimmten Zeitpunkt erfasst. Diese Vorgehensweise für die Eingabe Erfassung eignet sich gut für die meisten Spiele, da ihre Logik in der Regel in einer deterministischen Schleife ausgeführt wird, anstatt Sie als ereignisgesteuert zu betrachten. Es ist in der Regel einfacher, Spiel Befehle aus Eingaben zu interpretieren, die alle gleichzeitig gesammelt werden, als aus vielen einzelnen Eingaben, die im Laufe der Zeit gesammelt werden.

Sie rufen einen Rotier-Spielcontroller durch Aufrufen von [rawgamecontroller. getcurrentreading](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___)ab. Diese Funktion füllt Arrays für Schaltflächen, Switches und Achsen auf, die den Zustand des rohspiel Controllers enthalten.

Im folgenden Beispiel wird der aktuelle Zustand eines RAW-Spiel Controllers abgefragt:

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

Es gibt keine Garantie dafür, welche Position in den einzelnen Arrays den Eingabe Wert unterschiedlicher Controller Typen enthalten soll. Sie müssen also überprüfen, welche Eingabe für die Methoden " [rawgamecontroller. getbuttonlabel](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) " und " [rawgamecontroller. gezwitchkind](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_)" verwendet wird.

**Getbuttonlabel** informiert Sie über den Text oder das Symbol, der auf der physischen Schaltfläche gedruckt wird, und nicht über die Funktion der Schaltfläche &mdash; . Daher wird es am besten als Hilfe für die Benutzeroberfläche verwendet, wenn Sie den Player Hinweise dazu geben möchten, welche Schaltflächen welche Funktionen ausführen. **Gezwitchkind** informiert Sie über den Typ des Schalters (d. h. die Anzahl der Positionen), aber nicht über den Namen.

Es gibt keine standardisierte Methode, um die Bezeichnung einer Achse oder eines Schalters zu erhalten, sodass Sie diese selbst testen müssen, um zu bestimmen, welche Eingabe das ist.

Wenn Sie über einen bestimmten Controller verfügen, den Sie unterstützen möchten, können Sie die Datei " [rawgamecontroller. hardwareproductid](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) " und " [rawgamecontroller. hardwarevendorid](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) " erhalten und überprüfen, ob diese dem Controller entsprechen. Die Position jeder Eingabe in jedem Array ist für jeden Controller mit derselben **hardwareproductid** und **hardwarevendorid**identisch, sodass Sie sich keine Gedanken über die Logik machen müssen, die unter verschiedenen Controllern desselben Typs möglicherweise inkonsistent ist.

Neben dem rohstatus des Spiel Controllers gibt jede Lesevorgang einen Zeitstempel zurück, der genau angibt, wann der Zustand abgerufen wurde. Der Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten vorheriger Ablesungen oder zum Zeitpunkt der Spielsimulation herzustellen.

### <a name="reading-the-buttons-and-switches"></a>Lesen der Schaltflächen und Schalter

Jede der Schaltflächen des RAW-Spiel Controllers bietet ein digitales Lesegerät, das angibt, ob es gedrückt oder freigegeben wird (nach oben). Schaltflächen Messwerte werden als einzelne boolesche Werte in einem einzelnen Array dargestellt. Die Bezeichnung für jede Schaltfläche kann mithilfe von [rawgamecontroller. getbuttonlabel](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) mit dem Index des booleschen Werts im Array gefunden werden. Jeder Wert wird als [gamecontrollerbuttonlabel](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)dargestellt.

Im folgenden Beispiel wird bestimmt, ob die **xboxa** -Schaltfläche gedrückt wird:

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

Manchmal möchten Sie möglicherweise ermitteln, ob eine Schaltfläche von gedrückter zu freigegeben oder freigegeben ist, ob mehrere Schaltflächen gedrückt oder freigegeben sind oder ob ein Satz von Schaltflächen auf eine bestimmte Weise angeordnet &mdash; ist, nicht. Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) sowie unter [Erkennen von komplexen Tastenanordnungen](input-practices-for-games.md#detecting-complex-button-arrangements).

Switchwerte werden als Array von [gamecontrollerswitchposition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition)bereitgestellt. Da diese Eigenschaft ein Bitfeld ist, wird die bitweise Maskierung verwendet, um die Richtung des Schalters zu isolieren.

Im folgenden Beispiel wird ermittelt, ob sich die einzelnen Switches in der Position an der Position befinden:

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

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>Lesen der analogen Eingaben (Sticks, Trigger, Drosselungen usw.)

Eine analoge Achse ermöglicht das Lesen zwischen 0,0 und 1,0. Dies schließt jede Dimension auf einem Stick ein, z. b. x und Y für standardsticks oder sogar X-, Y-und Z-Achsen (Roll, Pitch bzw. Yaw) für Flight-Sticks.

Die Werte können analoge Trigger, Drosselungen oder andere Typen von analogen Eingaben darstellen. Diese Werte werden nicht mit Bezeichnungen bereitgestellt. Daher wird empfohlen, dass Ihr Code mit einer Vielzahl von Eingabegeräten getestet wird, um sicherzustellen, dass Sie mit Ihren Annahmen ordnungsgemäß entsprechen.

Bei allen Achsen ist der Wert ungefähr 0,5 für einen Stick, wenn er sich in der Mittelpunkt Position befindet, aber es ist normal, dass der genaue Wert variiert, auch bei nachfolgenden Werten. Strategien zum mindern dieser Variation werden weiter unten in diesem Abschnitt erläutert.

Im folgenden Beispiel wird gezeigt, wie die analogen Werte von einem Xbox-Controller gelesen werden:

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

Wenn Sie die Stick-Werte lesen, werden Sie bemerken, dass Sie nicht zuverlässig ein neutrales Lesen von 0,5 verursachen, wenn Sie sich im Ruhezustand in der Mitte befinden; Stattdessen werden bei jedem Verschieben des Sticks unterschiedliche Werte in der Nähe von 0,5 erzeugt. Zur Kompensierung dieser Abweichungen können Sie einen kleinen _inaktiven Bereich_ implementieren (also einen zu ignorierenden Wertebereich nahe der idealen Mittelposition).

Eine Möglichkeit, eine deadzone zu implementieren, besteht darin, zu bestimmen, wie weit der Stab von der Mitte verschoben wurde, und die Messwerte zu ignorieren, die sich in der Nähe der von Ihnen gewählten Entfernung befinden. Sie können die Entfernung ungefähr so berechnen, &mdash; dass Sie nicht genau ist, da die Mess-Messwerte im wesentlichen Polar sind, nicht planar, &mdash; sondern nur mit dem Pythagorean-Theorem. Dadurch entsteht ein radialer inaktiver Bereich.

Im folgenden Beispiel wird eine grundlegende radiale deadzone mit dem Pythagorean-Theorem veranschaulicht:

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

## <a name="see-also"></a>Weitere Informationen

* [Eingaben für Spiele](input-for-games.md)
* [Eingabemethoden für Spiele](input-practices-for-games.md)
* [Windows. Gaming. Input-Namespace](/uwp/api/windows.gaming.input)
* [Windows. Gaming. Input. rawgamecontroller-Klasse](/uwp/api/windows.gaming.input.rawgamecontroller)
* [Windows. Gaming. Input. igamecontroller-Schnittstelle](/uwp/api/windows.gaming.input.igamecontroller)
* [Windows. Gaming. Input. igamecontrollerbatteryinfo-Schnittstelle](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)