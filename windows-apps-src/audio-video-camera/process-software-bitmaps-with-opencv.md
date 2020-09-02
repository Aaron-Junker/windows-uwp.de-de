---
ms.assetid: ''
description: In diesem Artikel wird erläutert, wie Sie die softwarder Map-Klasse mit der Open Source-Maschinelles Sehen Bibliothek (opencv) verwenden.
title: Verarbeiten von Bitmaps mit OpenCV
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: a917c4efc8da8fbdabbdc753aacf23724ae17055
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363803"
---
# <a name="process-bitmaps-with-opencv"></a>Verarbeiten von Bitmaps mit OpenCV

In diesem Artikel wird die Verwendung der **[softwaremulmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** -Klasse erläutert, die von vielen unterschiedlichen Windows-Runtime-APIs zum Darstellen von Bildern verwendet wird, mit der Open Source-Maschinelles Sehen Bibliothek (opencv), einer Open Source-systemeigenen Code Bibliothek, die eine Vielzahl von Bildverarbeitungsalgorithmen bereitstellt. 

In den Beispielen in diesem Artikel werden die Schritte zum Erstellen eines systemeigenen Codes Windows-Runtime Komponente erläutert, die in einer UWP-App verwendet werden kann, einschließlich apps, die mit c# erstellt werden. Diese Hilfskomponente macht eine einzelne Methode (" **Blur**") verfügbar, die die Funktion zur weich Bildverarbeitung von opencv verwendet. Die Komponente implementiert private Methoden, die einen Zeiger auf den zugrunde liegenden Bild Datenpuffer erhalten, der direkt von der OpenCV-Bibliothek verwendet werden kann. so können Sie die Hilfskomponente einfach erweitern, um andere opencv-Verarbeitungsfunktionen zu implementieren. 

* Eine Einführung in die Verwendung von **softwaremulmap**finden Sie unter [erstellen, bearbeiten und Speichern von Bitmapbildern](imaging.md). 
* Informationen zur Verwendung der OpenCV-Bibliothek finden Sie unter [https://opencv.org](https://opencv.org) .
* Informationen zur Verwendung der in diesem Artikel gezeigten opencv Helper-Komponente mit **[mediaframereader](/uwp/api/windows.media.capture.frames.mediaframereader)** zum Implementieren der Bildverarbeitung von Frames von einer Kamera in Echtzeit finden Sie unter [Verwenden von opencv mit mediaframereader](use-opencv-with-mediaframereader.md).
* Ein umfassendes Codebeispiel, das verschiedene Effekte implementiert, finden Sie im GitHub-Repository für Windows Universal Samples im [Beispiel Kamera Frames und opencv](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) .

> [!NOTE] 
> Das in diesem Artikel beschriebene Verfahren, das von der opencvhelper-Komponente verwendet wird, erfordert, dass sich die zu verarbeitenden Image Daten im CPU-Speicher und nicht im GPU-Speicher befinden. Für APIs, die es Ihnen ermöglichen, den Speicherort von Bildern anzufordern, z. b. die **[mediacapture](/uwp/api/windows.media.capture.mediacapture)** -Klasse, sollten Sie also den CPU-Speicher angeben.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Erstellen einer hilfsWindows-Runtime Komponente für opencv Interop

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. Fügen Sie der Projekt Mappe einen neuen systemeigenen Code Windows-Runtime Komponenten Projekt hinzu.

1. Fügen Sie Ihrer Projekt Mappe in Visual Studio ein neues Projekt hinzu, indem Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Projekt Mappe und dann auf **Add->neues Projekt**klicken. 
2. Wählen Sie unter der Kategorie **Visual C++** die Option **Windows-Runtime Komponente (universelle Windows-Komponente)** aus. Benennen Sie in diesem Beispiel das Projekt "opencvbridge", und klicken Sie auf **OK**. 
3. Wählen Sie im Dialogfeld **Neues universelles Windows-Projekt** das Ziel und die minimale Betriebssystemversion für Ihre APP aus, und klicken Sie auf **OK**.
4. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf die automatisch gerendete Datei Class1. cpp, und wählen Sie **Entfernen**aus. wenn das Bestätigungs Dialogfeld angezeigt wird, wählen Sie **Löschen**. Löschen Sie dann die Header Datei Class1. h.
5. Klicken Sie mit der rechten Maustaste auf das Projektsymbol opencvbridge, und wählen Sie **Add->Class...** aus. Geben Sie im Dialogfeld **Klasse hinzufügen** im Feld **Klassen Name den Namen** "opencvhelper" ein, und klicken Sie dann auf **OK**. Code wird den erstellten Klassendateien in einem späteren Schritt hinzugefügt.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. Hinzufügen der opencv-nuget-Pakete zum Komponenten Projekt

1. Klicken Sie mit der rechten Maustaste auf das Projektsymbol opencvbridge in Projektmappen-Explorer und wählen Sie **nuget-Pakete verwalten... aus.**
2. Wenn das Dialogfeld nuget-Paket-Manager geöffnet wird, wählen Sie die Registerkarte **Durchsuchen** aus, und geben Sie im Suchfeld "opencv. Win" ein.
3. Wählen Sie "opencv. Win. Core" aus, und klicken Sie auf **Installieren**. Klicken Sie im Dialogfeld **Vorschau** auf **OK**.
4. Verwenden Sie das gleiche Verfahren, um das Paket "opencv. Win. imgproc" zu installieren.

>[!NOTE]
>Opencv. Win. Core und opencv. Win. imgproc werden nicht regelmäßig aktualisiert und bestehen nicht in den Geschäfts Kompatibilitäts Prüfungen. diese Pakete sind daher nur für Experimente gedacht.

### <a name="3-implement-the-opencvhelper-class"></a>3. Implementieren der opencvhelper-Klasse

Fügen Sie den folgenden Code in die Header Datei "opencvhelper. h" ein. Dieser Code enthält opencv-Header Dateien für das *Core* -und *imgproc* -Paket, das wir installiert haben, und deklariert drei Methoden, die in den folgenden Schritten angezeigt werden.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h" id="SnippetOpenCVHelperHeader":::

Löschen Sie den vorhandenen Inhalt der Datei "opencvhelper. cpp", und fügen Sie dann die folgenden include-Direktiven hinzu. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperInclude":::

Fügen Sie nach den Include-Direktiven die folgenden **using** -Direktiven hinzu. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperUsing":::

Fügen Sie als nächstes die **getpointertopixeldata** -Methode zu opencvhelper. cpp hinzu. Diese Methode nimmt eine **[softwardedumap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** an und ruft durch eine Reihe von Konvertierungen eine COM-Schnittstellen Darstellung der Pixeldaten ab, über die ein Zeiger auf den zugrunde liegenden Datenpuffer als **char** -Array abgerufen werden kann. 

Zuerst wird ein **[bitmapbuffer](/uwp/api/windows.graphics.imaging.bitmapbuffer)** mit den Pixeldaten durch Aufrufen von **[LockBuffer](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** abgerufen. dabei wird ein Lese-/Schreibpuffer angefordert, damit die Daten in der OpenCV-Bibliothek geändert werden können.  " **[Kreatereferenzierung](/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** " wird aufgerufen, um ein **[imemorybufferreference](/uwp/api/windows.foundation.imemorybufferreference)** -Objekt zu erhalten. Als nächstes wird die **imemorybufferbyteaccess** -Schnittstelle als **iinspectable**, als Basisschnittstelle für alle Windows-Runtime Klassen und **[QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** aufgerufen, um eine **[imemorybufferbyteaccess](/previous-versions/mt297505(v=vs.85))** -com-Schnittstelle zu erhalten, mit der wir den Pixeldaten Puffer als **char** -Array abrufen können. Füllen Sie schließlich das **char** -Array durch Aufrufen von **[imemorybufferbyteaccess:: GetBuffer](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)**. Wenn einer der Konvertierungs Schritte in dieser Methode fehlschlägt, gibt die Methode **false**zurück, um anzugeben, dass die weitere Verarbeitung nicht fortgesetzt werden kann.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperGetPointerToPixelData":::

Fügen Sie als nächstes die unten gezeigte Methode **TryConvert** hinzu. Diese Methode verwendet eine **softwardedumap** und versucht, Sie in ein **Matt** -Objekt zu konvertieren. Dies ist das Matrix Objekt, das von opencv zum Darstellen von Bild Daten Puffern verwendet wird. Diese Methode ruft die oben definierte **getpointertopixeldata** -Methode auf, um eine **char** -Array Darstellung des Pixeldaten Puffers zu erhalten. Wenn dies erfolgreich ist, wird der Konstruktor für die **Matt** -Klasse aufgerufen und übergibt dabei die Pixel Breite und-Höhe, die aus dem Quell- **softwarebitmap** -Objekt abgerufen werden. 

> [!NOTE] 
> In diesem Beispiel wird die CV_8UC4 Konstante als Pixel Format für das erstellte **Mat** -Objekt angegeben. Dies bedeutet, dass die an diese Methode über gegebene **Software Update** -Eigenschaft den **[BitmapPixelFormat](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** -Eigenschafts Wert  **[BGRA8](/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** mit einem prämultiplizierten Alpha aufweisen muss, der CV_8UC4 entspricht, um mit diesem Beispiel zu arbeiten.

Eine flache Kopie des erstellten **Mat** -Objekts wird von der-Methode zurückgegeben, sodass die weitere Verarbeitung auf denselben Datenpuffer des Daten Pixels angewendet wird, auf den von der **softwardedumap** verwiesen wird, und nicht auf eine Kopie dieses Puffers.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperTryConvert":::

Zum Schluss implementiert diese Beispiel-Hilfsklasse eine einzelne Bild Verarbeitungsmethode (weich), die einfach die oben definierte **TryConvert** -Methode verwendet, um ein **Mat** -Objekt **abzurufen, das**die Quell Bitmap und die Zielbitmap für den weichzeichenvorgang darstellt, und dann die " **Blur** "-Methode aus der opencv imgproc-Bibliothek aufruft. Der andere Parameter für " **Blur** " gibt die Größe des Weichzeichnereffekts in X-und Y-Richtung an.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperBlur":::


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Ein einfaches Beispiel für ein opencv Map-Beispiel mit der Hilfskomponente
Nachdem die opencvbridge-Komponente erstellt wurde, können wir eine einfache c#-app erstellen, die die **opencv-** Methode zum Ändern einer **Software Update**-Methode verwendet. Wenn Sie über Ihre UWP-App auf die Windows-Runtime Komponente zugreifen möchten, müssen Sie zunächst einen Verweis auf die Komponente hinzufügen. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste unter dem UWP-App-Projekt auf den Knoten **Verweise** , und wählen Sie **Verweis hinzufügen...** aus. Wählen Sie im Dialogfeld Verweis-Manager die Option **Projekte->Projekt**Mappe aus. Aktivieren Sie das Kontrollkästchen neben dem Projekt opencvbridge, und klicken Sie auf **OK**.

Der folgende Beispielcode ermöglicht dem Benutzer die Auswahl einer Bilddatei und die anschließende Verwendung von **[BitmapDecoder](/uwp/api/windows.graphics.imaging.bitmapencoder)** zum Erstellen **der Darstellung des** Bilds. Weitere Informationen zum Arbeiten mit **softwaremulmap**finden Sie unter [erstellen, bearbeiten und Speichern von Bitmapbildern](./imaging.md).

Wie bereits weiter oben in diesem Artikel erläutert, erfordert die **opencvhelper** -Klasse, dass alle bereitgestellten **softwaremulmap** -Bilder mit einem prämultiplizierten Alpha Wert im BGRA8-Pixel Format codiert werden. wenn das Bild nicht bereits in diesem Format vorliegt, ruft der Beispielcode **[Convert](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** auf, um das Bild in das erwartete Format zu konvertieren.

Als nächstes wird eine **softwarframemap** erstellt, die als Ziel für den weichzeichenvorgang verwendet wird. Die Eingabebild Eigenschaften werden als Argumente für den Konstruktor verwendet, um eine Bitmap mit überein stimmendem Format zu erstellen.

Es wird eine neue Instanz von **opencvhelper** erstellt, und die " **Blur** "-Methode wird aufgerufen, wobei die Quell-und Ziel Bitmaps übergeben werden. Schließlich wird eine **softwarebitmapsource** erstellt, um das Ausgabe Bild einem XAML- **Bild** Steuerelement zuzuweisen.

Dieser Beispielcode verwendet APIs aus den folgenden Namespaces, zusätzlich zu den Namespaces, die in der Standard Projektvorlage enthalten sind.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVMainPageUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVBlur":::

## <a name="related-topics"></a>Verwandte Themen

* [Referenz zu BitmapEncoder-Optionen](bitmapencoder-options-reference.md)
* [Bild Metadaten](image-metadata.md)
 

 
