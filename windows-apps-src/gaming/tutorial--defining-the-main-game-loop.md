---
title: Definieren des Hauptobjekts für das Spiel
description: In diesem Abschnitt widmen wir uns den Details des Hauptobjekts des Beispielspiels. Außerdem erfahren Sie, wie die implementierten Regeln in Interaktionen mit der Spielwelt übersetzt werden.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Hauptobjekt
ms.localizationpriority: medium
ms.openlocfilehash: 96aefc8b053dd7490f47910ca5bb79989855e1a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651495"
---
# <a name="define-the-main-game-object"></a>Definieren des Hauptobjekts für das Spiel

Sobald Sie das grundlegende Framework des beispielspiels angeordnet und implementiert einen Statuscomputer, der die allgemeine Benutzer und das Systemverhalten behandelt haben, sollten Sie überprüfen die Regeln und die Mechanismen, die das Beispiel in einem Spiel umwandeln. Sehen wir uns die Details der Hauptobjekt die Spiele-Beispiel, und wie Spiele Regeln in Interaktionen mit dem Spiel weltweit zu übersetzen.

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Ziel

Erfahren Sie, wie Sie einfache Entwicklungsszenarien Techniken zum Implementieren der game-Regeln und Mechanismen für eine UWP-DirectX-Spielen anwenden.

## <a name="main-game-object"></a>Hauptobjekt für das Spiel

In diesem Beispielspiel __Simple3DGame__ ist die wichtigste spielobjekt-Klasse. Eine Instanz von __Simple3DGame__ Objekt wird erstellt, der __App::Load__ Methode.

Die __Simple3DGame__ Klassenobjekt:
* Gibt an, die Gaming-Logik implementiert
* Enthält Methoden für die Kommunikation:
    * Änderungen bei der game Zustand, der den Zustandsautomaten, der in der app-Framework definiert.
    * Änderungen bei der game Zustand wird von der app auf das Spiel Objekt selbst.
    * Informationen zum Aktualisieren des Spiels Benutzeroberfläche ("Overlay" und "Heads-Up-Display"), Animationen und physikalische Eigenschaften (die Dynamics).

    >[!Note]
    >Aktualisieren der Grafiken erfolgt durch die __GameRenderer__ -Klasse, die Methoden zum Abrufen und Verwenden von Grafikressourcen-Gerät ein, die das Spiel enthält. Weitere Informationen finden Sie unter [Rendering-Framework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md).

* Dient als Container für die Daten, die eine Spiele-Sitzung Ebene zu definieren oder für die Lebensdauer, je nachdem, wie Sie Ihr Spiel auf hoher Ebene definieren. In diesem Fall die Spielzustands Daten sind für die Lebensdauer des Spiels und einmal, wenn ein Benutzer das Spiel startet, initialisiert werden.

Um Methoden und in dieses Objekt der Klasse definierten Daten anzuzeigen, wechseln Sie zu [Simple3DGame Objekt](#simple3dgame-object).

## <a name="initialize-and-start-the-game"></a>Initialisieren Sie und starten Sie das Spiel

Wenn ein Spieler das Spiel startet, muss das Spielobjekt den eigenen Zustand initialisieren, das Overlay erstellen und hinzufügen, die Variablen zur Nachverfolgung der Erfolge des Spielers festlegen und die Objekte instanziieren, die zum Erstellen der Level benötigt werden. In diesem Beispiel wird dies durchgeführt wird, wenn die neue __GameMain__ Instanz wird erstellt, [ __App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123). 

Das spielobjekt, __Simple3DGame__, wird erstellt, der __GameMain__ Konstruktor. Es ist dann initialisiert, mit der [ __Simple3DGame::Initialize__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) Methode während der [Async erstellen Aufgabe in der __GameMain__ Konstruktor](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize-Methode

Das Beispiel richtet die folgenden Komponenten in das spielobjekt ab:

* Ein neues Audiowiedergabeobjekt wird erstellt.
* Arrays für die Grafikgrundtypen des Spiels werden erstellt (einschließlich Arrays für die Levelgrundtypen, Munition und Hindernisse).
* Ein Speicherort für die Spielzustandsdaten mit der Bezeichnung *Game* wird erstellt und am durch [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619) festgelegten Speicherort der App-Dateneinstellungen platziert.
* Ein Spieltimer und die anfängliche spielinterne Overlaybitmap werden erstellt.
* Eine neue Kamera mit einem spezifischen Satz von Ansichts- und Projektionsparametern wird erstellt.
* Das Eingabegerät (Controller) wird auf die gleiche Ausgangsausrichtung festgelegt wie die Kamera, sodass die Ausgangsposition der Steuerung exakt der Kameraposition entspricht.
* Das Spielerobjekt wird erstellt und aktiviert. Wir verwenden eine Kugel-Objekt, um Nähe Wände und Hindernisse des Spielers zu erkennen und verhindern, dass die Kamera erste platziert, in der Lage, die zu Entwicklungsthemen beeinträchtigen könnte.
* Der Spielweltgrundtyp wird erstellt.
* Die Zylinderhindernisse werden erstellt.
* Die Ziele (**Face**-Objekte) werden erstellt und nummeriert.
* Die Munitionskugeln werden erstellt.
* Die Level werden erstellt.
* Der Highscore wird geladen.
* Alle ggf. zuvor gespeicherten Spielzustände werden geladen.

Das Spiel besitzt nun Instanzen aller wichtigen Komponenten: Spielwelt, Spieler, Hindernisse, Ziele und Munitionskugeln. Außerdem besitzt es Instanzen der Level. Diese stehen für Konfigurationen aller oben genannten Komponenten und ihrer Verhalten für die einzelnen spezifischen Level. Im nächsten Abschnitt widmen wir uns der Levelerstellung des Spiels.

## <a name="build-and-load-game-levels"></a>Erstellen und Laden von Level in Spielen

Die Hauptarbeit für die Ebene zur Erstellung, in abgeschlossen ist den meisten der __Level.h/.cpp__ Dateien finden Sie in der __GameLevels__ Ordner der beispiellösung. Da der Schwerpunkt auf einer sehr spezifischen Implementierung liegt, wird nicht wir werden hier behandelt. Entscheidend ist in diesem Zusammenhang, dass der Code für die einzelnen Levels jeweils als separates __LevelN__-Objekt ausgeführt wird. Wenn Sie das Spiel erweitern möchten, können Sie erstellen eine **Ebene** -Objekt, das eine zugewiesene Nummer, die als Parameter und nach dem Zufallsprinzip dauert fügt die Hindernisse und Ziele. Oder Sie lassen sie die Konfiguration der Ebene Daten aus einer Ressourcendatei oder sogar das Internet zu laden.

## <a name="define-the-game-play"></a>Definieren Sie die Ausführung des Spiels

Wir verfügen nun über alle Komponenten, die wir benötigen, um das Spiel zusammenzufügen. Die Ebenen im Speicher die primitiven erstellt wurden, und Sie sind bereit für den Player zu interagieren.

%Tdas besten Spiele sofort mit Player-Eingabe zu reagieren, und geben unmittelbar Feedback. Dies gilt für alle Arten von eines Spiels twitch integrierte-Aktion, in Echtzeit Egoshooter, eine gut durchdachte und Turn-basierte Strategie Spiele.

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame-Methode

Wenn Sie eine Ebene wiedergeben zu können, ist das Spiel den __Dynamics__ Zustand. 

[__GameMain::Update__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) ist die Haupt-Update-Schleife, die Zustand der Anwendung einmal pro Frame aktualisiert werden, wie unten dargestellt. In der Update-Schleife, ruft der [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) Methode, um die Arbeit zu verarbeiten, ist das Spiel in der __Dynamics__ Zustand.

```cpp
// Updates the application state once per frame.
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
        //...

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when playing a level, the game is in the Dynamics state. Work is handled by Simple3DGame::RunGame method.
            switch (runState)
            {
                
      //...
```
          
[__Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) behandelt die Datenmenge, die den aktuellen Zustand der Ausführung des Spiels für die aktuelle Iteration der Schleife Spiel definiert.

Flow-Logik in Spielen __RunGame__:
*  Die Methode aktualisiert den Zeitgeber, der die Sekunden bis zum Levelabschluss herunterzählt, und überprüft, ob die Zeit für den Level abgelaufen ist. Dies ist eine der Regeln des Spiels: beim Zeit abläuft, und alle Ziele nicht eingestellt wurde haben, ist es über.
*  Ist die Zeit abgelaufen, legt die Methode den Spielzustand **TimeExpired** fest und kehrt zur **Update**-Methode im vorherigen Code zurück.
*  Ist die Zeit noch nicht abgelaufen, wird für den Bewegungs- und Blickrichtungscontroller eine aktualisierte Kameraposition abgefragt. Genauer gesagt: eine Aktualisierung des Blickwinkels – ausgehend von der Kameraebene (Blickrichtung des Spielers) – und des Abstands, um den sich der Blickwinkel seit der letzten Controllerabfrage bewegt hat.
*  Die Kamera wird auf der Grundlage der neuen Daten des Bewegungs- und Blickrichtungscontrollers aktualisiert.
*  Die Dynamik (also die Animationen und das Verhalten von Objekten in der Spielwelt, die nicht vom Spieler gesteuert werden) wird aktualisiert. In diesem Beispiel spielen die [ __UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) Methode wird aufgerufen, um die Bewegung der Ammo aneinandergereihter Kreise, die die Animation Säule Schwierigkeit und das Verschieben der Ziele ausgelöst wurden haben zu aktualisieren. Weitere Informationen finden Sie unter [aktualisieren Sie das Spiel](#update-the-game-world).
*  Die Methode überprüft, ob die Kriterien für den erfolgreichen Abschluss eines Levels erfüllt sind. Ist dies der Fall, wird der endgültige Punktestand für das Level festgelegt und überprüft, ob es sich um das letzte Level (von sechs) handelt. Ist dies der Fall, gibt die Methode den Spielzustand **GameComplete** zurück. Andernfalls gibt sie den Spielzustand __LevelComplete__ zurück.
*  Wurde das Level nicht abgeschlossen, legt die Methode den Spielzustand auf __Active__ fest und springt zurück.

## <a name="update-the-game-world"></a>Aktualisieren Sie das Spiel

In diesem Beispiel, wenn das Spiel ausgeführt wird die [ __Simple3DGame::UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) Methode wird aufgerufen, von der [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)Methode (die Namen von [ __GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) zum Aktualisieren der Objekte, die in einer Spiele-Szene gerendert werden.

In der __UpdateDynamics__ Schleife, rufen Sie Methoden, die verwendet werden, um das Spiel während der Übertragung, unabhängig von der Player-Eingabe festgelegt, ein immersives Spiel zu erstellen, und stellen die Ebene stammen *aktiv*. Dazu gehören die Grafiken, die gerendert werden müssen, und laufende Animation Schleifen um zu bringen eine, pulsierender Welt auch, wenn keine Player-Eingabe vorhanden ist. Z. B. Strukturen weichen im Wind, Phasen cresting entlang Shore Linien, Mechanismen wie früher das Rauchen und Fremd Erlegen Strecken und verschieben. Hierzu gehört auch die Interaktion zwischen Objekten (beispielsweise Kollisionen zwischen der Spielerkugel und der Welt oder zwischen der Munition und den Hindernissen oder Zielen).

Die spielschleife sollten Sie stets eine regelmäßige Aktualisierung Spiel weltweit, ob er Spiellogik, physische Algorithmen basiert, oder ob es einfach ist zufällig, außer wenn das Spiel insbesondere angehalten wird. 

Im Beispielspiel wird dieses Prinzip als *Dynamik* bezeichnet. Es umfasst das Verhalten der Säulenhindernisse sowie die Bewegung und das physikalische Verhalten abgefeuerter Munitionskugeln. 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics-Methode 

Diese Methode behandelt vier Berechnungssätze:

* Die Positionen der Kugeln für die abgefeuerte Munition in der Spielwelt.
* Die Animation der Säulenhindernisse.
* Die Überschneidung des Spielers mit den Begrenzungen der Spielwelt.
* Die Kollisionen von Munitionskugeln mit den Hindernissen, den Zielen, anderen Munitionskugeln und der Spielwelt.

Bei der Animation der Hindernisse handelt es sich um eine in **Animate.h/.cpp** definierte Schleife. Das Verhalten der Ammo und alle Konflikte werden durch vereinfachte Physik Algorithmen definiert, im Code angegeben und parametrisierte durch eine Reihe von globalen Konstanten für das Spiel, einschließlich der Schwerkraft und Material-Eigenschaften. All dies wird im Koordinatenbereich der Spielwelt berechnet.

### <a name="review-the-flow"></a>Überprüfen Sie den flow

Nun, wir haben alle Objekte in der Szene aktualisiert und berechnet alle Konflikte, müssen wir diese Informationen verwenden, um die entsprechenden visuellen Änderungen zu zeichnen. 

Nach dem __GameMain::Update()__ schließt die aktuelle Iteration der die Spieleschleife, im Beispiel sofort aufgerufen __Render()__ die aktualisierten Objektdaten und generieren eine neue Szene bieten, den Player verwenden, als Hier angezeigt. Als Nächstes werfen wir einen Blick auf das Rendering.

```cpp
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
                // ...
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update(); // GameMain::Update calls Simple3DGame::RunGame() if game is in Dynamics state, uses Simple3DGame::UpdateDynamics() to update game world.
                m_renderer->Render(); //Render() is called immediately after the Update() loop
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

## <a name="render-the-game-worlds-graphics"></a>Rendern von Grafiken für das Spiel weltweit

Wir empfehlen, die Grafik in einem Spiel so oft wie möglich (im Höchstfall also bei jeder Iteration der Hauptschleife des Spiels) zu aktualisieren. Im Zuge der Schleifeniteration wird das Spiel unabhängig von einer Spielereingabe aktualisiert. Dies ermöglicht eine flüssige Darstellung der berechneten Animationen und Verhalten. Stellen Sie sich nur einmal eine einfache Szene mit Wasser vor, das sich nur bewegt, wenn der Spieler eine Taste drückt. Das Ergebnis wäre eine schrecklich langweilige Grafik. Ein gutes Spiel wird ruckelfrei und flüssig dargestellt.

Das Beispielspiel Schleife zurückrufen, siehe [ __GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Ist das Hauptfenster des Spiels sichtbar und nicht angedockt oder deaktiviert, werden weiterhin Updates für das Spiel ausgeführt und die Ergebnisse gerendert. Die [ __Rendern__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) rendert die Methode, die wir untersucht haben jetzt eine Darstellung dieses Zustands. Dies erfolgt sofort nach einem Aufruf von **aktualisieren**, wozu **RunGame** zum Updatestatus, das im vorherigen Abschnitt beschrieben wurde.

Diese Methode zeichnet die Projektion der dreidimensionalen Welt und legt anschließend das Direct2D-Overlay darüber. Nach Abschluss des Vorgangs zeigt sie die finale Swapchain mit den kombinierten Puffern an.

>[!Note]
>Es gibt zwei Zustände für das Beispielspiel Direct2D-Overlay: eine, in dem das Spiel das Spiel Info-Overlay, das die Bitmap für das Menü "Pause" und eine zeigt, in dem das Spiel zeigt das Fadenkreuz, zusammen mit der Rechtecke den Touchscreen verschieben-Informationen enthält, Controller. Der Text mit dem Spielstand wird in beiden Zuständen gezeichnet. Weitere Informationen finden Sie unter [Rendering-Framework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md).

### <a name="gamerendererrender-method"></a>GameRenderer::Render-Methode

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
   
        // ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            //...
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); // Renders the 3D objects
            }

        //...
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw(); //Start drawing the overlays
        
        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game); // Renders number of hits, shots, and time
        }

        if (m_gameInfoOverlay->Visible())
        {
            d2dContext->DrawBitmap(     // Renders the game overlay
                m_gameInfoOverlay->Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        //...
    }
}
```

## <a name="simple3dgame-object"></a>Simple3DGame-Objekt

Dies sind die Methoden und Daten, die in definierten die __Simple3DGame__ object-Klasse.

### <a name="methods"></a>Methoden

Die interne für definierten Methoden **Simple3DGame** enthalten:

-   **Initialisieren Sie**: Legt die Anfangswerte der globalen Variablen fest und initialisiert die Spielobjekte. Dies wird im behandelt die [initialisieren und starten Sie das Spiel](#initialize-and-start-the-game) Abschnitt.
-   **LoadGame**: Initialisiert ein neues Level und startet den entsprechenden Ladevorgang.
-   **LoadLevelAsync**: Eine asynchrone Aufgabe beginnt (Wenn Sie nicht mit asynchronen Aufgaben vertraut sind, finden Sie unter [Parallel Patterns Library](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) initialisieren die Ebene, und rufen Sie dann auf eine asynchrone Aufgabe für den Renderer an bestimmte Geräte auf Ressourcen zu laden. Diese Methode wird in einem gesonderten Thread ausgeführt. Daher können in diesem Thread nur [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)-Methoden (und keine [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)-Methoden) aufgerufen werden. Alle Gerätekontextmethoden werden in der **FinalizeLoadLevel**-Methode aufgerufen.
-   **FinalizeLoadLevel**: Führt alle Aktionen zum Laden des Levels aus, die im Hauptthread durchgeführt werden müssen. Dies schließt alle Aufrufe von Direct3D 11-Gerätekontextmethoden ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) ein.
-   **StartLevel**: Startet das Gameplay für ein neues Level.
-   **PauseGame**: Hält das Spiel an.
-   **RunGame**: Führt eine Iteration der Spielschleife aus. Wird jeweils einmal pro Iteration der Spielschleife von **App::Update** aufgerufen, sofern sich das Spiel im Zustand **Active** befindet.
-   **OnSuspending** und **OnResuming**: Hält die Audiowiedergabe des Spiels an bzw. setzt sie fort.

Und die privaten Methoden:

-   **LoadSavedState** und **SaveState**: Lädt bzw. speichert den aktuellen Zustand des Spiels.
-   **SaveHighScore** und **LoadHighScore**: Speichert bzw. lädt den spielübergreifenden Highscore.
-   **InitializeAmmo**: Setzt den Zustand der als Munition verwendeten Kugelobjekte in den Originalzustand für den Beginn einer neuen Runde zurück.
-   **UpdateDynamics**: Diese Methode ist sehr wichtig, da sie alle Spielobjekte auf der Grundlage vordefinierter Animationsroutinen, Physikeffekte und Steuerungseingaben aktualisiert. Hierbei handelt es sich gewissermaßen um das Herzstück der Interaktivität, die das Spiel ausmacht. Dies wird im behandelt die [aktualisieren Sie das Spiel](#update-the-game-world) Abschnitt.

Die anderen öffentlichen Methoden dienen zum Abrufen von Eigenschaften und geben Gameplay- und Overlay-spezifische Informationen an das App-Framework zurück, um diese anzuzeigen.

### <a name="data"></a>Data

Am Anfang des Codebeispiels befinden sich vier Objekte, deren Instanzen während der Ausführung der Spielschleife aktualisiert werden.

-   **MoveLookController** Objekt: Stellt die Player-Eingabe. Weitere Informationen finden Sie unter [Hinzufügen von Steuerelementen](tutorial--adding-controls.md).
-   **GameRenderer** object: Stellt die Direct3D 11-Renderer abgeleitet der **DirectXBase** -Klasse, die alle gerätespezifischen Objekte und ihre Darstellung behandelt. Weitere Informationen finden Sie unter [Rendering-Framework ich](tutorial--assembling-the-rendering-pipeline.md).
-   **Audio** Objekt: Steuert die Audiowiedergabe für das Spiel. Weitere Informationen finden Sie unter [Hinzufügen von Sounds](tutorial--adding-sound.md).

Die restlichen Spielvariablen enthalten die Listen mit den Grundtypen und die entsprechenden spielinternen Werte sowie Gameplay-spezifische Daten und Beschränkungen.

## <a name="next-steps"></a>Nächste Schritte

Jetzt sind Sie wahrscheinlich wissen, was die eigentliche Rendering-Engine: wie Aufrufe der __Rendern__ Methoden für die aktualisierte primitive erhalten auf dem Bildschirm in Pixel aktiviert. Dieser Vorgang wird in zwei Schritten beschrieben &mdash; [Rendering-Framework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md) und [Rendering-Framework II: Rendern von Spielen](tutorial-game-rendering.md). Sollten Sie sich dagegen mehr für die Aktualisierung des Spielzustands durch die Spielersteuerung interessieren, finden Sie entsprechende Informationen unter [Hinzufügen von Steuerelementen](tutorial--adding-controls.md).
