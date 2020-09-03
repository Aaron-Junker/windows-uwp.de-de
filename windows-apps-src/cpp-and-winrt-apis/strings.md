---
description: Mit C++/WinRT kannst du Windows-Runtime-APIs über standardmäßige breite C++-Zeichenfolgentypen aufrufen oder den Typ „winrt::hstring“ verwenden.
title: Behandeln von Zeichenfolgen in C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Zeichenfolge
ms.localizationpriority: medium
ms.openlocfilehash: 56b75710c2d259e59dac476bcb860a5e4c6938d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154394"
---
# <a name="string-handling-in-cwinrt"></a>Behandeln von Zeichenfolgen in C++/WinRT

Mit [C++/WinRT](./intro-to-using-cpp-with-winrt.md) kannst du Windows-Runtime-APIs über breite Zeichenfolgentypen (beispielsweise **std::wstring**) der C++-Standardbibliothek aufrufen. (Schmale Zeichenfolgentypen wie **std::string** können hierzu nicht verwendet werden.) C++/WinRT verfügt über einen benutzerdefinierten Zeichenfolgentyp namens [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Er ist in der C++/WinRT-Basisbibliothek `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h` definiert. Und das ist der Zeichenfolgentyp, der von Windows-Runtime-Konstruktoren, -Funktionen und -Eigenschaften akzeptiert und zurückgegeben wird. Dank der Konvertierungskonstruktoren und -operatoren von **hstring** hast du aber in vielen Fällen die Wahl, ob du **hstring** in deinem Clientcode nutzen möchtest. Wenn du APIs *erstellst*, musst du dich wahrscheinlich intensiver mit **hstring** beschäftigen.

In C++ stehen zahlreiche Zeichenfolgentypen zur Verfügung. Neben **std::basic_string** aus der C++-Standardbibliothek gibt es in vielen Bibliotheken auch noch Varianten davon. C++17 verfügt über Hilfsprogramme für die Zeichenfolgenkonvertierung sowie über **std::basic_string_view**, um die Lücken zwischen den verschiedenen Zeichenfolgentypen zu schließen.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) ermöglicht Konvertierungen mit **std::wstring_view**, um die Interoperabilität zu gewährleisten, für die **std::basic_string_view** entworfen wurde.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>Verwenden von **std::wstring** (und optional **winrt::hstring**) mit **Uri**
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) basiert auf [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

**hstring** verfügt jedoch über [Konvertierungskonstruktoren](/uwp/cpp-ref-for-winrt/hstring#hstringhstring-constructor), sodass du damit arbeiten kannst, ohne weiter darauf zu achten. Das folgende Codebeispiel zeigt die Umwandlung eines breiten Zeichenfolgenliterals, einer breiten Zeichenfolgeansicht und eines Elements vom Typ **std::wstring** in einen **URI**:

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

Der Eigenschaftenaccessor [**Uri::Domain**](/uwp/api/windows.foundation.uri.Domain) ist vom Typ **hstring**.

```cppwinrt
public:
    winrt::hstring Domain();
```

Aber dank des [Konvertierungsoperators für **std::wstring_view**](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view) von **hstring** muss dir auch dieses Detail nicht unbedingt bewusst sein.

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

Analog dazu wird von [**IStringable::ToString**](/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) eine hstring-Zeichenfolge zurückgegeben.

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** implementiert die [**IStringable**](/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable)-Schnittstelle.

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Du kannst die [**Funktion „hstring::c_str“** ](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) verwenden, um aus einer **hstring**-Zeichenfolge eine breite Standardzeichenfolge zu erhalten (genau wie bei **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Wenn du über eine **hstring**-Zeichenfolge verfügst, kannst du diese in einen **URI** konvertieren.

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

Zur Veranschaulichung verwenden wir eine Methode, die eine **hstring**-Zeichenfolge akzeptiert:

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

Alle der weiter oben behandelten Optionen gelten auch in solchen Fällen.

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** besitzt einen Memberkonvertierungsoperator (**std::wstring_view**), und die Konvertierung erfolgt kostenlos.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>**winrt::hstring**: Funktionen und Operatoren
Für [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) wurde eine Vielzahl von Konstruktoren, Operatoren, Funktionen und Iteratoren implementiert.

Da es sich bei **hstring** um einen Bereich handelt, kannst du „hstring“ mit bereichsbasiertem `for` oder mit `std::for_each` verwenden. Es bietet auch Vergleichsoperatoren für natürliche und effiziente Vergleiche mit den entsprechenden Pendants in der C++-Standardbibliothek. Außerdem ist bereits alles enthalten, um **hstring** als Schlüssel für assoziative Container verwenden zu können.

Viele C++ Bibliotheken verwenden **std::string** und arbeiten ausschließlich mit UTF-8-Text. Zur Vereinfachung stehen Hilfsprogramme wie [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) und [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) für die Konvertierung in beide Richtungen zur Verfügung.

`WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Weitere Beispiele und Informationen zu **hstring**-Funktionen und -Operatoren findest du im API-Referenzthema für [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>Die Gründe für **winrt::hstring** und **winrt::param::hstring**
Die Windows-Runtime-Implementierung basiert auf **wchar_t**-Zeichen, die binäre Anwendungsschnittstelle (Application Binary Interface, ABI) der Windows-Runtime ist jedoch keine Teilmenge dessen, was von **std::wstring** oder **std::wstring_view** bereitgestellt wird. Die Verwendung dieser Elemente wäre also äußerst ineffizient. Stattdessen steht in C++/WinRT **winrt::hstring** zur Verfügung, um eine unveränderliche Zeichenfolge darzustellen, die der zugrundeliegenden [HSTRING](/windows/desktop/WinRT/hstring)-Zeichenfolge entspricht und hinter einer Schnittstelle implementiert wird, die mit der Schnittstelle von **std::wstring** vergleichbar ist. 

Wie du vielleicht schon bemerkt hast, wird von C++/WinRT-Eingabeparametern, die logisch betrachtet **winrt::hstring** akzeptieren sollten, tatsächlich **winrt::param::hstring** erwartet. Der **param**-Namespace enthält eine Reihe von Typen, die ausschließlich zur Optimierung von Eingabeparametern dienen, um eine natürliche Bindung an Typen der C++-Standardbibliothek zu ermöglichen und Kopien und andere Ineffizienzen zu vermeiden. Diese Typen sollten nicht direkt verwendet werden. Wenn du eine Optimierung für deine eigenen Funktionen verwenden möchtest, verwende **std::wstring_view**. Siehe auch [Übergabe von Parametern in die ABI-Grenze](./pass-parms-to-abi.md).

Dadurch kannst du die Besonderheiten der Windows-Runtime-Zeichenfolgenverwaltung weitgehend ignorieren und effizient arbeiten, ohne dir neues Wissen anzueignen. Das ist angesichts der intensiven Nutzung von Zeichenfolgen in der Windows-Runtime ein bedeutender Vorteil.

## <a name="formatting-strings"></a>Formatieren von Zeichenfolgen
Eine Möglichkeit zum Formatieren von Zeichenfolgen ist **std::wostringstream**. Im folgenden Beispiel wird eine einfache Meldung der Debugablaufverfolgung formatiert und angezeigt:

```cppwinrt
#include <sstream>
#include <winrt/Windows.UI.Input.h>
#include <winrt/Windows.UI.Xaml.Input.h>
...
void MainPage::OnPointerPressed(winrt::Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    winrt::Windows::Foundation::Point const point{ e.GetCurrentPoint(nullptr).Position() };
    std::wostringstream wostringstream;
    wostringstream << L"Pointer pressed at (" << point.X << L"," << point.Y << L")" << std::endl;
    ::OutputDebugString(wostringstream.str().c_str());
}
```

## <a name="the-correct-way-to-set-a-property"></a>Die richtige Vorgehensweise zum Festlegen einer Eigenschaft

Sie können eine Eigenschaft festlegen, indem Sie einen Wert an eine Setter-Funktion übergeben. Hier ist ein Beispiel.

```cppwinrt
// The right way to set the Text property.
myTextBlock.Text(L"Hello!");
```

Der folgende Code ist falsch. Er wird kompiliert, jedoch ändert er lediglich den temporären **winrt::hstring**, der von der Accessorfunktion **Text()** zurückgegeben wird, und verwirft dann das Ergebnis.

```cppwinrt
// *Not* the right way to set the Text property.
myTextBlock.Text() = L"Hello!";
```

## <a name="important-apis"></a>Wichtige APIs
* [winrt::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [Funktion „winrt::to_hstring“](/uwp/cpp-ref-for-winrt/to-hstring)
* [Funktion „winrt::to_string“](/uwp/cpp-ref-for-winrt/to-string)