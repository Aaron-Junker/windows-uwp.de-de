---
title: Keyframeanimationen und Animationen für Beschleunigungsfunktionen
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: Lineare Keyframe-Animationen, Keyframe-Animationen mit einem „KeySpline“-Wert und Beschleunigungsfunktionen sind drei verschiedene Techniken für etwa das gleiche Szenario.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e68c667fe1e6d3ef61e60095e7fed02400044d07
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172424"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>Keyframeanimationen und Animationen für Beschleunigungsfunktionen



Lineare Keyframe-Animationen, Keyframe-Animationen mit einem **KeySpline**-Wert oder Beschleunigungsfunktionen sind drei verschiedene Techniken für ungefähr dasselbe Szenario: Erstellen einer komplexeren Storyboard-Animation, die ein nichtlineares Animationsverhalten ab einem bestimmten Startzustand bis zu einem Endzustand verwendet.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie vorher unbedingt das Thema [Storyboardanimationen](storyboarded-animations.md). Dieses Thema basiert auf den Animationskonzepten, die in [Storyboardanimationen](storyboarded-animations.md) erläutert wurden, und behandelt diese nicht erneut. In [Storyboard-Animationen](storyboarded-animations.md) wird beispielsweise das Anwenden von Animationen auf Ziele, das Verwenden von Storyboards als Ressourcen, die [**Timeline**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline)-Eigenschaftswerte wie z. B. [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration), [**FillBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior) usw. erläutert.

## <a name="animating-using-key-frame-animations"></a>Animieren mit Keyframe-Animationen

Keyframe-Animationen lassen mehr als einen Zielwert zu, der an einem gewissen Punkt auf der Animationszeitachse erreicht wird. Mit anderen Worten: Jeder Keyframe kann einen anderen zwischen Wert angeben, und der letzte Keyframe, der erreicht wird, ist der abschließende Animations Wert. Durch Angabe mehrerer Animationswerte können Sie komplexere Animationen erstellen. Keyframe-Animationen ermöglichen auch verschiedene Interpolationslogiken, die pro Animationstyp als unterschiedliche **KeyFrame**-Unterklassen implementiert werden. Insbesondere hat jeder Keyframe-Animationstyp eine **Discrete**-, **Linear**-, **Spline**- und **Easing**-Variation seiner **KeyFrame**-Klasse für die Angabe der Keyframes. Zum Festlegen einer Animation mit Keyframes für [**Double**](/dotnet/api/system.double) können Sie z. B. Keyframes mit [**DiscreteDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteDoubleKeyFrame), [**LinearDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.LinearDoubleKeyFrame), [**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame) und [**EasingDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.EasingDoubleKeyFrame) deklarieren. Sie können all diese Typen beliebig in einer einzelnen **KeyFrames**-Sammlung verwenden, um die Interpolation bei jedem Erreichen eines neuen Keyframes zu ändern.

Für das Interpolationsverhalten steuert jeder Keyframe die Interpolation, bis die **KeyTime**-Zeit erreicht ist. **Value** wird zum selben Zeitpunkt erreicht. Folgen weitere Keyframes, wird der Wert anschließend zum Startwert für den nächsten Keyframe in einer Sequenz.

Wenn beim Start der Animation kein Keyframe mit der **KeyTime** 0:0:0 vorhanden ist, ist der nicht animierte Wert der Eigenschaft der Startwert. Dies ähnelt der Art und Weise, in der eine **from**- / **to**- / **by** -Animation funktioniert, wenn kein **aus**vorhanden ist

Die Dauer einer Keyframe-Animation entspricht implizit der Dauer des höchsten **KeyTime**-Werts, der in einem ihrer Keyframes festgelegt ist. Sie können eine explizite [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) festlegen. Achten Sie dabei jedoch darauf, dass sie nicht kürzer als eine **KeyTime** in ihren eigenen Keyframes ist, andernfalls wird ein Teil der Animation abgeschnitten.

Neben der [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) können Sie alle [**Timeline**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline)-basierten Eigenschaften in einer Keyframe-Animation festlegen (genau wie bei einer **From**/**To**/**By**-Animation) da die Keyframe-Animationsklassen ebenfalls von der **Timeline** abgeleitet werden. Diese sind:

-   [**AutoReverse**](/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse): Sobald der letzte Keyframe erreicht ist, werden die Frames vom anderen Ende ausgehend in umgekehrter Reihenfolge wiederholt. So wird die scheinbare Dauer der Animation verdoppelt.
-   [**BeginTime**](/uwp/api/windows.ui.xaml.media.animation.timeline.begintime): verzögert den Start der Animation. Die Zeitachse für die **KeyTime**-Werte in den Frames beginnt erst nach Erreichen der **BeginTime** mit der Zählung, damit kein Risiko besteht, dass Frames abgeschnitten werden.
-   [**FillBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior): Steuert die Ereignisse nach dem Erreichen des letzten Keyframes. **FillBehavior** hat keine Auswirkungen auf Zwischenkeyframes.
-   [**RepeatBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty):
    -   Bei Festlegung auf **Forever** werden die Keyframes und die dazugehörige Zeitachse unendlich wiederholt.
    -   Bei Festlegung auf eine bestimmte Iterationsanzahl wird die Zeitachse entsprechend oft wiederholt.
    -   Bei Festlegung auf eine [**Duration**](/uwp/api/Windows.UI.Xaml.Duration) wird die Zeitachse bis zum Erreichen dieser Zeit wiederholt. Dadurch wird die Animation u. U. mitten in der Keyframe-Sequenz abgeschnitten, wenn sie keinen ganzzahligen Teiler der impliziten Dauer der Zeitachse darstellt.
-   [**SpeedRatio**](/uwp/api/windows.ui.xaml.media.animation.timeline.speedratioproperty) (selten verwendet)

### <a name="linear-key-frames"></a>Lineare Keyframes

Lineare Keyframes führen zu einer einfachen linearen Interpolation des Werts, bis die **KeyTime** des Frames erreicht ist. Dieses Interpolations Verhalten ähnelt dem einfacheren **von** / **bis hin**zu Animationen, die / im Thema [storyboarded Animationen](storyboarded-animations.md) beschrieben**werden** .

Im Folgenden erfahren Sie, wie die Renderhöhe eines Rechtecks mithilfe von linearen Keyframes skaliert wird. In diesem Beispiel wird eine Animation ausgeführt, bei der die Höhe des Rechtecks in den ersten vier Sekunden leicht und linear ansteigt und in der letzten Sekunde schnell skaliert wird, bis das Rechteck im Vergleich zum Start die doppelte Höhe erreicht hat.

```xml
<StackPanel>
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames
              Storyboard.TargetName="myRectangle"
              Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <LinearDoubleKeyFrame Value="1" KeyTime="0:0:0"/>
                <LinearDoubleKeyFrame Value="1.2" KeyTime="0:0:4"/>
                <LinearDoubleKeyFrame Value="2" KeyTime="0:0:5"/>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
    </StackPanel.Resources>
</StackPanel>
```

### <a name="discrete-key-frames"></a>Diskrete Keyframes

Diskrete Keyframes verwenden überhaupt keine Interpolation. Beim Erreichen einer **KeyTime** wird einfach der neue **Value** angewendet. Je nach der animierten UI-Eigenschaft führt dies häufig dazu, dass die Animation zu „springen“ scheint. Stellen Sie sicher, dass dieses ästhetische Verhalten gewünscht ist. Sie können die scheinbaren Sprünge reduzieren, indem Sie die Anzahl der deklarierten Keyframes erhöhen. Möchten Sie jedoch eine flüssige Animation erzielen, sollten Sie stattdessen lineare Keyframes oder Spline-Keyframes verwenden.

> [!NOTE]
> Diskrete Keyframes sind die einzige Möglichkeit, einen Wert, der nicht vom Typ " [**Double**](/dotnet/api/system.double)", " [**Point**](/uwp/api/Windows.Foundation.Point)" und " [**Color**](/uwp/api/Windows.UI.Color)" ist, mit einem " [**diskreteobjectkeyframe**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame)" zu animieren. Dies wird später in diesem Thema ausführlich erläutert.

### <a name="spline-key-frames"></a>Spline-Keyframes

Ein Spline-Keyframe erstellt einen variablen Übergang zwischen Werten gemäß dem Wert der **KeySpline** -Eigenschaft. Diese Eigenschaft gibt den ersten und zweiten Kontrollpunkt einer Bézierkurve an, mit der die Beschleunigung einer Animation beschrieben wird. Im Grunde definiert eine [**KeySpline**](/uwp/api/Windows.UI.Xaml.Media.Animation.KeySpline) eine Funktions-zu-Zeit-Beziehung, bei der das Funktions Zeitdiagramm die Form dieser Bezier-Kurve ist. In der Regel geben Sie einen **KeySpline** -Wert in einer XAML-Kurzform-Attribut Zeichenfolge mit vier [**doppelten**](/dotnet/api/system.double) Werten an, die durch Leerzeichen oder Kommas getrennt sind Diese Werte sind „X,Y“-Paare für zwei Kontrollpunkte der Bézierkurve. „X“ ist die Zeit, und „Y“ ist der Funktionsmodifizierer für den Wert. Jeder Wert sollte sich immer zwischen 0 und einschließlich 1 bewegen. Ohne Kontrollpunktänderung an einer **KeySpline** stellt die gerade Linie von 0,0 bis 1,1 eine Funktion der Zeit für eine lineare Interpolation dar. Ihre Kontrollpunkte ändern die Form der Kurve und somit das Verhalten der Funktion der Zeit für die Spline-Animation. Dies wird am besten visuell als Graph dargestellt. Sie können das Beispiel für die [Silverlight Keyspline-Schnellansicht](https://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample) in einem Browser ausführen, um zu sehen, wie die Kontrollpunkte die Kurve ändern, und wie eine Beispielanimation ausgeführt wird, wenn sie als **KeySpline**-Wert verwendet wird.

Im nächsten Beispiel wird die Anwendung von drei verschiedenen Keyframes auf eine Animation gezeigt, wobei der letzte eine Keyspline-Animation für einen [**Double**](/dotnet/api/system.double)-Wert ([**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame)) ist. Beachten Sie die Anwendung der Zeichenfolge „0.6,0.0 0.9,0.00“ für **KeySpline**. Dadurch entsteht eine Kurve, bei der die Animation zuerst scheinbar langsam ausgeführt wird, dann jedoch schnell den Wert erreicht, bevor die **KeyTime** erreicht ist.

```xml
<Storyboard x:Name="myStoryboard">
    <!-- Animate the TranslateTransform's X property
        from 0 to 350, then 50,
        then 200 over 10 seconds. -->
    <DoubleAnimationUsingKeyFrames
        Storyboard.TargetName="MyAnimatedTranslateTransform"
        Storyboard.TargetProperty="X"
        Duration="0:0:10" EnableDependentAnimation="True">

        <!-- Using a LinearDoubleKeyFrame, the rectangle moves 
            steadily from its starting position to 500 over 
            the first 3 seconds.  -->
        <LinearDoubleKeyFrame Value="500" KeyTime="0:0:3"/>

        <!-- Using a DiscreteDoubleKeyFrame, the rectangle suddenly 
            appears at 400 after the fourth second of the animation. -->
        <DiscreteDoubleKeyFrame Value="400" KeyTime="0:0:4"/>

        <!-- Using a SplineDoubleKeyFrame, the rectangle moves 
            back to its starting point. The
            animation starts out slowly at first and then speeds up. 
            This KeyFrame ends after the 6th second. -->
        <SplineDoubleKeyFrame KeySpline="0.6,0.0 0.9,0.00" Value="0" KeyTime="0:0:6"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

### <a name="easing-key-frames"></a>Beschleunigungskeyframes

Ein Beschleunigungs schlüsselframe ist ein Keyframe, in dem die Interpolation angewendet wird, und die Funktion im Zeitverlauf der Interpolation wird von mehreren vordefinierten mathematischen Formeln gesteuert. Tatsächlich können Sie mit einem Spline-Keyframe, wie bei einigen der Beschleunigungs Funktionstypen, dasselbe Ergebnis liefern, aber es gibt auch einige Beschleunigungsfunktionen, wie z. b. [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase), die Sie nicht mit einem Spline reproduzieren können.

Um eine Beschleunigungsfunktion auf einen Beschleunigungskeyframe anzuwenden, legen Sie die **EasingFunction**-Eigenschaft als Eigenschaftselement in XAML für diesen Keyframe fest. Geben Sie als Wert ein Objektelement für einen der Beschleunigungsfunktionstypen an.

In diesem Beispiel werden eine [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase) und dann eine [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) als aufeinander folgende Keyframes auf eine [**DoubleAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) angewendet, um einen Sprungeffekt zu erzielen.

```xml
<Storyboard x:Name="myStoryboard">
    <DoubleAnimationUsingKeyFrames Duration="0:0:10"
        Storyboard.TargetProperty="Height"
        Storyboard.TargetName="myEllipse">

        <!-- This keyframe animates the ellipse up to the crest 
            where it slows down and stops. -->
        <EasingDoubleKeyFrame Value="-300" KeyTime="00:00:02">
            <EasingDoubleKeyFrame.EasingFunction>
                <CubicEase/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>

        <!-- This keyframe animates the ellipse back down and makes
            it bounce. -->
        <EasingDoubleKeyFrame Value="0" KeyTime="00:00:06">
            <EasingDoubleKeyFrame.EasingFunction>
                <BounceEase Bounces="5"/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Dies ist jedoch nur ein Beispiel für Beschleunigungsfunktionen. Weitere Beispiele werden im nächsten Abschnitt behandelt.

## <a name="easing-functions"></a>Beschleunigungsfunktionen

Mit Beschleunigungsfunktionen können Sie benutzerdefinierte mathematische Formeln auf Animationen anwenden. Mathematische Vorgänge eignen sich oft zum Erstellen von Animationen, die ein reales physikalisches Modell in einem 2 D-Koordinatensystem simulieren. Beispielsweise sollte Ihr Objekt realistisch springen oder sich so verhalten, als ob es sich auf einer Feder befinden würde. Sie können den Keyframe oder sogar **von** / **bis zu**den entsprechenden Effekten verwenden, / **By** aber es würde eine beträchtliche Menge an Arbeit beanspruchen, und die Animation wäre weniger genau, als eine mathematische Formel zu verwenden.

Beschleunigungsfunktionen können auf drei verschiedene Arten auf Animationen angewendet werden:

-   Verwendung eines Beschleunigungskeyframes in einer Keyframe-Animation, wie im vorhergehenden Abschnitt beschrieben. Verwenden Sie [**EasingColorKeyFrame. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingcolorkeyframe.easingfunction), [**EasingDoubleKeyFrame. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction)oder [**EasingPointKeyFrame. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingpointkeyframe.easingfunction).
-   Durch Festlegen der **EasingFunction** -Eigenschaft für eine von **von** / **auf** / **durch** Animations Typen. Verwenden Sie [**ColorAnimation.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.coloranimation.easingfunction), [**DoubleAnimation.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction) oder [**PointAnimation.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.pointanimation.easingfunction).
-   Durch Festlegen von [**GeneratedEasingFunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) als Teil einer [**VisualTransition**](/uwp/api/Windows.UI.Xaml.VisualTransition). Dies ist spezifisch für das Definieren von visuellen Zuständen für Steuerelemente. Weitere Informationen finden Sie unter [**GeneratedEasingFunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) oder [Storyboards für visuelle Zustände](/previous-versions/windows/apps/jj819808(v=win.10)).

Nachfolgend finden Sie eine Liste der Beschleunigungsfunktionen:

-   [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase): zieht die Bewegung einer Animation vor ihrer Ausführung in dem angegebenen Pfad etwas zurück.
-   [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase): erstellt einen Sprungeffekt.
-   [**CircleEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase): Erstellt eine Animation, die anhand einer Winkelfunktion beschleunigt oder verzögert wird.
-   [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase): Erstellt eine Animation, die anhand der Formel f(t) = t3 beschleunigt oder verzögert wird.
-   [**ElasticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ElasticEase): Erstellt eine Animation ähnlich einer Sprungfeder, die auf und ab federt und schließlich zur Ruhe kommt.
-   [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase): Erstellt eine Animation, die anhand einer Exponentialfunktion beschleunigt oder verzögert wird.
-   [**PowerEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase): Erstellt eine Animation, die anhand der Formel f(t) = tp beschleunigt oder verzögert wird, wobei p der Eigenschaft [**Power**](/uwp/api/windows.ui.xaml.media.animation.powerease.power) entspricht.
-   [**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase): Erstellt eine Animation, die anhand der Formel f(t) = t2 beschleunigt oder verzögert wird.
-   [**QuarticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuarticEase): Erstellt eine Animation, die anhand der Formel f(t) = t4 beschleunigt oder verzögert wird.
-   [**QuinticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuinticEase): Erstellt eine Animation, die anhand der Formel f(t) = t5 beschleunigt oder verzögert wird.
-   [**SineEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.SineEase): erstellt eine Animation, die anhand einer Sinusfunktion beschleunigt oder verzögert wird.

Einige der Beschleunigungsfunktionen verfügen über eigene Eigenschaften. Beispielsweise verfügt [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) über die beiden Eigenschaften [**Bounces**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounces) und [**Bounciness**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounciness), die das Funktion-über-Zeit-Verhalten dieser bestimmten **BounceEase** ändern. Andere Beschleunigungsfunktionen wie z. B. [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase) haben keine weiteren Eigenschaften außer der Eigenschaft [**EasingMode**](/uwp/api/windows.ui.xaml.media.animation.easingfunctionbase.easingmode), die von allen Beschleunigungsfunktionen gemeinsam verwendet wird, und erzeugen immer dasselbe Funktion-über-Zeit-Verhalten.

Einige dieser Beschleunigungsfunktionen überschneiden sich geringfügig, je nach Einstellung der Einstellung der Eigenschaften für die Beschleunigungsfunktionen, die über Eigenschaften verfügen. [**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase) ist z. B. identisch mit einer [**PowerEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase), wobei [**Power**](/uwp/api/windows.ui.xaml.media.animation.powerease.power) 2 entspricht. Und [**zircleease**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase) ist im Grunde genommen eine Standardwert [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase).

Die [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase) -Beschleunigungs Funktion ist eindeutig, da Sie den Wert außerhalb des normalen Bereichs ändern kann, der **von** / **auf** oder von den Werten der Keyframes festgelegt wird. Die Animation wird gestartet, indem der Wert in umgekehrter Richtung geändert wird, wie es von einem normalen **von** / **bis zum** Verhalten erwartet wird, wieder zum **from** -oder Start-Wert wechselt und die Animation dann wie gewohnt ausführt.

Das Deklarieren einer Beschleunigungsfunktion für eine Keyframe-Animation wurde in einem vorhergehenden Beispiel gezeigt. Im nächsten Beispiel wird eine Beschleunigungs Funktion von auf eine **von** / **bis zur** / **By** Animation angewendet.

```xml
<StackPanel x:Name="LayoutRoot" Background="White">
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation From="30" To="200" Duration="00:00:3" 
                Storyboard.TargetName="myRectangle" 
                Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <DoubleAnimation.EasingFunction>
                    <BounceEase Bounces="2" EasingMode="EaseOut" 
                                Bounciness="2"/>
                </DoubleAnimation.EasingFunction>
            </DoubleAnimation>
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle x:Name="myRectangle" Fill="Blue" Width="200" Height="30"/>
</StackPanel>
```

Wenn eine Beschleunigungs Funktion auf eine **from** / -**to** / -**by** -Animation angewendet wird, ändert Sie die Funktions Zeitmerkmale, wie der Wert zwischen den **from** -und-Werten **To** während der [**Dauer**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) der Animation interpoliert. Ohne Beschleunigungsfunktion läge eine lineare Interpolation vor.

## <a name="span-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>Diskrete Objektwert Animationen

Ein Animationstyp sollte besonders hervorgehoben werden, da er die einzige Methode darstellt, einen animierten Wert auf Eigenschaften von einem anderen Typ als [**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point) oder [**Color**](/uwp/api/Windows.UI.Color) anzuwenden. Hierbei handelt es sich um die Keyframe-Animation [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames). Es besteht ein Unterschied zum Animieren mithilfe von [**Object**](/dotnet/api/system.object)-Werten, da es keine Möglichkeit zur Interpolation der Werte zwischen den Frames gibt. Beim Erreichen der [**KeyTime**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.keytime) des Frames wird der animierte Wert sofort auf den Wert festgelegt, der im **Value** des Keyframes angegeben ist. Da keine Interpolation vorhanden ist, wird nur ein Keyframe in der **ObjectAnimationUsingKeyFrames-Keyframes** -Auflistung verwendet: [**diskreteobjectkeyframe**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame).

Der [**Value**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) eines [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) wird oft mithilfe der Eigenschaftselementsyntax festgelegt, da der festzulegende Objektwert häufig nicht als Zeichenfolge zum Ausfüllen des **Value** in einer Attributsyntax ausgedrückt werden kann. Sie können die Attribut Syntax weiterhin verwenden, wenn Sie einen Verweis wie [statikresource](../../xaml-platform/staticresource-markup-extension.md)verwenden.

Die Verwendung von [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) in den Standardvorlagen erfolgt bei einem Verweis auf eine [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Ressource durch eine Vorlageneigenschaft. Bei diesen Ressourcen handelt es sich um [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)-Objekte, nicht nur um einen [**Color**](/uwp/api/Windows.UI.Color)-Wert. Außerdem werden Ressourcen verwendet, die als Systemthemen ([**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)) definiert sind. Sie können direkt einem Wert mit **Brush**-Typ wie [**TextBlock.Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) zugeordnet werden; eine indirekte Zielauswahl ist nicht erforderlich. Da **SolidColorBrush** jedoch nicht [**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point) oder **Color** ist, müssen Sie **ObjectAnimationUsingKeyFrames** verwenden, um die Ressource zu nutzen.

```xml
<Style x:Key="TextButtonStyle" TargetType="Button">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid Background="Transparent">
                    <TextBlock x:Name="Text"
                        Text="{TemplateBinding Content}"/>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal"/>
                            <VisualState x:Name="PointerOver">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPointerOverForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="Pressed">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPressedForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
...
                       </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Sie haben auch die Möglichkeit, [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) zu verwenden, um Eigenschaften mit einem Enumerationswert zu animieren. Im Folgenden finden Sie ein weiteres Beispiel einer benannten Formatvorlage, die aus den Standardvorlagen der Windows-Runtime stammt. Beachten Sie die Festlegung der [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility)-Eigenschaft, die eine [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility)-Enumerationskonstante akzeptiert. In diesem Fall können Sie den Wert mithilfe der Attributsyntax festlegen. Sie benötigen lediglich den nicht qualifizierten Konstantennamen aus einer Enumeration zum Festlegen einer Eigenschaft mit einem Enumerationswert, z. B. „Collapsed“.

```xml
<Style x:Key="BackButtonStyle" TargetType="Button">
    <Setter Property="Template">
      <Setter.Value>
        <ControlTemplate TargetType="Button">
          <Grid x:Name="RootGrid">
            <VisualStateManager.VisualStateGroups>
              <VisualStateGroup x:Name="CommonStates">
              <VisualState x:Name="Normal"/>
...           <VisualState x:Name="Disabled">
                <Storyboard>
                  <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Visibility">
                    <DiscreteObjectKeyFrame Value="Collapsed" KeyTime="0"/>
                  </ObjectAnimationUsingKeyFrames>
                </Storyboard>
              </VisualState>
            </VisualStateGroup>
...
          </VisualStateManager.VisualStateGroups>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
  </Setter>
</Style>
```

Sie können mehrere [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame)-Elemente für einen [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)-Framesatz verwenden. Dies ist eine interessante Erstellungsmethode für eine Diashow-Animation durch Animieren des Werts von [**Image.Source**](/uwp/api/windows.ui.xaml.controls.image.source) und ein Beispiel für ein Szenario, in dem mehrere Objektwerte hilfreich sind.

 ## <a name="related-topics"></a>Zugehörige Themen

* [PropertyPath-Syntax](../../xaml-platform/property-path-syntax.md)
* [Übersicht über Abhängigkeitseigenschaften](../../xaml-platform/dependency-properties-overview.md)
* [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**Storyboard.TargetProperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)