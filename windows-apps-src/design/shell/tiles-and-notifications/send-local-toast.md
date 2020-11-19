---
description: Erfahren Sie, wie Sie eine lokale Popup Benachrichtigung von UWP-apps senden und den Benutzer bearbeiten, indem Sie auf den Toast klicken.
title: Lokale Popup Benachrichtigung von UWP-apps senden
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from UWP apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, Senden von Popup Benachrichtigungen, Benachrichtigungen, Senden von Benachrichtigungen, Popup Benachrichtigungen, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, Exemplarische Vorgehensweise
ms.localizationpriority: medium
ms.openlocfilehash: 0a2e8c25aa7efcb96166b741a073122e3c077c08
ms.sourcegitcommit: 2a23972e9a0807256954d6da5cf21d0bbe7afb0a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2020
ms.locfileid: "94941826"
---
# <a name="send-a-local-toast-notification-from-uwp-apps"></a>Lokale Popup Benachrichtigung von UWP-apps senden


Eine Popup Benachrichtigung ist eine Meldung, die eine APP erstellen und an den Benutzer übermitteln kann, wenn Sie sich derzeit nicht in der APP befinden. Dieser Schnellstart führt Sie durch die Schritte zum Erstellen, bereitzustellen und Anzeigen einer Windows 10-Popup Benachrichtigung mit den neuen adaptiven Vorlagen und interaktiven Aktionen. Diese Aktionen werden durch eine lokale Benachrichtigung veranschaulicht. Dies ist die einfachste zu implementierende Benachrichtigung.

> [!IMPORTANT]
> Desktop Anwendungen (einschließlich gepackter [msix](/windows/msix/desktop/source-code-overview) -apps, apps, die [Pakete](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) mit geringer Dichte zum Abrufen der Paket Identität verwenden, und klassische, nicht gepackte Desktop-Apps) haben unterschiedliche Schritte zum Senden von Benachrichtigungen und zur Handhabung der Aktivierung Weitere Informationen zum Implementieren von-Umfassungen finden Sie in der Dokumentation zu [Desktop-Apps](toast-desktop-apps.md) .

> **Wichtige APIs**: die Klasse "- [Benachrichtigungs Klasse](/uwp/api/Windows.UI.Notifications.ToastNotification)", die Klasse "" der [Klasse](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) ""



## <a name="step-1-install-nuget-package"></a>Schritt 1: Installieren des nuget-Pakets

Installieren Sie das [nuget-Paket Microsoft. Toolkit. UWP. Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). In unserem Codebeispiel wird dieses Paket verwendet. Am Ende des Artikels werden die "einfachen" Code Ausschnitte bereitgestellt, die keine nuget-Pakete verwenden. Mithilfe dieses Pakets können Sie Popup Benachrichtigungen erstellen, ohne XML zu verwenden.


## <a name="step-2-add-namespace-declarations"></a>Schritt 2: Hinzufügen von Namespace Deklarationen

`Windows.UI.Notifications` enthält die Toast-APIs.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-send-a-toast"></a>Schritt 3: Senden eines Toast

In Windows 10 wird der Inhalt der Popup Benachrichtigung mit einer adaptiven Sprache beschrieben, die eine hohe Flexibilität bei der Anzeige Ihrer Benachrichtigung ermöglicht. Weitere Informationen finden Sie in der Dokumentation zum Popup [Inhalt](adaptive-interactive-toasts.md) .

Wir beginnen mit einer einfachen textbasierten Benachrichtigung. Erstellen Sie den Benachrichtigungs Inhalt (mit der Benachrichtigungs Bibliothek), und zeigen Sie die Benachrichtigung an!

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("picOfHappyCanyon", ToastActivationType.Foreground)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, Happy Canyon in Utah!")
    .GetToastContent();

// Create the notification
var notif = new ToastNotification(content.GetXml());

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```

## <a name="step-4-handling-activation"></a>Schritt 4: Behandeln der Aktivierung

Wenn der Benutzer auf die Benachrichtigung klickt (oder auf eine Schaltfläche in der Benachrichtigung bei der Vordergrund Aktivierung), wird die **app.XAML.cs** **onaktivierte** App der app aufgerufen.

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        string args = toastActivationArgs.Argument;

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

> [!IMPORTANT]
> Sie müssen ihren Frame initialisieren und das Fenster wie den **ongestarteten** -Code aktivieren. **Ongestartete wird nicht aufgerufen, wenn der Benutzer auf** den Popup klickt, auch wenn die app geschlossen wurde und zum ersten Mal gestartet wird. Häufig wird empfohlen, **ongestartete** und **onaktivierte** Methoden in ihrer eigenen Methode zu kombinieren `OnLaunchedOrActivated` , da die gleiche Initialisierung in beiden Fällen erfolgen muss.


## <a name="activation-in-depth"></a>Aktivierung ausführlich

Der erste Schritt, mit dem Ihre Benachrichtigungen handlungsfähig sind, besteht darin, ihrer Benachrichtigung einige Start Argumente hinzuzufügen, damit Ihre APP weiß, was gestartet werden soll, wenn der Benutzer auf die Benachrichtigung klickt (in diesem Fall sind einige Informationen angegeben, die später eine Konversation öffnen sollten, und wir wissen, welche bestimmte Konversation geöffnet werden soll).

Es wird empfohlen, das [QueryString.net](https://www.nuget.org/packages/QueryString.NET/) nuget-Paket zu installieren, um Abfrage Zeichenfolgen für Ihre Benachrichtigungs Argumente zu erstellen und zu analysieren, wie unten gezeigt.

```csharp
using Microsoft.QueryStringDotNET; // QueryString.NET

int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()

    // Arguments returned when user taps body of notification
    .AddToastActivationInfo(new QueryString() // Using QueryString.NET
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), ToastActivationType.Foreground)

    .AddText("Andrew sent you a picture")
    ...
```


Im folgenden finden Sie ein komplexeres Beispiel für die Handhabung der Aktivierung...

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {            
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


## <a name="adding-images"></a>Hinzufügen von Bildern

Sie können Benachrichtigungen zu Benachrichtigungen hinzufügen. Wir fügen ein Inline Image und ein Profil (app-Logo Überschreibungs Image) hinzu.

> [!NOTE]
> Images können aus dem App-Paket, dem lokalen Speicher der APP oder aus dem Web verwendet werden. Im Fall von Creators Update können webimages bei normalen Verbindungen bis zu 3 MB und bei getakteten Verbindungen 1 MB betragen. Auf Geräten, auf denen das Fall Creators Update noch nicht ausgeführt wird, dürfen webimages nicht größer als 200 KB sein.

> [!IMPORTANT]
> Http-Images werden nur in UWP/msix/Sparse-Apps unterstützt, die über die Internetfunktion in ihrem Manifest verfügen. Desktop-nicht-msix/Sparse-apps unterstützen keine http-Bilder. Sie müssen das Image in Ihre lokalen app-Daten herunterladen und lokal darauf verweisen.

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .GetToastContent();
    
...
```



## <a name="adding-buttons-and-inputs"></a>Hinzufügen von Schaltflächen und Eingaben

Sie können Schaltflächen und Eingaben hinzufügen, um Ihre Benachrichtigungen interaktiv zu gestalten. Schaltflächen können Ihre Vordergrund-APP, ein Protokoll oder Ihre Hintergrundaufgabe starten. Wir fügen ein Antwort Textfeld, eine "like"-Schaltfläche und eine Schaltfläche "View" (anzeigen) hinzu, mit der das Bild geöffnet wird.

<img alt="Toast with images and buttons" src="images/send-toast-03.png" width="364"/>

```csharp
int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Reference the text box's ID in order to place this button next to the text box
    .AddButton("tbReply", "Reply", ToastActivationType.Background, new QueryString()
    {
        { "action", "reply" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), imageUri: new Uri("Assets/Reply.png", UriKind.Relative))

    .AddButton("Like", ToastActivationType.Background, new QueryString()
    {
        { "action", "like" },
        { "conversationId", conversationId.ToString() }
    }.ToString())

    .AddButton("View", ToastActivationType.Foreground, new QueryString()
    {
        { "action", "viewImage" },
        { "imageUrl", image.ToString() }
    }.ToString())
    
    .GetToastContent();
    
...
```

Die Aktivierung von Vordergrund Schaltflächen erfolgt auf die gleiche Weise wie der hauptpopup-Text (Ihre APP.XAML.cs onaktivierte wird aufgerufen).



## <a name="handling-background-activation"></a>Behandeln der Hintergrund Aktivierung

Wenn Sie die Hintergrund Aktivierung für den Toast (oder eine Schaltfläche im Toast) angeben, wird die Hintergrundaufgabe ausgeführt, anstatt Ihre Vordergrund-APP zu aktivieren.

Weitere Informationen zu Hintergrundaufgaben finden Sie [unter unterstützen der APP mit Hintergrundaufgaben](../../../launch-resume/support-your-app-with-background-tasks.md).

Wenn Sie Build 14393 oder höher als Ziel verwenden, können Sie Prozess interne Hintergrundaufgaben verwenden, die die Dinge erheblich vereinfachen. Beachten Sie, dass Prozess interne Hintergrundaufgaben in älteren Versionen von Windows nicht ausgeführt werden können. In diesem Codebeispiel wird eine Prozess interne Hintergrundaufgabe verwendet.

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


Überschreiben Sie anschließend in Ihrer APP.XAML.cs die onbackgroundaktivierte Methode. Sie können dann die vordefinierten Argumente und die Benutzereingabe ähnlich der Vordergrund Aktivierung abrufen.

**App.xaml.cs**

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


## <a name="set-an-expiration-time"></a>Festlegen einer Ablaufzeit

In Windows 10 wechseln alle Popup Benachrichtigungen in den Wartungs Center, nachdem Sie vom Benutzer verworfen oder ignoriert wurden, sodass Benutzer ihre Benachrichtigung anzeigen können, nachdem das Popup entfernt wurde.

Wenn die Meldung in der Benachrichtigung aber nur für einen bestimmten Zeitraum relevant ist, sollten Sie eine Ablaufzeit für die Popup Benachrichtigung festlegen, damit den Benutzern keine veralteten Informationen aus ihrer App angezeigt werden. Wenn eine herauf Stufung beispielsweise nur für 12 Stunden gültig ist, legen Sie die Ablaufzeit auf 12 Stunden fest. Im folgenden Code wird die Ablaufzeit auf 2 Tage festgelegt.

> [!NOTE]
> Die standardmäßige und maximale Ablaufzeit für lokale Popup Benachrichtigungen beträgt 3 Tage.

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .GetToastContent();

// Set expiration time
var notif = new ToastNotification(content.GetXml())
{
    ExpirationTime = DateTime.Now.AddDays(2)
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Angeben eines Primärschlüssels für den Toast

Wenn Sie die gesendete Benachrichtigung Programm gesteuert entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen Primärschlüssel für die Benachrichtigung anzugeben. Anschließend können Sie diesen Primärschlüssel in Zukunft verwenden, um die Benachrichtigung zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/Entfernen von bereits übermittelten Popup Benachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Die Kombination aus Tag und Gruppe fungiert als zusammengesetzter Primärschlüssel. Die Gruppe ist der allgemeinere Bezeichner, in dem Sie Gruppen wie "wallposts", "Messages", "friendrequests" usw. zuweisen können. Und dann sollte die Kennung die Benachrichtigung selbst innerhalb der Gruppe eindeutig identifizieren. Mithilfe einer generischen Gruppe können Sie dann alle Benachrichtigungen aus dieser Gruppe entfernen, indem Sie die [removegroup-API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)verwenden.

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("New post on your wall!")
    .GetToastContent();

// Set tag/group
new ToastNotification(content.GetXml())
{
    Tag = "18365",
    Group = "wallPosts"
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```



## <a name="clear-your-notifications"></a>Löschen von Benachrichtigungen

UWP-apps sind dafür verantwortlich, ihre eigenen Benachrichtigungen zu entfernen und zu löschen. Wenn Ihre APP gestartet wird, löschen wir Ihre Benachrichtigungen nicht automatisch.

Windows entfernt nur dann automatisch eine Benachrichtigung, wenn der Benutzer explizit auf die Benachrichtigung klickt.

Im folgenden finden Sie ein Beispiel dafür, was eine Messaging-APP tun sollte...

1. Der Benutzer erhält mehrere Popup Informationen zu neuen Nachrichten in einer Konversation.
2. Der Benutzer tippt auf eines dieser Popups, um die Konversation zu öffnen.
3. Die APP öffnet die Konversation und löscht dann alle Popup für diese Konversation (durch Verwendung von [removegroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) in der von der APP bereitgestellten Gruppe für diese Konversation).
4. Das Aktions Center des Benutzers reflektiert nun den Benachrichtigungs Status ordnungsgemäß, da keine veralteten Benachrichtigungen für diese Konversation im Aktions Center vorhanden sind.

Informationen dazu, wie Sie alle Benachrichtigungen löschen oder bestimmte Benachrichtigungen entfernen, finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

```csharp
ToastNotificationManager.History.Clear();
```


## <a name="plain-code-snippets"></a>Einfache Code Ausschnitte

Wenn Sie die Benachrichtigungs Bibliothek nicht von nuget verwenden, können Sie das XML wie unten dargestellt manuell erstellen, um eine " [deastnotification](/uwp/api/Windows.UI.Notifications.ToastNotification)" zu erstellen.

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

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
* [Klasse "-Benachrichtigungs Klasse"](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [Die Klasse "atastnotificationactivatedeventargs"](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)