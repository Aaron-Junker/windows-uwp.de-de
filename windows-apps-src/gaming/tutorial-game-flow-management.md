---
title: Spielablaufverwaltung
description: Erfahren Sie, wie Sie Spielzustände initialisieren, Ereignisse behandeln und die Spielaktualisierungsschleife einrichten.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, directx
ms.localizationpriority: medium
ms.openlocfilehash: 4e4d8f43893b5f2a9a58c2eb6209ecb7d8dd1c21
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367580"
---
# <a name="game-flow-management"></a>Spielablaufverwaltung

Das Spiel hat nun ein Fenster, registriert einige Ereignishandler und lädt Assets asynchron. In diesem Abschnitt werden die Verwendung von Spielständen, die Verwaltung bestimmter wichtiger Spielzustände und die Erstellung einer Aktualisierungsschleife für die Spiel-Engine erläutert. Dann wird der Ablauf in der Benutzeroberfläche erläutert, und schließlich werden die Event-Handler und Events vorgestellt, die für ein UWP-Spiel erforderlich sind.

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="game-states-used-to-manage-game-flow"></a>Spielzustände zur Verwaltung des Spielablaufs

Wir verwalten den Spielablauf mithilfe von Spielzuständen. Wenn ein Benutzer eine UWP-Spiel-App aus einem angehaltenen Zustand fortsetzt, kann das Spiel eine beliebigen der möglichen Zustände besitzen.

In diesem Beispiel kann sich das Spiel beim Start in einem von drei Zuständen befinden:
* Die Spielschleife wird mitten in einem Level ausgeführt.
* Die Spielschleife wird nicht ausgeführt, da gerade ein Spiel abgeschlossen wurde. (Der Highscore ist festgelegt)
* Es wurde kein Spiel gestartet, oder das Spiel befindet sich zwischen zwei Leveln. (Der Highscore ist 0)

Sie können die Anzahl der erforderlichen Zustände je nach den Bedürfnissen Ihres Spiels festlegen. Berücksichtigen Sie dabei (wie gesagt), dass Ihr UWP-Spiel jederzeit beendet werden kann. Beim Fortsetzen des Spiels erwartet der Spieler, dass sich das Spiel so verhält, als sei es nie unterbrochen worden.

## <a name="game-states-initialization"></a>Initialisieren der Spielzustände

Bei der Spielinitialisierung müssen Sie sich nicht nur den erstmaligen Start des Spiel berücksichtigen, sondern auch einen Neustart, nachdem es angehalten oder beendet wurde. Das Beispielspiel speichert stets den Zustand, sodass der Eindruck entsteht, als würde es ohne Unterbrechung ausgeführt. 

Im angehaltenen Zustand ist der Spielablauf unterbrochen, aber die Ressourcen des Spiels befinden sich aber nach wie vor im Speicher. 

Dementsprechend stellt das Fortsetzungsereignis sicher, dass das Beispielspiel an der Stelle fortgesetzt wird, an der es zuletzt angehalten oder beendet wurde. Wird das Beispielspiel neu gestartet, nachdem es beendet wurde, startet das Spiel ganz normal und ermittelt dann den letzten bekannten Zustand, sodass der Spieler sofort weiterspielen kann.

Je nach Zustand hat der Spieler unterschiedliche Möglichkeiten. 

* Wird das Spiel mitten in einem Level fortgesetzt, entsteht der Eindruck, es sei  pausiert, und auf dem Overlay steht eine Fortsetzungsoption zur Verfügung.
* Wenn das Spiel in einem Zustand fortgesetzt wird, in dem es abgeschlossen ist, zeigt es die Highscores und eine Option zum Starten eines neuen Spiels an.
* Wird das Spiel dagegen vor dem Start eines Levels fortgesetzt, sieht der Spieler eine Startoption auf dem Overlay.

Das Spielbeispiel unterscheidet nicht zwischen einem Kaltstart des Spiels (also einem Spiel, das zum ersten Mal und ohne Anhalteereignis gestartet wird) und der Fortsetzung eines angehaltenen Spiels. Dieses Design wird bei jeder UWP-App erwartet.

In diesem Beispiel erfolgt die Initialisierung der Spielzustände in [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).

Im folgenden Flussdiagramm können Sie den Ablauf nachverfolgen. Dargestellt sind sowohl Initialisierung- als auch Aktualisierungsschleife.

* Die Initialisierung beginnt am Knoten __Start__ bei der Überprüfung des aktuellen Spielzustands. Den Spielcode finden Sie unter [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).
* Weitere Informationen über die Aktualisierungsschleife finden Sie unter [Aktualisieren der Spiel-Engine](#update-game-engine). Den Code des Spiels finden Sie unter [__App:: Update__](#appupdate-method).

![Hauptzustandsautomat für unser Spiel](images/simple-dx-game-flow-statemachine.png)

### <a name="gamemaininitializegamestate-method"></a>GameMain::InitializeGameState method

Die Methode __InitializeGameState__ wird von der Konstruktorklasse [__GameMain__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L32-L131) aufgerufen, die aufgerufen wird, wenn das Klassenobjekt __GameMain__ in der Methode [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123) erstellt wird.

```cpp

GameMain::GameMain(...)
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...

    create_task([this]()
    {
        ...

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState(); //Initialization of game states occurs here.
        
        ...
    
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
        
    }, task_continuation_context::use_current());
}

```

```cpp

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle of a level.
        // We are waiting for the user to continue the game.
        //...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        //...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        m_uiControl->SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    m_uiControl->ShowGameInfoOverlay();
}

```

## <a name="update-game-engine"></a>Aktualisieren der Spiele-Engine

In der Methode [__App::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L127-L130) ruft sie [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202) auf. In dieser Methode wird innerhalb der Spielschleife des Beispiels ein einfacher Zustandsautomat zur Behandlung aller wichtigen Aktionen implementiert, die der Spieler ausführen kann. Die höchste Ebene dieses Zustandsautomats behandelt das Laden eines Spiels, das Spielen eines bestimmten Levels oder das Fortsetzen eines Levels, nachdem das Spiel (vom System oder vom Spieler) angehalten wurde.

Im Spielbeispiel kann sich das Spiel in drei Hauptzuständen (__UpdateEngineState__) befinden:

1. __Warten auf Ressourcen__: Die Spielschleife wird durchlaufen, und ein Übergang ist erst möglich, wenn Ressourcen (insbesondere Grafikressourcen) verfügbar sind. Nach Abschluss der asynchronen Aufgaben zum Laden der Ressourcen wird der Zustand zu __ResourcesLoaded__ aktualisiert. Dieser Fall tritt üblicherweise zwischen Leveln ein, wenn ein Level neue Ressourcen von einem Datenträger, einem Spieleserver oder Cloudbackend lädt. Im Spielbeispiel simulieren wir dieses Verhalten, da an dieser Stelle keine zusätzlichen levelspezifischen Ressourcen für das Spiel geladen werden müssen.
2. __Drücken Sie warten__: Die Spielschleife wird durchlaufen, bis eine bestimmte Benutzereingabe erfolgt. Bei der Eingabe handelt es sich um eine Spieleraktion zum Laden eines Spiels, zum Starten eines Levels oder zum Fortsetzen eines Levels. Der Beispielcode bezieht sich auf diese Unterzustände als __PressResultState__-Aufzählungswerte.
3. In __Dynamics__: Die Spielschleife wird ausgeführt, und der Spieler spielt. Während der Spieler spielt, sucht das Spiel nach drei Bedingungen, auf die es reagieren kann: 
    * __TimeExpired__: Ablauf des Zeitlimits für einen Level
    * __LevelComplete__: Abschluss eines Levels durch den Spieler 
    * __GameComplete__: Abschluss aller Level durch den Spieler

Ihr Spiel ist einfach ein Zustandsautomat, der mehrere kleinere Zustandsautomaten enthält. Für jeden spezifischen Zustand muss es durch sehr spezifische Kriterien definiert werden. Wie es von einem Zustand in einen anderen übergeht, muss auf diskreten Benutzereingaben oder Systemaktionen beruhen (wie dem Laden von Grafikressourcen) basieren. Bedenken Sie bei der Planung Ihres Spiels den gesamten Spielablauf, um sicherzustellen, dass Sie alle möglichen Aktionen berücksichtigt haben, die Benutzer oder System ausführen können. Spiele können sehr kompliziert sein, und der Zustandsautomat ist ein wirksames Tool, um diese Komplexität darzustellen und den Umgang damit deutlich zu vereinfachen.

Betrachten wir nun den Code für die Aktualisierungsschleife unten.

### <a name="appupdate-method"></a>App::Update method

Die Struktur des Zustandsautomaten zur Aktualisierung der Spielengine

```cpp
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        //...
        break;

    case UpdateEngineState::ResourcesLoaded:
        //...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            //...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when the player is playing, the work is handled by this Simple3DGame::RunGame method.
            switch (runState)
            {
            case GameState::TimeExpired:
                //...
                break;

            case GameState::LevelComplete:
                //...
                break;

            case GameState::GameComplete:
                //...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-user-interface"></a>Aktualisierung der Benutzeroberfläche

Wir müssen den Spieler über den Status des Systems auf dem Laufenden halten und den Spielstatus ändern, abhängig von den Aktionen des Spielers und den Regeln, die das Spiel definieren. Viele Spiele, einschließlich dieses Beispiels, verwenden Elemente der Benutzeroberfläche (UI), um dem Spieler diese Informationen mitzuteilen. Die Benutzeroberfläche enthält Darstellungen des Spielzustands und andere spielspezifische Informationen wie Punktestand, Munitionsbestand oder die Anzahl verbleibender Chancen. Die UI wird auch als Overlay bezeichnet, da sie separat von der Hauptgrafikpipeline gerendert und über der 3D-Projektion dargestellt wird. Einige UI-Informationen werden auch als Heads-Up-Display (HUD) präsentiert, damit Nutzer diese Informationen verfolgen kann, ohne den Hauptspielbereich aus den Augen zu verlieren. Im Beispielspiel erstellen wir dieses Overlay mithilfe der Direct2D-APIs. Wir können das Overlay allerdings auch per XAML erstellen. Informationen hierzu finden Sie unter [Erweitern des Spielbeispiels](tutorial-resources.md).

Die Benutzeroberfläche umfasst zwei Komponenten:

-   Das HUD mit dem Punktestand und Infos zum aktuellen Zustand des Spielverlaufs.
-   Die Bitmap für den Pausenmodus – ein schwarzes Rechteck mit Text, das im angehaltenen/unterbrochenen Zustand des Spiels im Vordergrund angezeigt wird. Dies ist die Spielüberlagerung. Ausführlichere Informationen hierzu finden Sie unter [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md).

Wie nicht anders zu erwarten, besitzt auch das Overlay einen Zustandsautomaten. Das Overlay kann eine Meldung für den Levelstart oder eine Meldung mit dem Hinweis anzeigen, dass das Spiel vorbei ist. Es ist im Grunde ein Canvas-Element, mit dem Sie beliebige Infos zum Spielzustand  dem Spieler anzeigen können, wenn das Spiel unterbrochen oder angehalten wurde.

Das gerenderte Overlay kann einer der sechs Bildschirme sein, entsprechend einem der Spielzustände: 
1. Bildschirm zu Beginn des Spiels, wenn Ressourcen geladen werden
2. Bildschirm mit Spielstatistik
3. Bildschirm mit Nachricht über Levelstart
4. Bildschirm am Spielende, wenn alle Ebenen ohne Timeout abgeschlossen wurden
5. Bildschirm am Spielende, wenn ein Timeout aufgetreten ist
6. Bildschirm mit Unterbrechungsmenü

Dank der getrennten Behandlung von Benutzeroberfläche und Spielgrafikpipeline können Sie Arbeiten an der Benutzeroberfläche unabhängig von der Grafikengine des Spiels vornehmen. Außerdem wird der Code Ihres Spiels dadurch deutlich übersichtlicher.

Hier sehen Sie die Struktur des Overlay-Zustandsautomaten für das Spielbeispiel:

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        //...
        break;

    case GameInfoOverlayState::LevelStart:
        //...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        //...
        break;

    case GameInfoOverlayState::GameOverExpired:
        //...
        break;

    case GameInfoOverlayState::Pause:
        //...
        break;
    }
}
```

## <a name="events-handling"></a>Behandeln von Ereignissen

Unser Beispielcode hat eine Reihe von Handlern für bestimmte Ereignisse in **Initialize**, **SetWindow** und **Load** in App.ccp registriert. Dies sind wichtige Ereignisse, die funktionieren müssen, bevor wir Spielmechaniken hinzufügen oder die Grafikentwicklung starten können. Diese Ereignisse sind für eine ordnungsgemäße UWP-App-Erfahrung von grundlegender Bedeutung. Da eine UWP-App jederzeit aktiviert, deaktiviert, vergrößert, verkleinert, angedockt, abgedockt oder fortgesetzt werden kann, müssen diese Ereignisse so früh wie möglich registriert und so behandelt werden, dass der Spieler keine Unterbrechungen oder Überraschungen erlebt.

Dies sind die in diesem Beispiel verwendeten Ereignishandler sowie die jeweils behandelten Ereignisse:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ereignishandler</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView::Activated</strong></a>. Die Spiele-App befindet sich im Vordergrund, weshalb das Hauptfenster aktiviert ist.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>. Der DPI-Wert der Anzeige hat sich geändert, und das Spiel passt seine Ressourcen entsprechend an.
<div class="alert">
<strong>Beachten Sie</strong> <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>"corewindow"</strong> </a> Koordinaten sind als die DIPs (geräteunabhängigen Pixeln), für die <a href="https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>. Daher müssen Sie Direct2D über die DPI-Änderung informieren, damit die 2D-Ressourcen oder -Grundtypen korrekt angezeigt werden.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>. Die Ausrichtung der Anzeige ändert sich, und das Rendern muss aktualisiert werden.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>. Die Anzeige muss neu aufgebaut und das Spiel erneut gerendert werden.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication::Resuming</strong></a>. Die Spiele-App stellt das Spiel aus einem Anhaltezustand wieder her.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication::Suspending</strong></a>. Die Spiele-App speichert den eigenen Zustand auf einem Datenträger. Der Speichervorgang für den Zustand darf maximal fünf Sekunden dauern.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow::VisibilityChanged</strong></a>. Die Sichtbarkeit der Spiele-App hat sich geändert: Die App wurde entweder sichtbar, oder sie wurde durch eine andere sichtbar gewordene App unsichtbar.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow::Activated</strong></a>. Das Hauptfenster der Spiele-App wurde deaktiviert oder aktiviert, weshalb der Fokus entfernt und das Spiel angehalten oder der Fokus wiedererlangt werden muss. In beiden Fällen gibt das Overlay an, dass das Spiel angehalten wurde.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow:: Closed</strong></a>. Die Spiele-App schließt das Hauptfenster und hält das Spiel an.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Behandelt <a href="https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow::SizeChanged</strong></a>. Die Spiele-App ordnet die Grafikressourcen und das Overlay neu zu, um die Größenänderung umzusetzen, und aktualisiert anschließend das Renderziel.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema haben wir erläutert, wie der allgemeine Spielablauf mithilfe von Spielzuständen verwaltet wird, und dass ein Spiel aus mehreren verschiedenen Zustandsautomaten besteht. Wir haben zudem gezeigt, wie Sie die UI aktualisieren und wichtige App-Ereignishandler verwalten können. Nun können wir uns mit der Renderingschleife, dem Spiel und seinen Mechanismen befassen.
 
Sie können die anderen Komponenten, aus denen dieses Spiel besteht, in beliebiger Reihenfolge durcharbeiten:
* [Das Spiele Hauptobjekt definieren](tutorial--defining-the-main-game-loop.md)
* [Rendering-Framework I: Einführung zu rendern](tutorial--assembling-the-rendering-pipeline.md)
* [Rendering-Framework II: Rendern von Spielen](tutorial-game-rendering.md)
* [Fügen Sie eine Benutzeroberfläche hinzu](tutorial--adding-a-user-interface.md)
* [Hinzufügen von Steuerelementen](tutorial--adding-controls.md)
* [Hinzufügen von sound](tutorial--adding-sound.md)