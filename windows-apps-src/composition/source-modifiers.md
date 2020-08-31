---
title: Pull-to-refresh mit quellmodifizierervorgängen
description: Erfahren Sie, wie Sie mit der SourceModifier-Funktion eines interaktiontracker ein benutzerdefiniertes Pull-to-refresh-Steuerelement erstellen.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: b20b4b22d1de2252864287b97bedc4a1fc176602
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053960"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>Pull-to-refresh mit quellmodifizierervorgängen

In diesem Artikel erfahren Sie, wie Sie das Feature "SourceModifier" eines interaktiontracker verwenden und dessen Verwendung veranschaulichen, indem Sie ein benutzerdefiniertes Pull-to-refresh-Steuerelement erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie mit den Konzepten vertraut sind, die in den folgenden Artikeln erläutert werden:

- [Eingabegesteuerte Animationen](input-driven-animations.md)
- [Benutzerdefinierte Manipulations Erfahrung mit interaktiontracker](interaction-tracker-manipulations.md)
- [Beziehungs basierte Animationen](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>Was ist ein SourceModifier, und warum sind Sie nützlich?

Wie " [inertiamodifiers](inertia-modifiers.md)" erhalten Sie mit sourcemodifizierern eine präzisere Kontrolle über die Bewegung einer interaktiontracker. Aber im Gegensatz zu inertiamodifiers, die die Bewegung definieren, nachdem interaktiontracker in Trägheit gelangt ist, definieren sourcemodifizierer die Bewegung, während sich interaktiontracker noch im Interaktionszustand befindet. In diesen Fällen benötigen Sie eine andere Möglichkeit als die herkömmliche "Peitsche an den Finger".

Ein klassisches Beispiel hierfür ist die Pull-to-refresh-Benutzeroberfläche. wenn der Benutzer die Liste durchläuft, um den Inhalt zu aktualisieren, und die Liste der Auflistungen mit der gleichen Geschwindigkeit wie der Finger. Eine natürlichere Erfahrung besteht darin, ein Gefühl für den Widerstand zu bringen, während der Benutzer aktiv mit der Liste interagiert. Diese kleine Nuance trägt dazu bei, dass die Interaktion mit einer Liste dynamischer und ansprechender wird. Im Beispiel Abschnitt wird ausführlicher erläutert, wie Sie dies erstellen.

Es gibt zwei Typen von quellmodifizierertypen:

- Deltaposition – ist das Delta zwischen der aktuellen Frame Position und der vorherigen Rahmen Position des Fingers während der Touch Pan-Interaktion. Dieser quellmodifizierer ermöglicht es Ihnen, die Delta Position der Interaktion vor dem Senden zur weiteren Verarbeitung zu ändern. Dabei handelt es sich um einen Vector3-Typparameter, und der Entwickler kann die X-oder Y-oder Z-Attribute der Position ändern, bevor er an die interaktiontracker übergeben wird.
- Deltascale-ist das Delta zwischen der aktuellen Frame Skala und der vorherigen Frame Skala, die während der Interaktion mit dem Fingerabdruck angewendet wurde. Mit diesem quellmodifizierer können Sie den Zoom Grad der Interaktion ändern. Dabei handelt es sich um ein Attribut vom Typ float, das der Entwickler ändern kann, bevor es an den interaktiontracker übergeben wird.

Wenn sich interaktiontracker im Interaktionszustand befindet, wertet es jeden der zugewiesenen quellmodifier aus und bestimmt, ob eine dieser Elemente zutrifft. Dies bedeutet, dass Sie mehrere quellmodifiziererer erstellen und einer interaktionstracker zuweisen können. Wenn Sie die einzelnen definieren, müssen Sie die folgenden Schritte ausführen:

1. Definieren Sie die Bedingung – ein Ausdruck, der die bedingte Anweisung definiert, wenn dieser spezifische quellmodifizierer angewendet werden soll.
1. Definieren Sie die Delta Position/Delta Position/Delta Position – den quellmodifiziererausdruck, der die Delta Position oder Delta Position ändert, wenn die oben definierte Bedingung erfüllt ist.

## <a name="example"></a>Beispiel

Sehen wir uns nun an, wie Sie quellmodifizierers verwenden können, um eine benutzerdefinierte Pull-to-Refresh-Funktion mit einem vorhandenen XAML-ListView-Steuerelement zu erstellen. Wir verwenden eine Canvas als "Aktualisierungs Panel", die auf einer XAML-ListView gestapelt wird, um diese Oberfläche zu erstellen.

Für den Endbenutzer ist es sinnvoll, die Auswirkung von "Widerstand" zu erstellen, da der Benutzer die Liste (mit Fingerabdruck) aktiv durchläuft und die schwenken beendet, nachdem die Position einen bestimmten Punkt überschreitet.

![Liste mit Pull-to-refresh](images/animation/city-list.gif)

Den funktionierenden Code für diese Erfahrung finden Sie im Repository " [Windows UI dev Labs" auf GitHub](https://github.com/microsoft/WindowsCompositionSamples). Hier finden Sie Schritt-für-Schritt-Anleitungen zum Aufbau dieser Vorgehensweise.
In Ihrem XAML-Markup Code haben Sie Folgendes:

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

Da ListView ( `ThumbnailList` ) ein XAML-Steuerelement ist, das bereits einen Bildlauf ausführt, muss der Bildlauf mit dem übergeordneten Element () verkettet werden, `ContentPanel` Wenn es das oberste Element erreicht und keinen Bildlauf mehr durchführen kann. (In ContentPanel werden die quellmodifizierer angewendet.) Hierfür müssen Sie ScrollViewer. isverticalscrollchainingenabled im ListView-Markup auf " **true** " festlegen. Sie müssen auch den Verkettungs Modus für "visualinteraktionsource" auf " **Always**" festlegen.

Sie müssen den pointerpressedevent-Handler mit dem Parameter " _shanddeventstoo_ " als " **true**" festlegen. Ohne diese Option wird das pointerpressedevent-Element nicht mit dem ContentPanel verkettet, da das ListView-Steuerelement diese Ereignisse als behandelt kennzeichnet und nicht in der visuellen Kette gesendet wird.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

Nun können Sie das mit interaktiontracker verknüpfen. Beginnen Sie mit dem Einrichten von interaktiontracker, der visualinteraktionsource und dem Ausdruck, der die Position von interaktiontracker nutzt.

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

Bei dieser Einrichtung befindet sich der Bereich "Aktualisieren" außerhalb des Viewports an der Anfangsposition, und alle Benutzer sehen die ListView, wenn das Schwenken den ContentPanel erreicht. das pointerpressed-Ereignis wird ausgelöst, wobei Sie das System zum Verwenden von Interaction Tracker zum Steuern der Manipulations Umgebung auffordern.

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
> Wenn die Verkettung behandelter Ereignisse nicht erforderlich ist, kann das Hinzufügen des pointerpressedevent-Handlers mithilfe des-Attributs () direkt über das XAML-Markup erfolgen `PointerPressed="Window_PointerPressed"` .

Der nächste Schritt besteht im Einrichten der quellmodifizierer. Sie verwenden 2 quellmodifizierer, um dieses Verhalten zu erhalten. _Widerstand_ und _Beendigung_.

- Resistance – verschiebt die Delta Position. Y mit der Hälfte der Geschwindigkeit, bis die Höhe des aktualenden Bereichs erreicht ist.

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

- Beendigung – fahren Sie fort, nachdem sich der gesamte Fensterbereich auf dem Bildschirm befindet.

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

Dieses Diagramm bietet eine Visualisierung der SourceModifier-Einrichtung.

![Schwenken des Diagramms](images/animation/source-modifiers-diagram.png)

Mit den sourcemodifiziererelementen werden Sie feststellen, dass beim Schwenken der ListView nach unten und beim Erreichen des obersten Elements das Aktualisierungs Panel mit der Hälfte der Zeitangabe nach unten gezogen wird, bis die Höhe des Aktualisierungs Bereichs erreicht ist. Anschließend wird der Verschiebe Vorgang beendet.

Im vollständigen Beispiel wird eine Keyframe-Animation verwendet, um ein Symbol während der Interaktion im Bereich "erfrischendes Panel" zu drehen. Alle Inhalte können an ihrer Stelle verwendet werden oder die Position von interaktiontracker verwenden, um diese Animation separat zu steuern.
