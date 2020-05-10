---
Description: TabView bietet eine flexible Möglichkeit zum Organisieren mehrerer Dokumente auf dynamischen Registerkarten
title: Registerkartenansicht
template: detail.hbs
ms.date: 09/12/2019
ms.topic: article
keywords: Windows 10, UWP
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f5ec09f8fdfa0a8b4b8c7161cc6d91a3013230d7
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970685"
---
# <a name="tabview"></a>TabView

Das TabView-Steuerelement bietet eine Möglichkeit, eine Reihe von Registerkarten und deren jeweiligen Inhalt anzuzeigen. TabViews sind nützlich, um mehrere Seiten (oder Dokumente) mit Inhalten anzuzeigen und geben einem Benutzer die Möglichkeit, neue Registerkarten neu anzuordnen, zu öffnen oder zu schließen.

![Beispiel für eine TabView](images/tabview/tab-introduction.png)

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Das Steuerelement **TabView** erfordert die Windows-UI-Bibliothek. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Windows-UI-Bibliotheks-APIs:** [TabView-Klasse](/uwp/api/microsoft.ui.xaml.controls.tabview), [TabViewItem-Klasse](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

> [!TIP]
> In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt: `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Im Allgemeinen gibt es für Benutzeroberflächen mit Registerkarten zwei verschiedene Stile, die sich in Funktion und Aussehen unterscheiden: **Statische Registerkarten** sind die Arten von Registerkarten, die sich häufig in Fenstern für Einstellungen finden. Sie enthalten eine festgelegte Anzahl von Seiten in einer festgelegten Reihenfolge, die in der Regel vordefinierte Inhalte enthalten.
**Dokumentregisterkarten** sind die Arten von Registerkarten, die in einem Browser enthalten sind, z.B. in Microsoft Edge. Benutzer können Registerkarten erstellen, entfernen und neu anordnen, Registerkarten zwischen Fenstern verschieben und den Inhalt von Registerkarten ändern.

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) bietet Dokumentregisterkarten für UWP-Apps. Verwenden Sie unter den folgenden Umständen eine TabView:

- Benutzer können Registerkarten dynamisch öffnen, schließen oder neu anordnen.
- Benutzer können Dokumente oder Webseiten direkt auf Registerkarten öffnen.
- Benutzer können Registerkarten mithilfe von Drag & Drop zwischen Fenstern verschieben.

Wenn eine TabView für Ihre App nicht geeignet ist, sollten Sie Steuerelemente wie [Pivot](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot) oder [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) verwenden.

## <a name="anatomy"></a>Aufbau

Die folgende Abbildung zeigt die Bestandteile des [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)-Steuerelements. Das TabStrip-Element weist eine Kopf- und Fußzeile auf, aber im Gegensatz zu einem Dokument befinden sich die Kopf- und Fußzeile des TabStrips jeweils ganz links und ganz rechts auf dem TabStrip.

![Aufbau des TabView-Steuerelements](images/tabview/tab-view-anatomy.png)

Die nächste Abbildung zeigt die Bestandteile des [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)-Steuerelements. Beachten Sie, dass der Inhalt zwar innerhalb des TabView-Steuerelements angezeigt wird, der Inhalt aber tatsächlich ein Teil von TabViewItem ist.

![Aufbau des TabViewItem-Steuerelements](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>Erstellen einer Registerkartenansicht

Dieses Beispiel erstellt eine einfache [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) zusammen mit Ereignishandlern, um das Öffnen und Schließen von Registerkarten zu unterstützen.

```xaml
 <muxc:TabView AddTabButtonClick="TabView_AddTabButtonClick"
               TabCloseRequested="TabView_TabCloseRequested"/>
```

```csharp
// Add a new Tab to the TabView
private void TabView_AddTabButtonClick(muxc.TabView sender, object args)
{
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void TabView_TabCloseRequested(muxc.TabView sender, muxc.TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>Verhalten

Es gibt eine Reihe von Möglichkeiten, die Funktionalität einer [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) zu nutzen oder zu erweitern.

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>Binden von TabItemsSource an eine TabViewItemCollection

```xaml
<muxc:TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>Anzeigen der TabView-Registerkarten in der Titelleiste eines Fensters

Anstatt dass Registerkarten eine eigene Zeile unter der Titelleiste eines Fensters belegen, können Sie beides im gleichen Bereich zusammenführen. Dies spart vertikalen Platz für Ihre Inhalte und verleiht Ihrer App ein modernes Erscheinungsbild.

Da ein Benutzer ein Fenster anhand seiner Titelleiste ziehen kann, um das Fenster neu zu positionieren, ist es wichtig, dass die Titelleiste nicht vollständig mit Registerkarten ausgefüllt ist. Daher müssen Sie bei der Anzeige von Registerkarten in einer Titelleiste einen Teil der Titelleiste angeben, der als ziehbarer Bereich reserviert werden soll. Wenn Sie keinen ziehbaren Bereich angeben, wird die gesamte Titelleiste ziehbar, was verhindert, dass Ihre Registerkarten Eingabeereignisse empfangen. Wenn Ihre TabView in der Titelleiste eines Fensters angezeigt wird, sollten Sie immer einen [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter) in Ihre [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) aufnehmen und diesen als ziehbaren Bereich markieren.

Weitere Informationen finden Sie unter [Anpassen der Titelleiste](https://docs.microsoft.com/windows/uwp/design/shell/title-bar).

![Registerkarten in der Titelleiste](images/tabview/tab-extend-to-title.png)

```xaml
<muxc:TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
    <muxc:TabView.TabItems>
        <muxc:TabViewItem Header="Home" IsClosable="False">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Home" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
        <muxc:TabViewItem Header="Document 1">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Document" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
        <muxc:TabViewItem Header="Document 2">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Document" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
        <muxc:TabViewItem Header="Document 3">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Document" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
    </muxc:TabView.TabItems>

    <muxc:TabView.TabStripHeader>
        <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
    </muxc:TabView.TabStripHeader>
    <muxc:TabView.TabStripFooter>
        <Grid x:Name="CustomDragRegion" Background="Transparent" />
    </muxc:TabView.TabStripFooter>
</muxc:TabView>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> Um sicherzustellen, dass die Registerkarten in der Titelleiste nicht durch den Inhalt der Shell verdeckt werden, müssen Sie linke und rechte Überlagerungen berücksichtigen. In LTR-Layouts enthält die rechte Einblendung die Titelleistenschaltflächen und den Ziehbereich. Das Gegenteil ist bei RTL der Fall. Die SystemOverlayLeftInset- und SystemOverlayRightInset-Werte beziehen sich auf die physische linke und rechte Seite, also kehren Sie auch diese um, wenn Sie RTL verwenden.

### <a name="control-overflow-behavior"></a>Steuerelement-Überlaufverhalten

Wenn die Anzahl der Registerkarten in der Registerkartenleiste zunimmt, können Sie die Anzeige der Registerkarten steuern, indem Sie [TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode) festlegen.

| TabWidthMode-Wert | Verhalten                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Equal              | Wenn neue Registerkarten hinzugefügt werden, werden alle Registerkarten horizontal verkleinert, bis sie eine sehr geringe Mindestbreite erreichen.                                                       |
| SizeToContent      | Registerkarten weisen immer ihre „natürliche Größe“ auf: die Mindestgröße, die erforderlich ist, um ihr Symbol und ihre Überschrift anzuzeigen. Sie werden nicht vergrößert oder verkleinert, wenn Registerkarten hinzugefügt oder geschlossen werden. |

Unabhängig davon, welchen Wert Sie auswählen, kann es vorkommen, dass es zu viele Registerkarten gibt, die in Ihrem TabStrip-Element angezeigt werden. In diesem Fall werden Scrollbumper angezeigt, mit denen der Benutzer das TabStrip-Element nach links und rechts scrollen kann.

### <a name="guidance-for-tab-selection"></a>Hinweise zur Registerkartenauswahl

Die meisten Benutzer sind mit der Verwendung von Dokumentregisterkarten einfach deshalb vertraut, weil sie einen Webbrowser verwenden. Wenn sie Dokumentregisterkarten in Ihrer App verwenden, steuert ihre Erfahrung ihre Erwartungen, wie sich die Registerkarten verhalten sollten.

Unabhängig davon, wie der Benutzer mit einer Reihe von Dokumentregisterkarten interagiert, sollte es immer eine aktive Registerkarte geben. Wenn der Benutzer die ausgewählte Registerkarte schließt oder die ausgewählte Registerkarte in ein anderes Fenster verschiebt, sollte eine andere Registerkarte zur aktiven Registerkarte werden. [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) versucht dies automatisch, indem die nächste Registerkarte ausgewählt wird. Wenn es einen guten Grund gibt, warum Ihre App ein TabView-Element mit einer nicht ausgewählten Registerkarte zulassen sollte, ist der Inhaltsbereich der TabView einfach leer.

## <a name="keyboard-navigation"></a>Navigation mithilfe der Tastatur

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) unterstützt standardmäßig viele gängige Navigationsszenarien mit der Tastatur. In diesem Abschnitt werden die integrierten Funktionen erläutert und Empfehlungen für zusätzliche Funktionen gegeben, die für einige Apps hilfreich sein können.

### <a name="tab-and-cursor-key-behavior"></a>Verhalten der TAB- und CURSORTASTEN

Wenn der Fokus in den _TabStrip_-Bereich bewegt wird, erhält das ausgewählte [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) den Fokus. Der Benutzer kann dann mit den NACH-LINKS- und NACH-RECHTS-TASTEN den Fokus (nicht die Auswahl) auf andere Registerkarten im TabStrip-Element verschieben. Der Pfeiltastenfokus ist im TabStrip-Element und in der Schaltfläche zum Hinzufügen von Registerkarten „(+)“ eingeschlossen, falls vorhanden. Um den Fokus aus dem TabStrip-Bereich zu verschieben, kann der Benutzer die TAB-TASTE drücken, die den Fokus auf das nächste Element verschiebt, das den Fokus erhalten kann.

Verschieben des Fokus über die TAB-TASTE

![Verschieben des Fokus über die TAB-TASTE](images/tabview/tab-keyboard-behavior-1.png)

Pfeiltasten führen keine Fokusrotation aus

![Pfeiltasten führen keine Fokusrotation aus](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>Auswählen einer Registerkarte

Wenn ein TabViewItem den Fokus besitzt, wird durch Drücken der LEERTASTE oder der EINGABETASTE dieses TabViewItem ausgewählt.

Verwenden Sie die Pfeiltasten, um den Fokus zu verschieben, und drücken Sie dann die Leertaste, um die Registerkarte auszuwählen.

![Leertaste zum Auswählen einer Registerkarte](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>Tastenkombinationen zum Auswählen angrenzender Registerkarten

STRG+TAB wählt das nächste [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) aus. STRG+UMSCHALT+TAB wählt das vorherige TabViewItem aus. Zu diesem Zweck wird die Registerkartenliste „in einer Schleife durchlaufen“, sodass die Auswahl der nächsten Registerkarte, während die letzte Registerkarte ausgewählt ist, dazu führt, dass die erste Registerkarte ausgewählt wird.

### <a name="closing-a-tab"></a>Schließen einer Registerkarte

Durch Drücken von STRG+F4 wird das [TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested)-Ereignis ausgelöst. Behandeln Sie das Ereignis, und schließen Sie gegebenenfalls die Registerkarte.

### <a name="keyboard-guidance-for-app-developers"></a>Tastaturhinweise für App-Entwickler

Einige Anwendungen erfordern möglicherweise eine erweiterte Tastatursteuerung. Erwägen Sie, die folgenden Tastenkombinationen zu implementieren, wenn sie für Ihre Anwendung geeignet sind.

> [!WARNING]
> Wenn Sie einer vorhandenen Anwendung eine [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) hinzufügen, haben Sie möglicherweise bereits Tastenkombinationen erstellt, die den Tastenkombinationen der empfohlenen TabView-Tastenkombinationen zugeordnet sind. In diesem Fall müssen Sie sich überlegen, ob Sie Ihre vorhandenen Tastenkombinationen beibehalten oder dem Benutzer eine intuitive Registerkarten-Benutzeroberfläche bieten möchten.

- STRG+T sollte eine neue Registerkarte öffnen. Im Allgemeinen wird auf dieser Registerkarte ein vordefiniertes Dokument angezeigt, oder sie wird leer erstellt, wobei der Inhalt auf einfache Weise ausgewählt werden kann. Wenn der Benutzer Inhalte für eine neue Registerkarte auswählen muss, sollte das Steuerelement für die Inhaltsauswahl den Eingabefokus besitzen.
- STRG+W sollte die ausgewählte Registerkarte schließen. Denken Sie daran, dass TabView die nächste Registerkarte automatisch auswählt.
- STRG+UMSCHALT+T sollte kürzlich geschlossene Registerkarten erneut öffnen (oder genauer gesagt: neue Registerkarten mit dem gleichen Inhalt wie kürzlich geschlossene Registerkarten). Beginnen Sie mit der zuletzt geschlossenen Registerkarte, und bewegen Sie sich für jeden nachfolgenden Aufruf der Tastenkombination rückwärts. Beachten Sie, dass hierfür eine Liste der zuletzt geschlossenen Registerkarten verwaltet werden muss.
- STRG+1 sollte die erste Registerkarte in der Registerkartenliste auswählen. Analog dazu sollte STRG+2 die zweite Registerkarte auswählen, STRG+3 die dritte usw. bis STRG+8.
- STRG+9 sollte die letzte Registerkarte in der Registerkartenliste auswählen, und zwar unabhängig davon, wie viele Registerkarten in der Liste enthalten sind.
- Wenn Registerkarten mehr als nur den Befehl „Schließen“ bieten (z.B. Duplizieren oder Anheften einer Registerkarte), verwenden Sie ein Kontextmenü, um alle verfügbaren Aktionen anzuzeigen, die für eine Registerkarte ausgeführt werden können.

### <a name="implement-browser-style-keyboarding-behavior"></a>Implementieren von Tasaturverhalten im Browserstil

In diesem Beispiel werden einige der oben genannten Empfehlungen für ein [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview)-Element implementiert. Insbesondere werden in diesem Beispiel STRG+T, STRG+W, STRG+1 bis STRG+8 und STRG+9 implementiert.

```xaml
<muxc:TabView x:Name="TabRoot">
    <muxc:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </muxc:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</muxc:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Create new tab.
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    TabRoot.TabItems.Add(newTab);
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only remove the selected tab if it can be closed.
    if (((muxc.TabViewItem)TabRoot.SelectedItem).IsClosable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [MasterDetails](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/master-details)
- [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview)
- [Pivot](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot)
