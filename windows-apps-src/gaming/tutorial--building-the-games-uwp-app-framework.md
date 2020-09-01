---
title: Definieren des UWP-App-Frameworks für das Spiel
description: Der erste Schritt beim Programmieren eines universelle Windows-Plattform-Spiels (UWP) ist das Entwickeln des Frameworks, mit dem das App-Objekt mit Windows interagieren kann.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 2d762aebeaa5c97c1b23a91f0765cb5c09a91b46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163054"
---
#  <a name="define-the-games-uwp-app-framework"></a>Definieren des UWP-App-Frameworks für das Spiel

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

Der erste Schritt beim Programmieren eines universelle Windows-Plattform-Spiels (UWP) ist das Entwickeln des Frameworks, mit dem das App-Objekt mit Windows interagieren kann, einschließlich Windows-Runtime Features wie Suspend-Resume-Ereignis Behandlung, Änderungen im Fenster Fokus und ausrichten.

## <a name="objectives"></a>Ziele

* Richten Sie das Framework für ein DirectX-Spiel von universelle Windows-Plattform (UWP) ein, und implementieren Sie den Zustands Automat, der den gesamten Spielfluss definiert.

> [!NOTE]
> Um dieses Thema zu befolgen, sehen Sie sich den Quellcode für das [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) -Beispiel Spiel an, das Sie heruntergeladen haben.

## <a name="introduction"></a>Einführung

Im Thema [Einrichten des Spiel Projekts](tutorial--setting-up-the-games-infrastructure.md) haben wir die **wWinMain** -Funktion sowie die Schnittstellen [**iframeworkviewsource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) und [**iframeworkview**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) eingeführt. Wir haben gelernt, dass die **App** -Klasse (die Sie sehen können, die in der `App.cpp` Quell Code Datei im **Simple3DGameDX** -Projekt definiert ist) sowohl als *Ansichts Anbieterfactory* als auch als *Ansichts Anbieter*fungiert.

Dieses Thema wird von dort aus übernommen und geht ausführlicher darauf ein, wie die **App** -Klasse in einem Spiel die Methoden von **iframeworkview**implementieren sollte.

## <a name="the-appinitialize-method"></a>Die APP:: Initialize-Methode

Beim Anwendungsstart ist die erste Methode, die Windows aufruft, die Implementierung von [**iframeworkview:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize).

Ihre Implementierung sollte das grundlegendste Verhalten eines UWP-Spiels behandeln, z. b. sicherstellen, dass das Spiel eine Suspend (und ein mögliches späteres fortsetzen)-Ereignis behandeln kann, indem diese Ereignisse abonniert werden. Wir haben auch Zugriff auf das Anzeige Adapter Gerät, damit wir Grafik Ressourcen erstellen können, die vom Gerät abhängen.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

Vermeiden Sie nach Möglichkeit unformatierte Zeiger (und ist fast immer möglich).

- Bei Windows-Runtime Typen können Sie sehr häufig Zeiger ganz vermeiden und nur einen Wert auf dem Stapel erstellen. Wenn Sie einen Zeiger benötigen, verwenden Sie [**WinRT:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) (ein Beispiel hierfür finden Sie in Kürze).
- Verwenden Sie für eindeutige Zeiger den [**Std:: unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances) -und den **Std:: make_unique**.
- Verwenden Sie für freigegebene Zeiger [**Std:: shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances) und **Std:: make_shared**.

## <a name="the-appsetwindow-method"></a>Die APP:: SetWindow-Methode

Nach der **Initialisierung**ruft Windows die Implementierung von [**iframeworkview:: SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)auf und übergibt ein [**corewindow**](/uwp/api/windows.ui.core.corewindow) -Objekt, das das Hauptfenster des Spiels darstellt.

In **App:: SetWindow**abonnieren wir Fenster bezogene Ereignisse und konfigurieren einige Fenster-und Anzeigeverhalten. Beispielsweise erstellen wir einen Mauszeiger (über die [**corecursor**](/uwp/api/windows.ui.core.corecursor) -Klasse), der sowohl von Maus-als auch von Touchscreen-Steuerelementen verwendet werden kann. Wir übergeben auch das Window-Objekt an das Geräte abhängige Ressourcen Objekt.

Wir werden uns ausführlicher mit der Behandlung von Ereignissen im Thema [Game Flow Management](tutorial-game-flow-management.md#event-handling) beschäftigen.

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>Die APP:: Load-Methode

Nachdem das Hauptfenster festgelegt wurde, wird die Implementierung von [**iframeworkview:: Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) aufgerufen. Die **Auslastung** ist ein besserer Ort, um Spieldaten oder Assets vorab abzurufen, als " **Initialize** " und " **SetWindow**".

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

Wie Sie sehen können, wird die tatsächliche Arbeit an den Konstruktor des **gamemain** -Objekts delegiert, das wir hier machen. Die **gamemain** -Klasse wird in `GameMain.h` und definiert `GameMain.cpp` .

### <a name="the-gamemaingamemain-constructor"></a>Der gamemain:: gamemain-Konstruktor

Der **gamemain** -Konstruktor (und die anderen Member-Funktionen, die er aufruft) startet eine Reihe von asynchronen Ladevorgängen, um die Spielobjekte zu erstellen, Grafik Ressourcen zu laden und den Zustands Automat des Spiels zu initialisieren. Wir führen auch alle notwendigen Vorbereitungen aus, bevor das Spiel beginnt, z. b. das Festlegen von Start Zuständen oder globalen Werten.

Windows erzwingt eine Beschränkung für die Zeit, die das Spiel dauern kann, bevor die Verarbeitung der Eingabe beginnt. Daher bedeutet die Verwendung von asyc, wie wir hier Vorgehen, bedeutet, dass die **Last** schnell zurückgegeben werden kann, während die begonnene Arbeit im Hintergrund fortgesetzt wird. Wenn das Laden sehr lange dauert, oder wenn viele Ressourcen vorhanden sind, ist die Bereitstellung der Benutzer mit einer häufig aktualisierten Statusanzeige eine gute Idee. 

Wenn Sie mit der asynchronen Programmierung noch nicht vertraut sind, finden Sie weitere Informationen unter Parallelität [und asynchrone Vorgänge mit C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
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
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

Im folgenden finden Sie einen Überblick über die Abfolge der Aufgaben, die vom Konstruktor gestartet werden.

- Erstellen und initialisieren Sie ein Objekt vom Typ " **gamerenderer**". Weitere Informationen finden Sie unter [Rendering Framework I: Intro to Rendering](tutorial--assembling-the-rendering-pipeline.md).
- Erstellen und initialisieren Sie ein Objekt vom Typ **Simple3DGame**. Weitere Informationen finden Sie unter [Definieren des Hauptspiel Objekts](tutorial--defining-the-main-game-loop.md).
- Erstellen Sie das Steuerelement Objekt für die Spiel-UI, und zeigen Sie die Spiel Info Überlagerung an, um eine Statusanzeige beim Laden der Ressourcen Dateien anzuzeigen Weitere Informationen finden Sie unter [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md).
- Erstellen Sie ein Controller Objekt, um Eingaben vom Controller zu lesen (Touchscreen, Maus oder Xbox Wireless Controller). Weitere Informationen finden Sie unter [Hinzufügen von Steuerelementen](tutorial--adding-controls.md).
- Definieren Sie zwei rechteckige Bereiche in der unteren linken und unteren rechten Ecke des Bildschirms für die Steuerelemente "verschieben" und "Kamera berühren". Der Player verwendet das untere linke Rechteck (definiert im Aufrufen von **setmoverect**) als virtuelles Kontroll Pad, um die Kamera vorwärts und rückwärts zu bewegen, und Seite zu Seite. Das untere rechte Rechteck (definiert durch die **setfirerermethode** ) wird als virtuelle Schaltfläche verwendet, um den Ammo auszulösen.
- Verwenden sie Coroutinen, um das Laden von Ressourcen in separate Phasen zu unterbrechen. Der Zugriff auf den Direct3D-Gerätekontext ist auf den Thread beschränkt, in dem der Gerätekontext erstellt wurde. der Zugriff auf das Direct3D-Gerät für die Objekt Erstellung erfolgt zwar kostenlos. Folglich kann die " **gamerenderer:: forategamedeviceresourcesasync** "-Coroutine in einem separaten Thread von der Abschluss Aufgabe (**gamerenderer:: finalizecreategamedeviceresources**) ausgeführt werden, die im ursprünglichen Thread ausgeführt wird.
- Wir verwenden ein ähnliches Muster für das Laden von ebenendressourcen mit **Simple3DGame:: loadlevelasync** und **Simple3DGame:: finalizeloadlevel**.

Im nächsten Thema ([Game Flow Management](tutorial-game-flow-management.md)) wird mehr von **gamemain:: initializegamestate** angezeigt.

## <a name="the-apponactivated-method"></a>Die APP:: onaktivierte Methode

Als nächstes wird das [**coreapplicationview:: aktivierte**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) -Ereignis ausgelöst. Daher wird jeder **aktivierte aktivierte** Ereignishandler (z. b. die **App:: onaktivierte** Methode) aufgerufen.

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

Die einzige Arbeit, die wir hier tun, besteht darin, das Haupt- [**corewindow**](/uwp/api/windows.ui.core.corewindow)zu aktivieren. Alternativ dazu können Sie auch in **App:: SetWindow**auswählen.

## <a name="the-apprun-method"></a>Die APP:: Run-Methode

**Initialize**, **SetWindow**und **Load** haben die Stufe festgelegt. Nachdem das Spiel eingerichtet ist und ausgeführt wird, wird die Implementierung von [**iframeworkview:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) aufgerufen.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Auch hier wird die Arbeit an **gamemain**delegiert.

### <a name="the-gamemainrun-method"></a>Die gamemain:: Run-Methode

**Gamemain:: Run** ist die Hauptschleife des Spiels. Sie finden Sie in `GameMain.cpp` . Die grundlegende Logik besteht darin, dass das Fenster für das Spiel zwar geöffnet bleibt, aber alle Ereignisse verteilt, den Timer aktualisiert und anschließend die Ergebnisse der Grafik Pipeline gerenbt und präsentiert. Außerdem werden die Ereignisse, die für den Übergang zwischen den Spiel Zuständen verwendet werden, verteilt und verarbeitet.

Der Code hier befasst sich auch mit zwei Zuständen im Status Computer der Spiel-Engine.

- **Updateenginestate::D eaktivierter**. Dadurch wird festgelegt, dass das Spielfenster deaktiviert ist (der Fokus verliert) oder am gedockt ist. 
- **Updateenginestate::**"". Dies gibt an, dass der Client Bereich zu klein ist, um das Spiel zu Rendering.

In einem dieser Zustände hält das Spiel die Ereignisverarbeitung an und wartet darauf, dass sich das Fenster auf den Fokus, auf nicht NAP oder seine Größe ändert.

Wenn Ihr Spiel den Fokus hat, müssen Sie jedes in der Meldungswarteschlange eingehende Ereignis behandeln. Daher müssen Sie [**CoreWindowDispatch.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) mit der Option **ProcessAllIfPresent** aufrufen. Andere Optionen können Verzögerungen bei der Verarbeitung von Nachrichten Ereignissen verursachen, sodass Ihr Spiel nicht mehr reagiert oder das Verhalten von Finger Eingaben zu einem langsamen Verhalten führt.

Wenn das Spiel nicht sichtbar, angehalten oder angedockt ist, ist es nicht ratsam, Ressourcen zu nutzen, um Nachrichten zu senden, die niemals eintreffen. In diesem Fall muss Ihr Spiel die Option **processoneandallpending** verwenden. Diese Option wird blockiert, bis ein Ereignis abgerufen wird, und anschließend wird dieses Ereignis verarbeitet (sowie alle anderen Personen, die während der Verarbeitung der ersten in der Prozess Warteschlange eintreffen). **Corewindowdispatch. procesendvents** wird dann sofort nach der Verarbeitung der Warteschlange zurückgegeben.

```cppwinrt
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
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="the-appuninitialize-method"></a>Die APP:: Uninitialize-Methode

Nach Beendigung des Spiels wird die Implementierung von [**iframeworkview:: Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) aufgerufen. Das ist unsere Gelegenheit, eine Bereinigung durchzuführen. Das Schließen des App-Fensters bricht den Prozess der APP nicht ab. Stattdessen wird der Zustand des App-Singleton in den Arbeitsspeicher geschrieben. Wenn beim Freigeben dieses Speichers durch das System, einschließlich der besonderen Bereinigung von Ressourcen, etwas besonderes erforderlich ist, dann platzieren Sie den Code für diesen Bereinigung in der **Initialisierung**.

In unserem Fall ist " **App:: Uninitialize** " kein Vorgang.

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>Tipps

Wenn Sie Ihr eigenes Spiel entwickeln, entwerfen Sie den Startcode um die in diesem Thema beschriebenen Methoden. Im folgenden finden Sie eine einfache Liste der grundlegenden Vorschläge für jede Methode.

- Verwenden Sie **Initialize** , um die Hauptklassen zuzuordnen, und verbinden Sie die grundlegenden Ereignishandler.
- Verwenden Sie **SetWindow** , um beliebige Fenster spezifische Ereignisse zu abonnieren und das Hauptfenster an das Geräte abhängige Ressourcen Objekt zu übergeben, sodass dieses Fenster beim Erstellen einer Swapkette verwendet werden kann.
- Verwenden Sie **Load** , um das verbleibende Setup zu verarbeiten und die asynchrone Erstellung von Objekten und das Laden von Ressourcen zu initiieren. Wenn Sie temporäre Dateien oder Daten erstellen möchten, z. b. prozedurale generierte Assets, dann müssen Sie dies auch hier tun.

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema wurde ein Teil der grundlegenden Struktur eines UWP-Spiels behandelt, das DirectX verwendet. Es ist ratsam, diese Methoden zu berücksichtigen, da wir in späteren Themen auf einige davon verweisen werden.

Im nächsten Thema &mdash; [Game Flow Management](tutorial-game-flow-management.md) &mdash; werden wir ausführlich darauf eingehen, wie Spiele Zustände und Ereignis Behandlung verwaltet werden, um das Spiel zu halten.