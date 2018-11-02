---
author: drewbatgit
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: In diesem Artikel wird beschrieben, wie Sie Medien von einer universellen Windows-App für Remotegeräte umwandeln.
title: Medienumwandlung
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: da0bb4d25166dd62372d5902ff89221d20189c22
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "5991877"
---
# <a name="media-casting"></a>Medienumwandlung



In diesem Artikel wird beschrieben, wie Sie Medien von einer universellen Windows-App für Remotegeräte umwandeln.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Integrierte Medienumwandlung mit MediaPlayerElement

Die einfachste Methode zum Umwandeln von Medien aus einer universellen Windows-App ist die Verwendung der integrierten Umwandlungsfunktion des [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelements.

Um dem Benutzer das Öffnen einer wiederzugebenden Videodatei im **MediaPlayerElement**-Steuerelement zu ermöglichen, fügen Sie Ihrem Projekt die folgenden Namespaces hinzu.

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

Fügen Sie in der XAML-Datei der App ein **MediaPlayerElement** hinzu, und legen Sie [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) auf „true“ fest.

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

Fügen Sie eine Schaltfläche hinzu, über die der Benutzer die Auswahl einer Datei initiieren kann.

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

Erstellen Sie im [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737)-Ereignishandler für die Schaltfläche eine neue Instanz des [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Objekts, fügen Sie der [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850)-Sammlung Videodateitypen hinzu, und legen die Ausgangsposition auf die Videobibliothek des Benutzers fest.

Rufen Sie [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) auf, um das Dialogfeld für die Dateiauswahl zu starten. Diese Methode gibt ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt zurück, das die Videodatei darstellt. Stellen Sie sicher, dass die Datei nicht NULL ist; dies ist der Fall, wenn der Benutzer den Auswahlvorgang abbricht. Rufen Sie die [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227221.aspx)-Methode der Datei auf, um einen [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) für die Datei abzurufen. Abschließend erstellen Sie ein neues **MediaSource**-Objekt aus der ausgewählten Datei durch Aufrufen von [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909) und weisen es der [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source)-Eigenschaft des **MediaPlayerElement**-Objekts zu, um die Videodatei zur Videoquelle für das Steuerelement zu machen.

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

Nachdem das Video in das **MediaPlayerElement** geladen wurde, kann der Benutzer einfach die Umwandlungsschaltfläche in den Transportsteuerelementen wählen, um ein integriertes Dialogfeld zu öffnen, in dem er ein Gerät auswählen kann, für das die geladenen Medien umgewandelt werden.

![MediaElement-Umwandlungsschaltfläche](images/media-element-casting-button.png)

> [!NOTE] 
> Ab Windows10, Version1607, wird die Verwendung der **MediaPlayer**-Klasse zum Wiedergeben von Medienelementen empfohlen. **MediaPlayerElement** ist ein einfaches XAML-Steuerelement, das zum Rendern des Inhalts eines **MediaPlayer**-Objekts auf einer XAML-Seite verwendet wird. Das **MediaElement**-Steuerelement wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. Weitere Informationen zur Verwendung von **MediaPlayer** und **MediaPlayerElement** zum Wiedergeben von Medieninhalten finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md). Informationen zur Verwendung von **MediaSource** und dazugehörigen APIs für die Arbeit mit Medieninhalten finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Medienumwandlung mit CastingDevicePicker

Eine zweite Methode zum Umwandeln von Medien für ein Gerät ist die Verwendung der [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525)-Klasse. Zum Verwenden dieser Klasse schließen Sie den [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568)-Namespace in Ihr Projekt ein.

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

Deklarieren Sie eine Membervariable für das **CastingDevicePicker**-Objekt.

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

Wenn die Seite initialisiert wird, erstellen Sie eine neue Instanz der Umwandlungsauswahl, und legen Sie die [**Filter**](https://msdn.microsoft.com/library/windows/apps/dn972540)-Eigenschaft auf [**SupportsVideo**](https://msdn.microsoft.com/library/windows/apps/dn972526) fest, um anzugeben, dass die von der Auswahl aufgeführten Umwandlungsgeräte Video unterstützen müssen. Registrieren Sie einen Handler für das [**CastingDeviceSelected**](https://msdn.microsoft.com/library/windows/apps/dn972539)-Ereignis, das ausgelöst wird, wenn der Benutzer ein Gerät für die Umwandlung auswählt.

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

Fügen Sie in der XAML-Datei eine Schaltfläche hinzu, über die der Benutzer die Auswahl starten kann.

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

Rufen Sie im **Click**-Ereignishandler für die Schaltfläche [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/br208986) auf, um die Transformation eines UI-Elements relativ zu einem anderen Element abzurufen. In diesem Beispiel ist die Transformation die Position der Umwandlungsauswahl-Schaltfläche relativ zum visuellen Stamm des Anwendungsfensters. Rufen Sie die [**Show**](https://msdn.microsoft.com/library/windows/apps/dn972542)-Methode des [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525)-Objekts auf, um das Dialogfeld der Umwandlungsauswahl zu öffnen. Geben Sie die Position und die Abmessungen der Umwandlungsauswahl-Schaltfläche an, sodass das Dialogfeld als Flyout der vom Benutzer gewählten Schaltfläche angezeigt werden kann.

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

Rufen Sie im **CastingDeviceSelected**-Ereignishandler die [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547)-Methode der [**SelectedCastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972546)-Eigenschaft der Ereignisargumente auf, die das vom Benutzer ausgewählte Umwandlungsgerät darstellt. Registrieren Sie Handler für die Ereignisse [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) und [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523). Rufen Sie abschließend [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) auf, um die Umwandlung zu starten, indem das Ergebnis für die [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012)-Methode des **MediaPlayer**-Objekts des **MediaPlayerElement**-Steuerelements übergeben wird, um anzugeben, dass die Medien, die umgewandelt werden sollen, der Inhalt des **MediaPlayer** sind, der dem **MediaPlayerElement** zugeordnet ist.

> [!NOTE] 
> Die Umwandlungsverbindung muss im UI-Thread initiiert werden. Da **CastingDeviceSelected** nicht für den UI-Thread aufgerufen wird, müssen Sie diese Aufrufe innerhalb eines Aufrufs von [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) platzieren, sodass diese für den UI-Thread aufgerufen werden.

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

Aktualisieren Sie die Benutzeroberfläche in den Ereignishandlern **ErrorOccurred** und **StateChanged**, um den Benutzer über den aktuellen Status der Umwandlung zu informieren. Diese Ereignisse werden im folgenden Abschnitt zum Erstellen einer benutzerdefinierten Umwandlungsgeräteauswahl ausführlich erläutert.

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>Medienumwandlung mit einer benutzerdefinierten Geräteauswahl

Im folgenden Abschnitt wird beschrieben, wie Sie ein eigenes Benutzeroberflächenelement zur Umwandlungsgeräteauswahl erstellen, indem Sie die Umwandlungsgeräte aufzählen und die Verbindung im Code initiieren.

Um die verfügbaren Umwandlungsgeräte aufzuzählen, schließen Sie den [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459)-Namespace in Ihr Projekt ein.

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

Fügen Sie der XAML-Seite die folgenden Steuerelemente hinzu, um die einfache Benutzeroberfläche für dieses Beispiel zu implementieren:

-   Eine Schaltfläche zum Starten des Geräteüberwachungselements, das nach verfügbaren Umwandlungsgeräten sucht.
-   Ein [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)-Steuerelement, das den Benutzer über die laufende Umwandlungsenumeration informiert.
-   Ein [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) zum Auflisten der erkannten Umwandlungsgeräte. Definieren Sie eine [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) für das Steuerelement, sodass die Umwandlungsgeräteobjekte direkt dem Steuerelement zugewiesen werden können und die [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549)-Eigenschaft trotzdem weiterhin angezeigt wird.
-   Eine Schaltfläche, über die der Benutzer das Umwandlungsgerät trennen kann.

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

Deklarieren Sie im CodeBehind Membervariablen für [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) und [**CastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972510).

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

Aktualisieren Sie im **Click**-Handler für die *startWatcherButton* zuerst die Benutzeroberfläche, indem Sie die Schaltfläche deaktivieren und das Statussignal aktivieren, während die Geräteenumeration ausgeführt wird. Löschen Sie den Inhalt des Listenfelds mit Umwandlungsgeräten.

Erstellen Sie als Nächstes ein Geräteüberwachungselement durch Aufruf von [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427). Diese Methode kann verwendet werden, um viele verschiedene Arten von Geräten zu überwachen. Geben Sie mithilfe der von [**CastingDevice.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn972551) zurückgegebenen Geräteauswahlzeichenfolge an, dass Sie Geräte überwachen möchten, die die Videoumwandlung unterstützen.

Registrieren Sie abschließend Ereignishandler für die Ereignisse [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450), [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453), [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) und [**Stopped**](https://msdn.microsoft.com/library/windows/apps/br225457).

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

Das **Added**-Ereignis wird ausgelöst, wenn ein neues Gerät vom Überwachungselement erkannt wird. Erstellen Sie im Handler für dieses Ereignis ein neues [**CastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972524)-Objekt, indem Sie [**CastingDevice.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn972550) aufrufen und die ID des erkannten Umwandlungsgeräts übergeben, die in dem an den Handler übergebenen **DeviceInformation**-Objekt enthalten ist.

Fügen Sie das **CastingDevice** zum **ListBox** für Umwandlungsgeräte hinzu, damit der Benutzer es auswählen kann. Aufgrund der in XAML definierten [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830) wird die [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549)-Eigenschaft als Elementtext im Listenfeld verwendet. Da dieser Ereignishandler nicht für den UI-Thread aufgerufen wird, müssen Sie die Benutzeroberfläche innerhalb eines Aufrufs von [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) aktualisieren.

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

Das **Removed**-Ereignis wird ausgelöst, wenn das Überwachungselement erkennt, dass ein Umwandlungsgerät nicht mehr vorhanden ist. Vergleichen Sie die ID-Eigenschaft des **Added**-Objekts, die an den Handler übergeben wird, mit der ID jedes **Added**-Objekts in der [**Items**](https://msdn.microsoft.com/library/windows/apps/br242823)-Sammlung des Listenfelds. Wenn die ID übereinstimmt, entfernen Sie das Objekt aus der Sammlung. Zur Erinnerung: Da die Benutzeroberfläche aktualisiert wird, muss dieser Aufruf innerhalb eines **RunAsync**-Aufrufs erfolgen.

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

Das **EnumerationCompleted**-Ereignis wird ausgelöst, wenn das Überwachungselement die Erkennung von Geräten abgeschlossen hat. Aktualisieren Sie im Handler für dieses Ereignis die Benutzeroberfläche, um den Benutzer darüber zu informieren, dass die Enumeration abgeschlossen ist, und beenden Sie das Geräteüberwachungselement durch Aufruf von [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456).

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

Das Stopped-Ereignis wird ausgelöst, wenn das Beenden des Geräteüberwachungselements abgeschlossen ist. Beenden Sie im Handler für dieses Ereignis das [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538)-Steuerelement, und aktivieren Sie die *startWatcherButton* wieder, damit der Benutzer den Vorgang der Geräteenumeration neu starten kann.

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

Wenn der Benutzer eines der Umwandlungsgeräte im Listenfeld auswählt, wird das [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776)-Ereignis ausgelöst. Innerhalb dieses Handlers werden die Umwandlungsverbindung erstellt und die Umwandlung gestartet.

Stellen Sie zunächst sicher, dass das Geräteüberwachungselement beendet wurde, sodass kein Konflikt zwischen Geräteenumeration und Medienumwandlung auftritt. Erstellen Sie eine Umwandlungsverbindung, indem Sie [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) für das vom Benutzer ausgewählte **CastingDevice**-Objekt aufrufen. Fügen Sie Ereignishandler für die Ereignisse [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) und [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) hinzu.

Starten Sie die Medienumwandlung durch Aufrufen von [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520), und übergeben Sie dabei die Umwandlungsquelle, die durch den Aufruf der **MediaPlayer**-Methode [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) zurückgegeben wurde. Machen Sie zum Schluss die Schaltfläche zum Trennen sichtbar, damit der Benutzer die Medienumwandlung beenden kann.

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

Im Ereignishandler für geänderten Zustand hängt die ausgeführte Aktion vom neuen Zustand der Umwandlungsverbindung ab:

-   Beim Zustand **Connected** oder **Rendering** stellen Sie sicher, dass das **ProgressRing**-Steuerelement inaktiv ist und die Schaltfläche zum Trennen angezeigt wird.
-   Beim Zustand **Disconnected** heben Sie die Auswahl des aktuellen Umwandlungsgerät im Listenfeld auf, deaktivieren Sie das **ProgressRing**-Steuerelement, und blenden Sie die Schaltfläche zum Trennen aus.
-   Beim Zustand **Connecting** aktivieren Sie das **ProgressRing**-Steuerelement, und blenden Sie die Schaltfläche zum Trennen aus.
-   Beim Zustand **Disconnecting** aktivieren Sie das **ProgressRing**-Steuerelement, und blenden Sie die Schaltfläche zum Trennen aus.

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

Aktualisieren Sie im Handler für das **ErrorOccurred**-Ereignis die Benutzeroberfläche, um den Benutzer darüber zu informieren, dass ein Umwandlungsfehler aufgetreten ist, und heben Sie die Auswahl des aktuellen **CastingDevice**-Objekts im Listenfeld auf.

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

Implementieren Sie zum Schluss den Handler für die Schaltfläche zum Trennen. Beenden Sie die Medienumwandlung, und trennen Sie die Verbindung mit dem Umwandlungsgerät, indem Sie die [**DisconnectAsync**](https://msdn.microsoft.com/library/windows/apps/dn972518)-Methode des **CastingConnection**-Objekts aufrufen. Dieser Aufruf muss durch Aufrufen von [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) an den UI-Thread weitergeleitet werden.

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




