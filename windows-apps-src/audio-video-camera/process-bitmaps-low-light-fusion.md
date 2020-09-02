---
description: In diesem Artikel wird erläutert, wie Sie die lowlightfusion-Klasse zum Verarbeiten von Bitmaps verwenden.
title: Verarbeiten von Bitmaps mit der Low Light Fusion-API
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, Low Light Fusion, Bitmaps, Image processing
ms.localizationpriority: medium
ms.openlocfilehash: 4e82eb780efe83125a09417f349f84ee9451c1f0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363833"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Verarbeiten von Bitmaps mit der LowLightFusion-API

Low-Light-Images sind schwer zu erfassen mit guter Bildqualität, insbesondere auf mobilen Geräten mit fester Öffnung und Sensorgröße. Um eine geringe Beleuchtung zu kompensieren, können Geräte die Erkennungs Zeit oder den Sensor Gewinn erhöhen. Dies kann zu Bewegungsunschärfe und erhöhtem Rauschen in Bildern führen. 

Die [lowlightfusion-Klasse](/uwp/api/windows.media.core.lowlightfusion) verbessert die Qualität von Low-Light-Bildern, indem Pixel Informationen aus mehreren Frames in unmittelbarer Temporaler Nähe, d. h. kurze Burst Bilder, Abtast werden, um Rauschen und Bewegungsunschärfe zu verringern. Dies ist eine nützliche Funktion, um z. b. eine Fotobearbeitungs-App hinzuzufügen.

Diese Funktion wird auch über die [advancedphotocapture-Klasse](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)zur Verfügung gestellt, die den Low Light Fusion-Algorithmus auf eine Sequenz von Bildern direkt nach dem Erfassen der Bilder anwendet, wenn dies erforderlich ist. Weitere Informationen zur Implementierung dieses Features finden Sie unter [Low-Light Photo](./high-dynamic-range-hdr-photo-capture.md#low-light-photo-capture) Capture.

## <a name="prepare-the-images-for-processing"></a>Vorbereiten der Abbilder für die Verarbeitung

In diesem Beispiel zeigen wir, wie die [lowlightfusion-Klasse](/uwp/api/windows.media.core.lowlightfusion)und die [fileopenpicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) -Klasse verwendet werden, um Benutzern die Auswahl mehrerer Bilder zu ermöglichen, um eine geringe helle Fusion zu ermöglichen.

Zuerst müssen wir ermitteln, wie viele Bilder (auch als Frames bezeichnet) der Algorithmus akzeptiert, und eine Liste erstellen, die diese Frames enthält.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetMaxLLFFrames":::

Nachdem wir festgelegt haben, wie viele Frames der Low Light Fusion-Algorithmus akzeptiert, können wir mithilfe von [fileopenpicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) festlegen, welche Bilder im Algorithmus verwendet werden sollen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetFrames":::

Nachdem wir nun die richtige Anzahl von Frames ausgewählt haben, müssen wir die Frames in " [softwarframemaps](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) " decodieren und sicherstellen, dass die softwarframemaps das richtige Format für lowlightfusion aufweisen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetDecodeFrames":::


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Verbinden der Bitmaps zu einer einzelnen Bitmap

Nachdem wir nun über eine korrekte Anzahl von Frames in einem akzeptablen Format verfügen, können wir mit der **[fumenasync](/uwp/api/windows.media.core.lowlightfusion.fuseasync)** -Methode den Low Light Fusion-Algorithmus anwenden. Unser Ergebnis ist das verarbeitete Image mit verbesserter Übersichtlichkeit in Form einer softwarebitmap. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetFuseFrames":::

Zum Schluss bereinigen wir die resultierende Software Update Struktur durch Codieren und speichern in ein benutzerfreundliches, "reguläres" Bild, ähnlich wie bei den Eingabe Bildern, mit denen wir begonnen haben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetEncodeFrame":::


## <a name="before-and-after"></a>Vorher und nachher

Im folgenden finden Sie ein Beispiel für ein Eingabebild und das resultierende Ausgabe Bild nach dem Anwenden des Low Light Fusion-Algorithmus.

> [!div class="mx-tableFixed"] 
| Eingabe Rahmen | Ausgabe mit geringer Licht Fusion | 
|-------------|-------------------------|
| ![Eingabe Frame zum Low Light Fusion-Algorithmus](./images/LLF-Input.png) | ![Ergebnis Rahmen des Low Light Fusion-Algorithmus](./images/LLF-Output.png) |

Sie können im Eingabe Rahmen sehen, dass die Beleuchtung und die Klarheit der Schatten, die das Banner betreffen, verbessert wurden.

## <a name="related-topics"></a>Verwandte Themen 
[Lowlightfusion-Klasse](/uwp/api/windows.media.core.lowlightfusion)  
[Lowlightfusionresult-Klasse](/uwp/api/windows.media.core.lowlightfusionresult)
