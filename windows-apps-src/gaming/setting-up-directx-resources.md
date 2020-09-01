---
title: Einrichten von DirectX-Ressourcen und Darstellen eines Bilds
description: Hier zeigen wir Ihnen das Erstellen eines Direct3D-Geräts, einer Swapchain und einer Renderzielansicht sowie die Darstellung des gerenderten Bilds auf dem Bildschirm.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, Ressourcen, Bilder
ms.localizationpriority: medium
ms.openlocfilehash: cced3b5cb6ad9c3e1ffe077887c5f23ce95745bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159194"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>Einrichten von DirectX-Ressourcen und Darstellen eines Bilds



Hier zeigen wir Ihnen das Erstellen eines Direct3D-Geräts, einer Swapchain und einer Renderzielansicht sowie die Darstellung des gerenderten Bilds auf dem Bildschirm.

**Ziel:** Sie richten DirectX-Ressourcen in einer UWP-App (Universelle Windows-Plattform) mit C++ ein und zeigen eine Volltonfarbe an.

## <a name="prerequisites"></a>Voraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

**Zeitaufwand:** 20 Minuten.

## <a name="instructions"></a>Instructions

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. Deklarieren von Direct3D-Schnittstellenvariablen mit ComPtr

Wir deklarieren Direct3D-Schnittstellenvariablen mit der [intelligenten Zeigervorlage](/cpp/cpp/smart-pointers-modern-cpp) ComPtr aus der C++-Vorlagenbibliothek für Windows-Runtime (Windows Runtime C++ Template Library, WRL), damit wir die Lebensdauer dieser Variablen ausnahmesicher verwalten können. Mithilfe dieser Variablen können wir auf die [**ComPtr class**](/cpp/windows/comptr-class) und ihre Member zugreifen. Beispiel:

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Wenn Sie [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) mit comptr deklarieren, können Sie die **getaddressof** -Methode von comptr verwenden, um die Adresse des Zeigers auf **ID3D11RenderTargetView** (ID3D11RenderTargetView) zu erhalten, die \* \* an [**Verknüpfung id3d11devicecontext aus:: omstrendertargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)übergeben werden soll. **OMSetRenderTargets** bindet das Renderziel an den [Ausgabezusammenführungsstatus](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage), um das Renderziel als Ausgabeziel anzugeben.

Nachdem die Beispiel-App gestartet wurde, wird sie initialisiert und geladen. Danach kann sie ausgeführt werden.

### <a name="2-creating-the-direct3d-device"></a>2. Erstellen des Direct3D-Geräts

Für die Verwendung der Direct3D-API zum Rendern einer Szene müssen wir zunächst ein Direct3D-Gerät erstellen, das die Grafikkarte darstellt. Zum Erstellen des Direct3D-Geräts rufen wir die [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)-Funktion auf. Wir geben die Ebenen 9,1 bis 11,1 im Array von [**D3D- \_ Funktions \_ Ebenen**](/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) -Werten an. Direct3D durchläuft das Array der Reihe nach und gibt die höchste Ebene des unterstützten Features zurück. Um die höchste verfügbare Featureebene zu erhalten, werden die Array Einträge der **D3D \_ \_ Featureebene** von oben nach unten aufgelistet. Wir übergeben das Flag [**D3D11 \_ Create \_ Device \_ BGRA \_ Support**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) an den *Flags* -Parameter, damit Direct3D-Ressourcen mit Direct2D zusammenarbeiten. Bei Verwendung des Debugbuilds wird auch das [**D3D11 \_ Create \_ Device \_ Debug**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) -Flag übergeben. Weitere Informationen zum Debuggen von apps finden [Sie unter Verwenden der Debug-Ebene zum Debuggen von apps](/windows/desktop/direct3d11/using-the-debug-layer-to-test-apps).

Wir rufen das Direct3D 11,1-Gerät ([**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)) und den Gerätekontext ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) ab, indem wir das Gerät und den Gerätekontext von D3D11CreateDevice Abfragen, die von [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)zurückgegeben werden.

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

### <a name="3-creating-the-swap-chain"></a>3. Erstellen der Swapchain

Im nächsten Schritt erstellen wir eine Swapchain, die von dem Gerät zum Rendern und für die Anzeige verwendet wird. Wir deklarieren und initialisieren eine [**DXGI- \_ \_ SwapChain \_ DESC1**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) -Struktur, um die SwapChain zu beschreiben. Dann richten Sie die SwapChain als Flip-Model ein (d. h. eine Swapkette, die den [**DXGI-Swap-Effekt hat, den \_ \_ \_ \_ sequenziellen Wert kippen**](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) , der im **SwapEffect** -Member festgelegt ist) und den **formatmember** auf [**DXGI- \_ Format \_ B8G8R8A8 \_ unorm**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)festlegen. Wir legen das **count** -Element der [**DXGI \_ - \_ beispieldesc**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) -Struktur fest, das der **SampleDesc** -Member auf 1 festlegt, und der **Quality** -Member von **DXGI \_ Sample \_ DESC** auf 0 (null), da Flip-Model mehrere Sample Antialiasing (MSAA) nicht unterstützt. Wir legen den **BufferCount**-Member auf 2 fest, sodass die Swapchain für die Darstellung auf dem Anzeigegerät einen Frontpuffer sowie einen Hintergrundpuffer verwenden kann, der als Renderziel dient.

Wir rufen das zugrunde liegende DXGI-Gerät durch Abfragen des Direct3D 11.1-Geräts ab. Zur Minimierung des Stromverbrauchs – einem wichtigen Aspekt für batteriebetriebene Geräte wie Laptops und Tablet-PCs – rufen wir die [**IDXGIDevice1::SetMaximumFrameLatency**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency)-Methode mit dem Wert 1 auf. Dieser Wert ist die maximale Anzahl an Hintergrundpufferframes, die von DXGI in die Warteschlange verschoben werden können. Dadurch wird sichergestellt, dass die App erst nach der vertikalen Austastung gerendert wird.

Für die endgültige Erstellung der Swapchain müssen wir die übergeordnete Factory vom DXGI-Gerät abrufen. Zum Abrufen des Adapters für das Gerät rufen wir [**IDXGIDevice::GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) auf. Anschließend rufen wir auf dem Adapter [**IDXGIObject::GetParent**](/windows/desktop/api/dxgi/nf-dxgi-idxgiobject-getparent) auf, um die übergeordnete Factory ([**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2)) zu erhalten. Zum Erstellen der Swapchain rufen wir [**IDXGIFactory2::CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) mit der Swapchainbeschreibung und dem Kernfenster der App auf.

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

### <a name="4-creating-the-render-target-view"></a>4. Erstellen der Renderziel-Ansicht

Zum Rendern von Grafik in dem Fenster müssen wir eine Renderziel-Ansicht erstellen. Wir rufen [**IDXGISwapChain::GetBuffer**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-getbuffer) zum Abrufen des Swapchain-Hintergrundpuffers auf, der beim Erstellen der Renderziel-Ansicht verwendet werden soll. Wir geben den Hintergrundpuffer als 2D-Textur an ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)). Zum Erstellen der Renderziel-Ansicht rufen wir [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) mit dem Hintergrundpuffer der Swapchain auf. Sie müssen angeben, dass das gesamte Kern Fenster gezeichnet werden soll, indem Sie den Ansichts Anschluss ([**D3D11 \_ Viewport**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_viewport)) als vollständige Größe des Hintergrund Puffers der Austausch Kette angeben. Wir verwenden den Viewport in einem Aufruf von [**ID3D11DeviceContext::RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports), um den Viewport an die [Rasterprogrammstufe](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) der Pipeline zu binden. In der Rasterprogrammstufe werden Vektorinformationen in ein Rasterbild konvertiert. In diesem Fall ist keine Konvertierung erforderlich, da wir nur eine Volltonfarbe anzeigen.

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

### <a name="5-presenting-the-rendered-image"></a>5. Darstellen des gerenderten Bilds

Wir rufen eine Endlosschleife auf, um die Szene fortlaufend zu rendern und anzuzeigen.

In dieser Schleife wird Folgendes aufgerufen:

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets), um das Renderziel als Ausgabeziel anzugeben.
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview), um das Renderziel mit einer Volltonfarbe anzuzeigen.
3.  [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present), um das gerenderte Bild im Fenster darzustellen.

Da wir zuvor die maximale Framelatenz auf 1 festgelegt haben, reduziert Windows die Geschwindigkeit der Renderschleife generell auf die Bildschirmaktualisierungsrate, die normalerweise etwa 60 Hz beträgt. Windows reduziert die Geschwindigkeit der Renderschleife, indem die App in den Ruhezustand versetzt wird, wenn sie [**Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) aufruft. Windows versetzt die App so lange in den Ruhezustand, bis die Bildschirmanzeige aktualisiert wird.

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

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. Ändern der Größe des App-Fensters und des Swapchainpuffers

Wenn sich die Größe des App-Fensters ändert, muss die App die Größe der Swapchainpuffer ändern, die Renderziel-Ansicht neu erstellen und anschließend das gerenderte Bild in der geänderten Größe darstellen. Zum Ändern der Größe der Swapchainpuffer rufen wir [**IDXGISwapChain::ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) auf. In diesem Befehl wird die Anzahl der Puffer und das Format der Puffer unverändert belassen (der *BUFFERCOUNT* -Parameter ist auf zwei und der *newformat* -Parameter im [**DXGI- \_ Format \_ B8G8R8A8 \_ unorm**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)). Wir legen für die Größe des Swapchain-Hintergrundpuffers denselben Wert wie für das Fenster mit geänderter Größe fest. Nachdem wir die Größe des Swapchainpuffers geändert haben, erstellen wir das neue Renderziel, und wir stellen das neue gerenderte Bild genauso wie beim Initialisieren der App dar.

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

[Erstellen von Shadern und Zeichnen von Primitiven](creating-shaders-and-drawing-primitives.md)

 

 