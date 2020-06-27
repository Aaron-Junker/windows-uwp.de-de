---
title: Erweitern des Beispiel Spiels
description: Erfahren Sie, wie Sie eine XAML-Überlagerung für ein UWP DirectX-Spiel implementieren.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06b52e5b6fdba1db83c941e770cd49360085accf
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409549"
---
# <a name="extend-the-sample-game"></a>Erweitern des Beispiel Spiels

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

An dieser Stelle haben wir die wichtigsten Komponenten eines grundlegenden universelle Windows-Plattform (UWP) DirectX 3D-Spiel behandelt. Sie können das Framework für ein Spiel einrichten, einschließlich der Ansichts Anbieter-und Renderingpipeline, und eine einfache Spiel Schleife implementieren. Sie können auch eine grundlegende Überlagerung der Benutzeroberfläche erstellen, Sounds integrieren und Steuerelemente implementieren. Sie sind auf dem besten Weg, ein eigenes Spiel zu erstellen, aber wenn Sie weitere Hilfe und Informationen benötigen, sehen Sie sich diese Ressourcen an.

-   [DirectX-Grafiken und -Spiele](/windows/desktop/directx)
-   [Übersicht über Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews)
-   [Referenz für Direct3D 11](/windows/desktop/direct3d11/d3d11-graphics-reference)

## <a name="using-xaml-for-the-overlay"></a>Verwenden von XAML für die Überlagerung

Eine Alternative, die wir nicht ausführlich besprochen haben, ist die Verwendung von XAML anstelle von [Direct2D](/windows/desktop/Direct2D/direct2d-portal) für die Überlagerung. XAML hat gegenüber Direct2D viele Vorteile für das Zeichnen von Elementen der Benutzeroberfläche. Der wichtigste Vorteil besteht darin, dass dadurch das Aussehen und fühlen von Windows 10 in Ihr DirectX-Spiel vereinfacht wird. Viele der allgemeinen Elemente, Stile und Verhalten, die eine UWP-App ausmachen, sind eng in das XAML-Modell integriert und ersparen einem Spieleentwickler eine Menge Arbeit bei der Implementierung. Falls Ihr eigenes Spieldesign eine komplizierte Benutzeroberfläche hat, empfiehlt sich unter Umständen die Verwendung von XAML anstelle von Direct2D.

Mit XAML können wir eine spielschnittstelle erstellen, die der Direct2D ähnelt, die zuvor erstellt wurde.

### <a name="xaml"></a>XAML
![XAML-Overlay](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![D2D-Overlay](./images/simple-dx-game-extend-d2d.PNG)

Obwohl Sie ähnliche Endergebnisse aufweisen, gibt es eine Reihe von Unterschieden zwischen der Implementierung von Direct2D-und XAML-Schnittstellen.

Funktion | XAML| Direct2D
:----------|:----------- | :-----------
Definieren von Overlay | Definiert in einer XAML-Datei, `\*.xaml` . Nachdem Sie XAML verstanden haben, werden das Erstellen und konfigurieren komplizierterer Überlagerungen im Vergleich zu Direct2D zu simpiler.| Definiert als eine Auflistung von Direct2D primitiven und [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) -Zeichen folgen, die manuell in einen Direct2D-Ziel Puffer eingefügt und geschrieben werden. 
Benutzeroberflächenelemente | XAML-Benutzeroberflächen Elemente stammen aus standardisierten Elementen, die Teil der Windows-Runtime XAML-APIs sind, darunter [**Windows:: UI:: XAML**](/uwp/api/Windows.UI.Xaml) und [**Windows:: UI:: XAML:: Controls**](/uwp/api/Windows.UI.Xaml.Controls). Der Code zum Behandeln des Verhaltens der XAML-UI-Elemente ist in der CodeBehind-Datei „Main.xaml.cpp“ definiert. | Einfache Formen können wie Rechtecke und Ellipsen gezeichnet werden.
Fenstergröße ändern | Verarbeitet natürlich die Größen Änderungs-und Ansichts Zustands Änderungs Ereignisse und wandelt das Overlay entsprechend um | Sie müssen manuell angeben, wie die Komponenten der Überlagerung neu gezeichnet werden sollen.

Ein weiterer großer Unterschied besteht in der [SwapChain](/windows/uwp/graphics-concepts/swap-chains). Sie müssen die SwapChain nicht an ein [**Windows:: UI:: Core:: corewindow**](/uwp/api/windows.ui.core.corewindow) -Objekt anfügen. Stattdessen ordnet eine DirectX-APP, die XAML integriert, eine austauschkette zu, wenn ein neues [**swapchainpanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel) -Objekt erstellt wird. 

Der folgende Code Ausschnitt zeigt, wie Sie XAML für das " **Swap** "-Element in der Datei " [**directxpage. XAML**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml) " deklarieren.
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

Das Objekt " **sexapchainpanel** " wird als [**Content**](/uwp/api/Windows.UI.Xaml.Window.Content) -Eigenschaft des aktuellen Fenster Objekts festgelegt, das [beim Start](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) von der APP-Singleton erstellt wird.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```

Wenn Sie die konfigurierte [**SwapChain an die swapchainpanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) -Instanz anfügen möchten, die von Ihrem XAML-Code definiert wird, müssen Sie einen Zeiger auf die zugrunde liegende systemeigene [**iswapchainpanelnative**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) -Schnittstellen Implementierung abrufen und [**iswapchainpanelnative:: setwapchain**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) für Sie erstellen. 

Der folgende Code Ausschnitt von [**DX::D eviceresources:: createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) erläutert dies für DirectX/XAML-Interop:

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

Wenn Sie die Version dieses Spiels herunterladen möchten, das XAML für das Overlay verwendet, wechseln Sie zum [Direct3D Shooting Sample Game (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).

Anders als die Version des in den restlichen Themen erläuterten samplingspiels definiert die XAML-Version das Framework in der [app. XAML. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) -und der [directxpage. XAML. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) -Datei anstelle von [app. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) bzw. [gameinfooverlay. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp).
