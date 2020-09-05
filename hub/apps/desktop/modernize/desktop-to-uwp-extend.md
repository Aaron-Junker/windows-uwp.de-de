---
title: Erweitern deiner App mit Windows-Benutzeroberfläche und -Komponenten
description: Erweitern Sie Ihre Desktopanwendung mit UWP-Projekten und Windows-Runtime-Komponenten, um moderne Windows 10-Umgebungen hinzuzufügen.
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 29064390e4e198d1220d40ff5ce58a63ea41e29a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172784"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>Erweitern deiner Desktop-App mit modernen UWP-Komponenten

Einige Windows 10-Umgebungen (z. B. eine für Touchbedienung aktivierte Benutzeroberflächenseite) müssen sich in einem modernen App-Container befinden. Wenn du diese Umgebungen hinzufügen möchtest, erweitere deine Desktopanwendung mit UWP-Projekten und Windows-Runtime-Komponenten.

In vielen Fällen kannst du die Windows-Runtime-APIs direkt in deiner Desktopanwendung aufrufen. Bevor du diese Anleitung liest, siehe [Verbessern für Windows 10](desktop-to-uwp-enhance.md).

> [!NOTE]
> Für die im vorliegenden Artikel beschriebenen Features muss deine Desktop-App eine [Paketidentität](modernize-packaged-apps.md) aufweisen, entweder durch das [Packen deiner Desktop-App in einem MSIX-Paket](/windows/msix/desktop/desktop-to-uwp-root) oder durch das [Gewähren einer App-Identität durch Verwendung eines Pakets mit geringer Datendichte](grant-identity-to-nonpackaged-apps.md).

Wenn du bereit bist, lass uns anfangen.

<a id="setup"></a>

## <a name="first-setup-your-solution"></a>Richte als Erstes deine Projektmappe ein.

Füge deiner Projektmappe ein oder mehrere UWP-Projekte und Laufzeitkomponenten hinzu.

Beginne mit einer Projektmappe, die ein **Paketerstellungsprojekt für Windows-Anwendungen** mit einem Verweis auf deine Desktopanwendung enthält.

Diese Abbildung zeigt ein Beispiel einer Projektmappe.

![Erweitern des Startprojekts](images/desktop-to-uwp/extend-start-project.png)

Wenn deine Projektmappe kein Paketerstellungsprojekt enthält, siehe [Packen deiner Desktopanwendung mit Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

### <a name="configure-the-desktop-application"></a>Konfigurieren der Desktopanwendung

Stelle sicher, dass deine Desktopanwendung Verweise auf die Dateien aufweist, die du für den Aufruf der Windows-Runtime-APIs benötigst.

Informationen hierzu findest du im Abschnitt [Einrichten deines Projekts](desktop-to-uwp-enhance.md#set-up-your-project).

### <a name="add-a-uwp-project"></a>Hinzufügen eines UWP-Projekts

Füge deiner Projektmappe eine **Leere App (Universelle Windows-App)** hinzu.

Hier erstellst du eine moderne XAML-Benutzeroberfläche oder verwendest APIs, die nur innerhalb eines UWP-Prozesses ausgeführt werden.

![UWP-Projekt](images/desktop-to-uwp/add-uwp-project-to-solution.png)

Klicke in deinem Paketerstellungsprojekt mit der rechten Maustaste auf den Knoten **Anwendungen**, und klicke dann auf **Verweis hinzufügen**.

![Verweis auf UWP-Projekt](images/desktop-to-uwp/add-uwp-project-reference.png)

Füge dann einen Verweis auf das UWP-Projekt hinzu.

![Verweis auf UWP-Projekt](images/desktop-to-uwp/choose-uwp-project.png)

Deine Projektmappe sieht etwa wie folgt aus:

![Projektmappe mit UWP-Projekt](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Optional) Hinzufügen einer Windows-Runtime-Komponente

Um einige Szenarien zu realisieren, musst du einer Windows-Runtime-Komponente Code hinzufügen.

![Laufzeitkomponente für App-Dienst](images/desktop-to-uwp/add-runtime-component.png)

Füge dann in deinem UWP-Projekt einen Verweis auf die Laufzeitkomponente hinzu. Deine Projektmappe sieht etwa wie folgt aus:

![Verweis auf die Laufzeitkomponente](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>Kompilieren der Projektmappe

Kompiliere deine Projektmappe, um sicherzustellen, dass keine Fehler angezeigt werden. Wenn Fehler auftreten, öffne den **Konfigurationsmanager** und stelle sicher, dass deine Projekte auf dieselbe Plattform ausgerichtet sind.

![Konfigurations-Manager](images/desktop-to-uwp/config-manager.png)

Wirf einen Blick auf einige Dinge, die du mit deinen UWP-Projekten und Laufzeitkomponenten tun kannst.

## <a name="show-a-modern-xaml-ui"></a>Anzeigen einer modernen XAML-Benutzeroberfläche

Als Teil deines Anwendungsflusses kannst du moderne XAML-basierte Benutzeroberflächen in deine Desktopanwendung integrieren. Diese Benutzeroberflächen passen sich natürlich an unterschiedliche Bildschirmgrößen und Auflösungen an und unterstützen moderne, interaktive Modelle, z. B. Touch- und Freihandeingaben.

Mit etwas XAML-Markup kannst du z. B. Benutzern leistungsstarke Kartenvisualisierungsfunktionen bereitstellen.

Diese Abbildung zeigt eine Windows Forms-Anwendung, die eine XAML-basierte, moderne Benutzeroberfläche mit einem Kartensteuerelement öffnet.

![Adaptives Design](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>Dieses Beispiel zeigt eine XAML-Benutzeroberfläche durch Hinzufügen eines UWP-Projekts zur Projektmappe. Dies ist der zuverlässige unterstützte Ansatz, um XAML-Benutzeroberflächen in einer Desktopanwendung anzuzeigen. Die Alternative zu diesem Ansatz besteht darin, der Desktopanwendung UWP-XAML-Steuerelemente mithilfe einer XAML Island direkt hinzuzufügen. XAML Islands sind derzeit als Entwicklervorschau verfügbar. Obwohl wir Sie ermutigen möchten, diese jetzt in Ihrem eigenen Prototypencode zu testen, empfehlen wir Ihnen nicht, sie zu diesem Zeitpunkt im Produktionscode zu verwenden. Diese APIs und Steuerelemente werden in künftigen Windows-Releases weiter ausreifen und stabilisiert. Weitere Informationen zu XAML Islands findest du unter [UWP-Steuerelemente in Desktopanwendungen](xaml-islands.md).

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Um eine XAML-basierte Benutzeroberfläche anzuzeigen, gehe so vor:

:one: [Einrichten deiner Projektmappe](#solution-setup)

:two: [Erstellen einer XAML-Benutzeroberfläche](#xaml-UI)

:three: [Hinzufügen einer Protokollerweiterung zum UWP-Projekt](#add-a-protocol-extension)

:four: [Starten der UWP-App aus Ihrer Desktop-App](#start)

:five: [Anzeigen der gewünschten Seite im UWP-Projekt](#parse)

<a id="solution-setup"></a>

### <a name="setup-your-solution"></a>Einrichten deiner Projektmappe

Allgemeine Anleitungen zum Einrichten deiner Projektmappe findest du im Abschnitt [Richte als Erstes deine Projektmappe ein](#setup) am Anfang dieser Anleitung.

Deine Projektmappe sieht in etwa wie folgt aus:

![Projektmappe für eine XAML-Benutzeroberfläche](images/desktop-to-uwp/xaml-ui-solution.png)

In diesem Beispiel heißt das Windows Forms-Projekt **Landmarks**. Das UWP-Projekt, das die XAML-Benutzeroberfläche enthält, heißt **MapUI**.

<a id="xaml-UI"></a>

### <a name="create-a-xaml-ui"></a>Erstellen einer XAML-Benutzeroberfläche

Füge deinem UWP-Projekt eine XAML-Benutzeroberfläche hinzu. Hier ist der XAML-Code für eine einfache Karte.

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

### <a name="add-a-protocol-extension"></a>Hinzufügen einer Protokollerweiterung

Öffne im **Projektmappen-Explorer** die Datei **package.appxmanifest** des Paketerstellungsprojekts in deiner Projektmappe, und füge diese Erweiterung hinzu.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

Versieh das Protokoll mit einem Namen. Gib den Namen der ausführbaren Datei des UWP-Projekts und den der Einstiegspunktklasse an.

Du kannst auch **Package.appxmanifest** im Designer öffnen, die Registerkarte **Deklarationen** wählen und dann dort die Erweiterung hinzufügen.

![Registerkarte „Deklarationen“](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Kartensteuerelemente laden Daten aus dem Internet herunter. Wenn du eines verwendest, musst du deinem Manifest auch die Funktion „Internetclient“ hinzufügen.

<a id="start"></a>

### <a name="start-the-uwp-app"></a>Starten der UWP-App

Erstelle zunächst in deiner Desktopanwendung einen [Uri](/dotnet/api/system.uri), der den Protokollnamen und alle Parameter enthält, die an die UWP-App übergeben werden sollen. Rufe dann die [LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync)-Methode auf.

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

<a id="parse"></a>

### <a name="parse-parameters-and-show-a-page"></a>Analysieren von Parametern und Anzeigen einer Seite

Überschreibe in der **App**-Klasse des UWP-Projekts den **OnActivated**-Ereignishandler. Wenn die App durch dein Protokoll aktiviert wird, analysiere die Parameter und öffne die Seite, die du benötigst.

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

Überschreibe im Code hinter deiner XAML-Seite die Methode ``OnNavigatedTo``, um die an die Seite übergebenen Parameter zu verwenden. In diesem Fall verwenden wir den Breiten- und Längengrad, die an diese Seite übergeben wurden, um einen Standort auf einer Karte anzuzeigen.

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

## <a name="making-your-desktop-application-a-share-target"></a>Festlegen deiner Desktopanwendung als Freigabeziel

Du kannst deine Desktopanwendung als Freigabeziel einrichten, damit Benutzer einfach Daten wie Bilder aus anderen Apps freigeben können, die Freigaben unterstützen.

Beispielsweise können Benutzer deine App zum Teilen von Bildern in Microsoft Edge in der App „Fotos“ auswählen. Hier ist eine WPF-Beispielanwendung, die diese Funktion nutzt.

![Freigabeziel](images/desktop-to-uwp/share-target.png).

Das vollständige Beispiel findest du [hier](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget).

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Um deine Anwendung zu einem Freigabeziel zu machen, gehe wie folgt vor:

:one: [Hinzufügen der Freigabezielerweiterung](#share-extension)

:two: [Überschreiben des Ereignishandlers OnShareTargetActivated](#override)

:three: [Hinzufügen von Desktoperweiterungen zum UWP-Projekt](#desktop-extensions)

:four: [Hinzufügen der voll vertrauenswürdigen Prozesserweiterung](#full-trust)

:five: [Ändern der Desktopanwendung zum Abrufen der freigegebenen Datei](#modify-desktop)

<a id="share-extension"></a>

Die folgenden Schritte  

### <a name="add-a-share-target-extension"></a>Hinzufügen der Freigabezielerweiterung

Öffne im **Projektmappen-Explorer** die Datei **package.appxmanifest** des Paketerstellungsprojekts in deiner Projektmappe, und füge diese Freigabezielerweiterung hinzu.

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

Gib den Namen der ausführbaren Datei des UWP-Projekts und der Einstiegspunktklasse an. Dieses Markup geht davon aus, dass der Name der ausführbaren Datei für die UWP-App `ShareTarget.exe` ist.

Du musst außerdem angeben, welche Arten von Dateien mit deiner App freigegeben werden können. In diesem Beispiel machen wir die Desktopanwendung [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) zu einem Freigabeziel für Bitmapbilder. Daher geben wir `Bitmap` als unterstützten Dateityp an.

<a id="override"></a>

### <a name="override-the-onsharetargetactivated-event-handler"></a>Überschreiben des Ereignishandlers OnShareTargetActivated

Überschreibe in der **App**-Klasse deines UWP-Projekts den Ereignishandler **OnShareTargetActivated**.

Dieser Ereignishandler wird aufgerufen, wenn Benutzer deine App zum Teilen ihrer Dateien auswählen.

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

In diesem Code speichern wir das Bild, das vom Benutzer freigegeben wird, in einem lokalen Speicherordner der Anwendung. Zu einem späteren Zeitpunkt ändern wir die Desktopanwendung so, dass Bilder aus demselben Ordner abgerufen werden. Die Desktopanwendung kann das leisten, da sie im gleichen Paket wie die UWP-Anwendung enthalten ist.

<a id="desktop-extensions"></a>

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Hinzufügen von Desktoperweiterungen zum UWP-Projekt

Füge die Erweiterung **Windows Desktop Extensions for the UWP** deinem UWP-App-Projekt hinzu.

![Desktoperweiterung](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust"></a>

### <a name="add-the-full-trust-process-extension"></a>Hinzufügen der voll vertrauenswürdigen Prozesserweiterung

Öffne im **Projektmappen-Explorer** die Datei **package.appxmanifest** des Paketerstellungsprojekts in deiner Projektmappe, und füge dann die voll vertrauenswürdige Prozesserweiterung neben der Freigabezielerweiterung hinzu, die du dieser Datei zuvor hinzugefügt hast.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Diese Erweiterung ermöglicht es der UWP-Anwendung, die Desktopanwendung zu starten, für die du eine Datei freigeben möchtest. In diesem Beispiel verweisen wir auf die ausführbare Datei der Desktopanwendung [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo).

<a id="modify-desktop"></a>

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Ändern der Desktopanwendung zum Abrufen der freigegebenen Datei

Ändere deine Desktopanwendung so, dass die freigegebene Datei gefunden und verarbeitet wird. In diesem Beispiel hat die UWP-App die freigegebene Datei im lokalen Datenordner der App gespeichert. Daher ändern wir die Desktopanwendung [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) so, dass Fotos aus diesem Ordner abgerufen werden können.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

Für Instanzen der Desktopanwendung, die bereits vom Benutzer geöffnet sind, könnten wir auch das [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher)-Ereignis behandeln und den Pfad zum Speicherort der Datei übergeben. Auf diese Weise wird in allen geöffneten Instanzen der Desktopanwendung das freigegebene Foto angezeigt.

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

Du fügst eine Hintergrundaufgaben hinzu, um selbst dann App-Code auszuführen, wenn die App angehalten wurde. Hintergrundaufgaben sind ideal für kleine Aufgaben, die keine Benutzerinteraktion erfordern. Beispielsweise kann deine Aufgabe E-Mails herunterladen, eine Popupbenachrichtigung über eine eingehende Chatnachricht zeigen oder auf eine Änderung in einer Systembedingung reagieren.

Hier ist ein Beispiel einer WPF-App, die eine Hintergrundaufgabe registriert.

![Hintergrundaufgabe](images/desktop-to-uwp/sample-background-task.png)

Die Aufgabe stellt eine HTTP-Anforderung und misst die Zeit, die die Anforderung benötigt, um eine Antwort zurückzugeben. Deine Aufgaben werden wahrscheinlich viel interessanter sein, aber dieses Beispiel eignet sich gut, um die grundlegende Funktionsweise einer Hintergrundaufgabe kennenzulernen.

Das vollständige Beispiel findest du [hier](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

### <a name="the-design-pattern"></a>Das Entwurfsmuster

Um einen Hintergrunddienst zu erstellen, gehe folgendermaßen vor:

:one: [Implementieren der Hintergrundaufgabe](#implement-task)

:two: [Konfigurieren der Hintergrundaufgabe](#configure-background-task)

:three: [Registrieren der Hintergrundaufgabe](#register-background-task)

<a id="implement-task"></a>

### <a name="implement-the-background-task"></a>Implementieren der Hintergrundaufgabe

Implementiere die Hintergrundaufgabe, indem du einem Windows-Runtime-Komponentenprojekt Code hinzufügst.

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

<a id="configure-background-task"></a>

### <a name="configure-the-background-task"></a>Konfigurieren der Hintergrundaufgabe

Öffne im Manifest-Designer die Datei **package.appxmanifest** des Paketerstellungsprojekts in deiner Projektmappe.

Füge auf der Registerkarte **Deklarationen** die Deklaration **Hintergrundaufgaben** hinzu.

![Option „Hintergrundaufgaben“](images/desktop-to-uwp/background-task-option.png)

Wähle dann die gewünschten Eigenschaften aus. Unser Beispiel verwendet die **Timer**-Eigenschaft.

![Timer-Eigenschaft](images/desktop-to-uwp/timer-property.png)

Gib den vollqualifizierten Namen der Klasse in der Windows-Runtime-Komponente an, die die Hintergrundaufgabe implementiert.

![Timer-Eigenschaft](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task"></a>

### <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

Füge deinem Desktopanwendungsprojekt Code hinzu, der die Hintergrundaufgabe registriert.

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

## <a name="find-answers-to-your-questions"></a>Antworten auf deine Fragen

Haben Sie Fragen? Frage uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Du kannst uns auch [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D) fragen.