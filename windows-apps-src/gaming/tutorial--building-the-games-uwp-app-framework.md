---
title: Definieren des UWP-App-Frameworks für das Spiel
description: Wenn Sie Code für ein UWP-Spiel mit DirectX erstellen, müssen Sie zunächst das Framework erstellen, das die Interaktion der Spielobjekte mit Windows ermöglicht.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: af5d73e0a786e33aff6274cd63ee5ae6ac77c133
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853002"
---
#  <a name="define-the-uwp-app-framework"></a>Definieren des UWP-App-Frameworks

Erstellen Sie ein Framework, um Ihre Spielobjekte mit Windows interagieren zu lassen, einschließlich der Windows-Runtime Eigenschaften, die Behandlung von Anhalte-/Fortsetzungsereignissen, Änderungen des Fensterfokus und Andocken.

Sie müssen zuerst einen Ansichtsanbieter abrufen, mit dem das App-Singleton (also das Windows-Runtime-Objekt, das eine Instanz Ihrer ausgeführten App definiert) auf die benötigten Grafikressourcen zugreifen kann. Über die Windows-Runtime ist Ihr Spiel zwar direkt mit der Grafikschnittstelle verbunden, Sie müssen aber angeben, welche Ressourcen Sie benötigen und wie diese behandelt werden sollen.

Das Ansichtsanbieterobjekt implementiert die __IFrameworkView__-Schnittstelle, die aus einer Reihe von Methoden besteht, die zum Erstellen dieses Spielbeispiels konfiguriert werden müssen.

Sie müssen diese fünf Methoden implementieren, die vom App-Singleton aufgerufen werden:
* [__Initialisieren__](#initialize-the-view-provider)
* [__SetWindow__](#configure-the-window-and-display-behaviors)
* [__Laden__](#load-method-of-the-view-provider)
* [__Lauf__](#run-method-of-the-view-provider)
* [__Uninitialize__](#uninitialize-method-of-the-view-provider)

Die __Initialize__-Methode wird beim Starten der App aufgerufen. Die __SetWindow__-Methode wird nach __Initialize__ aufgerufen. Es wird dann __Load__ aufgerufen. Wenn das Spiel fortgesetzt wird, wird die __Run__-Methode aufgerufen. Wenn das Spiel beendet wird, wird die __Uninitialize__-Methode aufgerufen. Weitere Informationen finden Sie unter [__IFrameworkView__ API-Referenz](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview). 

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Ziel

Richten Sie das Framework für ein UWP-DirectX-Spiel ein und implementieren den Zustandsautomaten, der den allgemeinen Spielablauf definiert.

## <a name="define-the-view-provider-factory-and-view-provider-object"></a>Die Ansichtsanbieterfactory und das Ansichtsanbieterobjekt werden definiert

Sehen wir uns die __wichtigsten__ Schleifen in __App.cpp__ an. 

In diesem Schritt erstellen wir eine Factory für die Ansicht (implementiert __IFrameworkViewSource__), wodurch wiederum Instanzen des Ansichtsanbieterobjekts erstellt werden (implementiert __IFrameworkView__), das die Ansicht definiert.

### <a name="main-method"></a>Main-Methode

Erstellen Sie ein neues __DirectXApplicationSource__, wenn Sie den GitHub-Beispielcode geladen haben. (Verwenden Sie __Direct3DApplicationSource__ bei Verwendung der ursprünglichen DirectX-Vorlage). Hierbei handelt es sich um die Ansichtsanbieterfactory, die __IFrameworkViewSource__ implementiert. Die Ansichtsanbieterfactory-Schnittstelle __IFrameworkViewSource__ verfügt über eine einzelne Methode __CreateView__, die definiert wird.

In __CoreApplication:: Run__ wird die __CreateView__-Methode aufgerufen, indem Sie das App-Singleton an __Direct3DApplicationSource__ oder __DirectXApplicationSource__ übergeben.

__CreateView__ gibt einen Verweis auf eine neue Instanz des App-Objekts an, das __IFrameworkView__ implementiert, was das __App__-Klassenobjekt in diesem Beispiel ist. Das __App__-Klassenobjekt ist das Ansichtsanbieterobjekt.

```cpp
// The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto directXApplicationSource = ref new DirectXApplicationSource();
    CoreApplication::Run(directXApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXApplicationSource::CreateView()
{
    return ref new App();
}
```

## <a name="initialize-the-view-provider"></a>Initialisieren des Ansichtsanbieter

Nachdem das Ansichtsanbieterobjekt erstellt wurde, ruft das App-Singleton die [**Initialize**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)-Methode beim Starten der Anwendung auf. Daher müssen die elementaren Verhaltensweisen eines UWP-Spiels unbedingt von dieser Methode behandelt werden. Hierzu zählt beispielsweise die Behandlung der Aktivierung des Hauptfensters. Außerdem muss von der Methode sichergestellt werden, dass das Spiel eine überraschende Unterbrechung (sowie eine mögliche spätere Fortsetzung) behandeln kann.

Dann kann die Spiele-App eine angehaltene Nachricht behandeln (oder fortsetzen). Aber es gibt immer noch kein Fenster, und das Spiel ist noch nicht initialisiert. Wir haben also noch ein wenig Arbeit vor uns.

### <a name="appinitialize-method"></a>App::Initialize-Methode

Bei dieser Methode können Sie verschiedene Ereignishandler für das Aktivieren, Anhalten und Fortsetzen des Spiels erstellen.

Ressourcenzugriff des Geräts zulassen Die __make_shared__-Funktion dient zum Erstellen von __Shared_ptr__, wenn die Speicherressource zum ersten Mal erstellt wird. Ein Vorteil der Verwendung von __Make_shared__ ist, dass sie ausnahmesicher ist. Sie verwendet auch den gleichen Aufruf, um den Arbeitsspeicher für Kontrollblock und die Ressource zuzuweisen und reduziert daher den Aufwand für die Erstellung.

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    // Register event handlers for app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="configure-the-window-and-display-behaviors"></a>Konfigurieren Sie das Fenster und Anzeigeverhalten

Nun sehen wir uns die Implementierung von [__SetWindow__](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) an. In der __SetWindow__-Methode konfigurieren Sie das Fenster und Anzeigeverhalten.

### <a name="appsetwindow-method"></a>App::SetWindow-Methode

Das App-Singleton stellt ein [__CoreWindow__](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)-Objekt bereit, das das Hauptfenster des Spiels darstellt, und macht seine Ressourcen und Ereignisse für das Spiel verfügbar. Jetzt kann das Spiel die grundlegenden Benutzeroberflächenkomponenten und Ereignisse hinzufügen.

Erstellen Sie dann einen Zeiger mit der __CoreCursor__-Methode, die von den Bedienelemente und von der Maus- und auch von der Fingereingabesteuerung genutzt werden kann.

Erstellen Sie zuletzt grundlegende Ereignisse für die Fenster, Größe, schließen und DPI-Änderungen (wenn sich das Anzeigegerät ändert). Weitere Informationen zu den Ereignissen finden Sie unter [Ereignisbehandlung](tutorial-game-flow-management.md#events-handling).

```cpp
void App::SetWindow(
    CoreWindow^ window
    )
{
    // Codes added to modify the original DirectX template project
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;
    // --end of codes added
    
    m_deviceResources->SetWindow(window);

    window->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);
        
    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    DisplayInformation^ currentDisplayInformation = DisplayInformation::GetForCurrentView();

    currentDisplayInformation->DpiChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDpiChanged);

    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);
    
    // Codes added to modify the original DirectX template project
    currentDisplayInformation->StereoEnabledChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnStereoEnabledChanged);
    // --end of codes added
    
    DisplayInformation::DisplayContentsInvalidated +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDisplayContentsInvalidated);
}
```

## <a name="load-method-of-the-view-provider"></a>Die Load-Methode des Ansichtsanbieters

Nachdem das Hauptfenster festgelegt ist, ruft das App-Singleton die [__Load__](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load)-Methode auf. Diese Methode verwendet eine Reihe asynchroner Aufgaben, um die Spielobjekte zu erstellen, Grafikressourcen zu laden und den Zustandsautomaten des Spiels zu initialisieren. Diese Methode eignet sich auch besser zum Vorabrufen von Spieldaten oder Ressourcen als **SetWindow** oder **Initialize**. 

Da Windows zeitliche Beschränkungen auferlegen kann, bevor Ihr Spiel Eingaben verarbeitet, können Sie mithilfe des asynchronen Aufgabenmusters die __Load__-Methode schnell abschließen, damit die Verarbeitung der Eingabe beginnen kann. Falls das Laden einige Zeit dauert (etwa aufgrund einer Vielzahl von Ressourcen), sollten Sie für Benutzer eine regelmäßig aktualisierte Statusleiste bereitstellen. In der Methode treffen wir alle nötigen Vorbereitungen für den Spielstart (wie Festlegen aller Startzustände oder globalen Werte).

Wenn Sie mit den Konzepten der asynchronen Programmierung und Task-Parallelität nicht vertraut sind, wechseln Sie zu [asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

### <a name="appload-method"></a>App::Load-Methode

Erstellen Sie die __GameMain__-Klasse, die Ladeaufgaben enthält.

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
        if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
````

### <a name="gamemain-constructor"></a>GameMain-Konstruktor

* Erstellen und initialisieren Sie den Spielrenderer. Weitere Informationen finden Sie unter [Rendering-Framework I: Einführung in das Rendering](tutorial--assembling-the-rendering-pipeline.md).
* Erstellen Sie die Initialisierung des Simple3Dgame-Objekts. Weitere Informationen finden Sie unter [Definieren des Hauptobjekts für das Spiel](tutorial--defining-the-main-game-loop.md).    
* Erstellen Sie das UI-Steuerelement und die Überlagerung von Spieledaten zum Anzeigen einer Statusleiste, während die Ressourcendateien geladen werden. Weitere Informationen finden Sie unter [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md).
* Erstellen Sie den Controller, damit er Eingaben aus dem Controller (Touchscreen, Maus oder Xbox Wireless Controller) lesen kann. Weitere Informationen finden Sie unter [Hinzufügen von Steuerelementen](tutorial--adding-controls.md).
* Nach dem Initialisieren des Controllers definieren wir zwei rechteckige Bereiche in den Ecken unten links und unten rechts auf dem Bildschirm für die Bewegungs- bzw. Kamerasteuerungen. Der Spieler verwendet das durch den Aufruf von **SetMoveRect** definierte Rechteck links unten als virtuelles Bedienfeld, um die Kamera vor und zurück sowie seitlich zu bewegen. Das durch die **SetFireRect**-Methode definierte Rechteck rechts unten wird als virtuelle Taste zum Abfeuern der Munition verwendet.
* Verwenden Sie __create_task__ und __create_task::then__, um das Laden von Ressourcen in zwei getrennte Phasen zu unterteilen. Da der Zugriff auf den Direct3D 11-Gerätekontext auf den Thread beschränkt ist, auf dem der Gerätekontext beim Zugriff auf das Direct3D 11-Gerät erstellt wurde und die Erstellung des Objekts den Threadbeschränkungen unterliegt, bedeutet dies, dass die **CreateGameDeviceResourcesAsync**-Aufgabe in einem separaten Thread ausgeführt werden kann, von der Aufgabe zur Vervollständigung (*FinalizeCreateGameDeviceResources*), die im Originalthread ausgeführt wird. Wir verwenden ein ähnliches Muster zum Laden von Levelressourcen mit **LoadLevelAsync** und **FinalizeLoadLevel**.

```cpp
GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = ref new GameRenderer(m_deviceResources);
    m_game = ref new Simple3DGame();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = ref new MoveLookController(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameConstants::TouchRectangleSize, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game so spin up the async task to load the level.
            return m_game->LoadLevelAsync().then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_game->SetCurrentLevelToSavedState();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
        else
        {
            // The game is not in the middle of a level so there aren't any level
            // resources to load.  Creating a no-op task so that in both cases
            // the same continuation logic is used.
            return create_task([]()
            {
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        // Since Game loading is an async task, the app visual state
        // may be too small or not have focus.  Put the state machine
        // into the correct state to reflect these cases.

        if (m_deviceResources->GetLogicalSize().Width < GameConstants::MinPlayableWidth)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::TooSmall;
            m_controller->Active(false);
            m_uiControl->HideGameInfoOverlay();
            m_uiControl->ShowTooSmall();
            m_renderNeeded = true;
        }
        else if (!m_haveFocus)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            m_controller->Active(false);
            m_uiControl->SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
        }
    }, task_continuation_context::use_current());
}
```

## <a name="run-method-of-the-view-provider"></a>Die Run-Methode des Ansichtsanbieters

Die drei zuvor beschriebenen Methoden: __Initialize__, __SetWindow__ und __Load__ haben die Vorbereitung abgeschlossen. Das Spiel kann jetzt auf die **Run**-Methode weitergehen und der Spaß kann beginnen! Die Ereignisse, die es für den Wechsel des Spielzustands nutzt, werden verteilt und verarbeitet. Die Grafik wird im Rahmen der Spielschleifendurchläufe aktualisiert.

### <a name="apprun"></a>App::Run

Starten Sie eine __While__-Schleife, die beendet wird, wenn der Spieler das Spielfenster schließt.

Der Beispielcode geht im Zustandsautomaten der Spielengine in einen von zwei Zuständen über:
    * __Deactivated__: Das Spielfenster wird deaktiviert (verliert also den Fokus) oder angedockt. In diesem Fall hält das Spiel die Ereignisverarbeitung an und wartet auf den Fensterfokus bzw. darauf, dass das Fenster wieder abgedockt wird.
    * __TooSmall__: Das Spiel aktualisiert den eigenen Zustand und rendert die Grafik für die Anzeige.

Wenn Ihr Spiel den Fokus hat, müssen Sie jedes in der Meldungswarteschlange eingehende Ereignis behandeln. Daher müssen Sie [**CoreWindowDispatch.ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) mit der Option **ProcessAllIfPresent** aufrufen. Andere Optionen können zu einer verzögerten Verarbeitung von Meldungsereignissen führen. So entsteht der Eindruck, dass das Spiel nicht reagiert oder Toucheingaben nur träge umgesetzt werden.

Wenn die App nicht sichtbar ist, angehalten oder angedockt wurde, möchten wir nicht, dass sie Meldungen ausgibt, die niemals ankommen, und dabei auch noch Ressourcen beansprucht. Daher muss das Spiel **ProcessOneAndAllPending** verwenden, was eine Blockierung bis zum Eingang eines Ereignisses zur Folge hat. Dieses Ereignis wird dann zusammen mit anderen Ereignissen verarbeitet, die während der Verarbeitung des ersten Ereignisses in der Prozesswarteschlange eingehen. [**Processvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) wird dann sofort nach der Verarbeitung der Warteschlange zurückgegeben.

```cpp
void App::Run()
{
    m_main->Run();
}

void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
                    switch (m_pressResult)
                    {
                    case PressResultState::LoadGame:
                        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
                        break;

                    case PressResultState::PlayLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                        break;

                    case PressResultState::ContinueLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::Pause);
                        break;
                    }
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // exiting due to window close.  Make sure to save state.
}
```

## <a name="uninitialize-method-of-the-view-provider"></a>Die Uninitialize-Methode des Ansichtsanbieters

Wenn der Benutzer letztendlich die Sitzung beendet, müssen wir sie bereinigen. An dieser Stelle kommt die **Uninitialize**-Eigenschaft ins Spiel.

In Windows 10 wird durch das Schließen des App-Fensters der App-Prozess nicht beendet, stattdessen wird der Zustand der APP-Singleton in den Arbeitsspeicher geschrieben. Sollte eine besondere Aktion wie eine spezielle Bereinigung von Ressourcen erforderlich sein, wenn das System diesen Speicher freigeben muss, platzieren Sie den Code für diese Bereinigung in dieser Methode.

### <a name="app-uninitialize"></a>App:: Uninitialize

```cpp
void App::Uninitialize()
{
}
```

## <a name="tips"></a>Tipps

Erstellen Sie Ihren Startcode bei der Entwicklung Ihres eigenen Spiels mithilfe dieser Methoden. Es folgt eine Liste mit einfachen Vorschlägen für die einzelnen Methoden:

-   Verwenden Sie **Initialize**, um Ihre Hauptklassen zuzuordnen und die grundlegenden Ereignishandler zu verbinden.
-   Verwenden Sie **SetWindow**, um das Hauptfenster zu erstellen und fensterspezifische Ereignisse zu verbinden.
-   Verwenden Sie **Load** für verbleibende Einrichtungsaufgaben und um das asynchrone Erstellen von Objekten und Laden von Ressourcen zu initiieren. Wenn temporäre Dateien oder Daten erstellt werden müssen, wie beispielsweise Assets, die im Rahmen von Prozeduren generiert werden, ist dies hier ebenfalls die richtige Stelle.


## <a name="next-steps"></a>Nächste Schritte

Dies schließt die grundlegende Struktur eines UWP-Spiels mit DirectX ab. Bedenken Sie diese fünf Methoden in anderen Bereichen, da diese in anderen Abschnitten erneut aufgegriffen werden. Als Nächstes nehmen wir einen detaillierten Einblick in die Verwaltung der Spielzustände und der Ereignisbehandlung unter [Spielablaufverwaltung](tutorial-game-flow-management.md), damit das Spiel fortgesetzt wird.



