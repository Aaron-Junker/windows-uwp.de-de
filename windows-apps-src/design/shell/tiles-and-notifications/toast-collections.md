---
title: Popup Auflistungen
description: Erfahren Sie, wie Sie Popup Benachrichtigungen für Ihre APP organisieren, indem Sie Benachrichtigungs Sammlungen im Aktions Center erstellen, aktualisieren oder entfernen.
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: Windows 10, UWP, Benachrichtigung, Sammlungen, Sammlung, Gruppen Benachrichtigungen, Gruppierungs Benachrichtigungen, Gruppe, organisieren, Aktions Center, Toast
ms.localizationpriority: medium
ms.openlocfilehash: 7cd99519f7213f85c50a14db0597daa4e10f8360
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156754"
---
# <a name="grouping-toast-notifications-with-collections"></a>Gruppieren von Popup Benachrichtigungen mit Sammlungen
Verwenden Sie Sammlungen zum Organisieren der APP-Auflistungen im Aktions Center. Mithilfe von Sammlungen können Benutzerinformationen im Aktions Center leichter finden und Entwicklern eine bessere Verwaltung Ihrer Benachrichtigungen ermöglichen.  Mit den unten aufgeführten APIs können Sie Benachrichtigungs Sammlungen entfernen, erstellen und aktualisieren.

> [!IMPORTANT]
> **Erfordert Creators Update**: Sie müssen das SDK 15063 als Ziel verwenden und Build 15063 oder höher ausführen, um Popup Auflistungen zu verwenden. Zu den zugehörigen APIs zählen [Windows. UI. Notification. deastcollection](/uwp/api/windows.ui.notifications.toastcollection)und [Windows. UI. Benachrichtigungen.](/uwp/api/windows.ui.notifications.toastcollectionmanager)

Das Beispiel unten zeigt eine Messaging-APP, die die Benachrichtigungen auf der Grundlage der Chatgruppe trennt. Jeder Titel (Comp Sci 160A Project Chat, Direct Messages, Lacrosse Teamchat) ist eine separate Sammlung.  Beachten Sie, dass die Benachrichtigungen eindeutig gruppiert werden, als ob Sie aus einer separaten App stammen, auch wenn Sie alle Benachrichtigungen von derselben APP sind.  Wenn Sie eine detailliertere Methode zum Organisieren von Benachrichtigungen suchen, finden Sie weitere Informationen unter Popup [Header](toast-headers.md).  
![Sammlungs Beispiel mit zwei verschiedenen Gruppen von Benachrichtigungen](images/toast-collection-example.png)

## <a name="creating-collections"></a>Erstellen von Sammlungen
Wenn Sie jede Sammlung erstellen, müssen Sie einen anzeigen Amen und ein Symbol angeben, die im Aktions Center als Teil des Sammlungs Titels angezeigt werden, wie in der Abbildung oben gezeigt. Auflistungen benötigen auch ein Launch-Argument, um der APP zu helfen, zur richtigen Position innerhalb der APP zu navigieren, wenn der Benutzer auf den Titel der Sammlung klickt.  

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
Wir werden das Senden von Benachrichtigungen aus drei verschiedenen Popup Pipelines abdecken: local, scheduled und Push.  Für jedes dieser Beispiele wird ein Beispiel für den Toast erstellt, der mit dem Code direkt unterhalb von gesendet werden soll. anschließend konzentrieren wir uns auf das Hinzufügen des Popups zu einer Sammlung über jede Pipeline.

Erstellen Sie die Benachrichtigungs Nutzlast:

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

### <a name="send-a-toast-to-a-collection"></a>Einen Toast an eine Sammlung senden

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

### <a name="send-a-push-toast-to-a-collection"></a>Pushtoast an eine Sammlung senden
Für pushtoasts müssen Sie den Header "X-WNS-CollectionId" der Post-Nachricht hinzufügen.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>Verwalten von Sammlungen
#### <a name="create-the-toast-collection-manager"></a>Erstellen des Popup-Sammlungs-Managers
Für den Rest der Code Ausschnitte im Abschnitt "Verwalten von Sammlungen" wird der folgende collectionmanager verwendet.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>Alle Sammlungen abrufen

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>Anzahl der erstellten Sammlungen erhalten

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>Entfernen einer Sammlung

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>Aktualisieren einer Sammlung
Sie können Sammlungen aktualisieren, indem Sie eine neue Sammlung mit der gleichen ID erstellen und die neue Instanz der Sammlung speichern.
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
## <a name="managing-toasts-within-a-collection"></a>Verwalten von Einfassungen innerhalb einer Sammlung
#### <a name="group-and-tag-properties"></a>Gruppen-und Tageigenschaften
Die Gruppen-und Tageigenschaften identifizieren eine Benachrichtigung innerhalb einer Sammlung eindeutig.  Die Gruppe (und das Tag) dient als zusammengesetzter Primärschlüssel (mehr als einen Bezeichner) zum Identifizieren der Benachrichtigung. Wenn Sie z. b. eine Benachrichtigung entfernen oder ersetzen möchten, müssen Sie in der Lage sein, die *Benachrichtigung* anzugeben, die Sie entfernen/ersetzen möchten; Dies erreichen Sie, indem Sie das Tag und die Gruppe angeben. Ein Beispiel hierfür ist eine Messaging-app.  Der Entwickler kann die Konversations-ID als Gruppe und die Nachrichten-ID als-Tag verwenden.

#### <a name="remove-a-toast-from-a-collection"></a>Entfernen eines Toast aus einer Sammlung
Sie können einzelne-Auflistungen mithilfe der Tag-und Gruppen-IDs entfernen oder alle-Auflistungen in einer Sammlung löschen.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>Alle Auflistungen innerhalb einer Sammlung löschen
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>Sammlungen in der Benachrichtigungs Schnellansicht
Sie können das [Benachrichtigungs](notifications-visualizer.md) schnell Ansichts Tool zum Entwerfen von Sammlungen verwenden. Führen Sie diese Schritte aus:

* Klicken Sie in der unteren rechten Ecke auf das Zahnrad Symbol. 
* Wählen Sie "Popup Sammlungen" aus.
* Oberhalb der Vorschau des Popup befindet sich das Dropdown Menü "Popup Sammlung". Wählen Sie Sammlungen verwalten aus.
* Klicken Sie auf "Sammlung hinzufügen", geben Sie die Details für die Sammlung ein, und speichern Sie Sie.
* Sie können weitere Sammlungen hinzufügen oder auf aus dem Feld Sammlungen verwalten klicken, um zum Hauptbildschirm zurückzukehren.
* Wählen Sie im Dropdown Menü "Popup Sammlung" die Sammlung aus, der Sie den Popup hinzufügen möchten.
* Wenn Sie den Popup auslösen, wird er der entsprechenden Sammlung im Aktions Center hinzugefügt.


## <a name="other-details"></a>Weitere Details
Die von Ihnen erstellten Popup Auflistungen werden auch in den Benachrichtigungseinstellungen des Benutzers berücksichtigt.  Benutzer können die Einstellungen für jede einzelne Sammlung umschalten, um diese Untergruppen ein-oder auszuschalten.  Wenn Benachrichtigungen auf der obersten Ebene der APP deaktiviert werden, werden alle Sammlungs Benachrichtigungen ebenfalls ausgeschaltet.  Außerdem werden für jede Sammlung standardmäßig 3 Benachrichtigungen im Aktions Center angezeigt, und der Benutzer kann ihn erweitern, sodass bis zu 20 Benachrichtigungen angezeigt werden.

## <a name="related-topics"></a>Zugehörige Themen

* [Popupinhalt](adaptive-interactive-toasts.md)
* [Popup Header](toast-headers.md)
* [Benachrichtigungs Bibliothek auf GitHub (Teil des Windows Community Toolkit)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)