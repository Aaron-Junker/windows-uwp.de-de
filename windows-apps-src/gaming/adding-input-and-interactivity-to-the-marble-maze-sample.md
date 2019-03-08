---
title: Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel
description: Lernen Sie mehr über bewährte Methoden, die bei der Arbeit mit Eingabegeräten beachtet werden sollten.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Eingabe, Beispiel
ms.localizationpriority: medium
ms.openlocfilehash: d545f696a93bfa8416e1a772ecc015867a3615c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611815"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel




UWP-Spiele können auf den verschiedensten Geräten ausgeführt werden, beispielsweise auf Desktopcomputern, Laptops und Tablets. Für ein Gerät sind viele verschiedene Eingabe- und Steuerungsmechanismen möglich. In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Eingabegeräten arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden.

> [!NOTE]
> Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](https://go.microsoft.com/fwlink/?LinkId=624011).

 
Hier sind einige der wichtigsten in diesem Dokument erörterten Punkte für das Arbeiten mit Eingaben in Ihrem Spiel:

-   Unterstützen Sie nach Möglichkeit mehrere Eingabegeräte, damit die Kunden Ihr Spiel ganz nach ihren persönlichen Vorlieben und Fähigkeiten spielen können. Die Verwendung eines Gamecontrollers und Sensors ist zwar optional, wird aber dringend empfohlen, um die Benutzerfreundlichkeit für die Spieler zu erhöhen. Die APIs für Gamecontroller und Sensoren soll Ihnen die Integration dieser Eingabegeräte erleichtern.

-   Zum Initialisieren der Toucheingabe müssen Sie sich für Windows-Ereignisse registrieren, beispielsweise für das Aktivieren, Freigeben und Verschieben des Zeigers. Zum Initialisieren des Beschleunigungsmessers erstellen Sie ein [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687)-Objekt, wenn Sie die Anwendung initialisieren. Der Xbox-Controller muss nicht initialisiert werden.

-   Überlegen Sie bei Spielen für einen Spieler, ob Sie die Eingaben aller möglichen Xbox-Controller kombinieren möchten. Auf diese Weise müssen Sie nicht nachverfolgen, welche Eingabe von welchem Controller kommt. Oder verfolgen Sie einfach die Eingaben vom zuletzt hinzugefügten Controller nach, wie in diesem Beispiel dargestellt.

-   Verarbeiten Sie erst Windows-Ereignisse und dann Eingabegeräte.

-   Der Xbox-Controller und der Beschleunigungsmesser unterstützen Abrufe. Das heißt, Sie können Daten abrufen, wenn Sie sie benötigen. Für die Toucheingabe sollten Sie Touchereignisse in Datenstrukturen aufzeichnen, die für Ihren Eingabeverarbeitungscode verfügbar sind.

-   Überlegen Sie, ob Sie Eingabewerte in ein gemeinsames Format normalisieren möchten. Auf diese Weise können Sie die Interpretation der Eingabe durch andere Komponenten des Spiels wie beispielsweise die Simulation von Physikeffekten vereinfachen und leichter Spiele schreiben, die sich für unterschiedliche Bildschirmauflösungen eignen.

## <a name="input-devices-supported-by-marble-maze"></a>Von Marble Maze unterstützte Eingabegeräte


Marble Maze unterstützt Xbox-Controller, Maus und Toucheingabe zum Auswählen von Menüelementen und Xbox-Controller, Maus, Toucheingabe und Beschleunigungsmesser zum Steuern des Spielverlaufs. Marble Maze ruft die Eingaben vom Controller mithilfe der [Windows::Gaming::Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)-API ab. Bei der Toucheingabe können Anwendungen Eingaben mit der Fingerspitze nachverfolgen und darauf reagieren. Ein Beschleunigungsmesser ist ein Sensor, der die Kraft misst, die entlang der X-, Y- und Z-Achsen angewendet wird. Mit der Windows-Runtime können Sie den aktuellen Zustand des Beschleunigungsmessers abrufen und Touchereignisse über den Ereignisbehandlungsmechanismus der Windows-Runtime empfangen.

> [!NOTE]
> In diesem Dokument wird der Begriff Toucheingabe für Touch- und Mauseingabe und sowie Zeigereingabe verwendet. Damit sind alle Geräte gemeint, die Zeigerereignisse verwenden. Da bei der Touch- und Mauseingabe Standardzeigerereignisse verwendet werden, können Sie jedes der Geräte verwenden, um Menüelemente auszuwählen und den Spielverlauf zu steuern.

 

> [!NOTE]
> Das Paketmanifest legt das **Querformat** als unterstützte Drehung für das Spiel fest. Damit wird verhindert, dass sich die Ausrichtung ändert, wenn Sie das Gerät drehen, um die Murmel zu bewegen. Um das Paketmanifest zu sehen, öffnen Sie **Package.appxmanifest** unter **Solution Explorer** in Visual Studio.

 

## <a name="initializing-input-devices"></a>Initialisieren von Eingabegeräten


Der Xbox-Controller muss nicht initialisiert werden. Zum Initialisieren der Toucheingabe müssen Sie sich für Windows-Ereignisse registrieren, beispielsweise für das Aktivieren (z. B. wenn der Benutzer die Maustaste drückt oder den Bildschirm berührt), Freigeben und Verschieben des Zeigers. Zum Initialisieren des Beschleunigungsmessers müssen Sie ein [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687)-Objekt erstellen, wenn Sie die Anwendung initialisieren.

Im folgenden Beispiel wird dargestellt, wie die **App::SetWindow**-Methode für die Zeigerereignisse [Windows::UI::Core::CoreWindow::PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerPressed) und [Windows::UI::Core::CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerReleased) des [Windows::UI::Core::CoreWindow::PointerMoved](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerMoved)-Elements registriert wird. Diese Ereignisse werden bei der Initialisierung der App und vor der Spielschleife registriert.

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

Die **MarbleMazeMain**-Klasse erstellt außerdem ein **std::map**-Objekt für Touchereignisse. Der Schlüssel für dieses Zuordnungsobjekt ist ein Wert, der den Eingabezeiger eindeutig identifiziert. Jeder Schlüssel ist der Entfernung zwischen jedem Touchpunkt und der Mitte des Bildschirms zugeordnet. Diese Werte verwendet Marble Maze später, um die Neigung des Labyrinths zu berechnen.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

Die **MarbleMazeMain**-Klasse enthält ein [Beschleunigungsmesserobjekt](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer).

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

Das **Beschleunigungsmesserobjekt** wird wie im folgenden Beispiel in der **MarbleMazeMain**-Konstruktor initialisiert. Die [Windows::Devices::Sensors::Accelerometer::GetDefault](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault)-Methode gibt eine Instanz des Standardbeschleunigungsmessers zurück. Wenn kein Standardbeschleunigungsmesser vorhanden ist, bleibt der **Accelerometer::GetDefault**-Wert **nullptr**.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navigieren in den Menüs

Sie haben folgende Möglichkeiten, mit der Maus, mit Toucheingabe oder mit dem Xbox-Controller in den Menüs zu navigieren:

-   Verwenden Sie das Steuerkreuz, um das aktive Menüelement zu ändern.
-   Verwenden Sie Toucheingabe, die A-Taste oder die Menütaste, um ein Menüelement auszuwählen oder das aktuelle Menü zu schließen, beispielsweise die Highscoretabelle.
-   Verwenden Sie die Menütaste, um das Spiel anzuhalten oder fortzusetzen.
-   Klicken Sie mit der Maus auf ein Menüelement, um die jeweilige Aktion auszuwählen.

###  <a name="tracking-xbox-controller-input"></a>Nachverfolgen von Xbox-Controllereingaben

Um die derzeit verbundenen Gamepads nachzuverfolgen, definiert **MarbleMazeMain** eine Membervariable **M_myGamepads**, die eine Sammlung von [Windows::Gaming::Input::Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad)-Objekten ist. Dies wird im Konstruktor wie folgt initialisiert:

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

Darüber hinaus registriert der **MarbleMazeMain**-Konstruktor Ereignisse, wenn Gamepads hinzugefügt oder entfernt werden:

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

Wenn ein Gamepad hinzugefügt wird, wird dieser **M_myGamepads** hinzugefügt; wenn ein Gamepad entfernt wird, wird überprüft, ob das Gamepad **M_myGamepads** ist, und wenn dies der Fall ist, entfernen wir diesen. In beiden Fällen legen wir **M_currentGamepadNeedsRefresh** auf **true** fest, was angibt, das wir **M_gamepad** erneut zuweisen müssen.

Schließlich legen wir ein Gamepad, **M_gamepad** und **M_currentGamepadNeedsRefresh** auf **false** fest:

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

In der **Update** -Methode wird überprüft, ob **M_gamepad** neu zugewiesen werden muss:

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

Wenn **M_gamepad** neu zugeordnet werden muss, müssen wir ihm den zuletzt hinzugefügten Gamepads mit **GetLastGamepad** zuweisen, der wie folgt definiert wird:

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

Diese Methode gibt einfach den letzten Gamepad **M_myGamepads** zurück.

Sie können an ein Windows 10-Gerät bis zu vier Xbox-Controller anschließen. Um zu vermeiden, dass Sie herausfinden müssen, welcher Controller aktiv ist, verfolgen wir einfach nur Titel des zuletzt hinzugefügten Gamepads. Wenn Ihr Spiel mehrere Spieler unterstützt, müssen Sie die Eingabe für jeden Spieler getrennt nachverfolgen.

Die **MarbleMazeMain::Update**-Methode fragt das Gamepad für die Eingabe ab:

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

Wir behalten Sie den Überblick über die Eingabe durch den letzten Frame mit **m_oldReading** und die letzte Eingabe mit **m_newReading**, die wir durch Aufrufen von [Gamepad::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading) erhalten. Dies gibt ein [GamepadReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadreading)-Objekt zurück, das Informationen über den aktuellen Zustand des Gamepads enthält.

Um zu überprüfen, ob eine Schaltfläche einfach gedrückt oder losgelassen wurde, definieren wir **MarbleMazeMain::ButtonJustPressed** und **MarbleMazeMain::ButtonJustReleased**, was die Tasteneingabe aus diesem und dem letzten Frame vergleicht. So kann eine Aktion nur beim ersten Drücken einer Taste ausgeführt werden und nicht, wenn die Taste gedrückt gehalten oder losgelassen wird.

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

[GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)-Messwerte werden mit bitweisen Operationen&mdash;verglichen und es wird überprüft, ob eine Schaltfläche mit *bitweise und* (&) geklickt wird. Wir ermitteln, ob eine Schaltfläche einfach gedrückt oder losgelassen wurde, indem der alte Messwert und der neue Messwert verglichen werden.

Mithilfe der oben aufgeführten Methoden, überprüfen wir, ob bestimmte Schaltflächen gedrückt wurden und führen die entsprechenden Aktionen aus. Wenn beispielsweise die Menütaste (**GamepadButtons::Menu**) gedrückt wird, ändert sich der Spielzustand von aktiv in angehalten oder von angehalten in aktiv.

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

Es wird auch überprüft, ob der Spieler die Ansicht-Taste drückt. In diesem Fall starten wir das Spiel erneut oder löschen die hohe Highscore-Tabelle:

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

Bei der Touch- und Mauseingabe wird ein Menüelement ausgewählt, wenn der Benutzer das Element berührt oder darauf klickt. Das folgende Beispiel zeigt, wie die **MarbleMazeMain::Update**-Methode die Zeigereingabe verarbeitet, um Menüelemente auszuwählen. Die **m\_PointQueue** Membervariable verfolgt die Speicherorte, in denen der Benutzer verwendete oder geklickt haben, auf dem Bildschirm. Im Abschnitt [Verarbeiten von Zeigereingaben](#processing-pointer-input) wird ausführlicher beschrieben, wie Marble Maze die Zeigereingaben erfasst.

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

Wenn die **MarbleMazeMain::Update**-Methode die Controller- und Toucheingabe verarbeitet hat und eine Taste gedrückt wurde, aktualisiert sie den Spielzustand.

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


Die Spielschleife und die **MarbleMazeMain::Update**-Methode aktualisieren gemeinsam den Zustand der Spielobjekte. Wenn Ihr Spiel Eingaben von mehreren Geräten akzeptiert, können Sie die Eingaben aller Geräte in einem Satz von Variablen sammeln. Auf diese Weise können Sie Code schreiben, der sich leichter verwalten lässt. Die **MarbleMazeMain::Update**-Methode definiert einen Variablensatz, mit dem Bewegungen von allen Geräten gesammelt werden.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

Die Eingabemechanismen der einzelnen Eingabegeräte können dabei unterschiedlich sein. Beispielsweise werden Zeigereingaben mit dem Ereignisbehandlungsmodell der Windows-Runtime behandelt. Die Eingabedaten des Xbox-Controllers dagegen rufen Sie bei Bedarf ab. Am besten halten Sie sich immer an die Eingabemechanismen, die für ein bestimmtes Gerät vorgeschrieben sind. In diesem Abschnitt wird beschrieben, wie Marble Maze die Eingaben von den einzelnen Geräten liest, die kombinierten Eingabewerte aktualisiert und sie verwendet, um den Spielzustand zu aktualisieren.

###  <a name="processing-pointer-input"></a>Verarbeiten von Zeigereingaben

Wenn Sie mit Zeigereingaben arbeiten, rufen Sie die [Windows::UI::Core::CoreDispatcher::ProcessEvents](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents)-Methode auf, um Windows-Ereignisse zu verarbeiten. Rufen Sie diese Methode in Ihrer Spielschleife auf, bevor Sie die Szene aktualisieren oder rendern. Marble Maze ruft dies in der **Implementierungscode**-Methode auf: 

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

Wenn das Fenster sichtbar ist, übergeben wir **CoreProcessEventsOption::ProcessAllIfPresent** an **ProcessEvents**, um alle Ereignisse in der Warteschlange sofort zu verarbeiten. Andernfalls übergeben wir **CoreProcessEventsOption::ProcessOneAndAllPending**, um alle Ereignisse in der Warteschlange zu verarbeiten und warten auf das das nächste neue Ereignis. Nach der Verarbeitung der Ereignisse rendert Marble Maze den nächsten Frame und zeigt ihn an.

Die Windows-Runtime ruft den registrierten Handler für jedes aufgetretene Ereignis auf. Die **App::SetWindow**-Methode registriert sich für Ereignisse und leitet Zeigerinformationen an die **MarbleMazeMain**-Klasse weiter.

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

Die **MarbleMazeMain**-Klasse reagiert auf Zeigerereignisse, indem sie das Zuordnungsobjekt aktualisiert, das Touchereignisse enthält. Die **MarbleMazeMain::AddTouch**-Methode wird aufgerufen, wenn der Zeiger zum ersten Mal gedrückt wird. Dies ist beispielsweise der Fall, wenn der Benutzer zum ersten Mal den Bildschirm eines touchfähigen Geräts berührt. Die **MarbleMazeMain::UpdateTouch**-Methode wird aufgerufen, wenn die Zeigerposition verschoben wird. Die **MarbleMazeMain::RemoveTouch**-Methode wird aufgerufen, wenn der Zeiger losgelassen wird. Dies ist beispielsweise der Fall, wenn der Benutzer den Bildschirm nicht mehr berührt.

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

Die **PointToTouch**-Funktion verschiebt die aktuelle Zeigerposition, sodass sich der Ursprung in der Mitte des Bildschirms befindet. Dann skaliert sie die Koordinaten so, dass sie ungefähr zwischen -1,0 und +1,0 liegen. Dadurch kann die einheitliche Neigung des Labyrinths für verschiedene Eingabemethoden leichter berechnet werden.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

Die **MarbleMazeMain::Update**-Methode aktualisiert die kombinierten Eingabewerte, indem sie den Neigungsfaktor um einen konstanten Skalierungswert erhöht. Dieser Skalierungswert wurde durch Experimentieren mit verschiedenen Werten ermittelt.

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

Zur Verarbeitung von Beschleunigungsmessereingaben ruft die **MarbleMazeMain::Update**-Methode die [Windows::Devices::Sensors::Accelerometer::GetCurrentReading](https://msdn.microsoft.com/library/windows/apps/br225699)-Methode auf. Diese Methode gibt ein [Windows::Devices::Sensors::AccelerometerReading](https://msdn.microsoft.com/library/windows/apps/br225688)-Objekt zurück, das die Ablesung eines Beschleunigungsmessers darstellt. Die Eigenschaften **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** und **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** enthalten die Schwerkraftbeschleunigung entlang der X-Achse bzw. der Y-Achse.

Das folgende Beispiel zeigt, wie die **MarbleMazeMain::Update**-Methode den Beschleunigungsmesser abruft und die kombinierten Eingabewerte aktualisiert. Wenn Sie das Gerät neigen, bewegt sich die Murmel aufgrund der Schwerkraft schneller.

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

Da Sie nicht sicher sein können, dass der Computer des Benutzers über einen [Beschleunigungsmesser](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) verfügt, müssen Sie immer sicherstellen, dass Sie ein gültiges Beschleunigungsmesserobjekt haben, und erst dann die Werte vom Beschleunigungsmesser abrufen.

### <a name="processing-xbox-controller-input"></a>Verarbeiten von Xbox-Controllereingaben

In der **MarbleMazeMain::Update**-Methode verwenden wir **m_newReading**-Eingaben vom linken Analogstick:

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

Wir überprüfen, ob die Eingabe vom linken Analogstick außerhalb des inaktiven Bereichs liegt, und wenn dies der Fall ist, fügen wir **combinedTiltX** und **combinedTiltY** hinzu (multipliziert mit dem Skalierungsfaktor), um die Phase zu verändern.

> [!IMPORTANT]
> Berücksichtigen Sie immer den inaktiven Bereich, wenn Sie mit dem Xbox-Controller arbeiten. Der inaktive Bereich bezieht sich auf die unterschiedliche Empfindlichkeit von Gamepads für die ersten Bewegungen. Bei manchen Controllern ergibt eine kleine Bewegung keinen Messwert, während sie bei anderen zu einem messbaren Ergebnis führt. Berücksichtigen Sie dies in Ihrem Spiel, indem Sie für die ersten Bewegungen des Ministicks einen Bereich für Nichtbewegungen erstellen. Weitere Informationen finden über den inaktiven Bereich finden Sie unter [Lesen der Ministicks](gamepad-and-vibration.md#reading-the-thumbsticks).

 

###  <a name="applying-input-to-the-game-state"></a>Anwenden von Eingaben auf den Spielzustand

Geräte melden Eingabewerte auf unterschiedliche Weise. So wird eine Zeigereingabe möglicherweise in Bildschirmkoordinaten angegeben, eine Controllereingabe aber in einem völlig anderen Format. Beim Kombinieren der Eingaben von mehreren Geräten in einem Satz von Eingabewerten besteht eine Herausforderung darin, die Werte in ein gemeinsames Format zu normalisieren oder konvertieren. Marble Maze normalisiert die Werte durch die sie auf den Bereich Skalierung \[Bereich von -1,0, 1,0\]. Mit der weiter oben in diesem Abschnitt beschriebenen **PointToTouch**-Funktion werden Bildschirmkoordinaten in normalisierte Werte konvertiert. Diese Werte liegen ungefähr zwischen -1,0 und +1,0.

> [!TIP]
> Auch wenn Ihre Anwendung eine einzige Eingabemethode verwendet, sollten Sie die Eingabewerte immer normalisieren. Auf diese Weise können Sie die Interpretation der Eingabe durch andere Komponenten des Spiels wie beispielsweise die Simulation von Physikeffekten vereinfachen und leichter Spiele schreiben, die sich für unterschiedliche Bildschirmauflösungen eignen.

 

Nachdem die **MarbleMazeMain::Update**-Methode die Eingabe verarbeitet hat, erstellt sie einen Vektor, der die Auswirkung der Neigung des Labyrinths auf die Murmel darstellt. Das folgende Beispiel zeigt, wie Marble Maze mit der [XMVector3Normalize](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.geometric.xmvector3normalize)-Funktion einen normalisierten Schwerkraftvektor erstellt. Die **maxTilt**-Variable schränkt die Neigung des Labyrinths ein und verhindert, dass das Labyrinth auf die Seite gekippt wird.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

Als letzten Schritt beim Aktualisieren der Szenenobjekte übergibt Marble Maze den aktualisierten Schwerkraftvektor der Simulation für Physikeffekte, aktualisiert diese Simulation für den seit dem vorherigen Frame verstrichenen Zeitraum und aktualisiert die Position und Ausrichtung der Murmel. Wenn die Murmel durch das Labyrinth gefallen ist, platziert die **MarbleMazeMain::Update**-Methode die Murmel wieder am letzten Prüfpunkt, den die Murmel berührt hat, und setzt den Zustand der Simulation für Physikeffekte zurück.

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

In diesem Abschnitt wird nicht beschrieben, wie die Simulation für Physikeffekte funktioniert. Details hierzu finden Sie in den Quellen für Marble Maze in **Physics.h** und **Physics.cpp**.

## <a name="next-steps"></a>Nächste Schritte


Lesen Sie [Hinzufügen von Audio zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md). Dort finden Sie Informationen zu einigen der wichtigen Methoden, die Sie beim Arbeiten mit Audio berücksichtigen sollten. In diesem Dokument wird erörtert, wie Marble Maze mithilfe von Microsoft Media Foundation und XAudio2 Audioressourcen lädt, mischt und wiedergibt.

## <a name="related-topics"></a>Verwandte Themen


* [Hinzufügen von Audio zum Marble Maze-Beispiel](adding-audio-to-the-marble-maze-sample.md)
* [Hinzufügen von visuellem Inhalt zum Marble Maze-Beispiel](adding-visual-content-to-the-marble-maze-sample.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




