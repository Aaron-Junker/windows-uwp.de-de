---
title: Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel
description: Informieren Sie sich über die wichtigsten Methoden, die bei der Arbeit mit Eingabegeräten zu berücksichtigen sind.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Input, Sample
ms.localizationpriority: medium
ms.openlocfilehash: d4c3742ed843deca9d7d8edba033addd2e4888fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172074"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel




Universelle Windows-Plattform (UWP) Spiele auf einer Vielzahl von Geräten ausgeführt werden, z. b. auf Desktop Computern, Laptops und Tablets. Ein Gerät kann über eine Vielzahl von Eingabe-und Steuerungsmechanismen verfügen. In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Eingabegeräten arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden.

> [!NOTE]
> Den diesem Dokument entsprechenden Beispielcode finden Sie im [DirectX Marble Maze-Spielbeispiel](https://github.com/microsoft/Windows-appsample-marble-maze).

 
Hier sind einige der wichtigsten in diesem Dokument erörterten Punkte für das Arbeiten mit Eingaben in Ihrem Spiel:

-   Unterstützen Sie nach Möglichkeit mehrere Eingabegeräte, damit die Kunden Ihr Spiel ganz nach ihren persönlichen Vorlieben und Fähigkeiten spielen können. Die Verwendung eines Gamecontrollers und Sensors ist zwar optional, wird aber dringend empfohlen, um die Benutzerfreundlichkeit für die Spieler zu erhöhen. Wir haben die Spiele Controller-und Sensor-APIs entworfen, damit Sie diese Eingabegeräte einfacher integrieren können.

-   Zum Initialisieren der Toucheingabe müssen Sie sich für Windows-Ereignisse registrieren, beispielsweise für das Aktivieren, Freigeben und Verschieben des Zeigers. Zum Initialisieren des Beschleunigungsmessers erstellen Sie ein [Windows::Devices::Sensors::Accelerometer](/uwp/api/Windows.Devices.Sensors.Accelerometer)-Objekt, wenn Sie die Anwendung initialisieren. Der Xbox-Controller erfordert keine Initialisierung.

-   Für Einzelspieler Spiele sollten Sie überprüfen, ob Eingaben von allen möglichen Xbox-Controllern kombiniert werden. Auf diese Weise müssen Sie nicht nachverfolgen, welche Eingabe von welchem Controller kommt. Sie können auch einfach nur Eingaben vom zuletzt hinzugefügten Controller nachverfolgen, wie in diesem Beispiel.

-   Verarbeiten Sie erst Windows-Ereignisse und dann Eingabegeräte.

-   Der Xbox-Controller und der Beschleunigungsmesser unterstützen den Abruf. Das heißt, Sie können Daten abrufen, wenn Sie sie benötigen. Für die Toucheingabe sollten Sie Touchereignisse in Datenstrukturen aufzeichnen, die für Ihren Eingabeverarbeitungscode verfügbar sind.

-   Überlegen Sie, ob Sie Eingabewerte in ein gemeinsames Format normalisieren möchten. Auf diese Weise können Sie die Interpretation der Eingabe durch andere Komponenten des Spiels wie beispielsweise die Simulation von Physikeffekten vereinfachen und leichter Spiele schreiben, die sich für unterschiedliche Bildschirmauflösungen eignen.

## <a name="input-devices-supported-by-marble-maze"></a>Von Marble Maze unterstützte Eingabegeräte


Marble Maze unterstützt den Xbox-Controller, die Maus und die Fingereingabe, um Menü Elemente auszuwählen, und den Xbox-Controller, die Maus, die Fingereingabe und den Beschleunigungsmesser zum Steuern von Spiel spielen. Marble Maze verwendet die [Windows:: Gaming:: Input](/uwp/api/windows.gaming.input) -APIs, um den Controller auf Eingaben abzufragen. Bei der Toucheingabe können Anwendungen Eingaben mit der Fingerspitze nachverfolgen und darauf reagieren. Ein Beschleunigungsmesser ist ein Sensor, der die auf die X-, Y-und Z-Achse angewendete Kraft misst. Mit der Windows-Runtime können Sie den aktuellen Zustand des Beschleunigungsmessers abrufen und Touchereignisse über den Ereignisbehandlungsmechanismus der Windows-Runtime empfangen.

> [!NOTE]
> In diesem Dokument wird die Fingereingabe verwendet, um auf ein beliebiges Gerät zu verweisen, das Zeiger Ereignisse verwendet. Da bei der Touch- und Mauseingabe Standardzeigerereignisse verwendet werden, können Sie jedes der Geräte verwenden, um Menüelemente auszuwählen und den Spielverlauf zu steuern.

 

> [!NOTE]
> Das Paket Manifest legt **Landscape** als einzige unterstützte Drehung für das Spiel fest, um zu verhindern, dass sich die Ausrichtung ändert, wenn Sie das Gerät drehen, um das Spiel für den Marmor auszuführen. Öffnen Sie " **Package. appxmanifest** " im **Projektmappen-Explorer** in Visual Studio, um das Paket Manifest anzuzeigen.

 

## <a name="initializing-input-devices"></a>Initialisieren von Eingabegeräten


Der Xbox-Controller erfordert keine Initialisierung. Um Touch zu initialisieren, müssen Sie sich für windowingereignisse registrieren, z. b. wenn der Zeiger aktiviert ist (z. b., wenn der Player die Maustaste drückt oder den Bildschirm berührt), freigegeben und verschoben wird. Zum Initialisieren des Beschleunigungsmessers müssen Sie ein [Windows::Devices::Sensors::Accelerometer](/uwp/api/Windows.Devices.Sensors.Accelerometer)-Objekt erstellen, wenn Sie die Anwendung initialisieren.

Im folgenden Beispiel wird gezeigt, wie die **App:: SetWindow** -Methode für die Zeiger Ereignisse [Windows:: UI:: Core:: corewindow::P ointerpressed](/uwp/api/windows.ui.core.corewindow.PointerPressed), [Windows:: UI:: Core:: corewindow::P ointerreleased](/uwp/api/windows.ui.core.corewindow.PointerReleased)und [Windows:: UI:: Core:: corewindow::P ointerverschoder](/uwp/api/windows.ui.core.corewindow.PointerMoved) Zeiger registriert wird. Diese Ereignisse werden während der Anwendungs Initialisierung und vor der Spiel Schleife registriert.

Die Ereignisse werden in einem separaten Thread behandelt, über den die Ereignishandler aufgerufen werden.

Weitere Informationen zum Initialisieren der Anwendung finden Sie unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md).

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

Die **marblemazemain** -Klasse erstellt auch ein **Std:: Map** -Objekt zum Speichern von Berührungs Ereignissen. Der Schlüssel für dieses Zuordnungsobjekt ist ein Wert, der den Eingabezeiger eindeutig identifiziert. Jeder Schlüssel ist der Entfernung zwischen jedem Touchpunkt und der Mitte des Bildschirms zugeordnet. Diese Werte verwendet Marble Maze später, um die Neigung des Labyrinths zu berechnen.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

Die **marblemazemain** -Klasse enthält auch ein [Beschleunigungsmesser](/uwp/api/Windows.Devices.Sensors.Accelerometer) -Objekt.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

Das **Accelerometer** -Objekt wird im **marblemazemain** -Konstruktor initialisiert, wie im folgenden Beispiel gezeigt. Die [Windows::D EVICES:: Sensors:: Accelerometer:: GetDefault](/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) -Methode gibt eine Instanz des Standard Zugriffs Messers zurück. Wenn kein Standard Beschleunigungsmesser vorhanden ist, gibt **Accelerometer:: GetDefault** den Wert **nullptr**zurück.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navigieren in den Menüs

Sie können die Maus, den Touchscreen oder den Xbox-Controller verwenden, um die Menüs wie folgt zu navigieren:

-   Verwenden Sie das Steuerkreuz, um das aktive Menüelement zu ändern.
-   Verwenden Sie die Schaltfläche "berühren", eine Schaltfläche oder die Menü Schaltfläche, um ein Menü Element auszuwählen oder das aktuelle Menü zu schließen, z. b. die Tabelle mit dem höchsten
-   Verwenden Sie die Menü Schaltfläche, um das Spiel anzuhalten oder fortzusetzen.
-   Klicken Sie mit der Maus auf ein Menüelement, um die jeweilige Aktion auszuwählen.

###  <a name="tracking-xbox-controller-input"></a>Nachverfolgen der Xbox Controller-Eingabe

Um die momentan mit dem Gerät verbundenen Gamepads nachzuverfolgen, definiert **marblemazemain** eine Member-Variable, **m_myGamepads**, die eine Auflistung von [Windows:: Gaming:: Input:: Gamepad](/uwp/api/windows.gaming.input.gamepad) -Objekten ist. Dies wird im Konstruktor wie folgt initialisiert:

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

Außerdem registriert der **marblemazemain** -Konstruktor Ereignisse für den Fall, dass Gamepads hinzugefügt oder entfernt werden:

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

Wenn ein Gamepad hinzugefügt wird, wird es **m_myGamepads**hinzugefügt. Wenn ein Gamepad entfernt wird, überprüfen wir, ob sich das Gamepad in **m_myGamepads**befindet, und wenn dies der Fall ist, wird es entfernt. In beiden Fällen legen wir **m_currentGamepadNeedsRefresh** auf " **true**" fest, um anzugeben, dass **m_gamepad**neu zugewiesen werden muss.

Zum Schluss weisen wir **m_gamepad** ein Gamepad zu und legen **m_currentGamepadNeedsRefresh** auf " **false**" fest:

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

In der **Update** -Methode wird überprüft, ob **m_gamepad** neu zugewiesen werden muss:

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

Wenn **m_gamepad** neu zugewiesen werden muss, weisen wir ihm das zuletzt hinzugefügte Gamepad mithilfe von **getlastgamepad**zu, das wie folgt definiert ist:

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

Diese Methode gibt einfach das letzte Gamepad in **m_myGamepads**zurück.

Sie können bis zu vier Xbox-Controller mit einem Windows 10-Gerät verbinden. Um zu vermeiden, dass Sie ermitteln müssen, welcher Controller der aktive Controller ist, verfolgen wir einfach nur das zuletzt hinzugefügte Gamepad. Wenn Ihr Spiel mehrere Spieler unterstützt, müssen Sie die Eingabe für jeden Spieler getrennt nachverfolgen.

Die **marblemazemain:: Update** -Methode fragt das Gamepad nach Eingaben ab:

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

Wir verfolgen die Eingaben, die wir im letzten Frame mit **m_oldReading**erhalten haben, und die letzte Eingabe mit **m_newReading**, die wir durch Aufrufen von " [Gamepad:: getcurrentreading](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading)" erhalten. Dies gibt ein [gamepadreading](/uwp/api/windows.gaming.input.gamepadreading) -Objekt zurück, das Informationen über den aktuellen Zustand des Gamepad enthält.

Um zu überprüfen, ob eine Schaltfläche gerade gedrückt oder freigegeben wurde, definieren wir " **marblemazemain:: buttonjustpressed** " und " **marblemazemain:: buttonjustrelease**", die Schaltflächen Messwerte aus diesem Frame und dem letzten Frame vergleichen. Auf diese Weise können wir eine Aktion nur zu dem Zeitpunkt ausführen, zu dem eine Schaltfläche anfänglich gedrückt oder freigegeben wird, und nicht, wenn Sie gespeichert wird:

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

[Gamepadbuttons](/uwp/api/windows.gaming.input.gamepadbuttons) -Messwerte werden mithilfe von bitweisen Vorgängen verglichen &mdash; . wir überprüfen, ob eine Schaltfläche mithilfe des *bitweisen and* (&) gedrückt wird. Wir legen fest, ob eine Schaltfläche durch einen Vergleich der alten Lese-und neuen Lesevorgänge einfach gedrückt oder freigegeben wurde.

Mithilfe der oben genannten Methoden überprüfen wir, ob bestimmte Schaltflächen gedrückt wurden, und führen alle entsprechenden Aktionen aus, die ausgeführt werden müssen. Wenn beispielsweise die Menü Schaltfläche (**gamepadbuttons:: Menu**) gedrückt wird, ändert sich der Spielzustand von aktiv in angehalten oder angehalten in aktiv.

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

Wir überprüfen auch, ob der Player die Schaltfläche "Ansicht" drückt. in diesem Fall wird das Spiel neu gestartet oder die Tabelle "High Score" gelöscht:

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

Wenn das Hauptmenü aktiv ist, ändert sich das aktive Menüelement, sobald das Steuerkreuz nach oben oder nach unten gedrückt wird. Wenn der Benutzer die aktuelle Auswahl auswählt, wird das entsprechende UI-Element als ausgewählt markiert.

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>Nachverfolgen von Touch- und Mauseingaben

Bei der Touch- und Mauseingabe wird ein Menüelement ausgewählt, wenn der Benutzer das Element berührt oder darauf klickt. Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: Update** -Methode Zeiger Eingaben für SELECT-Menü Elemente verarbeitet. Die Member-Variable **m \_ pointqueue** verfolgt die Speicherorte, an denen der Benutzer auf dem Bildschirm berührt oder darauf geklickt hat. Die Art und Weise, in der Marble Maze Zeiger Eingaben sammelt, wird weiter unten in diesem Dokument im Abschnitt [Verarbeiten von Zeiger Eingaben](#processing-pointer-input)ausführlicher beschrieben.

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

Die **UserInterface::HitTest**-Methode ermittelt, ob sich der angegebene Punkt innerhalb der Grenzen eines UI-Elements befindet. Alle UI-Elemente, die diesen Test bestanden haben, werden als berührt markiert. Diese Methode ermittelt mit der **PointInRect**-Hilfsfunktion, ob sich der angegebene Punkt innerhalb der Grenzen der einzelnen UI-Elemente befindet.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>Aktualisieren des Spielzustands

Nachdem die **marblemazemain:: Update** -Methode Controller-und Berührungs Eingaben verarbeitet hat, aktualisiert Sie den Spielzustand, wenn eine beliebige Schaltfläche gedrückt wurde.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>Steuern des Spielverlaufs


Die Spiel Schleife und die **marblemazemain:: Update** -Methode arbeiten zusammen, um den Status von Spielobjekten zu aktualisieren. Wenn Ihr Spiel Eingaben von mehreren Geräten akzeptiert, können Sie die Eingaben aller Geräte in einem Satz von Variablen sammeln. Auf diese Weise können Sie Code schreiben, der sich leichter verwalten lässt. Die **marblemazemain:: Update** -Methode definiert einen Satz von Variablen, der die Bewegung von allen Geräten akkumuliert.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

Die Eingabemechanismen der einzelnen Eingabegeräte können dabei unterschiedlich sein. Beispielsweise werden Zeigereingaben mit dem Ereignisbehandlungsmodell der Windows-Runtime behandelt. Umgekehrt rufen Sie die Eingabedaten von Xbox Controller ab, wenn Sie Sie benötigen. Am besten halten Sie sich immer an die Eingabemechanismen, die für ein bestimmtes Gerät vorgeschrieben sind. In diesem Abschnitt wird beschrieben, wie Marble Maze die Eingaben von den einzelnen Geräten liest, die kombinierten Eingabewerte aktualisiert und sie verwendet, um den Spielzustand zu aktualisieren.

###  <a name="processing-pointer-input"></a>Verarbeiten von Zeigereingaben

Wenn Sie mit Zeigereingaben arbeiten, rufen Sie die [Windows::UI::Core::CoreDispatcher::ProcessEvents](/uwp/api/windows.ui.core.coredispatcher.processevents)-Methode auf, um Windows-Ereignisse zu verarbeiten. Rufen Sie diese Methode in Ihrer Spielschleife auf, bevor Sie die Szene aktualisieren oder rendern. In Marble Maze wird dies in der **App:: Run** -Methode aufgerufen: 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

Wenn das Fenster sichtbar ist, übergeben wir **coreprocessiebsoption::P rocesallifpresent** an **ProcessEvents** , um alle Ereignisse in der Warteschlange zu verarbeiten und sofort zurückzukehren. Andernfalls übergeben wir **coreprocesereigsoption::P rocessoneandallpending** , um alle Ereignisse in der Warteschlange zu verarbeiten und auf das nächste neue Ereignis zu warten. Nach der Verarbeitung der Ereignisse rendert Marble Maze den nächsten Frame und zeigt ihn an.

Die Windows-Runtime ruft den registrierten Handler für jedes aufgetretene Ereignis auf. Die **App:: SetWindow** -Methode registriert sich für Ereignisse und leitet Zeiger Informationen an die **marblemazemain** -Klasse weiter.

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

Die **marblemazemain** -Klasse reagiert auf Zeiger Ereignisse, indem das Map-Objekt aktualisiert wird, das Berührungs Ereignisse enthält. Die **marblemazemain:: addtouch** -Methode wird aufgerufen, wenn der Zeiger zum ersten Mal gedrückt wird, z. b. wenn der Benutzer den Bildschirm anfänglich auf einem touchfähigen Gerät berührt. Die **marblemazemain:: updatetouch** -Methode wird aufgerufen, wenn die Zeigerposition verschoben wird. Die **marblemazemain:: removetouch** -Methode wird aufgerufen, wenn der Zeiger freigegeben wird, z. b. wenn der Benutzer den Bildschirm nicht mehr berührt.

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

Die **pointpotouchfunktion** übersetzt die aktuelle Zeigerposition, sodass sich der Ursprung in der Mitte des Bildschirms befindet, und skaliert dann die Koordinaten, sodass Sie ungefähr zwischen-1,0 und + 1,0 liegen. Dadurch kann die einheitliche Neigung des Labyrinths für verschiedene Eingabemethoden leichter berechnet werden.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

Die **marblemazemain:: Update** -Methode aktualisiert die kombinierten Eingabewerte, indem der Neigungs Faktor um einen konstanten Skalierungs Wert erhöht wird. Dieser Skalierungswert wurde durch Experimentieren mit verschiedenen Werten ermittelt.

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>Verarbeiten von Beschleunigungsmessereingaben

Um die Beschleunigungsmesser Eingabe zu verarbeiten, ruft die **marblemazemain:: Update** -Methode die [Windows::D EVICES:: Sensors:: Beschleunigungsmesser:: getcurrentreading](/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) -Methode auf. Diese Methode gibt ein [Windows::Devices::Sensors::AccelerometerReading](/uwp/api/Windows.Devices.Sensors.AccelerometerReading)-Objekt zurück, das die Ablesung eines Beschleunigungsmessers darstellt. Die Eigenschaften **Windows::D EVICES:: Sensors:: accelerometerreading:: accelerationx** und **Windows::D EVICES:: Sensors:: accelerometerreading:: accelerationy** enthalten die g-Force-Beschleunigung entlang der X-bzw. Y-Achse.

Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: Update** -Methode den Beschleunigungsmesser abruft und die kombinierten Eingabewerte aktualisiert. Wenn Sie das Gerät neigen, bewegt sich die Murmel aufgrund der Schwerkraft schneller.

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

Da Sie nicht sicher sein können, dass auf dem Computer des Benutzers ein Beschleunigungsmesser vorhanden ist, sollten Sie immer sicherstellen, dass Sie über ein gültiges [Beschleunigungsmesser](/uwp/api/Windows.Devices.Sensors.Accelerometer) Objekt verfügen, bevor Sie den Beschleunigungsmesser abrufen.

### <a name="processing-xbox-controller-input"></a>Verarbeiten der Xbox Controller-Eingabe

In der **marblemazemain:: Update** -Methode verwenden wir **m_newReading** , um Eingaben vom linken analogen Stick zu verarbeiten:

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

Wir überprüfen, ob die Eingabe vom linken analogen Stick außerhalb der unzustellbaren Zone liegt, und wenn dies der Fall ist, fügen wir Sie zu **combinedtiltx** und **combinedtilty** (multipliziert mit einem Skalierungsfaktor) hinzu, um die Stufe zu kippen.

> [!IMPORTANT]
> Wenn Sie mit dem Xbox-Controller arbeiten, müssen Sie immer die unzustellbare Zone berücksichtigen. Der inaktive Bereich bezieht sich auf die unterschiedliche Empfindlichkeit von Gamepads für die ersten Bewegungen. Bei manchen Controllern ergibt eine kleine Bewegung keinen Messwert, während sie bei anderen zu einem messbaren Ergebnis führt. Berücksichtigen Sie dies in Ihrem Spiel, indem Sie für die ersten Bewegungen des Ministicks einen Bereich für Nichtbewegungen erstellen. Weitere Informationen zur unzustellbaren Zone finden Sie unter [Lesen der](gamepad-and-vibration.md#reading-the-thumbsticks)Fingerabdrücke.

 

###  <a name="applying-input-to-the-game-state"></a>Anwenden von Eingaben auf den Spielzustand

Geräte melden Eingabewerte auf unterschiedliche Weise. So wird eine Zeigereingabe möglicherweise in Bildschirmkoordinaten angegeben, eine Controllereingabe aber in einem völlig anderen Format. Beim Kombinieren der Eingaben von mehreren Geräten in einem Satz von Eingabewerten besteht eine Herausforderung darin, die Werte in ein gemeinsames Format zu normalisieren oder konvertieren. Marble Maze normalisiert Werte, indem Sie Sie auf den Bereich \[ -1,0, 1,0, Skalieren \] . Die **pointtoken** -Funktion, die zuvor in diesem Abschnitt beschrieben wurde, konvertiert Bildschirm Koordinaten in normalisierte Werte, die ungefähr zwischen-1,0 und + 1,0 reichen.

> [!TIP]
> Auch wenn die Anwendung eine Eingabemethode verwendet, empfiehlt es sich, die Eingabewerte immer zu normalisieren. Auf diese Weise können Sie die Interpretation der Eingabe durch andere Komponenten des Spiels wie beispielsweise die Simulation von Physikeffekten vereinfachen und leichter Spiele schreiben, die sich für unterschiedliche Bildschirmauflösungen eignen.

 

Nachdem die **marblemazemain:: Update** -Methode Eingaben verarbeitet hat, wird ein Vektor erstellt, der die Auswirkung der Neigung des Maze auf den Marmor darstellt. Das folgende Beispiel zeigt, wie Marble Maze mit der [XMVector3Normalize](/windows/desktop/api/directxmath/nf-directxmath-xmvector3normalize)-Funktion einen normalisierten Schwerkraftvektor erstellt. Die Variable **maxtilt** schränkt den Betrag ein, um den das Maze kippt und verhindert, dass das Labyrinth auf der Seite gekippt wird.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

Als letzten Schritt beim Aktualisieren der Szenenobjekte übergibt Marble Maze den aktualisierten Schwerkraftvektor der Simulation für Physikeffekte, aktualisiert diese Simulation für den seit dem vorherigen Frame verstrichenen Zeitraum und aktualisiert die Position und Ausrichtung der Murmel. Wenn das Labyrinth durch das Labyrinth gefallen hat, platziert die **marblemazemain:: Update** -Methode den Marmor an dem letzten Prüfpunkt, den der Marmor berührt hat, und setzt den Zustand der Physik Simulation zurück.

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

In diesem Abschnitt wird nicht beschrieben, wie die Simulation für Physikeffekte funktioniert. Weitere Informationen hierzu finden Sie unter " **Physik. h** " und " **Physik. cpp** " in den Marble Maze-Quellen.

## <a name="next-steps"></a>Nächste Schritte


Lesen Sie [Hinzufügen von Audio zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md). Dort finden Sie Informationen zu einigen der wichtigen Methoden, die Sie beim Arbeiten mit Audio berücksichtigen sollten. In diesem Dokument wird erörtert, wie Marble Maze mithilfe von Microsoft Media Foundation und XAudio2 Audioressourcen lädt, mischt und wiedergibt.

## <a name="related-topics"></a>Zugehörige Themen


* [Hinzufügen von Audio zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md)
* [Hinzufügen von visuellem Inhalt zum Marble Maze-Beispiel](adding-visual-content-to-the-marble-maze-sample.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 