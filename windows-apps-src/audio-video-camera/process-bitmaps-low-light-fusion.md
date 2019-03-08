---
description: In diesem Artikel wird erläutert, wie Sie die LowLightFusion-Klasse zum Verarbeiten von Bitmaps nutzen.
title: Verarbeitung von Bitmaps mit der Low Light Fusion-API
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, Uwp, low light Fusion, Bitmaps, bildbearbeitung
ms.localizationpriority: medium
ms.openlocfilehash: e0677d2e4ce5e412c158b8a00da3a6338bae6c46
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656215"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Verarbeitung von Bitmaps mit der LowLightFusion-API

Bilder bei schwachem Licht lassen sich nur schwer mit guter Bildqualität aufnehmen, vor allem auf mobilen Geräten mit fester Blende und Sensorgröße. Zur Kompensation von schlechten Lichtverhältnissen können Geräte die Belichtungszeit oder Sensorverstärkung erhöhen, was zu Bewegungsunschärfe und erhöhtem Bildrauschen führen kann. 

Die [LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)-Klasse verbessert die Qualität von Bildern bei schwachem Licht durch Abtastung von Pixelinformationen aus mehreren Bildern in unmittelbarer zeitlicher Nähe, d. h. kurze Burst-Bilder, um Rauschen und Bewegungsunschärfe zu reduzieren. Dies ist z. B. nützlich, um eine Fotobearbeitungsanwendung hinzuzufügen.

Diese Funktion wird auch durch die [AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse zur Verfügung gestellt, die den Low Light Fusion-Algorithmus bei Bedarf auf eine Reihe von Bildern direkt nach der Aufnahme anwendet. Weitere Informationen zur Implementierung dieser Funktion finden Sie unter [Fotoaufnahme bei schlechten Lichtverhältnissen](https://docs.microsoft.com/windows/uwp/audio-video-camera/high-dynamic-range-hdr-photo-capture#low-light-photo-capture).

## <a name="prepare-the-images-for-processing"></a>Vorbereiten von Bildern für die Bearbeitung

In diesem Beispiel zeigen wir Ihnen, wie Sie die [LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)-Klasse und den [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) verwenden können, damit ein Benutzer mehrere Bilder auswählen kann, um Low Light Fusion-durchzuführen.

Zuerst müssen wir herausfinden, wie viele Bilder (auch als Frames bekannt) der Algorithmus akzeptiert, und eine Liste erstellen, um diese Frames zu halten.

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

Sobald wir festgelegt haben, wie viele Frames der Low Light Fusion-Algorithmus akzeptiert, können wir [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) verwenden, um den Benutzer auswählen zu lassen, welche Bilder im Algorithmus verwendet werden sollen.

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

Nachdem wir nun die richtige Anzahl von Frames ausgewählt haben, müssen wir die Frames in [SoftwareBitmaps](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) dekodieren und sicherstellen, dass die SoftwareBitmaps im richtigen Format für LowLightFusion vorliegen.

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Verarbeiten der Bitmaps zu einer einzigen Bitmap

Da wir nun eine korrekte Anzahl von Frames in einem akzeptablen Format haben, können wir die **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion.fuseasync)**-Methode verwenden, um den Low Light Fusion-Algorithmus anzuwenden. Unser Ergebnis wird das bearbeitete Bild mit verbesserter Klarheit in Form einer SoftwareBitmap sein. 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

Schließlich bereinigen wir die resultierende SoftwareBitmap, indem wir sie kodieren und in ein benutzerfreundliches, „normales“ Bild speichern, ähnlich den Eingangsbildern, mit denen wir begonnen haben.

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>Vorher und nachher

Hier ist ein Beispiel für zwei Eingangsbilder und das resultierende Ausgangsbild nach Anwendung des Low Light Fusion Algorithmus.

> [!div class="mx-tableFixed"] 
| Eingabeframe | Low Light Fusion-Ausgabe | 
|-------------|-------------------------|
| ![Eingabeframe für den Low Light Fusion Algorithmus](./images/LLF-Input.png) | ![Ergebnisframe des Low Light Fusion Algorithmus](./images/LLF-Output.png) |

Die Eingabeframe zeigt, dass die Beleuchtung und die Klarheit des das Banner umgebenen Schattens verbessert wurde.

## <a name="related-topics"></a>Verwandte Themen 
[LowLightFusion-Klasse](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult-Klasse](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)