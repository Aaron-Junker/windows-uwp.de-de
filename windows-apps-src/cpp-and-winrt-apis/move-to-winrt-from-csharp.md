---
description: In diesem Thema wird gezeigt, wie Sie C#-Code zum äquivalenten Code in C++/WinRT portieren.
title: Umstellen von C# auf C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, portieren, migrieren, C#
ms.localizationpriority: medium
ms.openlocfilehash: a63d38db613ebe6425a05ed20563405242ffd441
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328859"
---
# <a name="move-to-cwinrt-from-c"></a>Umstellen von C# auf C++/WinRT

In diesem Thema wird veranschaulicht, wie du den Code in einem [C#](/visualstudio/get-started/csharp)-Projekt in den entsprechenden Code in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) portierst.

## <a name="register-an-event-handler"></a>Registrieren eines Ereignishandlers

Du kannst einen Ereignishandler in XAML-Markup registrieren.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

In C# kann deine **OpenButton_Click**-Methode privat sein, und XAML kann sie trotzdem mit dem [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis verbinden, das von *OpenButton* ausgelöst wird.

In C++/WinRT muss deine **OpenButton_Click**-Methode in deinem [Implementierungstyp](/windows/uwp/cpp-and-winrt-apis/author-apis) öffentlich sein.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Alternativ dazu kannst du die registrierende Klasse als Friend deiner Implementierungsklasse und **OpenButton_Click** als privat festlegen.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="making-a-class-available-to-the-binding-markup-extension"></a>Verfügbarmachen einer Klasse für die {Binding}-Markuperweiterung

Wenn du die {Binding}-Markuperweiterung zum Binden von Daten an deinen Datentyp verwenden möchtest, findest du Informationen dazu unter [Mit {Binding} deklariertes Bindungsobjekt](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding).

## <a name="making-a-data-source-available-to-xaml-markup"></a>Verfügbarmachen einer Datenquelle für XAML-Markup

In C++/WinRT, Version 2.0.190530.8 und höher erstellt [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) einen Observable-Vektor, der sowohl **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** als auch **IObservableVector\<IInspectable\>** unterstützt.

Du kannst deine **Midl-Datei (.idl)** folgendermaßen erstellen (siehe auch [Einbeziehen von Laufzeitklassen in Midl-Dateien (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Die Implementierung sieht dann so aus.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

Weitere Informationen findest du unter [XAML-Elementsteuerelemente: Binden an eine C++/WinRT-Sammlung](/windows/uwp/cpp-and-winrt-apis/binding-collection).

## <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Verfügbarmachen einer Datenquelle für XAML-Markup (vor C++/WinRT 2.0.190530.8)

Die XAML-Datenbindung erfordert, dass eine Elementquelle **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** sowie eine der folgenden Schnittstellenkombinationen implementiert.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** und **INotifyCollectionChanged**
- **IBindableVector** und **IBindableObservableVector**
- **IBindableVector** allein (antwortet nicht auf Änderungen)
- **IVector\<IInspectable\>**
- **IBindableIterable** (Iteration und Speicherung von Elementen erfolgt in einer privaten Sammlung)

Eine generische Schnittstelle wie **IVector\<T\>** wird zur Laufzeit nicht erkannt. Jede **IVector\<T\>** -Schnittstelle weist eine andere IID (Schnittstellenbezeichner) auf, die eine Funktion von **T** ist. Jeder Entwickler kann den **T**-Satz nach Belieben erweitern, daher kennt der XAML-Bindungscode nie den vollständigen Satz, der abgefragt werden muss. Diese Einschränkung ist kein Problem für C#, weil jedes CLR-Objekt, das **IEnumerable\<T\>** implementiert, automatisch auch **IEnumerable** implementiert. Auf ABI-Ebene bedeutet dies, dass jedes Objekt, das **IObservableVector\<T\>** implementiert, automatisch auch **IObservableVector\<IInspectable\>** implementiert.

C++/WinRT bietet diese Garantie nicht. Wenn eine C++/WinRT-Laufzeitklasse **IObservableVector\<T\>** implementiert, kann nicht davon ausgegangen werden, dass auch eine Implementierung von **IObservableVector\<IInspectable\>** bereitgestellt wird.

Daher muss das vorherige Beispiel folgendermaßen aussehen.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

Die Implementierung sieht dann so aus.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

Wenn du auf Objekte in *m_bookSkus* zugreifen musst, musst du einen QI-Vorgang zurück zu **Bookstore::BookSku** durchführen.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

## <a name="tostring"></a>ToString()

C#-Typen stellen die [Object.ToString](/dotnet/api/system.object.tostring)-Methode bereit.

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

In C++/WinRT ist dies nicht direkt verfügbar, aber du kannst Alternativen nutzen.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT unterstützt auch [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) für eine begrenzte Anzahl von Typen. Du musst Überladungen für alle zusätzlichen Typen hinzufügen, für die du eine Stringification durchführen möchtest.

| Sprache | Stringification von „int“ | Stringification von „enum“ |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

Bei der Stringification eines enum-Typs musst du die Implementierung von **winrt::to_hstring** bereitstellen.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

Diese Stringifications werden häufig implizit von der Datenbindung verwendet.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Diese Bindungen führen **winrt::to_hstring** der gebundenen Eigenschaft durch. Im zweiten Beispiel (**StatusEnum**) musst du eine eigene Überladung von **winrt::to_hstring** bereitstellen, andernfalls erhältst du einen Compilerfehler.

## <a name="string-building"></a>Erstellung von string-Elementen

Für die Erstellung von string-Elementen verfügt C# über einen integrierten [**StringBuilder**](/dotnet/api/system.text.stringbuilder)-Typ.

| | C# | C++/WinRT |
|-|-|-|
| Builder-Klasse | `StringBuilder builder;` | `std::wstringstream stream;` |
| Anfügen eines string-Elements, NULL-Werte beibehalten | `builder.Append(s);` | `stream << std::wstring_view{ s };` |
| Ergebnisse extrahieren | `s = builder.ToString();` | `ws = stream.str();` |

## <a name="boxing-and-unboxing"></a>Boxing und Unboxing

C# führt automatisch ein Boxing von Skalaren zu Objekten durch. In C++/WinRT musst du die Funktion [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) explizit aufrufen. In beiden Sprachen musst du ein Unboxing explizit angeben. Siehe [Boxing und Unboxing mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

In der unten stehenden Tabelle werden folgende Definitionen verwendet.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| Vorgang | C# | C++/WinRT|
|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX und C# lösen Ausnahmen aus, wenn du versuchst, ein Unboxing eines NULL-Zeigers auf einen Werttyp auszuführen. C++/WinRT betrachtet dies als Programmierfehler und stürzt ab. Verwende in C++/WinRT die Funktion [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or), wenn ein Objekt nicht den von dir angenommenen Typ aufweist.

| Szenario | C# | C++/WinRT|
|-|-|-|
| Unboxing eines bekannten integer-Elements |`i = (int)o;` | `i = unbox_value<int>(o);` |
| Wenn „o“ NULL ist | `System.NullReferenceException` | Absturz |
| Wenn „o“ kein geboxter int-Wert ist | `System.InvalidCastException` | Absturz |
| Unboxing für „int“, Fallback bei NULL; Absturz in allen anderen Fällen | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Unboxing für „int“ (falls möglich); Fallback in allen anderen Fällen | `var box = o as int?;`<br>`i = box != null ? box.Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Boxing und Unboxing von string-Elementen

Ein string-Element ist in einigen Fällen ein Werttyp, in anderen Fällen ein Verweistyp. C# und C++/WinRT behandeln string-Elemente unterschiedlich.

Der ABI-Typ [**HSTRING**](/windows/win32/winrt/hstring) ist ein Zeiger auf ein als Verweis gezähltes string-Element. Er wird jedoch nicht von [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) abgeleitet, ist also technisch gesehen kein *Objekt*. Darüber hinaus stellt ein **HSTRING** mit Wert NULL ein leeres string-Element dar. Das Boxing von Elementen, die nicht von **IInspectable** abgeleitet werden, erfolgt über den Einschluss in [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_), und die Windows-Runtime stellt eine Standardimplementierung in Form des [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue)-Objekts bereit (benutzerdefinierte Typen werden als [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype) gemeldet).

C# stellt ein string-Element der Windows-Runtime als Referenztyp dar, C++/WinRT dagegen als Werttyp. Das bedeutet, dass eine geboxte NULL-Zeichenfolge unterschiedliche Darstellungen aufweisen kann, je nachdem, wie du dorthin gelangt bist.

| Vorgang | C# | C++/WinRT|
|-|-|-|
| Kategorie des string-Typs | Verweistyp | Werttyp |
| **HSTRING** mit NULL-Wert wird dargestellt als | `""` | `hstring{ nullptr }` |
| Sind NULL und `""` identisch? | Nein | Ja |
| Gültigkeit von NULL | `s = null;`<br>`s.Length` löst **NullReferenceException** aus | `s = nullptr;`<br>`s.size() == 0` (gültig) |
| Boxing eines string-Elements | `o = s;` | `o = box_value(s);` |
| Wenn `s` gleich `null` | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{nullptr});`<br>`o != nullptr` |
| Wenn `s` gleich `""` | `o = "";`<br>`o != null;` | `o = box_value(hstring{L""});`<br>`o != nullptr;` |
| Boxing eines string-Elements unter Beibehaltung von NULL | `o = s;` | `o = s.empty() ? nullptr : box_value(s);` |
| Boxing eines string-Elements erzwingen | `o = PropertyValue.CreateString(s);` | `o = box_value(s);` |
| Unboxing eines bekannten string-Elements | `s = (string)o;` | `s = unbox_value<hstring>(o);` |
| Wenn `o` NULL ist | `s == null; // not equivalent to ""` | Absturz |
| Wenn `o` kein geboxter string-Wert ist | `System.InvalidCastException` | Absturz |
| Unboxing für „string“, Fallback bei NULL; Absturz in allen anderen Fällen | `s = o != null ? (string)o : fallback;` | `s = o ? unbox_value<hstring>(o) : fallback;` |
| Unboxing für „string“ (falls möglich); Fallback in allen anderen Fällen | `var s = o as string ?? fallback;` | `s = unbox_value_or<hstring>(o, fallback);` |

In den beiden oben genannten Fällen *Unboxing mit Fallback* ist es möglich, dass ein Boxing eines string-Elements mit NULL-Wert erzwungen wurde. In diesem Fall wird das Fallback nicht verwendet. Der Ergebniswert ist eine leere Zeichenfolge, denn diese war enthalten.

## <a name="derived-classes"></a>Abgeleitete Klassen

Um aus einer Laufzeitklasse abzuleiten, muss die Basisklasse *zusammensetzbar* sein. In C# sind keine besonderen Schritte erforderlich, um Klassen zusammensetzbar zu machen, in C++/WinRT schon. Du verwendest das [unsealed-Schlüsselwort](/uwp/midl-3/intro#base-classes), um anzugeben, dass die Klasse als Basisklasse verwendet werden kann.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

In der Implementierungsheaderklasse musst du die Basisklassen-Headerdatei einschließen, bevor du den automatisch generierten Header für die abgeleitete Klasse einschließt. Andernfalls erhältst du Fehler wie „Ungültige Verwendung dieses Typs als Ausdruck“.

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="consuming-objects-from-xaml-markup"></a>Verwenden von Objekten aus XAML-Markup

In einem C#-Projekt kannst du private Member und benannte Elemente aus XAML-Markup nutzen. In C++/WinRT dagegen müssen alle Entitäten, die über die XAML-[ **{x:Bind}-Markuperweiterung**](/windows/uwp/xaml-platform/x-bind-markup-extension) genutzt werden, in IDL öffentlich verfügbar gemacht werden.

Durch Binden an einen booleschen Typ wird in C# `true` oder `false` angezeigt, in C++/WinRT dagegen **Windows.Foundation.IReference`1\<Boolean\>** .

Weitere Informationen sowie Codebeispiele findest du unter [Verwenden von Objekten aus Markup](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

## <a name="important-apis"></a>Wichtige APIs
* [Funktionsvorlage „winrt::single_threaded_observable_vector“](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Verwandte Themen
* [C#-Tutorials](/visualstudio/get-started/csharp)
* [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Datenbindung im Detail](/windows/uwp/data-binding/data-binding-in-depth)
* [XAML-Elementsteuerelemente; Binden an eine C++/WinRT-Sammlung](/windows/uwp/cpp-and-winrt-apis/binding-collection)