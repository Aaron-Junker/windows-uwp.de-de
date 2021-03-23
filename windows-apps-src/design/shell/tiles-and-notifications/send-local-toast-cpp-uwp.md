---
description: Erfahren Sie, wie Sie eine lokale Popup Benachrichtigung von einer C++ UWP-App senden und den Benutzer bearbeiten, indem Sie auf den Toast klicken.
title: Senden einer lokalen Popup Benachrichtigung von einer C++-UWP-App
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from a C++ UWP app
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: Windows 10, UWP, Senden von Popup Benachrichtigungen, Benachrichtigungen, Senden von Benachrichtigungen, Popup Benachrichtigungen, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, Exemplarische Vorgehensweise, cpp, c++, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5960d6f2a63ea2aa0aea26392db5958825aebf2b
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804964"
---
# <a name="send-a-local-toast-notification-from-c-uwp-apps"></a>Senden einer lokalen Popup Benachrichtigung von C++ UWP-apps

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> Wenn Sie eine C++-nicht-UWP-app schreiben, sehen Sie sich die [C++ WRL](send-local-toast-desktop-cpp-wrl.md) -Dokumentation an. Wenn Sie eine c#-app schreiben, sehen Sie sich die [c#-Dokumentation](send-local-toast.md)an.



## <a name="step-1-install-nuget-package"></a>Schritt 1: Installieren des nuget-Pakets

[!INCLUDE [nuget package](includes/nuget-package.md)]

In unserem Codebeispiel wird dieses Paket verwendet. Mithilfe dieses Pakets können Sie Popup Benachrichtigungen erstellen, ohne XML zu verwenden.


## <a name="step-2-add-namespace-declarations"></a>Schritt 2: Hinzufügen von Namespace Deklarationen

```cpp
using namespace Microsoft::Toolkit::Uwp::Notifications;
```


## <a name="step-3-send-a-toast"></a>Schritt 3: Senden eines Toast

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)
    ->AddText("Andrew sent you a picture")
    ->AddText("Check this out, The Enchantments in Washington!")
    ->Show();
```


## <a name="step-4-handling-activation"></a>Schritt 4: Behandeln der Aktivierung

Wenn der Benutzer auf die Benachrichtigung klickt (oder auf eine Schaltfläche in der Benachrichtigung bei der Vordergrund Aktivierung), wird die **app. XAML. cpp** **onactivation** ihrer app aufgerufen.

**App.xaml.cpp**

```cpp
void App::OnActivated(IActivatedEventArgs^ e)
{
    // Handle notification activation
    if (e->Kind == ActivationKind::ToastNotification)
    {
        ToastNotificationActivatedEventArgs^ toastActivationArgs = (ToastNotificationActivatedEventArgs^)e;

        // Obtain the arguments from the notification
        ToastArguments^ args = ToastArguments::Parse(toastActivationArgs->Argument);

        // Obtain any user input (text boxes, menu selections) from the notification
        auto userInput = toastActivationArgs->UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

[!INCLUDE [OnLaunched warning](includes/onlaunched-warning.md)]


## <a name="activation-in-depth"></a>Aktivierung ausführlich

Der erste Schritt, mit dem Ihre Benachrichtigungen handlungsfähig sind, besteht darin, ihrer Benachrichtigung einige Start Argumente hinzuzufügen, damit Ihre APP weiß, was gestartet werden soll, wenn der Benutzer auf die Benachrichtigung klickt (in diesem Fall sind einige Informationen angegeben, die später eine Konversation öffnen sollten, und wir wissen, welche bestimmte Konversation geöffnet werden soll).

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())

    // Arguments returned when user taps body of notification
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)

    ->AddText("Andrew sent you a picture")
    ->Show();
```



## <a name="adding-images"></a>Hinzufügen von Bildern

Sie können Benachrichtigungen zu Benachrichtigungen hinzufügen. Wir fügen ein Inline Image und ein Profil (app-Logo Überschreibungs Image) hinzu.

[!INCLUDE [images note](includes/images-note.md)]

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ...

    // Inline image
    ->AddInlineImage(ref new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    ->AddAppLogoOverride(ref new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop::Circle)
    
    ->Show();
```



## <a name="adding-buttons-and-inputs"></a>Hinzufügen von Schaltflächen und Eingaben

Sie können Schaltflächen und Eingaben hinzufügen, um Ihre Benachrichtigungen interaktiv zu gestalten. Schaltflächen können Ihre Vordergrund-APP, ein Protokoll oder Ihre Hintergrundaufgabe starten. Wir fügen ein Antwort Textfeld, eine "like"-Schaltfläche und eine Schaltfläche "View" (anzeigen) hinzu, mit der das Bild geöffnet wird.

<img src="images/toast-notification.png" width="628" alt="Screenshot of a toast notification with inputs and buttons"/>

```cpp
// Construct the content
(ref new ToastContentBuilder())
    ->AddArgument("conversationId", 9813)
    ...

    // Text box for replying
    ->AddInputTextBox("tbReply", "Type a response")

    // Buttons
    ->AddButton((ref new ToastButton())
        ->SetContent("Reply")
        ->AddArgument("action", "reply")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("Like")
        ->AddArgument("action", "like")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("View")
        ->AddArgument("action", "view"))
    
    ->Show();
```

Die Aktivierung von Vordergrund Schaltflächen wird auf die gleiche Weise wie der hauptpopup-Text behandelt (Ihre APP. XAML. cpp onactivation wird aufgerufen).



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


Überschreiben Sie dann in der Datei "App. XAML. cs" die onbackgroundaktivierte Methode. Sie können dann die vordefinierten Argumente und die Benutzereingabe ähnlich der Vordergrund Aktivierung abrufen.

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

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("Expires in 2 days...")
    ->Show(toast =>
    {
        toast->ExpirationTime = DateTime::Now->AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>Angeben eines Primärschlüssels für den Toast

Wenn Sie die gesendete Benachrichtigung Programm gesteuert entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen Primärschlüssel für die Benachrichtigung anzugeben. Anschließend können Sie diesen Primärschlüssel in Zukunft verwenden, um die Benachrichtigung zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/Entfernen von bereits übermittelten Popup Benachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Die Kombination aus Tag und Gruppe fungiert als zusammengesetzter Primärschlüssel. Die Gruppe ist der allgemeinere Bezeichner, in dem Sie Gruppen wie "wallposts", "Messages", "friendrequests" usw. zuweisen können. Und dann sollte die Kennung die Benachrichtigung selbst innerhalb der Gruppe eindeutig identifizieren. Mithilfe einer generischen Gruppe können Sie dann alle Benachrichtigungen aus dieser Gruppe entfernen, indem Sie die [removegroup-API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)verwenden.

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("New post on your wall!")
    ->Show(toast =>
    {
        toast.Tag = "18365";
        toast.Group = "wallPosts";
    });
```



## <a name="clear-your-notifications"></a>Löschen von Benachrichtigungen

Apps sind dafür verantwortlich, ihre eigenen Benachrichtigungen zu entfernen und zu löschen. Wenn Ihre APP gestartet wird, löschen wir Ihre Benachrichtigungen nicht automatisch.

Windows entfernt nur dann automatisch eine Benachrichtigung, wenn der Benutzer explizit auf die Benachrichtigung klickt.

Im folgenden finden Sie ein Beispiel dafür, was eine Messaging-APP tun sollte...

1. Der Benutzer erhält mehrere Popup Informationen zu neuen Nachrichten in einer Konversation.
2. Der Benutzer tippt auf eines dieser Popups, um die Konversation zu öffnen.
3. Die APP öffnet die Konversation und löscht dann alle Popup für diese Konversation (durch Verwendung von [removegroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) in der von der APP bereitgestellten Gruppe für diese Konversation).
4. Das Aktions Center des Benutzers reflektiert nun den Benachrichtigungs Status ordnungsgemäß, da keine veralteten Benachrichtigungen für diese Konversation im Aktions Center vorhanden sind.

Informationen dazu, wie Sie alle Benachrichtigungen löschen oder bestimmte Benachrichtigungen entfernen, finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

```cpp
ToastNotificationManagerCompat::History->Clear();
```



## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
* [Klasse "-Benachrichtigungs Klasse"](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [Die Klasse "atastnotificationactivatedeventargs"](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)