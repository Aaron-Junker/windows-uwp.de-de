---
title: Hostingvorschau für Kamera Barcode Scanner
description: Erfahren Sie, wie Sie eine Kamera-Barcode Scanner-Vorschau in Ihrer Anwendung hosten.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 6657fa52a5e3cac0821def7102b4bc93089b2ffe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159634"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Hosting einer Kamera-Barcode Scanner-Vorschau in Ihrer Anwendung
## <a name="step-1-setup-your-camera-preview"></a>Schritt 1: Einrichten der Kamera Vorschau
Der erste Schritt beim Hinzufügen einer Vorschau zu Ihrer Anwendung für den Kamera Barcode Scanner kann durch Befolgen der Anweisungen im Thema [Anzeigen der Kamera Vorschau](../audio-video-camera/simple-camera-preview-access.md) erreicht werden.  Nachdem Sie diesen Schritt abgeschlossen haben, kehren Sie zu diesem Thema für die Kamera-Barcode Scanner-spezifischen Änderungen zurück.

## <a name="step-2-update-capability-declarations"></a>Schritt 2: Aktualisieren von Funktions Deklarationen
Um zu verhindern, dass Ihre Benutzer die Zustimmungsaufforderung für das Mikrofon erhalten, können Sie dies von den Funktionen ausschließen, die im App-Manifest aufgeführt sind.

1. Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2. Wählen Sie die Registerkarte **Funktionen** aus.
3. Deaktivieren Sie das Kontrollkästchen für das **Mikrofon** .

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Schritt 3: Hinzufügen zusätzlicher using-Direktiven für die Medien Erfassung

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Schritt 4: Einrichten der mediacapture-Initialisierungs Einstellungen
Im folgenden Beispiel wird die [**mediacaptureinitializationsettings**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings)-Eigenschaft initialisiert. 

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
Ersetzen Sie den vorhandenen mediaCapture.Initializeasync () in *startpreviewasync ()* durch Folgendes:

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
> Unter [Anzeigen der Kamera Vorschau](../audio-video-camera/simple-camera-preview-access.md#add-capability-declarations-to-the-app-manifest) finden Sie weiterführende Themen zum Hosting einer Kamera Vorschau in Ihrer Anwendung.

## <a name="see-also"></a>Weitere Informationen

### <a name="samples"></a>Beispiele

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)