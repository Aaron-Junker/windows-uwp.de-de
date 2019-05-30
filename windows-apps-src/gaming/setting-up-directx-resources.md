---
title: Einrichten von DirectX-Ressourcen und Darstellen eines Bilds
description: Hier zeigen wir Ihnen das Erstellen eines Direct3D-Geräts, einer Swapchain und einer Renderzielansicht sowie die Darstellung des gerenderten Bilds auf dem Bildschirm.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX, Ressourcen, Bilder
ms.localizationpriority: medium
ms.openlocfilehash: 4e0fd2b5f34c17e39def107f72568916ca3ba6fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368002"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>Einrichten von DirectX-Ressourcen und Darstellen eines Bilds



Hier zeigen wir Ihnen das Erstellen eines Direct3D-Geräts, einer Swapchain und einer Renderzielansicht sowie die Darstellung des gerenderten Bilds auf dem Bildschirm.

**Ziel:** DirectX-Ressourcen in einer C++ Universal Windows Platform (UWP)-app einrichten und eine Volltonfarbe anzuzeigen.

## <a name="prerequisites"></a>Vorraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

**Zeit in Anspruch:** 20 Minuten.

## <a name="instructions"></a>Anweisungen

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. Deklarieren von Variablen von Direct3D-Schnittstelle mit comptr-Objekt

Wir deklarieren Direct3D-Schnittstellenvariablen mit der [intelligenten Zeigervorlage](https://docs.microsoft.com/cpp/cpp/smart-pointers-modern-cpp) ComPtr aus der C++-Vorlagenbibliothek für Windows-Runtime (Windows Runtime C++ Template Library, WRL), damit wir die Lebensdauer dieser Variablen ausnahmesicher verwalten können. Mithilfe dieser Variablen können wir auf die [**ComPtr class**](https://docs.microsoft.com/cpp/windows/comptr-class) und ihre Member zugreifen. Zum Beispiel:

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Wenn Sie deklarieren [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) mit ComPtr, können Sie dann die ComPtr **"getaddressof"** -Methode zum Abrufen der Adresse des Zeigers auf den  **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView) Übergabe an [ **ID3D11DeviceContext::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets). **OMSetRenderTargets** bindet das Renderziel an den [Ausgabezusammenführungsstatus](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage), um das Renderziel als Ausgabeziel anzugeben.

Nachdem die Beispiel-App gestartet wurde, wird sie initialisiert und geladen. Danach kann sie ausgeführt werden.

### <a name="2-creating-the-direct3d-device"></a>2. Erstellen des Direct3D-Geräts

Für die Verwendung der Direct3D-API zum Rendern einer Szene müssen wir zunächst ein Direct3D-Gerät erstellen, das die Grafikkarte darstellt. Zum Erstellen des Direct3D-Geräts rufen wir die [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)-Funktion auf. Geben wir Ebenen 9.1 über 11.1 im Array der [ **D3D\_FEATURE\_Ebene** ](https://docs.microsoft.com/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) Werte. Direct3D durchläuft das Array der Reihe nach und gibt die höchste Ebene des unterstützten Features zurück. Rufen Sie die höchste Funktionsebene zeigen wir Ihnen also der **D3D\_FEATURE\_Ebene** Einträge vom höchsten zum niedrigsten array. Wir übergeben die [ **D3D11\_erstellen\_Gerät\_BGRA\_Unterstützung** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) flag, das die *Flags* Parameter an Stellen Direct3D-Ressourcen zusammenarbeiten mit Direct2D. Wenn wir den Debugbuild verwenden, übergeben wir auch die [ **D3D11\_erstellen\_Gerät\_DEBUGGEN** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) Flag. Weitere Informationen zum Debuggen von Apps finden Sie unter [Verwenden der Debugschicht zum Debuggen von Apps](https://docs.microsoft.com/windows/desktop/direct3d11/using-the-debug-layer-to-test-apps).

Wir rufen das Direct3D 11.1-Gerät ([**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)) und den Gerätekontext ([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) durch Abfragen des Direct3D 11-Geräts und Gerätekontexts ab, die von [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) zurückgegeben werden.

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. Erstellen der SwapChain

Im nächsten Schritt erstellen wir eine Swapchain, die von dem Gerät zum Rendern und für die Anzeige verwendet wird. Wir deklarieren und initialisieren eine [ **DXGI\_SWAP\_Kette\_DESC1** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) Struktur, um die SwapChain zu beschreiben. Dann wir die SwapChain als Flip-Modell eingerichtet (, also eine SwapChain, die die [ **DXGI\_SWAP\_Auswirkung\_KIPPEN\_SEQUENZIELL** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) Wert festgelegt wird, die **SwapEffect** Member) und legen Sie die **Format** Member [ **DXGI\_FORMAT\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format). Legen wir die **Anzahl** Mitglied der [ **DXGI\_Beispiel\_DESC** ](https://docs.microsoft.com/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) Datenstruktur, die **SampleDesc** Member Gibt an, dass 1 und der **Qualität** Mitglied **DXGI\_Beispiel\_DESC** auf NULL, da Flip-Modell mehrere Beispiel Antialiasing (MSAA) nicht unterstützt. Wir legen den **BufferCount**-Member auf 2 fest, sodass die Swapchain für die Darstellung auf dem Anzeigegerät einen Frontpuffer sowie einen Hintergrundpuffer verwenden kann, der als Renderziel dient.

Wir rufen das zugrunde liegende DXGI-Gerät durch Abfragen des Direct3D 11.1-Geräts ab. Zur Minimierung des Stromverbrauchs – einem wichtigen Aspekt für batteriebetriebene Geräte wie Laptops und Tablet-PCs – rufen wir die [**IDXGIDevice1::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency)-Methode mit dem Wert 1 auf. Dieser Wert ist die maximale Anzahl an Hintergrundpufferframes, die von DXGI in die Warteschlange verschoben werden können. Dadurch wird sichergestellt, dass die App erst nach der vertikalen Austastung gerendert wird.

Für die endgültige Erstellung der Swapchain müssen wir die übergeordnete Factory vom DXGI-Gerät abrufen. Zum Abrufen des Adapters für das Gerät rufen wir [**IDXGIDevice::GetAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) auf. Anschließend rufen wir auf dem Adapter [**IDXGIObject::GetParent**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiobject-getparent) auf, um die übergeordnete Factory ([**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2)) zu erhalten. Zum Erstellen der Swapchain rufen wir [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) mit der Swapchainbeschreibung und dem Kernfenster der App auf.

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. Erstellen der Ansicht des Renderziels

Zum Rendern von Grafik in dem Fenster müssen wir eine Renderziel-Ansicht erstellen. Wir rufen [**IDXGISwapChain::GetBuffer**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-getbuffer) zum Abrufen des Swapchain-Hintergrundpuffers auf, der beim Erstellen der Renderziel-Ansicht verwendet werden soll. Wir geben den Hintergrundpuffer als 2D-Textur an ([**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)). Zum Erstellen der Renderziel-Ansicht rufen wir [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) mit dem Hintergrundpuffer der Swapchain auf. Wir müssen angeben, um für das gesamte Core Fenster gezeichnet werden soll, unter Angabe des anzeigen ([**D3D11\_VIEWPORT**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_viewport)) als die vollständige Größe des Hintergrundpuffers der Swapkette. Wir verwenden den Viewport in einem Aufruf von [**ID3D11DeviceContext::RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports), um den Viewport an die [Rasterprogrammstufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) der Pipeline zu binden. In der Rasterprogrammstufe werden Vektorinformationen in ein Rasterbild konvertiert. In diesem Fall ist keine Konvertierung erforderlich, da wir nur eine Volltonfarbe anzeigen.

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. Das gerenderte Bild darstellen

Wir rufen eine Endlosschleife auf, um die Szene fortlaufend zu rendern und anzuzeigen.

In dieser Schleife wird Folgendes aufgerufen:

1.  [**ID3D11DeviceContext::OMSetRenderTargets** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) des Renderziels als Ausgabeziel angeben.
2.  [**ID3D11DeviceContext::ClearRenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) das Renderziel auf eine Volltonfarbe zu löschen.
3.  [**IDXGISwapChain::Present** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) das gerenderte Bild an das Fenster zu präsentieren.

Da wir zuvor die maximale Framelatenz auf 1 festgelegt haben, reduziert Windows die Geschwindigkeit der Renderschleife generell auf die Bildschirmaktualisierungsrate, die normalerweise etwa 60 Hz beträgt. Windows reduziert die Geschwindigkeit der Renderschleife, indem die App in den Ruhezustand versetzt wird, wenn sie [**Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) aufruft. Windows versetzt die App so lange in den Ruhezustand, bis die Bildschirmanzeige aktualisiert wird.

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. Ändern der Größe des app-Fensters und der Swapkette Puffer

Wenn sich die Größe des App-Fensters ändert, muss die App die Größe der Swapchainpuffer ändern, die Renderziel-Ansicht neu erstellen und anschließend das gerenderte Bild in der geänderten Größe darstellen. Zum Ändern der Größe der Swapchainpuffer rufen wir [**IDXGISwapChain::ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) auf. In diesem Aufruf lassen wir die Anzahl der Puffer und das Format der Puffer nicht geändert (die *BufferCount* zwei Parameter und die *NewFormat* Parameter [ **DXGI\_ Formatieren Sie\_B8G8R8A8\_UNORM**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)). Wir legen für die Größe des Swapchain-Hintergrundpuffers denselben Wert wie für das Fenster mit geänderter Größe fest. Nachdem wir die Größe des Swapchainpuffers geändert haben, erstellen wir das neue Renderziel, und wir stellen das neue gerenderte Bild genauso wie beim Initialisieren der App dar.

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte


Wir haben ein Direct3D-Gerät, eine Swapchain und eine Renderziel-Ansicht erstellt und das gerenderte Bild auf dem Bildschirm dargestellt.

Als Nächstes zeichnen wir ein Dreieck auf dem Bildschirm.

[Erstellen von Shadern, und zeichnen primitive Typen](creating-shaders-and-drawing-primitives.md)

 

 




