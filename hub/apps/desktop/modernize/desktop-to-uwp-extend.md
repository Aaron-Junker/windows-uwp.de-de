---
Description: Erweitern Sie Ihre Desktopanwendung mit Windows-Benutzeroberflächen und -Komponenten
title: Erweitern Ihrer APP mit der Windows-Benutzeroberfläche und-Komponenten
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: a38f5fa7f3ef99f5970ec5d476fb65761aa39db4
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302584"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>Erweitern Ihrer Desktop-App mit modernen UWP-Komponenten

Einige Windows 10-Umgebungen (z. B. eine touch-fähige UI-Seite) müssen sich in einem Modern-App-Container befinden. Wenn Sie diese Erfahrungen hinzufügen möchten, erweitern Sie Ihre Desktop Anwendung mit UWP-Projekten und Windows-Runtime Komponenten.

In vielen Fällen können Sie Windows-Runtime-APIs direkt von der Desktop Anwendung aus anrufen. bevor Sie dieses Handbuch lesen, finden Sie weitere Informationen unter [verbessern für Windows 10](desktop-to-uwp-enhance.md).

> [!NOTE]
> Die in diesem Artikel beschriebenen Funktionen erfordern, dass Ihre Desktop-App über die [Paket Identität](modernize-packaged-apps.md)verfügt, indem [Sie entweder Ihre Desktop-app in einem msix-Paket Verpacken](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) oder [Ihre APP-Identität mithilfe eines sparsepakets gewähren](grant-identity-to-nonpackaged-apps.md).

Wenn Sie bereit sind, lassen Sie uns starten.

<a id="setup" />

## <a name="first-setup-your-solution"></a>Ihre Projektmappe einrichten

Fügen Sie Ihrer Projektmappe ein oder mehrere UWP-Projekte und Laufzeitkomponenten hinzu.

Beginnen Sie mit einer Projektmappe, die ein **Paketerstellungsprojekt für Windows-Anwendungen** mit einem Verweis auf Ihre Desktop-Anwendung enthält.

Diese Abbildung zeigt ein Beispiel für eine Projektmappe.

![Erweitern des Startprojekts](images/desktop-to-uwp/extend-start-project.png)

Wenn Ihre Lösung kein Paket Projekt enthält, finden Sie weitere Informationen unter [Verpacken der Desktop Anwendung mithilfe von Visual Studio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

### <a name="configure-the-desktop-application"></a>Konfigurieren der Desktop Anwendung

Stellen Sie sicher, dass Ihre Desktop Anwendung Verweise auf die Dateien enthält, die Sie benötigen, um Windows-Runtime-APIs aufzurufen.

Informationen hierzu finden Sie im Abschnitt [Einrichten des Projekts](desktop-to-uwp-enhance.md#set-up-your-project) .

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

### <a name="optional-add-a-windows-runtime-component"></a>Optionale Hinzufügen einer Windows-Runtime Komponente

Um einige Szenarios zu erreichen, müssen Sie einer Windows-Runtime Komponente Code hinzufügen.

![Runtime-Komponente App-Dienst](images/desktop-to-uwp/add-runtime-component.png)

Fügen Sie dann in Ihrem UWP-Projekt einen Verweis auf die Laufzeitkomponente hinzu. Ihre Projektmappe sieht etwa wie folgt aus:

![Verweis auf die Laufzeitkomponente](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>Erstellen der Projekt Mappe

Erstellen Sie die Lösung, um sicherzustellen, dass keine Fehler angezeigt werden. Wenn Sie Fehler erhalten, öffnen Sie **Configuration Manager** , und stellen Sie sicher, dass die Projekte auf dieselbe Plattform abzielen.

![Konfigurations-Manager](images/desktop-to-uwp/config-manager.png)

Werfen wir einen Blick auf einige Dinge, die Sie mit Ihren UWP-Projekten und Laufzeitkomponenten tun können.

## <a name="show-a-modern-xaml-ui"></a>Anzeigen einer modernen XAML-Benutzeroberfläche

Als Teil Ihres Anwendungsflusses können Sie moderne XAML-basierte Benutzeroberflächen in Ihre Desktopanwendung integrieren. Diese Benutzeroberflächen passen sich natürlich an unterschiedliche Bildschirmgrößen und Auflösungen an und unterstützen moderne, interaktive Modelle, z. B. Touch- und Freihandeingaben.

Mit etwas XAML-Markup-Code können Sie z. B. Benutzern leistungsstarke Kartenvisualisierungsfunktionen bereitstellen.

Diese Abbildung zeigt eine Windows Forms-Anwendung, die eine XAML-basierte, moderne Benutzeroberfläche öffnet, die ein Kartensteuerelement enthält.

![adaptives Design](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>Dieses Beispiel zeigt eine XAML-Benutzeroberfläche durch Hinzufügen eines UWP-Projekts zur Projekt Mappe. Dies ist der stabile unterstützte Ansatz, um XAML-Benutzeroberflächen in einer Desktop Anwendung anzuzeigen. Die Alternative zu diesem Ansatz besteht darin, der Desktop Anwendung mithilfe einer XAML-Insel direkt UWP-XAML-Steuerelemente hinzuzufügen. XAML-Inseln sind zurzeit als Entwicklervorschau verfügbar. Obwohl wir Sie ermutigen möchten, diese jetzt in Ihrem eigenen Prototypencode zu testen, empfehlen wir Ihnen nicht, sie zu diesem Zeitpunkt im Produktionscode zu verwenden. Diese APIs und Steuerelemente werden in zukünftigen Windows-Releases weiterhin ausgereift und stabilisiert. Weitere Informationen zu XAML-Inseln finden Sie unter [UWP-Steuerelemente in Desktop Anwendungen](xaml-islands.md) .

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Um eine XAML-basierte Benutzeroberfläche anzuzeigen, gehen Sie folgendermaßen vor:

:one: [Einrichten Ihrer Projektmappe](#solution-setup)

:two: [Erstellen einer XAML-Benutzeroberfläche](#xaml-UI)

:three: [Hinzufügen einer Protokollerweiterung zum UWP-Projekt](#add-a-protocol-extension)

:four: [Starten der UWP-App aus Ihrer Desktop-App](#start)

:five: [Anzeigen der gewünschten Seite im UWP-Projekt](#parse)

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

Öffnen Sie in **Projektmappen-Explorer**die Datei " **Package. appxmanifest** " des Verpackungs Projekts in ihrer Projekt Mappe, und fügen Sie diese Erweiterung hinzu.

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

Erstellen Sie zunächst in Ihrer Desktop-Anwendung eine [Uri](https://docs.microsoft.com/dotnet/api/system.uri), die den Protokollnamen und alle Parameter enthält, die an die UWP-App übergeben werden sollen. Rufen Sie dann die [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)-Methode auf.

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

Überschreiben Sie im Code hinter der XAML-Seite die ``OnNavigatedTo``-Methode, um die an die Seite übergebenen Parameter zu verwenden. In diesem Fall verwenden wir den Breiten- und Längengrad, die in diese Seite übergeben wurden, um einen Standort in einer Karte anzuzeigen.

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

Beispielsweise können Benutzer Ihre Anwendung zum Freigeben von Bildern von Microsoft Edge, der Fotos-App auswählen. Hier ist eine WPF-Beispielanwendung, die diese Funktion enthält.

![Freigabeziel](images/desktop-to-uwp/share-target.png).

Das komplette Beispiel finden Sie [hier](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget) .

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Damit einer Anwendung als Freigabeziel arbeitet, führen Sie folgende Aktionen aus:

:one: [Hinzufügen der Freigabezielerweiterung](#share-extension)

: zwei: über [Schreiben des onsharetargetaktivierten Ereignis Handlers](#override)

: 3: [Hinzufügen von Desktop Erweiterungen zum UWP-Projekt](#desktop-extensions)

: vier: [Hinzufügen der voll vertrauenswürdigen Prozess Erweiterung](#full-trust)

: 5: [Ändern der Desktop Anwendung zum erhalten der freigegebenen Datei](#modify-desktop)

<a id="share-extension" />

Die folgenden Schritte  

### <a name="add-a-share-target-extension"></a>Hinzufügen der Freigabezielerweiterung

Öffnen Sie in **Projektmappen-Explorer**die Datei " **Package. appxmanifest** " des Verpackungs Projekts in ihrer Projekt Mappe, und fügen Sie die Freigabe Ziel Erweiterung hinzu.

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

Geben Sie den Namen der ausführbaren Datei des UWP-Projekts an und den Namen der Einstiegspunkt-Klasse. Dieses Markup geht davon aus, dass der Name der ausführbaren Datei für die UWP-App `ShareTarget.exe`ist.

Sie müssen außerdem angeben, welche Arten von Dateien mit Ihrer App freigegeben können. In diesem Beispiel erstellen wir die [WPF-photostoredemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) -Desktop Anwendung als Freigabe Ziel für Bitmapbilder, sodass wir `Bitmap` für den unterstützten Dateityp angeben.

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>Überschreiben des onsharetargetaktivierten Ereignis Handlers

Überschreiben Sie den **onsharetargetaktivierten** Ereignishandler in der **App** -Klasse Ihres UWP-Projekts.

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

In diesem Code speichern wir das Image, das für den Benutzer freigegeben wird, in einem lokalen app-Speicherordner. Zu einem späteren Zeitpunkt ändern wir die Desktop Anwendung, um Bilder aus demselben Ordner abzurufen. Die Desktop Anwendung kann dies tun, da Sie im gleichen Paket wie die UWP-App enthalten ist.

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Hinzufügen von Desktop Erweiterungen zum UWP-Projekt

Fügen Sie dem UWP-App-Projekt die **Windows-Desktop Erweiterungen für die UWP-** Erweiterung hinzu.

![Desktop Erweiterung](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>Hinzufügen der voll vertrauenswürdigen Prozess Erweiterung

Öffnen Sie in **Projektmappen-Explorer**die Datei " **Package. appxmanifest** " des Verpackungs Projekts in der Projekt Mappe, und fügen Sie dann die voll vertrauenswürdige Prozess Erweiterung neben der Freigabe Ziel Erweiterung hinzu, die Sie zuvor hinzugefügt haben.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Diese Erweiterung ermöglicht der UWP-APP das Starten der Desktop Anwendung, für die Sie die Datei freigeben möchten. Wir verweisen beispielsweise auf die ausführbare Datei der [WPF photostoredemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) -Desktop Anwendung.

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Ändern der Desktop Anwendung zum erhalten der freigegebenen Datei

Ändern Sie Ihre Desktop Anwendung, um die freigegebene Datei zu suchen und zu verarbeiten. In diesem Beispiel speichert die UWP-APP die freigegebene Datei im Ordner "local App Data". Daher ändern wir die WPF-Desktop Anwendung " [photostoredemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) " so, dass Sie Fotos aus diesem Ordner abrufen kann.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

Für Instanzen der Desktop Anwendung, die bereits vom Benutzer geöffnet wurden, können wir auch das [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher) -Ereignis behandeln und den Pfad zum Speicherort der Datei übergeben. Auf diese Weise wird für alle geöffneten Instanzen der Desktop Anwendung das freigegebene Foto angezeigt.

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

Hier ist eine WPF-Beispielanwendung, die eine Hintergrundaufgabe registriert.

![hintergrundaufgabe](images/desktop-to-uwp/sample-background-task.png)

Die Aufgabe stellt eine HTTP-Anforderung und misst die Zeit, die die Anforderung benötigt, um eine Antwort zurückzugeben. Ihre Aufgaben werden wahrscheinlich viel interessanter sein, aber dieses Beispiel eignet sich gut, um die grundlegende Funktionsweise einer Hintergrundaufgabe zu lernen.

Weitere Informationen finden Sie [hier](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Um einen Hintergrunddienst zu erstellen, gehen Sie folgendermaßen vor:

:one: [Implementieren der Hintergrundaufgabe](#implement-task)

:two: [Konfigurieren der Hintergrundaufgabe](#configure-background-task)

:three: [Registrieren der Hintergrundaufgabe](#register-background-task)

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

Öffnen Sie im Manifest-Designer die Datei " **Package. appxmanifest** " des Verpackungs Projekts in der Projekt Mappe.

Fügen Sie auf der Registerkarte **Deklarationen** eine **Hintergrundaufgaben**-Deklaration hinzu.

![Hintergrundaufgaben-Option](images/desktop-to-uwp/background-task-option.png)

Wählen Sie dann die gewünschten Eigenschaften aus. Unser Beispiel verwendet die **Timer**-Eigenschaft.

![Time-Eigenschaft](images/desktop-to-uwp/timer-property.png)

Geben Sie den voll qualifizierten Namen der Klasse in der Windows-Runtime-Komponente an, die die Hintergrundaufgabe implementiert.

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

## <a name="find-answers-to-your-questions"></a>Finden Sie Antworten auf Ihre Fragen

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
