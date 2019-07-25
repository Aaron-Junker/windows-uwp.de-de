---
description: Mit C++/WinRT können Sie Windows-Runtime-APIs über C++-Standarddatentypen aufrufen.
title: C++-Standarddatentypen und C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Daten, Typen
ms.localizationpriority: medium
ms.openlocfilehash: a87ba48a0853058ba1259e079c586b97af551656
ms.sourcegitcommit: 8b4c1fdfef21925d372287901ab33441068e1a80
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844329"
---
# <a name="standard-c-data-types-and-cwinrt"></a>C++-Standarddatentypen und C++/WinRT

Mit [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) kannst du Windows-Runtime-APIs unter Verwendung von C++-Standarddatentypen aufrufen (einschließlich einiger Datentypen der C++-Standardbibliothek). Du kannst Standardzeichenfolgen an APIs übergeben (siehe [Behandeln von Zeichenfolgen in C++/WinRT](strings.md)) sowie Initialisierungslisten und Standardcontainer an APIs übergeben, die eine semantisch äquivalente Sammlung erwarten.

Siehe auch [Übergabe von Parametern in die ABI-Grenze](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi).

## <a name="standard-initializer-lists"></a>Standardinitialisierungslisten
Eine Initialisierungsliste (**std::initializer_list**) ist ein Konstrukt der C++-Standardbibliothek. Initialisierungslisten können beim Aufrufen bestimmter Windows-Runtime-Konstruktoren und -Methoden verwendet werden – etwa beim Aufrufen von [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes).

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

Damit das funktioniert, müssen zwei Voraussetzungen erfüllt sein. Erstens: Die Methode **DataWriter::WriteBytes** akzeptiert einen Parameter vom Typ [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**winrt::array_view** ist ein benutzerdefinierter C++/WinRT-Typ, der auf sichere Weise eine zusammenhängende Reihe von Werten darstellt und in der C++/WinRT-Basisbibliothek definiert ist: `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`.

Zweitens: **winrt::array_view** verfügt über einen Initialisierungslisten-Konstruktor.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

In vielen Fällen kannst du wählen, ob du **winrt::array_view** in deiner Programmierung berücksichtigen möchtest. Falls du dich *dagegen* entscheidest, musst du keinerlei Codeänderungen vornehmen, wenn in der C++-Standardbibliothek ein äquivalenter Typ auftaucht.

Du kannst eine Initialisierungsliste an eine Windows-Runtime-API übergeben, die einen Sammlungsparameter erwartet. Nehmen wir zum Beispiel **StorageItemContentProperties::RetrievePropertiesAsync**:

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Diese API kannst du mit einer Initialisierungsliste wie der folgenden aufrufen:

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

Hier spielen zwei Faktoren eine Rolle. Erstens: Der Aufgerufene konstruiert auf der Grundlage der Initialisierungsliste einen Standardvektor (**std::vector**). (Dieser Aufgerufene ist asynchron und kann daher das Objekt besitzen, was in diesem Fall erforderlich ist.) Zweitens: C++/WinRT bindet **std::vector** transparent (und ohne Kopien) als Windows-Runtime-Sammlungsparameter.

## <a name="standard-arrays-and-vectors"></a>Standardarrays und -vektoren
[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) verfügt auch über Konvertierungskonstruktoren für **std::vector** und **std::array**.

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

Du könntest also stattdessen **DataWriter::WriteBytes** mit einem Standardvektor (**std::vector**) aufrufen.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

Oder mit einem Standardarray (**std::array**).

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

C++/WinRT bindet **std::vector** als Windows-Runtime-Sammlungsparameter. Du kannst also **std::vector&lt;winrt::hstring&gt;** übergeben, und es wird in die entsprechende Windows-Runtime-Sammlung **winrt::hstring** konvertiert. Bei einem asynchronen Aufgerufenen ist noch ein weiteres Detail zu beachten. Die Implementierungsdetails dieses Falls machen die Angabe eines R-Werts (rvalue) erforderlich. Du musst also eine Kopie oder Verschiebung des Vektors angeben. Im folgenden Codebeispiel übertragen wir den Besitz des Vektors auf das Objekt des Parametertyps, der von dem asynchronen Aufgerufenen akzeptiert wird (und achten anschließend sorgfältig darauf, nach der Verschiebung nicht erneut auf `vecH` zuzugreifen). Weitere Informationen zu R-Werten findest du unter [Value categories, and references to them](cpp-value-categories.md) (Wertekategorien und entsprechende Verweise).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

**std::vector&lt;std::wstring&gt;** kann jedoch nicht übergeben werden, wenn eine Windows-Runtime-Sammlung erwartet wird. Das liegt daran, dass C++ die Typparameter dieser Sammlung nicht erzwingt, nachdem sie in die entsprechende Windows-Runtime-Sammlung **std::wstring** konvertiert wurde. Das folgende Codebeispiel lässt sich daher nur kompilieren, wenn stattdessen **std::vector&lt;winrt::hstring&gt;** übergeben wird, wie oben gezeigt:

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Unformatierte Arrays und Zeigerbereiche
Da die Möglichkeit besteht, dass die C++-Standardbibliothek später einmal einen äquivalenten Typ enthält, kannst du auf Wunsch oder bei Bedarf auch direkt mit **winrt::array_view** arbeiten.

**winrt::array_view** verfügt über Konvertierungskonstruktoren für ein unformatiertes Array sowie für einen Bereich von **T&ast;** (Zeiger auf den Elementtyp).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Funktionen und Operatoren für „winrt::array_view“
Für **winrt::array_view** wurde eine Vielzahl von Konstruktoren, Operatoren, Funktionen und Iteratoren implementiert. Da es sich bei **winrt::array_view** sich um einen Bereich handelt, kann das Element mit `for` (bereichsbasiert) oder mit **std::for_each** verwendet werden.

Weitere Beispiele und Informationen findest du im API-Referenzthema für [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** und Standarditerationskonstrukte
[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) ist ein Beispiel für eine Windows-Runtime-API, die eine Sammlung vom Typ [**IVector&lt;T&gt;** ](/uwp/api/windows.foundation.collections.ivector_t_) zurückgibt (projiziert in C++/WinRT als **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;** ). Dieser Typ kann mit Standarditerationskonstrukten wie `for` (bereichsbasiert) verwendet werden.

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>C++-Coroutinen mit asynchronen Windows-Runtime-APIs
Zum Aufrufen asynchroner Windows-Runtime-APIs kann weiterhin die [Parallel Patterns Library (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) verwendet werden. In vielen Fällen bieten C++-Coroutinen allerdings eine effiziente und einfacher zu programmierende Alternative für die Interaktion mit asynchronen Objekten. Weitere Informationen und Codebeispiele findest du unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md).

## <a name="important-apis"></a>Wichtige APIs
* [Schnittstelle „IVector&lt;T&gt;“](/uwp/api/windows.foundation.collections.ivector_t_)
* [Strukturvorlage „winrt::array_view“](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Verwandte Themen
* [Behandeln von Zeichenfolgen in C++/WinRT](strings.md)
