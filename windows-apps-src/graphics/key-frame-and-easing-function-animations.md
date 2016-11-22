---
author: Jwmsft
title: "Keyframeanimationen und Animationen für Beschleunigungsfunktionen"
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: "Lineare Keyframe-Animationen, Keyframe-Animationen mit einem „KeySpline“-Wert und Beschleunigungsfunktionen sind drei verschiedene Techniken für etwa das gleiche Szenario."
translationtype: Human Translation
ms.sourcegitcommit: 7b4676e5c5a66450b321ab6f5f8670f9491b7a9d
ms.openlocfilehash: 163109a8e87c0d270eeeed825958af7ec51ee336

---
# Keyframe-Animationen und Animationen für Beschleunigungsfunktionen

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Lineare Keyframe-Animationen, Keyframe-Animationen mit einem **KeySpline**-Wert oder Beschleunigungsfunktionen sind drei verschiedene Techniken für ungefähr dasselbe Szenario: Erstellen einer komplexeren Storyboard-Animation, die ein nichtlineares Animationsverhalten ab einem bestimmten Startzustand bis zu einem Endzustand verwendet.

## Voraussetzungen

Lesen Sie vorher unbedingt das Thema [Storyboardanimationen](storyboarded-animations.md). Dieses Thema basiert auf den Animationskonzepten, die in [Storyboardanimationen](storyboarded-animations.md) erläutert wurden, und behandelt diese nicht erneut. In [Storyboard-Animationen](storyboarded-animations.md) wird beispielsweise das Anwenden von Animationen auf Ziele, das Verwenden von Storyboards als Ressourcen, die [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517)-Eigenschaftswerte wie z.B. [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration), [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior) usw. erläutert.

## Animieren mit Keyframe-Animationen

Keyframe-Animationen lassen mehr als einen Zielwert zu, der an einem gewissen Punkt auf der Animationszeitachse erreicht wird. Anders ausgedrückt kann jeder Keyframe einen anderen Zwischenwert angeben, und der letzte erreichte Keyframe ist der endgültige Animationswert. Durch Angabe mehrerer Animationswerte können Sie komplexere Animationen erstellen. Keyframe-Animationen ermöglichen auch verschiedene Interpolationslogiken, die pro Animationstyp als unterschiedliche **KeyFrame**-Unterklassen implementiert werden. Insbesondere hat jeder Keyframe-Animationstyp eine **Discrete**-, **Linear**-, **Spline**- und **Easing**-Variation seiner **KeyFrame**-Klasse für die Angabe der Keyframes. Zum Festlegen einer Animation mit Keyframes für [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) können Sie z.B. Keyframes mit [**DiscreteDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243130), [**LinearDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210316), [**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446) und [**EasingDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210269) deklarieren. Sie können all diese Typen beliebig in einer einzelnen **KeyFrames**-Sammlung verwenden, um die Interpolation bei jedem Erreichen eines neuen Keyframes zu ändern.

Für das Interpolationsverhalten steuert jeder Keyframe die Interpolation, bis die **KeyTime**-Zeit erreicht ist. **Value** wird zum selben Zeitpunkt erreicht. Folgen weitere Keyframes, wird der Wert anschließend zum Startwert für den nächsten Keyframe in einer Sequenz.

Wenn beim Start der Animation kein Keyframe mit der **KeyTime** 0:0:0 vorhanden ist, ist der nicht animierte Wert der Eigenschaft der Startwert. Dies ist mit dem Verhalten einer **From**/**To**/**By**-Animation vergleichbar, wenn keine Angabe für **From** vorhanden ist.

Die Dauer einer Keyframe-Animation entspricht implizit der Dauer des höchsten **KeyTime**-Werts, der in einem ihrer Keyframes festgelegt ist. Sie können eine explizite [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) festlegen. Achten Sie dabei jedoch darauf, dass sie nicht kürzer als eine **KeyTime** in ihren eigenen Keyframes ist, andernfalls wird ein Teil der Animation abgeschnitten.

Neben der [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) können Sie alle [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517)-basierten Eigenschaften in einer Keyframe-Animation festlegen (genau wie bei einer **From**/**To**/**By**-Animation) da die Keyframe-Animationsklassen ebenfalls von der **Timeline** abgeleitet werden. Dies sind:

-   [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.autoreverse): Sobald der letzte Keyframe erreicht ist, werden die Frames vom anderen Ende ausgehend in umgekehrter Reihenfolge wiederholt. So wird die scheinbare Dauer der Animation verdoppelt.
-   [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.begintime): Verzögert den Start der Animation. Die Zeitachse für die **KeyTime**-Werte in den Frames beginnt erst nach Erreichen der **BeginTime** mit der Zählung, damit kein Risiko besteht, dass Frames abgeschnitten werden.
-   [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior): Steuert die Ereignisse nach dem Erreichen des letzten Keyframes. **FillBehavior** hat keine Auswirkungen auf Zwischenkeyframes.
-   [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty):
    -   Bei Festlegung auf **Forever** werden die Keyframes und die dazugehörige Zeitachse unendlich wiederholt.
    -   Bei Festlegung auf eine bestimmte Iterationsanzahl wird die Zeitachse entsprechend oft wiederholt.
    -   Bei Festlegung auf eine [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) wird die Zeitachse bis zum Erreichen dieser Zeit wiederholt. Dadurch wird die Animation u.U. mitten in der Keyframe-Sequenz abgeschnitten, wenn sie keinen ganzzahligen Teiler der impliziten Dauer der Zeitachse darstellt.
-   [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.speedratioproperty) (allgemein nicht verwendet)

### Lineare Keyframes

Lineare Keyframes führen zu einer einfachen linearen Interpolation des Werts, bis die **KeyTime** des Frames erreicht ist. Dieses Interpolationsverhalten gleicht am ehesten den einfacheren **From**/**To**/**By**-Animationen, die im Thema [Storyboard-Animationen](storyboarded-animations.md) beschrieben werden.

Im Folgenden erfahren Sie, wie die Renderhöhe eines Rechtecks mithilfe von linearen Keyframes skaliert wird. In diesem Beispiel wird eine Animation ausgeführt, bei der die Höhe des Rechtecks in den ersten vierSekunden leicht und linear ansteigt und in der letzten Sekunde schnell skaliert wird, bis das Rechteck im Vergleich zum Start die doppelte Höhe erreicht hat.

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

### Diskrete Keyframes

Diskrete Keyframes verwenden überhaupt keine Interpolation. Beim Erreichen einer **KeyTime** wird einfach der neue **Value** angewendet. Je nach der animierten UI-Eigenschaft führt dies häufig dazu, dass die Animation zu „springen“ scheint. Stellen Sie sicher, dass dieses ästhetische Verhalten gewünscht ist. Sie können die scheinbaren Sprünge reduzieren, indem Sie die Anzahl der deklarierten Keyframes erhöhen. Möchten Sie jedoch eine flüssige Animation erzielen, sollten Sie stattdessen lineare Keyframes oder Spline-Keyframes verwenden.

**Hinweis**  Diskrete Keyframes stellen die einzige Möglichkeit dar, einen Wert mit [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) zu animieren, der nicht den Typ [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) und [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) hat. Dies wird später in diesem Thema ausführlich erläutert.

 

### Spline-Keyframes

Ein Spline-Keyframe erstellt einen nicht linearen Übergang zwischen Werten entsprechend dem Wert für die Eigenschaft **KeySpline**. Diese Eigenschaft gibt den ersten und zweiten Kontrollpunkt einer Bézierkurve an, mit der die Beschleunigung einer Animation beschrieben wird. Grundsätzlich definiert eine [**KeySpline**](https://msdn.microsoft.com/library/windows/apps/BR210307) eine Beziehung der Funktion zur Zeit, wobei der Funktion-Zeit-Graph die Form dieser Bézierkurve hat. In der Regel geben Sie einen **KeySpline**-Wert in einer XAML-Kompaktattribut-Zeichenfolge an, der vierdurch Leerzeichen oder Kommata getrennte [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)-Werte umfasst. Diese Werte sind „X,Y“-Paare für zweiKontrollpunkte der Bézierkurve. „X“ ist die Zeit, und „Y“ ist der Funktionsmodifizierer für den Wert. Jeder Wert sollte sich immer zwischen 0 und einschließlich 1 bewegen. Ohne Kontrollpunktänderung an einer **KeySpline** stellt die gerade Linie von 0,0 bis 1,1 eine Funktion der Zeit für eine lineare Interpolation dar. Ihre Kontrollpunkte ändern die Form der Kurve und somit das Verhalten der Funktion der Zeit für die Spline-Animation. Dies wird am besten visuell als Graph dargestellt. Sie können das Beispiel für die [Silverlight Keyspline-Schnellansicht](http://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample) in einem Browser ausführen, um zu sehen, wie die Kontrollpunkte die Kurve ändern, und wie eine Beispielanimation ausgeführt wird, wenn sie als **KeySpline**-Wert verwendet wird.

Im nächsten Beispiel wird die Anwendung von drei verschiedenen Keyframes auf eine Animation gezeigt, wobei der letzte eine Keyspline-Animation für einen [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)-Wert ([**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446)) ist. Beachten Sie die Anwendung der Zeichenfolge „0.6,0.0 0.9,0.00“ für **KeySpline**. Dadurch entsteht eine Kurve, bei der die Animation zuerst scheinbar langsam ausgeführt wird, dann jedoch schnell den Wert erreicht, bevor die **KeyTime** erreicht ist.

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

### Beschleunigungskeyframes

Ein Beschleunigungskeyframe ist ein Keyframe, bei dem Interpolation angewendet wird und die Funktion der Zeit der Interpolation von mehreren vordefinierten mathematischen Formeln gesteuert wird. Mit einem Spline-Keyframe können Sie nahezu dasselbe Ergebnis erzielen wie mit einigen der Beschleunigungsfunktionstypen. Einige Beschleunigungsfunktionen, wie z.B. [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049), können jedoch nicht mit einem Spline reproduziert werden.

Um eine Beschleunigungsfunktion auf einen Beschleunigungskeyframe anzuwenden, legen Sie die **EasingFunction**-Eigenschaft als Eigenschaftselement in XAML für diesen Keyframe fest. Geben Sie als Wert ein Objektelement für einen der Beschleunigungsfunktionstypen an.

In diesem Beispiel werden eine [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126) und dann eine [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) als aufeinander folgende Keyframes auf eine [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) angewendet, um einen Sprungeffekt zu erzielen.

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

        !-- This keyframe animates the ellipse back down and makes
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

## Beschleunigungsfunktionen

Mithilfe von Beschleunigungsfunktionen können Sie benutzerdefinierte mathematische Formeln auf Animationen anwenden. Mathematische Vorgänge eignen sich oft zum Erstellen von Animationen, die ein reales physikalisches Modell in einem 2D-Koordinatensystem simulieren. Beispielsweise soll ein Objekt mit einer natürlich wirkenden Bewegung springen oder federn. Diese Effekte können Sie näherungsweise mit Keyframes oder sogar **From**/**To**/**By**-Animationen erzielen, aber dieses Verfahren ist sehr aufwändig, und die Animationen sind dann trotzdem weniger präzise als bei Verwendung einer mathematischen Formel.

Beschleunigungsfunktionen können auf dreiverschiedene Arten auf Animationen angewendet werden:

-   Verwendung eines Beschleunigungskeyframes in einer Keyframe-Animation, wie im vorhergehenden Abschnitt beschrieben. Verwenden Sie [**EasingColorKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210267), [**EasingDoubleKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction.aspx) oder [**EasingPointKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210279).
-   Durch Festlegen der Eigenschaft **EasingFunction** für einen der **From**/**To**/**By**-Animationstypen. Verwenden Sie [**ColorAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR243075), [**DoubleAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) oder [**PointAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210354).
-   Durch Festlegen von [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) als Teil einer [**VisualTransition**](https://msdn.microsoft.com/library/windows/apps/BR209034). Diese Methode wird nur zum Definieren von visuellen Zuständen für Steuerelemente verwendet. Weitere Informationen dazu finden Sie unter [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) oder [Storyboards für visuelle Zustände](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

Nachfolgend finden Sie eine Liste der Beschleunigungsfunktionen:

-   [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049): Zieht die Bewegung einer Animation vor ihrer Ausführung in dem angegebenen Pfad etwas zurück.
-   [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057): Erstellt einen Sprungeffekt.
-   [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063): Erstellt eine Animation, die anhand einer Winkelfunktion beschleunigt oder verzögert wird.
-   [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126): Erstellt eine Animation, die anhand der Formel f(t) = t3 beschleunigt oder verzögert wird.
-   [**ElasticEase**](https://msdn.microsoft.com/library/windows/apps/BR210282): Erstellt eine Animation ähnlich einer Sprungfeder, die auf und ab federt und schließlich zur Ruhe kommt.
-   [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294): Erstellt eine Animation, die anhand einer Exponentialfunktion beschleunigt oder verzögert wird.
-   [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399): Erstellt eine Animation, die anhand der Formel f(t) = tp beschleunigt oder verzögert wird, wobei p der Eigenschaft [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power) entspricht.
-   [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403): Erstellt eine Animation, die anhand der Formel f(t) = t2 beschleunigt oder verzögert wird.
-   [**QuarticEase**](https://msdn.microsoft.com/library/windows/apps/BR210405): Erstellt eine Animation, die anhand der Formel f(t) = t4 beschleunigt oder verzögert wird.
-   [**QuinticEase**](https://msdn.microsoft.com/library/windows/apps/BR210407): Erstellt eine Animation, die anhand der Formel f(t) = t5 beschleunigt oder verzögert wird.
-   [**SineEase**](https://msdn.microsoft.com/library/windows/apps/BR210439): Erstellt eine Animation, die anhand einer Sinusfunktion beschleunigt oder verzögert wird.

Einige der Beschleunigungsfunktionen verfügen über eigene Eigenschaften. Beispielsweise verfügt [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) über die beiden Eigenschaften [**Bounces**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounces.aspx) und [**Bounciness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounciness.aspx), die das Funktion-über-Zeit-Verhalten dieser bestimmten **BounceEase** ändern. Andere Beschleunigungsfunktionen wie z.B. [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126) haben keine weiteren Eigenschaften außer der Eigenschaft [**EasingMode**](https://msdn.microsoft.com/library/windows/apps/BR210275), die von allen Beschleunigungsfunktionen gemeinsam verwendet wird, und erzeugen immer dasselbe Funktion-über-Zeit-Verhalten.

Einige dieser Beschleunigungsfunktionen überschneiden sich geringfügig, je nach Einstellung der Einstellung der Eigenschaften für die Beschleunigungsfunktionen, die über Eigenschaften verfügen. [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403) ist z.B. identisch mit einer [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399), wobei [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power) 2 entspricht. Und [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063) ist im Grunde eine [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294) mit Standardwert.

Die Beschleunigungsfunktion [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049) ist einzigartig, da sie den Wert außerhalb des normalen Bereichs ändern kann, der durch **From**/**To** oder durch Keyframe-Werte festgelegt wurde. Die Animation wird gestartet, indem der Wert in der umgekehrten Richtung geändert wird, als von einem normalen **From**/**To**-Verhalten erwartet wird. Anschließend beginnt die Funktion wieder bei **From** oder dem Startwert und führt die Animation normal aus.

Das Deklarieren einer Beschleunigungsfunktion für eine Keyframe-Animation wurde in einem vorhergehenden Beispiel gezeigt. Im nächsten Beispiel wird eine Beschleunigungsfunktion auf eine **From**/**To**/**By**-Animation angewendet.

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

Wenn eine Beschleunigungsfunktion auf eine **From**/**To**/**By**-Animation angewendet wird, ändern sie die Funktion-über-Zeit-Merkmale der Interpolation des Werts zwischen den Werten **From** und **To** im Lauf der [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) der Animation. Ohne Beschleunigungsfunktion läge eine lineare Interpolation vor.

## <span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>Animationen mit diskreten Objektwerten

Ein Animationstyp sollte besonders hervorgehoben werden, da er die einzige Methode darstellt, einen animierten Wert auf Eigenschaften von einem anderen Typ als [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) oder [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) anzuwenden. Hierbei handelt es sich um die Keyframe-Animation [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320). Es besteht ein Unterschied zum Animieren mithilfe von [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)-Werten, da es keine Möglichkeit zur Interpolation der Werte zwischen den Frames gibt. Beim Erreichen der [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR210342) des Frames wird der animierte Wert sofort auf den Wert festgelegt, der im **Value** des Keyframes angegeben ist. Da keine Interpolation vorhanden ist, wird nur ein Keyframe in der Keyframe Collection **ObjectAnimationUsingKeyFrames** verwendet: [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132).

Der [**Value**](https://msdn.microsoft.com/library/windows/apps/BR210344) eines [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) wird oft mithilfe der Eigenschaftselementsyntax festgelegt, da der festzulegende Objektwert häufig nicht als Zeichenfolge zum Ausfüllen des **Value** in einer Attributsyntax ausgedrückt werden kann. Wenn Sie einen Verweis wie z.B. [StaticResource](https://msdn.microsoft.com/library/windows/apps/Mt185588) verwenden, können Sie die Attributsyntax dennoch verwenden.

Die Verwendung von [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) in den Standardvorlagen erfolgt bei einem Verweis auf eine [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)-Ressource durch eine Vorlageneigenschaft. Bei diesen Ressourcen handelt es sich um [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)-Objekte, nicht nur um einen [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)-Wert. Außerdem werden Ressourcen verwendet, die als Systemthemen ([**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807)) definiert sind. Sie können direkt einem Wert mit **Brush**-Typ wie [**TextBlock.Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) zugeordnet werden; eine indirekte Zielauswahl ist nicht erforderlich. Da **SolidColorBrush** jedoch nicht [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) oder **Color** ist, müssen Sie **ObjectAnimationUsingKeyFrames** verwenden, um die Ressource zu nutzen.

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

Sie haben auch die Möglichkeit, [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) zu verwenden, um Eigenschaften mit einem Enumerationswert zu animieren. Im Folgenden finden Sie ein weiteres Beispiel einer benannten Formatvorlage, die aus den Standardvorlagen der Windows-Runtime stammt. Beachten Sie die Festlegung der [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)-Eigenschaft, die eine [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR209006)-Enumerationskonstante akzeptiert. In diesem Fall können Sie den Wert mithilfe der Attributsyntax festlegen. Sie benötigen lediglich den nicht qualifizierten Konstantennamen aus einer Enumeration zum Festlegen einer Eigenschaft mit einem Enumerationswert, z.B. „Collapsed“.

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

Sie können mehrere [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132)-Elemente für einen [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)-Framesatz verwenden. Dies ist eine interessante Erstellungsmethode für eine Diashow-Animation durch Animieren des Werts von [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760) und ein Beispiel für ein Szenario, in dem mehrere Objektwerte hilfreich sind.

 ## Verwandte Themen

* [Eigenschaftspfadsyntax](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [Übersicht über Abhängigkeitseigenschaften](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)


<!--HONumber=Nov16_HO1-->


