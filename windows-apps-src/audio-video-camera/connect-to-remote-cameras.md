---
ms.assetid: ''
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit Remote Kameras herstellen und eine mediaframesourcegroup abrufen, um Frames von den einzelnen Kameras abzurufen.
title: Herstellen einer Verbindung mit Remotekameras
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c7b876cff994f775b770d22c103d27271047b269
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683633"
---
# <a name="connect-to-remote-cameras"></a>Herstellen einer Verbindung mit Remotekameras

In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit einem oder mehreren Remote Kameras herstellen und ein [**mediaframesourcegroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) -Objekt erhalten, mit dem Sie Frames von jeder Kamera lesen können. Weitere Informationen zum Lesen von Frames aus einer Medienquelle finden Sie unter [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md). Weitere Informationen zur Kopplung mit Geräten finden Sie unter [Pair Devices](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

> [!NOTE] 
> Die in diesem Artikel erläuterten Features sind nur ab Windows 10, Version 1903, verfügbar.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Erstellen einer devicewatcher-Klasse zum Überwachen von verfügbaren Remote Kameras

Die [**devicewatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) -Klasse überwacht die für Ihre APP verfügbaren Geräte und benachrichtigt Ihre APP, wenn Geräte hinzugefügt oder entfernt werden. Rufen Sie eine Instanz von **devicewatcher** durch Aufrufen von [**DeviceInformation. createwatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)ab, und übergeben Sie eine AQS-Zeichenfolge (Advanced Query Syntax), die den Typ der zu überwachenden Geräte identifiziert. Die AQS-Zeichenfolge, die Netzwerkkamera Geräte angibt, lautet wie folgt:

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> Die Hilfsmethode " [**mediaframesourcegroup. getdeviceselector**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) " gibt eine AQS-Zeichenfolge zurück, mit der lokal verbundene und Remote Netzwerkkameras überwacht werden. Wenn Sie nur Netzwerkkameras überwachen möchten, sollten Sie die oben gezeigte AQS-Zeichenfolge verwenden.


Wenn Sie den zurückgegebenen **devicewatcher** starten, indem Sie die [**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) -Methode aufrufen, wird das [**hinzugefügte**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) Ereignis für jede derzeit verfügbare Netzwerkkamera erhoben. Bis Sie den Watcher durch Aufrufen von [**Beenden**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop)beenden, wird das **hinzugefügte** Ereignis ausgelöst, wenn neue Netzwerkkamera Geräte verfügbar werden und das [**entfernte**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed) Ereignis ausgelöst wird, wenn ein Kamera Gerät nicht mehr verfügbar ist.

Die Ereignis Argumente, die an die **hinzugefügten** und **entfernten** Ereignishandler übergeben werden, sind ein Objekt vom Typ " [**toviceinformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) " bzw. " [**toviceinformationupdate**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationupdate) ". Jedes dieser Objekte verfügt über eine **ID** -Eigenschaft, bei der es sich um den Bezeichner der Netzwerkkamera handelt, für die das Ereignis ausgelöst wurde. Übergeben Sie diese ID an die [**mediaframesourcegroup. fromittel Async**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) -Methode, um ein [**mediaframesourcegroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) -Objekt abzurufen, das Sie zum Abrufen von Frames von der Kamera verwenden können.

## <a name="remote-camera-pairing-helper-class"></a>Hilfsklasse für Remote Kamera Kopplung

Das folgende Beispiel zeigt eine Hilfsklasse, die einen **devicewatcher** verwendet, um eine **ObservableCollection** von **mediaframesourcegroup** -Objekten zu erstellen und zu aktualisieren, um die Datenbindung an die Liste der Kameras zu unterstützen. Typische apps würden die **mediaframesourcegroup** in eine benutzerdefinierte Modell Klasse einschließen. Beachten Sie, dass die Hilfsklasse einen Verweis auf den [**coredispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) der APP beibehält und die Sammlung von Kameras innerhalb von Aufrufen von [**runasync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) aktualisiert, um sicherzustellen, dass die an die Auflistung gebundene Benutzeroberfläche im UI-Thread aktualisiert wird.

Außerdem wird in diesem Beispiel das [**devicewatcher. aktualisierte**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) -Ereignis zusätzlich zu den **hinzugefügten** und **entfernten** Ereignissen behandelt. Im **aktualisierten** Handler wird das zugehörige Remote Kamera Gerät aus entfernt und dann wieder der Sammlung hinzugefügt.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Zugehörige Themen

* [Kamera](camera.md)
* [Einfaches Foto, Video und Audioerfassung mit mediacapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Kamera Frames](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md)
 

 




