---
title: Spielablaufverwaltung
description: Erfahren Sie, wie Sie Spiel Zustände initialisieren, Ereignisse behandeln und die Spiel Aktualisierungs Schleife einrichten.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409629"
---
# <a name="game-flow-management"></a>Spielablaufverwaltung

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

Das Spiel verfügt jetzt über ein Fenster, hat einige Ereignishandler registriert und hat Objekte asynchron geladen. In diesem Thema wird die Verwendung von Spiel Zuständen, die Verwaltung bestimmter Schlüsselspiel Zustände und das Erstellen einer Update Schleife für die Game-Engine erläutert. Anschließend erfahren Sie mehr über den Benutzeroberflächen Fluss und erfahren schließlich mehr über die für ein UWP-Spiel benötigten Ereignishandler.

## <a name="game-states-used-to-manage-game-flow"></a>Zum Verwalten des spielflusses verwendete Spiel Zustände

Wir verwenden Spiel Zustände, um den Flow des Spiels zu verwalten.

Wenn das **Simple3DGameDX** -Beispiel Spiel zum ersten Mal auf einem Computer ausgeführt wird, befindet es sich in einem Zustand, in dem kein Spiel gestartet wurde. Nachfolgende Zeiten, in denen das Spiel ausgeführt wird, können in einem der folgenden Zustände vorliegen.

- Es wurde kein Spiel gestartet, oder das Spiel liegt zwischen Ebenen (das hohe Ergebnis ist NULL).
- Die Spiel Schleife wird ausgeführt und befindet sich in der Mitte einer Ebene.
- Die Spiel Schleife wird nicht ausgeführt, weil ein Spiel abgeschlossen ist (das hohe Ergebnis hat einen Wert ungleich null).

Ihr Spiel kann so viele Zustände aufweisen, wie es benötigt. Beachten Sie aber, dass Sie jederzeit beendet werden kann. Beim fortsetzen erwartet der Benutzer, dass er in dem Zustand fortgesetzt wird, in dem er sich befand, als er beendet wurde.

## <a name="game-state-management"></a>Spiele Zustands Verwaltung

Während der Spiel Initialisierung müssen Sie also den Kaltstart des Spiels unterstützen und das Spiel nach Beendigung des Spiels fortsetzen. Das **Simple3DGameDX** -Beispiel speichert immer seinen Spielzustand, um den Eindruck zu erwecken, dass er nie angehalten wurde.

Als Reaktion auf ein Suspend-Ereignis wird das-Spiel angehalten, die Ressourcen des Spiels befinden sich jedoch noch im Arbeitsspeicher. Ebenso wird das Fortsetzungs Ereignis behandelt, um sicherzustellen, dass das Beispiel Spiel in dem Zustand aufnimmt, in dem er sich befand, als er angehalten oder beendet wurde. Je nach Zustand hat der Spieler unterschiedliche Möglichkeiten. 

- Wenn das Spiel die mittlere Ebene wieder aufnimmt, wird es angehalten, und die Überlagerung bietet die Option, fortzufahren.
- Wenn das Spiel in einem Zustand fortgesetzt wird, in dem das Spiel abgeschlossen ist, werden die hohen Bewertungen und eine Option zum Spielen eines neuen Spiels angezeigt.
- Wenn das Spiel fortgesetzt wird, bevor eine Ebene begonnen hat, stellt das Overlay dem Benutzer eine Startoption zur Folge.

Das Beispiel Spiel unterscheidet nicht, ob das Spiel kalt gestartet wird, das erste Mal ohne Suspend-Ereignis gestartet wird oder ob der Status angehalten fortgesetzt wird. Dies ist der richtige Entwurf für alle UWP-apps.

In diesem Beispiel tritt die Initialisierung der Spiel Zustände in [**gamemain:: initializegamestate**](#the-gamemaininitializegamestate-method) auf (eine Gliederung dieser Methode wird im nächsten Abschnitt angezeigt).

Im folgenden finden Sie ein Flussdiagramm, das Sie bei der Visualisierung des Flows unterstützt. Es behandelt sowohl die Initialisierung als auch die Update Schleife.

- Die Initialisierung beginnt beim **Start** Knoten, wenn Sie den aktuellen Spielzustand überprüfen. Den Spiel Code finden Sie im nächsten Abschnitt unter [**gamemain:: initializegamestate**](#the-gamemaininitializegamestate-method) .
* Weitere Informationen zur Update Schleife finden Sie unter [Update Game Engine](#update-game-engine). Wechseln Sie für Game Code zu [**gamemain:: Update**](#the-gamemainupdate-method).

![Hauptzustandsautomat für unser Spiel](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>Die gamemain:: initializegamestate-Methode

Die **gamemain:: initializegamestate** -Methode wird indirekt über den Konstruktor der **gamemain** -Klasse aufgerufen, was das Ergebnis der Erstellung einer **gamemain** -Instanz in **App:: Load**ist.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>Aktualisieren der Spiel-Engine

Die **App:: Run** -Methode ruft **gamemain:: Run**auf. In **gamemain:: Run** ist ein grundlegender Zustands Automat, der alle wichtigen Aktionen verarbeitet, die ein Benutzer ausführen kann. Die höchste Stufe dieses Zustands Automaten umfasst das Laden eines Spiels, das Abspielen einer bestimmten Ebene oder das Fortsetzen einer Ebene, nachdem das Spiel angehalten wurde (vom System oder vom Benutzer).

Im Beispiel Spiel gibt es drei Haupt Zustände (dargestellt durch die **updateenginestate** -Enum), in denen sich das Spiel befinden kann.

1. **Updateenginestate:: waitingforresources**. Die Spielschleife wird durchlaufen, und ein Übergang ist erst möglich, wenn Ressourcen (insbesondere Grafikressourcen) verfügbar sind. Wenn die asynchronen Tasks zum Laden von Ressourcen ausgeführt werden, wird der Status auf **updateenginestate:: resourcesloaded**aktualisiert. Dies geschieht normalerweise zwischen Ebenen, wenn die Ebene neue Ressourcen von einem Datenträger, von einem Spielserver oder von einem Cloud-Back-End lädt. Im Beispiel Spiel simulieren wir dieses Verhalten, da das Beispiel zu diesem Zeitpunkt keine zusätzlichen Ressourcen pro Ebene benötigt.
2. **Updateenginestate:: waitingforpress**. Die Spielschleife wird durchlaufen, bis eine bestimmte Benutzereingabe erfolgt. Diese Eingabe ist eine Player Aktion zum Laden eines Spiels, zum Starten einer Ebene oder zum Fortsetzen einer Ebene. Der Beispielcode bezieht sich auf diese Unterzustände über die **pressresultstate** -Enumeration.
3. **Updateenginestate::D ynamics**. Die Spielschleife wird ausgeführt, und der Spieler spielt. Während der Benutzer spielt, prüft das Spiel drei Bedingungen, auf die es übertragen werden kann: 
 - **Gamestate:: timeabgelauf.** Ablauf des Zeitlimits für eine Ebene.
 - **Gamestate:: levelcomplete**. Abschluss einer Ebene durch den Player.
 - **Gamestate:: gamecomplete**. Abschluss aller Ebenen durch den Player.

Ein Spiel ist einfach ein Zustands Automat, der mehrere kleinere Zustands Automaten enthält. Jeder bestimmte Zustand muss mit sehr spezifischen Kriterien definiert werden. Übergänge von einem Zustand in einen anderen müssen auf diskreten Benutzereingaben oder System Aktionen basieren (z. b. das Laden von Grafik Ressourcen).

Berücksichtigen Sie beim Planen Ihres Spiels den gesamten Spielfluss, um sicherzustellen, dass Sie alle möglichen Aktionen adressiert haben, die der Benutzer oder das System ausführen kann. Ein Spiel kann sehr kompliziert sein, sodass ein Zustands Automat ein leistungsfähiges Tool ist, mit dem Sie diese Komplexität visualisieren und besser verwalten können.

Werfen wir einen Blick auf den Code für die Update-Schleife.

### <a name="the-gamemainupdate-method"></a>Die gamemain:: Update-Methode

Dies ist die Struktur des Zustands Automaten, der zum Aktualisieren der Spiel-Engine verwendet wird.

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

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
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>Aktualisieren der Benutzeroberfläche

Wir müssen festlegen, dass der Spieler den Zustand des Systems anbehält, und den Spielzustand abhängig von den Aktionen des Players und den Regeln, die das Spiel definieren, ändern können. Viele Spiele, einschließlich dieses Beispiel Spiels, verwenden häufig Benutzeroberflächenelemente (UI), um diese Informationen dem Player zu präsentieren. Die Benutzeroberfläche enthält Darstellungen des Spiel Zustands und anderer Wiedergabe spezifischer Informationen wie z. b. Score, Ammo oder die Anzahl der verbleibenden Chancen. Die Benutzeroberfläche wird auch als Overlay bezeichnet, da Sie getrennt von der Haupt Grafik Pipeline gerendert und oberhalb der 3D-Projektion platziert wird.

Einige Informationen zur Benutzeroberfläche werden auch als Heads-Up-Anzeige (HUD) präsentiert, um dem Benutzer zu ermöglichen, diese Informationen anzuzeigen, ohne dass die Augen vollständig vom Hauptmenü Bereich entfernt werden. Im Beispiel Spiel erstellen wir dieses Overlay mithilfe der Direct2D-APIs. Alternativ können wir dieses Overlay mithilfe von XAML erstellen, das wir unter [Erweitern des Beispiel Spiels](tutorial-resources.md)erörtern.

Die Benutzeroberfläche besteht aus zwei Komponenten.

- Die HUD, die das Ergebnis und Informationen zum aktuellen Zustand des Spiels enthält.
- Die Bitmap für den Pausenmodus – ein schwarzes Rechteck mit Text, das im angehaltenen/unterbrochenen Zustand des Spiels im Vordergrund angezeigt wird. Dies ist die Spielüberlagerung. Ausführlichere Informationen hierzu finden Sie unter [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md).

Wie nicht anders zu erwarten, besitzt auch das Overlay einen Zustandsautomaten. Das Overlay kann eine Ebene "Start" oder eine "Game Over"-Meldung anzeigen. Es handelt sich im Grunde um eine Canvas, in der wir alle Informationen über den Spielzustand ausgeben können, die dem Player angezeigt werden sollen, während das Spiel angehalten oder angehalten wird.

Der gerenderte Overlay kann je nach Status des Spiels einen der folgenden sechs Bildschirme aufweisen.

1. Der Bildschirm zum Laden des Status der Ressource am Anfang des Spiels.
2. Bildschirm für die Spielstatistik.
3. Bildschirm zum Starten der Ebene starten.
4. Game-Over-Bildschirm, wenn alle Ebenen abgeschlossen sind, ohne dass die Zeit abgelaufen ist.
5. Game-Over-Bildschirm, wenn die Zeit abgelaufen ist.
6. Menü Bildschirm anhalten.

Wenn Sie Ihre Benutzeroberfläche von der Grafik Pipeline Ihres Spiels trennen, können Sie unabhängig von der Grafik Rendering-Engine des Spiels daran arbeiten und die Komplexität des Spielcodes erheblich verringern.

Im folgenden wird erläutert, wie das Beispiel Spiel den Zustands Automat der Überlagerung strukturiert.

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>Ereignis Behandlung

Wie im Thema [Definieren des UWP-App-Frameworks des Spiels](tutorial--building-the-games-uwp-app-framework.md) erläutert, registrieren viele der Ansichts Anbieter Methoden der **App** -Klasse Ereignishandler. Diese Methoden müssen diese wichtigen Ereignisse ordnungsgemäß behandeln, bevor wir Spielmechanismen hinzufügen oder die Grafik Entwicklung starten.

Die richtige Behandlung der fraglichen Ereignisse ist für die UWP-App-Erfahrung von grundlegender Bedeutung. Da eine UWP-App jederzeit aktiviert, deaktiviert, geändert, angedockt, nicht angedockt, angehalten oder fortgesetzt werden kann, muss das Spiel für diese Ereignisse so bald wie möglich registriert werden und Sie so behandeln, dass die Benutzer Arbeit reibungslos und vorhersagbar ist.

Dies sind die Ereignishandler, die in diesem Beispiel verwendet werden, und die Ereignisse, die Sie behandeln.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ereignishandler</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">Behandelt <a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>coreapplicationview:: aktiviert</strong></a>. Die Spiele-App befindet sich im Vordergrund, weshalb das Hauptfenster aktiviert ist.</td>
</tr>
<tr class="even">
<td align="left">Ondpichanged</td>
<td align="left">Behandelt <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Grafiken::D isplay::D isplayinformation::D pichanged</strong></a>. Der dpi-Abschnitt der Anzeige hat sich geändert, und das Spiel passt seine Ressourcen entsprechend an.
<div class="alert">
<strong>Hinweis</strong> <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>corewindow</strong></a> -Koordinaten befinden sich in geräteunabhängigen Pixeln (Dips) für <a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>. Daher müssen Sie Direct2D über die DPI-Änderung informieren, damit die 2D-Ressourcen oder -Grundtypen korrekt angezeigt werden.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Behandelt <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Grafiken::D isplay::D isplayinformation:: orientationChanged</strong></a>. Die Ausrichtung der Anzeige Änderungen und des Renderings muss aktualisiert werden.</td>
</tr>
<tr class="even">
<td align="left">Ondisplaycontentsinvalisinvalidieren</td>
<td align="left">Behandelt <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Grafiken::D isplay::D isplayinformation::D isplaycontentsinvalidieren</strong></a>. Die Anzeige muss neu gezeichnet werden, und das Spiel muss erneut gerendert werden.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Behandelt <a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>coreapplication:: resuming</strong></a>. Die Spiele-App stellt das Spiel aus einem Anhaltezustand wieder her.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Behandelt <a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>coreapplication:: suspendieren</strong></a>. Die Spiele-App speichert den eigenen Zustand auf einem Datenträger. Der Speichervorgang für den Zustand darf maximal fünf Sekunden dauern.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Behandelt <a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>corewindow:: visibilitychanged</strong></a>. Die Sichtbarkeit der Spiele-App hat sich geändert: Die App wurde entweder sichtbar, oder sie wurde durch eine andere sichtbar gewordene App unsichtbar.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Behandelt <a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>corewindow:: aktiviert</strong></a>. Das Hauptfenster der Spiele-App wurde deaktiviert oder aktiviert, weshalb der Fokus entfernt und das Spiel angehalten oder der Fokus wiedererlangt werden muss. In beiden Fällen gibt das Overlay an, dass das Spiel angehalten wurde.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Behandelt <a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>corewindow:: Closed</strong></a>. Die Spiele-App schließt das Hauptfenster und hält das Spiel an.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Behandelt <a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>corewindow:: SizeChanged</strong></a>. Die Spiele-App ordnet die Grafikressourcen und das Overlay neu zu, um die Größenänderung umzusetzen, und aktualisiert anschließend das Renderziel.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema haben wir gesehen, wie der gesamte Spielfluss mithilfe von Spiel Zuständen verwaltet wird, und dass ein Spiel aus mehreren verschiedenen Zustands Computern besteht. Außerdem haben wir gesehen, wie Sie die Benutzeroberfläche aktualisieren und wichtige App-Ereignishandler verwalten. Nun sind wir bereit, die Renderingschleife, das Spiel und seine Mechanismen kennenzulernen.
 
Sie können die restlichen Themen durchlaufen, in denen dieses Spiel in beliebiger Reihenfolge dokumentiert wird.

- [Definieren des Hauptobjekts für das Spiel](tutorial--defining-the-main-game-loop.md)
- [Renderingframework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md)
- [Renderingframework II: Spiel Rendering](tutorial-game-rendering.md)
- [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md)
- [Steuerelemente hinzufügen](tutorial--adding-controls.md)
- [Hinzufügen von Sound](tutorial--adding-sound.md)
