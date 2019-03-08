---
Description: Erfahren Sie, wie Sie Benachrichtigungen im Info-Center mithilfe von Sammlungen zu gruppieren.
title: Popupsammlungen
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: Windows 10, UWP, Benachrichtigungen, Sammlungen, Sammlung, Gruppenbenachrichtigungen, Gruppieren von Benachrichtigungen, gruppieren, organisieren, Info-Center, Popup
ms.localizationpriority: medium
ms.openlocfilehash: 9b6818f876c094298a0a6636faa00efa9a192545
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600685"
---
# <a name="grouping-toast-notifications-with-collections"></a>Gruppieren von Popupbenachrichtigungen mit Sammlungen
Verwenden Sie Sammlungen, um Ihre App-Popups im Info-Center zu organisieren. Mithilfe von Sammlungen können Benutzer Informationen im Info-Center leichter finden und Entwickler können ihre Benachrichtigungen besser verwalten.  Die folgenden APIs ermöglichen das Entfernen, Erstellen und Aktualisieren von Benachrichtigungssammlungen.

> [!IMPORTANT]
> **Erfordert Creators Update**: Sie müssen SDK 15063 ausgerichtet und Build 15063 oder höher, um den Toast Sammlungen ausgeführt werden. Zugehörige APIs: [Windows.UI.Notifications.ToastCollection](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection) und [Windows.UI.Notifications.ToastCollectionManager](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollectionmanager)

Sie sehen das folgende Beispiel einer Messaging-App, die die Benachrichtigungen basierend auf der Chatgruppe trennt. Jeder Titel (Comp Sci 160A Project Chat, Direct Messages, Lacrosse Team Chat) ist eine separate Sammlung.  Beachten Sie, wie die Benachrichtigungen unterschiedlich gruppiert werden, als wären sie von einer separaten App, obwohl sie alle Benachrichtigungen von derselben App sind.  Wenn Sie Benachrichtigungen auf raffiniertere Art organisieren möchten, lesen Sie [Popup-Header](toast-headers.md).  
![Auflistungsbeispiel für die mit zwei verschiedenen Gruppen von Benachrichtigungen](images/toast-collection-example.png)

## <a name="creating-collections"></a>Erstellen von Sammlungen
Bei der Erstellung jeder Sammlung müssen Sie einen Anzeigenamen und ein Symbol angeben, die als Teil des Sammlungstitels im Info-Center angezeigt werden, wie in der Abbildung oben dargestellt. Sammlungen erfordern außerdem ein Start-Argument, damit die App zur richtigen Stelle in der App navigieren kann, wenn ein Benutzer auf den Titel der Sammlung klickt.  

### <a name="create-a-collection"></a>Erstellen einer Sammlung

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>Senden von Benachrichtigungen an eine Sammlung
Wir betrachten das Senden von Benachrichtigungen aus drei verschiedenen Popup-Pipelines: lokal, geplant und push.  Für jedes dieser Beispiele erstellen wir ein Beispielpopup, das mit dem nachfolgenden Code sofort gesendet wird. Dann konzentrieren wir uns darauf, wie das Popup über jede Pipeline einer Sammlung hinzugefügt wird.

Erstellen der Benachrichtigungsnutzlast:

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>Senden eines Popups an eine Sammlung

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>Hinzufügen eines geplanten Popups zu einer Sammlung

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>Senden eines Push-Popups an eine Sammlung
Für Push-Popups müssen Sie den X-WNS-CollectionId-Header zur POST-Nachricht hinzufügen.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>Verwalten von Sammlungen
#### <a name="create-the-toast-collection-manager"></a>Erstellen der Popupsammlungsverwaltung
Für die verbleibenden Codeausschnitte in diesem Abschnitt „Verwalten von Sammlungen“ verwenden wir den folgenden collectionManager.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>Abrufen aller Sammlungen

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>Abrufen der Anzahl der erstellten Sammlungen

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>Entfernen einer Sammlung

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>Aktualisieren einer Sammlung
Sie können Sammlungen aktualisieren, indem Sie eine neue Sammlung mit derselben ID erstellen und die neue Instanz der Sammlung speichern.
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>Verwalten von Popups in einer Sammlung
#### <a name="group-and-tag-properties"></a>Gruppierungs- und Tag-Eigenschaften
Die Gruppierungs- und Tag-Eigenschaften identifizieren zusammen eine Benachrichtigung in einer Sammlung eindeutig.  Gruppe (und Tag) dienen als zusammengesetzter primärer Schlüssel (mehr als eine ID) zur Identifizierung der Benachrichtigung. Wenn Sie z. B. eine Benachrichtigung entfernen oder ersetzen möchten, müssen Sie angeben können, *welche Benachrichtigung* entfernt/ersetzt werden soll. Dies erreichen Sie durch Angabe von Tag und Gruppe. Ein Beispiel ist eine Messaging-App.  Der Entwickler könnte die Unterhaltungs-ID als Gruppe und die Nachrichten-ID als Tag verwenden.

#### <a name="remove-a-toast-from-a-collection"></a>Entfernen eines Popups aus einer Sammlung
Sie können einzelne Popups mithilfe der Tag- und Gruppen-IDs entfernen oder alle Popups in einer Sammlung löschen.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>Löschen aller Popups in einer Sammlung
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>Sammlungen im Notifications Visualizer
Sie können beim Entwerfen Ihrer Sammlungen das Tool [Notifications Visualizer](notifications-visualizer.md) verwenden. Führen Sie die unten beschriebenen Schritte aus.

* Klicken Sie in der unteren rechten Ecke auf das Zahnradsymbol. 
* Wählen Sie „Popupsammlungen“ aus.
* Über der Vorschau des Popups wird das Dropdownmenü „Popupsammlungen“ angezeigt. Wählen Sie die Option zum Verwalten von Sammlungen aus.
* Klicken Sie auf „Sammlung hinzufügen“, geben Sie die Details zur Sammlung an, und speichern Sie.
* Sie können weitere Sammlungen hinzufügen oder die Option zum Verwalten von Sammlungen deaktivieren, um zum Hauptbildschirm zurückzukehren.
* Wählen Sie im Dropdownmenü „Popupsammlung“ die Sammlung aus, der Sie das Popup hinzufügen möchten.
* Wenn das Popup ausgelöst wird, wird es der entsprechenden Sammlung im Info-Center hinzugefügt.


## <a name="other-details"></a>Weitere Details
Die Popupsammlungen, die Sie erstellen, werden auch in den Benachrichtigungseinstellungen des Benutzers angezeigt.  Benutzer können die Einstellungen für jede einzelne Sammlung umschalten, um diese Untergruppen zu aktivieren oder zu deaktivieren.  Wenn Benachrichtigungen auf der obersten Ebene für die App deaktiviert werden, werden auch alle Sammlungsbenachrichtigungen deaktiviert.  Jede Sammlung zeigt standardmäßig drei Benachrichtigungen im Info-Center an. Der Benutzer kann sie erweitern, um bis zu 20 Benachrichtigungen anzuzeigen.

## <a name="related-topics"></a>Verwandte Themen

* [Inhalt der Popupbenachrichtigung](adaptive-interactive-toasts.md)
* [Toast-Header](toast-headers.md)
* [Benachrichtigungsbibliothek auf GitHub (Teil der Windows-Community-Toolkit)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)