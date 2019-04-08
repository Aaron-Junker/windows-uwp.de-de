---
title: Interoperabilität von DirectX und XAML
description: In Ihrem Spiel für die universelle Windows-Plattform (UWP) können Sie eine Kombination aus Extensible Application Markup Language (XAML) und Microsoft DirectX verwenden.
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Spiele, directx, xaml-interoperabilität
ms.localizationpriority: medium
ms.openlocfilehash: 34fb65ec53f6addccf8723b451d333d602c17908
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604705"
---
# <a name="directx-and-xaml-interop"></a>Interoperabilität von DirectX und XAML



In Spielen oder Apps für die universelle Windows-Plattform (UWP) können Sie eine Kombination aus Extensible Application Markup Language (XAML) und Microsoft DirectX verwenden. Dank der Kombination von XAML und DirectX können Sie flexible Frameworks für die Benutzeroberflächen erstellen, die mit den über DirectX gerenderten Inhalten kompatibel sind. Dies ist besonders für grafisch aufwendige Apps von Vorteil. In diesem Thema beschäftigen wir uns mit der Struktur einer UWP-App mit DirectX und gehen auf wichtige Typen ein, die beim Erstellen Ihrer UWP-App für die Zusammenarbeit mit DirectX erforderlich sind.

Wenn Ihre App hauptsächlich 2-D-Rendering verwendet, empfiehlt sich unter Umständen die Verwendung der [Win2D](https://github.com/microsoft/win2d) Windows-Runtime-Bibliothek. Diese Bibliothek wird von Microsoft verwaltet und basiert auf den Direct2D-Kerntechnologien. Sie trägt erheblich zur Vereinfachung des Verwendungsmusters für die Implementierung von 2D-Grafik bei und enthält hilfreiche Abstraktionen für einige der in diesem Dokument beschriebenen Techniken. Ausführlichere Informationen finden Sie auf der Projektseite. Bei diesem Dokument handelt es sich um einen Leitfaden für App-Entwickler, die sich *gegen* die Verwendung von Win2D entschieden haben.

> **Beachten Sie**  DirectX-APIs sind nicht als Windows-Runtime-Typen definiert, damit Sie in der Regel verwenden Sie Visual C++-komponentenerweiterungen (C++ / CX), XAML-UWP-Komponente entwickeln, die Interoperabilität mit DirectX. Sie können UWP-DirectX-Apps auch mit C# und XAML erstellen, indem Sie die DirectX-Aufrufe in eine separate Windows-Runtime-Metadatendatei einschließen.

 

## <a name="xaml-and-directx"></a>XAML und DirectX

DirectX bietet zwei leistungsstarke Bibliotheken für 2D-und 3D-Grafik: Direct2D und Direct3D von Microsoft. Obwohl XAML grundlegende 2D-Grundtypen und -Effekte unterstützt, erfordern zahlreiche Apps, wie Modellierungs-Apps und Spiele, eine komplexere Grafikunterstützung. In diesen Fällen können Sie Grafiken teilweise oder vollständig mit Direct2D und Direct3D rendern und XAML für alles andere verwenden.

Wenn Sie eine benutzerdefinierte Interoperabilität zwischen XAML und DirectX implementieren möchten, müssen Sie mit den beiden folgenden Konzepten vertraut sein:

-   Bei gemeinsam genutzten Flächen (Shared Surfaces) handelt es sich um Anzeigebereiche bestimmter Größe, die von XAML definiert werden. In diesen Bereichen können Sie indirekt mit DirectX und [Windows::UI::Xaml::Media::ImageSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagesource.aspx)-Typen zeichnen. Bei gemeinsam genutzten Flächen steuern Sie nicht, wann genau neuer Inhalt auf dem Bildschirm angezeigt wird. Stattdessen werden Änderungen an den gemeinsam genutzten Flächen mit den Updates des XAML-Frameworks synchronisiert.
-   [Swapchains](https://msdn.microsoft.com/library/windows/desktop/bb206356(v=vs.85).aspx) stellen eine Sammlung von Puffern dar, die verwendet werden, um Grafiken mit minimaler Verzögerung anzuzeigen. Swapchains werden üblicherweise mit 60 Frames pro Sekunde und separat vom UI-Thread aktualisiert. Im Gegensatz zu CPU-Ressourcen haben Swapchains allerdings einen höheren Arbeitsspeicherbedarf, um schnelle Aktualisierungen zu unterstützen, und ihre Verwendung ist komplizierter, da mehrere Threads verwaltet werden müssen.

Überlegen Sie sich, wofür Sie DirectX verwenden. Wird es für die Zusammenstellung und Animierung eines einzelnen Steuerelements verwendet, das in die Abmessungen des Anzeigefensters passt? Wird eine Ausgabe enthalten sein, die wie in einem Spiel in Echtzeit gerendert und gesteuert werden muss? In diesen Fällen empfiehlt sich wahrscheinlich die Implementierung einer Swapchain. Andernfalls können Sie normalerweise auch eine gemeinsam genutzte Fläche verwenden.

Nachdem Sie sich überlegt haben, wie DirectX verwendet werden soll, können Sie einen den folgenden Windows-Runtime-Typen verwenden, um DirectX-Rendering in eine UWP-App zu integrieren:

-   Wenn Sie ein statisches Bild erstellen oder ein komplexes Bild in ereignisgesteuerten Intervallen zeichnen möchten, zeichnen Sie mit [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) in eine gemeinsam genutzte Fläche. Dieser Typ ermöglicht die Behandlung einer DirectX-Zeichenoberfläche von bestimmter Größe. In der Regel wird dieser Typ beim Erstellen eines Bilds oder einer Textur als Bitmap verwendet, um in einem Dokument oder als UI-Element angezeigt zu werden. Der Typ ist in Szenarien mit Echtzeitinteraktivität wie hochleistungsfähige Spiele nicht gut geeignet. Dies liegt daran, dass Updates des **SurfaceImageSource**-Objekts mit Updates der XAML-Benutzeroberfläche synchronisiert werden. Somit kann es zu Verzögerungen beim visuellen Feedback kommen, z. B. in Form einer schwankenden Bildrate oder einer wahrgenommenen verzögerten Reaktion auf Echtzeiteingaben. Die Aktualisierungen sind jedoch noch schnell genug für dynamische Steuerelemente oder Datensimulationen.

-   Wenn die Größe des Bilds den verfügbaren Platz auf dem Bildschirm übersteigt und es vom Benutzer verschoben und gezoomt werden kann, verwenden Sie [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050). Dieser Typ ermöglicht die Behandlung einer DirectX-Zeichenoberfläche, die größer als der Bildschirm ist. Wie [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) wird dies für die dynamische Erstellung eines komplexen Bilds oder Steuerelements verwendet. Und wie **SurfaceImageSource** ist diese Klasse nicht gut für hochleistungsfähige Spiele geeignet. Beispielhafte XAML-Elemente, welche die Klasse **VirtualSurfaceImageSource** verwenden können, sind Kartensteuerelemente oder Viewer für große Dokumente mit hoher Bilddichte.

-   Wenn Sie DirectX verwenden, um Grafiken zu präsentieren, die in Echtzeit oder in regelmäßigen Intervallen mit niedriger Verzögerung aktualisiert werden müssen, verwenden Sie die Klasse [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834). So können Sie Grafiken aktualisieren, ohne dass eine Synchronisierung mit dem Aktualisierungstimer des XAML-Frameworks erforderlich ist. Mit diesem Typ können Sie direkt auf die Swapchain ([IDXGISwapChain1](https://msdn.microsoft.com/library/windows/desktop/hh404631)) der Grafikhardware zugreifen und XAML oberhalb des Renderziels anordnen. Dieser Typ ist hervorragend für Spiele und DirectX-Apps mit Vollbildansicht geeignet, in denen eine XAML-basierte Benutzeroberfläche erforderlich ist. Wenn Sie diese Vorgehensweise wählen, sollten Sie sich mit DirectX sehr gut auskennen, z. B. in Bezug auf die Bereiche Microsoft DirectX Graphics Infrastructure (DXGI), Direct2D und Direct3D. Weitere Informationen finden Sie unter [Programmieranleitung für Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## <a name="surfaceimagesource"></a>SurfaceImageSource


[SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) bietet gemeinsam genutzte Flächen, in die mit DirectX gezeichnet werden kann, und setzt die einzelnen Bestandteile dann zu App-Inhalten zusammen.

Im Folgenden erfahren Sie mehr über die grundlegende Vorgehensweise zum Erstellen und Aktualisieren eines [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)-Objekts im CodeBehind.

1.  Legen Sie die Größe der gemeinsam genutzten Fläche fest, indem Sie die Werte für die Höhe und Breite an den Konstruktor [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) übergeben. Sie können ebenfalls festlegen, ob für die gemeinsam genutzte Fläche Alpha-Unterstützung (Deckkraft) erforderlich ist.

    Zum Beispiel:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Rufen Sie einen Zeiger zu [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137) ab. Wandeln Sie das [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)-Objekt als [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (oder **IUnknown**) um, und rufen Sie **QueryInterface** für dieses auf, um die zugrunde liegende **ISurfaceImageSourceNativeWithD2D**-Implementierung abzurufen. Die für diese Implementierung festgelegten Methoden verwenden Sie dann, um das entsprechende Gerät festzulegen und die Zeichenoperationen auszuführen.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  Erstellen Sie das DXGI- und das D2D-Gerät, indem Sie zunächst [D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082) und [D2D1CreateDevice](https://msdn.microsoft.com/library/windows/desktop/hh404272(v=vs.85).aspx) aufrufen und das Gerät sowie den Kontext dann an [ISurfaceImageSourceNativeWithD2D::SetDevice](https://msdn.microsoft.com/library/dn302141.aspx) übergeben. 

    > [!NOTE]
    > Wenn Sie über einen Hintergrundthread in Ihr **SurfaceImageSource** zeichnen, müssen Sie außerdem sicherstellen, dass der Multithread-Zugriff auf dem DXGI-Gerät aktiviert ist. Dies sollte aus Leistungsgründen beim Zeichnen über einen Hintergrundthread erfolgen.

    Zum Beispiel:

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

4.  Stellen Sie einen Zeiger für das [ID2D1DeviceContext](https://msdn.microsoft.com/library/windows/desktop/bb174565)-Objekt zur Methode [ISurfaceImageSourceNativeWithD2D::BeginDraw](https://msdn.microsoft.com/library/dn302138.aspx) bereit, und verwenden Sie den zurückgegebenen Kontext für das Zeichnen, um die Inhalte des gewünschten Rechtecks innerhalb von **SurfaceImageSource** zu zeichnen. **ISurfaceImageSourceNativeWithD2D::BeginDraw** und die Zeichenbefehle können über einen Hintergrundthread aufgerufen werden. Es wird nur in dem Bereich gezeichnet, der im Parameter *updateRect* für Updates festgelegt wurde.

    Die Methode gibt den X-Y-Punkt-Offset für das Zielrechteck als *offset*-Parameter zurück. Mit diesem Offset können Sie festlegen, wo Ihr aktualisierter Inhalt mit dem **ID2D1DeviceContext** gezeichnet wird.

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

5. Rufen Sie [ISurfaceImageSourceNativeWithD2D::EndDraw](https://msdn.microsoft.com/library/dn302139.aspx) auf, um die Bitmap fertigzustellen. Die Bitmap kann als Quelle für ein XAML [Image](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) oder [ImageBrush](https://msdn.microsoft.com/library/windows/apps/br210101) verwendet werden. **ISurfaceImageSourceNativeWithD2D::EndDraw** darf nur über den UI-Thread aufgerufen werden.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > Das Aufrufen von [SurfaceImageSource::SetSource](https://msdn.microsoft.com/library/windows/apps/br243255) (übernommen von **IBitmapSource::SetSource**) löst zurzeit eine Ausnahme aus. Rufen Sie es nicht vom [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)-Objekt aus auf.

    > [!NOTE]
    > Anwendungen dürfen nicht in **SurfaceImageSource** zeichnen, wenn das ihnen zugeordnete [Fenster](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) ausgeblendet ist. Andernfalls schlagen die **ISurfaceImageSourceNativeWithD2D**-APIs fehl. Registrieren Sie sich dazu als ein Ereignislistener für das [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) Ereignis, um Änderungen an der Sichtbarkeit nachverfolgen zu können.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) erweitert [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041), wenn die Inhalte potenziell zu groß sind, um auf dem Bildschirm angezeigt zu werden, und die Inhalte virtualisiert werden müssen, um ein optimales Rendering zu gewährleisten.

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) unterscheidet sich von [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) durch die Verwendung der Callback-Methode [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337). Die Callback-Methode wird implementiert, um bestimmte Bereiche der Fläche zu aktualisieren, sobald sie auf dem Bildschirm angezeigt werden. Somit müssen Sie keine ausgeblendeten Bereiche löschen, da das XAML-Framework diese Aufgabe für Sie übernimmt.

Im Folgenden erfahren Sie mehr über die grundlegende Vorgehensweise zum Erstellen und Aktualisieren eines [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)-Objekts im CodeBehind:

1.  Erstellen Sie eine Instanz von [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) in der gewünschten Größe. Zum Beispiel:

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  Rufen Sie Zeiger zu [IVirtualSurfaceImageSourceNative](https://msdn.microsoft.com/library/windows/desktop/hh848328) und [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137) ab. Wandeln Sie das [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)-Objekt als [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) oder [IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509) um, und rufen Sie [QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521) für dieses auf, um die zugrunde liegenden **IVirtualSurfaceImageSourceNative**- und **ISurfaceImageSourceNativeWithD2D**-Implementierungen abzurufen. Die für diese Implementierungen festgelegten Methoden verwenden Sie dann, um das entsprechende Gerät festzulegen und die Zeichenoperationen auszuführen.

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

3.  Erstellen Sie das DXGI- und das D2D-Gerät, indem Sie zunächst **D3D11CreateDevice** und **D2D1CreateDevice** aufrufen und das D2D-Gerät dann an **ISurfaceImageSourceNativeWithD2D::SetDevice** übergeben.

    > [!NOTE]
    > Wenn Sie über einen Hintergrundthread in Ihr **VirtualSurfaceImageSource** zeichnen, müssen Sie außerdem sicherstellen, dass der Multithread-Zugriff auf dem DXGI-Gerät aktiviert ist. Dies sollte aus Leistungsgründen beim Zeichnen über einen Hintergrundthread erfolgen.

    Zum Beispiel:

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

4.  Rufen Sie [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) auf, wodurch eine Referenz zu Ihrer Implementierung von [IVirtualSurfaceUpdatesCallbackNative](https://msdn.microsoft.com/library/windows/desktop/hh848336) übergeben wird.

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

    Das Framework ruft Ihre Implementierung von [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) auf, wenn ein Bereich von [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) aktualisiert werden muss.

    Dieser Fall kann eintreten, wenn das Framework den Bereich bestimmt, der gezeichnet werden muss (etwa wenn der Benutzer die Ansicht der Fläche verschiebt oder zoomt), oder wenn die App die Methode [IVirtualSurfaceImageSourceNative::Invalidate](https://msdn.microsoft.com/library/windows/desktop/hh848332) in diesem Bereich aufgerufen hat.

5.  Verwenden Sie innerhalb der Methode [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337) die Methoden [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://msdn.microsoft.com/library/windows/desktop/hh848329) und [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://msdn.microsoft.com/library/windows/desktop/hh848330), um zu bestimmen, welche Bereiche der Fläche gezeichnet werden müssen.

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

    1.  Stellen Sie einen Zeiger für das **ID2D1DeviceContext**-Objekt zur Methode **ISurfaceImageSourceNativeWithD2D::BeginDraw** bereit, und verwenden Sie den zurückgegebenen Kontext für das Zeichnen, um die Inhalte des gewünschten Rechtecks innerhalb von **SurfaceImageSource** zu zeichnen. **ISurfaceImageSourceNativeWithD2D::BeginDraw** und die Zeichenbefehle können über einen Hintergrundthread aufgerufen werden. Es wird nur in dem Bereich gezeichnet, der im Parameter *updateRect* für Updates festgelegt wurde.

        Die Methode gibt den X-Y-Punkt-Offset für das Zielrechteck als *offset*-Parameter zurück. Mit diesem Offset können Sie festlegen, wo Ihr aktualisierter Inhalt mit dem **ID2D1DeviceContext** gezeichnet wird.

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

    3.  Rufen Sie **ISurfaceImageSourceNativeWithD2D::EndDraw** auf. Als Ergebnis erhalten Sie eine Bitmap.

> [!NOTE]
> Anwendungen dürfen nicht in **SurfaceImageSource** zeichnen, wenn das ihnen zugeordnete [Fenster](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) ausgeblendet ist. Andernfalls schlagen die **ISurfaceImageSourceNativeWithD2D**-APIs fehl. Registrieren Sie sich dazu als ein Ereignislistener für das [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) Ereignis, um Änderungen an der Sichtbarkeit nachverfolgen zu können.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel und Gaming


[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) ist der Windows-Runtime-Typ, der für die Unterstützung von High-End-Grafik und -Spielen mit direkter Swapchain-Verwaltung entwickelt wurde. Sie erstellen in diesem Fall Ihre eigene DirectX-Swapchain und verwalten die Präsentation Ihrer gerenderten Inhalte selbst.

Es gibt gewisse Einschränkungen für den [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)-Typ, um die bestmögliche Leistungsfähigkeit sicherzustellen:

-   Es sind nicht mehr als vier [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)-Instanzen pro App vorhanden.
-   Sie sollten festlegen, die DirectX-Swapkette Höhe und Breite (in [DXGI\_SWAP\_Kette\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) auf die aktuellen Maße des Elements der Swap-Kette. Falls nicht, wird der Anzeigeinhalt skaliert werden (mit **DXGI\_Skalierung\_STRETCH**) anpassen.
-   Sie müssen festlegen, dass Skalierungsmodus der DirectX-Swapkette (in [DXGI\_SWAP\_Kette\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) zu **DXGI\_Skalierung\_STRETCH**.
-   Der DirectX-Swapkette Alphamodus kann nicht festgelegt werden (in [DXGI\_SWAP\_Kette\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) zu **DXGI\_ALPHA\_Modus\_ INTEGRIERTE**.
-   Sie müssen die DirectX-Swapchain erstellen, indem Sie [IDXGIFactory2::CreateSwapChainForComposition](https://msdn.microsoft.com/library/windows/desktop/hh404558) aufrufen.

[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) wird basierend auf den Anforderungen Ihrer App aktualisiert und nicht entsprechend den Updates des XAML-Frameworks. Wenn Sie die Updates von **SwapChainPanel** mit denen des XAML-Frameworks synchronisieren möchten, registrieren Sie dieses für das Ereignis [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://msdn.microsoft.com/library/windows/apps/br228127). Andernfalls müssen Sie sämtliche threadübergreifenden Probleme berücksichtigen, wenn Sie versuchen, die XAML-Elemente über einen Thread zu aktualisieren, bei dem es sich nicht um den Thread handelt, der die Klasse **SwapChainPanel** aktualisiert.

Falls Sie in **SwapChainPanel** Zeigereingaben mit geringer Verzögerung empfangen müssen, verwenden Sie [SwapChainPanel::CreateCoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Das von dieser Methode zurückgegebene [CoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coreindependentinputsource)-Objekt ermöglicht den Empfang von Eingabeereignissen mit minimaler Verzögerung in einem Hintergrundthread. Hinweis: Nach dem Aufrufen dieser Methode werden keine normalen XAML-Zeigereingabeereignisse für **SwapChainPanel** mehr ausgelöst, da alle Eingaben in den Hintergrundthread umgeleitet werden.


> **Hinweis:**   Im Allgemeinen sollten Ihre DirectX-Apps Swapchains im Querformat und entsprechend der angezeigten Fenstergröße erstellen (in den meisten Microsoft Store-Spielen für gewöhnlich die native Bildschirmauflösung). Dadurch wird sichergestellt, dass Ihre App die optimale Swapchainimplementierung verwendet, wenn sie über keine sichtbaren XAML-Overlays verfügt. Wenn die App in das Hochformat gedreht wird, sollte sie [IDXGISwapChain1::SetRotation](https://msdn.microsoft.com/library/windows/desktop/hh446801) in der vorhandenen Swapchain aufrufen, eine Umwandlung des Inhalts anwenden, wenn erforderlich, und anschließend erneut [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144) auf der gleichen Swapchain aufrufen. Darüber hinaus sollte Ihre App jedes Mal erneut **SetSwapChain** auf der gleichen Swapchain aufrufen, wenn die Größe der Swapchain geändert wird, indem [IDXGISwapChain::ResizeBuffers](https://msdn.microsoft.com/library/windows/desktop/bb174577) aufgerufen wird.


 

Im Folgenden erfahren Sie mehr über die grundlegende Vorgehensweise zum Erstellen und Aktualisieren eines [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)-Objekts im CodeBehind:

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

2.  Rufen Sie einen Zeiger zur Schnittstelle [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143) ab. Wandeln Sie das [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)-Objekt als [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (oder **IUnknown**) um, und rufen Sie **QueryInterface** für dieses auf, um die zugrunde liegende **ISwapChainPanelNative**-Implementierung abzurufen.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Erstellen Sie das DXGI-Gerät und die Swapchain, und legen Sie für die Swapchain [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143) fest, indem Sie diese an [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144) übergeben.

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

## <a name="related-topics"></a>Verwandte Themen

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* ["Surfaceimagesource"](https://msdn.microsoft.com/library/windows/apps/hh702041)
* ["Virtualsurfaceimagesource"](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Programmierhandbuch für Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 




