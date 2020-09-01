---
title: Behandeln des Anhaltens von Apps
description: Hier erfahren Sie, wie Sie wichtige Anwendungsdaten speichern, wenn das System die App anhält.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 1c75200a768efd258aa84b20493b9d296578a05c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171824"
---
# <a name="handle-app-suspend"></a>Behandeln des Anhaltens von Apps

**Wichtige APIs**

- [**Wird angehalten**](/uwp/api/windows.ui.xaml.application.suspending)

Hier erfahren Sie, wie Sie wichtige Anwendungsdaten speichern, wenn das System die App anhält. Im Beispiel wird ein Ereignishandler für das [**Suspending**](/uwp/api/windows.ui.xaml.application.suspending)-Ereignis registriert und eine Zeichenfolge in einer Datei gespeichert.

## <a name="register-the-suspending-event-handler"></a>Registrieren des Suspending-Ereignishandlers

Registrieren Sie die Behandlung des [**Suspending**](/uwp/api/windows.ui.xaml.application.suspending)-Ereignisses, das angibt, dass die App ihre Anwendungsdaten speichern sollte, bevor sie vom System angehalten wird.

```csharp
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```

```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Suspending({ this, &MainPage::App_Suspending });
}
```

```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending +=
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## <a name="save-application-data-before-suspension"></a>Speichern von App-Daten vor dem Anhalten

Wenn die App das [**Suspending**](/uwp/api/windows.ui.xaml.application.suspending)-Ereignis behandelt, kann sie die wichtigen App-Daten in der Handlerfunktion speichern. Die App sollte die [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings)-Speicher-API verwenden, um einfache Anwendungsdaten synchron zu speichern.

```csharp
partial class MainPage
{
    async void App_Suspending(
        Object sender,
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object,
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated.
    End Sub

End Class
```

```cppwinrt
void MainPage::App_Suspending(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::ApplicationModel::SuspendingEventArgs const& /* e */)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

## <a name="release-resources"></a>Freigeben von Ressourcen

Sie sollten exklusive Ressourcen und Dateihandles freigeben, damit andere Apps darauf zugreifen können, während Ihre App angehalten wird. Beispiele für exklusive Ressourcen sind Kameras, E/A-Geräte, externe Geräte und Netzwerkressourcen. Durch die explizite Freigabe exklusiver Ressourcen und Dateihandles kann sichergestellt werden, dass andere Apps darauf zugreifen können, während Ihre App angehalten ist. Wenn die App fortgesetzt wird, sollte sie ihre exklusiven Ressourcen und Dateihandles erneut abrufen.

## <a name="remarks"></a>Bemerkungen

Das System hält Ihre App an, wenn der Benutzer zu einer anderen App oder zum Desktop bzw. Startbildschirm wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird diese vom System fortgesetzt. Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung. Das System stellt die App exakt so wieder her, wie sie unterbrochen wurde. Dadurch entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden.

Das System versucht, die App und die zugehörigen Daten im Arbeitsspeicher beizubehalten, während sie angehalten ist. Wenn das System aber nicht über die notwendigen Ressourcen verfügt, um die App im Arbeitsspeicher zu behalten, beendet es die App. Wenn der Benutzer wieder zu einer angehaltenen App wechselt, die beendet wurde, sendet das System ein [**Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)-Ereignis, und es sollte die App-Daten in der [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched)-Methode wiederherstellen.

Das System benachrichtigt eine App nicht, wenn sie beendet wird. Wenn Ihre App angehalten wird, muss sie daher die App-Daten speichern und die exklusiven Ressourcen und Dateihandles freigeben, damit diese beim erneuten Aktivieren der App nach der Beendigung wiederhergestellt werden können.

Wenn Sie innerhalb Ihres Handlers einen asynchronen Aufruf ausführen, wird die Steuerung sofort von diesem asynchronen Aufruf zurückgegeben. Das bedeutet, dass die Ausführung dann vom Ereignishandler zurückgegeben werden kann und die App in den nächsten Zustand übergeht, obwohl der asynchrone Aufruf noch nicht abgeschlossen wurde. Verwenden Sie die [**GetDeferral**](/uwp/api/Windows.ApplicationModel)-Methode für das an den Ereignishandler übergebene [**EnteredBackgroundEventArgs**](/uwp/api/Windows.ApplicationModel)-Objekt, um das Anhalten zu verzögern, bis Sie für das zurückgegebene [**Windows.Foundation.Deferral**](/uwp/api/windows.foundation.deferral)-Objekt die [**Complete**](/uwp/api/windows.foundation.deferral.complete)-Methode aufgerufen haben.

Durch eine Verzögerung verlängert sich nicht die Zeit, die Ihr Code ausgeführt werden muss, bevor die App beendet wird. Dabei wird nur die Beendigung verzögert, bis die *Complete*-Methode der Verzögerung aufgerufen wird oder die Frist abläuft, *je nachdem, was zuerst eintritt*. So verlängern Sie die Zeit im anhaltezustand [ **ExtendedExecutionSession**](run-minimized-with-extended-execution.md)

> [!NOTE]
> Um die Reaktionsfähigkeit des Systems in Windows 8.1 zu verbessern, erhalten apps Zugriff mit niedriger Priorität auf Ressourcen, nachdem Sie angehalten wurden. Zur Unterstützung dieser neuen Priorität wird das Timeout für den Anhaltevorgang ausgedehnt, sodass die App für normale Priorität unter Windows über einen 5-Sekunden-Timeout und unter Windows Phone über einen Timeout von 1 bis 10 Sekunden verfügt. Dieses Timeout-Fenster kann weder verlängert noch geändert werden.

**Hinweis zum Debuggen mit Visual Studio:** Visual Studio hindert Windows daran, eine APP zu sperren, die an den Debugger angefügt ist. Dies hat den Zweck, dem Benutzer das Anzeigen der Debugging-Benutzeroberfläche von Visual Studio zu ermöglichen, während die App ausgeführt wird. Beim Debuggen einer App können Sie mit Visual Studio ein Anhalteereignis an die App senden. Stellen Sie sicher, dass die Symbolleiste **Debugspeicherort** angezeigt wird, und klicken Sie dann auf das Symbol **Anhalten**.

## <a name="related-topics"></a>Zugehörige Themen

* [App-Lebenszyklus](app-lifecycle.md)
* [Behandeln der App-Aktivierung](activate-an-app.md)
* [Behandeln der App-Fortsetzung](resume-an-app.md)
* [UX-Richtlinien für das Starten, Anhalten und Fortsetzen von Apps](./index.md)
* [Erweiterte Ausführung](run-minimized-with-extended-execution.md)

 

 