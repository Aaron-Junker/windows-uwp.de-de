---
description: Dieses Thema zeigt, wie man zwischen Application Binary Interface (ABI) und C++/WinRT-Objekten konvertiert.
title: Interoperabilität zwischen C++/WinRT und der ABI
ms.date: 11/30/2018
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, Interoperabilität, ABI
ms.localizationpriority: medium
ms.openlocfilehash: ea9c41c134e1da0ffa131a597c856af7d75b5672
ms.sourcegitcommit: 99a30cc48d02e4034d4365f45d5d154b4c1e37ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106442296"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interoperabilität zwischen C++/WinRT und der ABI

In diesem Thema wird die Konvertierung zwischen SDK-ABI (Application Binary Interface) und [C++/WinRT](./intro-to-using-cpp-with-winrt.md)-Objekten behandelt. Die hier gezeigten Techniken ermöglichen die Implementierung von Interoperabilität zwischen Code, der diese beiden Programmiermethoden verwendet, und der Windows-Runtime. Außerdem kannst du damit deinen Code nach und nach von der ABI zu C++/WinRT migrieren.

Im Allgemeinen macht C++/WinRT ABI-Typen als **void\*** verfügbar, sodass Sie keine Plattformheaderdateien einschließen müssen.

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

Die Implementierungen der **as**-Funktionen rufen [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) auf. Wenn du Konvertierungen auf einer niedrigeren Ebene benötigst, die nur [**AddRef**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) aufrufen, kannst du die Hilfsfunktionen [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) und [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) verwenden. Im nächsten Codebeispiel werden dem obigen Codebeispiel diese Konvertierungen auf niedrigerer Ebene hinzugefügt:

> [!IMPORTANT]
> Bei der Zusammenarbeit mit ABI-Typen ist es entscheidend, dass der verwendete ABI-Typ der Standardschnittstelle des C++/WinRT-Objekts entspricht. Andernfalls werden durch Aufrufe von Methoden für den ABI-Typ am Ende tatsächlich Methoden im gleichen vtable-Slot auf der Standardschnittstelle aufgerufen, mit sehr unerwarteten Ergebnissen. Beachten Sie, dass [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) zur Kompilierungszeit nicht davor schützt, da es **void\*** für alle ABI-Typen verwendet und davon ausgeht, dass der Aufrufer darauf geachtet hat, die Typen nicht falsch abzugleichen. Damit soll vermieden werden, dass C++/WinRT-Header auf ABI-Header verweisen müssen, auch wenn ABI-Typen nie verwendet werden.

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uriAsIStringable, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uriAsIStringable, ptr.get());
    ptr = nullptr;
}
```

Hier siehst du weitere ähnliche Low-Level-Konvertierungstechniken, diesmal jedoch mit Rohzeigern auf ABI-Schnittstellentypen (definiert durch die Windows SDK-Header):

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uriAsIStringable, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uriAsIStringable, owning);
    owning->Release();
```

Für Konvertierungen auf der untersten Ebene, bei denen nur Adressen kopiert werden, kannst du die Hilfsfunktionen [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) und [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi) verwenden.

`WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uriAsIStringable)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uriAsIStringable));
    WINRT_ASSERT(!uriAsIStringable);
    winrt::attach_abi(uriAsIStringable, owning);
    WINRT_ASSERT(uriAsIStringable);
```

## <a name="convert_from_abi-function"></a>Funktion „convert_from_abi“
Diese Hilfsfunktion konvertiert einen ABI-Schnittstellen-Rohzeiger mit minimalem Mehraufwand in ein äquivalentes C++/WinRT-Objekt.

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr }; // `T` is a projected type.

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        winrt::put_abi(to)));

    return to;
}
```

Die Funktion ruft einfach [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) auf, um die Standardschnittstelle des gewünschten C++/WinRT-Typs abzufragen.

Wie wir bereits gesehen haben, ist für die Konvertierung von einem C++/WinRT-Objekt in den entsprechenden ABI-Schnittstellenzeiger keine Hilfsfunktion erforderlich. Verwende einfach die Memberfunktion [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (oder [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)), um nach der gewünschten Schnittstelle zu suchen. Die Funktionen **as** und **try_as** geben ein [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)-Objekt zurück, das den angeforderten ABI-Typ umschließt.

## <a name="code-example-using-convert_from_abi"></a>Codebeispiel mit „convert_from_abi“
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
        T to{ nullptr }; // `T` is a projected type.

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            winrt::put_abi(to)));

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

## <a name="interoperating-with-abi-com-interface-pointers"></a>Interaktion mit ABI-COM-Schnittstellenzeigern

Die folgende Hilfsfunktionsvorlage veranschaulicht das Kopieren eines ABI-COM-Schnittstellenzeigers eines bestimmten Typs in den entsprechenden projizierten Typ intelligenter Zeiger von C++/WinRT.

```cppwinrt
template<typename To, typename From>
To to_winrt(From* ptr)
{
    To result{ nullptr };
    winrt::check_hresult(ptr->QueryInterface(winrt::guid_of<To>(), winrt::put_abi(result)));
    return result;
}
...
ID2D1Factory1* com_ptr{ ... };
auto cppwinrt_ptr {to_winrt<winrt::com_ptr<ID2D1Factory1>>(com_ptr)};
```

Die nächste Hilfsfunktionsvorlage ist äquivalent, jedoch wird in ihr aus dem intelligenten Zeigertyp in den [Windows Implementation Libraries (WIL)](https://github.com/Microsoft/wil) kopiert.

```cppwinrt
template<typename To, typename From, typename ErrorPolicy>
To to_winrt(wil::com_ptr_t<From, ErrorPolicy> const& ptr)
{
    To result{ nullptr };
    if constexpr (std::is_same_v<typename ErrorPolicy::result, void>)
    {
        ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result));
    }
    else
    {
        winrt::check_result(ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result)));
    }
    return result;
}
```

Weitere Informationen findest du auch unter [Verwenden von COM-Komponenten mit C++/WinRT](./consume-com.md).

### <a name="unsafe-interop-with-abi-com-interface-pointers"></a>Unsichere Interaktion mit ABI-COM-Schnittstellenzeigern

In der folgenden Tabelle sind (zusätzlich zu anderen Vorgängen) unsichere Konvertierungen zwischen einem ABO-COM-Schnittstellenzeiger eines bestimmten Typs und dem entsprechenden projizierten Typ intelligenter Zeiger von C++/WinRT aufgeführt. Setzen Sie für den Code in der Tabelle diese Deklarationen voraus.

```cppwinrt
winrt::Sample s;
ISample* p;

void GetSample(_Out_ ISample** pp);
```

Setzen Sie außerdem voraus, dass **ISample** die Standardschnittstelle für **Sample** ist.

Sie können dies zur Kompilierzeit mit diesem Code bestätigen.

```cppwinrt
static_assert(std::is_same_v<winrt::default_interface<winrt::Sample>, winrt::ISample>);
```

| Vorgang | Vorgehensweise | Hinweise |
|-|-|-|
| **ISample\* *_ aus _* winrt::Sample** extrahieren | `p = reinterpret_cast<ISample*>(get_abi(s));` | *s* ist noch Besitzer des Objekts. |
| **ISample\* *_ von _* winrt::Sample** trennen | `p = reinterpret_cast<ISample*>(detach_abi(s));` | *s* ist nicht mehr Besitzer des Objekts. |
| **ISample\* *_ in ein neues _* winrt::Sample** übertragen | `winrt::Sample s{ p, winrt::take_ownership_from_abi };` | *s* wird Besitzer des Objekts. |
| **ISample\* *_ in _* winrt::Sample** einfügen | `*put_abi(s) = p;` | *s* wird Besitzer des Objekts. Objekte, die zuvor im Besitz von *s* waren, gehen verloren (wird beim Debuggen bestätigt). |
| **ISample\* *_ in _* winrt::Sample** empfangen | `GetSample(reinterpret_cast<ISample**>(put_abi(s)));` | *s* wird Besitzer des Objekts. Objekte, die zuvor im Besitz von *s* waren, gehen verloren (wird beim Debuggen bestätigt). |
| **ISample\* *_ in _* winrt::Sample** ersetzen | `attach_abi(s, p);` | *s* wird Besitzer des Objekts. Das Objekt, das zuvor im Besitz von *s* war, wird freigegeben. |
| **ISample\* *_ nach _* winrt::Sample** kopieren | `copy_from_abi(s, p);` | Es wird ein neuer Verweis von *s* auf das Objekt erstellt. Das Objekt, das zuvor im Besitz von *s* war, wird freigegeben. |
| **winrt::Sample** nach **ISample\** _ kopieren | `copy_to_abi(s, reinterpret_cast<void_&>(p));` | *p* erhält eine Kopie des Objekts. Objekte, die zuvor im Besitz von *p* waren, gehen verloren. |

## <a name="interoperating-with-the-abis-guid-struct"></a>Interaktion mit der GUID-Struktur der ABI

[**GUID**](/previous-versions/aa373931(v%3Dvs.80)) wird als **winrt::guid** projiziert. Wenn Sie APIs implementieren, müssen Sie **winrt::guid** für GUID-Parameter verwenden. Andernfalls erfolgen automatische Konvertierungen zwischen **winrt::guid** und **GUID**, solange Sie `unknwn.h` einschließen (von <windows.h> und vielen anderen Headerdateien implizit eingefügt), bevor Sie C++/WinRT-Header einschließen.

Wenn Sie dies nicht tun, können Sie `reinterpret_cast` zwischen ihnen erzwingen. Setzen Sie für die folgende Tabelle diese Deklarationen voraus.

```cppwinrt
winrt::guid winrtguid;
GUID abiguid;
```

| Konvertierung | Mit `#include <unknwn.h>` | Ohne `#include <unknwn.h>` |
|-|-|-|
| Von **winrt::guid** in **GUID** | `abiguid = winrtguid;` | `abiguid = reinterpret_cast<GUID&>(winrtguid);` |
| Von **GUID** in **winrt::guid** | `winrtguid = abiguid;` | `winrtguid = reinterpret_cast<winrt::guid&>(abiguid);` |

Sie können **winrt::guid** wie folgt konstruieren.

```cppwinrt
winrt::guid myGuid{ 0xC380465D, 0x2271, 0x428C, { 0x9B, 0x83, 0xEC, 0xEA, 0x3B, 0x4A, 0x85, 0xC1} };
```

Ein Gist, das zeigt, wie **winrt::guid** aus einer Zeichenfolge konstruiert wird, finden Sie unter [make_guid.cpp](https://gist.github.com/kennykerr/6c948882de395c25b3218ad8d4daf362).

## <a name="interoperating-with-the-abis-hstring"></a>Interaktion mit dem HSTRING der ABI

Die folgende Tabelle zeigt Konvertierungen zwischen **winrt::hstring** und [**HSTRING**](/windows/win32/winrt/hstring), sowie andere Vorgänge. Setzen Sie für den Code in der Tabelle diese Deklarationen voraus.

```cppwinrt
winrt::hstring s;
HSTRING h;

void GetString(_Out_ HSTRING* value);
```

| Vorgang | Vorgehensweise | Hinweise |
|-|-|-|
| **HSTRING** aus **hstring** extrahieren | `h = static_cast<HSTRING>(get_abi(s));` | *s* ist noch Besitzer der Zeichenfolge. |
| **HSTRING** von **hstring** trennen | `h = reinterpret_cast<HSTRING>(detach_abi(s));` | *s* ist nicht mehr Besitzer der Zeichenfolge. |
| **HSTRING** in **hstring** einfügen | `*put_abi(s) = h;` | *s* wird Besitzer der Zeichenfolge. Zeichenfolgen, die zuvor im Besitz von *s* waren, gehen verloren (wird beim Debuggen bestätigt). |
| **HSTRING** in **hstring** empfangen | `GetString(reinterpret_cast<HSTRING*>(put_abi(s)));` | *s* wird Besitzer der Zeichenfolge. Zeichenfolgen, die zuvor im Besitz von *s* waren, gehen verloren (wird beim Debuggen bestätigt). |
| **HSTRING** in **hstring** ersetzen | `attach_abi(s, h);` | *s* wird Besitzer der Zeichenfolge. Die Zeichenfolge, die zuvor im Besitz von *s* war, wird freigegeben. |
| **HSTRING** nach **hstring** kopieren | `copy_from_abi(s, h);` | *s* erstellt eine private Kopie der Zeichenfolge. Die Zeichenfolge, die zuvor im Besitz von *s* war, wird freigegeben. |
| **hstring** nach **HSTRING** kopieren | `copy_to_abi(s, reinterpret_cast<void*&>(h));` | *h* erhält eine Kopie der Zeichenfolge. Zeichenfolgen, die zuvor im Besitz von *h* waren, gehen verloren. |

Darüber hinaus führen die [Zeichenfolgen-Hilfsprogramme](https://github.com/microsoft/wil/wiki/String-helpers) der Windows Implementation Libraries (WIL) grundlegende Zeichenfolgenbearbeitung durch. Um die WIL-Zeichenfolgen-Hilfsprogramme zu verwenden, fügen Sie [<wil/resource.h>](https://github.com/microsoft/wil/blob/master/include/wil/resource.h) hinzu, und beachten Sie die folgende Tabelle. Ausführliche Informationen finden Sie unter den Links in der Tabelle.

| Vorgang | WIL-Zeichenfolgen-Hilfsprogramm für weitere Informationen |
|-|-|
| Einen unformatierten Unicode- oder ANSI-Zeichenfolgenzeiger und eine optionale Länge angeben. Einen entsprechend spezialisierten **unique_any**-Wrapper erhalten | [wil::make_something_string](https://github.com/microsoft/wil/wiki/String-helpers#wilmake_something_string) |
| Ein intelligentes Objekt entpacken, bis ein unformatierter, mit NULL endender Unicode-Zeichenfolgenzeiger gefunden wird | [wil::str_raw_ptr](https://github.com/microsoft/wil/wiki/String-helpers#wilstr_raw_ptr) |
| Die von einem intelligenten Zeigerobjekt umschlossene Zeichenfolge abrufen oder die leere Zeichenfolge `L""` abrufen, wenn der intelligente Zeiger leer ist | [wil::string_get_not_null](https://github.com/microsoft/wil/wiki/String-helpers#wilstring_get_not_null) |
| Beliebige Anzahl von Zeichenfolgen verketten | [wil::str_concat](https://github.com/microsoft/wil/wiki/String-helpers#wilstr_concat) |
| Eine Zeichenfolge aus einer Formatzeichenfolge im printf-Format und einer entsprechenden Parameterliste abrufen | [wil::str_printf](https://github.com/microsoft/wil/wiki/String-helpers#wilstr_printf) |

## <a name="important-apis"></a>Wichtige APIs
* [Funktion „AddRef“](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [Funktion „QueryInterface“](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Funktion „winrt::attach_abi“](/uwp/cpp-ref-for-winrt/attach-abi)
* [Strukturvorlage „winrt::com_ptr“](/uwp/cpp-ref-for-winrt/com-ptr)
* [Funktion „winrt::copy_from_abi“](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [Funktion „winrt::copy_to_abi“](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [Funktion „winrt::detach_abi“](/uwp/cpp-ref-for-winrt/detach-abi)
* [Funktion „winrt::get_abi“](/uwp/cpp-ref-for-winrt/get-abi)
* [Memberfunktion „winrt::Windows::Foundation::IUnknown::as“](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Memberfunktion „winrt::Windows::Foundation::IUnknown::try_as“](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
