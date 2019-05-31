---
description: In diesem Thema wird anhand eines vollständigen Direct2D-Codebeispiels verdeutlicht, wie Sie C++/WinRT für die Nutzung von COM-Klassen und -Schnittstellen verwenden.
title: Verwenden von COM-Komponenten mit C++ / WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, Uwp, Standard, c++, Cpp, Winrt, COM, Komponente, Klasse, Schnittstelle
ms.localizationpriority: medium
ms.openlocfilehash: 2c36c7b896b4d08240f08e85570110b45e0a9f3c
ms.sourcegitcommit: ba24a6237355119ef3e36687417f390c8722bd67
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2019
ms.locfileid: "66421261"
---
# <a name="consume-com-components-with-cwinrt"></a>Verwenden von COM-Komponenten mit C++ / WinRT

Können Sie die Funktionen des die [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Bibliothek COM-Komponenten, z. B. die 2D- und 3D-Grafik Hochleistungsgrafik der DirectX-APIs nutzen. C++ / WinRT ist die einfachste Möglichkeit, die DirectX verwenden, ohne dass die Leistung beeinträchtigt. In diesem Thema verwendet ein Direct2D-Codebeispiel veranschaulichen, wie Sie mithilfe von C++ / WinRT auf COM-Klassen und Schnittstellen zu nutzen. Sie können natürlich kombinieren die Programmierung von COM und Windows-Runtime im gleichen C++ / WinRT-Projekt.

Am Ende dieses Themas finden Sie eine vollständige quellcodeauflistung einer minimalen Direct2D-Anwendung. Wir heben Sie die Ausschnitte aus Code und verwenden sie zum veranschaulichen des COM-Komponenten mithilfe C++ / WinRT mit verschiedenen Funktionen von C++ / WinRT-Bibliothek.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Intelligente Zeiger für COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Beim Programmieren mit COM, arbeiten Sie direkt mit Schnittstellen anstatt mit Objekten (das auch hinter den Kulissen für Windows-Runtime-APIs, die eine Weiterentwicklung von COM sind "true"). Um eine Funktion auf eine COM-Klasse aufrufen, z. B. Sie aktivieren die Klasse, rufen Sie eine Schnittstelle zurück, und klicken Sie dann Sie Funktionen auf dieser Schnittstelle aufrufen. Der Zustand eines Objekts für den Zugriff auf zugreifen nicht Sie Datenmembern direkt. Stattdessen rufen Sie Accessor und Mutator-Funktionen in einer Schnittstelle.

Um spezifischere, wir reden über die Interaktion mit der Schnittstelle *Zeiger*. Und, nutzen wir das Vorhandensein des intelligenten Zeigers COM-Typs in C++"/ WinRT"&mdash;der [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) Typ.

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

Der obige Code zeigt, wie zu einen nicht initialisierten intelligenten Zeiger deklariert eine [ **ID2D1Factory1** ](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1) COM-Schnittstelle. Der intelligente Zeiger wird nicht initialisiert werden, damit sie noch nicht auf zeigt eine **ID2D1Factory1** Schnittstelle, die auf ein beliebiges tatsächliche Objekt (es ist nicht auf eine Schnittstelle überhaupt) gehören. Es besitzt das Potenzial dazu jedoch. und sie können über COM-Verweis gezählt wird, um die Lebensdauer des besitzenden Objekts der Schnittstelle zu verwalten, die darauf verweist, und klicken Sie auf dem Datenträger werden mit dem Sie die Funktionen auf dieser Schnittstelle aufrufen hat (wird ein intelligenter Zeiger).

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>COM-Funktionen, die einen Schnittstellenzeiger als zurückgeben **"void"**

Rufen Sie die [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) Funktion zum Schreiben in einen nicht initialisierten intelligente Zeiger zugrunde liegenden unformatierten Zeiger ist.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

Code über Aufrufe der [ **D2D1CreateFactory** ](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) Funktion, die eine **ID2D1Factory1** über seinen letzten Parameter, der hat-Schnittstellenzeiger **"void" \* \***  Typ. Viele COM-Funktionen geben eine **"void"\*\*** . Verwenden Sie für solche Funktionen [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) wie gezeigt.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>COM-Funktionen, die einen bestimmten Schnittstellenzeiger zurückgeben

Die [ **D3D11CreateDevice** ](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) -Funktion zurückgegeben wird ein [ **ID3D11Device** ](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) Schnittstellenzeiger über die Drittanbieter-aus-Last-Parameter, der verfügt **ID3D11Device\* \***  Typ. Verwenden Sie für Funktionen, die einen bestimmten Schnittstellenzeiger, wie zurückgeben [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

Das Codebeispiel im Abschnitt, bevor diese zeigt, wie die unformatierte aufgerufen **D2D1CreateFactory** Funktion. In der Tat, wenn das Codebeispiel in diesem Thema ruft jedoch **D2D1CreateFactory**, eine Hilfsprogramm-Funktionsvorlage, die die unformatierte API dient als Wrapper verwendet und im Codebeispiel wird also tatsächlich verwendet [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>COM-Funktionen, die einen Schnittstellenzeiger als zurückgeben **IUnknown**

Die [ **DWriteCreateFactory** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) Funktion gibt einen Schnittstellenzeiger für DirectWrite-Factory zurück, über die letzten Parameter [ **IUnknown** ](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)Typ. Verwenden Sie bei einer solchen Funktion [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function), aber Neuinterpretation umgewandelt werden, um **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Erneut von Arbeitsplätzen ein **winrt::com_ptr**

> [!IMPORTANT]
> Wenn Sie haben eine [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) , die bereits angebracht ist (die interne rohzeiger hat bereits ein Ziel) es darauf an ein anderes Objekt erneut Arbeitsplätzen werden sollen, und Sie müssen zunächst zuweisen `nullptr` Damit&mdash;wie im folgenden Codebeispiel gezeigt. Falls nicht, klicken Sie dann ein bereits angeschlossene **Com_ptr** zeichnen Sie das Problem wird Ihre Aufmerksamkeit (beim Aufrufen [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) oder [ **Com_ptr:: Put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) durch die Assertion, dass die internen Zeiger nicht null ist.

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

So überprüfen der Wert des einem HRESULT von einer COM-Funktion zurückgegeben und löst eine Ausnahme aus, falls es sich um einen Fehlercode darstellt, rufen Sie [ **winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>COM-Funktionen, die einen bestimmten Schnittstellenzeiger akzeptieren

Rufen Sie die [ **com_ptr::get** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) Funktion übergeben Ihrer **Com_ptr** an eine Funktion, die einen bestimmte Schnittstellenzeiger desselben Typs verwendet.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Funktionen, COM eine **IUnknown** Schnittstellenzeiger

Rufen Sie die [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function) Funktion übergeben Freigeben Ihrer **Com_ptr** an eine Funktion, die akzeptiert eine **IUnknown** Schnittstelle Zeiger.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Weiter- und Rückgabe von COM, intelligente Zeiger

Funktion, die einen intelligenten COM-Zeiger in Form einer **winrt::com_ptr** tun dies durch einen konstanten Verweis oder als Verweis.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Eine Funktion, die gibt eine **winrt::com_ptr** sollte dies über Wert.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Einen intelligenten COM-Zeiger für eine andere Schnittstelle Abfragen

Sie können die [ **com_ptr::as** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) -Funktion einen intelligenten COM-Zeiger für eine andere Schnittstelle abgefragt. Die Funktion löst eine Ausnahme aus, wenn die Abfrage nicht erfolgreich ist.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Verwenden Sie alternativ [ **com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function), womit einen Wert, der mit dem Sie überprüfen können `nullptr` um festzustellen, ob die Abfrage erfolgreich war.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Vollständige Quelle Codelisting aus einer minimalen Direct2D-Anwendung

Wenn Sie erstellen und diese Quellcodebeispiel dann zunächst in Visual Studio ausführen möchten, erstellen Sie ein neues **Core-App (C++ / WinRT)** . `Direct2D` ist ein sinnvolle Namen für das Projekt, aber Sie können die sie beliebig benennen. Open `App.cpp`, löschen Sie den gesamten Inhalt und fügen Sie in der folgenden Liste.

Der folgende Code verwendet die [Winrt::com_ptr::capture Funktion](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function) möglichst.

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

Wie Sie sehen können, C++ / WinRT bietet Unterstützung für implementieren und Aufrufen von COM-Schnittstellen. Für die Verwendung von COM-Typen wie BSTR und Variant-Wert, empfehlen wir die Verwendung von Wrapper, mit der [Windows Implementierung Bibliotheken (worauf)](https://github.com/Microsoft/wil), z. B. **wil::unique_bstr** und **wil::unique_ Variante** (die Verwaltung von Ressourcen Lebensdauer).

[Worauf](https://github.com/Microsoft/wil) hat Vorrang vor Frameworks wie die Active Template Library (ATL) und das visuelle Element C++ COM-Unterstützung des Compilers. Und wir empfehlen es über das Schreiben eigener Wrapper oder COM-Typen wie BSTR und VARIANT in ihre Rohdaten (zusammen mit der entsprechenden APIs) verwenden.

## <a name="important-apis"></a>Wichtige APIs
* [WinRT::check_hresult-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Vorlage für WinRT::com_ptr-Struktur](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
