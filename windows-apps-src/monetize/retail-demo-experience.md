---
title: Hinzufügen von Retail-Demo (RDX)-Funktionen zu Ihrer app
description: Vorbereiten Sie Ihrer app für den Einzelhandel Demo-Modus, hilft Ihrer app in den Verkaufsraum Einzelhandel zu veranschaulichen.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP, Demo-App für den Einzelhandel
ms.localizationpriority: medium
ms.openlocfilehash: 4c9f31da8e2509c41715a13fbc0bb0322782340a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366518"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Hinzufügen von Retail-Demo (RDX)-Funktionen zu Ihrer app

Fügen Sie einen Demomodus für den Einzelhandel in Ihre Windows-App ein, damit Kunden, die PCs und Geräte im Geschäft ausprobieren, direkt loslegen können.

Wenn Kunden im Einzelhandel sind, erwarten sie, dass Sie Demos von PCs und Geräten testen können. Sie verbringen oft eine erhebliche Teil ihrer Zeit mit apps über die [retail-Demo-Umgebung (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience).

Sie können Ihre app einrichten, unterschiedliche Umgebungen bei der im bereitzustellenden _normalen_ oder _Retail_ Modi. Beispielsweise wenn Ihre app ein Installationsvorgang beginnt, können Sie im Modus für im Einzelhandel überspringen und vorab aufgefüllt wird die app mit Daten und Beispiel-Standardeinstellungen, damit sie sofort beginnen können.

Hinsichtlich unserer Kunden besteht nur eine app zur Verfügung. Damit Kunden, die zwischen den beiden Modi unterscheiden können, empfehlen wir, während Ihre app im Einzelhandel befindet, das Wort "Retail" werden an gut sichtbarer Stelle in der Titelleiste oder an einem geeigneten Speicherort.

Zusätzlich zu den Microsoft Store-Anforderungen für apps muss RDX-fähige apps zudem kompatibel mit der RDX-Setup, Bereinigung und Update-Prozesse, um sicherzustellen, dass Kunden eine konsistente positive Erfahrung in der Filiale.

## <a name="design-principles"></a>Designprinzipien

* **Zeigen Sie mit die besten**. Verwenden Sie das Kundenerlebnis im Einzelhandel Demo, warum Ihre app eine gelungene, Showcase. Dies ist wahrscheinlich zum ersten Mal Ihr Kunde Ihre app so sie die beste Information angezeigt, die angezeigt wird.

* **Zeigen sie auf schnelle**. Kunden können ungeduldig sein – je schneller ein Benutzer den Wert ihrer App erfasst, desto besser.

* **Halten Sie die Story einfach**. Das Kundenerlebnis im Einzelhandel Demo ist ein Kurzpräsentation für Ihre app Wert.

* **Konzentrieren Sie sich von der Erfahrung mit**. Geben Sie den Benutzern die Zeit, um Ihre Inhalte zu verdauen. Es ist zwar wichtig, dass die Benutzer schnell zum wichtigsten Teil gelangen, aber nur mithilfe entsprechender Pausen können sie den Wert der App richtig erkennen.

## <a name="technical-requirements"></a>Technische Anforderungen

Wie RDX-fähige apps gedacht sind, um das beste aus Ihrer app für Privatkunden vorzustellen, müssen sie technische Anforderungen zu erfüllen und die Datenschutzbestimmungen, die den Microsoft Store für alle Retail-Demo-Umgebung apps entsprechen.

Dies kann als Prüfliste können Sie die für den Überprüfungsprozess vorbereiten und zur besseren Übersichtlichkeit des Testprozesses verwendet werden. Beachten Sie, dass diese Anforderungen nicht nur für den Prüfprozess, sondern für die gesamte Lebensdauer der Demo-App für den Einzelhandel (d. h. solange Ihre App auf den Vorführgeräten ausgeführt wird) eingehalten werden müssen.

### <a name="critical-requirements"></a>Wichtige Anforderungen

RDX-fähige apps, die diese kritischen Anforderungen nicht erfüllen werden so bald wie möglich von allen Retail-Demo-Geräten entfernt.

* **Nicht für personenbezogene Informationen (PII) Fragen**. Dazu gehören Informationen zur Anmeldung, Informationen zum Microsoft-Konto, oder wenden Sie sich an Details.

* **Fehler-Erfahrung**. Ihr App muss fehlerfrei funktionieren. Außerdem dürfen keine Fehler-Pop-ups oder -Benachrichtigungen angezeigt werden, wenn Kunden die Vorführgeräte verwenden. Fehler reflektieren negativ auf die app selbst, Ihre Marke, des Geräts Marke, Marke der Hersteller des Geräts und des Microsoft-Marke.

* **Kostenpflichtige apps benötigen einen Testmodus**. Ihre app muss entweder eine kostenlose oder enthalten einen [Testmodus](https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app). Kunden, die sich in einem Laden etwas ansehen, möchten dafür nicht zahlen.

### <a name="high-priority-requirements"></a>Anforderungen mit hoher Priorität

RDX-fähigen apps, die diese wichtigen Anforderungen nicht erfüllen müssen für eine Problembehebung sofort untersucht werden. Wenn eine umgehende Problembehebung nicht möglich ist, kann diese App von allen Vorführgeräten entfernt werden.

* **Offline-Erfahrung einprägsam**. Ihre app muss zu eine überzeugenden Umgebung für die offline zu veranschaulichen, wie etwa 50 % der Geräte, die im Einzelhandel offline sind. Dadurch wird sichergestellt, dass das Erlebnis für die Kunden auch dann positiv ist, wenn sie offline mit Ihrer App interagieren.

* **Aktualisiert die Darstellung von Inhalten in**. Ihre app sollte niemals Eingabeaufforderung für Updates, wenn online sein. Wenn Updates erforderlich sind, sollten sie automatisch ausgeführt werden.

* **Keine anonymen Kommunikation**. Da ein Kunde ein Retail-Demo-Gerät über einen anonymen Benutzer ist, sollte sie nicht auf Nachricht oder einen Dateifreigabe auf dem Gerät können.

* **Konsistentes Erlebnis unter Verwendung des Cleanup-Prozess bereitstellen und**. Die Verwendung eines Vorführgeräts sollte für alle Kunden gleich sein. Ihre app verwenden, sollten [Cleanupprozess](#cleanup-process) auf den gleichen Standardstatus nach jeder Verwendung zurückgegeben. Wir wollen nicht den nächsten Kunden an, welche den letzten Kunden zurückgelassen finden Sie unter. Dies umfasst z. B. Punktestände, Erfolge und aufgehobene Sperren.

* **Alter der entsprechende Inhalt**. Sämtliche app-Inhalte muss einem Teenager oder vom niedrigere Bewertung Kategorie zugewiesen. Weitere Informationen finden Sie unter [Veröffentlichen Ihrer app durch IARC bewertet](https://www.globalratings.com/for-developers.aspx) und [ESRB Bewertungen](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Anforderungen an die mittlere Priorität

Das Windows-Team für den Einzelhandel setzt sich unter Umständen direkt mit Entwicklern in Verbindung, um mit ihnen zu besprechen, wie diese Probleme behoben werden können.

* **Möglichkeit zum Ausführen von erfolgreich über eine Vielzahl von Geräten**. Apps müssen auf allen Geräten, einschließlich der Geräte mit Low-End-Spezifikationen ausführen. Wenn die app auf Geräten, die nicht die Mindestanforderungen erfüllt installiert ist, muss die app eindeutig den Benutzer dazu zu informieren. Die Mindestgeräteanforderungen müssen bekanntgegeben werden, damit die App immer mit höchster Leistung ausgeführt werden kann.

* **Anforderungen für den Einzelhandel Store-app-Größe**. Die App darf eine Größe von 800 MB nicht übersteigen. Wenden Sie sich an das Windows Retail Store-Team direkt für Weitere Informationen zu, wenn Ihre RDX-fähige Anwendung nicht die größenanforderungen erfüllt.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: Vorbereiten von Code für die Demo-Modus

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
Die [ **IsDemoModeEnabled** ](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) -Eigenschaft in der [ **RetailInfo** ](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) Dienstprogrammklasse, gehört der der [ Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) -Namespace in das Windows 10 SDK wird als ein boolescher Indikator verwendet, an die Ihre app wird ausgeführt - Codepfad die _normalen_ Modus oder den _Retail_ Modus.

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync(“demo”);
}

StorageFile file = await folder.GetFileAsync(“hello.txt”);
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync(“demo”).then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync(“hello.txt”);
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync(“hello.txt”).then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log(“Retail mode is enabled.”);
} else {
    Console.log(“Retail mode is not enabled.”);
}
```

### <a name="retailinfoproperties"></a>RetailInfo.Properties

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

## <a name="cleanup-process"></a>Cleanup-Prozess

Cleanup beginnt, zwei Minuten, nachdem ein Einkäufer die Interaktion mit dem Gerät beendet wurde. Die Retail-Demo wiedergegeben wird, und Windows beginnt das Zurücksetzen von Beispieldaten in die Kontakte, Fotos und anderen apps. Je nach Gerät kann dies von 1 bis 5 Minuten vollständig alles wieder in normalem zurücksetzen dauern. Dadurch wird sichergestellt, dass jeder Kunde im Ladengeschäft kann auf einem Gerät Aufwärts und die gleiche Erfahrung bei der Interaktion mit dem Gerät.

Schritt 1: Bereinigen
* Alle Win32- und Store-Apps werden geschlossen.
* Alle Dateien in bekannten Ordnern wie __Bilder__, __Videos__, __Musik__, __Dokumente__, __SavedPictures__, __CameraRoll__, __Desktop__ und __Downloads__ Ordner werden gelöscht.
* Unstrukturierte und strukturierte Roamingzustände werden gelöscht.
* Strukturierte lokale Zustände werden gelöscht.

Schritt 2:  Setup
* Für offline-Geräte: Ordner bleiben leer.
* Für online-Geräte: Ressourcen im Einzelhandel Demo können auf dem Gerät abgelegt werden, aus dem Microsoft Store.

### <a name="store-data-across-user-sessions"></a>Store-Daten über benutzersitzungen

Sie können zum Speichern von Daten über benutzersitzungen Informationen speichern __ApplicationData.Current.TemporaryFolder__ als Standard-Cleanup-Prozess automatisch löscht keine Daten in diesem Ordner. Beachten Sie diese Informationen mithilfe von gespeichert *LocalState* wird während des Bereinigungsvorgangs gelöscht.

### <a name="customize-the-cleanup-process"></a>Den Cleanup-Prozess anpassen

Implementieren Sie zum Anpassen des Cleanupprozess der `Microsoft-RetailDemo-Cleanup` app Service in Ihrer app.

Szenarien, bei denen eine benutzerdefinierte Bereinigungslogik erforderlich ist, umfasst das Ausführen einer umfangreichen Konfiguration herunterladen und Zwischenspeichern von Daten, oder möchten nicht *LocalState* Daten gelöscht werden.

Schritt 1: Deklarieren Sie die _Microsoft RetailDemo-Bereinigung_ -Dienst im app-Manifest.
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

Schritt 2: Implementieren Sie Ihre benutzerdefinierten Bereinigungslogik unter der _AppdataCleanup_ Case-Funktion mit den weiter unten angegebene Beispielvorlage.
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

* [Store und Abrufen von app-Daten](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Das Erstellen und nutzen einen app service](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Lokalisieren von app-Inhalte](https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Demo-Erfahrung im Einzelhandel (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
