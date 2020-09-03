---
description: In diesem Thema wird anhand eines vollständigen Direct2D-Codebeispiels verdeutlicht, wie Sie C++/WinRT für die Nutzung von COM-Klassen und -Schnittstellen verwenden.
title: Verwenden von COM-Komponenten mit C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, Standard, C++, cpp, Winrt, COM, Komponente, Klasse, Schnittstelle
ms.localizationpriority: medium
ms.openlocfilehash: 6ccd196b2cd571cc66523b34427ca17acd73388a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170344"
---
# <a name="consume-com-components-with-cwinrt"></a>Verwenden von COM-Komponenten mit C++/WinRT

Sie können die Möglichkeiten der [C++/WinRT](./intro-to-using-cpp-with-winrt.md)-Bibliothek verwenden, um COM-Komponenten zu nutzen, beispielsweise die sehr leistungsfähige 2D- und 3D-Grafik der DirectX-APIs. C++/WinRT ist die einfachste Möglichkeit, DirectX ohne Beeinträchtigung der Leistung zu verwenden. In diesem Thema wird anhand eines Direct2D-Codebeispiels verdeutlicht, wie Sie C++/WinRT für die Nutzung von COM-Klassen und -Schnittstellen verwenden. Sie können natürlich COM- und Windows-Runtimeprogrammierung gemischt innerhalb des gleichen C++/WinRT-Projekts verwenden.

Am Ende dieses Themas finden Sie eine vollständige Quellcodeauflistung für eine minimale Direct2D-Anwendung. Wir greifen Ausschnitte aus diesem Code heraus und verwenden sie, um das Nutzen von COM-Komponenten mithilfe von C++/WinRT unter Verwendung verschiedener Möglichkeiten der C++/WinRT-Bibliothek zu veranschaulichen.

## <a name="com-smart-pointers-winrtcom_ptr"></a>Intelligente COM-Zeiger ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Beim Programmieren mit COM arbeiten Sie direkt mit Schnittstellen statt mit Objekten (das trifft auch hinter den Kulissen auf Windows Runtime-APIs zu, die eine Weiterentwicklung von COM darstellen). Um beispielsweise eine Funktion einer COM-Klasse aufzurufen, aktivieren Sie die Klasse, erhalten eine Schnittstelle zurück und rufen dann Funktionen auf dieser Schnittstelle auf. Um auf den Status eines Objekts zuzugreifen, greifen Sie nicht direkt auf seine Datenmember zu; stattdessen rufen Sie Accessor- und Mutatorfunktionen auf einer Schnittstelle auf.

Genauer gesagt sprechen wir über die Interaktion mit *Schnittstellenzeigern*. Und bei diesem Vorhaben profitieren wir vom intelligenten COM-Zeigertyp in ++/WinRT &mdash; dem [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)-Typ.

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

Der Code oben zeigt, wie ein nicht initialisierter intelligenter Zeiger auf eine [**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1)-COM-Schnittstelle deklariert wird. Der intelligente Zeiger ist uninitialisiert, er verweist also noch nicht auf eine **ID2D1Factory1**-Schnittstelle, die zu irgendeinem realen Objekt gehört (er verweist auf gar keine Schnittstelle). Er hat aber das Potenzial dazu; und er besitzt (als intelligenter Zeiger) die Fähigkeit, mittels eines COM-Verweiszählers die Lebensdauer des Besitzerobjekts der Schnittstelle zu verwalten, auf die er verweist, und als Medium für den Aufruf von Funktionen auf dieser Schnittstelle zu dienen.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>COM-Funktionen, die einen Schnittstellenzeiger als **void** zurückgeben

Sie können die [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)-Funktion aufrufen, um in den zugrundeliegenden nackten Zeiger eines nicht initialisierten intelligenten Zeigers zu schreiben.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

Der Code oben ruft die [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory)-Funktion auf, die über ihren letzten Parameter einen **ID2D1Factory1**-Schnittstellenzeiger vom Typ **void\*\*** aufruft. Viele COM-Funktionen geben einen **void\*\*** -Typ zurück. Verwenden Sie für diese Funktionen [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function), wie dargestellt.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>COM-Funktionen, die einen bestimmten Schnittstellenzeiger zurückgeben

Die Funktion [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) gibt mithilfe ihres drittletzten Parameters einen [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)-Schnittstellenzeiger vom Typ **ID3D11Device\*\*** zurück. Verwenden Sie für Funktionen, die in dieser Art einen bestimmten Schnittstellenzeiger zurückgeben, [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

Das Codebeispiel in dem Abschnitt vor diesem zeigt das Aufrufen der nackten **D2D1CreateFactory**-Funktion. Aber wenn das Codebeispiel für dieses Thema **D2D1CreateFactory** aufruft, verwendet es dazu eine Hilfsfunktionsvorlage, die die nackte API umschließt, sodass das Codebeispiel tatsächlich [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) verwendet.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>COM-Funktionen, die einen Schnittstellenzeiger als **IUnknown** zurückgeben

Die Funktion [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) gibt über ihren letzten Parameter einen DirectWrite-Factoryschnittstellenzeiger vom Typ [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) zurück. Verwenden Sie für eine solche Funktion [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function), aber interpretieren Sie es durch Typwandlung als **IUnknown** um.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcom_ptr"></a>Neuzuweisen eines **winrt::com_ptr**

> [!IMPORTANT]
> Wenn Sie über einen [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) verfügen, der bereits zugewiesen ist (sein interner nackter Zeiger hat bereits ein Ziel), und Sie möchten ihn neu zuweisen, sodass er auf ein anderes Objekt verweist, müssen Sie ihm zuerst `nullptr` zuweisen &mdash; wie im Codebeispiel oben gezeigt. Wenn Sie das versäumen, bringt Ihnen ein bereits zugewiesener **com_ptr** das Problem zu Bewusstsein (wenn Sie [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) oder [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) aufrufen), indem er behauptet, dass sein interner Zeiger nicht NULL ist.

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>Verarbeiten von HRESULT-Fehlercodes

Um den Wert eines von einer COM-Funktion zurückgegebenen HRESULT zu überprüfen und für den Fall, dass es einen Fehlercode darstellt, eine Ausnahme auszulösen, rufen Sie [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult) auf.

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>COM-Funktionen, die einen bestimmten Schnittstellenzeiger annehmen

Sie können die [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function)-Funktion aufrufen, um Ihren **com_ptr** einer Funktion zu übergeben, die einen bestimmten Schnittstellenzeiger des gleichen Typs annimmt.

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>COM-Funktionen, die einen **IUnknown**-Schnittstellenzeiger annehmen

Sie können [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) verwenden, um Ihren **com_ptr** einer Funktion zu übergeben, die einen **IUnknown**-Schnittstellenzeiger annimmt.

Sie können die freie Funktion [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) verwenden, um die Adresse der zugrunde liegenden Rohdaten-[IUnknown-Schnittstelle](/windows/win32/api/unknwn/nn-unknwn-iunknown) eines Objekts eines projektierten Typs (mit anderen Worten, einen Zeiger darauf) zurückzugeben. Sie können diese Adresse dann an eine Funktion übergeben, die einen **IUnknown-** -Schnittstellenzeiger annimmt.

Informationen zu *projizierten Typen* finden Sie unter [Verwenden von APIs mit C++/WinRT](./consume-apis.md).

Ein Codebeispiel für **get_unknown** finden Sie unter [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) oder unter [Vollständige Quellcodeauflistung einer minimalen Direct2D-Anwendung](#full-source-code-listing-of-a-minimal-direct2d-application) in diesem Thema.

## <a name="passing-and-returning-com-smart-pointers"></a>Übergabe und Rückgabe von intelligenten COM-Zeigern

Eine Funktion, die einen intelligenten COM-Zeiger in der Form eines **winrt::com_ptr** annimmt, sollte dies als Konstantenverweis oder als Verweis tun.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Eine Funktion, die einen **winrt::com_ptr** zurückgibt, sollte dies als Wert tun.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Abfragen eines intelligenten COM-Zeigers für eine andere Schnittstelle

Sie können die Funktion [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) verwenden, um einen intelligenten COM-Zeiger für eine andere Schnittstelle abzufragen. Die Funktion löst eine Ausnahme aus, wenn die Abfrage nicht erfolgreich ist.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Verwenden Sie alternativ [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function), die einen Wert zurückgibt, den Sie anhand von `nullptr` überprüfen können, um zu sehen, ob die Abfrage erfolgreich war.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Vollständige Quellcodeauflistung einer minimalen Direct2D-Anwendung

> [!NOTE]
> Informationen zum Einrichten von Visual Studio für die C++/WinRT-Entwicklung&mdash;einschließlich Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen)&mdash; finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Wenn Sie dieses Quellcodebeispiel erstellen und ausführen möchten, dann installieren zuerst die neueste Version von C++/WinRT Visual Studio Extension (VSIX) (oder aktualisieren Sie auf diese Version); siehe Hinweis oben. Erstellen Sie dann in Visual Studio eine neue **Core App (C++/WinRT)** . `Direct2D` ist ein sinnvoller Name für das Projekt, Sie können aber auch jeden gewünschten anderen verwenden. Die neueste allgemein verfügbare Version von Windows SDK (d. h. keine Vorschauversion).

### <a name="step-1-edit-pchh"></a>Schritt 1 Bearbeiten von `pch.h`

Öffnen Sie `pch.h`, und fügen Sie `#include <unknwn.h>` unmittelbar nach dem Einschließen von `windows.h` hinzu. Dies liegt daran, dass wir [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) verwenden. Es empfiehlt sich, `#include <unknwn.h>` explizit immer aufzunehmen, wenn du **winrt::get_unknown** verwendest, auch wenn dieser Header bereits in einem anderen Header enthalten ist.

> [!NOTE]
> Wenn Sie diesen Schritt weglassen, wird der Buildfehler *"get_unknown": Bezeichner nicht gefunden* angezeigt.

### <a name="step-2-edit-appcpp"></a>Schritt 2 Bearbeiten von `App.cpp`

Öffnen Sie `App.cpp`, löschen Sie den gesamten Inhalt, und fügen Sie das unten aufgeführte Listing ein.

Der Code unten verwendet wo immer möglich die Funktion [winrt::com_ptr::capture function](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function). `WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Arbeiten mit COM-Typen wie BSTR und VARIANT

Wie Sie sehen können, bietet C++/WinRT sowohl für das Implementieren als auch für das Aufrufen von COM-Schnittstellen Unterstützung. Für die Verwendung von COM-Typen wie BSTR und VARIANT empfehlen wir die Verwendung von Wrappern, die von den [Windows Implementation Libraries (WIL)](https://github.com/Microsoft/wil) bereitgestellt werden, wie etwa **wil::unique_bstr** und **wil::unique_variant** (die die Lebensdauer von Ressourcen verwalten).

[WIL](https://github.com/Microsoft/wil) hat Vorrang vor Frameworks wie der Active Template Library (ATL) und der COM-Unterstützung des Visual C++-Compilers. Wir empfehlen die Verwendung dieser Wrapper anstelle von selbst erstellten oder der Verwendung von COM-Typen wie BSTR und VARIANT in ihrer nackten Form (in Kombination mit den passenden APIs).

## <a name="avoiding-namespace-collisions"></a>Vermeiden von Namespacekonflikten

Wie aus der Codeauflistung in diesem Thema zu ersehen ist, werden in C++/WinRT häufig using-Direktiven verwendet. In einigen Fällen kann dies jedoch dazu führen, dass in den globalen Namespace Namen importiert werden, die miteinander in Konflikt stehen. Hier ist ein Beispiel.

C++/WinRT enthält einen Typ mit dem Namen [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown), während in COM ein Typ mit dem Namen [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) definiert ist. Betrachten Sie folgenden Code in einem C++/WinRT-Projekt, das COM-Header verwendet.

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

Der nicht qualifizierte Name *IUnknown* verursacht einen Konflikt im globalen Namespace und führt deshalb zu dem Compilerfehler *Mehrdeutiges Symbol*. Um das Problem zu vermeiden, können Sie die C++/WinRT-Version des Namens wie folgt im **winrt**-Namespace isolieren.

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

Wenn Sie möchten, können Sie auch `using namespace winrt` verwenden. Sie müssen lediglich die globale Version von *IUnknown* wie folgt qualifizieren.

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

Selbstverständlich können Sie für jeden C++/ WinRT-Namespace auf diese Weise vorgehen.

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

Anschließend können Sie z. B. einfach mit **winrt::StorageFile** auf **winrt::Windows::Storage::StorageFile** verweisen.

## <a name="important-apis"></a>Wichtige APIs
* [winrt::check_hresult-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr-Strukturvorlage](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown-Struktur](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)