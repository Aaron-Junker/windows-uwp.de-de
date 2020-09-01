---
title: Definieren des Hauptobjekts für das Spiel
description: Nun sehen wir uns die Details des Haupt Objekts des Beispiel Spiels an und wie die implementierten Regeln in Interaktionen mit der Spiel Welt übersetzt werden.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, Games, Main-Objekt
ms.localizationpriority: medium
ms.openlocfilehash: 497a1f0dc16308b4b9360aff958b94f04b6283ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162994"
---
# <a name="define-the-main-game-object"></a>Definieren des Hauptobjekts für das Spiel

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

Nachdem Sie das grundlegende Framework des Beispiel Spiels angelegt und einen Zustands Automat implementiert haben, der das allgemeine Benutzer-und Systemverhalten behandelt, sollten Sie die Regeln und die Mechanismen untersuchen, mit denen das Beispiel Spiel in ein Spiel verwandelt wird. Betrachten wir nun die Details des Haupt Objekts des samplingspiels und das Übersetzen von Spielregeln in Interaktionen mit der Spiel Welt.

## <a name="objectives"></a>Ziele

- Erfahren Sie, wie Sie grundlegende Entwicklungsverfahren zum Implementieren von Spielregeln und-Mechanismen für ein UWP DirectX-Spiel anwenden.

## <a name="main-game-object"></a>Hauptobjekt für das Spiel

Im **Simple3DGameDX** -Beispiel Spiel ist **Simple3DGame** die Hauptklasse der Spielobjekte. Eine Instanz von **Simple3DGame** wird indirekt über die **App:: Load** -Methode erstellt.

Im folgenden finden Sie einige Funktionen der **Simple3DGame** -Klasse.

- Enthält die Implementierung der-Spiellogik.
- Enthält Methoden, die diese Details übermitteln.
  - Ändert den Spielzustand in den Zustands Automat, der im App-Framework definiert ist.
  - Ändert den Spielzustand von der APP zum Spielobjekt selbst.
  - Details zum Aktualisieren der Benutzeroberfläche des Spiels (Overlay und Heads-Up-Display), Animationen und Physik (Dynamics).
  > [!NOTE]
  > Das Aktualisieren von Grafiken wird von der **gamererererklasse** verarbeitet, die Methoden zum Abrufen und Verwenden von Grafikgeräte Ressourcen enthält, die vom Spiel verwendet werden. Weitere Informationen finden Sie unter [Rendering Framework I: Intro to Rendering](tutorial--assembling-the-rendering-pipeline.md).
- Dient als Container für die Daten, die eine Spielsitzung,-Ebene oder-Lebensdauer definieren, je nachdem, wie Sie Ihr Spiel auf hoher Ebene definieren. In diesem Fall sind die Spiel Zustandsdaten für die Lebensdauer des Spiels und werden einmal initialisiert, wenn ein Benutzer das Spiel aufruft.

Informationen zum Anzeigen der von dieser Klasse definierten Methoden und Daten finden Sie unter [der Simple3DGame-Klasse](#the-simple3dgame-class) unten.

## <a name="initialize-and-start-the-game"></a>Initialisieren und Starten des Spiels

Wenn ein Spieler das Spiel startet, muss das Spielobjekt den eigenen Zustand initialisieren, das Overlay erstellen und hinzufügen, die Variablen zur Nachverfolgung der Erfolge des Spielers festlegen und die Objekte instanziieren, die zum Erstellen der Level benötigt werden. In diesem Beispiel wird dies erreicht, wenn die **gamemain** -Instanz in " **App:: Load**" erstellt wird.

Das Game-Objekt vom Typ **Simple3DGame**wird im **gamemain:: gamemain** -Konstruktor erstellt. Sie wird dann mithilfe der **Simple3DGame:: Initialize** -Methode initialisiert, während der " **gamemain:: constructinbackground** Fire-and-Forget"-Coroutine, die von " **gamemain:: gamemain**" aufgerufen wird.

### <a name="the-simple3dgameinitialize-method"></a>Die Simple3DGame:: Initialize-Methode

Das Beispiel Spiel richtet diese Komponenten im Game-Objekt ein.

- Ein neues Audiowiedergabeobjekt wird erstellt.
- Arrays für die Grafikgrundtypen des Spiels werden erstellt (einschließlich Arrays für die Levelgrundtypen, Munition und Hindernisse).
- Ein Speicherort für die Spielzustandsdaten mit der Bezeichnung *Game* wird erstellt und am durch [**ApplicationData::Current**](/uwp/api/windows.storage.applicationdata.current) festgelegten Speicherort der App-Dateneinstellungen platziert.
- Ein Spieltimer und die anfängliche spielinterne Overlaybitmap werden erstellt.
- Eine neue Kamera mit einem spezifischen Satz von Ansichts- und Projektionsparametern wird erstellt.
- Das Eingabegerät (Controller) wird auf die gleiche Ausgangsausrichtung festgelegt wie die Kamera, sodass die Ausgangsposition der Steuerung exakt der Kameraposition entspricht.
- Das Spielerobjekt wird erstellt und aktiviert. Wir verwenden ein Sphere-Objekt, um die Nähe des Players zu Wänden und Hindernissen zu erkennen und zu verhindern, dass die Kamera an einer Position positioniert wird, die das Eintauchen möglicherweise unterbricht.
- Der Spielweltgrundtyp wird erstellt.
- Die Zylinderhindernisse werden erstellt.
- Die Ziele (**Face**-Objekte) werden erstellt und nummeriert.
- Die Munitionskugeln werden erstellt.
- Die Level werden erstellt.
- Der Highscore wird geladen.
- Alle ggf. zuvor gespeicherten Spielzustände werden geladen.

Das Spiel verfügt jetzt über Instanzen aller Hauptkomponenten &mdash; der Welt, des Players, der Hindernisse, der Ziele und der Ammo-Sphären. Außerdem besitzt es Instanzen der Level. Diese stehen für Konfigurationen aller oben genannten Komponenten und ihrer Verhalten für die einzelnen spezifischen Level. Sehen wir uns nun an, wie das Spiel die Ebenen erstellt.

## <a name="build-and-load-game-levels"></a>Spiele Ebenen erstellen und laden

In den Dateien, die `Level[N].h/.cpp` im Ordner " **gamelevels** " der Beispiellösung gefunden werden, wird der größte Teil der Arbeit auf der Ebene erstellt. Da es sich um eine sehr spezifische Implementierung konzentriert, werden Sie hier nicht behandelt. Wichtig ist, dass der Code für jede Ebene als separates **Ebene [N]** -Objekt ausgeführt wird. Wenn Sie das Spiel erweitern möchten, können Sie ein **Level [N]** -Objekt erstellen, das eine zugewiesene Zahl als Parameter annimmt und die Hindernisse und Ziele nach dem Zufallsprinzip platziert. Oder Sie können Konfigurationsdaten auf Lade Ebene aus einer Ressourcen Datei oder sogar aus dem Internet laden.

## <a name="define-the-gameplay"></a>Definieren des Spiels

An dieser Stelle haben wir alle Komponenten, die wir für die Entwicklung des Spiels benötigen. Die Ebenen wurden im Arbeitsspeicher aus den primitiven erstellt und sind bereit für den Start der Interaktion mit dem Player.

Die besten Spiele reagieren umgehend auf die Eingabe des Players und stellen sofortiges Feedback bereit. Dies gilt für jede Art von Spiel, von Twitch-Action und Echt Zeit Ersteller bis hin zu durchdachten, Turn-basierten Strategie spielen.

### <a name="the-simple3dgamerungame-method"></a>Die Simple3DGame:: rungame-Methode

Während der Ausführung einer Spielebene befindet sich das Spiel im **Dynamics** -Zustand. 

**Gamemain:: Update** ist die Haupt Update Schleife, die den Anwendungs Zustand einmal pro Frame aktualisiert, wie unten gezeigt. Die Update-Schleife ruft die **Simple3DGame:: rungame** -Methode auf, um die Arbeit zu verarbeiten, wenn sich das Spiel im **Dynamics** -Zustand befindet.

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
                ...
```
          
**Simple3DGame:: rungame** verarbeitet den Satz von Daten, der den aktuellen Zustand des Spiels für die aktuelle Iterationen der Spiel Schleife definiert.

Hier ist die Spielfluss Logik in **Simple3DGame:: rungame**.

- Die-Methode aktualisiert den Timer, der die Sekunden nach unten anzählt, bis die Ebene abgeschlossen ist, und testet, ob die Zeit der Ebene abgelaufen ist. Dies ist eine der Regeln des Spiels &mdash; , wenn die Zeit abgelaufen ist, wenn nicht alle Ziele erreicht wurden, dann ist das Spiel.
- Wenn die Zeit abgelaufen ist, legt die-Methode den **Zeit abgelaufenen** Spielzustand fest und kehrt zur **Update** -Methode im vorherigen Code zurück.
- Wenn die Zeit verbleibt, wird der Verschiebungs Controller für ein Update der Kameraposition abgerufen. genauer gesagt: ein Update für den Winkel der Ansicht, das normal von der Kameraebene projiziert wird (wo der Spieler sucht), und die Entfernung, die der Winkel seit der letzten Abruf Position des Controllers verschoben wurde.
- Die Kamera wird auf der Grundlage der neuen Daten des Bewegungs- und Blickrichtungscontrollers aktualisiert.
- Die Dynamik (also die Animationen und das Verhalten von Objekten in der Spielwelt, die nicht vom Spieler gesteuert werden) wird aktualisiert. In diesem Beispiel Spiel wird die **Simple3DGame:: updatedynamics** -Methode aufgerufen, um die Bewegung der ausgelösten Ammo-Bereiche, die Animation der Säulen Hindernisse und die Verschiebung der Ziele zu aktualisieren. Weitere Informationen finden Sie unter [Aktualisieren der Spiel Welt](#update-the-game-world).
- Die-Methode überprüft, ob die Kriterien für den erfolgreichen Abschluss einer Ebene erfüllt sind. Wenn dies der Fall ist, wird das Ergebnis für die Ebene abgeschlossen, und es wird überprüft, ob es sich um die letzte Ebene (von 6) handelt. Wenn es sich um die letzte Ebene handelt, gibt die Methode den **gamestate:: gamecomplete** -Spielzustand zurück. Andernfalls wird der **gamestate:: levelcomplete** -Spielstatus zurückgegeben.
- Wenn die Ebene nicht fertig ist, legt die-Methode den Spielzustand auf **gamestate:: Active**fest und gibt zurück.

## <a name="update-the-game-world"></a>Aktualisieren der Spiel Welt

Wenn das Spiel in diesem Beispiel ausgeführt wird, wird die **Simple3DGame:: updatedynamics** -Methode von der **Simple3DGame:: rungame** -Methode aufgerufen (die aus **gamemain:: Update**aufgerufen wird), um Objekte zu aktualisieren, die in einer Spielszene gerendert werden.

Eine-Schleife, wie z. b. **updatedynamics** , ruft alle Methoden auf, die verwendet werden, um die Spiel Welt in Bewegung unabhängig von der Player Eingabe festzulegen, um ein immersives Spielverhalten zu schaffen und die Ebene in den Leben zu bringen. Dies umfasst Grafiken, die gerendert werden müssen, und das Ausführen von Animations Schleifen, um eine dynamische Welt zu bringen, auch wenn keine Spieler Eingaben vorhanden sind. In Ihrem Spiel kann es Strukturen geben, die sich im Wind befinden, Wellen entlang der Küstenlinien, das Rauchen von Maschinen und die Art und Weise, in der Sie sich bewegen. Hierzu gehört auch die Interaktion zwischen Objekten (beispielsweise Kollisionen zwischen der Spielerkugel und der Welt oder zwischen der Munition und den Hindernissen oder Zielen).

Wenn das Spiel nicht speziell angehalten wurde, sollte die Spiel Schleife weiterhin die Spielewelt aktualisieren. ob dies auf der Spiellogik, physischen Algorithmen oder ganz einfach ist.

Im Beispiel Spiel heißt dieses Prinzip *Dynamics*und umfasst den Anstieg und den Rückgang der Säulen Hindernisse sowie das Bewegungsverhalten und das physische Verhalten der Ammo-Sphären, wenn Sie ausgelöst und in Bewegung sind.

### <a name="the-simple3dgameupdatedynamics-method"></a>Die Simple3DGame:: updatedynamics-Methode 

Diese Methode behandelt diese vier Sätze von Berechnungen.

- Die Positionen der Kugeln für die abgefeuerte Munition in der Spielwelt.
- Die Animation der Säulenhindernisse.
- Die Überschneidung des Spielers mit den Begrenzungen der Spielwelt.
- Die Kollisionen von Munitionskugeln mit den Hindernissen, den Zielen, anderen Munitionskugeln und der Spielwelt.

Die Animation der Hindernisse findet in einer Schleife statt, die in den Quell Code Dateien **animieren. h/. cpp** definiert ist. Das Verhalten des Ammo und aller Kollisionen wird durch vereinfachte Physik Algorithmen definiert, die im Code bereitgestellt und durch eine Reihe von globalen Konstanten für die Spiel Welt parametrisiert werden, einschließlich der Schweregrad-und Materialeigenschaften. All dies wird im Koordinatenbereich der Spielwelt berechnet.

### <a name="review-the-flow"></a>Überprüfen des Flows

Nachdem wir alle Objekte in der Szene aktualisiert und alle Kollisionen berechnet haben, müssen wir diese Informationen verwenden, um die entsprechenden visuellen Änderungen zu zeichnen. 

Nachdem **gamemain:: Update** die aktuelle iterierung der Game-Schleife abgeschlossen hat, ruft das Beispiel sofort **gamerenderer:: Rendering** auf, um die aktualisierten Objektdaten zu erstellen und eine neue Szene zu generieren, die dem Player angezeigt wird, wie unten gezeigt.

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
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>Renderdie Grafiken der Spiel Welt

Es wird empfohlen, die Grafiken in einem Spiel zu aktualisieren, im Idealfall genau so oft wie die Hauptspiel Schleife. Wenn die Schleife durchlaufen wird, wird der Zustand der Spiel Welt mit oder ohne Player Eingabe aktualisiert. Dadurch können die berechneten Animationen und Verhalten reibungslos angezeigt werden. Stellen Sie sich vor, dass eine einfache Wasser Szene, die nur verschoben wurde, wenn der Player auf eine Schaltfläche geklickt hat. Das wäre nicht realistisch. ein gutes Spiel sieht immer glatt und flüssig aus.

Erinnern Sie sich an die Beispiel Spiel Schleife, wie oben in **gamemain:: Run**gezeigt. Wenn das Hauptfenster des Spiels sichtbar ist, nicht angedockt oder deaktiviert ist, werden die Ergebnisse dieses Updates weiterhin aktualisiert und gerenppt. Die " **gamerenderer:: Rendering** "-Methode, die wir als nächstes untersuchen, rendert eine Darstellung dieses Zustands. Dies erfolgt unmittelbar nach einem **callmain:: Update**-Befehl, der **Simple3DGame:: rungame** zum Aktualisieren von Zuständen umfasst, wie im vorherigen Abschnitt erläutert wurde.

**Gamerderderer:: Rendering** zeichnet die Projektion der 3D-Welt und zeichnet dann die Direct2D-Überlagerung. Nach Abschluss des Vorgangs zeigt sie die finale Swapchain mit den kombinierten Puffern an.

> [!NOTE]
> Es gibt zwei Zustände für den Direct2D-Overlay des samplingspiels &mdash; , bei dem das Spiel das Spiel Info-Overlay anzeigt, das die Bitmap für das Menü Pause enthält, und eines, in dem das Spiel die Kreuz und die Rechtecke für den Touchscreen-Verschiebungs Controller anzeigt. Der Text mit dem Spielstand wird in beiden Zuständen gezeichnet. Weitere Informationen finden Sie unter [Rendering Framework I: Intro to Rendering](tutorial--assembling-the-rendering-pipeline.md).

### <a name="the-gamerendererrender-method"></a>Die gamerderderer:: Rendering-Methode

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>Die Simple3DGame-Klasse

Dabei handelt es sich um die Methoden und Datenmember, die von der **Simple3DGame** -Klasse definiert werden.

### <a name="member-functions"></a>Memberfunktionen

Die von **Simple3DGame** definierten öffentlichen Member-Funktionen enthalten die folgenden.

- **Initialisieren**. Legt die Anfangswerte der globalen Variablen fest und initialisiert die Spielobjekte. Dies wird im Abschnitt [initialisieren und Starten des Spiels](#initialize-and-start-the-game) beschrieben.
- **LoadGame**. Initialisiert eine neue Ebene und beginnt damit, Sie zu laden.
- **LoadLevelAsync**. Eine Coroutine, die die Ebene initialisiert und dann eine weitere Coroutine für den Renderer aufruft, um die gerätespezifischen ebenendourcen zu laden. Diese Methode wird in einem gesonderten Thread ausgeführt. Daher können in diesem Thread nur [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)-Methoden (und keine [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)-Methoden) aufgerufen werden. Alle Gerätekontextmethoden werden in der **FinalizeLoadLevel**-Methode aufgerufen. Wenn Sie mit der asynchronen Programmierung noch nicht vertraut sind, finden Sie weitere Informationen unter Parallelität [und asynchrone Vorgänge mit C++/WinRT](../cpp-and-winrt-apis/concurrency.md).
- **FinalizeLoadLevel**. Führt alle Aktionen zum Laden des Levels aus, die im Hauptthread durchgeführt werden müssen. Dies schließt alle Aufrufe von Direct3D 11-Gerätekontextmethoden ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) ein.
- **StartLevel**. Startet das Spiel für eine neue Ebene.
- **PauseGame**. Hält das Spiel an.
- **RunGame**. Führt eine Iteration der Spielschleife aus. Wird jeweils einmal pro Iteration der Spielschleife von **App::Update** aufgerufen, sofern sich das Spiel im Zustand **Active** befindet.
- **OnSuspending** und **OnResuming**. Halten Sie die Audiodatei an bzw. setzen Sie Sie fort.

Hier sind die private Member-Funktionen aufgeführt.

- **LoadSavedState** und **SaveState**. Laden/Speichern Sie den aktuellen Status des Spiels.
- **Loadhighscore** und **savehighscore**. Laden/Speichern Sie die hohe Bewertung über die Spiele hinweg.
- **InitializeAmmo**. Setzt den Zustand der als Munition verwendeten Kugelobjekte in den Originalzustand für den Beginn einer neuen Runde zurück.
- **UpdateDynamics**. Dies ist eine wichtige Methode, da Sie alle Spielobjekte auf der Grundlage von Animations Routinen, Physik und Steuerungs Eingaben aktualisiert. Hierbei handelt es sich gewissermaßen um das Herzstück der Interaktivität, die das Spiel ausmacht. Dies wird im Abschnitt [Aktualisieren der Spiel Welt](#update-the-game-world) behandelt.

Bei den anderen öffentlichen Methoden handelt es sich um einen Eigenschafts Accessor, der für die Anzeige ein-und über Lagerungs spezifische Informationen zum App-Framework zurückgibt

### <a name="data-members"></a>Datenmember

Diese Objekte werden aktualisiert, wenn die Spiel Schleife ausgeführt wird.

- Das Objekt " **muvelookcontroller** ". Stellt die Eingabe des Players dar. Weitere Informationen finden Sie unter [Hinzufügen von Steuerelementen](tutorial--adding-controls.md).
- **Gamerenderer** -Objekt. Stellt einen Direct3D 11-Renderer dar, der alle gerätespezifischen Objekte und deren Rendering behandelt. Weitere Informationen finden Sie unter [Rendering Framework I](tutorial--assembling-the-rendering-pipeline.md).
- **Audioobjekt** . Steuert die Audiowiedergabe für das Spiel. Weitere Informationen finden Sie unter [Hinzufügen von Sound](tutorial--adding-sound.md).

Die restlichen Spiel Variablen enthalten die Listen der primitiven und ihre jeweiligen in-Game-Beträge sowie die spezifischen Daten und Einschränkungen für das Spiel.

## <a name="next-steps"></a>Nächste Schritte

Wir müssen noch über die tatsächliche Renderingengine sprechen, &mdash; wie Aufrufe der **Rendermethoden** auf den aktualisierten primitiven in Pixel auf dem Bildschirm umgewandelt werden. Diese Aspekte werden in zwei Teilen beschrieben: Einführung in das &mdash; [Rendering](tutorial--assembling-the-rendering-pipeline.md) und [Rendering Framework II: Game Rendering](tutorial-game-rendering.md). Wenn Sie mehr darüber interessiert sind, wie die Player Steuerelemente den Spielzustand aktualisieren, finden Sie weitere Informationen unter [Hinzufügen von Steuerelementen](tutorial--adding-controls.md).