---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: In diesem Artikel werden die für UWP-Apps verfügbaren Kamera-Features aufgeführt, sowie die Links zu den Anleitungen für ihre Verwendung.
title: Kamera
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d7d7fcdeb3740ac4c6851170796243392676d1d1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254290"
---
# <a name="camera"></a>Kamera

Dieser Abschnitt enthält Richtlinien für das Erstellen von UWP (Universelle Windows-Plattform)-Apps, die die Kamera oder das Mikrofon verwenden, um Fotos, Videos oder Audiodateien zu erfassen.

## <a name="use-the-windows-built-in-camera-ui"></a>Verwenden der in Windows integrierten Kamera-UI

| Thema | Beschreibung |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capture photos and video with Windows built-in camera UI](capture-photos-and-video-with-cameracaptureui.md) | Zeigt, wie Sie die [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden. Wenn Sie einfach den Benutzer dazu befähigen möchten, ein Foto oder Video aufzuzeichnen und das Ergebnis an Ihre App zurückzugeben, ist dies die schnellste und einfachste Möglichkeit dafür.  |

## <a name="basic-mediacapture-tasks"></a>Grundlegende MediaCapture-Funktionen

| Thema | Beschreibung |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Display the camera preview](simple-camera-preview-access.md) | Beschreibt, wie Sie in einer UWP-App innerhalb einer XAML-Seite den Kameravorschau-Stream schnell anzeigen können. |
| [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Zeigt die einfachste Möglichkeit zum Aufnehmen von Fotos und Videos mit der [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)-Klasse. Die **MediaCapture**-Klasse stellt einen robusten Satz von APIs bereit, der eine Steuerung der Aufnahme-Pipeline auf unterster Ebene sowie fortgeschrittene Aufnahme-Szenarien ermöglicht. Dieser Artikel soll Ihnen jedoch helfen, Ihrer App die grundlegende Medienaufnahme schnell und problemlos hinzuzufügen. |
| [Camera UI features for mobile devices](camera-ui-features-for-mobile-devices.md) | Beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Erweiterte MediaCapture-Funktionen   
                                                                                                               
| Thema                                                                                             | Beschreibung                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Handle device and screen orientation with MediaCapture](handle-device-orientation-with-mediacapture.md) | Beschreibt, wie Sie beim Aufnehmen von Fotos und Videos mit einer Hilfsklasse die Geräteausrichtung handhaben. | 
| [Discover and select camera capabilities with camera profiles](camera-profiles.md) | Beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen. |
| [Set format, resolution, and frame rate for MediaCapture](set-media-encoding-properties.md) | Beschreibt, wie Sie die [**IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties)-Schnittstelle verwenden, um die Auflösung und Bildfrequenz des Kameravorschau-Datenstroms sowie von aufgenommenen Fotos und Videos festzulegen. In ihm wird auch gezeigt, wie Sie sicherstellen, dass das Seitenverhältnis des Vorschaudatenstroms mit dem Seitenverhältnis der aufgenommenen Medien übereinstimmt. |
| [HDR and low-light photo capture](high-dynamic-range-hdr-photo-capture.md) | Beschreibt, wie Sie die [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse verwenden, um HDR (High Dynamic Range)-Fotos und Fotos bei schlechten Lichtverhältnissen aufzunehmen. |
| [Manual camera controls for photo and video capture](capture-device-controls-for-photo-and-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen. |
| [Manual camera controls for video capture](capture-device-controls-for-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, einschließlich HDR-Video und Belichtungspriorität, zu ermöglichen.  |
| [Video stabilization effect for video capture](effects-for-video-capture.md) | Beschreibt, wie Sie den Videostabilisierungseffekt verwenden.  |
| [Scene anlysis for MediaCapture](scene-analysis-for-media-capture.md) | Beschreibt, wie Sie den [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) und den [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) verwenden, um den Inhalt des Vorschaudatenstroms der Medienaufnahme zu analysieren.  |
| [Capture a photo sequence with VariablePhotoSequence](variable-photo-sequence.md) | Beschreibt, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden.  |
| [Process media frames with MediaFrameReader](process-media-frames-with-mediaframereader.md) | Beschreibt, wie Sie [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) mit [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) verwenden, um Medienframes aus einer oder mehreren verfügbaren Quellen abzurufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entwickelt, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.  |
| [Get a preview frame](get-a-preview-frame.md) | Beschreibt, wie Sie einen einzelnen Vorschauframe aus dem Vorschaustream der Medienaufnahme abrufen.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>UWP-App-Beispiele für Kamera

* [Camera face detection sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [Camera preview frame sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [Camera HDR sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [Camera manual controls sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [Camera profile sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [Camera resolution sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [Camera starter kit](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [Camera video stabilization sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>Verwandte Themen

* [Audio, Video und Kamera](index.md)
 

 




