---
description: Dieses Thema zeigt, wie man zwischen Application Binary Interface (ABI) und C++/WinRT-Objekten konvertiert.
title: Interoperabilität zwischen C++/WinRT und der ABI
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, portieren, migrieren, interoperabilität, ABI
ms.localizationpriority: medium
ms.openlocfilehash: a33a52cd8c18b312dc9e020a4c4ba518c33b0dd9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639945"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interoperabilität zwischen C++/WinRT und der ABI

In diesem Thema wird gezeigt, wie für die Konvertierung zwischen SDK für anwendungsbinärdateischnittstelle (ABI) und [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Objekte. Sie können diese Techniken verwenden, um zwischen Code, der diese beiden Möglichkeiten der Programmierung mit Windows-Runtime verwendet, zu interagieren. Sie können sie außerdem verwenden, wenn Sie Ihren Code schrittweise von der ABI nach C++/WinRT verschieben.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>Was ist die Windows-Runtime-ABI, und welche Arten von ABI gibt es?
Eine Windows-Runtime-Klasse (Laufzeitklasse) ist eigentlich eine Abstraktion. Diese Abstraktion definiert eine binäre Schnittstelle (Application Binary Interface oder ABI), die es verschiedenen Programmiersprachen ermöglicht, mit einem Objekt zu interagieren. Unabhängig von der Programmiersprache erfolgt die Client-Code-Interaktion bei einem Windows-Runtime-Objekt auf der untersten Ebene, wobei die Client-Sprachkonstrukte in Aufrufe im ABI des Objekts übersetzt werden.

Die Windows SDK-Header im Ordner „%WindowsSdkDir%Include\10.0.17134.0\winrt” (passen Sie ggf. die SDK-Versionsnummer an) sind die Windows-Runtime-ABI-Header-Dateien. Sie wurden vom MIDL-Compiler erstellt. Hier ist ein Beispiel für die Verwendung eines dieser Header.

```
#include <windows.foundation.h>
```

Und hier ist ein vereinfachtes Beispiel für einen der ABI-Typen, die Sie in diesem speziellen SDK-Header finden. Beachten Sie den **ABI**-Namespace: **Windows::Foundation** und alle anderen Windows-Namespaces werden von den SDK-Headern innerhalb des **ABI**-Namespace deklariert.

```
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

**IUriRuntimeClass** ist eine COM-Schnittstelle. Da die Basis **IInspectable** ist, ist **IUriRuntimeClass** außerdem eine Windows-Runtime-Schnittstelle. Beachten Sie den Rückgabetyp **HRESULT** und nicht das Auslösen von Ausnahmen. Und die Verwendung von Artefakten wie dem **HSTRING**-Handle (es ist gute Praxis, dieses Handle wieder auf `nullptr` zu setzen, wenn Sie damit fertig sind). Dies gibt einen Vorgeschmack darauf, wie die Windows-Runtime auf der binären Ebene der Anwendung, also auf der COM-Programmierebene, aussieht.

Die Windows-Runtime basiert auf COM-APIs (Component Object Model). Sie können auf diese Weise oder über *Sprachprojektionen* auf die Windows-Runtime zugreifen. Eine Projektion verbirgt die COM-Details und bietet eine natürlichere Programmiererfahrung für eine bestimmte Sprache.

Wenn Sie z. B. in den Ordner „%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt” (passen Sie ggf. die SDK-Versionsnummer an) sehen, dann finden Sie die C++/WinRT-Sprachprojektionsheader. Es gibt einen Header für jeden Windows-Namespace, so wie es einen ABI-Header pro Windows-Namespace gibt. Hier ist ein Beispiel für die Einbindung eines der C++/WinRT-Header.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

Und aus diesem Header sehen Sie hier (vereinfacht) das C++/WinRT-Äquivalent zu dem ABI-Typ, den wir gerade gesehen haben.

```
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

Die Schnittstelle ist modernes Standard C++. Es beseitigt **HRESULT**-Elemente (C++/WinRT löst bei Bedarf Ausnahmen aus). Und die Zugriffsfunktion gibt ein einfaches String-Objekt zurück, das am Ende seines Gültigkeitsbereichs bereinigt wird.

Dieses Thema ist für Fälle relevant, in den Sie Interoperabilität mit Code gewährleisten oder Code portieren möchten, der auf der Application Binary Interface (ABI)-Ebene funktioniert.

## <a name="converting-to-and-from-abi-types-in-code"></a>Konvertierung von und zu ABI-Typen im Code
Zur Sicherheit und Einfachheit können Sie für Konvertierungen in beide Richtungen einfach [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) und [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) verwenden. Hier ist ein Codebeispiel (basierend auf der Projektvorlage **Console App**), das auch zeigt, wie Sie Namespace-Aliase für die verschiedenen Inseln verwenden können, um mit anderweitigen potenziellen Namespacekonflikten zwischen der C++/WinRT-Projektion und der ABI umzugehen.

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

Die Implementierungen der **as**-Funktionen rufen [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) auf. Wenn Sie Low-Level-Konvertierungen benötigen, die nur [**AddRef**](https://msdn.microsoft.com/library/windows/desktop/ms691379) aufrufen, können Sie die Hilfsfunktionen [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) und [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) verwenden. Dieses nächste Codebeispiel fügt diese Low-Level-Konvertierungen dem obigen Codebeispiel hinzu.

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

Hier sind andere ähnliche Low-Level-Konvertierungstechniken. Diese verwenden jedoch nur Zeiger auf ABI-Schnittstellentypen (die durch die Windows SDK-Header definiert sind).

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

Für die größtmöglichen Low-Level-Konvertierungen, die nur Adressen kopieren, können Sie die Hilfsfunktionen [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) und [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi) verwenden.

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

## <a name="convertfromabi-function"></a>convert_from_abi Funktion
Diese Helferfunktion konvertiert einen Raw-ABI-Interface-Zeiger in ein äquivalentes C++/WinRT-Objekt mit minimalem Overhead.

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

Die Funktion ruft einfach [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) auf, um die Standardschnittstelle des gewünschten C++/WinRT-Typs abzufragen.

Wie wir gesehen haben, ist eine Hilfsfunktion nicht erforderlich, um von einem C++/WinRT-Objekt in den entsprechenden ABI-Interface-Zeiger zu konvertieren. Verwenden Sie einfach die Funktion [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)-Mitgliedsfunktion (oder [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)), um nach der gewünschten Schnittstelle zu suchen. Die Funktionen **as** und **try_as** geben ein [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)-Wrapper-Objekt für den gewünschten ABI-Typ zurück.

## <a name="code-example-using-convertfromabi"></a>Codebeispiel mit convert_from_abi
Hier ist ein Codebeispiel, das diese Helferfunktion in der Praxis zeigt.

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
* [AddRef-Funktion](https://msdn.microsoft.com/library/windows/desktop/ms691379)
* [QueryInterface-Funktion](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [WinRT::attach_abi-Funktion](/uwp/cpp-ref-for-winrt/attach-abi)
* [Vorlage für WinRT::com_ptr-Struktur](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::copy_from_abi function](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [WinRT::copy_to_abi-Funktion](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [WinRT::detach_abi-Funktion](/uwp/cpp-ref-for-winrt/detach-abi)
* [WinRT::get_abi-Funktion](/uwp/cpp-ref-for-winrt/get-abi)
* [WinRT::Windows::Foundation::IUnknown:: Member-Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [WinRT::Windows::Foundation::IUnknown::try_as-Member-Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)
