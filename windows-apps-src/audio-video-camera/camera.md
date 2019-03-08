---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: In diesem Artikel werden die für UWP-Apps verfügbaren Kamera-Features aufgeführt, sowie die Links zu den Anleitungen für ihre Verwendung.
title: Kamera
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e190a6d5134cc1fba4ac8be970bb8d90847700e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594055"
---
# <a name="camera"></a>Kamera

Dieser Abschnitt enthält Richtlinien für das Erstellen von UWP (Universelle Windows-Plattform)-Apps, die die Kamera oder das Mikrofon verwenden, um Fotos, Videos oder Audiodateien zu erfassen.

## <a name="use-the-windows-built-in-camera-ui"></a>Verwenden der in Windows integrierten Kamera-UI

| Thema | Beschreibung |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Aufzeichnen von Fotos und Videos mit Windows integrierten Kamera-Benutzeroberfläche](capture-photos-and-video-with-cameracaptureui.md) | Zeigt, wie Sie die [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI)-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden. Wenn Sie einfach den Benutzer dazu befähigen möchten, ein Foto oder Video aufzuzeichnen und das Ergebnis an Ihre App zurückzugeben, ist dies die schnellste und einfachste Möglichkeit dafür.  |

## <a name="basic-mediacapture-tasks"></a>Grundlegende MediaCapture-Funktionen

| Thema | Beschreibung |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Zeigen Sie die Kameravorschau](simple-camera-preview-access.md) | Beschreibt, wie Sie in einer UWP-App innerhalb einer XAML-Seite den Kameravorschau-Stream schnell anzeigen können. |
| [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Zeigt die einfachste Möglichkeit zum Aufnehmen von Fotos und Videos mit der [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)-Klasse. Die **MediaCapture**-Klasse stellt einen leistungsfähigen Satz von APIs bereit, der eine Steuerung der Aufnahmepipeline auf unterster Ebene sowie fortgeschrittene Aufnahmeszenarien ermöglicht. Dieser Artikel soll Ihnen jedoch helfen, Ihre App schnell und einfach durch grundlegende Medienaufnahmefunktionen zu erweitern. |
| [Funktionen der Kamera-Benutzeroberfläche für mobile Geräte](camera-ui-features-for-mobile-devices.md) | Beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Erweiterte MediaCapture-Funktionen   
                                                                                                               
| Thema                                                                                             | Beschreibung                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Verarbeiten von Gerät und der Bildschirm-Ausrichtung mit MediaCapture](handle-device-orientation-with-mediacapture.md) | Beschreibt, wie Sie beim Aufnehmen von Fotos und Videos mit einer Hilfsklasse die Geräteausrichtung handhaben. | 
| [Ermitteln Sie, und wählen Sie die Kamera-Funktionen, mit der Kamera-Profile](camera-profiles.md) | Beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen. |
| [Das festgelegte Format, Auflösung und Framerate für MediaCapture](set-media-encoding-properties.md) | Beschreibt, wie Sie die [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011)-Schnittstelle verwenden, um die Auflösung und Bildfrequenz des Kameravorschau-Datenstroms sowie von aufgenommenen Fotos und Videos festzulegen. Außerdem wird beschrieben, wie Sie sicherstellen, dass das Seitenverhältnis des Vorschaudatenstroms mit dem Seitenverhältnis der aufgenommenen Medien übereinstimmt. |
| [HDR und Licht Fotoaufnahmen](high-dynamic-range-hdr-photo-capture.md) | Beschreibt, wie Sie die [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse verwenden, um HDR (High Dynamic Range)-Fotos und Fotos bei schlechten Lichtverhältnissen aufzunehmen. |
| [Steuerelemente der manuellen Kamera für Fotos und Videos erfassen](capture-device-controls-for-photo-and-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen. |
| [Steuerelemente der manuellen Kamera für Videoaufnahmen](capture-device-controls-for-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, einschließlich HDR-Video und Belichtungspriorität, zu ermöglichen.  |
| [Videostabilisierungs-Effekt für erfassen](effects-for-video-capture.md) | Beschreibt, wie Sie den Videostabilisierungseffekt verwenden.  |
| [Szene Analysis für MediaCapture](scene-analysis-for-media-capture.md) | Beschreibt, wie Sie den [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.SceneAnalysisEffect) und den [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.FaceDetectionEffect) verwenden, um den Inhalt des Vorschaudatenstroms der Medienaufnahme zu analysieren.  |
| [Eine Foto-Sequenz mit VariablePhotoSequence erfassen](variable-photo-sequence.md) | Beschreibt, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden.  |
| [Verarbeiten von Medien-Frames mit MediaFrameReader](process-media-frames-with-mediaframereader.md) | Beschreibt, wie Sie [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) mit [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) verwenden, um Medienframes aus einer oder mehreren verfügbaren Quellen abzurufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entworfen, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.  |
| [Erhalten Sie einen Frame (Vorschau)](get-a-preview-frame.md) | Beschreibt, wie Sie einen einzelnen Vorschauframe aus dem Vorschaustream der Medienaufnahme abrufen.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>UWP-App-Beispiele für Kamera

* [Beispiel für Camera Gesicht Erkennung](https://go.microsoft.com/fwlink/p/?LinkID=619486&clcid=0x409)
* [Beispiel für Camera Preview frame](https://go.microsoft.com/fwlink/p/?LinkID=620516&clcid=0x409)
* [Kamera HDR-Beispiel](https://go.microsoft.com/fwlink/p/?LinkID=620517&clcid=0x409)
* [Die manuellen Steuerelemente Beispiel Camera](https://go.microsoft.com/fwlink/p/?LinkID=627611&clcid=0x409)
* [Beispiel für Camera-Profil](https://go.microsoft.com/fwlink/p/?LinkID=620518&clcid=0x409)
* [Kamera-Beispiel "Auflösung"](https://go.microsoft.com/fwlink/p/?LinkID=624252&clcid=0x409)
* [Starterkit für Kamera](https://go.microsoft.com/fwlink/p/?LinkID=619479&clcid=0x409)
* [Videostabilisierungs-Beispiel Camera](https://go.microsoft.com/fwlink/p/?LinkID=620519&clcid=0x409)

## <a name="related-topics"></a>Verwandte Themen

* [Audio, Video und Kamera](index.md)
 

 




