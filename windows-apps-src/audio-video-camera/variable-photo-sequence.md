---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: In diesem Artikel wird erläutert, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden.
title: Variable Fotosequenz
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 376d5cb011ab4a72d7715a36a1522ab91303c1f9
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363713"
---
# <a name="variable-photo-sequence"></a>Variable Fotosequenz



In diesem Artikel wird erläutert, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden. Mit diesem Feature können Sie z. B. HDR-Bilder (High Dynamic Range) erstellen.

Wenn Sie HDR-Bilder aufnehmen, aber nicht Ihre eigenen Algorithmen zur Bildverarbeitung implementieren möchten, können Sie mithilfe der [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) API die in Windows integrierten HDR-Funktionen verwenden. Weitere Informationen finden Sie unter [HDR-Fotoaufnahmen (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md).

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienerfassung in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>Einrichten Ihrer App mithilfe der Aufnahme einer variablen Fotosequenz

Neben den Namespaces für die grundlegende Medienerfassung sind für die Implementierung der Aufnahme einer variablen Fotosequenz folgende Namespaces erforderlich.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVPSUsing":::

Deklarieren Sie eine Membervariable, um das [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture)-Objekt zu speichern, das verwendet wird, um die Aufnahme der Fotosequenz zu initiieren. Deklarieren Sie ein Array von [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekten, um die einzelnen aufgenommenen Bilder in der Sequenz zu speichern. Deklarieren Sie außerdem ein Array, um das [**CapturedFrameControlValues**](/uwp/api/Windows.Media.Capture.CapturedFrameControlValues)-Objekt für jeden Frame zu speichern. Dazu können Sie Ihren Algorithmus zur Bildverarbeitung verwenden, um zu ermitteln, welche Einstellungen zum Aufnehmen der einzelnen Frames verwendet wurden. Abschließend deklarieren Sie einen Index, mit dem Sie nachverfolgen können, welches Bild in der Sequenz gerade aufgenommen wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVPSMemberVariables":::

## <a name="prepare-the-variable-photo-sequence-capture"></a>Vorbereiten der Aufnahme einer variablen Fotosequenz

Nachdem Sie [MediaCapture](./index.md) initialisiert haben, müssen Sie sicherstellen, dass die variablen Fotosequenzen auf dem aktuellen Gerät unterstützt werden, indem Sie eine Instanz von [**VariablePhotoSequenceController**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) aus dem [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) der Medienerfassung abrufen und die [**Supported**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported)-Eigenschaft überprüfen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsVPSSupported":::

Rufen Sie ein [**FrameControlCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities)-Objekt von dem Controller der variablen Fotosequenz ab. Dieses Objekt besitzt eine Eigenschaft für jede Einstellung, die pro Frame einer Fotosequenz konfiguriert werden kann. Dazu gehören:

-   [**Offenlegung**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**Blinken**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**Fokus**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

In diesem Beispiel wird ein anderer Belichtungskorrekturwert für jeden Frame festgelegt. Um sicherzustellen, dass die Belichtungskorrektur für die Fotosequenzen auf dem aktuellen Gerät unterstützt wird, überprüfen Sie die [**Supported**](/uwp/api/windows.media.devices.exposurecompensationcontrol.supported)-Eigenschaft des [**FrameExposureCompensationCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities)-Objekts, auf das Sie über die **ExposureCompensation**-Eigenschaft zugreifen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsExposureCompensationSupported":::

Erstellen Sie ein neues [**FrameController**](/uwp/api/Windows.Media.Devices.Core.FrameController)-Objekt für die einzelnen Frames, die Sie aufnehmen möchten. In diesem Beispiel werden drei Frames aufgenommen. Legen Sie die Werte für die Steuerelemente fest, die Sie für jeden Frame variieren möchten. Löschen Sie dann die [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers)-Auflistung von **VariablePhotoSequenceController**, und fügen Sie der Auflistung die einzelnen Framecontroller hinzu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetInitFrameControllers":::

Erstellen Sie ein [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt, um die Codierung festzulegen, die Sie für die aufgenommenen Bilder verwenden möchten. Rufen Sie die statische Methode [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync) auf, und übergeben Sie die Codierungseigenschaften. Diese Methode gibt ein [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture)-Objekt zurück. Registrieren Sie abschließend Ereignishandler für das [**PhotoCaptured**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured)-Ereignis und das [**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped)-Ereignis.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPrepareVPS":::

## <a name="start-the-variable-photo-sequence-capture"></a>Starten der Aufnahme einer variablen Fotosequenz

Rufen Sie zum Starten der Aufnahme der variablen Fotosequenz [**VariablePhotoSequenceCapture.StartAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync) auf. Achten Sie darauf, dass Sie Arrays zum Speichern der aufgenommenen Bilder und Werte des Frame-Steuerelements initialisieren und den aktuellen Index auf 0 festlegen. Legen Sie für Ihre App die Aufnahmezustandsvariable fest, und aktualisieren Sie Ihre Benutzeroberfläche, um das Starten einer weiteren Aufnahme zu deaktivieren, während diese Aufnahme läuft.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetStartVPSCapture":::

## <a name="receive-the-captured-frames"></a>Empfangen der aufgenommenen Frames

Das [**PhotoCaptured**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured)-Ereignis wird für jeden aufgenommenen Frame ausgelöst. Speichern Sie die Werte des Frame-Steuerelements und das aufgenommene Bild für den Frame, und erhöhen Sie dann den aktuellen Frameindex. Dieses Beispiel zeigt, wie Sie eine [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Darstellung der einzelnen Frames abrufen können. Weitere Informationen zur Verwendung von **SoftwareBitmap** finden Sie unter [Bildverarbeitung](imaging.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOnPhotoCaptured":::

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>Abschluss der Aufnahme der variablen Fotosequenz

Das [**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped)-Ereignis wird ausgelöst, nachdem alle Frames in der Sequenz aufgenommen wurden. Aktualisieren Sie den Aufnahmestatus Ihrer App und Ihre Benutzeroberfläche, damit Benutzer neue Aufzeichnungen initiieren können. An dieser Stelle können Sie die aufgenommenen Bilder und Werte des Frame-Steuerelements an Ihren Bildverarbeitungscode übergeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOnStopped":::

## <a name="update-frame-controllers"></a>Aktualisieren von Framecontrollern

Wenn Sie eine weitere Aufnahme einer variablen Fotosequenz mit anderen Einstellungen pro Frame durchführen möchten, müssen Sie **VariablePhotoSequenceCapture** nicht komplett neu initialisieren. Löschen Sie entweder die [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers)-Auflistung, und fügen Sie neue Framecontroller hinzu, oder ändern Sie die vorhandenen Framecontrollerwerte. Im folgenden Beispiel wird das [**FrameFlashCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities)-Objekt überprüft, um sicherzustellen, dass das aktuelle Gerät Blitz und Blitzleistung für Frames variabler Fotosequenzen unterstützt. In diesem Fall wird jeder Frame aktualisiert, um den Blitz bei 100 % Energieverbrauch zu aktivieren. Die Belichtungskorrekturwerte, die zuvor für jeden Frame festgelegt wurden, sind weiterhin aktiv.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUpdateFrameControllers":::

## <a name="clean-up-the-variable-photo-sequence-capture"></a>Bereinigen der Aufnahme einer variablen Fotosequenz

Bereinigen Sie nach der Aufnahme variabler Fotosequenzen oder nach dem Anhalten Ihrer App das entsprechende Objekt, indem Sie [**FinishAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync) aufrufen. Heben Sie die Registrierung der Ereignishandler für das Objekt auf, und legen Sie es auf NULL fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpVPS":::

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
