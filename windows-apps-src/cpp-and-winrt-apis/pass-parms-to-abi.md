---
description: C++/WinRT vereinfacht die Übergabe von Parametern in die ABI-Grenze, indem automatische Konvertierungen für gängige Fälle bereitgestellt werden.
title: Übergabe von Parametern in die ABI-Grenze
ms.date: 07/10/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, übergeben, Parameter, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 9c5ce6a30e68fe6fc26316bc2f41c6e2556b98ef
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255254"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>Übergabe von Parametern in die ABI-Grenze

C++/WinRT vereinfacht mit den Typen im **winrt::param**-Namespace die Übergabe von Parametern in die ABI-Grenze, indem automatische Konvertierungen für gängige Fälle bereitgestellt werden. In [Behandeln von Zeichenfolgen](/windows/uwp/cpp-and-winrt-apis/strings) und [C++-Standarddatentypen und C++/WinRT](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types) finden Sie Codebeispiele und weitere Informationen.

> [!IMPORTANT]
> Verwenden Sie nicht selbst die Typen im **winrt::param**-Namespace. Sie dienen der Projektion.

Viele Typen sind in synchroner und asynchroner Version verfügbar. C++/WinRT verwendet die synchrone Version, wenn Sie einen Parameter an eine synchrone Methode übergeben. Die asynchrone Version wird verwendet, wenn Sie einen Parameter an eine asynchrone Methode übergeben. In der asynchronen Version werden zusätzliche Schritte ausgeführt, um dem Aufrufer das Mutieren der Sammlung vor Abschluss des Vorgangs zu erschweren. Beachten Sie jedoch, dass keine der Varianten die Sammlung vor dem Mutieren über einen anderen Thread schützt. Es liegt in Ihrer Verantwortung, das Mutieren zu verhindern.

## <a name="string-parameters"></a>Zeichenfolgenparameter

**winrt::param::hstring** vereinfacht die Übergabe von Parametern an APIs, die **HSTRING** unterstützen.

|Typen, die übergeben werden können|Hinweise|
|-|-|
|`{}`|Übergibt eine leere Zeichenfolge.|
|**winrt::hstring**||
|**std::wstring_view**|Für Literale können Sie `L"Name"sv` schreiben. In der Ansicht muss sich nach dem Ende ein NULL-Terminator befinden.|
|**std::wstring**|-|
|**wchar_t const\***|Eine NULL-terminierte Zeichenfolge.|

`nullptr` ist nicht zulässig. Verwenden Sie stattdessen `{}`.

Der Compiler kann zur Kompilierzeit `wcslen` für Zeichenfolgeliterale auswerten. Für Literale sind somit `L"Name"sv` und `L"Name"` äquivalent.

Beachten Sie, dass **std::wstring_view**-Objekte nicht NULL-terminiert sind. In C++/WinRT muss jedoch das Zeichen nach dem Ende der Zeichenfolge NULL sein. Wenn Sie eine **std::wstring_view** übergeben, die nicht NULL-terminiert ist, wird der Prozess beendet.

## <a name="iterable-parameters"></a>iterable-Parameter

**winrt::param::iterable\<T\>** und **winrt::param::async_iterable\<T\>** vereinfachen die Übergabe von Parametern an APIs, die **IIterable\<T\>** unterstützen.

Windows-Runtime-Sammlungen sind bereits **IIterable**.

|Typen, die übergeben werden können|Synchronisierung|Async|Hinweise|
|-|-|-|-|
| `nullptr` | Ja | Ja | Sie müssen sicherstellen, dass die zugrunde liegende Methode `nullptr` unterstützt.|
| **IIterable\<T\>** | Ja | Ja | Oder ein Typ, der sich in ihn konvertieren lässt.|
| **std::vector\<T\> const&** | Ja | Nein ||
| **std::vector\<T\>&&** | Ja | Ja | Inhalte werden in den Iterator verschoben, um Mutation zu verhindern.|
| **std::initializer_list\<T\>** | Ja | Ja | In der asynchronen Version werden die Elemente kopiert.|
| **std::initializer_list\<U\>** | Ja | Nein | **U** muss in **T** konvertierbar sein.|
| `{ ForwardIt begin, ForwardIt end }` | Ja | Nein | `*begin` muss in **T** konvertierbar sein.|

Beachten Sie, dass **IIterable\<U\>** und **std::vector\<U\>** nicht zulässig sind, selbst wenn **U** in **T** konvertiert werden kann. Für **std::vector\<U\>** können Sie die Version mit doppeltem Iterator verwenden (weitere Informationen finden Sie unten).

In manchen Fällen implementiert das Objekt, über das Sie verfügen, möglicherweise tatsächlich die gewünschte Instanz von **IIterable**. Beispielsweise wird **IIterable<StorageFile>** durch das von [**FileOpenPicker.PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) erzeugte **IVectorView\<StorageFile\>** implementiert. Es implementiert jedoch auch **IIterable<IStorageItem>** ; Sie müssen es lediglich explizit anfordern.

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

In anderen Fällen können Sie die Version mit doppeltem Iterator verwenden.

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

Die Version mit doppeltem Iterator eignet sich generell für Sammlungen, die nicht den oben genannten Szenarien entsprechen, solange Sie sie durchlaufen können und Elemente erzeugen, die sich in **T** konvertieren lassen. Wir haben oben diese Version verwendet, um einen Vektor abgeleiteter Typen zu durchlaufen. Hier verwenden wir sie, um einen Nicht-Vektor abgeleiteter Typen zu durchlaufen.

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

Die Implementierung von [**IIterator\<T\>.GetMany(T\[\])** ](/uwp/api/windows.foundation.collections.iiterator-1.getmany) ist effizienter, wenn der Iterator ein `RandomAcessIt` ist. Andernfalls wird der Bereich mehrmals durchlaufen.

|Typen, die übergeben werden können|Synchronisierung|Async|Hinweise|
|-|-|-|-|
| `nullptr` | Ja | Ja | Sie müssen sicherstellen, dass die zugrunde liegende Methode `nullptr` unterstützt.|
| **IIterable\<IKeyValuePair\<K, V\>\>** | Ja | Ja | Oder ein Typ, der sich in ihn konvertieren lässt.|
| **std::map\<K, V\> const&** | Ja | Nein ||
| **std::map\<K, V\>&&** | Ja | Ja | Inhalte werden in den Iterator verschoben, um Mutation zu verhindern.|
| **std::unordered_map\<K, V\> const&** | Ja | Nein ||
| **std::unordered_map\<K, V\>&&** | Ja | Ja | Inhalte werden in den Iterator verschoben, um Mutation zu verhindern.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Ja | Ja | Die Typen **K** und **V** müssen genau übereinstimmen. Schlüssel können nicht dupliziert werden. In der asynchronen Version werden die Elemente kopiert.|
| `{ ForwardIt begin, ForwardIt end }` | Ja | Nein | `begin->first` und `begin->second` müssen in **K** bzw. **V** konvertierbar sein.|

## <a name="vector-view-parameters"></a>Vektoransichtsparameter

**winrt::param::vector_view\<T\>** und **winrt::param::async_vector_view\<T\>** vereinfachen die Übergabe von Parametern an APIs, die **IVectorView\<T\>** unterstützen.

Sie können mithilfe von [**IVector\<T\>.GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) eine **IVectorView** aus einem **IVector** abrufen.

|Typen, die übergeben werden können|Synchronisierung|Async|Hinweise|
|-|-|-|-|
| `nullptr` | Ja | Ja | Sie müssen sicherstellen, dass die zugrunde liegende Methode `nullptr` unterstützt.|
| **IVectorView\<T\>** | Ja | Ja | Oder ein Typ, der sich in ihn konvertieren lässt.|
| **std::vector\<T\>const&** | Ja | Nein ||
| **std::vector\<T\>&&** | Ja | Ja | Inhalte werden in die Ansicht verschoben, um Mutation zu verhindern.|
| **std::initializer_list\<T\>** | Ja | Ja | Der Typ muss genau übereinstimmen. In der asynchronen Version werden die Elemente kopiert.|
| `{ ForwardIt begin, ForwardIt end }` | Ja | Nein | `*begin` muss in **T** konvertierbar sein.|

Die Version mit doppeltem Iterator kann verwendet werden, um Vektoransichten in Szenarien zu erstellen, in denen eine direkte Übergabe nicht möglich ist. Es wird jedoch empfohlen, einen `RandomAcessIter` zu übergeben, da Vektoren direkten Zugriff nicht unterstützen.

## <a name="map-view-parameters"></a>Kartenansichtsparameter

**winrt::param::map_view\<T\>** und **winrt::param::async_map_view\<T\>** vereinfachen die Übergabe von Parametern an APIs, die **IMapView\<T\>** unterstützen.

Sie können mithilfe von **IMap::GetView** eine **IMapView** aus **IMap** abrufen.

|Typen, die übergeben werden können|Synchronisierung|Async|Hinweise|
|-|-|-|-|
| `nullptr` | Ja | Ja | Sie müssen sicherstellen, dass die zugrunde liegende Methode `nullptr` unterstützt.|
| **IMapView\<K, V\>** | Ja | Ja | Oder ein Typ, der sich in ihn konvertieren lässt.|
| **std::map\<K, V\> const&** | Ja | Nein ||
| **std::map\<K, V\>&&** | Ja | Ja | Inhalte werden in die Ansicht verschoben, um Mutation zu verhindern.|
| **std::unordered_map\<K, V\> const&**  | Ja | Nein ||
| **std::unordered_map\<K, V\>&&** | Ja | Ja | Inhalte werden in die Ansicht verschoben, um Mutation zu verhindern.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Ja | Ja | Sowohl in der synchronen als auch in der asynchronen Version werden die Elemente kopiert. Schlüssel können nicht dupliziert werden.|

## <a name="vector-parameters"></a>Vektorparameter

**winrt::param::vector\<T\>** vereinfacht die Übergabe von Parametern an APIs, die **IVector\<T\>** unterstützen.

|Typen, die übergeben werden können|Hinweise|
|-|-|
| `nullptr` | Sie müssen sicherstellen, dass die zugrunde liegende Methode `nullptr` unterstützt.|
| **IVector\<T\>** | Oder ein Typ, der sich in ihn konvertieren lässt.|
| **std::vector\<T\>&&** | Die Inhalte werden in den Parameter verschoben, um Mutation zu verhindern. Die Ergebnisse werden nicht zurück verschoben.|
| **std::initializer_list\<T\>** | Die Inhalte werden in den Parameter kopiert, um Mutation zu verhindern.|

Wenn die Methode den Vektor mutiert, ist die einzige Möglichkeit zum Überwachen der Mutation das direkte Übergeben eines **IVector**. Wenn Sie **std::vector** übergeben, mutiert die Methode die Kopie und nicht das Original.

## <a name="map-parameters"></a>Kartenparameter

**winrt::param::map\<T\>** vereinfacht die Übergabe von Parametern an APIs, die **IMap\<T\>** unterstützen.

|Typen, die übergeben werden können|Hinweise|
|-|-|
| `nullptr` | Sie müssen sicherstellen, dass die zugrunde liegende Methode `nullptr` unterstützt.|
| **IMap\<T\>** | Oder ein Typ, der sich in ihn konvertieren lässt.|
| **std::map\<K, V\>&&** | Die Inhalte werden in den Parameter verschoben, um Mutation zu verhindern. Die Ergebnisse werden nicht zurück verschoben.|
| **std::unordered_map\<K, V\>&&** | Die Inhalte werden in den Parameter verschoben, um Mutation zu verhindern. Die Ergebnisse werden nicht zurück verschoben.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Die Inhalte werden in den Parameter kopiert, um Mutation zu verhindern.|

Wenn die Methode die Karte mutiert, ist die einzige Möglichkeit zum Überwachen der Mutation das direkte Übergeben von **IMap**. Wenn Sie **std::map** oder **std::unordered_map** übergeben, mutiert die Methode die Kopie und nicht das Original.

## <a name="array-parameters"></a>Arrayparameter

**winrt::array_view\<T\>** befindet sich nicht im **winrt::param**-Namespace, wird jedoch für Parameter verwendet, die Arrays im C-Stil – auch als *konforme Arrays* bezeichnet – sind.

|Typen, die übergeben werden können|Hinweise|
|-|-|
| `{}` | Ein leeres Array.|
| **array** | Ein konformes C-Array (`C array[N];`), wobei **C** in **T** konvertierbar ist und `sizeof(C) == sizeof(T)`. |
| **std::array<C, N>** | Ein C++-**std::array** von **C**, wobei **C** in **T** konvertierbar ist und `sizeof(C) == sizeof(T)`. |
| **std::vector<C>** | Ein C++-**std::vector** von **C**, wobei **C** in **T** konvertierbar ist und `sizeof(C) == sizeof(T)`. |
| `{ T*, T* }` | Ein Zeigerpaar, das den Bereich [Anfang, Ende) darstellt.|
| **std::initializer_list\<T\>** ||

Weitere Informationen finden Sie im Blogbeitrag [The various patterns for passing C-style arrays across the Windows Runtime ABI boundary](https://devblogs.microsoft.com/oldnewthing/20200205-00/?p=103398) (Die verschiedenen Muster für die Weitergabe von Arrays im C-Stil über die Windows-Runtime-ABI-Grenze).