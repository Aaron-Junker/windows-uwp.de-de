---
title: Erstellen von Tiefenpuffer-Geräteressourcen
description: Hier erfahren Sie, wie Sie die zum Unterstützen von Tiefentests für Schattenvolumen erforderlichen Direct3D-Geräteressourcen erstellen.
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Direct3D, Tiefenpuffer
ms.localizationpriority: medium
ms.openlocfilehash: f5ce1ec522a194111e175e41f82c4275cda4fbf5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613695"
---
# <a name="create-depth-buffer-device-resources"></a>Erstellen von Tiefenpuffer-Geräteressourcen




Hier erfahren Sie, wie Sie die zum Unterstützen von Tiefentests für Schattenvolumen erforderlichen Direct3D-Geräteressourcen erstellen. Teil 1 von [Exemplarische Vorgehensweise: Implementieren Sie mithilfe von Tiefenpuffern in Direct3D 11 Schattenvolumen](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="resources-youll-need"></a>Erforderliche Ressourcen


Zum Rendern einer Tiefenkarte für Schattenvolumen benötigen Sie die folgenden geräteabhängigen Direct3D-Ressourcen:

-   Eine Ressource (Puffer) für die Tiefenkarte
-   Eine Tiefenschablonenansicht und eine Shaderressourcenansicht für die Ressource
-   Ein Vergleichs-Samplerstatusobjekt
-   Konstantenpuffer für POV-Beleuchtungsmatrizen
-   Einen Viewport zum Rendern der Schattenkarte (normalerweise ein quadratischer Viewport)
-   Ein Renderstatusobjekt zum Aktivieren von Frontface-Culling
-   Außerdem benötigen Sie ein Renderstatusobjekt, um wieder zum Backface-Culling zu wechseln, falls Sie noch keines verwenden.

Beachten Sie, dass die Erstellung dieser Ressourcen in eine geräteabhängige Ressourcenerstellungsroutine eingebunden werden muss. Auf diese Weise kann Ihr Renderer die Ressourcen neu erstellen, wenn (beispielsweise) ein neuer Gerätetreiber installiert wird oder der Benutzer Ihre App auf einen Monitor verschiebt, der an einen anderen Grafikadapter angeschlossen ist.

## <a name="check-feature-support"></a>Überprüfen unterstützter Features


Rufen Sie vor dem Erstellen der Depth-Zuordnung, die [ **CheckFeatureSupport** ](https://msdn.microsoft.com/library/windows/desktop/ff476497) Methode auf dem Direct3D-Gerät anfordern **D3D11\_FEATURE\_D3D9\_ SCHATTEN\_Unterstützung**, und geben Sie einen [ **D3D11\_FEATURE\_Daten\_D3D9\_SCHATTEN\_Unterstützung** ](https://msdn.microsoft.com/library/windows/desktop/jj247569) Struktur.

```cpp
D3D11_FEATURE_DATA_D3D9_SHADOW_SUPPORT isD3D9ShadowSupported;
ZeroMemory(&isD3D9ShadowSupported, sizeof(isD3D9ShadowSupported));
pD3DDevice->CheckFeatureSupport(
    D3D11_FEATURE_D3D9_SHADOW_SUPPORT,
    &isD3D9ShadowSupported,
    sizeof(D3D11_FEATURE_D3D9_SHADOW_SUPPORT)
    );

if (isD3D9ShadowSupported.SupportsDepthAsTextureWithLessEqualComparisonFilter)
{
    // Init shadow map resources

```

Diese Funktion nicht unterstützt wird, versuchen Sie nicht zum Laden von Shadern, die für Shader Model 4 Level 9 kompiliert\_X, das Aufrufen von Vergleichsfunktionen Beispiel. Eine fehlende Unterstützung für dieses Feature bedeutet in vielen Fällen, dass es sich bei der GPU um ein älteres Gerät mit einem Treiber handelt, der nicht zur Unterstützung von mindestens WDDM 1.2 aktualisiert wurde. Wenn das Gerät unterstützt mindestens Stufe 10 feature\_0, und Sie können einen Beispiel-Vergleich-Shader für Shader Model 4 kompiliert laden\_0 stattdessen.

## <a name="create-depth-buffer"></a>Erstellen des Tiefenpuffers


Versuchen Sie als Erstes, die Tiefenkarte in einem Tiefenformat mit einer höheren Genauigkeit zu erstellen. Richten Sie zuerst die entsprechenden Eigenschaften der Shaderressourcenansicht ein. Falls die Erstellung der Ressource fehlschlägt (z. B. weil zu wenig Gerätespeicher verfügbar ist oder ein Format von der Hardware nicht unterstützt wird), können Sie es mit einem Format mit geringerer Genauigkeit probieren und die Eigenschaften entsprechend ändern.

Dieser Schritt ist optional, wenn Sie nur ein Format mit geringer Genauigkeit Tiefe, z. B. beim Rendern auf mittlere Auflösung Direct3D-Funktionsebene 9\_1 Geräte.

```cpp
D3D11_TEXTURE2D_DESC shadowMapDesc;
ZeroMemory(&shadowMapDesc, sizeof(D3D11_TEXTURE2D_DESC));
shadowMapDesc.Format = DXGI_FORMAT_R24G8_TYPELESS;
shadowMapDesc.MipLevels = 1;
shadowMapDesc.ArraySize = 1;
shadowMapDesc.SampleDesc.Count = 1;
shadowMapDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE | D3D11_BIND_DEPTH_STENCIL;
shadowMapDesc.Height = static_cast<UINT>(m_shadowMapDimension);
shadowMapDesc.Width = static_cast<UINT>(m_shadowMapDimension);

HRESULT hr = pD3DDevice->CreateTexture2D(
    &shadowMapDesc,
    nullptr,
    &m_shadowMap
    );
```

Erstellen Sie anschließend die Ressourcenansichten. Legen Sie den Mip-Slice für die Ansicht der Tiefenschablone auf null und die Mip-Ebenen für die Shaderressourcenansicht auf 1 fest. Verfügen beide über eine Dimension der Textur des TEXTURE2D, und beide müssen mit einem entsprechenden [ **DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059).

```cpp
D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
ZeroMemory(&depthStencilViewDesc, sizeof(D3D11_DEPTH_STENCIL_VIEW_DESC));
depthStencilViewDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
depthStencilViewDesc.Texture2D.MipSlice = 0;

D3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc;
ZeroMemory(&shaderResourceViewDesc, sizeof(D3D11_SHADER_RESOURCE_VIEW_DESC));
shaderResourceViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
shaderResourceViewDesc.Format = DXGI_FORMAT_R24_UNORM_X8_TYPELESS;
shaderResourceViewDesc.Texture2D.MipLevels = 1;

hr = pD3DDevice->CreateDepthStencilView(
    m_shadowMap.Get(),
    &depthStencilViewDesc,
    &m_shadowDepthView
    );

hr = pD3DDevice->CreateShaderResourceView(
    m_shadowMap.Get(),
    &shaderResourceViewDesc,
    &m_shadowResourceView
    );
```

## <a name="create-comparison-state"></a>Erstellen eines Vergleichsstatus


Erstellen Sie jetzt das Vergleichs-Samplerstatusobjekt. Die Funktionsebene 9\_1 unterstützt nur D3D11\_Vergleich\_weniger\_gleich. Die Filterungsoptionen werden ausführlicher unter [Unterstützen von Schattenkarten auf unterschiedlicher Hardware](target-a-range-of-hardware.md) erläutert. Oder Sie setzen einfach Punktfilterung ein, um schnellere Schattenkarten zu erhalten.

Beachten Sie, dass Sie, die D3D11 angeben können\_TEXTUR\_Adresse\_Adressmodus Rahmen und funktioniert auf Funktionsebene 9\_1 Geräte. Dies gilt für Pixelshader, die vor dem Tiefentest nicht testen, ob sich das Pixel im Ansichtsfrustum der Beleuchtung befindet. Wenn Sie für jeden Rand 0 oder 1 angeben, können Sie steuern, ob Pixel außerhalb des Lichtkegels den Tiefentest bestehen – was bedeutet, ob sie beleuchtet werden oder sich im Schatten befinden.

Feature level 9\_1, die folgenden erforderlichen Werte festgelegt werden müssen: **MinLOD** nastaven NA hodnotu NULL, **MaxLOD** nastaven NA hodnotu **D3D11\_FLOAT32\_MAX**, und **MaxAnisotropy** auf 0 (null) festgelegt ist.

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

## <a name="create-render-states"></a>Erstellen von Renderstatus


Erstellen Sie jetzt einen Renderstatus, den Sie zum Aktivieren von Frontface-Culling verwenden können. Beachten Sie, Funktionsebene 9\_1 Geräte erfordern **DepthClipEnable** festgelegt **"true"**.

```cpp
D3D11_RASTERIZER_DESC drawingRenderStateDesc;
ZeroMemory(&drawingRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
drawingRenderStateDesc.CullMode = D3D11_CULL_BACK;
drawingRenderStateDesc.FillMode = D3D11_FILL_SOLID;
drawingRenderStateDesc.DepthClipEnable = true; // Feature level 9_1 requires DepthClipEnable == true
DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &drawingRenderStateDesc,
        &m_drawingRenderState
        )
    );
```

Erstellen Sie einen Renderstatus, den Sie zum Aktivieren von Backface-Culling verwenden können. Falls Ihr Rendercode Backface-Culling bereits aktiviert, können Sie diesen Schritt überspringen.

```cpp
D3D11_RASTERIZER_DESC shadowRenderStateDesc;
ZeroMemory(&shadowRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
shadowRenderStateDesc.CullMode = D3D11_CULL_FRONT;
shadowRenderStateDesc.FillMode = D3D11_FILL_SOLID;
shadowRenderStateDesc.DepthClipEnable = true;

DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &shadowRenderStateDesc,
        &m_shadowRenderState
        )
    );
```

## <a name="create-constant-buffers"></a>Erstellen von Konstantenpuffern


Denken Sie daran, einen Konstantenpuffer für das Rendering aus der Perspektive der Beleuchtung zu erstellen. Mit diesem Konstantenpuffer können Sie die Beleuchtungsposition für den Shader angeben. Verwenden Sie für punktuelles Licht eine perspektivische Matrix und für gerichtetes Licht (z. B. Sonnenlicht) eine orthogonale Matrix.

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

Füllen Sie den Konstantenpuffer mit Daten. Aktualisieren Sie den Konstantenpuffer einmal während der Initialisierung und dann noch einmal, wenn sich die Lichtwerte seit dem vorherigen Frame geändert haben.

```cpp
{
    XMMATRIX lightPerspectiveMatrix = XMMatrixPerspectiveFovRH(
        XM_PIDIV2,
        1.0f,
        12.f,
        50.f
        );

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.projection,
        XMMatrixTranspose(lightPerspectiveMatrix)
        );

    // Point light at (20, 15, 20), pointed at the origin. POV up-vector is along the y-axis.
    static const XMVECTORF32 eye = { 20.0f, 15.0f, 20.0f, 0.0f };
    static const XMVECTORF32 at = { 0.0f, 0.0f, 0.0f, 0.0f };
    static const XMVECTORF32 up = { 0.0f, 1.0f, 0.0f, 0.0f };

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.view,
        XMMatrixTranspose(XMMatrixLookAtRH(eye, at, up))
        );

    // Store the light position to help calculate the shadow offset.
    XMStoreFloat4(&m_lightViewProjectionBufferData.pos, eye);
}
```

```cpp
context->UpdateSubresource(
    m_lightViewProjectionBuffer.Get(),
    0,
    NULL,
    &m_lightViewProjectionBufferData,
    0,
    0
    );
```

## <a name="create-a-viewport"></a>Erstellen eines Viewports


Sie benötigen einen separaten Viewport zum Rendern der Schattenkarte. Der Viewport ist keine geräteabhängige Ressource. Sie können ihn auch an einer anderen Stelle im Code erstellen. Wenn Sie den Viewport zusammen mit der Schattenkarte erstellen, ist es einfacher, die Dimensionen von Viewport und Schattenkarte deckungsgleich zu halten.

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

Im nächsten Teil dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie die Schattenkarte durch [Rendern in den Tiefenpuffer](render-the-shadow-map-to-the-depth-buffer.md) erstellen.

 

 




