---
title: Nachverfolgen von Dateisystemänderungen im Hintergrund
description: Beschreibt, wie Änderungen in Dateien und Ordner im Hintergrund nachverfolgen, wie sie Benutzer im System verschieben.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621575"
---
# <a name="track-file-system-changes-in-the-background"></a>Nachverfolgen von Dateisystemänderungen im Hintergrund

**Wichtige APIs**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

Die [ **StorageLibraryChangeTracker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) Klasse ermöglicht apps, die zum Nachverfolgen von Änderungen in Dateien und Ordnern, wie sie Benutzer im System verschieben. Mithilfe der **StorageLibraryChangeTracker** -Klasse, eine app kann nachverfolgen:

- Bei Dateivorgängen Sie einschließlich hinzufügen, löschen Sie, ändern.
- Ordner-Vorgänge wie umbenennungen und löscht.
- Dateien und Ordner auf dem Laufwerk verschieben.

Mithilfe dieses Handbuchs erfahren, das Nachrichtensystem-Programmiermodell für die Arbeit mit der änderungsnachverfolgung, zeigen Sie einen Beispielcode, und die verschiedenen Arten von Dateivorgängen, die vom nachverfolgt werden **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** funktioniert nur für benutzerbibliotheken, oder für einen beliebigen Ordner auf dem lokalen Computer. Dies schließt sekundären Laufwerke oder Wechseldatenträger aber enthält keine NAS-Laufwerke und Netzwerklaufwerke.

## <a name="using-the-change-tracker"></a>Verwenden die änderungsnachverfolgung

Die änderungsnachverfolgung wird als ein zirkulärer Puffer, der den letzten Speichern System implementiert *N* Dateisystemvorgänge. Apps sind die Änderungen aus dem Puffer gelesen, und klicken Sie dann in ihrer eigenen Erfahrungen verarbeiten können. Wenn die app mit den Änderungen abgeschlossen ist, wird die Änderungen gekennzeichnet, während der Verarbeitung und nie, finden Sie sie erneut ein.

Um die änderungsnachverfolgung in einem Ordner zu verwenden, gehen Sie folgendermaßen vor:

1. Aktivieren der änderungsnachverfolgung für den Ordner.
2. Warten Sie, bis Änderungen.
3. Lesen Sie die Änderungen.
4. Akzeptieren Sie die Änderungen.

In den nächsten Abschnitten führen durch jeden Schritt mit einigen Codebeispielen. Das vollständige Codebeispiel wird am Ende dieses Artikels bereitgestellt.

### <a name="enable-the-change-tracker"></a>Aktivieren der änderungsnachverfolgung

Das System darauf hinzuweisen, dass sie die änderungsnachverfolgung für eine angegebene Bibliothek interessiert ist ist, die die app muss als Erstes. Dies geschieht durch Aufrufen der [ **aktivieren** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) -Methode für die änderungsnachverfolgung für die Bibliothek von Interesse sind.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Einige wichtige Hinweise:

- Stellen Sie sicher, dass Ihre app verfügt über die Berechtigung zur richtigen Bibliothek im Manifest vor dem Erstellen der [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) Objekt. Finden Sie unter [Dateizugriffsberechtigungen](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) Weitere Details.
- [**Aktivieren Sie** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) ist threadsicher, nicht zurückgesetzt, den Zeiger und kann so oft wie (mehr dazu später gewünscht) aufgerufen werden.

![Ein leeres System zur änderungsnachverfolgung aktivieren](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Warten Sie, bis Änderungen

Nachdem die änderungsnachverfolgung initialisiert wurde, beginnt es alle Vorgänge aufzuzeichnen, die in einer Bibliothek auch auftreten, während die app nicht ausgeführt wird. Apps können sich registrieren, um jederzeit aktiviert werden, eine Änderung vorliegt, durch die Registrierung für, die [ **StorageLibraryChangedTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) Ereignis.

![Änderungen, die die änderungsnachverfolgung ohne die app mit dem Lesen hinzugefügt wird](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Lesen Sie die Änderungen

Die app kann dann Abrufen von Änderungen aus der änderungsnachverfolgung und erhalten Sie eine Liste der Änderungen seit dem letzten, die diese Option aktiviert. Der folgende Code zeigt, wie Sie eine Liste der Änderungen aus der änderungsprotokollierung abrufen.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

Die app ist dann für die Verarbeitung der Änderungen in eine eigene Benutzeroberfläche oder die Datenbank, die je nach Bedarf verantwortlich.

![Lesen Sie die Änderungen aus der änderungsprotokollierung, in eine app-Datenbank](images/changetracker-reading.png)

> [!TIP]
> Der zweite Aufruf von aktivieren ist gegen eine Racebedingung verwendet werden soll, wenn der Benutzer einen anderen Ordner in der Bibliothek hinzufügt, während Ihre app Änderungen gelesen wird. Ohne den zusätzlichen Aufruf von **aktivieren** Code Fehler EcSearchFolderScopeViolation (0 x 80070490), wenn der Benutzer die Ordner in ihrer Bibliothek geändert wird

### <a name="accept-the-changes"></a>Die Änderungen übernehmen

Nach Abschluss die app verarbeitet die Änderungen an, es sollte das System informiert, anzuzeigenden nie die Änderungen erneut durch Aufrufen der [ **AcceptChangesAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) Methode.

```csharp
await changeReader.AcceptChangesAsync();
```

![Markieren die Änderungen als lesen, damit sie nicht erneut angezeigt wird](images/changetracker-accepting.png)

Die app jetzt erhalten nur Änderungen, wenn die änderungsnachverfolgung in der Zukunft zu lesen.

- Wenn Änderungen, zwischen dem Aufruf stattgefunden haben [ **ReadBatchAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) und [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), die Zeiger werden nur auf die letzte Änderung, die die app angezeigt werden kann, erweitert werden. Diese anderen Änderungen werden beim nächsten aufgerufen **ReadBatchAsync**.
- Akzeptiert keine die Änderungen werden dazu, dass das System dem gleichen Satz von Änderungen das nächste zurück, die app ruft Uhrzeit **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Wichtige Punkte zu beachten

Wenn Sie die änderungsnachverfolgung zu verwenden, gibt es einige Dinge, die Sie, denken Sie daran berücksichtigen, um sicherzustellen, dass alles ordnungsgemäß funktioniert.

### <a name="buffer-overruns"></a>Pufferüberläufe

Obwohl wir versuchen, reservieren Sie ausreichend Platz in der änderungsprotokollierung durchlief, enthalten alle Vorgänge auf dem System durchgeführt werden, bis Ihre app gelesen werden kann, ist es sehr einfach, stellen Sie sich ein Szenario vor, in dem die Änderungen in die app Lesen nicht, bevor zirkuläre Puffer selbst überschrieben. Insbesondere dann, wenn der Benutzer das Wiederherstellen von Daten aus einer Sicherung oder eine umfangreiche Sammlung von Bildern von seiner smartphonekamera synchronisiert.

In diesem Fall **ReadBatchAsync** gibt den Fehlercode [ **StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Wenn Ihre app mit diesem Fehlercode empfängt, bedeutet dies ein paar Dinge:

* Der Puffer hat sich seit dem letzten überschrieben, die auf diese. Die beste Vorgehensweise ist erneut die Bibliothek, da keine Informationen aus der nachverfolgung unvollständig sein wird.
* Die änderungsnachverfolgung wird keine weitere Änderungen zurück, bis zum Aufruf von [ **zurücksetzen**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Nach dem Zurücksetzen die app aufgerufen wird, wird der Zeiger wird in der die letzte Änderung verschoben und nachverfolgung wird normal fortgesetzt.

Wird nur selten, dass diese Fälle abrufen, aber in Szenarien, in denen der Benutzer eine große Anzahl von Dateien auf ihren Datenträger verschoben wird, die wir die änderungsnachverfolgung erheblich zunehmen kann, und richten Sie zu viel Speicher möchten nicht. Dies sollte auf umfangreiche Dateisystemvorgänge zu reagieren, während nicht beschädigen die benutzerfreundlichkeit in Windows-apps ermöglichen.

### <a name="changes-to-a-storagelibrary"></a>Änderungen an einer StorageLibrary

Die [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) Klasse vorhanden ist, als eine virtuelle Gruppe der Stammordner, der anderen Ordner enthalten. Um dies mit einer Datei System System zur änderungsnachverfolgung abzustimmen, haben wir die folgenden Optionen aus:

- Alle Änderungen an untergeordnete Klasse der Stammordner für die Bibliothek werden in der änderungsprotokollierung durchlief dargestellt. Der Stammordner für die Bibliothek finden Sie mithilfe der [ **Ordner** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) Eigenschaft.
- Hinzufügen oder entfernen die Stammordner von einem **StorageLibrary** (über [ **RequestAddFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) und [ **RequestRemoveFolderAsync**  ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) erstellt einen Eintrag in der änderungsnachverfolgung nicht. Diese Änderungen nachverfolgt werden können, über die [ **DefinitionChanged** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) Ereignis oder durch Aufzählen der Stammordner in der Bibliothek mithilfe der [ **Ordner** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) Eigenschaft.
- Wenn ein Ordner mit Inhalt bereits in der Bibliothek hinzugefügt wird, stehen keine Änderung Benachrichtigung oder Änderung Tracker-Einträge generiert. Alle weiteren Änderungen an die untergeordneten Elemente des diesem Ordner generiert Benachrichtigungen und Tracker-Einträge zu ändern.

### <a name="calling-the-enable-method"></a>Aufrufen der Methode aktivieren

Apps sollten Aufrufen [ **aktivieren** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) , sobald sie beginnen, im Dateisystem nachverfolgen und vor jeder Enumeration der Änderungen. Dadurch wird sichergestellt, dass alle Änderungen, die von der änderungsnachverfolgung erfasst werden sollen.  

## <a name="putting-it-together"></a>Letzte Schritte

Hier wurde der gesamte Code, der verwendet wird, registrieren Sie sich für die Änderungen in der Videobibliothek und starten Sie die Änderungen aus der änderungsnachverfolgung abgerufen.

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
