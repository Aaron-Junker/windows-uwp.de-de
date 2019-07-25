---
description: Eine Sammlung, die effektiv an ein XAML-Elementsteuerelement gebunden werden kann, wird als *Observable*-Sammlung bezeichnet. Dieses Thema zeigt, wie man eine Observable-Sammlung implementiert und nutzt und wie man ein XAML-Elementsteuerelement daran bindet.
title: 'XAML-Elementsteuerelemente: Binden an eine C++/WinRT-Sammlung'
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, XAML, Steuerelement, binden, Sammlung
ms.localizationpriority: medium
ms.openlocfilehash: 258ec5e0690753c8ad9c3b0648867666397039a5
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270174"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML-Elementsteuerelemente: Binden an eine C++/WinRT-Sammlung

Eine Sammlung, die effektiv an ein XAML-Elementsteuerelement gebunden werden kann, wird als *Observable*-Sammlung bezeichnet. Dieses Konzept basiert auf dem Softwareentwurfsmuster, das als *Beobachter-Muster* bekannt ist. In diesem Thema erfährst du, wie du beobachtbare Sammlungen in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) implementierst und XAML-Elementsteuerelemente an sie bindest.

Zur besseren Nachvollziehbarkeit dieses Themas empfiehlt es sich, zuerst das unter [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](binding-property.md) beschriebene Projekt zu erstellen. In diesem Thema wird dem Projekt weiterer Code hinzugefügt, und es werden weitere Konzepte erläutert.

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe im Zusammenhang mit der Nutzung und Erstellung von Laufzeitklassen mit C++/WinRT findest du unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) sowie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>Was bedeutet *beobachtbar* für eine Sammlung?
Wenn eine Laufzeitklasse, die eine Sammlung darstellt, das Ereignis [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) auslöst, sobald ihr ein Element hinzugefügt oder ein Element daraus entfernt wird, ist die Laufzeitklasse eine beobachtbare Sammlung. Ein XAML-Elementsteuerelement kann an diese Ereignisse gebunden werden und sie behandeln, indem es die aktualisierte Sammlung abruft und sich anschließend selbst aktualisiert, um die aktuellen Elemente anzuzeigen.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Hinzufügen einer Sammlung vom Typ **BookSkus** zu **BookstoreViewModel**

In [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](binding-property.md) haben wir unserem Hauptansichtsmodell eine Eigenschaft vom Typ **BookSku** hinzugefügt. In diesem Schritt verwenden wir die Factoryfunktionsvorlage [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector), um auf der Grundlage des gleichen Ansichtsmodells eine beobachtbare Sammlung von **BookSku** zu implementieren.

Deklariere eine neue Eigenschaft in `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
}
...
```

> [!NOTE]
> Wie in der obigen MIDL 3.0-Auflistung zu sehen, handelt es sich beim Typ der Eigenschaft **BookSkus** um [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) von **BookSku**. Im nächsten Abschnitt dieses Themas binden wir die Elementquelle eines Listenfelds ([**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) an **BookSkus**. Ein Listenfeld ist ein Elementsteuerelement. Die Eigenschaft [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) muss daher auf einen Wert vom Typ **IObservableVector** oder **IVector** festgelegt werden – oder auf einen Interoperabilitätstyp wie [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

> [!WARNING]
> Der in diesem Thema gezeigt Code gilt für C++/WinRT, Version 2.0.190530.8 und höher. Wenn Sie eine frühere Version verwenden, müssen einige geringe Anpassungen am dargestellten Code vornehmen. Ändern Sie in der obigen MIDL 3.0-Auflistung die Eigenschaft **BookSkus** in [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) von [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Verwenden Sie dann **IInspectable** (anstelle von **BookSku**) auch in Ihrer Implementierung.

Speichere, und führe den Buildvorgang aus. Kopiere die Accessor-Stubs aus `BookstoreViewModel.h` und `BookstoreViewModel.cpp` in den Ordner `\Bookstore\Bookstore\Generated Files\sources`. (Ausführlichere Informationen findest du im vorherigen Thema [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](binding-property.md).) Implementiere diese Accessor-Stubs wie folgt:

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>Binden eines Listenfelds an die Eigenschaft **BookSkus**
Öffne `MainPage.xaml`. Darin befindet sich das XAML-Markup für unsere UI-Hauptseite. Füge das folgende Markup innerhalb des gleichen **StackPanel**-Elements hinzu wie die Schaltfläche (**Button**).

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

Füge in `MainPage.cpp` dem Ereignishandler für **Click** eine Codezeile hinzu, um am Ende der Sammlung ein Buch hinzuzufügen.

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

Erstelle nun das Projekt, und führe es aus. Klicke auf die Schaltfläche, um den Ereignishandler für **Click** auszuführen. Wir haben gesehen, dass die Implementierung von **Append** ein Ereignis auslöst, um die Benutzeroberfläche darauf hinzuweisen, dass sich die Sammlung geändert hat, und dass das Listenfeld (**ListBox**) die Sammlung erneut abfragt, um ihren eigenen Wert vom Typ **Items** zu aktualisieren. Genau wie zuvor ändert sich der Titel eines der Bücher, und diese Titeländerung wird sowohl auf der Schaltfläche als auch im Listenfeld angezeigt.

## <a name="important-apis"></a>Wichtige APIs
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Funktionsvorlage „winrt::make“](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Erstellen von APIs mit C++/WinRT](author-apis.md)
