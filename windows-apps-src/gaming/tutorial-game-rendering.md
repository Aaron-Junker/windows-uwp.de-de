---
title: Einrichten
description: Erfahren Sie, wie Sie die Rendering-Pipeline zum Anzeigen von Grafiken zusammenstellen. Spiele Rendering, einrichten und Vorbereiten von Daten.
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Rendering
ms.localizationpriority: medium
ms.openlocfilehash: a87382aeffb0e0b7a8eaca1c4baec8561049e91e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168244"
---
# <a name="rendering-framework-ii-game-rendering"></a>Renderingframework II: Spiel Rendering

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

In [Renderingframework I](tutorial--assembling-the-rendering-pipeline.md)haben wir erläutert, wie wir die szeneninformationen übernehmen und auf dem Bildschirm anzeigen. Nun gehen wir einen Schritt zurück und erfahren, wie Sie die Daten für das Rendering vorbereiten.

>[!Note]
>Wenn Sie den neuesten Spiel Code für dieses Beispiel nicht heruntergeladen haben, besuchen Sie das [Direct3D-Beispiel Spiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel ist Teil einer großen Auflistung von UWP-Funktions Beispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter herunterladen [der UWP-Beispiele von GitHub](../get-started/get-app-samples.md).

## <a name="objective"></a>Ziel

Schnelle Zusammenfassung des Ziels. Es wird erläutert, wie Sie ein einfaches Renderingframework einrichten, um die Grafikausgabe für ein UWP DirectX-Spiel anzuzeigen. Diese drei Schritte können Sie locker gruppieren.

 1. Herstellen einer Verbindung mit unserer Grafikschnittstelle
 2. Vorbereitung: Erstellen der Ressourcen, die zum Zeichnen der Grafiken erforderlich sind
 3. Anzeigen der Grafik: renderden Frame

[Rendering-Framework I: Einführung in das Rendering](tutorial--assembling-the-rendering-pipeline.md) erläuterte, wie Grafiken gerendert werden, und deckt die Schritte 1 und 3 ab. 

In diesem Artikel wird erläutert, wie Sie andere Teile dieses Frameworks einrichten und die erforderlichen Daten vor dem Rendering vorbereiten. Dies ist Schritt 2 des Vorgangs.

## <a name="design-the-renderer"></a>Entwerfen des Renderers

Der Renderer ist verantwortlich für das Erstellen und Verwalten aller D3D11-und D2D-Objekte, die zum Generieren der visuellen Spielelemente verwendet werden. Die __gamerenderer__ -Klasse ist der Renderer für dieses Beispiel Spiel und wurde entwickelt, um die Renderinganforderungen des Spiels zu erfüllen.

Dies sind einige Konzepte, die Sie beim Entwerfen des Renderers für das Spiel verwenden können:
* Da Direct3D 11-APIs als [com](/windows/desktop/com/the-component-object-model) -APIs definiert sind, müssen Sie [comptr](/cpp/windows/comptr-class) -Verweise auf die Objekte bereitstellen, die durch diese APIs definiert werden. Diese Objekte werden automatisch freigegeben, wenn ihr letzter Verweis den gültigen Bereich verlässt und die App beendet wird. Weitere Informationen finden Sie unter [comptr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr). Beispiel für diese Objekte: Konstante Puffer, Shader-Objekte- [Vertex-Shader](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [Pixel-Shader](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)und Shader-Ressourcen Objekte.
* Konstantenpuffer werden in dieser Klasse definiert, um die für das Rendering erforderlichen Daten zu speichern.
    * Verwenden Sie mehrere Konstante Puffer mit unterschiedlichen Frequenzen, um die Datenmenge zu reduzieren, die pro Frame an die GPU gesendet werden muss. In diesem Beispiel werden Konstanten basierend auf der Häufigkeit, mit der Sie aktualisiert werden müssen, in unterschiedliche Puffer eingeteilt. Dies ist die empfohlene Methode für die Direct3D-Programmierung. 
    * In diesem Beispiel Spiel werden 4 Konstante Puffer definiert.
        1. __m \_ constantbufferneverchanges__ enthält die Beleuchtungs Parameter. Es wird einmal in der __finalizecreategamedeviceresources__ -Methode festgelegt und nie wieder geändert.
        2. __m \_ constantbufferchangeonresize__ enthält die Projektions Matrix. Die Projektionsmatrix hängt von der Größe und dem Seitenverhältnis des Fensters ab. Sie wird in [__createwindowsizedependentresources__](#createwindowsizedependentresource-method) festgelegt und dann aktualisiert, nachdem Ressourcen in die [__finalizecreategamedeviceresources__](#finalizecreategamedeviceresources-method) -Methode geladen wurden. Wenn das Rendering in 3D erfolgt, wird es auch zweimal pro Frame geändert.
        3. __m \_ constantbufferchangeseveryframe__ enthält die Ansichts Matrix. Diese Matrix hängt von der Kameraposition und der Ausrichtung (der normal zur Projektion) ab und ändert sich einmal pro Frame in der __Renderingmethode__ . Dies wurde bereits in __Renderingframework I:__ Einführung in Rendering unter der [ __gamerenderer:: Rendering__ -Methode](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method)erläutert.
        4. __m \_ constantbufferchangeseveryprim__ enthält die Modell Matrix-und Materialeigenschaften der einzelnen primitiven. Die Modellmatrix transformiert Scheitelpunkte aus lokalen Koordinaten in globale Koordinaten. Diese Konstanten gelten speziell für die einzelnen Grundtypen und werden für jeden Draw-Aufruf aktualisiert. Dies wurde bereits in __Renderingframework I:__ Einführung in Rendering unter dem [primitiven Rendering](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering)erläutert.
* Shader-Ressourcen Objekte, die Texturen für die primitiven enthalten, werden ebenfalls in dieser Klasse definiert.
    * Einige Texturen sind vordefiniert ([DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) ist ein Dateiformat, das zum Speichern von komprimierten und unkomprimierten Texturen verwendet werden kann. DDS-Texturen werden für die Wände und den Boden der Welt sowie für die Ammo-Sphären verwendet.)
    * In diesem Beispiel Spiel lauten Shader-Ressourcen Objekte: __m \_ spheretexture__, __m \_ zylindridertexture__, __m \_ ceilingtexture__, __m \_ floortexture__, __m \_ wallstexture__.
* Shader-Objekte werden in dieser Klasse definiert, um die primitiven und Texturen zu berechnen. 
    * In diesem Beispiel Spiel sind die Shader-Objekte __m \_ Vertexshader__, __m \_ vertexshaderflat__und __m \_ Pixelshader__, __m \_ pixelshaderflat__.
    * Der Vertexshader verarbeitet die Grundtypen und die grundlegende Beleuchtung. Der Pixelshader (auch als Fragmentshader bezeichnet) verarbeitet die Texturen und alle pixelgenauen Effekte.
    * Es existieren zwei Versionen dieser Shader (regulär und flach) zum Rendern verschiedener Grundtypen. Der Grund für verschiedene Versionen ist, dass die flachen Versionen sehr viel einfacher sind und keine Glanzlichter oder irgendwelche pro-Pixel-Beleuchtungseffekte darstellen. Sie werden für die Wände verwendet und beschleunigen das Rendern auf Geräten mit geringerer Leistung.

## <a name="gamerendererh"></a>GameRenderer.h

Sehen wir uns nun den Code im Renderer-Klassenobjekt des Beispiel Spiels an.

```cppwinrt
// Class handling the rendering of the game
class GameRenderer : public std::enable_shared_from_this<GameRenderer>
{
public:
    GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources);

    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();
    // --- end of async related methods section

    winrt::Windows::Foundation::IAsyncAction CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game);
    void FinalizeCreateGameDeviceResources();
    winrt::Windows::Foundation::IAsyncAction LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    Simple3DGameDX::IGameUIControl* GameUIControl() { return &m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay.Visible(); }
    // --- end of rendering overlay section
...
private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>        m_deviceResources;

    ...

    // Shader resource objects
    winrt::com_ptr<ID3D11ShaderResourceView>    m_sphereTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_cylinderTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_ceilingTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_floorTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant buffers
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferNeverChanges;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    winrt::com_ptr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShader;
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShaderFlat;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShader;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShaderFlat;
    winrt::com_ptr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>Konstruktor

Als nächstes untersuchen wir den __gamerenderer__ -Konstruktor des Beispiel Spiels und vergleichen ihn mit dem __Sample3DSceneRenderer__ -Konstruktor, der in der DirectX 11-App-Vorlage bereitgestellt wird.

```cppwinrt
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
    m_gameInfoOverlay(deviceResources),
    m_gameHud(deviceResources, L"Windows platform samples", L"DirectX first-person game sample")
{
    // m_gameInfoOverlay is a GameHud object to render text in the top left corner of the screen.
    // m_gameHud is Game info rendered as an overlay on the top-right corner of the screen,
    // for example hits, shots, and time.

    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>Erstellen und Laden von DirectX-Grafik Ressourcen

Im Beispiel Spiel (und in der DirectX 11-App-Vorlage von Visual Studio __(universelle Windows__ -Vorlage) wird das Erstellen und Laden von Spiel Ressourcen mit diesen beiden Methoden implementiert, die vom __gamerenderer__ -Konstruktor aufgerufen werden:

* [__Createdevicedependentresources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)

## <a name="createdevicedependentresources-method"></a>Createdevicedependentresources-Methode

In der DirectX 11-App-Vorlage wird diese Methode verwendet, um den Scheitelpunkt und den Pixel-Shader asynchron zu laden, den Shader und den konstanten Puffer zu erstellen, ein Mesh mit Scheitel Punkten zu erstellen, die Positions-und Farbinformationen enthalten. 

Im Beispiel Spiel werden diese Vorgänge der Scene-Objekte stattdessen zwischen den Methoden " [__aliategamedeviceresourcesasync__](#creategamedeviceresourcesasync-method) " und " [__finalizecreategamedeviceresources__](#finalizecreategamedeviceresources-method) " aufgeteilt. 

Was ist für dieses Beispiel Spiel die Methode?

* Instanziierte Variablen (__m \_ gameresourcesloaded__ = false und __m \_ levelresourcesloaded__ = false), die angeben, ob Ressourcen geladen wurden, bevor Sie zum Rendering verschoben werden, da Sie asynchron geladen werden. 
* Da die HUD-und Overlay-Rendering in separaten Klassen Objekten vorliegen, müssen Sie hier __gamehud:: createdevicedependentresources__ und __gameinfooverlay:: createdevicedependentresources__ -Methoden aufrufen.

Im folgenden finden Sie den Code für " __gamerderderer:: createdevicedependentresources__".

```cppwinrt
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate whether resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud.CreateDeviceDependentResources();
    m_gameInfoOverlay.CreateDeviceDependentResources();
}
```

Im folgenden finden Sie eine Liste der Methoden, die zum Erstellen und Laden von Ressourcen verwendet werden.

- Createdevicedependentresources
  - "Kreategamedeviceresourcesasync" (hinzugefügt)
  - Finalizecreategamedeviceresources (hinzugefügt)
- CreateWindowSizeDependentResources

Bevor wir uns mit den anderen Methoden befassen, die zum Erstellen und Laden von Ressourcen verwendet werden, erstellen wir zunächst den Renderer und sehen, wie er in die Spiel Schleife passt.

## <a name="create-the-renderer"></a>Renderer erstellen

Der __gamerenderer__ wird im Konstruktor von __gamemain__erstellt. Außerdem werden die beiden anderen Methoden " [__creategamedeviceresourcesasync__](#creategamedeviceresourcesasync-method) " und " [__finalizecreategamedeviceresources__](#finalizecreategamedeviceresources-method) " aufgerufen, die zum Erstellen und Laden von Ressourcen hinzugefügt werden.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // Creation of GameRenderer
    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);

    ...

    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    ...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>Methode "| ategamedeviceresourcesasync"

__Creategamedeviceresourcesasync__ wird von der __gamemain__ -Konstruktormethode in der __Create \_ Task__ -Schleife aufgerufen, da wir Spiel Ressourcen asynchron laden.
        
__CreateDeviceResourcesAsync__ ist eine Methode, die als gesonderte Reihe asynchroner Aufgaben ausgeführt wird, um die Spielressourcen zu laden. Da erwartet wird, dass Sie in einem separaten Thread ausgeführt wird, hat Sie nur Zugriff auf die Geräte Methoden Direct3D 11 (die für __ID3D11Device__definiert sind) und nicht auf die Gerätekontext Methoden (die für __Verknüpfung id3d11devicecontext aus__definierten Methoden), sodass keine Renderingvorgänge ausgeführt werden.

Die __finalizecreategamedeviceresources__ -Methode wird im Haupt Thread ausgeführt und verfügt über Zugriff auf die Gerätekontext Methoden Direct3D 11.

Im Prinzip:
* Verwenden Sie in " __forategamedeviceresourcesasync__ " nur __ID3D11Device__ -Methoden, weil Sie frei Thread sind, was bedeutet, dass Sie in jedem Thread ausgeführt werden können. Außerdem wird davon ausgegangen, dass Sie nicht im gleichen Thread ausgeführt werden, in dem der einzige __gamerenderer__ erstellt wurde. 
* Verwenden Sie hier keine Methoden in __Verknüpfung id3d11devicecontext aus__ , da Sie in einem einzelnen Thread und im gleichen Thread wie __gamerenderer__ausgeführt werden müssen.
* Verwenden Sie diese Methode, um konstante Puffer zu erstellen.
* Verwenden Sie diese Methode, um Texturen (z. b. die DDS-Dateien) und [Shaderinformationen](tutorial--assembling-the-rendering-pipeline.md#shaders)(z. b. die CSO-Dateien) in die Shader zu laden.

Diese Methode wird für Folgendes verwendet:
* Erstellen Sie die 4 [Konstanten Puffer](tutorial--assembling-the-rendering-pipeline.md#buffer): __m \_ constantbufferneverchanges__, __m \_ constantbufferchangeonresize__, __m \_ constantbufferchangeseveryframe__, __m \_ constantbufferchangeseveryprim__
* Erstellen eines [samplerstatusobjekts](tutorial--assembling-the-rendering-pipeline.md#sampler-state) , das Stichproben Informationen für eine Textur kapselt
* Erstellen Sie eine Aufgaben Gruppe, die alle asynchronen Tasks enthält, die von der-Methode erstellt wurden. Er wartet auf den Abschluss aller asynchronen Aufgaben und ruft dann __finalizecreategamedeviceresources__auf.
* Erstellen Sie ein Lade Modul mithilfe des [Basic-Lade](tutorial--assembling-the-rendering-pipeline.md#the-basicloader-class)Moduls. Fügen Sie die Async-Ladevorgänge des Lade Moduls als Aufgaben in die zuvor erstellte Aufgaben Gruppe ein.
* Methoden wie __basicloader:: loadshaderasync__ und  __basicloader:: loadtextureasync__ werden zum Laden von verwendet:
    * kompilierte Shader-Objekte (vertextshader. CSO, vertexshaderflat. CSO, Pixelshader. CSO und pixelshaderflat. CSO). Weitere Informationen finden Sie unter [verschiedene Shader-Dateiformate](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats).
    * Spiele spezifische Texturen (Assets \\ seeloor. DDS, metal_texture. DDS, cellceiling. DDS, cellfloor. DDS, cellwall. DDS).

```cppwinrt
IAsyncAction GameRenderer::CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game)
{
    auto lifetime = shared_from_this();

    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method. It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC. See
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_buffer_desc
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    ...

    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11device-createbuffer.
    winrt::check_hresult(
        d3dDevice->CreateBuffer(&bd, nullptr, m_constantBufferNeverChanges.put())
        );

    ...

    // Define D3D11_SAMPLER_DESC. For API ref, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_sampler_desc.
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. For API ref, see
    // https://docs.microsoft.com/previous-versions/windows/desktop/legacy/aa366920(v=vs.85).
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    ...

    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    winrt::check_hresult(
        d3dDevice->CreateSamplerState(&sampDesc, m_samplerLinear.put())
        );

    // Start the async tasks to load the shaders and textures.

    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. For more info, see
    // https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader.
    BasicLoader loader{ d3dDevice };

    std::vector<IAsyncAction> tasks;

    uint32_t numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the
    // BasicLoader::LoadShaderAsync method. Push these method calls into a list of tasks.
    tasks.push_back(loader.LoadShaderAsync(L"VertexShader.cso", PNTVertexLayout, numElements, m_vertexShader.put(), m_vertexLayout.put()));
    tasks.push_back(loader.LoadShaderAsync(L"VertexShaderFlat.cso", nullptr, numElements, m_vertexShaderFlat.put(), nullptr));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShader.cso", m_pixelShader.put()));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShaderFlat.cso", m_pixelShaderFlat.put()));

    // Make sure the previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds,
    // cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks.
    tasks.push_back(loader.LoadTextureAsync(L"Assets\\seafloor.dds", nullptr, m_sphereTexture.put()));
    ...

    // Simulate loading additional resources by introducing a delay.
    tasks.push_back([]() -> IAsyncAction { co_await winrt::resume_after(GameConstants::InitialLoadingDelay); }());

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    for (auto&& task : tasks)
    {
        co_await task;
    }
}
```

## <a name="finalizecreategamedeviceresources-method"></a>Finalizecreategamedeviceresources-Methode

Die __finalizecreategamedeviceresources__ -Methode wird aufgerufen, nachdem alle Tasks der Lade Ressourcen, die sich in der __creategamedeviceresourcesasync__ -Methode befinden, abgeschlossen sind. 

* Initialisieren Sie constantbufferneverchanges mit den hellen Positionen und der Farbe. Lädt die anfänglichen Daten mit einem Gerätekontext Methoden-Aufrufen von __Verknüpfung id3d11devicecontext aus:: updatesubresource__in die Konstanten Puffer.
* Seit das Laden von asynchron geladenen Ressourcen abgeschlossen ist, ist es an der Zeit, Sie den entsprechenden Spielobjekten zuzuordnen.
* Erstellen Sie für jedes Spielobjekt das Mesh und das Material mithilfe der-Texturen, die geladen wurden. Ordnen Sie dann das Mesh und Material dem Spielobjekt zu.
* Für das Targets-Spielobjekt wird die Textur, die aus konzentrischen farbigen Ringen besteht, mit einem numerischen Wert im oberen Bereich, nicht aus einer Textur Datei geladen. Stattdessen wird er mithilfe des Codes in " __targettexture. cpp__" prozedursch generiert. Die __targettexture__ -Klasse erstellt die erforderlichen Ressourcen, um die Textur zur Initialisierungs Zeit in eine Offscreen-Ressource zu zeichnen. Die resultierende Textur wird dann den entsprechenden Ziel Spielobjekten zugeordnet.

" __Finalizecreategamedeviceresources__ " und " [__createwindowsizedependentresources__](#createwindowsizedependentresource-method) " verwenden ähnliche Teile von Code für diese:
* Verwenden Sie __setprojparameams__ , um sicherzustellen, dass die Kamera über die richtige Projektions Matrix verfügt. Weitere Informationen finden Sie unter [Kamera und Koordinaten Bereich](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space).
* Behandeln Sie die Bildschirmdrehung, indem Sie die 3D-Rotations Matrix mit der Projektions Matrix der Kamera multiplizieren. Aktualisieren Sie dann den konstanten-Puffer __constantbufferchangeonresize__ mit der resultierenden Projektions Matrix.
* Legen Sie __die globale Variable__ " __m \_ gameresourcesloaded__ " fest, um anzugeben, dass die Ressourcen nun in den Puffern geladen sind und für den nächsten Schritt bereit sind. Beachten Sie, dass wir diese Variable zunächst in der Konstruktormethode des __gamerenderer__mithilfe der Methode __gamerenderer:: createdevicedependentresources__ als __false__ initialisiert haben. 
* Wenn dieses __m \_ gameresourcesloaded__ auf __true__festgelegt ist, kann das Rendering der Scene-Objekte stattfinden. Dies wurde im Artikel " __Rendering-Framework I: Intro to Rendering__ " unter der [__gamerenderer:: Rendering-Methode__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method)behandelt.

```cppwinrt
// This method is called from the GameMain constructor.
// Make sure that 2D rendering is occurring on the same thread as the main rendering.
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4(3.5f, 2.5f, 5.5f, 1.0f);
    ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture textureGenerator(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    auto cylinderMesh = std::make_shared<CylinderMesh>(d3dDevice, (uint16_t)26);
    ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    auto cylinderMaterial = std::make_shared<Material>(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.get(),
        m_vertexShader.get(),
        m_pixelShader.get()
        );

    ...

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto&& object : m_game->RenderObjects())
    {
        if (object->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            object->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.get(),
                    m_vertexShaderFlat.get(),
                    m_pixelShaderFlat.get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            object->Mesh(std::make_shared<WorldFloorMesh>(d3dDevice));
        }
        ...
        else if (auto cylinder = dynamic_cast<Cylinder*>(object.get()))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (auto target = dynamic_cast<Face*>(object.get()))
        {
            const int bufferLength = 16;
            wchar_t str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            auto string{ winrt::hstring(str, len) };

            winrt::com_ptr<ID3D11ShaderResourceView> texture;
            textureGenerator.CreateTextureResourceView(string, texture.put());
            target->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            texture = nullptr;
            textureGenerator.CreateHitTextureResourceView(string, texture.put());
            target->HitMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            target->Mesh(targetMesh);
        }
        ...
    }

    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera().SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera().Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>Methode "kreatewindowsizedependentresource"

Die createwindowsizedependentresources-Methoden werden jedes Mal aufgerufen, wenn die Fenstergröße, die Ausrichtung, das Stereo aktivierte Rendering oder die Auflösung geändert wird. Im Beispiel Spiel wird die Projektions Matrix in __constantbufferchangeonresize__aktualisiert.

Die Fenstergröße wird auf diese Weise aktualisiert: 
* Das App-Framework ruft eines von mehreren möglichen Ereignissen ab, das eine Änderung des Fenster Zustands angibt. 
* Die Hauptspiel Schleife wird dann über das Ereignis informiert und ruft __createwindowsizedependentresources__ auf der Hauptklasse (__gamemain__) ab, die dann die __createwindowsizedependentresources__ -Implementierung in der Game Renderer-Klasse (__gamerenderer__) aufruft.
* Der primäre Auftrag dieser Methode besteht darin, sicherzustellen, dass die visuellen Elemente aufgrund einer Änderung der Fenster Eigenschaften nicht verwirrt oder ungültig werden.

Für dieses Beispiel Spiel ist eine Reihe von Methoden aufrufen identisch mit der [__finalizecreategamedeviceresources__](#finalizecreategamedeviceresources-method) -Methode. Informationen zu Code exemplarischen Vorgehensweisen finden Sie im vorherigen Abschnitt.

Die Renderingleistung für Spiel-HUD und über Lagerungs Fenster werden unter [Benutzeroberfläche hinzufügen](tutorial--adding-a-user-interface.md)behandelt.

```cppwinrt
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{
    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud.CreateWindowSizeDependentResources();

    ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    ...

    m_gameInfoOverlay.CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera().SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera().Projection()),
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
}
```

## <a name="next-steps"></a>Nächste Schritte

Dies ist der grundlegende Prozess zum Implementieren des Grafik Rendering-Frameworks eines Spiels. Je größer das Spiel ist, desto mehr Abstraktionen müssten Sie zum Verarbeiten von Hierarchien von Objekttypen und Animations Verhalten einsetzen. Sie müssen komplexere Methoden zum Laden und Verwalten von Ressourcen wie z. b. Netzen und Texturen implementieren. Als Nächstes erfahren Sie, wie Sie [eine Benutzeroberfläche hinzufügen](tutorial--adding-a-user-interface.md).