---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: In diesem Artikel wird beschrieben, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, z. B. HDR-Video und Belichtungspriorität, zu ermöglichen.
title: Manuelle Kamerasteuerelemente für die Videoaufnahme
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ac3a286a8e3961b66a8fd0e4cf20fa7665b6b081
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364013"
---
# <a name="manual-camera-controls-for-video-capture"></a>Manuelle Kamerasteuerelemente für die Videoaufnahme



In diesem Artikel wird beschrieben, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, z. B. HDR-Video und Belichtungspriorität, zu ermöglichen.

Die in diesem Artikel beschriebenen Steuerelemente des Videoaufnahmegeräts werden Ihrer App alle mithilfe desselben Musters hinzugefügt. Überprüfen Sie zunächst, ob das Steuerelement auf dem aktuellen Gerät unterstützt wird, auf dem Ihre App ausgeführt wird. Wenn das Steuerelement unterstützt wird, legen Sie den gewünschten Modus für das Steuerelement fest. Wenn ein bestimmtes Steuerelement auf dem aktuellen Gerät nicht unterstützt wird, sollten Sie das UI-Element, über das der Benutzer das Feature aktivieren kann, deaktivieren oder ausblenden.

Alle in diesem Artikel beschriebenen Gerätesteuerelement-APIs gehören dem [**Windows.Media.Devices**](/uwp/api/Windows.Media.Devices)-Namespace an.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

## <a name="hdr-video"></a>HDR-Video

Die Videofunktion „High Dynamic Range“ (HDR) bezieht sich auf die HDR-Verarbeitung des Videostreams des Aufnahmegeräts. Ermitteln Sie, ob HDR-Video unterstützt wird, indem Sie die [**HdrVideoControl.Supported**](/uwp/api/windows.media.devices.hdrvideocontrol.supported)-Eigenschaft auswählen.

Das HDR-Videosteuerelement unterstützt drei Modi: ein, aus und automatisch. Dies bedeutet, dass das Gerät dynamisch ermittelt, ob die Medienaufnahme durch die HDR-Videoverarbeitung verbessert wird, und HDR-Video aktiviert, wenn dies der Fall ist. Um festzustellen, ob ein bestimmter Modus auf dem aktuellen Gerät unterstützt wird, überprüfen Sie, ob die [**HdrVideoControl.SupportedModes**](/uwp/api/windows.media.devices.hdrvideocontrol.supportedmodes)-Auflistung den gewünschten Modus enthält.

Aktivieren oder deaktivieren Sie die HDR-Videoverarbeitung, in dem Sie [**HdrVideoControl.Mode**](/uwp/api/windows.media.devices.hdrvideocontrol.mode) auf den gewünschten Modus festlegen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetHdrVideoMode":::

## <a name="exposure-priority"></a>Belichtungspriorität

Wenn [**ExposurePriorityVideoControl**](/uwp/api/Windows.Media.Devices.ExposurePriorityVideoControl) aktiviert ist, werden die Videoframes des Aufnahmegeräts ausgewertet, um zu bestimmen, ob in dem Video ein Motiv unter schlechten Lichtverhältnissen aufgenommen wird. Wenn dies der Fall ist, verringert das Steuerelement die Bildfrequenz des aufgenommenen Videos, um die Belichtungszeit für jeden Frame zu erhöhen und die visuelle Qualität des aufgenommenen Videos zu verbessern.

Ermitteln Sie, ob das Steuerelement für die Belichtungspriorität auf dem aktuellen Gerät unterstützt wird, indem Sie die [**ExposurePriorityVideoControl.Supported**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.supported)-Eigenschaft überprüfen.

Aktivieren oder deaktivieren Sie das Steuerelement für die Belichtungspriorität, indem Sie [**ExposurePriorityVideoControl.Enabled**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.enabled) auf den gewünschten Modus festlegen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetEnableExposurePriority":::

## <a name="temporal-denoising"></a>Temporale Angabe
Ab Windows 10, Version 1803, können Sie die Temporale Angabe von Videos auf Geräten aktivieren, die diese unterstützen. Diese Funktion verbindet die Bilddaten aus mehreren angrenzenden Frames in Echtzeit, um Video Frames mit weniger visuellen Rauschen zu liefern.

Das [**videotemporaldenoisingcontrol-Steuer**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) Element ermöglicht der APP, zu bestimmen, ob Temporale Kennzeichnung auf dem aktuellen Gerät unterstützt wird. wenn dies der Fall ist, werden die angegebenen Modi unterstützt. Die verfügbaren kennzeichnenden Modi sind [**Off**](/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**on**](/uwp/api/windows.media.devices.videotemporaldenoisingmode)und [**Auto**](/uwp/api/windows.media.devices.videotemporaldenoisingmode). Ein Gerät unterstützt möglicherweise nicht alle Modi, aber jedes Gerät muss entweder " **Auto** " oder " **ein-und** **ausschalten**" unterstützen.

Im folgenden Beispiel wird eine einfache Benutzeroberfläche verwendet, um Options Felder bereitzustellen, die es dem Benutzer ermöglichen, zwischen den angegebenen Modi zu wechseln.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetDenoiseXAML":::

In der folgenden Methode wird die [**videotemporaldenoisingcontrol. Supported**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) -Eigenschaft geprüft, um festzustellen, ob Temporale Kennzeichnung auf dem aktuellen Gerät unterstützt wird. Wenn dies der Fall ist, überprüfen Sie, ob **Off** und **Auto** oder **on** unterstützt werden. in diesem Fall werden wir die Options Felder sichtbar machen. Im nächsten Schritt werden die Schaltflächen **Auto** und **on** sichtbar gemacht, wenn diese Methoden unterstützt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetUpdateDenoiseCapabilities":::

Im aktivierten **Ereignishandler** für die Options Felder wird der Name der Schaltfläche aktiviert und der entsprechende Modus durch Festlegen der [**videotemporaldenoisingcontrol. Mode**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode) -Eigenschaft festgelegt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseButtonChecked":::

### <a name="disabling-temporal-denoising-while-processing-frames"></a>Deaktivieren von temporalen kennzeichnen bei der Verarbeitung von Frames
Das Video, das mit Temporaler Bezeichnung verarbeitet wurde, kann für das menschliche Auge etwas angenehmer werden. Da Temporale kennzeichnen sich jedoch auf die Bild Konsistenz auswirken und die Menge der Details im Frame verringern kann, kann es sein, dass apps, die eine Bildverarbeitung in den Frames ausführen (z. b. Registrierung oder optische Zeichenerkennung), die Kennzeichnung Programm gesteuert deaktivieren möchten, wenn die Bildverarbeitung aktiviert ist.

Im folgenden Beispiel wird bestimmt, welche kennzeichnenden Modi unterstützt werden, und diese Informationen werden in einigen Klassen Variablen gespeichert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseFrameReaderVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseCapabilitiesForFrameProcessing":::

Wenn die APP Frame-Verarbeitung ermöglicht, wird der kennzeichnenden Modus auf **Off** festgelegt, wenn dieser Modus unterstützt wird, sodass die Frame Verarbeitung unformatierte Frames verwenden kann, die nicht angegeben wurden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEnableFrameProcessing":::

Wenn die APP Frame-Erzwingung deaktiviert, legt Sie den kennzeichnenden Modus auf **on** oder **Auto**fest, je nachdem, welcher Modus unterstützt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDisableFrameProcessing":::

Weitere Informationen zum Abrufen von Video Frames für die Bildverarbeitung finden Sie unter [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md).

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Verarbeiten von Medienframes mit „MediaFrameReader“](process-media-frames-with-mediaframereader.md)
*  [**Videotemporaldenoisingcontrol**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 
