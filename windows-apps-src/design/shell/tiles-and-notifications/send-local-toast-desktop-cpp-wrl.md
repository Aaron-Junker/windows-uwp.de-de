---
Description: Erfahren Sie, wie Win32-C++ WRL-apps lokale Popup Benachrichtigungen senden können und den Benutzer durch Klicken auf den Toast behandeln können.
title: Senden einer lokalen Popup Benachrichtigung von Win32 C++ WRL-apps
label: Send a local toast notification from Win32 C++ WRL apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Win32, Desktop, Popup Benachrichtigungen, Toast senden, lokalen Toast senden, Desktop Bridge, msix, Sparse-Paket, C++, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: e1e8aedd867dfdcabd382ebde1dd4c96a94d1001
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100318"
---
# <a name="send-a-local-toast-notification-from-win32-c-wrl-apps"></a>Senden einer lokalen Popup Benachrichtigung von Win32 C++ WRL-apps

Win32-Apps (einschließlich gepackter [msix](/windows/msix/desktop/source-code-overview) -apps, apps, die [Pakete](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) mit geringer Dichte zum Abrufen der Paket Identität verwenden, und klassische, nicht gepackte Win32-Apps) können interaktive Popup Benachrichtigungen wie Windows-apps senden. Allerdings gibt es einige spezielle Schritte für Win32-apps aufgrund der verschiedenen Aktivierungs Schemas und des potenziellen Mangels an Paket Identität, wenn Sie nicht msix oder ein sparsepaket verwenden.

> [!IMPORTANT]
> Wenn Sie eine UWP-app schreiben, finden Sie weitere Informationen in der [UWP-Dokumentation](send-local-toast.md). Weitere Desktop Sprachen finden Sie unter [Desktop c#](send-local-toast-desktop.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Schritt 1: Aktivieren des Windows 10 SDK

Wenn Sie das Windows 10 SDK nicht für ihre Win32-App aktiviert haben, müssen Sie dies zunächst tun. Es gibt einige wichtige Schritte...

1. `runtimeobject.lib`Zu **weiteren Abhängigkeiten** hinzufügen
2. Windows 10 SDK als Ziel

Klicken Sie mit der rechten Maustaste, und wählen Sie **Eigenschaften**

Wählen Sie im **Configuration** Menü der obersten Konfiguration **alle Konfigurationen** aus, sodass die folgende Änderung auf Debug und Release angewendet wird.

Fügen Sie unter **Linker-> Eingabe** `runtimeobject.lib` den **zusätzlichen Abhängigkeiten**hinzu.

Stellen Sie dann unter **Allgemein**sicher, dass die **Windows SDK-Version** auf 10,0 oder höher festgelegt ist (nicht auf Windows 8.1).


## <a name="step-2-copy-compat-library-code"></a>Schritt 2: Kopieren des Kompatibilitäts-Bibliothekscodes

Kopieren Sie die Datei [desktopnotificationmanagercompat. h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) und [desktopnotificationmanagercompat. cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) aus GitHub in Ihr Projekt. Die Kompatibilitäts-Bibliothek abstrahiert einen Großteil der Komplexität von Desktop Benachrichtigungen. Die folgenden Anweisungen erfordern die Kompatibilitäts-Bibliothek.

Wenn Sie vorkompilierte Header verwenden, stellen Sie sicher, dass `#include "stdafx.h"` die erste Zeile der Datei desktopnotificationmanagercompat. cpp ist.


## <a name="step-3-include-the-header-files-and-namespaces"></a>Schritt 3: einschließen der Header Dateien und-Namespaces

Schließen Sie die Header Datei der Kompatibilitäts-Bibliothek und die Header Dateien und-Namespaces für die Verwendung der Windows-Popup-APIs ein.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>Schritt 4: Implementieren Sie den Aktivator.

Sie müssen einen Handler für die Toast Aktivierung implementieren, damit Ihre APP etwas Unternehmen kann, wenn der Benutzer auf den Popup klickt. Dies ist erforderlich, damit der Popup im Aktions Center persistent gespeichert wird (da der Popup-Vorgang Tage später angezeigt werden kann, wenn die app geschlossen wird). Diese Klasse kann an beliebiger Stelle in Ihrem Projekt platziert werden.

Implementieren Sie wie unten gezeigt die **inotificationactivationcallback** -Schnittstelle (einschließlich einer UUID), und rufen Sie auch **cokreatableclass** auf, um die Klasse als com-erstellbar zu markieren. Erstellen Sie für Ihre UUID eine eindeutige GUID mit einem der vielen Online-GUID-Generatoren. Diese GUID CLSID (Klassen Bezeichner) gibt an, wie das Aktions Center weiß, welche Klasse für com aktiviert werden soll.

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


## <a name="step-5-register-with-notification-platform"></a>Schritt 5: Registrieren bei der Benachrichtigungs Plattform

Anschließend müssen Sie sich bei der Benachrichtigungs Plattform registrieren. Je nachdem, ob Sie msix/Sparse-Pakete oder klassisches Win32 verwenden, gibt es unterschiedliche Schritte. Wenn Sie beide unterstützen, müssen Sie beide Schritte ausführen (Sie müssen jedoch nicht Ihren Code verzweigen, da die Bibliothek dies für Sie erledigt!).


### <a name="msixsparse-package"></a>Msix/sparsespaket

Fügen Sie in der [MSIX](/windows/msix/desktop/source-code-overview) Datei "Package. [sparse package](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) appxmanifest" in der Datei " **Package. appxmanifest**" Folgendes hinzu:

1. Deklaration für **xmlns: com**
2. Deklaration für **xmlns: Desktop**
3. Im **ignorablenamespaces** -Attribut, **com** und **Desktop**
4. **com: Erweiterung** für den com-Activator, der die GUID aus Schritt #4 verwendet. Achten Sie darauf, `Arguments="-ToastActivated"` dass Sie das einschließen, damit Sie wissen, dass der Start von einem Toast stammt.
5. **Desktop: Erweiterung** für **Windows. toastnotificationactivation** zum Deklarieren der Popup-Activator-CLSID (die GUID aus Schritt #4).

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


### <a name="classic-win32"></a>Klassisches Win32

Wenn Sie klassisches Win32 verwenden (oder wenn Sie beides unterstützen), müssen Sie die Anwendungs Benutzer Modell-ID (aumid) und die Popup-Activator-CLSID (die GUID aus Schritt #4) in der Verknüpfung ihrer App im Startmenü deklarieren.

Wählen Sie eine eindeutige aumid aus, mit der ihre Win32-App identifiziert wird. Dies erfolgt in der Regel in der Form [CompanyName]. [AppName], aber Sie möchten sicherstellen, dass dies in allen apps eindeutig ist (Sie können am Ende einige Ziffern hinzufügen).

#### <a name="step-51-wix-installer"></a>Schritt 5,1: WiX-Installer

Wenn Sie WiX für das Installationsprogramm verwenden, bearbeiten Sie die Datei " **Product. wxs** ", um die beiden Verknüpfungs Eigenschaften zur Start Menü Verknüpfung hinzuzufügen, wie unten gezeigt. Stellen Sie sicher, dass die GUID aus Schritt #4 in eingeschlossen ist, `{}` wie unten gezeigt.

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


#### <a name="step-52-register-aumid-and-com-server"></a>Schritt 5,2: Registrieren von aumid und com-Server

Rufen Sie dann unabhängig vom Installationsprogramm im Startcode Ihrer APP (vor dem Aufrufen von Benachrichtigungs-APIs) die **registeraumidandcomserver** -Methode auf, und geben Sie dann die Benachrichtigungs-Activator-Klasse aus Schritt #4 und ihrer zuvor verwendeten aumid an.

```cpp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

Wenn Sie sowohl das msix/Sparse-Paket als auch das klassische Win32-Paket unterstützen, können Sie diese Methode unabhängig davon aufzurufen. Wenn Sie unter einem msix-oder Sparse-Paket ausführen, wird diese Methode einfach sofort zurückgegeben. Es ist nicht erforderlich, den Code zu verzweigen.

Diese Methode ermöglicht es Ihnen, die Kompatibilitäts-APIs aufzurufen, um Benachrichtigungen zu senden und zu verwalten, ohne die aumid ständig bereitstellen zu müssen. Außerdem wird der Registrierungsschlüssel LocalServer32 für den com-Server eingefügt.


## <a name="step-6-register-com-activator"></a>Schritt 6: com-Aktivierung registrieren

Sowohl für das msix/Sparse-Paket als auch für klassische Win32-apps müssen Sie den Typ des Benachrichtigungs aktivierers registrieren, damit Sie Popup Aktivierungen verarbeiten können.

Aufrufen Sie im Startcode Ihrer APP die folgende **registeractivator** -Methode. Dies muss aufgerufen werden, damit Sie beliebige Popup Aktivierungen empfangen können.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>Schritt 7: Senden einer Benachrichtigung

Das Senden einer Benachrichtigung ist mit UWP-apps identisch, mit dem Unterschied, dass Sie " **desktopnotificationmanagercompat** " verwenden, um einen " **deastnotifier**" zu erstellen. Die Kompatibilitäts-Bibliothek behandelt automatisch den Unterschied zwischen dem msix/Sparse-Paket und dem klassischen Win32, sodass Sie den Code nicht verzweigen müssen. Bei klassischem Win32 speichert die Kompatibilitäts-Bibliothek die von Ihnen beim Aufrufen von **registeraumidandcomserver** bereitgestellte aumid zwischen, sodass Sie sich keine Gedanken machen müssen, wann die aumid bereitgestellt oder nicht.

Stellen Sie sicher, dass Sie die **toastgeneric** -Bindung wie unten gezeigt verwenden, da die Legacy-Windows 8.1 Popup Benachrichtigungs Vorlagen Ihren in Schritt #4 erstellten com-Benachrichtigungs Aktivator nicht aktivieren.

> [!IMPORTANT]
> Http-Images werden nur in msix/Sparse-Paket-Apps unterstützt, die über die Internetfunktion in ihrem Manifest verfügen. Klassische Win32-apps unterstützen keine http-Bilder. Sie müssen das Image in Ihre lokalen app-Daten herunterladen und lokal darauf verweisen.

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
> Klassische Win32-Apps können keine Legacy-Popup Vorlagen verwenden (z. b. ToastText02). Die Aktivierung der Legacy Vorlagen schlägt fehl, wenn die com-CLSID angegeben wird. Sie müssen die oben gezeigten Windows 10-Standardvorlagen verwenden.


## <a name="step-8-handling-activation"></a>Schritt 8: Behandeln der Aktivierung

Wenn der Benutzer auf den Popup klickt, oder auf die Schaltflächen im Toast, wird die **Aktivierungs** Methode der **notificationactivator** -Klasse aufgerufen.

Innerhalb der Aktivierungsmethode können Sie die args analysieren, die Sie im Toast angegeben haben, und die Benutzereingabe abrufen, die der Benutzer eingegeben oder ausgewählt hat, und die APP dann entsprechend aktivieren.

> [!NOTE]
> Die **Aktivierungs** Methode wird in einem Trennzeichen von Ihrem Hauptthread aufgerufen.

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

Wenn Sie das Starten der APP ordnungsgemäß unterstützen möchten, während Ihre APP geschlossen ist, sollten Sie in der WinMain-Funktion bestimmen, ob Sie von einem Toast gestartet werden oder nicht. Wenn Sie von einem Toast aus gestartet wird, wird eine Start-arg von "-toastaktiviert" angezeigt. Wenn dies der Fall ist, sollten Sie den normalen Start Aktivierungscode nicht mehr ausführen und zulassen, dass der **notificationactivator** das Starten von Fenstern bei Bedarf behandelt.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
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


### <a name="activation-sequence-of-events"></a>Aktivierungs Sequenz von Ereignissen

Die Aktivierungs Sequenz lautet wie folgt...

Wenn Ihre APP bereits ausgeführt wird:

1. " **Aktivieren** " in " **notificationactivator** " wird aufgerufen.

Wenn Ihre APP nicht ausgeführt wird:

1. Ihre APP wird mit exe gestartet. Sie erhalten die Befehlszeilenargumente "-|" aktiviert ".
2. " **Aktivieren** " in " **notificationactivator** " wird aufgerufen.


### <a name="foreground-vs-background-activation"></a>Vordergrund-vs-Hintergrund Aktivierung
Bei Win32-apps wird die Vordergrund-und Hintergrund Aktivierung identisch behandelt. der com-Activator wird aufgerufen. Es liegt an Ihrem app-Code, zu entscheiden, ob ein Fenster angezeigt werden soll, oder einfach nur einige Aufgaben auszuführen und zu beenden. Wenn Sie also einen **ActivationType** von **Background** in Ihrem Popup Inhalt angeben, ändert sich das Verhalten nicht.


## <a name="step-9-remove-and-manage-notifications"></a>Schritt 9: entfernen und Verwalten von Benachrichtigungen

Das Entfernen und Verwalten von Benachrichtigungen ist identisch mit UWP-apps. Es wird jedoch empfohlen, dass Sie unsere Kompatibilitäts-Bibliothek verwenden, um eine **desktopnotificationhistorycompat** -Klasse zu erhalten, sodass Sie sich keine Gedanken über die Bereitstellung der aumid machen müssen, wenn Sie klassisches Win32 verwenden.

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

Informationen zum Bereitstellen und Debuggen Ihrer msix/Sparse-Paket-app finden Sie unter [ausführen, Debuggen und Testen einer gepackten Desktop-App](/windows/msix/desktop/desktop-to-uwp-debug)

Wenn Sie Ihre klassische Win32-App bereitstellen und debuggen möchten, müssen Sie die APP einmal vor dem Debuggen über das Installationsprogramm installieren, damit die Start Verknüpfung mit ihrer aumid und CLSID vorhanden ist. Nachdem die Start Verknüpfung vorhanden ist, können Sie in Visual Studio mit F5 Debuggen.

Wenn Ihre Benachrichtigungen einfach nicht in ihrer klassischen Win32-App angezeigt werden (und keine Ausnahmen ausgelöst werden), bedeutet dies wahrscheinlich, dass die Start Verknüpfung nicht vorhanden ist (installieren Sie Ihre APP über das Installationsprogramm), oder die im Code verwendete aumid stimmt nicht mit der aumid in der Start Verknüpfung.

Wenn Ihre Benachrichtigungen angezeigt werden, aber nicht im Aktions Center gespeichert sind (verschwindet, nachdem das Popup verworfen wurde), bedeutet dies, dass Sie den com-Activator nicht ordnungsgemäß implementiert haben.

Wenn Sie sowohl das msix/Sparse-Paket als auch die klassische Win32-App installiert haben, beachten Sie, dass die msix/Sparse-Paket-app die klassische Win32-App ersetzt, wenn Sie Popup Aktivierungen verarbeiten. Dies bedeutet, dass bei einem Klick von der klassischen Win32-App aus der klassischen Win32-App weiterhin die msix/Sparse-Paket-app gestartet wird. Wenn Sie die msix/Sparse-Paket-app deinstallieren, werden die Aktivierungen wieder in der klassischen Win32-APP wieder hergestellt.

Wenn Sie empfangen `HRESULT 0x800401f0 CoInitialize has not been called.` , achten Sie darauf, dass Sie `CoInitialize(nullptr)` in Ihrer APP aufrufen, bevor Sie die APIs aufrufen.

Wenn Sie `HRESULT 0x8000000e A method was called at an unexpected time.` beim Aufrufen der compat-APIs empfangen, bedeutet dies wahrscheinlich, dass Sie die erforderlichen Register Methoden nicht aufrufen konnten (oder wenn Sie Ihre APP derzeit nicht im msix/Sparse-Kontext ausführen).

Wenn Sie zahlreiche `unresolved external symbol` Kompilierungsfehler erhalten, haben Sie wahrscheinlich vergessen, `runtimeobject.lib` den **zusätzlichen Abhängigkeiten** in Schritt #1 hinzuzufügen (oder Sie haben Sie nur der Debug-Konfiguration und nicht der Releasekonfiguration hinzugefügt).


## <a name="handling-older-versions-of-windows"></a>Umgang mit älteren Versionen von Windows

Wenn Sie Windows 8.1 oder niedriger unterstützen, sollten Sie zur Laufzeit überprüfen, ob Sie unter Windows 10 ausgeführt werden, bevor Sie eine **desktopnotificationmanagercompat** -API aufrufen oder toastgenerische Toasts senden.

In Windows 8 wurden Popup Benachrichtigungen eingeführt, aber es wurden die Legacy-Popup [Vorlagen](/previous-versions/windows/apps/hh761494(v=win.10))wie ToastText01 verwendet. Die Aktivierung wurde vom in-Memory- **aktivierten** Ereignis in der **toastnotification** -Klasse behandelt, da Popups nur kurze Popups waren, die nicht persistent waren. In Windows 10 wurden interaktive Popup- [Auffassungen](adaptive-interactive-toasts.md)eingeführt, und es wurde auch ein Aktions Center eingeführt, in dem Benachrichtigungen mehrere Tage lang aufbewahrt werden. Die Einführung des Aktions Centers erforderte die Einführung eines com-Activators, sodass der Toast Tage nach der Erstellung aktiviert werden kann.

| OS | Mit dem generischen | COM-Activator | Legacy-Popup Vorlagen |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | Unterstützt | Unterstützt | Unterstützt (com-Server wird jedoch nicht aktiviert) |
| Windows 8.1/8 | – | – | Unterstützt |
| Windows 7 und niedriger | – | – | – |

Um zu prüfen, ob Sie unter Windows 10 ausgeführt werden, schließen Sie den `<VersionHelpers.h>` -Header ein, und überprüfen Sie die **IsWindows10OrGreater** -Methode Wenn dies true zurückgibt, rufen Sie weiterhin alle Methoden auf, die in dieser Dokumentation beschrieben werden. 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>Bekannte Probleme

**Korrigiert: die APP wird nach dem Klicken auf "Toast" nicht fokussiert**: in Builds 15063 und früher wurden keine Vordergrund Rechte an Ihre Anwendung übertragen, als der com-Server aktiviert wurde. Daher würde Ihre APP einfach blinken, wenn Sie versucht haben, Sie in den Vordergrund zu verschieben. Für dieses Problem gibt es keine Problem Umgehung. Wir haben dies in Build 16299 und höher korrigiert.


## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Popup Benachrichtigungen von Win32-apps](toast-desktop-apps.md)
* [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
