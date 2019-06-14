---
ms.assetid: CAC6A7C7-3348-4EC4-8327-D47EB6E0C238
title: Zugreifen auf die SD-Karte
description: Sie können weniger wichtige Daten auf einer optionalen microSD-Karte speichern und auf diese zugreifen. Dies gilt besonders für kostengünstige Geräte, die nur über einen begrenzten internen Speicher verfügen.
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, UWP, SD-Karte, Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 4573e0959cf9d4af9b3cef8ffbbce14847a9e521
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369494"
---
# <a name="access-the-sd-card"></a>Zugreifen auf die SD-Karte



Sie können weniger wichtige Daten auf einer optionalen microSD-Karte speichern. Dies gilt besonders für Mobilgeräte, die nur über einen begrenzten Speicher und einen SD-Kartensteckplatz verfügen.

In den meisten Fällen müssen Sie die **removableStorage**-Funktion in der App-Manifestdatei angeben, bevor die App Dateien auf der SD-Karte speichern und darauf zugreifen kann. Normalerweise müssen Sie außerdem die Registrierung durchführen, damit die Dateitypen behandelt werden können, die von der App gespeichert werden und auf die zugegriffen wird.

Sie können Dateien mithilfe der folgenden Methoden auf der optionalen SD-Karte speichern und darauf zugreifen:
- Dateiauswahlen
- Die [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage)-APIs.

## <a name="what-you-can-and-cant-access-on-the-sd-card"></a>Zugriffsmöglichkeiten auf der SD-Karte

### <a name="what-you-can-access"></a>Zugriffsmöglichkeiten

- Die App kann nur Dateien mit den Dateitypen lesen und schreiben, die in der App-Manifestdatei für die Verarbeitung registriert sind.
- Mit der App können auch Ordner erstellt und verwaltet werden.

### <a name="what-you-cant-access"></a>Kein Zugriff

- Systemordner und die darin enthaltenen Dateien sind für die App nicht sichtbar, und sie kann nicht darauf zugreifen.
- Für die App sind auch keine Dateien sichtbar, die mit dem Attribut "Hidden" (Ausgeblendet) gekennzeichnet sind. Das Attribut "Hidden" wird normalerweise verwendet, um das Risiko des versehentlichen Löschens von Daten zu verringern.
- Außerdem kann die App die Dokumentbibliothek nicht sehen und nicht über [**KnownFolders.DocumentsLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.documentslibrary) darauf zugreifen. Sie können aber auf der SD-Karte auf die Dokumentbibliothek zugreifen, indem Sie das Dateisystem durchlaufen.

## <a name="security-and-privacy-considerations"></a>Sicherheits- und Datenschutzaspekte

Wenn eine App Dateien an einem globalen Speicherort auf der SD-Karte speichert, werden diese Dateien nicht verschlüsselt und sind daher für andere Apps normalerweise zugänglich.

- Wenn sich die SD-Karte im Gerät befindet, können auch andere Apps auf Ihre Dateien zugreifen, die über die Registrierung für die Verarbeitung desselben Dateityps verfügen.
- Wenn die SD-Karte aus dem Gerät herausgenommen wird und über einen PC darauf zugegriffen wird, sind Ihre Dateien im Datei-Explorer sichtbar und für andere Apps zugänglich.

Wenn eine auf der SD-Karte installierte App Dateien in [**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) speichert, werden diese Dateien jedoch verschlüsselt und sind für andere Apps nicht zugänglich.

## <a name="requirements-for-accessing-files-on-the-sd-card"></a>Anforderungen für den Dateizugriff auf der SD-Karte

Zum Zugreifen auf Dateien auf der SD-Karte müssen Sie normalerweise die folgenden Dinge angeben.

1.  Sie müssen die **removableStorage**-Funktion in der App-Manifestdatei angeben.
2.  Außerdem müssen Sie die Dateierweiterungen für die Verarbeitung registrieren, die dem Medientyp zugeordnet sind, auf den Sie zugreifen möchten.

Verwenden Sie die vorangehende Methode außerdem für den Zugriff auf Mediendateien auf der SD-Karte, ohne auf einen bekannten Ordner wie **KnownFolders.MusicLibrary** zu verweisen oder auf Mediendateien zuzugreifen, die außerhalb der Medienbibliotheksordner gespeichert sind.

Um auf Mediendateien (Musik, Fotos oder Videos) zuzugreifen, die in den Medienbibliotheken in bekannten Ordnern gespeichert sind, müssen Sie nur die zugeordnete Funktion in der App-Manifestdatei angeben: **musicLibrary**, **picturesLibrary** oder **videoLibrary**. Es ist nicht erforderlich, die **removableStorage**-Funktion anzugeben. Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

## <a name="accessing-files-on-the-sd-card"></a>Zugreifen auf Dateien auf der SD-Karte

### <a name="getting-a-reference-to-the-sd-card"></a>Abrufen eines Verweises auf die SD-Karte

Der Ordner [**KnownFolders.RemovableDevices**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) ist der Stamm-[**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) für die Wechselmedien, die derzeit an das Gerät angeschlossen sind. Wenn eine SD-Karte vorhanden ist, stellt der erste (und einzige) **StorageFolder** unter dem Ordner **KnownFolders.RemovableDevices** die SD-Karte dar.

Verwenden Sie Code der folgenden Art, um zu ermitteln, ob eine SD-Karte vorhanden ist, und um einen Verweis darauf als [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) zu erhalten.

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    // An SD card is present and the sdCard variable now contains a reference to it.
}
else
{
    // No SD card is present.
}
```

> [!NOTE]
> Wenn Ihr SD-Kartenleser ein eingebetteter Reader (z. B. ein Steckplatz im Laptop oder PC selbst) ist, kann möglicherweise nicht über „KnownFolders.RemovableDevices“ auf ihn zugegriffen werden.

### <a name="querying-the-contents-of-the-sd-card"></a>Abfragen des Inhalts der SD-Karte

Die SD-Karte kann viele Ordner und Dateien enthalten, die nicht als bekannte Ordner erkannt werden und nicht abgefragt werden können, indem ein Speicherort unter [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) verwendet wird. Zum Auffinden von Dateien muss die App den Inhalt der Karte aufzählen, indem sie das Dateisystem rekursiv durchläuft. Verwenden Sie [**GetFilesAsync (CommonFileQuery.DefaultQuery)** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) und [**GetFoldersAsync (CommonFolderQuery.DefaultQuery)** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfoldersasync), um die Inhalte der SD-Karte auf effiziente Weise abzurufen.

Wir empfehlen Ihnen, zum Durchlaufen der SD-Karte einen Hintergrundthread zu verwenden. Eine SD-Karte kann viele Gigabyte an Daten enthalten.

Die App kann Benutzer auch zum Auswählen bestimmter Ordner mithilfe der Dateiauswahl auffordern.

Beim Zugreifen auf das Dateisystem auf der SD-Karte mit einem Pfad, der von [**KnownFolders.RemovableDevices**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) abgeleitet ist, verhalten sich die untenstehenden Methoden wie folgt:

-   Die [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync)-Methode gibt die Union (Vereinigung) der Dateierweiterungen, die Sie für die Verarbeitung registriert haben, und der Dateierweiterungen zurück, die den von Ihnen angegebenen Medienbibliothekfunktionen zugeordnet sind.
-   Für die [**GetFileFromPathAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync)-Methode tritt ein Fehler auf, wenn Sie die Dateierweiterung der Datei, auf die Sie zugreifen möchten, nicht für die Verarbeitung registriert haben.

## <a name="identifying-the-individual-sd-card"></a>Identifizieren einer individuellen SD-Karte

Nach der ersten Bereitstellung der SD-Karte generiert das Betriebssystem einen eindeutigen Bezeichner für die Karte. Diese ID wird in einer Datei im Stammordner der Karte im Ordner "WPSystem" gespeichert. Mithilfe dieser ID kann eine App bestimmen, ob die Karte erkannt wird. Wenn die Karte von einer App erkannt wird, kann die App bestimmte Vorgänge, die vorher durchgeführt wurden, unter Umständen verschieben. Der Inhalt der Karte kann sich seit dem letzten Zugriff auf die Karte durch die App aber geändert haben.

Angenommen, eine App indiziert E-Books. Falls die App die gesamte SD-Karte bereits auf E-Book-Dateien untersucht und einen Index für die E-Books angelegt hat, kann diese Liste sofort angezeigt werden, sobald die Karte wieder eingesetzt wird und die App die Karte erkannt hat. In einem separaten Vorgang kann ein Hintergrundthread mit geringer Priorität gestartet werden, um nach neuen E-Books zu suchen. Außerdem kann ein Fehler verarbeitet werden, der bei einer Suche nach einem nicht mehr vorhandenen E-Book auftritt, wenn ein Benutzer versucht, auf das gelöschte E-Book zuzugreifen.

Der Name der Eigenschaft, in der diese ID enthalten ist, lautet **WindowsPhone.ExternalStorageId**.

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    var allProperties = sdCard.Properties;
    IEnumerable<string> propertiesToRetrieve = new List<string> { "WindowsPhone.ExternalStorageId" };

    var storageIdProperties = await allProperties.RetrievePropertiesAsync(propertiesToRetrieve);

    string cardId = (string)storageIdProperties["WindowsPhone.ExternalStorageId"];

    if (...) // If cardID matches the cached ID of a recognized card.
    {
        // Card is recognized. Index contents opportunistically.
    }
    else
    {
        // Card is not recognized. Index contents immediately.
    }
}
```

 

 
