---
description: Erfahren Sie, wie Sie eine lokale Popup Benachrichtigung aus c#-apps senden und den Benutzer bearbeiten, indem Sie auf den Toast klicken.
title: Lokale Popup Benachrichtigung von c#-apps senden
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from C# apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, Senden von Popup Benachrichtigungen, Benachrichtigungen, Senden von Benachrichtigungen, Popup Benachrichtigungen, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, Exemplarische Vorgehensweise, c#, CSharp, Win32, Desktop
ms.localizationpriority: medium
ms.openlocfilehash: 9e6130b703cc1f8a0163ea539ba97cf4a08388b5
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804339"
---
# <a name="send-a-local-toast-notification-from-c-apps"></a>Lokale Popup Benachrichtigung von c#-apps senden

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> Wenn Sie eine C++-app schreiben, sehen Sie sich die [C++ UWP](send-local-toast-cpp-uwp.md) -oder [C++ WRL](send-local-toast-desktop-cpp-wrl.md) -Dokumentation an.


## <a name="step-1-install-nuget-package"></a>Schritt 1: Installieren des nuget-Pakets

[!INCLUDE [nuget package](includes/nuget-package.md)]

[!INCLUDE [nuget package .NET warnings](includes/nuget-package-dotnet-warnings.md)]

In unserem Codebeispiel wird dieses Paket verwendet. Mithilfe dieses Pakets können Sie Popup Benachrichtigungen erstellen, ohne XML zu verwenden. Außerdem können Desktop-Apps Toasts senden.


## <a name="step-2-send-a-toast"></a>Schritt 2: Senden eines Toast

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```csharp
// Requires Microsoft.Toolkit.Uwp.Notifications NuGet package
new ToastContentBuilder()
    .AddArgument("action", "viewConversation")
    .AddArgument("conversationId", 9813)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, The Enchantments in Washington!")
    .Show();
```

Versuchen Sie, diesen Code auszuführen, und Sie sollten sehen, dass die Benachrichtigung angezeigt wird.


## <a name="step-3-handling-activation"></a>Schritt 3: Behandeln der Aktivierung

Nachdem Sie eine Benachrichtigung angezeigt haben, müssen Sie den Benutzer wahrscheinlich auf die Benachrichtigung klicken (d. h., Sie können bestimmte Inhalte aufrufen, nachdem der Benutzer darauf geklickt hat, Ihre APP im allgemeinen öffnen oder eine Aktion durchführen, wenn der Benutzer auf die Benachrichtigung klickt).

Die Schritte für die Aktivierung der Aktivierung unterscheiden sich für UWP-, Desktop-(msix) und Desktop-Apps (entpackt).


#### <a name="uwp"></a>[UWP](#tab/uwp)

Wenn der Benutzer auf die Benachrichtigung klickt (oder auf eine Schaltfläche in der Benachrichtigung mit der Vordergrund Aktivierung), wird die Datei **app. XAML. cs** **onactivation** ihrer app aufgerufen, und die hinzugefügten Argumente werden zurückgegeben.

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        ToastArguments args = ToastArguments.Parse(toastActivationArgs.Argument);

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

[!INCLUDE [OnLaunched warning](includes/onlaunched-warning.md)]

#### <a name="desktop-msix"></a>[Desktop (msix)](#tab/desktop-msix)

Fügen Sie zunächst in der Datei " **Package. appxmanifest**" Folgendes hinzu:

1. Deklaration für **xmlns: com**
1. Deklaration für **xmlns: Desktop**
1. Im **ignorablenamespaces** -Attribut, **com** und **Desktop**
1. **Desktop: Erweiterung** für **Windows. toastnotificationactivation** zum Deklarieren der Popup-Activator-CLSID (mit einer neuen GUID Ihrer Wahl).
1. Nur msix: **com: Erweiterung** für den com-Activator, der die GUID aus Schritt #4 verwendet. Achten Sie darauf, `Arguments="-ToastActivated"` dass Sie das einschließen, damit Sie wissen, dass der Start aus einer Benachrichtigung stammt.

**Package.appxmanifest**

```xml
<!--Add these namespaces-->
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```

Abonnieren **Sie dann im Startcode Ihrer APP** (app. XAML. cs OnStartup für WPF) das onaktivierte Ereignis.

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]


[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]


#### <a name="desktop-unpackaged"></a>[Desktop (nicht gepackt)](#tab/desktop)

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]

Abonnieren Sie das onaktivierte Ereignis **im Startcode Ihrer APP** (app. XAML. cs OnStartup für WPF).

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]

---


## <a name="step-4-handling-uninstallation"></a>Schritt 4: Behandeln der nicht Installation

#### <a name="uwp"></a>[UWP](#tab/uwp)

Sie müssen nichts tun! Wenn UWP-apps deinstalliert werden, werden alle Benachrichtigungen und alle anderen zugehörigen Ressourcen automatisch bereinigt.

#### <a name="desktop-msix"></a>[Desktop (msix)](#tab/desktop-msix)

Sie müssen nichts tun! Wenn msix-apps deinstalliert werden, werden alle Benachrichtigungen und alle anderen zugehörigen Ressourcen automatisch bereinigt.

#### <a name="desktop-unpackaged"></a>[Desktop (nicht gepackt)](#tab/desktop)

Wenn Ihre APP über ein Deinstallationsprogramm verfügt, sollten Sie im Deinstallationsprogramm den Befehl ausführen `ToastNotificationManagerCompat.Uninstall();` . Wenn es sich bei ihrer App um eine "Portable App" ohne Installer handelt, sollten Sie diese Methode beim Beenden der APP aufrufen, es sei denn, Sie haben Benachrichtigungen, die nach dem Schließen der APP persistent gespeichert werden sollen.

Mit der Deinstallations Methode werden geplante und aktuelle Benachrichtigungen bereinigt, zugeordnete Registrierungs Werte entfernt und alle zugehörigen temporären Dateien entfernt, die von der Bibliothek erstellt wurden.

---


## <a name="adding-images"></a>Hinzufügen von Bildern

Sie können Benachrichtigungen zu Benachrichtigungen hinzufügen. Wir fügen ein Inline Image und ein Profil (app-Logo Überschreibungs Image) hinzu.

[!INCLUDE [images note](includes/images-note.md)]

> [!IMPORTANT]
> Http-Images werden nur in UWP/msix/Sparse-Apps unterstützt, die über die Internetfunktion in ihrem Manifest verfügen. Desktop-nicht-msix/Sparse-apps unterstützen keine http-Bilder. Sie müssen das Image in Ihre lokalen app-Daten herunterladen und lokal darauf verweisen.

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content and show the toast!
new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .Show();
```



## <a name="adding-buttons-and-inputs"></a>Hinzufügen von Schaltflächen und Eingaben

Sie können Schaltflächen und Eingaben hinzufügen, um Ihre Benachrichtigungen interaktiv zu gestalten. Schaltflächen können Ihre Vordergrund-APP, ein Protokoll oder Ihre Hintergrundaufgabe starten. Wir fügen ein Antwort Textfeld, eine "like"-Schaltfläche und eine Schaltfläche "View" (anzeigen) hinzu, mit der das Bild geöffnet wird.

<img src="images/toast-notification.png" width="628" alt="Screenshot of a toast notification with inputs and buttons"/>

```csharp
int conversationId = 384928;

// Construct the content
new ToastContentBuilder()
    .AddArgument("conversationId", conversationId)
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Buttons
    .AddButton(new ToastButton()
        .SetContent("Reply")
        .AddArgument("action", "reply")
        .SetBackgroundActivation())

    .AddButton(new ToastButton()
        .SetContent("Like")
        .AddArgument("action", "like")
        .SetBackgroundActivation())

    .AddButton(new ToastButton()
        .SetContent("View")
        .AddArgument("action", "viewImage")
        .AddArgument("imageUrl", image.ToString()))
    
    .Show();
```

Die Aktivierung von Vordergrund Schaltflächen wird auf die gleiche Weise behandelt wie der hauptpopup-Text (die app. XAML. cs onactivation wird aufgerufen).

Beachten Sie, dass beim Klicken auf die Schaltflächen auch Argumente hinzugefügt werden, die dem Popup der obersten Ebene (z. b. Konversations-ID) hinzugefügt werden, solange die Schaltflächen die AddArgument-API verwenden, wie oben gezeigt (wenn Sie benutzerdefinierte Argumente auf einer Schaltfläche zuweisen, werden die Argumente der obersten Ebene nicht eingeschlossen)



## <a name="handling-background-activation"></a>Behandeln der Hintergrund Aktivierung

#### <a name="uwp"></a>[UWP](#tab/uwp)

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
                ToastArguments arguments = ToastArguments.Parse(details.Argument);
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```

#### <a name="desktop"></a>[Desktop](#tab/desktop-msix+desktop)

Bei Desktop Anwendungen werden Hintergrund Aktivierungen genauso behandelt wie Vordergrund Aktivierungen (der **onaktivierte** Ereignishandler wird ausgelöst). Sie können auswählen, dass keine Benutzeroberfläche angezeigt und die APP nach der Aktivierung der Aktivierung geschlossen werden soll.

---


## <a name="set-an-expiration-time"></a>Festlegen einer Ablaufzeit

In Windows 10 wechseln alle Popup Benachrichtigungen in den Wartungs Center, nachdem Sie vom Benutzer verworfen oder ignoriert wurden, sodass Benutzer ihre Benachrichtigung anzeigen können, nachdem das Popup entfernt wurde.

Wenn die Meldung in der Benachrichtigung aber nur für einen bestimmten Zeitraum relevant ist, sollten Sie eine Ablaufzeit für die Popup Benachrichtigung festlegen, damit den Benutzern keine veralteten Informationen aus ihrer App angezeigt werden. Wenn eine herauf Stufung beispielsweise nur für 12 Stunden gültig ist, legen Sie die Ablaufzeit auf 12 Stunden fest. Im folgenden Code wird die Ablaufzeit auf 2 Tage festgelegt.

> [!NOTE]
> Die standardmäßige und maximale Ablaufzeit für lokale Popup Benachrichtigungen beträgt 3 Tage.

```csharp
// Create toast content and show the toast!
new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .Show(toast =>
    {
        toast.ExpirationTime = DateTime.Now.AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>Angeben eines Primärschlüssels für den Toast

Wenn Sie die gesendete Benachrichtigung Programm gesteuert entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen Primärschlüssel für die Benachrichtigung anzugeben. Anschließend können Sie diesen Primärschlüssel in Zukunft verwenden, um die Benachrichtigung zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/Entfernen von bereits übermittelten Popup Benachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Die Kombination aus Tag und Gruppe fungiert als zusammengesetzter Primärschlüssel. Die Gruppe ist der allgemeinere Bezeichner, in dem Sie Gruppen wie "wallposts", "Messages", "friendrequests" usw. zuweisen können. Und dann sollte die Kennung die Benachrichtigung selbst innerhalb der Gruppe eindeutig identifizieren. Mithilfe einer generischen Gruppe können Sie dann alle Benachrichtigungen aus dieser Gruppe entfernen, indem Sie die [removegroup-API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)verwenden.

```csharp
// Create toast content and show the toast!
new ToastContentBuilder()
    .AddText("New post on your wall!")
    .Show(toast =>
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

```csharp
ToastNotificationManagerCompat.History.Clear();
```



## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
* [Klasse "-Benachrichtigungs Klasse"](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [Die Klasse "atastnotificationactivatedeventargs"](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)