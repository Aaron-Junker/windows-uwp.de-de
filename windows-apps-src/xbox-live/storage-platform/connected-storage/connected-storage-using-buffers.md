---
title: Arbeiten mit Speicher verbundene Puffern
description: Erfahren Sie, zum Arbeiten mit Speicherpuffer verbunden.
ms.assetid: 1d9d1b52-4bfe-4cd9-8b80-50ca6c0e9ae1
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 3df95e4807e8d3457143e67eebfb62011bf365cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595025"
---
# <a name="working-with-connected-storage-buffers"></a>Arbeiten mit Speicher verbundene Puffern

Die Speicher-API von verbunden verwendet **Windows::Storage::Streams::Buffer** Instanzen, um Daten in und aus einer Anwendung zu übergeben. Da WinRT-Typen verfügbar gemacht werden rohzeiger nicht möglich, den Zugriff auf die Daten von einer Instanz des Puffer-erfolgt über **DataReader** und **DataWriter** Klassen. Allerdings **Puffer** implementiert auch die COM-Schnittstelle **IBufferByteAccess**, wodurch es möglich, einen Zeiger direkt an den Pufferdaten abrufen.

### <a name="to-get-a-pointer-to-a-buffer-instances-data"></a>Um einen Zeiger auf eine Instanz des Puffer-Daten

1.  Verwendung **neu interpretiert werden soll\_Umwandlung** umwandeln die Puffer-Instanz als **IUnknown**.

```cpp
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
```

2.  Abfrage der **IUnknown** eine Schnittstelle für die **IBufferByteAccess** COM-Schnittstelle.

```cpp
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
```

3.  Verwendung **IBufferByteAccess::Buffer** auf einen Zeiger direkt in den Puffer, der Daten zu erhalten.

```cpp
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
```

Das folgende Codebeispiel zeigt z. B. wie beim Erstellen des Puffers, der die aktuelle Systemzeit enthält. Da Puffer einen separaten Kapazität und Länge-Wert sein. ist es erforderlich, um die Kapazität und die Länge explizit festzulegen. Standardmäßig ist die Länge 0.

```cpp
    inline byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer)
    {
        using namespace Windows::Storage::Streams;
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
        return bytes;
    }

    IBuffer^ WrapRawBuffer( void* ptr, size_t size )
    {
        using namespace Windows::Storage::Streams;

        //uint32 size = sizeof(FILETIME);
        Buffer^ buffer = ref new Buffer(size);
        buffer->Length = size;

        memcpy(GetBufferData(buffer),ptr,size);


        return buffer;

    };
```
