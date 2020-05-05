---
description: Dieses Thema enthält einen umfassenden Katalog der technischen Details der Portierung des Quellcodes in einem [C#](/visualstudio/get-started/csharp)-Projekt zum entsprechenden Äquivalent in [C++/WinRT-](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
title: Umstellen von C# auf C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, portieren, migrieren, C#
ms.localizationpriority: medium
ms.openlocfilehash: 804c22b782dada9c0bde3c379ebfe5a37f1dcff9
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81759937"
---
# <a name="move-to-cwinrt-from-c"></a>Umstellen von C# auf C++/WinRT

Dieses Thema enthält einen umfassenden Katalog der technischen Details der Portierung des Quellcodes in einem [C#](/visualstudio/get-started/csharp)-Projekt zum entsprechenden Äquivalent in [C++/WinRT-](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

Eine Fallstudie zum Portieren eines der App-Beispiele für die universelle Windows-Plattform (UWP) finden Sie im Begleitthema [Portieren des Beispiels „Zwischenablage“ (Clipboard) von C# zu C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp). Sie können Praxis und Erfahrung mit dem Portieren sammeln, indem Sie der exemplarischen Vorgehensweise folgen und in deren Verlauf das Beispiel für sich selbst portieren.

## <a name="how-to-prepare-and-what-to-expect"></a>Wie Sie sich vorbereiten und was Sie erwarten dürfen

Die Fallstudie [Portieren des Beispiels „Zwischenablage“ (Clipboard) von C# zu C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) veranschaulicht Beispiele der Arten von Softwareentwurfsentscheidungen, die Sie beim Portieren eines Projekts zu C++/WinRT treffen. Es ist also eine gute Idee, sich auf die Portierung vorzubereiten, indem man sich ein solides Verständnis davon verschafft, wie der vorhandene Code funktioniert. Auf diese Weise erhalten Sie einen guten Überblick über die Funktionen der App und die Struktur des Codes, und die Entscheidungen, die Sie treffen, werden Sie stets vorwärts und in die richtige Richtung bringen.

Die Arten der zu erwartenden Portierungsänderungen lassen sich in vier Kategorien gruppieren.

- [**Portieren der Sprachprojektion**](#port-the-language-projection). Die Windows-Runtime (WinRT) wird in verschiedene Programmiersprachen *projiziert*. Jede dieser Sprachprojektionen ist so konzipiert, dass sie idiomatisch an die betreffende Programmiersprache angepasst ist. Für C# werden einige Windows-Runtime-Typen als .NET-Typen projiziert. So wird [**System.Collections.Generic.IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1) beispielsweise zurück zu [**Windows.Foundation.Collections.IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1) übersetzt. Ebenfalls werden in C# einige Windows-Runtime-Operationen als praktische C#-Sprachfunktionen projiziert. In C# verwenden Sie beispielsweise die `+=`-Operatorsyntax zum Registrieren eines Ereignisbehandlungsdelegaten. Sie übersetzen also Sprachfeatures wie diese zurück in die grundlegende Operation, die gerade ausgeführt wird (in diesem Beispiel die Ereignisregistrierung).
- [**Portieren der Sprachsyntax**](#port-language-syntax). Viele dieser Änderungen sind einfache mechanische Umwandlungen, bei denen ein Symbol durch ein anderes ersetzt wird. Zum Beispiel wird ein Punkt (`.`) in einen Doppelpunkt (`::`) geändert.
- [**Portieren von Sprachprozeduren**](#port-language-procedure). Einige davon können einfache, sich wiederholende Änderungen sein (wie z. B. `myObject.MyProperty` in `myObject.MyProperty()`). Andere benötigen tiefer gehende Änderungen (z. B. die Portierung einer Prozedur, die die Verwendung von **System.Text.StringBuilder** beinhaltet, in eine Prozedur, die die Verwendung von **std::wostringstream** beinhaltet).
- [**Portieren von Tasks, die spezifisch für C++/WinRT sind**](#porting-tasks-that-are-specific-to-cwinrt). Bestimmte Details der Windows-Runtime werden von C# implizit im Hintergrund erledigt. Diese Details werden explizit in C++/WinRT durchgeführt. Ein Beispiel ist die Verwendung einer `.idl`-Datei, um Ihre Laufzeitklassen zu definieren.

Der Rest dieses Themas ist nach dieser Taxonomie gegliedert.

## <a name="port-the-language-projection"></a>Portieren der Sprachprojektion

||C#|C++/WinRT|Siehe auch|
|-|-|-|-|
|Nicht typisiertes Objekt|`object` oder [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[Portieren der **EnableClipboardContentChangedNotifications**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Projektionsnamespaces|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|Größe einer Sammlung|`collection.Count`|`collection.Size()`|[Portieren der **BuildClipboardFormatsOutputString**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Schreibgeschützte Sammlung|[**IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)|[Portieren der **BuildClipboardFormatsOutputString**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Ereignishandlerdelegat als Klassenmember|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[Portieren der **EnableClipboardContentChangedNotifications**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Ereignishandlerdelegat widerrufen|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[Portieren der **EnableClipboardContentChangedNotifications**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|Assoziativer Container|[**IDictionary\<K, V\>** ](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>** ](/uwp/api/windows.foundation.collections.imap-2)||
|Vektormemberzugriff|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>Registrieren/Widerrufen eines Ereignishandlers

In C++/WinRT haben Sie mehrere syntaktische Optionen, um einen Ereignishandlerdelegaten zu registrieren/widerrufen, wie in [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events) beschrieben. Siehe auch [Portieren der **EnableClipboardContentChangedNotifications**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications).

Manchmal, z. B. wenn ein Ereignisempfänger (ein Objekt, das ein Ereignis behandelt) kurz vor der Zerstörung steht, möchten Sie einen Ereignishandler vielleicht widerrufen, damit die Ereignisquelle (das Objekt, das das Ereignis auslöst) kein zerstörtes Objekt aufruft. Mehr dazu erfahren Sie unter [Einen registrierten Delegaten widerrufen](/windows/uwp/cpp-and-winrt-apis/handle-events#revoke-a-registered-delegate). Erstellen Sie in solchen Fällen eine **event_token**-Membervariable für Ihre Ereignishandler. Siehe z. B. [Portieren der **EnableClipboardContentChangedNotifications**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications).

Ein Ereignishandler kann auch in XAML-Markup registriert werden.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

In C# kann deine **OpenButton_Click**-Methode privat sein, und XAML kann sie trotzdem mit dem [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis verbinden, das von *OpenButton* ausgelöst wird.

In C++/WinRT muss deine **OpenButton_Click**-Methode in deinem [Implementierungstyp](/windows/uwp/cpp-and-winrt-apis/author-apis) öffentlich sein, *wenn du sie in XAML-Markup registrieren möchtest*. Wenn du einen Ereignisbehandler nur in imperativem Code registrierst, dann muss der Ereignishandler nicht öffentlich sein.

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

Alternativ dazu kannst du die registrierende XAML-Seite als Friend deines Implementierungstyps und **OpenButton_Click** als privat festlegen.

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

## <a name="port-language-syntax"></a>Portieren der Sprachsyntax

||C#|C++/WinRT|Siehe auch|
|-|-|-|-|
|Zugriffsmodifizierer|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[Portieren der **Button_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#button_click)|
|Asynchrone Aktion|`async Task ...`|`IAsyncAction ...`||
|Asynchroner Vorgang|`async Task<T> ...`|`IAsyncOperation<T> ...`||
|Fire-and-Forget-Methode (impliziert „Asynchron“)|`async void ...`|`winrt::fire_and_forget ...`|[Portieren der **CopyButton_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Kooperatives Wait|`await ...`|`co_await ...`|[Portieren der **CopyButton_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Zugriff auf eine Enumerationskonstante|`E.Value`|`E::Value`|[Portieren der **DisplayChangedFormats**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats)|
|Namespacetrennzeichen|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[Portieren der **UpdateStatus**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|Parameterdeklaration für eine Methode|`MyType`|`MyType const&`|[Parameterübergabe](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|Parameterdeklaration für eine asynchrone Methode|`MyType`|`MyType`|[Parameterübergabe](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|Statische Methode aufrufen|`T.Method()`|`T::Method()`||
|Zeichenfolgen|`string` oder **System.String**|[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring)|[Verarbeitung von Zeichenfolgen in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings)|
|Zeichenfolgenliteral|`"a string literal"`|`L"a string literal"`|[Portieren des Konstruktors, **Current** und **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Ausführliche Zeichenfolgeliterale/Rohzeichenfolgenliterale|`@"verbatim string literal"`|`LR"(raw string literal)"`|[Portieren der **DisplayToast**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast)|
|Auf ein Datenmember zugreifen|`this.variable`|`this->variable`||
|Using-directive|`using A.B.C;`|`using namespace A::B::C;`|[Portieren des Konstruktors, **Current** und **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Hergeleiteter (oder abgeleiteter) Typ|`var`|`auto`|[Portieren der **BuildClipboardFormatsOutputString**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|

> [!NOTE]
> Wenn eine Header Datei keine `using namespace`-Direktive für einen bestimmten Namespace enthält, müssen Sie alle Typnamen für diesen Namespace vollständig qualifizieren oder zumindest ausreichend qualifizieren, damit der Compiler Sie finden kann. Ein Beispiel finden Sie unter [Portieren der **DisplayToast**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast).

### <a name="porting-classes-and-members"></a>Portieren von Klassen und Membern

Sie müssen für jeden C#-Typ entscheiden, ob er in einen Windows-Runtime-Typ oder eine reguläre C++-Klasse/-Struktur/-Enumeration portiert werden soll. Weitere Informationen und ausführliche Beispiele, die veranschaulichen, wie Sie diese Entscheidungen treffen, finden Sie unter [Portieren des Beispiels „Zwischenablage“ (Clipboard) von C# zu C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp).

Eine C#-Eigenschaft wird in der Regel zu einer Accessorfunktion, einer Mutatorfunktion und einem Unterstützungsdatenmember. Weitere Informationen und ein Beispiel finden Sie unter [Portieren der **IsClipboardContentChangedEnabled-** -Eigenschaft](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#isclipboardcontentchangedenabled).

Legen Sie nicht statische Felder als Datenmember Ihres [Implementierungstyps](/windows/uwp/cpp-and-winrt-apis/author-apis) fest.

Ein statisches C#-Feld wird zu einer statischen C++/WinRT-Accessor- und/oder Mutatorfunktion. Weitere Informationen und ein Beispiel finden Sie unter [Portieren des Konstruktors, **Current** und **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name).

Auch für jede einzelne Memberfunktion müssen Sie sich entscheiden, ob sie in die IDL gehört, oder ob es sich um eine öffentliche oder private Memberfunktion Ihres Implementierungstyps handelt. Weitere Informationen und Beispiele zur Entscheidungsfindung finden Sie unter [IDL für den **MainPage**-Typ](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type).

### <a name="porting-xaml-markup-and-asset-files"></a>Portieren von XAML-Markup- und Ressourcendateien

Im Falle von [Portieren des Beispiels „Zwischenablage“ (Clipboard) von C# zu C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) können wir über das C#- und das C++/WinRT-Projekt hinweg *das gleiche*  XAML-Markup (einschließlich der Ressourcen) und die gleichen Ressourcendateien verwenden. In einigen Fällen sind Änderungen an Markup erforderlich, um dies zu erreichen. Siehe [Kopieren von XAML und Stilen, die notwendig sind, um das Portieren von **MainPage** abzuschließen](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage).

## <a name="port-language-procedure"></a>Portieren von Sprachprozeduren

||C#|C++/WinRT|Siehe auch|
|-|-|-|-|
|Verwaltung der Lebensdauer in einer asynchronen Methode|NICHT ZUTREFFEND|`auto lifetime{ get_strong() };` oder<br>`auto lifetime = get_strong();`|[Portieren der **CopyButton_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Verwerfen|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[Portieren der **CopyImage**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copyimage)|
|Objekt erstellen|`new MyType(args)`|`MyType{ args }` oder<br>`MyType(args)`|[Portieren der **Scenarios**-Eigenschaft](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios)|
|Nicht initialisierten Verweis erstellen|`MyType myObject;`|`MyType myObject{ nullptr };` oder<br>`MyType myObject = nullptr;`|[Portieren des Konstruktors, **Current** und **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Objekt in Variable mit Argumenten erstellen|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` oder <br>`auto myObject{ MyType(args) };` oder <br>`auto myObject = MyType{ args };` oder <br>`auto myObject = MyType(args);` oder <br>`MyType myObject{ args };` oder <br>`MyType myObject(args);`|[Portieren der **Footer_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|Objekt in Variable ohne Argumente erstellen|`var myObject = new T();`|`MyType myObject;`|[Portieren der **BuildClipboardFormatsOutputString**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Objektinitialisierung (kompakt)|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|Massenvektorvorgang|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[Portieren der **CopyButton_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|Sammlung durchlaufen|`foreach (var v in c)`|`for (auto&& v : c)`|[Portieren der **BuildClipboardFormatsOutputString**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Ausnahme abfangen|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[Portieren der **PasteButton_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Ausnahmedetails|`ex.Message`|`ex.message()`|[Portieren der **PasteButton_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|Eigenschaftswert abrufen|`myObject.MyProperty`|`myObject.MyProperty()`|[Portieren der **notifyuser**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser)|
|Eigenschaftswert festlegen|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|Eigenschaftswert schrittweise erhöhen|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>Für Zeichenfolgen zu einem Generator wechseln||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|Sprachzeichenfolge in Windows-Runtime-Zeichenfolge|NICHT ZUTREFFEND|`winrt::hstring{ s }`||
|Erstellung von string-Elementen|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[Erstellung von string-Elementen](#string-building)|
|Zeichenfolgeninterpolierung|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) und/oder [**winrt::hstring::operator+** ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[Portieren der **OnNavigatedTo**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto)|
|Leere Zeichenfolge für den Vergleich|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[Portieren der **UpdateStatus**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|Wörterbuchvorgänge|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|Typkonvertierung (bei Fehler auslösen)|`(MyType)v`|`v.as<MyType>()`|[Portieren der **Footer_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|Typkonvertierung (bei Fehler auf „Null“ festlegen)|`v as MyType`|`v.try_as<MyType>()`|[Portieren der **PasteButton_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|XAML-Elemente mit „x:Name“ sind Eigenschaften.|`MyNamedElement`|`MyNamedElement()`|[Portieren des Konstruktors, **Current** und **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|Zum UI-Thread wechseln|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync** oder [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[Portieren der **NotifyUser**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser) und [Portieren der **HistoryAndRoaming**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#historyandroaming)|

In den folgenden Abschnitten finden Sie ausführliche Informationen zu einigen Elementen in der Tabelle.

### <a name="tostring"></a>ToString()

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

| Language | Stringification von „int“ | Stringification von „enum“ |
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

Siehe auch [Portieren der **Footer_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click).

### <a name="string-building"></a>Erstellung von string-Elementen

Für die Erstellung von string-Elementen verfügt C# über einen integrierten [**StringBuilder**](/dotnet/api/system.text.stringbuilder)-Typ.

| | C# | C++/WinRT |
|-|-|-|
| Erstellung von string-Elementen | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| Anfügen einer Windows-Runtime-Zeichenfolge unter Beibehalten von NULL-Werten | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| Neue Zeile hinzufügen |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| Auf das Ergebnis zugreifen | `s = builder.ToString();` | `ws = builder.str();` |

Weitere Informationen finden Sie auch unter [Portieren der **BuildClipboardFormatsOutputString**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring) und [Portieren der **DisplayChangedFormats**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats).

## <a name="porting-tasks-that-are-specific-to-cwinrt"></a>Portieren von Tasks, die spezifisch für C++/WinRT sind

### <a name="define-your-runtime-classes-in-idl"></a>Definieren Ihrer Laufzeitklassen in IDL

Weitere Informationen finden Sie unter [IDL für den **MainPage**-Typ](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type) und [Konsolidieren Ihrer `.idl`-Dateien](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#consolidate-your-idl-files).

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>Schließen Sie die erforderlichen C++/WinRT-Headerdateien des Windows-Namespace ein.

Wann immer Sie in C++/WinRT einen Typ aus einem Windows-Namespace verwenden möchten, müssen Sie die entsprechende C++/WinRT-Windows-Namespace-Headerdatei einschließen. Ein Beispiel finden Sie unter [Portieren der **NotifyUser**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser).

### <a name="boxing-and-unboxing"></a>Boxing und Unboxing

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
| Unboxing für „int“ (falls möglich); Fallback in allen anderen Fällen | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

Ein Beispiel finden Sie unter [Portieren der **OnNavigatedTo**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto) und [Portieren der **Footer_Click**-Methode](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click).

#### <a name="boxing-and-unboxing-a-string"></a>Boxing und Unboxing von string-Elementen

Ein string-Element ist in einigen Fällen ein Werttyp, in anderen Fällen ein Verweistyp. C# und C++/WinRT behandeln string-Elemente unterschiedlich.

Der ABI-Typ [**HSTRING**](/windows/win32/winrt/hstring) ist ein Zeiger auf ein als Verweis gezähltes string-Element. Er wird jedoch nicht von [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) abgeleitet, ist also technisch gesehen kein *Objekt*. Darüber hinaus stellt ein **HSTRING** mit Wert NULL ein leeres string-Element dar. Das Boxing von Elementen, die nicht von **IInspectable** abgeleitet werden, erfolgt über den Einschluss in [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_), und die Windows-Runtime stellt eine Standardimplementierung in Form des [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue)-Objekts bereit (benutzerdefinierte Typen werden als [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype) gemeldet).

C# stellt ein string-Element der Windows-Runtime als Referenztyp dar, C++/WinRT dagegen als Werttyp. Das bedeutet, dass eine geboxte NULL-Zeichenfolge unterschiedliche Darstellungen aufweisen kann, je nachdem, wie du dorthin gelangt bist.

| Verhalten | C# | C++/WinRT|
|-|-|-|
| Deklarationen | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| Kategorie des string-Typs | Verweistyp | Werttyp |
| **HSTRING** mit NULL-Wert wird dargestellt als | `""` | `hstring{}` |
| Sind NULL und `""` identisch? | Nein | Ja |
| Gültigkeit von NULL | `s = null;`<br>`s.Length` löst NullReferenceException aus | `s = hstring{};`<br>`s.size() == 0` (gültig) |
| Wenn Sie einem Objekt eine NULL-Zeichenfolge zuweisen | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Wenn Sie einem Objekt `""` zuweisen | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Grundlegendes Boxing und Unboxing

| Vorgang | C# | C++/WinRT|
|-|-|-|
| Boxing eines string-Elements | `o = s;`<br>Eine leere Zeichenfolge wird zu einem Nicht-NULL-Objekt. | `o = box_value(s);`<br>Eine leere Zeichenfolge wird zu einem Nicht-NULL-Objekt. |
| Unboxing eines bekannten string-Elements | `s = (string)o;`<br>Ein NULL-Objekt wird zu einer NULL-Zeichenfolge.<br>InvalidCastException, falls keine Zeichenfolge. | `s = unbox_value<hstring>(o);`<br>Absturz eines NULL-Objekts.<br>Absturz, falls keine Zeichenfolge. |
| Unboxing einer möglichen Zeichenfolge | `s = o as string;`<br>Ein NULL-Objekt oder eine Nicht-Zeichenfolge wird zu einer NULL Zeichenfolge.<br><br>oder<br><br>`s = o as string ?? fallback;`<br>NULL oder eine Nicht-Zeichenfolge wird zu einem Fallback.<br>Leere Zeichenfolge beibehalten. | `s = unbox_value_or<hstring>(o, fallback);`<br>NULL oder eine Nicht-Zeichenfolge wird zu einem Fallback.<br>Leere Zeichenfolge beibehalten. |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>Verfügbarmachen einer Klasse für die {Binding}-Markuperweiterung

Wenn du die {Binding}-Markuperweiterung zum Binden von Daten an deinen Datentyp verwenden möchtest, findest du Informationen dazu unter [Mit {Binding} deklariertes Bindungsobjekt](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding).

### <a name="consuming-objects-from-xaml-markup"></a>Verwenden von Objekten aus XAML-Markup

In einem C#-Projekt kannst du private Member und benannte Elemente aus XAML-Markup nutzen. In C++/WinRT dagegen müssen alle Entitäten, die über die XAML-[ **{x:Bind}-Markuperweiterung**](/windows/uwp/xaml-platform/x-bind-markup-extension) genutzt werden, in IDL öffentlich verfügbar gemacht werden.

Durch Binden an einen booleschen Typ wird in C# `true` oder `false` angezeigt, in C++/WinRT dagegen **Windows.Foundation.IReference`1\<Boolean\>** .

Weitere Informationen sowie Codebeispiele findest du unter [Verwenden von Objekten aus Markup](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

### <a name="making-a-data-source-available-to-xaml-markup"></a>Verfügbarmachen einer Datenquelle für XAML-Markup

In C++/WinRT, Version 2.0.190530.8 und höher erstellt [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) einen Observable-Vektor, der sowohl **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** als auch **IObservableVector\<IInspectable\>** unterstützt. Ein Beispiel finden Sie unter [Portieren der **Scenarios**-Eigenschaft](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios).

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

Weitere Informationen finden Sie unter [XAML-Elementsteuerelemente: Binden an eine C++/WinRT-Sammlung](/windows/uwp/cpp-and-winrt-apis/binding-collection) und [Sammlungen mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/collections).

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Verfügbarmachen einer Datenquelle für XAML-Markup (vor C++/WinRT 2.0.190530.8)

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

### <a name="derived-classes"></a>Abgeleitete Klassen

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

In der Headerdatei deines [Implementierungstyps](/windows/uwp/cpp-and-winrt-apis/author-apis) musst du die Basisklassen-Headerdatei einschließen, bevor du den automatisch generierten Header für die abgeleitete Klasse einschließt. Andernfalls erhältst du Fehler wie „Ungültige Verwendung dieses Typs als Ausdruck“.

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

## <a name="important-apis"></a>Wichtige APIs
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Zugehörige Themen
* [C#-Tutorials](/visualstudio/get-started/csharp)
* [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)
* [Datenbindung im Detail](/windows/uwp/data-binding/data-binding-in-depth)