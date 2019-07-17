---
title: Portieren der Spielschleife
description: In diesem Thema wird veranschaulicht, wie Sie ein Fenster für ein UWP-Spiel (Universelle Windows-Plattform) implementieren und die Spielschleife portieren. Außerdem wird die Erstellung eines IFrameworkView-Elements zum Steuern eines CoreWindow-Vollbilds erläutert.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, portieren, Spielschleife, Direct3D 9, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: 9b3a18d9ee63a2ecded07f8b779195d5274b6210
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141832"
---
# <a name="port-the-game-loop"></a>Portieren der Spielschleife



**Zusammenfassung**

-   [Teil 1: Initialisieren von Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Teil 2: Konvertieren Sie die Rendering-framework](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Teil 3: Portieren der Spielschleife


In diesem Thema wird veranschaulicht, wie Sie ein Fenster für ein UWP-Spiel (Universelle Windows-Plattform) implementieren und die Spielschleife übertragen. Außerdem wird die Erstellung eines [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)-Elements zum Steuern eines [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)-Vollbilds erläutert. Teil 3 der exemplarischen Vorgehensweise [Portieren einer einfachen Direct3D 9-App zu DirectX 11 und UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) .

## <a name="create-a-window"></a>Erstellen eines Fensters


Zum Einrichten eines Desktopfensters mit einem Direct3D 9-Viewport musste das herkömmliche Fensterframework für Desktop-Apps implementiert werden. Es musste ein HWND-Element erstellt, die Fenstergröße festgelegt, ein Rückruf zur Fensterverarbeitung eingerichtet, die Sichtbarkeit hergestellt werden usw.

Dagegen verfügt die UWP-Umgebung über ein deutlich einfacheres System. Anstatt ein herkömmliches Fenster einzurichten, wird von einem Microsoft Store-Spiel, für das DirectX verwendet wird, das [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)-Element implementiert. Diese Schnittstelle ist für DirectX-Apps und -Spiele vorhanden, um die direkte Ausführung in einem [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) innerhalb des App-Containers zu ermöglichen.

> **Beachten Sie**    Windows stellt verwaltete Zeiger auf Ressourcen wie das Quellobjekt für die Anwendung und die [ **"corewindow"** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow). Finden Sie unter [ **Handle für Objekt (^)** ](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx).

 

Muss die "main"-Klasse erben [ **"iframeworkview"** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) und implementieren Sie die fünf **"iframeworkview"** Methoden: [**Initialisieren Sie**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize), [ **"SetWindow"** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [ **Load**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load), [ **Ausführen** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run), und [ **deinitialisieren**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize). Zusätzlich zur Erstellung des **IFrameworkView**-Elements, in dem Ihr Spiel (im Wesentlichen) enthalten ist, müssen Sie eine Factoryklasse implementieren, über die eine Instanz des **IFrameworkView**-Elements erstellt wird. Das Spiel verfügt weiterhin über eine ausführbare Datei mit einer **main()** -Methode. Mithilfe von "main" kann jedoch lediglich die Factory verwendet werden, um die **IFrameworkView**-Instanz zu erstellen.

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

Die Spielschleife befindet sich in der [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run)-Methode (anstatt **main()** ), weil die Funktionen des Spiels über die [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)-Klasse abgewickelt werden.

Anstatt ein Framework zur Behandlung von Meldungen zu implementieren und [**PeekMessage**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-peekmessagea) aufzurufen, können wir die [**ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents)-Methode aufrufen, die in das [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)-Element des App-Fensters integriert ist. Es ist nicht erforderlich, dass die Spielschleife über Verzweigungen verfügt und Meldungen behandelt. Rufen Sie einfach **ProcessEvents** auf, und fahren Sie fort.

Spielschleife im Direct3D 11-Microsoft Store-Spiel

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

Finden Sie unter den folgenden Artikeln der detaillierten Microsoft Store-Spiele-Entwicklung:

-   [Exemplarische Vorgehensweise: eine einfache UWP-Spiel mit DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Audio für Spiele](working-with-audio-in-your-directx-game.md)
-   [Move-suchen-Steuerelemente für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




