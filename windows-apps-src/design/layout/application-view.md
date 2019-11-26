---
Description: Verwenden Sie die applicationview-Klasse, um verschiedene Teile der app in separaten Fenstern anzuzeigen.
title: Verwenden der applicationview-Klasse zum Anzeigen sekundärer Fenster für eine APP
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1a89596979f84c1ec4d698d14deacf8f852a7fbd
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258194"
---
# <a name="show-multiple-views-with-applicationview"></a>Anzeigen mehrerer Ansichten mit applicationview

Sie können Ihren Benutzern zu mehr Produktivität verhelfen, indem Sie ihnen ermöglichen, unabhängige Teile der App in separaten Fenstern anzuzeigen. Wenn Sie für eine App mehrere Fenster erstellen, verhält sich jedes Fenster anders. Auf der Taskleiste wird jedes Fenster separat angezeigt. Die Benutzer können App-Fenster unabhängig voneinander verschieben, deren Größe ändern, Fenster anzeigen und ausblenden und zwischen App-Fenstern wechseln, als würde es sich um separate Apps handeln. Jedes Fenster agiert in seinem eigenen Thread.

> **Wichtige APIs**: [**ApplicationViewSwitcher**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher), [**CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="what-is-a-view"></a>Was ist eine Ansicht?

Eine App-Ansicht ist die 1:1-Zuordnung eines Threads und eines Fensters, das die App zur Anzeige von Inhalten verwendet. Sie wird durch ein [**Windows.ApplicationModel.Core.CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)-Objekt dargestellt.

Ansichten werden durch das [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication)-Objekt verwaltet. Sie rufen [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) auf, um ein [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)-Objekt zu erstellen. Die **CoreApplicationView** vereint ein [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) und einen [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) (der in den Eigenschaften [**CoreWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) und [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher) gespeichert ist). Sie können sich **CoreApplicationView** als das Objekt vorstellen, das von der Windows-Runtime für die Interaktion mit dem zentralen Windows-System verwendet wird.

In der Regel arbeiten Sie nicht direkt mit der [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView). Stattdessen wird die [**ApplicationView**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationView)-Klasse von der Windows-Runtime im [**Windows.UI.ViewManagement**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement)-Namespace bereitgestellt. Diese Klasse stellt Eigenschaften, Methoden und Ereignisse bereit, die Sie bei der Interaktion zwischen App und Windowing-System verwenden. Wenn Sie mit einer **ApplicationView** arbeiten möchten, rufen Sie die statische [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview)-Methode auf, die eine an den aktuellen Thread der **CoreApplicationView** gebundene Instanz von **ApplicationView** abruft.

Entsprechend umschließt das XAML-Framework das [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)-Objekt in einem [**Windows.UI.XAML.Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window)-Objekt. In einer XAML-App interagieren Sie normalerweise mit dem **Window**-Objekt, anstatt direkt mit dem **CoreWindow** zu arbeiten.

## <a name="show-a-new-view"></a>Anzeigen einer neuen Ansicht

Während jedes App-Layout einzigartig ist, empfehlen wir Ihnen, eine Schaltfläche für ein „Neues Fenster” an einem geeigneten Ort zu platzieren, wie etwa die obere rechte Ecke des Inhalts, der in einem neuen Fenster geöffnet werden kann. Erwägen Sie außerdem, die Kontextmenüoption „In einem neuen Fenster öffnen” einzufügen.

Sehen wir uns die Schritte zum Erstellen einer neuen Ansicht an. Die neue Ansicht wird hier als Reaktion auf das Anklicken einer Schaltfläche gestartet.

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**So zeigen Sie eine neue Ansicht an**

1.  Rufen Sie [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) auf, um ein neues Fenster und einen Thread für den anzuzeigenden Inhalt zu erstellen.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Verfolgen Sie die [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) der neuen Ansicht nach. Über die ID können Sie die Ansicht später anzeigen.

    Sie können Ihre App mit einer bestimmten Infrastruktur ausstatten, um das Nachverfolgen der erstellten Ansichten zu erleichtern. Ein Beispiel bietet die `ViewLifetimeControl`-Klasse im [MultipleViews-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MultipleViews).

    ```csharp
    int newViewId = 0;
    ```

3.  Füllen Sie das Fenster im neuen Thread auf.

    Mithilfe der [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)-Methode können Sie die Arbeit am UI-Thread für die neue Ansicht planen. Mithilfe eines [Lambdaausdrucks](https://msdn.microsoft.com/library/bb397687.aspx) übergeben Sie eine Funktion als Argument an die **RunAsync**-Methode. Die in der Lambdafunktion ausgeführten Arbeiten finden im Thread der neuen Ansicht statt.

    In XAML fügen Sie der [**Content**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)-Eigenschaft von [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) in der Regel einen [**Frame**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) hinzu und navigieren dann den **Frame** zu einer XAML-[**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page), auf der Sie den App-Inhalt definiert haben. Weitere Informationen zu Frames und Seiten finden Sie unter [Peer-to-Peer-Navigation zwischen zwei Seiten](../basics/navigate-between-two-pages.md).

    Nachdem das neue [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) aufgefüllt wurde, müssen Sie dieActivate[ **-Methode von** Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate) aufrufen, um das **Window** später anzuzeigen. Diese Arbeit findet im Thread der neuen Ansicht statt, sodass das neue **Window** aktiviert ist.

    Schließlich rufen Sie die [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) der neuen Ansicht ab, die Sie später zum Anzeigen der Ansicht verwenden. Auch diese Arbeit wird im Thread der neuen Ansicht erledigt, sodass [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) die **Id** der neuen Ansicht abruft.

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  Zeigen Sie die neue Ansicht an, indem Sie [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) aufrufen.

    Nachdem Sie eine neue Ansicht erstellt haben, können Sie sie in einem neuen Fenster anzeigen, indem Sie die [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync)-Methode aufrufen. Der *viewId*-Parameter für diese Methode ist eine ganze Zahl, die die einzelnen Ansichten in der App eindeutig identifiziert. Sie rufen die [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id) der Ansicht entweder über die **ApplicationView.Id**-Eigenschaft oder die [**ApplicationView.GetApplicationViewIdForWindow**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow)-Methode ab.

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>Die Hauptansicht


Die erste Ansicht, die beim Starten der App erstellt wird, wird als *Hauptansicht* bezeichnet. Diese Ansicht ist in der [**CoreApplication.MainView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.mainview)-Eigenschaft gespeichert und ihre [**IsMain**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain)-Eigenschaft ist „true“. Diese Ansicht wird nicht von Ihnen, sondern von der App erstellt. Der Thread der Hauptansicht dient als Manager für die App und alle App-Aktivierungsereignisse werden in diesem Thread übermittelt.

Wenn sekundäre Ansichten geöffnet sind, kann das Fenster der Hauptansicht ausgeblendet werden, z. B. durch Klicken auf die Schaltfläche „Schließen“ (X) in der Fenstertitelleiste. Der Thread bleibt jedoch aktiv. Durch Aufrufen von [**Close**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.close) im [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) der Hauptansicht wird eine **InvalidOperationException** ausgelöst. (Verwenden Sie [**Application.Exit**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.exit), um Ihre App zu schließen.) Sobald der Thread der Hauptansicht beendet wird, wird die App geschlossen.

## <a name="secondary-views"></a>Sekundäre Ansichten


Andere Ansichten sind sekundäre Ansichten. Dies schließt auch Ansichten ein, die durch Aufrufen von [**CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) im App-Code erstellt werden. Sowohl die Hauptansicht als auch sekundäre Ansichten werden in der [**CoreApplication.Views**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.views)-Auflistung gespeichert. Normalerweise erstellen Sie sekundäre Ansichten in Reaktion auf eine Benutzeraktion. In einigen Fällen erstellt das System sekundäre Ansichten für Ihre App.

> [!NOTE]
> Sie können das Windows-Feature *Zugewiesener Zugriff* verwenden, um eine App im [Kioskmodus](https://docs.microsoft.com/windows/manage/set-up-a-device-for-anyone-to-use) auszuführen. In diesem Fall erstellt das System eine sekundäre Ansicht, um die App-Benutzeroberfläche über dem Sperrbildschirm darzustellen. Von der App erstellte sekundäre Ansichten sind nicht zulässig. Wenn Sie also versuchen, Ihre eigene sekundäre Ansicht im Kioskmodus anzuzeigen, wird eine Ausnahme ausgelöst.

## <a name="switch-from-one-view-to-another"></a>Wechseln zwischen Ansichten

Erwägen Sie, den Benutzern eine Möglichkeit zur Navigation von einem sekundären Fenster zurück zum Hauptfenster anzubieten. Verwenden Sie dazu die [**ApplicationViewSwitcher.SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync)-Methode. Sie rufen diese Methode über den Thread des Fensters auf, von dem aus Sie wechseln, und übergeben die Ansichts-ID des Fensters, zu dem Sie wechseln.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Wenn Sie [**SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) verwenden, können Sie auswählen, ob das erste Fenster geschlossen und aus der Taskleiste entfernt werden soll, indem Sie den Wert von [**ApplicationViewSwitchingOptions**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions) angeben.

## <a name="related-topics"></a>Verwandte Themen

- [Mehrere Ansichten anzeigen](show-multiple-views.md)
- [Mehrere Ansichten mit appwindow anzeigen](app-window.md)
- [Applicationviewswitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- ["Kreatenewview"](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
