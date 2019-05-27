---
description: Eine Collection, die effektiv an ein XAML-Items-Steuerelement gebunden werden kann, wird als *Observable*-Collection bezeichnet. Dieses Thema zeigt, wie man eine Observable-Collection implementiert und nutzt und wie man ein XAML-Items-Steuerelement daran bindet.
title: XAML-Items-Steuerelemente; Binden an eine C++/WinRT-Collection
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, XAML, steuerelement, binden, collection
ms.localizationpriority: medium
ms.openlocfilehash: 7669c6536f28d5f979567f5b433dbf614800bec3
ms.sourcegitcommit: d23dab1533893b7fe0f01ca6eb273edfac4705e6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/15/2019
ms.locfileid: "65627678"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML-Items-Steuerelemente; Binden an eine C++/WinRT-Collection

Eine Collection, die effektiv an ein XAML-Items-Steuerelement gebunden werden kann, wird als *Observable*-Collection bezeichnet. Dieses Konzept basiert auf dem Software-Design-Muster, das als *Observer-Pattern* bekannt ist. In diesem Thema wird gezeigt, wie sichtbare Auflistungen im implementiert [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), und Binden von XAML-ItemsControl darauf.

Wenn Sie zusammen mit diesem Thema folgen möchten, empfehlen wir, dass Sie zuerst das Projekt erstellen, das beschrieben ist [XAML-Steuerelemente, binden an eine C++"/ WinRT"-Eigenschaft](binding-property.md). In diesem Thema wird das Projekt mehr Code hinzugefügt, und der in diesem Thema erläuterten Konzepte hinzugefügt.

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe, die Ihr Verständnis für die Verwendung von Laufzeitklassen mit C++/WinRT unterstützen, finden Sie unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) und [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>Was bedeutet *observable* für eine Collection?
Wenn eine Laufzeitklasse, die eine Collection repräsentiert, das Ereignis [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) auslöst, wenn ein Element hinzugefügt oder entfernt wird, dann ist die Laufzeitklasse eine Observable-Collection. Ein XAML-Items-Steuerelement kann sich an diese Ereignisse binden und sie verarbeiten, indem es die aktualisierte Collection abruft und sich dann aktualisiert, um die aktuellen Elemente anzuzeigen.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++WinRT Visual Studio-Erweiterung (VSIX) und das NuGet-Paket (die zusammen bieten die Projektvorlage und Buildunterstützung) finden Sie unter [Visual Studio-Unterstützung für C++"/ WinRT"](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Hinzufügen einer **BookSkus**-Collection zu **BookstoreViewModel**

In [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md) haben wir eine Eigenschaft vom Typ **BookSku** zu unserem Hauptansichtsmodell hinzugefügt. In diesem Schritt verwenden wir die [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) Factory-Funktionsvorlage, um die beispielprojektdateien implementieren eine Auflistung von **BookSku** auf der gleiche Ansichtsmodell.

Deklarieren Sie eine neue Eigenschaft in `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> In der obigen MIDL-3.0-Auflistung, beachten Sie, dass der Typ des der **BookSkus** Eigenschaft [ **IObservableVector** ](/uwp/api/windows.foundation.collections.ivector_t_) von [ **"iinspectable"** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Im nächsten Abschnitt dieses Themas, wir werden Bindung werden die Elemente Quelle ein [ **ListBox** ](/uwp/api/windows.ui.xaml.controls.listbox) zu **BookSkus**. Ein Listenfeld, das ist ein ItemsControl-Element, und die ordnungsgemäße Festlegung der [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) -Eigenschaft müssen Sie es auf einen Wert vom Typ festgelegt **IObservableVector** (oder **IVector**) der **"iinspectable"**, oder eines Typs Interoperabilität, z. B. [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

Speichern und erstellen Sie das Projekt. Kopieren Sie die Accessor-Stubs aus `BookstoreViewModel.h` und `BookstoreViewModel.cpp` in die `\Bookstore\Bookstore\Generated Files\sources` Ordner (Weitere Informationen finden Sie im vorherige Thema [XAML-Steuerelemente, binden an eine C++"/ WinRT"-Eigenschaft](binding-property.md)). Implementieren Sie diese Stubs Accessor wie folgt.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>Binden einer Listbox an die **BookSkus**-Eigenschaft
Öffnen Sie `MainPage.xaml` mit dem XAML-Markup für unsere UI-Hauptseite. Fügen Sie den folgende Markup-Code in dem **StackPanel** hinzu, indem sich der **Button** befindet.

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

In `MainPage.cpp` fügen Sie dem **Click**-Ereignis-Handler eine Zeile Code hinzu, um ein Buch zur Collection hinzuzufügen.

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

Erstellen Sie nun das Projekt und führen Sie es aus. Klicken Sie auf die Schaltfläche, um den **Click**-Ereignis-Handler auszuführen. Wir haben gesehen, dass die Implementierung von **Append** ein Ereignis auslöst, um die Benutzeroberfläche wissen zu lassen, dass sich die Collection geändert hat. Die **ListBox** fragt die Collection erneut ab, um ihren eigenen **Items**-Wert zu aktualisieren. Nach wie vor ändert sich der Titel eines der Bücher, und diese Titeländerung wird sowohl auf der Schaltfläche als auch in der Listbox angezeigt.

## <a name="important-apis"></a>Wichtige APIs
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Vorlage für WinRT::Make-Funktion](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Verwandte Themen
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Erstellen von APIs mit C++/WinRT](author-apis.md)
