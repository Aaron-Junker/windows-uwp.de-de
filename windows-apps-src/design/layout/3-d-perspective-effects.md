---
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: 3D-Perspektiveneffekte für XAML-UI
description: Mithilfe der perspektivischen Transformation können Sie 3D-Effekte auf Inhalte in Ihren Windows-Runtime-Apps anwenden. Sie können z. B. wie hier gezeigt die Illusion schaffen, dass sich ein Objekt auf Sie zu oder von Ihnen wegbewegt.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8db56882833b9d3bd8a6d2932d04e07a72b205e2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365251"
---
# <a name="3-d-perspective-effects-for-xaml-ui"></a>3D-Perspektiveneffekte für XAML-UI


Mithilfe der perspektivischen Transformation können Sie 3D-Effekte auf Inhalte in Ihren Windows-Runtime-Apps anwenden. Sie können z. B. wie hier gezeigt die Illusion schaffen, dass sich ein Objekt auf Sie zu oder von Ihnen wegbewegt.

![Bild mit perspektivischer Transformation](images/3dsimple.png)

Eine weitere häufige Anwendung von perspektivischen Transformationen besteht in der Anordnung von Objekten relativ zueinander, wodurch wie hier ein 3D-Effekt erzeugt wird.

![Stapeln von Objekten zum Erzeugen eines 3D-Effekts](images/3dstacking.png)

Neben dem Erzeugen von statischen 3D-Effekten können Sie die Eigenschaften der perspektivischen Transformation animieren, um 3D-Bewegungseffekte zu erzielen.

Sie haben sich soeben damit vertraut gemacht, wie perspektivische Transformationen auf Bilder angewendet werden. Sie können diese Effekte jedoch auf jedes [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) anwenden, einschließlich von Steuerelementen. Sie können beispielsweise wie folgt einen 3D-Effekt auf einen ganzen Container von Steuerelementen anwenden:

![Auf einen Container von Elementen angewendeter 3D-Effekt](images/skewedstackpanel.png)

Dieses Beispiel wurde mit folgendem XAML-Code erstellt:

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

Wenden Sie sich nun den Eigenschaften von [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) zu. Damit werden Objekte im 3D-Raum gedreht und bewegt. Im folgenden Beispiel können Sie mit den Eigenschaften experimentieren und feststellen, wie sie sich auf ein Objekt auswirken.

## <a name="planeprojection-class"></a>PlaneProjection-Klasse

Sie können 3D-Effekte auf jedes [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) anwenden. Dazu legen Sie die [**Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection)-Eigenschaft für das UIElement mit einer [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) fest. Die **PlaneProjection** definiert, wie die Transformation im Raum gerendert wird. Im nächsten Beispiel wird ein einfacher Fall veranschaulicht.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

Die Abbildung zeigt, wie das Bild gerendert wird. Die X-Achse, die Y-Achse und die Z-Achse sind als rote Linien dargestellt. Das Bild wird mit der [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx)-Eigenschaft um 35 Grad um die X-Achse rückwärts gedreht.

![RotateX minus 35 Grad](images/3drotatexminus35.png)

Die [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy)-Eigenschaft führt eine Drehung um die Y-Achse des Drehmittelpunkts aus.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![RotateY minus 35 Grad](images/3drotateyminus35.png)

Die [**RotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationz)-Eigenschaft führt eine Drehung um die Z-Achse des Drehmittelpunkts aus (eine Linie, die eine Senkrechte zur Objektfläche darstellt).

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![RotateZ minus 45 Grad](images/3drotatezminus35.png)

Für die Drehungseigenschaften kann ein positiver oder negativer Wert für die Drehung in beiden Richtungen angegeben werden. Die absolute Zahl kann größer als 360 sein, wodurch das Objekt um mehr als eine volle Drehung gedreht wird.

Sie können den Drehmittelpunkt mithilfe der Eigenschaften [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx), [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) und [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz) verschieben. Standardmäßig verläuft die Drehachse direkt durch den Objektmittelpunkt, wodurch das Objekt um seinen Mittelpunkt gedreht wird. Wenn Sie jedoch den Drehmittelpunkt an den Rand des Objekts verschieben, wird das Objekt um den betreffenden Rand gedreht. Der Standardwert für **CenterOfRotationX** und **CenterOfRotationY** ist 0,5, und der Standardwert für **CenterOfRotationZ** ist 0. Für **CenterOfRotationX** und **CenterOfRotationY** wird der Drehpunkt mit Werten zwischen 0 und 1 auf eine bestimmte Position auf dem Objekt festgelegt. Durch den Wert "0" wird ein Rand des Objekts angegeben, während mit dem Wert "1" der gegenüberliegende Rand angegeben wird. Werte außerhalb dieses Bereichs sind zulässig, und der Drehmittelpunkt wird entsprechend verschoben. Da die Z-Achse des Drehmittelpunkts durch die Fläche des Objekts gezeichnet wird, können Sie den Drehmittelpunkt mit einer negativen Zahl hinter das Objekt und mit einer positiven Zahl vor das Objekt (auf den Betrachter zu) verschieben.

[**CenterOfRotationX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx) verschiebt den Mittelpunkt der Drehung entlang der x-Achse gleichzeitig auf das Objekt beim [ **CenterOfRotationY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) verschiebt das Center oder Drehung entlang der y-Achse das Objekt. In der folgenden Abbildung wird die Auswirkung von verschiedenen Werten für **CenterOfRotationY** veranschaulicht.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.5" (default)**

![CenterOfRotationY ist "0,5"](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.1"**

![CenterOfRotationY ist "0,1"](images/3dcenterofrotationy0point1.png)

Beachten Sie, wie das Bild um den Mittelpunkt gedreht wird, wenn die [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy)-Eigenschaft auf den Standardwert "0,5" festgelegt ist. Es wird nahe seinem oberen Rand gedreht, wenn die Eigenschaft auf "0,1" festgelegt ist. Ein ähnliches Verhalten ist zu beobachten, wenn die [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx)-Eigenschaft geändert wird, um eine Verschiebung zu bewirken, wobei das Objekt durch die [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy)-Eigenschaft gedreht wird.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.5" (default)**

![CenterOfRotationX ist "0,5"](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.9" (rechten Rand)**

![CenterOfRotationX ist "0,9"](images/3dcenterofrotationx0point9.png)

Platzieren Sie den Drehmittelpunkt mithilfe von [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz) über bzw. unter der Fläche des Objekts. Auf diese Weise können Sie das Objekt wie einen Planeten auf seiner Umlaufbahn um einen Stern um den betreffenden Punkt drehen.

## <a name="positioning-an-object"></a>Positionieren eines Objekts

Sie haben nun erfahren, wie Sie ein Objekt im Raum drehen können. Mit den folgenden Eigenschaften können Sie diese gedrehten Objekte relativ zueinander im Raum positionieren:

-   [**LocalOffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx) verschiebt ein Objekt entlang der x-Achse der Ebene eines Objekts gedreht.
-   [**LocalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) verschiebt ein Objekt entlang der y-Achse der Ebene eines Objekts gedreht.
-   [**LocalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) verschiebt ein Objekt entlang der z-Achse, der die Ebene mit dem ein Objekt gedreht.
-   [**GlobalOffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) verschiebt ein Objekt entlang der x-Achse Bildschirm rechtsbündig ausgerichtet.
-   [**GlobalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) verschiebt ein Objekt entlang der y-Achse Bildschirm rechtsbündig ausgerichtet.
-   [**GlobalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) verschiebt ein Objekt entlang der z-Achse Bildschirm rechtsbündig ausgerichtet.

**Lokaler Versatz**

Die Eigenschaften [**LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx), [**LocalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) und [**LocalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) versetzen ein Objekt entlang der entsprechenden Achse der Fläche des Objekts, nachdem dieses gedreht wurde. Daher bestimmt die Drehung des Objekts die Richtung, in die das Objekt versetzt wird. Zum Verdeutlichen dieses Konzepts wird im folgenden Beispiel **LocalOffsetX** von 0 bis 400 Grad animiert, und [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) wird von 0 bis 65 Grad animiert.

Im vorigen Beispiel ist zu beobachten, dass das Objekt entlang seiner eigenen X-Achse verschoben wird. Das Objekt wird gleich zu Beginn der Animation, wenn der Wert von [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) nahe null liegt (d. h. parallel zum Bildschirm), entlang des Bildschirms in der X-Richtung verschoben. Während das Objekt jedoch in Richtung des Betrachters gedreht wird, erfolgt eine Verschiebung entlang der X-Achse der Objektfläche in Richtung Betrachter. Wenn Sie die **RotationY**-Eigenschaft jedoch mit einem Wert von -65 Grad animiert haben, wird das Objekt in einer Kurvenbewegung vom Betrachter weg gedreht.

[**LocalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) funktioniert ähnlich wie [ **LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx), außer dass sie entlang der vertikalen Achse, so dass die Änderung verschoben [ **RotationX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) wirkt sich auf die Richtung **LocalOffsetY** verschiebt das Objekt. Im nächsten Beispiel wird **LocalOffsetY** von 0 bis 400 Grad animiert, und **RotationX** wird von 0 bis 65 Grad animiert.

[**LocalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) übersetzt das Objekt senkrecht zur Ebene des Objekts, als wären Sie direkt über das Center über das Objekt, auf dem Sie ein Vektor gezeichnet wurde. Zum Veranschaulichen der Funktionsweise von **LocalOffsetZ** wird im folgenden Beispiel **LocalOffsetZ** von 0 bis 400 Grad animiert, und [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) wird von 0 bis 65 Grad animiert.

Das Objekt bewegt sich zu Beginn der Animation, wenn der Wert von [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) nahe null liegt (d. h. parallel zum Bildschirm), direkt in Richtung des Betrachters, mit der Abwärtsdrehung des Objekts wird dieses jedoch nach unten verschoben.

**Globaler Versatz**

Die Eigenschaften [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx), [**GlobalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) und [**GlobalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) versetzen das Objekt entlang der Achsen relativ zum Bildschirm. Anders als bei den Eigenschaften für den lokalen Versatz ist daher die Achse, entlang der das Objekt verschoben wird, unabhängig von der auf das Objekt angewendeten Drehung. Diese Eigenschaften sind hilfreich, wenn Sie das Objekt lediglich entlang der X-, Y- oder Z-Achse des Bildschirms verschieben möchten und eine Drehung des Objekts keine Rolle spielt.

Im nächsten Beispiel wird [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) von 0 bis 400 Grad animiert, und [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) wird von 0 bis 65 Grad animiert.

Sie stellen fest, dass in diesem Beispiel die Richtung des Objekts während seiner Drehung nicht geändert wird. Dies liegt daran, dass das Objekt ungeachtet seiner Drehung entlang der X-Achse des Bildschirms gedreht wird.

## <a name="positioning-an-object"></a>Positionieren eines Objekts

Sie können den [**Matrix3DProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix3DProjection)-Typ und den [**Matrix3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.Matrix3D)-Typ für Semi-3D-Szenarien nutzen, die komplexer als die von [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) unterstützten sind. **Matrix3DProjection** bietet Ihnen eine komplette 3D-Transformationsmatrix, die auf ein beliebiges [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) angewendet werden kann. Somit können Sie Transformationsmatrizen und Perspektivmatrizen beliebiger Modelle auf Elemente anwenden. Beachten Sie, dass diese APIs nur im Minimalzustand vorliegen. Wenn Sie sie also nutzen möchten, müssen Sie den Code schreiben, mit dem die 3D-Transformationsmatrizen ordnungsgemäß erstellt werden. Daher ist es einfacher, für einfache 3D-Szenarien **PlaneProjection** zu verwenden.
