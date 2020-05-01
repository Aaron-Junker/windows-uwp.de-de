---
description: C++/WinRT verfügt über Funktionen und Basisklassen, mit denen Sie beim Implementieren bzw. Übergeben von Sammlungen viel Zeit sparen können.
title: Sammlungen mit C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Sammlung
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4f1b15ec377b030a467dded634abe3fdde717896
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "68270152"
---
# <a name="collections-with-cwinrt"></a>Sammlungen mit C++/WinRT

Intern verfügt eine Windows-Runtime-Sammlung über viele komplizierte bewegliche Teile. Wenn du aber ein Sammlungsobjekt an eine Windows-Runtime-Funktion übergeben oder deine eigenen Sammlungseigenschaften und -typen implementieren möchtest, kannst du zur Unterstützung die in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) enthaltenen Funktionen und Basisklassen verwenden. Mit diesen Features wird für dich die Komplexität verringert, und du sparst viel Zeit und Aufwand.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) ist die Windows-Runtime-Schnittstelle, die von Elementsammlungen mit zufälligem Zugriff implementiert wird. Wenn du **IVector** selbst implementieren würdest, müsstest du auch [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_) und [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_) implementieren. Auch wenn du *unbedingt* einen benutzerdefinierten Sammlungstyp benötigst, bedeutet dies sehr viel Arbeit. Wenn du aber über Daten in einem **std::vector**-Element (oder **std::map** oder **std::unordered_map**) verfügst und diese lediglich an eine Windows-Runtime-API übergeben möchtest, solltest du diesen Aufwand nach Möglichkeit vermeiden. *Dies ist möglich*, weil du in C++/WinRT Unterstützung bei der effizienten Erstellung von Sammlungen ohne größeren Aufwand erhältst.

Weitere Informationen findest du auch unter [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](binding-collection.md).

## <a name="helper-functions-for-collections"></a>Hilfsfunktionen für Sammlungen

### <a name="general-purpose-collection-empty"></a>Leere universelle Auflistung

In diesem Abschnitt wird die Erstellung einer Sammlung beschrieben, die anfänglich leer ist und *nach* der Erstellung aufgefüllt wird.

Zum Abrufen eines neuen Objekts mit einem Typ, bei dem eine universelle Sammlung implementiert wird, kannst du die Funktionsvorlage [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) aufrufen. Das Objekt wird als [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) zurückgegeben. Dies ist die Schnittstelle, über die du die Funktionen und Eigenschaften des zurückgegebenen Objekts aufrufst.

Falls du die folgenden Codebeispiele kopieren und direkt in die Hauptquellcodedatei eines Projekts vom Typ **Windows-Konsolenanwendung (C++/WinRT)** einfügen möchtest, musst du in den Projekteigenschaften zuerst die Option **Vorkompilierte Header nicht verwenden** festlegen.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

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

Wie im obigen Codebeispiel zu sehen ist, kannst du nach dem Erstellen der Sammlung Elemente anfügen, diese durchlaufen und das Objekt im Allgemeinen so wie alle Windows-Runtime-Sammlungsobjekte behandeln, die du ggf. von einer API erhalten hast. Falls du eine unveränderliche Ansicht der Sammlung benötigst, kannst du wie gezeigt [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) aufrufen. Das obige Muster – zum Erstellen und Nutzen einer Sammlung – ist für einfache Szenarien geeignet, in denen du Daten an eine API übergeben oder von der API erhalten möchtest. Du kannst überall dort ein **IVector**- oder **IVectorView**-Element übergeben, wo ein [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)-Element erwartet wird.

Im obigen Codebeispiel wird mit dem Aufruf von **winrt::init_apartment** der Thread in der Windows-Runtime initialisiert – standardmäßig in einem Multithread-Apartment. Der Aufruf initialisiert darüber hinaus auch COM.

### <a name="general-purpose-collection-primed-from-data"></a>Universelle Sammlung mit Datenauffüllung

In diesem Abschnitt wird der Fall beschrieben, in dem du eine Sammlung erstellen und gleichzeitig auffüllen möchtest.

Du kannst den Mehraufwand für das Aufrufen von **Append** aus dem vorherigen Codebeispiel vermeiden. Unter Umständen verfügst du bereits über die Quelldaten, oder du möchtest die Quelldaten vor dem Erstellen des Windows-Runtime-Sammlungsobjekts auffüllen. Hierzu gehst du wie folgt vor.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Du kannst ein temporäres Objekt mit deinen Daten an **winrt::single_threaded_vector** übergeben (wie oben mit `coll1`). Oder du kannst ein **std::vector**-Element in die Funktion verschieben (unter der Annahme, dass du darauf nicht mehr zugreifst). In beiden Fällen übergibst du ein *rvalue*-Element in die Funktion. So kann der Compiler effizient arbeiten und das Kopieren der Daten vermeiden. Weitere Informationen zu R-Werten (*rvalues*) findest du unter [Value categories, and references to them](cpp-value-categories.md) (Wertekategorien und entsprechende Verweise).

Wenn du ein XAML-Elementsteuerelement an deine Sammlung binden möchtest, ist dies möglich. Achte hierbei aber auf Folgendes: Für das richtige Festlegen der [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)-Eigenschaft muss du sie auf einen Wert vom Typ **IVector** von **IInspectable** festlegen (oder eines Interoperabilitätstyps wie [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)).

Hier ist ein Codebeispiel angegeben, bei dem eine Sammlung mit einem Typ erstellt wird, der für das Binden geeignet ist. Anschließend wird ein Element angefügt. Du findest den Kontext für dieses Codebeispiel unter [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](binding-collection.md).

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Du kannst eine Windows-Runtime-Sammlung aus Daten erstellen und diese so darstellen, dass alles für die Übergabe an eine API bereit ist – ohne Kopiervorgänge.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

In den obigen Beispielen *kann* die von uns erstellte Sammlung an ein XAML-Elementsteuerelement gebunden werden, aber die Sammlung ist nicht „beobachtbar“ (observable).

### <a name="observable-collection"></a>Beobachtbare Sammlung

Rufe zum Abrufen eines neuen Objekts mit einem Typ, bei dem eine *beobachtbare* Sammlung implementiert wird, die Funktionsvorlage [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) mit einem beliebigen Elementtyp auf. Verwende aber **IInspectable** als Elementtyp, um eine beobachtbare Sammlung für das Binden an ein XAML-Elementsteuerelement vorzubereiten.

Das Objekt wird als [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) zurückgegeben. Dies ist die Schnittstelle, über die du (bzw. das Steuerelement, an das sie gebunden ist) die Funktionen und Eigenschaften des zurückgegebenen Objekts aufrufst.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Weitere Informationen und Codebeispiele zum Binden deiner Steuerelemente der Benutzeroberfläche an eine beobachtbare Sammlung findest du unter [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](binding-collection.md).

### <a name="associative-collection-map"></a>Assoziative Sammlung (Karte)

Es sind assoziative Sammlungsversionen der beiden Funktionen vorhanden, die wir uns angesehen haben.

- Die Funktionsvorlage [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) gibt eine nicht beobachtbare assoziative Sammlung als [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_) zurück.
- Die Funktionsvorlage [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) gibt eine beobachtbare assoziative Sammlung als [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_) zurück.

Du kannst diese Sammlungen optional mit Daten füllen, indem du an die Funktion einen *rvalue* vom Typ **std::map** oder **std::unordered_map** übergibst.

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

### <a name="single-threaded"></a>Singlethread

Die „Singlethread“-Angabe in den Namen dieser Funktionen weist darauf hin, dass sie keine Parallelität ermöglichen. Sie sind also nicht „threadsicher“. Die Erwähnung von Threads bezieht sich nicht auf Apartments, da die von diesen Funktionen zurückgegebenen Objekte alle „agil“ sind (siehe [Agile Objekte in C++/WinRT](agile-objects.md)). Es bedeutet lediglich, dass es sich um Singlethread-Objekte handelt. Dies ist auch völlig ausreichend, wenn du Daten in der einen oder der anderen Richtung über die ABI (Application Binary Interface) übergeben möchtest.

## <a name="base-classes-for-collections"></a>Basisklassen für Sammlungen

Wenn du zur Erzielung vollständiger Flexibilität deine eigene benutzerdefinierte Sammlung implementieren möchtest, ist es ratsam, hierfür nicht die schwierige Vorgehensweise zu wählen. Eine Ansicht eines benutzerdefinierten Vektors würde beispielsweise wie folgt aussehen, *wenn keine Basisklassen von C++/WinRT verwendet werden*.

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

Stattdessen ist es viel einfacher, die Ansicht eines benutzerdefinierten Vektors aus der Strukturvorlage [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) abzuleiten und nur die Funktion **get_container** zu implementieren, um den Container mit deinen Daten verfügbar zu machen.

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

Der von **get_container** zurückgegebene Container muss die Schnittstellen **begin** und **end** bereitstellen, die von **winrt::vector_view_base** erwartet werden. Dies wird von **std::vector** bereitgestellt, wie im obigen Beispiel zu sehen ist. Du kannst aber alle Container zurückgeben, die denselben Vertrag erfüllen, z. B. auch deinen eigenen benutzerdefinierten Container.

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

Dies sind die Basisklassen, die von C++/WinRT bereitgestellt werden, um dich bei der Implementierung von benutzerdefinierten Sammlungen zu unterstützen.

### <a name="winrtvector_view_base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Siehe hierzu auch die obigen Codebeispiele.

### <a name="winrtvector_base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### <a name="winrtobservable_vector_base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### <a name="winrtmap_view_base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### <a name="winrtmap_base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### <a name="winrtobservable_map_base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [Schnittstelle „IObservableVector“](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Schnittstelle „IVector“](/uwp/api/windows.foundation.collections.ivector_t_)
* [Strukturvorlage „winrt::map_base“](/uwp/cpp-ref-for-winrt/map-base)
* [Strukturvorlage „winrt::map_view_base“](/uwp/cpp-ref-for-winrt/map-view-base)
* [Strukturvorlage „winrt::observable_map_base“](/uwp/cpp-ref-for-winrt/observable-map-base)
* [Strukturvorlage „winrt::observable_vector_base“](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [Funktionsvorlage „winrt::single_threaded_observable_map“](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [Funktionsvorlage „winrt::single_threaded_map“](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [Funktionsvorlage „winrt::single_threaded_observable_vector“](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [Funktionsvorlage „winrt::single_threaded_vector“](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [Strukturvorlage „winrt::vector_base“](/uwp/cpp-ref-for-winrt/vector-base)
* [Strukturvorlage „winrt::vector_view_base“](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Verwandte Themen
* [Wertekategorien und zugehörige Verweise](cpp-value-categories.md)
* [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](binding-collection.md)
