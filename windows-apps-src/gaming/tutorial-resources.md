---
title: Erweitern des Spielbeispiels
description: Hier erfahren Sie, wie Sie eine XAML-Überlagerung für ein UWP-DirectX-Spiel implementieren.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7cb1c9f9cf6cbc6cce0c5d4547ed503bb9a06e56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660115"
---
# <a name="extend-the-game-sample"></a>Erweitern des Spielbeispiels

Sie sind nun mit den Schlüsselkomponenten eines einfachen dreidimensionalen DirectX 3D-Spiels für die Universelle Windows-Plattform (UWP) vertraut. Sie können das Framework für ein Spiel (einschließlich Ansichtsanbieter und Rendering-Pipeline) einrichten und eine einfache Spielschleife implementieren. Zudem können Sie ein einfaches Benutzeroberflächenoverlay erstellen und Soundeffekte und Steuerelemente einbauen. Sie sind auf dem Weg, Ihre eigenes Spiel zu erstellen. Wenn Sie weitere Hilfe und Informationen benötigen, sehen Sie sich diese Ressourcen an.

-   [DirectX-Grafiken und -Spiele](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Direct3D 11-Übersicht](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Direct3D 11-Referenz](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="using-xaml-for-the-overlay"></a>Verwenden von XAML für die Überlagerung


Eine Alternative, auf die wir noch nicht näher eingegangen sind, ist die Verwendung von XAML anstelle von [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) für das Overlay. XAML hat gegenüber Direct2D viele Vorteile, insbesondere für die Zeichnung der Elemente der Benutzeroberfläche. Der wichtigste Vorteil ist, dass es macht das Aussehen und Verhalten von Windows 10 in Ihrer DirectX-Spielen, die einfacher zu integrieren. Viele der allgemeinen Elemente, Stile und Verhalten, die eine UWP-App ausmachen, sind eng in das XAML-Modell integriert und ersparen einem Spieleentwickler eine Menge Arbeit bei der Implementierung. Falls Ihr eigenes Spieldesign eine komplizierte Benutzeroberfläche hat, empfiehlt sich unter Umständen die Verwendung von XAML anstelle von Direct2D.

Mit XAML können wir eine Spielschnittstelle erstellen, die der vorher in Direct2D erstellten ähnelt.

### <a name="xaml"></a>XAML
![XAML-Überlagerung](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![D2D-Überlagerung](./images/simple-dx-game-extend-d2d.PNG)

Während sie ähnliche Ergebnisse erhalten, gibt es eine Reihe von Unterschieden zwischen dem Implementieren von Direct2D- und XAML-Schnittstellen.

Feature | XAML| Direct2D
:----------|:----------- | :-----------
Definieren der Überlagerung | In einer XAML-Datei definiert `\*.xaml`. Wenn Sie XAML-Code verstehen, wird das Erstellen und Konfigurieren von komplizierteren Overlays einfacher gegenüber Direct2D.| Sie definieren das Overlay als Sammlung von Direct2D-Grundtypen und [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)-Zeichenfolgen, die manuell platziert und in einen Direct2D-Zielpuffer geschrieben werden. 
Benutzeroberflächenelemente | Die XAML UI-Elemente basieren auf standardisierten Elementen, die Teil der XAML-APIs der Windows-Runtime sind. Hierzu gehören unter anderem [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) und [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716). Der Code zum Behandeln des Verhaltens der XAML-UI-Elemente ist in der CodeBehind-Datei „Main.xaml.cpp“ definiert. | Einfache Formen können z. B. wie Rechtecke und Ellipsen gezeichnet werden.
Größenanpassung der Fenster | Behandelt Größenänderungen und Statusänderungsereignisse, transformiert dabei das Overlay entsprechend | Es muss manuell angegeben werden, wie die Overlaykomponenten neu gezeichnet werden muss


Ein weiterer wichtiger Unterschied ist die [Swapkette](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains). Die Swapchain muss nicht direkt mit einem [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)-Objekt verknüpft werden. Stattdessen ordnet eine DirectX-App mit XAML eine Swapchain zu, wenn ein neues [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel)-Objekt konstruiert wird. 

Der folgende Codeausschnitt zeigt, wie Sie XAML-Code für das **SwapChainPanel** in der [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml)-Datei deklarieren.
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">


    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

Das **SwapChainPanel**-Objekt wird als [**Content**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content)-Eigenschaft des aktuellen Fensterobjekts festgelegt, das [beim Start](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51)  durch das Singleton erstellt wurde

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


Um die konfigurierte Swapchain der [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)-Bereichsinstanz zuzuordnen, die Sie mit XAML definiert haben, müssen Sie einen Zeiger auf die zugrunde liegende Implementierung der systemeigenen [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/dn302143)-Schnittstelle abrufen und in einem entsprechenden Aufruf von [**ISwapChainPanelNative::SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) Ihre konfigurierte Swapchain übergeben. 

Der folgende Codeausschnitt aus [**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) gibt Details für DirectX/XAML-Interoperabilität an:

```cpp
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

Weitere Informationen zu diesem Prozess finden Sie unter [Interoperabilität von DirectX und XAML](directx-and-xaml-interop.md).

## <a name="sample"></a>Beispiel

Eine Version des Spiels, die XAML für das Overlay verwendet, finden Sie in [Beispiel für einen Direct3D-Shooter (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).


(Im Gegensatz zur in den anderen Themen behandelten Version des Spielbeispiels definiert die XAML-Version ihr Framework nicht in den Dateien [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) und [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp), sondern in den Dateien [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) und [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp).)