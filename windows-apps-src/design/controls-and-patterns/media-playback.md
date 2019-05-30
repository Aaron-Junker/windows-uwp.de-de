---
Description: Media Player wird zum Anzeigen und Wiedergeben von Videos, Audiodateien und Bildern verwendet.
title: Media Player
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b212ff435e58bdb8766972d1832bbf0690db3ed1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364742"
---
# <a name="media-player"></a>Media Player



Der Media Player wird verwendet, um Videos und Audio anzuzeigen und zu hören. Die Medienwiedergabe kann inline (eingebettet auf einer Seite oder mit einer Gruppe von anderen Steuerelementen) oder in einer dedizierten Vollbildansicht erfolgen. Sie können die Schaltflächen des Players ändern, den Hintergrund der Steuerelementleiste ändern und Layouts wie gewünscht anordnen. Beachten Sie jedoch, dass Benutzer eine Reihe grundlegender Steuerelemente (Wiedergabe/Pause, Zurückspringen, Vorwärts springen) erwarten.

![Media Player-Element mit Transportsteuerelementen](images/controls/mtc_double_video_inprod.png)

> **Wichtige APIs:** [MediaPlayerElement Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement), [MediaTransportControls-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediatransportcontrols)


> [!NOTE]
> **MediaPlayerElement** steht erst ab Windows 10, Version 1607, zur Verfügung. Bei der Entwicklung von Apps für Vorgängerversionen von Windows 10 muss stattdessen das [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) verwendet werden. Alle Empfehlungen auf dieser Seite gelten auch für MediaElement.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie einen Mediaplayer, wenn Sie Audio- oder Videodateien in Ihrer App wiedergeben möchten. Verwenden Sie zum Anzeigen einer Sammlung von Bildern die [Flip-Ansicht](flipview.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/MediaPlayerElement">MediaPlayerElement</a> oder <a href="xamlcontrolsgallery:/item/MediaPlayer">MediaPlayer</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Ein Media Player in der Windows 10-Erste Schritte-App.

![Ein Medienelement in der Windows 10-Erste Schritte-App.](images/control-examples/mtc_getstarted_example.png)

## <a name="create-a-media-player"></a>Erstellen eines Media Players
Sie fügen Ihrer App Medien hinzu, indem Sie ein [MediaElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)-Objekt in XAML erstellen und die [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) auf eine [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) festlegen, die auf eine Audio- oder Videodatei verweist.

Mit diesem XAML-Code wird ein [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) erstellt, dessen [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source)-Eigenschaft auf den URI einer Videodatei festgelegt wird, bei der es sich um eine lokale Datei der App handelt. Das **MediaPlayerElement** beginnt mit der Wiedergabe, wenn die Seite geladen wird. Um zu verhindern, dass die Medienwiedergabe sofort beginnt, können Sie die [Automatische Wiedergabe](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.autoplay) auf **false** setzen.

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="ms-appx:///Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

Mit diesem XAML-Code wird ein [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) -Objekt mit aktivierten integrierten Transportsteuerelementen und mit einer [AutoPlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.autoplay)-Eigenschaft, die auf **false** festgelegt ist, erstellt.


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="ms-appx:///Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### <a name="media-transport-controls"></a>Steuerelemente für den Medientransport
[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) verfügt über integrierte Transportsteuerelemente für Wiedergabe, Beenden, Anhalten, Lautstärke, Stummschaltung, Suche/Status, Untertitel und Audiotitelauswahl. Um diese Steuerelemente zu aktivieren, muss [AreTransportControlsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled) auf **true** gesetzt werden. Um sie zu deaktivieren, legen Sie **AreTransportControlsEnabled** auf **false** fest. Die Mediensteuerungselemente werden durch die Klasse [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) dargestellt. Sie können die Transportsteuerelemente unverändert verwenden oder auf verschiedene Weise anpassen. Weitere Informationen finden Sie in der Referenz zur Klasse [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) und unter [Erstellen einer benutzerdefinierten Mediensteuerung](custom-transport-controls.md).

Die Transportsteuerelemente unterstützen Layouts mit einer Zeile und mit Doppelzeile. Im ersten Beispiel sehen Sie ein einzeiliges-Layout mit der Wiedergabe/Pause-Schaltfläche links von der Medienzeitachse. Dieses Layout wird am besten für die Wiedergabe von Inlinemedien und kompakte Bildschirme reserviert.

![Beispiel für MTC-Steuerelemente, einzelne Zeile](images/controls/mtc_single_inprod_02.png)

Das Layout mit doppelzeiligen Steuerelementen (siehe unten) wird für die meisten Nutzungsszenarien empfohlen, insbesondere für größere Bildschirme. Dieses Layout bietet mehr Platz für Steuerelemente und erleichtert dem Benutzer die Bedienung der Zeitachse.

![Beispiel für MTC-Steuerelemente, Doppelzeile](images/controls/mtc_double_inprod.png)

**Informationssystem-Medientypen Transportsteuerelemente**

Das [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) wird automatisch in die Mediensteuerung des Systems integriert. Die Medientransportsteuerelemente des Systems sind die Steuerelemente, die angezeigt werden, wenn Hardwaretasten für Medien betätigt werden, z. B. die Medientasten auf Tastaturen. Weitere Informationen finden Sie unter [SystemMediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls).

> **Beachten Sie** &nbsp; &nbsp; [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) kann nicht automatisch mit dem System Medientransport steuert, damit Sie sie sich verbinden müssen integriert werden. Weitere Informationen finden Sie unter [Steuerelemente für den Systemmedientransport](https://docs.microsoft.com/windows/uwp/audio-video-camera/system-media-transport-controls).


### <a name="set-the-media-source"></a>Festlegen der Medienquelle
Um Dateien im Netzwerk oder in die App eingebettete Dateien wiederzugeben, legen Sie die [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source)-Eigenschaft auf eine [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) mit dem Pfad der Datei fest.

**Tipp**  zum Öffnen von Dateien über das Internet müssen Sie deklarieren die **Internet (Client)** -Funktion in Ihrer app-Manifest (Package.appxmanifest). Weitere Informationen zum Deklarieren von Funktionen finden Sie unter [Deklaration der App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

 

Dieser Code versucht, die [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source)-Eigenschaft des im XAML-Code definierten [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) auf den Pfad einer Datei festzulegen, der in ein [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Element eingegeben wurde.

```xaml
<TextBox x:Name="txtFilePath" Width="400"
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

Um die Medienquelle auf eine in die App eingebettete Mediendatei festzulegen, initialisieren Sie einen [Uri](https://docs.microsoft.com/uwp/api/windows.foundation.uri.) mit dem Pfadpräfix **ms-appx:///** , erstellen anschließend eine [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) mit dem URI und legen anschließend die [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) auf den URI fest. Für eine Datei mit dem Namen **video1.mp4**, die sich in dem Unterordner **Videos** befindet, würde der Pfad beispielsweise wie folgt aussehen: **ms-appx:///Videos/video1.mp4**.

Mit diesem Code wird die [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source)-Eigenschaft des [MediaPlayerElements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) definiert, das zuvor in XAML auf **ms-appx:///Videos/video1.mp4** festgelegt wurde.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

### <a name="open-local-media-files"></a>Öffnen von lokalen Mediendateien
Zum Öffnen von Dateien im lokalen System oder aus OneDrive können Sie das [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Element verwenden, um die Datei abzurufen, und [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source), um die Medienquelle festzulegen; alternativ können Sie programmgesteuert auf die Benutzermedienordner zugreifen.

Falls für Ihre App jedoch der Zugriff auf die **Musik**- oder **Videoordner** ohne Benutzerinteraktion erforderlich ist (wenn Sie beispielsweise sämtliche Musik- oder Videodateien einer Sammlung des Benutzers aufzählen und in Ihrer App anzeigen), müssen Sie die Funktionen der **Musik-** und **Videobibliothek** deklarieren. Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

Das [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Element benötigt keine besonderen Funktionen für den Zugriff auf Dateien im lokalen Dateisystem (etwa die Ordner **Musik** oder **Video** des Benutzers), da Benutzer die vollständige Kontrolle darüber haben, auf welche Datei zugegriffen wird. Aus Sicherheits- und Datenschutzgründen ist es am sinnvollsten, die Anzahl der von der App verwendeten Funktionen möglichst gering zu halten.

**Zum Öffnen von lokaler Medien, die mithilfe der fileopenpicker-Klasse**

1.  Rufen Sie [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) auf, um dem Benutzer die Auswahl einer Mediendatei zu ermöglichen.

    Verwenden Sie die Klasse [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker), um eine Mediendatei auszuwählen. Legen Sie den [FileTypeFilter](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) fest, um zu bestimmen, welche Dateitypen von **FileOpenPicker** angezeigt werden. Rufen Sie [PickSingleFileAsync](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) auf, um die Dateiauswahl zu starten und die Datei abzurufen.

2.  Verwenden Sie eine [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource), um die ausgewählte Mediendatei als [MediaPlayerElement.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) festzulegen.

    Um die [StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) zu verwenden, die vom [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) zurückgegeben wurde, müssen Sie die [CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)-Methode für [MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) aufrufen und diese als [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) von [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) festlegen. Rufen Sie anschließend [Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play) für das [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) auf, um die Medien zu starten.


Dieses Beispiel erläutert, wie Sie [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) dazu verwenden, eine Datei auszuwählen und diese als [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) eines [MediaPlayerElements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) festlegen.

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### <a name="set-the-poster-source"></a>Festlegen der Posterquelle
Mit der Eigenschaft [PosterSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.PosterSource) können Sie eine visuelle Darstellung für Ihr [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) bereitstellen, bevor die Medien geladen werden. Eine **PosterSource** ist ein Bild, z. B. ein Screenshot oder Filmplakat, das anstelle der Medien angezeigt wird. Die **PosterSource** wird in folgenden Fällen angezeigt:

-   Wenn keine gültige Quelle festgelegt ist. Beispiel: [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) ist nicht festgelegt, **Source** wurde auf **Null** festgelegt, oder die Quelle ist ungültig (z. B. wenn ein [MediaFailed](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediafailed)-Ereignis eintritt).
-   Während Medien geladen werden. Beispiel: Es ist eine gültige Source festgelegt, das Ereignis [MediaOpened](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediaopened) ist jedoch noch nicht eingetreten.
-   Beim Streamen von Medien auf ein anderes Gerät.
-   Wenn die Medien nur Audio enthalten.

Nachfolgend ein Beispiel für ein [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement), dessen [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) auf einen Albumtitel festgelegt ist und dessen [PosterSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.PosterSource) eine Abbildung des Albumcovers enthält.

```xaml
<MediaPlayerElement Source="ms-appx:///Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### <a name="keep-the-devices-screen-active"></a>Abblenden/Ausschalten des Gerätebildschirms verhindern
Normalerweise wird bei einem Gerät das Display abgeblendet (und schließlich ausgeschaltet), um bei Abwesenheit des Benutzers den Akku zu schonen. Bei Video-Apps muss der Bildschirm jedoch eingeschaltet bleiben, damit der Benutzer das Video sehen kann. Um ein Deaktivieren der Anzeige zu verhindern, wenn keine Benutzeraktion mehr festgestellt werden kann (z. B. bei der Wiedergabe eines Videos in einer App), können Sie [DisplayRequest.RequestActive](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive) aufrufen. Mithilfe der Klasse [DisplayRequest](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) können Sie Windows anweisen, das Display eingeschaltet zu lassen, damit der Benutzer das Video sehen kann.

Um Energie zu sparen und den Akku zu schonen, sollten Sie [DisplayRequest.RequestRelease](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) aufrufen, um die Displayanfrage freizugeben, wenn diese nicht mehr benötigt wird. Windows deaktiviert automatisch die aktiven Displayanforderungen der App, wenn die App vom Bildschirm entfernt wird, und aktiviert die Displayanforderungen wieder, wenn Ihre App wieder in den Vordergrund gesetzt wird.

Unten sind einige Situationen aufgeführt, in denen Sie die Displayanforderung freigeben sollten:

-   Die Videowiedergabe wird angehalten (beispielsweise durch eine Benutzeraktion, zum Puffern oder für eine Anpassung aufgrund von begrenzter Bandbreite).
-   Die Wiedergabe wird gestoppt. Beispielsweise ist die Wiedergabe des Videos beendet oder die Darstellung vorüber.
-   Ein Wiedergabefehler ist aufgetreten. Es können beispielsweise Probleme mit der Netzwerkverbindung bestehen, oder eine Datei kann beschädigt sein.

> **Hinweis**&nbsp;&nbsp; Wenn [MediaPlayerElement.IsFullWindow](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow) auf „true“ gesetzt ist und Medien wiedergegeben werden, wird die Deaktivierung der Anzeige automatisch verhindert.

**Auf den Bildschirm aktiv bleibt.**

1.  Erstellen einer globalen [DisplayRequest](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest)-Variable. Initialisieren Sie sie mit dem Wert NULL.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Rufen Sie [RequestActive](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive) auf, um Windows mitzuteilen, dass für die App das Display eingeschaltet bleiben muss.

3.  Rufen Sie [RequestRelease](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) auf, um die Displayanfrage freizugeben, wenn die Videowiedergabe beendet, angehalten oder durch einen Wiedergabefehler unterbrochen wird. Falls für die App keine aktiven Displayanforderungen mehr vorhanden sind, schont Windows den Akku, indem das Display abgeblendet (und schließlich ausgeschaltet) wird, wenn das Gerät nicht verwendet wird.

    Jedes [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) hat eine [PlaybackSession](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.playbacksession) vom Typ [MediaPlaybackSession](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession), die verschiedene Aspekte der Medienwiedergabe steuert wie [PlaybackRate](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate), [PlaybackState](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackstate) und [Position](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position). Hier wenden Sie das Ereignis [PlaybackStateChanged](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackstatechanged) auf  [MediaPlayer.PlaybackSession](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.playbacksession) an, um Situationen zu erkennen, in denen die Displayanfrage freigeben werden sollte. Verwenden Sie dann die [NaturalVideoHeight](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight)-Eigenschaft, um festzustellen, ob eine Audio- oder Videodatei wiedergegeben wird, und lassen Sie den Bildschirm nur eingeschaltet, wenn eine Videodatei wiedergegeben wird.

    ```xaml
    <MediaPlayerElement x:Name="mpe" Source="ms-appx:///Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### <a name="control-the-media-player-programmatically"></a>Programmgesteuertes Steuern der Medienwiedergabe
Das [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) bietet zahlreiche Eigenschaften, Methoden und Ereignisse zur Steuerung der Audio- und Videowiedergabe über die Eigenschaft [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer). Eine vollständige Liste der Eigenschaften, Methoden und Ereignisse finden Sie auf der Referenzseite zum [MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) .

### <a name="advanced-media-playback-scenarios"></a>Erweiterte Medienwiedergabeszenarien
Für komplexere Medienwiedergabeszenarien, z. B. die Wiedergabe einer Playlist, Umschalten zwischen Audiosprachen oder Erstellen benutzerdefinierter Metadatentracks, legen Sie [MediaPlayerElement.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) auf eine [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) oder eine [MediaPlaybackList](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist) fest. Finden Sie unter den [Medienwiedergabe](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource) Weitere Informationen zum verschiedene erweiterte Medien--Funktionen zu aktivieren.

### <a name="enable-full-window-video-rendering"></a>Aktivieren des Videorenderings im Vollfenstermodus

Legen Sie die Eigenschaft [IsFullWindow](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.isfullwindow) fest, um das Rendering im Vollbildmodus zu aktivieren und zu deaktivieren. Wenn Sie das Rendering im Vollfenstermodus in Ihrer App programmgesteuert festlegen, sollten Sie immer die **IsFullWindow**-Eigenschaft verwenden, anstatt diese Einstellung manuell vorzunehmen. **IsFullWindow** stellt sicher, dass Optimierungen auf Systemebene zum Verbessern der Leistung und Akkulaufzeit durchgeführt werden. Wenn das Rendering im Vollfenstermodus nicht korrekt eingerichtet wird, werden diese Optimierungen möglicherweise nicht angewendet.

Der folgende Code erstellt einen [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) zum Umschalten des Renderings im Vollbildmodus.

```xaml
<AppBarButton Icon="FullScreen"
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### <a name="resize-and-stretch-video"></a>Ändern der Größe und Strecken von Videos

Verwenden Sie die Eigenschaft [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.stretch), um die Art und Weise zu ändern, wie der Videoinhalt und/oder die [PosterSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.postersource) den Container ausfüllt, in dem sich diese/dieser befindet. Die Größe des Videos wird dabei entsprechend dem [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch)-Wert geändert bzw. gestreckt. Die **Stretch**-Zustände sind mit den Bildformateinstellungen bei vielen Fernsehern vergleichbar. Sie können dieses Verhalten mit einer Schaltfläche verknüpfen und dem Benutzer die Auswahl der gewünschten Einstellung ermöglichen.

-   [None](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) zeigt die native Auflösung des Inhalts in dessen Originalgröße an.
-   [Uniform](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) füllt unter Beibehaltung des Seitenverhältnisses und des Bildinhalts den größtmöglichen Platz aus. Dies kann zu horizontalen oder vertikalen schwarzen Balken an den Rändern des Videos führen. Dieser Zustand ist mit Breitbildmodi vergleichbar.
-   [UniformToFill](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) füllt den gesamten Platz unter Beibehaltung des Seitenverhältnisses aus. Dies kann dazu führen, dass ein Teil des Bilds abgeschnitten wird. Dieser Zustand ist mit Vollbildmodi vergleichbar.
-   [Fill](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) füllt den gesamten Platz aus, ohne das Seitenverhältnis beizubehalten. Das Bild wird nicht zugeschnitten, kann aber gestreckt werden. Dieser Zustand ist mit Streckmodi vergleichbar.

![Stretch-Enumerationswerte](images/Image_Stretch.jpg)

In diesem Beispiel werden die [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch)-Optionen mit einem [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) durchlaufen. Eine **switch**-Anweisung überprüft den aktuellen Zustand der [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.stretch)-Eigenschaft und legt diese auf den nächsten Wert in der **Stretch**-Aufzählung fest. So kann der Benutzer zwischen verschiedenen Streckungszuständen wechseln.

```xaml
<AppBarButton Icon="Switch"
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### <a name="enable-low-latency-playback"></a>Aktivieren der Wiedergabe mit geringer Wartezeit

Setzen Sie die Eigenschaft [RealTimePlayback](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.realtimeplayback) für ein [MediaPlayerElement.MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) auf **true**, damit das Media Player-Element die anfängliche Wartezeit für die Wiedergabe reduzieren kann. Dies ist von entscheidender Bedeutung für Apps mit bidirektionaler Kommunikation und kann für einige Spieleszenarien erforderlich sein. Beachten Sie, dass dieser Modus viele Ressourcen und eine höhere Leistung benötigt.

Das folgende Beispiel erstellt ein [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) uns setzt [RealTimePlayback](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.realtimeplayback) auf **true**.


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## <a name="recommendations"></a>Empfehlungen

MediaPlayer unterstützt helle und dunkle Designs. Dunkle Designs bieten für die Mehrzahl der Unterhaltungsszenarien jedoch eine bessere Umgebung. Der dunkle Hintergrund bietet einen besseren Kontrast, insbesondere bei schwierigen Lichtbedingungen, und verhindert Beeinträchtigungen der Steuerleiste auf der Benutzeroberfläche.

Unterstützen Sie beim Abspielen von Videoinhalten eine dedizierte Anzeigeumgebung, indem Sie den Vollbildmodus gegenüber dem Inlinemodus bevorzugen. Die Vollbildansicht ist optimal. Die Optionen sind im Inlinemodus eingeschränkt.

Wenn Sie die nötige Bildschirmfläche haben oder für 10 Fuß-Umgebungen entwerfen, sollten Sie sich für das Layout mit Doppelzeile entscheiden. Es bietet mehr Platz für Steuerelemente als das kompakte einzeilige Layout, und die Navigation mit dem Gamepad in 10-Fuß-Szenarien ist einfacher.

> **Hinweis**&nbsp;&nbsp; Im Artikel [Designing for Xbox and TV](../devices/designing-for-tv.md) finden Sie weitere Informationen zur Optimierung Ihrer Anwendung für 10-Fuß-Umgebungen.

Die Standardsteuerelemente wurden für die Medienwiedergabe optimiert. Sie können jedoch benutzerdefinierte Optionen hinzufügen, die Sie für den Media Player benötigen, um für Ihre App eine optimale Umgebung bereitzustellen. Weitere Informationen zum Hinzufügen von benutzerdefinierten Steuerelementen finden Sie unter [Erstellen benutzerdefinierter Transportsteuerelemente](custom-transport-controls.md).

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Befehlsdesigngrundlagen für UWP-Apps](https://docs.microsoft.com/windows/uwp/layout/commanding-basics)
- [Grundlagen von inhaltsdesign für UWP-apps](https://docs.microsoft.com/windows/uwp/layout/content-basics)
