---
title: Pull-to-Refresh mit Source-Modifiern
description: Erstellen Sie benutzerdefinierte Pull-to-Refresh-Steuerelemente mit SourceModifiern
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 834f631cd5c4b8696e75f83f194b95f809b1cf8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649865"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>Pull-to-Refresh mit Source-Modifiern

In diesem Artikel gehen wir näher auf die Verwendung des SourceModifier-Features von InteractionTracker ein und demonstrieren dessen Verwendung, indem wir ein benutzerdefiniertes Pull-to-Refresh-Steuerelement erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen hier davon aus, dass Sie mit den in diesen Artikeln behandelten Konzepten vertraut sind:

- [Eingabe-driven Animationen](input-driven-animations.md)
- [Benutzerdefinierte Bearbeitung Erfahrungen mit InteractionTracker](interaction-tracker-manipulations.md)
- [Beziehung basierend Animationen](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>Was sind SourceModifier und warum sind sie nützlich?

Wie [InertiaModifiers](inertia-modifiers.md) ermöglichen SourceModifiers eine feinere Kontrolle über die Bewegung eines InteractionTrackers. Im Gegensatz zu InertiaModifiers, die die Bewegung definieren, nachdem der InteractionTracker die Trägheit angewendet hat, definieren SourceModifiers die Bewegung, während sich der InteractionTracker noch im Zustand der Interaktion befindet. In diesen Fällen wünschen Sie sich ein anderes Erlebnis als das traditionelle "dem Finger folgen".

Ein klassisches Beispiel dafür ist das Pull-to-Refresh-Erlebnis: Wenn der Benutzer die Liste zieht, um den Inhalt und die Liste mit der Geschwindigkeit des Fingers zu aktualisieren und nach einer bestimmten Strecke stoppt, würde sich die Bewegung abrupt und mechanisch anfühlen. Ein natürlicheres Erlebnis wäre es, ein Gefühl des Widerstands zu erzeugen, während der Benutzer aktiv mit der Liste interagiert. Diese kleine Nuance trägt dazu bei, dass die Interaktion mit einer Liste dynamischer und ansprechender wird. Im Abschnitt „Beispiel“ gehen wir detaillierter darauf ein, wie man dies umsetzen kann.

Es gibt 2 SourceModifier-Typen:

- DeltaPosition – Das Delta zwischen der aktuellen Frame-Position und der vorherigen Frame-Position des Fingers während der Schwenken-Interaktion per Touch. Mit diesem Source-Modifier können Sie die Delta-Position der Interaktion verändern, bevor Sie sie zur weiteren Verarbeitung senden. Dies ist ein Parameter vom Typ Vector3 und der Entwickler kann wählen, ob er die X-, Y- oder Z-Attribute der Position ändern will, bevor er sie an den InteractionTracker übergibt.
- DeltaScale - Das Delta zwischen dem aktuellen Frame-Maßstab und dem vorherigen Frame-Maßstab, das während der Touch-Zoom-Interaktion angewendet wurde. Mit diesem Source-Modifier können Sie die Zoomstufe der Interaktion verändern. Dies ist ein Float-Attribut, das der Entwickler modifizieren kann, bevor er es an den InteractionTracker übergibt.

Wenn sich InteractionTracker im Zustand „Interacting“ befindet, wertet er jeden ihm zugeordneten Source-Modifier aus und stellt fest, ob einer von ihnen zutrifft. Das bedeutet, dass Sie mehrere Source-Modifier erstellen und einem InteractionTracker zuweisen können. Bei der Definition eines jeden müssen Sie jedoch Folgendes tun:

1. Definieren Sie die Bedingung – ein Ausdruck, der die bedingte Anweisung definiert, bei der dieser spezifische Source-Modifier angewendet werden soll.
1. Definieren Sie DeltaPosition/DeltaScale – Der Source-Modifier, der die DeltaPosition oder DeltaScale ändert, wenn die oben definierte Bedingung erfüllt ist.

## <a name="example"></a>Beispiel

Sehen wir uns nun an, wie Sie Source-Modifier verwenden können, um ein benutzerdefiniertes Pull-to-Refresh-Erlebnis mit einem bestehenden XAML ListView-Steuerelement zu erstellen. Wir werden ein Canvas als „Refresh-Panel“ verwenden, das auf einem XAML ListView gestapelt wird.

Für die Endbenutzererfahrung möchten wir den Effekt des „Widerstandes“ erzeugen, da der Benutzer die Liste aktiv (mit Berührung) schwenkt und das Schwenken stoppt, nachdem die Position einen bestimmten Punkt überschritten hat.

![Liste mit Pull-to-Refresh](images/animation/city-list.gif)

Den Arbeitscode für diese Erfahrung finden Sie im [Window UI Dev Labs-Repo auf GitHub](https://github.com/Microsoft/WindowsUIDevLabs). Hier ist die exemplarische Vorgehensweise zum Erstellen der Erfahrung.
In Ihrem XAML-Markup-Code haben Sie folgendes:

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

Da es sich bei dem ListView (`ThumbnailList`) um ein XAML-Steuerelement handelt, das bereits scrollt, müssen Sie das Scrolling mit dem übergeordneten Element (`ContentPanel`) verknüpfen, wenn es das oberste Element erreicht und nicht mehr scrollen kann. ("ContentPanel" ist, in dem Sie die Modifizierer für die Quelle angewendet wird.) Um dies zu ermöglichen, müssen Sie ScrollViewer.IsVerticalScrollChainingEnabled festlegen, um **"true"** im ListView-Markup. Außerdem müssen Sie den Verkettungsmodus in VisualInteractionSource auf **Always** setzen.

Sie müssen den PointerPressedEvent-Handler mit dem _handlingEventsToo_-Parameter auf **true** setzen. Ohne diese Option wird PointerPressedEvent nicht mit ContentPanel verkettet, da das ListView-Steuerelement diese Ereignisse als behandelt markiert und sie nicht über die visuelle Verkettung gesendet werden.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

Jetzt sind Sie bereit, dies mit InteractionTracker zu verknüpfen. Beginnen Sie mit der Einrichtung von InteractionTracker, VisualInteractionSource und dem Ausdruck, der die Position von InteractionTracker nutzt.

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

Bei dieser Einstellung befindet sich das Refresh-Panel außerhalb des Ansichtsfensters in seiner Ausgangsposition und alles was der Benutzer sieht ist das ListView. Wenn das Schwenken das ContentPanel erreicht, wird das PointerPressed-Ereignis ausgelöst, bei dem Sie das System auffordern, InteractionTracker zu verwenden, um das Manipulationserlebnis zu steuern.

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> Wenn das Verketten von behandelten Ereignissen nicht benötigt wird, kann der PointerPressedEvent-Handler mit dem Attribut (`PointerPressed="Window_PointerPressed"`) direkt über das XAML-Markup hinzugefügt werden.

Der nächste Schritt ist das Einrichten der Source-Modifier. Sie werden zwei Source-Modifier verwenden, um dieses Verhalten zu erreichen: _Resistance_ und _Stop_.

- Resistance – Bewegen Sie die DeltaPosition Y mit halber Geschwindigkeit, bis sie die Höhe des RefreshPanels erreicht.

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- Stop – Stoppen Sie die Bewegung, nachdem das gesamte RefreshPanel auf dem Bildschirm angezeigt wird.

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

Dieses Diagramm zeigt eine Visualisierung des SourceModifiers-Setups.

![Schwenken-Diagramm](images/animation/source-modifiers-diagram.png)

Mit den SourceModifiers werden Sie beim Schwenken der ListView nach unten und dem Erreichen des obersten Elemente bemerken, dass das Refresh-Panel mit der halben Geschwindigkeit des Schwenkens heruntergezogen wird, bis es die Höhe von RefreshPanels erreicht. Dann stoppt die Bewegung.

Im vollständigen Beispiel wird eine Keyframe-Animation verwendet, um ein Symbol während der Interaktion im RefreshPanel zu drehen. Jeder Inhalt kann stattdessen werden oder die Position von InteractionTracker nutzen, um diese Animation separat zu steuern.
