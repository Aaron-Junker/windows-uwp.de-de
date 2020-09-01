---
title: Anwenden von Texturen auf Grundtypen
description: Wir laden an dieser Stelle unformatierte Texturdaten und wenden diese auf einen 3D-Grundtyp an. Dazu verwenden wir den Würfel, den wir in „Verwenden von Tiefe und Effekten für Grundtypen“ erstellt haben.
ms.assetid: aeed09e3-c47a-4dd9-d0e8-d1b8bdd7e9b4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Texturen, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 349dcc4e8cc65a6265579e84c6810dcac79938a1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156484"
---
# <a name="apply-textures-to-primitives"></a>Anwenden von Texturen auf Grundtypen

Wir laden an dieser Stelle unformatierte Texturdaten und wenden diese auf einen 3D-Grundtyp an. Dazu verwenden wir den Würfel, den wir unter [Verwenden von Tiefe und Effekten auf Grundtypen](using-depth-and-effects-on-primitives.md) erstellt haben. Außerdem stellen wir ein einfaches Skalarprodukt-Lichtmodell vor. Die Oberflächen des Würfels im Modell sind, je nach Entfernung von der und Winkel zur Lichtquelle, heller oder dunkler.

**Ziel:** Texturen auf Grundtypen anwenden.

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Thema optimal zu nutzen, müssen Sie mit C++ vertraut sein. Außerdem benötigen Sie grundlegende Kenntnisse in den Konzepten der Grafik Programmierung. Und im Idealfall müssen Sie sich bereits mit dem Schnellstart, dem [Einrichten von DirectX-Ressourcen und dem Anzeigen eines Bilds, dem](setting-up-directx-resources.md) [Erstellen von Shadern und dem Zeichnen von primitiven](creating-shaders-and-drawing-primitives.md)und [der Verwendung von tiefen und Effekten für primitive](using-depth-and-effects-on-primitives.md)befassen.

**Zeitaufwand:** 20 Minuten.

<a name="instructions"></a>Instructions
------------
### <a name="1-defining-variables-for-a-textured-cube"></a>1. Definieren von Variablen für einen Würfel mit Texturen

Zunächst müssen wir die **BasicVertex**-Struktur und die **ConstantBuffer**-Struktur für den Würfel mit Texturen definieren. Diese Strukturen bestimmen die Vertexpositionen, die Ausrichtungen und die Texturen für den Würfel. Außerdem haben sie Einfluss darauf, wie der Würfel angezeigt wird. Weiterhin deklarieren wir Variablen analog zum vorangehenden Lernprogramm [Verwenden von Tiefe und Effekten für Grundtypen](using-depth-and-effects-on-primitives.md).

```cppcx
struct BasicVertex
{
    DirectX::XMFLOAT3 pos;  // Position
    DirectX::XMFLOAT3 norm; // Surface normal vector
    DirectX::XMFLOAT2 tex;  // Texture coordinate
};

struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};

// This class defines the application as a whole.
ref class Direct3DTutorialFrameworkView : public IFrameworkView
{
private:
    Platform::Agile<CoreWindow> m_window;
    ComPtr<IDXGISwapChain1> m_swapChain;
    ComPtr<ID3D11Device1> m_d3dDevice;
    ComPtr<ID3D11DeviceContext1> m_d3dDeviceContext;
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    ComPtr<ID3D11DepthStencilView> m_depthStencilView;
    ComPtr<ID3D11Buffer> m_constantBuffer;
    ConstantBuffer m_constantBufferData;
```

### <a name="2-creating-vertex-and-pixel-shaders-with-surface-and-texture-elements"></a>2. Erstellen von Vertex- und Pixelshadern mit Oberflächen- und Texturelementen

Die an dieser Stelle erstellten Vertex- und Pixelshader sind komplexer als die im vorangehenden Lernprogramm [Verwenden von Tiefe und Effekten für Grundtypen](using-depth-and-effects-on-primitives.md). Der Vertexshader dieser App wandelt jede Vertexposition in eine Projektionsfläche um und übergibt die Koordinaten der Vertexttextur über den Pixelshader.

Das Array der D3D11- [** \_ Eingabe \_ Element- \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) -Strukturen der APP, die das Layout des Vertex-Shader-Codes beschreiben, verfügt über drei Layoutelemente: ein Element definiert die Scheitelpunkt Position, ein anderes Element definiert den normalen Oberflächen Knoten (die Richtung, in der die Oberfläche normal ist), und das dritte Element definiert die Texturkoordinaten.

Wir erstellen einen Vertex, einen Index sowie konstante Puffer, die einen kreisenden Würfel mit Textur definieren.

**So erstellen Sie einen kreisenden Würfel mit Textur**

1.  Zunächst definieren wir den Würfel. Jedem Vertex werden eine Position, ein normaler Oberflächenvektor sowie Texturkoordinaten zugewiesen. Um für die einzelnen Seiten unterschiedliche normale Vektoren und Texturkoordinaten definieren und verwenden zu können, werden mehrere Scheitelpunkte für jede Ecke definiert.
2.  Als nächstes beschreiben wir die Vertex-und Index Puffer[**( \_ D3D11 \_ buffer**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) Debug and [**D3D11 \_ subresource \_ Data**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)) mithilfe der Cubedefinition. Wir rufen [**ID3D11Device::CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) einmal für jeden Puffer auf.
3.  Als Nächstes erstellen wir einen konstanten Puffer ([**D3D11 \_ buffer \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc)Debug) zum Übergeben von Modell-, Ansichts-und Projektions Matrizen an den Vertex-Shader. Später können wir den Konstantenpuffer zum Drehen des Würfels und zum Anwenden einer Perspektivenprojektion auf den Würfel verwenden. Wir rufen [**ID3D11Device::CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) auf, um den Konstantenpuffer zu erstellen.
4.  Abschließend geben wir die Transformation der Ansicht an, die der Kameraposition X = 0, Y = 1, Z = 2 entspricht.

```cppcx
auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode)
{
    ComPtr<ID3D11VertexShader> vertexShader;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateVertexShader(
            vertexShaderBytecode->Data,
            vertexShaderBytecode->Length,
            nullptr,
            &vertexShader
        )
    );

    // Create an input layout that matches the layout defined in the vertex shader code.
    // These correspond to the elements of the BasicVertex struct defined above.
    const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
    {
        { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    };

    ComPtr<ID3D11InputLayout> inputLayout;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateInputLayout(
            basicVertexLayoutDesc,
            ARRAYSIZE(basicVertexLayoutDesc),
            vertexShaderBytecode->Data,
            vertexShaderBytecode->Length,
            &inputLayout
        )
    );
});

// Load the raw pixel shader bytecode from disk and create a pixel shader with it.
auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode)
{
    ComPtr<ID3D11PixelShader> pixelShader;
    DX::ThrowIfFailed(
        m_d3dDevice->CreatePixelShader(
            pixelShaderBytecode->Data,
            pixelShaderBytecode->Length,
            nullptr,
            &pixelShader
        )
    );
});

// Create vertex and index buffers that define a simple unit cube.
auto createCubeTask = (createPSTask && createVSTask).then([this]()
{
    // In the array below, which will be used to initialize the cube vertex buffers,
    // multiple vertices are used for each corner to allow different normal vectors and
    // texture coordinates to be defined for each face.
    BasicVertex cubeVertices[] =
    {
        { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Y (top face)
        { DirectX::XMFLOAT3(0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Y (bottom face)
        { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +X (right face)
        { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -X (left face)
        { DirectX::XMFLOAT3(-0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(-0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Z (front face)
        { DirectX::XMFLOAT3(0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Z (back face)
        { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },
    };

    unsigned short cubeIndices[] =
    {
        0, 1, 2,
        0, 2, 3,

        4, 5, 6,
        4, 6, 7,

        8, 9, 10,
        8, 10, 11,

        12, 13, 14,
        12, 14, 15,

        16, 17, 18,
        16, 18, 19,

        20, 21, 22,
        20, 22, 23
    };

    D3D11_BUFFER_DESC vertexBufferDesc = { 0 };
    vertexBufferDesc.ByteWidth = sizeof(BasicVertex) * ARRAYSIZE(cubeVertices);
    vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
    vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    vertexBufferDesc.CPUAccessFlags = 0;
    vertexBufferDesc.MiscFlags = 0;
    vertexBufferDesc.StructureByteStride = 0;

    D3D11_SUBRESOURCE_DATA vertexBufferData;
    vertexBufferData.pSysMem = cubeVertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;

    ComPtr<ID3D11Buffer> vertexBuffer;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            &vertexBuffer
        )
    );

    D3D11_BUFFER_DESC indexBufferDesc;
    indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(cubeIndices);
    indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
    indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
    indexBufferDesc.CPUAccessFlags = 0;
    indexBufferDesc.MiscFlags = 0;
    indexBufferDesc.StructureByteStride = 0;

    D3D11_SUBRESOURCE_DATA indexBufferData;
    indexBufferData.pSysMem = cubeIndices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;

    ComPtr<ID3D11Buffer> indexBuffer;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            &indexBuffer
        )
    );

    // Create a constant buffer for passing model, view, and projection matrices
    // to the vertex shader.  This will allow us to rotate the cube and apply
    // a perspective projection to it.

    D3D11_BUFFER_DESC constantBufferDesc = { 0 };
    constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
    constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
    constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    constantBufferDesc.CPUAccessFlags = 0;
    constantBufferDesc.MiscFlags = 0;
    constantBufferDesc.StructureByteStride = 0;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &constantBufferDesc,
            nullptr,
            &m_constantBuffer
        )
    );

    // Specify the view transform corresponding to a camera position of
    // X = 0, Y = 1, Z = 2.  For a generalized camera class, see Lesson 5.

    m_constantBufferData.view = DirectX::XMFLOAT4X4(
        -1.00000000f, 0.00000000f, 0.00000000f, 0.00000000f,
        0.00000000f, 0.89442718f, 0.44721359f, 0.00000000f,
        0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
        0.00000000f, 0.00000000f, 0.00000000f, 1.00000000f
    );
});
```

### <a name="3-creating-textures-and-samplers"></a>3. Erstellen von Texturen und Mustern

Hier werden Textur Daten auf einen Cube angewendet, anstatt Farben wie im vorherigen Tutorial anzuwenden, [wobei Tiefe und Effekte für primitive verwendet](using-depth-and-effects-on-primitives.md)werden.

Wir verwenden unformatierte Texturdaten, um die Texturen zu erstellen.

**Erstellen von Texturen und Samplern**

1.  Wir beginnen damit, unformatierte Texturdaten aus der Datei „texturedata.bin“ auf dem Datenträger zu lesen.
2.  Als Nächstes erstellen wir eine [**D3D11 \_ subresource- \_ Daten**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) Struktur, die auf die unformatierten Textur Daten verweist.
3.  Anschließend füllen wir eine [**D3D11 \_ TEXTURE2D \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_texture2d_desc) -Struktur, um die Textur zu beschreiben. Anschließend übergeben wir die [**D3D11 \_ subresource- \_ Daten**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) -und **D3D11 \_ TEXTURE2D- \_ ** Erstellungs Strukturen in einem-Rückruf an [**ID3D11Device:: CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) , um die Textur zu erstellen.
4.  Jetzt erstellen wir eine Shaderressourcenansicht der Textur, damit diese von Shadern verwendet werden kann. Um die Shader-Ressourcen Ansicht zu erstellen, füllen wir eine [**D3D11 \_ Shader- \_ Ressourcen \_ Ansicht \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_shader_resource_view_desc) (Debug) auf, um die Shader-Ressourcen Ansicht zu beschreiben, und übergeben die Beschreibung der Shader-Ressourcen Ansicht und die Textur an [**ID3D11Device:: anateshaderresourceview**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createshaderresourceview). Die Beschreibung der Ansicht sollte im Allgemeinen der Beschreibung der Textur entsprechen.
5.  Anschließend erstellen wir einen Musterzustand für die Textur. Dieser Musterzustand legt anhand der relevanten Texturdaten fest, wie die Farbe für eine bestimmte Texturkoordinate bestimmt wird. Wir füllen eine [**D3D11- \_ samplerstruktur \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_sampler_desc) auf, um den samplerstatus zu beschreiben. Anschließend übergeben wir die **D3D11 \_ - \_ samplerstruktur** in einem Aufrufen von [**ID3D11Device:: kreatesamplerstate**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createsamplerstate) , um den samplerzustand zu erstellen.
6.  Zum Schluss deklarieren wir eine *degree*-Variable, mit deren Hilfe wir den Würfel animieren werden, indem er in jedem Frame gedreht wird.

```cppcx
// Load the raw texture data from disk and construct a subresource description that references it.
auto loadTDTask = DX::ReadDataAsync(L"texturedata.bin");

auto constructSubresourceTask = loadTDTask.then([this](const std::vector<byte>& textureData)
{
    D3D11_SUBRESOURCE_DATA textureSubresourceData = { 0 };
    textureSubresourceData.pSysMem = textureData.data();

    // Specify the size of a row in bytes, known as a priori about the texture data.
    textureSubresourceData.SysMemPitch = 1024;

    // As this is not a texture array or 3D texture, this parameter is ignored.
    textureSubresourceData.SysMemSlicePitch = 0;

    // Create a texture description from information known as a priori about the data.
    // Generalized texture loading code can be found in the Resource Loading sample.
    D3D11_TEXTURE2D_DESC textureDesc = { 0 };
    textureDesc.Width = 256;
    textureDesc.Height = 256;
    textureDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
    textureDesc.Usage = D3D11_USAGE_DEFAULT;
    textureDesc.CPUAccessFlags = 0;
    textureDesc.MiscFlags = 0;

    // Most textures contain more than one MIP level.  For simplicity, this sample uses only one.
    textureDesc.MipLevels = 1;

    // As this will not be a texture array, this parameter is ignored.
    textureDesc.ArraySize = 1;

    // Don't use multi-sampling.
    textureDesc.SampleDesc.Count = 1;
    textureDesc.SampleDesc.Quality = 0;

    // Allow the texture to be bound as a shader resource.
    textureDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;

    ComPtr<ID3D11Texture2D> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &textureDesc,
            &textureSubresourceData,
            &texture
        )
    );

    // Once the texture is created, we must create a shader resource view of it
    // so that shaders may use it.  In general, the view description will match
    // the texture description.
    D3D11_SHADER_RESOURCE_VIEW_DESC textureViewDesc;
    ZeroMemory(&textureViewDesc, sizeof(textureViewDesc));
    textureViewDesc.Format = textureDesc.Format;
    textureViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
    textureViewDesc.Texture2D.MipLevels = textureDesc.MipLevels;
    textureViewDesc.Texture2D.MostDetailedMip = 0;

    ComPtr<ID3D11ShaderResourceView> textureView;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(
            texture.Get(),
            &textureViewDesc,
            &textureView
        )
    );

    // Once the texture view is created, create a sampler.  This defines how the color
    // for a particular texture coordinate is determined using the relevant texture data.
    D3D11_SAMPLER_DESC samplerDesc;
    ZeroMemory(&samplerDesc, sizeof(samplerDesc));

    samplerDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;

    // The sampler does not use anisotropic filtering, so this parameter is ignored.
    samplerDesc.MaxAnisotropy = 0;

    // Specify how texture coordinates outside of the range 0..1 are resolved.
    samplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    samplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    samplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;

    // Use no special MIP clamping or bias.
    samplerDesc.MipLODBias = 0.0f;
    samplerDesc.MinLOD = 0;
    samplerDesc.MaxLOD = D3D11_FLOAT32_MAX;

    // Don't use a comparison function.
    samplerDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;

    // Border address mode is not used, so this parameter is ignored.
    samplerDesc.BorderColor[0] = 0.0f;
    samplerDesc.BorderColor[1] = 0.0f;
    samplerDesc.BorderColor[2] = 0.0f;
    samplerDesc.BorderColor[3] = 0.0f;

    ComPtr<ID3D11SamplerState> sampler;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(
            &samplerDesc,
            &sampler
        )
    );
});

// This value will be used to animate the cube by rotating it every frame;
float degree = 0.0f;
```

### <a name="4-rotating-and-drawing-the-textured-cube-and-presenting-the-rendered-image"></a>4. Drehen und Zeichnen des Würfels mit Texturen und Darstellen des gerenderten Bilds

Um die Szene ohne Unterbrechung zu rendern und anzuzeigen, wird analog zu früheren Beispielen eine Endlosschleife aufgerufen. Wir rufen die **rotationY**-Inlinefunktion (BasicMath.h) mit einer Rotationsmenge auf, um die Werte festzulegen, mit deren Hilfe sich die Modellmatrix des Würfels um die Y-Achse dreht. Anschließend rufen wir [**ID3D11DeviceContext::UpdateSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) auf, um den Konstantenpuffer zu aktualisieren und das Würfelmodell zu drehen. Nun rufen wir [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) auf, um das Renderingziel und die Ansicht für die Tiefe/Schablone anzugeben. Wir rufen [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) auf, um das Renderziel in eine blaue Volltonfarbe zu bereinigen. Anschließend rufen wir [**ID3D11DeviceContext::ClearDepthStencilView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-cleardepthstencilview) auf, um den Tiefenpuffer zu leeren.

In der Endlosschleife zeichnen wir auch den Würfel mit Texturen auf der blauen Oberfläche.

**So zeichnen Sie den Würfel mit Texturen**

1.  Wir rufen zunächst [**ID3D11DeviceContext::IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) auf, um das Streamen der Vertexpufferdaten in den Eingabeassemblerzustand zu beschreiben.
2.  Als Nächstes rufen wir [**ID3D11DeviceContext::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) und [**ID3D11DeviceContext::IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) auf, um die Vertex- und Indexpuffer an den Eingabeassemblerzustand zu binden.
3.  Im nächsten Schritt wird [**Verknüpfung id3d11devicecontext aus:: iasetprimitivetopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) mit dem Wert der [**D3D11- \_ primitiven \_ Topologie \_ Trianglestrip**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) aufgerufen, um anzugeben, dass die Vertex-Daten als Dreiecks Streifen in der Eingabe-Assembler-Phase interpretiert werden sollen.
4.  Im nächsten Schritt rufen wir [**ID3D11DeviceContext::VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) auf, um die Vertexshaderphase mit dem Vertexshadercode zu initialisieren. Zudem rufen wir [**ID3D11DeviceContext::PSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) auf, um die Pixelshaderphase mit dem Pixelshadercode zu initialisieren.
5.  Dann rufen wir [**ID3D11DeviceContext::VSSetConstantBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) auf, um den im Vertexshader-Pipelinezustand verwendeten Konstantenpuffer festzulegen.
6.  Danach rufen wir [**PSSetShaderResources**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshaderresources) auf, um die Shaderressourcenansicht der Textur an die Pipelinephase des Pixelshaders zu binden.
7.  Danach rufen wir [**PSSetSamplers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetsamplers) auf, um den Musterzustand für die Pipelinephase des Pixelshaders festzulegen.
8.  Schließlich rufen wir [**ID3D11DeviceContext::DrawIndexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) auf, um den Würfel zu zeichnen und ihn an die Renderpipeline zu übermitteln.

Analog zu früheren Lernprogrammen wird mit einem Aufruf von [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) das gerenderte Bild im Fenster dargestellt.

```cppcx
// Update the constant buffer to rotate the cube model.
m_constantBufferData.model = DirectX::XMMatrixRotationY(-degree);
degree += 1.0f;

m_d3dDeviceContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_constantBufferData,
    0,
    0
);

// Specify the render target and depth stencil we created as the output target.
m_d3dDeviceContext->OMSetRenderTargets(
    1,
    m_renderTargetView.GetAddressOf(),
    m_depthStencilView.Get()
);

// Clear the render target to a solid color, and reset the depth stencil.
const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
m_d3dDeviceContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    clearColor
);

m_d3dDeviceContext->ClearDepthStencilView(
    m_depthStencilView.Get(),
    D3D11_CLEAR_DEPTH,
    1.0f,
    0
);

m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(BasicVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
    0,
    1,
    vertexBuffer.GetAddressOf(),
    &stride,
    &offset
);

m_d3dDeviceContext->IASetIndexBuffer(
    indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0
);

m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
    vertexShader.Get(),
    nullptr,
    0
);

m_d3dDeviceContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf()
);

m_d3dDeviceContext->PSSetShader(
    pixelShader.Get(),
    nullptr,
    0
);

m_d3dDeviceContext->PSSetShaderResources(
    0,
    1,
    textureView.GetAddressOf()
);

m_d3dDeviceContext->PSSetSamplers(
    0,
    1,
    sampler.GetAddressOf()
);

// Draw the cube.
m_d3dDeviceContext->DrawIndexed(
    ARRAYSIZE(cubeIndices),
    0,
    0
);

// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop will generally be throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the application on Present until the screen is refreshed.
DX::ThrowIfFailed(
    m_swapChain->Present(1, 0)
);
```

## <a name="summary"></a>Zusammenfassung

In diesem Thema haben wir Rohdaten für die Textur geladen und diese Daten auf ein 3D-primitiv angewendet.