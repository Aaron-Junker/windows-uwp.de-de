---
title: Interoperabilität von DirectX und XAML
description: In Ihrem Spiel für die universelle Windows-Plattform (UWP) können Sie eine Kombination aus Extensible Application Markup Language (XAML) und Microsoft DirectX verwenden.
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, XAML-Interop
ms.localizationpriority: medium
ms.openlocfilehash: fc5e5323f509759754822849bd7dc93e7a45eaee
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156474"
---
# <a name="directx-and-xaml-interop"></a>Interoperabilität von DirectX und XAML

In Spielen oder Apps für die universelle Windows-Plattform (UWP) können Sie eine Kombination aus Extensible Application Markup Language (XAML) und Microsoft DirectX verwenden. Dank der Kombination von XAML und DirectX können Sie flexible Frameworks für die Benutzeroberflächen erstellen, die mit den über DirectX gerenderten Inhalten kompatibel sind. Dies ist besonders für grafisch aufwendige Apps von Vorteil. In diesem Thema beschäftigen wir uns mit der Struktur einer UWP-App mit DirectX und gehen auf wichtige Typen ein, die beim Erstellen Ihrer UWP-App für die Zusammenarbeit mit DirectX erforderlich sind.

Falls Ihre App hauptsächlich 2-D-Rendering verwendet, empfiehlt sich unter Umständen die Verwendung der [Win2D](https://github.com/microsoft/win2d)-Windows-Runtime-Bibliothek. Diese Bibliothek wird von Microsoft verwaltet und basiert auf den Direct2D-Kerntechnologien. Sie trägt erheblich zur Vereinfachung des Verwendungsmusters für die Implementierung von 2D-Grafik bei und enthält hilfreiche Abstraktionen für einige der in diesem Dokument beschriebenen Techniken. Ausführlichere Informationen finden Sie auf der Projektseite. Bei diesem Dokument handelt es sich um einen Leitfaden für App-Entwickler, die sich *gegen* die Verwendung von Win2D entschieden haben.

> [!NOTE]
> DirectX-APIs sind nicht als Windows-Runtime Typen definiert, aber Sie können in der Regel [C++/WinRT](../cpp-and-winrt-apis/index.md) verwenden, um XAML-UWP-Komponenten zu entwickeln, die mit DirectX zusammenarbeiten. Sie können UWP-DirectX-Apps auch mit C# und XAML erstellen, indem Sie die DirectX-Aufrufe in eine separate Windows-Runtime-Metadatendatei einschließen.

## <a name="xaml-and-directx"></a>XAML und DirectX

DirectX bietet mit Direct2D und Microsoft Direct3D zwei leistungsstarke Bibliotheken für 2D- und 3D-Grafiken. Obwohl XAML grundlegende 2D-Grundtypen und -Effekte unterstützt, erfordern zahlreiche Apps, wie Modellierungs-Apps und Spiele, eine komplexere Grafikunterstützung. In diesen Fällen können Sie Grafiken teilweise oder vollständig mit Direct2D und Direct3D rendern und XAML für alles andere verwenden.

Wenn Sie eine benutzerdefinierte Interoperabilität zwischen XAML und DirectX implementieren möchten, müssen Sie mit den beiden folgenden Konzepten vertraut sein:

-   Bei gemeinsam genutzten Flächen (Shared Surfaces) handelt es sich um Anzeigebereiche bestimmter Größe, die von XAML definiert werden. In diesen Bereichen können Sie indirekt mit DirectX und [Windows::UI::Xaml::Media::ImageSource](/uwp/api/windows.ui.xaml.media.imagesource)-Typen zeichnen. Bei gemeinsam genutzten Flächen steuern Sie nicht, wann genau neuer Inhalt auf dem Bildschirm angezeigt wird. Stattdessen werden Änderungen an den gemeinsam genutzten Flächen mit den Updates des XAML-Frameworks synchronisiert.
-   [Swapchains](/windows/desktop/direct3d9/what-is-a-swap-chain-) stellen eine Sammlung von Puffern dar, die verwendet werden, um Grafiken mit minimaler Verzögerung anzuzeigen. Swapchains werden üblicherweise mit 60 Frames pro Sekunde und separat vom UI-Thread aktualisiert. Im Gegensatz zu CPU-Ressourcen haben Swapchains allerdings einen höheren Arbeitsspeicherbedarf, um schnelle Aktualisierungen zu unterstützen, und ihre Verwendung ist komplizierter, da mehrere Threads verwaltet werden müssen.

Überlegen Sie sich, wofür Sie DirectX verwenden. Wird es für die Zusammenstellung und Animierung eines einzelnen Steuerelements verwendet, das in die Abmessungen des Anzeigefensters passt? Wird eine Ausgabe enthalten sein, die wie in einem Spiel in Echtzeit gerendert und gesteuert werden muss? In diesen Fällen empfiehlt sich wahrscheinlich die Implementierung einer Swapchain. Andernfalls können Sie normalerweise auch eine gemeinsam genutzte Fläche verwenden.

Nachdem Sie festgelegt haben, wie Sie DirectX verwenden möchten, verwenden Sie einen dieser Windows-Runtime Typen, um DirectX-Rendering in Ihre UWP-App einzubinden:

-   Wenn Sie ein statisches Bild erstellen oder ein komplexes Bild in ereignisgesteuerten Intervallen zeichnen möchten, zeichnen Sie mit der Klasse [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) in eine gemeinsam genutzte Fläche. Dieser Typ ermöglicht die Behandlung einer DirectX-Zeichenoberfläche von bestimmter Größe. In der Regel wird dieser Typ beim Erstellen eines Bilds oder einer Textur als Bitmap verwendet, um in einem Dokument oder als UI-Element angezeigt zu werden. Der Typ ist in Szenarien mit Echtzeitinteraktivität wie hochleistungsfähige Spiele nicht gut geeignet. Dies liegt daran, dass Updates des **SurfaceImageSource**-Objekts mit Updates der XAML-Benutzeroberfläche synchronisiert werden. Somit kann es zu Verzögerungen beim visuellen Feedback kommen, z. B. in Form einer schwankenden Bildrate oder einer wahrgenommenen verzögerten Reaktion auf Echtzeiteingaben. Die Aktualisierungen sind jedoch noch schnell genug für dynamische Steuerelemente oder Datensimulationen.

-   Wenn die Größe des Bilds den verfügbaren Platz auf dem Bildschirm übersteigt und es vom Benutzer verschoben und gezoomt werden kann, verwenden Sie die Klasse [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource). Dieser Typ ermöglicht die Behandlung einer DirectX-Zeichenoberfläche, die größer als der Bildschirm ist. Wie [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) wird diese Klasse für die dynamische Erstellung eines komplexen Bilds oder Steuerelements verwendet. Und wie **SurfaceImageSource** ist diese Klasse nicht gut für hochleistungsfähige Spiele geeignet. Beispielhafte XAML-Elemente, welche die Klasse **VirtualSurfaceImageSource** verwenden können, sind Kartensteuerelemente oder Viewer für große Dokumente mit hoher Bilddichte.

-   Wenn Sie DirectX verwenden, um Grafiken zu präsentieren, die in Echtzeit oder in regelmäßigen Intervallen mit niedriger Verzögerung aktualisiert werden müssen, verwenden Sie die Klasse [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel). So können Sie Grafiken aktualisieren, ohne dass eine Synchronisierung mit dem Aktualisierungstimer des XAML-Frameworks erforderlich ist. Mit dieser Klasse können Sie direkt auf die Swapchain ([IDXGISwapChain1](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)) der Grafikhardware zugreifen und XAML oberhalb des Renderziels anordnen. Dieser Typ ist hervorragend für Spiele und DirectX-Apps mit Vollbildansicht geeignet, in denen eine XAML-basierte Benutzeroberfläche erforderlich ist. Wenn Sie diese Vorgehensweise wählen, sollten Sie sich mit DirectX sehr gut auskennen, z. B. in Bezug auf die Bereiche Microsoft DirectX Graphics Infrastructure (DXGI), Direct2D und Direct3D. Weitere Informationen finden Sie unter [Programmier Handbuch für Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews).

## <a name="surfaceimagesource"></a>SurfaceImageSource

[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) bietet gemeinsam genutzte Flächen, in die mit DirectX gezeichnet werden kann, und setzt die einzelnen Bestandteile dann zu App-Inhalten zusammen.

Hier ist der grundlegende Prozess zum Erstellen und Aktualisieren eines [surfakeimagesource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) -Objekts im Code Behind:

1.  Legen Sie die Größe der gemeinsam genutzten Fläche fest, indem Sie die Werte für die Höhe und Breite an den Konstruktor der Klasse [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) übergeben. Sie können ebenfalls festlegen, ob für die gemeinsam genutzte Fläche Alpha-Unterstützung (Deckkraft) erforderlich ist.

    Beispiel:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Einen Zeiger auf [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d)erhalten. Wandeln Sie das [surfakeimagesource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) -Objekt als [iinspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (oder **IUnknown**) um, und wenden Sie **QueryInterface** darauf an, um die zugrunde liegende **ISurfaceImageSourceNativeWithD2D** -Implementierung abzurufen. Die für diese Implementierung festgelegten Methoden verwenden Sie dann, um das entsprechende Gerät festzulegen und die Zeichenoperationen auszuführen.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  Erstellen Sie die DXGI-und D2D-Geräte, indem Sie zuerst [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) aufrufen und [D2D1CreateDevice](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-d2d1createdevice) dann das Gerät und den Kontext an [ISurfaceImageSourceNativeWithD2D:: setdevice](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-setdevice)übergeben. 

    > [!NOTE]
    > Wenn Sie von einem Hintergrund Thread aus zu " **surfakeimagesource** " ziehen, müssen Sie auch sicherstellen, dass das DXGI-Gerät den Multithreadzugriff aktiviert hat. Dies sollte nur erfolgen, wenn Sie aus Leistungsgründen aus einem Hintergrund Thread zeichnen.

    Beispiel:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  Geben Sie einen Zeiger auf ein [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) -Objekt auf [ISurfaceImageSourceNativeWithD2D:: beginDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-begindraw)an, und verwenden Sie den zurückgegebenen Zeichnungs Kontext, um den Inhalt des gewünschten Rechtecks innerhalb von **surfakeimagesource**zu zeichnen. **ISurfaceImageSourceNativeWithD2D:: beginDraw** und die Zeichnungs Befehle können von einem Hintergrund Thread aufgerufen werden. Es wird nur in dem Bereich gezeichnet, der im Parameter *updateRect* für Updates festgelegt wurde.

    Die Methode gibt den X-Y-Punkt-Offset für das Zielrechteck als *offset*-Parameter zurück. Mithilfe dieses Offsets legen Sie fest, wo der aktualisierte Inhalt mit **ID2D1DeviceContext**gezeichnet werden soll.

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. Aufrufen von [ISurfaceImageSourceNativeWithD2D:: EndDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-enddraw) zum Vervollständigen der Bitmap. Die Bitmap kann als Quelle für ein [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image)- oder [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)-Element (XAML) verwendet werden. **ISurfaceImageSourceNativeWithD2D:: EndDraw** muss nur aus dem UI-Thread aufgerufen werden.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > Durch den Aufruf von [surfaceimagesource:: SetSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) (geerbt von **ibitmapsource:: SetSource**) wird derzeit eine Ausnahme ausgelöst. Rufen Sie es nicht vom [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)-Objekt aus auf.

    > [!NOTE]
    > Anwendungen müssen das Zeichnen in **surfakeimagesource** vermeiden, wenn das zugehörige [Fenster](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) ausgeblendet ist. andernfalls schlagen **ISurfaceImageSourceNativeWithD2D** -APIs fehl. Um dies zu erreichen, registrieren Sie sich als Ereignislistener für das [Window. visibilitychanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) -Ereignis, um Sichtbarkeits Änderungen zu verfolgen.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) erweitert [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource), wenn die Inhalte potenziell zu groß sind, um auf dem Bildschirm angezeigt zu werden, und die Inhalte virtualisiert werden müssen, um ein optimales Rendering sicherzustellen.

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) unterscheidet sich von [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) durch die Verwendung der Callback-Methode [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded). Die Callback-Methode wird implementiert, um bestimmte Bereiche der Fläche zu aktualisieren, sobald sie auf dem Bildschirm angezeigt werden. Somit müssen Sie keine ausgeblendeten Bereiche löschen, da das XAML-Framework diese Aufgabe für Sie übernimmt.

Im folgenden finden Sie den grundlegenden Prozess zum Erstellen und Aktualisieren eines [virtualsurfakeimagesource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) -Objekts im Code Behind:

1.  Erstellen Sie eine Instanz von [virtualsurfakeimagesource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) mit der gewünschten Größe. Beispiel:

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  Sie erhalten Zeiger auf [ivirtualsurfakeimagesourcenative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative) und [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d). Wandeln Sie das [virtualsurfaceimagesource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) -Objekt als " [iinspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) " oder " [IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)" um, und nennen Sie [QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) , um die zugrunde liegenden **ivirtualsurfaceimagesourcenative** -und **ISurfaceImageSourceNativeWithD2D** -Implementierungen abzurufen. Verwenden Sie die Methoden, die für diese Implementierungen definiert sind, um das Gerät festzulegen und die zeichnen-Vorgänge auszuführen.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  Erstellen Sie die DXGI-und D2D-Geräte, indem Sie zuerst **D3D11CreateDevice** und **D2D1CreateDevice**aufrufen, und übergeben Sie dann das D2D-Gerät an **ISurfaceImageSourceNativeWithD2D:: setdevice**.

    > [!NOTE]
    > Wenn Sie von einem Hintergrund Thread aus in " **virtualsurfakeimagesource** " zeichnen, müssen Sie auch sicherstellen, dass das DXGI-Gerät den Multithreadzugriff aktiviert hat. Dies sollte nur erfolgen, wenn Sie aus Leistungsgründen aus einem Hintergrund Thread zeichnen.

    Beispiel:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Rufen Sie die Methode [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) auf, wodurch eine Referenz zu Ihrer Implementierung der Schnittstelle [IVirtualSurfaceUpdatesCallbackNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative) übergeben wird.

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }

    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    Daraufhin ruft das Framework Ihre Implementierung der Methode [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) auf, wenn ein Bereich der Klasse [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) aktualisiert werden muss.

    Dieser Fall kann eintreten, wenn das Framework den Bereich bestimmt, der gezeichnet werden muss (etwa wenn der Benutzer die Ansicht der Fläche verschiebt oder zoomt), oder wenn die App die Methode [IVirtualSurfaceImageSourceNative::Invalidate](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-invalidate) in diesem Bereich aufgerufen hat.

5.  Verwenden Sie innerhalb der Methode [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded) die Methoden [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterectcount) und [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterects), um zu bestimmen, welche Bereiche der Fläche gezeichnet werden müssen.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  Führen Sie schließlich für jeden Bereich, der aktualisiert werden soll, Folgendes aus:

    1.  Geben Sie einen Zeiger auf das **ID2D1DeviceContext** -Objekt auf **ISurfaceImageSourceNativeWithD2D:: beginDraw**an, und verwenden Sie den zurückgegebenen Zeichnungs Kontext, um den Inhalt des gewünschten Rechtecks innerhalb von **surfakeimagesource**zu zeichnen. **ISurfaceImageSourceNativeWithD2D:: beginDraw** und die Zeichnungs Befehle können von einem Hintergrund Thread aufgerufen werden. Es wird nur in dem Bereich gezeichnet, der im Parameter *updateRect* für Updates festgelegt wurde.

        Die Methode gibt den X-Y-Punkt-Offset für das Zielrechteck als *offset*-Parameter zurück. Mithilfe dieses Offsets legen Sie fest, wo der aktualisierte Inhalt mit **ID2D1DeviceContext**gezeichnet werden soll.

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  Zeichnen Sie Ihre spezifischen Inhalte in diesem Bereich. Achten Sie aber darauf, dass Sie das Zeichnen auf die begrenzten Bereiche beschränken, um die optimale Leistung sicherzustellen.

    3.  Aufrufen von **ISurfaceImageSourceNativeWithD2D:: EndDraw**. Als Ergebnis erhalten Sie eine Bitmap.

> [!NOTE]
> Anwendungen müssen das Zeichnen in **surfakeimagesource** vermeiden, wenn das zugehörige [Fenster](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) ausgeblendet ist. andernfalls schlagen **ISurfaceImageSourceNativeWithD2D** -APIs fehl. Um dies zu erreichen, registrieren Sie sich als Ereignislistener für das [Window. visibilitychanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) -Ereignis, um Sichtbarkeits Änderungen zu verfolgen.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel und Gaming


[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) ist der Windows-Runtime-Typ, der für die Unterstützung von High-End-Grafiken und -Spielen mit direkter Swapchain-Verwaltung entwickelt wurde. Sie erstellen in diesem Fall Ihre eigene DirectX-Swapchain und verwalten die Präsentation Ihrer gerenderten Inhalte selbst.

Es gibt gewisse Einschränkungen für die Klasse [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel), um die bestmögliche Leistungsfähigkeit sicherzustellen:

-   Es sind nicht mehr als vier [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)-Instanzen pro App vorhanden.
-   Sie sollten die Höhe und Breite der DirectX-Swap-Kette (in der [DXGI- \_ \_ SwapChain \_ DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) auf die aktuellen Dimensionen des SwapChain-Elements festlegen. Wenn Sie dies nicht tun, wird der Anzeige Inhalt skaliert (mit der ** \_ Stretch- \_ Streckung von DXGI**).
-   Sie müssen den Skalierungs Modus der DirectX-Swap-Kette (in der [DXGI- \_ \_ SwapChain \_ DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) auf die **DXGI- \_ Skalierungs \_ Streckung**festlegen.
-   Sie müssen die DirectX-Swapchain erstellen, indem Sie die Methode [IDXGIFactory2::CreateSwapChainForComposition](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition) aufrufen.

Die Klasse [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) wird basierend auf den Anforderungen Ihrer App aktualisiert und nicht entsprechend den Updates des XAML-Frameworks. Wenn Sie die Updates der Klasse **SwapChainPanel** mit denen des XAML-Frameworks synchronisieren möchten, registrieren Sie die Klasse für das Ereignis [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering). Andernfalls müssen Sie sämtliche threadübergreifenden Probleme berücksichtigen, wenn Sie versuchen, die XAML-Elemente über einen Thread zu aktualisieren, bei dem es sich nicht um den Thread handelt, der die Klasse **SwapChainPanel** aktualisiert.

Falls Sie in **SwapChainPanel** Zeigereingaben mit geringer Verzögerung empfangen müssen, verwenden Sie [SwapChainPanel::CreateCoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Das von dieser Methode zurückgegebene [CoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.core.coreindependentinputsource)-Objekt ermöglicht den Empfang von Eingabeereignissen mit minimaler Verzögerung in einem Hintergrundthread. Hinweis: Nach dem Aufrufen dieser Methode werden keine normalen XAML-Zeigereingabeereignisse für **SwapChainPanel** mehr ausgelöst, da alle Eingaben in den Hintergrundthread umgeleitet werden.


> **Hinweis**   Im Allgemeinen sollten Ihre DirectX-apps Austausch Ketten in quer Ausrichtung und die Anzeige Fenstergröße (in der Regel die systemeigene Bildschirmauflösung in den meisten Microsoft Store spielen) erstellen. Dadurch wird sichergestellt, dass Ihre App die optimale Swapchainimplementierung verwendet, wenn sie über keine sichtbaren XAML-Overlays verfügt. Wenn die App in das Hochformat gedreht wird, sollte sie [IDXGISwapChain1::SetRotation](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) in der vorhandenen Swapchain aufrufen, eine Umwandlung des Inhalts anwenden, wenn erforderlich, und anschließend erneut [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) auf der gleichen Swapchain aufrufen. Analog dazu sollte Ihre APP "" auch dann erneut für die gleiche **SwapChain** aufrufen, wenn die Größe der [Swapkette durch Aufrufen von idxgiswapchain:: resizebuffers](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)geändert wird.


 

Es folgt ein grundlegender Prozess zum Erstellen und Aktualisieren eines " [sexapchainpanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) "-Objekts im Code Behind:

1.  Rufen Sie eine Instanz eines Swapchainbereichs für Ihre App ab. Die Instanzen sind in XAML mit dem Tag `<SwapChainPanel>` gekennzeichnet.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    Unten finden Sie ein Beispiel für das Tag `<SwapChainPanel>`.

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  Rufen Sie einen Zeiger zur Schnittstelle [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) ab. Wandeln Sie das [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)-Objekt in [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (oder **IUnknown**) um, und rufen Sie **QueryInterface** für dieses auf, um die zugrundeliegende **ISwapChainPanelNative**-Implementierung abzurufen.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Erstellen Sie das DXGI-Gerät und die Swapkette, und legen Sie die [Swapkette auf iswapchainpanelnative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) fest, indem Sie Sie an [setwapchain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain)übergeben.

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  Zeichnen Sie in die DirectX-Swapchain, und präsentieren Sie sie, um die Inhalte anzuzeigen.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Die XAML-Elemente werden aktualisiert, wenn das Windows-Runtime-Layout/die Rendering-Logik ein Update signalisiert.

## <a name="related-topics"></a>Zugehörige Themen

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)
* [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)
* [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)
* [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)
* [Programmieranleitung für Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews)