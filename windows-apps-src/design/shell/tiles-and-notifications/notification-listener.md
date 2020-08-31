---
Description: Erfahren Sie, wie Sie mit dem benachrichtigungslistener auf alle Benachrichtigungen des Benutzers zugreifen.
title: Benachrichtigungs Listener
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, UWP, benachrichtigungslistener, usernotificationlistener, Dokumentation, Zugriffs Benachrichtigungen
ms.localizationpriority: medium
ms.openlocfilehash: dc2afb36337439cd115273cd9df8ee1cb2eb3741
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169184"
---
# <a name="notification-listener-access-all-notifications"></a>Benachrichtigungslistener: Zugriff auf alle Benachrichtigungen

Der benachrichtigungslistener ermöglicht den Zugriff auf die Benachrichtigungen eines Benutzers. Smartwatches und andere Wearables können den benachrichtigungslistener verwenden, um die Telefon Benachrichtigungen an das tragbaren-Gerät zu senden. Home Automation-Apps können benachrichtigungslistener verwenden, um beim Empfang von Benachrichtigungen bestimmte Aktionen auszuführen, z. b. das Blinken der Beleuchtung beim Empfang eines Aufrufes. 

> [!IMPORTANT]
> **Erfordert Anniversary Update**: Sie müssen das SDK 14393 als Ziel verwenden und Build 14393 oder höher ausführen, um den benachrichtigungslistener zu verwenden.


> **Wichtige APIs**: [usernotificationlistener-Klasse](/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [usernotificationchangedauslöserklasse](/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>Aktivieren Sie den Listener durch Hinzufügen der Benutzer Benachrichtigungsfunktion. 

Um den benachrichtigungslistener zu verwenden, müssen Sie dem App-Manifest die Funktion "benutzerbenachrichtigungslistener"

1. Doppelklicken Sie in Visual Studio im Projektmappen-Explorer auf die `Package.appxmanifest` Datei, um den Manifest-Designer zu öffnen.
2. Öffnen Sie die Registerkarte Funktionen.
3. Überprüfen Sie die **benutzerbenachrichtigungslistener**


## <a name="check-whether-the-listener-is-supported"></a>Überprüfen, ob der Listener unterstützt wird

Wenn Ihre APP ältere Versionen von Windows 10 unterstützt, müssen Sie die [apiinformation-Klasse](/uwp/api/Windows.Foundation.Metadata.ApiInformation) verwenden, um zu überprüfen, ob der Listener unterstützt wird.  Wenn der Listener nicht unterstützt wird, sollten Sie keine Aufrufe der listenerapis ausführen.

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


## <a name="requesting-access-to-the-listener"></a>Anfordern des Zugriffs auf den Listener

Da der Listener den Zugriff auf die Benutzer Benachrichtigungen zulässt, müssen die Benutzer Ihrer APP die Berechtigung erteilen, auf Ihre Benachrichtigungen zuzugreifen. Während der ersten Laufzeiten Ihrer APP sollten Sie den Zugriff für die Verwendung des Benachrichtigungs-Listener anfordern. Wenn Sie möchten, können Sie einige vorläufige Benutzeroberflächen anzeigen, in denen erläutert wird, warum Ihre APP Zugriff auf die Benutzer Benachrichtigungen benötigt, bevor Sie [requestaccessasync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync)aufrufen, sodass der Benutzer versteht, warum er den Zugriff erlauben soll.

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

Der Benutzer kann den Zugriff jederzeit über die Windows-Einstellungen widerrufen. Daher sollte Ihre APP den Zugriffsstatus stets über die [getaccessstatus](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) -Methode überprüfen, bevor Sie Code ausführen, der den benachrichtigungslistener verwendet. Wenn der Benutzer den Zugriff widerruft, schlagen die APIs automatisch fehl, anstatt eine Ausnahme auszulösen (z. b. gibt die API zum Abrufen aller Benachrichtigungen einfach eine leere Liste zurück).


## <a name="access-the-users-notifications"></a>Zugreifen auf die Benachrichtigungen des Benutzers

Mit dem benachrichtigungslistener können Sie eine Liste der aktuellen Benachrichtigungen des Benutzers erhalten. Aufrufen Sie einfach die [getnotificationsasync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) -Methode, und geben Sie den Typ der Benachrichtigungen an, die abgerufen werden sollen (derzeit werden nur Popup Benachrichtigungen unterstützt).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>Anzeigen der Benachrichtigungen

Jede Benachrichtigung wird als [usernotification](/uwp/api/windows.ui.notifications.usernotification)dargestellt, das Informationen über die APP bereitstellt, von der aus die Benachrichtigung stammt, den Zeitpunkt, zu dem die Benachrichtigung erstellt wurde, die ID der Benachrichtigung und die Benachrichtigung selbst.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

Die [appinfo](/uwp/api/windows.ui.notifications.usernotification.AppInfo) -Eigenschaft enthält die Informationen, die Sie zum Anzeigen der Benachrichtigung benötigen.

> [!NOTE]
> Es wird empfohlen, den gesamten Code für die Verarbeitung einer einzelnen Benachrichtigung in einem try/catch-Vorgang zu umgeben, falls eine unerwartete Ausnahme auftritt, wenn Sie eine einzelne Benachrichtigung erfassen. Sie sollten nicht vollständig fehlschlagen, um andere Benachrichtigungen nur aufgrund eines Problems mit einer bestimmten Benachrichtigung anzuzeigen.

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

Der Inhalt der Benachrichtigung selbst (z. b. der Benachrichtigungs Text) ist in der [Benachrichtigungs](/uwp/api/windows.ui.notifications.usernotification.Notification) Eigenschaft enthalten. Diese Eigenschaft enthält den visuellen Teil der Benachrichtigung. (Wenn Sie mit dem Senden von Benachrichtigungen unter Windows vertraut sind, werden Sie feststellen, dass die Eigenschaften " [Visual](/uwp/api/windows.ui.notifications.notification.Visual) " und " [Visual.](/uwp/api/windows.ui.notifications.notificationvisual.Bindings) Binding" im [Benachrichtigungs](/uwp/api/windows.ui.notifications.notification) Objekt den Anforderungen entsprechen, die Entwickler beim pping einer Benachrichtigung senden.)

Wir möchten nach der Toast Bindung suchen (bei fehlerfestem Code sollten Sie überprüfen, ob die Bindung nicht NULL ist). Aus der Bindung können Sie die Textelemente abrufen. Sie können beliebig viele Textelemente anzeigen. (Im Idealfall sollten Sie alle anzeigen.) Sie können auswählen, dass die Textelemente anders behandelt werden sollen. behandeln Sie z. b. die erste als Titeltext und nachfolgende Elemente als Textkörper Text.

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


## <a name="remove-a-specific-notification"></a>Entfernen einer bestimmten Benachrichtigung

Wenn die tragbaren oder der Dienst dem Benutzer das Verwerfen von Benachrichtigungen ermöglicht, können Sie die tatsächliche Benachrichtigung entfernen, sodass der Benutzer Sie später nicht mehr auf dem Telefon oder PC sehen kann. Geben Sie einfach die Benachrichtigungs-ID (abgerufen aus dem [usernotification](/uwp/api/windows.ui.notifications.usernotification) -Objekt) der Benachrichtigung an, die Sie entfernen möchten: 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>Alle Benachrichtigungen löschen

Mit der [usernotificationlistener. clearnotification](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) -Methode werden alle Benachrichtigungen des Benutzers gelöscht. Verwenden Sie diese Methode mit Vorsicht. Sie sollten nur alle Benachrichtigungen löschen, wenn die tragbaren oder der Dienst alle Benachrichtigungen anzeigt. Wenn die tragbaren oder der Dienst nur bestimmte Benachrichtigungen anzeigt und der Benutzer auf die Schaltfläche "Benachrichtigungen löschen" klickt, erwartet der Benutzer nur, dass die Benachrichtigungen entfernt werden. das Aufrufen der [clearnotification](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) -Methode würde jedoch bewirken, dass alle Benachrichtigungen, einschließlich derjenigen, die von ihrer tragbaren oder dem Dienst nicht angezeigt wurden, entfernt werden.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>Hintergrundaufgabe zum Hinzufügen/Verwerfen von Benachrichtigungen

Eine gängige Methode, um einer APP das Lauschen auf Benachrichtigungen zu ermöglichen, ist das Einrichten einer Hintergrundaufgabe, damit Sie wissen, wann eine Benachrichtigung hinzugefügt oder verworfen wurde, unabhängig davon, ob Ihre APP aktuell ausgeführt wird.

Dank des [einzelnen Prozessmodells](../../../launch-resume/create-and-register-an-inproc-background-task.md) , das in der Anniversary Update-Aktualisierung hinzugefügt wurde, ist das Hinzufügen von Hintergrundaufgaben recht einfach. Nachdem Sie den Benutzer Zugriff auf den benachrichtigungslistener erhalten und Zugriff zum Ausführen von Hintergrundaufgaben erhalten haben, registrieren Sie einfach eine neue Hintergrundaufgabe, und legen Sie [usernotificationchangedauslöst](/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) mithilfe der Popup [Benachrichtigungs Art](/uwp/api/windows.ui.notifications.notificationkinds)fest.

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

Überschreiben Sie anschließend in Ihrer APP.XAML.cs die [onbackgroundaktivierte](/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) Methode, wenn Sie dies noch nicht getan haben, und verwenden Sie eine Switch-Anweisung für den Aufgaben Namen, um zu bestimmen, welcher der vielen Hintergrund Task Trigger aufgerufen wurde.

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

Bei der Hintergrundaufgabe handelt es sich einfach um eine "Schulter Tap": Sie stellt keine Informationen darüber bereit, welche bestimmte Benachrichtigung hinzugefügt oder entfernt wurde. Wenn Ihre Hintergrundaufgabe ausgelöst wird, sollten Sie die Benachrichtigungen auf ihrer tragbaren synchronisieren, damit Sie die Benachrichtigungen auf der Plattform widerspiegeln. Dadurch wird sichergestellt, dass bei einem Fehler bei der Hintergrundaufgabe die Benachrichtigungen auf ihrer tragbaren weiterhin wieder hergestellt werden können, wenn die Hintergrundaufgabe das nächste Mal ausgeführt wird.

`SyncNotifications` ist eine Methode, die Sie implementieren. im nächsten Abschnitt wird gezeigt, wie. 


## <a name="determining-which-notifications-were-added-and-removed"></a>Ermitteln der hinzugefügten und entfernten Benachrichtigungen

Wenn Sie in Ihrer- `SyncNotifications` Methode ermitteln möchten, welche Benachrichtigungen hinzugefügt oder entfernt wurden (Synchronisieren von Benachrichtigungen mit ihrer Wearable), müssen Sie das Delta zwischen der aktuellen Benachrichtigungs Sammlung und den Benachrichtigungen in der Plattform berechnen.

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


## <a name="foreground-event-for-notification-addeddismissed"></a>Vordergrund Ereignis für hinzugefügte/verwerfen Benachrichtigung

> [!IMPORTANT] 
> Bekanntes Problem: in Builds vor Build 17763/Oktober 2018 Update/Version 1809 verursacht das Vordergrund Ereignis eine CPU-Schleife und/oder funktioniert nicht. Wenn Sie Unterstützung für diese früheren Builds benötigen, verwenden Sie stattdessen die Hintergrundaufgabe.

Sie können auch Benachrichtigungen von einem in-Memory-Ereignishandler überwachen...

```csharp
// Subscribe to foreground event
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // Your code for handling the notification
}
```


## <a name="howto-fixdelays-in-the-background-task"></a>Beheben von Verzögerungen in der Hintergrundaufgabe

Wenn Sie Ihre APP testen, bemerken Sie möglicherweise, dass die Hintergrundaufgabe manchmal verzögert ist und nicht mehrere Minuten lang auslöst. Um die Verzögerung zu beheben, fordern Sie den Benutzer auf, zu den Systemeinstellungen zu wechseln-> System > Akku-> Akku Nutzung nach APP, suchen Sie Ihre APP in der Liste, wählen Sie Sie aus, und legen Sie Sie auf "immer zugelassen im Hintergrund" fest.Danach sollte die Hintergrundaufgabe immer innerhalb von ungefähr einer Sekunde der empfangenen Benachrichtigung ausgelöst werden.