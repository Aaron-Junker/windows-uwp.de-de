---
title: Marble Maze-Anwendungsstruktur
description: Die Struktur einer DirectX-UWP-App unterscheidet sich von der einer herkömmlichen Desktopanwendung.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
ms.date: 09/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Spiele, Beispiel, Directx, Struktur
ms.localizationpriority: medium
ms.openlocfilehash: a04e6714772d9b17c281f81ad93582d1fb691c9b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368501"
---
# <a name="marble-maze-application-structure"></a>Marble Maze-Anwendungsstruktur




Die Struktur einer DirectX-UWP-App unterscheidet sich von der einer herkömmlichen Desktopanwendung. Die Windows-Runtime verwendet keine Handletypen wie z. B. [HWND](https://docs.microsoft.com/windows/desktop/WinProg/windows-data-types) und keine Funktionen wie z. B. [CreateWindow](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-createwindowa), sondern stellt Schnittstellen, z. B. [Windows::UI::Core::ICoreWindow](https://docs.microsoft.com/uwp/api/Windows.UI.Core.ICoreWindow) bereit, sodass Sie UWP-Apps auf modernere Weise und mit stärkerer Objektorientierung entwickeln können. In diesem Abschnitt der Dokumentation ist dargestellt, wie der App-code von Marble Maze strukturiert ist.

> [!NOTE]
> Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](https://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Es folgen einige wichtige Punkte, die in diesem Dokument für das Strukturieren von Spielcode erläutert werden:

-   Richten Sie in der Initialisierungsphase Laufzeit- und Bibliothekskomponenten ein, die das Spiel verwendet. Laden Sie auch spielspezifische Ressourcen.
-   Bei UWP-Apps muss der Start der Ereignisverarbeitung innerhalb von 5 Sekunden nach dem Start der App erfolgen. Laden Sie daher beim Laden Ihrer App nur wichtige Ressourcen. Spiele sollten umfangreiche Ressourcen im Hintergrund laden und einen Statusbildschirm anzeigen.
-   In der Spielschleife sollten vier Aktionen ausgeführt werden: Verarbeiten von Windows-Ereignissen, Lesen von Benutzereingaben, Aktualisieren von Szenenobjekten und Rendern der Szene.
-   Reagieren Sie mithilfe von Handlern auf Fensterereignisse. (Diese ersetzen die Fenstermeldungen in Windows-Desktopanwendungen.)
-   Verwenden Sie einen Zustandsautomaten, um den Fluss und die Reihenfolge der Spiellogik zu steuern.

##  <a name="file-organization"></a>Dateiorganisation


Einige der Komponenten in Marble Maze können mit geringfügigen oder ohne Änderungen für andere Spiele wiederverwendet werden. Sie können die Organisation und die Ideen aus diesen Dateien für Ihr eigenes Spiel anpassen. In der folgenden Tabelle sind die wichtigen Quellcodedateien kurz beschrieben.

| Dateien                                      | Beschreibung                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| App.h, App.cpp               | Definiert die **App**- und **DirectXApplicationSource**-Klassen, die die Ansicht (Fenster, Thread und Ereignisse) der App kapseln.                                                     |
| Audio.h, Audio.cpp                         | Definiert die **Audio**-Klasse, die Audioressourcen verwaltet.                                                                                                                          |
| BasicLoader.h, BasicLoader.cpp             | Definiert die **BasicLoader**-Klasse, die Dienstprogrammmethoden bereitstellt, mit denen Sie Texturen, Gitter und Shader laden können.                                                                  |
| BasicMath.h                                | Definiert Strukturen und Funktionen, mit denen Sie Vektor- und Matrixdaten und -berechnungen verwenden können. Viele dieser Funktionen sind mit HLSL-Shadertypen kompatibel.                     |
| BasicInformationen zu einigen der wichtigsten Vorgehensweisen, die Sie beim Verwenden von visuellen Ressourcen berücksichtigen sollten, finden Sie unter Writer.h, BasicInformationen zu einigen der wichtigsten Vorgehensweisen, die Sie beim Verwenden von visuellen Ressourcen berücksichtigen sollten, finden Sie unter Writer.cpp | Definiert die **BasicReaderWriter**-Klasse, die die Windows-Runtime zum Lesen und Schreiben von Dateidaten in einer UWP-App verwendet.                                                                    |
| BasicShapes.h, BasicShapes.cpp             | Definiert die **BasicShapes**-Klasse, die Dienstprogrammmethoden zum Erstellen von Grundformen wie Würfeln und Kugeln bereitstellt. (Diese Dateien werden von der Marble Maze-Implementierung nicht verwendet). |                                                                                  |
| Camera.h, Camera.cpp                       | Definiert die **Camera**-Klasse, die die Position und Ausrichtung einer Kamera bereitstellt.                                                                                               |
| Collision.h, Collision.cpp                 | Verwaltet Informationen über das Aufeinandertreffen der Murmel mit anderen Objekten, z. B. dem Labyrinth.                                                                                                       |
| DDSTextureLoader.h, DDSTextureLoader.cpp   | Definiert die **CreateDDSTextureFromMemory**-Funktion, die Texturen im DDS-Format aus einem Speicherpuffer lädt.                                                              |
| DirectXHelper.h             | Definiert die DirectX-Hilfsfunktionen, die in vielen DirectX-UWP-Apps hilfreich sind.                                                                            |
| LoadScreen.h, LoadScreen.cpp               | Definiert die **LoadScreen**-Klasse, die bei der App-Initialisierung einen Ladebildschirm anzeigt.                                                                                         |
| MarbleMazeMain.h, MarbleMazeMain.cpp               | Definiert die **MarbleMazeMain**-Klasse, die spielspezifische Ressourcen verwaltet und den Großteil der Spiellogik definiert.                                                                          |
| MediaStreamer.h, MediaStreamer.cpp         | Definiert die **MediaStreamer**-Klasse, die Media Foundation verwendet, um das Spiel beim Verwalten von Audioressourcen zu unterstützen.                                                                            |
| PersistentState.h, PersistentState.cpp     | Definiert die **PersistentState**-Klasse, die primitive Datentypen aus einem Sicherungsspeicher liest und in einen Sicherungsspeicher schreibt.                                                                      |
| Physics.h, Physics.cpp                     | Definiert die **Physics**-Klasse, die die physische Simulation zwischen der Murmel und dem Labyrinth implementiert.                                                                              |
| Primitives.h                               | Definiert geometrische Typen, die vom Spiel verwendet werden.                                                                                                                                   |
| SampleOverlay.h, SampleOverlay.cpp         | Definiert die **SampleOverlay**-Klasse, die allgemeine 2D- und Benutzeroberflächendaten und -vorgänge bereitstellt.                                                                               |
| SDKMesh.h, SDKMesh.cpp                     | Definiert die **SDKMesh**-Klasse, die Gitter lädt und rendert, die im SDK Mesh-Format (.sdkmesh) vorliegen.                                                                                |
| StepTimer.h               | Definiert die **StepTimer**-Klasse, die eine einfache Möglichkeit zum Abrufen von Gesamtzeiten und verstrichenen Zeiten bietet.
| UserInterface.h, UserInterface.cpp         | Definiert die Funktionalität, die mit der Benutzeroberfläche (z. B. dem Menüsystem und der Bestenliste) verknüpft ist.                                                                        |

 

##  <a name="design-time-versus-run-time-resource-formats"></a>Entwurfszeit- und Laufzeitressourcenformate


Verwenden Sie nach Möglichkeit Laufzeitformate anstelle von Entwurfszeitformaten, um Spielressourcen effizienter zu laden.

Ein *Entwurfszeitformat* ist das Format, das Sie verwenden, wenn Sie die Ressource entwerfen. In der Regel verwenden 3D-Designer Entwurfszeitformate. Einige Entwurfszeitformate sind auch textbasiert, um sie in jedem textbasierten Editor ändern zu können. Entwurfszeitformate können sehr ausführlich sein und mehr Informationen enthalten, als das Spiel erfordert. Ein *Laufzeitformat* ist das Binärformat, das vom Spiel gelesen wird. Laufzeitformate sind in der Regel kompakter und effizienter zu laden als die entsprechenden Entwurfszeitformate. Daher verwenden die meisten Spiele zur Laufzeit Laufzeitressourcen.

Auch wenn das Spiel ein Entwurfszeitformat direkt lesen kann, bietet die Verwendung eines separaten Laufzeitformats viele Vorteile. Da Laufzeitformate häufig kompakter sind, benötigen sie weniger Speicherplatz und weniger Zeit für die Übertragung über ein Netzwerk. Außerdem werden Laufzeitfomate häufig als im Speicher abgebildete Datenstrukturen dargestellt. Deshalb können sie viel schneller in den Speicher geladen werden als beispielsweise eine XML-basierte Textdatei. Da separate Laufzeitformate in der Regel binär codiert werden, lassen sich diese vom Endbenutzer letztendlich schwieriger ändern.

HLSL-Shader sind ein Beispiel für Ressourcen, die verschiedene Entwurfszeit- und Laufzeitformate verwenden. Marble Maze verwendet HLSL-Dateien als Entwurfszeitformat und CSO-Dateien als Laufzeitformat. Eine HLSL-Datei enthält den Shaderquellcode und eine CSO-Datei den entsprechenden Shaderbytecode. Wenn Sie HLSL-Dateien offline konvertieren und für das Spiel CSO-Dateien bereitstellen, müssen Sie die HLSL-Quelldateien beim Laden des Spiels nicht in Bytecode konvertieren.

Aus anleitungstechnischen Gründen enthält das Marble Maze-Projekt für viele Ressourcen sowohl das Entwurfszeitformat als auch das Laufzeitformat. Für ein eigenes Spiel benötigen Sie jedoch nur die Entwurfszeitformate im Quellprojekt, da Sie diese bei Bedarf in Laufzeitformate konvertieren können. In dieser Dokumentation wird gezeigt, wie die Entwurfszeitformate in Laufzeitformate konvertiert werden.

##  <a name="application-life-cycle"></a>Anwendungslebenszyklus


Marble Maze folgt dem Lebenszyklus einer typischen UWP-App. Weitere Informationen zum Lebenszyklus von UWP-Apps finden Sie unter [App-Lebenszyklus](https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle).

Ein UWP-Spiel initialisiert in der Regel Laufzeitkomponenten wie Direct3D, Direct2D und alle von ihm verwendeten Eingabe-, Audio- oder Physikbibliotheken. Es lädt auch spielspezifische Ressourcen, die vor dem Starten des Spiels erforderlich sind. Diese Initialisierung erfolgt einmalig während einer Spielsitzung.

Nach der Initialisierung führen Spiele in der Regel die *Spielschleife* aus. In dieser Schleife führen Spiele in der Regel vier Aktionen aus: Verarbeiten von Windows-Ereignissen, Sammeln von Eingaben, Aktualisieren von Szenenobjekten und Rendern der Szene. Wenn das Spiel die Szene aktualisiert, kann es den aktuellen Eingabezustand für die Szenenobjekte übernehmen und physische Ereignisse wie das Aufeinandertreffen von Objekten simulieren. Das Spiel kann auch andere Aktivitäten wie die Wiedergabe von Soundeffekten oder das Senden von Daten über das Netzwerk ausführen. Wenn das Spiel die Szene rendert, zeichnet es den aktuellen Zustand der Szene auf und zeichnet es auf das Anzeigegerät. In den nachfolgenden Abschnitten sind diese Aktivitäten ausführlicher beschrieben.

##  <a name="adding-to-the-template"></a>Ergänzen der Vorlage


Die **DirectX 11-App (Universal Windows)** -Vorlage erstellt ein Kernfenster, in dem Sie mit Direct3D rendern können. Die Vorlage enthält auch die **DeviceResources**-Klasse, die alle zum Rendern von 3D-Inhalten in einer UWP-App erforderlichen Direct3D-Geräteressourcen erstellt.

Die **App**-Klasse erstellt das **MarbleMazeMain**-Klassenobjekt, startet das Laden von Ressourcen, führt eine Schleife zum Upgrade des Timers aus und ruft die **MarbleMazeMain::Render**-Methode für jeden Frame auf. Die **App::OnWindowSizeChanged**, **App::OnDpiChanged** und **App::OnOrientationChanged**-Methoden rufen die **MarbleMazeMain::CreateWindowSizeDependentResources** -Methode auf, und die **Implementierungscode**-Methode ruft die **MarbleMazeMain::Update** und **MarbleMazeMain::Render**-Methoden auf.

Das folgende Beispiel zeigt, an welcher Stelle der **App::SetWindow**-Methode das **MarbleMazeMain**-Klassenobjekt erstellt. Die **DeviceResources**-Klasse wird an die Methode übergeben, sodass sie die Direct3D-Objekte zum Rendern verwenden kann.

```cpp
    m_main = std::unique_ptr<MarbleMazeMain>(new MarbleMazeMain(m_deviceResources));
```

Die **App**-Klasse startet auch das Laden der verzögerten Ressourcen für das Spiel. Ausführliche Informationen dazu finden Sie im nächsten Abschnitt.

Darüber hinaus richtet die **App**-Klasse die Ereignishandler für die [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)-Ereignisse ein. Wenn die Handler für diese Ereignisse aufgerufen werden, übergeben sie die Eingabe an die **MarbleMazeMain**-Klasse.

## <a name="loading-game-assets-in-the-background"></a>Laden von Spielressourcen im Hintergrund


Damit das Spiel innerhalb von 5 Sekunden nach dem Starten auf Fensterereignisse reagieren kann, wird empfohlen, die Spielressourcen asynchron oder im Hintergrund zu laden. Während die Objekte im Hintergrund geladen werden, kann das Spiel auf Fensterereignisse reagieren.

> [!NOTE]
> Sie können auch das Hauptmenü anzeigen, wenn es bereit ist, und den übrigen Ressourcen erlauben, den Ladevorgang im Hintergrund fortzusetzen. Wenn der Benutzer eine Menüoption auswählt, bevor alle Ressourcen geladen wurden, kann beispielsweise durch Anzeigen einer Statusleiste angegeben werden, dass Szenenressourcen weiter geladen werden.

 

Selbst wenn das Spiel relativ wenige Spielressourcen enthält, empfiehlt es sich aus zwei Gründen, diese asynchron zu laden. Zum einen ist es schwierig, sicherzustellen, dass alle Ressourcen auf allen Geräten und in allen Konfigurationen schnell geladen werden. Zum anderen ist der Code bei frühzeitiger Integration des asynchronen Ladens zum Skalieren bereit, wenn Sie Funktionalitäten hinzufügen.

Das asynchrone Laden von Ressourcen beginnt mit der **App::Load**-Methode. Diese Methode verwendet die [Aufgaben](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class)-Klasse, um Spielressourcen im Hintergrund zu laden.

```cpp
    task<void>([=]()
    {
        m_main->LoadDeferredResources(true, false);
    });
```

Die **MarbleMazeMain** -Klasse definiert die *m\_DeferredResourcesReady* Flags an, dass der asynchrone Ladevorgang abgeschlossen ist. Die **MarbleMazeMain::LoadDeferredResources**-Methode lädt die Spielressourcen und legt anschließend dieses Kennzeichen fest. Während der Aktualisierung(**MarbleMazeMain::Update**) und des Renderns (**MarbleMazeMain::Render**) der App wird dieses Kennzeichen überprüft. Ist dieses Flag festgelegt, wird das Spiel normal fortgesetzt. Wurde das Flag noch nicht festgelegt, zeigt das Spiel den Ladebildschirm an.

Weitere Informationen zur asynchronen Programmierung für UWP-Apps finden Sie unter [Asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

> [!TIP]
> Sollten Sie z. B. einen Spielcode schreiben, der Teil einer Windows-Runtime-C++-Bibliothek (d. h. eine DLL) ist, können Sie im Abschnitt [Erstellen asynchroner Vorgänge in C++ für UWP-Apps](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps) lesen, wie asynchrone Vorgänge erstellt werden, die von Apps und anderen Bibliotheken genutzt werden können.

 

## <a name="the-game-loop"></a>Die Spielschleife


Die **App::Run**-Methode führt die Hauptspielschleife (**MarbleMazeMain::Update**) aus. Diese Methode wird für jeden Frame aufgerufen.

Um den Ansichts- und Fenstercode vom spielspezifischen Code zu trennen, haben wir die **App::Run**-Methode zum Weiterleiten von Aktualisierungs- und Renderaufrufen an das **MarbleMazeMain**-Objekt implementiert.

Im folgenden Beispiel ist die **App::Run**-Methode dargestellt, die die Hauptspielschleife enthält. Die Spielschleife aktualisiert die Gesamtdauer- und Bilddauervariablen und aktualisiert und rendert anschließend die Szene. Dadurch wird auch sichergestellt, dass Inhalt nur gerendert wird, wenn das Fenster sichtbar ist.

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }

    // The app is exiting so do the same thing as if the app were being suspended.
    m_main->OnSuspending();

#ifdef _DEBUG
    // Dump debug info when exiting.
    DumpD3DDebug();
#endif //_DEGBUG
}
```

## <a name="the-state-machine"></a>Der Zustandsautomat


Spiele enthalten in der Regel einen *Zustandsautomaten* (auch als *finiter Zustandsautomat* oder FSM bezeichnet), um den Fluss und die Reihenfolge der Spiellogik zu steuern. Ein Zustandsautomat enthält eine bestimmte Anzahl von Zuständen und die Fähigkeit, zwischen diesen zu wechseln. Ein Zustandsautomat wird normalerweise von einem *Ausgangszustand* aus gestartet, wechselt zu einem oder mehreren *Zwischenzuständen* und wird möglicherweise in einem *abschließenden Zustand* beendet.

Eine Spielschleife verwendet häufig einen Zustandsautomaten, damit sie die Logik ausführen kann, die für den aktuellen Spielzustand spezifisch ist. Marble Maze definiert die **GameState**-Enumeration, die jeden möglichen Zustand des Spiels definiert.

```cpp
enum class GameState
{
    Initial,
    MainMenu,
    HighScoreDisplay,
    PreGameCountdown,
    InGameActive,
    InGamePaused,
    PostGameResults,
};
```

Der **MainMenu**-Zustand definiert beispielsweise, dass das Hauptmenü angezeigt wird und das Spiel nicht aktiv ist. Der **InGameActive**-Zustand definiert dagegen, dass das Spiel aktiv ist und das Menü nicht angezeigt wird. Die **MarbleMazeMain** -Klasse definiert die **m\_GameState** Membervariable, die den aktiven Status des spielen enthalten soll.

Die Methoden **MarbleMazeMain::Update** und **MarbleMazeMain::Render** verwenden die switch-Anweisung, um die Logik für den aktuellen Zustand auszuführen. Das folgende Beispiel zeigt, wie diese switch-Anweisung für die **MarbleMazeMain::Update**-Methode aussehen könnte (zur besseren Veranschaulichung der Struktur wurden Einzelheiten ausgespart).

```cpp
switch (m_gameState)
{
case GameState::MainMenu:
    // Do something with the main menu. 
    break;

case GameState::HighScoreDisplay:
    // Do something with the high-score table. 
    break;

case GameState::PostGameResults:
    // Do something with the game results. 
    break;

case GameState::InGamePaused:
    // Handle the paused state. 
    break;
}
```

Hängt die Spiellogik oder das Rendering von einem bestimmten Spielzustand ab, ist dies in dieser Dokumentation hervorgehoben.

## <a name="handling-app-and-window-events"></a>Behandlung von App- und Fensterereignissen


Die Windows-Runtime stellt ein objektorientiertes System zur Ereignisbehandlung bereit, damit Sie Windows-Meldungen leichter verwalten können. Sie müssen einen Ereignishandler oder eine Ereignisbehandlungsmethode bereitstellen, die auf das Ereignis reagiert, um ein Ereignis in einer Anwendung zu nutzen. Sie müssen den Ereignishandler außerdem bei der Ereignisquelle registrieren. Dieser Prozess wird oft als Ereignisverknüpfung bezeichnet.

### <a name="supporting-suspend-resume-and-restart"></a>Unterstützen des Anhaltens, Fortsetzens und Neustartens

Marble Maze wird angehalten, wenn der Benutzer aus dem Spiel herauswechselt oder Windows in den Energiesparmodus versetzt wird. Das Spiel wird fortgesetzt, wenn der Benutzer es in den Vordergrund bringt oder der Stromsparmodus für Windows beendet wird. Im Allgemeinen werden Apps nicht geschlossen. Die Apps kann von Windows beendet werden, wenn sich diese im angehaltenen Zustand befindet und Windows die von der App verwendeten Ressourcen (z. B. den Arbeitsspeicher) benötigt. Eine App wird von Windows benachrichtigt, wenn diese gerade angehalten oder fortgesetzt wird, sie wird jedoch nicht benachrichtigt, wenn sie gerade beendet wird. Daher muss die App – ab dem Zeitpunkt, an dem sie von Windows benachrichtigt wird, dass sie gerade angehalten wird – alle Daten speichern können, die erforderlich wären, um den aktuellen Benutzerzustand wiederherzustellen, wenn die App neu gestartet wird. Verfügt die App über einen signifikanten Benutzerzustand, der einen hohen Speicheraufwand erfordert, kann zudem ein regelmäßiges Speichern des Zustands erforderlich sein, noch bevor die App die Anhaltebenachrichtigung empfängt. Marble Maze reagiert aus zwei Gründen auf Anhalte- und Fortsetzungsbenachrichtigungen:

1.  Wird die App angehalten, speichert das Spiel den aktuellen Spielzustand und hält die Audiowiedergabe an. Wird die App fortgesetzt, setzt das Spiel die Audiowiedergabe fort.
2.  Wird die App geschlossen und später neu gestartet, wird das Spiel ab dem vorherigen Zustand fortgesetzt.

Marble Maze führt zum Anhalten und Fortsetzen folgende Aufgaben aus:

-   An Schlüsselpunkten des Spiels, beispielsweise dann, wenn der Benutzer einen Kontrollpunkt erreicht, speichert es den Zustand im beständigen Speicher.
-   Es reagiert auf Anhaltebenachrichtigungen, indem es seinen Zustand dauerhaft speichert.
-   Es reagiert auf Fortsetzungsbenachrichtigungen, indem es seinen Zustand aus dem beständigen Speicher lädt. Es lädt den vorherigen Zustand auch während des Starts.

Marble Maze definiert die **PersistentState**-Klasse, um das Anhalten und Fortsetzen zu unterstützen. (Siehe **PersistentState.h** und **PersistentState.cpp**). Diese Klasse verwendet die [Windows::Foundation::Collections::IPropertySet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet)-Schnittstelle, um Eigenschaften zu lesen und zu schreiben. Die **PersistentState**-Klasse stellt Methoden bereit, die primitive Datentypen (z. B. **Bool**, **Int**, **Float**, [XMFLOAT3](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat3) und [Platform::String](https://docs.microsoft.com/cpp/cppcx/platform-string-class)) aus einem Sicherungsspeicher lesen und in einen Sicherungsspeicher schreiben.

```cpp
ref class PersistentState
{
internal:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ settingsValues,
        _In_ Platform::String^ key
        );

    void SaveBool(Platform::String^ key, bool value);
    void SaveInt32(Platform::String^ key, int value);
    void SaveSingle(Platform::String^ key, float value);
    void SaveXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 value);
    void SaveString(Platform::String^ key, Platform::String^ string);

    bool LoadBool(Platform::String^ key, bool defaultValue);
    int  LoadInt32(Platform::String^ key, int defaultValue);
    float LoadSingle(Platform::String^ key, float defaultValue);

    DirectX::XMFLOAT3 LoadXMFLOAT3(
        Platform::String^ key, 
        DirectX::XMFLOAT3 defaultValue);

    Platform::String^ LoadString(
        Platform::String^ key, 
        Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

Die **MarbleMazeMain**-Klasse enthält ein **PersistentState**-Objekt. Der **MarbleMazeMain**-Konstruktor initialisiert dieses Objekt und stellt den lokalen Anwendungsdatenspeicher als Sicherungsdatenspeicher bereit.

```cpp
m_persistentState = ref new PersistentState();

m_persistentState->Initialize(
    Windows::Storage::ApplicationData::Current->LocalSettings->Values,
    "MarbleMaze");
```

Marble Maze speichert den Zustand, wenn die Murmel einen Prüfpunkt oder das Ziel passiert (in der **MarbleMazeMain::Update**-Methode) und das Fenster den Fokus verliert (in der **MarbleMazeMain::OnFocusChange**-Methode). Enthält das Spiel eine große Menge an Zustandsdaten, empfiehlt es sich, den Zustand auf ähnliche Weise gelegentlich im beständigen Speicher zu speichern, da Sie nur einige Sekunden Zeit haben, um auf die Anhaltebenachrichtigung zu reagieren. Empfängt die App eine Anhaltebenachrichtigung, muss sie daher nur die Zustandsdaten speichern, die geändert wurden.

Zum Reagieren auf Anhalte- und Fortsetzungsbenachrichtigungen definiert die **MarbleMazeMain**-Klasse die **SaveState**- und **LoadState**-Methoden, die beim Anhalten und Fortsetzen aufgerufen werden. Die **MarbleMazeMain::OnSuspending**-Methode behandelt das Anhalteereignis und die **MarbleMazeMain::OnResuming**-Methode das Fortsetzungsereignis.

Die **MarbleMazeMain::OnSuspending**-Methode speichert den Spielzustand und hält das Audio an.

```cpp
void MarbleMazeMain::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

Die **MarbleMazeMain::SaveState**-Methode speichert Spielzustandswerte wie die aktuelle Position und Geschwindigkeit der Murmel, den letzten Prüfpunkt und die Bestenliste.

```cpp
void MarbleMazeMain::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());

    m_persistentState->SaveSingle(
        ":ElapsedTime", 
        m_inGameStopwatchTimer.GetElapsedTime());

    m_persistentState->SaveInt32(":GameState", static_cast<int>(m_gameState));
    m_persistentState->SaveInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    int i = 0;
    HighScoreEntries entries = m_highScoreTable.GetEntries();
    const int bufferLength = 16;
    char16 str[bufferLength];

    m_persistentState->SaveInt32(":ScoreCount", static_cast<int>(entries.size()));

    for (auto iter = entries.begin(); iter != entries.end(); ++iter)
    {
        int len = swprintf_s(str, bufferLength, L"%d", i++);
        Platform::String^ string = ref new Platform::String(str, len);

        m_persistentState->SaveSingle(
            Platform::String::Concat(":ScoreTime", string), 
            iter->elapsedTime);

        m_persistentState->SaveString(
            Platform::String::Concat(":ScoreTag", string), 
            iter->tag);
    }
}
```

Beim Fortsetzen des Spiels muss nur das Audio fortgesetzt werden. Es ist kein Laden des Zustands aus dem beständigen Speicher erforderlich, da der Zustand bereits im Arbeitsspeicher geladen ist.

Wie das Spiel ein Audio anhält und fortsetzt, wird im Dokument [Hinzufügen von Audio zum Beispielspiel Marble Maze](adding-audio-to-the-marble-maze-sample.md) erläutert.

Zur Unterstützung des Neustarts ruft der beim Start aufgerufene **MarbleMazeMain::Initialize**-Konstruktor die **MarbleMazeMain::LoadState**-Methode auf. Die **MarbleMazeMain::LoadState**-Methode liest den Zustand in den Spielobjekten und übernimmt diesen. Diese Methode legt außerdem den aktuellen Spielzustand als angehalten fest, wenn das Spiel angehalten wurde oder aktiv war, als es angehalten wurde. Das Spiel wird angehalten, sodass der Benutzer nicht von unerwarteten Aktivitäten überrascht wird. Sie wechselt auch zum Hauptmenü, wenn sich das Spiel nicht in einem Wiedergabezustand befand, als es angehalten wurde.

```cpp
void MarbleMazeMain::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(
        ":Position", 
        m_physics.GetPosition());

    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(
        ":Velocity", 
        m_physics.GetVelocity());

    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(
        ":GameState", 
        static_cast<int>(m_gameState));

    int currentCheckpoint = m_persistentState->LoadInt32(
        ":Checkpoint", 
        static_cast<int>(m_currentCheckpoint));

    switch (static_cast<GameState>(gameState))
    {
    case GameState::Initial:
        break;

    case GameState::MainMenu:
    case GameState::HighScoreDisplay:
    case GameState::PreGameCountdown:
    case GameState::PostGameResults:
        SetGameState(GameState::MainMenu);
        break;

    case GameState::InGameActive:
    case GameState::InGamePaused:
        m_inGameStopwatchTimer.SetVisible(true);
        m_inGameStopwatchTimer.SetElapsedTime(elapsedTime);
        m_physics.SetPosition(position);
        m_physics.SetVelocity(velocity);
        m_currentCheckpoint = currentCheckpoint;
        SetGameState(GameState::InGamePaused);
        break;
    }

    int count = m_persistentState->LoadInt32(":ScoreCount", 0);

    const int bufferLength = 16;
    char16 str[bufferLength];

    for (int i = 0; i < count; i++)
    {
        HighScoreEntry entry;
        int len = swprintf_s(str, bufferLength, L"%d", i);
        Platform::String^ string = ref new Platform::String(str, len);

        entry.elapsedTime = m_persistentState->LoadSingle(
            Platform::String::Concat(":ScoreTime", string), 
            0.0f);

        entry.tag = m_persistentState->LoadString(
            Platform::String::Concat(":ScoreTag", string), 
            L"");

        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> [!IMPORTANT]
> Marble Maze unterscheidet nicht zwischen Kaltstart – d. h. einem erstmaligen Start ohne vorheriges Anhalteereignis – und dem Fortsetzen vom angehaltenen Zustand aus. Dies ist der empfohlene Entwurf für alle UWP-Apps.

Weitere Informationen zu Anwendungsdaten finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

##  <a name="next-steps"></a>Nächste Schritte


Informationen zu einigen der wichtigsten Vorgehensweisen, die Sie beim Verwenden von visuellen Ressourcen berücksichtigen sollten, finden Sie unter [Hinzufügen von visuellem Inhalt zum Beispielspiel Marble Maze](adding-visual-content-to-the-marble-maze-sample.md) .

## <a name="related-topics"></a>Verwandte Themen

* [Hinzufügen von visuellem Inhalt zum Marble Maze-Beispiel](adding-visual-content-to-the-marble-maze-sample.md)
* [Marble Maze-Beispiel-Grundlagen](marble-maze-sample-fundamentals.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




