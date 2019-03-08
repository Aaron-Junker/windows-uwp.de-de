---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: In diesem Artikel wird beschrieben, wie Sie die AdvancedPhotoCapture-Klasse verwenden, um HDR-Fotos (High Dynamic Range) und Fotos bei schlechten Lichtverhältnissen aufzunehmen.
title: HDR-Fotoaufnahmen (High Dynamic Range) und Fotoaufnahmen bei schlechten Lichtverhältnissen
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dde7f966e47aa6c35e3bc4e508eddabf13e313ee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598945"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>HDR-Fotoaufnahmen (High Dynamic Range) und Fotoaufnahmen bei schlechten Lichtverhältnissen



Dieser Artikel beschreibt, wie Sie die [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Klasse zum Aufnehmen von HDR-Fotos (High Dynamic Range) verwenden. Mithilfe dieser API können Sie außerdem einen Referenzframe aus der HDR-Aufnahme abrufen, bevor die Verarbeitung des endgültigen Bilds abgeschlossen ist.

Die folgenden Artikel beziehen sich ebenfalls auf HDR-Aufnahmen:

-   Sie können den [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) verwenden, damit das System den Inhalt des Vorschaudatenstroms der Medienaufnahme auswerten kann, um zu ermitteln, ob das Aufnahmeergebnis durch eine HDR-Verarbeitung verbessert werden kann. Weitere Informationen finden Sie im Artikel [Szenenanalyse für die Medienaufnahme](scene-analysis-for-media-capture.md) (MediaCapture).

-   Verwenden Sie die [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680)-Klasse für Videoaufnahmen mit dem integrierten Windows-HDR-Verarbeitungsalgorithmus. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](capture-device-controls-for-video-capture.md).

-   Mit der [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564)-Klasse können Sie eine Abfolge von Fotos unter Verwendung unterschiedlicher Aufnahmeeinstellungen für jedes Foto aufnehmen und Ihren eigenen HDR- oder einen anderen Verarbeitungsalgorithmus implementieren. Weitere Informationen finden Sie unter [Variable Fotosequenz](variable-photo-sequence.md).



> [!NOTE] 
> Ab Windows 10, Version 1709, wird das Aufzeichnen von Video und mithilfe von **AdvancedPhotoCapture** unterstützt.  Dies wurde in früheren Versionen nicht unterstützt. Diese Änderung bedeutet, dass Sie ein vorbereitetes **[LowLagMediaRecording](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording)** und **[AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture)** zur gleichen Zeit ausführen können. Sie können die Videoaufnahme zwischen Aufrufen starten oder beenden, um **[MediaCapture.PrepareAdvancedPhotoCaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** und **[AdvancedPhotoCapture.FinishAsync ](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)** durchzuführen. Sie können auch **[AdvancedPhotoCapture.CaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** aufrufen, während Sie ein Video aufzeichnen. Allerdings können einige **AdvancedPhotoCapture**-Szenarien, wie das Aufnehmen eines HDR-Fotos beim Aufzeichnen von Video, dazu führen, dass einige Videoframes geändert werden, die von HDR aufgenommen wurden, was zu einer negativen Benutzererfahrung führt. Aus diesem Grund unterscheidet sich die Liste der Modi, die von **[AdvancedPhotoControl.SupportedModes](https://docs.microsoft.com/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** während der Videoaufzeichnung zurückgegebenen wird. Überprüfen Sie diesen Wert sofort nach dem Starten oder Beenden der Videoaufnahme, um sicherzustellen, dass der gewünschte Modus im aktuellen Zustand der Videoaufnahme unterstützt wird.


> [!NOTE] 
> Ab Windows 10, Version 1709, wenn **AdvancedPhotoCapture** auf den HDR-Modus festgelegt ist, wird die [**FlashControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Enabled)-Eigenschaft ignoriert, und der Blitz wird nie ausgelöst. Wenn die **FlashControl.Enabled-Eigenschaft** bei anderen Aufnahmemodi auf „True” festgelegt ist, überschreibt sie die **AdvancedPhotoCapture**-Einstellungen. Dies führt dazu, dass ein normales Foto mit Blitz aufgenommen wird. Wenn [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Auto) auf "true" festgelegt ist, verwendet **AdvancedPhotoCapture** den Blitz oder auch nicht, je nach dem Standardverhalten des Kameratreibers für die Bedingungen in der aktuellen Szene. In früheren Versionen setzte die **AdvancedPhotoCapture** Blitz-Einstellung immer die **FlashControl.Enabled**-Einstellung außer Kraft.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

Es ist ein universelles Windows-Beispiel verfügbar, das die Verwendung der **AdvancedPhotoCapture**-Klasse veranschaulicht. Sie können dieses Beispiel verwenden, um sich die API im Kontext anzusehen, oder es als Ausgangspunkt für Ihre eigene App nutzen. Weitere Informationen finden Sie im [Beispiel für erweiterte Kameraaufnahmen](https://go.microsoft.com/fwlink/?LinkID=620517).

## <a name="advanced-photo-capture-namespaces"></a>Namespaces für erweiterte Fotoaufnahmen

Neben den erforderlichen Namespaces für die grundlegende Medienaufnahme werden in den Codebeispielen in diesem Artikel APIs in den folgenden Namespaces verwendet.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]

## <a name="hdr-photo-capture"></a>HDR-Fotoaufnahme

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>Ermitteln, ob die HDR-Fotoaufnahme vom aktuellen Gerät unterstützt wird

Das in diesem Artikel beschriebene HDR-Aufnahmeverfahren wird mithilfe des [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekts ausgeführt. Nicht alle Geräte unterstützen die HDR-Aufnahme mit **AdvancedPhotoCapture**. Ermitteln Sie, ob das Gerät, auf dem Ihre App derzeit ausgeführt wird, dieses Verfahren unterstützt, indem Sie zunächst den [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) des **MediaCapture**-Objekts und anschließend die [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840)-Eigenschaft abrufen. Überprüfen Sie die [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844)-Sammlung des Videogerätecontrollers, um festzustellen, ob sie [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845) enthält. Wenn dies der Fall ist, wird die HDR-Aufnahme mit **AdvancedPhotoCapture** unterstützt.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>Konfigurieren und Vorbereiten des AdvancedPhotoCapture-Objekts

Da Sie von mehreren Stellen innerhalb des Codes aus auf die [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Instanz zugreifen müssen, deklarieren Sie eine Membervariable als Container für das Objekt.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Erstellen Sie in der App nach der Initialisierung des **MediaCapture**-Objekts ein [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837)-Objekt, und legen Sie den Modus auf [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845) fest. Rufen Sie die [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841)-Methode des [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840)-Objekts auf, und übergeben Sie das erstellte **AdvancedPhotoCaptureSettings**-Objekt.

Rufen Sie die [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)-Methode des **MediaCapture**-Objekts auf, und übergeben Sie ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993)-Objekt, das den von der Aufnahme zu verwendenden Codierungstyp angibt. Die **ImageEncodingProperties**-Klasse enthält statische Methoden zum Erstellen der Bildcodierungen, die vom **MediaCapture**-Objekt unterstützt werden.

**PrepareAdvancedPhotoCaptureAsync** gibt das [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekt zurück, mit dem Sie die Fotoaufnahme initiieren. Sie können dieses Objekt zum Registrieren von Handlern für die Ereignisse [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) und [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) verwenden, die später in diesem Artikel erläutert werden.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

### <a name="capture-an-hdr-photo"></a>Aufnehmen eines HDR-Fotos

Nehmen Sie ein HDR-Foto auf, indem Sie die [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388)-Methode des [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekts aufrufen. Diese Methode gibt ein [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378)-Objekt zurück, das das aufgenommene Foto in seiner [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382)-Eigenschaft angibt.

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

Die meisten Foto-Apps versuchen, die Drehung eines aufgenommenen Fotos in der Bilddatei zu codieren, damit es von anderen Apps und Geräten richtig angezeigt werden kann. Dieses Beispiel veranschaulicht die Verwendung der **CameraRotationHelper**-Hilfsklasse zum Berechnen der richtigen Ausrichtung für die Datei. Eine vollständige Beschreibung und Auflistung dieser Klasse finden Sie im Artikel [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Die **SaveCapturedFrameAsync**-Hilfsmethode, die das Bild auf einem Datenträger speichert, wird an späterer Stelle in diesem Artikel erläutert.

### <a name="get-optional-reference-frame"></a>Abrufen des optionalen Referenzframes

Beim HDR-Prozess werden mehrere Frames aufgenommen und nach Aufnahme aller Frames zu einem einzigen Bild zusammengesetzt. Durch Behandeln des [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)-Ereignisses können Sie auf einen Frame zugreifen, nachdem er aufgenommen wurde, aber bevor das gesamte HDR-Verfahren abgeschlossen ist. Dies ist jedoch nicht erforderlich, wenn Sie lediglich am endgültigen HDR-Fotoergebnis interessiert sind.

> [!IMPORTANT]
> [**OptionalReferencePhotoCaptured** ](https://msdn.microsoft.com/library/windows/apps/mt181392) für Geräte, die Hardware HDR unterstützen und daher keine referenzbildern generiert nicht ausgelöst wird. Ihre App sollte den Fall behandeln, in dem dieses Ereignis nicht ausgelöst wird.

Da der Referenzframe außerhalb des Kontexts des Aufrufs von **CaptureAsync** auftritt, steht ein Mechanismus zur Übergabe von Kontextinformationen an den **OptionalReferencePhotoCaptured**-Handler zur Verfügung. Rufen Sie zunächst ein Objekt auf, das als Container für die Kontextinformationen dient. Sie können den Namen und den Inhalt dieses Objekts frei wählen. In diesem Beispiel wird ein Objekt definiert, das über Member zum Nachverfolgen des Dateinamens und der Kameraausrichtung der Aufnahme verfügt.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Erstellen Sie eine neue Instanz des Kontextobjekts, füllen Sie dessen Member, und übergeben Sie es an die Überladung von [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388), die ein Objekt als Parameter akzeptiert.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

Wandeln Sie im [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)-Ereignishandler die [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405)-Eigenschaft des [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404)-Objekts in Ihre Kontextobjektklasse um. In diesem Beispiel wird der Dateiname geändert, um den Referenzframe vom endgültigen HDR-Bild zu unterscheiden. Anschließend wird die **SaveCapturedFrameAsync**-Hilfsmethode aufgerufen, um das Bild zu speichern.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>Empfangen einer Benachrichtigung nach Aufnahme aller Frames

Die HDR-Fotoaufnahme besteht aus zwei Schritten. Zuerst werden mehrere Frames aufgenommen, dann werden die Frames zum endgültigen HDR-Bild verarbeitet. Während der laufenden Aufnahme der HDR-Quellbilder können Sie keine weiteren Aufnahmen starten. Sie können jedoch eine Aufnahme starten, wenn alle Frames aufgenommen wurden, die HDR-Nachverarbeitung aber noch nicht abgeschlossen ist. Das [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387)-Ereignis wird ausgelöst, wenn die HDR-Aufnahmen abgeschlossen sind, um Sie darüber zu informieren, dass Sie eine weitere Aufnahme starten können. In einem typischen Szenario wird die Aufnahmeschaltfläche in der Benutzeroberfläche deaktiviert, wenn die HDR-Aufnahme beginnt, und aktiviert, wenn **AllPhotosCaptured** ausgelöst wird.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

### <a name="clean-up-the-advancedphotocapture-object"></a>Bereinigen des AdvancedPhotoCapture-Objekts

Wenn die App alle Fotos aufgenommen hat, beenden Sie vor dem Löschen des **MediaCapture**-Objekts das [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Objekt, indem Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) aufrufen und die Membervariable auf NULL festlegen.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]


## <a name="low-light-photo-capture"></a>Fotoaufnahme bei schlechten Lichtverhältnissen
Ab Windows 10, Version 1607, kann **AdvancedPhotoCapture** verwendet werden, um Fotos mit einem integrierten Algorithmus aufzunehmen, der die Qualität von Fotos verbessert, die bei schlechten Lichtverhältnissen aufgenommen werden. Wenn Sie das Feature für schlechte Lichtverhältnisse der [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse verwenden, wertet das System die aktuelle Szene aus und wendet bei Bedarf einen Algorithmus an, um schwierige Lichtverhältnisse zu kompensieren. Wenn das System feststellt, dass der Algorithmus nicht benötigt wird, wird stattdessen eine normale Aufnahme ausgeführt.

Ermitteln Sie vor der Verwendung der Aufnahme bei schlechten Lichtverhältnissen, ob das Gerät, auf dem Ihre App derzeit ausgeführt wird, dieses Verfahren unterstützt, indem Sie den [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) des **MediaCapture**-Objekts und anschließend die [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840)-Eigenschaft abrufen. Überprüfen Sie, ob die [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844)-Sammlung des Videogerätecontrollers [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845) enthält. Wenn dies der Fall ist, wird die Aufnahme bei schlechten Lichtverhältnissen mit **AdvancedPhotoCapture** unterstützt. 
[!code-cs[LowLightSupported1](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported1)]

[!code-cs[LowLightSupported2](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported2)]

Deklarieren Sie anschließend eine Membervariable zum Speichern des **AdvancedPhotoCapture**-Objekts. 

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Erstellen Sie nach der Initialisierung des **MediaCapture**-Objekts in der App ein [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837)-Objekt, und legen Sie den Modus auf [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845) fest. Rufen Sie die [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841)-Methode des [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840)-Objekts auf, und übergeben Sie das erstellte **AdvancedPhotoCaptureSettings**-Objekt.

Rufen Sie die [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)-Methode des **MediaCapture**-Objekts auf, und übergeben Sie ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993)-Objekt, das den von der Aufnahme zu verwendenden Codierungstyp angibt. 

[!code-cs[CreateAdvancedCaptureLowLightAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureLowLightAsync)]

Rufen Sie zum Aufnehmen eines Fotos [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync) auf.

[!code-cs[CaptureLowLight](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureLowLight)]

In diesem Beispiel wird wie im obigen HDR-Beispiel eine Hilfsklasse namens **CameraRotationHelper** verwendet, um den Drehungswert zu ermitteln, der im Bild codiert werden muss, damit es von anderen Apps und Geräten richtig angezeigt werden kann. Eine vollständige Beschreibung und Auflistung dieser Klasse finden Sie im Artikel [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Die **SaveCapturedFrameAsync**-Hilfsmethode, die das Bild auf einem Datenträger speichert, wird an späterer Stelle in diesem Artikel erläutert.

Sie können mehrere Fotos bei schlechten Lichtverhältnissen aufnehmen, ohne das **AdvancedPhotoCapture**-Objekt neu zu konfigurieren. Nach den Aufnahmen sollten Sie jedoch [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) aufrufen, um das Objekt und die zugehörigen Ressourcen zu bereinigen.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## <a name="working-with-advancedcapturedphoto-objects"></a>Arbeiten mit AdvancedCapturedPhoto-Objekten
[**AdvancedPhotoCapture.CaptureAsync** ](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync) gibt ein [ **AdvancedCapturedPhoto** ](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto) Objekt, das das erfasste Foto darstellt. Dieses Objekt macht die [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto.Frame)-Eigenschaft verfügbar, die ein [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame)-Objekt zurückgibt, das das Bild darstellt. Das [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.OptionalReferencePhotoCaptured)-Ereignis stellt auch ein **CapturedFrame**-Objekt in seinen Ereignisargumenten bereit. Nachdem Sie ein Objekt dieses Typs abgerufen haben, können Sie es für verschiedene Zwecke verwenden, unter anderem zum Erstellen einer [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) oder Speichern des Bilds in einer Datei. 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>Abrufen einer „SoftwareBitmap” aus einem CapturedFrame-Objekt
Eine **SoftwareBitmap** kann ganz einfach aus einem **CapturedFrame**-Objekt abgerufen werden, indem Sie auf die [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap)-Eigenschaft des Objekts zugreifen. Die meisten Codierungsformate unterstützen **SoftwareBitmap** mit **AdvancedPhotoCapture** jedoch nicht. Sie sollten daher sicherstellen, dass die Eigenschaft nicht NULL ist, bevor Sie sie verwenden.

[!code-cs[SoftwareBitmapFromCapturedFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapFromCapturedFrame)]

Das nicht komprimierte Format NV12 ist in der aktuellen Version das einzige Codierungsformat, das **SoftwareBitmap** für **AdvancedPhotoCapture** unterstützt. Wenn Sie das Feature verwenden möchten, müssen Sie daher diese Codierung beim Aufrufen von [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) angeben. 

[!code-cs[UncompressedNv12](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUncompressedNv12)]

Natürlich können Sie das Bild immer in einer Datei speichern und die Datei dann in einem separaten Schritt in eine **SoftwareBitmap** laden. Weitere Informationen zur Verwendung von **SoftwareBitmap** finden Sie im Artikel zum [**Erstellen, Bearbeiten und Speichern von Bitmapbildern**](imaging.md).

## <a name="save-a-capturedframe-to-a-file"></a>Speichern eines „CapturedFrame” in einer Datei
Die [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame)-Klasse implementiert die IInputStream-Schnittstelle, sodass sie als Eingabe für einen [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) verwendet werden kann. Anschließend kann ein [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) verwendet werden, um die Bilddaten auf einen Datenträger zu schreiben.

Im folgenden Beispiel werden ein neuer Ordner in der Bildbibliothek des Benutzers und eine Datei in diesem Ordner erstellt. Beachten Sie, dass Ihre App die Funktion **Bildbibliothek** in der App-Manifestdatei enthalten muss, um auf dieses Verzeichnis zugreifen zu können. Anschließend wird ein Dateidatenstrom zur angegebenen Datei geöffnet. Als Nächstes wird [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder.CreateAsync) aufgerufen, um den Decoder aus dem **CapturedFrame** zu erstellen. Danach erstellt [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214) einen Encoder aus dem Dateidatenstrom und dem Decoder.

In den nächsten Schritten wird die Ausrichtung des Fotos mithilfe der [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.BitmapProperties) des Encoders in der Bilddatei codiert. Weitere Informationen zur Handhabung der Ausrichtung beim Aufnehmen von Bildern finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Zum Schluss wird das Bild mit einem Aufruf von [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) in die Datei geschrieben.

[!code-cs[SaveCapturedFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSaveCapturedFrameAsync)]

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
