---
description: Mit C++/WinRT können Sie Windows-Runtime-APIs über Standard-C++ Datentypen aufrufen.
title: C++-Standarddatentypen und C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, datentypen
ms.localizationpriority: medium
ms.openlocfilehash: 7b0b529bbf397b76acb1eb589095a84f5c85745c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654285"
---
# <a name="standard-c-data-types-and-cwinrt"></a>C++-Standarddatentypen und C++/WinRT

Mit [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), können Sie mithilfe von Standard-c++-Datentypen, einschließlich einige C++-Standardbibliothek-Datentypen Windows-Runtime-APIs aufrufen. Können Sie standard-Zeichenfolgen an APIs übergeben (finden Sie unter [Zeichenfolgenbehandlung in C++ / WinRT](strings.md)), und Sie können Initialisierer Listen und Standardcontainern übergeben, APIs, die eine Auflistung semantisch gleichwertige zu erwarten.

## <a name="standard-initializer-lists"></a>Standard-Initialisierungslisten
Eine Initialisierungsliste (**std::initializer_list**) ist ein Konstrukt aus der C++ Standard Library. Sie können Initialisierungslisten verwenden, wenn Sie bestimmte Windows-Runtime-Konstruktoren und -Methoden aufrufen. Beispielsweise können Sie [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes) mit einer Initialisierungsliste aufrufen.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to an array_view before being passed to WriteBytes.
}
```

Es gibt zwei Elemente, die daran beteiligt sind. Die **DataWriter::WriteBytes**-Methode nimmt zunächst einen Parameter vom Typ [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) entgegen.

```cppwinrt
void WriteBytes(array_view<uint8_t const> value) const
```

 **array_view** ist ein benutzerdefinierter C++/WinRT-Typ, der eine zusammenhängende Reihe von Werten repräsentiert (er ist unter `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h` in der C++/WinRT-Basisbibliothek definiert).

Zweitens hat **array_view** einen Initialisierungslisten-Konstruktor.

```cppwinrt
template <typename T> array_view(std::initializer_list<T> value) noexcept
```

In vielen Fällen können Sie wählen, ob Sie **array_view** in Ihrer Programmierung berücksichtigen wollen oder nicht. Wenn Sie sich dafür entscheiden, dies *nicht* zu tun, müssen Sie keinen Code zu ändern, wenn ein entsprechender Typ in der C++ Standard Library auftaucht.

Sie können eine Initialisierungsliste an eine Windows-Runtime-API übergeben, die einen Collection-Parameter erwartet. Nehmen wir zum Beispiel **StorageItemContentProperties::RetrievePropertiesAsync**.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Sie können dieses API mit einer Initialisierungsliste wie dieser aufrufen.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

Hier spielen zwei Faktoren eine Rolle. Zuerst konstruiert der Callee ein **std::vector** aus der Initialisierungsliste (dieser Callee ist asynchron, also kann er das Objekt besitzen). Zweitens bindet C++/WinRT **std::vector** transparent (und ohne Einführung von Kopien) als Windows-Runtime-Collection-Parameter.

## <a name="standard-arrays-and-vectors"></a>Standard-Arrays und -Vektoren
**array_view** hat ebenfalls **std::vector**- und **std::array**-Konvertierungskonstruktoren.

```cppwinrt
template <typename C, size_type N> array_view(std::array<C, N>& value) noexcept
template <typename C> array_view(std::vector<C>& vectorValue) noexcept
```

Sie können also **DataWriter::WriteBytes** mit einem **std::vector** aufrufen.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to an array_view before being passed to WriteBytes.
```

Oder mit einem **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to an array_view before being passed to WriteBytes.
```

C++/WinRT bindet **std::vector** als Windows-Runtime-Collection-Parameter. Sie können also ein **std::vector&lt;winrt::hstring&gt;** übergeben und es wird in die entsprechende Windows-Runtime-Collection **winrt::hstring** konvertiert. Es gibt eine zusätzliche Details zu berücksichtigen, wenn die aufgerufene Methode asynchron ist. Aufgrund der Implementierungsdetails von diesem Fall müssen Sie die zu Rvalue-Wert, daher müssen Sie einen Kopier- oder Verschiebevorgang des Vektors angeben. Im folgenden Codebeispiel, wir den Besitz des Vektors verschieben, auf das Objekt dem Parametertyp von der aufgerufenen asynchrone akzeptiert (und wir sind dann nicht auf eine sorgfältige `vecH` erneut nach dem Verschieben). Sie können weitere Informationen zu Rvalues finden Sie unter [Wert Kategorien und Verweise darauf](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Sie können aber kein **std::vector&lt;std::wstring&gt;** übergeben, da eine Windows-Runtime-Collection erwartet wird. Dies liegt daran, dass die C++ die Typparameter dieser Collection nicht erzwingt, nachdem sie in die entsprechende Windows-Runtime-Collection **std::wstring** konvertiert wurde. Im folgenden Codebeispiel wird nicht kompiliert, daher (und die Lösung besteht darin, übergeben Sie einen **Std:: vector&lt;winrt::hstring&gt;**  stattdessen, wie oben gezeigt).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Raw-Arrays und Zeigerbereiche
In Anbetracht dessen, dass in der C++ Standard Library in Zukunft ein äquivalenter Typ existieren könnte, können Sie auch direkt mit **array_view** arbeiten, wenn Sie dies wünschen oder benötigen.

**Array_view** verfügt über konvertierungskonstruktoren von einem unformatierten Arrays und aus einem Bereich von **T&ast;**  (Zeiger auf den Typ des Elements).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view Funktionen und Operatoren
Eine Vielzahl von Konstruktoren, Operatoren, Funktionen und Iteratoren sind für **array_view** implementiert. Ein **array_view** ist ein Bereich, also können Sie ihn gültigkeitsbereichbasiert mit `for` oder mit **std::for_each** verwenden.

Weitere Beispiele und Informationen finden Sie im API-Referenzthema zu [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;**  und standard iterationskonstrukten
[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) ist ein Beispiel für eine Windows-Runtime-API, die eine Auflistung des Typs zurückgibt [ **IVector&lt;T&gt;**  ](/uwp/api/windows.foundation.collections.ivector_t_) (wird in C++ projiziert. C++ / WinRT als **Winrt::Windows::Foundation::Collections::IVector&lt;T&gt;**). Können Sie dieses Typs mit standard-Iteration-Konstrukte, wie z. B. bereichsbasierte `for`.

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>C++-Coroutinen mit asynchroner Windows-Runtime-APIs
Sie können weiterhin verwenden die [Parallel Patterns Library (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) beim asynchronen Windows-Runtime-APIs aufrufen. Allerdings bieten C++-Coroutinen in vielen Fällen eine effiziente und mehr problemlos codierte Idiom für die Interaktion mit asynchronen Objekte. Weitere Informationen und Codebeispiele, finden Sie unter [Parallelität und asynchrone Vorgänge mit C++ / WinRT](concurrency.md).

## <a name="important-apis"></a>Wichtige APIs
* [IVector&lt;T&gt; Schnittstelle](/uwp/api/windows.foundation.collections.ivector_t_)
* [Vorlage für WinRT::array_view-Struktur](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Verwandte Themen
* [Zeichenfolgenbehandlung in C++ / WinRT](strings.md)
