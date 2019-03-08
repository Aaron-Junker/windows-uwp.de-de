---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: In diesem Artikel wird beschrieben, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, z. B. HDR-Video und Belichtungspriorität, zu ermöglichen.
title: Manuelle Kamerasteuerelemente für die Videoaufnahme
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f144ef398fc55e79d2f0190c61214cdf1aa93b68
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607655"
---
# <a name="manual-camera-controls-for-video-capture"></a>Manuelle Kamerasteuerelemente für die Videoaufnahme



In diesem Artikel wird beschrieben, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, z. B. HDR-Video und Belichtungspriorität, zu ermöglichen.

Die in diesem Artikel beschriebenen Steuerelemente des Videoaufnahmegeräts werden Ihrer App alle mithilfe desselben Musters hinzugefügt. Überprüfen Sie zunächst, ob das Steuerelement auf dem aktuellen Gerät unterstützt wird, auf dem Ihre App ausgeführt wird. Wenn das Steuerelement unterstützt wird, legen Sie den gewünschten Modus für das Steuerelement fest. Wenn ein bestimmtes Steuerelement auf dem aktuellen Gerät nicht unterstützt wird, sollten Sie das UI-Element, über das der Benutzer das Feature aktivieren kann, deaktivieren oder ausblenden.

Alle in diesem Artikel beschriebenen Gerätesteuerelement-APIs gehören dem [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902)-Namespace an.

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

## <a name="hdr-video"></a>HDR-Video

Die Videofunktion „High Dynamic Range“ (HDR) bezieht sich auf die HDR-Verarbeitung des Videostreams des Aufnahmegeräts. Ermitteln Sie, ob HDR-Video unterstützt wird, indem Sie die [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682)-Eigenschaft auswählen.

Das HDR-Videosteuerelement unterstützt drei Modi: ein, aus und automatisch. Dies bedeutet, dass das Gerät dynamisch ermittelt, ob die Medienaufnahme durch die HDR-Videoverarbeitung verbessert wird, und HDR-Video aktiviert, wenn dies der Fall ist. Um festzustellen, ob ein bestimmter Modus auf dem aktuellen Gerät unterstützt wird, überprüfen Sie, ob die [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683)-Auflistung den gewünschten Modus enthält.

Aktivieren oder deaktivieren Sie die HDR-Videoverarbeitung, in dem Sie [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) auf den gewünschten Modus festlegen.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>Belichtungspriorität

Wenn [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644) aktiviert ist, werden die Videoframes des Aufnahmegeräts ausgewertet, um zu bestimmen, ob in dem Video ein Motiv unter schlechten Lichtverhältnissen aufgenommen wird. Wenn dies der Fall ist, verringert das Steuerelement die Bildfrequenz des aufgenommenen Videos, um die Belichtungszeit für jeden Frame zu erhöhen und die visuelle Qualität des aufgenommenen Videos zu verbessern.

Ermitteln Sie, ob das Steuerelement für die Belichtungspriorität auf dem aktuellen Gerät unterstützt wird, indem Sie die [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647)-Eigenschaft überprüfen.

Aktivieren oder deaktivieren Sie das Steuerelement für die Belichtungspriorität, indem Sie [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) auf den gewünschten Modus festlegen.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="temporal-denoising"></a>Temporäre Entstörung
Ab Windows 10, Version 1803, können Sie die temporäre Entstörung für Videos auf Geräten verwenden, die dies unterstützen. Dieses Feature fixiert die Bilddaten aus mehreren benachbarten Frames in Echtzeit, und produziert Video-Frames mit weniger visuellen Störungen.

Mit [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) kann Ihre App bestimmen, ob die temporäre Entstörung auf dem aktuellen Gerät unterstützt wird und wenn Ja, welche temporäre Entstörungsmodi unterstützt werden. Denoising Modi sind verfügbar [ **aus**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), [ **auf**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), und [ **automatisch** ](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode). Ein Gerät möglicherweise nicht alle Modi unterstützt, aber jedes Gerät unterstützen muss entweder **automatisch** oder **auf** und **aus**.

Das folgende Beispiel verwendet eine einfache Benutzeroberfläche für Optionsfelder, damit der Benutzer zwischen den Entstörungsmodi wechseln kann.

[!code-cs[SnippetDenoiseXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetDenoiseXAML)]

In der folgenden Methode ist die Eigenschaft [**VideoTemporalDenoisingControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) festgelegt, um festzustellen, ob die temporäre Entstörung überhaupt auf dem aktuellen Gerät unterstützt wird. Wenn Ja, überprüfen Sie, dass **Aus** und **Auto** oder **Ein** unterstützt werden. In diesem Fall machen wir unsere Optionsfelder sichtbar. Als Nächstes werden die Schaltflächen **Auto** und **Ein** eingeblendet, wenn diese Methoden unterstützt werden.

[!code-cs[SnippetUpdateDenoiseCapabilities](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetUpdateDenoiseCapabilities)]

Im **Checked**-Ereignishandler für die Optionsfelder ist der Namen der Schaltfläche aktiviert und der entsprechende Modus festgelegt, indem die [**VideoTemporalDenoisingControl.Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode)-Eigenschaft festgelegt wird.

[!code-cs[SnippetDenoiseButtonChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseButtonChecked)]

### <a name="disabling-temporal-denoising-while-processing-frames"></a>Deaktivieren der temporären Entstörung bei der Verarbeitung von Frames
Video, das mit der temporären Entstörung verarbeitet wurde, kann für das menschliche Auge ansprechender wirken. Da die temporäre Entstörung sich auf die Konsistenz der Bilder auswirken kann und die Details der Frame verringern kann, können Apps, die Bildverarbeitung der Frames durchführen wie z. B. die Registrierung oder optische Zeichenerkennung, die Entstörung programmgesteuert deaktivieren, wenn die Bildverarbeitung aktiviert ist.

Im folgenden Beispiel wird bestimmt, welche Entstörungsmodi unterstützt werden. Diese Informationen werden in einigen Klassenvariablen gespeichert.

[!code-cs[SnippetDenoiseFrameReaderVars](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseFrameReaderVars)]

[!code-cs[SnippetDenoiseCapabilitiesForFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseCapabilitiesForFrameProcessing)]

Wenn die App die Frameverarbeitung aktiviert, wird der Entstörungsmodus auf **Aus** festgelegt, wenn dieser Modus unterstützt wird, damit die Frameverarbeitung unformatierte Frames ohne Entstörungsmodus verwenden kann.

[!code-cs[SnippetEnableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEnableFrameProcessing)]

Wenn die App die Bildverarbeitung deaktiviert, wird der Entstörungsmodus auf **Ein** oder **Auto** festgelegt, je nachdem, welcher Modus unterstützt wird.

[!code-cs[SnippetDisableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDisableFrameProcessing)]

Weitere Informationen über das Abrufen von Videoframes für die Bildverarbeitung finden Sie unter [Verarbeiten von Medienframes mit "MediaFrameReader"](process-media-frames-with-mediaframereader.md).

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Verarbeiten von Medien-Frames mit MediaFrameReader](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 




