---
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: Zeichnen von Formen
description: Erfahren Sie, wie Sie Formen wie Ellipsen, Rechtecke, Polygone und Pfade zeichnen. Mithilfe der Klasse Path visualisieren Sie eine ziemlich komplexe vektorbasierte Zeichnungssprache in einer XAML-Benutzeroberfläche. Beispielsweise können Sie auf diese Weise Bézierkurven zeichnen.
ms.date: 11/16/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b02677cb6f38bc86d5123e835f658bedd83543de
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173934"
---
# <a name="draw-shapes"></a>Zeichnen von Formen

Erfahren Sie, wie Sie Formen wie Ellipsen, Rechtecke, Polygone und Pfade zeichnen. Mithilfe der Klasse [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) visualisieren Sie eine ziemlich komplexe vektorbasierte Zeichnungssprache in einer XAML-Benutzeroberfläche. Beispielsweise können Sie auf diese Weise Bézierkurven zeichnen.

> **Wichtige APIs:** [Path-Klasse](/uwp/api/Windows.UI.Xaml.Shapes.Path), [Windows.UI.Xaml.Shapes-Namespace](/uwp/api/Windows.UI.Xaml.Shapes), [Windows.UI.Xaml.Media-Namespace](/uwp/api/Windows.UI.Xaml.Media)


Zwei Sätze von Klassen definieren einen Bereich in der XAML-Benutzeroberfläche: [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)-Klassen und [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry)-Klassen. Der Hauptunterschied zwischen diesen Klassen besteht darin, dass einer **Shape**-Klasse ein Pinsel zugeordnet ist und diese auf dem Bildschirm gerendert werden kann, während eine **Geometry**-Klasse einfach einen Bereich definiert und nur gerendert wird, wenn sie Informationen zu einer anderen UI-Eigenschaft beiträgt. Sie können sich eine **Shape**-Klasse als [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) vorstellen, dessen Umrandung durch eine **Geometry**-Klasse definiert ist. In diesem Thema werden hauptsächlich die **Shape**-Klassen behandelt.

Die [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)-Klassen sind [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line), [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse), [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon), [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) und [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path). **Path** ist interessant, da Sie mit dieser Klasse beliebige Geometrien definieren können, und die Klasse [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) stellt eine Möglichkeit zum Definieren der Teile eines **Path** dar.

## <a name="fill-and-stroke-for-shapes"></a>Füllungen und Striche für Formen

Damit eine [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)-Klasse auf dem App-Canvas gerendert wird, müssen Sie ihr einen [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) zuweisen. Legen Sie die [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft der **Shape**-Klasse auf den gewünschten **Brush** fest. Weitere Informationen zu Pinseln finden Sie unter [Verwenden von Pinseln](../style/brushes.md).

Eine [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)-Klasse kann auch einen [**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke) aufweisen. Dies ist eine Linie, die um den Rand der Form gezeichnet wird. Ein **Stroke** erfordert auch einen [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Dieser definiert die Darstellung und sollte einen Wert ungleich Null für [**StrokeThickness**](/uwp/api/windows.ui.xaml.shapes.shape.strokethickness) aufweisen. **StrokeThickness** ist eine Eigenschaft, die die Stärke des Rands um die Form definiert. Wenn Sie keinen **Brush**-Wert für **Stroke** angeben oder **StrokeThickness** auf 0 festlegen, wird kein Rahmen um die Form gezeichnet.

## <a name="ellipse"></a>Ellipse

Eine [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) ist eine Form mit einem kurvenförmigen Rand. Zum Erstellen einer einfachen **Ellipse** geben Sie [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) und einen [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) für [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) an.

Im nächsten Beispiel wird eine [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) mit einer [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) von 200 und einer [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) von 200 erstellt, die die Farbe [**SteelBlue**](/uwp/api/windows.ui.colors.steelblue) für [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) als [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) verwendet.

```xaml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

```csharp
var ellipse1 = new Ellipse();
ellipse1.Fill = new SolidColorBrush(Windows.UI.Colors.SteelBlue);
ellipse1.Width = 200;
ellipse1.Height = 200;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(ellipse1);
```

Dies ist die gerenderte [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

![Eine gerenderte Ellipse](images/shapes-ellipse.jpg)

In diesem Fall ist die [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) das, was die meisten Menschen als einen Kreis bezeichnen würden. So werden Kreisformen in XAML definiert: Sie verwenden eine **Ellipse** mit gleicher [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) und [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height).

Wenn eine [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) in einem UI-Layout positioniert wird, wird davon ausgegangen, dass ihre Größe einem Rechteck dieser [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) und [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) entspricht. Der Bereich außerhalb des Rands weist kein Rendering auf, gehört jedoch weiterhin zur Größe des Layoutschlitzes.

Eine Gruppe von 6 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)-Elementen ist Teil der Steuerelementvorlage für das [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)-Steuerelement. 2 konzentrische **Ellipse**-Elemente sind Teil eines [**RadioButton**](/uwp/api/Windows.UI.Xaml.Controls.RadioButton).

## <a name="span-idrectanglespanspan-idrectanglespanspan-idrectanglespanrectangle"></a><span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>Rectangle

Ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) ist eine vierseitige Form, bei der die gegenüberliegenden Seiten gleich sind. Um ein einfaches **Rectangle** zu erstellen, geben Sie eine [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), eine [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) und einen Wert für [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) an.

Sie können die Ecken eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) abrunden. Um abgerundete Ecken zu erstellen, geben Sie einen Wert für die Eigenschaften [**RadiusX**](/uwp/api/windows.ui.xaml.shapes.rectangle.radiusx) und [**RadiusY**](/uwp/api/windows.ui.xaml.shapes.rectangle.radiusy) an. Diese Eigenschaften geben die x-Achse und die y-Achse einer Ellipse an, die die Kurven der Ecken angibt. Der zulässige Maximalwert für **RadiusX** ist [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) geteilt durch zwei. Der zulässige Maximalwert für **RadiusY** ist [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) geteilt durch zwei.

Im nächsten Beispiel wird ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) mit einer [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) von 200 und einer [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) von 100 erstellt. Es verwendet den Wert [**Blue**](/uwp/api/windows.ui.colors.blue) von [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) als [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) und den Wert [**Black**](/uwp/api/windows.ui.colors.black) von **SolidColorBrush** als [**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke). Die [**StrokeThickness**](/uwp/api/windows.ui.xaml.shapes.shape.strokethickness) wird als 3 festgelegt. Die [**RadiusX**](/uwp/api/windows.ui.xaml.shapes.rectangle.radiusx)-Eigenschaft wird auf 50, und die [**RadiusY**](/uwp/api/windows.ui.xaml.shapes.rectangle.radiusy)-Eigenschaft wird auf 10 festgelegt. Damit erhält das **Rectangle** abgerundete Ecken.

```xaml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
```

```csharp
var rectangle1 = new Rectangle();
rectangle1.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
rectangle1.Width = 200;
rectangle1.Height = 100;
rectangle1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
rectangle1.StrokeThickness = 3;
rectangle1.RadiusX = 50;
rectangle1.RadiusY = 10;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(rectangle1);
```

Hier die gerenderte [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Klasse.

![Ein gerendertes Rechteck](images/shapes-rectangle.jpg)

**Tipp**  In einigen Szenarien für UI-Definitionen ist die Verwendung eines [**Border**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Elements anstelle eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Controls.Border)-Elements möglicherweise angemessener. Wenn Sie eine rechteckige Form um anderen Inhalt zeichnen möchten, ist **Border** u. U. besser geeignet. Rahmen können untergeordnete Elemente enthalten, und ihre Größe passt sich automatisch an den Inhalt an. Dagegen sind die Breite und Höhe von **Rectangle**-Elementen auf feste Werte festgelegt. Die Ecken von **Border** können durch Festlegen der [**CornerRadius**](/uwp/api/windows.ui.xaml.controls.border.cornerradius)-Eigenschaft auch abgerundet werden.

Andererseits ist [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) wahrscheinlich eine bessere Wahl für die Steuerelementkomposition. Eine **Rectangle**-Form wird in vielen Steuerelementvorlagen verwendet, da sie als FocusVisual-Teil für fokussierbare Steuerelemente verwendet wird. Befindet sich das Steuerelement in einem fokussierten Ansichtszustand, wird dieses Rechteck sichtbar gemacht; in anderen Zuständen ist es ausgeblendet.

## <a name="polygon"></a>Polygon

Ein [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) ist eine Form, deren Grenze durch eine beliebige Anzahl von Punkten angegeben wird. Die Grenze wir erstellt, indem eine Linie von einem Punkt zum nächsten gezeichnet wird, bis der letzte Punkt wieder mit dem ersten abschließt. Die [**Points**](/uwp/api/windows.ui.xaml.shapes.polygon.points)-Eigenschaft definiert die Sammlung von Punkten, die zusammen die Grenze ergeben. In XAML definieren Sie die Punkte mit einer durch Trennzeichen getrennten Liste. Verwenden Sie im CodeBehind eine [**PointCollection**](/uwp/api/Windows.UI.Xaml.Media.PointCollection), um die Punkte zu definieren, und fügen Sie der Sammlung jeden einzelnen Punkt als [**Point**](/uwp/api/Windows.Foundation.Point)-Wert hinzu.

Die einzelnen Punkte müssen nicht explizit so deklariert werden, dass der Anfangs- und der Endpunkt als derselbe [**Point**](/uwp/api/Windows.Foundation.Point)-Wert angegeben werden. Die Renderinglogik für ein [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) geht davon aus, dass Sie eine geschlossene Form definieren, und verbindet den Anfangspunkt implizit mit dem Endpunkt.

Im nächsten Beispiel wird ein [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) mit 4 Punkten bei `(10,200)`, `(60,140)`, `(130,140)`und `(180,200)` erstellt. Für [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) wird der [**LightBlue**](/uwp/api/windows.ui.colors.lightblue)-Wert [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) verwendet. Da kein [**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke)-Wert vorhanden ist, wird keine Umrandung erstellt.

```xaml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polygon1 = new Polygon();
polygon1.Fill = new SolidColorBrush(Windows.UI.Colors.LightBlue);

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polygon1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polygon1);
```

Dies ist das gerenderte [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon).

![Ein gerendertes Polygon](images/shapes-polygon.jpg)

**Tipp**  Ein [**Point**](/uwp/api/Windows.Foundation.Point)-Wert wird in XAML oft als Typ für andere Szenarien als das Deklarieren der Scheitelpunkte von Formen verwendet. Ein **Point** ist beispielsweise Teil der Ereignisdaten für Fingereingabeereignisse, damit Sie ermitteln können, wo genau in Koordinatenbereich die Fingereingabeaktion stattgefunden hat. Weitere Informationen zu **Point** und dessen Verwendung in XAML oder Code finden Sie in der API-Referenz für [**Point**](/uwp/api/Windows.Foundation.Point).

## <a name="line"></a>Zeile

Eine [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) ist eine einfache Verbindung zweier Punkte in einem Koordinatenbereich. Eine **Line** ignoriert alle Werte für [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill), da sie keinen Innenbereich hat. Geben Sie bei einer **Line** Werte für die Eigenschaften [**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke) und [**StrokeThickness**](/uwp/api/windows.ui.xaml.shapes.shape.strokethickness) an, da die **Line** andernfalls nicht gerendert werden kann.

Sie verwenden keine [**Point**](/uwp/api/Windows.Foundation.Point)-Werte, um eine [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line)-Form anzugeben, sondern diskrete [**Double**](/dotnet/api/system.double)-Werte für [**X1**](/uwp/api/windows.ui.xaml.shapes.line.x1), [**Y1**](/uwp/api/windows.ui.xaml.shapes.line.y1), [**X2**](/uwp/api/windows.ui.xaml.shapes.line.x2) und [**Y2**](/uwp/api/windows.ui.xaml.shapes.line.y2). Dadurch wird das Markup für horizontale oder vertikale Linien minimiert. Beispielsweise definiert `<Line Stroke="Red" X2="400"/>` eine horizontale Linie mit einer Länge von 400 Pixeln. Die anderen X,Y-Eigenschaften sind standardmäßig 0. Mit diesem XAML wird daher bei Punkten eine Linie von `(0,0)` bis `(400,0)` gezeichnet. Anschließend können Sie [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) zum Verschieben der gesamten **Line** verwenden, wenn sie an einem anderen Punkt als (0,0) beginnen soll.

```xaml
<Line Stroke="Red" X2="400"/>
```

```csharp
var line1 = new Line();
line1.Stroke = new SolidColorBrush(Windows.UI.Colors.Red);
line1.X2 = 400;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(line1);
```

## <a name="span-id_polylinespanspan-id_polylinespanspan-id_polylinespan-polyline"></a><span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span> Polyline

Eine [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) ähnelt einem [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon), da bei beiden die Grenze der Form durch eine Reihe von Punkten definiert wird. Bei einer **Polyline** wird jedoch der letzte Punkt nicht mit dem ersten verbunden.

**Hinweis**  Sie könnten zwar in den [**Points**](/uwp/api/windows.ui.xaml.shapes.polyline.points) explizit einen identischen Anfangs- und Endpunkt für [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) festlegen. In diesem Fall sollten Sie jedoch wahrscheinlich eher ein [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) verwenden.

Wenn Sie die [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) einer [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) angeben, füllt **Fill** den Innenraum der Form. Dies gilt auch, wenn der Anfangs- und Endpunkt der [**Points**](/uwp/api/windows.ui.xaml.shapes.polyline.points), die für **Polyline** festgelegt wurden, keine Überschneidungen aufweisen. Wenn Sie keine **Fill** angeben, ähnelt die **Polyline** dem Rendering für mehrere einzelne [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line)-Elemente, bei denen sich die Anfangs- und Endpunkte aufeinander folgender Linien überschneiden.

Wie bei einem [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) definiert die [**Points**](/uwp/api/windows.ui.xaml.shapes.polyline.points)-Eigenschaft die Sammlung von Punkten, die zusammen die Grenze ergeben. In XAML definieren Sie die Punkte mit einer durch Trennzeichen getrennten Liste. Im CodeBehind verwenden Sie eine [**PointCollection**](/uwp/api/Windows.UI.Xaml.Media.PointCollection), um die Punkte zu definieren, und fügen der Sammlung jeden einzelnen Punkt als eine [**Point**](/uwp/api/Windows.Foundation.Point)-Struktur hinzu.

Im nächsten Beispiel wird eine [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) mit vier Punkten bei `(10,200)`, `(60,140)`, `(130,140)` und `(180,200)` erstellt. Es ist ein [**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke) definiert, jedoch keine [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill).

```xaml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polyline1 = new Polyline();
polyline1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
polyline1.StrokeThickness = 4;

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polyline1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polyline1);
```

Dies ist die gerenderte [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline). Beachten Sie, dass der erste und letzte Punkt nicht durch die [**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke)-Umrandung miteinander verbunden sind, anders als bei einem [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon).

![Eine gerenderte Polylinie](images/shapes-polyline.jpg)

## <a name="path"></a>Pfad

Ein [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) ist die vielseitigste [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)-Klasse, da Sie mit dieser beliebige Geometrien definieren können. Diese Vielseitigkeit führt jedoch auch zu Komplexität. Betrachten wir nun, wie ein einfacher **Path** in XAML erstellt wird.

Sie definieren die Geometrie des Pfads mit der [**Data**](/uwp/api/windows.ui.xaml.shapes.path.data)-Eigenschaft. Es gibt zwei Techniken zum Festlegen von **Data**:

- Sie können einen Zeichenfolgenwert für [**Data**](/uwp/api/windows.ui.xaml.shapes.path.data) in XAML festlegen. In diesem Format verwendet der **Path.Data**-Wert ein Serialisierungsformat für Grafiken. Nachdem er einmal festgelegt wurde, wird dieser Wert normalerweise nicht mehr als Zeichenfolge bearbeitet. Stattdessen arbeiten Sie mit Entwicklungstools in einer Entwurfs- oder Zeichnungsmetapher auf einer Oberfläche. Anschließend speichern oder exportieren Sie die Ausgabe und erhalten eine XAML-Datei oder ein XAML-Zeichenfolgenfragment mit **Path.Data**-Informationen.
- Sie können die [**Data**](/uwp/api/windows.ui.xaml.shapes.path.data)-Eigenschaft für ein einzelnes [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry)-Objekt festlegen. Die kann im Code oder in XAML erfolgen. Diese einzelne **Geometry** ist typischerweise eine [**GeometryGroup**](/uwp/api/windows.ui.xaml.media.geometrygroup), die als Container dient und mehrere Geometriedefinitionen für das Objektmodell in einem Objekt zusammenfassen kann. In aller Regel wird diese Vorgehensweise gewählt, um mindestens eine der Kurven und komplexen Formen, die als [**Segments**](/uwp/api/windows.ui.xaml.media.pathfigure.segments)-Werte definiert werden können, für ein [**PathFigure**](/uwp/api/Windows.UI.Xaml.Media.PathFigure) zu verwenden, beispielsweise [**BezierSegment**](/uwp/api/Windows.UI.Xaml.Media.BezierSegment).

Dieses Beispiel zeigt einen [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path), der möglicherweise daraus entstanden ist, dass Blend für Visual Studio zur Erzeugung einiger Vektorformen verwendet wurde und das Ergebnis als XAML gespeichert wurde. Der vollständige **Path** besteht aus einem Bézierkurven- und einem Liniensegment. Dieses Beispiel soll in erster Linie die Elemente im [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data)-Serialisierungsformat zeigen und verstehen helfen, wofür die Zahlen stehen.

Diese [**Data**](/uwp/api/windows.ui.xaml.shapes.path.data) beginnen mit dem move-Befehl, der durch „M“ gekennzeichnet ist und einen Anfangspunkt für den Pfad erstellt.

Das erste Segment ist eine kubische Bézierkurve, die bei `(100,200)` beginnt und bei `(400,175)` endet. Sie wird mit zwei Kontrollpunkten bei `(100,25)` und `(400,350)` gezeichnet. Dieses Segment wird durch den „C“-Befehl in der Zeichenfolge des [**Data**](/uwp/api/windows.ui.xaml.shapes.path.data)-Attributs angegeben.

Das zweite Segment beginnt mit dem Befehl „H“ für eine absolute horizontale Linie. Dieser gibt an, dass eine Linie vom bisherigen Endpunkt des Pfads, `(400,175)`, zum neuen Endpunkt, `(280,175)`, gezeichnet werden soll. Da es sich um einen Befehl für eine horizontale Linie handelt, ist der angegebene Wert eine x-Koordinate.

```xaml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
```

Hier ist der gerenderte [**Pfad**](/uwp/api/Windows.UI.Xaml.Shapes.Path).

![Ein gerenderter Pfad](images/shapes-path.jpg)

Das nächste Beispiel zeigt die Verwendung der anderen vorgestellten Technik: [**GeometryGroup**](/uwp/api/windows.ui.xaml.media.geometrygroup) mit [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry). Dieses Beispiel stellt einige der beteiligten Geometrietypen vor, die als Teil einer **PathGeometry** verwendet werden können: [**PathFigure**](/uwp/api/Windows.UI.Xaml.Media.PathFigure) und die verschiedenen Elemente, die als Segment in [**PathFigure.Segments**](/uwp/api/windows.ui.xaml.media.pathfigure.segments) dienen können.

```xaml
<Path Stroke="Black" StrokeThickness="1" Fill="#CCCCFF">
    <Path.Data>
        <GeometryGroup>
            <RectangleGeometry Rect="50,5 100,10" />
            <RectangleGeometry Rect="5,5 95,180" />
            <EllipseGeometry Center="100, 100" RadiusX="20" RadiusY="30"/>
            <RectangleGeometry Rect="50,175 100,10" />
            <PathGeometry>
                <PathGeometry.Figures>
                    <PathFigureCollection>
                        <PathFigure IsClosed="true" StartPoint="50,50">
                            <PathFigure.Segments>
                                <PathSegmentCollection>
                                    <BezierSegment Point1="75,300" Point2="125,100" Point3="150,50"/>
                                    <BezierSegment Point1="125,300" Point2="75,100"  Point3="50,50"/>
                                </PathSegmentCollection>
                            </PathFigure.Segments>
                        </PathFigure>
                    </PathFigureCollection>
                </PathGeometry.Figures>
            </PathGeometry>
        </GeometryGroup>
    </Path.Data>
</Path>
```

```csharp
var path1 = new Windows.UI.Xaml.Shapes.Path();
path1.Fill = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 204, 204, 255));
path1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
path1.StrokeThickness = 1;

var geometryGroup1 = new GeometryGroup();
var rectangleGeometry1 = new RectangleGeometry();
rectangleGeometry1.Rect = new Rect(50, 5, 100, 10);
var rectangleGeometry2 = new RectangleGeometry();
rectangleGeometry2.Rect = new Rect(5, 5, 95, 180);
geometryGroup1.Children.Add(rectangleGeometry1);
geometryGroup1.Children.Add(rectangleGeometry2);

var ellipseGeometry1 = new EllipseGeometry();
ellipseGeometry1.Center = new Point(100, 100);
ellipseGeometry1.RadiusX = 20;
ellipseGeometry1.RadiusY = 30;
geometryGroup1.Children.Add(ellipseGeometry1);

var pathGeometry1 = new PathGeometry();
var pathFigureCollection1 = new PathFigureCollection();
var pathFigure1 = new PathFigure();
pathFigure1.IsClosed = true;
pathFigure1.StartPoint = new Windows.Foundation.Point(50, 50);
pathFigureCollection1.Add(pathFigure1);
pathGeometry1.Figures = pathFigureCollection1;

var pathSegmentCollection1 = new PathSegmentCollection();
var pathSegment1 = new BezierSegment();
pathSegment1.Point1 = new Point(75, 300);
pathSegment1.Point2 = new Point(125, 100);
pathSegment1.Point3 = new Point(150, 50);
pathSegmentCollection1.Add(pathSegment1);

var pathSegment2 = new BezierSegment();
pathSegment2.Point1 = new Point(125, 300);
pathSegment2.Point2 = new Point(75, 100);
pathSegment2.Point3 = new Point(50, 50);
pathSegmentCollection1.Add(pathSegment2);
pathFigure1.Segments = pathSegmentCollection1;

geometryGroup1.Children.Add(pathGeometry1);
path1.Data = geometryGroup1;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(path1);
```

Hier ist der gerenderte [**Pfad**](/uwp/api/Windows.UI.Xaml.Shapes.Path).

![Ein gerenderter Pfad](images/shapes-path-2.png)

Durch die Verwendung von [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) wird das Ergebnis lesbarer als durch das Ausfüllen einer [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data)-Zeichenfolge. Andererseits verwendet [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data) eine Syntax, die mit Imagepfaddefinitionen für skalierbare Vektorgrafiken (SVG) kompatibel ist. Daher kann es nützlich sein, Grafiken aus SVG oder als Ausgabe von einem Tool wie etwa Blend zu portieren.