---
title: Hinzufügen von visuellen Inhalten zum Marble Maze-Beispiel
description: In diesem Dokument wird beschrieben, wie das Spiel Marble Maze Direct3D und Direct2D in der App-Umgebung der universellen Windows Plattform verwendet wird, sodass Sie die Muster erlernen und anpassen können, wenn Sie mit Ihrem eigenen Spielinhalt arbeiten.
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.date: 09/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Beispiel, DirectX, Grafiken
ms.localizationpriority: medium
ms.openlocfilehash: 8e00842a03eecb91e22cedf987830b28e960efd0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258549"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>Hinzufügen von visuellen Inhalten zum Marble Maze-Beispiel




In diesem Dokument wird beschrieben, wie das Spiel Marble Maze Direct3D und Direct2D in der App-Umgebung der universellen Windows Plattform verwendet wird, sodass Sie die Muster erlernen und anpassen können, wenn Sie mit Ihrem eigenen Spielinhalt arbeiten. Informationen dazu, wie visuelle Spielkomponenten in die Anwendungsgesamtstruktur von Marble Maze passen, finden Sie unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md).

Bei der Entwicklung der visuellen Aspekte von Marble Maze haben wir die folgenden grundlegenden Schritte ausgeführt:

1.  Erstellen eines Basisframeworks zur Initialisierung der Direct3D- und Direct2D-Umgebungen.
2.  Verwenden Sie Bild-und Modell Bearbeitungsprogramme, um die 2D-und 3D-Objekte zu entwerfen, die im Spiel angezeigt werden.
3.  Stellen Sie sicher, dass 2D-und 3D-Assets ordnungsgemäß geladen und im Spiel angezeigt werden.
4.  Integrieren von Vertex- und Pixelshadern zur Verbesserung der visuellen Qualität der Spielobjekte.
5.  Integrieren der Spiellogik, beispielsweise Animationen und Benutzereingabe.

Wir konzentrieren uns auch zuerst auf das Hinzufügen von 3D-Assets und dann auf 2D-Assets. Beispielsweise haben wir den Schwerpunkt zunächst auf die grundlegende Spiellogik gelegt, bevor wir das Menüsystem und den Timer hinzugefügt haben.

Darüber hinaus mussten wir einige dieser Schritte während des Entwicklungsprozesses mehrmals durchlaufen. Wenn wir z. b. Änderungen an den Netz-und Marmor Modellen vorgenommen haben, mussten wir auch den Shader-Code ändern, der diese Modelle unterstützt.

> [!NOTE]
> Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](https://github.com/microsoft/Windows-appsample-marble-maze).

 
Es folgen einige wichtige Punkte, die in diesem Dokument erläutert werden und die beim Arbeiten mit DirectX und visuellen Spielinhalten relevant sind, und zwar beim Initialisieren der DirectX-Grafikbibliotheken, beim Laden von Szenenressourcen und beim Aktualisieren und Rendern der Szene:

-   Das Hinzufügen von Spielinhalt umfasst normalerweise viele Schritte. Für diese Schritte ist oftmals auch eine Iteration erforderlich. Spieleentwickler konzentrieren sich häufig zuerst auf das Hinzufügen von 3D-Spielinhalten und dann auf das Hinzufügen von 2D-Inhalten
-   Erreichen Sie mehr Kunden, und liefern Sie ihnen eine großartiges Spielerlebnis, indem Sie möglichst viele Grafikhardwarekomponenten unterstützen.
-   Nehmen Sie eine saubere Trennung der Entwurfszeit- und Laufzeitformate vor. Strukturieren Sie Ihre Entwurfszeitobjekte, um die Flexibilität zu maximieren und schnelle Inhaltsiterationen zu ermöglichen. Formatieren und komprimieren Sie die Objekte so, dass sie zur Laufzeit so effizient wie möglich geladen und gerendert werden.
-   Die Erstellung der Direct3D- und Direct2D-Geräte in einer UWP-App ähnelt weitestgehend der Vorgehensweise für eine klassische Windows-Desktop-App. Ein wichtiger Unterschied besteht in der Art und Weise der Zuordnung der Swapchain zum Ausgabefenster.
-   Stellen Sie beim Entwickeln des Spiels sicher, dass das von Ihnen ausgewählte Gitterformat die wichtigen Szenarien unterstützt. Wenn für Ihr Spiel beispielsweise eine Kollision erforderlich ist, stellen Sie sicher, dass Sie Kollisionsdaten aus Ihren Gittern abrufen.
-   Separieren Sie die Spiellogik von der Renderlogik, indem Sie zunächst alle Szenenobjekte aktualisieren, bevor Sie sie rendern.
-   In der Regel zeichnen Sie Ihre 3D-Szenen Objekte und dann alle 2D-Objekte, die vor der Szene angezeigt werden.
-   Synchronisieren Sie die Zeichnung mit der vertikalen Austastung, um sicherzustellen, dass das Spiel keine Zeit für das Zeichen von Frames verwendet, die letztendlich nie auf dem Bildschirm angezeigt werden. Ein *vertikaler leerer* Wert ist die Zeit zwischen dem Abschluss der Zeichnungs Erstellung durch einen Frame und dem nächsten Frame.

## <a name="getting-started-with-directx-graphics"></a>Erste Schritte mit DirectX-Grafiken


Als wir das Spiel für das Marble Maze universelle Windows-Plattform (UWP) geplant haben C++ , wählten wir und Direct3D 11,1 aus, da Sie hervorragende Wahlmöglichkeiten für die Erstellung von 3D-Spielen haben, die eine maximale Kontrolle über Rendering und hohe Leistung erfordern. DirectX 11.1 unterstützt Hardware von DirectX 9 bis DirectX 11 und kann Sie daher dabei unterstützen, mehr Kunden auf effizientere Art und Weise zu erreichen, da Sie so den Code für frühere DirectX-Versionen nicht neu schreiben müssen.

Marble Maze verwendet Direct3D 11,1, um die 3D-Spiel Ressourcen, nämlich den Marmor und das Maze, zu Rendering. Marble Maze verwendet auch Direct2D, DirectWrite und Windows Imaging Component (WIC), um die 2D-Spiel Ressourcen zu zeichnen, z. b. die Menüs und den Timer.

Die Entwicklung eines Spiels erfordert Planung. Wenn Sie noch nicht mit DirectX-Grafiken vertraut sind, empfiehlt es sich, [DirectX: Getting Started](directx-getting-started.md) zu lesen, um sich mit den grundlegenden Konzepten der Erstellung eines UWP DirectX-Spiels vertraut zu machen. Wenn Sie dieses Dokument lesen und den Quellcode für Marble Maze durcharbeiten, finden Sie in den folgenden Ressourcen weitere ausführliche Informationen zu DirectX-Grafiken:

-   [Grafik zu Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11): Beschreibt Direct3D 11, eine leistungsstarke hardwarebeschleunigte 3D-Grafik-API zum Rendern von 3D-Geometrie auf der Windows-Plattform.
-   [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal): Beschreibt Direct2D, eine hardwarebeschleunigte, 2D-Grafik-API, die leistungsstarke und qualitativ hochwertige Rendering für 2D-Geometrie, Bitmaps und Text bereitstellt.
-   [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal): Beschreibt DirectWrite, das hochwertiges Text Rendering unterstützt.
-   [Windows Imaging Component](https://docs.microsoft.com/windows/desktop/wic/-wic-lh): Beschreibt WIC, eine erweiterbare Plattform, die eine Low-Level-API für digitale Images bereitstellt.

### <a name="feature-levels"></a>Featureebenen

Direct3D 11 führt ein Paradigma namens *Funktionsebenen*ein. Eine Featureebene ist ein klar definierter Satz mit GPU-Funktionen. Mithilfe von Featureebenen können Sie Ihr Spiel für die Ausführung mit früheren Versionen von Direct3D-Hardware vorsehen. Marble Maze unterstützt die Featureebene 9.1, da keine erweiterten Features von den höheren Ebenen erforderlich sind. Es wird empfohlen, einen möglichst großen Hardwarebereich zu unterstützen und den Spielinhalt zu skalieren, sodass all Ihre Kunden von einem großartigen Spielerlebnis profitieren können – ganz gleich, ob Sie einen normalen Computer oder Highend-Computer besitzen. Weitere Informationen zu Featureebenen finden Sie unter [Direct3D 11 unter kompatibler Hardware](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel).

## <a name="initializing-direct3d-and-direct2d"></a>Initialisieren von Direct3D und Direct2D


Ein Gerät repräsentiert die Grafikkarte. Die Erstellung der Direct3D- und Direct2D-Geräte in einer UWP-App ähnelt weitestgehend der Vorgehensweise für eine klassische Windows-Desktop-App. Der wesentliche Unterschied besteht in der Art und Weise, wie Sie die Direct3D-Swapchain mit dem Windowing-System verbinden.

Die **DeviceResources**-Klasse ist eine Grundlage für die Verwaltung von Direct3D und Direct2D. Diese Klasse kümmert sich um die allgemeine Infrastruktur, nicht um Spiel spezifische Ressourcen. Marble Maze definiert die **marblemazemain** -Klasse für die Behandlung von spielspezifischen Assets, die über einen Verweis auf ein **deviceresources** -Objekt verfügen, um ihm Zugriff auf Direct3D und Direct2D zu gewähren.

Während der Initialisierung erstellt der **deviceresources** -Konstruktor geräteunabhängige Ressourcen und die Geräte Direct3D und Direct2D.

```cpp
// Initialize the Direct3D resources required to run. 
DX::DeviceResources::DeviceResources() :
    m_screenViewport(),
    m_d3dFeatureLevel(D3D_FEATURE_LEVEL_9_1),
    m_d3dRenderTargetSize(),
    m_outputSize(),
    m_logicalSize(),
    m_nativeOrientation(DisplayOrientations::None),
    m_currentOrientation(DisplayOrientations::None),
    m_dpi(-1.0f),
    m_deviceNotify(nullptr)
{
    CreateDeviceIndependentResources();
    CreateDeviceResources();
}
```

Die **DeviceResources**-Klasse separiert diese Funktion, sodass sie leichter reagieren kann, wenn sich die Umgebung ändert. Beispielsweise ruft sie die **CreateWindowSizeDependentResources**-Methode auf, wenn sich die Fenstergröße ändert.

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>Initialisieren der Direct2D-, DirectWrite- und WIC-Factorys

Die **DeviceResources::CreateDeviceIndependentResources**-Methode erstellt die Factorys für Direct2D, DirectWrite und WIC. In DirectX-Grafiken bilden Factorys den Ausgangspunkt zum Erstellen von Grafikressourcen. Marble Maze gibt **D2D1\_Factory\_Typ\_Single\_Threadtyp** an, da er alle Zeichnungen auf dem Haupt Thread ausführt.

```cpp
// These are the resources required independent of hardware. 
void DX::DeviceResources::CreateDeviceIndependentResources()
{
    // Initialize Direct2D resources.
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
    // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    // Initialize the Direct2D Factory.
    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory2),
            &options,
            &m_d2dFactory
            )
        );

    // Initialize the DirectWrite Factory.
    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory2),
            &m_dwriteFactory
            )
        );

    // Initialize the Windows Imaging Component (WIC) Factory.
    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory2,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>Erstellen der Direct3D- und Direct2D-Geräte

Die **deviceresources:: kreatedeviceresources** -Methode ruft [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) auf, um das Geräte Objekt zu erstellen, das den Direct3D-Anzeige Adapter darstellt. Da Marble Maze die Featureebene 9,1 und höher unterstützt, gibt die **deviceresources:: deatedeviceresources** -Methode die Ebenen 9,1 bis 11,1 im **featurelevels** -Array an. Direct3D geht die Liste der Reihe nach durch und stellt die erste verfügbare Featureebene für die App bereit. Aus diesem Grund werden die **D3D\_-Funktion\_** Arrays Array Einträge vom höchsten zum niedrigsten aufgelistet, sodass die APP die höchste verfügbare Featureebene erhält. Die **DeviceResources::CreateDeviceResources**-Methode erhält das Direct3D 11.1-Gerät durch Abfragen des Direct3D 11-Geräts, das von **D3D11CreateDevice** zurückgegeben wird.

```cpp
// This flag adds support for surfaces with a different color channel ordering
// than the API default. It is required for compatibility with Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
    if (DX::SdkLayersAvailable())
    {
        // If the project is in a debug build, enable debugging via SDK Layers 
        // with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
    }
#endif

// This array defines the set of DirectX hardware feature levels this app will support.
// Note the ordering should be preserved.
// Don't forget to declare your application's minimum required feature level in its
// description.  All applications are assumed to support 9.1 unless otherwise stated.
D3D_FEATURE_LEVEL featureLevels[] =
{
    D3D_FEATURE_LEVEL_11_1,
    D3D_FEATURE_LEVEL_11_0,
    D3D_FEATURE_LEVEL_10_1,
    D3D_FEATURE_LEVEL_10_0,
    D3D_FEATURE_LEVEL_9_3,
    D3D_FEATURE_LEVEL_9_2,
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;

HRESULT hr = D3D11CreateDevice(
    nullptr,                    // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,   // Create a device using the hardware graphics driver.
    0,                          // Should be 0 unless the driver is D3D_DRIVER_TYPE_SOFTWARE.
    creationFlags,              // Set debug and Direct2D compatibility flags.
    featureLevels,              // List of feature levels this app can support.
    ARRAYSIZE(featureLevels),   // Size of the list above.
    D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for UWP apps.
    &device,                    // Returns the Direct3D device created.
    &m_d3dFeatureLevel,         // Returns feature level of device created.
    &context                    // Returns the device immediate context.
    );

if (FAILED(hr))
{
    // If the initialization fails, fall back to the WARP device.
    // For more information on WARP, see:
    // https://go.microsoft.com/fwlink/?LinkId=286690
    DX::ThrowIfFailed(
        D3D11CreateDevice(
            nullptr,
            D3D_DRIVER_TYPE_WARP, // Create a WARP device instead of a hardware device.
            0,
            creationFlags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &device,
            &m_d3dFeatureLevel,
            &context
            )
        );
}

// Store pointers to the Direct3D 11.1 API device and immediate context.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );

DX::ThrowIfFailed(
    context.As(&m_d3dContext)
    );
```

Anschließend erstellt die **DeviceResources::CreateDeviceResources**-Methode das Direct2D-Gerät. Direct2D verwendet Microsoft DXGI (DirectX Graphics Infrastructure) für die Interoperabilität mit Direct3D. DXGI ermöglicht das Freigeben von Videospeicheroberflächen zwischen Grafiklaufzeiten. Marble Maze verwendet das zugrunde liegende DXGI-Gerät vom Direct3D-Gerät zum Erstellen des Direct2D-Geräts aus der Direct2D-Factory.

```cpp
// Create the Direct2D device object and a corresponding context.
ComPtr<IDXGIDevice3> dxgiDevice;
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

Weitere Informationen zu DXGI und zur Interoperabilität zwischen Direct2D und Direct3D finden Sie unter [Übersicht über DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi) und [Übersicht über die Interoperabilität von Direct2D und Direct3D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-and-direct3d-interoperation-overview).

### <a name="associating-direct3d-with-the-view"></a>Zuordnen von Direct3D zur Ansicht

Die **DeviceResources::CreateWindowSizeDependentResources**-Methode erstellt die Grafikressourcen, die von einer bestimmten Fenstergröße abhängig sind, beispielsweise von der Swapchain und den Direct3D- und Direct2D-Renderzielen. Ein wichtiger Aspekt, in dem sich eine DirectX-UWP-App von einer Desktop-App unterscheidet, ist die Art und Weise, wie die Swapchain dem Ausgabefenster zugeordnet wird. Eine Swapchain dient der Anzeige des Puffers, in dem das Gerät rendert, auf dem Monitor. In der [Marble Maze-Anwendungs Struktur](marble-maze-application-structure.md) wird beschrieben, wie sich das Fenster für eine UWP-APP von einer Desktop-App unterscheidet. Da eine UWP-APP nicht mit [HWND](https://docs.microsoft.com/windows/desktop/WinProg/windows-data-types) -Objekten funktioniert, muss das Marble Maze die [IDXGIFactory2::](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) -Methode verwenden, um die Geräte Ausgabe der Ansicht zuzuordnen. Im folgenden Beispiel wird der Teil der **DeviceResources::CreateWindowSizeDependentResources**-Methode gezeigt, mit dem die Swapchain erstellt wird.

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &m_swapChain
        )
    );
```

Um den Energieverbrauch zu minimieren, was für Geräte mit Akku Betrieb wie Laptops und Tablets wichtig ist, ruft die **deviceresources:: createwindowsizedependentresources** -Methode die [IDXGIDevice1:: setmaximumframelatency](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) -Methode auf, um sicherzustellen, dass das Spiel nur nach dem vertikalen leeren gerendert wird. Die Synchronisierung mit dem vertikalen Leerraum wird ausführlicher im Abschnitt " [Darstellung der Szene](#presenting-the-scene) in diesem Dokument" beschrieben.

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both 
// reduces latency and ensures that the application will only render after each
// VSync, minimizing power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

Die **DeviceResources::CreateWindowSizeDependentResources**-Methode initialisiert die Grafikressourcen anhand einer Vorgehensweise, die für die meisten Spiele geeignet ist.

> [!NOTE]
> Die Begriffs *Ansicht* hat im Windows-Runtime eine andere Bedeutung als in Direct3D. In der Windows-Runtime bezieht sich eine Ansicht auf die Sammlung von Einstellungen zur Benutzeroberfläche für eine App. Dazu gehören auch der Anzeigebereich und das Eingabeverhalten sowie der zum Verarbeiten verwendete Thread. Die benötigte Konfiguration und die benötigten Einstellungen geben Sie beim Erstellen einer Ansicht an. Der Einrichtungsprozess der App-Ansicht wird unter [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md) beschrieben.
> In Direct3D hat der Begriff "Ansicht" mehrere Bedeutungen. Eine Ressourcen Ansicht definiert die unter Ressourcen, auf die eine Ressource zugreifen kann. Wenn beispielsweise ein Texturobjekt einer Shader-Ressourcenansicht zugeordnet wird, kann dieser Shader später auf die Textur zugreifen. Ein Vorteil einer Ressourcenansicht besteht darin, dass Daten auf unterschiedliche Art und Weise in verschiedenen Stufen der Rendering-Pipeline interpretiert werden können. Weitere Informationen zu Ressourcen Ansichten finden Sie unter [Ressourcen Ansichten](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-intro).
> Bei der Verwendung im Kontext einer Ansichtstransformation oder Ansichtstransformationsmatrix bezieht sich der Begriff Ansicht auf den Standort und die Ausrichtung der Kamera. Bei einer Ansichtstransformation werden Objekte in der Welt um die Position und Ausrichtung der Kamera herum neu angeordnet. Weitere Informationen zu Ansichtstransformationen finden Sie unter [Ansichtstransformation (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/view-transform). Die Verwendung von Ressourcen- und Matrixansichten in Marble Maze werden in diesem Thema detaillierter beschrieben.

 

## <a name="loading-scene-resources"></a>Laden von Szenenressourcen


Marble Maze verwendet die **basicloader** -Klasse, die in " **basicloader. h**" deklariert ist, um Texturen und Shader zu laden. Marble Maze verwendet die **sdkmesh** -Klasse, um die 3D-Netzen für das Labyrinth und den Marmor zu laden.

Um eine reagierende App zu gewährleisten, lädt Marble Maze die Szenenressourcen asynchron oder im Hintergrund. Während die Objekte im Hintergrund geladen werden, kann das Spiel auf Fensterereignisse reagieren. Dieser Prozess wird in diesem Handbuch unter [Laden von Spielobjekten im Hintergrund](marble-maze-application-structure.md#loading-game-assets-in-the-background) detaillierter erklärt.

###  <a name="loading-the-2d-overlay-and-user-interface"></a>Laden der 2D-Überlagerung und-Benutzeroberfläche

In Marble Maze ist die Überlagerung das Bild, das am oberen Rand des Bildschirms angezeigt wird. Die Überlagerung wird immer vor der Szene angezeigt. In Marble Maze enthält das Overlay das Windows-Logo und die Text Zeichenfolge **DirectX Marble Maze-Spielbeispiel**. Die Verwaltung der Überlagerung wird von der **sampleoverlay** -Klasse durchgeführt, die in " **sampleoverlay. h**" definiert ist. Obwohl wir die Überlagerung als Teil der Direct3D-Beispiele verwenden, können Sie diesen Code anpassen, um beliebige Bilder anzuzeigen, die vor Ihrer Szene erscheinen.

Ein wichtiger Aspekt der Überlagerung besteht darin, dass die **sampleoverlay** -Klasse ihren Inhalt während der Initialisierung zeichnet oder zwischenspeichert, wenn der Inhalt [](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1bitmap1) nicht geändert wird. Beim Zeichnen muss die **SampleOverlay**-Klasse lediglich die Bitmap auf dem Bildschirm zeichnen. Auf diese Art und Weise müssen nicht für jeden Frame teure Routinen wie Textzeichnungen ausgeführt werden.

Die Benutzeroberfläche (UI) besteht aus 2D-Komponenten, z. b. Menüs und Heads-up-anzeigen (HUDs), die vor der Szene angezeigt werden. Marble Maze definiert die folgenden UI-Elemente:

-   Menüelemente, die dem Benutzer das Starten des Spiels oder das Anzeigen von Bestenlisten ermöglichen.
-   Einen Timer, der drei Sekunden lang abwärts zählt, bevor das Spiel beginnt.
-   Einen Timer, der die verstrichene Spielzeit verfolgt.
-   Eine Tabelle, in der die schnellsten Zeiten aufgelistet werden.
-   Text, der **angeh** alten wird, wenn das Spiel angehalten wird.

Marble Maze definiert Spiel spezifische Benutzeroberflächen Elemente in **Userinterface. h**. Marble Maze definiert die **ElementBase**-Klasse als Basistyp für alle UI-Elemente. Die **ElementBase**-Klasse definiert Attribute wie Größe, Position, Ausrichtung und Transparenz eines UI-Elements. Zudem kontrolliert sie, wie Elemente aktualisiert und gerendert werden.

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

Durch die Bereitstellung einer allgemeinen Basisklasse für UI-Elemente muss die **UserInterface**-Klasse, mit der die Benutzeroberfläche verwaltet wird, lediglich eine Sammlung von **ElementBase**-Objekten enthalten. Dadurch werden die UI-Verwaltung vereinfacht und ein wiederverwendbarer Benutzeroberflächenmanager bereitgestellt. Marble Maze definiert Typen, die aus **ElementBase** abgeleitet werden und spielspezifische Verhalten implementieren. **HighScoreTable** definiert beispielsweise das Verhalten für die Highscore-Tabelle. Informationen zu diesen Typen finden Sie im Quellcode.

> [!NOTE]
> Da XAML es Ihnen ermöglicht, komplexe Benutzeroberflächen, wie z. b. in Simulations-und Strategie spielen, einfacher zu erstellen, sollten Sie die Verwendung von XAML zum Definieren der Benutzeroberfläche in Erwägung gezogen. Informationen zum Entwickeln einer Benutzeroberfläche in XAML in einem DirectX UWP-Spiel finden Sie unter [Erweitern des Spiel](tutorial-resources.md)Beispiels, das sich auf das Beispiel für das DirectX 3D-Spielbeispiel bezieht.

 

###  <a name="loading-shaders"></a>Laden von Shadern

Marble Maze verwendet die **BasicLoader::LoadShader**-Methode zum Laden eines Shaders aus einer Datei.

Shader bilden derzeit in Spielen die grundlegende Einheit der GPU-Programmierung. Fast die gesamte 3D-Grafik Verarbeitung wird durch Shader gesteuert, unabhängig davon, ob es sich um eine Modell Transformation und Szenen Beleuchtung handelt, oder um eine komplexere Geometrie Verarbeitung, von der Zeichen Gruppierung bis zum Mosaik. Weitere Informationen zum Shader-Programmierungsmodell finden Sie unter [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl).

Marble Maze verwendet Vertex- und Pixel-Shader. Ein Vertex-Shader wird immer für einen Eingabevertex ausgeführt und erzeugt einen Vertex als Ausgabe. Ein Pixel-Shader verwendet numerische Werte, Texturdaten, interpolierte Vertex-Werte und andere Daten, um eine Pixelfarbe als Ausgabe zu erzeugen. Da ein Shader jeweils ein Element transformiert, kann Grafikhardware, die mehrere Shader-Pipelines bereitstellt, Elementgruppen parallel bearbeiten. Die Anzahl paralleler Pipelines, die für die GPU zur Verfügung stehen, kann deutlich größer sein als die Anzahl, die für die CPU verfügbar ist. Daher kann der Durchsatz selbst mit einfachen Shadern deutlich verbessert werden.

Die **marblemazemain:: loaddeferredresources** -Methode lädt einen Scheitelpunkt-Shader und einen Pixel-Shader, nachdem die Überlagerung geladen wurde. Die Entwurfszeit Versionen dieser Shader werden in **basicvertexshader. HLSL** bzw. **basicpixelshader. HLSL**definiert. Marble Maze wendet diese Shader in der Renderphase sowohl auf die Kugel als auch auf das Labyrinth an.

Das Marble Maze-Projekt beinhaltet sowohl HLSL (Entwurfszeitformat)- als auch CSO (Laufzeitformat)-Versionen der Shader-Dateien. Zur Erstellungszeit verwendet Visual Studio den Effektcompiler "fxc.exe" zum Kompilieren der HLSL-Quelldatei in einen CSO-Binär-Shader. Weitere Informationen zum Effektcompiler-Tool finden Sie unter [Effektcompiler-Tool](https://docs.microsoft.com/windows/desktop/direct3dtools/fxc).

Der Vertex-Shader verwendet die bereitgestellten Modell-, Ansichts- und Projektionsmatrizen zum Umwandeln der Eingabegeometrie. Die Positionsdaten aus der Eingabegeometrie werden umgewandelt und zweimal ausgegeben: einmal im Bildschirmbereich, der zum Rendern benötigt wird, und noch einmal im Raum, um dem Pixel-Shader das Ausführen von Beleuchtungsberechnungen zu ermöglichen. Der Oberflächennormalvektor wird in einen Raum umgewandelt, der auch vom Pixel-Shader für die Beleuchtung verwendet wird. Die Texturkoordinaten werden unverändert an den Pixel-Shader übergeben.

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

Der Pixel-Shader empfängt die Ausgabe des Vertex-Shaders als Eingabe. Dieser Shader führt Beleuchtungsberechnungen durch, um einen auslaufenden Blickpunkt nachzuahmen, der über das Labyrinth schwebt und an der Position der Kugel ausgerichtet ist. Die Beleuchtung ist für die Oberflächen am stärksten, die direkt auf das Licht zeigen. Die diffuse Komponente läuft auf null aus, während die Oberflächennormale eine senkrechte Position zum Licht einnimmt. Zudem nimmt die Umgebungsbeleuchtung ab, während die Normale vom Licht weg zeigt. Punkte, die sich näher an der Kugel befinden (und demzufolge näher an der Mitte des Blickpunkts), werden stärker beleuchtet. Die Beleuchtung wird jedoch für Punkte unterhalb der Kugel moduliert, um einen sanften Schatten zu simulieren. In einer echten Umgebung würde ein Objekt wie die weiße Kugel den Blickpunkt diffus auf andere Objekte in der Szene reflektieren. Dies wird für die Oberflächen angenähert, die im Sichtbereich der hellen Kugelhälfte liegen. Die zusätzlichen Beleuchtungsfaktoren befinden sich im relativen Winkel und Abstand zur Kugel. Die resultierende Pixelfarbe ist eine Zusammenstellung der Stichprobentextur und dem Ergebnis der Beleuchtungsberechnungen.

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> [!WARNING]
> Der kompilierte Pixelshader enthält 32 arithmetische Anweisungen und eine Textur Anweisung. Dieser Shader sollte auf Desktopcomputern und Tablets im Highend-Bereich gute Ergebnisse liefern. Ein normaler Computer kann diesen Shader jedoch möglicherweise nicht verarbeiten und weiterhin eine interaktive Framerate bereitstellen. Ziehen Sie die typische Hardware Ihrer Zielgruppe in Erwägung, und entwickeln Sie die Shader so, dass sie zu den Hardwarefunktionen passen.

 

Die **marblemazemain:: loaddeferredresources** -Methode verwendet die **basicloader:: loadshader** -Methode, um die Shader zu laden. Im folgenden Beispiel wird der Vertex-Shader geladen. Das Lauf Zeitformat für diesen Shader lautet " **basicvertexshader. CSO**". Die **m\_Vertexshader** -Member-Variable ist ein [ID3D11VertexShader](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader) -Objekt.

```cpp
BasicLoader^ loader = ref new BasicLoader(m_deviceResources->GetD3DDevice());

D3D11_INPUT_ELEMENT_DESC layoutDesc [] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
m_vertexStride = 44; // must set this to match the size of layoutDesc above

Platform::String^ vertexShaderName = L"BasicVertexShader.cso";
loader->LoadShader(
    vertexShaderName,
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

Die **m\_inputlayout** -Member-Variable ist ein [ID3D11InputLayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout) -Objekt. Das Objekt für das Eingabelayout kapselt den Eingabestatus der Eingabeassemblerphase (IA-Phase). Eine Aufgabe der IA-Phase besteht darin, die Effizienz der Shader zu erhöhen. Dazu werden systemgenerierte Werte verwendet, die auch als *Semantik* bezeichnet werden, um nur die Grundtypen oder Vertizes zu verarbeiten, die noch nicht verarbeitet wurden.

Verwenden Sie die [ID3D11Device:: kreateinputlayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) -Methode, um ein Eingabe Layout aus einem Array von Eingabe Element Beschreibungen zu erstellen. Das Array enthält ein oder mehrere Eingabeelemente. Jedes Eingabeelement beschreibt dabei ein Vertex-Datenelement von einem Vertex-Puffer. Der vollständige Satz der Eingabeelementbeschreibungen dient der Beschreibung aller Vertex-Datenelemente von sämtlichen Vertex-Puffern, die an die IA-Phase gebunden werden. 

**layoutdesc** im obigen Code Ausschnitt zeigt die layoutbeschreibung, die von Marble Maze verwendet wird. Die Layoutbeschreibung beschreibt einen Vertex-Puffer, der vier Vertex-Datenelemente enthält. Die wichtigen Teile der einzelnen Einträge im Array sind der semantische Name, das Datenformat und das Byte-Offset. Das **POSITION**-Element gibt beispielsweise die Vertex-Position im Objektraum an. Es startet beim Byte-Offset 0 und enthält drei Gleitkommakomponenten (für insgesamt 12 Byte). Das **NORMAL**-Element gibt den Normalvektor an. Es startet beim Byte-Offset 12, da es im Layout direkt nach **POSITION** erscheint, wofür 12 Byte erforderlich sind. Das **NORMAL**-Element enthält eine nicht signierte ganze Zahl mit vier Komponenten und 32 Bit.

Vergleichen Sie das Eingabelayout mit der vom Vertex-Shader definierten **sVSInput**-Struktur, wie im folgenden Beispiel dargestellt. Die **sVSInput**-Struktur definiert die Elemente **POSITION**, **NORMAL** und **TEXCOORD0**. Die DirectX-Laufzeit ordnet jedes Element im Layout der Eingabestruktur zu, die vom Shader definiert wird.

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

Im Dokument [Semantik](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) werden alle verfügbaren Semantiken detaillierter beschrieben.

> [!NOTE]
> In einem Layout können Sie zusätzliche Komponenten angeben, die nicht verwendet werden, um mehrere Shader für die gemeinsame Verwendung desselben Layouts zu aktivieren. Beispielsweise wird das **TANGENT**-Element nicht vom Shader verwendet. Sie können das **TANGENT**-Element verwenden, wenn Sie mit Techniken wie der Normalzuordnung experimentieren möchten. Mithilfe der Normalzuordnung, die auch als Bumpmapping bezeichnet wird, können Sie auf den Oberflächen von Objekten den Bumpeffekt erzeugen. Weitere Informationen zum Bumpmapping finden Sie unter [Bumpmapping (Direct3D 9)](https://docs.microsoft.com/windows/desktop/direct3d9/bump-mapping).

 

Weitere Informationen über die eingabeassemblyphase finden Sie in der Eingabe [-Assembler-](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage) Phase und [in den ersten Schritten mit der Eingabe-Assembler-](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage-getting-started)Phase.

Der Prozess der Verwendung von Vertex- und Pixel-Shadern zum Rendern der Szene wird später in diesem Dokument im Abschnitt [Rendern der Szene](#rendering-the-scene) beschrieben.

### <a name="creating-the-constant-buffer"></a>Erstellen des Konstantenpuffers

Der Direct3D-Puffer gruppiert eine Sammlung von Daten. Ein Konstantenpuffer ist ein Puffertyp, mit dessen Hilfe Sie Daten an Shader übergeben können. Marble Maze verwendet einen Konstantenpuffer, um die Modellansicht (oder Weltansicht) sowie die Projektmatrizen für das aktive Szenenobjekt aufzunehmen.

Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: loaddeferredresources** -Methode einen konstanten Puffer erstellt, der später Matrix Daten enthält. Im Beispiel wird ein **D3D11-\_Puffer\_** der Struktur "" erstellt, der das **\_BIND\_Constant\_Buffer** -Flag verwendet, um die Verwendung als Konstanten Puffer anzugeben. In diesem Beispiel wird diese Struktur an die [ID3D11Device:: up Buffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) -Methode übergeben. Die **m-\_constantbuffer** -Variable ist ein [ID3D11Buffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) -Objekt.

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};

// Multiple of 16 bytes
constantBufferDesc.ByteWidth = ((sizeof(ConstantBuffer) + 15) / 16) * 16;

constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;

// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &constantBufferDesc,
        nullptr,    // leave the buffer uninitialized
        &m_constantBuffer
        )
    );
```

Die **marblemazemain:: Update** -Methode aktualisiert die **constantbuffer** -Objekte später, eine für das Maze und eine für den Marmor. Die **marblemazemain:: Rendering** -Methode bindet dann jedes **constantbuffer** -Objekt an den konstanten Puffer, bevor jedes Objekt gerendert wird. Das folgende Beispiel zeigt die **constantbuffer** -Struktur, die sich in **marblemazemain. h**befindet.

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;

    XMFLOAT3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Um besser zu verstehen, wie Konstantenpuffer Shader-Code zugeordnet werden, vergleichen Sie die **constantbuffer** -Struktur in **marblemazemain. h** mit dem Konstanten Konstanten **constantbuffer** , der vom Vertex-Shader in **basicvertexshader. HLSL**definiert wird:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

Das Layout der **ConstantBuffer**-Struktur stimmt mit dem **cbuffer**-Objekt überein. Die **cbuffer**-Variable gibt das Register b0 an. Demnach werden die Daten des Konstantenpuffers im Register 0 gespeichert. Die **marblemazemain:: Rendering** -Methode gibt Register 0 an, wenn der Konstante Puffer aktiviert wird. Auf diesen Prozess wird später in diesem Dokument detaillierter eingegangen.

Weitere Informationen zu Konstantenpuffern finden Sie unter [Einführung in Puffer in Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro). Weitere Informationen zum Register-Schlüsselwort finden Sie unter [Register](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-register).

###  <a name="loading-meshes"></a>Laden von Gittern

Marble Maze verwendet SDK-Mesh als Laufzeitformat, da dieses Format eine grundlegende Methode zum Laden von Gitterdaten für Beispielanwendungen bereitstellt. Für die Produktionsverwendung sollten Sie ein Gitterformat nutzen, das die spezifischen Anforderungen Ihres Spiels erfüllt.

Die **marblemazemain:: loaddeferredresources** -Methode lädt Mesh-Daten, nachdem der Scheitelpunkt und die Pixel-Shader geladen wurden. Bei einem Gitter handelt es sich um eine Sammlung von Vertex-Daten, die oftmals Informationen wie Positionen, Normale, Farben, Materialien und Texturkoordinaten enthält. Meshes werden in der Regel in 3D-Erstellungs Software erstellt und in Dateien verwaltet, die vom Anwendungscode getrennt sind. Die Kugel und das Labyrinth sind zwei Beispiele für Gitter, die im Spiel verwendet werden.

Marble Maze verwendet die **SDKMesh**-Klasse zum Verwalten von Gittern. Diese Klasse ist in " **sdkmesh. h**" deklariert. **SDKMesh** stellt Methoden zum Laden, Rendern und Löschen von Gitterdaten bereit.

> [!IMPORTANT]
> In Marble Maze wird das SDK-Mesh-Format verwendet, und die **sdkmesh** -Klasse wird nur zur Veranschaulichung bereitstellt. Obwohl das SDK-Mesh-Format hilfreich zum Lernen und Erstellen von Prototypen ist, handelt es sich dabei um ein sehr einfaches Format, das die Anforderungen der meisten Spielentwicklungen möglicherweise nicht erfüllt. Es wird empfohlen, ein Gitterformat zu verwenden, das die spezifischen Anforderungen Ihres Spiels erfüllt.

 

Das folgende Beispiel zeigt, wie die **marblemazemain:: loaddeferredresources** -Methode die **sdkmesh:: Create** -Methode verwendet, um Mesh-Daten für das Maze und die Kugel zu laden.

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  <a name="loading-collision-data"></a>Laden von Kollisionsdaten

Obwohl der Schwerpunkt dieses Abschnitts nicht darauf liegt, wie Marble Maze die physikalische Simulation zwischen der Kugel und dem Labyrinth implementiert, gilt es zu beachten, dass die Gittergeometrie für das physikalische System beim Laden der Gitter gelesen wird.

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

Die Art und Weise, wie Sie Kollisionsdaten laden, hängt größtenteils vom verwendeten Lauf Zeitformat ab. Weitere Informationen dazu, wie Marble Maze die Kollisions Geometrie aus einer SDK-Mesh-Datei lädt, finden Sie unter der **marblemazemain:: extracttrianglesfrommesh** -Methode im Quellcode.

## <a name="updating-game-state"></a>Aktualisieren des Spielzustands


Marble Maze separiert die Spiellogik von der Renderlogik, indem zunächst alle Szenenobjekte vor dem Rendern aktualisiert werden.

Die [Anwendungs Struktur von Marble Maze](marble-maze-application-structure.md) beschreibt die Haupt-Spiel Schleife. Die Aktualisierung der Szene, die Bestandteil der Spielschleife ist, erfolgt nach dem Verarbeiten von Windows-Ereignissen und Eingaben und vor dem Rendern der Szene. Die **marblemazemain:: Update** -Methode behandelt das Update der Benutzeroberfläche und des Spiels.

### <a name="updating-the-user-interface"></a>Aktualisieren der Benutzeroberfläche

Die **marblemazemain:: Update** -Methode ruft die **Userinterface:: Update** -Methode auf, um den Status der Benutzeroberfläche zu aktualisieren.

```cpp
UserInterface::GetInstance().Update(
    static_cast<float>(m_timer.GetTotalSeconds()), 
    static_cast<float>(m_timer.GetElapsedSeconds()));
```

Die **UserInterface::Update**-Methode aktualisiert jedes Element in der UI-Sammlung.

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

Klassen, die von **elementbase** (definiert in **Userinterface. h**) abgeleitet werden, implementieren die **Update** -Methode, um bestimmte Verhaltensweisen auszuführen. Die **StopwatchTimer::Update**-Methode aktualisiert beispielsweise die abgelaufene Zeit anhand der bereitgestellten Menge und aktualisiert den später angezeigten Text.

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  <a name="updating-the-scene"></a>Aktualisieren der Szene

Die **marblemazemain:: Update** -Methode aktualisiert das Spiel auf der Grundlage des aktuellen Zustands des Zustands Automaten (dem in **m_gameState**gespeicherten **gamestate**). Wenn sich das Spiel im aktiven Zustand (**gamestate:: ingameactive**) befindet, aktualisiert Marble Maze die Kamera, um dem Marmor zu folgen, aktualisiert den Ansichts Matrix Teil der Konstanten Puffer und aktualisiert die Physik-Simulation.

Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: Update** -Methode die Position der Kamera aktualisiert. Marble Maze verwendet die **m\_resetcamera** -Variable, um zu markieren, dass die Kamera zurückgesetzt werden muss, damit Sie sich direkt über dem Marmor befindet. Die Kamera wird zurückgesetzt, wenn das Spiel startet oder die Kugel durch das Labyrinth fällt. Wenn das Hauptmenü oder die Highscoreanzeige aktiv ist, wird die Kamera auf eine konstante Position festgelegt. Andernfalls verwendet Marble Maze den *timeDelta*-Parameter zum Interpolieren der Kameraposition zwischen den Ist- und Zielpositionen. Die Zielposition befindet sich leicht über und vor der Kugel. Die Verwendung der abgelaufenen Framezeit ermöglicht der Kamera das schrittweise Folgen oder Verfolgen der Kugel.

```cpp
static float eyeDistance = 200.0f;
static XMFLOAT3A eyePosition = XMFLOAT3A(0, 0, 0);

// Gradually move the camera above the marble.
XMFLOAT3A targetEyePosition;
XMStoreFloat3A(
    &targetEyePosition, 
    XMLoadFloat3A(&marblePosition) - (XMLoadFloat3A(&g) * eyeDistance));

if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&eyePosition) 
            + ((XMLoadFloat3A(&targetEyePosition) - XMLoadFloat3A(&eyePosition)) 
                * min(1, static_cast<float>(m_timer.GetElapsedSeconds()) * 8)
            )
    );
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&marblePosition) + XMVectorSet(75.0f, -150.0f, -75.0f, 0.0f));

    m_camera->SetViewParameters(
        eyePosition, 
        marblePosition, 
        XMFLOAT3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, XMFLOAT3(0.0f, 1.0f, 0.0f));
}
```

Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: Update** -Methode die Konstanten Puffer für den Marmor und das Maze aktualisiert. Die Modell- oder Weltmatrix des Labyrinths bleibt immer die Identitätsmatrix. Außer der Hauptdiagonale, deren Elemente alle Eins lauten, handelt es sich bei der Identitätsmatrix um eine Quadratmatrix, die aus Nullen besteht. Die Modellmatrix der Kugel basiert auf der zugehörigen Positionsmatrix multipliziert mit der zugehörigen Rotationsmatrix.

```cpp
// Update the model matrices based on the simulation.
XMStoreFloat4x4(&m_mazeConstantBufferData.model, XMMatrixIdentity());

XMStoreFloat4x4(
    &m_marbleConstantBufferData.model, 
    XMMatrixTranspose(
        XMMatrixMultiply(
            marbleRotationMatrix, 
            XMMatrixTranslationFromVector(XMLoadFloat3A(&marblePosition))
        )
    )
);

// Update the view matrix based on the camera.
XMFLOAT4X4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

Informationen dazu, wie die **marblemazemain:: Update** -Methode Benutzereingaben liest und die Bewegung des Marmors simuliert, finden [Sie unter Hinzufügen von Eingaben und Interaktivität zum Beispiel für Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md).

## <a name="rendering-the-scene"></a>Rendern der Szene


Das Rendern einer Szene umfasst normalerweise die folgenden Schritte.

1.  Festlegen des Tiefenschablonenpuffers für das aktuelle Renderziel.
2.  Löschen der Render- und Schablonenansichten.
3.  Vorbereiten der Vertex- und Pixel-Shader für die Zeichnung.
4.  Rendering der 3D-Objekte in der Szene.
5.  Rendering beliebiger 2D-Objekte, die vor der Szene angezeigt werden sollen.
6.  Darstellen des gerenderten Bilds auf dem Monitor.

Mit der **marblemazemain:: Rendering** -Methode werden die Ansichten des Renderziels und der tiefen Schablone gebunden, diese Ansichten gelöscht, die Szene gezeichnet und die Überlagerung gezeichnet.

###  <a name="preparing-the-render-targets"></a>Vorbereiten der Renderziele

Vor dem Rendern der Szene müssen Sie den Tiefenschablonenpuffer für das aktuelle Renderziel festlegen. Wenn nicht feststeht, dass die Szene über alle Bildschirmpixel gezeichnet wird, löschen Sie auch die Render- und Schablonenansichten. Marble Maze löscht die Render- und Schablonenansichten in jedem Frame, um sicherzustellen, dass keine sichtbaren Artefakte aus dem vorherigen Frame vorhanden sind.

Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: Rendering** -Methode die [Verknüpfung id3d11devicecontext aus:: omsetrendertargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) -Methode aufruft, um das Renderziel und den tiefen Schablonen Puffer als aktuelle festzulegen.

```cpp
auto context = m_deviceResources->GetD3DDeviceContext();

// Reset the viewport to target the whole screen.
auto viewport = m_deviceResources->GetScreenViewport();
context->RSSetViewports(1, &viewport);

// Reset render targets to the screen.
ID3D11RenderTargetView *const targets[1] = 
    { m_deviceResources->GetBackBufferRenderTargetView() };

context->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

// Clear the back buffer and depth stencil view.
context->ClearRenderTargetView(
    m_deviceResources->GetBackBufferRenderTargetView(), 
    DirectX::Colors::Black);

context->ClearDepthStencilView(
    m_deviceResources->GetDepthStencilView(), 
    D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 
    1.0f, 
    0);
```

Die [ID3D11RenderTargetView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) -und [ID3D11DepthStencilView](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) -Schnittstellen unterstützen den Textur Ansichts Mechanismus, der von Direct3D 10 und höher bereitgestellt wird. Weitere Informationen zu Texturansichten finden Sie unter [Texturansichten (Direct3D 10)](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-access-views). Die Methode [omseetrendertargets](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) bereitet die Ausgabe-Fusion-Phase der Direct3D-Pipeline vor. Weitere Informationen zur Ausgabezusammenführungsphase finden Sie unter [Ausgabezusammenführungsphase](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage).

### <a name="preparing-the-vertex-and-pixel-shaders"></a>Vorbereiten der Vertex- und Pixel-Shader

Führen Sie vor dem Rendern der Szenenobjekte die folgenden Schritte aus, um die Vertex- und Pixel-Shader für die Zeichnung vorzubereiten:

1.  Legen Sie das Shader-Eingabelayout als aktuelles Layout fest.
2.  Legen Sie die Vertex und Pixel-Shader als aktuelle Shader fest.
3.  Aktualisieren Sie die Konstantenpuffer mit Daten, die Sie an die Shader übergeben müssen.

> [!IMPORTANT]
> Marble Maze verwendet ein paar aus Vertex-und Pixel-Shadern für alle 3D-Objekte. Wenn für Ihr Spiel mehrere Shader-Paare verwendet werden, müssen Sie diese Schritte immer dann ausführen, wenn Sie Objekte zeichnen, für die verschiedene Shader verwendet werden. Zur Reduzierung des Aufwands bezüglich der Änderung des Shader-Zustands wird empfohlen, die Renderaufrufe für alle Objekte zu gruppieren, die dieselben Shader verwenden.

 

Im Abschnitt [Laden von Shadern](#loading-shaders) in diesem Dokument wird beschrieben, wie das Eingabelayout beim Erstellen des Vertex-Shaders erstellt wird. Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: Rendering** -Methode die [Verknüpfung id3d11devicecontext aus:: iasetinputlayout](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) -Methode verwendet, um dieses Layout als aktuelles Layout festzulegen.

```cpp
m_deviceResources->GetD3DDeviceContext()->IASetInputLayout(m_inputLayout.Get());
```

Das folgende Beispiel zeigt, wie die **marblemazemain:: Rendering** -Methode die [Verknüpfung id3d11devicecontext aus:: vssetshader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) -Methode und die [Verknüpfung id3d11devicecontext aus::P ssetshader](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) -Methode verwendet, um den Scheitelpunkt und die Pixel-Shader als aktuelle Shader festzulegen.

```cpp
// Set the vertex shader stage state.
m_deviceResources->GetD3DDeviceContext()->VSSetShader(
    m_vertexShader.Get(),   // use this vertex shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetShader(
    m_pixelShader.Get(),    // use this pixel shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetSamplers(
    0,                          // starting at the first sampler slot
    1,                          // set one sampler binding
    m_sampler.GetAddressOf());  // to use this sampler
```

Nachdem **marblemazemain:: Rendering** die Shader und deren Eingabe Layout festgelegt hat, wird die [Verknüpfung id3d11devicecontext aus:: updatesubresource](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) -Methode verwendet, um den konstanten Puffer mit den Modell-, Ansichts-und Projektions Matrizen für das Maze zu aktualisieren. Die **UpdateSubresource**-Methode kopiert die Matrixdaten aus dem CPU-Speicher in den GPU-Speicher. Beachten Sie, dass die Modell-und Ansichts Komponenten der **constantbuffer** -Struktur in der **marblemazemain:: Update** -Methode aktualisiert werden. Die **marblemazemain:: Rendering** -Methode ruft dann die [Verknüpfung id3d11devicecontext aus:: vssetconstantbuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) -Methode und die [Verknüpfung id3d11devicecontext aus::P ssetconstantbuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers) -Methode auf, um diesen Konstanten Puffer als aktuellen festzulegen.

```cpp
// Update the constant buffer with the new data.
m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0);

m_deviceResources->GetD3DDeviceContext()->VSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer

m_deviceResources->GetD3DDeviceContext()->PSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer
```

Die **marblemazemain:: Rendering** -Methode führt ähnliche Schritte aus, um den zu rendernden Marmor vorzubereiten.

### <a name="rendering-the-maze-and-the-marble"></a>Rendern des Labyrinths und der Kugel

Nachdem Sie die aktuellen Shader aktiviert haben, können Sie die Szenenobjekte zeichnen. Die **marblemazemain:: Rendering** -Methode ruft die **sdkmesh:: Rendering** -Methode auf, um das Maze-Mesh zu renhnen.

```cpp
m_mazeMesh.Render(
    m_deviceResources->GetD3DDeviceContext(), 
    0, 
    INVALID_SAMPLER_SLOT, 
    INVALID_SAMPLER_SLOT);
```

Die **marblemazemain:: Rendering** -Methode führt ähnliche Schritte aus, um den Marmor zu erzeugen.

Wie bereits in diesem Dokument erwähnt wurde, wird die **SDKMesh**-Klasse zu Demonstrationszwecken bereitgestellt. Sie wird jedoch nicht für die Verwendung in einem Spiel in Produktionsqualität empfohlen. Beachten Sie jedoch, dass die **sdkmesh:: rendermesh** -Methode, die von **sdkmesh:: Rendering**aufgerufen wird, die Methoden [Verknüpfung id3d11devicecontext aus:: iasetvertexbuffers](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) und [Verknüpfung id3d11devicecontext aus:: iasetindexbuffer](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) verwendet, um den aktuellen Scheitelpunkt und Index Puffer festzulegen, die das Mesh definieren, und die Methode [:D Verknüpfung id3d11devicecontext aus](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexedinstanced) Weitere Informationen zum Arbeiten mit Vertex- und Indexpuffern finden Sie unter [Einführung in Puffer in Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro).

### <a name="drawing-the-user-interface-and-overlay"></a>Zeichnen der Benutzeroberfläche und Überlagerung

Nach dem Zeichnen von 3D-Szenen Objekten zeichnet das Marble Maze die 2D-Benutzeroberflächen Elemente, die vor der Szene angezeigt werden.

Die **marblemazemain:: Rendering** -Methode wird beendet, indem die Benutzeroberfläche und die Überlagerung gezeichnet werden.

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render(m_deviceResources->GetOrientationTransform2D());

m_deviceResources->GetD3DDeviceContext()->BeginEventInt(L"Render Overlay", 0);
m_sampleOverlay->Render();
m_deviceResources->GetD3DDeviceContext()->EndEvent();
```

Die **Userinterface:: Rendering** -Methode verwendet ein [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) -Objekt, um die Benutzeroberflächen Elemente zu zeichnen. Diese Methode legt den Zeichnungszustand fest, sie zeichnet alle aktiven UI-Elemente und stellt dann den vorherigen Zeichnungszustand wieder her.

```cpp
void UserInterface::Render(D2D1::Matrix3x2F orientation2D)
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(orientation2D);

    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

Die **SampleOverlay::Render**-Methode verwendet eine ähnliche Technik zum Zeichnen der Überlagerungsbitmap.

###  <a name="presenting-the-scene"></a>Darstellen der Szene

Nachdem alle 2D-und 3D-Szenen Objekte gezeichnet wurden, stellt das Marmor Labyrinth das gerenderte Bild für den Monitor dar. Das Spiel synchronisiert die Zeichnung mit der vertikalen Austastung, um sicherzustellen, dass keine Zeit für das Zeichnen von Frames verwendet wird, die letztendlich nie auf dem Bildschirm angezeigt werden. Marble Maze verarbeitet beim Darstellen der Szene auch Geräteänderungen.

Nach dem Zurückgeben der **marblemazemain:: Rendering** -Methode ruft die Game-Schleife die **DX::D eviceresources::P Resent** -Methode auf, um das gerenderte Bild an den Monitor oder die Anzeige zu senden. Die **DX::D eviceresources::P Resent** -Methode ruft [idxgiswap:P Chain](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) auf, um den aktuellen Vorgang auszuführen, wie im folgenden Beispiel gezeigt:

```cpp
// The first argument instructs DXGI to block until VSync, putting the application
// to sleep until the next VSync. This ensures we don't waste any cycles rendering
// frames that will never be displayed to the screen.
HRESULT hr = m_swapChain->Present(1, 0);
```

In diesem Beispiel ist **m\_SwapChain** ein [IDXGISwapChain1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) -Objekt. Die Initialisierung dieses Objekts wird im Abschnitt [Initialisieren von Direct3D und Direct2D](#initializing-direct3d-and-direct2d) in diesem Dokument beschrieben.

Der erste Parameter für [idxgiswapchain::P Resent](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1), *syncinterval*, gibt die Anzahl der vertikalen Leerräume an, die gewartet werden soll, bevor der Frame dargestellt wird. Marble Maze gibt den Wert 1 an, sodass bis zur nächsten vertikalen Austastung gewartet wird.

Die [idxgiswapchain::P Resent](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) -Methode gibt einen Fehlercode zurück, der angibt, dass das Gerät entfernt wurde oder anderweitig fehlgeschlagen ist. In diesem Fall initialisiert Marble Maze das Gerät erneut.

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

## <a name="next-steps"></a>Nächste Schritte


Lesen Sie den Abschnitt [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md), um Informationen zu einigen wichtigen Verfahren zu erhalten, die Sie beim Arbeiten mit Eingabegeräten beachten sollten. In diesem Dokument wird erläutert, wie Marble Maze Finger Eingaben, Beschleunigungsmesser, Xbox-Controller und Maus Eingaben unterstützt.

## <a name="related-topics"></a>Verwandte Themen


* [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze-Anwendungs Struktur](marble-maze-application-structure.md)
* [Entwickeln von Marble Maze, einem UWP- C++ Spiel in und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




