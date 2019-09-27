---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Datenbindung im Detail
description: Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann.
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 69a72a3e0a3f34e847fb182d562ac4d87f1614a2
ms.sourcegitcommit: 046ab7dc03ba721d05bb452766d7460c96892858
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71318910"
---
# <a name="data-binding-in-depth"></a>Datenbindung im Detail

**Wichtige APIs**

-   [ **{x:Bind}-Markup Erweiterung**](../xaml-platform/x-bind-markup-extension.md)
-   [**Binding-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
-   [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)
-   [**INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)

> [!NOTE]
> In diesem Thema werden die Datenbindungsfeatures ausführlich beschrieben. Eine kurze und praktische Einführung finden Sie unter [Übersicht „Datenbindung“](data-binding-quickstart.md).

In diesem Thema wird die Datenbindung in universelle Windows-Plattform (UWP)-Anwendungen behandelt. Die hier erläuterten APIs befinden sich im [ **Windows. UI. XAML. Data** -Namespace](/uwp/api/windows.ui.xaml.data).

Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann. Mit der Datenbindung können Sie Datenaspekte von Benutzeroberflächenaspekten trennen, was zu einem einfacheren konzeptionellen Modell und besserer Lesbarkeit, Testbarkeit und Wartung Ihrer App führt.

Sie können die Datenbindung einfach verwenden, um nur Werte aus einer Datenquelle anzuzeigen, wenn die Benutzeroberfläche das erste Mal angezeigt wird, jedoch nicht auf Änderungen an diesen Werten reagieren. Dabei handelt es sich um einen als *einmalige*Bindung bezeichneten Bindungs Modus, der sich für einen Wert eignet, der sich während der Laufzeit nicht ändert. Wahlweise können Sie auch die Werte "beobachten" und die Benutzeroberfläche aktualisieren, wenn Sie sich ändern. Dieser Modus wird als unidirektional *bezeichnet und eignet sich gut*für schreibgeschützte Daten. Letztendlich können Sie die Daten sowohl feststellen als auch aktualisieren, damit die vom Benutzer in der Benutzeroberfläche vorgenommenen Änderungen automatisch in die Datenquelle übernommen werden. Dieser Modus wird als bidirektional *bezeichnet und eignet sich gut*für Daten mit Lese-/Schreibzugriff. Beispiele:

-   Sie können den einmaligen Modus verwenden, um ein [**Bild**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) an das Foto des aktuellen Benutzers zu binden.
-   Sie können den unidirektionalen Modus verwenden, um eine [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) an eine Sammlung von Echtzeitnachrichten Artikeln zu binden, die nach dem Zeitungs Abschnitt gruppiert sind.
-   Sie können den bidirektionalen Modus verwenden, um ein [**Textfeld**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) an den Namen eines Kunden in einem Formular zu binden.

Unabhängig vom Modus gibt es zwei Arten von Bindungen, die beide in der Regel in Benutzeroberflächen Markup deklariert werden. Sie können entweder die [{x:Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) oder die [{Binding}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) verwenden. Sie können sogar eine Mischung aus den beiden in derselben App verwenden – sogar im gleichen Benutzeroberflächenelement. {x:Bind} ist neu in Windows 10 und bietet eine bessere Leistung. Alle in diesem Thema beschriebenen Details gelten für beide Arten von Bindungen, es sei denn, es wird ausdrücklich auf eine Abweichung hingewiesen.

**Beispiel-apps, die {x:Bind} veranschaulichen**

-   [{x:Bind}-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=619989).
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper).
-   [Beispiel für XAML-Benutzeroberflächengrundlagen](https://go.microsoft.com/fwlink/p/?linkid=619992)

**Beispiel-apps, die {Binding} veranschaulichen**

-   Laden Sie die App [Bookstore1](https://go.microsoft.com/fwlink/?linkid=532950) herunter.
-   Laden Sie die App [Bookstore2](https://go.microsoft.com/fwlink/?linkid=532952) herunter.

## <a name="every-binding-involves-these-pieces"></a>Was zu jeder Bindung gehört

-   Eine *Bindungsquelle*: Dies ist die Quelle der Daten für die Bindung und kann eine Instanz einer Klasse mit Membern sein, deren Werte Sie in der Benutzeroberfläche anzeigen möchten.
-   Ein *Bindungsziel*: Dies ist eine [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) des [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) in Ihrer Benutzeroberfläche, die die Daten anzeigt.
-   Ein *Bindungsobjekt*: Dies ist der Teil, der Datenwerte von der Quelle an das Ziel sowie optional vom Ziel zurück an die Quelle überträgt. Das Bindungsobjekt wird zur XAML-Ladezeit von der [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)- oder [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)-Markuperweiterung erstellt.

In den folgenden Abschnitten betrachten wird die Bindungsquelle, das Bindungsziel und das Bindungsobjekt genauer. Außerdem verknüpfen wir die Abschnitte durch ein Beispiel, bei dem der Inhalt einer Schaltfläche an eine Zeichenfolgeneigenschaft mit dem Namen **NextButtonText** gebunden wird, die zur Klasse **HostViewModel** gehört.

### <a name="binding-source"></a>Bindungsquelle

Nachfolgend wird eine sehr einfache Implementierung einer Klasse gezeigt, die wir als Bindungsquelle verwenden könnten.

Wenn Sie [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)verwenden, fügen Sie dem Projekt neue Dateien für die **mittlere Datei (. idl)** hinzu, wie im C++folgenden Beispiel gezeigt. Ersetzen Sie den Inhalt dieser neuen Dateien durch den in der Auflistung gezeigten [Mittelwert 3,0](/uwp/midl-3/intro) -Code, erstellen Sie das Projekt `HostViewModel.h` , `.cpp`um und zu generieren, und fügen Sie dann den generierten Dateien Code hinzu, damit dieser der Auflistung entspricht. Weitere Informationen zu diesen generierten Dateien und zum Kopieren der Dateien in das Projekt finden Sie unter [XAML-Steuerelemente, binden C++an eine/WinRT-Eigenschaft](/windows/uwp/cpp-and-winrt-apis/binding-property).

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

Diese Implementierung der **HostViewModel**-Klasse und der zugehörigen **NextButtonText**-Eigenschaft ist nur für eine einmalige Bindung geeignet. Unidirektionale und bidirektionale Bindungen sind jedoch sehr häufig, und bei diesen Arten von Bindungen wird die Benutzeroberfläche als Reaktion auf Änderungen bei den Datenwerten der Bindungsquelle automatisch aktualisiert. Damit diese Bindungen ordnungsgemäß funktionieren, muss die Bindungsquelle für das Bindungsobjekt „feststellbar“ sein. Wenn wir in unserem Beispiel also eine unidirektionale oder bidirektionale Bindung an die **NextButtonText**-Eigenschaft anwenden möchten, müssen alle Änderungen, die zur Laufzeit am Wert dieser Eigenschaft vorgenommen werden, für das Bindungsobjekt feststellbar sein.

Eine Möglichkeit für die Umsetzung besteht darin, die Klasse, die die Bindungsquelle darstellt, von [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) abzuleiten und einen Datenwert über eine [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) verfügbar zu machen. Auf diese Weise wird ein [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) feststellbar. **FrameworkElements** sind gute Bindungsquellen, die sofort verwendet werden können.

Eine einfachere Möglichkeit, eine Klasse feststellbar zu machen – und eine Notwendigkeit für Klassen, die bereits über eine Basisklasse verfügen – besteht in der Implementierung von [**System.ComponentModel.INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN). Hierzu muss lediglich ein einzelnes Ereignis mit dem Namen **PropertyChanged** implementiert werden. Weiter unten finden Sie ein Beispiel für die Verwendung von **HostViewModel**.

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

Die **NextButtonText**-Eigenschaft ist jetzt feststellbar. Wenn Sie eine uni- oder bidirektionale Bindung an diese Eigenschaft erstellen (was weiter unten genauer gezeigt wird), abonniert das sich ergebende Bindungsobjekt das **PropertyChanged**-Ereignis. Wenn das Ereignis ausgelöst wird, erhält der Handler des Bindungsobjekts ein Argument mit dem Namen der Eigenschaft, die geändert wurde. Daher weiß das Bindungsobjekt, welchen Eigenschaftswert es erneut lesen muss.

Damit Sie das oben gezeigte Muster nicht mehrmals implementieren müssen, können Sie, wenn Sie verwenden C# , einfach von der Klasse **bindablebase** -Bass ableiten, die Sie im Quiz- [Spiel](https://github.com/microsoft/Windows-appsample-networkhelper) Beispiel (im Ordner "Allgemein") finden. Dies ist ein Beispiel hierfür.

```csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> Für C++/WinRT wird jede Lauf Zeit Klasse, die Sie in der Anwendung deklarieren, die von einer Basisklasse abgeleitet wird, als *Zusammensetz* Bare Klasse bezeichnet. Für zusammensetzbare Klassen gelten bestimmte Einschränkungen. Damit eine Anwendung die Tests des [Zertifizierungskits für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) besteht, das von Visual Studio sowie vom Microsoft Store zur Überprüfung von Übermittlungen verwendet wird, und erfolgreich in den Microsoft Store aufgenommen werden kann, muss eine zusammensetzbare Klasse letztendlich von einer Windows-Basisklasse abgeleitet sein. Das bedeutet, dass es sich am Stamm der Vererbungshierarchie um einen Klassentyp aus einem Windows.*-Namespace handeln muss. Wenn du eine Laufzeitklasse von einer Basisklasse ableiten musst, um beispielsweise eine Klasse vom Typ **BindableBase** zur Ableitung deiner Ansichtsmodelle zu implementieren, kannst du [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject) als Grundlage für die Ableitung verwenden.

Das Auslösen des **PropertyChanged**-Ereignisses mit dem Argument [**String.Empty**](https://docs.microsoft.com/dotnet/api/system.string.empty?redirectedfrom=MSDN) oder **null** gibt an, dass alle Eigenschaften, die keine Indexereigenschaften sind, für das Objekt erneut gelesen werden sollen. Sie können das-Ereignis aufweisen, um anzugeben, dass Indexer-Eigenschaften für das Objekt geändert wurden, indem\[\]Sie für bestimmte Indexer (wobei *Indexer* den Indexwert ist) oder den Wert "Item\["für alle Indexer. \]

Eine Bindungsquelle kann entweder als einzelnes Objekt behandelt werden, dessen Eigenschaften Daten enthalten, oder als Sammlung von Objekten. In C# und Visual Basic Code können Sie eine einmalige Bindung an ein Objekt durchführen, das [List (of T)](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1?redirectedfrom=MSDN) implementiert, um eine Sammlung anzuzeigen, die sich zur Laufzeit nicht ändert. Nehmen Sie für eine feststellbare Sammlung (es wird festgestellt, wann Elemente zur Sammlung hinzugefügt oder aus dieser entfernt werden) stattdessen eine unidirektionale Bindung an [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN) vor. In C++-Code können Sie sowohl für feststellbare als auch für nicht feststellbare Sammlungen eine Bindung an [**Vector&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.numerics.vector-1?redirectedfrom=MSDN) vornehmen. Wenn Sie eine Bindung an Ihre eigenen Sammlungsklassen vornehmen möchten, verwenden Sie die Anleitung in der folgenden Tabelle.

|Szenario|C# und VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|An ein Objekt binden|Es kann sich um jedes Objekt handeln.|Es kann sich um jedes Objekt handeln.|Das Objekt muss [**BindableAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) besitzen oder [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) implementieren.|
|Benachrichtigungen über Eigenschafts Änderungen von einem gebundenen Objekt erhalten.|Das Objekt muss " [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN)" implementieren.| Das Objekt muss " [**INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)" implementieren.|Das Objekt muss " [**INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)" implementieren.|
|An eine Sammlung binden| [**Liste (von T)** ](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1?redirectedfrom=MSDN)|[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) von [**iinspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)oder [**ibindableobservablevector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). Weitere Informationen finden [Sie unter XAML Items- C++Steuerelemente; binden an eine/WinRT](../cpp-and-winrt-apis/binding-collection.md) -Auflistung und [Sammlungen mit C++/WinRT](../cpp-and-winrt-apis/collections.md).| [**Vektor&lt;T&gt;** ](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class)|
|Erhalten von Sammlungs Änderungs Benachrichtigungen aus einer gebundenen Sammlung.|[**ObservableCollection (of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN)|[**Iobservablevector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) von [**iinspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable).|[**Iobservablevector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_)|
|Implementieren einer Sammlung, die Bindungen unterstützt|Erweitern von [**List(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1?redirectedfrom=MSDN) oder Implementieren von [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN), [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?redirectedfrom=MSDN)(Of [**Object**](https://docs.microsoft.com/dotnet/api/system.object?redirectedfrom=MSDN)), [**IEnumerable**](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable?redirectedfrom=MSDN) oder [**IEnumerable**](https://docs.microsoft.com/dotnet/api/system.collections.generic.ienumerable-1?redirectedfrom=MSDN)(Of **Object**). Die Bindung an generische **IList(Of T)** und **IEnumerable(Of T)** wird nicht unterstützt.|Implementieren Sie [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) von [**iinspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Weitere Informationen finden [Sie unter XAML Items- C++Steuerelemente; binden an eine/WinRT](../cpp-and-winrt-apis/binding-collection.md) -Auflistung und [Sammlungen mit C++/WinRT](../cpp-and-winrt-apis/collections.md).|Implementieren Sie [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector), [**IBindableIterable**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableIterable), [**IVector**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IVector_T_)&lt;[**Object**](https://docs.microsoft.com/dotnet/api/system.object?redirectedfrom=MSDN)^&gt;, [**IIterable**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IIterable_T_)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable)\*&gt; oder **IIterable**&lt;**IInspectable**\*&gt;. Die Bindung an generische **IVector&lt;T&gt;** und **IIterable&lt;T&gt;** wird nicht unterstützt.|
| Implementieren Sie eine Auflistung, die Sammlungs Änderungs Benachrichtigungen unterstützt. | Erweitern Sie [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN), oder implementieren Sie (nicht generische) [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN) und [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN).|Implementieren Sie [**iobservablevector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) von [**iinspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)oder [**ibindableobservablevector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).|Implementieren Sie [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector) und [**IBindableObservableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector).|
|Implementieren einer Sammlung, die inkrementelles Laden unterstützt|Erweitern Sie [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN), oder implementieren Sie (nicht generische) [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN) und [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN). Implementieren Sie zusätzlich [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading).|Implementieren Sie [**iobservablevector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) von [**iinspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)oder [**ibindableobservablevector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). Implementieren Sie zusätzlich [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading).|Implementieren Sie [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector), [**IBindableObservableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector) und [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading).|

Sie können mithilfe von inkrementellem Laden Listensteuerelemente an beliebig große Datenquellen binden und dennoch eine hohe Leistung erreichen. Zum Beispiel können Sie Listensteuerelemente an Ergebnisse von Bildabfragen in Bing binden, ohne alle Ergebnisse auf einmal laden zu müssen. Stattdessen laden Sie nur einige der Ergebnisse sofort und weitere Ergebnisse nach Bedarf. Um das inkrementelle Laden zu unterstützen, müssen Sie [**isupportincrementzuweisung**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading) für eine Datenquelle implementieren, die Sammlungs Änderungs Benachrichtigungen unterstützt. Wenn das Datenbindungsmodul weitere Daten anfordert, muss Ihre Datenquelle die entsprechenden Anforderungen senden, die Ergebnisse integrieren und anschließend die entsprechende Benachrichtigung senden, um die UI zu aktualisieren.

### <a name="binding-target"></a>Bindungsziel

In den beiden folgenden Beispielen ist die **Button. Content** -Eigenschaft das Bindungs Ziel, und ihr Wert wird auf eine Markup Erweiterung festgelegt, die das Bindungs Objekt deklariert. Es wird zuerst [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) angezeigt, dann [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension). Das Deklarieren von Bindungen im Markup wird häufig verwendet (es ist praktisch, lesbar und toolfreundlich). Sie können Markups jedoch auch vermeiden und eine Instanz der [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)-Klasse stattdessen auch bei Bedarf imperativ (programmgesteuert) erstellen.

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

Wenn Sie/WinRT oder C++Visual C++ Component Extensions (C++/CX) verwenden, müssen Sie das [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) -Attribut zu jeder Lauf Zeit Klasse hinzufügen, für die Sie die [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) -Markup Erweiterung mit verwenden möchten.

> [!IMPORTANT]
> Wenn Sie [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)verwenden, ist das [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) -Attribut verfügbar, wenn Sie die Windows SDK Version 10.0.17763.0 (Windows 10, Version 1809) oder höher installiert haben. Ohne dieses Attribut müssen Sie die [icustompropertyprovider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) -Schnittstelle und die [icustomproperty](/uwp/api/windows.ui.xaml.data.icustomproperty) -Schnittstelle implementieren, um die [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) -Markup Erweiterung verwenden zu können.

### <a name="binding-object-declared-using-xbind"></a>Mit {x:Bind} deklariertes Bindungsobjekt

Vor dem Erstellen des [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)-Markups müssen wir jedoch noch einen Schritt durchführen. Wir müssen die Bindungsquellenklasse aus der Klasse verfügbar machen, die die Markupseite darstellt. Dies wird durch Hinzufügen einer Eigenschaft (vom Typ " **hostviewmodel** " in diesem Fall) zur **MainPage** -Seiten Klasse hinzugefügt.

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

Wenn dies erledigt ist, können wir uns das Markup, das das Bindungsobjekt deklariert, genauer ansehen. Das unten aufgeführte Beispiel verwendet das gleiche **Button.Content**-Bindungsziel, das wir weiter oben im Abschnitt „Bindungsziel“ verwendet haben, und zeigt seine Bindung an die **HostViewModel.NextButtonText**-Eigenschaft.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Beachten Sie den Wert, den wir für **Path** angeben. Dieser Wert wird im Kontext der Seite selbst interpretiert, und in diesem Fall beginnt der Pfad mit dem Verweis auf die **ViewModel** -Eigenschaft, die wir soeben zur **MainPage** -Seite hinzugefügt haben. Diese Eigenschaft gibt eine **HostViewModel**-Instanz zurück, und so können wir damit wir dieses Objekt mit einem Punkt verknüpfen, um auf die **HostViewModel.NextButtonText**-Eigenschaft zuzugreifen. Außerdem geben wir **Mode** an, um die Standardeinstellung für einmaliges Binden [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) zu überschreiben.

Die [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path)-Eigenschaft unterstützt eine Vielzahl von Syntaxoptionen zum Binden geschachtelter Eigenschaften, angefügter Eigenschaften sowie von Ganzzahl- und Zeichenfolgenindexern. Weitere Informationen finden Sie unter [Property-path-Syntax](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax). Die Bindung an Zeichenfolgenindexer hat dieselbe Wirkung wie die Bindung an dynamische Eigenschaften, ohne [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) implementieren zu müssen. Informationen zu weiteren Einstellungen finden Sie unter [{x:Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension).

Um zu veranschaulichen, dass die **hostviewmodel. nextbuttontext** -Eigenschaft tatsächlich Observable ist, fügen Sie der Schaltfläche einen **Click** -Ereignishandler hinzu, und aktualisieren Sie den Wert von " **hostviewmodel. nextbuttontext**". Erstellen Sie, führen Sie aus, und klicken Sie auf die Schaltfläche, um den Wert der **Inhalts** Aktualisierung der Schaltfläche anzuzeigen.

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> Änderungen an [**TextBox. Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) werden an eine bidirektionale gebundene Quelle gesendet, wenn das [**Textfeld**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) den Fokus verliert, und nicht nach jedem Benutzer Tastatur Strich.

**DataTemplate und x:datatype**

Innerhalb einer [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) (ob Elementvorlage, Inhaltsvorlage oder Kopfzeilenvorlage) wird der Wert von **Path** nicht im Kontext der Seite, sondern des Datenobjekts interpretiert, das als Vorlage verwendet werden soll. Um {x:Bind} in einer Datenvorlage so zu verwenden, dass die Bindungen zum Zeitpunkt des Kompilierens überprüft werden können(und effizienter Code für sie generiert wird), muss **DataTemplate** den Typ des Datenobjekts mithilfe von **x:DataType** deklarieren. Das unten aufgeführte Beispiel kann als **ItemTemplate** eines Elementsteuerelements verwendet werden, das an eine Sammlung von **SampleDataGroup**-Objekten gebunden ist.

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Schwach typisierte Objekte in Ihrem Pfad**

Angenommen, Sie verfügen über einen Typ mit dem Namen „SampleDataGroup“, der eine Zeichenfolgeneigenschaft mit dem Namen „Title“ implementiert. Und Sie verfügen über eine Eigenschaft "MainPage. sampledatagroupasobject", die vom Typ "Object" ist, aber tatsächlich eine Instanz von sampledatagroup zurückgibt. Die `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>`-Bindung führt zu einem Kompilierungsfehler, da die Title-Eigenschaft nicht im Typobjekt gefunden wird. Die Lösung hierfür ist das Hinzufügen einer Umwandlung zur Path-Syntax: `<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`. Hier ein weiteres Beispiel, in dem "Element" als Objekt deklariert ist, jedoch tatsächlich ein "TextBlock" ist: `<TextBlock Text="{x:Bind Element.Text}"/>`. Und eine Umwandlung behebt das Problem: `<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`.

**Wenn Ihre Daten asynchron geladen werden**

Code zur Unterstützung von **{X:Bind}** wird während der Kompilierung in den partiellen Klassen für Ihre Seiten generiert. Diese Dateien befinden sich in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (für C#). Der generierte Code enthält einen Handler für das [**Loading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)-Ereignis der Seite, der die **Initialize**-Methode für eine generierte Klasse aufruft, die die Bindungen Ihrer Seite darstellt. **Initialize** ruft im Gegenzug **Update** auf, um Daten zwischen der Bindungsquelle und dem Ziel zu verschieben. **Loading** wird genau vor dem ersten Messdurchlauf der Seite oder des Benutzersteuerelements ausgelöst. Wenn Ihre Daten asynchron geladen werden, sind sie zum Zeitpunkt des Aufrufs von **Initialize** möglicherweise nicht bereit. Nachdem Sie die Daten geladen haben, können Sie daher durch den Aufruf von `this.Bindings.Update();`einmalige Bindungen erzwingen. Wenn Sie nur einmalige Bindungen für asynchron geladene Daten benötigen, ist es viel günstiger, Sie auf diese Weise zu initialisieren, als unidirektionale Bindungen und Überwachen von Änderungen. Wenn Ihre Daten keinen differenzierteren Änderungen unterliegen und wahrscheinlich im Rahmen einer bestimmten Aktion aktualisiert werden, können Sie einmalige Bindungen erstellen und durch Aufrufen von **Update** jederzeit ein manuelles Update erzwingen.

> [!NOTE]
> **{x:Bind}** eignet sich nicht für spät gebundene Szenarios, z. b. durch Navigieren der Wörterbuch Struktur eines JSON-Objekts oder durch die Eingabe von Enten. "Ente Typisierung" ist eine schwache Form der Typisierung auf der Basis von lexikalischen Übereinstimmungen in Eigenschaftsnamen (a in, "Wenn Sie durchläuft, gefällt und quakt wie eine Ente, dann eine Ente"). Bei der Typisierung würde eine Bindung an die **Age** -Eigenschaft mit einer **Person** oder einem **Wein** Objekt gleichermaßen erfüllt werden (vorausgesetzt, dass diese Typen jeweils über eine **Age** -Eigenschaft verfügen). Verwenden Sie für diese Szenarien die **{Binding}** -Markup Erweiterung.

### <a name="binding-object-declared-using-binding"></a>Mit {Binding} deklariertes Bindungsobjekt

Wenn Sie C++/WinRT oder Visual C++ Component Extensions (C++/CX) verwenden und dann die [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) -Markup Erweiterung verwenden, müssen Sie der Lauf Zeit Klasse, an die Sie binden möchten, das [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) -Attribut hinzufügen. Um [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)verwenden zu können, benötigen Sie dieses Attribut nicht.

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> Wenn Sie [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)verwenden, ist das [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) -Attribut verfügbar, wenn Sie die Windows SDK Version 10.0.17763.0 (Windows 10, Version 1809) oder höher installiert haben. Ohne dieses Attribut müssen Sie die [icustompropertyprovider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) -Schnittstelle und die [icustomproperty](/uwp/api/windows.ui.xaml.data.icustomproperty) -Schnittstelle implementieren, um die [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) -Markup Erweiterung verwenden zu können.

[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) geht standardmäßig davon aus, dass Sie eine Bindung an den [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) der Markupseite vornehmen. Daher legen wir den **DataContext** der Seite als eine Instanz der Bindungsquellenklasse fest (in diesem Fall des Typs **HostViewModel**). Das folgende Beispiel zeigt das Markup, mit dem das Bindungsobjekt deklariert wird. Wir verwenden das gleiche **Button.Content**-Bindungsziel, das wir bereits weiter oben im Abschnitt „Bindungsziel“ verwendet haben, und führen eine Bindung an die **HostViewModel.NextButtonText**-Eigenschaft vor.

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

Beachten Sie den Wert, den wir für **Path** angeben. Dieser Wert wird im Kontext der [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)-Eigenschaft der Seite interpretiert, die in diesem Beispiel auf eine Instanz von **HostViewModel** festgelegt ist. Der Pfad verweist auf die **HostViewModel.NextButtonText**-Eigenschaft. Wir können **Mode** weglassen, da der [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)-Standardwert einer unidirektionalen Bindung in diesem Fall funktioniert.

Der Standardwert von [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) für ein Benutzeroberflächenelement ist der von der übergeordneten Eigenschaft geerbte Wert. Sie können diesen Standardwert natürlich auch überschreiben, indem Sie **DataContext** explizit festlegen, und diese Eigenschaft wird wiederum standardmäßig von den untergeordneten Elementen geerbt. Das explizite Festlegen von **DataContext** für ein Element ist nützlich, wenn Sie mehrere Bindungen verwenden möchten, die dieselbe Quelle nutzen.

Ein Bindungsobjekt verfügt über eine **Source**-Eigenschaft, für die standardmäßig der [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) des Benutzeroberflächenelements festgelegt ist, für das die Bindung deklariert ist. Sie können diese Standardeinstellung überschreiben, indem Sie **Source**, **RelativeSource** oder **ElementName** explizit für die Bindung festlegen. (Ausführliche Informationen finden Sie unter [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension).)

In einem [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)-Objekt wird der [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) automatisch auf das Datenobjekt festgelegt, auf dem die Vorlage ausgeführt wird. Das unten aufgeführte Beispiel kann als **ItemTemplate** eines Elementsteuerelements verwendet werden, das eine Sammlung eines beliebigen Typs gebunden ist, der über Zeichenfolgeneigenschaften mit dem Namen **Title** und **Description** verfügt.

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> Standardmäßig werden Änderungen an [**TextBox. Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) an eine bidirektionale gebundene Quelle gesendet, wenn das [**Textfeld**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) den Fokus verliert. Damit Änderungen nach jedem Tastaturanschlag des Benutzers gesendet werden, legen Sie **UpdateSourceTrigger** auf **PropertyChanged** für die Bindung im Markup fest. Sie können auch vollständig steuern, wann Änderungen an die Quelle gesendet werden, indem Sie **UpdateSourceTrigger** auf **Explicit** festlegen. Sie behandeln dann Ereignisse für das Textfeld (in der Regel [**TextBox.TextChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)), rufen [**GetBindingExpression**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.getbindingexpression) am Ziel auf, um ein [**BindingExpression**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingexpression)-Objekt abzurufen, und rufen zum Schluss [**BindingExpression.UpdateSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingexpression.updatesource) auf, um die Datenquelle programmgesteuert zu aktualisieren.

Die [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path)-Eigenschaft unterstützt eine Vielzahl von Syntaxoptionen zum Binden geschachtelter Eigenschaften, angefügter Eigenschaften sowie von Ganzzahl- und Zeichenfolgenindexern. Weitere Informationen finden Sie unter [Property-path-Syntax](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax). Die Bindung an Zeichenfolgenindexer hat dieselbe Wirkung wie die Bindung an dynamische Eigenschaften, ohne [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) implementieren zu müssen. Die [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname)-Eigenschaft ist hilfreich für eine Element-an-Element-Bindung. Die [**RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource)-Eigenschaft hat mehrere Funktionen. Eine davon ist, dass sie eine leistungsfähigere Alternative zur Vorlagenbindung innerhalb von [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) ist. Informationen zu anderen Einstellungen finden Sie unter [{Binding}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) und der [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)-Klasse.

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>Was geschieht, wenn die Quelle und das Ziel nicht den gleichen Typ haben?

Wenn Sie die Sichtbarkeit eines Benutzeroberflächenelements basierend auf dem Wert einer booleschen Eigenschaft steuern möchten, oder wenn Sie ein Benutzeroberflächenelement mit einer Farbe rendern möchten, die eine Funktion des Bereichs oder Trends eines numerischen Werts ist, oder wenn Sie einen Datums- und/oder Uhrzeitwert in einer Benutzeroberflächen-Elementeigenschaft angezeigt möchten, die eine Zeichenfolge erwartet, müssen Sie Werte von einem Typ in einen anderen konvertieren. Es wird Fälle geben, bei denen die richtige Lösung ist, eine andere Eigenschaft des richtigen Typs über die Bindungsquellenklasse verfügbar zu machen und die Konvertierungslogik dort gekapselt und testfähig zu belassen. Dies ist jedoch weder eine flexible noch eine skalierbare Lösung, wenn Sie über eine große Anzahl bzw. umfangreiche Kombinationen von Quell- und Zieleigenschaften verfügen. In diesem Fall verfügen Sie über verschiedene Optionen:

* Bei Verwendung von {X:Bind} können Sie für diese Konvertierung direkt an eine Funktion binden.
* Sie können zudem einen Wertkonverter als Objekt zum Durchführen der Konvertierung angeben. 

## <a name="value-converters"></a>Wertkonverter

Nachfolgend ist ein Wertkonverter dargestellt, der sich für eine einmalige oder eine unidirektionale Bindung eignet und der einen [**DateTime**](https://docs.microsoft.com/dotnet/api/system.datetime?redirectedfrom=MSDN)-Wert in einen Zeichenfolgenwert mit dem Monat konvertiert. Die Klasse implementiert [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

```csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

Hier sehen Sie, wie Sie diesen Wertkonverter im Bindungsobjektmarkup nutzen.

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

Das Bindungsmodul ruft die Methoden [**Convert**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) und [**ConvertBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) auf, wenn der [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)-Parameter für die Bindung definiert ist. Wenn Daten aus der Quelle übergeben werden, ruft das Bindungsmodul **Convert** auf und übergibt die zurückgegebenen Daten an das Ziel. Wenn Daten aus dem Ziel übergeben werden (bei einer bidirektionalen Bindung), ruft das Bindungsmodul **ConvertBack** auf und übergibt die zurückgegebenen Daten an die Quelle.

Der Konverter weist auch optionale Parameter auf: [**Converterlanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)ermöglicht das Angeben der Sprache, die in der Konvertierung verwendet werden soll, und [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), wodurch ein Parameter für die Konvertierungs Logik übergeben werden kann. Ein Beispiel, in dem ein Konverterparameter verwendet wird, finden Sie unter [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

> [!NOTE]
> Wenn bei der Konvertierung ein Fehler auftritt, lösen Sie keine Ausnahme aus. Geben Sie stattdessen [**DependencyProperty.UnsetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue) zurück, wodurch die Datenübertragung beendet wird.

Um immer dann einen Standardwert anzuzeigen, wenn die Bindungsquelle nicht aufgelöst werden kann, legen Sie die **FallbackValue** -Eigenschaft für das Bindungsobjekt im Markup fest. Dies ist hilfreich bei der Behandlung von Konvertierungs- und Formatierungsfehlern. Außerdem ist es nützlich zum Binden an Quelleigenschaften, die in einer gebundenen Auflistung von heterogenen Typen ggf. nicht für alle Objekte vorhanden sind.

Wenn Sie ein Textsteuerelement an einen Wert binden, bei dem es sich nicht um eine Zeichenfolge handelt, wird der Wert vom Datenbindungsmodul in eine Zeichenfolge konvertiert. Wenn der Wert ein Verweistyp ist, ruft das Datenbindungsmodul den Zeichenfolgenwert ab, indem [**ICustomPropertyProvider.GetStringRepresentation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) oder [**IStringable.ToString**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) abgerufen wird (falls verfügbar). Andernfalls wird [**Object.ToString**](https://docs.microsoft.com/dotnet/api/system.object.tostring?redirectedfrom=MSDN#System_Object_ToString) aufgerufen. Beachten Sie jedoch, dass das Bindungsmodul alle **ToString**-Implementierungen ignoriert, bei denen die Basisklassenimplementierung ausgeblendet wird. Stattdessen sollte bei Implementierungen von Unterklassen die **ToString**-Basisklassenmethode überschrieben werden. Auf ähnliche Weise werden in systemeigenen Sprachen analog dazu scheinbar [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) und [**IStringable**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable) implementiert. Alle Aufrufe von **GetStringRepresentation** und **IStringable.ToString** werden jedoch an **Object.ToString** oder eine Überschreibung dieser Methode weitergeleitet, und niemals an eine neue **ToString**-Implementierung, bei der die Basisklassenimplementierung ausgeblendet wird.

> [!NOTE]
> Ab Windows 10, Version 1607, wird über das XAML-Framework ein integrierter Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand bereitgestellt. Der Konverter verknüpft **true** mit dem Enumerationswert **Visible** und **false** mit dem Wert **Collapsed**, sodass Sie eine Visibility-Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Für die Verwendung des integrierten Konverters muss die SDK-Zielversion der App mindestens 14393 lauten. Die Verwendung ist nicht möglich, wenn Ihre App für frühere Versionen von Windows 10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

## <a name="function-binding-in-xbind"></a>Funktionsbindung in {x:Bind}

{x:Bind} ermöglicht den letzten Schritt beim Umwandeln eines Bindungspfads in eine Funktion. Hiermit können Konvertierungen und Bindungen durchgeführt werden, die von mehreren Eigenschaften abhängen. Siehe [ **Funktionen in "x:Bind** ".](function-bindings.md)

<span id="resource-dictionaries-with-x-bind"/>

## <a name="element-to-element-binding"></a>Element-an-Element-Bindung

Sie können die Eigenschaft eines XAML-Elements an die Eigenschaft eines anderen XAML-Elements binden. Dies ist ein Beispiel hierfür im Markup.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

> [!IMPORTANT]
> Den erforderlichen Workflow für die Element-zu-Element-Bindung C++mit/WinRT finden Sie unter [Element-zu-Element-Bindung](/windows/uwp/cpp-and-winrt-apis/binding-property#element-to-element-binding).

## <a name="resource-dictionaries-with-xbind"></a>Ressourcenwörterbücher mit {x:Bind}

Die [{x:Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) hängt von der Codegenerierung ab. Sie benötigt daher eine CodeBehind-Datei mit einem Konstruktor, der **InitializeComponent** aufruft (um den generierten Code zu initialisieren). Sie verwenden das Ressourcenwörterbuch erneut, indem Sie seinen Typ instanziieren (sodass **InitializeComponent** aufgerufen wird), anstatt auf den Dateinamen zu verweisen. Nachfolgend ist ein Beispiel dafür aufgeführt, was Sie tun müssen, wenn Sie über ein vorhandenes Ressourcenwörterbuch verfügen und hierfür {x:Bind} verwenden möchten.

TemplatesResourceDictionary.xaml

```xaml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

```csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

```xaml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

## <a name="event-binding-and-icommand"></a>Ereignisbindung und ICommand

[{b:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) unterstützt ein Feature namens „Ereignisbindung“. Mit diesem Feature können Sie den Handler für ein Ereignis mit einer Bindung angeben, wobei es sich zusätzlich zum Behandeln von Ereignissen mit einer Methode in der CodeBehind-Datei um eine weitere Option handelt. Angenommen, Sie haben eine **RootFrame**-Eigenschaft für Ihre **MainPage**-Klasse.

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

Anschließend können Sie das **Click**-Ereignis einer Schaltfläche an das **Frame**-Objekt binden, das von der **RootFrame**-Eigenschaft zurückgegeben wird, wie hier gezeigt. Beachten Sie, dass die **IsEnabled**-Eigenschaft der Schaltfläche auch an einen anderen Member des gleichen **Frame** gebunden wird.

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

Überladene Methoden können nicht verwendet werden, um ein Ereignis mit diesem Verfahren zu behandeln. Wenn die Methode, die das Ereignis behandelt, darüber hinaus über Parameter verfügt, müssen diese alle von den Typen aller Ereignisparameter zugeordnet werden können. In diesem Fall ist [**Frame.GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward) nicht überladen und hat keine Parameter (wäre aber weiterhin gültig, auch wenn es zwei **object**-Parameter verwendet). [**Frame. GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) ist jedoch überladen, sodass wir diese Methode nicht mit dieser Technik verwenden können.

Das Verfahren zu Ereignisbindung ist ähnlich der Implementierung und Nutzung von Befehlen (ein Befehl ist eine Eigenschaft, die ein Objekt zurückgibt, das die [**ICommand**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand)-Schnittstelle implementiert). Sowohl [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) als auch [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) kann mit Befehlen verwendet werden. Damit Sie das Befehlsmuster nicht mehrmals implementieren müssen, können Sie die **DelegateCommand**-Hilfsklasse verwenden, die Sie im [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)-Beispiel (im Ordner „Common“) finden.

## <a name="binding-to-a-collection-of-folders-or-files"></a>Binden an eine Sammlung von Dateien oder Ordnern

Sie können die APIs im [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage)-Namespace verwenden, um Ordner- und Dateidaten abzurufen. Die verschiedenen **GetFilesAsync**-, **GetFoldersAsync**- und **GetItemsAsync**-Methoden geben jedoch keine Werte zurück, die zur Bindung an Listensteuerelemente geeignet sind. Stattdessen müssen Sie die Bindung an die Rückgabewerte der [**GetVirtualizedFilesVector**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfilesvector)-, [**GetVirtualizedFoldersVector**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfoldersvector)-, und [**GetVirtualizedItemsVector**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizeditemsvector)-Methoden der [**FileInformationFactory**](https://docs.microsoft.com/uwp/api/Windows.Storage.BulkAccess.FileInformationFactory)-Klasse vornehmen. Das folgende Codebeispiel aus dem [Beispiel für StorageDataSource und GetVirtualizedFilesVector](https://go.microsoft.com/fwlink/p/?linkid=228621) veranschaulicht das typische Verwendungsmuster. Vergessen Sie nicht, die **picturesLibrary**-Funktion im App-Paketmanifest zu deklarieren, und bestätigen Sie, dass Bilder im Bildbibliotheksordner vorhanden sind.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var library = Windows.Storage.KnownFolders.PicturesLibrary;
    var queryOptions = new Windows.Storage.Search.QueryOptions();
    queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
    queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

    var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

    var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
        fileQuery,
        Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
        190,
        Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
        false
        );

    var dataSource = fif.GetVirtualizedFilesVector();
    this.PicturesListView.ItemsSource = dataSource;
}
```

In der Regel verwenden Sie diese Vorgehensweise, um eine schreibgeschützte Ansicht der Datei- und Ordnerinformationen zu erstellen. Sie können Zwei-Wege-Bindungen an die Datei- und Ordnereigenschaften erstellen, zum Beispiel um Benutzern die Bewertung eines Titels in einer Musikansicht zu ermöglichen. Änderungen werden jedoch erst beibehalten, wenn Sie die entsprechende **SavePropertiesAsync**-Methode aufrufen (beispielsweise [**MusicProperties.SavePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.musicproperties.savepropertiesasync)). Sie müssen für Änderungen einen Commit ausführen, wenn das Element den Fokus verliert, da dadurch das Zurücksetzen der Auswahl ausgelöst wird.

Beachten Sie, dass eine Zwei-Wege-Bindung mit dieser Technik nur für indizierte Orte wie "Musik" funktioniert. Sie können durch Aufrufen der [**FolderInformation.GetIndexedStateAsync**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.folderinformation.getindexedstateasync)-Methode feststellen, ob ein Ort indiziert ist.

Darüber hinaus kann der Wert **null** von einem virtualisierten Vektor für einige Elemente vor dem Füllen des Werts zurückgegeben werden. Beispielsweise sollten Sie nach **null** suchen, bevor Sie den [**SelectedItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)-Wert eines Listensteuerelements verwenden, das an einen virtualisierten Vektor gebunden ist. Sie können stattdessen jedoch auch [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) verwenden.

## <a name="binding-to-data-grouped-by-a-key"></a>Binden an Daten, die durch einen Schlüssel gruppiert werden

Wenn Sie eine flache Auflistung von Elementen erstellen (z. b. durch eine **booksku** -Klasse dargestellte Bücher) und Sie die Elemente mithilfe einer allgemeinen Eigenschaft als Schlüssel gruppieren (z **. b. die Eigenschaft booksku. Authorname** ), wird das Ergebnis als gruppierte Daten bezeichnet. Wenn Sie Daten gruppieren, handelt es sich nicht mehr um eine flache Sammlung. Gruppierte Daten sind eine Auflistung von Gruppen Objekten, in denen jedes Group-Objekt

- ein Schlüssel, und
- eine Auflistung von Elementen, deren-Eigenschaft mit diesem Schlüssel übereinstimmt.

Das Ergebnis der Gruppierung der Bücher nach Autor Name ergibt eine Auflistung von Autoren namens Gruppen, in denen jede Gruppe

- ein Schlüssel, der ein Autorenname ist, und
- eine Auflistung der **booksku**, deren **authorName** -Eigenschaft mit dem Schlüssel der Gruppe übereinstimmt.

In der Regel binden Sie zum Anzeigen einer Sammlung die [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) eines Elementsteuerelements (wie beispielsweise [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) oder [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)) direkt an eine Eigenschaft, die eine Sammlung zurückgibt. Wenn dies eine flache Sammlung von Elementen ist, müssen Sie nichts Besonderes tun. Wenn es sich jedoch um eine Sammlung von Gruppenobjekten handelt (wie bei der Bindung gruppierter Daten), benötigen Sie die Dienste eines Zwischenobjekts, das als [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) bezeichnet wird und sich zwischen dem Elementsteuerelement und der Bindungsquelle befindet. Sie binden die **CollectionViewSource** an die Eigenschaft, die gruppierte Daten zurückgibt, und das Elementsteuerelement an die **CollectionViewSource**. Ein weiterer Vorteil einer **CollectionViewSource** besteht darin, dass sie das aktuelle Element verfolgt, sodass Sie mehr als ein Elementsteuerelement synchron beibehalten können, indem Sie alle Steuerelemente an die gleiche **CollectionViewSource** binden. Sie können über die [**ICollectionView.CurrentItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.icollectionview.currentitem)-Eigenschaft des Objekts, das von der [**CollectionViewSource.View**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.view)-Eigenschaft zurückgegeben wird, auch programmgesteuert auf das aktuelle Element zugreifen.

Um die Gruppierungsfunktion einer [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) zu aktivieren, legen Sie [**IsSourceGrouped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.issourcegrouped) auf **true** fest. Ob Sie auch die [**ItemsPath**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.itemspath)-Eigenschaft festlegen müssen, hängt davon ab, wie Sie die Gruppenobjekte erstellen. Es gibt zwei Möglichkeiten zum Erstellen eines Gruppenobjekts: das Muster „is-a-group“ und das Muster „has-a-group“. Im Muster „is-a-group“ wird das Gruppenobjekt von einem Sammlungstyp abgeleitet (beispielsweise **List&lt;T&gt;** ), sodass das Gruppenobjekt selbst die Gruppe von Elementen darstellt. Bei diesem Muster müssen Sie **ItemsPath** nicht festlegen. Im Muster „has-a-group“ hat das Gruppenobjekt eine oder mehrere Eigenschaften eines Sammlungstyps (beispielsweise **List&lt;T&gt;** ), sodass die Gruppe über eine Gruppe von Elementen (englisch „has a group“) in Form einer Eigenschaft verfügt (oder über mehrere Gruppen von Elementen in der Form mehrerer Eigenschaften). Bei diesem Muster müssen Sie **ItemsPath** auf den Namen der Eigenschaft festlegen, die die Gruppe von Elementen enthält.

Das folgende Beispiel veranschaulicht das Muster „has-a-group“. Die Seitenklasse verfügt über eine Eigenschaft namens [**ViewModel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext), die eine Instanz des Ansichtsmodells zurückgibt. Die [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) bindet an die **Authors**-Eigenschaft des Ansichtsmodells (**Authors** ist die Sammlung von Gruppenobjekten) und gibt darüber hinaus an, dass die **Author.BookSkus**-Eigenschaft die gruppierten Elemente enthält. Zuletzt wird [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) an die **CollectionViewSource** gebunden und erhält eine Definition des Gruppenstils, damit die Ansicht die Elemente in den Gruppen rendern kann.

```xaml
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

Sie haben zwei Möglichkeiten zum Implementieren des Musters „is-a-group“. Eine Möglichkeit besteht darin, eine eigene Gruppenklasse zu erstellen. Leiten Sie die Klasse von **List&lt;T&gt;** ab (wobei *T* der Typ der Elemente ist). Beispiel: `public class Author : List<BookSku>`Hyper-V-Hosts oder Hyper-V-Hostcluster in einem separaten Namespace als verwaltete Hyper-V-Hosts hinzuzufügen. Die zweite Möglichkeit besteht in der Verwendung eines [LINQ](https://docs.microsoft.com/previous-versions/bb397926(v=vs.140))-Ausdrucks zum dynamischen Erstellen von Gruppenobjekten (und einer Gruppenklasse) aus ähnlichen Eigenschaftswerten der **BookSku**-Elemente. Dieser Ansatz, bei dem nur eine flache Liste von Elementen beibehalten wird, die ad-hoc zusammen gruppiert werden, ist typisch für eine App, die über einen Clouddienst auf Daten zugreift. Sie können Bücher beispielsweise nach Autor oder Genre gruppieren, ohne dafür spezielle Gruppenklassen wie **Author** und **Genre** zu benötigen.

Das folgende Beispiel veranschaulicht das Muster „is-a-group“ unter Verwendung von [LINQ](https://docs.microsoft.com/previous-versions/bb397926(v=vs.140)). Dieses Mal werden die Bücher nach Genre gruppiert, wobei der Name des Genres in der Gruppenkopfzeile angezeigt wird. Dies wird durch den „Key“-Eigenschaftspfad als Verweis auf den [**Key**](https://docs.microsoft.com/dotnet/api/system.linq.igrouping-2.key?redirectedfrom=MSDN#System_Linq_IGrouping_2_Key)-Wert der Gruppe angezeigt.

```csharp
using System.Linq;
...
private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
{
    get
    {
        if (this.genres == null)
        {
            this.genres = from book in this.bookSkus
                          group book by book.genre into grp
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

Vergessen Sie nicht, dass Sie bei der Verwendung von [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) mit Datenvorlagen den Typ angeben müssen, an den die Bindung erfolgt, indem Sie einen **x:DataType**-Wert festlegen. Wenn es sich um einen generischen Typ handelt, können wir dies nicht im Markup ausdrücken. Daher müssen wir stattdessen [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) in der Gruppenstil-Kopfzeilenvorlage verwenden.

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

Mit einem [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Steuerelement haben Benutzer eine sehr gute Möglichkeit, gruppierte Daten anzuzeigen und darin zu navigieren. Die Beispiel-App [Bookstore2](https://go.microsoft.com/fwlink/?linkid=532952) veranschaulicht die Verwendung von **SemanticZoom**. In dieser App können Sie in der vergrößerten Ansicht eine Liste der Bücher gruppiert nach Autor anzeigen, oder Sie können die Ansicht verkleinern, um eine Sprungliste der Autoren anzuzeigen. Die Sprungliste ermöglicht eine wesentlich schnellere Navigation im Vergleich zum Blättern in der Bücherliste. Die vergrößerten und verkleinerten Ansichten sind eigentlich **ListView**- bzw. **GridView**-Steuerelemente, die an dieselbe **CollectionViewSource** gebunden sind.

![Abbildung zur Veranschaulichung eines semantischen Zooms](images/sezo.png)

Wenn Sie eine Bindung an hierarchische Daten vornehmen, wie z. B. Unterkategorien von Kategorien, können Sie die Hierarchieebenen in der Benutzeroberfläche mit einer Reihe von Elementsteuerelementen anzeigen. Eine Auswahl in einem Elementsteuerelement bestimmt den Inhalt der nachfolgenden Elementsteuerelemente. Die Listen können synchron gehalten werden, indem jede Liste jeweils an ihre eigene [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) und die **CollectionViewSource**-Instanzen in einer Kette gebunden werden. Dies wird als „Master-/Detailansicht“ (oder „Listen-/Detailansicht“) bezeichnet. Weitere Informationen finden Sie unter [Binden an hierarchische Daten und Erstellen einer Master-/Detailansicht](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>Diagnose und Debuggen von Problemen bei der Datenbindung

Ihr Bindungsmarkup enthält die Namen der Eigenschaften (und für C# manchmal Felder und Methoden). Wenn Sie eine Eigenschaft umbenennen, müssen Sie daher außerdem alle Bindungen ändern, die darauf verweisen. Wenn Sie dies vergessen, führt dies zu einem typischen Beispiel eines Datenbindungsfehlers, und Ihre App kann nicht kompiliert oder ordnungsgemäß ausgeführt werden.

Die von [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) und [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) erstellten Bindungsobjekte sind von der Funktionsweise her größtenteils identisch. {x:Bind} verfügt jedoch über Typinformationen für die Bindungsquelle und generiert zum Zeitpunkt der Kompilierung Quellcode. Bei Verwendung von {x:Bind} erhalten Sie die gleiche Art von Problemerkennung wie bei dem restlichen Code. Dazu gehört die Überprüfung von Bindungsausdrücken während des Kompilierens sowie das Debuggen durch Setzen von Haltepunkten im Quellcode, der als Teilklasse für die Seite erstellt wird. Diese Klassen befinden sich in den Dateien in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (in C#). Wenn bei einer Bindung ein Problem auftritt, aktivieren Sie im Microsoft Visual Studio-Debugger die Option **Bei nicht behandelten Ausnahmen unterbrechen**. Der Debugger unterbricht die Ausführung an dieser Stelle, und Sie können den Fehler dann debuggen. Der von {x:Bind} generierte Code folgt für jeden Teil des Bindungsquellenknoten-Diagramms dem gleichen Muster. Anhand der Informationen im Fenster für die **Aufrufliste** können Sie die Reihenfolge der Aufrufe bis zum Auftreten des Problems ermitteln.

[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) verfügt nicht über Typinformation für die Bindungsquelle. Wenn Sie jedoch die App mit angefügtem Debugger ausführen, werden alle Bindungsfehler in Visual Studio im Fenster **Ausgabe** angezeigt.

## <a name="creating-bindings-in-code"></a>Erstellen von Bindungen im Code

Hinweis  dieser Abschnitt gilt nur für [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension), da Sie keine [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) -Bindungen im Code erstellen können. Allerdings können einige der Vorteile von {x:Bind} auch mit [**DependencyObject.RegisterPropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback) erzielt werden, das Ihnen die Registrierung von Änderungsbenachrichtigungen für Abhängigkeitseigenschaften ermöglicht.

Sie können darüber hinaus Benutzeroberflächenelemente mit Daten verbinden, indem Sie anstelle von XAML prozeduralen Code verwenden. Erstellen Sie hierzu ein neues [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)-Objekt, legen Sie die entsprechenden Eigenschaften fest, und rufen Sie anschließend [**FrameworkElement.SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) oder [**BindingOperations.SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingoperations.setbinding) auf. Die programmgesteuerte Erstellung von Bindungen ist nützlich, wenn Sie die Bindungseigenschaftswerte zur Laufzeit auswählen oder eine einzelne Bindung für mehrere Steuerelemente gemeinsam verwenden möchten. Beachten Sie jedoch, dass Sie die Bindungseigenschaftswerte nach dem Aufrufen von **SetBinding** nicht mehr ändern können.

Im folgenden Beispiel wird gezeigt, wie Sie eine Bindung im Code implementieren.

```xaml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

```vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

## <a name="xbind-and-binding-feature-comparison"></a>Vergleich der Features von {x:Bind} und {Binding}

| Feature | {X:Bind} | {Binding} | Hinweise |
|---------|----------|-----------|-------|
| „Path“ ist die Standardeigenschaft. | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path-Eigenschaft | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | In x:Bind ist Path standardmäßig an Page als Stamm gebunden, nicht DataContext. | 
| Indexer | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Bindung an das in der Sammlung angegebene Element. Es werden nur ganzzahlbasierte Indizes unterstützt. | 
| Angefügte Eigenschaften | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Angefügte Eigenschaften werden in Klammern angegeben. Wenn die Eigenschaft nicht in einem XAML-Namespace deklariert wird, stellen Sie ihr einen XML-Namespace als Präfix voran, der einem Codenamespace am Anfang des Dokuments zugeordnet werden sollte. | 
| Umwandlung | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | Nicht erforderlich. | Umwandlungen werden in Klammern angegeben. Wenn die Eigenschaft nicht in einem XAML-Namespace deklariert wird, stellen Sie ihr einen XML-Namespace als Präfix voran, der einem Codenamespace am Anfang des Dokuments zugeordnet werden sollte. | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Konverter müssen im Stamm von Page/ResourceDictionary oder in „App.xaml“ deklariert werden. | 
| KonverterParameter, KonverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Konverter müssen im Stamm von Page/ResourceDictionary oder in „App.xaml“ deklariert werden. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Wird verwendet, wenn das Blatt des Bindungsausdrucks NULL ist. Verwenden Sie zum Angeben eines Zeichenfolgenwerts einfache Anführungszeichen. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Wird verwendet, wenn ein beliebiger Teil des Pfads für die Bindung (mit Ausnahme des Blatts) NULL ist. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Mit {x:Bind} nehmen Sie eine Bindung an ein Feld vor; Path standardmäßig an Page als Stamm gebunden, damit auf jedes benannte Element über sein Feld zugegriffen werden kann. | 
| RelativeSource Self (Selbst) | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | Bei {x:Bind}: Benennen Sie das Element, und verwenden Sie den Namen in Path. | 
| RelativeSource TemplatedParent | Nicht erforderlich | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | Mit {x:Bind} TargetType in ControlTemplate wird die Bindung an das übergeordnete Element der Vorlage angegeben. Für "{Binding}" kann eine reguläre Vorlagen Bindung in Steuerelement Vorlagen für die meisten Verwendungen verwendet werden. Verwenden Sie jedoch „TemplatedParent“, wenn Sie einen Konverter oder bidirektionale Bindungen verwenden müssen. | 
| Source | Nicht erforderlich | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | Für {x:Bind} können Sie das benannte Element direkt verwenden, eine Eigenschaft oder einen statischen Pfad verwenden. | 
| Modus | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | „Mode“ kann „OneTime“, „OneWay“ oder „TwoWay“ sein. Standardwert für {x:Bind} ist „OneTime“; Standardwert für {Binding} ist „OneWay“. | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger kann Default, LostFocus oder PropertyChanged sein. {X:Bind} unterstützt kein „UpdateSourceTrigger=Explicit”. {x:Bind} verwendet das PropertyChanged-Verhalten in allen Fällen, außer bei „TextBox.Text“, bei dem es das LostFocus-Verhalten nutzt. | 


