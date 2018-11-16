---
author: laurenhughes
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: In diesem Artikel wird das Laden und Speichern von Bilddateien mit BitmapDecoder und BitmapEncoder sowie das Verwenden des SoftwareBitmap-Objekts zum Darstellen von Bitmapbildern erläutert.
title: Erstellen, Bearbeiten und Speichern von Bitmapbildern
ms.author: lahugh
ms.date: 03/22/2018
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dc7d3d70291d29102af614f29fd4531523a961e1
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "6985951"
---
# <a name="create-edit-and-save-bitmap-images"></a>Erstellen, Bearbeiten und Speichern von Bitmapbildern



In diesem Artikel wird das Laden und Speichern von Bilddateien mit [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) und [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) sowie das Verwenden des [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekts zum Darstellen von Bitmapbildern erläutert.

Die **SoftwareBitmap**-Klasse ist eine vielseitige API, die aus mehreren Quellen, z.B. Bilddateien, [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/br243259)-Objekten, Direct3D-Oberflächen und Code erstellt werden kann. **SoftwareBitmap** ermöglicht Ihnen die einfache Konvertierung zwischen verschiedenen Pixelformaten und Alphamodi und einen Zugriff auf niedriger Ebene auf Pixeldaten. **SoftwareBitmap** stellt des Weiteren eine gemeinsame Schnittstelle dar, die von mehreren Features von Windows verwendet wird, darunter:

-   [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/dn278725) ermöglicht Ihnen das Abrufen von durch die Kamera erfassten Frames als **SoftwareBitmap**.

-   [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) ermöglicht Ihnen das Abrufen einer **SoftwareBitmap**-Repräsentation eines **VideoFrame**.

-   [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) ermöglicht Ihnen das Erkennen von Gesichtern in einer **SoftwareBitmap**.

Der Beispielcode in diesem Artikel verwendet APIs aus den folgenden Namespaces.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Erstellen eines SoftwareBitmap-Objekts aus einer Bilddatei mit „BitmapDecoder“

Rufen Sie zum Erstellen eines [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekts aus einer Datei eine Instanz von [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) ab, die die Bilddaten enthält. In diesem Beispiel wird [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) verwendet, damit Benutzer eine Bilddatei auswählen können.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

Rufen Sie die [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116)-Methode des **StorageFile**-Objekts auf, um einen Datenstrom mit wahlfreiem Zugriff mit den Bilddaten abzurufen. Rufen Sie die statische [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182)-Methode auf, um eine Instanz der [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176)-Klasse für den angegebenen Datenstrom abzurufen. Rufen Sie [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332) auf, um ein [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)-Objekt mit dem Bild abzurufen.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>Speichern eines SoftwareBitmap-Objekts in einer Datei mit „BitmapEncoder“

Rufen Sie zum Speichern eines **SoftwareBitmap**-Objekts in einer Datei eine Instanz von **StorageFile** auf, in der das Bild gespeichert wird. In diesem Beispiel wird [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) verwendet, damit Benutzer eine Ausgabedatei auswählen können.

[!code-cs[PickOuputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOuputFile)]

Rufen Sie die [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116)-Methode des **StorageFile**-Objekts auf, um einen Datenstrom mit wahlfreiem Zugriff abzurufen, in den das Bild geschrieben wird. Rufen Sie die statische [**BitmapEncoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226211)-Methode auf, um eine Instanz der [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206)-Klasse für den angegebenen Datenstrom abzurufen. Der erste Parameter für **CreateAsync** ist eine GUID, die den zum Codieren des Bilds zu verwendenden Codec darstellt. Die **BitmapEncoder**-Klasse stellt eine Eigenschaft bereit, die die ID für jeden vom Encoder unterstützten Codec enthält, z.B. [**JpegEncoderId**](https://msdn.microsoft.com/library/windows/apps/br226226).

Verwenden Sie die [**SetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887337)-Methode zum Festlegen des Bilds, das codiert werden soll. Sie können auch Werte der [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254)-Eigenschaft festlegen, um grundlegende Transformationen auf das Bild anzuwenden, während es codiert wird. Die [**IsThumbnailGenerated**](https://msdn.microsoft.com/library/windows/apps/br226225)-Eigenschaft bestimmt, ob eine Miniaturansicht vom Encoder generiert wird. Beachten Sie, dass nicht alle Dateiformate die Miniaturansicht unterstützen. Deshalb sollten Sie bei Verwendung dieses Features die Fehler zu nicht unterstützten Vorgängen auffangen, die ausgelöst werden, wenn die Miniaturansicht nicht unterstützt wird.

Rufen Sie die [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216)-Methode auf, damit der Encoder die Bilddaten in die angegebene Datei schreibt.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

Sie können beim Erstellen von **BitmapEncoder** zusätzliche Codierungsoptionen angeben, indem Sie ein neues [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338)-Objekt erstellen und dieses mit einem oder mehreren [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687)-Objekten auffüllen, die die Encodereinstellungen darstellen. Eine Liste der unterstützten Encoderoptionen finden Sie unter [Referenz zu BitmapEncoder-Optionen](bitmapencoder-options-reference.md).

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>Verwenden von „SoftwareBitmap“ mit einem XAML-Image-Steuerelement

Um ein Bild unter Verwendung des [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752)-Steuerelements auf einer XAML-Seite anzuzeigen, definieren Sie zunächst ein **Image**-Steuerelement in der XAML-Seite.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

Zurzeit unterstützt das Steuerelement **Image** nur Bilder, die BGRA8-Codierung und einen vormultiplizierten oder No-Alpha-Kanal verwenden. Bevor Sie versuchen, ein Bild anzuzeigen, sollten Sie einen Test durchführen, um sicherzustellen, dass es das richtige Format hat. Wenn dies nicht der Fall ist, verwenden Sie die statische [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362)-Methode von **SoftwareBitmap**, um das Bild in das unterstützte Format zu konvertieren.

Erstellen Sie ein neues [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854)-Objekt. Legen Sie den Inhalt des Quellobjekts durch Aufrufen von [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) fest, und übergeben Sie dabei ein **SoftwareBitmap**-Objekt. Anschließend können Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760)-Eigenschaft des **Image**-Steuerelements auf das neu erstellte **SoftwareBitmapSource**-Objekt festlegen.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

Sie können auch **SoftwareBitmapSource** verwenden, um für [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) ein **SoftwareBitmap**-Objekt als [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/br210105) festzulegen.

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>Erstellen eines SoftwareBitmap-Objekts aus „WriteableBitmap“

Sie können ein **SoftwareBitmap**-Objekt aus einem vorhandenen **WriteableBitmap**-Objekt erstellen, indem Sie die [**SoftwareBitmap.CreateCopyFromBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887370)-Methode aufrufen und die **PixelBuffer**-Eigenschaft des **WriteableBitmap**-Objekts zum Festlegen der Pixeldaten bereitstellen. Mit dem zweiten Argument können Sie ein Pixelformat für das neu erstellte **WriteableBitmap**-Objekt anfordern. Sie können die [**PixelWidth**](https://msdn.microsoft.com/library/windows/apps/br243253)- und [**PixelHeight**](https://msdn.microsoft.com/library/windows/apps/br243251)-Eigenschaften des **WriteableBitmap**-Objekts zum Festlegen der Abmessungen des neuen Bilds verwenden.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>Programmgesteuertes Erstellen oder Bearbeiten eines SoftwareBitmap-Objekts

Bisher wurde in diesem Thema das Arbeiten mit Bilddateien behandelt. Sie können ein neues **SoftwareBitmap**-Objekt auch programmgesteuert im Code erstellen und das gleiche Verfahren zum Zugreifen auf und Bearbeiten der Pixeldaten von **SoftwareBitmap** verwenden.

**SoftwareBitmap** verwendet COM-Interop, um den Rohdatenpuffer mit Pixeldaten zur Verfügung zu stellen.

Zur Verwendung von COM-Interop müssen Sie einen Verweis auf den **System.Runtime.InteropServices**-Namespace in Ihr Projekt einbeziehen.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

Initialisieren Sie die [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505)-COM-Schnittstelle, indem Sie Ihrem Namespace den folgenden Code hinzufügen.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

Erstellen Sie ein neues **SoftwareBitmap**-Objekt mit dem gewünschten Pixelformat und der gewünschten Größe. Sie können auch ein vorhandenes **SoftwareBitmap**-Objekt verwenden, für das Sie die Pixeldaten bearbeiten möchten. Rufen Sie die [**SoftwareBitmap.LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887380)-Methode zum Abrufen einer Instanz der [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325)-Klasse auf, die den Pixeldatenpuffer darstellt. Wandeln Sie **BitmapBuffer** in die **IMemoryBufferByteAccess**-COM-Schnittstelle um, und rufen Sie dann [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506) auf, um ein Bytearray mit Daten aufzufüllen. Verwenden Sie die [**BitmapBuffer.GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330)-Methode zum Abrufen eines [**BitmapPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887342)-Objekts, mit dem Sie den Offset in den Puffer für jedes Pixel berechnen können.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

Da diese Methode auf den Rohdatenpuffer zugreift, der Windows-Runtime-Typen zugrunde liegt, muss diese mithilfe des Schlüsselworts **unsafe** deklariert werden. Außerdem müssen Sie das Projekt in Microsoft Visual Studio so konfigurieren, dass die Kompilierung von unsicherem Code zugelassen wird, indem Sie die Seite **Eigenschaften** des Projekts öffnen, auf die Eigenschaftenseite **Build** klicken und das Kontrollkästchen **Unsicheren Code zulassen** aktivieren.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Erstellen eines SoftwareBitmap-Objekts aus einer Direct3D-Oberfläche

Zum Erstellen eines **SoftwareBitmap**-Objekts aus einer Direct3D-Oberfläche müssen Sie den [**Windows.Graphics.DirectX.Direct3D11**](https://msdn.microsoft.com/library/windows/apps/dn895104)-Namespace in Ihr Projekt einbeziehen.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

Rufen Sie die [**CreateCopyFromSurfaceAsync**](https://msdn.microsoft.com/library/windows/apps/dn887373)-Methode zum Erstellen eines neuen **SoftwareBitmap**-Objekts aus der Oberfläche auf. Wie der Name schon sagt, verfügt das neue **SoftwareBitmap**-Objekt über eine separate Kopie der Bilddaten. Änderungen am **SoftwareBitmap**-Objekt haben keine Auswirkungen auf die Direct3D-Oberfläche.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>Konvertieren eines SoftwareBitmap-Objekts in ein anderes Pixelformat

Die **SoftwareBitmap**-Klasse stellt die statische [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362)-Methode dar, mit der Sie auf einfache Weise ein neues **SoftwareBitmap**-Objekt erstellen können, das Pixelformat und Alphamodus aus einem vorhandenen **SoftwareBitmap**-Objekt verwendet. Beachten Sie, dass die neu erstellte Bitmap eine separate Kopie der Bilddaten darstellt. Änderungen an der neuen Bitmap haben keine Auswirkungen auf die Quellbitmap.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>Transcodieren einer Bilddatei

Sie können eine Bilddatei direkt von einem [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) in ein [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) transcodieren. Erstellen Sie eine [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)-Schnittstelle aus der Datei, die transcodiert werden soll. Erstellen Sie anhand des Eingabedatenstroms ein neues **BitmapDecoder**-Objekt. Erstellen Sie eine neue [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720)-Klasse für den Encoder zum Schreiben in und Aufrufen von [**BitmapEncoder.CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214), indem Sie den In-Memory-Datenstrom und das Decoderobjekt übergeben. Codierungsoptionen werden beim Transcodieren nicht unterstützt. Verwenden Sie stattdessen [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync). Alle Eigenschaften in der Eingabebilddatei, die Sie nicht speziell für den Encoder festlegen, werden unverändert in die Ausgabedatei geschrieben. Rufen Sie die [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216)-Methode auf, damit der Encoder den In-Memory-Datenstrom codiert. Navigieren Sie schließlich zum Anfang des Dateidatenstroms und des In-Memory-Datenstroms, und rufen Sie die [**CopyAsync**](https://msdn.microsoft.com/library/windows/apps/hh701827)-Methode auf, um den In-Memory-Datenstrom in den Dateidatenstrom zu schreiben.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>Verwandte Themen

* [Referenz zu BitmapEncoder-Optionen](bitmapencoder-options-reference.md)
* [Bildmetadaten](image-metadata.md)
 

 




