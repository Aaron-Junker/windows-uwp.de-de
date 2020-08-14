---
description: In diesem Thema werden zwei Hilfsfunktionen für die Konvertierung zwischen [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)- und [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)-Objekten gezeigt.
title: Interoperabilität zwischen C++/WinRT und C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, Interoperabilität, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: c786256efb5488fff65a8e2bdb4c5d2ca0fa181c
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88180795"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilität zwischen C++/WinRT und C++/CX

Bevor Sie dieses Thema lesen, benötigen Sie die Informationen im Thema [Umstellen von C++/CX auf C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx). In diesem Thema werden zwei Hauptstrategieoptionen für die Portierung Ihres [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)-Projekts nach [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) vorgestellt.

- Portieren des gesamten Projekts in einem Durchgang. Die einfachste Option für ein Projekt, das nicht zu groß ist. Bei einem Windows-Runtime-Komponentenprojekt ist diese Strategie Ihre einzige Option.
- Schrittweise Portierung des Projekts (Größe oder Komplexität Ihrer Codebasis kann dies erforderlich machen). Diese Strategie erfordert jedoch, dass Sie einem Portierungsprozess folgen, bei dem eine Zeit lang C++/CX- und C++/WinRT-Code nebeneinander im selben Projekt existieren. Für ein XAML-Projekt müssen Ihre XAML-Seitentypen zu jedem beliebigen Zeitpunkt *entweder* alle in C++/WinRT *oder* alle in C++/CX vorliegen.

Dieses Thema zur Interoperabilität ist relevant für die *zweite* Strategie&mdash;, und zwar für Fälle, in denen Sie Ihr Projekt schrittweise portieren müssen. In diesem Thema werden zwei Hilfsfunktionen gezeigt, die Sie für die Konvertierung eines C++/CX- in ein C++/WinRT-Objekt (und umgekehrt) innerhalb des gleichen Projekts verwenden können.

Diese Hilfsfunktionen sind sehr nützlich, wenn Sie Ihren Code schrittweise von C++ /CX nach C++/WinRT portieren. Sie könnten sich alternativ einfach dafür entscheiden, sowohl die C++/WinRT- als auch die C++/CX-Sprachprojektionen im selben Projekt zu verwenden, unabhängig davon, ob Sie portieren oder nicht, und diese Hilfsfunktionen für die Interoperabilität zwischen den beiden zu verwenden.

Nachdem Sie dieses Thema gelesen haben, finden Sie im erweiterten Thema [Asynchronität und Interoperabilität zwischen C++/WinRT und C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async) Informationen und Codebeispiele, die verdeutlichen, wie PPL-Aufgaben und Co-Routinen parallel im gleichen Projekt unterstützt werden (z. B. das Aufrufen von Co-Routinen aus Aufgabenketten).

## <a name="the-from_cx-and-to_cx-functions"></a>Funktionen **from_cx** und **from_cx**

Hier sehen Sie eine Quellcodeauflistung einer Headerdatei namens `interop_helpers.h`, die zwei Konvertierungshilfsfunktionen enthält. In den folgenden Abschnitten werden die Funktionen sowie die Erstellung und Verwendung der Headerdatei in Ihrem Projekt erläutert.

```cppwinrt
// interop_helpers.h
#pragma once

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
```

### <a name="the-from_cx-function"></a>Funktion **from_cx**

Die Hilfsfunktion **from_cx** konvertiert ein C++/CX-Objekt in ein äquivalentes C++/WinRT-Objekt. Die Funktion wandelt ein C++/CX-Objekt in den zugrundeliegenden [**IUnknown**](/windows/win32/api/unknwn/nn-unknwn-iunknown)-Schnittstellenzeiger um. Anschließend wird [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) für diesen Zeiger aufgerufen, um die Standardschnittstelle des C++/WinRT-Objekts abzufragen. **QueryInterface** ist das Äquivalent der Windows-Runtime-ABI (Application Binary Interface) für die C++/CX-Erweiterung `safe_cast`. Die Funktion [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) ruft schließlich die Adresse des zugrundeliegenden **IUnknown**-Schnittstellenzeigers eines C++/WinRT-Objekts ab, damit er auf einen anderen Wert festgelegt werden kann.

### <a name="the-to_cx-function"></a>Funktion **to_cx**

Die Hilfsfunktion **to_cx** konvertiert ein C++/WinRT-Objekt in ein äquivalentes C++/CX-Objekt. Die Funktion [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) ruft einen Zeiger auf die zugrundeliegende **IUnknown**-Schnittstelle eines C++/WinRT-Objekts ab. Die Funktion wandelt diesen Zeiger in ein C++/CX-Objekt um und verwendet anschließend die C++/CX-Erweiterung `safe_cast`, um den angeforderten C++/CX-Typ abzufragen.

### <a name="the-interop_helpersh-header-file"></a>Headerdatei `interop_helpers.h`

Um die beiden Hilfsfunktionen in Ihrem Projekt zu verwenden, führen Sie die folgenden Schritte aus.

- Fügen Sie ein neues **Header File (.h)** -Element zu Ihrem Projekt hinzu, und nennen Sie es `interop_helpers.h`.
- Ersetzen Sie den Inhalt von `interop_helpers.h` durch die oben stehende Codeauflistung.
- Fügen Sie diese include-Elemente zu `pch.h` hinzu.

```cppwinrt
// pch.h
...
#include <unknwn.h>
// Include C++/WinRT projected Windows API headers here.
...
#include <interop_helpers.h>
```

## <a name="taking-a-ccx-project-and-adding-cwinrt-support"></a>Erweitern eines bestehenden C++/CX-Projekts um C++/WinRT-Unterstützung

In diesem Abschnitt wird beschrieben, was zu tun ist, wenn Sie sich entschieden haben, Ihr bestehendes C++/CX-Projekt um C++/WinRT-Unterstützung zu erweitern und die Portierung dort vorzunehmen. Weitere Informationen finden Sie auch unter [Visual Studio-Unterstützung für C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Um C++/CX und C++/WinRT in einem C++/CX-Projekt zu mischen (einschließlich der Verwendung der Hilfsfunktionen **from_cx** and **to_cx** im Projekt) müssen Sie dem Projekt manuell C++/WinRT-Unterstützung hinzufügen.

Öffnen Sie zuerst Ihr C++/CX-Projekt in Visual Studio, und vergewissern Sie sich, dass die Projekteigenschaft **Allgemein** \> **Version der Zielplattform** auf 10.0.17134.0 (Windows 10, Version 1803) oder höher festgelegt ist.

### <a name="install-the-cwinrt-nuget-package"></a>Installieren des NuGet-Pakets „C++/WinRT“

Das NuGet-Paket [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) bietet Buildunterstützung für C++/WinRT (MSBuild-Eigenschaften und -Ziele). Um es zu installieren, klicken Sie auf die Menüoption **Projekt** \> **NuGet-Pakete verwalten...** . \> **Durchsuchen**, geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **Installieren**, um das Paket für das Projekt zu installieren.

> [!IMPORTANT]
> Durch Installieren des NuGet-Pakets „C++/WinRT“ wird die Unterstützung für C++/CX im Projekt deaktiviert. Wenn Sie die Portierung in einem Durchgang ausführen möchten, empfiehlt es sich, diese Unterstützung deaktiviert zu lassen, sodass die Buildmeldungen Ihnen helfen, all Ihre Abhängigkeiten von C++/CX zu finden (und zu portieren) und letztlich das ursprünglich reine C++/CX-Projekt in ein reines C++/WinRT-Projekt umzuwandeln. Im nächsten Abschnitt finden Sie aber Informationen darüber, wie Sie die Unterstützung wieder aktivieren.

### <a name="turn-ccx-support-back-on"></a>Erneutes Aktivieren der C++/CX-Unterstützung

Wenn Sie die Portierung in einem Durchgang ausführen, ist dies nicht erforderlich. Wenn Sie jedoch eine schrittweise Portierung vornehmen müssen, ist es an diesem Punkt erforderlich, die C++/CX-Unterstützung in Ihrem Projekt wieder zu aktivieren. Legen Sie in den Projekteigenschaften Folgendes fest: **C/C++** \> **Allgemein** \> **Windows-Runtime-Erweiterung verwenden** \> **Ja (/ZW)** .

Alternativ (oder bei einem XAML-Projekt zusätzlich) können Sie C++/CX-Unterstützung auf der Seite mit den C++/WinRT-Projekteigenschaften in Visual Studio hinzufügen. In den Projekteigenschaften wählen Sie **Allgemeine Eigenschaften** \> **C++/WinRT** \> **Projektsprache** \> **C++/CX** aus. Dadurch wird die folgende Eigenschaft zu Ihrer `.vcxproj`-Datei hinzugefügt.

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

> [!IMPORTANT]
> Wann immer Sie einen Build erstellen müssen, um den Inhalt einer **Midl-Datei (.idl)** in Stubdateien zu verarbeiten, müssen Sie **Projektsprache** wieder in **C++/WinRT** ändern. Nachdem der Build diese Stubdateien generiert hat, ändern Sie **Projektsprache** wieder in **C++ /CX**.

Eine Liste mit ähnlichen Anpassungsoptionen (um das Verhalten des Tools `cppwinrt.exe` zu optimieren) findest du in der [Infodatei](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) zum Microsoft.Windows.CppWinRT-NuGet-Paket.

### <a name="include-cwinrt-header-files"></a>Einfügen von C++/WinRT-Headerdateien

Sie sollten mindestens `winrt/base.h` wie unten gezeigt in Ihre vorkompilierte Headerdatei (normalerweise `pch.h`) einfügen.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
...
```

Mit ziemlicher Sicherheit werden Sie aber auch die Typen im Namespace **winrt::Windows::Foundation** benötigen. Und möglicherweise kennen Sie bereits weitere Namespaces, die Sie benötigen. Fügen Sie also die Windows-API-Header mit C++/WinRT-Projektion ein, die den Namespaces wie diesem entsprechen (Sie brauchen `winrt/base.h` jetzt nicht explizit einzufügen, da dies automatisch erfolgt).

```cppwinrt
// pch.h
...
#include <winrt/Windows.Foundation.h>
// Include any other C++/WinRT projected Windows API headers here.
...
```

Schauen Sie sich auch das Codebeispiel im folgenden Abschnitt an (*Erweitern eines bestehenden C++/WinRT-Projekts um C++/CX-Unterstützung*). Hier finden Sie eine Technik, die die Namespacealiase `namespace cx` und `namespace winrt` verwendet. Mit dieser Technik können Sie ansonsten mögliche Namespacekonflikte zwischen der C++/WinRT-Projektion und der C++/CX-Projektion verhindern.

### <a name="add-interop_helpersh-to-the-project"></a>Hinzufügen von `interop_helpers.h` zum Projekt

Sie können nun die Funktionen **from_cx** und **to_cx** zu Ihrem C++/CX-Projekt hinzufügen. Anweisungen dazu finden Sie im Abschnitt [Funktionen **from_cx** und **to_cx**](#the-from_cx-and-to_cx-functions) oben.

## <a name="taking-a-cwinrt-project-and-adding-ccx-support"></a>Erweitern eines bestehenden C++/WinRT-Projekts um C++/CX-Unterstützung

In diesem Abschnitt wird beschrieben, was zu tun ist, wenn Sie sich entschieden haben, ein neues C++/WinRT-Projekt zu erstellen und die Portierung dort vorzunehmen.

Um C++/WinRT und C++/CX in einem C++/WinRT-Projekt zu mischen (einschließlich der Verwendung der Hilfsfunktionen **from_cx** and **to_cx** im Projekt) müssen Sie dem Projekt manuell C++/CX-Unterstützung hinzufügen.

- Erstellen Sie ein neues C++/WinRT-Projekt in Visual Studio unter Verwendung einer der C++/WinRT-Projektvorlagen (siehe [Visual Studio-Unterstützung für C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).
- Aktivieren Sie die Projektunterstützung für C++/CX. Lege in den Projekteigenschaften Folgendes fest: **C/C++** \> **Allgemein** \> **Windows-Runtime-Erweiterung verwenden ** \> **Ja (/ZW)**.

### <a name="an-example-cwinrt-project-showing-the-two-helper-functions-in-use"></a>C++/WinRT-Beispielprojekt zur Veranschaulichung der Verwendung der beiden Hilfsfunktionen

In diesem Abschnitt können Sie ein C++/WinRT-Beispielprojekt erstellen, das die Verwendung von **from_cx** und **to_cx** zeigt. Es veranschaulicht auch die Verwendung von Namespacealiasen für die verschiedenen Codeinseln, um mit potenziellen Namespacekonflikten zwischen der C++/WinRT-Projektion und der C++/CX-Projektion umzugehen.

- Erstelle ein Projekt vom Typ **Visual C++** \> **Windows Universal** \> **Core-App (C++/WinRT)**.
- Lege in den Projekteigenschaften Folgendes fest: **C/C++** \> **Allgemein** \> **Windows-Runtime-Erweiterung verwenden ** \> **Ja (/ZW)**.
- Fügen Sie `interop_helpers.h` zum Projekt hinzu. Anweisungen dazu finden Sie im Abschnitt [Funktionen **from_cx** und **to_cx**](#the-from_cx-and-to_cx-functions) oben.
- Ersetzen Sie den Inhalt von `App.cpp` durch die folgende Codeauflistung, erstellen Sie das Projekt, und führen Sie es aus.

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
* [Schnittstelle „IUnknown“](/windows/win32/api/unknwn/nn-unknwn-iunknown)
* [Funktion „QueryInterface“](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Funktion „winrt::get_abi“](/uwp/cpp-ref-for-winrt/get-abi)
* [Funktion „winrt::put_abi“](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Zugehörige Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Umstellen von C++/CX auf C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [Asynchronität und Interoperabilität zwischen C++/WinRT und C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async)
