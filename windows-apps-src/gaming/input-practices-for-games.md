---
title: Eingabemethoden für Spiele
description: Erfahren Sie mehr über Muster und Verfahren zur effizienten Verwendung von Eingabegeräten.
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Eingabe
ms.localizationpriority: medium
ms.openlocfilehash: 8235b2c2029b2bb3b9351263a3c908879b4beba9
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684983"
---
# <a name="input-practices-for-games"></a>Eingabemethoden für Spiele

Auf dieser Seite werden Muster und Verfahren zur effizienten Verwendung von Eingabegeräten in Spielen für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite lernen Sie:

* Nachverfolgen von Spielern und den von ihnen aktuell verwendeten Eingabe- und Navigationsgeräten
* Erkennen von Tastenübergängen (von „Gedrückt“ zu „Losgelassen“ oder von „Losgelassen“ zu „Gedrückt“)
* Erkennen komplexer Tastenanordnungen mit einem einzelnen Test

## <a name="choosing-an-input-device-class"></a>Auswählen einer Eingabegeräteklasse

Es sind viele verschiedene Arten von Eingabe-APIs verfügbar, z. B. [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick), und [Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). Wie entscheiden Sie, welche API Sie für Ihr Spiel verwenden?

Sie sollten die API wählen, die für Ihr Spiel die am besten geeigneten Eingaben bietet. Wenn Sie z. B. Spiele für 2D-Plattformen entwickeln, können Sie wahrscheinlich nur die **Gamepad**-Klasse verwenden, ohne die zusätzlichen Funktionalitäten anderer Klassen zu berücksichtigen. Das Spiel wäre damit auf die Unterstützung von Gamepads beschränkt; seine einheitliche Oberfläche würde aber mit vielen Gamepads funktionieren, ohne weiteren Code zu benötigen.

Andererseits sollten Sie für komplexe Flug- und Renn-Simulationen alle [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)-Objekte als Grundlage auflisten, um auch weniger bekannte Geräte zu unterstützen, die manche Spieler noch vereinzelt verwenden, z. B. separate Pedale, Drosselklappen usw. 

Anschließend können Sie mithilfe einer Eingabeklasse-Methode wie **FromGameController** bzw. [Gamepad.FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller) feststellen, ob aktuelle Ansichten für das Gerät verfügbar sind. Wenn das Gerät z. B. auch ein **Gamepad** ist, können Sie die Schaltflächenzuordnungen der Benutzeroberfläche entsprechend anpassen, um die Standardtasten sinnvoller zuzuordnen. (Wenn Sie im Gegensatz dazu nur **RawGameController** verwenden, muss der Spieler seine Gamepad-Eingaben manuell konfigurieren.) 

Alternativ können Sie auch die Anbieter-ID (VID) und Produkt-ID (PID) des **RawGameController** vergleichen (jeweils durch [HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) und [HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId)), um vorgeschlagene Tastenzuordnungen für bekannte Geräte anzubieten, die trotzdem kompatibel sind mit unbekannten Geräten, die aus manuellen Zuordnungen von Spielern stammen.

## <a name="keeping-track-of-connected-controllers"></a>Nachverfolgen von angeschlossenen Controllern

Während jeder Controllertyp eine Liste der verbundenen Domänencontroller enthält (z. B. [Gamepad.Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)), empfiehlt es sich, Ihre eigene Liste von Domänencontrollern zu verwalten. Weitere Informationen finden Sie unter [die Gamepadliste](gamepad-and-vibration.md#the-gamepads-list) (jeder Controllertyp hat einen ähnlichen Namensabschnitt für sein eigenes Thema).

Was geschieht jedoch, wenn der Spieler den Controller trennt oder einen neuen installiert? Sie müssen diese Ereignisse behandeln, und die Liste entsprechend zu aktualisieren. Weitere Informationen finden Sie unter [Hinzufügen und Entfernen von Gamepads](gamepad-and-vibration.md#adding-and-removing-gamepads) (jeder Controllertyp hat einen ähnlichen Namensabschnitt für sein eigenes Thema).

Da die hinzugefügten und entfernten Ereignisse asynchron ausgelöst werden, könnten Sie beim Umgang mit der Liste der Domänencontroller falsche Ergebnisse erhalten. Wenn Sie daher auf die Liste der Domänencontroller zugreifen, sperren Sie diese, damit nur ein Thread zu jedem Zeitpunkt darauf zugreifen kann. Dies kann mit [Concurrency Runtime](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime) geschehen, insbesondere der [Critical_section-Klasse](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class)im **&lt;ppl.h&gt;** .

Die Liste der angeschlossenen Controller ist anfänglich leer, und dauert ein oder zwei Sekunden zum Auffüllen. Wenn Sie daher nur den aktuellen Gamepad der Startmethode zuweisen, ist dieser **null**!

Um dieses Problem zu beheben, müssen Sie eine Methode durchführen, die den wichtigsten Gamepad "aktualisiert" (in einem Spiel mit Einzelspieler; Multiplayer-Spiele erfordern anspruchsvollere Lösungen). Sie sollten diese Methode von beiden Controller-Ereignishandlern aufrufen, dem hinzugefügten und dem entfernten Controller oder in der Update-Methode.

Die folgende Methode gibt den ersten Gamepad der Liste zurück (oder **nullptr**, wenn die Liste leer ist). Denken Sie daran, **nullptr** jedes Mal zu überprüfen, wenn Sie mit dem Controller eine Aktion durchführen. Es liegt an Ihnen, ob Sie Spiele blockieren, wenn es kein Domänencontroller verbunden ist (z. B. durch das Anhalten des Spiels) oder einfach Spiele weiter spielen und die Eingabe ignorieren.

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

Hier ein Beispiel zum Behandeln von Eingaben von einem Gamepad:

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>Nachverfolgen von Benutzern und Geräten

Alle Eingabegeräte sind einem [Benutzer](https://docs.microsoft.com/uwp/api/windows.system.user) zugeordnet, damit seine Identität mit seinem Spiel, seinen Erfolgen, geänderten Einstellungen und anderen Aktivitäten verknüpft werden kann. Da sich Benutzer nach Belieben an- und abmelden können, ist es normal, dass sich andere Benutzer auf einem mit dem System verbundenen Eingabegerät anmelden, nachdem sich ein vorheriger Benutzer abgemeldet hat. Wenn sich ein Benutzer an- oder abmeldet, wird das [IGameController.UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged)-Ereignis ausgelöst. Sie können für diesen Fall einen Ereignishandler registrieren, um die Spieler und von ihnen verwendeten Geräte nachzuverfolgen.

Die Benutzeridentität dient auch dazu, Eingabegeräte einem [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md) zuzuordnen.

Aus diesen Gründen sollten Spielereingaben mithilfe der [User](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User)-Eigenschaft der Geräteklasse (geerbt von der [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)-Schnittstelle) nachverfolgt und korreliert werden.

Das Beispiel [UserGamepadPairingUWP (GitHub)](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) zeigt, wie Benutzer und von ihnen verwendete Geräte nachverfolgt werden können.

## <a name="detecting-button-transitions"></a>Erkennen von Tastenübergängen

In einigen Fällen möchten Sie wissen, wann eine Taste zuerst gedrückt oder losgelassen wird, also genau, wann der Zustand der Taste von „Losgelassen“ in „Gedrückt“ oder von „Gedrückt“ in „Losgelassen“ übergegangen ist. Um dies zu ermitteln und herauszufinden, was sich geändert hat, müssen Sie den vorherigen Geräte-Ablesewert beibehalten und mit dem aktuellen Ablesewert vergleichen.

Das folgende Beispiel zeigt anhand eines Gamepads, wie zuvor erhaltene Werte beibehalten werden können; das gleiche Prinzip gilt ebenso für andere Eingabegerätetypen wie Arcade-Joysticks, Rennlenkräder usw.

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

Als Erstes verschiebt `Game::Loop` den vorhandenen Wert von `newReading` (den Gamepad-Ablesewert aus der vorherigen Schleifeniteration) in `oldReading` und fügt dann einen neuen Gamepad-Ablesewert für die aktuelle Iteration in `newReading` ein. Mithilfe dieser Informationen können Sie den Übergang von Tasten erkennen.

Das folgende Beispiel zeigt eine einfache Möglichkeit, um Tastenzustandswechsel zu erkennen:

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Diese beiden Funktionen leiten zuerst den booleschen Zustand der Tastenauswahl von `newReading` und `oldReading` ab und stellen dann durch boolesche Logik fest, ob der Übergang in den Zielzustand erfolgt ist. Diese Funktionen geben nur **true** zurück, wenn der neue Ablesewert den Zielzustand enthält („Gedrückt“ bzw. „Losgelassen“) *und* der alte Ablesewert nicht auch den Zielzustand enthält. Andernfalls geben sie **false** zurück.

## <a name="detecting-complex-button-arrangements"></a>Erkennen komplexer Tastenanordnungen

Jede Taste eines Eingabegeräts liefert einen digitalen Wert, der angibt, ob sie im gedrückten Zustand (unten) oder losgelassenen Zustand (oben) ist. Aus Effizienzgründen werden die Ablesewerte der Tasten nicht als einzelne boolesche Werte dargestellt, sondern in Bitfeldern zusammengefasst, die durch gerätespezifische Enumerationen wie [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) dargestellt werden. Zum Ablesen bestimmter Tasten wird eine bitweise Maskierung verwendet, um die relevanten Werte zu isolieren. Eine Taste ist gedrückt (unten), wenn ihr Bit entsprechend festgelegt ist, andernfalls ist sie losgelassen (oben).

So wird der Gedrückt- oder Losgelassen-Zustand für einzelne Tasten bestimmt; hier anhand von Gamepads, aber das gleiche Prinzip gilt ebenso für andere Eingabegeräte wie Arcade-Joysticks, Rennlenkräder usw.

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

Wie man sieht, ist der Zustand einzelner Tasten leicht zu bestimmen; manchmal müssen Sie aber feststellen, ob mehrere oder verschiedene, in bestimmter Weise angeordnete Tasten gedrückt bzw. losgelassen sind. Mehrere Tasten sind komplexer zu testen als einzelne Tasten&mdash;, insbesondere bei gemischten Tastenzuständen&mdash;; es gibt jedoch eine einfache Formel, um sowohl einzelne als auch mehrere Tasten zu testen.

Im folgenden Beispiel wird festgestellt, ob die beiden Gamepad-Tasten A und B gedrückt sind.

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

Im folgenden Beispiel wird festgestellt, ob die beiden Gamepad-Tasten A und B losgelassen sind.

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

Im folgenden Beispiel wird festgestellt, ob Gamepad-Taste A gedrückt und Taste B losgelassen ist.

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Die Formel, die in allen fünf Beispielen verwendet wird, ist wie folgt aufgebaut: Die Anordnung der zu überprüfenden Tasten wird durch den Ausdruck auf der linken Seite des Gleichheitsoperators angegeben, während die Tasten, die berücksichtigt werden sollen, durch den Maskierungsausdruck auf der rechten Seite ausgewählt werden.

Das folgende Beispiel zeigt die Formel noch deutlicher, da das vorherige Beispiel umgeschrieben wurde.

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Diese Formel kann zum Testen beliebig vieler Tasten, Anordnungen und Zustände verwendet werden.

## <a name="get-the-state-of-the-battery"></a>Abrufen des Akkustatus

Für alle Gamecontroller, die die [IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)-Benutzeroberfläche implementieren, rufen Sie [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) auf der Controllerinstanz auf, um ein [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport)-Objekt mit Informationen zum Akku im Controller zu erhalten. Sie erhalten Eigenschaften wie die Rate, mit der der Akku geladen wird ([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), die geschätzte Energiekapazität eines neuen Akkus ([DesignCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)), und die vollständige Energieeffizienz des aufgeladenen aktuellen Akkus ([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)).

Für Gamecontroller, die detaillierte Berichte für den Akku unterstützen, erhalten Sie diese und weitere Informationen zum Akku, wie unter [Abrufen von Akkuinformationen](../devices-sensors/get-battery-info.md) aufgeführt. Die meisten Gamecontroller unterstützen jedoch nicht die Akkuberichtsstufe und verwenden kostengünstige Hardware. Für diese Domänencontroller müssen Sie Folgendes bedenken:

* **ChargeRateInMilliwatts** und **DesignCapacityInMilliwattHours** müssen immer **NULL** sein.

* Sie erhalten den Prozentsatz des Akkus durch die Berechnung von [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours) / **FullChargeCapacityInMilliwattHours**. Sollten Sie die Werte dieser Eigenschaften ignorieren und nur mit den berechneten Prozentsätzen arbeiten.

* Der Prozentsatz der vorherigen Aufzählungszeichen ist immer einer der folgenden:

    * 100 % (Voll)
    * 70 % (Mittel)
    * 40 % (Niedrig)
    * 10 % (Kritisch)

Wenn Ihr Code eine Aktion (z. B. das Zeichnen der UI) basierend auf dem Prozentsatz der verbleibende Akkulebensdauer ausführt, stellen Sie sicher, dass er den oben genannten Werten entspricht. Wenn Sie beispielsweise den Spieler warnen möchten, dass der Controller-Akku fast leer ist, wenn dieser 10 % erreicht.

## <a name="see-also"></a>Weitere Informationen:

* [Windows. System. User-Klasse](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Windows. Gaming. Input. igamecontroller-Schnittstelle](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows. Gaming. Input. gamepadbuttons-Aufzählung](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
