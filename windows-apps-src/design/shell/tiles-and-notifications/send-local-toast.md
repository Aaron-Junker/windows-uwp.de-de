---
Description: Erfahren Sie, wie Sie eine lokale Popup Benachrichtigung senden und den Benutzer bearbeiten, indem Sie auf den Toast klicken.
title: Senden einer lokalen Popupbenachrichtigung
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, Senden von Popup Benachrichtigungen, Benachrichtigungen, Senden von Benachrichtigungen, Popup Benachrichtigungen, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, Exemplarische Vorgehensweise
ms.localizationpriority: medium
ms.openlocfilehash: 23a1739b8f5859d128c97ff28350a548b61286d2
ms.sourcegitcommit: 63597f83f154ce41ebaf69c075093919c430297c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "82034184"
---
# <a name="send-a-local-toast-notification"></a>Senden einer lokalen Popupbenachrichtigung


Eine Popup Benachrichtigung ist eine Meldung, die eine APP erstellen und an den Benutzer übermitteln kann, wenn Sie sich derzeit nicht in der APP befinden. Dieser Schnellstart führt Sie durch die Schritte zum Erstellen, bereitzustellen und Anzeigen einer Windows 10-Popup Benachrichtigung mit den neuen adaptiven Vorlagen und interaktiven Aktionen. Diese Aktionen werden durch eine lokale Benachrichtigung veranschaulicht. Dies ist die einfachste zu implementierende Benachrichtigung.

> [!IMPORTANT]
> Desktop Anwendungen (einschließlich gepackter [msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) -apps, apps, die [Pakete](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) mit geringer Dichte zum Abrufen der Paket Identität verwenden, und klassische, nicht gepackte Win32-Apps) haben unterschiedliche Schritte zum Senden von Benachrichtigungen und zur Handhabung der Aktivierung Weitere Informationen zum Implementieren von-Umfassungen finden Sie in der Dokumentation zu [Desktop-Apps](toast-desktop-apps.md) .

Wir werden die folgenden Schritte durchlaufen:

### <a name="sending-a-toast"></a>Senden eines Toast

* Erstellen des visuellen Teils (Text und Image) der Benachrichtigung
* Hinzufügen von Aktionen zur Benachrichtigung
* Festlegen einer Ablaufzeit für den Toast
* Festlegen von Tag/Gruppe, sodass Sie den Toast zu einem späteren Zeitpunkt ersetzen/entfernen können
* Senden des Popups mithilfe der lokalen APIs

### <a name="handling-activation"></a>Aktivierungs Aktivierung

* Behandeln der Aktivierung beim Klicken auf den Text oder die Schaltflächen
* Behandeln der Vordergrund Aktivierung
* Behandeln der Hintergrund Aktivierung

> **Wichtige APIs**: die Klasse "- [Benachrichtigungs Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)", die Klasse "" der [Klasse](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) ""


## <a name="prerequisites"></a>Voraussetzungen

Um dieses Thema vollständig zu verstehen, ist Folgendes hilfreich...

* Ein funktionierendes wissen zu den Begriffen und Konzepten von Popup Benachrichtigungen. Weitere Informationen finden Sie unter [Übersicht über das Popup-und Aktions Center](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Eine Vertrautheit mit Windows 10-Popup Benachrichtigungs Inhalt. Weitere Informationen finden Sie in der Dokumentation zum Popup [Inhalt](adaptive-interactive-toasts.md).
* Ein Windows 10-UWP-App-Projekt

> [!NOTE]
> Anders als bei Windows 8/8.1 müssen Sie im Manifest Ihrer APP nicht mehr deklarieren, dass Ihre APP Popup Benachrichtigungen anzeigen kann. Alle Apps können Popup Benachrichtigungen senden und anzeigen.

> [!NOTE]
> **Windows 8/8.1-apps**: Verwenden Sie die [Archivierte Dokumentation](https://docs.microsoft.com/previous-versions/windows/apps/hh868254(v=win.10)).


## <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen

Es wird empfohlen, die beiden folgenden nuget-Pakete in Ihrem Projekt zu installieren. In unserem Codebeispiel werden diese Pakete verwendet. Am Ende des Artikels werden die "Vanille"-Code Ausschnitte bereitgestellt, die keine nuget-Pakete verwenden.

* [Microsoft. Toolkit. UWP. Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): Generieren Sie Popup Nutzlasten über Objekte anstelle von unformatiertem XML.
* [QueryString.net](https://www.nuget.org/packages/QueryString.NET/): generieren und Analysieren von Abfrage Zeichenfolgen mit C #


## <a name="add-namespace-declarations"></a>Hinzufügen von Namespace-Deklarationen

`Windows.UI.Notifications`enthält die Toast-APIs.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>Toast senden

In Windows 10 wird der Inhalt der Popup Benachrichtigung mit einer adaptiven Sprache beschrieben, die eine hohe Flexibilität bei der Anzeige Ihrer Benachrichtigung ermöglicht. Weitere Informationen finden Sie in der Dokumentation zum Popup [Inhalt](adaptive-interactive-toasts.md) .

### <a name="constructing-the-visual-part-of-the-content"></a>Erstellen des visuellen Teils der Inhalte

Zunächst erstellen wir den visuellen Teil des Inhalts, der den Text und die Bilder enthält, die dem Benutzer angezeigt werden sollen.

Dank der Benachrichtigungs Bibliothek ist das Erstellen des XML-Inhalts einfach. Wenn Sie die Benachrichtigungs Bibliothek nicht von nuget installieren, müssen Sie die XML-Datei manuell erstellen, wodurch Platz für Fehler bleibt.

> [!NOTE]
> Images können aus dem App-Paket, dem lokalen Speicher der APP oder aus dem Web verwendet werden. Im Fall von Creators Update können webimages bei normalen Verbindungen bis zu 3 MB und bei getakteten Verbindungen 1 MB betragen. Auf Geräten, auf denen das Fall Creators Update noch nicht ausgeführt wird, dürfen webimages nicht größer als 200 KB sein.

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


### <a name="constructing-actions-part-of-the-content"></a>Erstellen von Aktionen, die Teil des Inhalts sind

Nun fügen wir dem Inhalt Aktionen hinzu.

Im folgenden Beispiel haben wir ein Eingabe Element eingefügt, das es dem Benutzer ermöglicht, Text einzugeben, der an die APP zurückgegeben wird, wenn der Benutzer auf eine der Schaltflächen oder den Toast klickt.

Anschließend haben wir zwei Schaltflächen mit jeweils eigenem Aktivierungstyp, Inhalt und Argumenten hinzugefügt.
* **ActivationType** wird verwendet, um anzugeben, wie Ihre App aktiviert werden soll, wenn diese Aktion vom Benutzer ausgeführt wird. Sie können auswählen, ob Sie Ihre APP im Vordergrund starten, eine Hintergrundaufgabe starten oder eine andere app starten möchten. Unabhängig davon, ob Ihre APP Vordergrund oder Hintergrund auswählt, erhalten Sie immer die Benutzereingaben und die von Ihnen angegebenen Argumente, sodass Ihre APP die korrekte Aktion ausführen kann, z. b. das Senden der Nachricht oder das Öffnen einer Konversation.

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


### <a name="combining-the-above-to-construct-the-full-content"></a>Kombinieren des obigen, um den vollständigen Inhalt zu erstellen

Die Erstellung des Inhalts ist nun vollständig, und wir können ihn verwenden, um [**das Objekt "**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) Objekt Objekt" zu instanziieren.

**Hinweis**: Sie können auch einen Aktivierungstyp im Root-Element angeben, um anzugeben, welche Art der Aktivierung erfolgen soll, wenn der Benutzer auf den Text der Popup Benachrichtigung tippt. Wenn Sie auf den Hauptteil des Popups tippen, sollte Ihre APP normalerweise im Vordergrund gestartet werden, um eine konsistente Benutzer Darstellung zu schaffen. Sie können jedoch auch andere Aktivierungs Typen verwenden, um Ihr spezielles Szenario anzupassen, wenn es für den Benutzer am sinnvollsten ist.

Sie sollten immer die **Launch** -Eigenschaft festlegen, sodass Ihre APP weiß, welche Inhalte angezeigt werden sollen, wenn der Benutzer auf den Hauptteil des Toast tippt und die APP gestartet wird.

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

In Windows 10 wechseln alle Popup Benachrichtigungen in den Wartungs Center, nachdem Sie vom Benutzer verworfen oder ignoriert wurden, sodass Benutzer ihre Benachrichtigung anzeigen können, nachdem das Popup entfernt wurde.

Wenn die Meldung in der Benachrichtigung aber nur für einen bestimmten Zeitraum relevant ist, sollten Sie eine Ablaufzeit für die Popup Benachrichtigung festlegen, damit den Benutzern keine veralteten Informationen aus ihrer App angezeigt werden. Wenn eine herauf Stufung beispielsweise nur für 12 Stunden gültig ist, legen Sie die Ablaufzeit auf 12 Stunden fest. Im folgenden Code wird die Ablaufzeit auf 2 Tage festgelegt.

> [!NOTE]
> Die standardmäßige und maximale Ablaufzeit für lokale Popup Benachrichtigungen beträgt 3 Tage.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Angeben eines Primärschlüssels für den Toast

Wenn Sie die gesendete Benachrichtigung Programm gesteuert entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen Primärschlüssel für die Benachrichtigung anzugeben. Anschließend können Sie diesen Primärschlüssel in Zukunft verwenden, um die Benachrichtigung zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/Entfernen von bereits übermittelten Popup Benachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

Die Kombination aus Tag und Gruppe fungiert als zusammengesetzter Primärschlüssel. Die Gruppe ist der allgemeinere Bezeichner, in dem Sie Gruppen wie "wallposts", "Messages", "friendrequests" usw. zuweisen können. Und dann sollte die Kennung die Benachrichtigung selbst innerhalb der Gruppe eindeutig identifizieren. Mithilfe einer generischen Gruppe können Sie dann alle Benachrichtigungen aus dieser Gruppe entfernen, indem Sie die [removegroup-API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)verwenden.

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>Senden der Benachrichtigung

Nachdem Sie den Toast initialisiert haben, erstellen Sie einfach einen [toastnotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) , und rufen Sie Show () auf, und übergeben Sie die Popup Benachrichtigung.

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>Löschen von Benachrichtigungen

UWP-apps sind dafür verantwortlich, ihre eigenen Benachrichtigungen zu entfernen und zu löschen. Wenn Ihre APP gestartet wird, löschen wir Ihre Benachrichtigungen nicht automatisch.

Windows entfernt nur dann automatisch eine Benachrichtigung, wenn der Benutzer explizit auf die Benachrichtigung klickt.

Im folgenden finden Sie ein Beispiel dafür, was eine Messaging-APP tun sollte...

1. Der Benutzer erhält mehrere Popup Informationen zu neuen Nachrichten in einer Konversation.
2. Der Benutzer tippt auf eines dieser Popups, um die Konversation zu öffnen.
3. Die APP öffnet die Konversation und löscht dann alle Popup für diese Konversation (durch Verwendung von [removegroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) in der von der APP bereitgestellten Gruppe für diese Konversation).
4. Das Aktions Center des Benutzers reflektiert nun den Benachrichtigungs Status ordnungsgemäß, da keine veralteten Benachrichtigungen für diese Konversation im Aktions Center vorhanden sind.

Informationen dazu, wie Sie alle Benachrichtigungen löschen oder bestimmte Benachrichtigungen entfernen, finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).


## <a name="activation-handling"></a>Aktivierungs Behandlung

Wenn der Benutzer in Windows 10 auf den Popup klickt, kann der Popup Ihre APP auf zwei verschiedene Arten aktivieren...

* Vordergrund Aktivierung
* Hintergrund Aktivierung

> [!NOTE]
> Wenn Sie die Legacy-Popup Vorlagen aus Windows 8.1 verwenden, wird **ongestartete** anstelle von **onaktiviertem**aufgerufen. Die folgende Dokumentation gilt nur für moderne Windows 10-Benachrichtigungen, die die Benachrichtigungs Bibliothek verwenden (oder die Vorlage "" bei unformatierten XML-Daten).


### <a name="handling-foreground-activation"></a>Behandeln der Vordergrund Aktivierung

Wenn ein Benutzer in Windows 10 auf einen modernen Toast (oder eine Schaltfläche im Toast) klickt, wird **onactivation** anstelle von **ongestartete**aufgerufen, mit einer neuen aktivierungart – **toastnotification**. Daher kann der Entwickler eine Popup Aktivierung leicht unterscheiden und Aufgaben entsprechend ausführen.

Im unten gezeigten Beispiel können Sie die Argument Zeichenfolge abrufen, die Sie anfänglich im Popup Inhalt angegeben haben. Sie können auch die Eingabe abrufen, die der Benutzer in ihren Textfeldern und Auswahl Feldern bereitgestellt hat.

> [!IMPORTANT]
> Sie müssen ihren Frame initialisieren und das Fenster wie den **ongestarteten** -Code aktivieren. **Ongestartete wird nicht aufgerufen, wenn der Benutzer auf**den Popup klickt, auch wenn die app geschlossen wurde und zum ersten Mal gestartet wird. Häufig wird empfohlen, **ongestartete** und **onaktivierte** Methoden in `OnLaunchedOrActivated` ihrer eigenen Methode zu kombinieren, da die gleiche Initialisierung in beiden Fällen erfolgen muss.

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


## <a name="handling-background-activation"></a>Behandeln der Hintergrund Aktivierung

Wenn Sie die Hintergrund Aktivierung für den Toast (oder eine Schaltfläche im Toast) angeben, wird die Hintergrundaufgabe ausgeführt, anstatt Ihre Vordergrund-APP zu aktivieren.

Weitere Informationen zu Hintergrundaufgaben finden Sie [unter unterstützen der APP mit Hintergrundaufgaben](/windows/uwp/launch-resume/support-your-app-with-background-tasks).

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


Überschreiben Sie anschließend in Ihrer APP.XAML.cs die onbackgroundaktivierte Methode, sodass Sie die vordefinierten Argumente und die Benutzereingabe, ähnlich wie bei der Vordergrund Aktivierung, abrufen können.

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



## <a name="plain-vanilla-code-snippets"></a>Einfache "Vanille"-Code Ausschnitte

Wenn Sie die Benachrichtigungs Bibliothek nicht von nuget verwenden, können Sie das XML wie unten dargestellt manuell erstellen, um eine " [deastnotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)" zu erstellen.

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
* [Klasse "-Benachrichtigungs Klasse"](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)
* [Die Klasse "atastnotificationactivatedeventargs"](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
