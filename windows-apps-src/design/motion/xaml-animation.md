---
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
title: Übersicht über Animationen
description: Verwenden Sie die Animationen aus der Windows-Runtime-Animationsbibliothek, um das Windows-Erscheinungsbild in Ihre App zu integrieren.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d6fa8740fba35ae6a50d91846bab79a4858de076
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970485"
---
# <a name="animations-in-xaml"></a>Animationen in XAML

Animationen können Ihre APP durch Hinzufügen von Bewegung und Interaktivität verbessern. Durch Verwendung der Animationen aus der Windows-Runtime-Animationsbibliothek können Sie das Windows-Erscheinungsbild in Ihre App integrieren. Dieses Thema enthält eine Zusammenfassung der Animationen und Beispiele für typische Szenarien, in denen diese Animationen verwendet werden.

> [!TIP]
> Die Windows-Runtime-Steuerelemente für XAML enthalten bestimmte Arten von Animationen als integrierte Verhalten, die aus einer Animations Bibliothek stammen. Wenn Sie diese Steuerelemente in einer App verwenden, erreichen Sie das Erscheinungsbild von Animationen, ohne solche selbst programmieren zu müssen.

Animationen von der Windows-Runtime-Animationsbibliothek bieten folgende Vorteile:

-   Bewegungen, die sich an [Animationsprinzipien](https://docs.microsoft.com/windows/uwp/style/motion) anpassen.
-   Schnelle, flüssige Übergänge zwischen UI-Zuständen, die den Benutzer informieren, aber nicht ablenken.
-   Visuelles Verhalten, das dem Benutzer innerhalb einer App Übergänge anzeigt

Wenn der Benutzer beispielsweise einer Liste ein Element hinzufügt und das neue Element nicht unmittelbar in der Liste angezeigt wird, wird das neue Element entsprechend animiert. Die anderen Elemente werden innerhalb kurzer Zeit an ihren neuen Positionen animiert und machen Platz für das hinzugefügte Element. Das Übergangsverhalten macht die Steuerelementinteraktion für den Benutzer offensichtlicher.

In der Windows 10-Version 1607 wird eine neue [**ConnectedAnimationService**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)-API für die Implementierung von Animationen eingeführt, bei denen während einer Navigation ein Element erscheint, um den Wechsel zwischen Ansichten zu animieren. Das Verwendungsmuster dieser API unterscheidet sich vom Verwendungsmuster anderer Animationsbibliothek-APIs. Die Verwendung von **ConnectedAnimationService** wird auf der [Referenzseite](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) behandelt.

Die Animationsbibliothek stellt keine Animationen für jedes mögliche Szenario bereit. Es gibt Fälle, in denen Sie es vielleicht vorziehen würden, eine benutzerdefinierte Animation in XAML zu erstellen. Weitere Informationen finden Sie unter [Storyboardanimationen](storyboarded-animations.md).

Außerdem möchten Entwickler für bestimmte erweiterte Szenarien wie das Animieren eines Artikels basierend auf der Bildlaufposition eines ScrollViewer möglicherweise Interoperabilität mit visuellen Ebenen verwenden, um benutzerdefinierte Animationen zu implementieren. Weitere Informationen finden Sie unter [Visuelle Ebene](https://docs.microsoft.com/windows/uwp/composition/visual-layer).

## <a name="types-of-animations"></a>Arten der Animation

Das Windows-Runtime-Animationssystem und die Animationsbibliothek können mit dem Ziel verwendet werden, Steuerelemente und andere Elemente der Benutzeroberfläche zu aktivieren, um ein animiertes Verhalten zu erreichen. Es sind verschiedene Animationsarten verfügbar.

-   *Designübergänge* werden automatisch angewendet, wenn sich bestimmte Bedingungen in der UI ändern, einschließlich Steuerelemente oder Elemente aus den vordefinierten Windows-Runtime-XAML-UI-Typen. Diese werden als *Designübergänge* bezeichnet, da diese Animationen das Erscheinungsbild von Windows unterstützen und definieren, was die Apps bei bestimmten UI-Szenarien ausführen, wenn sie von einem Interaktionsmodus in einen anderen wechseln. Die Designübergänge sind Teil der Animationsbibliothek.
-   *Designanimationen* sind Animationen für eine oder mehrere Eigenschaften vordefinierter Windows-Runtime-XAML-UI-Typen. Designanimationen weichen von Designübergängen ab, da Designanimationen auf ein bestimmtes Element abzielen und in spezifischen visuellen Zuständen innerhalb eines Steuerelements vorhanden sind. Wohingegen Designübergänge Eigenschaften des Steuerelements zugewiesen sind, die sich außerhalb der visuellen Zustände befinden und sich auf die Übergänge zwischen diesen Zuständen auswirken. Viele der Windows-Runtime-XAML-Steuerelemente enthalten Designanimationen innerhalb von Storyboards, die Teil der Steuerelementvorlage sind, wobei die Animationen von visuellen Zuständen ausgelöst werden. So lange die Vorlagen also nicht geändert werden, sind die integrierten Designanimationen in Ihrer UI verfügbar. Ersetzen Sie jedoch die Vorlagen, ersetzen Sie ebenfalls die integrierten Steuerelement-Designanimationen. Um sie zurückzuerhalten, definieren Sie ein Storyboard mit Designanimationen innerhalb des Satzes visueller Zustände des Steuerelements. Sie können auch Designanimationen von Storyboards ausführen, die sich nicht innerhalb visueller Zustände befinden, und mit der [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin)-Methode starten. Das ist jedoch keine gängige Vorgehensweise. Die Designanimationen sind Teil der Animationsbibliothek.
-   *Visuelle Übergänge* werden angewendet, wenn ein Steuerelement von einem seiner definierten visuellen Zustände in einen anderen Zustand übergeht. Es handelt sich um benutzerdefinierte Animationen, die Sie schreiben, und sind normalerweise mit der benutzerdefinierten Vorlage, die Sie für ein Steuerelement schreiben, und den Definitionen für visuelle Zustände in dieser Vorlage verknüpft. Die Animation wird nur in der Zeit zwischen den Zuständen ausgeführt. Dies ist i. d. R. eine kurze Zeitdauer von höchstens einigen Sekunden. Weitere Informationen finden Sie im Abschnitt ["Visuelle Übergänge" der Storyboardanimationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).
-   *Storyboardanimationen* animieren den Wert einer Windows-Runtime-Abhängigkeitseigenschaft mit der Zeit. Storyboards können als Teil eines visuellen Übergangs definiert oder zur Laufzeit von der Anwendung ausgelöst werden. Weitere Informationen finden Sie unter [Storyboardanimationen](storyboarded-animations.md). Weitere Informationen zu Abhängigkeitseigenschaften und ihren Vorkommen finden Sie unter [Übersicht über Abhängigkeitseigenschaften](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview).
-   *Verbundene Animationen* werden von der neuen [**ConnectedAnimationService**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)-API bereitgestellt und ermöglichen Entwicklern die einfache Erstellung eines Effekts, bei dem während einer Navigation ein Element erscheint, um den Wechsel zwischen Ansichten zu animieren. Diese API steht ab der Windows 10-Version 1607 zur Verfügung. Weitere Informationen finden Sie unter [**ConnectedAnimationService**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice).

## <a name="animations-available-in-the-library"></a>In der Bibliothek verfügbare Animationen

Die folgenden Animationen werden in der Animationsbibliothek bereitgestellt. Klicken Sie auf den Namen einer Animation, um mehr über die Hauptnutzungsszenarios und ihre Definition zu erfahren und eine Beispielanimation anzuzeigen.

-   [Seitenübergang](#page-transition): Animiert Seitenübergänge in einem [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame).
-   [Inhalts- und Eingangsübergang](#content-transition-and-entrance-transition): blendet eines oder mehrere Inhaltselemente per Animation ein oder aus.
-   [Ein-, Aus- und Überblenden](#fade-in-out-and-crossfade): Zeigt vorübergehende Elemente oder Steuerelemente an oder aktualisiert einen Inhaltsbereich.
-   [Zeiger hoch/runter](#pointer-up-down): Gibt visuelles Feedback beim Tippen oder Klicken auf eine Kachel.
-   [Ändern der Position](#reposition): Verschiebt ein Element an eine neue Position.
-   [Popup einblenden/ausblenden](#show-hide-popup): Zeigt Kontext-UI oben in der Ansicht an.
-   [Rand-UI einblenden/ausblenden](#show-hide-edge-ui): Verschiebt UI am Bildschirmrand (auch große UI, z. B. ein Panel) in die oder aus der Ansicht.
-   [Listenelementänderungen](#list-item-changes): Fügt einer Liste Elemente hinzu, entfernt sie oder ordnet sie neu an.
-   [Drag & Drop](#drag-drop): gibt visuelles Feedback während eines Drag & Drop-Vorgangs.

### <a name="page-transition"></a>Seitenübergang

Verwenden Sie Seitenübergänge zum Animieren von Navigation innerhalb einer App. Da nahezu alle Apps irgendeine Art von Navigation verwenden, sind Animationen für Seitenübergänge der am häufigsten von Apps verwendete Designanimationstyp. Weitere Informationen zu Seitenübergangs-APIs finden Sie unter [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition).



### <a name="content-transition-and-entrance-transition"></a>Inhalts- und Eingangsübergang

Verwenden Sie Inhaltsübergangsanimationen ([**ContentThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition)), um Inhalte in die oder aus der aktuelle(n) Ansicht zu verschieben. Die Inhaltsübergangsanimationen werden z. B. verwendet, um Inhalt anzuzeigen, der beim ersten Laden der Seite noch nicht anzeigebereit war, oder wenn der Inhalt eines Seitenabschnitts geändert wird.

[**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) steht für eine Bewegung, die auf Inhalte angewendet werden kann, wenn eine Seite oder ein großer Teil der UI zum ersten Mal geladen wird. Daher kann der erste Eindruck eines Inhalts anderes Feedback vermitteln als eine Inhaltsänderung. [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) entspricht einer [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) mit den Standardparametern, kann aber außerhalb eines [**Frames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) verwendet werden.
 
 
<span id="fade-in-out-and-crossfade"/>

### <a name="fade-inout-and-crossfade"></a>Ein-, Aus- und Überblenden

Verwenden Sie die Ein-und Ausblendungsanimationen, um vorübergehend angezeigte Benutzeroberflächen oder Steuerelemente ein- bzw. auszublenden. In XAML werden diese als [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) und [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) dargestellt. Ein Beispiel ist eine App-Leiste, auf der aufgrund einer Benutzerinteraktion neue Steuerelemente angezeigt werden können. Ein anderes Beispiel ist eine vorübergehend angezeigte Bildlaufleiste oder Schwenkmarkierung, die ausgeblendet wird, wenn eine Zeit lang keine Benutzereingabe erkannt wurde. Apps sollten die Einblendungsanimation auch für den Übergang zwischen einem Platzhalterelement und dem endgültigen Element verwenden, wenn Inhalte dynamisch geladen werden.

Verwenden Sie eine Überblendungsanimation, um einen weichen Übergang zu erreichen, wenn sich der Zustand eines Elements ändert, z. B. wenn die aktuellen Inhalte einer Ansicht aktualisiert werden. Die XAML-Animationsbibliothek stellt keine dedizierte Überblendungsanimation (keine Entsprechung für [**crossFade**](https://docs.microsoft.com/previous-versions/windows/apps/br212661(v=win.10))) bereit, aber Sie können dasselbe Ergebnis durch Verwendung von [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) und [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) mit überlappendem Timing erzielen.

<span id="pointer-up-down"/>

### <a name="pointer-updown"></a>Zeiger hoch/runter

Verwenden Sie die Animationen [**PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) und [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation), um dem Benutzer Feedback bei einem erfolgreichen Tippen oder Klicken auf eine Kachel zu geben. Wenn ein Benutzer beispielsweise auf eine Kachel klickt oder tippt, wird die Animation für Zeiger runter wiedergegeben. Sobald das Klicken oder Tippen beendet wird, wird die Animation für Zeiger hoch wiedergegeben.

### <a name="reposition"></a>Ändern der Position

Verwenden Sie die Animationen für das Ändern der Position ([**RepositionThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) oder [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition)), um eines oder mehrere Elemente an eine neue Position zu verschieben. Das Verschieben der Kopfzeilen in einem Elementsteuerelement verwendet beispielsweise die Animation für das Ändern der Position.

<span id="show-hide-popup"/>

### <a name="showhide-popup"></a>Popup einblenden/ausblenden

Verwenden Sie [**PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) und [**PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation), wenn Sie ein [**Popup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) oder ähnliche kontextabhängige UI oben in der aktuellen Ansicht ein- und ausblenden möchten. [**PopupThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) ist ein Designübergang, der hilfreiches Feedback bietet, wenn Sie ein Popup einfach ausblenden möchten.

<span id="show-hide-edge-ui"/>

### <a name="showhide-edge-ui"></a>Rand-UI einblenden/ausblenden

Verwenden Sie die [**EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)-Animationen, um kleine UI-Elemente am Bildschirmrand in die und aus der Ansicht zu verschieben. Verwenden Sie diese Animationen zum Beispiel, wenn Sie eine benutzerdefinierte App-Leiste oben oder unten auf dem Bildschirm oder ein Benutzeroberfläche für Fehler und Warnungen oben auf dem Bildschirm anzeigen.

Verwenden Sie die [**PaneThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)-Animation, um einen Bereich oder ein Panel ein- oder auszublenden. Dies ist für große randbasierte UI vorgesehen, z. B. eine benutzerdefinierte Tastatur oder ein Aufgabenbereich.

### <a name="list-item-changes"></a>Listenelementänderungen

Verwenden Sie die [**AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition)-Animation, um dem Hinzufügen oder Löschen von Listenelementen animiertes Verhalten hinzuzufügen. Beim Hinzufügen ordnet der Übergang zunächst vorhandene Elemente in der Liste neu an, um Platz für die neuen Elemente zu schaffen, und fügt dann die neuen Elemente hinzu. Beim Löschen entfernt der Übergang Elemente aus einer Liste und ordnet die verbleibenden Listenelemente bei Bedarf neu an, wenn die gelöschten Elemente entfernt wurden.

Es besteht zudem eine separate [**ReorderThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition), die zum Ändern der Position in einer Liste verwendet wird. Diese wird anders als die Löschung eines Elements und das Hinzufügen des Elements an einer neuen Position mit den zugeordneten Animationen für das Löschen/Hinzufügen animiert.

Beachten Sie, dass diese Animationen in den standardmäßigen [**ListView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)- und [**GridView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview)-Vorlagen enthalten sind, sodass Sie diese Animationen nicht manuell hinzufügen müssen, wenn Sie diese Steuerelemente bereits verwenden.

<span id="drag-drop"/>

### <a name="dragdrop"></a>Drag & Drop

Verwenden Sie die Ziehanimation ([**DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation), [**DragOverThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation)) und die Ablegeanimation ([**DropTargetItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation)), um visuelles Feedback zu geben, wenn der Benutzer ein Element zieht oder ablegt.

Wenn aktiv, zeigen die Animationen dem Benutzer an, dass die Liste um ein abgelegtes Element neu angeordnet werden kann. Für Benutzer ist es hilfreich, zu wissen, wo das Element in einer Liste platziert wird, wenn es am aktuellen Ort abgelegt wird. Die Animationen geben visuelles Feedback dazu, dass ein gezogenes Element zwischen zwei anderen Elementen in der Liste abgelegt werden kann und dass diese Elemente den Platz freigegeben.

## <a name="using-animations-with-custom-controls"></a>Verwenden von Animationen mit benutzerdefinierten Steuerelementen

In der folgenden Tabelle werden die Animationen zusammengefasst, die zum Erstellen einer benutzerdefinierten Version dieser Windows-Runtime-Steuerelemente verwendet werden sollten:

| UI-Typ | Empfohlene Animation |
|---------|-----------------------|
| Dialogfeld | [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) und [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| Flyout | [**PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) und [**PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| QuickInfo | [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) und [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| Kontextmenü | [**PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) und [**PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Befehlsleiste | [**EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| Aufgabenbereich oder randbasiertes Panel | [**PaneThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| Inhalt eines UI-Containers | [**ContentThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| Für Steuerelemente oder wenn keine andere Animation zutrifft | [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation) und [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |

 

## <a name="transition-animation-examples"></a>Beispiele zu Übergangsanimationen

Im Optimalfall verwendet Ihre App Animationen, um die Benutzeroberfläche interessanter und ansprechender zu gestalten, ohne dabei die Benutzer zu irritieren. Das erreichen Sie unter anderem mit animierten UI-Übergängen: Beim Ein- und Ausblenden von Elementen und anderen Änderungen lenkt die Animation die Aufmerksamkeit des Benutzers auf den betreffenden Aspekt. Beispielsweise können Sie Schaltflächen mit einer schnellen Bewegung ein- oder ausblenden, statt diese einfach nur hinzuzufügen oder zu entfernen. Wir haben eine Reihe von APIs für empfohlene und typische Animationsübergänge entwickelt, die sich nahtlos einfügen. Dieses Beispiel zeigt, wie Sie eine Animation auf eine Schaltfläche anwenden, damit diese schnell in die Ansicht gezogen wird.

```xml
<Button Content="Transitioning Button">
     <Button.Transitions>
         <TransitionCollection> 
             <EntranceThemeTransition/>
         </TransitionCollection>
     </Button.Transitions>
 </Button>
 ```

In diesem Codebeispiel haben wir der Übergangssammlung der Schaltfläche das [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition)-Objekt hinzugefügt. Beim ersten Rendern der Schaltfläche wird diese dann mit einer schnellen Bewegung in die Ansicht gezogen und nicht einfach nur der Anzeige hinzugefügt. Sie können für das Animationsobjekt bestimmte Eigenschaften festlegen, um einzustellen, wie weit und aus welcher Richtung das Objekt gezogen wird. Die API ist jedoch einfach gehalten und für ein konkretes Szenario gedacht: einen interessanten Eingang.

In den Stilressourcen der App können Sie außerdem Übergangsanimationsdesigns definieren und den gewünschten Effekt auf diese Weise einheitlich anwenden. Dieses Beispiel deckt sich mit dem vorhergehenden Beispiel, mit dem Unterschied, dass hier ein [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) verwendet wird:

```xml
<UserControl.Resources>
     <Style x:Key="DefaultButtonStyle" TargetType="Button">
         <Setter Property="Transitions">
             <Setter.Value>
                 <TransitionCollection>
                     <EntranceThemeTransition/>
                 </TransitionCollection>
             </Setter.Value>
        </Setter>
    </Style>
</UserControl.Resources>
      
<StackPanel x:Name="LayoutRoot">
    <Button Style="{StaticResource DefaultButtonStyle}" Content="Transitioning Button"/>
</StackPanel>
```

Die Beispiele oben wenden einen Designübergang auf ein einzelnes Steuerelement an. Sie steigern die Wirkung von Designübergängen jedoch weiter, wenn Sie diese auf einen Objektcontainer anwenden. Dabei wird der Übergang auf alle untergeordneten Objekte in dem betreffenden Container angewendet. In dem folgenden Beispiel wird eine [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition)-Animation auf ein [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) von Rechtecken angewendet.

```xml
<!-- If you set an EntranceThemeTransition animation on a panel, the
     children of the panel will automatically offset when they animate
     into view to create a visually appealing entrance. -->        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
            <EntranceThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- The sequence children appear depends on their order in 
         the panel's children, not necessarily on where they render
         on the screen. Be sure to arrange your child elements in
         the order you want them to transition into view. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

Die untergeordneten Rechtecke des [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) werden visuell ansprechend nacheinander eingeblendet – und nicht alle auf einmal, wie es bei der einzelnen Anwendung der Animation auf jedes Rechteck der Fall wäre.

Im Folgenden finden Sie eine Demonstration dieser Animation:

![Animation, in der das untergeordnete Rechteck eingeblendet wird](./images/animation-child-rectangles.gif)

Untergeordnete Objekte in einem Container können außerdem mit einer fließenden Bewegung neu angeordnet werden, wenn sich die Position eines oder mehrerer untergeordneter Objekte ändert. In dem folgenden Beispiel wird eine [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition)-Animation auf ein Rechteckraster angewendet. Wird eines der Rechtecke entfernt, bewegen sich die anderen Rechtecke an ihre neue Position.

```xml
<Button Content="Remove Rectangle" Click="RemoveButton_Click"/>
        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
                    
            <!-- Without this, there would be no animation when items 
                 are removed. -->
            <RepositionThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- All these rectangles are just to demonstrate how the items
         in the grid re-flow into position when one of the child items
         are removed. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

```cs
private void RemoveButton_Click(object sender, RoutedEventArgs e)
{
    if (rectangleItems.Items.Count > 0)
    {    
        rectangleItems.Items.RemoveAt(0);
    }                         
}
```

```cpp
// .h
private:
void RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);

//.cpp
void BlankPage::RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    if (rectangleItems->Items->Size > 0)
    {    
        rectangleItems->Items->RemoveAt(0);
    }
}
```

Sie können auf ein einzelnes Objekt oder einen Objektcontainer mehrere Übergangsanimationen anwenden. Wenn Sie beispielsweise die Rechteckliste mit einer Animation einblenden und außerdem Positionsänderungen animieren möchten, können Sie [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) und [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) wie folgt anwenden:

```xml
...
<ItemsControl.ItemContainerTransitions>
    <TransitionCollection>
        <EntranceThemeTransition/>                    
        <RepositionThemeTransition/>
    </TransitionCollection>
</ItemsControl.ItemContainerTransitions>
...      
```

Es stehen verschiedene Übergangseffekte zur Verfügung, um UI-Elemente beim Hinzufügen, Entfernen, Neuanordnen usw. zu animieren. Die Namen dieser APIs enthalten alle „ThemeTransition“:

| API | BESCHREIBUNG |
|-----|-------------|
| [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) | Stellt eine Windows-Charakteranimation für die Seitennavigation in einem [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) bereit. |
| [**AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) | Stellt animiertes Übergangsverhalten für das Hinzufügen oder Löschen von untergeordneten Objekten oder Inhalten über Steuerelemente bereit. Das betreffende Steuerelement ist dabei typischerweise ein Elementcontainer. |
| [**ContentThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition) | Stellt animiertes Übergangsverhalten für Änderungen der Inhalte eines Steuerelements bereit. Sie können diese Animation zusammen mit [**AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) anwenden. |
| [**EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition) | Stellt das animierte Übergangsverhalten für (kleine) Rand-UI-Übergänge bereit. |
| [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) | Stellt das animierte Übergangsverhalten für die erste Anzeige eines Steuerelements bereit. |
| [**PaneThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition) | Stellt das animierte Übergangsverhalten für Bereich-UI-Übergänge (große Rand-UI) bereit. |
| [**PopupThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) | Stellt das animierte Übergangsverhalten bereit, das für Einblendkomponenten von Steuerelementen (z. B. QuickInfo-UI eines Objekts) gilt, wenn diese eingeblendet werden. |
| [**ReorderThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition) | Stellt animiertes Übergangsverhalten für Änderungen in der Anordnung der Elemente von Listenansicht-Steuerelementen bereit. Dies ist typischerweise die Folge von Drag & Drop-Vorgängen. Die Animationsmerkmale können sich je nach Steuerelement und Design unterscheiden. |
| [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) | Stellt animiertes Übergangsverhalten für Positionsänderungen von Steuerelementen bereit. |

 

## <a name="theme-animation-examples"></a>Beispiele zu Designanimationen

Die Anwendung von Übergangsanimationen ist denkbar einfach. Aber vielleicht möchten Sie Zeit und Abfolge der Animationseffekte genauer steuern. Auch Designanimationen verwenden ein einheitliches Design für das Animationsverhalten, umfassen jedoch mehr Steuerungsmöglichkeiten. Für Designanimationen ist weniger Markup erforderlich als für benutzerdefinierte Animationen. Hier blenden wir mit [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) ein Rechteck aus.

```xml
<StackPanel>    
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <FadeOutThemeAnimation TargetName="myRectangle" />
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle PointerPressed="Rectangle_Tapped" x:Name="myRectangle"  
              Fill="Blue" Width="200" Height="300"/>
</StackPanel>
```

```cs
// When the user taps the rectangle, the animation begins.
private void Rectangle_Tapped(object sender, PointerRoutedEventArgs e)
{
    myStoryboard.Begin();
}
```

```vb
' When the user taps the rectangle, the animation begins.
Private Sub Rectangle_Tapped(sender As Object, e As PointerRoutedEventArgs)
    myStoryboard.Begin()
End Sub
```

```cpp
//.h
void Rectangle_Tapped(Platform::Object^ sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs^ e);

//.cpp
void BlankPage::Rectangle_Tapped(Object^ sender, PointerRoutedEventArgs^ e)
{
    myStoryboard->Begin();
}
```

Anders als Übergangsanimationen besitzt eine Designanimation keinen integrierten Trigger (der Übergang), der sie automatisch ausführt. Verwenden Sie ein [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard), um eine Designanimation bei Definition in XAML zu integrieren. Außerdem können Sie das Standardverhalten der Animation ändern. Sie können z. B. die Geschwindigkeit beim Ausblenden verringern, indem Sie den [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)-Zeitwert für [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) erhöhen.

**Hinweis**  zur Veranschaulichung grundlegender Animationstechniken verwenden wir app-Code zum Starten der Animation durch Aufrufen von [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)-Methoden. Sie können mit den **Storyboard**-Animationen [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin), [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop), [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause) und [**Resume** steuern, wie die ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.resume) **Storyboard**-Methoden ausgeführt werden. Bibliotheksanimationen werden so jedoch normalerweise nicht in Apps integriert. Integrieren Sie stattdessen Bibliotheksanimationen in die XAML-Stile und -Vorlagen für Steuerelemente oder Elemente. Vorlagen und Ansichtszustände sind etwas komplexer. Die Verwendung von Bibliotheksanimationen in Ansichtszuständen wird jedoch im Thema [Storyboardanimationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)) behandelt.

 

Sie können noch eine Reihe weiterer Designanimationen auf UI-Elemente anwenden und so Animationseffekte erreichen. Die Namen dieser APIs enthalten alle „ThemeAnimation“:

| API | BESCHREIBUNG |
|-----|-------------|
| [**DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation) | Stellt die vorkonfigurierte Animation dar, die auf gezogene Elemente angewendet wird. |
| [**DragOverThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation) | Stellt die vorkonfigurierte Animation dar, die auf Elemente unter gezogenen Elementen angewendet wird. |
| [**DropTargetItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation) | Vorkonfigurierte Animation, die auf mögliche Drop-Ziel-Elemente angewendet wird. |
| [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) | Vorkonfigurierte Deckkraftanimation, die beim ersten Einblenden von Steuerelementen angewendet wird. |
| [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) | Vorkonfigurierte Deckkraftanimation, die beim Ausblenden oder Entfernen von Steuerelementen aus der UI angewendet wird. |
| [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) | Vorkonfigurierte Animation für Benutzeraktionen, bei denen auf ein Element getippt oder geklickt wird. |
| [**PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) | Vorkonfigurierte Animation für eine Benutzeraktion, die nach dem Tippen auf ein Element und Auslösen der Aktion ausgeführt wird. |
| [**PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) | Vorkonfigurierte Animation, die auf Einblendkomponenten von Steuerelementen beim Anzeigen angewendet wird. In dieser Animation sind Deckkraft und Translation kombiniert. |
| [**PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation) | Vorkonfigurierte Animation, die auf Einblendkomponenten von Steuerelementen beim Schließen oder Entfernen angewendet wird. In dieser Animation sind Deckkraft und Translation kombiniert. |
| [**RepositionThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) | Vorkonfigurierte Animation für die Neupositionierung eines Objekts. |
| [**SplitCloseThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.SplitCloseThemeAnimation) | Die vorkonfigurierte Animation, die eine Zielbenutzeroberfläche mithilfe einer Animation im Format einer [**ComboBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) verbirgt, die geöffnet und geschlossen wird. |
| [**SplitOpenThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.SplitOpenThemeAnimation) | Die vorkonfigurierte Animation, die eine Zielbenutzeroberfläche mithilfe einer Animation im Format einer [**ComboBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) anzeigt, die geöffnet und geschlossen wird. |
| [**DrillInThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation) | Stellt eine vorkonfigurierte Animation bereit, die ausgeführt wird, wenn ein Benutzer in einer logischen Hierarchie in Vorwärtsrichtung navigiert, wie von einer Gestaltungsvorlage zu einer Detailseite. |
| [**DrillOutThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drilloutthemeanimation) | Stellt eine vorkonfigurierte Animation bereit, die ausgeführt wird, wenn ein Benutzer in einer logischen Hierarchie in Rückwärtsrichtung navigiert, wie von einer Detailseite zu einer Gestaltungsvorlage. |

 

## <a name="create-your-own-animations"></a>Erstellen eigener Animationen

Wenn Designanimationen für Ihre Zwecke nicht ausreichen, können Sie eigene Animationen erstellen. Sie animieren Objekte, indem Sie mindestens einen ihrer Eigenschaftenwerte animieren. Beispielsweise können Sie die Breite eines Rechtecks, den Winkel einer [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) oder den Farbwert einer Schaltfläche animieren. Wir bezeichnen diese Art der benutzerdefinierten Animation als Storyboardanimation, um sie von den Bibliotheksanimationen zu unterscheiden, die von der Windows-Runtime bereits als vorkonfigurierter Animationstyp bereitgestellt werden. Für Storyboardanimationen verwenden Sie eine Animation, die Werte eines bestimmten Typs ändern kann (beispielsweise [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation), zum Animieren eines **Double**-Elements), und platzieren die Animation in ein [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard), um sie zu steuern.

Damit eine Eigenschaft animiert werden kann, muss es sich um eine *Abhängigkeitseigenschaft* handeln. Weitere Informationen zu Abhängigkeitseigenschaften finden Sie unter [Übersicht über Abhängigkeitseigenschaften](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview). Weitere Informationen zum Erstellen von benutzerdefinierten Storyboardanimationen, u. a. zum Ausrichten und Steuern der Animationen, finden Sie unter [Storyboardanimationen](storyboarded-animations.md).

Der größte Bereich einer App-UI-Definition in XAML, in dem benutzerdefinierte Storyboardanimationen definiert werden, ist das Definieren visueller Zustände für Steuerelemente in XAML. Dies erfolgt entweder beim Erstellen einer neuen Steuerelementklasse oder beim erneuten Erstellen einer Vorlage für ein vorhandenes Steuerelement, das visuelle Zustände in der Steuerelementvorlage aufweist. Weitere Informationen finden Sie unter [Storyboarding-Animationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

 

 




