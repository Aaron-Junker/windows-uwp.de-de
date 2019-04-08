---
description: C++/WinRT verfügt über Funktionen und Basisklassen, mit denen Sie beim Implementieren bzw. Übergeben von Sammlungen viel Zeit sparen können.
title: Sammlungen mit C++ / WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, Uwp, standard, c++, Cpp, Winrt, Projektion, Auflistung
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a50ab5f70faa0c8f8b73eada38444bcafd444d8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635435"
---
# <a name="collections-with-cwinrt"></a>Sammlungen mit C++ / WinRT

Intern verfügt über eine Windows-Runtime-Sammlung auf sehr kompliziert "bewegliche Teile" aus. Aber wenn Sie ein Auflistungsobjekt an eine Windows-Runtime-Funktion übergeben oder eine eigene Auflistung – Eigenschaften und Auflistungstypen implementieren möchten, sind Funktionen und Basisklassen in [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) , die Sie unterstützen. Diese Funktionen vereinfachen Sie die volle Kontrolle, und sparen Sie Zeit und Mühe ein enormer Aufwand verbunden.

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_) ist die Windows-Runtime-Schnittstelle implementiert, die von jedem random-Access-Auflistung von Elementen. Würden Sie implementieren **IVector** selbst außerdem müssten Sie implementieren [ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [ **IVectorView** ](/uwp/api/windows.foundation.collections.ivectorview_t_), und [ **IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Auch wenn Sie *müssen* einen benutzerdefinierten Auflistungstyp, die eine Menge Arbeit. Aber wenn Sie über Daten, in verfügen einem **Std:: vector** (oder ein **Std:: Map**, oder ein **Std:: unordered_map**) möchten lediglich, die an eine Windows-Runtime-API übergeben, und Sie vermeiden möchten dieser Ebene arbeiten, wenn möglich. Und vermeiden es *ist* möglich, da C++ / WinRT hilft Ihnen die Erstellung von Auflistungen effizient und mit wenig Aufwand.

Siehe auch [ItemsControl für XAML, binden an einen C++ / WinRT-Auflistung](binding-collection.md).

> [!NOTE]
> Wenn die Windows-SDK-Version 10.0.17763.0 (Windows 10, Version 1809) nicht installiert haben, oder höher, dann haben Sie keinen Zugriff auf Funktionen und Klassen, die in diesem Thema dokumentiert sind. Stattdessen finden Sie unter [, wenn Sie eine ältere Version des Windows SDK ist](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) eine Auflistung einer Observable-Vektor-Vorlage, die Sie stattdessen verwenden können.

## <a name="helper-functions-for-collections"></a>Hilfsfunktionen für Sammlungen

### <a name="general-purpose-collection-empty"></a>Allgemeine, leere Auflistung

Dieser Abschnitt enthält die möglichen Szenario für die zum Erstellen einer Sammlung, die anfänglich leer ist; und füllen Sie es dann *nach* erstellen.

Um ein neues Objekt eines Typs abzurufen, die eine allgemeine Auflistung implementiert, können Sie den Aufrufen der [ **winrt::single_threaded_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-vector) Funktionsvorlage. Das Objekt wird zurückgegeben, als ein [ **IVector**](/uwp/api/windows.foundation.collections.ivector_t_), und das ist die Schnittstelle, die über den Sie die Funktionen und Eigenschaften des zurückgegebenen Objekts aufrufen.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

Wie im obigen Codebeispiel sehen ist, nach der Erstellung der Sammlung kann Elemente angefügt, durchlaufen sie, und behandeln Sie das Objekt in der Regel, wie Sie ein beliebiges Sammlungsobjekt Windows-Runtime, die Sie über eine API erhalten haben, können. Wenn Sie eine unveränderliche Ansicht über der Auflistung benötigen, rufen Sie [ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), wie gezeigt. Der oben gezeigten Muster&mdash;des Erstellens und Nutzens einer Sammlung&mdash;eignet sich für einfache Szenarien möchten Sie zum Übergeben von Daten in oder Abrufen von Daten aus einer API. Können Sie übergeben ein **IVector**, oder ein **IVectorView**und überall ein [ **IIterable** ](/uwp/api/windows.foundation.collections.iiterable_t_) wird erwartet.

Im Codebeispiel oben den Aufruf von **winrt::init_apartment** initialisiert COM; standardmäßig in einem Multithread-Apartment.

### <a name="general-purpose-collection-primed-from-data"></a>Allgemeine Sammlung von Daten sind

Dieser Abschnitt behandelt das Szenario, in dem Sie eine Sammlung erstellen und füllen diese zur gleichen Zeit möchten.

Sie können vermeiden, den Aufwand für Aufrufe **Append** im vorherigen Codebeispiel. Möglicherweise verfügen Sie bereits die Daten, oder Sie möchten die Daten vor der Erstellung der Windows-Runtime-Auflistungsobjekt zu füllen. Hier ist wie das geht.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Sie können ein temporäres Objekt, das mit Ihrer Daten an übergeben **winrt::single_threaded_vector**, wie bei `coll1`weiter oben. Oder Sie verschieben eine **Std:: vector** (vorausgesetzt, Sie wird nicht auf zugreifen werden sie erneut) an die Funktion übergeben. In beiden Fällen sind Sie übergeben eine *Rvalue* an die Funktion übergeben. Die können des Compilers, effiziente und kopieren die Daten zu vermeiden. Wenn Sie mehr erfahren möchten *Rvalues*, finden Sie unter [Wert Kategorien und Verweise darauf](cpp-value-categories.md).

Wenn Sie ein XAML-Elemente-Steuerelement in Ihrer Sammlung binden möchten, können Sie Sie. Beachten Sie jedoch, die die ordnungsgemäße Festlegung der [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) -Eigenschaft müssen Sie es auf einen Wert vom Typ festgelegt **IVector** von **"iinspectable"** (oder eines Typs Interoperabilität, z. B. [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Hier ist ein Codebeispiel, die erzeugt eine Auflistung von einem Typ, der für die Bindung geeignet, und ein Element hinzufügt.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Sie können eine Windows-Runtime-Sammlung aus Daten erstellen und bereiten Sie eine Ansicht für diese Übergabe an eine API, ohne Alles kopieren.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

In den obigen Beispielen die Auflistung erstellen wir *können* gebunden werden, um eine XAML-Elemente-Steuerelement, aber die Auflistung nicht Observable-Objekt.

### <a name="observable-collection"></a>Observable-Auflistung

Um ein neues Objekt eines Typs abzurufen, die implementiert eine *Observable* Auflistung, rufen die [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) Funktionsvorlage mit einer Typ des Elements. Verwenden Sie jedoch eine Observable-Auflistung für die Bindung an ein Steuerelement für den XAML-Elemente, geeignet zu machen **"iinspectable"** als Typ des Elements.

Das Objekt wird zurückgegeben, als ein [ **IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), und das ist die Schnittstelle, die über den Sie das Steuerelement an das es gebunden ist) aufgerufen werden (oder Funktionen und Eigenschaften des zurückgegebenen Objekts.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Weitere Details und Codebeispiele, über das Binden Ihre Benutzers Benutzeroberfläche (UI) steuert, in eine Observable-Auflistung, finden Sie unter [ItemsControl für XAML, binden an einen C++ / WinRT-Auflistung](binding-collection.md).

### <a name="associative-collection-map"></a>Assoziativen Auflistung (Map)

Es gibt Versionen assoziativen Auflistung, der zwei Funktionen, denen wir uns angesehen haben, haben.

- Die [ **winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map) Vorlage für die Funktion gibt eine assoziative nicht-Observable-Auflistung als ein [ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- Die [ **winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) Vorlage für die Funktion gibt eine assoziative Observable-Auflistung als ein [ **IObservableMap** ](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Sie können optional auch diese Auflistungen mit Daten vorzubereiten, durch die Übergabe an die Funktion eine *Rvalue* des Typs **Std:: Map** oder **Std:: unordered_map**.

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>Singlethread-

Die "Singlethread" in den Namen dieser Funktionen gibt an, dass sie nicht, dass eine der Parallelität angeben&mdash;heißt, sie sind nicht threadsicher. Die Erwähnung von Threads ist unabhängig von Apartments, da die von diesen Funktionen zurückgegebenen Objekte aller agile sind (finden Sie unter [Agile Objekte in C++ / WinRT](agile-objects.md)). Es ist einfach, dass die Objekte Singlethread sind. Und das ist völlig geeignet, wenn Sie nur die Daten auf eine Weise, oder die andere über die anwendungsbinärdateischnittstelle (ABI) übergeben möchten.

## <a name="base-classes-for-collections"></a>Basisklassen für Sammlungen

Wenn Sie die vollständige Flexibilität erhalten Sie, Sie möchten eine eigene benutzerdefinierte Auflistung zu implementieren, sollten Sie vermeiden, die die harte Art. Beispielsweise sieht wie eine benutzerdefinierte Vektoransicht aussehen würde *ohne Unterstützung von C++ / WinRT Basisklassen*.

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

Stattdessen ist es viel einfacher, leiten Sie Ihre benutzerdefinierten Vektoransicht aus der [ **winrt::vector_view_base** ](/uwp/cpp-ref-for-winrt/vector-view-base) Struct-Vorlage, und implementieren Sie einfach die **Get_container** -Funktion Machen Sie der Container, die Ihre Daten verfügbar.

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

Der Container, die vom **Get_container** müssen Bereitstellen der **beginnen** und **End** Schnittstelle **winrt::vector_view_base** erwartet. Wie im Beispiel oben gezeigt **Std:: vector** bereitstellt. Sie können jedoch zurückgeben, Container, der den gleichen Vertrag, einschließlich Ihrer eigenen benutzerdefinierten Container erfüllt.

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

Hierbei handelt es sich um die Basis-Klassen C++ / WinRT bietet, damit Sie die benutzerdefinierte Sammlungen implementieren können.

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Finden Sie unter die oben aufgeführten Codebeispiele.

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>Wichtige APIs
* [ItemsControl.ItemsSource-Eigenschaft](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector-Schnittstelle](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector-Schnittstelle](/uwp/api/windows.foundation.collections.ivector_t_)
* [Vorlage für WinRT::map_base-Struktur](/uwp/cpp-ref-for-winrt/map-base)
* [Vorlage für WinRT::map_view_base-Struktur](/uwp/cpp-ref-for-winrt/map-view-base)
* [Vorlage für WinRT::observable_map_base-Struktur](/uwp/cpp-ref-for-winrt/observable-map-base)
* [Vorlage für WinRT::observable_vector_base-Struktur](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [Vorlage für WinRT::single_threaded_observable_map-Funktion](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [Vorlage für WinRT::single_threaded_map-Funktion](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [Vorlage für WinRT::single_threaded_observable_vector-Funktion](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [Vorlage für WinRT::single_threaded_vector-Funktion](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [Vorlage für WinRT::vector_base-Struktur](/uwp/cpp-ref-for-winrt/vector-base)
* [Vorlage für WinRT::vector_view_base-Struktur](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Verwandte Themen
* [Wertekategorien und zugehörige Verweise](cpp-value-categories.md)
* [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](binding-collection.md)
