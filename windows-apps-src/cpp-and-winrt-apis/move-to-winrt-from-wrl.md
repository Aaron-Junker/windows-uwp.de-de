---
description: In diesem Thema wird gezeigt, wie Sie WRL-Code zum entsprechenden Äquivalent in C++/WinRT portieren.
title: Von WRL zu C++/WinRT wechseln
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, portieren, migrieren, WRL
ms.localizationpriority: medium
ms.openlocfilehash: e81f82fe823ee0fdf81741c89576adf268940d91
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630745"
---
# <a name="move-to-cwinrt-from-wrl"></a>Von WRL zu C++/WinRT wechseln
In diesem Thema wird gezeigt, wie Sie Portieren [Windows Runtime C++ Template Library (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) Code in den entsprechenden [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

Der erste Schritt beim Portieren von c++ / WinRT C++ manuell hinzufügen ist c++ / WinRT-Unterstützung für Ihr Projekt (finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Installieren Sie zu diesem Zweck die [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) in Ihr Projekt. Öffnen des Projekts in Visual Studio, klicken Sie auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Navigieren Sie**geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **installieren** zum Installieren des Pakets für das betreffende Projekt. Ein Effekt der, die diese Änderung wird die Unterstützung für [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) ist im Projekt deaktiviert. Wenn Sie C++/CX im Projekt verwenden, können Sie die Unterstützung deaktiviert lassen und Ihren C++/CX-Code in C++/WinRT aktualisieren (siehe [Wechsel zu C++/WinRT von C++/CX](move-to-winrt-from-cx.md)). Oder Sie können Unterstützung wieder aktivieren (in den Projekteigenschaften, **C/C++-** \> **allgemeine** \> **Windows-Runtime-Erweiterung nutzen** \> **Ja (/ Zw)**), und zuerst darauf konzentrieren, für das Portieren von WRL-Code. C++ / CX und C++ / WinRT-Code kann gleichzeitig im selben Projekt, mit Ausnahme von XAML-Unterstützung des Compilers und der Windows-Runtime-Komponenten verwendet werden (finden Sie unter [verschieben in C++ / WinRT in C++ / CX](move-to-winrt-from-cx.md)).

Festlegen von Projekteigenschaften **allgemeine** \> **Zielplattformversion** zu 10.0.17134.0 (Windows 10, Version 1803) oder höher.

Fügen Sie Ihrer vorkompilierten Headerdatei (in der Regel `pch.h`) `winrt/base.h` hinzu.

```cppwinrt
#include <winrt/base.h>
```

Wenn Sie alle C++/WinRT-projizierten Windows-API-Header hinzufügen (z. B. `winrt/Windows.Foundation.h`), müssen Sie so nicht explizit `winrt/base.h` einschließen, da dies automatisch erfolgt.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>Portieren von intelligenten WRL-COM-Zeigern ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
Portieren von Code, der verwendet **Microsoft::WRL::ComPtr\<T\>**  verwenden [ **winrt::com_ptr\<T\>**](/uwp/cpp-ref-for-winrt/com-ptr). Im Folgenden finden Sie ein Codebeispiel für „Vorher“ und „Nachher“: In der *Nachher*-Version ruft die [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)-Mitgliedsfunktion den zugrunde liegenden Rohzeiger ab, sodass dieser definiert werden kann.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> Wenn Sie haben eine [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) , die bereits angebracht ist (die interne rohzeiger hat bereits ein Ziel) es darauf an ein anderes Objekt erneut Arbeitsplätzen werden sollen, und Sie müssen zunächst zuweisen `nullptr` Damit&mdash;wie im folgenden Codebeispiel gezeigt. Falls nicht, klicken Sie dann ein bereits angeschlossene **Com_ptr** zeichnen Sie das Problem wird Ihre Aufmerksamkeit (beim Aufrufen [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) oder [ **Com_ptr:: Put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) durch die Assertion, dass die internen Zeiger nicht null ist.

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

Im nächsten Beispiel (in der *Nachher* Version) ruft die [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)-Mitgliedfunktion den zugrunde liegenden normalen Zeiger als einen Zeiger auf einen Zeiger auf „void“ ab.

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

Ersetzen Sie **ComPtr::Get** durch [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function).

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

Sollen den zugrunde liegende unformatierten Zeiger an eine Funktion übergeben, die einen Zeiger auf erwartet **IUnknown**, verwenden Sie die [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) kostenlose-Funktion, wie in diesem nächsten dargestellt Beispiel:.

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

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>Portieren von einer WRL-Modul (Microsoft::WRL::Module)
Sie können nach und nach C++/WinRT-Code zu einem vorhandenen Projekt hinzufügen, das WRL verwendet, um eine Komponente zu implementieren; Ihre vorhandenen WRL-Klassen werden weiterhin unterstützt. Dieser Abschnitt zeigt, wie das geht.

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
* [Vorlage für WinRT::com_ptr-Struktur](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Verwandte Themen
* [Einführung in C++ / WinRT](intro-to-using-cpp-with-winrt.md)
* [Verschieben von c++ / WinRT in C++ / CX](move-to-winrt-from-cx.md)
* [Windows Runtime C++-Vorlagenbibliothek (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
