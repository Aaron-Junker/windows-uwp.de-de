---
author: stevewhims
description: Mit C++/WinRT können Sie Windows-Runtime-APIs mit Standard-C++ String-Typen aufrufen, oder Sie können den Typ winrt::hstring verwenden.
title: String-Verarbeitung in C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, string
ms.localizationpriority: medium
ms.openlocfilehash: 865267a6897a551613479a099d10dd6d5a91c315
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "5401069"
---
# <a name="string-handling-in-cwinrt"></a>Zeichenkettenverarbeitung in C++/WinRT

Mit [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), Sie können Windows-Runtime-APIs mit Standard-c++ wide-String-Typen wie z. B. **Std:: wstring** aufrufen (Hinweis: nicht mit narrow-String-Typen wie z. B. **Std:: String**). C++/WinRT hat einen benutzerdefinierten String-Typ namens [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (definiert in der C++/WinRT-Basisbibliothek `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`). Und das ist der String-Typ, den Windows-Runtime-Konstruktoren, -Funktionen und -Eigenschaften tatsächlich entgegennehmen und zurückgeben. Aber in vielen Fällen können Sie dank der Konvertierungskonstruktoren und Konvertierungsoperatoren von **hstring** wählen, ob Sie **hstring** in Ihrem Client-Code nutzen oder nicht. Wenn Sie APIs *erstellen*, ist es wahrscheinlicher, dass Sie **hstring** kennen müssen.

Es gibt viele String-Typen in C++. Zusätzlich zu **std::basic_string** aus der C++ Standard Library existieren in vielen Bibliotheken Varianten. C++17 verfügt über String-Konvertierungstools und **std::basic_string_view**, um die Lücken zwischen den String-Typen zu schließen.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) bietet eine Konvertierbarkeit über **std::wstring_view**, um die Interoperabilität zu gewährleisten, für die **std::basic_string_view** entworfen wurde.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>Verwendung von **std::wstring** (und optional **winrt::hstring**) mit **Uri**
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) ist aus einem [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) aufgebaut.

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

Aber **hstring** hat [Konvertierungskonstruktoren](/uwp/api/windows.foundation.uri#hstringhstring-constructor), mit denen Sie damit arbeiten können, ohne sich dessen bewusst sein zu müssen. Hier ist ein Codebeispiel, das zeigt, wie man ein **Uri** aus einem Wide-String-Literal, aus einer Wide-String-Ansicht und aus einem **std::wstring** macht.

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

Die Eigenschaftszugriffsfunktion [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain) ist vom Typ **hstring**.

```cppwinrt
public:
    winrt::hstring Domain();
```

Aber auch dieses Detail ist optional (dank des [Konvertierungsoperators für **std::wstring_view**](/uwp/api/hstring#hstringoperator-stdwstringview) von **hstring**.

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

Ebenso gibt [**IStringable::ToString**](https://msdn.microsoft.com/library/windows/desktop/dn302136) ein hstring zurück.

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** implementiert die [**IStringable**](https://msdn.microsoft.com/library/windows/desktop/dn302135)-Schnittstelle.

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Sie können die [**hstring::c_str-Funktion**](/uwp/api/windows.foundation.uri#hstringcstr-function) verwenden, um einen Standard Wide-String aus einem **hstring** zu erhalten (genau wie aus einem **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Wenn Sie einen **hstring** haben, dann können Sie daraus einen **Uri** machen.

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

Nehmen wir eine Methode, die einen **hstring** entgegennimmt.

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

Alle Optionen, die Sie gerade gesehen haben, gelten auch in solchen Fällen.

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** hat einen MItgliedkonvertierungsoperator **std::wstring_view**, und die Konvertierung wird kostenlos durchgeführt.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>**winrt::hstring** Funktionen und Operatoren
Eine Vielzahl von Konstruktoren, Operatoren, Funktionen und Iteratoren sind für [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) implementiert.

Ein **hstring** ist ein Gültigkeitsbereich, also können Sie ihn mit einem bereichsbasierten `for` oder mit `std::for_each` verwenden. Es bietet auch Vergleichsoperatoren für den natürlichen und effizienten Vergleich mit seinen Pendants in der C++ Standard Library. Und es enthält alles, was Sie brauchen, um **hstring** als Schlüssel für assoziative Container zu verwenden.

Wir stellen fest, dass viele C++ Bibliotheken **std::string** verwenden und ausschließlich mit UTF-8-Text arbeiten. Zur Bequemlichkeit bieten wir Hilfsprogramme an, wie z.B. [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) und [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring), für die Konvertierung in beide Richtungen.

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Weitere Beispiele und Informationen über **hstring**-Funktionen und -Operatoren finden Sie im Referenzthema der [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)-API.

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>Die Gründe für **winrt::hstring** und **winrt::param::hstring**
Die Windows-Runtime ist in Form von **wchar_t** Zeichen implementiert, aber das Application Binary Interface (ABI) von Windows-Runtime ist keine Teilmenge der Möglichkeiten von **std::wstring **oder **std::wstring_view**. Diese zu nutzen, würde zu erheblicher Ineffizienz führen. Stattdessen bietet C++/WinRT **winrt::hstring**, das einen unveränderlichen String darstellt, der mit dem zugrundeliegenden [HSTRING](https://msdn.microsoft.com/library/windows/desktop/br205775) übereinstimmt und hinter einer Schnittstelle ähnlich der von **std::wstring** implementiert ist. 

Sie werden feststellen, dass C++/WinRT-Eingabeparameter, die logisch **winrt::hstring** akzeptieren, tatsächlich **winrt::param::hstring** erwarten. Der **param**-Namespace enthält eine Reihe von Typen, die ausschließlich zur Optimierung von Eingabeparametern verwendet werden, um für eine natürliche Bindung an C++ Standard Library-Typen zu sorgen und Kopien und andere Ineffizienzen zu vermeiden. Sie sollten diese Typen nicht direkt verwenden. Wenn Sie eine Optimierung für Ihre eigenen Funktionen verwenden wollen, dann verwenden Sie **std::wstring_view**.

Das Ergebnis ist, dass Sie die Besonderheiten der Windows-Runtime-String-Verwaltung weitgehend ignorieren und mit dem, was Sie wissen, effizient arbeiten können. Wenn man bedenkt, wie stark Strings in der Windows-Runtime verwendet werden, ist dies sehr wichtig.

# <a name="formatting-strings"></a>Strings formatieren
Eine Option für die Stringformatierung ist **std::wstringstream**. Hier ist ein Beispiel, das eine einfache Debug-Trace-Meldung formatiert und anzeigt.

```cppwinrt
#include <sstream>
...
void OnPointerPressed(IInspectable const&, PointerEventArgs const& args)
{
    float2 const point = args.CurrentPoint().Position();
    std::wstringstream wstringstream;
    wstringstream << L"Pointer pressed at (" << point.x << L"," << point.y << L")" << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());
}
```

## <a name="important-apis"></a>Wichtige APIs
* [winrt::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [to_hstring-Funktion](/uwp/cpp-ref-for-winrt/to-hstring)
* [WinRT:: to_string-Funktion](/uwp/cpp-ref-for-winrt/to-string)
