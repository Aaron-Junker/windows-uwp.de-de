---
title: Gamepad und Vibration
description: Verwenden Sie die Windows.Gaming.Input-Gamepad-APIs zum Erkennen, Lesen und Senden von Vibrations- und Impulsbefehlen an Gamepads.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.date: 09/06/2018
ms.topic: article
keywords: Windows 10, UWP, Games, Gamepad, Vibration
ms.localizationpriority: medium
ms.openlocfilehash: 66844b78893ffa8cb92b6b17bd11d87c1d4b1aa0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175324"
---
# <a name="gamepad-and-vibration"></a>Gamepad und Vibration

Auf dieser Seite werden die Grundlagen der Programmierung für Xbox One-Gamepads mittels [Windows.Gaming.Input.Gamepad][gamepad] und verwandter APIs für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:

* Wie Sie eine Liste mit verbundenen Gamepads und deren Benutzern erstellen
* Wie Sie ermitteln, ob ein Gamepad hinzugefügt oder entfernt wurde
* Wie Sie Eingaben von Gamepads lesen
* Wie Sie Vibrations- und Impulsbefehle senden
* Verhalten von Gamepads als UI-Navigationsgeräte

## <a name="gamepad-overview"></a>Gamepadübersicht

Gamepads wie der Xbox Wireless Controller und der Xbox Wireless Controller S sind allgemeine Eingabegeräte für Spiele. Sie sind die Standardeingabegeräte für Xbox One und werden auch häufig von Windows-Spielern als Alternative zur Tastatur und Maus genutzt. Gamepads werden in Windows 10- und UWP-Apps für Xbox durch den [Windows.Gaming.Input][]-Namespace unterstützt.

Xbox One-Gamepads sind mit einem direktionalen Pad (oder einem D-Pad) ausgestattet. Die Schaltflächen **A**, **B**, **X**, **Y**, **View**und **Menu** ; Left und Right thumbsticks,-Stoßdämpfer und-Trigger und insgesamt vier Vibrationsmotoren. Beide Ministicks liefern jeweils zwei analoge Werte für die X- und die Y-Achse und können auch gedrückt und somit als Taste verwendet werden. Jeder-Vorgang stellt eine analoge Lesefunktion bereit, die angibt, wie weit Sie zurückgezogen wird.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` unterstützt auch Xbox 360 Gamepads, die über dasselbe Steuerelement Layout wie Standard-Xbox One-Gamepads verfügen.

### <a name="vibration-and-impulse-triggers"></a>Vibration und Impulse Triggers

Xbox One-Gamepads verfügen über zwei unabhängige Motoren für starke und dezente Gamepadvibrationen sowie über zwei dedizierte Motoren für kurze intensive Vibrationen an den einzelnen Triggern. (Aufgrund dieses einzigartigen Features werden die Trigger des Xbox One-Gamepads als _Impulse Triggers_ bezeichnet.)

> [!NOTE]
> Xbox 360 Gamepads sind nicht mit _Impuls Triggern_ausgestattet.

Weitere Informationen finden Sie in der [Übersicht über Vibration und Impulse Triggers](#vibration-and-impulse-triggers-overview).

### <a name="thumbstick-deadzones"></a>Inaktive Ministick-Bereiche

Ein Ministick, der sich in der Mittelstellung (und damit im Ruhezustand) befindet, liefert im Idealfall immer den gleichen neutralen Wert für die X- und die Y-Achse. Aufgrund von mechanischen Kräften und der Empfindlichkeit des Ministicks handelt es sich bei den tatsächlichen Werten in der Mittelposition jedoch lediglich um (ggf. schwankende) Näherungswerte für den idealen neutralen Wert. Aus diesem Grund müssen Sie _immer einen_ &mdash; Bereich von Werten in der Nähe der idealen Mittelpunkt Position verwenden, die ignoriert werden, &mdash; um Fertigungs Unterschiede, den mechanischen Verschleiß oder andere Gamepad-Probleme auszugleichen.

Durch größere inaktive Bereiche lassen sich ganz einfach beabsichtigte Eingaben von unbeabsichtigten Eingaben unterscheiden.

Weitere Informationen finden Sie unter [Lesen der Ministicks](#reading-the-thumbsticks).

### <a name="ui-navigation"></a>Benutzeroberflächennavigation

Um den Aufwand für die Unterstützung unterschiedlicher Eingabegeräte für die Benutzeroberflächennavigation zu verringern und die Konsistenz zwischen Spielen und Geräten zu fördern, dienen die meisten _physischen_ Eingabegeräte gleichzeitig als getrennte _logische_ Eingabegeräte, die als [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md) bezeichnet werden. Der Benutzeroberflächen-Navigationscontroller stellt ein gemeinsames Vokabular für Benutzeroberflächen-Navigationsbefehle über verschiedene Eingabegeräte hinweg bereit.

Bei einem UI-Navigations Controller ordnen Gamepads den [erforderlichen Satz](ui-navigation-controller.md#required-set) von Navigations Befehlen den Schaltflächen Linker fingerstick, D-Pad, **Ansicht**, **Menü**, **a**und **B** zu.

| Navigationsbefehl | Gamepad-Eingabe                       |
| ------------------:| ----------------------------------- |
|                 Nach oben | Linker Ministick nach oben/Steuerkreuz nach oben       |
|               Nach unten | Linker Ministick nach unten/Steuerkreuz nach unten   |
|               Left | Linker Ministick nach links/Steuerkreuz nach links   |
|              Right | Linker Ministick nach rechts/Steuerkreuz nach rechts |
|               Sicht | Ansicht-Taste                         |
|               Menü | Menü-Taste                         |
|             Akzeptieren | A-Taste                            |
|             Abbrechen | B-Taste                            |

Darüber hinaus ordnen Gamepads den gesamten [optionalen Satz](ui-navigation-controller.md#optional-set) der Navigationsbefehle den restlichen Eingaben zu.

| Navigationsbefehl | Gamepad-Eingabe          |
| ------------------:| ---------------------- |
|            BILD-AUF | Linker Trigger           |
|          BILD-AB | Rechter Trigger          |
|          Seite nach links | Linker Bumper            |
|         Seite nach rechts | Rechter Bumper           |
|          Bildlauf nach oben | Rechter Ministick nach oben    |
|        Bildlauf nach unten | Rechter Ministick nach unten  |
|        Bildlauf nach links | Rechter Ministick nach links  |
|       Bildlauf nach rechts | Rechter Ministick nach rechts |
|          Kontext 1 | X-Taste               |
|          Kontext 2 | Y-Taste               |
|          Kontext 3 | Linken Ministick drücken  |
|          Kontext 4 | Rechten Ministick drücken |

## <a name="detect-and-track-gamepads"></a>Erkennen und Nachverfolgen von Gamepads

Gamepads werden vom System verwaltet. Daher müssen Sie diese nicht erstellen oder initialisieren. Das System stellt eine Liste mit verbundenen Gamepads sowie Ereignisse bereit, um Sie zu benachrichtigen, wenn ein Gamepad hinzugefügt oder entfernt wird.

### <a name="the-gamepads-list"></a>Die Gamepadliste

Die [Gamepad][]-Klasse stellt die statische Eigenschaft [Gamepads][] bereit. Hierbei handelt es sich um eine schreibgeschützte Liste mit derzeit verbundenen Gamepads. Da Sie möglicherweise nur an einigen der verbundenen Gamepads interessiert sind, wird empfohlen, dass Sie Ihre eigene Sammlung beibehalten, anstatt über die-Eigenschaft auf Sie zuzugreifen `Gamepads` .

Im folgenden Beispiel werden alle verbundenen Gamepads in eine neue Sammlung kopiert. Beachten Sie Folgendes: da andere Threads im Hintergrund auf diese Auflistung zugreifen (in den Ereignissen " [gamepadadded][] " und " [gamepadremoved][] "), müssen Sie jeden Code sperren, der die Auflistung liest oder aktualisiert.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>Hinzufügen und Entfernen von Gamepads

Beim Hinzufügen oder Entfernen eines Gamepad werden die Ereignisse " [gamepadadded][] " und " [gamepadremoved][] " ausgelöst. Sie können Handler für diese Ereignisse registrieren, um die derzeit verbundenen Gamepads nachzuverfolgen.

Im folgenden Beispiel wird mit der Nachverfolgung eines hinzugefügten Gamepads begonnen.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

Im folgenden Beispiel wird die Nachverfolgung eines Gamepad beendet, das entfernt wurde. Sie müssen auch den Inhalt der Gamepads behandeln, die Sie nachverfolgen, wenn Sie entfernt werden. mit diesem Code werden z. b. nur die Eingaben von einem Gamepad nachverfolgt und einfach `nullptr` beim Entfernen festgelegt. Sie müssen jeden Frame überprüfen, wenn Ihr Gamepad aktiv ist, und das Gamepad aktualisieren, von dem Sie eine Eingabe sammeln, wenn Controller verbunden und getrennt sind.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

Weitere Informationen finden Sie unter [Eingabemethoden für Spiele](input-practices-for-games.md) .

### <a name="users-and-headsets"></a>Benutzer und Headsets

Jedes Gamepad kann mit einem Benutzerkonto verknüpft werden, um die Identität des Benutzers mit dem Spiel zu verknüpfen, und mit einem Headset verbunden werden, um Sprachchats oder Features im Spiel zu unterstützen. Weitere Informationen zu Benutzern und Headsets finden Sie unter [Nachverfolgen von Benutzern und ihren Geräten](input-practices-for-games.md#tracking-users-and-their-devices) und [Headsets](headset.md).

## <a name="reading-the-gamepad"></a>Lesen des Gamepads

Nachdem Sie das Gamepad identifiziert haben, für das Sie sich interessieren, können Sie Eingaben davon erfassen. Anders als im Fall anderer Eingaben, die Sie möglicherweise kennen, teilen Gamepads Zustandsänderungen jedoch nicht durch das Auslösen von Ereignissen mit. Stattdessen müssen Sie den aktuellen Status regelmäßig lesen, indem Sie ihn _abrufen_.

### <a name="polling-the-gamepad"></a>Abfragen des Gamepads

Beim Abruf wird eine Momentaufnahme des Navigationsgeräts an einem bestimmten Zeitpunkt erfasst. Diese Vorgehensweise für die Eingabe Erfassung eignet sich gut für die meisten Spiele, da ihre Logik in der Regel in einer deterministischen Schleife ausgeführt wird, anstatt Sie als ereignisgesteuert zu betrachten. Es ist in der Regel einfacher, Spiel Befehle aus Eingaben zu interpretieren, die alle gleichzeitig gesammelt werden, als aus vielen einzelnen Eingaben, die im Laufe der Zeit gesammelt werden.

Gamepads werden durch Aufrufen von [GetCurrentReading][] abgefragt. Diese Funktion gibt einen [GamepadReading][]-Wert mit dem Zustand des Gamepads zurück.

Im folgenden Beispiel wird der aktuelle Zustand eines Gamepads abgefragt.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

Zusätzlich zum Zustand des Gamepads enthält jeder Wert einen Zeitstempel, der den genauen Zeitpunkt angibt, zu dem der Zustand abgerufen wurde. Der Zeitstempel ist nützlich, um einen Bezug zu den Zeitpunkten vorheriger Ablesungen oder zum Zeitpunkt der Spielsimulation herzustellen.

### <a name="reading-the-thumbsticks"></a>Lesen der Ministicks

Jeder Ministick liefert einen analogen Wert zwischen -1,0 und +1,0 auf der X- und der Y-Achse. Auf der X-Achse entspricht der Wert -1,0 der äußerst linken Ministickposition und der Wert +1,0 der äußerst rechten Position. Auf der Y-Achse entspricht der Wert -1,0 der niedrigsten Ministickposition und der Wert +1,0 der höchsten Position. In beiden Achsen ist der Wert ungefähr 0,0, wenn sich der Stift in der Mittelpunkt Position befindet, aber es ist normal, dass der genaue Wert variiert, auch bei nachfolgenden Werten. Strategien zum mindern dieser Variation werden weiter unten in diesem Abschnitt erläutert.

Der X-Achsenwert des linken Ministicks wird aus der `LeftThumbstickX`-Eigenschaft der [GamepadReading][]-Struktur gelesen. Der Y-Achsenwert stammt aus der `LeftThumbstickY`-Eigenschaft. Der X-Achsenwert des rechten Ministicks wird aus der `RightThumbstickX`-Eigenschaft gelesen. Der Y-Achsenwert stammt aus der `RightThumbstickY`-Eigenschaft.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

Sie werden feststellen, dass die gelesenen Ministickwerte nicht zuverlässig einen neutralen 0,0-Wert liefern, wenn sich der Ministick in der Mittelstellung (und damit im Ruhezustand) befindet. Stattdessen erhalten Sie verschiedene Näherungswerte für 0,0, wann immer der Ministicks bewegt wurde und wieder in die Mittelstellung zurückkehrt. Zur Kompensierung dieser Abweichungen können Sie einen kleinen _inaktiven Bereich_ implementieren (also einen zu ignorierenden Wertebereich nahe der idealen Mittelposition). Zur Implementierung eines inaktiven Bereichs können Sie beispielsweise ermitteln, wie weit sich der Ministick von der Mittelposition entfernt hat, und dabei die Werte ignorieren, die eine bestimmte, von Ihnen gewählte Entfernung unterschreiten. Sie können die Entfernung ungefähr so berechnen, &mdash; dass Sie nicht genau ist, da Finger erationswerte im wesentlichen Polar sind, nicht planar, &mdash; sondern nur mit dem Pythagorean-Theorem. Dadurch entsteht ein radialer inaktiver Bereich.

Das folgende Beispiel veranschaulicht einen einfachen radialen inaktiven Bereich unter Verwendung des Satzes des Pythagoras.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

Jeder Ministick kann auch gedrückt werden und somit als Taste fungieren. Weitere Informationen zum Lesen dieser Eingabe finden Sie unter [Lesen der Tasten](#reading-the-buttons).

### <a name="reading-the-triggers"></a>Lesen der Trigger

Die Trigger werden als Gleitkommawerte zwischen 0,0 (vollständig losgelassen) und 1,0 (vollständig gedrückt) dargestellt. Der Wert des linken Triggers wird aus der `LeftTrigger`-Eigenschaft der [GamepadReading][]-Struktur gelesen. Der Wert des rechten Triggers stammt aus der `RightTrigger`-Eigenschaft.

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>Lesen der Tastenwerte

Jede der Gamepad-Schaltflächen &mdash; stellt die vier Richtungen des D-Pad, des linken und des rechten bupers, den linken und rechten Fingerabdruck Press, **A**, **B**, **X**, **Y**, **View**und **Menu** &mdash; bereit und stellt ein digitales Lesematerial bereit, das angibt, ob es gedrückt oder freigegeben wird (nach oben). Aus Effizienzgründen werden Schaltflächen Messwerte nicht als einzelne boolesche Werte dargestellt. Stattdessen werden Sie alle in ein einzelnes Bitfeld gepackt, das von der [gamepadbuttons][] -Enumeration repräsentiert wird.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

Die Tastenwerte werden aus der `Buttons`-Eigenschaft der [GamepadReading][]-Struktur gelesen. Da diese Eigenschaft ein Bitfeld ist, wird eine bitweise Maskierung verwendet, um den Wert der Taste zu isolieren, an der Sie interessiert sind, Wenn das entsprechende Bit festgelegt ist, wird die Schaltfläche gedrückt (unten). Andernfalls wird Sie freigegeben (up).

Im folgenden Beispiel wird ermittelt, ob die A-Taste gedrückt ist.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

Im folgenden Beispiel wird ermittelt, ob die A-Taste losgelassen wurde.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

Manchmal möchten Sie möglicherweise ermitteln, ob eine Schaltfläche von gedrückter zu freigegeben oder freigegeben ist, ob mehrere Schaltflächen gedrückt oder freigegeben werden oder ob eine Reihe von Schaltflächen auf eine bestimmte Weise angeordnet ist &mdash; , nicht. Informationen zum Ermitteln dieser Bedingungen finden Sie unter [Erkennen von Tastenübergängen](input-practices-for-games.md#detecting-button-transitions) sowie unter [Erkennen von komplexen Tastenanordnungen](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-gamepad-input-sample"></a>Ausführen des Gamepad-Eingabebeispiels

Im [GamepadUWP-Beispiel _(GitHub)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) wird veranschaulicht, wie Sie eine Verbindung mit einem Gamepad herstellen und dessen Zustand lesen.

## <a name="vibration-and-impulse-triggers-overview"></a>Übersicht über Vibration und Impulse Triggers

Die Vibrationsmotoren in einem Gamepad erzeugen fühlbares Feedback für den Benutzer. Dies wird in Spielen verwendet, um die Immersion zu verbessern, Statusinformationen (wie etwa eine Beschädigung) zu vermitteln, auf wichtige, in der Nähe befindliche Objekte hinzuweisen oder für andere kreative Zwecke.

Xbox One-Gamepads sind mit insgesamt vier unabhängigen Vibrationsmotoren ausgestattet. Zwei sind große Motoren im Gamepad-Text. der linke Motor bietet eine grobe, große Amplitude-Vibration, während der Rechte Motor eine sanftere, präziere Vibration bietet. Die anderen beiden Motoren sind klein, befinden sich in den Triggern und erzeugen kurze, intensive Vibrationen direkt an den Triggerfingern des Benutzers. Aufgrund dieses einzigartigen Features des Xbox One-Gamepads werden die Trigger dieses Gamepads als _Impulse Triggers_ bezeichnet. Gemeinsam lässt sich mithilfe dieser Motoren eine Vielzahl von Tastempfindungen erzeugen.

## <a name="using-vibration-and-impulse"></a>Verwenden von Vibrationen und Impulsen

Gamepadvibrationen werden über die [Vibration][]-Eigenschaft der [Gamepad][]-Klasse gesteuert. `Vibration` eine Instanz der [gamepadvibration][] -Struktur, die aus vier Gleit Komma Werten besteht. Jeder Wert stellt die Intensität eines Motors dar.

Obwohl die Elemente der `Gamepad.Vibration` Eigenschaft direkt geändert werden können, empfiehlt es sich, eine separate `GamepadVibration` Instanz mit den gewünschten Werten zu initialisieren und Sie dann in die- `Gamepad.Vibration` Eigenschaft zu kopieren, um die tatsächlichen Motor Intensitäten gleichzeitig zu ändern.

Im folgenden Beispiel wird das gleichzeitige Ändern aller Motorintensitäten veranschaulicht.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

### <a name="using-the-vibration-motors"></a>Verwenden der Vibrationsmotoren

Der linke und der rechte Vibrationsmotor akzeptieren Gleitkommawerte zwischen 0,0 (keine Vibration) und 1,0 (stärkste Vibration). Die Intensität des linken Motors wird durch die `LeftMotor`-Eigenschaft der [GamepadVibration][]-Struktur festgelegt. Die Intensität des rechten Motors wird durch die `RightMotor`-Eigenschaft festgelegt.

Im folgenden Beispiel wird die Intensität beider Vibrationsmotoren festgelegt und die Gamepadvibration aktiviert.

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

Vergessen Sie nicht, dass diese beiden Motoren nicht identisch sind. Wenn Sie die Eigenschaften also auf den gleichen Wert festlegen, werden in den beiden Motoren nicht die gleichen Vibrationen erzeugt. Bei einem beliebigen Wert erzeugt der linke Motor eine stärkere Vibrationen in einer niedrigeren Frequenz als der Rechte Motor, bei dem &mdash; für denselben Wert &mdash; eine schonerschwingung mit einer höheren Häufigkeit erzeugt wird. Selbst bei Verwendung des Maximalwerts erreicht der linke Motor nicht die hohen Frequenzen des rechten Motors, und mit dem rechten Motor lassen sich nicht die gleichen hohen Kräfte erzeugen wie mit dem linken Motor. Da die Motoren allerdings fest mit dem Gamepadgehäuse verbunden sind, nehmen Spieler die Vibrationen nicht vollständig unabhängig voneinander wahr, obwohl die Motoren unterschiedliche Eigenschaften haben und mit unterschiedlicher Intensität vibrieren können. Dadurch lässt sich eine größere, ausdrucksstärkere Bandbreite von Empfindungen vermitteln als mit zwei identischen Motoren.

### <a name="using-the-impulse-triggers"></a>Verwenden der Impulse Triggers

Jeder Impulse Trigger-Motor akzeptiert einen Gleitkommawert zwischen 0,0 (keine Vibration) und 1,0 (stärkste Vibration). Die Intensität des linken Triggermotors wird durch die `LeftTrigger`-Eigenschaft der [GamepadVibration][]-Struktur festgelegt. Die Intensität des rechten Triggers wird durch die `RightTrigger`-Eigenschaft festgelegt.

Das folgende Beispiel legt die Intensität der beiden Impulse Triggers fest und aktiviert sie.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

Im Gegensatz zu den anderen Motoren sind die beiden Vibrationsmotoren innerhalb der Trigger identisch und erzeugen bei Verwendung des gleichen Werts jeweils die gleiche Vibration. Da diese Motoren jedoch nicht fest verbunden sind, nehmen die Spieler die Vibrationen unabhängig voneinander wahr. Dank dieses Designs können über beide Trigger gleichzeitig vollständig unabhängige Empfindungen erzeugt werden, um spezifischere Informationen zu vermitteln als mit den Motoren im Gamepadgehäuse möglich wäre.

## <a name="run-the-gamepad-vibration-sample"></a>Ausführen des Gamepad-Vibrationsbeispiels

Das [GamepadVibrationUWP-Beispiel _(GitHub)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) veranschaulicht, wie mithilfe der Gamepad-Vibrationsmotoren und der Impulse Triggers eine Reihe von Effekten erzeugt wird.

## <a name="see-also"></a>Weitere Informationen

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Eingabemethoden für Spiele](input-practices-for-games.md)

[Windows. Gaming. Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[Gamepad]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepads]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadadded]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadremoved]: /uwp/api/Windows.Gaming.Input.Gamepad
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.Gamepad
[vibration]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadreading]: /uwp/api/Windows.Gaming.Input.GamepadReading
[gamepadbuttons]: /uwp/api/Windows.Gaming.Input.GamepadButtons
[gamepadvibration]: /uwp/api/Windows.Gaming.Input.GamepadVibration