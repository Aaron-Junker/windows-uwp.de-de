---
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
Description: Wischen Befehle ist ein Touch-Beschleuniger für Kontextmenüs.
title: Swipe
label: Swipe
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3d575eb25d9a013aa999cf304db6d83faeb87765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626655"
---
# <a name="swipe"></a>Swipe

Wischgestenbasierte Befehle sind ein Beschleuniger für Kurzbefehlmenüs. Sie ermöglichen es Benutzern per Toucheingabe, häufig verwendete Aktionen auszuführen, ohne Zustände innerhalb der App zu ändern.

> **Wichtige APIs:** [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [ListView-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![Anzeige- und Ausführungsdesigns](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Wischgestenbasierte Befehle sparen Platz. Sie eignen sich in Situationen, in denen der gleiche Vorgang mehrmals hintereinander schnell wiederholt wird. Außerdem ermöglichen Sie „schnelle Aktionen“ für Elemente an, für die kein vollständiges Popup oder keine Zustandsänderung auf der Seite erforderlich ist.

Wischgestenbasierte Befehle sollten verwendet werden, wenn Sie über eine große Gruppe von Elementen verfügen, die alle 1-3 Aktionen durchführen, die ein Benutzer möglicherweise regelmäßig ausführen möchte. Diese Aktionen können u.a. Folgendes umfassen:

- Löschen
- Markieren oder Archivieren
- Speichern oder Herunterladen
- Beantworten

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/SwipeControl">die App zu öffnen und SwipeControl in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Video-Zusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>Wie funktioniert das Wischen?

UWP Wischen Befehle verfügt über zwei Modi: [Offenlegen](/uwp/api/windows.ui.xaml.controls.swipemode) und [ausführen](/uwp/api/windows.ui.xaml.controls.swipemode). Außerdem werden vier verschiedene Wischrichtungen unterstützt: nach oben, unten, links und rechts.Wischen Richtungen: nach oben, unten, links und rechts.

### <a name="reveal-mode"></a>Anzeigemodus

Im Modus „Einblenden“ wischt der Benutzer über ein Element, um ein Menü mit einem oder mehreren Befehlen zu öffnen, und muss explizit darauf tippen, um diesen Befehl auszuführen. Wenn der Anwender über ein Element wischt und es loslässt, bleibt das Menü geöffnet, bis entweder ein Befehl ausgewählt wird oder das Menü durch streifen, tippen oder wischen des geöffneten Elements vom Bildschirm erneut geschlossen wird.

![Wischen zum Einblenden](images/SwipeCommand-Reveal_v2.gif)

Der Anzeigemodus ist ein sicherer, flexibler Wischmodus und kann für die meisten Aktionen, auch potenziell schädliche Aktionen wie Löschen verwendet werden kann.

Wenn der Benutzer eine der Menüoptionen auswählt, die im geöffneten und positionierten Zustand des Anzeigemodus angezeigt werden, wird der Befehl für dieses Element aufgerufen und das Steuerelement „Wischen“ geschlossen.

### <a name="execute-mode"></a>Ausführungsmodus

Im Ausführungsmodus wischt der Benutzer über ein Element, um einen einzelnen Befehl mit dem Wischvorgang anzuzeigen oder auszuführen. Wenn der Benutzer das Element, über das er gewischt hat, loslässt, bevor er über einen Schwellenwert hinaus wischt, wird das Menü wird geschlossen und der Befehl nicht ausgeführt. Wenn der Benutzer über den Schwellenwert hinaus wischt und das Element dann loslässt, wird der Befehl sofort ausgeführt.

![Ausführen per Wischen](images/SwipeCommand_Delete_v2.gif)

Wenn der Benutzer seinen Finger nach dem Erreichen des Schwellenwerts nicht loslässt und das Element durch Wischen wieder geschlossen wird, wird weder der Befehl noch eine Aktion für das Element ausgeführt.

Der Ausführungsmodus bietet visuelles Feedback durch Farbe und den Bezeichnungstext, während ein Element gezogen wird.

Der Ausführungsmodus ist besonders hilfreich, wenn der Benutzer ihn für eine häufig durchgeführte Aktion verwendet.

Er kann auch für destruktivere Aktionen wie das Löschen eines Elements verwendet werden. Beachten Sie jedoch, dass „Ausführen“ nur eine Wischaktion in eine Richtung erfordert, während der Benutzer für „Anzeigen“ ausdrücklich auf eine Schaltfläche klicken muss.

### <a name="swipe-directions"></a>Wischrichtungen

Es kann in alle Richtungen gewischt werden: nach oben, unten, links und rechts. Jede Wischrichtung kann eigene Wischelemente oder Inhalte enthalten, es kann allerdings jeweils nur eine Instanz der Richtung für ein einzelnes Element festgelegt werden.

Beispielsweise sind zwei [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems)-Definitionen für dasselbe SwipeControl nicht möglich.

## <a name="how-to-create-a-swipe-command"></a>Erstellen eines Wischbefehls

Wischbefehle verfügen über zwei Komponenten, die Sie definieren müssen:

- [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), das Inhalte umschließt. In einer Sammlung, z. B. eine ListView, befindet sich diese innerhalb der DataTemplate.
- Die Menüelemente Wischen, d.h. ein oder mehrere [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem) Objekte in der wischsteuerelement direktionale Containern platziert: [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems), oder [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)

Inhalte zum Wischen können als Inline-Inhalte platziert oder im Abschnitt „Ressourcen“ Ihrer Seite oder App platziert werden.

Nachfolgend finden Sie ein Beispiel für ein SwipeControl, das Text umschließt. Es zeigt die Hierarchie der XAML-Elemente, die zum Erstellen eines Wischbefehls erforderlich sind.

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

In diesem Thema befassen wir uns mit einem vollständigeren Beispiel dazu, wie Wischbefehle normalerweise in einer Liste verwendet werden. In diesem Beispiel richten Sie einen Löschbefehl ein, der den Ausführungsmodus verwendet, und ein Menü mit anderen Befehlen, die den Anzeigemodus verwenden. Beide Befehlsgruppen werden im Abschnitt „Ressourcen“ der Seite definiert. Sie wenden die Wischbefehle auf die Elemente in einer ListView an.

Erstellen Sie zunächst die Wischelemente, die die Befehle darstellen, als Ressourcen auf Seitenebene. SwipeItem verwendet eine [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) als Symbol. Erstellen Sie die Symbole ebenfalls als Ressourcen.

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

Denken Sie daran, die Menüelemente in Ihren Wischinhalten mit knappen und präzisen Beschriftungen beizubehalten. Diese Aktionen sollten die primären Aktionen werden, die ein Benutzer möglicherweise mehrmals innerhalb eines kurzen Zeitraums ausführen möchte.

Das Einrichten eines Wischbefehls für die Verwendung in einer Sammlung oder ListView erfolgt auf genau die gleiche Weise wie das Definieren eines einzelnen Wischbefehls (wie oben gezeigt), mit der Ausnahme, dass das SwipeControl in einer DataTemplate definiert wird, damit es auf jedes Element in der Sammlung angewendet wird.

Hier ist eine ListView mit der SwipeControl, die im DataTemplate-Element angewendet wird. Die Eigenschaften „LeftItems“ und „RightItems“ verweisen auf die Wischelemente, die Sie als Ressourcen erstellen.

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

## <a name="handle-an-invoked-swipe-command"></a>Behandeln eines aufgerufenen Wischbefehls

Um auf einen Wischbefehl zu reagieren, behandeln Sie dessen [Invoked](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked)-Ereignis. (Für Weitere Informationen kann ein Benutzer einen Befehl aufrufen, überprüfen Sie die _wie Wischen funktioniert?_ weiter oben in diesem Artikel.) In der Regel ist ein Streifen-Befehl in einer ListView oder Liste-ähnliches Szenario ein. Wenn dann ein Befehl aufgerufen wird, können Sie eine Aktion für das gezogene Element ausführen.

Gehen Sie zum Behandeln des Invoked-Ereignisses für das zuvor erstellte Wischelement _delete_ wie folgt vor.

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

Bei dem Datenelement handelt es sich um das SwipeControl von DataContext. In Ihrem Code können Sie auf das Element zugreifen, das durch Abrufen der SwipeControl.DataContext-Eigenschaft aus den Ereignisargumenten gezogen wurde, wie hier gezeigt.

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> Hier wurden die Elemente der ListView.Items-Sammlung der Einfachheit halber direkt hinzugefügt, damit das Element ebenfalls auf die gleiche Weise gelöscht wird. Wenn Sie die ListView.ItemsSource stattdessen auf eine Sammlung festlegen, was üblicher ist, müssen Sie das Element aus der Quellsammlung löschen.

In diesem speziellen Fall haben Sie das Element aus der Liste entfernt, sodass der endgültige visuelle Zustand des gezogenen Elements nicht von Bedeutung ist. In Situationen, in denen Sie einfach eine Aktion ausführen möchten und die Wischsteuerung danach wieder zuklappen möchten, können Sie die Eigenschaft [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) auf einen der [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked)-Enumerationswerte festlegen.

- **Auto**
  - Im Ausführungsmodus bleibt das geöffnete Wischelement beim Aufrufen geöffnet.
  - Im Anzeigemodus bleibt das geöffnete Wischelement beim Aufrufen reduziert.
- **Schließen**
  - Wenn das Element aufgerufen wird, wird das Steuerelement „Wischen“ immer zugeklappt und kehrt unabhängig vom jeweiligen Modus zum Normalzustand zurück.
- **RemainOpen**
  - Wenn das Element aufgerufen wird, bleibt das Steuerelement „Wischen“ unabhängig vom jeweiligen Modus immer geöffnet.

Hier wird ein Wischelement vom Typ _Antwort_ so festgelegt, dass es nach dem Aufrufen geschlossen wird.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwenden Sie den Wischvorgang nicht bei FlipViews, Hubs oder Pivots. Die Kombination kann aufgrund von in Konflikt stehenden Wischrichtungen für den Benutzer verwirrend sein.
- Kombinieren Sie das horizontale Wischen nicht mit horizontaler Navigation bzw. das vertikale Wischen mit vertikaler Navigation.
- Stellen Sie sicher, das der vom Benutzer durchgeführte Wischvorgang dieselbe Aktion ist und konsistent für alle bezogenen, wischfähigen Elemente durchgeführt wird.
- Verwenden Sie „Wischen“ für die Hauptaktionen, die Benutzer ausführen.
- Verwenden Sie „Wischen” für Elemente, bei denen die gleiche Aktion mehrmals wiederholt wird.
- Verwenden Sie horizontales Wischen bei größeren und vertikales Wischen bei kleineren Elementen.
- Verwenden Sie kurze und knappe Beschriftungen.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Listenansicht und der Rasteransicht](listview-and-gridview.md)
- [Elementcontainer und Vorlagen](item-containers-templates.md)
- [Zum Aktualisieren nach unten ziehen](pull-to-refresh.md)
