---
Description: Verwenden Sie das Steuerelement "Parser", um der APP Tiefe und Bewegung hinzuzufügen.
title: Verwenden Sie Parallax, um der App Tiefe und Bewegung hinzuzufügen.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d659683d6871d9d48fd17b73c74477e7bd03e258
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970495"
---
# <a name="parallax"></a>Parallax

"Parser" ist ein visueller Effekt, bei dem Elemente, die näher am Viewer liegen, schneller verschoben werden als Elemente im Hintergrund. "Parser" erstellt ein Gefühl für tiefe, Perspektive und Bewegung. In einer UWP-App können Sie das-Steuerelement "Parser" verwenden, um einen-Effekt zu erzeugen.  

> **APIs für die Windows-Benutzeroberflächen Bibliothek:** parameterxview- [Klasse](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview), [verticalshift-Eigenschaft](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift), [horizontalshift-Eigenschaft](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **Plattform-APIs**: parametal [View-Klasse](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [verticalshift-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [horizontalshift-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die Katalog-App für <strong style="font-weight: semi-bold">XAML</strong> -Steuerelemente installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ParallaxView">die APP zu öffnen, und sehen Sie sich die in Aktion enthaltene Ansicht an</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>"Parser" und "flüssiges Design System"

 Mit dem Fluent Design-System können Sie moderne, klare Benutzeroberflächen erstellen, die Licht, Tiefe, Bewegung, Material und Skalierung enthalten. "Parser" ist eine fließende Entwurfs System Komponente, die Ihre APP um Bewegung, Tiefe und Skalierung erweitert. Weitere Informationen finden Sie in der [Übersicht über das fließende Design](/windows/apps/fluent-design-system).

## <a name="how-it-works-in-a-user-interface"></a>Funktionsweise in einer Benutzeroberfläche

In einer Benutzeroberfläche können Sie einen parameteffekt erstellen, indem Sie unterschiedliche Objekte mit unterschiedlichen Raten verschieben, wenn die Benutzeroberfläche einen Bildlauf durchführt. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Um dies zu veranschaulichen, betrachten wir zwei Inhalts Ebenen, eine Liste und ein Hintergrundbild.  Die Liste wird auf dem Hintergrundbild platziert. Dadurch wird bereits die Illusion angezeigt, dass die Liste näher am Viewer liegt.  Um den Teil der Wirkung zu erzielen, soll das Objekt, das uns am nächsten geht, "schneller" als das Objekt, das sich weiter entfernt.  Wenn der Benutzer einen Bildlauf zur Schnittstelle durchführt, verschiebt sich die Liste schneller als das Hintergrundbild, was die Illusion von Tiefe erzeugt.

 ![Beispiel für Parallaxe mit einer Liste und einem Hintergrundbild](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Verwenden des Steuer Elements "parametenxview" zum Erstellen eines-Parametern

Zum Erstellen eines-Parametern verwenden Sie das-Steuerelement " [Parser](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) ". Dieses Steuerelement bindet die Scrollposition eines Vordergrund Elements, z. b. eine Liste, an ein Hintergrund Element, z. b. ein Bild. Wenn Sie einen Bildlauf durch das Vordergrund Element durchführen, animiert es das background-Element, um einen-Effekt zu erzeugen. 

Wenn Sie das Steuerelement "-Steuerelement" verwenden möchten, geben Sie ein Quell Element, ein Background-Element und die [verticalshift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) -Eigenschaften (für das vertikale Scrollen) und/oder [horizontalshift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (für horizontales Scrollen) auf einen Wert größer als 0 (null) an. 
* Die Source-Eigenschaft nimmt einen Verweis auf das Vordergrund Element an. Damit der Teil des Parametern auftritt, sollte der Vordergrund ein [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) oder ein Element sein, das einen ScrollViewer enthält, z. b. ein [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) -oder [RichTextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)-Element. 

* Um das background-Element festzulegen, fügen Sie dieses Element als untergeordnetes Element des-Steuer Elements "Parser" hinzu. Beim Background-Element kann es sich um ein beliebiges [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)handeln, z. b. ein [Bild](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) oder ein Panel, das zusätzliche Benutzeroberflächen Elemente 

Um einen Teil des Effekts zu erstellen, muss sich die "Parser"-Sicht hinter dem Vordergrund Element befinden. Mit dem [Raster](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) -und dem [Canvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas) -Panel können Sie Elemente auf der obersten Ebene anlagern, sodass Sie gut mit dem Steuerelement "parameallaxview" funktionieren.  

In diesem Beispiel wird ein Teil des Parametern für eine Liste erstellt:
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>     
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
```    

Die "Parser"-Sicht passt automatisch die Größe des Bilds an, sodass es für den unter-und-Vorgang funktioniert, sodass Sie sich keine Gedanken machen müssen, dass das Bild nicht mehr in der Ansicht angezeigt wird.

## <a name="customizing-the-parallax-effect"></a>Anpassen des Parametern 

Mit den Eigenschaften verticalshift und horizontalshift können Sie den Grad des-Parametern steuern.

* Die verticalshift-Eigenschaft gibt an, wie weit der Hintergrund während des gesamten Parametern vertikal verschoben werden soll. Der Wert 0 bedeutet, dass der Hintergrund überhaupt nicht verschoben wird.
* Die horizontalshift-Eigenschaft gibt an, wie weit der Hintergrund während des gesamten Parametern horizontal verschoben werden soll. Der Wert 0 bedeutet, dass der Hintergrund überhaupt nicht verschoben wird.

Größere Werte führen zu einer größeren Auswirkung. 

Eine umfassende Liste der Möglichkeiten zum Anpassen von "Parser" finden Sie in der Klasse "paramexview". 

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwenden von "Parser" in Listen mit einem Hintergrundbild
- Verwenden Sie ggf. die Verwendung von "Parser" in ListViewItems, wenn ListViewItems ein Bild enthalten.
- Nicht überall verwenden, die über Auslastung kann seine Auswirkung verringern

## <a name="related-articles"></a>Verwandte Artikel

- [Parametanview-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Fluent Design für UWP](/windows/apps/fluent-design-system)
- [Wissenschaft im System: fließende Design und Tiefe](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
