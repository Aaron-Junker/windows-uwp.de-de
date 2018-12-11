---
ms.assetid: ''
description: In diesem Artikel wird erläutert, wie Sie die SoftwareBitmap-Klasse mit der Open Source Computer Vision-Bibliothek (OpenCV) verwenden.
title: Prozess-Bitmaps mit OpenCV
ms.date: 03/19/2018
ms.topic: article
keywords: Windows10, UWP, Opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: 45f76744070a7557939d1d7f2307113852737072
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "8875032"
---
# <a name="process-bitmaps-with-opencv"></a>Prozess-Bitmaps mit OpenCV

In diesem Artikel wird erläutert, wie Sie die **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)**-Klasse verwenden, die von vielen verschiedenen UWP-APIs zur Darstellung von Bildern mit der Open Source Computer Vision-Bibliothek (OpenCV), Open-Source, der systemeigenen Code-Bibliothek verwendet wird, die eine Vielzahl von Algorithmen für die Bildverarbeitung bietet. 

Die Beispiele in diesem Artikel erläutern das Erstellen eines systemeigenen der Komponente für Windows-Runtime, die von einer UWP-App, einschließlich Apps, die mit C# erstellt werden, verwendet werden kann. Diese Helferkomponente stellt eine einzelne Methode bereit, **Weichzeichner**, die die Weichzeichner-Verarbeitungsfunktion von OpenCVs verwenden. Die Komponente implementiert private Methoden, die einen Zeiger auf den zugrunde liegenden Bilddatenpuffer abruft, der direkt von der OpenCV-Bibliothek verwendet werden kann. Dies vereinfacht das Erweitern der Helferkomponente, um andere OpenCV Verarbeitungsfeatures zu implementieren. 

* Eine Einführung in die Verwendung von **SoftwareBitmap** finden Sie im Artikel [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md). 
* Informationen zum Verwenden der OpenCV-Bibliothek finden Sie unter [http://opencv.org](http://opencv.org).
* Zur Verwendung der in diesem Artikel verwendeten OpenCV Helper-Komponente mit **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** zum Implementieren der Echtzeit-Image-Verarbeitung von Frames von einer Kamera, lesen Sie [Verwenden von OpenCV mit MediaFrameReader](use-opencv-with-mediaframereader.md).
* Ein vollständiges Codebeispiel mit unterschiedlichen implementierten Effekten finden Sie unter [Kamera-Frames + OpenCV – Beispiel](https://go.microsoft.com/fwlink/?linkid=854003) im GitHub-Repository für Beispiele für die Universelle Windows-Plattform.

> [!NOTE] 
> Die Technik, die von der OpenCV-Helferkomponente verwendet wird und die in diesem Artikel ausführlich beschrieben ist, erfordert, dass sich die zu verarbeitenden Bilddaten im CPU-Speicher und nicht im GPU-Speicher befinden. Bei APIs, die es Ihnen ermöglichen, den Speicherort der Bilder anzufordern wie beispielsweise der **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)**-Klasse, sollten Sie den CPU-Speicher angeben.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Erstellen Sie eine Helferkomponente für Windows-Runtime für die Interoperabilität mit OpenCV

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. Fügen Sie Ihrem Projekt einen systemeigenen Code für die Komponente für Windows-Runtime hinzu.

1. Fügen Sie der Lösung in Visual Studio ein neues Projekt hinzu, indem Sie mit der rechten Maustaste auf den Projektmappen-Explorer klicken und **Hinzufügen -> Neues Projekt** auswählen. 
2. Wählen Sie in der Kategorie **Visual C++** **Komponente für Windows-Runtime (Universal Windows)** aus. Geben Sie in diesem Beispiel den Namen "OpenCVBridge" ein und klicken Sie auf **OK**. 
3. Wählen Sie im Dialogfeld **Neues universelles Windows-Projekt** das Ziel und die Mindestbetriebssystemversion für Ihre App aus und klicken Sie dann auf **OK**.
4. Klicken Sie mit der rechten Maustaste auf die automatisch erstellte Datei „Class1.cpp” im Projektmappen-Explorer, und wählen Sie **Entfernen**, wenn das Bestätigungsdialogfeld angezeigt wird. Wählen Sie anschließend **Löschen** aus. Löschen Sie dann die Headerdatei "Class1.h".
5. Klicken Sie mit der rechten Maustaste auf das Symbol für das OpenCVBridge-Projekt und wählen Sie **Hinzufügen -> Klasse... ** aus. Geben Sie im Dialogfeld **Klasse hinzufügen** "OpenCVHelper" als **Klassennamen** ein, und klicken Sie dann auf **OK**. Der Code wird der erstellten Klassendateien in einem späteren Schritt hinzugefügt.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. Fügen Sie Ihrem Komponentenprojekt das OpenCV NuGet-Pakete hinzu

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt OpenCVBridge, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Wenn das Dialogfeld "NuGet-Paket-Manager" geöffnet wird, wählen Sie die Registerkarte **Durchsuchen** aus, und geben Sie "OpenCV.Win" in das Suchfeld ein.
3. Wählen Sie "OpenCV.Win.Core" aus und klicken Sie auf **installieren**. Klicken Sie im Dialogfeld **Vorschau** auf **OK**.
4. Verwenden Sie das gleiche Verfahren zum Installieren des "OpenCV.Win.ImgProc"-Pakets.

> [!NOTE]
> OpenCV.Win.Core und OpenCV.Win. ImgProc werden nicht regelmäßig aktualisiert, werden jedoch weiterhin für das Erstellen von OpenCVHelper empfohlen, wie auf dieser Seite beschrieben.

### <a name="3-implement-the-opencvhelper-class"></a>3. Implementieren Sie die OpenCVHelper-Klasse

Fügen Sie der Headerdatei „OpenCVHelper.h” folgenden Code hinzu. Dieser Code enthält OpenCV-Headerdateien für die installierten *Core* und *ImgProc*-Pakete und deklariert drei Methoden, die in den folgenden Schritten erklärt werden.

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

Löschen Sie den vorhandenen Inhalt der Datei „OpenCVHelper.cpp ”und fügen Sie die folgenden include-Direktiven hinzu. 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

Nach den Include-Direktiven, fügen Sie die folgenden **using**-Direktiven hinzu. 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

Fügen Sie als nächstes zu OpenCVHelper.cpp die Methode **GetPointerToPixelData** hinzu. Diese Methode akzeptiert ein **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** und ruft durch eine Reihe von Konvertierungen eine COM-Schnittstellendarstellung der Pixeldaten ab, mit deren Hilfe wir einen Zeiger auf den zugrunde liegenden Datenpuffer als **Zeichen**-Array abrufen. 

Als erstes wird ein **[BitmapBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer)** mit Pixeldaten abgerufen, der **[LockBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** aufruft und einen Lese-/Schreibzugriff zu Puffern anfordert, damit die OpenCV/Bibliothek die Pixeldaten bearbeiten kann.  **[CreateReference](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** wird zum Erstellen eines **[IMemoryBufferReference](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)**-Objekts aufgerufen. Als Nächstes wird die **IMemoryBufferByteAccess**-Schnittstelle in ein **IInspectable** umgewandelt, die Basisschnittstelle aller Windows-Runtime-Klassen, und **[QueryInterface ](https://msdn.microsoft.com/library/windows/desktop/ms682521(v=vs.85).aspx)** wird aufgerufen, um eine **[IMemoryBufferByteAccess](https://msdn.microsoft.com/library/mt297505(v=vs.85).aspx)**-COM-Schnittstelle aufzurufen, mit der wir den Pixeldatenpuffer als **Zeichen**-Array erhalten können. Füllen Sie anschließend das **Zeichen**-Array durch Aufrufen des **[IMemoryBufferByteAccess::GetBuffer](https://msdn.microsoft.com/library/mt297506(v=vs.85).aspx)**. Wenn einer der Konvertierungsschritte in dieser Methode fehlschlägt, gibt die Methode **false** an, was bedeutet, dass eine weitere Verarbeitung nicht fortgesetzt werden kann.

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

Fügen Sie als nächstes die unten aufgeführte Methode **TryConvert** hinzu. Diese Methode akzeptiert ein **SoftwareBitmap** und versucht, es in ein **Mat**-Objekt zu konvertieren, das das von OpenCV verwendete Matrix-Objekt zur Darstellung von Bilddatenpuffern ist. Diese Methode ruft die oben aufgeführte **GetPointerToPixelData**-Methode auf, um eine **Zeichen**-Array-Darstellung des Pixeldatenpuffers zu erhalten. Wenn dies erfolgreich ist, wird der Konstruktor für die **Mat**-Klasse aufgerufen und übergibt die Pixelbreite und -Höhe, die vom **SoftwareBitmap**-Quellobjekt erhalten wird. 

> [!NOTE] 
> In diesem Beispiel gibt die Konstante CV_8UC4 das Pixelformat für das erstellte **Mat**-Objekt an. Dies bedeutet, dass die an die **SoftwareBitmap** übergebe Methode einen **[BitmapPixelFormat](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)**-Eigenschaftswerte von **[BGRA8](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** mit vormultipliziertem Alpha haben muss, dem Äquivalent von CV_8UC4, damit dies in diesem Beispiel funktioniert.

Eine flache Kopie des erstellten **Mat**-Objekts wird von der Methode zurückgegeben, damit eine weitere Verarbeitung auf dem gleichen Daten-Pixeldatenpuffer verwendet wird, auf den die **SoftwareBitmap** verweist, und keine Kopie dieser Puffer.

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

Schließlich implementiert diese Beispielhilfsklasse eine einzelne Bildverarbeitungsmethode, **Weichzeichner**, die die **TryConvert**-Methode zum Abrufen des zuvor definierten **Mat**-Objekts verwendet, das die Quell-Bitmap und die Zielbitmap für den Weichzeichner-Vorgang darstellt und anschließend **Weichzeichner**-Methode aus der OpenCV ImgProc-Bibliothek abruft. Der andere Parameter für **Weichzeichner** gibt die Größe des Weichzeichnereffekts in X- und Y-Richtungen an.

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Ein einfaches Beispiel für SoftwareBitmap OpenCV mit Helferkomponenten
Nachdem die Komponente OpenCVBridge erstellt wurde, erstellen wir eine einfache C#-App, die die OpenCV-**Weichzeichner**-Methode zum Ändern einer **SoftwareBitmap** verwendet. Um auf die Komponente für Windows-Runtime von Ihrer UWP-App zuzugreifen, müssen Sie zunächst einen Verweis auf die Komponente hinzufügen. Klicken Sie mit der rechten Maustaste im Projektmappen-Explorer auf den Knoten **Verweise** unter Ihrem UWP-App-Projekt und wählen Sie **Verweis hinzufügen... ** aus. Wählen Sie im Dialogfeld "Verweis-Manager" **Solution-Projekte >** aus. Aktivieren Sie das Kontrollkästchen neben dem OpenCVBridge-Projekt, und klicken Sie auf **OK**.

Der nachfolgende Beispielcode ermöglicht es dem Benutzer, eine Bilddatei auszuwählen und verwendet dann **[BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)** zum Erstellen einer **SoftwareBitmap** -Darstellung des Bilds. Weitere Informationen zur Verwendung von **SoftwareBitmap** finden Sie im Artikel [Erstellen, Bearbeiten und Speichern von Bitmapbildern](https://docs.microsoft.com/windows/uwp/audio-video-camera/imaging).

Wie zuvor in diesem Artikel erläutert erfordert die **OpenCVHelper** Klasse, dass alle bereitgestellten **SoftwareBitmap**-Bilder im Pixelformat BGRA8-Format mit vormultiplizierten Alphawerten codiert werden. Wenn das Image nicht bereits in diesem Format existiert, ruft der Beispielcode **[Konvertieren](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** auf, um das Bild in das entsprechende Format zu konvertieren.

Als Nächstes wird ein **SoftwareBitmap** erstellt, um als Ziel für den Weichzeichnervorgang verwendet zu werden. Die Eingabe-Bildeigenschaften werden als Argumente für den Konstruktor verwendet, um eine Bitmap mit übereinstimmendem Format erstellen.

Es wird eine neue Instanz von **OpenCVHelper** erstellt, die **Weichzeichner**-Methode wird aufgerufen und übergibt die Quell- und Ziel-Bitmaps. Schließlich wird ein **SoftwareBitmapSource** erstellt, um dem Ausgabebild ein XAML **Bild**-Steuerelement zuzuweisen.


[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>Verwandte Themen

* [Referenz zu BitmapEncoder-Optionen](bitmapencoder-options-reference.md)
* [Bildmetadaten](image-metadata.md)
 

 




