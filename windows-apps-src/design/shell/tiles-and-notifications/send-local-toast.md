---
description: Erfahren Sie, wie Sie eine lokale Popupbenachrichtigung von C#-Apps senden und den Benutzer behandeln, der auf das Popup klickt.
title: Senden einer lokalen Popupbenachrichtigung von C#-Apps
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from C# apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, Senden von Popupbenachrichtigungen, Benachrichtigungen, Senden von Benachrichtigungen, Popupbenachrichtigungen, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, exemplarische Vorgehensweise, c#, csharp, win32, Desktop
ms.localizationpriority: medium
ms.openlocfilehash: 23896b65bf2e4e0a9fc2edf5744b647d6d9c26ed
ms.sourcegitcommit: 6cd970686d1ea7176b7e6651f349a14551709820
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559387"
---
# <a name="send-a-local-toast-notification-from-c-apps"></a>Senden einer lokalen Popupbenachrichtigung von C#-Apps

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> Wenn Sie eine C++-App schreiben, lesen Sie die Dokumentation zu [C++ UWP](send-local-toast-cpp-uwp.md) oder [C++ WRL.](send-local-toast-desktop-cpp-wrl.md)


## <a name="step-1-install-nuget-package"></a>Schritt 1: Installieren des NuGet-Pakets

[!INCLUDE [nuget package](includes/nuget-package.md)]

[!INCLUDE [nuget package .NET warnings](includes/nuget-package-dotnet-warnings.md)]

In unserem Codebeispiel wird dieses Paket verwendet. Mit diesem Paket können Sie Popupbenachrichtigungen ohne XML erstellen, und Desktop-Apps können Popups senden.


## <a name="step-2-send-a-toast"></a>Schritt 2: Senden eines Popups

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```csharp
// Requires Microsoft.Toolkit.Uwp.Notifications NuGet package version 7.0 or greater
new ToastContentBuilder()
    .AddArgument("action", "viewConversation")
    .AddArgument("conversationId", 9813)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, The Enchantments in Washington!")
    .Show(); // Not seeing the Show() method? Make sure you have version 7.0, and if you're using .NET 5, your TFM must be net5.0-windows10.0.17763.0 or greater
```

Versuchen Sie, diesen Code zu verwenden, und die Benachrichtigung sollte angezeigt werden.


## <a name="step-3-handling-activation"></a>Schritt 3: Behandeln der Aktivierung

Nachdem Sie eine Benachrichtigung angezeigt haben, müssen Sie wahrscheinlich den Benutzer behandeln, der auf die Benachrichtigung klickt (ob dies bedeutet, dass nach dem Klicken des Benutzers ein bestimmter Inhalt angezeigt wird, ihre App im Allgemeinen geöffnet wird oder eine Aktion erfolgt, wenn der Benutzer auf die Benachrichtigung klickt).

Die Schritte zum Behandeln der Aktivierung unterscheiden sich für UWP-, Desktop- (MSIX) und Desktop-Apps (nicht verpackt).


#### <a name="uwp"></a>[UWP](#tab/uwp)

Wenn der Benutzer auf Ihre Benachrichtigung klickt (oder auf eine Schaltfläche in der Benachrichtigung mit Vordergrundaktivierung), wird **App.xaml.cs** **OnActivated** Ihrer App aufgerufen, und die argumente, die Sie hinzugefügt haben, werden zurückgegeben.

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

#### <a name="desktop-msix"></a>[Desktop (MSIX)](#tab/desktop-msix)

Fügen Sie zunächst in **Ihrer Datei Package.appxmanifest** Dies hinzu:

1. Deklaration für **xmlns:com**
1. Deklaration für **xmlns:desktop**
1. Im **IgnorableNamespaces-Attribut** com **und** **desktop**
1. **desktop:Erweiterung** für **windows.toastNotificationActivation** zum Deklarieren ihrer Popupaktivator-CLSID (mit einer neuen GUID Ihrer Wahl).
1. Nur MSIX: **com:Extension** for the COM activator using the GUID from step #4. Stellen `Arguments="-ToastActivated"` Sie sicher, dass Sie einschließen, damit Sie wissen, dass Ihr Start über eine Benachrichtigung erfolgt ist.

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

Abonnieren Sie dann **im Startcode Ihrer App** (App.xaml.cs OnStartup für WPF) das OnActivated-Ereignis.

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]


[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]


#### <a name="desktop-unpackaged"></a>[Desktop (entpackt)](#tab/desktop)

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]

Abonnieren **Sie im Startcode Ihrer App** (App.xaml.cs OnStartup für WPF) das OnActivated-Ereignis.

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]

---


## <a name="step-4-handling-uninstallation"></a>Schritt 4: Behandeln der Deinstallation

#### <a name="uwp"></a>[UWP](#tab/uwp)

Sie müssen nichts tun! Wenn UWP-Apps deinstalliert werden, werden alle Benachrichtigungen und alle anderen zugehörigen Ressourcen automatisch bereinigt.

#### <a name="desktop-msix"></a>[Desktop (MSIX)](#tab/desktop-msix)

Sie müssen nichts tun! Wenn MSIX-Apps deinstalliert werden, werden alle Benachrichtigungen und alle anderen zugehörigen Ressourcen automatisch bereinigt.

#### <a name="desktop-unpackaged"></a>[Desktop (entpackt)](#tab/desktop)

Wenn Ihre App über ein Deinstallationsprogramm verfügt, sollten Sie in Ihrem Deinstallationsprogramm `ToastNotificationManagerCompat.Uninstall();` aufrufen. Wenn Es sich bei Ihrer App um eine "portable App" ohne Installationsprogramm handelt, sollten Sie diese Methode beim Beenden der App aufrufen, es sei denn, Sie verfügen über Benachrichtigungen, die beibehalten werden sollen, nachdem Ihre App geschlossen wurde.

Die Deinstallationsmethode bereinigt alle geplanten und aktuellen Benachrichtigungen, entfernt alle zugehörigen Registrierungswerte und entfernt alle zugeordneten temporären Dateien, die von der Bibliothek erstellt wurden.

---


## <a name="adding-images"></a>Hinzufügen von Bildern

Sie können Benachrichtigungen umfangreiche Inhalte hinzufügen. Wir fügen ein Inlinebild und ein Profilbild (App-Logoüberschreibung) hinzu.

[!INCLUDE [images note](includes/images-note.md)]

> [!IMPORTANT]
> HTTP-Images werden nur in UWP-/MSIX-/Sparse-Apps unterstützt, die über die Internetfunktion in ihrem Manifest verfügen. Desktop-Nicht-MSIX-/Sparse-Apps unterstützen keine HTTP-Images. Sie müssen das Image in Ihre lokalen App-Daten herunterladen und lokal darauf verweisen.

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

Sie können Schaltflächen und Eingaben hinzufügen, um Ihre Benachrichtigungen interaktiv zu gestalten. Schaltflächen können Ihre Vordergrund-App, ein Protokoll oder Ihre Hintergrundaufgabe starten. Wir fügen ein Antworttextfeld, eine Schaltfläche "Gefällt mir" und eine Schaltfläche "Ansicht" hinzu, mit der das Bild geöffnet wird.

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

Die Aktivierung von Vordergrundschaltflächen wird auf die gleiche Weise wie der Hauptkörper des Popups behandelt (Ihre App.xaml.cs OnActivated wird aufgerufen).

Beachten Sie, dass Argumente, die dem Popup der obersten Ebene hinzugefügt werden (z. B. die Konversations-ID), auch zurückgegeben werden, wenn auf die Schaltflächen geklickt wird, solange schaltflächen die AddArgument-API wie oben beschrieben verwenden (wenn Sie argumente auf einer Schaltfläche benutzerdefiniert zuweisen, werden die Argumente der obersten Ebene nicht eingeschlossen).



## <a name="handling-background-activation"></a>Behandeln der Hintergrundaktivierung

#### <a name="uwp"></a>[UWP](#tab/uwp)

Wenn Sie die Hintergrundaktivierung für Das Popup (oder für eine Schaltfläche innerhalb des Popups) angeben, wird Ihre Hintergrundaufgabe ausgeführt, anstatt Ihre Vordergrund-App zu aktivieren.

Weitere Informationen zu Hintergrundaufgaben finden Sie unter [Unterstützen Ihrer App mit Hintergrundaufgaben.](../../../launch-resume/support-your-app-with-background-tasks.md)

Wenn Sie auf Build 14393 oder höher abzielen, können Sie prozessspezifische Hintergrundaufgaben verwenden, die die Dinge deutlich vereinfachen. Beachten Sie, dass In-Process-Hintergrundaufgaben unter älteren Versionen von Windows nicht ausgeführt werden können. In diesem Codebeispiel wird eine In-Process-Hintergrundaufgabe verwendet.

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


Überschreiben Sie dann in Ihrer Datei App.xaml.cs die OnBackgroundActivated-Methode. Sie können dann die vordefinierten Argumente und Benutzereingaben ähnlich der Vordergrundaktivierung abrufen.

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

Bei Desktopanwendungen werden Hintergrundaktivierungen genauso wie Vordergrundaktivierungen behandelt (Ihr **OnActivated-Ereignishandler** wird ausgelöst). Sie können auswählen, dass keine Benutzeroberfläche angezeigt und Ihre App nach der Aktivierung geschlossen wird.

---


## <a name="set-an-expiration-time"></a>Festlegen einer Ablaufzeit

In Windows 10 werden alle Popupbenachrichtigungen im Action Center angezeigt, nachdem sie vom Benutzer geschlossen oder ignoriert wurden, sodass Benutzer Ihre Benachrichtigung nach dem Popup sehen können.

Wenn die Nachricht in Ihrer Benachrichtigung jedoch nur für einen bestimmten Zeitraum relevant ist, sollten Sie eine Ablaufzeit für die Popupbenachrichtigung festlegen, damit den Benutzern keine veralteten Informationen aus Ihrer App angezeigt werden. Wenn eine Heraufstufung beispielsweise nur 12 Stunden gültig ist, legen Sie die Ablaufzeit auf 12 Stunden fest. Im folgenden Code legen wir die Ablaufzeit auf 2 Tage fest.

> [!NOTE]
> Die Standard- und maximale Ablaufzeit für lokale Popupbenachrichtigungen beträgt 3 Tage.

```csharp
// Create toast content and show the toast!
new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .Show(toast =>
    {
        toast.ExpirationTime = DateTime.Now.AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>Geben Sie einen Primärschlüssel für Ihr Popup an.

Wenn Sie die gesendete Benachrichtigung programmgesteuert entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen Primärschlüssel für Ihre Benachrichtigung bereitzustellen. Anschließend können Sie diesen Primärschlüssel in Zukunft verwenden, um die Benachrichtigung zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/Entfernen bereits übermittelter Popupbenachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popupbenachrichtigungen in Info-Center (XAML).](/previous-versions/windows/apps/dn631260(v=win.10))

Tag und Gruppe fungieren kombiniert als zusammengesetzter Primärschlüssel. Die Gruppe ist der allgemeinere Bezeichner, mit dem Sie Gruppen wie "wallPosts", "messages", "friendRequests" usw. zuweisen können. Anschließend sollte tag die Benachrichtigung selbst innerhalb der Gruppe eindeutig identifizieren. Mithilfe einer generischen Gruppe können Sie dann alle Benachrichtigungen aus dieser Gruppe entfernen, indem Sie die [RemoveGroup-API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)verwenden.

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



## <a name="clear-your-notifications"></a>Löschen Ihrer Benachrichtigungen

Apps sind dafür verantwortlich, ihre eigenen Benachrichtigungen zu entfernen und zu löschen. Wenn Ihre App gestartet wird, löschen wir Ihre Benachrichtigungen NICHT automatisch.

Windows entfernt eine Benachrichtigung nur automatisch, wenn der Benutzer explizit auf die Benachrichtigung klickt.

Hier ist ein Beispiel dafür, was eine Messaging-App tun sollte...

1. Benutzer empfängt mehrere Popups zu neuen Nachrichten in einer Konversation
2. Benutzer tippt auf eines dieser Popups, um die Konversation zu öffnen
3. Die App öffnet die Konversation und löscht dann alle Popups für diese Konversation (mit [removeGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) in der von der App bereitgestellten Gruppe für diese Konversation).
4. Das Aktionscenter des Benutzers spiegelt nun den Benachrichtigungsstatus ordnungsgemäß wider, da im Aktionscenter keine veralteten Benachrichtigungen für diese Konversation mehr übrig sind.

Informationen zum Löschen aller Benachrichtigungen oder zum Entfernen bestimmter Benachrichtigungen finden Sie unter Schnellstart: Verwalten von Popupbenachrichtigungen [in Info-Center (XAML).](/previous-versions/windows/apps/dn631260(v=win.10))

```csharp
ToastNotificationManagerCompat.History.Clear();
```



## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [Dokumentation zu Popupinhalten](adaptive-interactive-toasts.md)
* [ToastNotification-Klasse](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs-Klasse](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
