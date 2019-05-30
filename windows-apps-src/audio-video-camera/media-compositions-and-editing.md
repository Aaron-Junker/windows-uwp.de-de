---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Mithilfe der APIs im Windows.Media.Editing-Namespace können Sie schnell Apps entwickeln, die Benutzern das Erstellen von Medienkompositionen aus Audio- und Videoquelldateien ermöglichen.
title: Medienkompositionen und -bearbeitung
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4054f1f4ce4db7f158c1297b748ecea8cab83602
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360742"
---
# <a name="media-compositions-and-editing"></a>Medienkompositionen und -bearbeitung



In diesem Artikel wird beschrieben, wie Sie mithilfe der APIs im [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing)-Namespace schnell Apps entwickeln, die Benutzern das Erstellen von Medienkompositionen aus Audio- und Videoquelldateien ermöglichen. Zu den Features des Frameworks zählen die Möglichkeit, programmgesteuert mehrere Videoclips zusammen anzufügen, Video- und Bildüberlagerungen sowie Hintergrundaudio hinzuzufügen und Audio- und Videoeffekte anzuwenden. Nach der Erstellung können Medienkompositionen zur Wiedergabe oder Freigabe in eine Medienflatfile gerendert werden; alternativ können sie auch auf einen Datenträger serialisiert und von diesem deserialisiert werden, sodass die Benutzer Kompositionen laden und ändern können, die sie zuvor erstellt haben. Alle diese Funktionen werden in einer benutzerfreundlichen Windows-Runtime-Schnittstelle bereitgestellt, die den Umfang und die Komplexität des zum Ausführen dieser Aufgaben erforderlichen Codes im Vergleich zur [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) -Low-Level-API erheblich verringert.

## <a name="create-a-new-media-composition"></a>Erstellen einer neuen Medienkomposition

Die [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition)-Klasse ist der Container für alle Medienclips, aus denen die Komposition besteht, und dient zum Rendern der endgültigen Komposition, zum Laden und Speichern von Kompositionen auf Datenträger und zum Bereitstellen eines Vorschaudatenstroms der Komposition zur Anzeige in der Benutzeroberfläche. Um **MediaComposition** in einer App zu verwenden, schließen Sie den [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing)-Namespace und den [**Windows.Media.Core**](https://docs.microsoft.com/uwp/api/Windows.Media.Core)-Namespace ein, der zugehörige benötigte APIs bereitstellt.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


Auf das **MediaComposition**-Objekt wird von mehreren Punkten im Code aus zugegriffen. Daher deklarieren Sie normalerweise eine Membervariable, um das Objekt darin zu speichern.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Der Konstruktor für **MediaComposition** akzeptiert keine Argumente.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>Hinzufügen von Medienclips zu einer Komposition

Medienkompositionen enthalten in der Regel einen oder mehrere Videoclips. Sie können ein [**FileOpenPicker**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker)-Element verwenden, um Benutzern die Auswahl einer Videodatei zu ermöglichen. Nachdem die Datei ausgewählt wurde, erstellen Sie ein neues [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip)-Objekt als Container für den Videoclip, indem Sie [**MediaClip.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync) aufrufen. Anschließend fügen Sie den Clip der [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips)-Liste des **MediaComposition**-Objekts hinzu.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Medienclips werden in der **MediaComposition** in der gleichen Reihenfolge angezeigt wie in der [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips)-Liste.

-   Ein **MediaClip** kann nur einmal in eine Komposition eingeschlossen werden. Der Versuch, einen **MediaClip** hinzuzufügen, der bereits von der Komposition verwendet wird, führt zu einem Fehler. Um einen Videoclip mehrmals in einer Komposition wiederzuverwenden, rufen Sie zum Erstellen neuer **MediaClip**-Objekte [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) auf, die dann der Komposition hinzugefügt werden können.

-   Universelle Windows-Apps sind nicht zum Zugreifen auf das gesamte Dateisystem berechtigt. Die [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist)-Eigenschaft der [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions)-Klasse ermöglicht der App, einen Datensatz einer Datei zu speichern, die vom Benutzer ausgewählt wurde, sodass Sie Berechtigungen für den Dateizugriff beibehalten können. Die **FutureAccessList** kann maximal 1000 Einträge enthalten; die App muss die Liste daher verwalten, um sicherzustellen, dass sie nicht zu voll wird. Dies ist besonders wichtig, wenn Sie das Laden und Ändern zuvor erstellter Kompositionen unterstützen möchten.

-   Eine **MediaComposition** unterstützt Videoclips in MP4-Format.

-   Wenn eine Videodatei mehrere eingebettete Audiotitel enthält, können Sie durch Festlegen der [**SelectedEmbeddedAudioTrackIndex**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex)-Eigenschaft auswählen, welcher Audiotitel in der Komposition verwendet werden soll.

-   Um einen **MediaClip** mit einer einzigen Farbfüllung für den gesamten Frame zu erstellen, rufen Sie [**CreateFromColor**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromcolor) auf, und geben Sie eine Farbe und eine Dauer für den Clip an.

-   Um einen **MediaClip** aus einer Bilddatei zu erstellen, rufen Sie [**CreateFromImageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) auf, und geben Sie eine Bilddatei und eine Dauer für den Clip an.

-   Um einen **MediaClip** aus einer [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) zu erstellen, rufen Sie [**CreateFromSurface**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromsurface) auf, und geben Sie eine Oberfläche und eine Dauer für den Clip an.

## <a name="preview-the-composition-in-a-mediaelement"></a>Anzeigen einer Vorschau der Komposition in einem MediaElement

Damit der Benutzer die Medienkomposition anzeigen kann, fügen Sie der XAML-Datei, die die Benutzeroberfläche definiert, ein [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) hinzu.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Deklarieren Sie eine Membervariable vom Typ [**MediaStreamSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaStreamSource).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Rufen Sie für das **MediaComposition**-Objekt die [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource)-Methode auf, um eine **MediaStreamSource** für die Komposition zu erstellen. Erstellen Sie ein [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource)-Objekt durch Aufrufen der Factorymethode [**CreateFromMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediastreamsource) und weisen Sie diese der Eigenschaft [**Quelle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) Eigenschaft des **MediaPlayerElement** zu. Jetzt kann die Komposition in der Benutzeroberfläche angezeigt werden.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   Die **MediaComposition** muss mindestens einen Medienclip enthalten, bevor [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) aufgerufen wird; andernfalls ist das zurückgegebene Objekt NULL.

-   Die **MediaElement**-Zeitachse wird nicht automatisch mit den Änderungen an der Komposition aktualisiert. Es wird empfohlen, dass Sie beide rufen **GeneratePreviewMediaStreamSource** und legen Sie die **MediaPlayerElement** **Quelle** Eigenschaft jedes Mal, wenn Sie eine Gruppe von Änderungen machen die Komposition und möchten die Benutzeroberfläche zu aktualisieren.

Es wird empfohlen, das **MediaStreamSource**-Objekt und die [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.source)-Eigenschaft von **MediaPlayerElement** auf Null festzulegen, wenn der Benutzer die Seite verlässt, um die zugehörigen Ressourcen freizugeben.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>Rendern der Komposition in eine Videodatei

Um eine Medienkomposition in eine Videoflatfile zu rendern, sodass sie freigegeben und auf anderen Geräten angezeigt werden kann, müssen Sie APIs aus dem [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding)-Namespace verwenden. Um die Benutzeroberfläche mit dem Fortschritt des asynchronen Vorgangs zu aktualisieren, benötigen Sie außerdem APIs aus dem [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)-Namespace.

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Nachdem Sie dem Benutzer die Auswahl einer Ausgabedatei mit einem [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)-Element erlaubt haben, rendern Sie die Komposition in die ausgewählte Datei, indem Sie die [**RenderToFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.rendertofileasync)-Methode des **MediaComposition**-Objekts aufrufen. Der Rest des Codes im folgenden Beispiel folgt einfach dem Muster zum Behandeln von [**AsyncOperationWithProgress**](https://docs.microsoft.com/previous-versions//br205807(v=vs.85)).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   [  **MediaTrimmingPreference**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) ermöglicht es Ihnen, der Geschwindigkeit des Transcodierungvorgangs Vorrang vor der Genauigkeit der Kürzung benachbarter Medienclips zu geben. **Fast** bewirkt eine schnellere Transcodierung mit weniger genauer Kürzung, **Precise** bewirkt eine langsamere Transcodierung mit genauerer Kürzung.

## <a name="trim-a-video-clip"></a>Kürzen eines Videoclips

Um die Dauer eines Videoclips in einer Komposition zu kürzen, legen Sie die [**TrimTimeFromStart**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromstart)-Eigenschaft, die [**TrimTimeFromEnd**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromend)-Eigenschaft (oder beide) des [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip)-Objekts fest.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Sie können jede beliebige Benutzeroberfläche verwenden, um dem Benutzer die Angabe des Start- und Endwerts für die Kürzung zu ermöglichen. Das obige Beispiel verwendet die [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position)-Eigenschaft der [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) zu dem **MediaPlayerElement**, um festzustellen welcher **MediaClip** an der aktuellen Position in der Komposition abgespielt wird; dazu werden [**StartTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) und [**EndTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.endtimeincomposition) überprüft. Anschließend werden die Eigenschaften **Position** und **StartTimeInComposition** erneut verwendet, um die Zeitspanne zu berechnen, um die der Clipanfang gekürzt werden muss. Die **FirstOrDefault**-Methode ist eine Erweiterungsmethode aus dem **System.Linq**-Namespace, die den Code zur Auswahl von Elementen aus einer Liste vereinfacht.
-   Mit der [**OriginalDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.originalduration)-Eigenschaft des **MediaClip**-Objekts können Sie die Dauer des Medienclips ermitteln, ohne dass eine Kürzung angewendet wird.
-   Die [**TrimmedDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimmedduration)-Eigenschaft informiert Sie über die Dauer des Medienclips nach der Kürzung.
-   Wenn Sie einen Kürzungswert angeben, der größer als die ursprüngliche Dauer des Clips ist, wird kein Fehler ausgelöst. Wenn jedoch eine Komposition nur einen einzigen Clip enthält und dieser durch Angabe eines hohen Kürzungswerts auf die Länge null gekürzt wird, wird bei einem nachfolgenden Aufruf von [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) NULL zurückgegeben, so als enthielte die Komposition keine Clips.

## <a name="add-a-background-audio-track-to-a-composition"></a>Hinzufügen eines Hintergrundaudiotitels zu einer Komposition

Um einer Komposition einen Hintergrundtitel hinzuzufügen, laden Sie eine Audiodatei, und erstellen Sie dann durch Aufrufen der [**BackgroundAudioTrack.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync)-Factorymethode ein [**BackgroundAudioTrack**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.BackgroundAudioTrack)-Objekt. Fügen Sie **BackgroundAudioTrack** anschließend zur [**BackgroundAudioTracks**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks)-Eigenschaft der Komposition hinzu.

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Ein **MediaComposition** unterstützt Hintergrund Audiospuren in den folgenden Formaten: MP3, WAV, FLAC

-   Ein Hintergrundaudiotitel

-   Wie bei Videodateien sollten Sie die [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions)-Klasse verwenden, um den Zugriff auf Dateien in der Komposition zu bewahren.

-   Ebenso wie ein **MediaClip** kann auch ein **BackgroundAudioTrack**-Objekt nur einmal in eine Komposition eingeschlossen werden. Der Versuch, einen **BackgroundAudioTrack** hinzuzufügen, der bereits von der Komposition verwendet wird, führt zu einem Fehler. Um einen Audiotitel mehrmals in einer Komposition wiederzuverwenden, rufen Sie [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) zum Erstellen neuer **MediaClip**-Objekte auf, die dann der Komposition hinzugefügt werden können.

-   Standardmäßig beginnt die Wiedergabe von Hintergrundaudiotiteln am Anfang der Komposition. Wenn mehrere Hintergrundtitel vorhanden sind, wird die Wiedergabe aller Titel am Anfang der Komposition gestartet. Damit die Wiedergabe eines Hintergrundaudiotitels zu einem anderen Zeitpunkt beginnt, legen Sie die [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.delay)-Eigenschaft auf die gewünschte Zeitverschiebung fest.

## <a name="add-an-overlay-to-a-composition"></a>Hinzufügen einer Überlagerung zu einer Komposition

Mithilfe von Überlagerungen können Sie mehrere Videoebenen in einer Komposition übereinander stapeln. Eine Komposition kann mehrere Überlagerungsebenen enthalten, die wiederum jeweils mehrere Überlagerungen enthalten können. Erstellen Sie ein [**MediaOverlay**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlay)-Objekt, indem Sie einen **MediaClip** im Konstruktor übergeben. Legen Sie die Position und die Deckkraft der Überlagerung fest, erstellen Sie anschließend eine neue [**MediaOverlayLayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlayLayer), und fügen Sie das **MediaOverlay**-Objekt der [**Overlays**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays)-Liste hinzu. Fügen Sie zum Schluss die **MediaOverlayLayer** zur [**OverlayLayers**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.overlaylayers)-Liste der Komposition hinzu.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Überlagerungen innerhalb einer Ebene werden basierend auf ihrer Reihenfolge in der **Overlays**-Liste der enthaltenden Ebene in Z-Reihenfolge sortiert. Höhere Indizes innerhalb der Liste werden auf niedrigeren Indizes gerendert. Dies gilt auch für Überlagerungsebenen innerhalb einer Komposition. Eine Ebene mit höherem Index in der **OverlayLayers**-Liste der Komposition wird auf niedrigeren Indizes gerendert.

-   Da Überlagerungen übereinander gestapelt statt nacheinander wiedergegeben werden, beginnt die Wiedergabe alle Überlagerungen standardmäßig am Anfang der Komposition. Damit die Wiedergabe einer Überlagerung zu einem anderen Zeitpunkt beginnt, legen Sie die [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaoverlay.delay)-Eigenschaft auf die gewünschte Zeitverschiebung fest.

## <a name="add-effects-to-a-media-clip"></a>Hinzufügen von Effekten zu einem Medienclip

Jeder **MediaClip** in einer Komposition enthält eine Liste von Audio- und Videoeffekten, der mehrere Effekte hinzugefügt werden können. Die Effekte müssen [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) bzw. [**IVideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) implementieren. Im folgenden Beispiel wird anhand der aktuellen **MediaPlayerElement**-Position der aktuell angezeigte **MediaClip** gewählt und dann eine neue Instanz der [**VideoStabilizationEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) erstellt und an die [**VideoEffectDefinitions**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions)-Liste des Medienclips angefügt.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>Speichern einer Komposition in einer Datei

Medienkompositionen können in eine Datei serialisiert werden, um sie zu einem späteren Zeitpunkt zu ändern. Wählen Sie eine Ausgabedatei aus, und rufen Sie dann die [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition)-Methode [**SaveAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.saveasync) auf, um die Komposition zu speichern.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>Laden einer Komposition aus einer Datei

Medienkompositionen können aus einer Datei deserialisiert werden, sodass der Benutzer die Komposition anzeigen und ändern kann. Wählen Sie eine Kompositionsdatei aus, und rufen Sie dann die [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition)-Methode [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.loadasync) auf, um die Komposition zu laden.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Wenn eine Mediendatei in der Komposition an einem Speicherort abgelegt ist, auf den Ihre App nicht zugreifen kann, und nicht in der [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist)-Eigenschaft der [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions)-Klasse für Ihre App enthalten ist, tritt beim Laden der Komposition ein Fehler auf.

 

 




