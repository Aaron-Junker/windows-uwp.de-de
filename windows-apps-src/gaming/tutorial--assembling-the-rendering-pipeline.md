---
title: Einführung in Rendering
description: Erfahren Sie, wie Sie die Renderingpipeline zum Anzeigen von Grafiken entwickeln. Einführung in das Rendering.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Rendering
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409639"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Renderingframework I: Einführung in Rendering

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

Bisher haben wir erläutert, wie ein universelle Windows-Plattform (UWP)-Spiel strukturiert wird und wie Sie einen Zustands Automat definieren, um den Flow des Spiels zu verarbeiten. Nun ist es an der Zeit, zu erfahren, wie das Renderingframework entwickelt wird. Sehen wir uns an, wie das Beispiel Spiel die Spieleszene mithilfe von Direct3D 11 rendert.

Direct3D 11 enthält eine Reihe von APIs, die den Zugriff auf die erweiterten Funktionen von Hochleistungs Grafikhardware ermöglichen, die zum Erstellen von 3D-Grafiken für grafikintensive Anwendungen wie Spiele verwendet werden kann.

Das Rendern von Spielgrafiken auf dem Bildschirm bedeutet im Grunde, dass Sie eine Sequenz von Frames auf dem Bildschirm In jedem Frame müssen Sie Objekte, die in der Szene sichtbar sind, auf der Grundlage der Ansicht Rendering.

Um einen Frame zu rendereinigen, müssen Sie die erforderlichen szeneninformationen an die Hardware übergeben, damit Sie auf dem Bildschirm angezeigt werden können. Wenn Sie auf dem Bildschirm etwas anzeigen möchten, müssen Sie das Rendering starten, sobald das Spiel gestartet wird.

## <a name="objectives"></a>Ziele

Zum Einrichten eines grundlegenden renderingframe Works, um die Grafikausgabe für ein UWP DirectX-Spiel anzuzeigen. Diese drei Schritte können Sie locker unterbrechen.

1. Stellen Sie eine Verbindung mit der Grafikschnittstelle her.
2. Erstellen Sie die zum Zeichnen der Grafiken benötigten Ressourcen.
3. Zeigen Sie die Grafiken an, indem Sie den Frame Rendering.

In diesem Thema wird erläutert, wie Grafiken gerendert werden, die Schritte 1 und 3 abdecken.

[Renderingframework II: das Spiel Rendering](tutorial-game-rendering.md) deckt Schritt 2 &mdash; ab, wie das Renderingframework eingerichtet wird und wie Daten vorbereitet werden, bevor das Rendering erfolgen kann.

## <a name="get-started"></a>Erste Schritte

Es ist ratsam, sich mit den grundlegenden Grafiken und renderingkonzepten vertraut zu machen. Wenn Sie noch nicht mit Direct3D und Rendering vertraut sind, finden Sie unter [Begriffe und Konzepte](#terms-and-concepts) eine kurze Beschreibung der in diesem Thema verwendeten Grafiken und renderingbegriffe.

Für dieses Spiel stellt die **gamererererklasse** den Renderer für dieses Beispiel Spiel dar. Er ist dafür verantwortlich, alle Direct3D 11-und Direct2D-Objekte zu erstellen und zu verwalten, die zum Generieren der visuellen Spielelemente verwendet werden. Außerdem verwaltet Sie einen Verweis auf das **Simple3DGame** -Objekt, das zum Abrufen der Liste der zu Rendering enden Objekte verwendet wird, sowie den Status des Spiels für die Heads-Up-Anzeige (HUD).

In diesem Teil des Tutorials konzentrieren wir uns auf das Rendern von 3D-Objekten im Spiel.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Herstellen einer Verbindung mit der Grafikschnittstelle

Informationen zum Zugreifen auf die Hardware für das Rendering finden Sie im Thema [Definieren des UWP-App-Frameworks des Spiels](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method) .

### <a name="the-appinitialize-method"></a>Die APP:: Initialize-Methode

Die **Std:: make_shared** -Funktion, wie unten gezeigt, wird verwendet, um eine **shared_ptr** für **DX::D eviceresources**zu erstellen, die auch Zugriff auf das Gerät ermöglicht.

In Direct3D 11 wird ein [Gerät](#device) zum Zuordnen und zerstören von Objekten, zum renderprimitiv und zum kommunizieren mit der Grafikkarte über den Grafiktreiber verwendet.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Anzeigen der Grafiken durch Rendern des Frames

Die Spielszene muss beim Start des Spiels dargestellt werden. Die Anweisungen zum Rendern beginnen in der [**gamemain:: Run**](#gamemainrun-method) -Methode, wie unten gezeigt.

Dies ist der einfache Fluss.

1. **Update**
2. **Render**
3. **Anzahl**

### <a name="gamemainrun-method"></a>Gamemain:: Run-Methode

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

### <a name="update"></a>Aktualisieren

Weitere Informationen zur Aktualisierung von Spiel Zuständen in der [**gamemain:: Update**](tutorial-game-flow-management.md#the-gamemainupdate-method) -Methode finden Sie im Thema [Game Flow Management](tutorial-game-flow-management.md) .

### <a name="render"></a>Rendern

Das Rendering wird durch Aufrufen der [**gamerenderer:: Rendering**](#gamerendererrender-method) -Methode aus **gamemain:: Run**implementiert.

Wenn das [Stereo Rendering](#stereo-rendering) aktiviert ist, gibt es zwei Renderingvorgänge &mdash; für das linke und eine für die Rechte. Bei jedem Renderingdurchlauf binden wir das Renderziel und die Ansicht der tiefen Schablone an das Gerät. Wir löschen außerdem die Ansicht der tiefen Schablone.

> [!NOTE]
> Das Stereo Rendering kann mithilfe anderer Methoden, wie z. b. Einzelpass Stereo, mithilfe von Scheitelpunkt Instanzen oder Geometry-Shadern erreicht werden. Die Two-Rendering-übergibt-Methode ist eine langsamere, aber bequemere Methode zum Erreichen von Stereo-Rendering.

Nachdem das Spiel ausgeführt wurde und Ressourcen geladen wurden, aktualisieren wir die [Projektions Matrix](#projection-transform-matrix), einmal pro Renderingdurchlauf. -Objekte unterscheiden sich geringfügig von den einzelnen Ansichten. Als nächstes richten Sie die [Grafik Rendering-Pipeline](#rendering-pipeline)ein. 

> [!NOTE]
> Weitere Informationen zum Laden von Ressourcen finden Sie unter [Erstellen und Laden von DirectX-Grafik Ressourcen](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) .

In diesem Beispiel Spiel ist der Renderer so konzipiert, dass er ein Standardmäßiges Scheitelpunkt Layout für alle Objekte verwendet. Dies vereinfacht den Shader-Entwurf und ermöglicht einfache Änderungen zwischen Shadern, unabhängig von der Geometrie der Objekte.

#### <a name="gamerendererrender-method"></a>Gamerderderer:: Rendering-Methode

Der Direct3D-Kontext wird so festgelegt, dass ein Eingabe Scheitelpunkt Layout verwendet wird. Eingabe-Layoutobjekte beschreiben, wie Vertex-Puffer Daten in die [Renderingpipeline](#rendering-pipeline)gestreamt werden. 

Als nächstes legen wir den Direct3D-Kontext so fest, dass die zuvor definierten Konstanten Puffer verwendet werden, die von der [Vertex-Shader](#vertex-shaders-and-pixel-shaders) -Pipeline Stufe und der [Pixelshader](#vertex-shaders-and-pixel-shaders) -Pipeline Phase verwendet werden. 

> [!NOTE]
> Weitere Informationen zur Definition der Konstanten Puffer finden Sie unter [Rendering Framework II: Game Rendering](tutorial-game-rendering.md) .

Da das Eingabe Layout und der Satz konstanter Puffer für alle Shader in der Pipeline verwendet werden, wird es einmal pro Frame eingerichtet.

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

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

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
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
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>Primitives Rendering

Beim Rendern der Szene durchlaufen Sie alle Objekte, die gerendert werden müssen. Die folgenden Schritte werden für jedes Objekt (primitiv) wiederholt.

- Aktualisieren Sie den konstanten Puffer (**m_constantBufferChangesEveryPrim**) mit der [World Transformation Matrix](#world-transform-matrix) und den Material Informationen des Modells.
- Die **m_constantBufferChangesEveryPrim** enthält Parameter für jedes Objekt. Sie enthält die Transformation für Objekt-zu-Welt-Transformation sowie Materialeigenschaften wie Farbe und Glanz Exponent für Beleuchtungsberechnungen.
- Legen Sie den Direct3D-Kontext fest, um das Eingabe Vertex-Layout für die Mesh-Objektdaten zu verwenden, die in die Eingabe-Assembler-Phase (IA) der [Renderingpipeline](#rendering-pipeline)gestreamt werden.
- Legen Sie den Direct3D-Kontext für die Verwendung eines [Index Puffers](#index-buffer) in der IA-Phase fest. Geben Sie die primitiven Informationen an: Typ, Datenreihen Folge.
- Senden Sie einen zeichnen-Befehl, um den indizierten, nicht instanzierten primitiven zu zeichnen. Die **gameobject:: Rendering** -Methode aktualisiert den primitiven [Konstanten Puffer](#constant-buffer-or-shader-constant-buffer) mit den Daten, die für einen bestimmten primitiven spezifisch sind. Dies führt zu einem **drawindexed** -Rückruf für den Kontext, um die Geometrie der einzelnen primitiven zu zeichnen. Mit diesem zeichnen-Befehl werden Befehle und Daten in der GPU (Graphics Processing Unit) als parametrisiert durch die Konstanten Puffer Daten in die Warteschlange eingereiht. Jeder Draw-Aufruf führt den Vertex-Shader einmal pro Scheitelpunkt aus, und der [Pixel-Shader](#vertex-shaders-and-pixel-shaders) einmal für jedes Pixel jedes Dreiecks im primitiven. Die Texturen sind Teil des Zustands, den der Pixelshader für das Rendering verwendet.

Dies sind die Gründe für die Verwendung mehrerer konstanter Puffer.

- Das Spiel verwendet mehrere Konstante Puffer, aber es muss nur einmal pro primitiver Zeit aktualisiert werden. Wie bereits erwähnt, entsprechen Konstante Puffer den Eingaben für die Shader, die für die einzelnen primitiven ausgeführt werden. Einige Daten sind statisch (**m_constantBufferNeverChanges**). einige Daten sind über dem Frame (**m_constantBufferChangesEveryFrame**) konstant, wie z. b. die Position der Kamera. und einige Daten sind spezifisch für die primitive, z. b. Farbe und Texturen (**m_constantBufferChangesEveryPrim**).
- Der Spielrenderer teilt diese Eingaben in verschiedene Konstantenpuffer auf, um die von CPU und GPU verwendete Speicherbandbreite zu optimieren. Dieser Ansatz hilft auch, die Datenmenge zu minimieren, die die GPU nachverfolgen muss. Die GPU verfügt über eine große Warteschlange von Befehlen, und jedes Mal, wenn das Spiel **Draw**aufruft, wird dieser Befehl zusammen mit den zugeordneten Daten in die Warteschlange eingereiht. Wenn das Spiel den Grundtypkonstantenpuffer aktualisiert und den nächsten **Draw**-Befehl ausgibt, fügt der Grafiktreiber diesen Befehl und die dazugehörigen Daten der Warteschlange hinzu. Zeichnet das Spiel 100 Grundtypen, kann die Warteschlange 100 Kopien der Konstantenpufferdaten enthalten. Um die Datenmenge zu minimieren, die das Spiel an die GPU sendet, verwendet das Spiel einen separaten primitiven Konstanten Puffer, der nur die Updates für die einzelnen primitiven enthält.

#### <a name="gameobjectrender-method"></a>Gameobject:: Rendering-Methode

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
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
```

#### <a name="meshobjectrender-method"></a>Meshobject:: Rendering-Methode

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>Deviceresources::P Resent-Methode

Wir nennen die **deviceresources::P Resent** -Methode, um den Inhalt anzuzeigen, den wir in den Puffern abgelegt haben.

Wir verwenden den Begriff SwapChain für eine Sammlung von Puffern, die zum Anzeigen von Frames für den Benutzer verwendet werden. Jedes Mal, wenn eine Anwendung einen neuen Frame für die Anzeige darstellt, wird der erste Puffer in der SwapChain an der Stelle des angezeigten Puffers angezeigt. Dieser Prozess wird als Austausch oder Flipping bezeichnet. Weitere Informationen finden Sie unter [austauschen von Ketten](../graphics-concepts/swap-chains.md).

- Die **Present** -Methode der **IDXGISwapChain1** -Schnittstelle weist [DXGI](#dxgi) an, bis zur vertikalen Synchronisierung (VSYNC) zu blockieren, sodass die Anwendung bis zur nächsten VSYNC in den Standbymodus versetzt wird. Dadurch wird sichergestellt, dass keine Zyklen zum Rendern von Frames verschwendet werden, die nie auf dem Bildschirm angezeigt werden.
- Die **verwerdview** -Methode der **ID3D11DeviceContext3** -Schnittstelle verwirft den Inhalt des [Renderziels](#render-target). Dies ist nur dann ein gültiger Vorgang, wenn die vorhandenen Inhalte vollständig überschrieben werden. Wenn geänderte oder scrollläufe verwendet werden, sollte dieser-Befehl entfernt werden.
* Verwerfen Sie den Inhalt der [tiefen Schablone](#depth-stencil)mithilfe derselben **verwerfen View** -Methode.
* Die Methode " **Lenker** " wird verwendet, um das Szenario des [Geräts](#device) zu verwalten, das entfernt wird. Wenn das Gerät entweder durch eine Verbindungs Trennung oder ein Treiber Upgrade entfernt wurde, müssen Sie alle Geräte Ressourcen neu erstellen. Weitere Informationen finden Sie unter [Behandeln von entfernten Geräte Szenarien in Direct3D 11](handling-device-lost-scenarios.md).

> [!TIP]
> Um eine reibungslose Framerate zu erzielen, müssen Sie sicherstellen, dass der Arbeitsaufwand zum Rendering eines Frames in den Zeitraum zwischen vsyncs passt.

```cppwinrt
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
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema wurde erläutert, wie Grafiken auf der Anzeige gerendert werden, und es wird eine kurze Beschreibung für einige der verwendeten renderingbegriffe (unten) bereitgestellt. Weitere Informationen zum Rendering finden Sie im Thema [Renderingframework II: Game Rendering](tutorial-game-rendering.md) . Sie erfahren, wie Sie die vor dem Rendering benötigten Daten vorbereiten.

## <a name="terms-and-concepts"></a>Begriffe und Konzepte

### <a name="simple-game-scene"></a>Einfache Spielszene

Eine einfache Spielszene besteht aus einigen wenigen Objekten mit mehreren Lichtquellen.

Die Form eines Objekts wird durch einen Satz von X-, Y-, Z-Koordinaten im Raum definiert. Die tatsächliche Renderposition in der Spiel Welt kann durch Anwenden einer Transformationsmatrix auf die Positions-X-, Y-, Z-Koordinaten festgelegt werden. Es kann auch eine Reihe von Texturkoordinaten &mdash; Sie und V aufweisen &mdash; , die angeben, wie ein Material auf das Objekt angewendet wird. Dadurch werden die Oberflächeneigenschaften des Objekts definiert, und Sie können sehen, ob ein Objekt über eine grobe Oberfläche (z. b. eine Tennis Kugel) oder eine glatte Glanz Fläche (wie eine Kegel Kugel) verfügt.

Szenen-und Objektinformationen werden vom Renderingframework verwendet, um den Szene Frame nach Frame neu zu erstellen, sodass er auf dem Bildschirm angezeigt wird.

### <a name="rendering-pipeline"></a>Renderingpipeline

Die Renderingpipeline ist der Prozess, mit dem 3D-szeneninformationen in ein auf dem Bildschirm angezeigtes Bild übersetzt werden. In Direct3D 11 ist diese Pipeline programmierbarer. Sie können die Stufen an die Unterstützung ihrer Renderinganforderungen anpassen. Phasen mit allgemeinen shaderkernen können mithilfe der HLSL-Programmiersprache Programmiersprache verwendet werden. Sie wird auch als *Grafik Rendering-Pipeline*oder einfach als *Pipeline*bezeichnet.

Damit Sie diese Pipeline erstellen können, müssen Sie mit diesen Details vertraut sein.

- [HLSL](#hlsl). Es wird empfohlen, das HLSL-Shader-Modell 5,1 und höher für UWP DirectX-Spiele zu verwenden.
- [Shader](#shaders).
- [Vertex-Shader und Pixel-Shader](#vertex-shaders-and-pixel-shaders).
- [Shader-Stufen](#shader-stages).
- [Verschiedene Shader-Dateiformate](#various-shader-file-formats).

Weitere Informationen finden Sie Untergrund Legendes [zur Direct3D 11-Renderingpipeline](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline) und [Grafik Pipeline](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="hlsl"></a>HLSL

HLSL ist die allgemeine Shader-Sprache für DirectX. Mit HLSL können Sie C-ähnliche programmierbare Shader für die Direct3D-Pipeline erstellen. Weitere Informationen finden Sie unter [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl).

#### <a name="shaders"></a>Shader

Ein Shader kann als Satz von Anweisungen betrachtet werden, die bestimmen, wie die Oberfläche eines Objekts beim Rendern angezeigt wird. Solche, die mit HLSL programmiert werden, werden als HLSL-Shader bezeichnet. Quell Code Dateien für [HLSL]) (#HLSL)-Shader haben die `.hlsl` Dateierweiterung. Diese Shader können zur Laufzeit oder zur Laufzeit kompiliert und zur Laufzeit in die entsprechende Pipeline Phase festgelegt werden. Ein kompiliertes Shader-Objekt verfügt über eine `.cso` Dateierweiterung.

Direct3D 9-Shader können mit Shadermodell 1, Shader Model 2 und Shader Model 3 entworfen werden. Direct3D 10-Shader können nur auf Shadermodell 4 ausgelegt werden. Direct3D 11-Shader können auf Shader-Modell 5 entworfen werden. Direct3D 11,3 und Direct3D 12 können auf Shader-Modell 5,1 entworfen werden, und Direct3D 12 kann auch auf Shader-Modell 6 entworfen werden.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Vertex-Shader und Pixel-Shader

Die Daten werden als Datenstrom von primitiven in die Grafik Pipeline eingegeben und von verschiedenen Shadern, wie z. b. den Vertex-Shadern und Pixel-Shadern, verarbeitet. 

Vertex-Shader verarbeiten Scheitel Punkte, wobei in der Regel Vorgänge wie Transformationen, Skinning und Beleuchtung durchgeführt werden. Pixel-Shader ermöglichen umfassende Schattierungs Techniken wie z. b. pro Pixel-Beleuchtung und Nachbearbeitung. Es kombiniert Konstante Variablen, Textur Daten, interpoliert pro Scheitelpunkt Werte und andere Daten, um pro-Pixel-Ausgaben zu liefern. 

#### <a name="shader-stages"></a>Shaderphasen

Eine Sequenz dieser verschiedenen Shader, die für die Verarbeitung dieses Datenstroms definiert ist, wird in einer Renderingpipeline als Shader-Stufen bezeichnet. Die tatsächlichen Phasen sind von der Direct3D-Version abhängig, enthalten jedoch in der Regel die Scheitelpunkt-, Geometrie-und Pixel Stufen. Es gibt auch andere Stufen, z. b. die Hülle und die Domänen-Shader für Mosaik und den Compute-Shader. Alle diese Phasen sind mithilfe von [HLSL](#hlsl)vollständig programmierbar. Weitere Informationen finden Sie unter [Grafik Pipeline](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="various-shader-file-formats"></a>Verschiedene shaderdateiformate

Hier sind die Shader-Code Dateierweiterungen.

- Eine Datei mit der `.hlsl` Erweiterung enthält den Quellcode [HLSL]) (#HLSL).
- Eine Datei mit der `.cso` Erweiterung enthält ein kompiliertes Shader-Objekt.
- Eine Datei mit der- `.h` Erweiterung ist eine Header Datei, aber in einem Shader-Code Kontext definiert diese Header Datei ein Bytearray, das Shader-Daten enthält.
- Eine Datei mit der `.hlsli` Erweiterung enthält das Format der Konstanten Puffer. Im Beispiel Spiel ist die Datei **Shader**  >  **constantbuffers. hlsli**.

> [!NOTE]
> Sie Betten einen Shader ein, indem Sie entweder eine `.cso` Datei zur Laufzeit laden oder eine `.h` Datei in Ihrem ausführbaren Code hinzufügen. Aber Sie würden nicht beides für denselben Shader verwenden.

### <a name="deeper-understanding-of-directx"></a>Tieferes Verständnis von DirectX

Direct3D 11 ist ein Satz von APIs, mit denen Sie Grafiken für grafikintensive Anwendungen erstellen können, wie z. b. Spiele, bei denen wir eine gute Grafikkarte zur Verarbeitung intensiver Berechnungen benötigen. In diesem Abschnitt werden die Direct3D 11-Grafik Programmier Konzepte kurz erläutert: Ressource, unter Bericht, Gerät und Gerätekontext.

#### <a name="resource"></a>Resource

Sie können sich Ressourcen (auch als Geräte Ressourcen bezeichnet) als Informationen zum Rendering eines Objekts vorstellen, wie z. b. Textur, Position oder Farbe. Ressourcen stellen Daten für die Pipeline bereit und definieren, was während der Szene gerendert wird. Ressourcen können von ihren Spiel Medien geladen oder dynamisch zur Laufzeit erstellt werden.

Eine Ressource ist tatsächlich ein Bereich im Arbeitsspeicher, auf den von der Direct3D- [Pipeline](#rendering-pipeline)aus zugegriffen werden kann. Damit die Pipeline effizient auf den Speicher zugreifen kann, müssen für die Pipeline bereitgestellte Daten (etwa Eingabegeometrie, Shaderressourcen und Texturen) in einer Ressource gespeichert werden. Es gibt zwei Arten von Ressourcen, aus denen alle Direct3D-Ressourcen abgeleitet sind: Puffer und Textur. Für jede Pipelinephase können bis zu 128 Ressourcen aktiv sein. Weitere Informationen finden Sie unter [Ressourcen](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Unterressource

Der Begriff "subresource" verweist auf eine Teilmenge einer Ressource. Direct3D kann auf eine gesamte Ressource verweisen, oder Sie kann auf Teilmengen einer Ressource verweisen. Weitere Informationen finden Sie unter [subresource](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Tiefen Schablone

Eine tiefen Schablone-Ressource enthält das Format und den Puffer, um Tiefe und Schablonen Informationen zu speichern. Sie wird mit einer Textur Ressource erstellt. Weitere Informationen zum Erstellen einer tiefen Schablone-Ressource finden Sie unter [Konfigurieren der tiefen Schablone-Funktionalität](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil). Wir greifen mithilfe der tiefen Schablone, die mithilfe der [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) -Schnittstelle implementiert wird, auf die Tiefe der tiefen Schablone zu.

Ausführliche Informationen gibt Aufschluss darüber, welche Bereiche von Polygonen sich hinter anderen befinden, damit wir bestimmen können, welche ausgeblendet sind. Schablone Info gibt Aufschluss darüber, welche Pixel maskiert werden. Sie kann verwendet werden, um besondere Effekte zu erzeugen, da Sie bestimmt, ob ein Pixel gezeichnet wird. legt das-Bit auf 1 oder 0 fest. 

Weitere Informationen finden Sie unter [Ansicht der tiefen Schablone](../graphics-concepts/depth-stencil-view--dsv-.md), [tiefen Puffer](../graphics-concepts/depth-buffers.md)und [Schablonen Puffer](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Renderziel

Ein Renderziel ist eine Ressource, in die wir am Ende eines Renderpass schreiben können. Sie wird in der Regel mithilfe der [ID3D11Device:: samaterendertargetview](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) -Methode erstellt, wobei der SwapChain-Hintergrund Puffer (bei dem es sich auch um eine Ressource handelt) als Eingabeparameter verwendet wird. 

Jedes Renderziel sollte auch über eine entsprechende Ansicht der tiefen Schablone verfügen, denn wenn wir " [omsetrendertargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) " verwenden, um das Renderziel vor der Verwendung festzulegen, erfordert es auch eine Ansicht der tiefen Schablone. Wir greifen über die renderzielansicht auf die renderzielressource zu, die mithilfe der [ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) -Schnittstelle implementiert 

#### <a name="device"></a>Gerät

Sie können sich ein Gerät als Möglichkeit vorstellen, Objekte zuzuordnen und zu zerstören, primitive zu renderalen und mit der Grafikkarte über den Grafiktreiber zu kommunizieren. 

Ein Direct3D-Gerät ist die renderingkomponente von Direct3D, um eine genauere Erläuterung zu erhalten. Ein Gerät kapselt und speichert den Renderstatus, führt Transformationen und Beleuchtungsvorgänge aus und rastert ein Bild auf einer Oberfläche. Weitere Informationen finden Sie unter [Geräte](../graphics-concepts/devices.md) .

Ein Gerät wird durch die [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) -Schnittstelle dargestellt. Mit anderen Worten: die **ID3D11Device** -Schnittstelle stellt einen virtuellen Anzeige Adapter dar und wird verwendet, um Ressourcen zu erstellen, die sich im Besitz eines Geräts befinden. 

Es gibt verschiedene Versionen von ID3D11Device. [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5) ist die neueste Version und fügt den in **ID3D11Device4**neuen Methoden hinzu. Weitere Informationen dazu, wie Direct3D mit der zugrunde liegenden Hardware kommuniziert, finden Sie unter [Windows-Gerätetreiber Modell (WDDM)-Architektur](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Jede Anwendung muss über mindestens ein Gerät verfügen. von den meisten Anwendungen wird nur eine erstellt. Erstellen Sie ein Gerät für einen der Hardwaretreiber, die auf Ihrem Computer installiert sind, indem Sie **D3D11CreateDevice** oder **D3D11CreateDeviceAndSwapChain** aufrufen und den Treibertyp mit dem **D3D_DRIVER_TYPE** -Flag angeben. Jedes Gerät kann abhängig von der gewünschten Funktionalität einen oder mehrere Geräte Kontexte verwenden. Weitere Informationen finden Sie unter [D3D11CreateDevice-Funktion](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

#### <a name="device-context"></a>Gerätekontext

Ein Gerätekontext wird verwendet, um den [Pipeline](#rendering-pipeline) Status festzulegen und renderingbefehle mithilfe der [Ressourcen](#resource) zu generieren, die im Besitz eines [Geräts](#device)sind. 

Direct3D 11 implementiert zwei Arten von Geräte Kontexten, eine für sofortiges Rendering und die andere für verzögertes Rendering. Beide Kontexte werden mit einer [Verknüpfung id3d11devicecontext aus](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) -Schnittstelle dargestellt. 

Die **Verknüpfung id3d11devicecontext aus** -Schnittstellen haben unterschiedliche Versionen. **ID3D11DeviceContext4** fügt den in **ID3D11DeviceContext3**neue Methoden hinzu.

**ID3D11DeviceContext4** wird in Windows 10 Creators Update eingeführt und ist die neueste Version der **Verknüpfung id3d11devicecontext aus** -Schnittstelle. Anwendungen, die auf Windows 10 Creators Update und höher abzielen, sollten diese Schnittstelle anstelle früherer Versionen verwenden. Weitere Informationen finden Sie unter [ID3D11DeviceContext4](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4).

#### <a name="dxdeviceresources"></a>DX::D eviceresources

Die **DX::D eviceresources** -Klasse befindet sich in den Dateien " **deviceresources. cpp** / **. h** " und steuert alle DirectX-Geräte Ressourcen.

### <a name="buffer"></a>Buffer

Eine Puffer Ressource ist eine Auflistung von vollständig typisierten Daten, die in-Elementen gruppiert sind. Sie können Puffer verwenden, um eine Vielzahl von Daten zu speichern, z. b. Positions Vektoren, normale Vektoren, Texturkoordinaten in einem Vertex-Puffer, Indizes in einem Index Puffer oder einen Gerätezustand. Puffer Elemente können gepackte Datenwerte (z. b. **R8G8B8A8** Surface Values), einzelne 8-Bit-Ganzzahlen oder Gleit Komma Werte mit 4 32 Bit enthalten.

Es sind drei Puffer Typen verfügbar: Vertex-Puffer, Index Puffer und konstanter Puffer.

#### <a name="vertex-buffer"></a>Vertex-Puffer

Enthält die Vertex-Daten, die zum Definieren der Geometrie verwendet werden. Vertex-Daten enthalten Positionskoordinaten, Farbdaten, Texturkoordinaten Daten, normale Daten usw. 

#### <a name="index-buffer"></a>Index Puffer

Enthält ganzzahlige Offsets in Scheitelpunkt Puffer und wird zum effizienteren Rendering von primitiven verwendet. Ein Index Puffer enthält einen sequenziellen Satz von 16-Bit-oder 32-Bit-Indizes. Jeder Index wird zum Identifizieren eines Scheitel Punkts in einem Scheitelpunkt Puffer verwendet.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Konstanter Puffer oder Shader-Konstantenpuffer

Ermöglicht das effiziente Bereitstellen von Shader-Daten an die Pipeline. Sie können Konstante Puffer als Eingaben für die Shader verwenden, die für die einzelnen primitiven ausgeführt werden, und die Ergebnisse der Stream-Output-Phase der Renderingpipeline speichern. Konzeptionell sieht ein konstanter Puffer genau wie ein Vertex-Puffer mit einem einzelnen Element aus.

#### <a name="design-and-implementation-of-buffers"></a>Entwurf und Implementierung von Puffern

Sie können Puffer auf der Grundlage des-Datentyps entwerfen, z. b. wie in unserem Beispiel Spiel, ein Puffer für statische Daten erstellt wird, ein weiterer für Daten, die über den Frame konstant sind, und ein anderer für Daten, die spezifisch für ein primitiv sind.

Alle Puffer Typen werden von der **ID3D11Buffer** -Schnittstelle gekapselt, und Sie können durch Aufrufen von **ID3D11Device:: createbuffer**eine Puffer Ressource erstellen. Ein Puffer muss jedoch an die Pipeline gebunden werden, bevor darauf zugegriffen werden kann. Puffer können gleichzeitig an mehrere Pipeline Stufen zum Lesen gebunden werden. Ein Puffer kann auch an eine einzelne Pipeline Phase zum Schreiben gebunden werden. Allerdings kann derselbe Puffer nicht gleichzeitig für Lese-und Schreibvorgänge gebunden werden.

Sie können Puffer auf diese Weise binden.

- In der Eingabe-Assembler-Phase durch Aufrufen von **Verknüpfung id3d11devicecontext aus** -Methoden wie **Verknüpfung id3d11devicecontext aus:: iasetvertexbuffers** und **Verknüpfung id3d11devicecontext aus:: iasetindexbuffer**.
- In die Stream-Output-Phase durch Aufrufen von **Verknüpfung id3d11devicecontext aus:: sosettargets**.
- In die Shader-Phase durch Aufrufen von Shader-Methoden, z. **b. Verknüpfung id3d11devicecontext aus:: vssetconstantbuffers**.

Weitere Informationen finden Sie unter [Introduction to Buffers in Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro).

### <a name="dxgi"></a>DXGI

Bei Microsoft DirectX Graphics Infrastructure (DXGI) handelt es sich um ein Subsystem, das einige der Low-Level-Aufgaben kapselt, die von Direct3D benötigt werden. Bei der Verwendung von DXGI in einer Multithread-Anwendung muss besonders darauf geachtet werden, dass keine Deadlocks auftreten. Weitere Informationen finden Sie unter [Multithreading und DXGI](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi) .

### <a name="feature-level"></a>Funktionsebene

Die Featureebene ist ein Konzept, das in Direct3D 11 eingeführt wurde und die Vielfalt von Grafikkarten auf neuen und vorhandenen Computern behandelt. Eine Featureebene ist ein klar definierter Satz von GPU-Funktionen (Graphics Processing Unit). 

Jede Grafikkarte implementiert abhängig von den installierten GPUs eine bestimmte Ebene der DirectX-Funktionalität. In früheren Versionen von Microsoft Direct3D konnten Sie die Version von Direct3D, die die Videokarte implementiert hat, ermitteln und dann die Anwendung entsprechend programmieren. 

Wenn Sie ein Gerät erstellen, können Sie mit der Featureebene versuchen, ein Gerät für die gewünschte Funktionsebene zu erstellen. Wenn die Geräte Erstellung funktioniert, ist diese Featureebene vorhanden. Falls nicht, wird diese Featureebene von der Hardware nicht unterstützt. Sie können entweder versuchen, ein Gerät auf einer niedrigeren Funktionsebene neu zu erstellen, oder Sie können die Anwendung beenden. Zum Beispiel erfordert die 12_0-Featureebene Direct3D 11,3 oder Direct3D 12 und das Shader-Modell 5,1. Weitere Informationen finden Sie unter [Direct3D Feature Levels: Overview for each Feature Level](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

Mithilfe von featurestufen können Sie eine Anwendung für Direct3D 9, Microsoft Direct3D 10 oder Direct3D 11 entwickeln und dann auf der Hardware mit 9, 10 oder 11 ausführen (mit einigen Ausnahmen). Weitere Informationen finden Sie unter [Direct3D Feature Levels](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

### <a name="stereo-rendering"></a>Stereo-Rendering

Das Stereo Rendering wird verwendet, um die Illusion von Tiefe zu verbessern. Es verwendet zwei Bilder, eines von Links und das andere von der rechten Seite, um eine Szene auf dem Bildschirm anzuzeigen. 

Mathematisch wird eine Stereo Projektions Matrix angewendet, bei der es sich um einen geringfügigen horizontalen Offset rechts und Links von der regulären Mono-Projektions Matrix handelt, um dies zu erreichen.

Wir haben zwei Renderingdurchläufen durchgeführt, um in diesem Beispiel Spiel das Stereo-Rendering

- An das Rechte Renderziel binden, die richtige Projektion anwenden und dann das primitive Objekt zeichnen.
- An linkes Renderziel binden, linke Projektion anwenden und dann das primitive Objekt zeichnen.

### <a name="camera-and-coordinate-space"></a>Kamera-und Koordinatenraum

Das Spiel enthält den erforderlichen Code, um die Spielwelt in seinem eigenen Koordinatensystem (auch als Spielweltbereich oder Szenenbereich bezeichnet) zu aktualisieren. Alle Objekte, einschließlich der Kamera, werden in diesem Bereich positioniert und ausgerichtet. Weitere Informationen finden Sie unter [Koordinatensysteme](../graphics-concepts/coordinate-systems.md).

Ein Vertex-Shader führt die große Arbeit bei der Umstellung von den Modell Koordinaten in Geräte Koordinaten mit dem folgenden Algorithmus durch (wobei V ein Vektor und M eine Matrix ist).

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`ist eine Transformationsmatrix für Modell Koordinaten zu globalen Koordinaten, die auch als " [World Transform Matrix](#world-transform-matrix)" bezeichnet wird. Diese Matrix wird vom Grundtyp bereitgestellt.
- `M(world-to-view)`ist eine Transformationsmatrix für globale Koordinaten zum Anzeigen von Koordinaten. Diese werden auch als [Ansichts Transformationsmatrix](#view-transform-matrix)bezeichnet.
  - Diese Matrix wird von der Ansichtsmatrix der Kamera bereitgestellt. Die Position wird durch die Position der Kamera und die Aussehens Vektoren definiert (das *sehen* Sie sich den Vektor an, der direkt auf die Szene von der Kamera zeigt, und der Such Vektor, der sich aufwärts senkrecht *zu der Szene* befindet).
  - Im Beispiel Spiel ist **m_viewMatrix** die Transformationsmatrix der Sicht und wird mithilfe von **Kamera:: setviewparametriams**berechnet.
- `M(view-to-device)`ist eine Transformationsmatrix für Ansichts Koordinaten zu Geräte Koordinaten, auch als [Projektions Transformationsmatrix](#projection-transform-matrix)bezeichnet.
  - Diese Matrix wird von der Projektion der Kamera bereitgestellt. Es enthält Informationen darüber, wie viel von diesem Bereich in der endgültigen Szene tatsächlich sichtbar ist. Das Feld of View (FOV), das Seitenverhältnis und die Clippingebenen definieren die Projektions Transformationsmatrix.
  - Im Beispiel Spiel definiert **m_projectionMatrix** die Transformation zu den Projektions Koordinaten, die mithilfe von " **Camera:: setprojparameams** " berechnet werden (bei der Stereo Projektion verwenden Sie zwei Projektions Matrizen &mdash; für jede Ansicht eines Auges).

Der Shader-Code in `VertexShader.hlsl` wird mit diesen Vektoren und Matrizen aus den konstanten Puffern geladen und führt diese Transformation für jeden Scheitelpunkt aus.

### <a name="coordinate-transformation"></a>Koordinatentransformation

Direct3D verwendet drei Transformationen, um die 3D-Modell Koordinaten in Pixelkoordinaten (Bildschirmraum) zu ändern. Diese Transformationen sind Welt Transformation, Ansichts Transformation und Projektions Transformation. Weitere Informationen finden Sie unter [Transformations Übersicht](../graphics-concepts/transform-overview.md).

#### <a name="world-transform-matrix"></a>World Transform Matrix

Eine Welt transformiert Änderungen an den Koordinaten des Modell Raums, wobei Scheitel Punkte in Bezug auf den lokalen Ursprung eines Modells definiert werden, in den Raum, in dem Vertices relativ zu einem Ursprung definiert werden, der von allen Objekten in einer Szene gemeinsam genutzt wird. Im Grunde stellt die Welt Transformation ein Modell in die Welt. Daher ist der Name. Weitere Informationen finden Sie unter [Welt Transformation](../graphics-concepts/world-transform.md).

#### <a name="view-transform-matrix"></a>Transformationsmatrix anzeigen

Die Transformation "Ansicht" lokalisiert den Viewer im Raum und wandelt Vertices in den Kamerabereich um. Im Kamerabereich befindet sich die Kamera oder der Viewer am Ursprung und sucht in der positiven z-Richtung. Weitere Informationen finden Sie unter [View Transform](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Projektions Transformationsmatrix

Die Projektions Transformation konvertiert die Anzeige "Frustum" in eine Cuboid-Form. Ein Anzeige Wert ist ein 3D-Volume in einer Szene, das relativ zur Kamera des Viewports positioniert ist. Bei einem Viewport handelt es sich um ein 2D-Rechteck, in das eine 3D-Szene projiziert wird. Weitere Informationen finden Sie unter [Viewports und Clipping](../graphics-concepts/viewports-and-clipping.md) .

Da das Near-Ende der Anzeige von Frustum kleiner als das Ende ist, wirkt sich dies auf das Erweitern von Objekten aus, die sich in der Nähe der Kamera befinden. auf diese Weise wird die Perspektive auf die Szene angewendet. Daher werden Objekte, die sich näher an dem Player befinden, größer angezeigt. andere Objekte, die weiter entfernt werden, werden kleiner angezeigt.

Mathematisch ist die Projektions Transformation eine Matrix, bei der es sich in der Regel um eine Skalierungs-und perspektivische Projektion handelt. Es funktioniert wie die Kamera einer Kamera. Weitere Informationen finden Sie unter [Projektions Transformation](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>Samplerstatus

Der samplerzustand bestimmt, wie Textur Daten mithilfe von Textur Adressierungs Modi, Filterung und Detailgenauigkeit entnommen werden. Die Stichprobenentnahme erfolgt jedes Mal, wenn ein Textur Pixel (oder Texel) aus einer Textur gelesen wird.

Eine Textur enthält ein Array von texeln. Die Position jedes Texts wird durch angegeben `(u,v)` , wobei `u` die Breite und `v` die Höhe ist, und wird auf der Grundlage der Textur Breite und-Höhe zwischen 0 und 1 zugeordnet. Die resultierenden Texturkoordinaten werden verwendet, um bei der Stichprobenentnahme einer Textur ein Texturieren.

Wenn Texturkoordinaten unter 0 oder über 1 liegen, definiert der Textur Adress Modus, wie die Textur Koordinate eine texturposition adressiert. Wenn Sie z. b **. textuleseschneidermode. Clamp**verwenden, wird jede Koordinate außerhalb des 0-1-Bereichs an einen maximalen Wert von 1 und den minimalen Wert 0 (null) vor der Stichprobenentnahme gebunden.

Wenn die Textur für das Polygon zu groß oder zu klein ist, wird die Textur so gefiltert, dass Sie an den Platz passt. Durch einen Vergrößerungs Filter wird eine Textur vergrößert, ein Minimierung-Filter reduziert die Textur, sodass Sie in einen kleineren Bereich passt. Die Textur Vergrößerung wiederholt das Beispiel textexaus für eine oder mehrere Adressen, die ein blurrier-Bild ergibt. Die Textur Minimierung ist komplizierter, da mehr als ein Texttyp in einem einzelnen Wert kombiniert werden muss. Dies kann abhängig von den Textur Daten zu Aliasing-oder Verzweigungs Kanten führen. Der gängigste Ansatz für die Minimierung ist die Verwendung einer MipMap. Eine MipMap ist eine Textur mit mehreren Ebenen. Die Größe jeder Ebene ist eine Potenz von 2 kleiner als die vorherige Ebene bis zu einer 1 x 1-Textur. Bei Verwendung der minifizierung wählt ein Spiel die MipMap-Ebene aus, die der Größe am nächsten liegt, die zur Rendering-Zeit benötigt wird. 

### <a name="the-basicloader-class"></a>Die basicloader-Klasse

**Basicloader** ist eine einfache Loader-Klasse, die Unterstützung für das Laden von Shader, Texturen und Netzen aus Dateien auf dem Datenträger bietet. Sie stellt synchrone und asynchrone Methoden bereit. In diesem Beispiel Spiel befinden sich die `BasicLoader.h/.cpp` Dateien im Ordner **Hilfsprogramme** .

Weitere Informationen finden Sie unter [Basic Loader](complete-code-for-basicloader.md).