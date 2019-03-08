---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: Dieser Artikel beschreibt, wie Sie mit SceneAnalysisEffect und FaceDetectionEffect den Inhalt des Vorschaudatenstroms der Medienaufnahme analysieren.
title: Effekte für die Analyse von Kamera-Frames
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3359c3e7d7841e68dd00dc318eb1de9f1418df06
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635825"
---
# <a name="effects-for-analyzing-camera-frames"></a>Effekte für die Analyse von Kamera-Frames



Dieser Artikel beschreibt, wie Sie mit [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) und [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776) den Inhalt des Vorschaudatenstroms der Medienaufnahme analysieren.

## <a name="scene-analysis-effect"></a>Effekt der Szenenanalyse

Die [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902)-Klasse analysiert die Videoframes im Vorschaudatenstrom der Medienaufnahme und empfiehlt Verarbeitungsoptionen zur Verbesserung des Aufnahmeergebnisses. Der Effekt ermittelt derzeit, ob die Verwendung der HDR-Verarbeitung (High Dynamic Range) die Aufnahme verbessern würde.

Wenn der Effekt die Verwendung von HDR empfiehlt, können Sie dies auf folgende Weise tun:

-   Verwenden Sie die [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386)-Klasse für Videoaufnahmen mit dem integrierten Windows-HDR-Verarbeitungsalgorithmus. Weitere Informationen finden Sie unter [HDR-Fotoaufnahmen (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md).

-   Verwenden Sie die [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680)-Klasse für Videoaufnahmen mit dem integrierten Windows-HDR-Verarbeitungsalgorithmus. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](capture-device-controls-for-video-capture.md).

-   Verwenden Sie die [**VariablePhotoSequenceControl**](https://msdn.microsoft.com/library/windows/apps/dn640573)-Klasse für die Aufnahme einer Framesequenz, die Sie anschließend mithilfe einer benutzerdefinierten HDR-Implementierung zusammensetzen können. Weitere Informationen finden Sie unter [Variable Fotosequenz](variable-photo-sequence.md).

### <a name="scene-analysis-namespaces"></a>Namespaces der Szenenanalyse

Um die Szenenanalyse verwenden zu können, muss Ihre App zusätzlich zu den für die grundlegende Medienaufnahme erforderlichen Namespaces die folgenden Namespaces enthalten.

[!code-cs[SceneAnalysisUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalysisUsing)]

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>Initialisieren und Hinzufügen des Szenenanalyseneffekts zum Vorschaudatenstrom

Videoeffekte werden mit zwei APIs, einer Effektdefinition, die die für das Aufnahmegerät benötigten Einstellungen zum Initialisieren des Effekts bereitstellt, und einer Effektinstanz implementiert, die zum Steuern des Effekts verwendet werden kann. Da Sie ggf. von mehreren Stellen innerhalb des Codes aus auf die Effektinstanz zugreifen müssen, sollten Sie in der Regel eine Membervariable als Container für das Objekt deklarieren.

[!code-cs[DeclareSceneAnalysisEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSceneAnalysisEffect)]

Erstellen Sie nach dem Initialisieren des **MediaCapture**-Objekts in Ihrer App eine neue Instanz der [**SceneAnalysisEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn948903)-Klasse.

Registrieren Sie den Effekt mit dem Aufnahmegerät durch Aufrufen der [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035)-Methode für Ihr **MediaCapture**-Objekt, stellen Sie **SceneAnalysisEffectDefinition** bereit, und geben Sie [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) an, damit der Effekt im Gegensatz zum Aufnahmedatenstrom auf den Videovorschaudatenstrom angewendet wird. **AddVideoEffectAsync** gibt eine Instanz des hinzugefügten Effekts zurück. Da diese Methode mit verschiedenen Effekttypen verwendet werden kann, müssen Sie die zurückgegebene Instanz in ein [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902)-Objekt umwandeln.

Um die Ergebnisse der Szenenanalyse zu erhalten, müssen Sie einen Handler für das [**SceneAnalyzed**](https://msdn.microsoft.com/library/windows/apps/dn948920)-Ereignis registrieren.

Derzeit umfasst der Szenenanalyseeffekt nur die HDR-Analyzer. Aktivieren Sie die HDR-Analyse, indem Sie die [**HighDynamicRangeControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948827)-Eigenschaft des Effekts auf „true“ festlegen.

[!code-cs[CreateSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateSceneAnalysisEffectAsync)]

### <a name="implement-the-sceneanalyzed-event-handler"></a>Implementieren des SceneAnalyzed-Ereignishandlers

Die Ergebnisse der Szenenanalyse werden im **SceneAnalyzed**-Ereignishandler zurückgegeben. Das an den Handler übergebene [**SceneAnalyzedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948922)-Objekt verfügt über ein [**SceneAnalysisEffectFrame**](https://msdn.microsoft.com/library/windows/apps/dn948907)-Objekt mit einem [**HighDynamicRangeOutput**](https://msdn.microsoft.com/library/windows/apps/dn948830)-Objekt. Die [**Certainty**](https://msdn.microsoft.com/library/windows/apps/dn948833)-Eigenschaft der HDR-Ausgabe liefert einen Wert zwischen 0 und 1,0, wobei 0 angibt, dass die HDR-Verarbeitung das Aufnahmeergebnis nicht verbessern und 1,0, dass die HDR-Verarbeitung dieses verbessern würde. Sie können den Schwellenwert festlegen, an dem Sie HDR verwenden möchten, oder die Ergebnisse für den Benutzer anzeigen und diesen darüber entscheiden lassen.

[!code-cs[SceneAnalyzed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalyzed)]

Das an den Handler übergebene [**HighDynamicRangeOutput**](https://msdn.microsoft.com/library/windows/apps/dn948830)-Objekt verfügt auch über eine [**FrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn948834)-Eigenschaft, die die vorgeschlagenen Framecontroller für die Aufnahme einer variablen Fotosequenz für HDR-Verarbeitung enthält. Weitere Informationen finden Sie unter [Variable Fotosequenz](variable-photo-sequence.md).

### <a name="clean-up-the-scene-analysis-effect"></a>Bereinigen des Szenenanalyseeffekts

Wenn Ihre App die Aufnahme abgeschlossen hat, deaktivieren Sie vor dem Löschen des **MediaCapture**-Objekts den Szenenanalyseeffekt, indem Sie die [**HighDynamicRangeAnalyzer.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948827)-Eigenschaft des Effekts auf „false“ festlegen und die Registrierung Ihres [**SceneAnalyzed**](https://msdn.microsoft.com/library/windows/apps/dn948920)-Ereignishandlers aufheben. Rufen Sie [**MediaCapture.ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) auf, und geben Sie den Videovorschaudatenstrom an, da der Effekt diesem Datenstrom hinzugefügt wurde. Legen Sie schließlich die Membervariable auf NULL fest.

[!code-cs[CleanUpSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpSceneAnalysisEffectAsync)]

## <a name="face-detection-effect"></a>Gesichtserkennungseffekt

[  **FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776) identifiziert die Position von Gesichtern innerhalb des Vorschaudatenstroms der Medienaufnahme. Mit dem Effekt erhalten Sie eine Benachrichtigung, wenn ein Gesicht im Vorschaudatenstrom erkannt wird, und es wird der Begrenzungsrahmen für jedes erkannte Gesicht im Vorschauframe bereitgestellt. Auf unterstützten Geräten stellt die Gesichtserkennung auch erweiterte Belichtung und Fokus auf das wichtigste Gesicht in der Szene bereit.

### <a name="face-detection-namespaces"></a>Namespaces der Gesichtserkennung

Um die Gesichtserkennung verwenden zu können, muss Ihre App die folgenden Namespaces zusätzlich zu den für die grundlegende Medienaufnahme erforderlichen Namespaces enthalten.

[!code-cs[FaceDetectionUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>Initialisieren und Hinzufügen des Gesichterkennungseffekts zum Vorschaudatenstrom

Videoeffekte werden mit zwei APIs, einer Effektdefinition, die die für das Aufnahmegerät benötigten Einstellungen zum Initialisieren des Effekts bereitstellt, und einer Effektinstanz implementiert, die zum Steuern des Effekts verwendet werden kann. Da Sie ggf. von mehreren Stellen innerhalb des Codes aus auf die Effektinstanz zugreifen müssen, sollten Sie in der Regel eine Membervariable als Container für das Objekt deklarieren.

[!code-cs[DeclareFaceDetectionEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareFaceDetectionEffect)]

Erstellen Sie nach dem Initialisieren des **MediaCapture**-Objekts in Ihrer App eine neue Instanz von [**FaceDetectionEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn948778). Legen Sie die [**DetectionMode**](https://msdn.microsoft.com/library/windows/apps/dn948781)-Eigenschaft fest, um eine schnellere oder genauere Gesichtserkennung zu bevorzugen. Legen Sie die [**SynchronousDetectionEnabled**](https://msdn.microsoft.com/library/windows/apps/dn948786)-Eigenschaft fest, um anzugeben, dass eingehende Frames nicht durch eine andauernde Gesichtserkennung verzögert werden, da dies zu einer stockenden Vorschau führen kann.

Registrieren Sie den Effekt mit dem Aufnahmegerät durch Aufrufen der [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035)-Methode für Ihr **MediaCapture**-Objekt, stellen Sie **FaceDetectionEffectDefinition** bereit, und geben Sie [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) an, damit der Effekt im Gegensatz zum Aufnahmedatenstrom auf den Videovorschaudatenstrom angewendet wird. **AddVideoEffectAsync** gibt eine Instanz des hinzugefügten Effekts zurück. Da diese Methode mit verschiedenen Effekttypen verwendet werden kann, müssen Sie die zurückgegebene Instanz in ein [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776)-Objekt umwandeln.

Aktivieren bzw. deaktivieren Sie den Effekt, indem Sie die [**FaceDetectionEffect.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948818)-Eigenschaft festlegen. Geben Sie an, wie oft der Effekt Frames analysieren soll, indem Sie die [**FaceDetectionEffect.DesiredDetectionInterval**](https://msdn.microsoft.com/library/windows/apps/dn948814)-Eigenschaft festlegen. Beide Eigenschaften können angepasst werden, während der Medienaufnahme ausgeführt wird.

[!code-cs[CreateFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFaceDetectionEffectAsync)]

### <a name="receive-notifications-when-faces-are-detected"></a>Empfangen von Benachrichtigungen, wenn Gesichter erkannt werden

Wenn Sie während der Gesichtserkennung Aktionen ausführen möchten, z. B. Zeichnen eines Rahmens um die erkannten Gesichter in der Videovorschau, können Sie eine Registrierung für das [**FaceDetected**](https://msdn.microsoft.com/library/windows/apps/dn948820)-Ereignis durchführen.

[!code-cs[RegisterFaceDetectionHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterFaceDetectionHandler)]

Im Handler für das Ereignis erhalten Sie eine Liste mit allen in einem Frame erkannten Gesichtern, indem Sie auf die [**FaceDetectionEffectFrame.DetectedFaces**](https://msdn.microsoft.com/library/windows/apps/dn948792)-Eigenschaft von [**FaceDetectedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948774) zugreifen. Die [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126)-Eigenschaft ist eine [**BitmapBounds**](https://msdn.microsoft.com/library/windows/apps/br226169)-Struktur, die das Rechteck mit dem erkannten Gesicht relativ zu den Abmessungen des Vorschaudatenstroms beschreibt. Einen Beispielcode, der die Koordinaten des Vorschaudatenstroms in Bildschirmkoordinaten umwandelt, finden Sie unter [Gesichtserkennungsbeispiel für UWP](https://go.microsoft.com/fwlink/?LinkId=619486).

[!code-cs[FaceDetected](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetected)]

### <a name="clean-up-the-face-detection-effect"></a>Bereinigen des Gesichtserkennungseffekts

Wenn Ihre App die Aufnahme abgeschlossen hat, deaktivieren Sie vor dem Löschen des **MediaCapture**-Objekts den Gesichtserkennungseffekt mit [**FaceDetectionEffect.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn948818), und heben Sie die Registrierung Ihres [**FaceDetected**](https://msdn.microsoft.com/library/windows/apps/dn948820)-Ereignishandlers auf, wenn Sie einen registriert haben. Rufen Sie [**MediaCapture.ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) auf, und geben Sie den Videovorschaudatenstrom an, da der Effekt diesem Datenstrom hinzugefügt wurde. Legen Sie schließlich die Membervariable auf NULL fest.

[!code-cs[CleanUpFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpFaceDetectionEffectAsync)]

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>Überprüfen der Fokus- und Belichtungsunterstützung für erkannte Gesichter

Nicht alle Geräte verfügen über ein Aufnahmegerät, das den Fokus und die Belichtung auf Grundlage der erkannten Gesichter anpassen kann. Da Gesichtserkennung Geräteressourcen in Anspruch nimmt, sollten Sie die Gesichtserkennung nur auf Geräten aktivieren, die das Feature zur Verbesserung der Aufnahme verwenden können. Rufen Sie zum Festzustellen, ob die gesichtsbasierte Aufnahmeoptimierung verfügbar ist, die [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)-Klasse für Ihre initialisierte [MediaCapture](capture-photos-and-video-with-mediacapture.md) ab, und rufen Sie dann die [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064)-Klasse des Videogerätecontrollers ab. Überprüfen Sie, ob die [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069)-Eigenschaft mindestens eine Region unterstützt. Überprüfen Sie dann, ob die [**AutoExposureSupported**](https://msdn.microsoft.com/library/windows/apps/dn279065)-Eigenschaft oder die [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066)-Eigenschaft den Wert „true“ aufweist. Wenn diese Bedingungen erfüllt sind, kann das Gerät die Vorteile der Gesichtserkennung für eine verbesserte Aufnahme nutzen.

[!code-cs[AreFaceFocusAndExposureSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAreFaceFocusAndExposureSupported)]

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




