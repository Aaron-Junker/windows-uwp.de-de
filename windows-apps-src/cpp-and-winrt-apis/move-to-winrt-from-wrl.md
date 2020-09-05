---
description: In diesem Thema wird gezeigt, wie Sie WRL-Code zum entsprechenden Äquivalent in C++/WinRT portieren.
title: Umstellen von WRL auf C++/WinRT
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, WRL
ms.localizationpriority: medium
ms.openlocfilehash: f7e7000a70a71f8ad90fd38c74d6e29882b4cbaa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157256"
---
# <a name="move-to-cwinrt-from-wrl"></a>Umstellen von WRL auf C++/WinRT
In diesem Thema wird gezeigt, wie Sie Code der [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) zum entsprechenden Äquivalent in [C++/WinRT](./intro-to-using-cpp-with-winrt.md) portieren.

Der erste Schritt beim Portieren zu C++/WinRT besteht darin, C++/WinRT-Unterstützung manuell Ihrem Projekt hinzuzufügen (siehe [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Installieren Sie zu diesem Zweck das [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) in Ihrem Projekt. Öffne das Projekt in Visual Studio, klicke auf **Projekt** \> **NuGet-Pakete verwalten**. \> **Durchsuchen**, geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **Installieren**, um das Paket für das Projekt zu installieren. Eine Auswirkung dieser Änderung ist, dass die Unterstützung für [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) im Projekt deaktiviert wird. Wenn Sie C++/CX im Projekt verwenden, können Sie die Unterstützung deaktiviert lassen und Ihren C++/CX-Code in C++/WinRT aktualisieren (siehe [Wechsel zu C++/WinRT von C++/CX](move-to-winrt-from-cx.md)). Sie können die Unterstützung auch wieder aktivieren (in den Projekteigenschaften, **C/C++** \> **Allgemein** \> **Windows-Runtime-Erweiterung verwenden** \> **Ja (/ZW)** ), und sich zunächst auf das Portieren des WRL-Codes konzentrieren. C++/CX- und C++/WinRT-Code kann in demselben Projekt gleichzeitig vorhanden sein, mit Ausnahme von XAML-Compilerunterstützung und der Windows-Runtime-Komponenten (siehe [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md)).

Definieren Sie die Projekteigenschaft **Allgemein** \> **Zielplattformversion** mit 10.0.17134.0 (Windows 10, Version 1803) oder höher.

Fügen Sie Ihrer vorkompilierten Headerdatei (in der Regel `pch.h`) `winrt/base.h` hinzu.

```cppwinrt
#include <winrt/base.h>
```

Wenn Sie Windows-API-Header mit C++/WinRT-Projektion hinzufügen (z. B. `winrt/Windows.Foundation.h`), müssen Sie `winrt/base.h` nicht so explizit wie hier einfügen, da dies automatisch erfolgt.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptr"></a>Portieren von intelligenten WRL-COM-Zeigern ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
Portieren Sie jeglichen Code, der **Microsoft::WRL::ComPtr\<T\>** verwendet, um [**winrt::com_ptr\<T\>** ](/uwp/cpp-ref-for-winrt/com-ptr) zu verwenden. Im Folgenden finden Sie ein Codebeispiel für „Vorher“ und „Nachher“: In der *Nachher*-Version ruft die [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function)-Mitgliedsfunktion den zugrunde liegenden Rohzeiger ab, sodass dieser definiert werden kann.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> Wenn Sie über einen [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) verfügen, der bereits zugewiesen ist (sein interner Rohzeiger hat bereits ein Ziel), und Sie möchten ihn neu zuweisen, sodass er auf ein anderes Objekt verweist, müssen Sie ihm zuerst `nullptr` zuweisen&mdash; wie im Codebeispiel oben gezeigt. Wenn Sie das versäumen, bringt Ihnen ein bereits zugewiesener **com_ptr** das Problem zu Bewusstsein (wenn Sie [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) oder [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) aufrufen), indem er behauptet, dass sein interner Zeiger nicht NULL ist.

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

Im nächsten Beispiel (in der *Nachher* Version) ruft die [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)-Mitgliedfunktion den zugrunde liegenden Rohzeiger als einen Zeiger auf einen Zeiger auf „void“ ab.

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

Ersetzen Sie **ComPtr::Get** durch [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function).

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

Wenn Sie den zugrunde liegenden Rohzeiger an eine Funktion übergeben möchten, der einen Zeiger auf **IUnknown** erwartet, verwenden Sie die Funktion [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown), wie im nächsten Beispiel gezeigt.

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>Portieren eines WRL-Moduls (Microsoft::WRL::Module)
Sie können nach und nach C++/WinRT-Code zu einem vorhandenen Projekt hinzufügen, das WRL verwendet, um eine Komponente zu implementieren; Ihre vorhandenen WRL-Klassen werden weiterhin unterstützt. Die Vorgehensweise wird in diesem Abschnitt erläutert.

Wenn Sie einen neuen Projekttyp **Komponente für Windows-Runtime (C++/WinRT)** in Visual Studio erstellen und erzeugen, wird die Datei `Generated Files\module.g.cpp` für Sie generiert. Diese Datei enthält die Definitionen von zwei nützlichen C++/WinRT-Funktionen (siehe unten), die Sie kopieren und dem Projekt hinzufügen können. Diese Funktionen sind **WINRT_CanUnloadNow** und **WINRT_GetActivationFactory**. Wie Sie sehen können, rufen diese bedingt WRL auf, um Sie in jedem Portierungsstadium zu unterstützen.

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

Nachdem Sie diese Funktionen Ihrem Projekt hinzugefügt haben, rufen Sie, anstatt [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method) direkt aufzurufen, **WINRT_GetActivationFactory** auf (die die WRL-Funktion intern aufruft). Im Folgenden finden Sie ein Codebeispiel für „Vorher“ und „Nachher“:

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

Anstatt [**Module::Terminate**](/cpp/windows/module-terminate-method) direkt aufzurufen, rufen Sie **WINRT_CanUnloadNow** auf (die die WRL Funktion intern aufruft). Im Folgenden finden Sie ein Codebeispiel für „Vorher“ und „Nachher“:

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>Wichtige APIs
* [Strukturvorlage „winrt::com_ptr“](/uwp/cpp-ref-for-winrt/com-ptr)
* [Struktur „winrt::Windows::Foundation::IUnknown“](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Zugehörige Themen
* [Einführung in C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md)
* [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)