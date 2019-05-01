---
ms.assetid: ''
description: In diesem Artikel veranschaulicht remotekameras herstellen und eine MediaFrameSourceGroup abzurufenden Frames in jede Kamera zu erhalten.
title: Herstellen einer Verbindung remotekameras mit
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bc719b8dad2adef0542edf284d257846052eac21
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63789584"
---
# <a name="connect-to-remote-cameras"></a>Herstellen einer Verbindung remotekameras mit

In diesem Artikel erfahren Sie, wie Sie eine oder mehrere remotekameras herstellen und Abrufen einer [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) -Objekt, das Ihnen ermöglicht, Frames in jede Kamera zu lesen. Weitere Informationen zum Lesen von Frames aus eine Medienquelle, finden Sie unter [Verarbeiten von Medien-Frames mit MediaFrameReader](process-media-frames-with-mediaframereader.md). Weitere Informationen zu Kopplung mit Geräten, finden Sie unter [koppeln Geräte](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

> [!NOTE] 
> In diesem Artikel beschriebenen Features sind nur verfügbar, die ab Windows 10, Version 1903 sein.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Erstellen Sie eine Klasse DeviceWatcher für verfügbare remotekameras ansehen

Die [ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) Klasse überwacht die Geräte Ihrer app zur Verfügung, und Ihre app benachrichtigt, wenn Geräte hinzugefügt oder entfernt werden. Rufen Sie eine Instanz des **DeviceWatcher** durch Aufrufen von [ **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), und übergeben Sie eine Advanced Query Syntax (AQS)-Zeichenfolge, die den Typ des identifiziert Geräte, die Sie überwachen möchten. Die AQS-Zeichenfolge, die Netzwerkgeräte für die Kamera angeben, lautet wie folgt:

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> Die Hilfsmethode [ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) gibt eine AQS-Zeichenfolge, die lokal verbunden und remote Netzwerkkameras überwachen möchten. Um nur Netzwerkkameras überwachen zu können, sollten Sie die oben gezeigte AQS-Zeichenfolge verwenden.


Beim Starten der zurückgegebenen **DeviceWatcher** durch Aufrufen der [ **starten** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) -Methode, löst es das [ **Added** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) -Ereignis für jedes Netzwerk-Kamera, die derzeit verfügbar ist. Bis Sie durch Aufrufen die Watcher beenden [ **beenden**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop), **Added** Ereignis wird ausgelöst, wenn neue Kamera-Netzwerkgeräte verfügbar sind und die [ **Entfernt** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed) Ereignis wird ausgelöst, wenn ein Kameragerät nicht verfügbar ist.

Die Ereignis-Args übergeben wird, in der **Added** und **entfernt** Ereignishandler sind eine [ **DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) oder ein [  **DeviceInformationUpdate** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate) Zielabhängigkeitsobjekt. Jedes dieser Objekte verfügt eine **Id** Eigenschaft, die den Bezeichner für die Netzwerk-Kamera ist für die das Ereignis ausgelöst wurde. Übergeben Sie diese ID in der [ **MediaFrameSourceGroup.FromIdAsync** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) -Methode zum Abrufen einer [ **MediaFrameSourceGroup** ](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) -Objekt, das Sie zum verwenden können Abrufen von Frames von der Kamera.

## <a name="remote-camera-pairing-helper-class"></a>Ereignispaarbildung Remote Kamera-Hilfsklasse

Das folgende Beispiel zeigt eine Hilfsklasse, die verwendet eine **DeviceWatcher** erstellen und Aktualisieren einer **ObservableCollection** von **MediaFrameSourceGroup** Objekten zur Unterstützung der Datenbindung an die Liste der Kameras. Typische apps zu umschließen, würde die **MediaFrameSourceGroup** in eine benutzerdefinierte Modellklasse. Beachten Sie, dass die Hilfsklasse einen Verweis auf die app verwaltet [ **"coredispatcher"** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) und aktualisiert die Auflistung von Kameras in Aufrufe an [ **RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) um sicherzustellen, dass die Benutzeroberfläche, die an die Auflistung gebunden, die im UI-Thread aktualisiert wird.

In diesem Beispiel wird darüber hinaus behandelt der [ **DeviceWatcher.Updated** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) Ereignis zusätzlich zu der **Added** und **entfernt** Ereignisse. In der **aktualisierte** Handler auf, die zugehörigen remote Kamera aufgehoben und dann wieder in der Auflistung hinzugefügt wird.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Camera-frames](https://go.microsoft.com/fwlink/?LinkId=823230)
* [Verarbeiten von Medien-Frames mit MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 




