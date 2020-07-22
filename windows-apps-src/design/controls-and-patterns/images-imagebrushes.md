---
Description: Erfahren Sie, wie Sie Bilder in Ihre App integrieren. Dazu gehört auch die Verwendung der APIs der beiden XAML-Hauptklassen, Image und ImageBrush.
title: Bilder und Bildpinsel
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 24c19e6746b7b8f346fcb2edd2056470ac909216
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493615"
---
# <a name="images-and-image-brushes"></a>Bilder und Bildpinsel

Sie können zum Anzeigen von Bildern das **Image**-Objekt oder das **ImageBrush**-Objekt verwenden. Ein Image-Objekt rendert ein Bild. Ein ImageBrush-Objekt zeichnet ein anderes Objekt mit einem Bild. 

> **Wichtige APIs:** [Image-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), [Source-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source), [ImageBrush-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush), [ImageSource-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)

## <a name="are-these-the-right-elements"></a>Sind dies die richtigen Elemente?
Verwenden Sie ein **Image**-Element, um ein eigenständiges Bild in Ihrer App anzuzeigen.

Verwenden Sie **ImageBrush**, um ein Image auf ein anderes Objekt anzuwenden. „ImageBrush“ kann u. a. für dekorative Effekte für Text oder Hintergründe für Steuerelemente oder Layoutcontainer verwendet werden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Falls die App <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> installiert ist, klicke <a href="xamlcontrolsgallery:/item/Image">hier</a>, um die App zu öffnen und das Bild in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>Erstellen eines Bilds

### <a name="image"></a>Abbild
In diesem Beispiel wird die Erstellung eines Bilds unter Verwendung des [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)-Objekts veranschaulicht.


```XAML
<Image Width="200" Source="sunset.jpg" />
```

Hier ist das gerenderte Image-Objekt.

![Beispiel für ein Image-Element](images/Image_Licorice.jpg)

In diesem Beispiel gibt die [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)-Eigenschaft den Speicherort des Bilds an, das du anzeigen möchtest. Zum Festlegen der Quelle kannst du eine absolute URL (z. B. http://contoso.com/myPicture.jpg) ) oder eine relative URL für deine App-Verpackungsstruktur angeben. In unserem Beispiel legen wir die Bilddatei „licorice.jpg“ im Stammverzeichnis unseres Projekts ab und deklarieren Projekteinstellungen, die die Bilddatei als Inhalt einbeziehen.

### <a name="imagebrush"></a>ImageBrush

Mit dem [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)-Objekt kannst du ein Bild verwenden, um einen Bereich zu zeichnen, der ein [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush)-Objekt akzeptiert. So kannst du beispielsweise ein ImageBrush-Objekt für den Wert der [Fill](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft einer [Ellipse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) oder für den Wert der [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background)-Eigenschaft einer [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) verwenden.

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

Wenn du den Wert für [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) oder [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) eines Objekts vom Typ **Image** nicht festlegst, wird es mit den von **Source** angegebenen Bilddimensionen angezeigt. Durch das Festlegen von **Width** und **Height** wird ein rechteckiger Bereich erstellt, in dem das Bild angezeigt wird. Mithilfe der [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.stretch)-Eigenschaft kannst du angeben, wie das Bild den enthaltenden Bereich ausfüllen soll. Die Stretch-Eigenschaft akzeptiert die folgenden Werte, die durch die [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch)-Enumeration definiert werden:

-   **None**: Das Bild wird nicht gestreckt, um den Ausgabebereich auszufüllen. Bei dieser Stretch-Einstellung ist Folgendes zu beachten: Ist das Quellbild größer als der enthaltende Bereich, wird das Bild abgeschnitten, was in der Regel nicht wünschenswert ist, da du anders als bei der bewussten Verwendung von [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) keine Kontrolle über den Anzeigebereich hast.
-   **Uniform**: Das Bild wird auf die Abmessungen der Ausgabe skaliert. Das Seitenverhältnis des Inhalts bleibt jedoch erhalten. Dies ist der Standardwert.
-   **UniformToFill**: Das Bild wird skaliert, sodass es den Ausgabebereich vollständig ausfüllt, und das ursprüngliche Seitenverhältnis wird beibehalten.
-   **Fill**: Das Bild wird auf die Abmessungen der Ausgabe skaliert. Da Höhe und Breite des Inhalts unabhängig voneinander dimensioniert werden, wird das ursprüngliche Seitenverhältnis möglicherweise nicht beibehalten. Mit anderen Worten, das Bild wird eventuell verzerrt, um den Ausgabebereich vollständig auszufüllen

![Ein Beispiel für Streckeinstellungen](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>Zuschneiden von Bildern

Mit der [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)-Eigenschaft kannst du einen Bereich der Bildausgabe beschneiden. Die Clip-Eigenschaft wird für eine [Geometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Geometry)-Klasse festgelegt. Das Beschneiden wird derzeit nur für Rechtecke unterstützt.

Im nächsten Beispiel erfährst du, wie du eine [RectangleGeometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry)-Klasse als Zuschneidebereich für ein Bild verwendest. In diesem Beispiel definieren wir ein **Image**-Objekt mit einer Höhe von 200. Eine **RectangleGeometry**-Klasse definiert ein Rechteck für den Bereich des Bilds, der angezeigt wird. Die [Rect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rectanglegeometry.rect)-Eigenschaft ist auf „25,25,100,150“ festgelegt. Dadurch wird ein Rechteck mit der Startposition „25,25“, der Breite „100“ und der Höhe „150“ definiert. Nur der Teil des Bilds, der sich innerhalb des Rechteckbereichs befindet, wird angezeigt.

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

Du kannst eine Deckkraft ([Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity)) auf ein Bild anwenden, sodass es halb durchscheinend gerendert wird. Die Transparenzwerte reichen von 0,0 bis 1,0, wobei 1,0 vollständig deckend und 0,0 vollständig durchsichtig bedeutet. Im Beispiel wird dargestellt, wie eine Transparenz von 0,5 auf ein Bild angewendet wird.

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

Die APIs für [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) und [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) enthalten keine dedizierten Methoden für das Codieren und Decodieren von Medienformaten. Sämtliche Codier- und Decodiervorgänge sind integriert. Aspekte dieser Vorgänge sind auf der Oberfläche allenfalls als Bestandteil von Ereignisdaten für Load-Ereignisse sichtbar. Falls du gezielt mit der Codierung und Decodierung von Bildern arbeiten möchtest, weil deine App beispielsweise Bildkonvertierungen oder Bildbearbeitungsfunktionen ausführt, musst du die im [Windows.Graphics.Imaging](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging)-Namespace verfügbaren APIs verwenden. Die APIs werden außerdem von der Windows-Bilderstellungskomponente (Windows Imaging Component, WIC) unterstützt.

Ab Windows 10, Version 1607, unterstützt das **Image**-Element animierte GIF-Bilder. Bei Verwendung eines **BitmapImage** als **Source** für das Bild können Sie auf BitmapImage-APIs zugreifen, um die Wiedergabe des animierten GIF-Bilds zu steuern. Weitere Informationen findest du in den Anmerkungen auf der Seite für die [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)-Klasse.

> **Hinweis:** &nbsp;&nbsp;Animierte GIFs werden unterstützt, wenn deine App für die Version 1607 von Windows 10 kompiliert wurde und mindestens unter der Version 1607 ausgeführt wird. Wenn Ihre App für frühere Versionen kompiliert wurde oder auf früheren Versionen ausgeführt wird, wird der erste Frame des GIF-Bilds angezeigt, ist jedoch nicht animiert.

Weitere Informationen zu App-Ressourcen und zum Packen von Bildquellen in einer App finden Sie unter [Definieren von App-Ressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965321(v=win.10)).

### <a name="writeablebitmap"></a>WriteableBitmap

[WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) stellt eine Bitmap-Quelle ([BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource)) bereit, die geändert werden kann und nicht die grundlegende dateibasierte Decodierung aus der WIC verwendet. Sie können Bilder dynamisch bearbeiten und das aktualisierte Bild erneut rendern. Verwende zum Definieren des Pufferinhalts eines **WriteableBitmap**-Elements die [PixelBuffer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer)-Eigenschaft, um auf den Puffer zuzugreifen, und einen Datenstrom oder sprachspezifischen Puffertyp, um ihn zu füllen. Beispielcode findest du unter [WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap).

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

Die [RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap)-Klasse kann die XAML-Benutzeroberflächenstruktur aus einer ausgeführten App erfassen und dann eine Bitmap-Bildquelle darstellen. Nach der Erfassung kann diese Bildquelle auf andere Teile der App angewendet, vom Benutzer als Ressourcen- oder App-Daten gespeichert oder für andere Szenarien verwendet werden. Ein besonders hilfreiches Szenario ist die Erstellung eines Laufzeitminiaturbilds einer XAML-Seite für ein Navigationsschema – beispielsweise zur Bereitstellung eines Bildlinks über ein [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)-Steuerelement. **RenderTargetBitmap** verfügt über einige Einschränkungen hinsichtlich des Inhalts, der in dem erfassten Bild angezeigt wird. Weitere Informationen findest du im API-Referenzthema für [RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap).

### <a name="image-sources-and-scaling"></a>Bildquellen und Skalierung

Erstellen Sie Ihre Bildquellen in mehreren empfohlenen Größen, damit die App immer gut aussieht, wenn Windows sie skaliert. Beim Angeben von **Source** für **Image** können Sie eine Benennungskonvention verwenden, die automatisch auf die richtige Ressource für die aktuelle Skalierung verweist. Einzelheiten zur Namenskonvention sowie weiterführende Informationen findest du unter [Schnellstart: Verwenden von Datei- oder Bildressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965325(v=win.10)).

Weitere Informationen zur Berücksichtigung der Skalierung in Ihrem App-Design finden Sie unter [UX-Richtlinien für Layout und Skalierung](https://developer.microsoft.com/windows/apps/design).

### <a name="image-and-imagebrush-in-code"></a>„Image“ und „ImageBrush“ im Code

In der Regel werden das Image- und das ImageBrush-Element mit XAML und nicht mit Code angegeben. Das liegt daran, dass diese Elemente häufig von Entwicklungstools als Teil einer XAML-UI-Definition ausgegeben werden.

Wenn du „Image“ oder „ImageBrush“ mithilfe von Code definierst, verwende die Standardkonstruktoren, und lege anschließend die relevante Quelleigenschaft ([Image.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) oder [ImageBrush.ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)) fest. Für die Quelleigenschaften ist ein [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)-Objekt (kein URI) erforderlich, wenn du sie mithilfe von Code festlegst. Falls es sich bei deiner Quelle um einen Datenstrom handelt, initialisiere den Wert mit der [SetSourceAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync)-Methode. Ist deine Quelle ein URI (wozu auch App-Inhalte gehören, die das Schema **ms-appx** oder **ms-resource** verwenden), verwende den [BitmapImage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage)-Konstruktor, der einen URI akzeptiert. Für den Fall, dass beim Abrufen oder Decodieren der Bildquelle Probleme mit der Zeitsteuerung auftreten und du alternativen Inhalt anzeigen musst, bis die Bildquelle verfügbar ist, empfiehlt es sich unter Umständen auch, das [ImageOpened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened)-Ereignis zu behandeln. Beispielcode finden Sie unter [XAML-Steuerelementekatalog](https://docs.microsoft.com/samples/microsoft/xaml-controls-gallery/xaml-controls-gallery/).

> [!NOTE]
> Wenn du Bilder mithilfe von Code einrichtest, kannst du die automatische Behandlung für den Zugriff auf nicht qualifizierte Ressourcen mit aktuellen Skalierungs- und Kulturqualifizierern verwenden. Alternativ kannst du [ResourceManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) und [ResourceMap](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap) mit Qualifizierern für Kultur und Skalierung verwenden, um die Ressourcen direkt abzurufen. Weitere Informationen finden Sie unter [Ressourcenverwaltungssystem](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

-   [Audio, Video und Kamera](https://docs.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Image-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)
-   [ImageBrush-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
