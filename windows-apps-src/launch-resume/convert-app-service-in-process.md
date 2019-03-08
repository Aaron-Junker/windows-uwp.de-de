---
title: Umwandeln eines App-Diensts für die Ausführung im gleichen Prozess wie die Host-App
description: Konvertieren Sie App-Dienstcode, der in einem separaten Hintergrundvorgang auf Code gestoßen ist, der im selben Prozess wie Ihr App-Dienstanbieter ausgeführt wird.
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, Uwp, app service
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: a976ac69d289a5582c2f3546227adba707ac5297
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643415"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>Umwandeln eines App-Diensts für die Ausführung im gleichen Prozess wie die Host-App

Eine [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) ermöglicht einer anderen Anwendung, Ihre App im Hintergrund zu aktivieren und einen direkten Kommunikationsweg mit ihr zu starten.

Mit der Einführung von In-Process-App-Diensten können zwei ausgeführte Vordergrundanwendungen einen direkten Kommunikationsweg über eine App-Dienst-Verbindung aufweisen. App-Dienste können jetzt im gleichen Prozess wie die Vordergrundanwendung ausgeführt werden. Die Kommunikation zwischen Apps wird dadurch sehr viel einfacher, während gleichzeitig die Notwendigkeit entfällt, den Dienstcode in ein separates Projekt zu trennen.

Um einen Out-of-Process-App-Dienst in ein In-Process-Modell zu konvertieren, sind zwei Änderungen erforderlich. Die erste ist eine Manifest-Änderung.

> ```xml
> <Package
>    ...
>   <Applications>
>       <Application Id=...
>           ...
>           EntryPoint="...">
>           <Extensions>
>               <uap:Extension Category="windows.appService">
>                   <uap:AppService Name="InProcessAppService" />
>               </uap:Extension>
>           </Extensions>
>           ...
>       </Application>
>   </Applications>
> ```

Entfernen Sie die `EntryPoint` -Attribut aus der `<Extension>` Element da jetzt [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) ist der Einstiegspunkt, der verwendet wird, wenn die app Service aufgerufen wird.

Die zweite Änderung besteht darin, die Dienstlogik aus ihrem eigenen Hintergrundaufgabenprojekt in Methoden zu verschieben, die über **OnBackgroundActivated()** aufgerufen werden können.

Jetzt kann Ihre Anwendung den App-Dienst direkt ausführen. Z. B. in "App.Xaml.cs":

[!NOTE] Der folgende Code ist nicht angegeben wird, z. B. 1 (Out-of-Process-Dienst). Der folgende Code dient lediglich zur Veranschaulichung und sollte nicht als Teil von Beispiel 2 (in-Process-Dienst) verwendet werden.  Zum Fortsetzen des Artikels Übergang aus Beispiel weiterhin verwenden den Code, z. B. 1 anstelle der Veranschaulichung der folgende Code 1 (Out-of-Process-Dienst) in Beispiel 2 (in-Process-Dienst).

``` cs
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
...

sealed partial class App : Application
{
  private AppServiceConnection _appServiceConnection;
  private BackgroundTaskDeferral _appServiceDeferral;

  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
      _appServiceDeferral = taskInstance.GetDeferral();
      taskInstance.Canceled += OnAppServicesCanceled;
      _appServiceConnection = appService.AppServiceConnection;
      _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
      _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
  }

  private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
  {
      AppServiceDeferral messageDeferral = args.GetDeferral();
      ValueSet message = args.Request.Message;
      string text = message["Request"] as string;

      if ("Value" == text)
      {
          ValueSet returnMessage = new ValueSet();
          returnMessage.Add("Response", "True");
          await args.Request.SendResponseAsync(returnMessage);
      }
      messageDeferral.Complete();
  }

  private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
  {
      _appServiceDeferral.Complete();
  }

  private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
  {
      _appServiceDeferral.Complete();
  }
}
```

Im obigen Code steuert die `OnBackgroundActivated`-Methode die App-Dienst-Aktivierung. Alle Ereignisse, die für die Kommunikation über eine [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) erforderlich sind, werden registriert, und das Aufgaben-Verzögerungsobjekt wird gespeichert, damit es nach Abschluss der Kommunikation zwischen den Anwendungen als abgeschlossen gekennzeichnet werden kann.

Wenn die App eine Anforderung empfängt, liest sie das zur Verfügung gestellte [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx), um festzustellen, ob die Zeichenfolgen `Key` und `Value` vorhanden sind. Wenn sie vorhanden sind, gibt der App-Dienst ein Wertepaar von Zeichenfolgen `Response` und `True` an die App auf der anderen Seite der **AppServiceConnection** zurück.

Weitere Informationen zum Herstellen einer Verbindung und Kommunizieren mit anderen Apps finden Sie unter [Erstellen und Verwenden eines App-Diensts](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396).
