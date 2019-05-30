---
title: Einrichtung
description: Hier erfahren Sie, wie Sie die Renderingpipeline zum Anzeigen von Grafiken zusammenstellen. Spiel-Rendering, Einrichten und Vorbereiten von Daten.
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Rendern
ms.localizationpriority: medium
ms.openlocfilehash: 2b44558232247de969f22d5767a16d921cfbf252
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367571"
---
# <a name="rendering-framework-ii-game-rendering"></a>Rendering-Framework II: Rendern von Spielen

In [Rendering-Framework I](tutorial--assembling-the-rendering-pipeline.md) haben wir behandelt, wie wir Szeneninformationen erfassen und auf dem Bildschirm anzeigen. Jetzt gehen wir einen Schritt zurück und erläutern, wie Sie die Daten für das Rendering vorbereiten.

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Ziel

Kurze Zusammenfassung des Ziels dieses Abschnitts: Es soll vermittelt werden, wie Sie ein einfaches Rendering-Framework einrichten, um die Grafikausgabe für ein UWP-DirectX-Spiel anzuzeigen. Dazu sind drei Schritte erforderlich.

 1. Einrichten einer Verbindung zur Grafikschnittstelle
 2. Vorbereitung: Erstellen Sie die Ressourcen, die wir die Grafiken gezeichnet werden soll müssen.
 3. Zeigen Sie die Grafiken an: Den Frame zu rendern

[Rendering-Framework I: Einführung in Rendering](tutorial--assembling-the-rendering-pipeline.md) erläutert, wie Grafiken gerendert werden, deckt die Schritte 1 und 3. 

In diesem Artikel wird erläutert, wie Sie andere Teile dieses Frameworks einrichten und die erforderlichen Daten vorbereiten, bevor das Rendern (Schritt 2 des Prozesses) ausgeführt werden kann.

## <a name="design-the-renderer"></a>Entwerfen des Renderers

Der Renderer ist verantwortlich für das Erstellen und Verwalten alle D3D11- und D2D-Objekte, die verwendet, um die visuellen Spielelemente generieren. Die Klasse __GameRenderer__ ist der Renderer für dieses Beispielspiel und wurde entwickelt, um Rendering-Anforderungen des Spiels zu erfüllen.

Hier einige Konzepte, die Sie verwenden können, um den Renderer für Ihr Spiel zu entwerfen:
* Da Direct3D 11-APIs als [COM](https://docs.microsoft.com/windows/desktop/com/the-component-object-model)-APIs definiert sind, müssen Sie [ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class)-Verweise auf die von diesen APIs definierten Objekte bereitstellen. Diese Objekte werden automatisch freigegeben, wenn ihr letzter Verweis den gültigen Bereich verlässt und die App beendet wird. Weitere Informationen finden Sie unter [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr). Beispiel für diese Objekte: Konstantenpuffer, Shaderobjekte – [Vertex-Shader](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [Pixel-Shader](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders) und Shaderressourcenobjekte.
* Konstantenpuffer sind in dieser Klasse definiert, um verschiedene Daten aufzunehmen, die zum Rendern benötigt werden.
    * Durch die Verwendung mehrerer Konstantenpuffer für unterschiedliche Häufigkeiten kann die Menge an Daten reduziert werden, die pro Frame an die GPU gesendet werden müssen. In diesem Beispiel werden die Konstanten basierend auf der Häufigkeit, mit der sie aktualisiert werden müssen, auf verschiedene Puffer verteilt. Dies ist die empfohlene Methode für die Direct3D-Programmierung. 
    * In diesem Beispielspiel sind 4 Konstantenpuffer definiert.
        1. __m\_ConstantBufferNeverChanges__ enthält die Beleuchtungsparameter. Es wird einmal in der Methode __FinalizeCreateGameDeviceResources__ festgelegt und ändert sich nie wieder.
        2. __m\_ConstantBufferChangeOnResize__ die Projektionsmatrix enthält. Die Projektionsmatrix hängt von der Größe und dem Seitenverhältnis des Fensters ab. Sie wird in [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) festgelegt und aktualisiert, nachdem Ressourcen in die Methode [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) geladen wurden. Beim Rendern in 3D wird sie auch zweimal pro Frame geändert.
        3. __m\_ConstantBufferChangesEveryFrame__ enthält die Ansichtsmatrix. Diese Matrix hängt von der Kameraposition und der Blickrichtung (der Projektionsnormalen) ab und ändert sich einmal pro Frame in __Render__-Methode. Dies wurde weiter oben erläutert __Rendering-Framework I: Einführung in Rendering__unter der [ __GameRenderer::Render__ Methode](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).
        4. __m\_ConstantBufferChangesEveryPrim__ enthält die Modell-Matrix und Material-Eigenschaften, der jeden Primitiv. Die Modellmatrix transformiert Scheitelpunkte aus lokalen Koordinaten in globale Koordinaten. Diese Konstanten gelten speziell für die einzelnen Grundtypen und werden für jeden Draw-Aufruf aktualisiert. Dies wurde weiter oben erläutert __Rendering-Framework I: Einführung in Rendering__unter der [primitiven Textrendering](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering).
* Shaderressourcenobjekte, die Texturen für die Grundtypen enthalten, werden in dieser Klasse ebenfalls definiert.
    * Einige Texturen sind vordefiniert ([DDS](https://docs.microsoft.com/windows/desktop/direct3ddds/dx-graphics-dds-pguide) ist ein Dateiformat, das zum Speichern von komprimierten und unkomprimierten Texturen verwendet werden kann. DDS-Texturen werden für die Wände und den Boden der Welt sowie für die Munitionskugeln verwendet.)
    * In diesem Beispiel spielen Ressourcenobjekte Shader sind: __m\_SphereTexture__, __m\_CylinderTexture__, __m\_CeilingTexture__, __m\_FloorTexture__, __m\_WallsTexture__.
* Shaderobjekte werden in dieser Klasse zur Berechnung der Grundtypen und Texturen definiert. 
    * In diesem Beispiel spielen die Shader-Objekte sind __m\_VertexShader__, __m\_VertexShaderFlat__, und __m\_PixelShader__, __m\_PixelShaderFlat__.
    * Der Vertexshader verarbeitet die Grundtypen und die grundlegende Beleuchtung. Der Pixelshader (auch als Fragmentshader bezeichnet) verarbeitet die Texturen und alle pixelgenauen Effekte.
    * Es existieren zwei Versionen dieser Shader (regulär und flach) zum Rendern verschiedener Grundtypen. Die flachen Versionen sind wesentlich einfacher und bieten weder Glanzlichter noch Pixelbeleuchtungseffekte. Sie werden für die Wände verwendet und beschleunigen das Rendern auf Geräten mit geringerer Leistung.

## <a name="gamerendererh"></a>GameRenderer.h

Betrachten wir nun den Code im Renderer-Klassenobjekt des Spiel und vergleichen ihn mit __Sample3DSceneRenderer.h__ aus der DirectX 11 App-Vorlage.

```cpp
// Class object handling the rendering of the game
ref class GameRenderer
{
internal:
    GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources);
    
    // Compared with Sample3DSceneRenderer.h in the DirectX 11 App template sample. 
    
    // These methods are present.
    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();

    // Added: Async related methods to prepare 3D game objects for rendering
    concurrency::task<void> CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game);
    void FinalizeCreateGameDeviceResources();
    concurrency::task<void> LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();
    // --- end of async related methods section

    // Added: Methods for rendering overlay
    Simple3DGameDX::IGameUIControl^ GameUIControl()  { return m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay->Visible(); }
    // --- end of rendering overlay section

    //...
    protected private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>                m_deviceResources;

    // ...

    // Shader resource objects
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_sphereTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_cylinderTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_ceilingTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_floorTexture;
    Microsoft::WRL::ComPtr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant Buffers
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferNeverChanges;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    Microsoft::WRL::ComPtr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    Microsoft::WRL::ComPtr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShader;
    Microsoft::WRL::ComPtr<ID3D11VertexShader>          m_vertexShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShader;
    Microsoft::WRL::ComPtr<ID3D11PixelShader>           m_pixelShaderFlat;
    Microsoft::WRL::ComPtr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>Konstruktor

Untersuchen wir nun den Code im __Renderer__-Konstruktor des Spiel und vergleichen ihn mit dem Konstruktor __Sample3DSceneRenderer.h__ aus der DirectX 11 App-Vorlage.

```cpp
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(const std::shared_ptr<DX::DeviceResources>& deviceResources) : //...
{
    // Compared with Sample3DSceneRenderer::Sample3DSceneRenderer in the DirectX 11 App template sample. 
    
    // Added: Create a new GameHud object to rendered text on the top left corner of the screen
    m_gameHud = ref new GameHud(
        deviceResources,
        "Windows platform samples",
        "DirectX first-person game sample"
        );
    //--- end of new GameHud object section
        
    // Added: Game info rendered as an overlay on the top right corner of the screen (eg. Hits, Shots, Time)    
    m_gameInfoOverlay = ref new GameInfoOverlay(deviceResources);
    //--- end of game info rendered as overlay section

    // These methods are present.
    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>Erstellen und Laden von DirectX-Grafikressourcen

Im Spielbeispiel (und in der Vorlage __DirectX 11-App (Universal Windows)__ von Visual Studio) wird das Erstellen und Laden von Spielressourcen mithilfe dieser beiden Methoden implementiert, die vom __GameRenderer__-Konstruktor aufgerufen werden. :

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)

## <a name="createdevicedependentresources-method"></a>CreateDeviceDependentResources-Methode

In der DirectX 11-App-Vorlage wird diese Methode verwendet, um Vertex- und Pixel-Shader asynchron zu laden, den Shader und den Konstantenpuffer zu erstellen und ein Gittermodell mit Vertices zu erstellen, die Positions- und Farbinformationen enthalten. 

Im Spielbeispiel werden diese Operationen der Szenenobjekte stattdessen auf die Methoden [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) und [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) aufgeteilt. 

Was übernimmt diese Methode in diesem Spielbeispiel?

* Variablen instanziiert (__m\_GameResourcesLoaded__ = False und __m\_LevelResourcesLoaded__ = False), der angibt, ob es sich bei Ressourcen geladen wurden, bevor Sie fortfahren Weiter zum Rendern, da es asynchron geladen werden. 
* Da HUD- und Overlay-Rendering in separaten Klassenobjekten erfolgt, rufen Sie hier die Methoden __GameHud::CreateDeviceDependentResources__ und __GameInfoOverlay::CreateDeviceDependentResources__ auf.

Hier der vollständige Code für __GameRenderer::CreateDeviceDependentResources__.

```cpp
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate if resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud->CreateDeviceDependentResources();
    m_gameInfoOverlay->CreateDeviceDependentResources();
}
```
In der folgenden Tabelle sind die Methoden aufgeführt, die zum Erstellen und Laden von Ressourcen verwendet werden. __CreateGameDeviceResourcesAsync__ und __FinalizeCreateGameDeviceResources__ wurden im Spielbeispiel hinzugefügt, sodass Ressourcen asynchron geladen werden.

|Ursprüngliche DirectX 11-App-Vorlage           |Spielbeispiel                                                                |
|-------------------------------------------|---------------------------------------------------------------------------|
|CreateDeviceDependentResources             |CreateDeviceDependentResources                                             |
|                                           | - CreateGameDeviceResourcesAsync (hinzugefügt)                                  |
|                                           | - FinalizeCreateGameDeviceResources (hinzugefügt)                               |
|CreateWindowSizeDependentResources         |CreateWindowSizeDependentResources                                         |

Bevor wir uns mit den anderen Methoden beschäftigen, die zum Erstellen und Laden von Ressourcen verwendet werden, erstellen wir zuerst den Renderer und sehen uns an, wie er in die Spielschleife passt.

## <a name="create-the-renderer"></a>Renderer erstellen

Der __GameRenderer__ wird im __GameMain__-Konstruktor erstellt. Es ruft auch die beiden anderen Methoden auf, [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) und [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method), die zum Erstellen und Laden von Ressourcen hinzugefügt werden.

```cpp

GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) : // ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // These methods are used in the DirectX 11 App template to create the class objects used for rendering. 
    // But are replaced in this game sample with GameRenderer as shown below.
    // m_sceneRenderer = std::unique_ptr<Sample3DSceneRenderer>(new Sample3DSceneRenderer(m_deviceResources));
    // m_fpsTextRenderer = std::unique_ptr<SampleFpsTextRenderer>(new SampleFpsTextRenderer(m_deviceResources));
    
    // Creation of GameRenderer
    m_renderer = ref new GameRenderer(m_deviceResources);
    
    //...

     create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();
    
    //...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>CreateGameDeviceResourcesAsync-Methode

__CreateGameDeviceResourcesAsync__ aufgerufen wird die __GameMain__ Konstruktormethode in der __erstellen\_Aufgabe__ Schleife, da es spielen Ressourcen asynchron geladen werden.
        
__CreateDeviceResourcesAsync__ ist eine Methode, die als gesonderte Reihe asynchroner Aufgaben ausgeführt wird, um die Spielressourcen zu laden. Da sie in einem gesonderten Thread ausgeführt werden soll, kann sie nur auf die Direct3D 11-Gerätemethoden (definiert in __ID3D11Device__) und nicht auf die Gerätekontextmethoden (definiert in __ID3D11DeviceContext__) zugreifen, führt also keinerlei Rendering aus.

Die Methode __FinalizeCreateGameDeviceResources__ wird im Hauptthread ausgeführt und kann auf die Direct3D 11-Gerätekontextmethoden zugreifen.

Prinzip:
* Verwenden Sie nur __ID3D11Device__-Methoden in __CreateGameDeviceResourcesAsync__, da diese auf jedem Thread ausgeführt werden können. Es wird erwartet, dass sie nicht in dem Thread laufen, in dem __GameRenderer__ erstellt wurde. 
* Verwenden Sie hier keine Methoden in __ID3D11DeviceContext__, da sie in einem einzelnen Thread und in demselben Thread wie __GameRenderer__ ausgeführt werden müssen.
* Verwenden Sie diese Methode, um Konstantenpuffer zu erstellen.
* Verwenden Sie diese Methode zum Laden von Texturen (z. B. die DDS-Dateien) und Shaderinformationen (z. B. die CSO-Dateien) in die [Shader](tutorial--assembling-the-rendering-pipeline.md#shaders).

Verwendungszweck dieser Methode:
* Erstellen Sie die 4 [Konstantenpuffer](tutorial--assembling-the-rendering-pipeline.md#buffer): __m\_ConstantBufferNeverChanges__, __m\_ConstantBufferChangeOnResize__, __m \_ConstantBufferChangesEveryFrame__, __m\_ConstantBufferChangesEveryPrim__
* Erstellen eines [sampler-state](tutorial--assembling-the-rendering-pipeline.md#sampler-state)-Objekts, das Samplinginformationen für eine Textur kapselt
* Erstellen einer Aufgabengruppe, die alle asynchronen Aufgaben enthält, die von der Methode erstellt wurden. Die Methode wartet auf den Abschluss dieser asynchronen Aufgaben und ruft dann __FinalizeCreateGameDeviceResources__ auf.
* Erstellen eines Lademoduls mithilfe von [Basic Loader](tutorial--assembling-the-rendering-pipeline.md#basicloader). Fügen Sie die asynchronen Ladevorgänge des Lademoduls als Aufgaben in die zuvor erstellte Aufgabengruppe ein.
* Methoden wie __BasicLoader::LoadShaderAsync__ and  __BasicLoader::LoadTextureAsync__ dienen zum Laden von:
    * kompilierten Shaderobjekten (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso). Weitere Informationen finden Sie unter [Verschiedene Shader-Dateiformate](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats).
    * Spielen Sie bestimmte Texturen (Medienobjekte\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).

```cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method.  It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC.
    // For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476092.aspx
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));
    
    bd.Usage = D3D11_USAGE_DEFAULT;
    // ...
    
    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476501.aspx
    
    DX::ThrowIfFailed(
        d3dDevice->CreateBuffer(&bd, nullptr, &m_constantBufferNeverChanges) 
        );
    // ...
    
    // Define D3D11_SAMPLER_DESC. For API ref, go to: https://msdn.microsoft.com/library/windows/desktop/ff476207.aspx
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. 
    // For API ref, go to: https://msdn.microsoft.com/en-us/library/windows/desktop/aa366920(v=vs.85).aspx
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    // ...
    
    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    DX::ThrowIfFailed(
        d3dDevice->CreateSamplerState(&sampDesc, &m_samplerLinear)
        );

    // Start the async tasks to load the shaders and textures (resources).
    
    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. 
    // For more info, go to: https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader
    BasicLoader^ loader = ref new BasicLoader(d3dDevice);

    std::vector<task<void>> tasks;

    uint32 numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the BasicLoader::LoadShaderAsync method
    // Push these method calls into a list of tasks
    tasks.push_back(loader->LoadShaderAsync("VertexShader.cso", PNTVertexLayout, numElements, &m_vertexShader, &m_vertexLayout));
    tasks.push_back(loader->LoadShaderAsync("VertexShaderFlat.cso", nullptr, numElements, &m_vertexShaderFlat, nullptr));
    tasks.push_back(loader->LoadShaderAsync("PixelShader.cso", &m_pixelShader));
    tasks.push_back(loader->LoadShaderAsync("PixelShaderFlat.cso", &m_pixelShaderFlat));

    // Make sure the previous versions are set to NULL before any of the textures are loaded.
    m_sphereTexture = nullptr;
    // ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds, cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks
    tasks.push_back(loader->LoadTextureAsync("Assets\\seafloor.dds", nullptr, &m_sphereTexture));
    // ...
    
    tasks.push_back(create_task([]()
    {
        // Simulate loading additional resources by introducing a delay.
        wait(GameConstants::InitialLoadingDelay);
    }));

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    return when_all(tasks.begin(), tasks.end());
}
```

## <a name="finalizecreategamedeviceresources-method"></a>FinalizeCreateGameDeviceResources-Methode

Die Methode __FinalizeCreateGameDeviceResources__ wird aufgerufen, nachdem alle Aufgaben zum Laden von Ressourcen in der Methode __CreateGameDeviceResourcesAsync__ abgeschlossen sind. 

* Initialisieren Sie constantBufferNeverChanges mit den Beleuchtungspositionen und der Farbe. Laden Sie die Anfangsdaten mit einem Gerätekontext-Methodenaufruf von __ID3D11DeviceContext :: UpdateSubresource__ in die Konstantenpuffer.
* Da asynchron geladene Ressourcen vollständig geladen wurden, ist es an der Zeit, sie mit den entsprechenden Spielobjekten zu verknüpfen.
* Erstellen Sie für jedes Spielobjekt das Gittermodell und das Material mit den Texturen, die geladen wurden. Dann ordnen Sie Gittermodell und Material den Spielobjekten zu.
* Die Textur für die Zielspielobjekte (die aus konzentrischen farbigen Ringen besteht, oben mit einem numerischen Wert) wird nicht aus einer Texturdatei geladen. Sie wird stattdessen mithilfe des Codes in __TargetTexture.cpp__ generiert. Die Klasse __TargetTexture__ erstellt die erforderlichen Ressourcen, um die Textur zur Initialisierungszeit in eine Off-Screen-Ressource zu zeichnen. Die resultierende Textur wird dann den entsprechenden Zielspielobjekten zugeordnet.

__FinalizeCreateGameDeviceResources__ und [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) teilen übereinstimmende Teile des Codes für folgende Aufgaben:
* Verwenden Sie __SetProjParams__, um sicherzustellen, dass die Kamera mit der richtigen Projektionsmatrix arbeitet. Weitere Informationen finden Sie unter [Kamera und Koordinatenraum](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space).
* Behandeln Sie die Rotation des Bildschirms durch Rechtsmultiplikation der 3D-Rotationsmatrix mit der Projektionsmatrix der Kamera. Aktualisieren Sie dann den __ConstantBufferChangeOnResize__-Konstantenpuffer mit der resultieRendern Projektionsmatrix.
* Legen Sie die __m\_GameResourcesLoaded__ __booleschen__ globale Variable, um anzugeben, dass die Ressourcen in den Puffern bereit für den nächsten Schritt jetzt geladen werden. Wir haben diese Variable zu Beginn in der __GameRenderer__-Konstruktormethode über die __GameRenderer::CreateDeviceDependentResources__-Methode mit __FALSE__ initialisiert. 
* Wenn dies __m\_GameResourcesLoaded__ ist __"true"__ , die szeneobjekte rendern kann stattfinden. Dies wurde im behandelt die __Rendering-Framework I: Einführung in Rendering__ Artikel unter [ __GameRenderer::Render Methode__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).

```cpp
// When creating this sample game using the DirectX 11 App template, this method needs to be created.
// This new method is called from GameMain constructor in the .then loop.
// Make sure the 2D rendering is occurring on the same thread as the main rendering.
// Note: Helper class .h and .cpp files used in this method are located in the SharedContent/cpp/GameContent folder
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize constantBufferNeverChanges with the light positions and color.
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();
    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4( 3.5f, 2.5f,  5.5f, 1.0f);
    // ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.Get(),
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

    TargetTexture^ textureGenerator = ref new TargetTexture(
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

    MeshObject^ cylinderMesh = ref new CylinderMesh(d3dDevice, 26);
    // ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    Material^ cylinderMaterial = ref new Material(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.Get(),
        m_vertexShader.Get(),
        m_pixelShader.Get()
        );

    // ...
    auto objects = m_game->RenderObjects();

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto object = objects.begin(); object != objects.end(); object++)
    {

        if ((*object)->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            (*object)->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.Get(),
                    m_vertexShaderFlat.Get(),
                    m_pixelShaderFlat.Get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            (*object)->Mesh(ref new WorldFloorMesh(d3dDevice));
        }
        // ...
        else if (Cylinder^ cylinder = dynamic_cast<Cylinder^>(*object))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (Face^ target = dynamic_cast<Face^>(*object))
        {
            const int bufferLength = 16;
            char16 str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            Platform::String^ string = ref new Platform::String(str, len);

            ComPtr<ID3D11ShaderResourceView> texture;
            textureGenerator->CreateTextureResourceView(string, &texture);
            target->NormalMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            textureGenerator->CreateHitTextureResourceView(string, &texture);
            target->HitMaterial(
                ref new Material(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.Get(),
                    m_vertexShader.Get(),
                    m_pixelShader.Get()
                    )
                );

            target->Mesh(targetMesh);
        }
        // ...
    }


    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera()->SetProjParams(
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
            XMMatrixTranspose(m_game->GameCamera()->Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.Get(),
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

## <a name="createwindowsizedependentresource-method"></a>CreateWindowSizeDependentResource-Methode

CreateWindowSizeDependentResources-Methoden werden jedes Mal aufgerufen, wenn sich Fenstergröße, Ausrichtung, Stereorendering oder die Auflösung ändert. Im Beispielspiel aktualisiert die Projektionsmatrix in __ConstantBufferChangeOnResize__.

Fenstergrößenressourcen werden auf folgende Weise aktualisiert: 
* Das App-Framework erhält eines von mehreren möglichen Ereignissen, die auf eine Änderung des Fensterstatus hinweisen. 
* Die Hauptspielschleife wird dann über das Ereignis informiert und ruft __CreateWindowSizeDependentResources__ in der Instanz der Hauptklasse (__GameMain__) auf, die dann die __CreateWindowSizeDependentResources__-Implementierung in der Spielrendererklasse (__GameRenderer__) aufruft.
* Die vorrangige Aufgabe dieser Methode ist die Sicherstellung, dass die visuellen Elemente aufgrund einer Änderung der Fenstereigenschaften nicht verwechselt oder ungültig werden.

Für dieses Spielbeispiel sind mehrere Methodenaufrufe mit der Methode [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) identisch. Eine Erläuterung des Codes finden Sie im vorherigen Abschnitt.

Die Anpassungen für die HUD- und Overlay-Fenstergröße des Spiels werden unter [Hinzufügen einer Benutzerschnittstelle](tutorial--adding-a-user-interface.md) behandelt.

```cpp
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{

    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud->CreateWindowSizeDependentResources();

    // ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    // ...

    m_gameInfoOverlay->CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera()->SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera()->Projection()),
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
}
```

## <a name="next-steps"></a>Nächste Schritte

Dies war der grundlegende Prozess zur Implementierung des Rendering-Frameworks für die Grafik eines Spiels. Je größer Ihr Spiel ist, desto mehr Abstraktionen müssten Sie implementieren, um Hierarchien von Objekttypen und Animationsverhalten zu handhaben. Sie müssen komplexere Methoden zum Laden und Verwalten von Assets wie Gittermodelle und Texturen implementieren. Als Nächstes wird das [Hinzufügen einer Benutzeroberfläche](tutorial--adding-a-user-interface.md) erläutert.