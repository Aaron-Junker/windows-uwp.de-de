---
Description: Erfahren Sie, wie Win32 C++ WRL-apps lokale Popupbenachrichtigungen zu senden und verarbeiten den Benutzer auf den Toast.
title: Senden von Popupbenachrichtigungen über C++ WRL-Apps
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP, win32, Desktop, Popupbenachrichtigungen, Popup senden, lokale Popupbenachrichtigungen senden, Desktop Bridge, C++, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: af6c4392d97bdbf06c3e185b8a1c0235225bfe5a
ms.sourcegitcommit: 81021b7930905beb67383a08b09520cf95c68fd9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65940313"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>Senden von Popupbenachrichtigungen über C++ WRL-Apps

Desktop-Apps (Desktop-Brücke und klassische Win32) können interaktive Popupbenachrichtigungen wie Universelle Windows-Plattform (UWP)-Apps senden. Es gibt jedoch einige besondere Schritte für Desktop-Apps mit anderen Aktivierungsschematas und dem potenziellen Mangel der Paketidentität, wenn Sie die Desktop-Brücke nicht verwenden.

> [!IMPORTANT]
> Wenn Sie eine UWP-App entwickeln, finden Sie Informationen in der [UWP-Dokumentation](send-local-toast.md). Bei anderen Desktop Sprachen finden Sie Informationen unter [Desktop C#](send-local-toast-desktop.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Schritt 1: Aktivieren des Windows 10 SDK

Wenn Sie Windows 10 SDK nicht für Ihre Win32-App aktiviert haben, müssen Sie dies zuerst tun. Es sind einige wichtige Schritte erforderlich...

1. Hinzufügen von `runtimeobject.lib` auf **Zusätzliche Abhängigkeiten**
2. Aufrufen von Windows 10 SDK

Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie dann **Eigenschaften** aus.

Wählen Sie im oberen **Konfigurationsmenü** **alle Konfigurationen** aus, damit die folgende Änderung auf das Debuggen und die Freigabe angewendet wird.

Fügen Sie unter **Linker-> Eingabe**`runtimeobject.lib` den **zusätzlichen Abhängigkeiten** hinzu.

Unter **Allgemein**, stellen Sie sicher, dass die **Windows SDK-Version** auf 10.0 oder höher festgelegt ist (nicht Windows 8.1).


## <a name="step-2-copy-compat-library-code"></a>Schritt 2: Compat-Bibliothekscode kopieren

Kopieren Sie [DesktopNotificationManagerCompat.h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) und [DesktopNotificationManagerCompat.cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) von GitHub-Datei in Ihr Projekt. Die Compat-Bibliothek abstrahiert einen Großteil der Komplexität der Desktop-Benachrichtigungen. Die folgenden Anweisungen erfordern die Compat-Bibliothek.

Wenn Sie vorkompilierte Header verwenden, stellen Sie sicher, dass `#include "stdafx.h"` als erste Zeile in der Datei DesktopNotificationManagerCompat.cpp erscheint.


## <a name="step-3-include-the-header-files-and-namespaces"></a>Schritt 3: Enthalten Sie die Headerdateien und namespaces

Fügen Sie die Headerdatei der Compat-Bibliothek und die Headerdatei und den Namespaces hinzu, die im Zusammenhang mit UWP-Popup-APIs verwendet werden.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>Schritt 4: Implementieren Sie die Aktivierung

Sie müssen einen Handler für den Toast-Aktivierung implementieren, sodass klickt der Benutzer auf Ihre Toast, Ihre app etwas durchführen kann. Dies ist erforderlich für das Popup, damit es im Info-Center beibehalten wird (da auf das Popup Tage später geklickt werden kann, wenn die App geschlossen ist). Diese Klasse kann an eine beliebige Stelle in Ihrem Projekt platziert werden.

Implementieren Sie die **INotificationActivationCallback**-Schnittstelle wie folgt, einschließlich der UUID, und rufen Sie auch **CoCreatableClass** auf, um die Klasse als COM-erstellbar zu kennzeichnen. Erstellen Sie mithilfe einer der vielen online GUID-Generatoren eine eindeutige GUID für die UUID. Durch diese GUID CLSID (Klassen-ID) weiß das Info-Center, welche Klasse für COM aktiviert werden soll.

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
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

**Package.appxmanifest**

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

```cpp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

Wenn Desktop-Brücke und klassisches Win32 unterstützt werden, können Sie diese Methode unabhängig davon aufgerufen. Wenn Sie die Methode mit der Desktop-Brücke ausführen, wird sie sofort zurückgegeben. Sie müssen den Code nicht verzweigen.

Mit dieser Methode können Sie Compat-APIs zum Senden und Verwalten von Benachrichtigungen aufrufen, ohne ständig die AUMID angeben zu müssen. Es fügt ebenfalls den LocalServer32-Schlüssel für den COM-Server hinzu.


## <a name="step-6-register-com-activator"></a>Schritt 6: Registrieren von COM-Aktivierung

Für Desktop-Brücken und klassische Win32-Apps müssen Sie Ihren Benachrichtigung-Aktivatortyp registrieren, damit Sie Popupaktivierungen behandeln können.

Rufen Sie im Startcode Ihrer App, die folgende **RegisterActivator**-Methode auf. Dies muss in der Reihenfolge aufgerufen werden, in der Sie alle Popupaktivierungen erhalten.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>Schritt 7: Senden einer Benachrichtigung

Das Senden einer Benachrichtigung ist für UWP-Apps identisch, außer dass Sie **DesktopNotificationManagerCompat** zum Erstellen eines **ToastNotifier** verwenden. Die Compat-Bibliothek behandelt den Unterschied zwischen Desktop-Brücke und klassischer Win32 automatisch, damit Sie keinen Code verzweigen müssen. Für klassisches Win32 speichert die Compat-Bibliothek Ihre AUMID, die Sie beim Aufrufen von **RegisterAumidAndComServer** bereitgestellt haben, damit Sie sich nicht die AUMID kümmern müssen, wann Sie diese bereitstellen und wann nicht.

Stellen Sie sicher, dass Sie den unten angezeigten **ToastGeneric** verwenden, da ältere Vorlagen der Windows 8.1 Popupbenachrichtigungen nicht den COM-Benachrichtigungs-Aktivator aktivieren, den Sie in Schritt 4 # erstellt haben.

> [!IMPORTANT]
> HTTP-Bilder werden nur in Desktop-Brücken-Apps unterstützt, die die Internetfunktion im Manifest haben. Klassische Win32-Apps unterstützen keine HTTP-Bilder. Sie müssen das Bild in die lokalen App-Daten herunterladen und lokal darauf verweisen.

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc.Get(), &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> Klassische Win32-Apps können keine älteren Popupvorlagen (z. B. ToastText02) verwenden. Die Aktivierung älterer Vorlagen schlägt fehl, wenn die COM-CLSID angegeben wird. Wie oben erwähnt, müssen Sie die Windows 10 ToastGeneric-Vorlagen verwenden.


## <a name="step-8-handling-activation"></a>Schritt 8: Behandeln der Aktivierung

Wenn der Benutzer auf das Popup oder auf Schaltflächen im Popup klickt, wird die **Activate**-Methode Ihre **NotificationActivator**-Klasse aufgerufen.

Innerhalb der Activate-Methode können Sie die Argumente analysieren, die Sie im Popup angegeben haben und die Benutzereingabe abrufen, die der Benutzer eingegeben oder ausgewählt hat und dann entsprechend Ihre App aktivieren.

> [!NOTE]
> Die **Activate**-Methode wird in einem separaten Thread aus dem Hauptthread aufgerufen.

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

Si sollten in der WinMain-Funktion festlegen, ob Sie über ein Popup oder nicht gestartet werden soll, um einen ordnungsgemäßen Start zu unterstützen, während die App geschlossen ist. Wenn dies von einem Popup gestartet wird, wird ein Start-Argument von "-ToastActivated" gesendet. Sobald dies angezeigt wird, sollten Sie das Ausführen aller normaler Aktivierungscodes beenden, und den Start von **NotificationActivator**-Fenstern zu ermöglichen.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>Aktivierungssequenz von Ereignissen

Die Aktivierungssequenz wie folgt...

Wenn Ihre App bereits ausgeführt wird:

1. **Activate** in Ihrer **NotificationActivator** wird aufgerufen

Wenn Ihre App nicht ausgeführt wird:

1. Ihre App wird über eine exe-Datei gestartet, und Sie erhalten ein Befehlszeilenargument von "-ToastActivated"
2. **Activate** in Ihrer **NotificationActivator** wird aufgerufen


### <a name="foreground-vs-background-activation"></a>Vordergrund- und Hintergrundaktivierung
Für Desktop-Apps erfolgt die Vordergrund und Hintergrundaktivierung genauso – die COM-Aktivierung wird aufgerufen. Je nach dem Code Ihrer App wird entschieden, ob ein Fenster angezeigt wird oder einfach einige Aufgaben ausgeführt werden und die Aktion anschließend beendet wird. Aus diesem Grund ändert die Angabe eines **activationType** im **Hintergrund** Ihres Popup-Inhalts nichts am Verhalten.


## <a name="step-9-remove-and-manage-notifications"></a>Schritt 9: Entfernen und Verwalten von Benachrichtigungen

Das Entfernen und Verwalten von Benachrichtigungen ist identisch mit UWP-Apps. Es wird jedoch empfohlen, dass Sie unsere Compat-Bibliothek verwenden, um eine **DesktopNotificationHistoryCompat** zu erhalten, damit Sie sich keine Gedanken über das Bereitstellen der AUMID machen müssen, wenn Sie die klassische Win32 verwenden.

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>Schritt 10: Bereitstellen und Debuggen

Wenn Sie die Desktop-Brücke-App bereitstellen und debuggen möchten, lesen Sie [Ausführen, Debuggen und Testen eine verpackten Desktop-App](/windows/uwp/porting/desktop-to-uwp-debug).

Um eine klassische Win32-App bereitzustellen und zu verwalten, müssen Sie Ihrer App durch den Installer einmal vor dem Debuggen installieren, damit die Verknüpfung auf dem Startmenü mit AUMID und CLSID vorhanden ist. Nachdem die Verknüpfung auf dem Startmenü vorhanden ist, können Sie mithilfe von F5 in Visual Studio debuggen.

Wenn Ihre Benachrichtigungen nicht einfach in Ihrer klassische Win32-App angezeigt werden (und keine Ausnahmen ausgelöst werden), bedeutet dies wahrscheinlich, dass die Verknüpfung im Startmenü nicht vorhanden ist (installieren Sie Ihre App über das Installationsprogramm), oder die AUMID, die Sie im Code verwendet haben, nicht der AUMID der Verknüpfung auf dem Startmenü entspricht.

Wenn Ihre Benachrichtigungen im Info-Center erscheinen aber nicht dort angezeigt werden (verschwinden, nachdem das Popup geschlossen ist) beibehalten werden nicht persistent angezeigt werden, bedeutet dies, dass Sie die den COM-Aktivator nicht ordnungsgemäß implementiert haben.

Wenn Sie sowohl Ihre Desktop-Brücke und die klassische Win32-App installiert haben, beachten Sie, dass die App Desktop-Brücke die klassische Win32-App bei der Behandlung von Popup-Aktivierungen ablöst. Dies bedeutet, dass Popups aus der klassische Win32-App beim Klicken auf die Desktop-Brücke-App gestartet werden. Das Deinstallieren der App Desktop-Brücke setzt die Aktivierung auf die klassische Win32-App zurück.

Wenn Sie `HRESULT 0x800401f0 CoInitialize has not been called.` erhalten, müssen Sie `CoInitialize(nullptr)` in Ihrer App vor dem Aufrufen der APIs aufrufen.

Wenn Sie `HRESULT 0x8000000e A method was called at an unexpected time.` beim Aufrufen der Compat APIs erhalten, bedeutet dies wahrscheinlich, dass Sie die erforderliche Register-Methoden nicht aufgerufen haben (oder bei einer Desktop-Brücke-App, dass Sie derzeit die App nicht unter im Desktop-Brücken-Kontext ausführen).

Wenn Sie zahlreiche `unresolved external symbol` Kompilierungsfehler erhalten, haben Sie wahrscheinlich vergessen, `runtimeobject.lib` auf die **zusätzliche Abhängigkeiten** in Schritt 1 hinzuzufügen (oder Sie haben sie nur der Debugkonfiguration und nicht der Releasekonfiguration hinzugefügt).


## <a name="handling-older-versions-of-windows"></a>Behandeln von älteren Versionen von Windows

Wenn Sie Windows 8.1 oder niedriger unterstützen, sollten Sie zur Laufzeit überprüfen, ob Sie Windows 10 ausführen, bevor Sie eine **DesktopNotificationManagerCompat**-API aufrufen oder ToastGeneric-Popupbenachrichtigungen senden.

Mit Windows 8 wurden Popupbenachrichtigungen eingeführt, allerdings verwendet es die [älteren Popupvorlagen](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh761494(v=win.10)) wie z. B. ToastText01. Die Aktivierung wurde vom **Activated**-Ereignis im Speicher von der **ToastNotification**-Klasse behandelt, da Popups nur als kurze Popups angezeigt wurden, die nicht beibehalten wurden. Windows 10 führte [interaktive ToastGeneric-Popups](adaptive-interactive-toasts.md) ein sowie das Info-Center, in dem Benachrichtigungen für mehrere Tage beibehalten werden. Die Einführung des Info-Centers erforderte die Einführung eines COM-Aktivators, damit Ihr Popups auch noch einige Tage nach dem Erstellen aktiviert werden können.

| Betriebssystem | ToastGeneric | COM-Aktivator | Ältere Popupvorlagen |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | Unterstützt | Unterstützt | Unterstützt (aktiviert den COM-Server nicht) |
| Windows 8.1 / 8 | Nicht zutreffend | Nicht zutreffend | Unterstützt |
| Windows 7 und niedriger | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend |

Um zu überprüfen, ob Sie Windows 10 ausführen, geben Sie den `<VersionHelpers.h>` Header an und überprüfen Sie die **IsWindows10OrGreater**-Methode. Wenn „true” zurückgegeben wird, rufen Sie weiterhin alle in dieser Dokumentation beschriebenen Methoden auf. 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>Bekannte Probleme

**FEST: App nicht nach dem Klicken auf die Toast Fokus**: In Builds 15063 und früher wurden nicht Foreground-Rechte für Ihre Anwendung übertragen werden, wenn wir den COM-Server aktiviert. Aus diesem Grund führte Ihre App einfach einen Flash aus, bei dem Versuch, sie in den Vordergrund zu verschieben. Es gibt hierfür keine Problemumgehung. Wir haben dies in Builds 16299 und höher behoben.


## <a name="resources"></a>Ressourcen

* [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Toastbenachrichtigungen aus desktop-apps](toast-desktop-apps.md)
* [Toast-Content-Dokumentation](adaptive-interactive-toasts.md)
