---
title: Portieren der Spielschleife
description: In diesem Thema wird veranschaulicht, wie Sie ein Fenster für ein UWP-Spiel (Universelle Windows-Plattform) implementieren und die Spielschleife portieren. Außerdem wird die Erstellung eines IFrameworkView-Elements zum Steuern eines CoreWindow-Vollbilds erläutert.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Porting, Game Loop, Direct3D 9, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: e62b7e6576ff1b39cbeba2c201f929952abd4105
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159094"
---
# <a name="port-the-game-loop"></a>Portieren der Spielschleife



**Zusammenfassung**

-   [Teil 1: Initialisieren von Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Teil 2: Konvertieren des Renderingframeworks](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Teil 3: Portieren der Spielschleife


In diesem Thema wird veranschaulicht, wie Sie ein Fenster für ein UWP-Spiel (Universelle Windows-Plattform) implementieren und die Spielschleife übertragen. Außerdem wird die Erstellung eines [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)-Elements zum Steuern eines [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Vollbilds erläutert. Teil 3 der exemplarischen Vorgehensweise [Portieren einer einfachen Direct3D 9-App zu DirectX 11 und UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) .

## <a name="create-a-window"></a>Erstellen eines Fensters


Zum Einrichten eines Desktopfensters mit einem Direct3D 9-Viewport musste das herkömmliche Fensterframework für Desktop-Apps implementiert werden. Es musste ein HWND-Element erstellt, die Fenstergröße festgelegt, ein Rückruf zur Fensterverarbeitung eingerichtet, die Sichtbarkeit hergestellt werden usw.

Dagegen verfügt die UWP-Umgebung über ein deutlich einfacheres System. Anstatt ein herkömmliches Fenster einzurichten, implementiert ein Microsoft Store Spiel, das DirectX verwendet, [**iframeworkview**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView). Diese Schnittstelle ist für DirectX-Apps und -Spiele vorhanden, um die direkte Ausführung in einem [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) innerhalb des App-Containers zu ermöglichen.

> **Hinweis**    Windows bietet verwaltete Zeiger auf Ressourcen, z. b. das Quell Anwendungs Objekt und das [**corewindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Objekt. Siehe [**handle to Object Operator (^)**](/cpp/extensions/handle-to-object-operator-hat-cpp-component-extensions).

 

Ihre main-Klasse muss von [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) erben und die fünf **IFrameworkView**-Methoden implementieren: [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize), [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load), [**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) und [**Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize). Zusätzlich zur Erstellung des **IFrameworkView**-Elements, in dem Ihr Spiel (im Wesentlichen) enthalten ist, müssen Sie eine Factoryklasse implementieren, über die eine Instanz des **IFrameworkView**-Elements erstellt wird. Das Spiel verfügt weiterhin über eine ausführbare Datei mit einer **main()**-Methode. Mithilfe von "main" kann jedoch lediglich die Factory verwendet werden, um die **IFrameworkView**-Instanz zu erstellen.

main-Funktion

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

IFrameworkView-Factory

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>Portieren der Spielschleife


Sehen wir uns die Spielschleife unserer Direct3D 9-Implementierung an. Dieser Code ist in der main-Funktion der App vorhanden. Mit jeder Iteration dieser Schleife wird eine Fenstermeldung verarbeitet oder ein Frame gerendert.

Spielschleife im Direct3D 9-Desktopspiel

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

Die Spielschleife in der UWP-Version unseres Spiels ist ähnlich, dabei jedoch einfacher aufgebaut:

Die Spielschleife befindet sich in der [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)-Methode (anstatt **main()**), weil die Funktionen des Spiels über die [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)-Klasse abgewickelt werden.

Anstatt ein Framework zur Behandlung von Meldungen zu implementieren und [**PeekMessage**](/windows/desktop/api/winuser/nf-winuser-peekmessagea) aufzurufen, können wir die [**ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents)-Methode aufrufen, die in das [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)-Element des App-Fensters integriert ist. Es ist nicht erforderlich, dass die Spielschleife über Verzweigungen verfügt und Meldungen behandelt. Rufen Sie einfach **ProcessEvents** auf, und fahren Sie fort.

Spiele Schleife in Direct3D 11 Microsoft Store Spiel

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

Nun verfügen wir über eine UWP-App, von der die gleiche grundlegende Grafikinfrastruktur eingerichtet und der gleiche farbige Würfel wie im DirectX 9-Beispiel gerendert wird.

## <a name="where-do-i-go-from-here"></a>Wie geht es weiter?


Richten Sie ein Lesezeichen für [DirectX 11-Portierung – Häufig gestellte Fragen](directx-porting-faq.md) ein.

Die DirectX-UWP-Vorlagen enthalten eine stabile Direct3D-Geräteinfrastruktur, die bereit für die Nutzung mit Ihrem UWP-Spiel ist. Eine Anleitung zum Auswählen der richtigen Vorlage finden Sie unter [Erstellen eines DirectX-Spieleprojekts aus einer Vorlage](user-interface.md).

Besuchen Sie die folgenden ausführlichen Microsoft Store Spiele Entwicklungs Artikel:

-   [Exemplarische Vorgehensweise: Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Audio für Spiele](working-with-audio-in-your-directx-game.md)
-   [Verschiebungs Steuerelemente für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 