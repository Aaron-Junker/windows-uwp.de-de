---
Description: Erfahren Sie, wie Sie Bilder in Ihre App integrieren. Dazu gehört auch die Verwendung der APIs der beiden XAML-Hauptklassen, Image und ImageBrush.
title: Bilder und Bildpinsel
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5451e0dd94510873f87b981a817fb55ba2c4f985
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622015"
---
# <a name="images-and-image-brushes"></a>Bilder und Bildpinsel

Sie können zum Anzeigen von Bildern das **Image**-Objekt oder das **ImageBrush**-Objekt verwenden. Ein Image-Objekt rendert ein Bild. Ein ImageBrush-Objekt zeichnet ein anderes Objekt mit einem Bild. 

> **Wichtige APIs:** [Image-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [Datenquelleneigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx), [ImageBrush-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx), [ImageSource-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)

## <a name="are-these-the-right-elements"></a>Sind dies die richtigen Elemente?
Verwenden Sie ein **Image**-Element, um ein eigenständiges Bild in Ihrer App anzuzeigen.

Verwenden Sie **ImageBrush**, um ein Image auf ein anderes Objekt anzuwenden. „ImageBrush“ kann u. a. für dekorative Effekte für Text oder Hintergründe für Steuerelemente oder Layoutcontainer verwendet werden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/Image">die App zu öffnen und Image in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>Erstellen eines Bilds

### <a name="image"></a>Bild
Dieses Beispiel zeigt, wie mit dem [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)-Objekt ein Bild erstellt wird.


```XAML
<Image Width="200" Source="sunset.jpg" />
```

Hier ist das gerenderte Image-Objekt.

![Beispiel für ein Image-Element](images/Image_Licorice.jpg)

Die [Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx)-Eigenschaft in diesem Beispiel gibt den Speicherort des Bildes an, das Sie anzeigen möchten. Legen Sie die Quelle, indem Sie eine absolute URL angeben (z. B. http://contoso.com/myPicture.jpg) oder durch Angabe einen URL, die relativ zu Ihrer app-Paket-Struktur ist. In unserem Beispiel legen wir die Bilddatei „licorice.jpg“ im Stammverzeichnis unseres Projekts ab und deklarieren Projekteinstellungen, die die Bilddatei als Inhalt einbeziehen.

### <a name="imagebrush"></a>ImageBrush

Mit dem [ImageBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)-Objekt können Sie ein Bild dazu verwenden, einen Bereich zu zeichnen, der ein [Brush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx)-Objekt annimmt. So können Sie ein ImageBrush-Objekt für den Wert der [Fill](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx)-Eigenschaft einer [Ellipse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.ellipse.aspx)-Klasse oder die [Background](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx)-Eigenschaft einer [Canvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)-Klasse verwenden.

Im nächsten Beispiel ist dargestellt, wie „ImageBrush“ zum Zeichnen eines Ellipse verwendet wird.

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Hier ist die Ellipse, die von „ImageBrush“ gezeichnet wurde.

![Eine Ellipse, die mit „ImageBrush“ gezeichnet wurde](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>Strecken von Bildern

Wenn Sie den [Width](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx)-Wert oder [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx)-Wert eines **Image**-Objekts nicht festlegen, wird es mit den von **Source** angegebenen Abmessungen des Bilds angezeigt. Durch das Festlegen von **Width** und **Height** wird ein rechteckiger Bereich erstellt, in dem das Bild angezeigt wird. Mit der [Stretch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.stretch.aspx)-Eigenschaft können Sie festlegen, wie das Bild den Bereich ausfüllt. Die Stretch-Eigenschaft übernimmt die Werte, die durch die [Stretch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx)-Enumeration definiert werden:

-   **Keine**: Die Grafik nicht gestreckt, um die Ausgabe Abmessungen füllen. Beachten Sie bei dieser Stretch-Einstellung Folgendes: Wenn das Quellbild größer als der enthaltende Bereich ist, wird das Bild abgeschnitten. Dies sollte hier normalerweise jedoch vermieden werden, da nicht gesteuert werden kann, welcher Ausschnitt angezeigt wird, wie dies bei einer absichtlichen [Clip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx)-Anwendung der Fall ist.
-   **Einheitliche**: Das Bild wird skaliert, entsprechend den Ausgabe. Das Seitenverhältnis des Inhalts bleibt jedoch erhalten. Dies ist der Standardwert.
-   **UniformToFill**: Das Bild wird skaliert, sodass er vollständig den Ausgabebereich ausfüllt, jedoch die ursprünglichen Seitenverhältnisse beizubehalten behält.
-   **Geben Sie**: Das Bild wird skaliert, entsprechend den Ausgabe. Da Höhe und Breite des Inhalts unabhängig voneinander dimensioniert werden, wird das ursprüngliche Seitenverhältnis möglicherweise nicht beibehalten. Mit anderen Worten, das Bild wird eventuell verzerrt, um den Ausgabebereich vollständig auszufüllen

![Ein Beispiel für Streckeinstellungen](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>Zuschneiden von Bildern

Mit der [Clip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx)-Eigenschaft können Sie einen Bildausgabebereich beschneiden. Die Clip-Eigenschaft wird für eine [Geometry](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.geometry.aspx)-Klasse festgelegt. Das Beschneiden wird derzeit nur für Rechtecke unterstützt.

Im nächsten Beispiel erfahren Sie, wie Sie eine [RectangleGeometry](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.aspx)-Klasse als Zuschneidebereich für ein Bild verwenden. In diesem Beispiel definieren wir ein **Image**-Objekt mit einer Höhe von 200. Eine **RectangleGeometry**-Klasse definiert ein Rechteck für den Bereich des Bilds, der angezeigt wird. Die [Rect](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.rect.aspx)-Eigenschaft ist auf „25,25,100,150“ festgelegt, wodurch ein Rechteck definiert ist, das bei Position 25,25 mit einer Breite von 100 und einer Höhe von 150 startet. Nur der Teil des Bilds, der sich innerhalb des Rechteckbereichs befindet, wird angezeigt.

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

Hier sehen Sie das zugeschnittene Bild auf einem schwarzen Hintergrund.

![Ein Image-Objekt, das mit einer RectangleGeometry-Klasse zugeschnitten wurde](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>Anwenden von Transparenz

Sie können [Opacity](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx) auf ein Bild anwenden, sodass das Bild teilweise durchsichtig gerendert wird. Die Transparenzwerte reichen von 0,0 bis 1,0, wobei 1,0 vollständig deckend und 0,0 vollständig durchsichtig bedeutet. Im Beispiel wird dargestellt, wie eine Transparenz von 0,5 auf ein Bild angewendet wird.

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
```

Dies ist das gerenderte Bild mit einer Transparenz von 0,5 und einem schwarzen Hintergrund, der durch das teilweise durchlässige Bild zu sehen ist.

![Ein Image-Objekt mit einer Transparenz von 0,5.](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>Bilddateiformate

**Image** und **ImageBrush** können die folgenden Bilddateiformate anzeigen:

-   JPEG (Joint Photographic Experts Group)
-   PNG (Portable Network Graphics)
-   BMP (Bitmap)
-   GIF (Graphics Interchange Format)
-   TIFF (Tagged Image File Format)
-   JPEG XR
-   ICO (Symbole)

Die APIs für [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) und [BitmapSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) enthalten keine dedizierten Methoden für die Kodierung und Dekodierung von Medienformaten. Sämtliche Codier- und Decodiervorgänge sind integriert. Aspekte dieser Vorgänge sind auf der Oberfläche allenfalls als Bestandteil von Ereignisdaten für Load-Ereignisse sichtbar. Falls Sie gezielt mit der Codierung und Decodierung von Bildern arbeiten möchten, weil Ihre App beispielsweise Bildkonvertierungen oder Bildbearbeitungsfunktionen ausführt, sollten Sie die im [Windows.Graphics.Imaging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.graphics.imaging.aspx)-Namespace verfügbaren APIs verwenden. Die APIs werden außerdem von der Windows-Bilderstellungskomponente (Windows Imaging Component, WIC) unterstützt.

Ab Windows 10, Version 1607, unterstützt das **Image**-Element animierte GIF-Bilder. Bei Verwendung eines **BitmapImage** als **Source** für das Bild können Sie auf BitmapImage-APIs zugreifen, um die Wiedergabe des animierten GIF-Bilds zu steuern. Weitere Informationen finden Sie in den Anmerkungen auf der Seite für die [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx)-Klasse.

> **Hinweis**&nbsp;&nbsp;Die Unterstützung für animierte GIFs ist verfügbar, wenn Ihre App für Windows 10, Version 1607, kompiliert wurde und auf Version 1607 (oder höher) ausgeführt wird. Wenn Ihre App für frühere Versionen kompiliert wurde oder auf früheren Versionen ausgeführt wird, wird der erste Frame des GIF-Bilds angezeigt, ist jedoch nicht animiert.

Weitere Informationen zu App-Ressourcen und zum Packen von Bildquellen in einer App finden Sie unter [Definieren von App-Ressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).

### <a name="writeablebitmap"></a>WriteableBitmap

[WriteableBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx) stellt eine [BitmapSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) bereit, die geändert werden kann und nicht die grundlegende dateibasierte Dekorierung der WIC verwendet. Sie können Bilder dynamisch bearbeiten und das aktualisierte Bild erneut rendern. Verwenden Sie zum Definieren des Pufferinhalts eines **WriteableBitmap**-Elements die [PixelBuffer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer.aspx)-Eigenschaft, um auf den Puffer zuzugreifen, und einen Datenstrom oder sprachspezifischen Puffertyp, um ihn zu füllen. Beispielcode finden Sie unter [WriteableBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx).

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

Die [RenderTargetBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx)-Klasse kann die XAML-Benutzeroberflächenstruktur einer aktiven App erfassen und erstellt anschließend eine Bitmapbildquelle. Nach der Erfassung kann diese Bildquelle auf andere Teile der App angewendet, vom Benutzer als Ressourcen- oder App-Daten gespeichert oder für andere Szenarien verwendet werden. Ein besonders hilfreiches Szenario ist die Erstellung eines Laufzeitminiaturbilds einer XAML-Seite für ein Navigationsschema. Dies kann beispielsweise die Bereitstellung eines Bildlinks über ein [Hub](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hub.aspx)-Steuerelement sein. **RenderTargetBitmap** verfügt über einige Einschränkungen hinsichtlich des Inhalts, der in dem erfassten Bild angezeigt wird. Weitere Informationen finden Sie im API-Referenzthema für [RenderTargetBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx).

### <a name="image-sources-and-scaling"></a>Bildquellen und Skalierung

Erstellen Sie Ihre Bildquellen in mehreren empfohlenen Größen, damit die App immer gut aussieht, wenn Windows sie skaliert. Beim Angeben von **Source** für **Image** können Sie eine Benennungskonvention verwenden, die automatisch auf die richtige Ressource für die aktuelle Skalierung verweist. Weitere Informationen die Benennungskonvention und Weitere Informationen finden Sie unter [Schnellstart: Verwenden von Datei-oder Bildressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325).

Weitere Informationen zur Berücksichtigung der Skalierung in Ihrem App-Design finden Sie unter [UX-Richtlinien für Layout und Skalierung](https://msdn.microsoft.com/library/windows/apps/dn611863).

### <a name="image-and-imagebrush-in-code"></a>„Image“ und „ImageBrush“ im Code

In der Regel werden das Image- und das ImageBrush-Element mit XAML und nicht mit Code angegeben. Das liegt daran, dass diese Elemente häufig von Entwicklungstools als Teil einer XAML-UI-Definition ausgegeben werden.

Wenn Sie „Image“ oder „ImageBrush“ im Code definieren, sollten Sie die Standardkonstruktoren verwenden und anschließend die relevanten Source-Eigenschaften ([Image.Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) oder [ImageBrush.ImageSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)) festlegen. Die Source-Eigenschaften erfordern ein [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx)-Objekt (keinen URI), wenn Sie diese mithilfe von Code festlegen. Falls es sich bei Ihrer Source um einen Stream handelt, initialisieren Sie den Wert mit der [SetSourceAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync.aspx)-Methode. Ist Ihre Quelle ein (URI), der Ihrer App Inhalt mit dem **ms-appx**- oder dem **ms-resource**-Schema hinzufügt, verwenden Sie den [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/br243238.aspx)-Konstruktor, für den ein URI angegeben wird. Wenn beim Abrufen oder Decodieren der Bildquelle Probleme mit der Zeitsteuerung auftreten und Sie alternativen Inhalt anzeigen müssen, bis die Bildquelle verfügbar ist, können Sie auch das [ImageOpened](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx)-Ereignis behandeln. Beispielcode finden Sie unter [Beispiel für XAML-Bilder](https://go.microsoft.com/fwlink/p/?linkid=238575).

> [!NOTE]
> Wenn Sie Bilder mithilfe von Code festlegen, können Sie die automatische Behandlung für den Zugriff auf nicht qualifizierte Ressourcen mit den aktuellen Skalierungs- und Kulturqualifizierern verwenden. Alternativ können Sie auch [ResourceManager](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemanager.aspx) und [ResourceMap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemap.aspx) mit Qualifizierern für Kultur und Skalierung verwenden, um die Ressourcen direkt abzurufen. Weitere Informationen finden Sie unter [Ressourcenverwaltungssystem](https://msdn.microsoft.com/library/windows/apps/xaml/jj552947.aspx).

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

-   [Audio, Video und Kamera](https://msdn.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Image-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)
-   [ImageBrush-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)