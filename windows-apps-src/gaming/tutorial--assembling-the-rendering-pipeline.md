---
title: Einführung in das Rendering
description: Hier erfahren Sie, wie Sie die Renderingpipeline zum Anzeigen von Grafiken zusammenstellen. Einführung in das Rendering.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Rendern
ms.localizationpriority: medium
ms.openlocfilehash: 4c16f1fbb55374b1d04c9fc9f5f7eae72ad19b00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604855"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Rendering-Framework I: Einführung in das Rendering

Mittlerweile wissen Sie, wie ein Spiel für die universelle Windows-Plattform (UWP) aufgebaut sein muss, damit es verwendet werden kann, und wie Sie einen Zustandsautomaten zum Behandeln des Spielablaufs definieren. Jetzt erfahren Sie, wie Sie die Rendering-Framework zusammenstellen. Sehen wir uns, wie das Beispielspiel auf die game-Szene mit Direct3D 11 (bekannt als DirectX 11) gerendert wird.

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

Direct3D 11 enthält eine Reihe von APIs, die Zugriff auf die erweiterten Funktionen von High-End-Grafik Hardware bereitstellen, die zum Erstellen von 3D-Grafiken für Grafik-intensive Anwendungen wie z. B. Spiele verwendet werden.

Das Rendern von Spielegrafiken auf dem Bildschirm ist ein Rendern einer Frame-Sequenz auf dem Bildschirm. In jeder einzelnen Frame werden Objekte gerendert, die basierend auf der Ansicht in der Szene angezeigt werden. 

Um ein Frame zu rendern, müssen Sie die für die Szene erforderlichen Informationen an die Hardware übergeben, damit sie auf dem Bildschirm angezeigt werden können. Wenn etwas auf dem Bildschirm angezeigt werden soll, müssen Sie das Rendering starten, sobald das Spiel startet.

## <a name="objective"></a>Ziel

Um ein einfaches Rendering-Framework einzurichten, und die Grafikausgabe für ein UWP-DirectX-Spiel anzuzeigen können Sie diese lose in drei Schritten gruppieren.

 1. Einrichten einer Verbindung zur Grafikschnittstelle
 2. Erstellen der Ressourcen, die zum Zeichnen der Grafiken benötigt werden
 3. Anzeigen die Grafiken: Rendern des Frames

In diesem Artikel wird erläutert, wie Grafiken mit Schritt 1 bis 3 gerendert werden.

[Rendering-Framework II: Rendern von Spielen](tutorial-game-rendering.md) deckt Schritt 2; einrichten, der Rendering-Framework und wie die Daten vorbereitet sind, vor dem Rendering kann der Fall sein.

## <a name="get-started"></a>Beginnen

Bevor Sie beginnen, sollten Sie sich mit den grundlegenden Grafik- und Rendering-Konzepten vertraut machen. Wenn Sie noch nie mit Direct3D und Rendering gearbeitet haben, finden Sie unter [Begriffe und Konzepte](#terms-and-concepts) eine kurze Beschreibung der Grafik- und Rendering-Terminologie, die in diesem Artikel verwendet werden.

Für dieses Spiel stellt das __GameRenderer__-Klassenobjekt den Renderer für dieses Beispielspiel dar.  Er ist verantwortlich für das Erstellen und Verwalten alle Direct3D 11- und Direct2D-Objekte, die verwendet, um die visuellen Spielelemente generieren.  Er enthält auch einen Verweis auf das __Simple3DGame__-Objekt zum Abrufen einer Liste von Objekten und dem Status des Spiels für die HUD-Anzeige. 

In diesem Teil des Lernprogramms konzentrieren wir uns auf das Rendering von 3D-Objekten im Spiel.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Einrichten einer Verbindung zur Grafikschnittstelle

Um auf die Hardware für das Rendern zuzugreifen, finden Sie im UWP-Framework-Artikel unter [__App::Initialize__](tutorial--building-the-games-uwp-app-framework.md#appinitialize-method) weitere Informationen.

Die __stellen\_freigegebene Funktion__Siehe [unten](#appinitialize-method), dient zum Erstellen einer __freigegebenen\_Ptr__ zu [ __DX::D EviceResources__](#dxdeviceresources), Zugriff auf das Gerät bereit. 

In Direct3D 11 dient ein [Gerät](#device) zum Zuordnen und zerstören von Objekten, um Grundtypen zu rendern und mit der Grafikkarte über die Grafiktreiber zu kommunizieren.

### <a name="appinitialize-method"></a>App::Initialize-Methode

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    //...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Anzeigen die Grafiken: Rendern des Frames

Die Spieleszene muss gerendert werden, wenn das Spiel gestartet wird. Die Anweisungen für das Rendern beginnen in der [__GameMain::Run__](#gamemainrun-method)-Methode, wie unten dargestellt.

Der einfache Fluss ist:
1. __Update__
2. __Rendern__
3. __Vorhanden__

### <a name="gamemainrun-method"></a>GameMain::Run-Methode

```cpp
// Comparing this to App::Run() in the DirectX 11 App template
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            // Added: Switching different game states since this game sample is more complex
            switch (m_updateState) 
            {

                // ...
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                // 1. Update
                // Also exist in DirectX 11 App template: Update loop to get the latest game changes
                Update();
                
                // 2. Render
                // Present also in DirectX 11 App template: Prepare to render
                m_renderer->Render();
                
                // 3. Present
                // Present also in DirectX 11 App template: Present the 
                // contents of the swap chain to the screen.
                m_deviceResources->Present(); 
                
                // Added: resetting a variable we've added to indicate that 
                // render has been done and no longer needed to render
                m_renderNeeded = false; 
            }
        }
        else
        {   
            // Present also in DirectX 11 App template
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending); 
        }
    }
    m_game->OnSuspending();  // exiting due to window close. Make sure to save state.
}
```

### <a name="update"></a>Update/Aktualisieren

Weitere Informationen über das Aktualisieren der Spielzustände finden Sie im Artikel [Spielablaufverwaltung](tutorial-game-flow-management.md) der [__App:: Update__ und __GameMain::Update__](tutorial-game-flow-management.md#appupdate-method)-Methode.

### <a name="render"></a>Rendern

Das Rendern wird durch Aufrufen der [__GameRenderer::Render__](#gamerendererrender-method)-Methode im __GameMain::Run__ implementiert.

Wenn [Stereorendering](#stereo-rendering) aktiviert ist, gibt es zwei Renderingdurchgänge: einen für das rechte Auge und einen für das linke Auge. In jeder Renderingdurchlauf binden wir das Renderziel und die Ansicht der tiefenschablone auf dem Gerät. Wir löschen danach die Tiefenschablonen-Ansicht.

> [!Note]
> Stereorendering kann mit anderen Methoden wie z. B. einem einzigen Stereo-Durchlauf mit Vertex-Instanzerstellung oder Geometry-Shader erzielt werden. Die beiden Renderingdurchgangs-Methoden sind langsamer, aber einfacher zum Stereo-Rendering.

Wenn das Spiel vorhanden ist und Ressourcen geladen werden, aktualisieren Sie die [Projektionsmatrix](#projection-transform-matrix), einmal pro Renderingdurchgang. Objekte sehen in den verschiedenen Ansichten etwas anders aus. Richten Sie nun die [Grafikrenderingpipeline](#rendering-pipeline) ein. 

> [!Note]
> Weitere Informationen finden Sie unter [Erstellen und Laden von DirectX-Grafikressourcen](tutorial-game-rendering.md#create-and-load-directx-graphic-resources), um zu sehen, wie Ressourcen geladen werden.

In diesem Beispielspiel verwendet der Renderer ein Standard-Vertex-Layout für alle verwendeten Objekte. Dies vereinfacht das Shader-Design und ermöglicht eine einfache Änderungen zwischen den Shaders, unabhängig von der Objekt-Geometrie.

#### <a name="gamerendererrender-method"></a>GameRenderer::Render-Methode

Legen Sie den Direct3D-Kontext fest, um ein Eingabevertex-Layout zu verwenden. Eingabevertex-Objekte beschreiben das Streamen der Vertexpufferdaten in die [Renderingpipeline](#rendering-pipeline). 

Wir legen Sie als Nächstes den Direct3D-Kontext, die zuvor definierten Konstantenpuffer verwendet, mit denen die [Vertex-Shader](#vertex-shaders-and-pixel-shaders) Phase pipeline und die [Pixel-Shader](#vertex-shaders-and-pixel-shaders) pipeline-Phase. 

> [!Note]
> Finden Sie unter [Rendering-Framework II: Rendern von Spielen](tutorial-game-rendering.md) für Weitere Informationen über die Definition der Konstante Puffer.

Da das gleiche Eingabelayout und der Satz von Konstantenpuffern für alle Shader in der Pipeline verwendet wird, wird es einmal pro Frame festgelegt.

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            
            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() }; 
            
            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap()); 
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;

                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, 
            // go to https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476454.aspx
            d3dContext->IASetInputLayout(m_vertexLayout.Get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476491.aspx

            d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476470.aspx

            d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); /
            }
        }
        else
        {
            const float ClearColor[4] = {0.5f, 0.5f, 0.8f, 1.0f};

            // Only need to clear the background when not rendering the full 3D scene since
            // the 3D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (i > 0)
            {
                // Doing the Right Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), ClearColor);
            }
        }

        // Start of 2D rendering
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // ...
    }
}
```

### <a name="primitive-rendering"></a>Rendern der Grundtypen

Beim Rendern der Szene durchlaufen Sie alle Objekte, die gerendert werden müssen. Die folgenden Schritte werden für jedes Objekt (Grundtyp) wiederholt.

* Aktualisieren Sie den Konstantenpuffer (__m\_ConstantBufferChangesEveryPrim__) mit des Modells des [globale Transformationsmatrix](#world-transform-matrix) und wesentliche Informationen.
* Die __m\_ConstantBufferChangesEveryPrim__ Parameter für jedes Objekt enthält.  Beinhaltet das Objekt für die globale Transformationsmatrix sowie die grundlegenden Eigenschaften wie Farbe und Glanzlichterexponenten für die Berechnung der Beleuchtung.
* Legen Sie den Direct3D-Kontext für das Eingabevertex-Layout für die Gitterobjektdaten fest, das in die Eingabeassemblerphase (IA)-Phase der [Renderingpipeline](#rendering-pipeline) gestreamt werden soll
* Legen Sie den Direct3D-Kontext für den [Indexpuffer](#index-buffer) in der IA-Phase fest. Geben Sie die Grundtyp-Informationen an: Typ, Datenreihenfolge.
* Übermitteln Sie einen Draw-Aufruf zum Zeichnen des indizierten, nicht instanziierten Grundtyps. Die __GameObject::Render__-Methode aktualisiert den [Grundtypkonstantenpuffer](#constant-buffer-or-shader-constant-buffer) mit den spezifischen Daten eines Grundtyps. Dadurch erfolgt ein __DrawIndexed__-Aufruf für den Kontext, um die Geometrie jedes Grundtyps zu zeichnen. Durch diesen Draw-Aufruf werden Befehle und Daten in die Warteschlange der GPU eingereiht (entsprechend der Parametrisierung durch die Konstantenpufferdaten). Jeder Draw-Aufruf führt den Vertex-Shader einmal pro Vertex, und klicken Sie dann die [Pixel-Shader](#vertex-shaders-and-pixel-shaders) einmal für jedes Pixel der einzelnen Dreiecke in der Primitive. Die Texturen sind Teil des Zustands, den der Pixelshader für das Rendering verwendet.

Gründe für die Verwendung mehrerer Konstantenpuffer:
    * Im Spiel werden mehrere Konstantenpuffer verwendet; diese müssen aber nur einmal pro Grundtyp aktualisiert werden. Sie können sich die Konstantenpuffer als Eingabe für die für jeden Grundtyp ausgeführten Shader vorstellen. Einige Daten sind statisch (__m\_ConstantBufferNeverChanges__); einige Daten sind über den Frame Konstanten (__m\_ConstantBufferChangesEveryFrame__), wie Sie die Position der Kamera und einige Daten bezieht sich auf den Grundtyp, wie seine Farbe und Texturen (__m\_ConstantBufferChangesEveryPrim__)
    * Der Spielrenderer teilt diese Eingaben in verschiedene Konstantenpuffer auf, um die von CPU und GPU verwendete Speicherbandbreite zu optimieren. Durch diesen Ansatz lässt sich auch die Datenmenge minimieren, die von der GPU nachverfolgt werden muss. Die GPU verfügt über eine große Befehlswarteschlange und dieser Befehl bei jedem Aufruf von __Draw__ zusammen mit den dazugehörigen Daten in die Warteschlange eingereiht wird. Wenn das Spiel den Grundtypkonstantenpuffer aktualisiert und den nächsten __Draw__-Befehl ausgibt, fügt der Grafiktreiber diesen Befehl und die dazugehörigen Daten der Warteschlange hinzu. Zeichnet das Spiel 100 Grundtypen, kann die Warteschlange 100 Kopien der Konstantenpufferdaten enthalten. Um die an die GPU gesendete Datenmenge zu minimieren, wird im Spiel ein separater Grundtypkonstantenpuffer verwendet, der nur die Aktualisierungen für jeden Grundtyp enthält.

#### <a name="gameobjectrender-method"></a>GameObject::Render-Methode

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m\_active || (m\_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}

#### MeshObject::Render method

void MeshObject::Render(\_In\_ ID3D11DeviceContext *context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476453.aspx
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476455.aspx
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476409.aspx
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="present"></a>Darstellen

Wir rufen die __DX::DeviceResources::Present__-Methode auf, um den Inhalt des Puffers zu platzieren und anzuzeigen.

Eine Swapchain ist eine Sammlung von Puffern, die zum Anzeigen von Frames für den Benutzer verwendet werden. Bei jeder Darstellung eines neuen Frames für eine Anzeige durch eine Anwendung wird der erste Puffer der Swapchain zum Anzeigepuffer. Dieser Prozess wird Austauschen oder Kippen genannt. Weitere Informationen finden Sie unter [Swapchain](../graphics-concepts/swap-chains.md).

* Die in der __IDXGISwapChain1__ Schnittstelle __vorhanden__-Methode weist [DXGI](#dxgi) an, bis zur vertikalen Synchronisierung (VSync) zu blockieren und die Anwendung bis zum nächsten VSYNC-Vorgang in den Standbymodus versetzen. Dadurch wird sichergestellt, dass Sie kein Durchlaufrendern des Frames vergeuden, das nie auf dem Bildschirm angezeigt werden.
* Die in der __ID3D11DeviceContext3__ Schnittstelle enthaltene __DiscardView__-Methode verwirft den Inhalt des [Renderziels](#render-target). Dies ist nur dann ein gültiger Vorgang, wenn der vorhandene Inhalt vollständig überschrieben wird. Wenn verschmutzte oder Bildlauf-Rechtecke verwendet werden, sollte dieser Aufruf entfernt werden.
* Mit der gleichen __DiscardView__-Methode, verwerfen Sie den Inhalt der [Tiefenschablonen](#depth-stencil).
* Die __HandleDeviceLost__-Methode wird verwendet, um das Szenario zu verwalten, wenn das [Gerät](#device) entfernt wird. Wenn das Gerät entweder durch ein Trennen der Verbindung oder dem Upgrade des Treibers entfernt wurde, müssen Sie alle Geräteressourcen neu erstellen. Weitere Informationen finden Sie unter [Behandeln von Szenarien mit entfernten Geräten in Direct3D 11](handling-device-lost-scenarios.md)

> [!Tip]
> Um eine reibungslose Framerate zu erreichen, müssen Sie sicherstellen, dass die Menge der zu rendernden Frames in die Zeit zwischen den VSyncs passt.

```cpp
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());

    // Discard the contents of the depth-stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    // For more info about how to handle a device lost scenario, go to:
    // https://docs.microsoft.com/windows/uwp/gaming/handling-device-lost-scenarios
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        DX::ThrowIfFailed(hr);
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wird erläutert, wie Grafiken auf dem Bildschirm gerendert werden sowie eine kurze Beschreibung für die Rendering-Terminologie. Weitere Informationen zum Rendern in das [Rendering-Framework II: Rendern von Spielen](tutorial-game-rendering.md) Dokument aus, und erfahren Sie, wie die Daten, die erforderlich sind, vor dem Rendern vorbereiten.

## <a name="terms-and-concepts"></a>Begriffe und Konzepte

### <a name="simple-game-scene"></a>Einfache Spielszenen

Eine einfache Spieleszene besteht aus wenigen Objekten mit mehreren Lichtquellen.

Ein Objekt wird durch eine Reihe von X-, Y- und Z-Koordinaten im Raum definiert. Die tatsächliche Rendering-Position in der Spielwelt kann durch das Anwenden einer Transformationsmatrix mit fester Breite der X, Y, Z-Koordinaten festgelegt werden. Sie müssen auch eine Reihe von Texturkoordinaten haben – U und V geben an, wie ein Material auf das Objekt angewendet wird. Diese definiert die Surface-Eigenschaften des Objekts und gibt Ihnen die Möglichkeit, festzustellen, ob ein Objekt über eine grobe Oberfläche wie ein Tennisball oder eine glatte glänzende Oberfläche wie ein Bowlingball verfügt.

Szenen- und Objekt-Informationen werden von der Rendering-Framework verwendet, um die Szene von Frame zu Frame neu zu erstellen, und sie auf Ihrem Bildschirm lebendig werden zu lassen.

### <a name="rendering-pipeline"></a>Renderingpipeline

Im Prozess der Renderingpipeline werden die 3D-Szenen-Informationen zu einem Bild auf dem Bildschirm übersetzt. In Direct3D 11 ist diese Pipeline programmierbar. Sie können die Phasen zur Unterstützung der Rendering-Anforderungen anpassen. Stufen, die allgemeine Shaderkerne bereitstellen, sind mit der HLSL-Programmiersprache programmierbar. Dies ist auch als Grafikrenderingpipeline oder einfach Pipeline bekannt.

Um diese Pipeline erstellen zu können, müssen Sie damit vertraut sein:
* [HLSL](#HLSL). Wir empfehlen die Verwendung von HLSL-Shader Model 5.1 und höher für UWP-DirectX-Spiele.
* [Shader](#Shaders)
* [Vertex-Shader und Pixel-Shader](#vertext-shaders-pixel-shaders)
* [Shader-Stufen](#shader-stages)
* [Verschiedene Shader-Dateiformate](#various-shader-file-formats)

Weitere Informationen finden Sie unter [Understand the Direct3D 11 rendering pipeline](https://msdn.microsoft.com/library/windows/desktop/dn643746.aspx) und [Grafik-Pipeline](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="hlsl"></a>HLSL

HLSL ist die High Level Shading-Sprache für DirectX. Mit HLSL können Sie wie in C-programmierbare Shader für die Direct3D-Pipeline erstellen. Weitere Informationen finden Sie unter [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561.aspx).

#### <a name="shaders"></a>Shader

Shader können als ein Satz von Anweisungen betrachtet werden, die bestimmen, wie die Oberfläche eines Objekts gerendert wird. Diejenigen, die mithilfe von HLSL programmiert werden, werden als HLSL-Shader bezeichnet. Quellcodedateien für [HLSL])(#hlsl)-Shader haben die Erweiterung der HLSL-Datei. Diese Shader können zum Zeitpunkt der Erstellung oder Laufzeit kompiliert werden und zur Laufzeit auf die entsprechende Pipelinephase festgelegt werden; ein kompiliertes Shader-Objekt hat die Dateierweiterung ".cso".

Direct3D 9-Shader können mit Shadermodell 1, Shadermodell 2 und Shadermodell 3 entworfen werden. Direct3D 10 Shader können nur auf Shadermodell 4 entworfen werden. Direct3D 11-Shader können auf Shadermodell 5 entworfen werden. Direct3D 11.3 und Direct3D 12 können auf Shadermodell 5.1 entwickelt werden und Direct3D 12 kann auch auf Shadermodell 6 entworfen werden.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Vertex-Shader sind Pixelshader

Daten werden in der Grafikpipeline als einen Stream von Grundtypen und durch verschiedene Shader z. B. den Vertex-Shader und Pixel-Shader verarbeitet. 

Vertex-Shader verarbeiten Scheitelpunkte und führt dabei in der Regel Vorgänge wie Transformationen, das Anwenden von Skins und Beleuchtung durch.  Pixelshader aktivieren umfassende Schattierungstechniken wie z. B. Beleuchtung pro Pixel und Nachbearbeitung. Diese kombinieren Konstantenvariablen, Texturdaten, interpolierte Vertex-Werte und andere Daten für Pro-Pixel-Ausgaben. 

#### <a name="shader-stages"></a>Shaderstufen

Eine Sequenz dieser verschiedene Shader für die Verarbeitung dieser Grundtypen wird als Shaderstufen in einer Renderingpipeline bezeichnet. Die tatsächlichen Phasen hängen von der Version von Direct3D ab, aber in der Regel umfassen sie Vertex-, Geometrie- und Pixel-Phasen. Es gibt auch andere Phasen, z. B. Geometrie- und Domänen-Shader für Tesselation und Compute-Shader. Alle diese Stufen sind vollständig programmierbar mithilfe von [HLSL])(#hlsl). Weitere Informationen finden Sie unter [Grafikpipeline](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="various-shader-file-formats"></a>Verschiedene Shader-Dateiformate

Shader-Code-Dateierweiterungen:
    * Eine Datei mit HLSL-Erweiterung enthält [HLSL])(#hlsl)-Quellcode.
    * Eine Datei mit CSO-Erweiterung enthält ein kompiliertes Shader-Objekt.
    * Eine Datei mit der Erweiterung .h ist eine Headerdatei, aber im Kontext des Shader-Code definiert diese Headerdatei ein Byte-Array, das Shader-Daten enthält.
    * Eine Datei mit HLSLI-Erweiterung enthält das Format einer HLSLI-Datei: . Im Beispielspiel ist es die Datei __Shader__>__ConstantBuffers.hlsli__.

> [!Note]
> Sie würden den Shader einbetten, indem Sie entweder eine CSO-Datei zur Laufzeit laden oder die h-Datei in Ihren ausführbaren Code hinzufügen. Jedoch werden nicht beide für den gleichen Shader verwendet.

### <a name="deeper-understanding-of-directx"></a>Weitere Kenntnisse über DirectX

Direct3D 11 ist eine Reihe von APIs, mit deren Hilfe wir Grafiken für Grafik-intensive Anwendung erstellen, z. B. Spiele, in der wir eine gute Grafikkarte benötigen, um die ressourcenintensive Berechnung verarbeiten zu können. In diesem Abschnitt werden die Direct3D 11-Grafik-Programmierkonzepte kurz erläutert: Ressource, Unterressource, Gerät und Gerätekontext.

#### <a name="resource"></a>Ressource

Für diejenigen, die noch nie damit gearbeitet haben, können Sie sich Ressourcen (auch bekannt als Geräteressourcen) als Informationen vorstellen, um ein Objekt wie Textur, Position, Farbe zu rendern. Ressourcen stellen Daten für die Pipeline bereit und definieren, was während der Szene gerendert wird. Ressourcen können von Ihren Spielmedien geladen oder zur Laufzeit dynamisch erstellt werden.

Eine Ressource ist ein Bereich im Speicher, auf den die Direct3D-[Pipeline](#rendering-pipeline) zugreifen kann. Damit die Pipeline effizient auf den Speicher zugreifen kann, müssen für die Pipeline bereitgestellte Daten (wie z. B. Input-Geometrie, Shader-Ressourcen und Texturen) in einer Ressource gespeichert werden. Es gibt zwei Arten von Ressourcen, aus denen alle Direct3D-Ressourcen abgeleitet sind: Puffer und Textur. Bis zu 128 Ressourcen können für jede Pipeline-Phase aktiv sein. Weitere Informationen finden Sie unter [Ressourcen](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Unterressourcen

Der Begriff Unterressource bezieht sich auf die Teilmenge einer Ressource. Direct3D kann auf eine gesamte Ressource verweisen oder auf die Teilmenge einer Ressource. Weitere Informationen finden Sie unter [Unterressource](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Tiefenschablone

Eine Tiefenschablonenressource stellt das Format und den Puffer zur Speicherung von Tiefen- und Schabloneninformationen bereit. Es wird mit der eine Texturressource erstellt. Weitere Informationen zum Erstellen einer Tiefenschablonen-Ressource finden Sie unter [Konfigurieren der Tiefenschablonenfunktionalität](https://msdn.microsoft.com/library/windows/desktop/bb205074.aspx). Wir greifen über die implementierte Tiefenschablonenansicht auf die Tiefenschablonenressource zu, mithilfe der [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377.aspx)-Schnittstelle.

Tiefeninformationen steuern, welche Bereiche von Polygonen dargestellt werden. Schabloneninformationen steuern, welche Pixel maskiert werden. Es kann verwendet werden, um Spezialeffekte zu erzeugen, da es bestimmt, ob ein Pixel gezeichnet oder nicht gezeichnet wird. Es setzt das Bit auf 1 oder 0. 

Weitere Informationen finden Sie unter: [Ansicht der tiefenschablone](../graphics-concepts/depth-stencil-view--dsv-.md), [Tiefenpuffer](../graphics-concepts/depth-buffers.md), und [Stencilpuffer](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Renderziel

Eine Renderziel ist eine Ressource, in die wir am Ende eines Renderingdurchgangs schreiben können. Es wird häufig mit der [ID3D11Device::CreateRenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476517.aspx)-Methode erstellt, mit der Hintergrundpuffer-Swapkette (die auch eine Ressource ist) als Eingabeparameter. 

Jedes Renderziel sollte auch eine entsprechende Tiefenschablonenansicht haben, da wir [OMSetRenderTargets](https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx) zum Festlegen des Renderziel vor der Verwendung verwenden, was auch eine Tiefenschablonenansicht erfordert. Wir greifen auf die Renderzielressource über die implementierte Renderzielansicht mit der [ID3D11RenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476582.aspx)-Schnittstelle zu. 

#### <a name="device"></a>Gerät

Für Benutzer, die mit Direct3D 11 nicht vertraut sind, ist dies ein Gerät zum Zuordnen und zerstören von Objekten, um Grundtypen zu rendern und mit der Grafikkarte über die Grafiktreiber zu kommunizieren. 

Oder präzise ausgedrückt: Direct3D-Gerät ist die Komponente zum Rendern von Direct3D. Ein Gerät kapselt und speichert den Renderstatus, führt Transformationen und Beleuchtungsvorgänge aus, und rastert ein Bild zu einer Oberfläche. Weitere Informationen finden Sie unter [Geräte](../graphics-concepts/devices.md)

Ein Gerät wird durch die [ID3D11Device](https://msdn.microsoft.com/library/windows/desktop/ff476379.aspx)-Schnittstelle dargestellt. In anderen Worten: die ID3D11Device-Schnittstelle stellt eine virtuelle Grafikkarte dar und wird verwendet, um die Ressourcen zu erstellen, die das Gerät besitzt. 

Beachten Sie, dass es verschiedene Versionen von ID3D11Device gibt, [ID3D11Device5](https://msdn.microsoft.com/library/windows/desktop/mt492478.aspx) ist die neueste Version und fügt neue Informationen zum ID3D11Device4 hinzu. Weitere Informationen zur Kommunikation zwischen Direct3D mit der zugrunde liegenden Hardware finden Sie unter [Windows Device Driver Model (WDDM)-Architektur](https://docs.microsoft.com/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Jede Anwendung muss mindestens ein Gerät haben, die meisten Apps erstellen nur ein Gerät. Erstellen Sie ein Gerät für eine der die Hardwaretreiber, die auf Ihrem Computer installiert ist, durch den Aufruf __D3D11CreateDevice__ oder __D3D11CreateDeviceAndSwapChain__ und geben Sie den Treibertyp mit der D3D\_ Treiber\_Typflag. Jedes Gerät kann einen oder mehrere Gerätekontexte benutzen, je nach Bedarf der gewünschten Funktion. Weitere Informationen finden Sie unter [D3D11CreateDevice-Funktion](https://msdn.microsoft.com/library/windows/desktop/ff476082.aspx).

#### <a name="device-context"></a>Gerätekontext

Der Gerätekontext wird verwendet, um den [Pipelinestatus](#rendering-pipeline) festzulegen und Renderbefehle mit [Ressourcen](#resource) eines [Geräts](#device) zu erzeugen. 

Direct3D 11 implementiert zwei Arten von Gerätekontexten, eins für das sofortige Rendern und das andere für das verzögerte Rendern. Beide Kontexte werden durch eine [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476385.aspx)-Schnittstelle dargestellt.  

Die __ID3D11DeviceContext__-Schnittstellen haben unterschiedliche Versionen; __ID3D11DeviceContext4__ fügt neue Methoden zu __ID3D11DeviceContext3__ hinzu.

Hinweis: __ID3D11DeviceContext4__ wird in das Windows 10 Creators Update eingeführt und ist die neueste Version von der __ID3D11DeviceContext__ Schnittstelle. Anwendungen für das Windows 10 Creators Update sollten diese Schnittstelle anstelle von früheren Versionen verwenden. Weitere Informationen finden Sie unter [ID3D11DeviceContext4](https://msdn.microsoft.com/library/windows/desktop/mt492481.aspx).

#### <a name="dxdeviceresources"></a>DX::DeviceResources

Die __DX::DeviceResources__-Klasse befindet sich in der __DeviceResources.cpp__/__.h__-Datei und steuert alle Geräteressourcen von DirectX. In dem Spiele-Beispielprojekt und der DirectX 11-App-Projektvorlage, befinden sich diese Dateien im Ordner __Commons__. Sie können auf die neueste Version dieser Dateien zuzugreifen, wenn Sie ein neues DirectX 11-App-Vorlageprojekt in Visual Studio 2015 oder höher erstellen.

### <a name="buffer"></a>Puffer

Eine Pufferressource ist eine Sammlung vollständig typisierter Daten, die zu Elementen gruppiert werden. Verwenden Sie Puffer zum Speichern von Daten, wie Positionsvektoren, normale Vektoren, Texturkoordinaten in einem Vertexpuffer, Indexe in einem Indexpuffer oder den Gerätezustand. Pufferelemente können gepackte Datenwerten (wie R8G8B8A8-Oberflächenwerte), einzelne 8-Bit-Ganzahlen oder vier 32-Bit-Gleitkommawerte enthalten.

Es gibt drei Arten von Puffer verfügbar: Vertexpuffer, Indexpuffer und Konstantenpuffer.

#### <a name="vertex-buffer"></a>Vertexpuffer

Enthält Scheitelpunktdaten, die zur Definition Ihrer Geometrie verwendet werden. Zu den Scheitelpunktdaten gehören Positionskoordinaten, Farbdaten, Texturkoordinatendaten, Normale-Daten usw. 

#### <a name="index-buffer"></a>Indexpuffer

Enthalten Ganzzahl-Offsets in Vertexpuffern und werden zum effizienteren Rendern der Grundtypen verwendet. Ein Indexpuffer enthält eine aufeinanderfolgende Reihe von 16-Bit- oder 32-Bit-Indizes; jeder Index wird verwendet, um einen Scheitelpunkt im Vertexpuffer zu kennzeichnen.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Konstantenpuffer oder Shader-Konstantenpuffer

Ermöglicht Ihnen, Shader-Daten effizient an die Pipeline zu liefern. Sie können Konstantenpuffer als Eingabe für die Shader verwenden, die für jeden Grundtyp ausgeführt werden und die Ergebnisse der Stream-Ausgabe-Stufe der Renderingpipeline speichern. Vom Konzept her sieht ein Konstantenpuffer wie ein Vertexpuffer mit einem Element aus.

#### <a name="design-and-implementation-of-buffers"></a>Entwurf und -Implementierung der Puffer

Sie können Puffer basierend auf den Datentypen entwickeln, z. B., wie in unserem Spielbeispiel, wobei ein Puffer für statische Daten erstellt wird, ein weiterer für Daten, die innerhalb eines Frames konstant sind und eine weiterer für Daten, die speziell für einen Grundtyp gelten.

Alle Puffertypen sind von der __ID3D11Buffer__-Schnittstelle gekapselt, und Sie können eine Pufferressource erstellen, indem Sie __id3d11Device:: CreateBuffer__ aufrufen. E Puffer muss in der Pipeline gebunden werden, damit darauf zugegriffen werden kann. Puffer können zum Lesen nicht gleichzeitig mit mehreren Phasen der Pipeline gebunden werden. Ein Puffer kann auch an eine einzelne Pipelinephase zum Schreiben gebunden werden. Allerdings kann nicht der gleiche Puffer zum Lesen und Schreiben gleichzeitig gebunden werden.

Binden Sie Puffer an:
    * den Eingabeassemblerzustand durch Aufrufen der __ID3D11DeviceContext__-Methoden, wie __ID3D11DeviceContext::IASetVertexBuffers__ und __ID3D11DeviceContext::IASetIndexBuffer__
    * Die Streamoutputstufe durch Aufrufen von __ID3D11DeviceContext::SOSetTargets__
    * Die Shader-Stufe durch Aufrufen der Shader-Methoden, wie __id3d11DeviceContext:: VSSetConstantBuffers__

Weitere Informationen zu finden Sie unter [Einführung in Puffer in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898.aspx).

### <a name="dxgi"></a>DXGI

Microsoft DirectX Graphics Infrastructure (DXGI) ist ein neues Subsystem, das mit Windows Vista eingeführt wurde, die einige der Aufgaben auf niedriger Ebene kapselt, die Direct3D 10, benötigt 10.1, 11 und 11.1. Bei Verwendung von DXGI in einer Multithread-Anwendung ist besondere Vorsicht geboten, um sicherzustellen, dass keine Deadlocks auftreten. Weitere Informationen finden Sie unter [DirectX Graphics Infrastructure (DXGI): Best Practices-Multithreading](https://msdn.microsoft.com/library/windows/desktop/ee417025.aspx#multithreading_and_dxgi)

### <a name="feature-level"></a>Featureebene

Die Featureebene ist ein Konzept, das in Direct3D 11 eingeführt wurde, um die Vielfalt der Grafikkarten in neuen und vorhandenen Computern zu behandeln. Eine Featureebene ist ein klar definierter Satz mit GPU-Funktionen. 

Jede Grafikkarte implementiert eine gewisse DirectX-Funktion, abhängig von den installierten GPUs. In früheren Versionen von Microsoft Direct3D konnten Sie die Version der implementierten Direct3D-Grafikkarte sehen und die Anwendung entsprechend programmieren. 

Mit der Featureebene können Sie bei der Erstellung eines Geräts versuchen, ein Gerät für die Featureebene zu erstellen, die Sie anfordern möchten. Wenn die Geräteerstellung funktioniert, ist die Featureebene vorhanden, andernfalls wird die Featureebene von der Hardware nicht unterstützt. Sie können entweder versuchen, ein Gerät auf einer niedrigeren Featureebene neu zu erstellen oder Sie können die Anwendung beenden. Z. B. die 12\_0 Funktionsebene erfordert Direct3D 11.3 oder Direct3D 12- und Shader Model 5.1. Weitere Informationen finden Sie unter [Direct3D-Funktionsebenen: Übersicht über die für jede Funktion](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx#Overview).

Verwenden die Funktionsebenen, können Sie eine Anwendung entwickeln, für die Direct3D 9, Microsoft Direct3D 10 und Direct3D 11 und führen Sie anschließend auf 9, 10 oder 11-Hardware (mit einigen Ausnahmen). Weitere Informationen finden Sie unter [Direct3D-Featureebenen](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx).

### <a name="stereo-rendering"></a>Stereorendering

Stereorendering wird verwendet, um die Illusion der Tiefe zu verbessern. Er verwendet zwei Bilder, eine vom linken Auge und das andere vom rechten Auge, um eine Szene auf dem Bildschirm anzuzeigen. 

Mathematisch gesehen verwenden wir eine Stereo-Projektionsmatrix, die einen leicht horizontalen Versatz nach rechts und links neben der regulären Mono-Projektionsmatrix hat, um dies zu erreichen.

Wir haben zwei Renderingdurchgänge, um Stereorendering in diesem Beispielspiel zu erreichen:
* Binden Sie diese an das rechte Renderziel, wenden Sie die rechte Projektion an und Zeichnen Sie das Grundtypobjekt.
* Binden Sie diese an das linke Renderziel, wenden Sie die linke Projektion an und Zeichnen Sie das Grundtypobjekt.

### <a name="camera-and-coordinate-space"></a>Kamera und Koordinatenraum

Das Spiel enthält den erforderlichen Code, um die Spielwelt in seinem eigenen Koordinatensystem (auch als Spielweltbereich oder Szenenbereich bezeichnet) zu aktualisieren. Alle Objekte, einschließlich der Kamera, werden in diesem Bereich positioniert und ausgerichtet. Weitere Informationen zu Standortsystemen finden Sie unter [Koordinatensysteme](../graphics-concepts/coordinate-systems.md).

Ein Vertexshader übernimmt die schwere Aufgabe, die Modellkoordinaten mit dem folgenden Algorithmus in die Gerätekoordinaten zu konvertieren (wobei V ein Vektor und M eine Matrix ist).

V(Gerät) = V(Modell) x M(model-to-world) x M(world-to-view) x M(view-to-device).

Dabei gilt Folgendes: 
* M(Model-to-World) ist eine Transformationsmatrix für die Modellkoordinaten in Spielweltkoordinaten, auch bekannt als die [World transform matrix](#world-transform-matrix). Diese Matrix wird vom Grundtyp bereitgestellt.
* M(world-to-view) ist eine Transformationsmatrix für die Weltkoordinaten in Spielansichtskoordinaten, auch bekannt als die [Ansichtstransformationsmatrix](#view-transform-matrix).
    * Diese Matrix wird von der Ansichtsmatrix der Kamera bereitgestellt. Es wird durch die Position der Kamera und die Blickvektoren definiert (der Vektor "anschauen", der von der Kamera in die Szene weist, und der Vektor "nach oben schauen" senkrecht zur Szene).
    * Im Beispielspiel __m\_ViewMatrix__ Ansichtstransformationsmatrix und wird mit __Camera::SetViewParams__ 
* M(view-to-device) ist eine Transformationsmatrix für die Ansichtskoordinaten in Gerätekoordinaten, auch bekannt als die [Projektstransformationsmatrix](#projection-transform-matrix).
    * Diese Matrix wird von der Projektion der Kamera bereitgestellt. Es enthält Informationen darüber, wie dieser Platz tatsächlich in der endgültigen Szene sichtbar ist. Das Blickfeld (FoV), das Seitenverhältnis und die Clippingebenen definieren die Projektionstransformationsmatrix.
    * Im Beispielspiel __m\_ProjectionMatrix__ Transformation definiert, zu die Koordinaten der Projektion mit berechnet __Camera::SetProjParams__ (für Stereo Projektion, Sie verwenden zwei Projektion Matrizen: eine für jede Einblick in die.) 

Der Shadercode in VertexShader.hlsl wird mit diesen Vektoren und Matrizen aus den Konstantenpuffern geladen und führt diese Transformation für jeden Vertex aus.

### <a name="coordinate-transformation"></a>Koordinatentransformation

Direct3D verwendet drei Transformationen, um Ihre 3D-Modell-Koordinaten in Pixelkoordinaten zu verwandeln (Bildschirmbereich). Diese Transformationen sind die globale Transformation, die Ansichtstransformation und Projektionstransformation. Weitere Informationen finden Sie unter [Übersicht über Transformationen](../graphics-concepts/transform-overview.md).

#### <a name="world-transform-matrix"></a>Welttransformationsmatrix

Eine Welttransformationsmatrix ändert die Koordinaten des Modellbereichs, in dem Scheitelpunkte definiert werden, die relativ zum lokalen Ursprung des Modells im Weltraum sind, wo Scheitelpunkte relativ zum Ursprung für alle Objekte in einer Szene definiert werden. Die Welttransformation platziert ein Modell in der Welt, daher ihr Name. Weitere Informationen finden Sie unter [Welttransformationen](../graphics-concepts/world-transform.md).

####  <a name="view-transform-matrix"></a>Ansichtstransformationsmatrix

Die Ansichtstransformation lokalisiert den Betrachter im Weltbereich und transformiert dazu Scheitelpunkte in den Kamerabereich. Im Kamerabereich befindet sich die Kamera, bzw. der Betrachter, am Ursprungspunkt und blickt in die positive z-Richtung. Weitere Informationen finden Sie unter [Ansichtstransformation](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Projektionstransformationsmatrix

Die Projektionstransformation konvertiert das Ansichtsfrustum in eine Quaderform. Ein Pyramidenstumpf ist ein 3D-Körper in einer Szene, der relativ zur Viewport-Kamera positioniert ist. Ein Viewport ist ein 2D-Rechteck, auf das eine 3D-Szene projiziert wird. Weitere Informationen finden Sie unter [Viewports und Zuschneiden](../graphics-concepts/viewports-and-clipping.md).

Da das nähergelegene Ende des Ansichtsfrustrums kleiner als das weiter entfernt liegende Ende, hat dies die Wirkung, dass näher bei der Kamera liegende Objekte erweitert werden. So wird Perspektive auf die Szene angewandt. Daher erscheinen Objekte, die näher am Spieler sind, größer; Objekte, die weiter entfernt sind, werden kleiner angezeigt.

Die Projektionstransformation ist mathematisch gesehen eine Matrix, die in der Regel eine Skalierung und Perspektive verwendet. Sie funktioniert wie die Foto-App einer Kamera. Weitere Informationen finden Sie unter [Projektionstransformationen](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>Musterzustand

Der Musterzustand legt fest, wie Texturdaten verwendet werden mit Textur und deren Modi, Filtern und Genauigkeit. Sampling erfolgt jedes Mal, wenn ein Texturpixel oder Texel aus einer Textur gelesen wird.

Eine Textur enthält ein Array von Texel oder Texturpixeln. Die Position des einzelnen Texels wird von (u, v) festgelegt, wobei u die Breite und v ist die Höhe ist und zwischen 0 und 1 zugeordnet ist die Texturbreite und Höhe abhängig. Die resultierende Texturkoordinaten werden auf einem Texel beim Sampling einer Textur verwendet.

Wenn Texturkoordinaten unter 0 oder 1 sind, definiert der Textur-Adressmodus wie die Texturkoordinate einen den Ort des Texels festlegt. Bei Verwendung __TextureAddressMode.Clamp__ wird eine Koordinate außerhalb des 0-1-Bereichs auf einen Maximalwert von 1 und Mindestwert von 0 vor dem Sampling festgelegt.

Wenn die Textur zu groß oder zu klein für das Polygon ist, wird die Textur gefiltert und dem Abstand angepasst. Ein Vergrößerungsfilter vergrößert die Textur, ein Verkleinerungsfilter reduziert die Textur, um ihn einem kleineren Bereich anzupassen. Texturvergrößerung wiederholt die Texel-Beispiel für eine oder mehrere Adressen, was ein verschwommenes Image ergibt. Texturverkleinerung ist komplizierter, da es mehr als einen Texelwert in einen einzelnen Wert kombiniert. Dadurch können Aliasing oder ausgefranste Ränder je nach der Texturdaten entstehen. Der am häufigsten verwendete Ansatz für die Verkleinerung ist die Verwendung eine Mipmap. Eine Mipmap ist eine Textur mit mehreren Ebenen. Die Größe der einzelnen Ebenen ist um eine Zweierpotenz kleiner als die vorherigen Ebenen bis zur Textur 1 x 1. Wenn die Verkleinerung verwendet wird, wählt ein Spiel die Mipmap-Ebene, die am der Größe des Renderns am nächsten liegt. 

### <a name="basicloader"></a>BasicLoader

__BasicLoader__ ist eine einfache Loader-Klasse für die Unterstützung beim Laden von Shader, Textur und Gitter von Dateien auf einem Datenträger. Sie bietet synchrone und asynchrone Methoden an. In diesem Beispielspiel befinden sich die BasicLoader.h/.cpp-Dateien im Ordner __Commons__.

Weitere Informationen finden Sie unter [Basic Loader](complete-code-for-basicloader.md).