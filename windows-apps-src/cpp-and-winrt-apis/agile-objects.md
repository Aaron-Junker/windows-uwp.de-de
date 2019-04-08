---
description: Ein agiles Objekt ist ein Objekt, auf das von jedem Thread aus zugegriffen werden kann. Ihre C++/WinRT-Typen sind standardmäßig agil, aber Sie können diese Option deaktivieren.
title: Agile Objekte mit C++/WinRT
ms.date: 10/20/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, agil, objekt, agilität, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 2481396d9348250e14ebfc2d1f940b663b405f77
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639665"
---
# <a name="agile-objects-in-cwinrt"></a>Agile Objekte in C++ / WinRT

In der Mehrzahl der Fälle kann eine Instanz von einer Windows-Runtime-Klasse zugegriffen werden, von jedem Thread (ebenso wie die meisten Standard-c++-Objekten). Solche eine Windows-Runtime-Klasse ist *agile*. Nur eine kleine Anzahl von Windows-Runtime-Klassen, die mit Windows ausgeliefert nicht-agile werden, aber wenn Sie zu verwenden, Sie ihre Threadmodell und Marshallingverhalten berücksichtigen müssen (Marshalling ist übergeben von Daten über eine Apartmentgrenze). Es ist ein geeigneter Standard für alle Windows-Runtime-Objekt, werden agile, damit Ihre eigenen [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Typen sind standardmäßig agile.

Aber Sie können das Programm verlassen. Sie müssen möglicherweise einen zwingenden Grund ein Objekt des Typs befinden, z. B. in einem bestimmten Singlethread-Apartment erforderlich ist. Dies hat typischerweise mit Wiedereinsprungsvoraussetzungen zu tun. Aber auch die UI-APIs (User Interface, Benutzerschnittstelle) bieten zunehmend agile Objekte. Im Allgemeinen ist Agilität die einfachste und leistungsfähigste Option. Auch wenn Sie eine Aktivierungs-Factory implementieren, muss diese agil sein (auch, wenn Ihre entsprechende Laufzeitklasse es nicht ist).

> [!NOTE]
> Windows-Runtime basiert auf COM. Im Sinne von COM ist eine agile Klasse mit `ThreadingModel` = *Both* registriert. Weitere Informationen zu COM, Threadingmodelle und Apartments, finden Sie unter [verstehen und Verwenden von COM-Threadingmodellen](https://msdn.microsoft.com/library/ms809971).

## <a name="code-examples"></a>Codebeispiele

Verwenden wir eine beispielimplementierung einer Runtime-Klasse, die veranschaulichen, wie C + c++ / WinRT unterstützt Flexibilität.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Da wir dies nicht explizit ausschließen, ist diese Implementierung agil. Die [**winrt::implementiert**](/uwp/cpp-ref-for-winrt/implements) Basisstruktur implementiert [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) und [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). Die **IMarshal**-Implementierung verwendet **CoCreateFreeThreadedMarshaler**, um das Legacy-Code zu unterstützen, der nichts über **IAgileObject** weiß.

Dieser Code prüft ein Objekt auf Agilität. Der Aufruf von [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) löst eine Ausnahme aus, wenn `myimpl` nicht agil ist.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

Anstatt eine Ausnahme zu verarbeiten, können Sie [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) aufrufen.

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** hat keine eigenen Methoden, sodass man damit nicht viel anfangen kann. Diese nächste Variante ist eher typisch.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** ist eine *Marker-Schnittstelle*. Der Nutzen der Abfrage von **IAgileObject** besteht aus der Informationen zum Erfolg oder Misserfolg.

## <a name="opting-out-of-agile-object-support"></a>Vermeiden der Unterstützung von agilen Objekten

Sie können die Unterstützung für agile Objekte explizit deaktivieren, indem Sie die [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile)-Markerstruktur als template-Argument an Ihre Basisklasse übergeben.

Wenn Sie direkt von **winrt::implements** ableiten.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

Wenn Sie eine Laufzeitklasse schreiben.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

Dabei spielt es keine Rolle, wo im Variadic-Parameterpaket die Markerstruktur erscheint.

Unabhängig davon, ob Sie die Flexibilität deaktivieren, können Sie implementieren **IMarshal** selbst. Beispielsweise können Sie die **winrt::non_agile** Markierung, vermeiden Sie die standardmäßige Implementierung der Flexibilität, und implementieren die **IMarshal** selbst&mdash;möglicherweise den Marshal-by-Value-Semantik unterstützt.

## <a name="agile-references-winrtagileref"></a>Agile Referenzen (winrt::agile_ref)

Wenn Sie ein Objekt verwenden, das nicht agil ist, aber Sie es in einem potentiell agilen Kontext weitergeben müssen, dann ist eine Option die Verwendung der [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)-Strukturvorlage. So erhalten Sie eine agile Referenz auf eine Instanz eines nicht agilen Typs oder auf eine Schnittstelle eines nicht agilen Objekts.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

Oder Sie können die Hilfsfunktion [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) verwenden.

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

In beiden Fällen kann `agile` nun frei an einen Thread in einem anderen Apartment übergeben und dort verwendet werden.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

Der Aufruf [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agilerefget-function) gibt einen Proxy zurück, der in dem Thread-Kontext, in dem **get** aufgerufen wird, sicher verwendet werden kann.

## <a name="important-apis"></a>Wichtige APIs

* [IAgileObject-Schnittstelle](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [IMarshal-Schnittstelle](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [Vorlage für WinRT::agile_ref-Struktur](/uwp/cpp-ref-for-winrt/agile-ref)
* [Vorlage für WinRT::Implements-Struktur](/uwp/cpp-ref-for-winrt/implements)
* [Vorlage für WinRT::make_agile-Funktion](/uwp/cpp-ref-for-winrt/make-agile)
* [WinRT::non_agile Marker-Struktur](/uwp/cpp-ref-for-winrt/non-agile)
* [WinRT::Windows::Foundation::IUnknown:: als Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [WinRT::Windows::Foundation::IUnknown::try_as-Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>Verwandte Themen

* [Verstehen und Verwenden von COM-Threadmodell](https://msdn.microsoft.com/library/ms809971)
