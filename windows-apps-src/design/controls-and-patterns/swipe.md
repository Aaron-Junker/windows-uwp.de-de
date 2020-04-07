---
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
Description: Bei wischgestenbasierten Befehlen handelt es sich um Beschleuniger für die Toucheingabe für Kontextmenüs.
title: Swipe
label: Swipe
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 315edbddccc51b7e742bf9beffad8497a104ce03
ms.sourcegitcommit: 8be8ed1ef4e496055193924cd8cea2038d2b1525
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614093"
---
# <a name="swipe"></a>Swipe

Wischgestenbasierte Befehle sind ein Beschleuniger für Kontextmenüs. Benutzer können damit per Toucheingabe leicht häufig verwendete Aktionen ausführen, ohne Zustände innerhalb der App ändern zu müssen.

> **Windows-UI-Bibliotheks-APIs:** [SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem)
>
> **Plattform-APIs:** [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [ListView class](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![Helles Design für Ausführen und Einblenden](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Wischgestenbasierte Befehle sparen Platz. Sie eignen sich in Situationen, in denen der gleiche Vorgang für mehrere Elemente mehrmals hintereinander schnell wiederholt wird. Darüber hinaus ermöglichen sie „schnelle Aktionen“ für Elemente, für die kein vollständiges Popup oder keine Zustandsänderung auf der Seite erforderlich ist.

Wischgestenbasierte Befehle sollten bei einer großen Gruppe von Elementen verwendet werden, die jeweils ein bis drei Aktionen enthalten, die ein Benutzer möglicherweise regelmäßig ausführen möchte. Zu diesen Aktionen zählen u. a. folgende:

- Löschen
- Markieren oder Archivieren
- Speichern oder Herunterladen
- Antworten

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn du die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert hast, klicke hier, um <a href="xamlcontrolsgallery:/item/SwipeControl">die App zu öffnen und SwipeControl in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Videozusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>Wie funktionieren Wischgesten?

Für wischgestenbasierte UWP-Befehle gibt es zwei Modi: [Einblenden](/uwp/api/windows.ui.xaml.controls.swipemode) und [Ausführen](/uwp/api/windows.ui.xaml.controls.swipemode). Außerdem werden vier verschiedene Wischrichtungen unterstützt: nach oben, nach unten, nach links und nach rechts.

### <a name="reveal-mode"></a>Einblendmodus

Im Modus „Einblenden“ wischt der Benutzer über ein Element, um ein Menü mit einem oder mehreren Befehlen zu öffnen. Er muss explizit auf einen Befehl tippen, um diesen auszuführen. Wenn der Benutzer über ein Element wischt und es loslässt, bleibt das Menü geöffnet, bis der Benutzer einen Befehl auswählt oder das Menü schließt, indem er in die entgegengesetzte Richtung wischt, es durch eine Tippgeste schließt oder das geöffnete Element vom Bildschirm wischt.

![Wischen zum Einblenden](images/SwipeCommand-Reveal_v2.gif)

Der Einblendmodus ist ein sicherer, flexiblerer Wischmodus und kann für die meisten Menüaktionen (auch möglicherweise destruktive Aktionen wie Löschen) verwendet werden.

Wählt der Benutzer eine der Menüoptionen aus, die im geöffneten und statischen Zustand des Einblendmodus angezeigt werden, wird der Befehl für dieses Element aufgerufen und das Steuerelement zum Wischen geschlossen.

### <a name="execute-mode"></a>Ausführungsmodus

Im Ausführungsmodus wischt der Benutzer über ein Element, um mit dieser Wischgeste einen einzelnen Befehl einzublenden und auszuführen. Lässt der Benutzer das entsprechende Element los, bevor ein bestimmter Schwellenwert erreicht wurde, wird das Menü geschlossen, und der Befehl wird nicht ausgeführt. Wenn der Benutzer wischt, bis ein bestimmter Schwellenwert erreicht wurde und das Element dann loslässt, wird der Befehl sofort ausgeführt.

![Wischen zum Ausführen](images/SwipeCommand_Delete_v2.gif)

Wenn der Benutzer die Finger nach dem Erreichen des Schwellenwerts nicht anhebt und das gewünschte Element wieder geschlossen wird, wird weder der Befehl noch eine Aktion für das Element ausgeführt.

Der Ausführungsmodus gibt beim Wischen über ein Element mithilfe von Farben und Beschriftungsausrichtung visuelles Feedback.

Der Ausführungsmodus ist besonders hilfreich, wenn der Benutzer ihn für eine häufig ausgeführte Aktion verwendet.

Er kann auch für destruktivere Aktionen wie das Löschen eines Elements verwendet werden. Beachte jedoch, dass für das Ausführen nur eine Wischaktion in eine Richtung erforderlich ist, während der Benutzer für das Einblenden explizit auf eine Schaltfläche klicken muss.

### <a name="swipe-directions"></a>Wischrichtungen

Es kann in alle Richtungen gewischt werden: nach oben, nach unten, nach links und nach rechts. Jede Wischrichtung kann eigene Wischelemente oder -inhalte enthalten, für ein einzelnes wischbares Element kann allerdings jeweils nur eine Richtung festgelegt werden.

Beispielsweise sind nicht zwei [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems)-Definitionen für dasselbe SwipeControl-Element möglich.

## <a name="how-to-create-a-swipe-command"></a>Erstellen eines wischgestenbasierten Befehls

Für wischgestenbasierte Befehle müssen zwei Komponenten definiert werden:

- Die Komponente [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), die deine Inhalte umschließt. In einer Sammlung (etwa vom Typ „ListView“) befindet sich diese innerhalb von DataTemplate.
- Die Elemente des Menüs „Wischen“. Dabei handelt es sich um mindestens ein [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem)-Objekt, das in direktionalen Containern des Steuerelements „Wischen“ platziert ist: [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems) oder [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems).

Inhalte zum Wischen können inline oder im Ressourcenabschnitt deiner Seite oder App platziert werden.

Nachfolgend siehst du ein Beispiel für ein SwipeControl-Element, das Text umschließt. Es zeigt die Hierarchie der XAML-Elemente, die zum Erstellen eines wischgestenbasierten Befehls erforderlich sind.

```xaml
<SwipeControl HorizontalAlignment="Center" VerticalAlignment="Center">
    <SwipeControl.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Pin">
                <SwipeItem.IconSource>
                    <SymbolIconSource Symbol="Pin"/>
                </SwipeItem.IconSource>
            </SwipeItem>
        </SwipeItems>
    </SwipeControl.LeftItems>

     <!-- Swipeable content -->
    <Border Width="180" Height="44" BorderBrush="Black" BorderThickness="2">
        <TextBlock Text="Swipe to Pin" Margin="4,8,0,0"/>
    </Border>
</SwipeControl>
```

Nun befassen wir uns mit einem umfassenderen Beispiel dazu, wie wischgestenbasierte Befehle normalerweise in einer Liste verwendet werden. In diesem Beispiel richtest du einen Löschbefehl ein, der den Ausführungsmodus verwendet, sowie ein Menü mit anderen Befehlen, das den Einblendmodus verwendet. Beide Befehlsgruppen werden im Ressourcenabschnitt der Seite definiert. Du wendest die wischgestenbasierten Befehle auf die Elemente in ListView an.

Erstelle zunächst die Wischelemente, die die Befehle darstellen, als Ressourcen auf Seitenebene. SwipeItem verwendet [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) als Symbol. Erstelle die Symbole ebenfalls als Ressourcen.

```xaml
<Page.Resources>
    <SymbolIconSource x:Key="ReplyIcon" Symbol="MailReply"/>
    <SymbolIconSource x:Key="DeleteIcon" Symbol="Delete"/>
    <SymbolIconSource x:Key="PinIcon" Symbol="Pin"/>

    <SwipeItems x:Key="RevealOptions" Mode="Reveal">
        <SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"/>
        <SwipeItem Text="Pin" IconSource="{StaticResource PinIcon}"/>
    </SwipeItems>

    <SwipeItems x:Key="ExecuteDelete" Mode="Execute">
        <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
                   Background="Red"/>
    </SwipeItems>
</Page.Resources>
```

Denke daran, für die Menüelemente in deinen Wischinhalten kurze und präzise Beschriftungen zu verwenden. Bei diesen Aktionen sollte es sich um die primären Aktionen handeln, die ein Benutzer möglicherweise mehrmals innerhalb eines kurzen Zeitraums ausführen möchte.

Beim Einrichten eines wischgestenbasierten Befehls für die Verwendung in einer Sammlung oder Listenansicht (ListView) gehst du genau so vor wie beim Definieren eines einzelnen wischgestenbasierten Befehls (wie oben gezeigt). Die einzige Ausnahme besteht darin, dass das SwipeControl-Element in DataTemplate definiert wird, damit es auf jedes Element in der Sammlung angewendet wird.

Nachfolgend siehst du eine Listenansicht (ListView) mit dem SwipeControl-Element, das auf das DataTemplate-Element angewendet wird. Die Eigenschaften „LeftItems“ und „RightItems“ verweisen auf die Wischelemente, die du als Ressourcen erstellt hast.

```xaml
<ListView x:Name="sampleList" Width="300">
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
        </Style>
    </ListView.ItemContainerStyle>
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <SwipeControl x:Name="ListViewSwipeContainer"
                          LeftItems="{StaticResource RevealOptions}"
                          RightItems="{StaticResource ExecuteDelete}"
                          Height="60">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="{x:Bind}" FontSize="18"/>
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit..." FontSize="12"/>
                    </StackPanel>
                </StackPanel>
            </SwipeControl>
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

## <a name="handle-an-invoked-swipe-command"></a>Behandeln eines aufgerufenen wischgestenbasierten Befehls

Du behandelst das [Invoked](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked)-Ereignis, um auf einen wischgestenbasierten Befehl zu reagieren. (Weitere Informationen zum Aufrufen eines Befehls durch einen Benutzer findest du weiter oben in diesem Artikel im Abschnitt _Wie funktionieren Wischgesten?_ .) In der Regel wird ein wischgestenbasierter Befehl in ListView oder einem listenähnlichen Element verwendet. In diesem Fall kannst du beim Aufrufen eines Befehls eine Aktion für das Element ausführen, über das du gewischt hast.

Gehe zum Behandeln des Invoked-Ereignisses für das zuvor erstellte Wischelement _delete_ wie folgt vor:

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

Bei dem Datenelement handelt es sich um DataContext von SwipeControl. In deinem Code kannst du auf das Element zugreifen, über das gewischt wurde, indem du die Eigenschaft „SwipeControl.DataContext“ aus den Ereignisargumenten abrufst, wie hier gezeigt.

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> Hier wurden die Elemente der Einfachheit halber direkt der Sammlung „ListView.Items“ hinzugefügt, damit das Element auf die gleiche Weise gelöscht wird. Legst du stattdessen „ListView.ItemsSource“ auf eine Sammlung fest (was üblicher ist), musst du das Element aus der Quellsammlung löschen.

In diesem speziellen Fall hast du das Element aus der Liste entfernt, sodass der endgültige visuelle Zustand des gewischten Elements nicht von Bedeutung ist. In Situationen, in denen du einfach eine Aktion ausführen und die wischgestenbasierte Steuerung danach wieder zuklappen möchtest, kannst du die Eigenschaft [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) auf einen der [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked)-Enumerationswerte festlegen.

- **Auto**
  - Im Ausführungsmodus bleibt das geöffnete Wischelement beim Aufrufen geöffnet.
  - Im Einblendmodus wird das geöffnete Wischelement beim Aufrufen zugeklappt.
- **Close**
  - Wenn das Element aufgerufen wird, wird das Steuerelement „Wischen“ immer zugeklappt und kehrt unabhängig vom jeweiligen Modus zum Normalzustand zurück.
- **RemainOpen**
  - Wenn das Element aufgerufen wird, bleibt das Steuerelement „Wischen“ unabhängig vom jeweiligen Modus immer geöffnet.

Hier wird ein Wischelement vom Typ _reply_ so festgelegt, dass es nach dem Aufrufen geschlossen wird.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwende den Wischvorgang nicht für FlipViews, Hubs oder Pivots. Die Kombination kann aufgrund von widersprüchlichen Wischrichtungen für den Benutzer verwirrend sein.
- Kombiniere horizontales Wischen nicht mit horizontaler Navigation bzw. vertikales Wischen nicht mit vertikaler Navigation.
- Stelle sicher, dass es sich bei dem vom Benutzer durchgeführten Wischvorgang um dieselbe Aktion handelt und dass sie konsistent für alle zugehörigen wischfähigen Elemente ausgeführt wird.
- Verwende Wischvorgänge für die vom Benutzer ausgeführten Hauptaktionen.
- Verwende Wischvorgänge für Elemente, für die die gleiche Aktion mehrmals wiederholt wird.
- Verwende horizontales Wischen bei breiteren und vertikales Wischen bei höheren Elementen.
- Verwende kurze, präzise Beschriftungen.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Listenansicht und Rasteransicht](listview-and-gridview.md)
- [Elementcontainer und Vorlagen](item-containers-templates.md)
- [Aktualisierung durch Ziehen](pull-to-refresh.md)
