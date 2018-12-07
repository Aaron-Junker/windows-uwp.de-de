---
Description: Learn how to use Notification Listener to access all of the user's notifications.
title: Notification-Listener
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows10, Uwp, notification listener, Usernotificationlistener, Dokumentation, Zugriff auf Benachrichtigungen
ms.localizationpriority: medium
ms.openlocfilehash: de1032eb3d0d364a62beff0a1af8f84240c11d87
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "8807462"
---
# <a name="notification-listener-access-all-notifications"></a>Notification-Listener: Zugriff auf alle Benachrichtigungen

Der Notification-Listener ermöglicht den Zugriff auf die Benachrichtigungen eines Benutzers. Smartwatches und andere Wearables können die Benachrichtigungsfunktion nutzen, um die Benachrichtigungen des Telefons an das tragbare Gerät zu senden. Die heimautomation können Notification-Listener verwenden, um bestimmte Aktionen auszuführen, wenn Benachrichtigungen, z. B. blinken empfangen werden, wenn Sie einen Anruf erhalten. 

> [!IMPORTANT]
> **Benötigt Anniversary Update**: Sie müssen das SDK 14393 als Ziel und Build 14393 oder höher ausführen, um Notification-Listener verwenden zu können.


> **Wichtige APIs**: [UserNotificationListener-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [UserNotificationChangedTrigger-Klasse](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>Aktivieren des Listeners durch Hinzufügen der User Notification-Funktion 

Um den Notification-Listener verwenden zu können, müssen Sie die User Notification Listener-Funktion zu Ihrem Anwendungsmanifest hinzufügen.

1. Klicken Sie in Visual Studio im Projektmappen-Explorer doppelt auf Ihre `Package.appxmanifest`-Datei, um den Manifestdesigner zu öffnen.
2. Öffnen Sie die Registerkarte „Funktionen“.
3. Überprüfen Sie die **User Notification Listener**-Funktion.


## <a name="check-whether-the-listener-is-supported"></a>Prüfen, ob der Listener unterstützt wird

Wenn Ihre App ältere Versionen von Windows 10 unterstützt, müssen Sie die [ApiInformation](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation)-Klasse verwenden, um zu überprüfen, ob der Listener unterstützt wird.  Wenn der Listener nicht unterstützt wird, vermeiden Sie Aufrufe der Listener-APIs.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Notifications.Management.UserNotificationListener"))
{
    // Listener supported!
}
 
else
{
    // Older version of Windows, no Listener
}
```


## <a name="requesting-access-to-the-listener"></a>Zugriff auf den Listener anfordern

Da der Listener den Zugriff auf die Benachrichtigungen des Benutzers erlaubt, müssen Benutzer Ihrer App Zugriffsberechtigung für die Benachrichtigungen erteilen. Während der ersten Ausführung Ihrer App sollten Sie den Zugriff beantragen, um den Notification-Listener zu verwenden. Wenn Sie möchten, können Sie eine Vorab-Benutzeroberfläche anzeigen, die erklärt, warum Ihre Anwendung Zugriff auf die Benachrichtigungen des Benutzers benötigt, bevor Sie [RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync) aufrufen, damit der Benutzer versteht, warum er Zugriff erlauben sollte.

```csharp
// Get the listener
UserNotificationListener listener = UserNotificationListener.Current;
 
// And request access to the user's notifications (must be called from UI thread)
UserNotificationListenerAccessStatus accessStatus = await listener.RequestAccessAsync();
 
switch (accessStatus)
{
    // This means the user has granted access.
    case UserNotificationListenerAccessStatus.Allowed:
 
        // Yay! Proceed as normal
        break;
 
    // This means the user has denied access.
    // Any further calls to RequestAccessAsync will instantly
    // return Denied. The user must go to the Windows settings
    // and manually allow access.
    case UserNotificationListenerAccessStatus.Denied:
 
        // Show UI explaining that listener features will not
        // work until user allows access.
        break;
 
    // This means the user closed the prompt without
    // selecting either allow or deny. Further calls to
    // RequestAccessAsync will show the dialog again.
    case UserNotificationListenerAccessStatus.Unspecified:
 
        // Show UI that allows the user to bring up the prompt again
        break;
}
```

Der Benutzer kann den Zugriff jederzeit über Windows-Einstellungen widerrufen. Aus diesem Grund sollten Ihre app immer den Zugriffsstatus über die [GetAccessStatus](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) -Methode vor der Ausführung von Code, der den Notification-Listener verwendet. Wenn der Benutzer den Zugriff widerruft, schlagen die APIs stillschweigend fehl, anstatt eine Ausnahme auszulösen (z. B. gibt die API zum Abrufen aller Benachrichtigungen einfach eine leere Liste zurück).


## <a name="access-the-users-notifications"></a>Zugriff auf Benachrichtigungen des Benutzers

Mit dem Listener können Sie eine Liste der aktuellen Benachrichtigungen des Benutzers erhalten. Rufen Sie einfach die Methode [GetNotificationsAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) auf und geben Sie den Typ der Benachrichtigungen an, die Sie erhalten möchten (derzeit werden nur Popup-Benachrichtigungen unterstützt).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>Anzeigen der Benachrichtigungen

Jede Benachrichtigung wird als [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification) dargestellt, die Informationen über die App, von der die Benachrichtigung stammt, den Zeitpunkt der Erstellung der Benachrichtigung, die Benachrichtigungs-ID und die Benachrichtigung selbst enthält.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

Die [AppInfo](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.AppInfo) -Eigenschaft enthält die Informationen, die mit der Benachrichtigung angezeigt werden sollen.

> [!NOTE]
> Wir empfehlen, Ihren gesamten Code für die Verarbeitung einer einzelnen Benachrichtigung von einem Try/Catch zu umschließen, falls beim Erfassen einer einzelnen Benachrichtigung eine unerwartete Ausnahme auftritt. Das Anzeigen von anderen Benachrichtigungen sollte nicht komplett fehlschlagen, nur weil ein Problem mit einer bestimmten Benachrichtigung aufgetreten ist.

```csharp
// Select the first notification
UserNotification notif = notifs[0];
 
// Get the app's display name
string appDisplayName = notif.AppInfo.DisplayInfo.DisplayName;
 
// Get the app's logo
BitmapImage appLogo = new BitmapImage();
RandomAccessStreamReference appLogoStream = notif.AppInfo.DisplayInfo.GetLogo(new Size(16, 16));
await appLogo.SetSourceAsync(await appLogoStream.OpenReadAsync());
```

Der Inhalt der Benachrichtigung selbst, wie z. B. der Benachrichtigungstext, ist in der Eigenschaft [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.Notification) enthalten. Diese Eigenschaft enthält den visuellen Teil der Benachrichtigung. (Wenn Sie mit dem Senden von Benachrichtigungen unter Windows vertraut sind, werden Sie feststellen, dass die [Visual](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification.Visual)- und [Visual Bindings](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationvisual.Bindings)-Eigenschaften im [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification)-Objekt dem entsprechen, was Entwickler beim anzeigen einer Benachrichtigung senden.)

Wir wollen nach der Toast-Bindung suchen (für fehlersicheren Code sollten Sie überprüfen, ob die Bindung nicht null ist). Aus der Bindung können Sie die Textelemente entnehmen. Sie können sich beliebig viele Textelemente anzeigen lassen. (Im Idealfall sollten Sie alle anzeigen.) Sie können die Textelemente unterschiedlich behandeln, z. B. das erste als Titeltext und nachfolgende Elemente als Fließtext.

```csharp
// Get the toast binding, if present
NotificationBinding toastBinding = notif.Notification.Visual.GetBinding(KnownNotificationBindings.ToastGeneric);
 
if (toastBinding != null)
{
    // And then get the text elements from the toast binding
    IReadOnlyList<AdaptiveNotificationText> textElements = toastBinding.GetTextElements();
 
    // Treat the first text element as the title text
    string titleText = textElements.FirstOrDefault()?.Text;
 
    // We'll treat all subsequent text elements as body text,
    // joining them together via newlines.
    string bodyText = string.Join("\n", textElements.Skip(1).Select(t => t.Text));
}
```


## <a name="remove-a-specific-notification"></a>Eine bestimmte Benachrichtigung entfernen

Wenn Ihr Wearable oder Service es dem Benutzer erlaubt, Benachrichtigungen zu schließen, können Sie die Benachrichtigung entfernen, so dass der Benutzer sie später nicht mehr auf seinem Telefon oder PC sieht. Geben Sie einfach die Benachrichtigungs-ID (die Sie vom Objekt [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification) erhalten haben) der Benachrichtigung an, die Sie entfernen möchten: 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>Alle Benachrichtigungen löschen

Die Methode [UserNotificationListener.ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) löscht alle Benachrichtigungen des Benutzers. Verwenden Sie diese Methode mit Vorsicht. Sie sollten nur dann alle Benachrichtigungen löschen, wenn in Ihrem Wearable oder Service ALLE Benachrichtigungen angezeigt werden. Wenn Ihr Wearable oder Service nur bestimmte Benachrichtigungen anzeigt, wenn der Benutzer auf die Schaltfläche „?Benachrichtigungen löschen“ klickt, erwartet der Benutzer lediglich, dass bestimmte Benachrichtigungen entfernt werden. Wenn Sie jedoch die [ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications)-Methode aufrufen, werden tatsächlich alle Benachrichtigungen, einschließlich der Benachrichtigungen, die nicht angezeigt werden, entfernt.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>Hintergrundaufgabe für hinzugefügte/geschlossene Benachrichtigung

Eine gängige Methode, um eine Anwendung zum Abrufen von Benachrichtigungen zu erstellen, ist die Einrichtung einer Hintergrundaufgabe, damit Sie wissen, wann eine Benachrichtigung hinzugefügt oder geschlossen wurde, unabhängig davon, ob Ihre Anwendung gerade läuft.

Dank des [gemeinsamen Prozessmodells](../../../launch-resume/create-and-register-an-inproc-background-task.md), das im Anniversary Update hinzugefügt wurde, ist das Hinzufügen von Hintergrundaufgaben relativ einfach. Nachdem Sie im Code Ihrer Hauptanwendung den Zugriff des Benutzers auf Notification-Listener erhalten haben und Zugriff auf die Ausführung von Hintergrundaufgaben erhalten haben, registrieren Sie einfach eine neue Hintergrundaufgabe und setzen Sie den [UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) mit der [Toast-Benachrichtigung](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationkinds).

```csharp
// TODO: Request/check Listener access via UserNotificationListener.Current.RequestAccessAsync
 
// TODO: Request/check background task access via BackgroundExecutionManager.RequestAccessAsync
 
// If background task isn't registered yet
if (!BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals("UserNotificationChanged")))
{
    // Specify the background task
    var builder = new BackgroundTaskBuilder()
    {
        Name = "UserNotificationChanged"
    };
 
    // Set the trigger for Listener, listening to Toast Notifications
    builder.SetTrigger(new UserNotificationChangedTrigger(NotificationKinds.Toast));
 
    // Register the task
    builder.Register();
}
```

Dann überschreiben Sie in Ihrer App.xaml.cs die [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.OnBackgroundActivated)-Methode, falls Sie es noch nicht getan haben, und verwenden eine Switch-Anweisung für den Tasknamen, um festzustellen, welche Ihrer vielen Trigger für Hintergrundaufgaben aufgerufen wurde.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

Die Hintergrundaufgabe ist lediglich ein Hinweis: Sie gibt keine Auskunft darüber, welche Benachrichtigung hinzugefügt oder entfernt wurde. Wenn Ihre Hintergrundaufgabe ausgelöst wird, sollten Sie die Benachrichtigungen auf Ihrem Wearable synchronisieren, damit sie die Benachrichtigungen in der Plattform widerspiegeln. Wenn Ihre Hintergrundaufgabe fehlschlägt, können Sie beim nächsten Ausführen Ihrer Hintergrundaufgabe immer noch Benachrichtigungen über Ihre Wearable wiederherstellen.

`SyncNotifications` ist eine Methode, die Sie implementieren. Der nächste Abschnitt zeigt wie. 


## <a name="determining-which-notifications-were-added-and-removed"></a>Ermitteln welche Benachrichtigungen hinzugefügt und entfernt wurden

In Ihrer `SyncNotifications`-Methode müssen Sie das Delta zwischen Ihrer aktuellen Benachrichtigungssammlung und den Benachrichtigungen in der Plattform berechnen (Benachrichtigungen mit Ihrem Wearable synchronisieren), um festzustellen, welche Benachrichtigungen hinzugefügt oder entfernt wurden.

```csharp
// Get all the current notifications from the platform
IReadOnlyList<UserNotification> userNotifications = await listener.GetNotificationsAsync(NotificationKinds.Toast);
 
// Obtain the notifications that our wearable currently has displayed
IList<uint> wearableNotificationIds = GetNotificationsOnWearable();
 
// Copy the currently displayed into a list of notification ID's to be removed
var toBeRemoved = new List<uint>(wearableNotificationIds);
 
// For each notification in the platform
foreach (UserNotification userNotification in userNotifications)
{
    // If we've already displayed this notification
    if (wearableNotificationIds.Contains(userNotification.Id))
    {
        // We want to KEEP it displayed, so take it out of the list
        // of notifications to remove.
        toBeRemoved.Remove(userNotification.Id);
    }
 
    // Otherwise it's a new notification
    else
    {
        // Display it on the Wearable
        SendNotificationToWearable(userNotification);
    }
}
 
// Now our toBeRemoved list only contains notification ID's that no longer exist in the platform.
// So we will remove all those notifications from the wearable.
foreach (uint id in toBeRemoved)
{
    RemoveNotificationFromWearable(id);
}
```


## <a name="foreground-event-for-notification-addeddismissed"></a>Vordergrundereignis für hinzugefügt/geschlossene Benachrichtigungen

> [!IMPORTANT] 
> Bekanntes Problem: das Ereignis im Vordergrund bewirkt eine Schleife CPU auf die aktuellen Versionen von Windows und zuvor vor, die nicht funktioniert. Verwenden Sie nicht das Ereignis im Vordergrund. Ein bevorstehenden Windows Update werden wir dieses Problem beheben.

Verwenden Sie anstelle der vordergrundereignis für eine Hintergrundaufgabe [einzelprozessmodell](../../../launch-resume/create-and-register-an-inproc-background-task.md) den zuvor aufgeführten Code. Die Hintergrundaufgabe zudem können Sie ereignisbenachrichtigungen beide zu erhalten, während die app geschlossen ist oder ausgeführt wird.

```csharp
// Subscribe to foreground event (DON'T USE THIS)
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // NOTE: This event WILL CAUSE CPU LOOPS, DO NOT USE. Use the background task instead.
}
```


## <a name="howto-fixdelays-in-the-background-task"></a>So wird's gemacht-Fixdelays in der Hintergrundaufgabe

Wenn Sie Ihre app zu testen, werden Sie möglicherweise feststellen, dass die Hintergrundaufgabe manchmal verzögert ist und mehrere Minuten lang nicht ausgelöst. Um die Verzögerung zu beheben, an die Benutzer Togo auf den Systemeinstellungen -> System -> Akku -> Akkunutzung nach app, Ihre app in der Liste finden, auszuwählen, und legen sie "immer im Hintergrund zugelassen."Danach sollte die Hintergrundaufgabe immer innerhalb einer Sekunde der Benachrichtigung angestoßen werden.
