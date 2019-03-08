---
Description: Zeigen Sie mehrere Teile Ihrer app in separaten Fenstern.
title: Anzeigen mehrerer Ansichten für eine App
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7ed69dc912e916f7964c125550621c22dfcd9555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607625"
---
# <a name="show-multiple-views-for-an-app"></a>Anzeigen mehrerer Ansichten für eine App

![Drahtmodell, das eine App mit mehreren Fenstern zeigt.](images/multi-view.gif)

Sie können Ihren Benutzern zu mehr Produktivität verhelfen, indem Sie ihnen ermöglichen, unabhängige Teile der App in separaten Fenstern anzuzeigen. Wenn Sie für eine App mehrere Fenster erstellen, verhält sich jedes Fenster anders. Auf der Taskleiste wird jedes Fenster separat angezeigt. Die Benutzer können App-Fenster unabhängig voneinander verschieben, deren Größe ändern, Fenster anzeigen und ausblenden und zwischen App-Fenstern wechseln, als würde es sich um separate Apps handeln. Jedes Fenster agiert in seinem eigenen Thread.

> **Wichtige APIs**: [**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094), [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

## <a name="when-should-an-app-use-multiple-views"></a>Wann sollte eine App mehrere Ansichten verwenden?
Es gibt eine Reihe von Szenarien, die von mehreren Ansichten profitieren können. Hier finden Sie einige Beispiele:
 - Eine E-Mail-App, die Benutzern beim Verfassen einer neuen E-Mail eine Liste der empfangenen Nachrichten anzeigt.
 - Eine Adressbuch-App, mit der Benutzer Kontaktinformationen für mehrere Personen nebeneinander vergleichen können.
 - Eine Musikplayer-App, mit der Benutzer beim Durchsuchen einer Liste anderer verfügbarer Musiktitel sehen können, was wiedergegeben wird.
 - Eine Notizen-App, mit der Benutzer Informationen von einer Seite in eine andere kopieren können.
 - Eine Lese-App, mit der Benutzer mehrere Artikel für das spätere Lesen öffnen können, um zunächst alle wichtigen Schlagzeilen zu durchsuchen.

Weitere Informationen zum Erstellen separaten Instanzen Ihrer App finden Sie unter [Erstellen einer UWP-App mit mehreren Instanzen](../../launch-resume/multi-instance-uwp.md).

## <a name="what-is-a-view"></a>Was ist eine Ansicht?

Eine App-Ansicht ist die 1:1-Zuordnung eines Threads und eines Fensters, das die App zur Anzeige von Inhalten verwendet. Sie wird durch ein [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)-Objekt dargestellt.

Ansichten werden durch das [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016)-Objekt verwaltet. Sie rufen [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) auf, um ein [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)-Objekt zu erstellen. Die **CoreApplicationView** vereint ein [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) und einen [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) (der in den Eigenschaften [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) und [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264) gespeichert ist). Sie können sich **CoreApplicationView** als das Objekt vorstellen, das von der Windows-Runtime für die Interaktion mit dem zentralen Windows-System verwendet wird.

In der Regel arbeiten Sie nicht direkt mit der [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). Stattdessen wird die [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/hh701658)-Klasse von der Windows-Runtime im [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/br242295)-Namespace bereitgestellt. Diese Klasse stellt Eigenschaften, Methoden und Ereignisse bereit, die Sie bei der Interaktion zwischen App und Windowing-System verwenden. Wenn Sie mit einer **ApplicationView** arbeiten möchten, rufen Sie die statische [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672)-Methode auf, die eine an den aktuellen Thread der **CoreApplicationView** gebundene Instanz von **ApplicationView** abruft.

Entsprechend umschließt das XAML-Framework das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt in einem [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041)-Objekt. In einer XAML-App interagieren Sie normalerweise mit dem **Window**-Objekt, anstatt direkt mit dem **CoreWindow** zu arbeiten.

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

**Um eine neue Ansicht anzuzeigen.**

1.  Rufen Sie [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291) auf, um ein neues Fenster und einen Thread für den anzuzeigenden Inhalt zu erstellen.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Verfolgen Sie die [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) der neuen Ansicht nach. Über die ID können Sie die Ansicht später anzeigen.

    Sie können Ihre App mit einer bestimmten Infrastruktur ausstatten, um das Nachverfolgen der erstellten Ansichten zu erleichtern. Ein Beispiel bietet die `ViewLifetimeControl`-Klasse im [MultipleViews-Beispiel](https://go.microsoft.com/fwlink/p/?LinkId=620574).

    ```csharp
    int newViewId = 0;
    ```

3.  Füllen Sie das Fenster im neuen Thread auf.

    Mithilfe der [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)-Methode können Sie die Arbeit am UI-Thread für die neue Ansicht planen. Mithilfe eines [Lambdaausdrucks](https://go.microsoft.com/fwlink/p/?LinkId=389615) übergeben Sie eine Funktion als Argument an die **RunAsync**-Methode. Die in der Lambdafunktion ausgeführten Arbeiten finden im Thread der neuen Ansicht statt.

    In XAML fügen Sie der [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051)-Eigenschaft von [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) in der Regel einen [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) hinzu und navigieren dann den **Frame** zu einer XAML-[**Seite**](https://msdn.microsoft.com/library/windows/apps/br227503), auf der Sie den App-Inhalt definiert haben. Weitere Informationen finden Sie unter [Peer-zu-Peer-Navigation zwischen zwei Seiten](../basics/navigate-between-two-pages.md).

    Nachdem das neue [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) aufgefüllt wurde, müssen Sie die [**Activate**](https://msdn.microsoft.com/library/windows/apps/br209046)-Methode von **Window** aufrufen, um das **Window** später anzuzeigen. Diese Arbeit findet im Thread der neuen Ansicht statt, sodass das neue **Window** aktiviert ist.

    Schließlich rufen Sie die [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) der neuen Ansicht ab, die Sie später zum Anzeigen der Ansicht verwenden. Auch diese Arbeit wird im Thread der neuen Ansicht erledigt, sodass [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) die **Id** der neuen Ansicht abruft.

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

4.  Zeigen Sie die neue Ansicht an, indem Sie [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) aufrufen.

    Nachdem Sie eine neue Ansicht erstellt haben, können Sie sie in einem neuen Fenster anzeigen, indem Sie die [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101)-Methode aufrufen. Der *viewId*-Parameter für diese Methode ist eine ganze Zahl, die die einzelnen Ansichten in der App eindeutig identifiziert. Sie rufen die [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) der Ansicht entweder über die **ApplicationView.Id**-Eigenschaft oder die [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281109)-Methode ab.

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>Die Hauptansicht


Die erste Ansicht, die beim Starten der App erstellt wird, wird als *Hauptansicht* bezeichnet. Diese Ansicht ist in der [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465)-Eigenschaft gespeichert und ihre [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452)-Eigenschaft ist „true“. Diese Ansicht wird nicht von Ihnen, sondern von der App erstellt. Der Thread der Hauptansicht dient als Manager für die App und alle App-Aktivierungsereignisse werden in diesem Thread übermittelt.

Wenn sekundäre Ansichten geöffnet sind, kann das Fenster der Hauptansicht ausgeblendet werden, z. B. durch Klicken auf die Schaltfläche „Schließen“ (X) in der Fenstertitelleiste. Der Thread bleibt jedoch aktiv. Durch Aufrufen von [**Close**](https://msdn.microsoft.com/library/windows/apps/br209049) im [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) der Hauptansicht wird eine **InvalidOperationException** ausgelöst. (Verwenden [ **Application.Exit** ](https://msdn.microsoft.com/library/windows/apps/br242327) zu Ihrer app zu schließen.) Wenn die Hauptansicht der Thread beendet wird, wird die app geschlossen.

## <a name="secondary-views"></a>Sekundäre Ansichten


Andere Ansichten sind sekundäre Ansichten. Dies schließt auch Ansichten ein, die durch Aufrufen von [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) im App-Code erstellt werden. Sowohl die Hauptansicht als auch sekundäre Ansichten werden in der [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861)-Auflistung gespeichert. Normalerweise erstellen Sie sekundäre Ansichten in Reaktion auf eine Benutzeraktion. In einigen Fällen erstellt das System sekundäre Ansichten für Ihre App.

> [!NOTE]
> Sie können das Windows-Feature *Zugewiesener Zugriff* verwenden, um eine App im [Kioskmodus](https://technet.microsoft.com/library/mt219050.aspx) auszuführen. In diesem Fall erstellt das System eine sekundäre Ansicht, um die App-Benutzeroberfläche über dem Sperrbildschirm darzustellen. Von der App erstellte sekundäre Ansichten sind nicht zulässig. Wenn Sie also versuchen, Ihre eigene sekundäre Ansicht im Kioskmodus anzuzeigen, wird eine Ausnahme ausgelöst.

## <a name="switch-from-one-view-to-another"></a>Wechseln zwischen Ansichten

Erwägen Sie, den Benutzern eine Möglichkeit zur Navigation von einem sekundären Fenster zurück zum Hauptfenster anzubieten. Verwenden Sie dazu die [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097)-Methode. Sie rufen diese Methode über den Thread des Fensters auf, von dem aus Sie wechseln, und übergeben die Ansichts-ID des Fensters, zu dem Sie wechseln.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Wenn Sie [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) verwenden, können Sie auswählen, ob das erste Fenster geschlossen und aus der Taskleiste entfernt werden soll, indem Sie den Wert von [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105) angeben.

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

* Bieten Sie einen klaren Einstiegspunkt zur sekundären Ansicht anhand des Symbols „Neues Fenster öffnen” an.
* Übermitteln Sie den Benutzern den Zweck der sekundäre Ansicht.
* Stellen Sie sicher, dass Ihre App in einer Ansicht voll funktionsfähig ist und dass Benutzer nur aus praktischen Gründen eine sekundäre Ansicht öffnen.
* Verlassen Sie sich nicht auf die sekundäre Ansicht, um Benachrichtigungen oder andere vorübergehende visuelle Elemente anzubieten.

## <a name="related-topics"></a>Verwandte Themen

* [ApplicationViewSwitcher](https://msdn.microsoft.com/library/windows/apps/dn281094)
* [CreateNewView](https://msdn.microsoft.com/library/windows/apps/dn297278)
 
