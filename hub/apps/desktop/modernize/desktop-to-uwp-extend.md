---
Description: Erweitern Sie Ihre Desktopanwendung mit Windows-Benutzeroberflächen und -Komponenten
title: Erweitern Sie Ihre Desktopanwendung mit Windows-Benutzeroberflächen und -Komponenten
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 672485dd505227da0a59a220edaa9648e2521e63
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359626"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>Erweitern Sie Ihre desktop-Apps mit modernen UWP-Komponente

Einige Windows 10-Umgebungen (z. B. eine touch-fähige UI-Seite) müssen sich in einem Modern-App-Container befinden. Wenn Sie diese Funktionen hinzufügen möchten, erweitern Sie Ihre Desktopanwendung um UWP-Projekte und die Komponente für Windows-Runtime.

In vielen Fällen können Windows-Runtime-APIs direkt aus Ihrer desktop-Anwendung aufrufen, sodass Sie dieses Handbuch, überprüfen Sie vor dem finden Sie unter [für Windows 10 verbessern](desktop-to-uwp-enhance.md).

> [!NOTE]
> Die in diesem Artikel beschriebenen Funktionen erfordern, dass Sie ein Windows-app-Paket für die desktop-Anwendung erstellen. Wenn Sie noch dies nicht getan haben, finden Sie unter [Paket desktopanwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root).

Wenn Sie bereit sind, lassen Sie uns starten.

<a id="setup" />

## <a name="first-setup-your-solution"></a>Ihre Projektmappe einrichten

Fügen Sie Ihrer Projektmappe ein oder mehrere UWP-Projekte und Laufzeitkomponenten hinzu.

Beginnen Sie mit einer Projektmappe, die ein **Paketerstellungsprojekt für Windows-Anwendungen** mit einem Verweis auf Ihre Desktop-Anwendung enthält.

Diese Abbildung zeigt ein Beispiel für eine Projektmappe.

![Erweitern des Startprojekts](images/desktop-to-uwp/extend-start-project.png)

Wenn Ihre Lösung ein Packaging-Projekt nicht enthält, finden Sie unter [die desktop-Anwendung mit Visual Studio-Paket](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

### <a name="configure-the-desktop-application"></a>Konfigurieren Sie die desktop-Anwendung

Stellen Sie sicher, dass die desktop-Anwendung Verweise auf die Dateien, die Sie benötigen Windows-Runtime-APIs aufrufen.

Zu diesem Zweck finden Sie unter den [Einrichten des Projekts](desktop-to-uwp-enhance.md#set-up-your-project) Abschnitt.

### <a name="add-a-uwp-project"></a>Hinzufügen eines UWP-Projekts

Fügen Sie der Projektmappe eine **Leere App (Universelle Windows-App)** hinzu.

Hier erstellen Sie eine moderne XAML-Benutzeroberfläche oder verwenden APIs, die nur innerhalb eines UWP-Prozesses ausgeführt werden.

![UWP-Projekt](images/desktop-to-uwp/add-uwp-project-to-solution.png)

Klicken Sie in Ihrem Paketerstellungsprojekt mit der rechten Maustaste auf den Knoten **Anwendungen**, und klicken Sie dann auf **Verweis hinzufügen**.

![Referenzieren des UWP-Projekts](images/desktop-to-uwp/add-uwp-project-reference.png)

Fügen Sie einen Verweis auf das UWP-Projekt hinzu.

![Referenzieren des UWP-Projekts](images/desktop-to-uwp/choose-uwp-project.png)

Ihre Projektmappe sieht etwa wie folgt aus:

![Projektmappe mit UWP-Projekt](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Optional) Komponente für Windows-Runtime hinzufügen

Um einige Szenarien zu erreichen, müssen Sie einer Komponente für Windows-Runtime Code hinzufügen.

![Runtime-Komponente App-Dienst](images/desktop-to-uwp/add-runtime-component.png)

Fügen Sie dann in Ihrem UWP-Projekt einen Verweis auf die Laufzeitkomponente hinzu. Ihre Projektmappe sieht etwa wie folgt aus:

![Verweis auf die Laufzeitkomponente](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>Erstellen Sie die Projektmappe

Erstellen Sie die Projektmappe, um sicherzustellen, dass keine Fehler angezeigt werden. Wenn Sie Fehler erhalten, öffnen Sie **Configuration Manager** und stellen Sie sicher, dass Ihre Projekte die gleiche Plattform als Ziel.

![Konfigurations-manager](images/desktop-to-uwp/config-manager.png)

Werfen wir einen Blick auf einige Dinge, die Sie mit Ihren UWP-Projekten und Laufzeitkomponenten tun können.

## <a name="show-a-modern-xaml-ui"></a>Anzeigen einer modernen XAML-Benutzeroberfläche

Als Teil Ihres Anwendungsflusses können Sie moderne XAML-basierte Benutzeroberflächen in Ihre Desktopanwendung integrieren. Diese Benutzeroberflächen passen sich natürlich an unterschiedliche Bildschirmgrößen und Auflösungen an und unterstützen moderne, interaktive Modelle, z. B. Touch- und Freihandeingaben.

Mit etwas XAML-Markup-Code können Sie z. B. Benutzern leistungsstarke Kartenvisualisierungsfunktionen bereitstellen.

Diese Abbildung zeigt eine Windows Forms-Anwendung, die eine XAML-basierte, moderne Benutzeroberfläche öffnet, die ein Kartensteuerelement enthält.

![adaptives Design](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>Dieses Beispiel zeigt eine XAML UI von der Lösung ein UWP-Projekt hinzugefügt. Das ist die stabile unterstützten Ansatz für die XAML-Benutzeroberflächen in einer Desktopanwendung angezeigt. Die Alternative zu diesem Ansatz werden UWP XAML-Steuerelemente direkt an die desktop-Anwendung hinzufügen, mit einer XAML-Insel. XAML-Inseln sind derzeit als Entwicklervorschau verfügbar. Obwohl Sie diese in Ihrem eigenen Prototypcode ausprobieren können, jetzt bald, empfehlen wir nicht, die Sie im Produktionscode zu diesem Zeitpunkt verwenden. Diese APIs und Steuerelementen weiterhin, heranzureifen und stabilisieren in zukünftigen Versionen von Windows. Weitere Informationen zu XAML-Inseln finden Sie unter [UWP-Steuerelementen in desktop-Anwendungen](xaml-islands.md)

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Um eine XAML-basierte Benutzeroberfläche anzuzeigen, gehen Sie folgendermaßen vor:

: ein: [Richten Sie Ihrer Projektmappe ein](#solution-setup)

: zwei: [Erstellen Sie eine XAML-Benutzeroberfläche](#xaml-UI)

: drei: [Hinzufügen einer Protokoll-Erweiterung zum UWP-Projekt](#add-a-protocol-extension)

: vier: [Starten Sie die UWP-app über Ihre desktop-app](#start)

: fünf: [In der UWP-Projekt anzeigen der Seite, Sie möchten](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>Einrichten Ihrer Projektmappe

Allgemeine Anleitungen zum Einrichten Ihrer Projektmappe finden Sie im Abschnitt [Ihre Projektmappe einrichten](#setup) am Anfang dieses Handbuchs.

Ihre Projektmappe sieht in etwa wie folgt aus:

![Projektmappe für eine XAML-Benutzeroberfläche](images/desktop-to-uwp/xaml-ui-solution.png)

In diesem Beispiel heißt das Windows Forms-Projekt **Landmarks**, und das UWP-Projekt, das die XAML-Benutzeroberfläche enthält, heißt **MapUI**.

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>Erstellen einer XAML-Benutzeroberfläche

Fügen Sie Ihrem UWP-Projekt eine XAML-Benutzeroberfläche hinzu. Hier sehen Sie den XAML-Code für eine grundlegende Karte.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"
                     HorizontalAlignment="Left"
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>Fügen Sie eine Protokollerweiterung hinzu.

In **Projektmappen-Explorer**öffnen die **"Package.appxmanifest"** Datei das Packaging-Projekt in Ihrer Projektmappe, und fügen diese Erweiterung hinzu.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

Benennen Sie dem Protokoll einen Namen, geben Sie den Namen der ausführbaren Datei des UWP-Projekts an und den Name der Einstiegspunkt-Klasse an.

Sie können auch das **Package.appxmanifest** im Designer öffnen, die **Deklarationen** Registerkarte wählen und dann dort die Erweiterung hinzufügen.

![Deklarationen-Registerkarte](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Kartensteuerelemente laden Daten aus dem Internet herunter. Wenn Sie eines verwenden, müssen Sie auch die Funktion „Internetclient“ in Ihrem Manifest hinzufügen.

<a id="start" />

### <a name="start-the-uwp-app"></a>Starten der UWP-App

Erstellen Sie zunächst in Ihrer Desktop-Anwendung eine [Uri](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN), die den Protokollnamen und alle Parameter enthält, die an die UWP-App übergeben werden sollen. Rufen Sie dann die [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)-Methode auf.

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>Analysieren von Parametern und Anzeigen einer Seite

In der **App** Klasse des UWP-Projekts überschreiben den **OnActivated**-Ereignishandler. Wenn die App durch Ihr Protokoll aktiviert wird, analysieren Sie die Parameter und öffnen Sie die Seite, die Sie benötigen.

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

Überschreiben Sie in den Code hinter der XAML-Seite, die ``OnNavigatedTo`` Methode, um die Parameter in die Seite übergeben. In diesem Fall verwenden wir den Breiten- und Längengrad, die in diese Seite übergeben wurden, um einen Standort in einer Karte anzuzeigen.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

## <a name="making-your-desktop-application-a-share-target"></a>Ihre Desktopanwendung als Freigabeziel gestalten

Sie können Ihre Desktopanwendung als Freigabeziel einrichten, damit Benutzer einfach Daten wie Bilder aus anderen Apps freigeben können, die Freigaben unterstützen.

Beispielsweise können Benutzer Ihre Anwendung Bilder von Microsoft Edge, die Fotos-app freigeben. Hier ist eine WPF-beispielanwendung, die über diese Fähigkeit verfügt.

![Freigabeziel](images/desktop-to-uwp/share-target.png).

Finden Sie unter dem vollständigen Beispiel [hier](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Damit einer Anwendung als Freigabeziel arbeitet, führen Sie folgende Aktionen aus:

: ein: [Fügen Sie eine freigabeerweiterung für Ziel](#share-extension)

: zwei: [Überschreiben Sie den OnShareTargetActivated-Ereignishandler](#override)

: drei: [Hinzufügen von desktop-Erweiterungen zu UWP-Projekt](#desktop-extensions)

: vier: [Fügen Sie die volle Vertrauenswürdigkeit-Prozess-Erweiterung](#full-trust)

: fünf: [Ändern Sie die desktop-Anwendung beim Abrufen der freigegebenen Datei](#modify-desktop)

<a id="share-extension" />

Die folgenden Schritte aus  

### <a name="add-a-share-target-extension"></a>Hinzufügen der Freigabezielerweiterung

In **Projektmappen-Explorer**öffnen die **"Package.appxmanifest"** Datei der Verpackung-Projekt in der Projektmappe, und fügen Sie die freigabeerweiterung-Ziel hinzu.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Geben Sie den Namen der ausführbaren Datei des UWP-Projekts an und den Namen der Einstiegspunkt-Klasse. Dieses Markup wird davon ausgegangen, dass der Name der ausführbaren Datei für Ihre UWP-app `ShareTarget.exe`.

Sie müssen außerdem angeben, welche Arten von Dateien mit Ihrer App freigegeben können. In diesem Beispiel nehmen wir die [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) Desktopanwendung Freigabeziel für Bitmap images, daher wir geben `Bitmap` für den unterstützten Dateityp.

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>Überschreiben Sie den OnShareTargetActivated-Ereignishandler

Überschreiben der **OnShareTargetActivated** -Ereignishandler in der **App** Klasse von Ihr UWP-Projekt.

Dieser Ereignishandler wird aufgerufen, wenn Benutzer Ihre App zum Teilen von Dateien auswählen.

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

In diesem Code speichern wir das Bild, das vom Benutzer in einer lokalen Speicherordner apps freigegeben ist. Später ändern wir die Desktopanwendung aus demselben Ordner, Images per Pull abgerufen. Die desktop-Anwendung kann dies tun, da es im gleichen Paket wie die UWP-app enthalten ist.

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Hinzufügen von desktop-Erweiterungen zu UWP-Projekt

Hinzufügen der **Windows-Desktop-Erweiterungen für die UWP** Erweiterung für das UWP-app-Projekt.

![Desktop-Erweiterung](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>Fügen Sie die volle Vertrauenswürdigkeit-Prozess-Erweiterung

In **Projektmappen-Explorer**öffnen die **"Package.appxmanifest"** Datei das Packaging-Projekt in Ihrer Projektmappe, und fügen Sie dann auf die volle Vertrauenswürdigkeit Prozess Erweiterung neben die freigabeerweiterung Ziel, dass Sie dies hinzufügen die Datei weiter oben.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Diese Erweiterung ermöglicht, dass die UWP-app, um die desktop-Anwendung zu starten, mit der Sie die Freigabe einer Datei soll. Im Beispiel wir verweisen, an die ausführbare Datei von der [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) desktop-Anwendung.

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Ändern Sie die desktop-Anwendung beim Abrufen der freigegebenen Datei

Ändern Sie die desktop-Anwendung zum Suchen und die freigegebene Datei verarbeiten. In diesem Beispiel gespeichert, die UWP-app die freigegebene Datei im Ordner "lokale app-Daten". Aus diesem Grund ändern wir die [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) Desktopanwendung für Pull Fotos aus diesem Ordner.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

Für Instanzen der desktop-Anwendung, die bereits vom Benutzer zu öffnen, können wir auch behandeln die [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher?view=netframework-4.7.2) Ereignis und übergeben Sie den Pfad zum Speicherort Datei. Auf diese Weise werden alle geöffneten Instanzen von desktop-Anwendung die Fotofreigabe angezeigt.

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>Erstellen einer Hintergrundaufgabe

Sie fügen eine Hintergrundaufgaben hinzu, um selbst dann App-Code auszuführen, wenn die App angehalten wurde. Hintergrundaufgaben sind ideal für kleine Aufgaben, die keine Benutzerinteraktion erfordern. Beispielsweise kann Ihre Aufgabe E-Mails herunterladen, eine Popupbenachrichtigung über eine eingehende Chatnachricht zeigen oder auf eine Änderung in einer Systembedingung reagieren.

Hier ist eine WPF-beispielanwendung, die eine Hintergrundaufgabe registriert.

![hintergrundaufgabe](images/desktop-to-uwp/sample-background-task.png)

Die Aufgabe stellt eine HTTP-Anforderung und misst die Zeit, die die Anforderung benötigt, um eine Antwort zurückzugeben. Ihre Aufgaben werden wahrscheinlich viel interessanter sein, aber dieses Beispiel eignet sich gut, um die grundlegende Funktionsweise einer Hintergrundaufgabe zu lernen.

Finden Sie unter dem vollständigen Beispiel [hier](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Um einen Hintergrunddienst zu erstellen, gehen Sie folgendermaßen vor:

: ein: [Implementieren Sie die Hintergrundaufgabe](#implement-task)

: zwei: [Konfigurieren Sie die Hintergrundaufgabe](#configure-background-task)

: drei: [Die Hintergrundaufgabe registrieren](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>Implementieren der Hintergrundaufgabe

Implementieren Sie die Hintergrundaufgabe, indem Sie einem Komponentenprojekt für Windows-Runtime Code hinzufügen.

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>Konfigurieren der Hintergrundaufgabe

Öffnen Sie im manifest-Designer, der **"Package.appxmanifest"** Datei das Packaging-Projekt in der Projektmappe.

Fügen Sie auf der Registerkarte **Deklarationen** eine **Hintergrundaufgaben**-Deklaration hinzu.

![Hintergrundaufgaben-Option](images/desktop-to-uwp/background-task-option.png)

Wählen Sie dann die gewünschten Eigenschaften aus. Unser Beispiel verwendet die **Timer**-Eigenschaft.

![Time-Eigenschaft](images/desktop-to-uwp/timer-property.png)

Geben Sie den vollqualifizierten Namen der Klasse in der Komponente für Windows-Runtime, die die Hintergrundaufgabe implementiert.

![Time-Eigenschaft](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

Fügen Sie Ihrem Projekt für die Desktop-Anwendung, das die Hintergrundaufgabe registriert, Code hinzu.

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```

## <a name="support-and-feedback"></a>Support und Feedback

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
