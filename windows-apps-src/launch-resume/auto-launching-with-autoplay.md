---
title: Automatisches Starten mit automatischer Wiedergabe
description: Sie können die automatische Wiedergabe verwenden, um Ihre App als Option bereitzustellen, wenn ein Benutzer ein Gerät an seinen PC anschließt. Hierzu zählen andere Geräte als Volumegeräte, wie Kameras oder Medienplayer, oder Volumegeräte wie USB-Sticks, SD-Karten oder DVDs.
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f49d70ceadad8309419846aa26cb9f97df1c82ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620895"
---
# <a name="span-iddevlaunchresumeauto-launchingwithautoplayspanauto-launching-with-autoplay"></a><span id="dev_launch_resume.auto-launching_with_autoplay"></span>Mit der Wiedergabe automatisch starten

Sie können die **automatische Wiedergabe** verwenden, um Ihre App als Option bereitzustellen, wenn ein Benutzer ein Gerät an seinen PC anschließt. Hierzu zählen andere Geräte als Volumegeräte, wie Kameras oder Medienplayer, oder Volumegeräte wie USB-Sticks, SD-Karten oder DVDs. Die **automatische Wiedergabe** bietet Ihnen auch die Möglichkeit, Ihre App als Option anzubieten, wenn Benutzer mithilfe von Näherung (Kopplung) Dateien zwischen zwei PCs freigeben.

> **Beachten Sie**  Wenn Sie ein Gerätehersteller und zugeordnet werden soll Ihre [Microsoft Store-Geräte-app](https://go.microsoft.com/fwlink/p/?LinkID=301381) als ein **AutoPlay** Handler für das Gerät, können Sie die app im Identifizieren der die Metadaten des Geräts. Weitere Informationen finden Sie im Thema [Automatische Wiedergabe für Microsoft Store-Geräte-Apps](https://go.microsoft.com/fwlink/p/?LinkId=306684).

## <a name="register-for-autoplay-content"></a>Registrieren für Inhalt für die automatische Wiedergabe

Sie können Apps als Optionen für Inhaltsereignisse für die **automatische Wiedergabe** registrieren. Inhaltsereignisse der **automatischen Wiedergabe** werden ausgelöst, wenn ein Volumegerät wie etwa die Speicherkarte einer Kamera, eine DVD oder ein USB-Stick in den PC eingelegt bzw. daran angeschlossen wird. Dieses Beispiel zeigt, wie Sie eine App als Option für die **automatische Wiedergabe** identifizieren, wenn ein Volumegerät einer Kamera angeschlossen wird.

In diesem Lernprogramm haben Sie eine App erstellt, die Bilddateien anzeigt oder in „Bilder“ kopiert. Sie haben die App für das Inhaltsereignis **ShowPicturesOnArrival** der automatischen Wiedergabe registriert.

Die automatische Wiedergabe löst außerdem Inhaltsereignisse für Inhalte aus, die mittels Näherung (Koppeln) zwischen PCs freigegeben werden. Sie können die Schritte und den Code aus diesem Abschnitt auch für Dateien verwenden, die mittels Näherung zwischen PCs freigegeben werden. Die folgende Tabelle enthält die Inhaltsereignisse für die automatische Wiedergabe, die Sie verwenden können, um Inhalte mithilfe der Näherungsfunktion zu teilen.

| Aktion         | Inhaltsereignis für die automatische Wiedergabe  |
|----------------|-------------------------|
| Teilen von Musik  | PlayMusicFilesOnArrival |
| Teilen von Videos | PlayVideoFilesOnArrival |

 
Wenn Dateien mithilfe der Näherungsfunktion freigegeben werden, enthält die **Files**-Eigenschaft des **FileActivatedEventArgs**-Objekts einen Verweis auf einen Stammordner mit allen freigegebenen Dateien.

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Schritt 1: Erstellen eines neuen Projekts, und fügen Sie die automatische Wiedergabe-Deklarationen

1.  Öffnen Sie Microsoft Visual Studio, und wählen Sie **Neues Projekt** im Menü **Datei** aus. Wählen Sie im Abschnitt **Visual C#** unter **Windows** die Option **Leere App (Universelle Windows-App)** aus. Nennen Sie die App **AutoPlayDisplayOrCopyImages**, und klicken Sie auf **OK.**
2.  Öffnen Sie die Datei „Package.appxmanifest“, und wählen Sie die Registerkarte **Funktionen** aus. Wählen Sie die Funktionen **Wechselmedien** und **Bildbibliothek** aus. Dadurch erhält die App Zugriff auf Wechselmedien für Kameraspeicher und lokale Bilder.
3.  Klicken Sie in der Manifestdatei auf die Registerkarte **Deklarationen**. Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Inhalt automatisch wiedergeben** aus, und klicken Sie anschließend auf **Hinzufügen05**. Wählen Sie das neue Element vom Typ **Inhalt automatisch wiedergeben** aus, das der Liste **Unterstützte Deklarationen** hinzugefügt wurde.
4.  Eine Deklaration vom Typ **Inhalt automatisch wiedergeben** identifiziert Ihre App als Option, wenn die automatische Wiedergabe ein Inhaltsereignis auslöst. Das Ereignis basiert auf dem Inhalt eines Volumegeräts (beispielsweise eine DVD oder ein Speicherstick). Für die automatische Wiedergabe wird der Inhalt des Volumegeräts ermittelt und festgelegt, welches Inhaltsereignis ausgelöst wird. Enthält das Stammverzeichnis des Volumes ein DCIM, AVCHD oder eine PRIVATE\\ACHD-Ordner, oder wenn ein Benutzer aktiviert hat **auswählen, was Sie mit jeder Art von Medien** in der Systemsteuerung für die automatische Wiedergabe und Bilder befinden sich im Stammverzeichnis des Volumes, klicken Sie dann Automatische Wiedergabe löst die **ShowPicturesOnArrival** Ereignis. Geben Sie im Abschnitt **Startaktionen** die Werte aus der nachfolgenden Tabelle 1 für die Aktion beim ersten Start an.
5.  Klicken Sie im Abschnitt **Startaktionen** für das Element vom Typ **Inhalt automatisch wiedergeben** auf **Neu hinzufügen**, um eine zweite Startaktion hinzuzufügen. Geben Sie die Werte aus der nachfolgenden Tabelle 2 für die Aktion beim zweiten Start an:
6.  Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Dateitypzuordnungen** aus, und klicken Sie anschließend auf **Hinzufügen**. In den Eigenschaften der neuen **Dateitypzuordnungen** legen Sie die Deklaration der **Anzeigenamen** Feld **AutoPlay kopieren oder Bilder anzeigen** und **Namen** Feld **Image\_association1**. Klicken Sie im Abschnitt **Unterstützte Dateitypen** auf **Neu hinzufügen**. Legen Sie im Feld **Dateityp** die Option **.jpg** fest. Legen Sie im Abschnitt **Unterstützte Dateitypen** das Feld **Dateityp** der neuen Dateizuordnung auf **.png** fest. Für Inhaltsereignisse filtert die automatische Wiedergabe alle Dateitypen heraus, die nicht explizit Ihrer App zugeordnet sind.
7.  Speichern und schließen Sie die Manifestdatei.

**Tabelle 1**

| Einstellung             | Wert                 |
|---------------------|-----------------------|
| Verb                | show                  |
| Aktionsanzeigename | Bilder anzeigen         |
| Inhaltsereignis       | ShowPicturesOnArrival |

Die Einstellung **Aktionsanzeigename** gibt die Zeichenfolge an, die bei der automatischen Wiedergabe für Ihre App angezeigt wird. Die Einstellung **Verb** dient zum Angeben eines Werts, der für die ausgewählte Option an Ihre App übergeben wird. Sie können mehrere Startaktionen für Ereignisse der automatischen Wiedergabe angeben und mit der Einstellung **Verb** ermitteln, welche Option ein Benutzer für Ihre App ausgewählt hat. Für welche Option sich der Benutzer entschieden hat, erfahren Sie durch Überprüfen der **verb**-Eigenschaft der an die App übergebenen Startereignisargumente. Für die Einstellung **Verb** können Sie einen beliebigen Wert verwenden. Einzige Ausnahme ist **open**: Dieser Wert ist reserviert.

**Tabelle 2**  

| Einstellung             | Wert                      |
|--------------------:|----------------------------|
| Verb                | copy                       |
| Aktionsanzeigename | Bilder in die Bibliothek kopieren |
| Inhaltsereignis       | ShowPicturesOnArrival      |

### <a name="step-2-add-xaml-ui"></a>Schritt 2: Hinzufügen der XAML-Benutzeroberfläche

Öffnen Sie die Datei „MainPage.xaml“, und fügen Sie dem Standardabschnitt &lt;Grid&gt; folgenden XAML-Code hinzu.

```xml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap"
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top"
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### <a name="step-3-add-initialization-code"></a>Schritt 3: Fügen Sie Code zum Initialisieren der hinzu.

Der Code in diesem Schritt überprüft den Verb-Wert in der **Verb**-Eigenschaft. Dabei handelt es sich um eines der Startargumente, die beim **OnFileActivated**-Ereignis an die App übergeben werden. Anschließend ruft der Code entsprechend der vom Benutzer ausgewählten Option eine Methode auf. Beim Kameraspeicherereignis wird bei der automatischen Wiedergabe der Stammordner des Kameraspeichers an die App übergeben. Sie können diesen Ordner aus dem ersten Element der **Files**-Eigenschaft abrufen.

Öffnen Sie die Datei „App.xaml.cs“, und fügen Sie der **App**-Klasse den folgenden Code hinzu.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    if (args.Verb == "show")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call DisplayImages with root folder from camera storage.
        page.DisplayImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    if (args.Verb == "copy")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call CopyImages with root folder from camera storage.
        page.CopyImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    base.OnFileActivated(args);
}
```

> **Beachten Sie**  der `DisplayImages` und `CopyImages` Methoden werden in den folgenden Schritten hinzugefügt.

### <a name="step-4-add-code-to-display-images"></a>Schritt 4: Fügen Sie Code zum Anzeigen von Bildern

Fügen Sie in der Datei „MainPage.xaml.cs“ der **MainPage**-Klasse folgenden Code hinzu.

```cs
async internal void DisplayImages(Windows.Storage.StorageFolder rootFolder)
{
    // Display images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();
    for (int i = 0; i < fileList.Count; i++)
    {
        var file = (Windows.Storage.StorageFile)fileList[i];
        WriteMessageText(file.Name + "\n");
        DisplayImage(file, i);
    }
}

async private void DisplayImage(Windows.Storage.IStorageItem file, int index)
{
    try
    {
        var sFile = (Windows.Storage.StorageFile)file;
        Windows.Storage.Streams.IRandomAccessStream imageStream =
            await sFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
        Windows.UI.Xaml.Media.Imaging.BitmapImage imageBitmap =
            new Windows.UI.Xaml.Media.Imaging.BitmapImage();
        imageBitmap.SetSource(imageStream);
        var element = new Image();
        element.Source = imageBitmap;
        element.Height = 100;
        Thickness margin = new Thickness();
        margin.Top = index * 100;
        element.Margin = margin;
        FilesCanvas.Children.Add(element);
    }
    catch (Exception e)
    {
       WriteMessageText(e.Message + "\n");
    }
}

// Write a message to MessageBlock on the UI thread.
private Windows.UI.Core.CoreDispatcher messageDispatcher = Window.Current.CoreWindow.Dispatcher;

private async void WriteMessageText(string message, bool overwrite = false)
{
    await messageDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            if (overwrite)
                FilesBlock.Text = message;
            else
                FilesBlock.Text += message;
        });
}
```

### <a name="step-5-add-code-to-copy-images"></a>Schritt 5: Fügen Sie Code zum Kopieren von Bildern

Fügen Sie in der Datei „MainPage.xaml.cs“ der **MainPage**-Klasse folgenden Code hinzu.

```cs
async internal void CopyImages(Windows.Storage.StorageFolder rootFolder)
{
    // Copy images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();

    try
    {
        var folderName = "Images " + DateTime.Now.ToString("yyyy-MM-dd HHmmss");
        Windows.Storage.StorageFolder imageFolder = await
            Windows.Storage.KnownFolders.PicturesLibrary.CreateFolderAsync(folderName);

        foreach (Windows.Storage.IStorageItem file in fileList)
        {
            CopyImage(file, imageFolder);
        }
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy images.\n" + e.Message + "\n");
    }
}

async internal void CopyImage(Windows.Storage.IStorageItem file,
                              Windows.Storage.StorageFolder imageFolder)
{
    try
    {
        Windows.Storage.StorageFile sFile = (Windows.Storage.StorageFile)file;
        await sFile.CopyAsync(imageFolder, sFile.Name);
        WriteMessageText(sFile.Name + " copied.\n");
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy file.\n" + e.Message + "\n");
    }
}
```

### <a name="step-6-build-and-run-the-app"></a>Schritt 6: Erstellen und Ausführen der App

1.  Drücken Sie F5, um die App zu erstellen und bereitzustellen (im Debugmodus).
2.  Legen Sie eine Kameraspeicherkarte oder ein anderes Speichergerät einer Kamera in Ihren PC ein, um die App auszuführen. Wählen Sie dann in der Liste mit den Optionen für die automatische Wiedergabe eine der Inhaltsereignisoptionen aus, die Sie in der Datei „package.appxmanifest“ angegeben haben. Dieser Beispielcode blendet Bilder im DCIM-Ordner auf der Speicherkarte einer Kamera nur ein oder kopiert diese. Wenn die Kameraspeicherkarte Bilder in einem AVCHD oder PRIVATE speichert\\ACHD-Ordner, Sie müssen den Code entsprechend aktualisieren.
    **Beachten Sie**  Wenn Sie nicht über eine Kameraspeicherkarte verfügen, können Sie ein Flashlaufwerk verwenden, besitzt er einen Ordner namens **DCIM** im Stammverzeichnis, und wenn die DCIM-Ordner einen Unterordner enthält, die Bilder enthält.

## <a name="register-for-an-autoplay-device"></a>Registrieren für ein Gerät mit automatischer Wiedergabe


Sie können Apps als Optionen für Geräteereignisse der **automatischen Wiedergabe** registrieren. Geräteereignisse zur **automatischen Wiedergabe** werden ausgelöst, wenn ein Gerät an einen PC angeschlossen wird.

Hier wird gezeigt, wie Sie eine App als Option für die **automatische Wiedergabe** identifizieren, wenn eine Kamera an einen PC angeschlossen wird. Die app registriert wird, als Handler für die **WPD\\ImageSourceAutoPlay** Ereignis. Dabei handelt es sich um ein häufiges Ereignis, das vom WPD-System (Windows Portable Device) ausgelöst wird, wenn es von Kameras und anderen Bildverarbeitungsgeräten benachrichtigt wird, dass es sich dabei um eine ImageSource mit MTP handelt. Weitere Informationen finden Sie unter [Tragbare Windows-Geräte](https://msdn.microsoft.com/library/windows/hardware/ff597729).

**Wichtige**  der [**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654) APIs sind Teil der [desktop Gerätefamilie](https://msdn.microsoft.com/library/windows/apps/dn894631). Apps können diese APIs nur für Windows 10-Geräte in der desktop-Gerät-Familie, wie z. B. PCs verwenden.

 

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Schritt 1: Erstellen eines neuen Projekts, und fügen Sie die automatische Wiedergabe-Deklarationen

1.  Öffnen Sie Visual Studio, und wählen Sie im Menü **Datei** die Option **Neues Projekt** aus. Wählen Sie im Abschnitt **Visual C#** unter **Windows** die Option **Leere App (Universelle Windows-App)** aus. Benennen Sie die app **AutoPlayDevice\_Kamera** , und klicken Sie auf **OK.**
2.  Öffnen Sie die Datei „Package.appxmanifest“, und wählen Sie die Registerkarte **Funktionen** aus. Wählen Sie die Funktion **Wechselmedien** aus. Damit geben Sie der App Zugriff auf die Daten auf der Kamera als Wechselmedien-Volumegerät.
3.  Klicken Sie in der Manifestdatei auf die Registerkarte **Deklarationen**. Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Gerät automatisch wiedergeben** aus, und klicken Sie anschließend auf **Hinzufügen**. Wählen Sie das neue Element vom Typ **Gerät automatisch wiedergeben** aus, das der Liste **Unterstützte Deklarationen** hinzugefügt wurde.
4.  Die Deklaration **Gerät automatisch wiedergeben** erkennt Ihre App als Option, wenn von der automatischen Wiedergabe ein Geräteereignis ausgelöst wird. Geben Sie im Abschnitt **Startaktionen** die Werte aus der nachfolgenden Tabelle für die Aktion beim ersten Start an.
5.  Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Dateitypzuordnungen** aus, und klicken Sie anschließend auf **Hinzufügen**. In den Eigenschaften der neuen **Dateitypzuordnungen** legen Sie die Deklaration der **Anzeigenamen** Feld **Bilder anzeigen, von der Kamera** und **Namen**Feld **Kamera\_association1**. Klicken Sie im Abschnitt **Unterstützte Dateitypen** auf **Neu hinzufügen** (falls erforderlich). Legen Sie im Feld **Dateityp** die Option **.jpg** fest. Klicken Sie im Abschnitt **Unterstützte Dateitypen** erneut auf **Neu hinzufügen**. Legen Sie das Feld **Dateityp** der neuen Dateizuordnung auf **.png** fest. Für Inhaltsereignisse filtert die automatische Wiedergabe alle Dateitypen heraus, die nicht explizit Ihrer App zugeordnet sind.
6.  Speichern und schließen Sie die Manifestdatei.

| Einstellung             | Wert            |
|---------------------|------------------|
| Verb                | show             |
| Aktionsanzeigename | Bilder anzeigen    |
| Inhaltsereignis       | WPD\\"ImageSource" |

Die Einstellung **Aktionsanzeigename** gibt die Zeichenfolge an, die bei der automatischen Wiedergabe für Ihre App angezeigt wird. Die Einstellung **Verb** dient zum Angeben eines Werts, der für die ausgewählte Option an Ihre App übergeben wird. Sie können mehrere Startaktionen für Ereignisse der automatischen Wiedergabe angeben und mit der Einstellung **Verb** ermitteln, welche Option ein Benutzer für Ihre App ausgewählt hat. Für welche Option sich der Benutzer entschieden hat, erfahren Sie durch Überprüfen der **verb**-Eigenschaft der an die App übergebenen Startereignisargumente. Für die Einstellung **Verb** können Sie einen beliebigen Wert verwenden. Einzige Ausnahme ist **open**: Dieser Wert ist reserviert. Ein Beispiel zur Verwendung mehrerer Verben in einer einzelnen App finden Sie unter [Registrieren für Inhalt für die automatische Wiedergabe](#register-for-autoplay-content).

### <a name="step-2-add-assembly-reference-for-the-desktop-extensions"></a>Schritt 2: Hinzufügen von Assembly-Verweis für die desktop-Erweiterungen

Die APIs, die für den Speicherzugriff auf einem tragbaren Windows-Gerät erforderlich sind ([**Windows.Devices.Portable.StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654)), sind Teil der [Desktopgerätefamilie](https://msdn.microsoft.com/library/windows/apps/dn894631). Dies bedeutet, dass eine spezielle Assembly erforderlich ist, um die APIs zu verwenden, und dass diese Aufrufe nur auf einem Gerät der Desktopgerätefamilie (z. B. einem PC) funktionieren.

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie dann **Verweis hinzufügen** aus.
2.  Erweitern Sie die Option **Universal Windows**, und klicken Sie auf **Erweiterungen**.
3.  Wählen Sie dann **Windows Desktop Extensions for the UWP** aus, und klicken Sie auf **OK**.

### <a name="step-3-add-xaml-ui"></a>Schritt 3: Hinzufügen der XAML-Benutzeroberfläche

Öffnen Sie die Datei „MainPage.xaml“, und fügen Sie dem Standardabschnitt &lt;Grid&gt; folgenden XAML-Code hinzu.

```xml
<StackPanel Orientation="Vertical" Margin="10,0,-10,0">
    <TextBlock FontSize="24">Device Information</TextBlock>
    <StackPanel Orientation="Horizontal">
        <TextBlock x:Name="DeviceInfoTextBlock" FontSize="18" Height="400" Width="400" VerticalAlignment="Top" />
        <ListView x:Name="ImagesList" HorizontalAlignment="Left" Height="400" VerticalAlignment="Top" Width="400">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical">
                        <Image Source="{Binding Path=Source}" />
                        <TextBlock Text="{Binding Path=Name}" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
            <ListView.ItemsPanel>
                <ItemsPanelTemplate>
                    <WrapGrid Orientation="Horizontal" ItemHeight="100" ItemWidth="120"></WrapGrid>
                </ItemsPanelTemplate>
            </ListView.ItemsPanel>
        </ListView>
    </StackPanel>
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Schritt 4: Gültigkeitsdauer des Aktivierungscodes hinzufügen

Der Code in diesem Schritts verweist auf die Kamera als [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654), indem er die Geräteinformations-ID der Kamera an die [**FromId**](https://msdn.microsoft.com/library/windows/apps/br225655)-Methode übergibt. Die Geräteinformations-ID der Kamera wird ermittelt, indem die Ereignisargumente zunächst in [**DeviceActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224710) umgewandelt werden und anschließend der Wert von der [**DeviceInformationId**](https://msdn.microsoft.com/library/windows/apps/br224711)-Eigenschaft abgerufen wird.

Öffnen Sie die Datei „App.xaml.cs“, und fügen Sie der **App**-Klasse den folgenden Code hinzu.

```cs
protected override void OnActivated(IActivatedEventArgs args)
{
   if (args.Kind == ActivationKind.Device)
   {
      Frame rootFrame = null;
      // Ensure that the current page exists and is activated
      if (Window.Current.Content == null)
      {
         rootFrame = new Frame();
         rootFrame.Navigate(typeof(MainPage));
         Window.Current.Content = rootFrame;
      }
      else
      {
         rootFrame = Window.Current.Content as Frame;
      }
      Window.Current.Activate();

      // Make sure the necessary APIs are present on the device
      bool storageDeviceAPIPresent =
      Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.Portable.StorageDevice");

      if (storageDeviceAPIPresent)
      {
         // Reference the current page as type MainPage
         var mPage = rootFrame.Content as MainPage;

         // Cast the activated event args as DeviceActivatedEventArgs and show images
         var deviceArgs = args as DeviceActivatedEventArgs;
         if (deviceArgs != null)
         {
            mPage.ShowImages(Windows.Devices.Portable.StorageDevice.FromId(deviceArgs.DeviceInformationId));
         }
      }
      else
      {
         // Handle case where APIs are not present (when the device is not part of the desktop device family)
      }

   }

   base.OnActivated(args);
}
```

> **Beachten Sie**  der `ShowImages` Methode wird im nächsten Schritt hinzugefügt.

### <a name="step-5-add-code-to-display-device-information"></a>Schritt 5: Fügen Sie Code zum Anzeigen von Geräteinformationen

Informationen zur Kamera erhalten Sie über die Eigenschaften der [**StorageDevice**](https://msdn.microsoft.com/library/windows/apps/br225654)-Klasse. Der Code in diesem Schritt zeigt bei der Ausführung der App den Gerätenamen und andere Informationen für den Benutzer an. Anschließend ruft der Code die Methoden GetImageList und GetThumbnail auf, die Sie im nächsten Schritt hinzufügen, um Miniaturansichten der auf der Kamera gespeicherten Bilder anzuzeigen.

Fügen Sie in der Datei „MainPage.xaml.cs“ der **MainPage**-Klasse folgenden Code hinzu.

```cs
private Windows.Storage.StorageFolder rootFolder;

internal async void ShowImages(Windows.Storage.StorageFolder folder)
{
    DeviceInfoTextBlock.Text = "Display Name = " + folder.DisplayName + "\n";
    DeviceInfoTextBlock.Text += "Display Type =  " + folder.DisplayType + "\n";
    DeviceInfoTextBlock.Text += "FolderRelativeId = " + folder.FolderRelativeId + "\n";

    // Reference first folder of the device as the root
    rootFolder = (await folder.GetFoldersAsync())[0];
    var imageList = await GetImageList(rootFolder);

    foreach (Windows.Storage.StorageFile img in imageList)
    {
        ImagesList.Items.Add(await GetThumbnail(img));
    }
}
```

> **Beachten Sie**  der `GetImageList` und `GetThumbnail` Methoden werden im nächsten Schritt hinzugefügt.

### <a name="step-6-add-code-to-display-images"></a>Schritt 6: Fügen Sie Code zum Anzeigen von Bildern

Der Code in diesem Schritt zeigt Miniaturansichten der Bilder an, die auf der Kamera gespeichert sind. Der Code ruft die Miniaturansicht mit asynchronen Aufrufen an die Kamera ab. Der nächste asynchrone Aufruf erfolgt aber nicht, bis der vorherige asynchrone Aufruf abgeschlossen ist. Auf diese Weise wird sichergestellt, dass nur jeweils ein Aufruf an die Kamera gesendet wird.

Fügen Sie in der Datei „MainPage.xaml.cs“ der **MainPage**-Klasse folgenden Code hinzu.

```cs
async private System.Threading.Tasks.Task<List<Windows.Storage.StorageFile>> GetImageList(Windows.Storage.StorageFolder folder)
{
    var result = await folder.GetFilesAsync();
    var subFolders = await folder.GetFoldersAsync();
    foreach (Windows.Storage.StorageFolder f in subFolders)
        result = result.Union(await GetImageList(f)).ToList();

    return (from f in result orderby f.Name select f).ToList();
}

async private System.Threading.Tasks.Task<Image> GetThumbnail(Windows.Storage.StorageFile img)
{
    // Get the thumbnail to display
    var thumbnail = await img.GetThumbnailAsync(Windows.Storage.FileProperties.ThumbnailMode.SingleItem,
                                                100,
                                                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale);

    // Create a XAML Image object bind to on the display page
    var result = new Image();
    result.Height = thumbnail.OriginalHeight;
    result.Width = thumbnail.OriginalWidth;
    result.Name = img.Name;
    var imageBitmap = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
    imageBitmap.SetSource(thumbnail);
    result.Source = imageBitmap;

    return result;
}
```

### <a name="step-7-build-and-run-the-app"></a>Schritt 7: Erstellen und Ausführen der App

1.  Drücken Sie F5, um die App zu erstellen und bereitzustellen (im Debugmodus).
2.  Führen Sie Ihre App aus, indem Sie eine Kamera an Ihren Computer anschließen. Wählen Sie die App anschließend in der Liste mit Optionen für die automatische Wiedergabe aus.
    **Beachten Sie**  nicht alle Kameras angekündigt werden soll, für die **WPD\\"ImageSource"** AutoPlay-Gerät-Ereignis.

## <a name="configure-removable-storage"></a>Konfigurieren von Wechselmedien

Sie können ein Volumegerät wie eine Speicherkarte oder einen USB-Stick als Gerät für die **automatische Wiedergabe** identifizieren, wenn das Volumegerät an einen PC angeschlossen wird. Das ist vor allem dann sehr nützlich, wenn Sie möchten, dass dem Benutzer bei der **automatischen Wiedergabe** eine bestimmte App für Ihr Volumegerät angeboten wird.

Im Folgenden zeigen wir, wie Sie Ihr Volumegerät als Gerät zur **automatischen Wiedergabe** identifizieren.

Identifizieren Sie Ihr Volumegerät als Gerät für die **automatische Wiedergabe**, indem Sie dem Stammverzeichnis des Geräts die Datei „autorun.inf“ hinzufügen. Fügen Sie in der Datei „autorun.inf“ im **AutoRun**-Abschnitt einen **CustomEvent**-Schlüssel hinzu. Wenn Ihr Volumegerät an einen PC angeschlossen wird, findet die **automatische Wiedergabe** die Datei „autorun.inf“ und behandelt Ihr Volume als ein Gerät. Die **automatische Wiedergabe** erstellt anhand des Namens, den Sie für den **CustomEvent**-Schlüssel angegeben haben, ein Ereignis zur **automatischen Wiedergabe**. Sie können eine App erstellen und sie dann als Handler für dieses Ereignis zur **automatischen Wiedergabe** registrieren. Wenn das Gerät an den PC angeschlossen wird, bietet die **automatische Wiedergabe** Ihre App als Handler für Ihr Volumegerät an. Weitere Informationen zu Dateien vom Typ „autorun.inf“ finden Sie unter [Autorun.inf-Einträge](https://msdn.microsoft.com/library/windows/desktop/cc144200).

### <a name="step-1-create-an-autoruninf-file"></a>Schritt 1: Erstellen Sie eine autorun.inf-Datei

Fügen Sie im Stammverzeichnis Ihres Volumegeräts eine Datei namens „autorun.inf“ hinzu. Öffnen Sie die Datei „autorun.inf“, und fügen Sie den folgenden Text hinzu.

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### <a name="step-2-create-a-new-project-and-add-autoplay-declarations"></a>Schritt 2: Erstellen eines neuen Projekts, und fügen Sie die automatische Wiedergabe-Deklarationen

1.  Öffnen Sie Visual Studio, und wählen Sie im Menü **Datei** die Option **Neues Projekt** aus. Wählen Sie im Abschnitt **Visual C#** unter **Windows** die Option **Leere App (Universelle Windows-App)** aus. Nennen Sie die Anwendung **AutoPlayCustomEvent**, und klicken Sie auf **OK.**
2.  Öffnen Sie die Datei „Package.appxmanifest“, und wählen Sie die Registerkarte **Funktionen** aus. Wählen Sie die Funktion **Wechselmedien** aus. Damit erhält Ihre App Zugriff auf die Dateien und Ordner auf dem Wechselmediengerät.
3.  Klicken Sie in der Manifestdatei auf die Registerkarte **Deklarationen**. Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Inhalt automatisch wiedergeben** aus, und klicken Sie anschließend auf **Hinzufügen**. Wählen Sie das neue Element vom Typ **Inhalt automatisch wiedergeben** aus, das der Liste **Unterstützte Deklarationen** hinzugefügt wurde.

    **Beachten Sie**  Alternativ Sie können auch zum Hinzufügen einer **Gerät automatisch wiedergeben** Deklaration für das benutzerdefinierte AutoPlay-Ereignis.  
4.  Geben Sie im Abschnitt **Startaktionen** der Ereignisdeklaration **Inhalt automatisch Wiedergeben** für die Aktion beim ersten Start die Werte in der folgenden Tabelle ein.
5.  Wählen Sie in der Dropdownliste **Verfügbare Deklarationen** die Option **Dateitypzuordnungen** aus, und klicken Sie anschließend auf **Hinzufügen**. In den Eigenschaften der neuen **Dateitypzuordnungen** legen Sie die Deklaration der **Anzeigenamen** Feld **anzeigen.ms Dateien** und **Namen** Feld **ms\_Zuordnung**. Klicken Sie im Abschnitt **Unterstützte Dateitypen** auf **Neu hinzufügen**. Legen Sie im Feld **Dateityp** die Option **.ms** fest. Für Inhaltsereignisse filtert die automatische Wiedergabe alle Dateitypen heraus, die nicht explizit Ihrer App zugeordnet sind.
6.  Speichern und schließen Sie die Manifestdatei.

| Einstellung             | Wert                         |
|---------------------|-------------------------------|
| Verb                | show                          |
| Aktionsanzeigename | Dateien anzeigen                    |
| Inhaltsereignis       | AutoPlayCustomEventQuickstart |

Der Wert für **Inhaltsereignis** ist der Text, den Sie für den **CustomEvent**-Schlüssel in der Datei „autorun.inf“ angegeben haben. Die Einstellung **Aktionsanzeigename** gibt die Zeichenfolge an, die bei der automatischen Wiedergabe für Ihre App angezeigt wird. Die Einstellung **Verb** dient zum Angeben eines Werts, der für die ausgewählte Option an Ihre App übergeben wird. Sie können mehrere Startaktionen für Ereignisse der automatischen Wiedergabe angeben und mit der Einstellung **Verb** ermitteln, welche Option ein Benutzer für Ihre App ausgewählt hat. Für welche Option sich der Benutzer entschieden hat, erfahren Sie durch Überprüfen der **verb**-Eigenschaft der an die App übergebenen Startereignisargumente. Für die Einstellung **Verb** können Sie einen beliebigen Wert verwenden. Einzige Ausnahme ist **open**: Dieser Wert ist reserviert.

### <a name="step-3-add-xaml-ui"></a>Schritt 3: Hinzufügen der XAML-Benutzeroberfläche

Öffnen Sie die Datei „MainPage.xaml“, und fügen Sie dem Standardabschnitt &lt;Grid&gt; folgenden XAML-Code hinzu.

```xml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Schritt 4: Gültigkeitsdauer des Aktivierungscodes hinzufügen

Der Code in diesem Schritt ruft eine Methode zum Anzeigen der Ordner im Stammlaufwerk Ihres Volumegeräts auf. Für das Inhaltsereignis zur automatischen Wiedergabe übergibt die automatische Wiedergabe den Stammordner des Speichergeräts in den Startargumenten, die der Anwendung während des **OnFileActivated**-Ereignisses übergeben werden. Sie können diesen Ordner aus dem ersten Element der **Files**-Eigenschaft abrufen.

Öffnen Sie die Datei „App.xaml.cs“, und fügen Sie der **App**-Klasse den folgenden Code hinzu.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    var rootFrame = Window.Current.Content as Frame;
    var page = rootFrame.Content as MainPage;

    // Call ShowFolders with root folder from device storage.
    page.DisplayFiles(args.Files[0] as Windows.Storage.StorageFolder);

    base.OnFileActivated(args);
}
```

> **Beachten Sie**  der `DisplayFiles` Methode wird im nächsten Schritt hinzugefügt.

 

### <a name="step-5-add-code-to-display-folders"></a>Schritt 5: Fügen Sie Code, um Ordner anzuzeigen.

Fügen Sie in der Datei „MainPage.xaml.cs“ der **MainPage**-Klasse folgenden Code hinzu.

```cs
internal async void DisplayFiles(Windows.Storage.StorageFolder folder)
{
    foreach (Windows.Storage.StorageFile f in await ReadFiles(folder, ".ms"))
    {
        FilesBlock.Text += "  " + f.Name + "\n";
    }
}

internal async System.Threading.Tasks.Task<IReadOnlyList<Windows.Storage.StorageFile>>
    ReadFiles(Windows.Storage.StorageFolder folder, string fileExtension)
{
    var options = new Windows.Storage.Search.QueryOptions();
    options.FileTypeFilter.Add(fileExtension);
    var query = folder.CreateFileQueryWithOptions(options);
    var files = await query.GetFilesAsync();

    return files;
}
```

### <a name="step-6-build-and-run-the-qpp"></a>Schritt 6: Erstellen und Ausführen der qpp

1.  Drücken Sie F5, um die App zu erstellen und bereitzustellen (im Debugmodus).
2.  Legen Sie eine Speicherkarte oder ein anderes Speichergerät in Ihren PC ein, um Ihre App auszuführen. Wählen Sie die App anschließend in der Liste mit den Handleroptionen für die automatische Wiedergabe aus.

## <a name="autoplay-event-reference"></a>Referenz für Ereignisse der automatischen Wiedergabe


Das **automatische Wiedergabesystem** ermöglicht es, Apps für eine Vielzahl von Arrival-Ereignissen für Geräte und Volumes (Datenträger) zu registrieren. Wenn Sie die App für Inhaltsereignisse für die **automatische Wiedergabe** registrieren möchten, müssen Sie die Funktion **Wechselmedien** im Paketmanifest aktivieren. In der folgenden Tabelle sind die Ereignisse aufgeführt, die Sie für die automatische Wiedergabe registrieren können, wenn sie ausgelöst werden.

| Szenario                                                           | Ereignis                              | Beschreibung   |
|--------------------------------------------------------------------|------------------------------------|---------------|
| Anzeigen von Fotos auf einer Kamera                                           | **WPD\ImageSource**                | Ausgelöst für Kameras, die als tragbare Windows-Geräte mit ImageSource-Funktion erkannt wurden. |
| Abspielen von Musik auf einem Audioplayer                                     | **WPD\AudioSource**                | Ausgelöst für Mediaplayer, die als tragbare Windows-Geräte mit AudioSource-Funktion erkannt wurden. |
| Abspielen von Videos auf einer Videokamera                                     | **WPD\VideoSource**                | Ausgelöst für Videokameras, die als tragbare Windows-Geräte mit VideoSource-Funktion erkannt wurden. |
| Zugriff auf ein verbundenes Flash-Laufwerk oder auf eine externe Festplatte              | **StorageOnArrival**               | Ausgelöst, wenn ein Laufwerk oder ein Volume an den PC angeschlossen wird.   Wenn das Laufwerk oder Volume den Ordner „DCIM“, „AVCHD“ oder „PRIVATE\ACHD“ im Stammverzeichnis des Datenträgers enthält, wird stattdessen das **ShowPicturesOnArrival**-Ereignis ausgelöst. |
| Anzeigen von Fotos auf einem Massenspeicher (Legacy)                            | **ShowPicturesOnArrival**          | Dieses Ereignis wird ausgelöst, wenn das Laufwerk oder das Volume den Ordner „DCIM“, „AVCHD“ oder „PRIVATE\ACHD“ im Stammverzeichnis der Platte enthält. Wenn der Benutzer in der Systemsteuerung unter „Automatische Wiedergabe“ die Option **Gewünschte Aktion für jeden Medientyp auswählen** aktiviert hat, überprüft die automatische Wiedergabe das am PC angeschlossene Volume, um den Inhaltstyp auf dem Datenträger zu ermitteln. Wenn Bilder gefunden werden, wird **ShowPicturesOnArrival** ausgelöst. |
| Empfangen von Fotos mit Näherungsfreigabe (Koppeln und Senden)             | **ShowPicturesOnArrival**          | Wenn Benutzer Inhalt mit Näherung (Koppeln und Senden) senden, prüft die automatische Wiedergabe die freigegebenen Dateien auf den Inhaltstyp. Wenn Bilder gefunden werden, wird **ShowPicturesOnArrival** ausgelöst. |
| Abspielen von Musik von einem Massenspeicher (Legacy)                             | **PlayMusicFilesOnArrival**        | Wenn der Benutzer in der Systemsteuerung unter „Automatische Wiedergabe“ die Option **Gewünschte Aktion für jeden Medientyp auswählen** aktiviert hat, überprüft die automatische Wiedergabe das am PC angeschlossene Volume, um den Inhaltstyp auf dem Datenträger zu ermitteln.  Wenn Musikdateien gefunden werden, wird **PlayMusicFilesOnArrival** ausgelöst. |
| Empfangen von Musik mit Näherungsfreigabe (Koppeln und Senden)              | **PlayMusicFilesOnArrival**        | Wenn Benutzer Inhalt mit Näherung (Koppeln und Senden) senden, prüft die automatische Wiedergabe die freigegebenen Dateien auf den Inhaltstyp. Wenn Musikdateien gefunden werden, wird **PlayMusicFilesOnArrival** ausgelöst. |
| Abspielen von Videos von einem Massenspeicher (Legacy)                            | **PlayVideoFilesOnArrival**        | Wenn der Benutzer in der Systemsteuerung unter „Automatische Wiedergabe“ die Option **Gewünschte Aktion für jeden Medientyp auswählen** aktiviert hat, überprüft die automatische Wiedergabe das am PC angeschlossene Volume, um den Inhaltstyp auf dem Datenträger zu ermitteln. Wenn Videodateien gefunden werden, wird **PlayVideoFilesOnArrival** ausgelöst. |
| Empfangen von Videos mit Näherungsfreigabe (Koppeln und Senden)             | **PlayVideoFilesOnArrival**        | Wenn Benutzer Inhalt mit Näherung (Koppeln und Senden) senden, prüft die automatische Wiedergabe die freigegebenen Dateien auf den Inhaltstyp. Wenn Videodateien gefunden werden, wird **PlayVideoFilesOnArrival** ausgelöst. |
| Verarbeitung von gemischten Dateigruppen von einem verbundenen Gerät aus               | **MixedContentOnArrival**          | Wenn der Benutzer in der Systemsteuerung unter „Automatische Wiedergabe“ die Option **Gewünschte Aktion für jeden Medientyp auswählen** aktiviert hat, überprüft die automatische Wiedergabe das am PC angeschlossene Volume, um den Inhaltstyp auf dem Datenträger zu ermitteln. Wenn kein bestimmter Inhaltstyp gefunden wird (z. B. Bilder), wird **MixedContentOnArrival** ausgelöst. |
| Verarbeitung von gemischten Dateigruppen mit Näherungsfreigabe (Koppeln und Senden) | **MixedContentOnArrival**          | Wenn Benutzer Inhalt mit Näherung (Koppeln und Senden) senden, prüft die automatische Wiedergabe die freigegebenen Dateien auf den Inhaltstyp. Wenn kein bestimmter Inhaltstyp gefunden wird (z. B. Bilder), wird **MixedContentOnArrival** ausgelöst. |
| Verarbeitung von Videos von optischen Medien                                    | **PlayDVDMovieOnArrival**<br />**PlayBluRayOnArrival**<br />**PlayVideoCDMovieOnArrival**<br />**PlaySuperVideoCDMovieOnArrival** | Wenn ein Datenträger in das optische Laufwerk eingelegt wird, prüft die automatische Wiedergabe die Dateien, um den Inhaltstyp zu ermitteln. Wenn eine Videodateien gefunden wurde, wird das Ereignis entsprechend dem Typ des optischen Datenträgers ausgelöst. |
| Verarbeitung von Musik von optischen Medien                                    | **PlayCDAudioOnArrival**<br />**PlayDVDAudioOnArrival** | Wenn ein Datenträger in das optische Laufwerk eingelegt wird, prüft die automatische Wiedergabe die Dateien, um den Inhaltstyp zu ermitteln. Wenn eine Musikdateien gefunden wurde, wird das Ereignis entsprechend dem Typ des optischen Datenträgers ausgelöst. |
| Abspielen erweiterter Datenträger                                                | **PlayEnhancedCDOnArrival**<br />**PlayEnhancedDVDOnArrival** | Wenn ein Datenträger in das optische Laufwerk eingelegt wird, prüft die automatische Wiedergabe die Dateien, um den Inhaltstyp zu ermitteln. Wenn ein erweiterter Datenträger gefunden wurde, wird das Ereignis entsprechend dem Typ des optischen Datenträgers ausgelöst. |
| Verarbeitung von beschreibbaren optischen Datenträgern                                     | **HandleCDBurningOnArrival** <br />**HandleDVDBurningOnArrival** <br />**HandleBDBurningOnArrival** | Wenn ein Datenträger in das optische Laufwerk eingelegt wird, prüft die automatische Wiedergabe die Dateien, um den Inhaltstyp zu ermitteln. Wenn ein Datenträger mit Schreibzugriff gefunden wurde, wird das Ereignis entsprechend dem Typ des optischen Datenträgers ausgelöst. |
| Verarbeitung von anderen Geräte- oder Volumeverbindungen                       | **UnknownContentOnArrival**        | Ausgelöst für alle Ereignisse, wenn Inhalt gefunden wurde, der mit einem der Inhaltsereignisse für die automatische Wiedergabe übereinstimmt. Es wird nicht empfohlen, dieses Ereignis zu verwenden. Sie sollten Ihre Anwendung nur für bestimmte Ereignisse für die automatische Wiedergabe registrieren, die sie auch verarbeiten kann. |

Mit dem Eintrag **CustomEvent** in der Datei „autorun.inf“ können Sie angeben, dass von der automatischen Wiedergabe ein benutzerdefiniertes Inhaltsereignis für die automatische Wiedergabe ausgelöst wird. Weitere Informationen finden Sie unter [Autorun.inf-Einträge](https://msdn.microsoft.com/library/windows/desktop/cc144200).

Sie können Ihre App als Ereignishandler für Inhalte oder Geräte zur automatischen Wiedergabe registrieren, indem Sie der Datei „package.appxmanifest“ eine Erweiterung für die App hinzufügen. Wenn Sie Visual Studio verwenden, können Sie auf der Registerkarte **Deklarationen** eine Deklaration vom Typ **Inhalt automatisch wiedergeben** oder **Gerät automatisch wiedergeben** hinzufügen. Wenn Sie die Datei „package.appxmanifest“ für die App direkt bearbeiten, fügen Sie dem Paketmanifest ein [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400)-Element hinzu, das als **Category** entweder **windows.autoPlayContent** oder **windows.autoPlayDevice** festlegt. So wird beispielsweise durch den folgenden Eintrag im Paketmanifest eine Erweiterung für **Inhalt automatisch wiedergeben** hinzugefügt, um die App als Handler für das **ShowPicturesOnArrival**-Ereignis zu registrieren.

```xml
  <Applications>
    <Application Id="AutoPlayHandlerSample.App">
      <Extensions>
        <Extension Category="windows.autoPlayContent">
          <AutoPlayContent>
            <LaunchAction Verb="show" ActionDisplayName="Show Pictures"
                          ContentEvent="ShowPicturesOnArrival" />
          </AutoPlayContent>
        </Extension>
      </Extensions>
    </Application>
  </Applications>
```

 

 
