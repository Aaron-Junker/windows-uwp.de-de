---
Description: Erfahren Sie, wie Win32 C# apps können lokale Popupbenachrichtigungen zu senden und verarbeiten den Benutzer auf den Toast.
title: Senden von Popupbenachrichtigungen über C#-Apps
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: Windows 10, Uwp, win32, Desktop, Popupbenachrichtigungen, senden Sie ein Popup, senden Sie die lokalen Toast, desktop-Brücke, C#, C scharfe, Toast-Benachrichtigung, Wpf
ms.localizationpriority: medium
ms.openlocfilehash: 907ba19812c9a34a7a91f42fefac4c190bfd394b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648875"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>Senden von Popupbenachrichtigungen über C#-Apps

Desktop-Apps (Desktop-Brücke und klassische Win32) können interaktive Popupbenachrichtigungen wie Universelle Windows-Plattform (UWP)-Apps senden. Es gibt jedoch einige besondere Schritte für Desktop-Apps mit anderen Aktivierungsschematas und dem potenziellen Mangel der Paketidentität, wenn Sie die Desktop-Brücke nicht verwenden.

> [!IMPORTANT]
> Wenn Sie eine UWP-App entwickeln, finden Sie Informationen in der [UWP-Dokumentation](send-local-toast.md). Bei anderen Desktop Sprachen finden Sie Informationen unter [Desktop C++ WRL](send-local-toast-desktop-cpp-wrl.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Schritt 1: Aktivieren des Windows 10 SDK

Wenn Sie Windows 10 SDK nicht für Ihre Win32-App aktiviert haben, müssen Sie dies zuerst tun.

Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie dann **Projekt entladen** aus.

![Entladen eines Projekts](images/win32-unload-project.png)

Klicken Sie dann mit der rechten Maustaste auf das Projekt, und wählen Sie **[Projektname] csproj bearbeiten** aus

![Bearbeiten eines Projekts](images/win32-edit-project.png)

Unterhalb des vorhandenen `<TargetFrameworkVersion>`-Knotens, fügen Sie einen neuen `<TargetPlatformVersion>`-Knoten hinzu und geben Sie Ihre Mindestversion von Windows 10 an. Das aktuell verwendete SDK ist die aktuelle SDK-Version, die Sie auf Ihrem Entwicklercomputer installiert haben. Dies gibt Ihre erlaubte Mindestversion an (und ermöglicht es Ihnen, auf das Windows SDK zu verweisen).

```xml
...
<TargetFrameworkVersion>...</TargetFrameworkVersion>
<TargetPlatformVersion>10.0.10240.0</TargetPlatformVersion>
...
```

Speichern und Laden Sie das Projekt dann neu.

![Ein Projekt neu laden](images/win32-reload-project.png)


## <a name="step-2-reference-the-apis"></a>Schritt 2: Verweisen auf die APIs

Öffnen Sie den Verweis-Manager (klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen -> Verweis hinzufügen**) und dann **Windows -> Core** aus und geben Sie folgende Referenzen an:

* Windows.Data
* Windows.UI

![Verweis-Manager](images/win32-add-windows-reference.png)


## <a name="step-3-copy-compat-library-code"></a>Schritt 3: Compat-Bibliothekscode kopieren

Kopieren Sie [DesktopNotificationManagerCompat.cs-Datei von GitHub](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CS/DesktopToastsApp/DesktopNotificationManagerCompat.cs) in Ihr Projekt. Die Compat-Bibliothek abstrahiert einen Großteil der Komplexität der Desktop-Benachrichtigungen. Die folgenden Anweisungen erfordern die Compat-Bibliothek.


## <a name="step-4-implement-the-activator"></a>Schritt 4: Implementieren Sie die Aktivierung

Sie müssen einen Handler für den Toast-Aktivierung implementieren, sodass klickt der Benutzer auf Ihre Toast, Ihre app etwas durchführen kann. Dies ist erforderlich für das Popup, damit es im Info-Center beibehalten wird (da auf das Popup Tage später geklickt werden kann, wenn die App geschlossen ist). Diese Klasse kann an eine beliebige Stelle in Ihrem Projekt platziert werden.

Erweitern Sie die **NotificationActivator**-Klasse, und fügen Sie die drei Attribute hinzu, die unten aufgeführt sind. Erstellen Sie dann eine eindeutige GUID CLSID für Ihre App mithilfe einer der vielen online GUID-Generatoren. Durch diese CLSID (Klassen-ID) weiß das Info-Center, welche Klasse für COM aktiviert werden soll.

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


## <a name="step-5-register-with-notification-platform"></a>Schritt 5: Registrieren bei benachrichtigungsplattform

Anschließend müssen Sie eine Registrierung mit der Benachrichtigungsplattform durchführen. Es sind unterschiedliche Schritte erforderlich, je nachdem, ob Sie die Desktop-Brücke oder klassische Win32 verwenden. Wenn Sie beide unterstützen, müssen Sie beide Schritte durchführen (der Code bleibt allerdings gleich, die Bibliothek führt die Verzweigung für Sie durch!).


### <a name="desktop-bridge"></a>Desktop-Brücke

Bei Verwendung der Desktop-Brücke (oder wenn Sie beide Modi unterstützen), fügen Sie Ihrem **Package.appxmanifest** hinzu:

1. Deklaration für **Xmlns:com**
2. Deklaration für **xmlns:desktop**
3. Im **IgnorableNamespaces**-Attribut **com** und **desktop**
4. **com:Extension** für den COM-Aktivator mithilfe der GUID aus Schritt 4. Stellen Sie sicher, dass `Arguments="-ToastActivated"` hinzugefügt wurde, damit Sie wissen, dass der Start über ein Popup ausgeführt wurde
5. **desktop:Extension** für **windows.toastNotificationActivation**, um den Popup-Aktivator CLSID zu deklarieren (der GUID aus Schritt #4).

**Datei "Package.appxmanifest"**

```xml
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


### <a name="classic-win32"></a>Klassisch Win32

Wenn Sie eine klassische Win32 verwenden (oder wenn Sie beide Modi unterstützen), müssen Sie Ihre Anwendungsbenutzermodell-ID (AUMID) und den Popupbenachrichtigungs-Aktivator CLSID (der GUID aus Schritt 4 #) auf der App-Verknüpfung im Startmenü deklarieren.

Wählen Sie einen eindeutigen AUMID, der Ihrer Win32-App identifiziert. Dies geschieht in der Regel in Form von [Firmenname].[AppName], es sollte allerdings in allen Apps eindeutig sein (fügen Sie Ziffern am Ende hinzu).

#### <a name="step-51-wix-installer"></a>Schritt 5.1: WiX-Installationsprogramms

Wenn Sie WiX als Installer verwenden, bearbeiten Sie die **Product.wxs**-Datei, um dem Startmenü zwei Verknüpfungseigenschaften hinzuzufügen, wie unten dargestellt. Stellen Sie sicher, dass Ihre GUID aus Schritt 4 # in `{}` eingeschlossen ist, wie unten dargestellt.

**Product.WXS**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Um Benachrichtigungen tatsächlich verwenden zu können, müssen Sie Ihrer App durch den Installer einmal vor dem Debuggen installieren, damit die Verknüpfung auf dem Startmenü mit AUMID und CLSID vorhanden ist. Nachdem die Verknüpfung auf dem Startmenü vorhanden ist, können Sie mithilfe von F5 in Visual Studio debuggen.


#### <a name="step-52-register-aumid-and-com-server"></a>Schritt 5.2: Registrieren Sie die AUMID und COM-server

Rufen Sie unabhängig vom Installer im App Startcode (vor dem Aufrufen der Benachrichtigungs-APIs), die **RegisterAumidAndComServer**-Methode auf, und geben Sie die Benachrichtigungs-Aktivator-Klasse von Schritt 4 und die oben verwendete AUMID an.

```csharp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

Wenn Desktop-Brücke und klassisches Win32 unterstützt werden, können Sie diese Methode unabhängig davon aufgerufen. Wenn Sie die Methode mit der Desktop-Brücke ausführen, wird sie sofort zurückgegeben. Sie müssen den Code nicht verzweigen.

Mit dieser Methode können Sie Compat-APIs zum Senden und Verwalten von Benachrichtigungen aufrufen, ohne ständig die AUMID angeben zu müssen. Es fügt ebenfalls den LocalServer32-Schlüssel für den COM-Server hinzu.


## <a name="step-6-register-com-activator"></a>Schritt 6: Registrieren von COM-Aktivierung

Für Desktop-Brücken und klassische Win32-Apps müssen Sie Ihren Benachrichtigung-Aktivatortyp registrieren, damit Sie Popupaktivierungen behandeln können.

Rufen Sie im App-Startcode die folgende **RegisterActivator**-Methode auf und übergeben Sie die Implementierung der **NotificationActivator**-Klasse, die Sie in Schritt 4 # erstellt haben. Dies muss in der Reihenfolge aufgerufen werden, in der Sie alle Popupaktivierungen erhalten.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-7-send-a-notification"></a>Schritt 7: Senden einer Benachrichtigung

Das Senden einer Benachrichtigung ist für UWP-Apps identisch, außer dass Sie die **DesktopNotificationManagerCompat**-Klasse zum Erstellen eines **ToastNotifier** verwenden. Die Compat-Bibliothek behandelt den Unterschied zwischen Desktop-Brücke und klassischer Win32 automatisch, damit Sie keinen Code verzweigen müssen. Für klassisches Win32 speichert die Compat-Bibliothek Ihre AUMID, die Sie beim Aufrufen von **RegisterAumidAndComServer** bereitgestellt haben, damit Sie sich nicht die AUMID kümmern müssen, wann Sie diese bereitstellen und wann nicht.

> [!NOTE]
> Installieren Sie die [Benachrichtigungsbibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), damit Sie Benachrichtigungen mithilfe von C#-Code erstellen können anstelle von unformatiertem XML.

Stellen Sie sicher, dass Sie den unten angezeigten **ToastContent** verwenden (oder die Vorlage „ToastGeneric”, wenn Sie XML selbst erstellen), da ältere Vorlagen der Windows 8.1 Popupbenachrichtigungen nicht den COM-Benachrichtigungs-Aktivator aktivieren, den Sie in Schritt 4 # erstellt haben.

> [!IMPORTANT]
> HTTP-Bilder werden nur in Desktop-Brücken-Apps unterstützt, die die Internetfunktion im Manifest haben. Klassische Win32-Apps unterstützen keine HTTP-Bilder. Sie müssen das Bild in die lokalen App-Daten herunterladen und lokal darauf verweisen.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContent()
{
    // Arguments when the user taps body of toast
    Launch = "action=viewConversation&conversationId=5",

    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Hello world!"
                }
            }
        }
    }
};

// Create the XML document (BE SURE TO REFERENCE WINDOWS.DATA.XML.DOM)
var doc = new XmlDocument();
doc.LoadXml(toastContent.GetContent());

// And create the toast notification
var toast = new ToastNotification(doc);

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> Klassische Win32-Apps können keine älteren Popupvorlagen (z. B. ToastText02) verwenden. Die Aktivierung älterer Vorlagen schlägt fehl, wenn die COM-CLSID angegeben wird. Wie oben erwähnt, müssen Sie die Windows 10 ToastGeneric-Vorlagen verwenden.


## <a name="step-8-handling-activation"></a>Schritt 8: Behandeln der Aktivierung

Wenn der Benutzer auf das Popup klickt, wird die **OnActivated**-Methode Ihre **NotificationActivator**-Klasse aufgerufen.

Innerhalb der OnActivated-Methode können Sie die Argumente analysieren, die Sie im Popup angegeben haben und die Benutzereingabe abrufen, die der Benutzer eingegeben oder ausgewählt hat und dann entsprechend Ihre App aktivieren.

> [!NOTE]
> Die **OnActivated**-Methode wird nicht im UI-Thread aufgerufen. Wenn Sie UI-Thread Vorgänge ausführen möchten, rufen Sie `Application.Current.Dispatcher.Invoke(callback)` auf.

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

Si sollten in der `App.xaml.cs`-Datei die **OnStartup**-Methode (für WPF-Apps) überschreiben, um festzulegen, ob Sie über ein Popup oder nicht gestartet werden soll, um einen ordnungsgemäßen Start zu unterstützen, während die App geschlossen ist. Wenn dies von einem Popup gestartet wird, wird ein Start-Argument von "-ToastActivated" gesendet. Sobald dies angezeigt wird, sollten Sie das Ausführen aller normaler Aktivierungscodes beenden, und den Start des **OnActivated**-Code Handler starten.

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


### <a name="activation-sequence-of-events"></a>Aktivierungssequenz von Ereignissen

Für WPF ist die Aktivierungssequenz wie folgt...

Wenn Ihre App bereits ausgeführt wird:

1. **OnActivated** in Ihrer **NotificationActivator** wird aufgerufen

Wenn Ihre App nicht ausgeführt wird:

1. **OnStartup** in `App.xaml.cs` wird mit **Args** von "-ToastActivated" aufgerufen
2. **OnActivated** in Ihrer **NotificationActivator** wird aufgerufen


### <a name="foreground-vs-background-activation"></a>Vordergrund- und Hintergrundaktivierung
Für Desktop-Apps erfolgt die Vordergrund und Hintergrundaktivierung genauso – die COM-Aktivierung wird aufgerufen. Je nach dem Code Ihrer App wird entschieden, ob ein Fenster angezeigt wird oder einfach einige Aufgaben ausgeführt werden und die Aktion anschließend beendet wird. Aus diesem Grund ändert die Angabe eines **ActivationType** im **Hintergrund** Ihres Popup-Inhalts nichts am Verhalten.


## <a name="step-9-remove-and-manage-notifications"></a>Schritt 9: Entfernen und Verwalten von Benachrichtigungen

Das Entfernen und Verwalten von Benachrichtigungen ist identisch mit UWP-Apps. Es wird jedoch empfohlen, dass Sie unsere Compat-Bibliothek verwenden, um eine **DesktopNotificationHistoryCompat** zu erhalten, damit Sie sich keine Gedanken über das Bereitstellen der AUMID machen müssen, wenn Sie die klassische Win32 verwenden.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-10-deploying-and-debugging"></a>Schritt 10: Bereitstellen und Debuggen

Wenn Sie die Desktop-Brücke-App bereitstellen und debuggen möchten, lesen Sie [Ausführen, Debuggen und Testen eine verpackten Desktop-App](/windows/uwp/porting/desktop-to-uwp-debug).

Um eine klassische Win32-App bereitzustellen und zu verwalten, müssen Sie Ihrer App durch den Installer einmal vor dem Debuggen installieren, damit die Verknüpfung auf dem Startmenü mit AUMID und CLSID vorhanden ist. Nachdem die Verknüpfung auf dem Startmenü vorhanden ist, können Sie mithilfe von F5 in Visual Studio debuggen.

Wenn Ihre Benachrichtigungen nicht einfach in Ihrer klassische Win32-App angezeigt werden (und keine Ausnahmen ausgelöst werden), bedeutet dies wahrscheinlich, dass die Verknüpfung im Startmenü nicht vorhanden ist (installieren Sie Ihre App über das Installationsprogramm), oder die AUMID, die Sie im Code verwendet haben, nicht der AUMID der Verknüpfung auf dem Startmenü entspricht.

Wenn Ihre Benachrichtigungen im Info-Center erscheinen aber nicht dort angezeigt werden (verschwinden, nachdem das Popup geschlossen ist) beibehalten werden nicht persistent angezeigt werden, bedeutet dies, dass Sie die den COM-Aktivator nicht ordnungsgemäß implementiert haben.

Wenn Sie sowohl Ihre Desktop-Brücke und die klassische Win32-App installiert haben, beachten Sie, dass die App Desktop-Brücke die klassische Win32-App bei der Behandlung von Popup-Aktivierungen ablöst. Dies bedeutet, dass Popups aus der klassische Win32-App beim Klicken auf die Desktop-Brücke-App gestartet werden. Das Deinstallieren der App Desktop-Brücke setzt die Aktivierung auf die klassische Win32-App zurück.


## <a name="known-issues"></a>Bekannte Probleme

**FEST: App nicht nach dem Klicken auf die Toast Fokus**: In Builds 15063 und früher wurden nicht Foreground-Rechte für Ihre Anwendung übertragen werden, wenn wir den COM-Server aktiviert. Aus diesem Grund führte Ihre App einfach einen Flash aus, bei dem Versuch, sie in den Vordergrund zu verschieben. Es gibt hierfür keine Problemumgehung. Wir haben dies in Builds 16299 und höher behoben.


## <a name="resources"></a>Ressourcen

* [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Toastbenachrichtigungen aus desktop-apps](toast-desktop-apps.md)
* [Toast-Content-Dokumentation](adaptive-interactive-toasts.md)

