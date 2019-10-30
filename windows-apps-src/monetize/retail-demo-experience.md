---
title: Hinzufügen von Features für die Einzelhandels Demo (RDX) zu Ihrer APP
description: Bereiten Sie Ihre APP für den Einzelhandel-Demomodus vor, und helfen Sie Ihnen dabei, Ihre APP im Einzelhandels Verkaufs Boden vorzustellen.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP, Demo-App für den Einzelhandel
ms.localizationpriority: medium
ms.openlocfilehash: 5be39760ee2b8837cfb9b0809a354262e790970b
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73052000"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Hinzufügen von Features für die Einzelhandels Demo (RDX) zu Ihrer APP

Fügen Sie einen Demomodus für den Einzelhandel in Ihre Windows-App ein, damit Kunden, die PCs und Geräte im Geschäft ausprobieren, direkt loslegen können.

Wenn sich Kunden in einem Einzelhandelsgeschäft befinden, erwarten Sie, dass Sie Demos von PCs und Geräten ausprobieren können. Sie verbringen häufig einen beträchtlichen Teil der Zeit, die Sie mit apps über die Analyse Umgebung für den Einzelhandel und die Verwendung von [RDX (Retail Demo)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)spielen

Sie können Ihre APP so einrichten, dass im _normalen_ oder _Einzelhandels_ Modus unterschiedliche Umgebungen bereitgestellt werden. Wenn Ihre APP z. b. mit einem Setup Vorgang gestartet wird, können Sie Sie im Einzelhandels Modus überspringen und die APP vorab mit Beispiel Daten und Standardeinstellungen auffüllen, damit Sie direkt in den Gang springen können.

Aus Sicht unserer Kunden gibt es nur eine APP. Um Kunden bei der Unterscheidung zwischen den beiden Modi zu unterstützen, wird empfohlen, dass Sie das Wort "Retail" in der Titelleiste oder an einem geeigneten Ort anzeigt, während sich Ihre APP im Einzelhandels Modus befindet.

Zusätzlich zu den Microsoft Store Anforderungen für apps müssen RDX-fähige apps auch mit den RDX-Setup-, Bereinigungs-und Aktualisierungs Prozessen kompatibel sein, um sicherzustellen, dass Kunden im Einzelhandelsgeschäft eine konsistente Leistung aufweisen.

## <a name="design-principles"></a>Designprinzipien

* **Zeigen Sie Ihren besten**an. Verwenden Sie die Demo für den Einzelhandel, um zu veranschaulichen, warum Ihre APP in die Dies ist wahrscheinlich das erste Mal, wenn Ihr Kunde Ihre APP zum ersten Mal sehen wird, zeigen Sie Ihnen das beste Element!

* **Zeigen Sie Sie schnell**an. Kunden können ungeduldig sein – je schneller ein Benutzer den Wert ihrer App erfasst, desto besser.

* **Halten Sie die Story einfach**. Die Demo für den Einzelhandel ist eine Lift-Tonhöhe für den Wert Ihrer APP.

* **Konzentrieren Sie sich auf die**Leistung. Geben Sie den Benutzern die Zeit, um Ihre Inhalte zu verdauen. Es ist zwar wichtig, dass die Benutzer schnell zum wichtigsten Teil gelangen, aber nur mithilfe entsprechender Pausen können sie den Wert der App richtig erkennen.

## <a name="technical-requirements"></a>Technische Anforderungen

Da RDX-fähige apps das beste Ihrer APP für Kunden im Einzelhandel darstellen sollen, müssen Sie technische Anforderungen erfüllen und den Datenschutzbestimmungen entsprechen, die die Microsoft Store für alle Einzelhandel-Demo-apps hat.

Dies kann als Checkliste verwendet werden, um Sie bei der Vorbereitung des Überprüfungs Vorgangs zu unterstützen und die Klarheit im Testprozess zu gewährleisten. Beachten Sie, dass diese Anforderungen nicht nur für den Prüfprozess, sondern für die gesamte Lebensdauer der Demo-App für den Einzelhandel (d. h. solange Ihre App auf den Vorführgeräten ausgeführt wird) eingehalten werden müssen.

### <a name="critical-requirements"></a>Kritische Anforderungen

RDX-fähige apps, die diese kritischen Anforderungen nicht erfüllen, werden so bald wie möglich von allen Einzelhandels Demogeräten entfernt.

* **Fragen Sie keine personenbezogenen Informationen (PII)** . Dies schließt Anmelde Informationen, Microsoft-Konto Informationen oder Kontaktinformationen ein.

* **Fehlerfreie**Darstellung. Ihr App muss fehlerfrei funktionieren. Außerdem dürfen keine Fehler-Pop-ups oder -Benachrichtigungen angezeigt werden, wenn Kunden die Vorführgeräte verwenden. Die Fehler spiegeln sich negativ auf die APP selbst, Ihre Marke, die Marke des Geräts, die Marke des Herstellers und die Marke von Microsoft wider.

* **Kostenpflichtige Apps müssen einen Testmodus aufweisen**. Ihre APP muss entweder kostenlos sein oder einen [Testmodus](https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)enthalten. Kunden, die sich in einem Laden etwas ansehen, möchten dafür nicht zahlen.

### <a name="high-priority-requirements"></a>Anforderungen mit hoher Priorität

RDX-fähige apps, die diese Anforderungen mit hoher Priorität nicht erfüllen, müssen sofort auf eine Korrektur hin untersucht werden. Wenn eine umgehende Problembehebung nicht möglich ist, kann diese App von allen Vorführgeräten entfernt werden.

* **Unvergessliches Offline-Erlebnis**. Ihre APP muss eine tolle Offline-Benutzerversion veranschaulichen, da ungefähr 50% der Geräte an Einzelhandelsstandorten offline sind. Dadurch wird sichergestellt, dass das Erlebnis für die Kunden auch dann positiv ist, wenn sie offline mit Ihrer App interagieren.

* **Aktualisierte Inhalts**Darstellung. Ihre APP sollte nicht bei der Online Aktualisierung aufgefordert werden. Wenn Updates erforderlich sind, sollten Sie im Hintergrund ausgeführt werden.

* **Keine anonyme Kommunikation**. Da ein Kunde, der ein Einzelhandels Demogerät verwendet, ein anonymer Benutzer ist, sollte er nicht in der Lage sein, Inhalte vom Gerät zu richten oder freizugeben.

* **Gewährleisten Sie mithilfe des Bereinigungs Prozesses konsistente**Oberflächen. Die Verwendung eines Vorführgeräts sollte für alle Kunden gleich sein. Ihre APP sollte den [Bereinigungs Prozess](#cleanup-process) verwenden, um nach jeder Verwendung denselben Standardstatus zurückzugeben. Der nächste Kunde soll nicht sehen, was der letzte Kunde zurückgelassen hat. Dies umfasst z. B. Punktestände, Erfolge und aufgehobene Sperren.

* **Alter geeigneter Inhalt**. Allen APP-Inhalten muss die Bewertungskategorie "Teen" oder "Lower" zugewiesen werden. Weitere Informationen finden Sie unter [erhalten der Bewertung ihrer app durch IARC](https://www.globalratings.com/for-developers.aspx) und [ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Anforderungen mit mittlerer Priorität

Das Windows-Team für den Einzelhandel setzt sich unter Umständen direkt mit Entwicklern in Verbindung, um mit ihnen zu besprechen, wie diese Probleme behoben werden können.

* **Die Möglichkeit, für eine Reihe von Geräten erfolgreich auszuführen**. Apps müssen auf allen Geräten gut ausgeführt werden, einschließlich Geräten mit Low-End-Spezifikationen. Wenn die APP auf Geräten installiert ist, die die Mindestanforderungen nicht erfüllen, muss die APP den Benutzer klar über diese Informationen informieren. Die Mindestgeräteanforderungen müssen bekanntgegeben werden, damit die App immer mit höchster Leistung ausgeführt werden kann.

* **Erfüllen der Größenanforderungen der Einzelhandelsgeschäft-App**. Die App darf eine Größe von 800 MB nicht übersteigen. Wenden Sie sich direkt an das Windows Retail Store-Team, um weitere Informationen zu erhalten, wenn Ihre RDX-fähige APP nicht die Größenanforderungen erfüllt.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>Retailinfo-API: Vorbereiten des Codes für den Demomodus

### <a name="isdemomodeenabled"></a>ISDE momodeaktiviert
Die [**isdemomodeaktivierte**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) Eigenschaft in der Klasse " [**retailinfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) Utility", die Teil des [Windows. System. profile](https://docs.microsoft.com/uwp/api/windows.system.profile) -Namespace im Windows 10 SDK ist, wird als boolescher Indikator verwendet, um anzugeben, auf welchem Codepfad Ihre APP ausgeführt wird.der Modus oder der _Einzelhandels_ Modus.

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>"Retailinfo. Properties"

Wenn [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) „true“ zurückgibt, können Sie mithilfe von [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) einen Satz von Eigenschaften zum Gerät abfragen, um eine besser angepasste Verwendung der Demo-App zu ermöglichen. Diese Eigenschaften umfassen [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) usw.

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```cpp
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>Bereinigungs Prozess

Der Cleanup beginnt zwei Minuten, nachdem ein Einkäufer die Interaktion mit dem Gerät beendet hat. Die Einzelhandels Demo wird abgespielt, und Windows beginnt mit dem Zurücksetzen von Beispiel Daten in den Kontakten, Fotos und anderen apps. Abhängig vom Gerät kann es zwischen 1-5 Minuten dauern, bis alles wieder in den Normalzustand zurückgesetzt wird. Dadurch wird sichergestellt, dass jeder Kunde im Einzelhandelsgeschäft ein Gerät durchlaufen und bei der Interaktion mit dem Gerät dieselbe Benutzerfreundlichkeit aufweisen kann.

Schritt 1: Bereinigung
* Alle Win32- und Store-Apps werden geschlossen.
* Alle Dateien in bekannten Ordnern wie __Bilder__, __Videos__, __Musik__, __Dokumente__, __SavedPictures__, __CameraRoll__, __Desktop__ und __Downloads__ Ordner werden gelöscht.
* Unstrukturierte und strukturierte Roamingzustände werden gelöscht.
* Strukturierte lokale Zustände werden gelöscht.

Schritt 2: Einrichten
* Offlinegeräte: Ordner bleiben leer
* Onlinegeräte: Demoressourcen für den Einzelhandel können vom Microsoft Store per Push an das Gerät übertragen werden.

### <a name="store-data-across-user-sessions"></a>Daten über Benutzersitzungen hinweg speichern

Zum Speichern von Daten über Benutzersitzungen hinweg können Sie Informationen in " __ApplicationData. Current. temporaryFolder__ " speichern, da die Daten in diesem Ordner nicht automatisch von dem standardmäßigen Cleanupprozess gelöscht werden. Beachten Sie, dass die mit *localstate* gespeicherten Informationen während des Bereinigungs Prozesses gelöscht werden.

### <a name="customize-the-cleanup-process"></a>Anpassen des Bereinigungs Prozesses

Um den Cleanupprozess anzupassen, implementieren Sie die APP Service-`Microsoft-RetailDemo-Cleanup` in Ihrer APP.

Szenarios, in denen eine benutzerdefinierte Bereinigungs Logik erforderlich ist, umfassen das Ausführen eines umfangreichen Setups, das herunterladen und Zwischenspeichern von Daten oder das Löschen von *localstate* -Daten.

Schritt 1: Deklarieren Sie den _Microsoft-retaildemo-Cleanup-_ Dienst im App-Manifest.
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

Schritt 2: Implementieren Sie die benutzerdefinierte Bereinigungs Logik unter der case-Funktion " _appdatacleanup_ " mithilfe der unten stehenden Beispiel Vorlage.
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>Verwandte Links

* [Speichern und Abrufen von App-Daten](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Erstellen und Nutzen eines App Service](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Lokalisieren von App-Inhalten](https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Demo Darstellung im Einzelhandel (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
