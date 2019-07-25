---
description: In diesem Thema werden zwei Hilfsfunktionen für die Konvertierung zwischen C++/CX- und C++/WinRT-Objekten gezeigt.
title: Interoperabilität zwischen C++/WinRT und C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, Interoperabilität, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: a6b57627cbf9021732a8a66818250ffc1fca915f
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660125"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilität zwischen C++/WinRT und C++/CX

Strategien für das schrittweise Portieren von Code aus deinem [ C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)-Projekt in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) findest du unter [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md).

In diesem Thema werden zwei Hilfsfunktionen gezeigt, die du für Konvertierungen zwischen C++/CX- und C++/WinRT-Objekten innerhalb des gleichen Projekts verwenden kannst. Sie ermöglichen die Interoperabilität zwischen Code mit den beiden Sprachprojektionen. Du kannst sie aber auch verwenden, um deinen Code von C++/CX in C++/WinRT zu portieren.

## <a name="fromcx-and-tocx-functions"></a>Funktionen „from_cx“ und „to_cx“
Die folgende Hilfsfunktion konvertiert ein C++/CX-Objekt in ein äquivalentes C++/WinRT-Objekt. Die Funktion wandelt ein C++/CX-Objekt in den zugrundeliegenden [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)-Schnittstellenzeiger um. Anschließend wird [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) für diesen Zeiger aufgerufen, um die Standardschnittstelle des C++/WinRT-Objekts abzufragen. **QueryInterface** ist das Äquivalent der Windows-Runtime-ABI (Application Binary Interface) für die C++/CX-Erweiterung „safe_cast“. Die Funktion [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) ruft schließlich die Adresse des zugrundeliegenden **IUnknown**-Schnittstellenzeigers eines C++/WinRT-Objekts ab, damit er auf einen anderen Wert festgelegt werden kann.

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

Die folgende Hilfsfunktion konvertiert ein C++/WinRT-Objekt in ein äquivalentes C++/CX-Objekt. Die Funktion [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) ruft einen Zeiger auf die zugrundeliegende **IUnknown**-Schnittstelle eines C++/WinRT-Objekts ab. Die Funktion wandelt diesen Zeiger in ein C++/CX-Objekt um und verwendet anschließend die C++/CX-Erweiterung „safe_cast“, um den angeforderten C++/CX-Typ abzufragen.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Beispielprojekt zur Veranschaulichung der Verwendung der beiden Hilfsfunktionen

Das Szenario einer schrittweisen Portierung von Code aus einem C++/CX-Projekt in C++/WinRT lässt sich ganz einfach reproduzieren. Erstelle dazu in Visual Studio ein neues Projekt mit einer der C++/WinRT-Projektvorlagen. (Weitere Informationen findest du unter [Visual Studio support for C++/WinRT, XAML, the VSIX extension, and the NuGet package](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) (Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket).)

Dieses Beispielprojekt veranschaulicht auch die Verwendung von Namespacealiasen für die verschiedenen Codeinseln, um mit potenziellen Namespacekonflikten zwischen der C++/WinRT-Projektion und der C++/CX-Projektion umzugehen.

- Erstelle ein Projekt vom Typ **Visual C++** \> **Windows Universal** > **Core-App (C++/WinRT)** .
- Lege in den Projekteigenschaften Folgendes fest: **C/C++** \> **Allgemein** \> **Windows-Runtime-Erweiterung verwenden**  \> **Ja (/ZW)** . Dadurch wird die Projektunterstützung für C++/CX aktiviert.
- Ersetze den Inhalt von `App.cpp` durch das folgende Codelisting:

`WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

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
* [Schnittstelle „IUnknown“](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [Funktion „QueryInterface“](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Funktion „winrt::get_abi“](/uwp/cpp-ref-for-winrt/get-abi)
* [Funktion „winrt::put_abi“](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md)
