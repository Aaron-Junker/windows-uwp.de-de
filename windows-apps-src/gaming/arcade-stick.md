---
title: Arcade-Joystick
description: Verwenden Sie die Windows.Gaming.Input-Arcade-Joystick-APIs zum Erkennen und Lesen von Arcade-Joysticks.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Arcade-Joysticks, Eingabe
ms.localizationpriority: medium
ms.openlocfilehash: 6f9e3ff29dfb17b6e2a07df52153013b5266206e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593145"
---
# <a name="arcade-stick"></a>Arcade-Joystick

Auf dieser Seite werden die Grundlagen der Programmierung für Xbox One-Arcade-Joysticks mittels [Windows.Gaming.Input.ArcadeStick][arcadestick] und verwandter APIs für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:

* Erstellen einer Liste der verbundenen Arcade-Joysticks und ihrer Benutzer
* Ermitteln, ob ein Arcade-Joystick hinzugefügt oder entfernt wurde
* Lesen der Eingabe von einem oder mehreren Arcade-Joysticks
* Verhalten von Arcade-Sticks als UI-Navigation-Geräte

## <a name="arcade-stick-overview"></a>Übersicht über Arcade-Joysticks

Bei Arcade-Joysticks handelt es sich um Eingabegeräte, die den Eindruck klassischer Arcade-Automaten vermitteln und aufgrund ihrer äußerst präzisen digitalen Steuerelemente geschätzt werden. Arcade-Joysticks sind das ideale Eingabegerät für Kampfspiele und andere Spiele im Arcade-Stil und eignen sich für alle Spiele mit komplett digitaler Steuerung. Arcade-Joysticks werden in Windows 10- und Xbox One-UWP-Apps durch den Namespace [Windows.Gaming.Input][] unterstützt.

Xbox One Arcade-Sticks verfügen über eine 8-Wege-Joystick digitale, sechs **Aktion** Schaltflächen (A1-a6 in der folgenden Abbildung dargestellt), und zwei **spezielle** Schaltflächen (dargestellt als S1 und S2); sie sind alle digitale Eingabegeräte, die analogen Steuerelemente oder Vibration nicht unterstützen. Xbox One Arcade-Sticks verfügen auch über **Ansicht** und **Menü** Schaltflächen, die zur Unterstützung der Navigation in der Benutzeroberfläche verwendet, aber sie werden nicht unterstützt von Gaming-Befehle und nicht ohne weiteres möglich als Joystick-Schaltflächen .

![Arcade-Speicherstick mit 4-Wege-Joystick, 6 Aktionsschaltflächen (A1-A6) und 2 besondere Schaltflächen (S1 und S2)](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>Benutzeroberflächennavigation

Um den Aufwand für die Unterstützung vieler unterschiedlicher Eingabegeräte für die Benutzeroberflächennavigation zu verringern und die Konsistenz zwischen Spielen und Geräten zu fördern, dienen die meisten _physischen_ Eingabegeräte gleichzeitig als getrennte _logische_ Eingabegeräte, die als [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md) bezeichnet werden. Der Benutzeroberflächen-Navigationscontroller stellt ein gemeinsames Vokabular für Benutzeroberflächen-Navigationsbefehle über verschiedene Eingabegeräte hinweg bereit.

Als einen UI-navigationscontroller sticks Arcade Zuordnung der [erforderliche Satz](ui-navigation-controller.md#required-set) der Navigationsbefehle aus, um den Joystick und **anzeigen**, **Menü**, **Aktion 1**, und **Aktion 2** Schaltflächen.

| Navigationsbefehl | Eingabe per Arcade-Joystick  |
| ------------------:| ------------------- |
|                 Nach oben | Joystick nach oben            |
|               Nach unten | Joystick nach unten          |
|               Nach links | Joystick nach links          |
|              Nach rechts | Joystick nach rechts         |
|               Ansicht | Ansicht-Taste         |
|               Menü | Menü-Taste         |
|             Annehmen | Taste für Aktion 1     |
|             Abbrechen | Taste für Aktion 2     |

Arcade-Joysticks ordnen keines der [optionalen Sätze](ui-navigation-controller.md#optional-set) mit Navigationsbefehlen zu.

## <a name="detect-and-track-arcade-sticks"></a>Ermitteln und Nachverfolgen von Arcade-Joysticks

Erkennen und Nachverfolgen von Arcade-Sticks funktioniert auf genau die gleiche Weise wie für Gamepads, außer mit dem [ArcadeStick][] -Klasse anstelle der [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) Klasse. Weitere Informationen finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md).

<!-- Arcade sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected arcades sticks and events to notify you when an arcade stick is added or removed.

### The arcade sticks list

The [ArcadeStick][] class provides a static property, [ArcadeSticks][], which is a read-only list of arcade sticks that are currently connected. Because you might only be interested in some of the connected arcade sticks, it's recommended that you maintain your own collection instead of accessing them through the `ArcadeSticks` property.

The following example copies all connected arcade sticks into a new collection. Note that because other threads in the background will be accessing this collection (in the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events), you need to place a lock around any code that reads or updates the collection.

```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();
critical_section myLock{};

for (auto arcadeStick : ArcadeStick::ArcadeSticks)
{
    // Check if the arcade stick is already in myArcadeSticks; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myArcadeSticks), end(myArcadeSticks), arcadeStick);

    if (it == end(myArcadeSticks))
    {
        // This code assumes that you're interested in all arcade sticks.
        myArcadeSticks->Append(arcadeStick);
    }
}
```

### Adding and removing arcade sticks

When an arcade stick is added or removed the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events are raised. You can register handlers for these events to keep track of the arcade sticks that are currently connected.

The following example starts tracking an arcade stick that's been added.

```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // Check if the just-added arcade stick is already in myArcadeSticks; if it
    // isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

The following example stops tracking an arcade stick that's been removed.

```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each arcade stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-arcade-stick"></a>Lesen des Arcade-Joysticks

Nachdem Sie den Arcade-Joystick identifiziert haben, für den Sie sich interessieren, können Sie Eingaben von ihm erfassen. Anders als im Fall anderer Eingaben, die Sie möglicherweise kennen, teilen Arcade-Joysticks Zustandsänderungen jedoch nicht durch das Auslösen von Ereignissen mit. Stattdessen müssen Sie den aktuellen Status regelmäßig lesen, indem Sie ihn _abrufen_.

### <a name="polling-the-arcade-stick"></a>Abfragen des Arcade-Joysticks

Beim Abfragen wird ein Snapshot des Arcade-Joysticks zu einem bestimmten Zeitpunkt erstellt. Dieser Ansatz zum Erfassen von Eingaben ist für die meisten Spiele geeignet, da deren Logik üblicherweise in einer deterministischen Schleife ausgeführt wird und nicht ereignisgesteuert ist. Es ist in der Regel auch einfacher, Befehle in Spielen anhand von Eingaben zu interpretieren, die alle gemeinsam erfasst werden, als anhand zahlreicher Eingaben, die im Laufe der Zeit erfasst werden.

Sie fragen einen Arcade-Joystick ab, indem Sie [GetCurrentReading][] aufrufen. Diese Funktion gibt ein [ArcadeStickReading][]-Element zurück, das den Zustand des Arcade-Joysticks enthält.

Im folgenden Beispiel wird der aktuelle Zustand eines Arcade-Joysticks abgefragt.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

Zusätzlich zum Zustand des Arcade-Joysticks enthält jede Ablesung einen Zeitstempel, der den genauen Zeitpunkt angibt, zu dem der Zustand abgerufen wurde. Der Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten vorheriger Ablesungen oder zum Zeitpunkt der Spielsimulation herzustellen.

### <a name="reading-the-buttons"></a>Lesen der Tastenwerte

Die Schaltflächen für Arcade Stick&mdash;die vier Anweisungen von den Joystick, sechs **Aktion** Schaltflächen und die beiden **spezielle** Schaltflächen&mdash;bietet einen lesen digitalen Inhalte, der angibt, ob Es wurde gedrückt () oder (nach oben) veröffentlicht. Für Effizienz sind nicht Schaltfläche Messwerten als einzelne boolesche Werte dargestellt werden. stattdessen diese sind alle verteilt in ein einzelnes Bitfeld, das durch dargestellt ist die [ArcadeStickButtons][] Enumeration.

> [!NOTE]
> Arcade-Sticks verfügen über zusätzliche Schaltflächen, die für die Navigation in der Benutzeroberfläche verwendet, z. B. die **Ansicht** und **Menü** Schaltflächen. Diese Tasten sind nicht in der Enumeration `ArcadeStickButtons` enthalten und können nur gelesen werden, wenn auf den Arcade-Joystick als Benutzeroberflächen-Navigationsgerät zugegriffen wird. Weitere Informationen finden Sie unter [Benutzeroberflächen-Navigationsgerät](ui-navigation-controller.md).

Die Schaltflächenwerte werden von der `Buttons`-Eigenschaft der [ArcadeStickReading][]-Struktur gelesen. Da diese Eigenschaft ein Bitfeld ist, wird eine bitweise Maskierung verwendet, um den Wert der Taste zu isolieren, an der Sie interessiert sind, Die Taste ist gedrückt (unten), wenn das entsprechende Bit festgelegt ist. Andernfalls ist sie nicht gedrückt (oben).

Im folgende Beispiel wird bestimmt, ob die **Aktion 1** gedrückt wird.

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

Im folgende Beispiel wird bestimmt, ob die **Aktion 1** losgelassen wird.

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

In einigen Fällen möchten Sie vielleicht ermitteln, ob eine Taste von „gedrückt“ zu „nicht gedrückt“ bzw. umgekehrt wechselt, oder ob mehrere Tasten gedrückt bzw. nicht gedrückt oder in bestimmter Weise angeordnet sind&mdash;einige gedrückt, andere nicht. Weitere Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) und [Erkennen von komplexen Tastenanordungen](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Ausführen des InputInterfacing-Beispiels

Das [InputInterfacingUWP-Beispiel _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) veranschaulicht, wie Arcade-Joysticks und verschiedene Arten von Eingabegeräten zusammen verwendet werden und wie diese Eingabegeräte sich bei der Verwendung als Benutzeroberflächen-Navigationcontrollern verhalten.

## <a name="see-also"></a>Siehe auch

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Eingabe-Methoden für Spiele](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[arcadesticks]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadesticks.aspx
[arcadestickadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickadded.aspx
[arcadestickremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.getcurrentreading.aspx
[arcadestickreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickreading.aspx
[arcadestickbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickbuttons.aspx
