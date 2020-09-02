---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: In diesem Artikel wird beschrieben, wie Sie die AdvancedPhotoCapture-Klasse verwenden, um HDR-Fotos (High Dynamic Range) und Fotos bei schlechten Lichtverhältnissen aufzunehmen.
title: HDR-Fotoaufnahmen (High Dynamic Range) und Fotoaufnahmen bei schlechten Lichtverhältnissen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2072f1e7fad5c9652200fe067de8abe0afaede2a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362653"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>HDR-Fotoaufnahmen (High Dynamic Range) und Fotoaufnahmen bei schlechten Lichtverhältnissen



Dieser Artikel beschreibt, wie Sie die [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse zum Aufnehmen von HDR-Fotos (High Dynamic Range) verwenden. Mithilfe dieser API können Sie außerdem einen Referenzframe aus der HDR-Aufnahme abrufen, bevor die Verarbeitung des endgültigen Bilds abgeschlossen ist.

Die folgenden Artikel beziehen sich ebenfalls auf HDR-Aufnahmen:

-   Sie können den [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) verwenden, damit das System den Inhalt des Vorschaudatenstroms der Medienaufnahme auswerten kann, um zu ermitteln, ob das Aufnahmeergebnis durch eine HDR-Verarbeitung verbessert werden kann. Weitere Informationen finden Sie im Artikel [Szenenanalyse für die Medienaufnahme](scene-analysis-for-media-capture.md) (MediaCapture).

-   Verwenden Sie die [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl)-Klasse für Videoaufnahmen mit dem integrierten Windows-HDR-Verarbeitungsalgorithmus. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](capture-device-controls-for-video-capture.md).

-   Mit der [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture)-Klasse können Sie eine Abfolge von Fotos unter Verwendung unterschiedlicher Aufnahmeeinstellungen für jedes Foto aufnehmen und Ihren eigenen HDR- oder einen anderen Verarbeitungsalgorithmus implementieren. Weitere Informationen finden Sie unter [Variable Photo Sequence](variable-photo-sequence.md).



> [!NOTE] 
> Beginnend mit Windows 10, Version 1709, wird das Aufzeichnen von Videos und die Verwendung von **advancedphotocapture** gleichzeitig unterstützt.  Dies wird in früheren Versionen nicht unterstützt. Diese Änderung bedeutet, dass Sie gleichzeitig ein vorbereitetes **[lowlagmediarecording](/uwp/api/windows.media.capture.lowlagmediarecording)** und **[advancedphotocapture](/uwp/api/windows.media.capture.advancedphotocapture)** haben können. Sie können die Videoaufzeichnung zwischen Aufrufen von **[mediacapture. prepareadvancedphotocaptureasync](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** und **[advancedphotocapture. finishasync](/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)** starten oder anhalten. Sie können auch **[advancedphotocapture. captureasync](/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** aufrufen, während das Videoaufzeichnung ist. Einige **advancedphotocapture** -Szenarien, wie z. b. das Aufzeichnen eines HDR-Fotos beim Aufzeichnen von Videos, bewirken jedoch, dass einige Video Frames von der HDR-Erfassung geändert werden, was zu einer negativen Benutzer Leistung führt. Aus diesem Grund unterscheidet sich die Liste der Modi, die von " **[advancedphotocontrol. supportedmodes](/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** " zurückgegeben werden, während das Video aufgezeichnet wird. Sie sollten diesen Wert sofort nach dem Starten oder Beenden der Videoaufzeichnung überprüfen, um sicherzustellen, dass der gewünschte Modus im aktuellen Video Aufzeichnungsstatus unterstützt wird.


> [!NOTE] 
> Ab Windows 10, Version 1709, wenn **advancedphotocapture** auf den HDR-Modus festgelegt ist, wird die Einstellung der Eigenschaft " [**Flash Control. aktiviert**](/uwp/api/windows.media.devices.flashcontrol.enabled) " ignoriert, und der Flash wird nie ausgelöst. Bei anderen Erfassungs Modi, wenn das **Flash Control. aktiviert**ist, werden die **advancedphotocapture** -Einstellungen außer Kraft gesetzt, und es wird ein normales Foto mit Flash aufgezeichnet. Wenn " [**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto) " auf "true" festgelegt ist, kann " **advancedphotocapture** " Flash verwenden, abhängig vom Standardverhalten des Kamera Treibers für die Bedingungen in der aktuellen Szene. In früheren Versionen überschreibt die Einstellung " **advancedphotocapture** " immer die Einstellung " **Flash Control. aktiviert** ".

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

Es ist ein universelles Windows-Beispiel verfügbar, das die Verwendung der **AdvancedPhotoCapture**-Klasse veranschaulicht. Sie können dieses Beispiel verwenden, um sich die API im Kontext anzusehen, oder es als Ausgangspunkt für Ihre eigene App nutzen. Weitere Informationen finden Sie im [Beispiel für erweiterte Kameraaufnahmen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture).

## <a name="advanced-photo-capture-namespaces"></a>Namespaces für erweiterte Fotoaufnahmen

Neben den erforderlichen Namespaces für die grundlegende Medienaufnahme werden in den Codebeispielen in diesem Artikel APIs in den folgenden Namespaces verwendet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHDRPhotoUsing":::

## <a name="hdr-photo-capture"></a>HDR-Fotoaufnahme

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>Ermitteln, ob die HDR-Fotoaufnahme vom aktuellen Gerät unterstützt wird

Das in diesem Artikel beschriebene HDR-Aufnahmeverfahren wird mithilfe des [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Objekts ausgeführt. Nicht alle Geräte unterstützen die HDR-Aufnahme mit **AdvancedPhotoCapture**. Ermitteln Sie, ob das Gerät, auf dem Ihre App derzeit ausgeführt wird, dieses Verfahren unterstützt, indem Sie zunächst den [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) des **MediaCapture**-Objekts und anschließend die [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl)-Eigenschaft abrufen. Überprüfen Sie die [**supportedmodes**](/uwp/api/windows.media.devices.advancedphotocontrol.supportedmodes) -Auflistung des Videogeräte Controllers, um festzustellen, ob Sie [**advancedphoto ode. HDR**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode)enthält. Wenn dies der Fall ist, wird die HDR-Erfassung mit **advancedphotocapture** unterstützt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHdrSupported":::

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>Konfigurieren und Vorbereiten des AdvancedPhotoCapture-Objekts

Da Sie von mehreren Stellen innerhalb des Codes aus auf die [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Instanz zugreifen müssen, deklarieren Sie eine Membervariable als Container für das Objekt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareAdvancedCapture":::

Erstellen Sie in Ihrer APP nach dem Initialisieren des **mediacapture** -Objekts ein [**advancedphotocapturesettings**](/uwp/api/Windows.Media.Devices.AdvancedPhotoCaptureSettings) -Objekt, und legen Sie den Modus auf [**advancedphotomode. HDR**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode)fest. Nennen Sie die [**configure**](/uwp/api/windows.media.devices.advancedphotocontrol.configure) -Methode des [**advancedphotocontrol**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl) -Objekts, und übergeben Sie das von Ihnen erstellte **advancedphotocapturesettings** -Objekt.

Rufen Sie die [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)-Methode des **MediaCapture**-Objekts auf, und übergeben Sie ein [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt, das den von der Aufnahme zu verwendenden Codierungstyp angibt. Die **ImageEncodingProperties**-Klasse enthält statische Methoden zum Erstellen der Bildcodierungen, die vom **MediaCapture**-Objekt unterstützt werden.

**PrepareAdvancedPhotoCaptureAsync** gibt das [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Objekt zurück, mit dem Sie die Fotoaufnahme initiieren. Sie können dieses Objekt zum Registrieren von Handlern für die Ereignisse [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) und [**AllPhotosCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.allphotoscaptured) verwenden, die später in diesem Artikel erläutert werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateAdvancedCaptureAsync":::

### <a name="capture-an-hdr-photo"></a>Aufnehmen eines HDR-Fotos

Nehmen Sie ein HDR-Foto auf, indem Sie die [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync)-Methode des [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Objekts aufrufen. Diese Methode gibt ein [**AdvancedCapturedPhoto**](/uwp/api/Windows.Media.Capture.AdvancedCapturedPhoto)-Objekt zurück, das das aufgenommene Foto in seiner [**Frame**](/uwp/api/windows.media.capture.advancedcapturedphoto.frame)-Eigenschaft angibt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureHdrPhotoAsync":::

Die meisten Foto-Apps versuchen, die Drehung eines aufgenommenen Fotos in der Bilddatei zu codieren, damit es von anderen Apps und Geräten richtig angezeigt werden kann. Dieses Beispiel veranschaulicht die Verwendung der **CameraRotationHelper**-Hilfsklasse zum Berechnen der richtigen Ausrichtung für die Datei. Eine vollständige Beschreibung und Auflistung dieser Klasse finden Sie im Artikel [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Die **SaveCapturedFrameAsync**-Hilfsmethode, die das Bild auf einem Datenträger speichert, wird an späterer Stelle in diesem Artikel erläutert.

### <a name="get-optional-reference-frame"></a>Abrufen des optionalen Referenzframes

Beim HDR-Prozess werden mehrere Frames aufgenommen und nach Aufnahme aller Frames zu einem einzigen Bild zusammengesetzt. Durch Behandeln des [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured)-Ereignisses können Sie auf einen Frame zugreifen, nachdem er aufgenommen wurde, aber bevor das gesamte HDR-Verfahren abgeschlossen ist. Dies ist jedoch nicht erforderlich, wenn Sie lediglich am endgültigen HDR-Fotoergebnis interessiert sind.

> [!IMPORTANT]
> [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured) wird nicht auf Geräten ausgelöst, die Hardware-HDR unterstützen und daher keine Referenzframes generieren. Ihre App sollte den Fall behandeln, in dem dieses Ereignis nicht ausgelöst wird.

Da der Referenzframe außerhalb des Kontexts des Aufrufs von **CaptureAsync** auftritt, steht ein Mechanismus zur Übergabe von Kontextinformationen an den **OptionalReferencePhotoCaptured**-Handler zur Verfügung. Rufen Sie zunächst ein Objekt auf, das als Container für die Kontextinformationen dient. Sie können den Namen und den Inhalt dieses Objekts frei wählen. In diesem Beispiel wird ein Objekt definiert, das über Member zum Nachverfolgen des Dateinamens und der Kameraausrichtung der Aufnahme verfügt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAdvancedCaptureContext":::

Erstellen Sie eine neue Instanz des Kontextobjekts, füllen Sie dessen Member, und übergeben Sie es an die Überladung von [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync), die ein Objekt als Parameter akzeptiert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureWithContext":::

Wandeln Sie im [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured)-Ereignishandler die [**Context**](/uwp/api/windows.media.capture.optionalreferencephotocapturedeventargs.context)-Eigenschaft des [**OptionalReferencePhotoCapturedEventArgs**](/uwp/api/Windows.Media.Capture.OptionalReferencePhotoCapturedEventArgs)-Objekts in Ihre Kontextobjektklasse um. In diesem Beispiel wird der Dateiname geändert, um den Referenzframe vom endgültigen HDR-Bild zu unterscheiden. Anschließend wird die **SaveCapturedFrameAsync**-Hilfsmethode aufgerufen, um das Bild zu speichern.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOptionalReferencePhotoCaptured":::

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>Empfangen einer Benachrichtigung nach Aufnahme aller Frames

Die HDR-Fotoaufnahme besteht aus zwei Schritten. Zuerst werden mehrere Frames aufgenommen, dann werden die Frames zum endgültigen HDR-Bild verarbeitet. Während der laufenden Aufnahme der HDR-Quellbilder können Sie keine weiteren Aufnahmen starten. Sie können jedoch eine Aufnahme starten, wenn alle Frames aufgenommen wurden, die HDR-Nachverarbeitung aber noch nicht abgeschlossen ist. Das [**AllPhotosCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.allphotoscaptured)-Ereignis wird ausgelöst, wenn die HDR-Aufnahmen abgeschlossen sind, um Sie darüber zu informieren, dass Sie eine weitere Aufnahme starten können. In einem typischen Szenario wird die Aufnahmeschaltfläche in der Benutzeroberfläche deaktiviert, wenn die HDR-Aufnahme beginnt, und aktiviert, wenn **AllPhotosCaptured** ausgelöst wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAllPhotosCaptured":::

### <a name="clean-up-the-advancedphotocapture-object"></a>Bereinigen des AdvancedPhotoCapture-Objekts

Wenn die App alle Fotos aufgenommen hat, beenden Sie vor dem Löschen des **MediaCapture**-Objekts das [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Objekt, indem Sie [**FinishAsync**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) aufrufen und die Membervariable auf NULL festlegen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpAdvancedPhotoCapture":::


## <a name="low-light-photo-capture"></a>Fotoaufnahme bei schlechten Lichtverhältnissen
Ab Windows 10, Version 1607, kann **AdvancedPhotoCapture** verwendet werden, um Fotos mit einem integrierten Algorithmus aufzunehmen, der die Qualität von Fotos verbessert, die bei schlechten Lichtverhältnissen aufgenommen werden. Wenn Sie das Feature für schlechte Lichtverhältnisse der [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse verwenden, wertet das System die aktuelle Szene aus und wendet bei Bedarf einen Algorithmus an, um schwierige Lichtverhältnisse zu kompensieren. Wenn das System feststellt, dass der Algorithmus nicht benötigt wird, wird stattdessen eine normale Aufnahme ausgeführt.

Ermitteln Sie vor der Verwendung der Aufnahme bei schlechten Lichtverhältnissen, ob das Gerät, auf dem Ihre App derzeit ausgeführt wird, dieses Verfahren unterstützt, indem Sie den [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) des **MediaCapture**-Objekts und anschließend die [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl)-Eigenschaft abrufen. Überprüfen Sie, ob die [**SupportedModes**](/uwp/api/windows.media.devices.advancedphotocontrol.supportedmodes)-Sammlung des Videogerätecontrollers [**AdvancedPhotoMode.LowLight**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode) enthält. Wenn dies der Fall ist, wird die Aufnahme bei schlechten Lichtverhältnissen mit **AdvancedPhotoCapture** unterstützt. 
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetLowLightSupported1":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetLowLightSupported2":::

Deklarieren Sie anschließend eine Membervariable zum Speichern des **AdvancedPhotoCapture**-Objekts. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareAdvancedCapture":::

Erstellen Sie nach der Initialisierung des **MediaCapture**-Objekts in der App ein [**AdvancedPhotoCaptureSettings**](/uwp/api/Windows.Media.Devices.AdvancedPhotoCaptureSettings)-Objekt, und legen Sie den Modus auf [**AdvancedPhotoMode.LowLight**](/uwp/api/Windows.Media.Devices.AdvancedPhotoMode) fest. Rufen Sie die [**Configure**](/uwp/api/windows.media.devices.advancedphotocontrol.configure)-Methode des [**AdvancedPhotoControl**](/uwp/api/Windows.Media.Devices.AdvancedPhotoControl)-Objekts auf, und übergeben Sie das erstellte **AdvancedPhotoCaptureSettings**-Objekt.

Rufen Sie die [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)-Methode des **MediaCapture**-Objekts auf, und übergeben Sie ein [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt, das den von der Aufnahme zu verwendenden Codierungstyp angibt. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateAdvancedCaptureLowLightAsync":::

Rufen Sie zum Aufnehmen eines Fotos [**CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) auf.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCaptureLowLight":::

In diesem Beispiel wird wie im obigen HDR-Beispiel eine Hilfsklasse namens **CameraRotationHelper** verwendet, um den Drehungswert zu ermitteln, der im Bild codiert werden muss, damit es von anderen Apps und Geräten richtig angezeigt werden kann. Eine vollständige Beschreibung und Auflistung dieser Klasse finden Sie im Artikel [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Die **SaveCapturedFrameAsync**-Hilfsmethode, die das Bild auf einem Datenträger speichert, wird an späterer Stelle in diesem Artikel erläutert.

Sie können mehrere Fotos bei schlechten Lichtverhältnissen aufnehmen, ohne das **AdvancedPhotoCapture**-Objekt neu zu konfigurieren. Nach den Aufnahmen sollten Sie jedoch [**FinishAsync**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) aufrufen, um das Objekt und die zugehörigen Ressourcen zu bereinigen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpAdvancedPhotoCapture":::

## <a name="working-with-advancedcapturedphoto-objects"></a>Arbeiten mit AdvancedCapturedPhoto-Objekten
[**AdvancedPhotoCapture.CaptureAsync**](/uwp/api/windows.media.capture.advancedphotocapture.captureasync) gibt ein [**AdvancedCapturedPhoto**](/uwp/api/Windows.Media.Capture.AdvancedCapturedPhoto)-Objekt zurück, das das aufgenommene Foto darstellt. Dieses Objekt macht die [**Frame**](/uwp/api/windows.media.capture.advancedcapturedphoto.frame)-Eigenschaft verfügbar, die ein [**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame)-Objekt zurückgibt, das das Bild darstellt. Das [**OptionalReferencePhotoCaptured**](/uwp/api/windows.media.capture.advancedphotocapture.optionalreferencephotocaptured)-Ereignis stellt auch ein **CapturedFrame**-Objekt in seinen Ereignisargumenten bereit. Nachdem Sie ein Objekt dieses Typs abgerufen haben, können Sie es für verschiedene Zwecke verwenden, unter anderem zum Erstellen einer [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) oder Speichern des Bilds in einer Datei. 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>Abrufen einer „SoftwareBitmap” aus einem CapturedFrame-Objekt
Eine **SoftwareBitmap** kann ganz einfach aus einem **CapturedFrame**-Objekt abgerufen werden, indem Sie auf die [**SoftwareBitmap**](/uwp/api/windows.media.capture.capturedframe.softwarebitmap)-Eigenschaft des Objekts zugreifen. Die meisten Codierungsformate unterstützen **SoftwareBitmap** mit **AdvancedPhotoCapture** jedoch nicht. Sie sollten daher sicherstellen, dass die Eigenschaft nicht NULL ist, bevor Sie sie verwenden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmapFromCapturedFrame":::

Das nicht komprimierte Format NV12 ist in der aktuellen Version das einzige Codierungsformat, das **SoftwareBitmap** für **AdvancedPhotoCapture** unterstützt. Wenn Sie das Feature verwenden möchten, müssen Sie daher diese Codierung beim Aufrufen von [**PrepareAdvancedPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync) angeben. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUncompressedNv12":::

Natürlich können Sie das Bild immer in einer Datei speichern und die Datei dann in einem separaten Schritt in eine **SoftwareBitmap** laden. Weitere Informationen zur Verwendung von **SoftwareBitmap** finden Sie im Artikel zum [**Erstellen, Bearbeiten und Speichern von Bitmapbildern**](imaging.md).

## <a name="save-a-capturedframe-to-a-file"></a>Speichern eines „CapturedFrame” in einer Datei
Die [**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame)-Klasse implementiert die IInputStream-Schnittstelle, sodass sie als Eingabe für einen [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) verwendet werden kann. Anschließend kann ein [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) verwendet werden, um die Bilddaten auf einen Datenträger zu schreiben.

Im folgenden Beispiel werden ein neuer Ordner in der Bildbibliothek des Benutzers und eine Datei in diesem Ordner erstellt. Beachten Sie, dass Ihre App die Funktion **Bildbibliothek** in der App-Manifestdatei enthalten muss, um auf dieses Verzeichnis zugreifen zu können. Anschließend wird ein Dateidatenstrom zur angegebenen Datei geöffnet. Als Nächstes wird [**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) aufgerufen, um den Decoder aus dem **CapturedFrame** zu erstellen. Danach erstellt [**CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) einen Encoder aus dem Dateidatenstrom und dem Decoder.

In den nächsten Schritten wird die Ausrichtung des Fotos mithilfe der [**BitmapProperties**](/uwp/api/windows.graphics.imaging.bitmapencoder.bitmapproperties) des Encoders in der Bilddatei codiert. Weitere Informationen zur Handhabung der Ausrichtung beim Aufnehmen von Bildern finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Zum Schluss wird das Bild mit einem Aufruf von [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) in die Datei geschrieben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSaveCapturedFrameAsync":::

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
