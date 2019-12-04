---
Description: Verwenden Sie das ParallaxView-Steuerelement, um Tiefe und Bewegung zu Ihrer App hinzuzufügen.
title: Verwenden Sie "Parser", um der APP Tiefe und Bewegung hinzuzufügen.
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
ms.openlocfilehash: ab37c49115acbccaf81725b74def85891fd73219
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735065"
---
# <a name="parallax"></a>Parallax

Parallax ist ein visueller Effekt, bei dem sich Elemente, die näher am Betrachter liegen, schneller als Elemente im Hintergrund bewegen. Parallax erzeugt ein Gefühl von Tiefe, Perspektive und Bewegung. In einer UWP-App können Sie das ParallaxView-Steuerelement verwenden, um einen Parallax-Effekt zu erzeugen.  

> **Wichtige APIs**: [ParallaxView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [VerticalShift-Eigenschaft](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [HorizontalShift-Eigenschaft](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ParallaxView">die App zu öffnen und ParallaxView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>Parallax und das Fluent Design-System

 Mit dem Fluent Design-System können Sie moderne, klare Benutzeroberflächen erstellen, die Licht, Tiefe, Bewegung, Material und Skalierung enthalten. Parallax ist eine Komponente des Fluent Design-Systems, die Bewegung, Tiefe und Skalierungsmöglichkeiten in Ihre App bringt. Weitere Informationen finden Sie in der [Übersicht über Fluent Design für UWP](/windows/apps/fluent-design-system).

## <a name="how-it-works-in-a-user-interface"></a>Funktionsweise in einer Benutzeroberfläche

In einer Benutzeroberfläche lassen sich Parallax-Effekte erzeugen, indem bei einem Bildlauf oder Schwenken der Benutzeroberfläche verschiedene Objekte unterschiedlich schnell bewegt werden. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Um dies zu veranschaulichen, sehen wir uns zwei Inhaltsebenen an, eine Liste und ein Hintergrundbild.  Die Liste befindet sich über dem Hintergrundbild. Dadurch wird bereits der Eindruck vermittelt, dass die Liste sich näher am Betrachter befindet.  Um jetzt den Parallax-Effekt zu erzielen, soll sich das Objekt, welches uns am nächsten liegt, „schneller” bewegen als das Objekt, welches weiter entfernt ist.  Wenn der Benutzer einen Bildlauf in der Oberfläche durchführt, bewegt sich die Liste schneller als das Hintergrundbild, wodurch eine Illusion von Tiefe erzeugt wird.

 ![Beispiel für Parallaxe mit einer Liste und einem Hintergrundbild](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Verwenden des ParallaxView-Steuerelements zum Erzeugen eines Parallax-Effekts

Um einen Parallax-Effekt zu erzeugen, verwenden Sie das [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview)-Steuerelement. Dieses Steuerelement verbindet die Bildlaufposition eines Vordergrundelements, z. B. einer Liste, mit einem Hintergrundelement, z. B. einem Bild. Bei einem Bildlauf durch das Vordergrundelement wird das Hintergrundelement animiert, um einen Parallax-Effekt zu erzeugen. 

Um das ParallaxView-Steuerelement zu verwenden, stellen Sie ein Quellelement und ein Hintergrundelement bereit und legen für die Eigenschaften [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (für den vertikalen Bildlauf) und/oder [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (für den horizontalen Bildlauf) einen größeren Wert als Null fest. 
* Die Quelleigenschaft akzeptiert einen Verweis auf das Vordergrundelement. Damit der Parallax-Effekt auftritt, sollte sich im Vordergrund ein [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)- oder ein anderes Element befinden, das ein ScrollViewer-Element enthält, z. B. [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) oder [RichTextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). 

* Um das Hintergrundelement festzulegen, fügen Sie dieses Element als untergeordnetes Element zum ParallaxView-Steuerelement hinzu. Das Hintergrundelement kann ein beliebiges [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) sein, z. B. ein [Bild](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) oder ein Panel, das zusätzliche UI-Elemente enthält. 

Um einen Parallax-Effekt zu erstellen, muss sich ParallaxView hinter dem Vordergrundelement befinden. Mithilfe des [Raster](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid)- und des [Canvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)-Panels können Sie Elemente übereinanderstapeln, damit sie sich gut mit dem ParallaxView-Steuerelement verwenden lassen.  

In diesem Beispiel wird ein Parallax-Effekt für eine Liste erstellt:
 
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

ParallaxView passt die Größe des Bilds automatisch an den entsprechenden Parallax-Vorgang an. Sie müssen sich also keine Sorgen machen, dass das Bild beim Bildlauf aus dem Sichtfeld verschwindet.

## <a name="customizing-the-parallax-effect"></a>Anpassen des Parallax-Effekts 

Mithilfe der Eigenschaften VerticalShift und HorizontalShift können Sie das Ausmaß des Parallax-Effekts steuern.

* Die VerticalShift-Eigenschaft gibt an, wie weit der Hintergrund während des gesamten Parallax-Vorgangs vertikal verschoben werden soll. Der Wert 0 bedeutet, dass der Hintergrund überhaupt nicht verschoben wird.
* Die HorizontalShift-Eigenschaft gibt an, wie weit der Hintergrund während des gesamten Parallax-Vorgangs horizontal verschoben werden soll. Der Wert 0 bedeutet, dass der Hintergrund überhaupt nicht verschoben wird.

Mit höheren Werten lassen sich dramatischere Effekte erzeugen. 

Die vollständige Liste der Methoden zum Anpassen von Parallax finden Sie in der ParallaxView-Klasse. 

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwenden Sie Parallax in Listen mit einem Hintergrundbild
- Verwenden Sie Parallax gegebenenfalls in ListViewItems, wenn ListViewItems ein Bild enthalten.
- Verwenden Sie es nicht überall. Eine übermäßige Verwendung kann den Effekt verringern.

## <a name="related-articles"></a>Verwandte Artikel

- [Parametanview-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Fluent Design für UWP](/windows/apps/fluent-design-system)
- [Wissenschaft im System: fließende Design und Tiefe](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
