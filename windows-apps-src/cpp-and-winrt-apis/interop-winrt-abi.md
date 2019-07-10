---
description: Dieses Thema zeigt, wie man zwischen Application Binary Interface (ABI) und C++/WinRT-Objekten konvertiert.
title: Interoperabilität zwischen C++/WinRT und der ABI
ms.date: 11/30/2018
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, Interoperabilität, ABI
ms.localizationpriority: medium
ms.openlocfilehash: a1745f9ad98ed8dac2e54e17d18467981eafdcec
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360226"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interoperabilität zwischen C++/WinRT und der ABI

In diesem Thema wird die Konvertierung zwischen SDK-ABI (Application Binary Interface) und [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)-Objekten behandelt. Die hier gezeigten Techniken ermöglichen die Implementierung von Interoperabilität zwischen Code, der diese beiden Programmiermethoden verwendet, und der Windows-Runtime. Außerdem kannst du damit deinen Code nach und nach von der ABI zu C++/WinRT migrieren.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>Was ist die Windows-Runtime-ABI, und welche ABI-Typen gibt es?
Eine Windows-Runtime-Klasse (Laufzeitklasse) ist eigentlich eine Abstraktion. Diese Abstraktion definiert eine binäre Schnittstelle (Application Binary Interface, ABI), die es verschiedenen Programmiersprachen ermöglicht, mit einem Objekt zu interagieren. Die Interaktion des Clientcodes mit einem Windows-Runtime-Objekt findet unabhängig von der Programmiersprache auf der untersten Ebene statt. Dabei werden Clientsprachkonstrukte in Aufrufe für die ABI des Objekts übersetzt.

Die Windows SDK-Header im Ordner „%WindowsSdkDir%Include\10.0.17134.0\winrt” sind die Windows-Runtime-ABI-Header-Dateien. (Hinweis: Die SDK-Versionsnummer muss ggf. angepasst werden.) Sie wurden vom MIDL-Compiler erstellt. Das folgende Beispiel zeigt die Einbindung eines dieser Header:

```cpp
#include <windows.foundation.h>
```

Und hier ist ein vereinfachtes Beispiel für einen der ABI-Typen aus diesem speziellen SDK-Header. Beachte den **ABI**-Namespace: **Windows::Foundation** (und alle anderen Windows-Namespaces) werden durch die SDK-Header innerhalb des **ABI**-Namespace deklariert.

```cpp
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

**IUriRuntimeClass** ist eine COM-Schnittstelle. Das ist aber noch nicht alles: Aufgrund ihrer Basis (**IInspectable**) ist **IUriRuntimeClass** auch eine Windows-Runtime-Schnittstelle. Beachte den Rückgabetyp **HRESULT** (anstelle der Auslösung von Ausnahmen). Interessant ist auch die Verwendung von Artefakten wie dem **HSTRING**-Handle. (Es empfiehlt sich, dieses Handle wieder auf `nullptr` festzulegen, wenn du es nicht mehr benötigst.) Das alles vermittelt einen ersten Eindruck davon, wie die Windows-Runtime auf der binären Ebene der Anwendung (sprich: auf der COM-Programmierebene) aussieht.

Die Windows-Runtime basiert auf COM-APIs (Component Object Model). Auf die Windows-Runtime kann entweder auf diese Weise oder über *Sprachprojektionen* zugegriffen werden. Eine Projektion verbirgt die COM-Details und bietet eine natürlichere Programmiererfahrung für eine bestimmte Sprache.

Der Ordner „%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt” enthält beispielsweise die C++/WinRT-Sprachprojektionsheader. (Auch hier muss ggf. die SDK-Versionsnummer entsprechend angepasst werden.) Genau wie bei den ABI-Headern steht für jeden Windows-Namespace ein Header zur Verfügung. Das folgende Beispiel zeigt die Einbindung eines der C++/WinRT-Header:

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

Und aus diesem Header siehst du hier (vereinfacht) das C++/WinRT-Äquivalent des ABI-Typs, mit dem wir uns gerade beschäftigt haben:

```cppwinrt
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

Bei der hier verwendeten Schnittstelle handelt es sich um modernen C++-Standardcode. **HRESULT**-Elemente gehören damit der Vergangenheit an. (C++/WinRT löst bei Bedarf Ausnahmen aus.) Die Accessorfunktion gibt ein einfaches Zeichenfolgenobjekt zurück, das am Ende seines Bereichs bereinigt wird.

Dieses Thema ist für Fälle relevant, in denen du Interoperabilität mit Code benötigst oder Code portieren möchtest, der auf der ABI-Ebene (Application Binary Interface) verwendet wird.

## <a name="converting-to-and-from-abi-types-in-code"></a>Konvertieren von ABI-Typen im Code
Aus Sicherheitsgründen und zur Vereinfachung kannst du für Konvertierungen in beide Richtungen einfach [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) und [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) verwenden. Das folgende Codebeispiel basiert auf der Projektvorlage **Console App** und zeigt auch die Verwendung von Namespacealiasen für die verschiedenen Inseln, um mit potenziellen Namespacekonflikten zwischen der C++/WinRT-Projektion und der ABI umzugehen:

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

Die Implementierungen der **as**-Funktionen rufen [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) auf. Wenn du Konvertierungen auf einer niedrigeren Ebene benötigst, die nur [**AddRef**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) aufrufen, kannst du die Hilfsfunktionen [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) und [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) verwenden. Im nächsten Codebeispiel werden dem obigen Codebeispiel diese Konvertierungen auf niedrigerer Ebene hinzugefügt:

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uri, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uri, ptr.get());
    ptr = nullptr;
}
```

Hier siehst du weitere ähnliche Low-Level-Konvertierungstechniken, diesmal jedoch mit Rohzeigern auf ABI-Schnittstellentypen (definiert durch die Windows SDK-Header):

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

Für Konvertierungen auf der untersten Ebene, bei denen nur Adressen kopiert werden, kannst du die Hilfsfunktionen [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) und [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi) verwenden.

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uri)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>Funktion „convert_from_abi“
Diese Hilfsfunktion konvertiert einen ABI-Schnittstellen-Rohzeiger mit minimalem Mehraufwand in ein äquivalentes C++/WinRT-Objekt.

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

Die Funktion ruft einfach [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) auf, um die Standardschnittstelle des gewünschten C++/WinRT-Typs abzufragen.

Wie wir bereits gesehen haben, ist für die Konvertierung von einem C++/WinRT-Objekt in den entsprechenden ABI-Schnittstellenzeiger keine Hilfsfunktion erforderlich. Verwende einfach die Memberfunktion [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (oder [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)), um nach der gewünschten Schnittstelle zu suchen. Die Funktionen **as** und **try_as** geben ein [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)-Objekt zurück, das den angeforderten ABI-Typ umschließt.

## <a name="code-example-using-convertfromabi"></a>Codebeispiel mit „convert_from_abi“
Das folgende Codebeispiel zeigt diese Hilfsfunktion in der Praxis:

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr };

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="important-apis"></a>Wichtige APIs
* [Funktion „AddRef“](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [Funktion „QueryInterface“](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Funktion „winrt::attach_abi“](/uwp/cpp-ref-for-winrt/attach-abi)
* [Strukturvorlage „winrt::com_ptr“](/uwp/cpp-ref-for-winrt/com-ptr)
* [Funktion „winrt::copy_from_abi“](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [Funktion „winrt::copy_to_abi“](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [Funktion „winrt::detach_abi“](/uwp/cpp-ref-for-winrt/detach-abi)
* [Funktion „winrt::get_abi“](/uwp/cpp-ref-for-winrt/get-abi)
* [Memberfunktion „winrt::Windows::Foundation::IUnknown::as“](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Memberfunktion „winrt::Windows::Foundation::IUnknown::try_as“](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
