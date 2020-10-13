---
Description: Erfahren Sie, wie Win32-c#-apps lokale Popup Benachrichtigungen senden können und den Benutzer durch Klicken auf den Toast behandeln können.
title: Senden einer lokalen Popup Benachrichtigung von Win32 c#-apps
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from Win32 C# apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: 'Windows 10, UWP, Win32, Desktop, Popup Benachrichtigungen, Senden eines Popup, Senden eines lokalen Popup, Desktop Bridge, msix, sparsesloadbenachrichtigungen, c#, C-Sharp, Popup Benachrichtigung, WPF, Senden von Popup Benachrichtigungen WPF, Popup Benachrichtigung, e/a, Popup Benachrichtigung C #'
ms.localizationpriority: medium
ms.openlocfilehash: b13927bbd12a5cb306018ca02cd8730f580182cd
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984646"
---
# <a name="send-a-local-toast-notification-from-win32-c-apps"></a>Senden einer lokalen Popup Benachrichtigung von Win32 c#-apps

Win32-Apps (einschließlich gepackter [msix](/windows/msix/desktop/source-code-overview) -apps, apps, die [Pakete](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) mit geringer Dichte zum Abrufen der Paket Identität verwenden, und klassische, nicht gepackte Win32-Apps) können interaktive Popup Benachrichtigungen wie Windows-apps senden. Allerdings gibt es einige spezielle Schritte für Win32-apps aufgrund der verschiedenen Aktivierungs Schemas und des potenziellen Mangels an Paket Identität, wenn Sie keine msix-oder Sparse-Pakete verwenden.

> [!IMPORTANT]
> Wenn Sie eine UWP-app schreiben, finden Sie weitere Informationen in der [UWP-Dokumentation](send-local-toast.md). Weitere Desktop Sprachen finden Sie unter [Win32 C++ WRL](send-local-toast-desktop-cpp-wrl.md).


## <a name="step-1-install-the-notifications-library"></a>Schritt 1: Installieren der Benachrichtigungs Bibliothek

Installieren Sie das `Microsoft.Toolkit.Uwp.Notifications` [nuget-Paket](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) in Ihrem Projekt.

Diese [Benachrichtigungs Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) fügt den Kompatibilitäts-Bibliotheks Code zum Arbeiten mit Popup Benachrichtigungen von Win32-apps hinzu. Außerdem verweist Sie auf die UWP-sdche und ermöglicht das Erstellen von Benachrichtigungen mit c# anstelle von unformatierten XML-Daten. Der Rest dieses Schnellstarts hängt von der Benachrichtigungs Bibliothek ab.


## <a name="step-2-implement-the-activator"></a>Schritt 2: Implementieren Sie den Aktivator.

Sie müssen einen Handler für die Toast Aktivierung implementieren, damit Ihre APP etwas Unternehmen kann, wenn der Benutzer auf den Popup klickt. Dies ist erforderlich, damit der Popup im Aktions Center persistent gespeichert wird (da der Popup-Vorgang Tage später angezeigt werden kann, wenn die app geschlossen wird). Diese Klasse kann an beliebiger Stelle in Ihrem Projekt platziert werden.

Erstellen Sie eine neue **mynotificationactivator** -Klasse, und erweitern Sie die **notificationactivator** -Klasse. Fügen Sie die drei unten aufgeführten Attribute hinzu, und erstellen Sie mit einem der vielen Online-GUID-Generatoren eine eindeutige GUID-CLSID für Ihre APP. Diese CLSID (Klassen Bezeichner) gibt an, wie das Aktions Center weiß, welche Klasse für com aktiviert werden soll.

**MyNotificationActivator.cs** (diese Datei erstellen)

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-3-register-with-notification-platform"></a>Schritt 3: Registrieren bei der Benachrichtigungs Plattform

Anschließend müssen Sie sich bei der Benachrichtigungs Plattform registrieren. Je nachdem, ob Sie msix/Sparse-Pakete oder klassisches Win32 verwenden, gibt es unterschiedliche Schritte. Wenn Sie beide unterstützen, müssen Sie beide Schritte ausführen (Sie müssen jedoch nicht Ihren Code verzweigen, da die Bibliothek dies für Sie erledigt!).


#### <a name="msixsparse-packages"></a>[Msix/Pakete mit geringer Dichte](#tab/msix-sparse)

Fügen Sie in der Datei " **Package. appxmanifest**" Folgendes hinzu, wenn Sie ein [msix](/windows/msix/desktop/source-code-overview) -Paket oder ein [Sparse-Paket](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) verwenden (oder beide unterstützen):

1. Deklaration für **xmlns: com**
2. Deklaration für **xmlns: Desktop**
3. Im **ignorablenamespaces** -Attribut, **com** und **Desktop**
4. **com: Erweiterung** für den com-Activator, der die GUID aus Schritt #2 verwendet. Achten Sie darauf, `Arguments="-ToastActivated"` dass Sie das einschließen, damit Sie wissen, dass der Start von einem Toast stammt.
5. **Desktop: Erweiterung** für **Windows. toastnotificationactivation** zum Deklarieren der Popup-Activator-CLSID (die GUID aus Schritt #2).

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

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


#### <a name="classic-win32"></a>[Klassisches Win32](#tab/classic)

Wenn Sie klassisches Win32 verwenden (oder wenn Sie beides unterstützen), müssen Sie die Anwendungs Benutzer Modell-ID (aumid) und die Popup-Activator-CLSID (die GUID aus Schritt #2) in der Verknüpfung ihrer App im Startmenü deklarieren.

Wählen Sie eine eindeutige aumid aus, mit der ihre Win32-App identifiziert wird. Dies erfolgt in der Regel in der Form [CompanyName]. [AppName], aber Sie möchten sicherstellen, dass dies in allen apps eindeutig ist (Sie können am Ende einige Ziffern hinzufügen).

### <a name="step-31-wix-installer"></a>Schritt 3,1: WiX-Installer

Wenn Sie WiX für das Installationsprogramm verwenden, bearbeiten Sie die Datei " **Product. wxs** ", um die beiden Verknüpfungs Eigenschaften zur Start Menü Verknüpfung hinzuzufügen, wie unten gezeigt. Stellen Sie sicher, dass die GUID aus Schritt #2 in eingeschlossen ist, `{}` wie unten gezeigt.

**Product. wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Um Benachrichtigungen tatsächlich verwenden zu können, müssen Sie die APP zunächst vor dem Debuggen über das Installationsprogramm installieren, damit die Start Verknüpfung mit ihrer aumid und CLSID vorhanden ist. Nachdem die Start Verknüpfung vorhanden ist, können Sie in Visual Studio mit F5 Debuggen.


### <a name="step-32-register-aumid-and-com-server"></a>Schritt 3,2: Registrieren von aumid und com-Server

Rufen Sie dann unabhängig vom Installationsprogramm im Startcode Ihrer APP (vor dem Aufrufen von Benachrichtigungs-APIs) die **registeraumidandcomserver** -Methode auf, und geben Sie dann die Benachrichtigungs-Activator-Klasse aus Schritt #2 und ihrer zuvor verwendeten aumid an.

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

Wenn Sie sowohl das msix/Sparse-Paket als auch das klassische Win32-Paket unterstützen, können Sie diese Methode unabhängig davon aufzurufen. Wenn Sie in einem msix/Sparse-Paket ausführen, wird diese Methode einfach sofort zurückgegeben. Es ist nicht erforderlich, den Code zu verzweigen.

Diese Methode ermöglicht es Ihnen, die Kompatibilitäts-APIs aufzurufen, um Benachrichtigungen zu senden und zu verwalten, ohne die aumid ständig bereitstellen zu müssen. Außerdem wird der Registrierungsschlüssel LocalServer32 für den com-Server eingefügt.

---


## <a name="step-4-register-com-activator"></a>Schritt 4: Registrieren des com-Activators

Sowohl für das msix/Sparse-Paket als auch für klassische Win32-apps müssen Sie den Typ des Benachrichtigungs aktivierers registrieren, damit Sie Popup Aktivierungen verarbeiten können.

Aufrufen Sie im Startcode Ihrer APP die folgende **registeractivator** -Methode, und übergeben Sie die Implementierung der **notificationactivator** -Klasse, die Sie in Schritt #2 erstellt haben. Dies muss aufgerufen werden, damit Sie beliebige Popup Aktivierungen empfangen können.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-5-send-a-notification"></a>Schritt 5: Senden einer Benachrichtigung

Das Senden einer Benachrichtigung ist mit UWP-apps identisch, mit dem Unterschied, dass Sie die **desktopnotificationmanagercompat** -Klasse verwenden, um einen Objekt **-Benachrichtigungs**Dienst zu erstellen. Die Kompatibilitäts-Bibliothek behandelt automatisch den Unterschied zwischen dem msix/Sparse-Paket und dem klassischen Win32, sodass Sie den Code nicht verzweigen müssen. Bei klassischem Win32 speichert die Kompatibilitäts-Bibliothek die von Ihnen beim Aufrufen von **registeraumidandcomserver** bereitgestellte aumid zwischen, sodass Sie sich keine Gedanken machen müssen, wann die aumid bereitgestellt oder nicht.

> [!NOTE]
> Installieren Sie die [Benachrichtigungs Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) , damit Sie mithilfe von c#, wie unten gezeigt, Benachrichtigungen erstellen können, anstatt unformatierten XML-Code zu verwenden.

Stellen Sie sicher, dass Sie den unten gezeigten **Inhalts Inhalt** verwenden (oder die toastgeneric-Vorlage, wenn Sie ein manuelles Erstellen von XML verwenden), da die Legacy-Windows 8.1 Popup Benachrichtigungs Vorlagen den in Schritt #2 erstellten com-Benachrichtigungs Aktivator nicht aktivieren.

> [!IMPORTANT]
> Http-Images werden nur in msix/Sparse-Paket-Apps unterstützt, die über die Internetfunktion in ihrem Manifest verfügen. Klassische Win32-apps unterstützen keine http-Bilder. Sie müssen das Image in Ihre lokalen app-Daten herunterladen und lokal darauf verweisen.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContentBuilder()
    .AddToastActivationInfo("action=viewConversation&conversationId=5", ToastActivationType.Foreground)
    .AddText("Hello world!")
    .GetToastContent();

// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> Klassische Win32-Apps können keine Legacy-Popup Vorlagen verwenden (z. b. ToastText02). Die Aktivierung der Legacy Vorlagen schlägt fehl, wenn die com-CLSID angegeben wird. Sie müssen die oben gezeigten Windows 10-Standardvorlagen verwenden.


## <a name="step-6-handling-activation"></a>Schritt 6: Behandeln der Aktivierung

Wenn der Benutzer auf den Popup klickt, wird die **onaktivierte** Methode der **notificationactivator** -Klasse aufgerufen.

Innerhalb der onaktivierten Methode können Sie die args analysieren, die Sie im Toast angegeben haben, und die Benutzereingabe abrufen, die der Benutzer eingegeben oder ausgewählt hat, und die APP dann entsprechend aktivieren.

> [!NOTE]
> Die **onaktivierte** Methode wird nicht im UI-Thread aufgerufen. Wenn Sie UI-Thread Vorgänge durchführen möchten, müssen Sie den Befehl ausführen `Application.Current.Dispatcher.Invoke(callback)` .

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

Um das Starten zu unterstützen, während Ihre APP geschlossen ist, sollten Sie in der `App.xaml.cs` Datei die **OnStartup** -Methode (für WPF-Apps) überschreiben, um zu bestimmen, ob Sie von einem Toast gestartet werden. Wenn Sie von einem Toast aus gestartet wird, wird eine Start-arg von "-toastaktiviert" angezeigt. Wenn Sie dies sehen, sollten Sie den normalen Start Aktivierungscode nicht mehr ausführen und den Start des **onaktivierten** Code Handles zulassen.

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>Aktivierungs Sequenz von Ereignissen

Für WPF lautet die Aktivierungs Sequenz wie folgt...

Wenn Ihre APP bereits ausgeführt wird:

1. " **Onaktivierte** " in " **notificationactivator** " wird aufgerufen.

Wenn Ihre APP nicht ausgeführt wird:

1. " **OnStartup** " in `App.xaml.cs` wird mit den **args** "-onastaktivierte" aufgerufen.
2. " **Onaktivierte** " in " **notificationactivator** " wird aufgerufen.


### <a name="foreground-vs-background-activation"></a>Vordergrund-vs-Hintergrund Aktivierung
Bei Win32-apps wird die Vordergrund-und Hintergrund Aktivierung identisch behandelt. der com-Activator wird aufgerufen. Es liegt an Ihrem app-Code, zu entscheiden, ob ein Fenster angezeigt werden soll, oder einfach nur einige Aufgaben auszuführen und zu beenden. Wenn Sie also einen **ActivationType** von **Background** in Ihrem Popup Inhalt angeben, ändert sich das Verhalten nicht.


## <a name="step-7-remove-and-manage-notifications"></a>Schritt 7: entfernen und Verwalten von Benachrichtigungen

Das Entfernen und Verwalten von Benachrichtigungen ist identisch mit UWP-apps. Es wird jedoch empfohlen, dass Sie unsere Kompatibilitäts-Bibliothek verwenden, um eine **desktopnotificationhistorycompat** -Klasse zu erhalten, sodass Sie sich keine Gedanken über die Bereitstellung der aumid machen müssen, wenn Sie klassisches Win32 verwenden.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-8-deploying-and-debugging"></a>Schritt 8: Bereitstellen und Debuggen

Informationen zum Bereitstellen und Debuggen Ihrer msix-App finden Sie unter [ausführen, Debuggen und Testen einer gepackten Desktop-App](/windows/msix/desktop/desktop-to-uwp-debug)

Wenn Sie Ihre klassische Win32-App bereitstellen und debuggen möchten, müssen Sie die APP einmal vor dem Debuggen über das Installationsprogramm installieren, damit die Start Verknüpfung mit ihrer aumid und CLSID vorhanden ist. Nachdem die Start Verknüpfung vorhanden ist, können Sie in Visual Studio mit F5 Debuggen.

Wenn Ihre Benachrichtigungen einfach nicht in ihrer klassischen Win32-App angezeigt werden (und keine Ausnahmen ausgelöst werden), bedeutet dies wahrscheinlich, dass die Start Verknüpfung nicht vorhanden ist (installieren Sie Ihre APP über das Installationsprogramm), oder die im Code verwendete aumid stimmt nicht mit der aumid in der Start Verknüpfung.

Wenn Ihre Benachrichtigungen angezeigt werden, aber nicht im Aktions Center gespeichert sind (verschwindet, nachdem das Popup verworfen wurde), bedeutet dies, dass Sie den com-Activator nicht ordnungsgemäß implementiert haben.

Wenn Sie sowohl das msix/Sparse-Paket als auch die klassische Win32-App installiert haben, beachten Sie, dass die msix/Sparse-Paket-app die klassische Win32-App ersetzt, wenn Sie Popup Aktivierungen verarbeiten. Dies bedeutet, dass bei einem Klick von der klassischen Win32-App aus der klassischen Win32-App weiterhin die msix/Sparse-Paket-app gestartet wird. Wenn Sie die msix/Sparse-Paket-app deinstallieren, werden die Aktivierungen wieder in der klassischen Win32-APP wieder hergestellt.


## <a name="known-issues"></a>Bekannte Probleme

**Korrigiert: die APP wird nach dem Klicken auf "Toast" nicht fokussiert**: in Builds 15063 und früher wurden keine Vordergrund Rechte an Ihre Anwendung übertragen, als der com-Server aktiviert wurde. Daher würde Ihre APP einfach blinken, wenn Sie versucht haben, Sie in den Vordergrund zu verschieben. Für dieses Problem gibt es keine Problem Umgehung. Wir haben dies in Build 16299 und höher korrigiert.


## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Popup Benachrichtigungen von Win32-apps](toast-desktop-apps.md)
* [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
