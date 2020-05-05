---
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: Transformationen – Übersicht
description: Hier erfahren Sie, wie Sie Transformationen in der Windows-Runtime-API \#verwenden, indem Sie die relativen Koordinatensysteme der Elemente in der UI ändern.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: aa60db28003c4f231cf36b653c5e69b422978c1a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "71340068"
---
# <a name="transforms-overview"></a>Transformationen – Übersicht

Hier erfahren Sie, wie Sie Transformationen in der Windows-Runtime-API durch Ändern des relativen Koordinatensystems der UI-Elemente verwenden. Hiermit kann die Darstellung individueller XAML-Elemente angepasst werden, z. B. durch Skalieren, Drehen oder Transformieren der Position im zweidimensionalen Raum (XY).

## <a name="span-idwhat_is_a_transform_spanspan-idwhat_is_a_transform_spanspan-idwhat_is_a_transform_spanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>Was ist eine Transformation?

Eine *Transformation* definiert, wie Punkte aus einem Koordinatenbereich einem anderen Koordinatenbereich zugeordnet (transformiert) werden. Beim Anwenden von Transformationen auf UI-Elemente wird die Darstellung des UI-Elements auf dem Bildschirm geändert.

Transformationen können in vier breite Bereiche klassifiziert werden: Übersetzung, Drehung, Skalierung und Neigung (oder Scherung). Zum Ändern der Darstellung von UI-Elementen anhand von Grafik-APIs erstellen Sie am besten Transformationen, die nur jeweils einen Vorgang definieren. Auf diese Weise definiert die Windows-Runtime eine diskrete Klasse für jede der Transformationsklassifikationen:

-   [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform): übersetzt ein Element in den x-y-Raum durch Festlegen von Werten für [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) und [**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.y).
-   [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform): skaliert die Transformation auf der Grundlage eines Mittelpunkts durch Festlegen von Werten für [**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centerx), [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centery), [**ScaleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) und [**ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform): zur Drehung im X-Y-Raum durch Festlegen von Werten für [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle), [**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centerx) und [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centery).
-   [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform): zur Neigung bzw. Scherung im X-Y-Raum durch Festlegen von Werten für [**AngleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.anglex), [**AngleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.angley), [**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.centerx) und [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centeryproperty).

Von diesen Werten werden Sie für UI-Szenarien höchstwahrscheinlich [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) und [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform) am häufigsten verwenden.

Transformationen lassen sich kombinieren, und dies wird von zwei Windows-Runtime-Klassen unterstützt: [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform) und [**TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup). In einem **CompositeTransform**-Element werden Transformationen in folgender Reihenfolge angewendet: Skalieren, Neigen, Drehen, Übersetzen. Verwenden Sie **TransformGroup** anstelle von **CompositeTransform**, wenn die Transformationen in einer anderen Reihenfolge angewendet werden sollen. Weitere Informationen finden Sie unter [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform).

## <a name="span-idtransforms_and_layoutspanspan-idtransforms_and_layoutspanspan-idtransforms_and_layoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>Transformationen und Layout

Im XAML-Layout werden Transformationen nach Abschluss des Layoutdurchlaufs angewendet. Berechnungen zum verfügbaren Raum und andere Layout-Entscheidungen werden demnach vor der Anwendung der Transformationen durchgeführt. Da das Layout an erster Stelle steht, werden Sie mitunter unerwartete Ergebnisse erhalten, wenn Sie Elemente transformieren, die sich in einer [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)-Zelle oder in einem ähnlichen Layoutcontainer befinden, der Raum im Layout zuordnet. Das transformierte Element wird möglicherweise abgeschnitten oder verdeckt dargestellt, da in einem Bereich gezeichnet wird, für den bei der Raumeinteilung innerhalb des übergeordneten Containers die Abmessungen nach der Transformation nicht berechnet wurden. Experimentieren Sie ggf. mit den Transformationsergebnissen und passen Sie die Einstellungen an. Anstatt beispielsweise auf ein adaptives Layout und eine Größenanpassung mit Sternvariablen zu vertrauen, müssen Sie möglicherweise die **Center**-Eigenschaften ändern oder die Pixelmaße für den Layoutraum deklarieren, um sicherzustellen, dass das übergeordnete Element genügend Platz zuweist.

**Migrationshinweis:**  Windows Presentation Foundation (WPF) besaß die Eigenschaft **LayoutTransform**, die Transformationen vor dem Layoutdurchlauf anwendete. Windows-Runtime-XAML unterstützt jedoch keine **LayoutTransform**-Eigenschaft. (In Microsoft Silverlight war diese Eigenschaft auch nicht vorhanden.)

## <a name="span-idapplying_a_transform_to_a_ui_elementspanspan-idapplying_a_transform_to_a_ui_elementspanspan-idapplying_a_transform_to_a_ui_elementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>Anwenden einer Transformation auf ein UI-Element

Wenn Sie eine Transformation auf ein Objekt anwenden, legen Sie damit normalerweise die [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)-Eigenschaft fest. Durch Festlegen dieser Eigenschaft wird das Objekt nicht wirklich Pixel für Pixel geändert. Tatsächlich wendet die Eigenschaft die Transformation innerhalb des lokalen Koordinatenraums an, in dem das Objekt vorhanden ist. Die Renderlogik und der Vorgang (Post-Layout) rendern die kombinierten Koordinatenbereiche. Hierdurch entsteht der Eindruck, als hätte das Objekt sein Erscheinungsbild und möglicherweise auch die Layoutposition geändert (wenn [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) angewendet wurde).

Standardmäßig befindet sich der Mittelpunkt jeder Rendertransformation am Ursprung des lokalen Koordinatensystems des Zielobjekts (0,0). Die einzige Ausnahme ist eine [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), für die keine Mittelpunkteigenschaften festgelegt werden können, da der Übersetzungseffekt unabhängig vom Mittelpunkt derselbe ist. Alle anderen Transformationen verfügen über Eigenschaften, die **CenterX**- und **CenterY**-Werte festlegen.

Wenn Sie Transformationen mit [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)-Elementen verwenden, beachten Sie, dass eine weitere Eigenschaft für [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) vorhanden ist, die sich auf das Verhalten der Transformation auswirkt: [**RenderTransformOrigin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransformorigin). Die **RenderTransformOrigin**-Eigenschaft deklariert, ob sich die gesamte Transformation auf den Standardpunkt (0,0) eines Elements oder einen anderen Ursprungspunkt innerhalb des relativen Koordinatenraums des Elements beziehen soll. Normalerweise wird mit (0,0) die Transformation in der oberen linken Ecke platziert. Je nachdem, welcher Effekt erzielt werden soll, empfiehlt sich die Änderung des **RenderTransformOrigin**-Elements anstelle der Anpassung der **CenterX**- und **CenterY**-Werte für die Transformationen. Wenn Sie sowohl **RenderTransformOrigin** als auch **CenterX** / **CenterY**-Werte anwenden, können die Ergebnisse verwirrend sein. Dies gilt besonders, wenn Sie die Werte animieren.

Bei Treffertests reagiert ein Objekt, auf das eine Transformation angewendet wird, weiterhin erwartungsgemäß und im Einklang mit der visuellen Darstellung im zweidimensionalen Raum (XY) auf Eingaben. Haben Sie beispielsweise ein [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-Element verwendet, um ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Element auf der Benutzeroberfläche seitwärts um 400 Pixel zu verschieben, reagiert das **Rectangle**-Element auf [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)-Ereignisse, sobald der Benutzer den Punkt berührt, an dem das **Rectangle**-Element dargestellt wird. Wenn der Benutzer den Bereich berührt, in dem sich das **Rectangle**-Element vor der Übersetzung befand, erhalten Sie keine falschen Ereignisse. Im Hinblick auf Z-Index-Überlegungen, die sich auf Treffertests auswirken, macht das Anwenden von Transformationen keinen Unterschied. Der Z-Index, der bestimmt, von welchem Element Eingabeereignisse für einen Punkt im X-Y-Raum gehandhabt werden, wird immer noch anhand der untergeordneten Reihenfolge ausgewertet, die im Container deklariert ist. Diese Reihenfolge entspricht normalerweise der Reihenfolge, in der die Elemente im XAML-Code deklariert werden, obwohl für untergeordnete Elemente eines [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)-Objekts die Reihenfolge durch Anwenden der angehängten [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95))-Eigenschaft auf untergeordnete Elemente angepasst werden kann.

## <a name="span-idother_transform_propertiesspanspan-idother_transform_propertiesspanspan-idother_transform_propertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>Andere Transformationseigenschaften

-   [**Brush.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.transform), [**Brush.RelativeTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.relativetransform): diese beeinflussen, wie ein [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Element den Koordinatenraum innerhalb des Bereichs verwendet, auf den das **Brush**-Element angewendet wird, um visuelle Eigenschaften (z. B. Vorder- und Hintergründe) festzulegen. Diese Transformationen sind für die meisten gängigen Brush-Elemente (mit denen normalerweise Volltonfarben mit [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) festgelegt werden) zwar nicht relevant, sie können jedoch beim Zeichnen in Bereichen mit einem [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)- oder [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)-Element hilfreich sein.
-   [**Geometry.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.geometry.transform): Sie können diese Eigenschaft zum Anwenden einer Transformation auf ein geometrisches Objekt verwenden, bevor dieses geometrische Objekt für einen [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data)-Eigenschaftswert verwendet wird.

## <a name="span-idanimating_a_transformspanspan-idanimating_a_transformspanspan-idanimating_a_transformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>Animieren einer Transformation

[**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)-Objekte können animiert werden. Um ein **Transform**-Element zu animieren, wenden Sie eine kompatible Animation auf die entsprechende Eigenschaft an. Normalerweise bedeutet das, dass Sie [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation)- oder [**DoubleAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimationusingkeyframes)-Objekte zum Definieren der Animation verwenden, da alle Transformationseigenschaften vom Typ [**Double**](https://docs.microsoft.com/dotnet/api/system.double) sind. Animationen, die sich auf eine für einen [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)-Wert verwendete Transformation auswirken, werden selbst dann nicht als abhängige Animationen betrachtet, wenn Ihre Dauer nicht Null ist. Weitere Informationen zu abhängigen Animationen finden Sie unter [Storyboardanimationen](/windows/uwp/design/motion/storyboarded-animations).

Wenn Sie Eigenschaften animieren, um einen ähnlichen Darstellungseffekt zu erzielen wie bei einer Transformation (wenn Sie beispielsweise die Eigenschaften [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) und [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) eines [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)-Elements animieren, anstatt ein [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-Element anzuwenden), werden diese nahezu immer als abhängige Animationen behandelt. Die Animationen müssen aktiviert werden, und bei dieser Animation können beträchtliche Leistungsprobleme auftreten, insbesondere wenn während der Animation des Objekts Benutzerinteraktionen unterstützt werden sollen. Aus diesem Grund empfiehlt sich die Verwendung einer Transformation und deren Animation anstelle der Animation einer anderen Eigenschaft, bei der die Animation als abhängige Animation behandelt werden würde.

Zur Ausrichtung auf die Transformation muss ein [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)-Element als Wert für das [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)-Element vorhanden sein. Normalerweise wird ein Element für den entsprechenden Transformationstyp im ursprünglichen XAML-Code platziert. Manchmal sind keine Eigenschaften für diese Transformation festgelegt.

Typischerweise wird eine indirekte Zieltechnik verwendet, um die Animationen auf die Eigenschaften einer Transformation anzuwenden. Weitere Informationen zur Syntax für indirektes Zielen finden Sie unter [Storyboardanimationen](/windows/uwp/design/motion/storyboarded-animations) und [Eigenschaftspfadsyntax](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax).

Standardformate für Steuerelemente definieren manchmal Animationen von Transformationen als Teil ihres Verhaltens im visuellen Zustand. Beispiel: Die visuellen Zustände für das [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)-Element verwenden animierte [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform)-Werte, um die Punkte im Ring zu drehen.

Im Anschluss folgt ein einfaches Beispiel für die Animation einer Transformation. In diesem Fall wird das [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle)-Element eines [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform)-Elements animiert, um ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Element um den visuellen Mittelpunkt zu drehen. In diesem Beispiel wird das **RotateTransform**-Element benannt, daher ist kein indirektes Animationsziel erforderlich. Alternativ kann die Transformation unbenannt bleiben, und Sie können das Element benennen, auf das die Transformation angewendet wird und eine indirekte Ausrichtung verwenden (etwa `(UIElement.RenderTransform).(RotateTransform.Angle)`).

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <a name="span-idaccounting_for_coordinate_frames_of_reference_at_run_timespanspan-idaccounting_for_coordinate_frames_of_reference_at_run_timespanspan-idaccounting_for_coordinate_frames_of_reference_at_run_timespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>Angeben von Referenz-Koordinatennetzen zur Laufzeit

[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) verfügt über eine Methode mit der Bezeichnung [**TransformToVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transformtovisual), die ein [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)-Element generieren kann, das die Referenz-Koordinatennetze für zwei UI-Elemente abgleicht. So können Sie Elemente mit dem standardmäßigen Referenz-Koordinatennetz der App vergleichen, wenn der Stamm der visuellen Struktur als erster Parameter übergeben wird. Dies kann hilfreich sein, wenn ein Eingabeereignis von einem anderen Element erfasst wurde, oder wenn Layoutverhalten ohne das Anfordern einer Layoutübergabe prognostiziert werden soll.

Von Zeigerereignissen abgerufene Daten bieten Zugriff auf eine [**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpoint.getcurrentpoint)-Methode, wobei ein *relativeTo*-Parameter zum Ändern des Referenz-Koordinatennetzes in ein spezifisches Element anstelle des App-Standards angegeben werden kann. Mit diesem Ansatz wird schlicht eine Übersetzungstransformation intern angewendet, und die X-Y-Koordinatendaten werden beim Erstellen des zurückgegebenen [**PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint)-Elements transformiert.

## <a name="span-iddescribing_a_transform_mathematicallyspanspan-iddescribing_a_transform_mathematicallyspanspan-iddescribing_a_transform_mathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>Mathematische Beschreibung einer Transformation

Eine Transformation kann als Transformationsmatrix beschrieben werden. Mit einer 3x3-Matrix werden die Transformationen in einer zweidimensionalen X-Y-Ebene beschrieben. Affine Transformationsmatrizes können multipliziert werden und so eine Reihe linearer Transformationen ergeben, z. B. Drehen und Neigen (Scheren), gefolgt von Übersetzen. Die finale Spalte einer affinen Transformationsmatrix ist gleich (0, 0, 1). Demzufolge müssen Sie in der mathematischen Beschreibung lediglich die Member der ersten beiden Spalten angeben.

Die mathematische Beschreibung einer Transformation ist ggf. hilfreich, wenn Sie über mathematisches Fachwissen verfügen oder mit Grafikprogrammierungstechniken vertraut sind, die auch Matrizen zum Beschreiben von Transformationen von Koordinatenraum verwenden. Es gibt eine von [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) abgeleitete Klasse, mit der Sie eine Transformation direkt als 3x3-Matrix beschreiben können: [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform). **MatrixTransform** besitzt eine [**Matrix**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrixtransform.matrix)-Eigenschaft, die eine Struktur mit sechs Eigenschaften beinhaltet: [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11), [**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12), [**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21), [**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22), [**OffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) und [**OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety). Jede [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix)-Eigenschaft verwendet einen **Double**-Wert und entspricht den sechs relevanten Werten (Spalten 1 und 2) einer affinen Transformationsmatrix.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11)         | [**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21)         | 0   |
| [**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12)         | [**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22)         | 0   |
| [**OffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) | [**OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety) | 1   |

 

Alle mit einem [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-, [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform)-, [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform)- oder [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform)-Objekt beschreibbaren Transformationen können ebenfalls mit einem [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform)-Element mit einem [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix)-Wert beschrieben werden. Normalerweise werden allerdings das **TranslateTransform**-Element und die anderen Elemente verwendet, da die Eigenschaften für diese Transformationsklassen einfacher in Konzepte zu fassen sind als das Festlegen der Vektorkomponenten in einem **Matrix**-Element. Es ist auch einfacher, die diskreten Eigenschaften von Transformationen zu animieren. Bei einem **Matrix**-Element handelt es sich eigentlich um eine Struktur und nicht um ein [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)-Element. Daher können keine individuellen animierten Werte unterstützt werden.

Einige XAML-Designtools, die Ihnen das Anwenden von Transformationsvorgängen ermöglichen, serialisieren die Ergebnisse als [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform)-Element. In diesem Fall empfiehlt es sich, erneut das gleiche Designtool zu verwenden, um den Transformationseffekt zu ändern und den XAML-Code erneut zu serialisieren, anstatt die [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix)-Werte direkt im XAML-Code zu ändern.

## <a name="span-id3-d_transformsspanspan-id3-d_transformsspanspan-id3-d_transformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>3D-Transformationen

In Windows 10 wurde für XAML die neue Eigenschaft [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d) zum Erstellen von 3D-Effekten mit UI eingeführt. Verwenden Sie hierzu [**PerspectiveTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.perspectivetransform3d), um der Szene eine gemeinsame 3D-Perspektive (oder Kamera) hinzuzufügen, und verwenden Sie dann [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.compositetransform3d), um ein Element genau wie bei [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform) im 3D-Raum zu transformieren. Unter [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d) wird die Implementierung von 3D-Transformationen besprochen.

 Für einfachere 3D-Effekte, die nur ein einzelnes Objekt betreffen, kann die [**UIElement.Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection)-Eigenschaft verwendet werden. Wenn Sie eine [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) als Wert für diese Eigenschaft verwenden, erhalten Sie das gleiche Ergebnis wie bei der Anwendung einer festen Perspektiventransformation und mindestens einer 3D-Transformation auf das Element. Diese Art der Transformation wird im Thema [3D-Perspektiven-Effekte für XAML-UI](3-d-perspective-effects.md) ausführlicher beschrieben.

## <a name="span-idrelated_topicsspanrelated-topics"></a><span id="related_topics"></span>Verwandte Themen

* [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
* [3D-Perspektiveneffekte für XAML-UI](3-d-perspective-effects.md)
* [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)
 

 





