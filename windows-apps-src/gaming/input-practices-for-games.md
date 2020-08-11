---
title: Eingabemethoden für Spiele
description: Erfahren Sie mehr über Muster und Verfahren zur effizienten Verwendung von Eingabegeräten.
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Input
ms.localizationpriority: medium
ms.openlocfilehash: aa2036cb8d91b17d084e4e4922d01d4256bdb7de
ms.sourcegitcommit: 29eb375bc634bf733be58107c1d648dc818da7f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88051325"
---
# <a name="input-practices-for-games"></a>Eingabemethoden für Spiele

In diesem Thema werden Muster und Techniken für die effektive Verwendung von Eingabegeräten in universelle Windows-Plattform spielen (UWP) beschrieben.

In diesem Thema lernen Sie Folgendes:

* Nachverfolgen von Spielern und den von ihnen aktuell verwendeten Eingabe- und Navigationsgeräten
* Erkennen von Tastenübergängen (von „Gedrückt“ zu „Losgelassen“ oder von „Losgelassen“ zu „Gedrückt“)
* Erkennen komplexer Tastenanordnungen mit einem einzelnen Test

## <a name="choosing-an-input-device-class"></a>Auswählen einer Eingabegeräte Klasse

Ihnen stehen viele verschiedene Arten von Eingabe-APIs zur Verfügung, z. b. " [arcadebug](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick)", " [Flightstick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick)" und " [Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad)". Wie entscheiden Sie, welche API für Ihr Spiel verwendet werden soll?

Sie sollten auswählen, welche API Ihnen die am besten geeignete Eingabe für das Spiel liefert. Wenn Sie z. b. ein 2D-Plattformspiel erstellen, können Sie wahrscheinlich einfach die **Gamepad** -Klasse verwenden und sich nicht mit der zusätzlichen Funktionalität von anderen Klassen beschäftigen. Dies würde das Spiel nur auf die Unterstützung von Gamepads beschränken und eine konsistente Schnittstelle bereitstellen, die über viele verschiedene Gamepads ohne zusätzlichen Code hinweg funktioniert.

Andererseits können Sie für komplexe Flug-und Rennsimulationen alle [rawgamecontroller](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) -Objekte als Baseline auflisten, um sicherzustellen, dass Sie alle Nischen Geräte unterstützen, die von begeisterten Playern genutzt werden können, einschließlich Geräten wie separaten Pedalen oder Drosselungen, die noch von einem einzelnen Player verwendet werden. 

Von dort aus können Sie die **fromgamecontroller** -Methode einer Eingabe Klasse, wie z [. b. Gamepad. fromgamecontroller](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller), verwenden, um festzustellen, ob jedes Gerät über eine allgemeinere Ansicht verfügt. Wenn das Gerät z. b. auch ein **Gamepad**ist, können Sie die Benutzeroberfläche der Schaltflächen Zuordnung anpassen, um dies widerzuspiegeln, und einige sinnvolle Standard Schaltflächen Zuordnungen bereitstellen, aus denen Sie auswählen können. (Im Gegensatz dazu muss der Spieler die Gamepad-Eingaben manuell konfigurieren, wenn Sie nur **rawgamecontroller**verwenden.) 

Alternativ können Sie sich die Hersteller-ID (vid) und die Produkt-ID (PID) eines **rawgamecontroller** (mit [hardwarevendorid](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) bzw. [hardwareproductid](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId)) ansehen und vorgeschlagene Schaltflächen Zuordnungen für beliebte Geräte bereitstellen, während Sie weiterhin mit unbekannten Geräten kompatibel sind, die in der Zukunft über manuelle Zuordnungen durch den Player auftreten.

## <a name="keeping-track-of-connected-controllers"></a>Verfolgen verbundener Controller

Obwohl jeder Controllertyp eine Liste verbundener Controller (z [. b. Gamepad. Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)) enthält, empfiehlt es sich, eine eigene Liste von Controllern beizubehalten. Weitere Informationen finden Sie in [der Gamepads-Liste](gamepad-and-vibration.md#the-gamepads-list) (jeder Controllertyp weist einen entsprechend benannten Abschnitt zu einem eigenen Thema auf).

Was geschieht jedoch, wenn der Player den Controller entschließt oder einen neuen einfügt? Sie müssen diese Ereignisse behandeln und die Liste entsprechend aktualisieren. Weitere Informationen finden Sie unter [Hinzufügen und Entfernen von Gamepads](gamepad-and-vibration.md#adding-and-removing-gamepads) (auch hier hat jeder Controllertyp einen ähnlich benannten Abschnitt zu einem eigenen Thema).

Da die hinzugefügten und entfernten Ereignisse asynchron ausgelöst werden, erhalten Sie möglicherweise falsche Ergebnisse beim Umgang mit der Liste der Controller. Daher sollten Sie immer dann, wenn Sie auf die Controller Liste zugreifen, eine Sperre dafür durchführen, dass jeweils nur ein Thread darauf zugreifen kann. Dies kann mit dem [Concurrency Runtime](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime), insbesondere der CRITICAL_SECTION- [Klasse](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class), in ** &lt; ppl. h &gt; **erfolgen.

Ein weiterer Aspekt ist, dass die Liste der verbundenen Controller anfänglich leer ist und ein zweites oder zwei zum Auffüllen benötigt. Wenn Sie also nur das aktuelle Gamepad in der Start-Methode zuweisen, ist es **null**!

Um dies zu beheben, sollten Sie über eine Methode verfügen, die das Haupt Gamepad "aktualisiert" (in einem Einzelspielerspiel; Multiplayerspiele benötigen anspruchsvollere Lösungen). Sie sollten diese Methode dann sowohl in Ihrem Controller hinzugefügten Controller als auch in der Update-Methode als von einem Controller entfernte Ereignishandler oder in der Update-Methode

Die folgende Methode gibt einfach das erste Gamepad in der Liste zurück (oder **nullptr** , wenn die Liste leer ist). Dann müssen Sie sich nur noch einmal auf **nullptr** überprüfen, wenn Sie mit dem Controller etwas tun. Es liegt an Ihnen, ob Sie das Spiel blockieren möchten, wenn kein Controller verbunden ist (z. b. durch Anhalten des Spiels), oder wenn Sie einfach das Design fortsetzen möchten, während die Eingabe ignoriert wird.

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

Im folgenden finden Sie ein Beispiel für die Handhabung von Eingaben von einem Gamepad:

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

## <a name="tracking-users-and-their-devices"></a>Nachverfolgen von Benutzern und ihren Geräten

Alle Eingabegeräte sind einem [Benutzer](https://docs.microsoft.com/uwp/api/windows.system.user) zugeordnet, damit seine Identität mit seinem Spiel, seinen Erfolgen, geänderten Einstellungen und anderen Aktivitäten verknüpft werden kann. Benutzer können sich bei der Ausführung anmelden oder abmelden, und es ist üblich, dass sich ein anderer Benutzer auf einem Eingabegerät anmeldet, das mit dem System verbunden bleibt, nachdem sich der vorherige Benutzer abgemeldet hat. Wenn ein Benutzer sich anmeldet, wird das Ereignis [igamecontroller. userchanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged) ausgelöst. Sie können einen Ereignishandler für dieses Ereignis registrieren, um Spieler und die von ihnen verwendeten Geräte nachzuverfolgen.

Die Benutzeridentität ist auch die Art und Weise, in der ein Eingabegerät dem entsprechenden [UI-Navigations Controller](ui-navigation-controller.md)zugeordnet ist.

Aus diesen Gründen sollte die Eingabe von Player nachverfolgt und mit der [User](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) -Eigenschaft der Device-Klasse (geerbt von der [igamecontroller](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller) -Schnittstelle) korreliert werden.

Das [usergamepadplpyinguwp](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/) -Beispiel veranschaulicht, wie Sie die Benutzer und die von Ihnen verwendeten Geräte nachverfolgen können.

## <a name="detecting-button-transitions"></a>Erkennen von Tastenübergängen

In einigen Fällen möchten Sie wissen, wann eine Taste zuerst gedrückt oder losgelassen wird, also genau, wann der Zustand der Taste von „Losgelassen“ in „Gedrückt“ oder von „Gedrückt“ in „Losgelassen“ übergegangen ist. Um dies zu ermitteln und herauszufinden, was sich geändert hat, müssen Sie den vorherigen Geräte-Ablesewert beibehalten und mit dem aktuellen Ablesewert vergleichen.

Im folgenden Beispiel wird eine grundlegende Vorgehensweise zum Speichern der vorherigen Lektüre veranschaulicht. Gamepads werden hier angezeigt, aber die Prinzipien sind für den Faden Strich, das Rennrad und die anderen Eingabegeräte Typen identisch.

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

Im folgenden Beispiel wird ein grundlegender Ansatz zum Erkennen von Schaltflächen Übergängen veranschaulicht:

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

Diese beiden Funktionen leiten zuerst den booleschen Zustand der Schaltflächen Auswahl von `newReading` und ab und `oldReading` führen dann eine boolesche Logik aus, um zu bestimmen, ob der Ziel Übergang aufgetreten ist. Diese Funktionen geben nur **true** zurück, wenn der neue Ablesewert den Zielzustand enthält („Gedrückt“ bzw. „Losgelassen“) *und* der alte Ablesewert nicht auch den Zielzustand enthält. Andernfalls geben sie **false** zurück.

## <a name="detecting-complex-button-arrangements"></a>Erkennen komplexer Tastenanordnungen

Jede Schaltfläche eines Eingabe Geräts bietet ein digitales Lesematerial, das angibt, ob es gedrückt oder freigegeben wird (nach oben). Aus Effizienzgründen werden die Ablesewerte der Tasten nicht als einzelne boolesche Werte dargestellt, sondern in Bitfeldern zusammengefasst, die durch gerätespezifische Enumerationen wie [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) dargestellt werden. Zum Lesen bestimmter Tastenwerte wird eine bitweise Maskierung verwendet, um die für Sie relevanten Werte zu isolieren. Eine Schaltfläche wird gedrückt (unten), wenn das entsprechende Bit festgelegt ist. Andernfalls wird Sie freigegeben (up).

Erinnern Sie sich, wie einzelne Schaltflächen als gedrückt oder freigegeben festgelegt werden. Gamepads werden hier angezeigt, aber die Prinzipien sind für den Faden Strich, das Rennrad und die anderen Eingabegeräte Typen identisch.

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

Wie Sie sehen können, ist die Bestimmung des Zustands einer einzelnen Schaltfläche geradlinig, aber manchmal möchten Sie möglicherweise feststellen, ob mehrere Schaltflächen gedrückt oder freigegeben werden, oder ob eine Reihe von Schaltflächen auf eine bestimmte Art und Weise angeordnet ist &mdash; , einige nicht. Das Testen mehrerer Schaltflächen ist komplexer als das Testen einzelner Schaltflächen &mdash; , vor allem mit dem Potenzial des &mdash; gemischtem Schaltflächen Zustands, aber es gibt eine einfache Formel für diese Tests, die für einzelne und mehrere Schaltflächen Tests gleichermaßen gilt.

Im folgenden Beispiel wird bestimmt, ob die Gamepad-Schaltflächen A und B gedrückt werden:

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

Im folgenden Beispiel wird bestimmt, ob die Gamepad-Schaltflächen A und B freigegeben werden:

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

Im folgenden Beispiel wird bestimmt, ob Gamepad Button A gedrückt wird, während Schaltfläche B losgelassen wird:

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Die Formel, die in allen fünf Beispielen verwendet wird, ist wie folgt aufgebaut: Die Anordnung der zu überprüfenden Tasten wird durch den Ausdruck auf der linken Seite des Gleichheitsoperators angegeben, während die Tasten, die berücksichtigt werden sollen, durch den Maskierungsausdruck auf der rechten Seite ausgewählt werden.

Im folgenden Beispiel wird diese Formel deutlicher veranschaulicht, indem das vorherige Beispiel umgeschrieben wird:

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Die Formel kann zum Testen einer beliebigen Anzahl von Tasten in einer beliebigen Anordnung ihrer Zustände verwendet werden.

## <a name="get-the-state-of-the-battery"></a>Den Zustand des Akkus erhalten

Für alle Spiele Controller, die die [igamecontrollerbatteryinfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo) -Schnittstelle implementieren, können Sie [trygetbatteryreport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) für die Controller Instanz aufrufen, um ein [batteryreport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) -Objekt zu erhalten, das Informationen über den Akku im Controller bereitstellt. Sie können Eigenschaften wie die Geschwindigkeit, mit der der Akku belastet wird ([chargerateinmilliwatt](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), die geschätzte Energiekapazität eines neuen Akkus ([designcapacityinmilliwatthours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) und die vollständig berechnete Energiekapazität des aktuellen Akkus ([fullchargecapacityinmilliwatthours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)) erhalten.

Für Game Controller, die eine ausführliche Akku Berichterstattung unterstützen, können Sie diese und weitere Informationen zum Akku erhalten, wie in " [Get Akku Information](../devices-sensors/get-battery-info.md)" beschrieben. Allerdings unterstützen die meisten Spiele Controller diese Ebene der Akku Berichterstattung nicht, sondern verwenden stattdessen kostengünstige Hardware. Für diese Controller müssen Sie die folgenden Aspekte berücksichtigen:

* **Chargerateinmilliwatt** und **designcapacityinmilliwatthours** sind immer **null**.

* Sie können den Akku Prozentsatz durch Berechnen von [restingcapacityinmilliwatthours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours)  /  **fullchargecapacityinmilliwatthours**erhalten. Die Werte dieser Eigenschaften sollten ignoriert werden, und es sollte nur der berechnete Prozentsatz behandelt werden.

* Der Prozentsatz aus dem vorherigen Aufzählungs Punkt ist immer eine der folgenden:

    * 100% (vollständig)
    * 70% (Mittel)
    * 40% (niedrig)
    * 10% (kritisch)

Wenn Ihr Code Aktionen (z. b. das Zeichnen der Benutzeroberfläche) durchführt, die auf dem Prozentsatz der verbleibenden Akku Lebensdauer basieren, stellen Sie sicher, dass die Werte den obigen Wenn Sie z. b. den Player warnen möchten, wenn der Akku des Controllers niedrig ist, gehen Sie so vor, wenn er 10% erreicht.

## <a name="see-also"></a>Weitere Informationen

* [Windows.System. Benutzerklasse](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Windows. Gaming. Input. igamecontroller-Schnittstelle](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows. Gaming. Input. gamepadbuttons-Aufzählung](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
* [Usergamepadpiriinguwp-Beispiel](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/)