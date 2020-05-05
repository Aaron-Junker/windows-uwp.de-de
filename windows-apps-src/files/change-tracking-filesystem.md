---
title: Nachverfolgen von Dateisystemänderungen im Hintergrund
description: Hier erfährst du, wie du im Hintergrund Änderungen an Dateien und Ordnern nachverfolgst, die von Benutzern im System verschoben werden.
ms.date: 12/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1cef2fb660681d3e382eb8ca7dcb92456756f627
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75685235"
---
# <a name="track-file-system-changes-in-the-background"></a>Nachverfolgen von Dateisystemänderungen im Hintergrund

**Wichtige APIs**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

Die Klasse [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) ermöglicht es Apps, Änderungen an Dateien und Ordnern nachzuverfolgen, die von Benutzern im System verschoben werden. Mithilfe der Klasse **StorageLibraryChangeTracker** kann eine App Folgendes nachverfolgen:

- Dateivorgänge wie Hinzufügen, Löschen und Ändern
- Ordnervorgänge wie Umbenennungen und Löschungen
- Verschiebung von Dateien und Ordnern auf dem Laufwerk

In diesem Handbuch lernst du das Programmiermodell für die Verwendung der Änderungsnachverfolgung kennen. Außerdem findest du hier etwas Beispielcode sowie Informationen zu den verschiedenen Arten von Dateivorgängen, die von **StorageLibraryChangeTracker** nachverfolgt werden.

**StorageLibraryChangeTracker** eignet sich für Benutzerbibliotheken sowie für jeden beliebigen Ordner auf dem lokalen Computer. Dies schließt sekundäre Laufwerke und Wechseldatenträger mit ein, gilt aber nicht für NAS- oder Netzlaufwerke.

## <a name="using-the-change-tracker"></a>Verwenden der Änderungsnachverfolgung

Die Änderungsnachverfolgung wird im System als zirkulärer Puffer implementiert, in dem die letzten *N* Dateisystemvorgänge gespeichert werden. Apps können die Änderungen aus dem Puffer lesen und in ihrer eigenen Umgebung verarbeiten. Wenn die App die Verarbeitung der Änderungen abgeschlossen hat, werden diese als verarbeitet markiert und in Zukunft nicht mehr angezeigt.

Führe die folgenden Schritte aus, um die Änderungsnachverfolgung für einen Ordner zu verwenden:

1. Aktiviere die Änderungsnachverfolgung für den Ordner.
2. Warte auf Änderungen.
3. Lies die Änderungen.
4. Akzeptiere die Änderungen.

Die obigen Schritte werden in den nächsten Abschnitten anhand von Codebeispielen veranschaulicht. Das vollständige Codebeispiel befindet sich am Ende des Artikels.

### <a name="enable-the-change-tracker"></a>Aktivieren der Änderungsnachverfolgung

Die App muss dem System zunächst signalisieren, dass sie die Änderungsnachverfolgung für eine bestimmte Bibliothek verwenden möchte. Hierzu ruft sie für die relevante Bibliothek die Methode [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) für die Änderungsnachverfolgung auf.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Einige wichtige Hinweise:

- Vergewissere dich vor der Erstellung des Objekts [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary), dass deine App im Manifest über Berechtigungen für die korrekte Bibliothek verfügt. Ausführlichere Informationen findest du unter [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).
- [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) ist threadsicher, setzt deinen Zeiger nicht zurück und kann beliebig oft aufgerufen werden. (Dies wird später noch genauer erläutert.)

![Aktivieren einer leeren Änderungsnachverfolgung](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Warten auf Änderungen

Nach der Initialisierung beginnt die Änderungsnachverfolgung damit, sämtliche Vorgänge zu erfassen, die innerhalb der Bibliothek stattfinden – selbst wenn die App gar nicht ausgeführt wird. Apps können sich für das Ereignis [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) registrieren, um aktiviert zu werden, sobald eine Änderung vorgenommen wird.

![Änderungen, die der Änderungsnachverfolgung hinzugefügt werden, ohne von der App gelesen zu werden](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Lesen der Änderungen

Die App kann nun Änderungen aus der Änderungsnachverfolgung abrufen, um eine Liste mit den Änderungen seit der letzten Überprüfung zu erhalten. Der folgende Code zeigt das Abrufen einer Liste mit Änderungen aus der Änderungsnachverfolgung:

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

Die App muss die Änderungen nun nach Bedarf in ihrer eigenen Umgebung oder Datenbank verarbeiten.

![Lesen der Änderungen aus der Änderungsnachverfolgung in eine App-Datenbank](images/changetracker-reading.png)

> [!TIP]
> Der zweite Aufruf dient zur Vermeidung einer Racebedingung für den Fall, dass der Benutzer der Bibliothek einen weiteren Ordner hinzufügt, während deine App Änderungen liest. Ohne den zusätzlichen Aufruf von **Enable** tritt ein ecSearchFolderScopeViolation-Fehler (0x80070490) auf, wenn der Benutzer die Ordner in ihrer Bibliothek ändert.

### <a name="accept-the-changes"></a>Akzeptieren der Änderungen

Nachdem die App die Änderungen verarbeitet hat, muss sie das System durch Aufrufen der Methode [**AcceptChangesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) anweisen, die Änderungen nicht mehr anzuzeigen.

```csharp
await changeReader.AcceptChangesAsync();
```

![Markieren von Änderungen als gelesen, damit sie nicht mehr angezeigt werden](images/changetracker-accepting.png)

Bei zukünftigen Lesevorgängen für die Änderungsnachverfolgung erhält die App nun nur noch neue Änderungen.

- Sollten Änderungen zwischen dem Aufruf von [**ReadBatchAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) und dem Aufruf von [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) stattgefunden haben, wird der Zeiger nur bis zur neuesten Änderung versetzt, die von der App erfasst wurde. Die übrigen Änderungen sind beim nächsten Aufruf von **ReadBatchAsync** weiterhin verfügbar.
- Werden die Änderungen nicht akzeptiert, gibt das System die gleiche Gruppe von Änderungen zurück, wenn die App das nächste Mal **ReadBatchAsync** aufruft.

## <a name="important-things-to-remember"></a>Wichtige Punkte

Im Zusammenhang mit der Verwendung der Änderungsnachverfolgung müssen einige Dinge berücksichtigt werden, um sicherzustellen, dass alles ordnungsgemäß funktioniert.

### <a name="buffer-overruns"></a>Pufferüberläufe

Wir versuchen zwar, in der Änderungsnachverfolgung genügend Speicherplatz zu reservieren, damit alle ausgeführten Vorgänge gespeichert werden können, bis sie von deiner App gelesen werden, es kann jedoch schnell passieren, dass der zirkuläre Puffer sich selbst überschreibt, bevor die App die Änderungen gelesen hat. Dieser Fall kann insbesondere eintreten, wenn der Benutzer Daten aus einer Sicherung wiederherstellt oder eine umfangreiche Bildersammlung von seinem Smartphone synchronisiert.

In diesem Fall gibt **ReadBatchAsync** den Fehlercode [**StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype) zurück. Wenn deine App diesen Fehlercode empfängt, bedeutet das Folgendes:

* Der Puffer hat sich seit der letzten Betrachtung selbst überschrieben. In diesem Fall empfiehlt es sich, die Bibliothek erneut zu durchforsten, da die Nachverfolgungsinformationen unvollständig sind.
* Die Änderungsnachverfolgung gibt bis zum Aufrufen von [**Reset**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset) keine weiteren Änderungen zurück. Nachdem die App „Reset“ aufgerufen hat, wird der Zeiger an der Position der neuesten Änderung platziert, und die Nachverfolgung wird normal fortgesetzt.

Solche Fälle sind zwar eher selten, wenn ein Benutzer aber eine große Anzahl von Dateien auf dem Datenträger verschiebt, soll die Änderungsnachverfolgung nicht zu stark aufgebläht werden und dadurch zu viel Speicherplatz beanspruchen. Mit dieser Lösung können Apps auf umfangreiche Dateisystemvorgänge reagieren, ohne die Benutzerfreundlichkeit unter Windows zu beeinträchtigen.

### <a name="changes-to-a-storagelibrary"></a>Änderungen an einer Speicherbibliothek (StorageLibrary)

Die Klasse [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) ist eine virtuelle Gruppe von Stammordnern, die wiederum weitere Ordner enthalten. Um dies mit einer Dateisystem-Änderungsnachverfolgung unter einen Hut zu bringen, haben wir folgende Entscheidungen getroffen:

- Sämtliche Änderungen an Nachfolgern der Stammbibliothekordner werden in der Änderungsnachverfolgung berücksichtigt. Die Stammbibliothekordner können mithilfe der Eigenschaft [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) ermittelt werden.
- Das Hinzufügen oder Entfernen von Stammordnern zu bzw. aus einem Element vom Typ **StorageLibrary** (über [**RequestAddFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) bzw. [**RequestRemoveFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) führt nicht zur Erstellung eines Eintrags in der Änderungsnachverfolgung. Diese Änderungen können über das Ereignis [**DefinitionChanged**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) oder mittels Aufzählung der Stammordner in der Bibliothek (mithilfe der Eigenschaft [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)) nachverfolgt werden.
- Wenn der Bibliothek ein nicht leerer Ordner hinzugefügt wird, werden keine Änderungsbenachrichtigungen oder Einträge in der Änderungsnachverfolgung generiert. Spätere Änderungen an den Nachfolgern dieses Ordners führen zur Generierung von Benachrichtigungen sowie zu Einträgen in der Änderungsnachverfolgung.

### <a name="calling-the-enable-method"></a>Aufrufen der Methode „Enable“

Apps müssen die Methode [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) aufrufen, sobald sie mit der Nachverfolgung des Dateisystems beginnen, sowie vor jeder Aufzählung der Änderungen. Dadurch wird sichergestellt, dass alle Änderungen von der Änderungsnachverfolgung erfasst werden.  

## <a name="putting-it-together"></a>Zusammenführung

Hier siehst du den gesamten Code. Er dient dazu, eine Registrierung für die Änderungen aus der Videobibliothek durchzuführen und mit dem Abrufen der Änderungen aus der Änderungsnachverfolgung zu beginnen.

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```
