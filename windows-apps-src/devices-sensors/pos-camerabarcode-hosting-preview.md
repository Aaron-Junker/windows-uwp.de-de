---
title: Hosting-Vorschau für Kamera-Strichcodescanner
description: Hier erfahren Sie, wie Sie die Vorschau für Kamera-Strichcodescanner in Ihrer Anwendung hosten
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: b3391a7640e49fb43ac0f7ea33a0fa992c4a7495
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243283"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Die Vorschau für Kamera-Strichcodescanner in Ihrer Anwendung hosten
## <a name="step-1-setup-your-camera-preview"></a>Schritt 1: Einrichten der Kamera Vorschau
Der erste Schritt beim Hinzufügen einer Vorschau Ihrer Anwendung für Kamera-Strichcodescanner kann mithilfe der Anweisungen im Thema [Anzeigen der Kameravorschau](../audio-video-camera/simple-camera-preview-access.md) erreicht werden.  Nachdem Sie diesen Schritt abgeschlossen haben, kehren Sie zu diesem Thema zurück, um Änderungen am Kamera-Strichcodescanner vorzunehmen.

## <a name="step-2-update-capability-declarations"></a>Schritt 2: Funktions Deklarationen aktualisieren
Um zu verhindern, dass Ihre Benutzer die Zustimmungsaufforderung für das Mikrofon erhalten, können Sie dies von den aufgeführten Funktionen im App-Manifest ausschließen.

1. Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2. Wählen Sie die Registerkarte **Funktionen** aus.
3. Deaktivieren Sie das Kontrollkästchen für **Mikrofon**.

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Schritt 3: Zusätzliche using-Direktive für Medien Erfassung hinzufügen

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Schritt 4: Einrichten der mediacapture-Initialisierungs Einstellungen
Die folgende Beispiel wird [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings) initialisiert. 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>Schritt 5: Zuordnen des mediacapture-Objekts zum Kamera Barcode Scanner
Ersetzen Sie den vorhandenen mediaCapture.InitializeAsync() in *StartPreviewAsync()* durch:

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> Unter [Anzeigen der Kameravorschau](https://docs.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access#add-capability-declarations-to-the-app-manifest) finden Sie weitere Informationen und erweiterte Themen zum Hosten einer Kameravorschau in Ihrer Anwendung.

## <a name="see-also"></a>Siehe auch

### <a name="samples"></a>Proben

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
