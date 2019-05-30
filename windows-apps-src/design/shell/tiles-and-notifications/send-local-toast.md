---
Description: Hier erfahren Sie, wie Sie eine lokale Popupbenachrichtigung senden und mit dem Benutzer umgehen, der auf das Popup klickt.
title: Senden einer lokalen Popupbenachrichtigung
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, Senden von Popupbenachrichtigungen, Benachrichtigungen, Benachrichtigungen senden, Popupbenachrichtigungen, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, exemplarische Vorgehensweise
ms.localizationpriority: medium
ms.openlocfilehash: 4c8def5d1a220fe0458dd5c244d875b991b2b76c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365930"
---
# <a name="send-a-local-toast-notification"></a>Senden einer lokalen Popupbenachrichtigung


Eine Popupbenachrichtigung ist eine Nachricht, die eine App erstellen und an den Benutzer übermitteln kann, während diese(r) Ihre App aktuell nicht verwendet. Diese Schnellstartanleitung führt Sie durch die Schritte der Erstellung, Übermittlung und Anzeige einer Windows 10-Popupbenachrichtigung mit den neuen adaptiven Vorlagen und interaktiven Aktionen. Diese Aktionen werden mit einer lokalen Benachrichtigung demonstriert – der einfachsten Benachrichtigung, die Sie implementieren können.

> [!IMPORTANT]
> Desktopanwendungen (Desktop-Brücke- und klassische Win32-Apps) haben verschiedene Schritte zum Senden von Benachrichtigungen und Behandeln der Aktivierung. Weitere Informationen zum Implementieren von Popups finden Sie in der Dokumentation zu [Desktop-Apps](toast-desktop-apps.md).

Wir werden die folgenden Schritte durchlaufen:

### <a name="sending-a-toast"></a>Senden eines Popups

* Erstellen des visuellen Teils (Text und Bilder) der Benachrichtigung
* Hinzufügen von Aktionen zur Benachrichtigung
* Festlegen einer Ablaufzeit für das Popup
* Festlegen von Tag/Group, sodass Sie das Popup zu einem späteren Zeitpunkt ersetzen/entfernen können
* Senden Ihres Popups mithilfe der lokalen APIs

### <a name="handling-activation"></a>Behandeln der Aktivierung

* Behandeln der Aktivierung beim Klicken auf den Text oder Schaltflächen
* Behandeln der Vordergrundaktivierung
* Behandeln der Hintergrundaktivierung

> **Wichtige APIs:** [ToastNotification Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification), [ToastNotificationActivatedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>Vorraussetzungen

Um dieses Thema vollständig zu verstehen, ist Folgendes hilfreich...

* Ein grundlegendes Verständnis der Begriffe und Konzepte rund um Popupbenachrichtigungen. Weitere Informationen finden Sie unter [Popup- und Aktion center Overview](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Kenntnisse in Bezug auf den Inhalt von Windows 10-Popupbenachrichtigungen. Weitere Informationen finden Sie in der [Dokumentation zu Popup-Inhalt](adaptive-interactive-toasts.md).
* Ein Windows 10-UWP-App-Projekt

> [!NOTE]
> Im Gegensatz zu Windows 8/8.1 müssen Sie in Ihrem App-Manifest nicht mehr deklarieren, dass Ihre App Popupbenachrichtigungen anzeigen kann. Alle Apps können Popupbenachrichtigungen senden und anzeigen.

> [!NOTE]
> **Windows 8/8.1-apps**: Verwenden Sie die [archivierte Dokumentation](https://docs.microsoft.com/previous-versions/windows/apps/hh868254(v=win.10)).


## <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen

Wir empfehlen die Installation der beiden folgenden NuGet-Pakete für Ihr Projekt. In unserem Codebeispiel werden diese Pakete verwendet. Am Ende des Artikels finden Sie die „Vanilla“-Codeausschnitte, die keine NuGet-Pakete verwenden.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): Toast-Nutzlasten über Objekte anstelle von unformatierten XML-Daten zu generieren.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): Generieren und Analysieren von Abfragezeichenfolgen mitC#


## <a name="add-namespace-declarations"></a>Hinzufügen von Namespacedeklarationen

`Windows.UI.Notifications` enthält den Toast-APIs.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>Senden eines Popups

In Windows 10 wird der Inhalt der Popupbenachrichtigung mithilfe einer adaptiven Sprache beschrieben, die eine sehr flexible Darstellung Ihrer Benachrichtigung erlaubt. Weitere Informationen finden Sie in der [Dokumentation zu Popupinhalt](adaptive-interactive-toasts.md).

### <a name="constructing-the-visual-part-of-the-content"></a>Erstellen des visuellen Teils des Inhalts

Beginnen wir mit der Erstellung des visuellen Teils des Inhalts, der den Text und Bilder umfasst, der dem Benutzer angezeigt werden soll.

Dank der Benachrichtigungen-Bibliothek ist das Generieren von XML-Inhalt einfach. Wenn Sie die Benachrichtigungsbibliothek von NuGet nicht installieren, müssen Sie den XML-Code manuell erstellen, was zu Fehlern führen kann.

> [!NOTE]
> Die verwendeten Bilder können aus dem App-Paket, dem lokalen Speicher der App oder aus dem Web stammen. Im Fall Creators Update kann die Größe der Webbilder 3 MB für normale Verbindungen und 1 MB für getaktete Verbindungen betragen. Auf Geräten, die noch nicht das Fall Creators Update haben, dürfen Webbilder nicht größer als 200 KB sein.

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://picsum.photos/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
{
    BindingGeneric = new ToastBindingGeneric()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = title
            },
 
            new AdaptiveText()
            {
                Text = content
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>Erstellen des Aktionsteils des Inhalts

Lassen Sie uns nun Aktionen zum Inhalt hinzufügen.

Im folgenden Beispiel haben wir ein Eingabeelement hinzugefügt, das dem Benutzer ermöglicht, Text einzugeben, der an die App zurückgegeben wird, wenn der Benutzer auf eine der Schaltflächen oder das Popup selbst klickt.

Wir haben dann zwei Schaltflächen hinzugefügt, die jeweils einen eigenen Aktivierungstyp, eigenen Inhalt und eigene Argumente angeben.
* **ActivationType** wird verwendet, um anzugeben, wie Ihre App aktiviert werden soll, wenn diese Aktion vom Benutzer ausgeführt wird. Sie können entscheiden, Ihre App im Vordergrund zu starten, eine Hintergrundaufgabe zu starten oder eine andere App per Protokollstart zu starten. Egal ob Ihre App im Vordergrund oder Hintergrund ist, Sie erhalten immer die Benutzereingaben und die Argumente, die Sie angegeben haben, damit Ihre App die richtigen Maßnahmen durchführen kann, wie das Senden der Nachricht oder das Öffnen einer Unterhaltung.

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>Kombinieren der obigen Ausführungen zum Erstellen des gesamten Inhalts

Die Erstellung des Inhalts ist jetzt fertig und wir können diesen verwenden, um Ihr [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)-Objekt zu instanziieren.

**Hinweis:** : Sie können auch einen Aktivierungstyp innerhalb des Stammelements angeben, um festzulegen, welche Art von Aktivierung erfolgen muss, wenn der Benutzer auf den Text der Popupbenachrichtigung tippt. Normalerweise sollte durch Tippen auf den Text des Popups Ihre App im Vordergrund gestartet werden, um eine konsistente Benutzererfahrung zu schaffen. In Fällen, in denen es sinnvoll ist, können Sie jedoch andere Aktivierungstypen verwenden, die für Ihr spezielles Szenario geeignet sind.

Sie sollten die **Start**-Eigenschaft immer angeben, um festzulegen, welche Art von Aktivierung erfolgen muss, wenn der Benutzer auf den Text der Popupbenachrichtigung tippt.

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>Festlegen einer Ablaufzeit

In Windows 10 werden alle Popupbenachrichtigungen ins Info-Center verschoben, nachdem sie vom Benutzer geschlossen oder ignoriert wurden, sodass Benutzer sich Ihre Benachrichtigung ansehen können, nachdem das Popup ausgeblendet wurde.

Wenn die Nachricht in Ihrer Benachrichtigung jedoch nur für einen bestimmten Zeitraum relevant ist, sollten Sie für die Popupbenachrichtigung eine Ablaufzeit festlegen, damit dem Benutzer keine veralteten Informationen von Ihrer App angezeigt werden. Wenn beispielsweise ein Angebot nur 12 Stunden gültig ist, legen Sie die Ablaufzeit auf 12 Stunden fest. Im folgenden Code legen wir die Ablaufzeit auf 2 Tage fest.

> [!NOTE]
> Die standardmäßige und maximale Ablaufzeit für lokale Popupbenachrichtigungen ist 3 Tage.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Angeben eines primären Schlüssels für das Popup

Wenn Sie die Benachrichtigung, die Sie senden, programmgesteuert entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen primären Schlüssel für die Benachrichtigung anzugeben. Sie können diesen primären Schlüssel dann zukünftig verwenden, um die Benachrichtigung zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/entfernen bereits übermittelt, Popupbenachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popupbenachrichtigungen im Info-Center (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

Tag und Group fungieren kombiniert als ein zusammengesetzter primärer Schlüssel. Die Gruppe ist dem generischen Bezeichner, in denen Sie Gruppen wie "WallPosts", "Messages", "FriendRequests" zuweisen. Ein, und klicken Sie dann Tag sollte die Benachrichtigung selbst aus, in der Gruppe eindeutig identifiziert. Unter Verwendung einer allgemeinen Gruppe können Sie dann mithilfe der [RemoveGroup-API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) alle Benachrichtigungen aus dieser Gruppe entfernen.

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>Senden der Benachrichtigung

Wenn Sie das Popup initialisiert haben, erstellen Sie einfach einen [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier), und rufen Sie Show() auf, um Ihre Popupbenachrichtigung zu übergeben.

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>Löschen der Benachrichtigungen

UWP-Apps sind verantwortlich für das Entfernen und Löschen ihrer eigenen Benachrichtigungen. Wenn Ihre App gestartet wird, löschen wir NICHT automatisch Ihre Benachrichtigungen.

Windows entfernt nur dann automatisch eine Benachrichtigung, wenn der Benutzer explizit auf die Benachrichtigung klickt.

Hier ist ein Beispiel dafür, was eine Nachrichten-App tun sollte...

1. Benutzer erhält mehrere Popups über neue Nachrichten in einer Unterhaltung
2. Benutzer tippt auf eines dieser Popups, um die Unterhaltung zu öffnen
3. Die App öffnet die Unterhaltung und löscht dann alle Popups für die Unterhaltung (mithilfe von [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) für die von der App bereitgestellte Gruppe für diese Unterhaltung)
4. Im Info-Center des Benutzers wird jetzt ordnungsgemäß der Zustand der Benachrichtigung angegeben, da im Info-Center keine veralteten Benachrichtigungen für die Unterhaltung vorhanden sind.

Weitere Informationen zu aller Benachrichtigungen zu deaktivieren oder bestimmte Benachrichtigungen zu entfernen, finden Sie unter [Schnellstart: Verwalten von Popupbenachrichtigungen im Info-Center (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).


## <a name="handling-activation"></a>Behandeln der Aktivierung

Wenn der Benutzer in Windows 10 auf das Popup klickt, haben Sie zwei Möglichkeiten der Aktivierung Ihrer App durch das Popup...

* Vordergrundaktivierung
* Hintergrundaktivierung

> [!NOTE]
> Wenn Sie die älteren Popupvorlagen von Windows 8.1 verwenden, wird statt **OnActivated** **OnLaunched** aufgerufen. Die folgende Dokumentation gilt nur für moderne Windows 10-Benachrichtigungen, die die Benachrichtigungsbibliothek (oder die ToastGeneric-Vorlage bei Verwendung von XML-Rohdaten) verwenden.


### <a name="handling-foreground-activation"></a>Behandeln der Vordergrundaktivierung

Wenn ein Benutzer in Windows 10 auf ein modernes Popup (oder eine Schaltfläche im Popup) klickt, wird anstelle von **OnLaunched** **OnActivated** mit der neuen Art der Aktivierung – **ToastNotification** – aufgerufen. Dadurch kann der Entwickler eine Popupaktivierung auf einfache Weise erkennen und Aufgaben entsprechend ausführen.

Im Beispiel unten können Sie die Argumentzeichenfolge abrufen, die Sie ursprünglich im Popupinhalt angegeben haben. Sie können auch die Eingabe des Benutzers in Ihren Textfeldern und Auswahlfeldern abrufen.

> [!IMPORTANT]
> Sie müssen Ihren Frame initialisieren und Ihr Fenster aktivieren, wie Ihren **OnLaunched**-Code. **OnLaunched wird NICHT aufgerufen, wenn der Benutzer auf das Popup klickt**, auch wenn die App geschlossen war und zum ersten Mal gestartet wird. Wir empfehlen häufig, **OnLaunched** und **OnActivated** zu Ihrer eigenen `OnLaunchedOrActivated`-Methode zu kombinieren, da für beide die gleiche Initialisierung erfolgen muss.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="handling-background-activation"></a>Behandeln der Hintergrundaktivierung

Wenn Sie für das Popup (oder eine Schaltfläche in dem Popup) Hintergrundaktivierung angeben, wird anstelle einer Aktivierung Ihrer Vordergrund-App die Hintergrundaufgabe ausgeführt.

Weitere Informationen zu Hintergrundaufgaben finden Sie unter [Unterstützen Ihrer App mit Hintergrundaufgaben](/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Wenn Sie Build 14393 oder höher verwenden möchten, können Sie Hintergrundaufgaben innerhalb von Prozessen verwenden, was den Vorgang erheblich vereinfacht. Beachten Sie, dass Hintergrundaufgaben innerhalb von Prozessen nicht auf älteren Versionen von Windows ausgeführt werden können. Wir verwenden in diesem Codebeispiel eine Hintergrundaufgabe innerhalb von Prozessen.

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


Wenn Sie dann in Ihrer App.xaml.cs-Datei die OnBackgroundActivated-Methode überschreiben, können Sie die vordefinierten Argumente und Benutzereingaben ähnlich wie bei der Vordergrundaktivierung abrufen.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```



## <a name="plain-vanilla-code-snippets"></a>Einfache „Vanilla“-Codeausschnitte

Wenn Sie die Benachrichtigungsbibliothek von NuGet nicht verwenden, können Sie den XML-Code manuell erstellen, wie unten dargestellt, um eine [ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) zu erstellen.

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>Ressourcen

* [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast)
* [Toast-Content-Dokumentation](adaptive-interactive-toasts.md)
* [ToastNotification-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
