---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: Dieser Artikel beschreibt, wie Sie mit SceneAnalysisEffect und FaceDetectionEffect den Inhalt des Vorschaudatenstroms der Medienaufnahme analysieren.
title: Effekte für die Analyse von Kamera-Frames
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 365f6ddde709d81a308059af2ec6fa9051e8ad92
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173654"
---
# <a name="effects-for-analyzing-camera-frames"></a>Effekte für die Analyse von Kamera-Frames



Dieser Artikel beschreibt, wie Sie mit [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) und [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) den Inhalt des Vorschaudatenstroms der Medienaufnahme analysieren.

## <a name="scene-analysis-effect"></a>Effekt der Szenenanalyse

Die [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect)-Klasse analysiert die Videoframes im Vorschaudatenstrom der Medienaufnahme und empfiehlt Verarbeitungsoptionen zur Verbesserung des Aufnahmeergebnisses. Der Effekt ermittelt derzeit, ob die Verwendung der HDR-Verarbeitung (High Dynamic Range) die Aufnahme verbessern würde.

Wenn der Effekt die Verwendung von HDR empfiehlt, können Sie dies auf folgende Weise tun:

-   Verwenden Sie die [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse für Videoaufnahmen mit dem integrierten Windows-HDR-Verarbeitungsalgorithmus. Weitere Informationen finden Sie unter [HDR-Fotoaufnahmen (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md).

-   Verwenden Sie die [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl)-Klasse für Videoaufnahmen mit dem integrierten Windows-HDR-Verarbeitungsalgorithmus. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](capture-device-controls-for-video-capture.md).

-   Verwenden Sie die [**VariablePhotoSequenceControl**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController)-Klasse für die Aufnahme einer Framesequenz, die Sie anschließend mithilfe einer benutzerdefinierten HDR-Implementierung zusammensetzen können. Weitere Informationen finden Sie unter [Variable Photo Sequence](variable-photo-sequence.md).

### <a name="scene-analysis-namespaces"></a>Namespaces der Szenenanalyse

Um die Szenenanalyse verwenden zu können, muss Ihre App zusätzlich zu den für die grundlegende Medienaufnahme erforderlichen Namespaces die folgenden Namespaces enthalten.

[!code-cs[SceneAnalysisUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalysisUsing)]

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>Initialisieren und Hinzufügen des Szenenanalyseneffekts zum Vorschaudatenstrom

Videoeffekte werden mit zwei APIs, einer Effektdefinition, die die für das Aufnahmegerät benötigten Einstellungen zum Initialisieren des Effekts bereitstellt, und einer Effektinstanz implementiert, die zum Steuern des Effekts verwendet werden kann. Da Sie ggf. von mehreren Stellen innerhalb des Codes aus auf die Effektinstanz zugreifen müssen, sollten Sie in der Regel eine Membervariable als Container für das Objekt deklarieren.

[!code-cs[DeclareSceneAnalysisEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSceneAnalysisEffect)]

Erstellen Sie nach dem Initialisieren des **MediaCapture**-Objekts in Ihrer App eine neue Instanz der [**SceneAnalysisEffectDefinition**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition)-Klasse.

Registrieren Sie den Effekt mit dem Aufnahmegerät durch Aufrufen der [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync)-Methode für Ihr **MediaCapture**-Objekt, stellen Sie **SceneAnalysisEffectDefinition** bereit, und geben Sie [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) an, damit der Effekt im Gegensatz zum Aufnahmedatenstrom auf den Videovorschaudatenstrom angewendet wird. **AddVideoEffectAsync** gibt eine Instanz des hinzugefügten Effekts zurück. Da diese Methode mit verschiedenen Effekttypen verwendet werden kann, müssen Sie die zurückgegebene Instanz in ein [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect)-Objekt umwandeln.

Um die Ergebnisse der Szenenanalyse zu erhalten, müssen Sie einen Handler für das [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed)-Ereignis registrieren.

Derzeit umfasst der Szenenanalyseeffekt nur die HDR-Analyzer. Aktivieren Sie die HDR-Analyse, indem Sie die [**HighDynamicRangeControl.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled)-Eigenschaft des Effekts auf „true“ festlegen.

[!code-cs[CreateSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateSceneAnalysisEffectAsync)]

### <a name="implement-the-sceneanalyzed-event-handler"></a>Implementieren des SceneAnalyzed-Ereignishandlers

Die Ergebnisse der Szenenanalyse werden im **SceneAnalyzed**-Ereignishandler zurückgegeben. Das an den Handler übergebene [**SceneAnalyzedEventArgs**](/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs)-Objekt verfügt über ein [**SceneAnalysisEffectFrame**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame)-Objekt mit einem [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput)-Objekt. Die [**Certainty**](/uwp/api/windows.media.core.highdynamicrangeoutput.certainty)-Eigenschaft der HDR-Ausgabe liefert einen Wert zwischen 0 und 1,0, wobei 0 angibt, dass die HDR-Verarbeitung das Aufnahmeergebnis nicht verbessern und 1,0, dass die HDR-Verarbeitung dieses verbessern würde. Sie können den Schwellenwert festlegen, an dem Sie HDR verwenden möchten, oder die Ergebnisse für den Benutzer anzeigen und diesen darüber entscheiden lassen.

[!code-cs[SceneAnalyzed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalyzed)]

Das an den Handler übergebene [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput)-Objekt verfügt auch über eine [**FrameControllers**](/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers)-Eigenschaft, die die vorgeschlagenen Framecontroller für die Aufnahme einer variablen Fotosequenz für HDR-Verarbeitung enthält. Weitere Informationen finden Sie unter [Variable Photo Sequence](variable-photo-sequence.md).

### <a name="clean-up-the-scene-analysis-effect"></a>Bereinigen des Szenenanalyseeffekts

Wenn Ihre App die Aufnahme abgeschlossen hat, deaktivieren Sie vor dem Löschen des **MediaCapture**-Objekts den Szenenanalyseeffekt, indem Sie die [**HighDynamicRangeAnalyzer.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled)-Eigenschaft des Effekts auf „false“ festlegen und die Registrierung Ihres [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed)-Ereignishandlers aufheben. Rufen Sie [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) auf, und geben Sie den Videovorschaudatenstrom an, da der Effekt diesem Datenstrom hinzugefügt wurde. Legen Sie schließlich die Membervariable auf NULL fest.

[!code-cs[CleanUpSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpSceneAnalysisEffectAsync)]

## <a name="face-detection-effect"></a>Gesichtserkennungseffekt

[**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) identifiziert die Position von Gesichtern innerhalb des Vorschaudatenstroms der Medienaufnahme. Mit dem Effekt erhalten Sie eine Benachrichtigung, wenn ein Gesicht im Vorschaudatenstrom erkannt wird, und es wird der Begrenzungsrahmen für jedes erkannte Gesicht im Vorschauframe bereitgestellt. Auf unterstützten Geräten stellt die Gesichtserkennung auch erweiterte Belichtung und Fokus auf das wichtigste Gesicht in der Szene bereit.

### <a name="face-detection-namespaces"></a>Namespaces der Gesichtserkennung

Um die Gesichtserkennung verwenden zu können, muss Ihre App die folgenden Namespaces zusätzlich zu den für die grundlegende Medienaufnahme erforderlichen Namespaces enthalten.

[!code-cs[FaceDetectionUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>Initialisieren und Hinzufügen des Gesichterkennungseffekts zum Vorschaudatenstrom

Videoeffekte werden mit zwei APIs, einer Effektdefinition, die die für das Aufnahmegerät benötigten Einstellungen zum Initialisieren des Effekts bereitstellt, und einer Effektinstanz implementiert, die zum Steuern des Effekts verwendet werden kann. Da Sie ggf. von mehreren Stellen innerhalb des Codes aus auf die Effektinstanz zugreifen müssen, sollten Sie in der Regel eine Membervariable als Container für das Objekt deklarieren.

[!code-cs[DeclareFaceDetectionEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareFaceDetectionEffect)]

Erstellen Sie nach dem Initialisieren des **MediaCapture**-Objekts in Ihrer App eine neue Instanz von [**FaceDetectionEffectDefinition**](/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition). Legen Sie die [**DetectionMode**](/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode)-Eigenschaft fest, um eine schnellere oder genauere Gesichtserkennung zu bevorzugen. Legen Sie die [**SynchronousDetectionEnabled**](/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled)-Eigenschaft fest, um anzugeben, dass eingehende Frames nicht durch eine andauernde Gesichtserkennung verzögert werden, da dies zu einer stockenden Vorschau führen kann.

Registrieren Sie den Effekt mit dem Aufnahmegerät durch Aufrufen der [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync)-Methode für Ihr **MediaCapture**-Objekt, stellen Sie **FaceDetectionEffectDefinition** bereit, und geben Sie [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) an, damit der Effekt im Gegensatz zum Aufnahmedatenstrom auf den Videovorschaudatenstrom angewendet wird. **AddVideoEffectAsync** gibt eine Instanz des hinzugefügten Effekts zurück. Da diese Methode mit verschiedenen Effekttypen verwendet werden kann, müssen Sie die zurückgegebene Instanz in ein [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect)-Objekt umwandeln.

Aktivieren bzw. deaktivieren Sie den Effekt, indem Sie die [**FaceDetectionEffect.Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled)-Eigenschaft festlegen. Geben Sie an, wie oft der Effekt Frames analysieren soll, indem Sie die [**FaceDetectionEffect.DesiredDetectionInterval**](/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval)-Eigenschaft festlegen. Beide Eigenschaften können angepasst werden, während der Medienaufnahme ausgeführt wird.

[!code-cs[CreateFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFaceDetectionEffectAsync)]

### <a name="receive-notifications-when-faces-are-detected"></a>Empfangen von Benachrichtigungen, wenn Gesichter erkannt werden

Wenn Sie während der Gesichtserkennung Aktionen ausführen möchten, z. B. Zeichnen eines Rahmens um die erkannten Gesichter in der Videovorschau, können Sie eine Registrierung für das [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected)-Ereignis durchführen.

[!code-cs[RegisterFaceDetectionHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterFaceDetectionHandler)]

Im Handler für das Ereignis erhalten Sie eine Liste mit allen in einem Frame erkannten Gesichtern, indem Sie auf die [**FaceDetectionEffectFrame.DetectedFaces**](/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces)-Eigenschaft von [**FaceDetectedEventArgs**](/uwp/api/Windows.Media.Core.FaceDetectedEventArgs) zugreifen. Die [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox)-Eigenschaft ist eine [**BitmapBounds**](/uwp/api/Windows.Graphics.Imaging.BitmapBounds)-Struktur, die das Rechteck mit dem erkannten Gesicht relativ zu den Abmessungen des Vorschaudatenstroms beschreibt. Einen Beispielcode, der die Koordinaten des Vorschaudatenstroms in Bildschirmkoordinaten umwandelt, finden Sie unter [Gesichtserkennungsbeispiel für UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection).

[!code-cs[FaceDetected](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetected)]

### <a name="clean-up-the-face-detection-effect"></a>Bereinigen des Gesichtserkennungseffekts

Wenn Ihre App die Aufnahme abgeschlossen hat, deaktivieren Sie vor dem Löschen des **MediaCapture**-Objekts den Gesichtserkennungseffekt mit [**FaceDetectionEffect.Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled), und heben Sie die Registrierung Ihres [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected)-Ereignishandlers auf, wenn Sie einen registriert haben. Rufen Sie [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) auf, und geben Sie den Videovorschaudatenstrom an, da der Effekt diesem Datenstrom hinzugefügt wurde. Legen Sie schließlich die Membervariable auf NULL fest.

[!code-cs[CleanUpFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpFaceDetectionEffectAsync)]

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>Überprüfen der Fokus- und Belichtungsunterstützung für erkannte Gesichter

Nicht alle Geräte verfügen über ein Aufnahmegerät, das den Fokus und die Belichtung auf Grundlage der erkannten Gesichter anpassen kann. Da Gesichtserkennung Geräteressourcen in Anspruch nimmt, sollten Sie die Gesichtserkennung nur auf Geräten aktivieren, die das Feature zur Verbesserung der Aufnahme verwenden können. Rufen Sie zum Festzustellen, ob die gesichtsbasierte Aufnahmeoptimierung verfügbar ist, die [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController)-Klasse für Ihre initialisierte [MediaCapture](./index.md) ab, und rufen Sie dann die [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl)-Klasse des Videogerätecontrollers ab. Überprüfen Sie, ob die [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions)-Eigenschaft mindestens eine Region unterstützt. Überprüfen Sie dann, ob die [**AutoExposureSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported)-Eigenschaft oder die [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported)-Eigenschaft den Wert „true“ aufweist. Wenn diese Bedingungen erfüllt sind, kann das Gerät die Vorteile der Gesichtserkennung für eine verbesserte Aufnahme nutzen.

[!code-cs[AreFaceFocusAndExposureSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAreFaceFocusAndExposureSupported)]

## <a name="related-topics"></a>Zugehörige Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 