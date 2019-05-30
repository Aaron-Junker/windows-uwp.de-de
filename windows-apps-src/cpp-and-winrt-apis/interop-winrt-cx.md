---
description: In diesem Thema werden zwei Hilfsfunktionen gezeigt, die verwendet werden können, um zwischen C++ / CX- und C++ / WinRT-Objekten zu konvertieren.
title: Interoperabilität zwischen C++/WinRT und C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, portieren, migrieren, interoperabilität, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 5394443b4832864e5b46bfbf917c04f0af6d8a19
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360218"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilität zwischen C++/WinRT und C++/CX

Strategien für die schrittweise das Portieren von Code in Ihre [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) Projekt [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) finden Sie im [verschieben in C++ / WinRT in C++ / CX](move-to-winrt-from-cx.md).

In diesem Thema wird gezeigt, zwei Hilfsfunktionen, die Sie verwenden können, für die Konvertierung zwischen C++ / CX und C++ / WinRT-Objekte im selben Projekt. Können Sie Sie verwenden, um Interoperabilität zwischen Code, der die zwei sprachprojektionen verwendet werden soll, oder Sie können die Funktionen verwenden, wie Sie den Code in C++ Portieren c++ / CX verwenden, um C++ / WinRT.

## <a name="fromcx-and-tocx-functions"></a>Funktionen from_cx und to_cx
Die folgende Hilfsfunktion konvertiert ein C++/CX-Objekt in ein äquivalentes C++/WinRT-Objekt. Die Funktion bildet ein C++/CX-Objekt auf den zugrundeliegenden [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) Schnittstellenzeiger ab. Sie ruft dann [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) auf diesem Zeiger auf, um de Standardschnittstelle des C++/WinRT-Objekts abzurufen. **QueryInterface** ist das Windows-Runtime Application Binary Interface (ABI) Äquivalent zur safe_cast-Erweiterung von C++/CX. Und die Funktion [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) ermittelt die Adresse des zugrundeliegenden **IUnknown**-Schnittstellenzeiger eines C++/WinRT-Objekts, sodass er auf einen anderen Wert gesetzt werden kann.

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

Die folgende Hilfsfunktion konvertiert ein C++/WinRT-Objekt in ein äquivalentes C++/CX-Objekt. Die Funktion [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) liefert einen Zeiger auf die **IUnknown**-Schnittstelle eines C++/WinRT-Objekts. Die Funktion bildet diesen Zeiger auf ein C++/CX-Objekt ab, bevor sie die safe_cast-Erweiterung von C++/CX verwendet, um den gewünschten C++/CX-Typ abzufragen.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Zeigt die beiden Hilfsfunktionen verwendet Beispielprojekt

Reproduziert werden, auf einfache Weise das Szenario der schrittweise das Portieren von Code in einem C++ / CX-Projekt in C++ / WinRT, beginnen Sie, indem das Erstellen eines neuen Projekts in Visual Studio mithilfe eines C++ / WinRT-Projektvorlagen (finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT ](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).

Dieses Beispielprojekt auch wird veranschaulicht, wie Sie Namespacealiase für die verschiedenen Inseln von Code, um verarbeiten andernfalls potenziellen Namespace-Konflikt zwischen der C++ / WinRT Projektions- und C++ / CX-Projektion.

- Erstellen Sie eine **Visual C++** \> **Windows Universal** > **Core-App (C++ / WinRT)** Projekt.
- In den Projekteigenschaften **C/C++-** \> **allgemeine** \> **Windows-Runtime-Erweiterung nutzen** \> **Ja (/ Zw)** . Dies aktiviert die Unterstützung von Projekten für C++ / CX.
- Ersetzen Sie den Inhalt der `App.cpp` mit der codeauflistung unten.

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>Wichtige APIs
* [IUnknown-Schnittstelle](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [QueryInterface-Funktion](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [WinRT::get_abi-Funktion](/uwp/cpp-ref-for-winrt/get-abi)
* [WinRT::put_abi-Funktion](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md)
