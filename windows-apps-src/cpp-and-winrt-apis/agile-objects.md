---
description: Ein agiles Objekt ist ein Objekt, auf das von jedem Thread aus zugegriffen werden kann. Ihre C++/WinRT-Typen sind standardmäßig agil, aber Sie können diese Option auch deaktivieren.
title: Agile Objekte mit C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, agil, objekt, agilität, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 71800de1d209a0164ab5a7e90bbc191c0f9bebe9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154634"
---
# <a name="agile-objects-in-cwinrt"></a>Agile Objekte in C++/WinRT

In den meisten Fällen kann von jedem Thread aus auf eine Instanz einer Windows-Runtime-Klasse zugegriffen werden (wie es bei den meisten C++-Standardobjekten der Fall ist). Eine solche Windows-Runtime-Klasse ist *agile*. Nur eine kleine Anzahl von Windows-Runtime-Klassen, die mit Windows bereitgestellt werden, sind nicht agil. Wenn Sie sie nutzen, müssen Sie ihr Threadingmodell und ihr Marshallingverhalten berücksichtigen (Marshalling ist die Weitergabe von Daten über eine Apartmentgrenze). Es ist ein guter Standard für jedes Windows-Runtime-Objekt, agil zu sein, sodass Ihre eigenen [C++/WinRT](./intro-to-using-cpp-with-winrt.md)-Typen standardmäßig agil sind.

Sie können dieses Verhalten jedoch abwählen. Möglicherweise haben Sie einen zwingenden Grund, ein Objekt Ihres Typs zu beschränken (z.B. in einem Singlethread-Apartment). Dies hat typischerweise mit Wiedereinsprungsvoraussetzungen zu tun. Aber auch die UI-APIs (User Interface, Benutzerschnittstelle) bieten zunehmend agile Objekte. Im Allgemeinen ist Agilität die einfachste und leistungsfähigste Option. Auch wenn Sie eine Aktivierungsfactory implementieren, muss diese agil sein (selbst dann, wenn Ihre entsprechende Laufzeitklasse es nicht ist).

> [!NOTE]
> Windows-Runtime basiert auf COM. Im Sinne von COM wird eine agile Klasse mit `ThreadingModel` = *Both* registriert. Weitere Informationen zu COM-Threadingmodellen finden Sie unter [Verstehen und Verwenden von COM-Threadingmodellen](/previous-versions/ms809971(v=msdn.10)).

## <a name="code-examples"></a>Codebeispiele

Sehen wir uns uns anhand einer Beispielimplementierung einer Laufzeitklasse an, wie C++/WinRT Agilität unterstützt.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Da wir dies nicht explizit ausschließen, ist diese Implementierung agil. Die [**winrt::implementiert**](/uwp/cpp-ref-for-winrt/implements)-Basisstruktur implementiert [**IAgileObject**](/windows/desktop/api/objidl/nn-objidl-iagileobject) und [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). Die **IMarshal**-Implementierung verwendet **CoCreateFreeThreadedMarshaler**, um Legacycode zu unterstützen, dem **IAgileObject** nicht bekannt ist.

Dieser Code prüft ein Objekt auf Agilität. Der Aufruf von [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) löst eine Ausnahme aus, wenn `myimpl` nicht agil ist.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

Anstatt eine Ausnahme zu verarbeiten, können Sie [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) aufrufen.

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** hat keine eigenen Methoden, sodass hier keine Möglichkeiten vorhanden sind. Diese nächste Variante ist eher typisch.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** ist eine *Markerschnittstelle*. Der Nutzen der Abfrage von **IAgileObject** besteht aus den Informationen zum Erfolg oder Misserfolg.

## <a name="opting-out-of-agile-object-support"></a>Vermeiden der Unterstützung von agilen Objekten

Sie können die Unterstützung für agile Objekte explizit deaktivieren, indem Sie die [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile)-Markerstruktur als template-Argument an Ihre Basisklasse übergeben.

Wenn Sie direkt von **winrt::implements** ableiten.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

Wenn Sie eine Laufzeitklasse erstellen.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

Dabei spielt es keine Rolle, wo im variadic-Parameterpaket die Markerstruktur verwendet wird.

Unabhängig davon, ob Sie die Agilität abwählen, können Sie **IMarshal** selbst implementieren. Beispielsweise können Sie den Marker **winrt::non_agile** verwenden, um die Agilitätsstandardimplementierung zu vermeiden, und **IMarshal** selbst implementieren (um die Marshal-by-Value-Semantik zu unterstützen).

## <a name="agile-references-winrtagile_ref"></a>Agile-Verweise (winrt::agile_ref)

Wenn Sie ein Objekt verwenden, das nicht agil ist, Sie es aber in einem potenziell agilen Kontext übergeben müssen, dann ist eine Option die Verwendung der [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)-Strukturvorlage. So erhalten Sie einen agilen Verweis auf eine Instanz eines nicht agilen Typs oder auf eine Schnittstelle eines nicht agilen Objekts.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

Sie können auch die Hilfsfunktion [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) verwenden.

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

Der Aufruf [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) gibt einen Proxy zurück, der im Threadkontext, in dem **get** aufgerufen wird, sicher verwendet werden kann.

## <a name="important-apis"></a>Wichtige APIs

* [IAgileObject-Schnittstelle](/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [IMarshal-Schnittstelle](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref-Strukturvorlage](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements-Strukturvorlage](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile Funktionsvorlage](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile-Markerstruktur](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown::as-Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as-Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>Zugehörige Themen

* [Verstehen und Verwenden von COM-Threadingmodellen](/previous-versions/ms809971(v=msdn.10))