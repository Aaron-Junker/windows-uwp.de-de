---
Description: NavigationView ist ein adaptives Steuerelement, mit dem Muster für die Navigation auf oberster Ebene für deine App implementiert werden.
title: Navigationsansicht
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c0d12b3b043546cd908fb474fa8ca9656d8dc56e
ms.sourcegitcommit: bac5574a1f47a5b38c984a5482272c9e49a9c91e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71100852"
---
# <a name="navigation-view"></a>Navigationsansicht

Mit dem NavigationView-Steuerelement wird die Navigation auf oberster Ebene für deine App bereitgestellt. Es ermöglicht die Anpassung an verschiedene Bildschirmgrößen und unterstützt sowohl den Navigationsstil _oben_ als auch _links_.

![Navigation „oben“](images/nav-view-header.png)<br/>
_Navigationsansicht unterstützt die Anordnung von Bereichen bzw. Menüs sowohl „oben“ als auch „links“_

> **Plattform-APIs**: [Windows.UI.Xaml.Controls.NavigationView-Klasse](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows-UI-Bibliotheks-APIs**: [Microsoft.UI.Xaml.Controls.NavigationView-Klasse](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Für einige Features von NavigationView, z. B. die Navigation _oben_, ist Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) erforderlich.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

NavigationView ist ein adaptives Navigationssteuerelement, das für folgende Zwecke gut geeignet ist:

- Bereitstellen einer einheitlichen Navigationsumgebung für die gesamte App
- Sparen von Platz auf dem Bildschirm in kleineren Fenstern
- Organisieren des Zugriffs auf viele Navigationskategorien

Weitere Navigationsmuster findest du unter [Navigationsdesigngrundlagen](../basics/navigation-basics.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn du die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert hast, kannst du <a href="xamlcontrolsgallery:/item/NavigationView">hier klicken, um die App zu öffnen und NavigationView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Anzeigemodi

> Für die PaneDisplayMode-Eigenschaft ist Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) erforderlich.

Du kannst die PaneDisplayMode-Eigenschaft verwenden, um verschiedene Navigationsstile oder Anzeigemodi für das NavigationView-Element zu konfigurieren.

:::row:::
    :::column:::
    ### <a name="top"></a>Oben
    Dieser Bereich befindet sich oberhalb des Inhalts.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für Navigation oben](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Wir empfehlen die Navigation _oben_, wenn Folgendes gilt:

- Du hast fünf oder weniger Navigationskategorien der obersten Ebene, die alle die gleiche Wichtigkeit aufweisen. Alle weiteren Navigationskategorien der obersten Ebene, die sich im Dropdown-Überlaufmenü befinden, werden als weniger wichtig angesehen.
- Du musst alle Navigationsoptionen auf dem Bildschirm anzeigen.
- Du benötigst mehr Platz für deinen App-Inhalt.
- Die Navigationskategorien deiner App lassen sich nicht eindeutig durch Symbole darstellen.

:::row:::
    :::column:::
    ### <a name="left"></a>Nach links
    Der Bereich wird erweitert und links vom Inhalt angeordnet.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für erweiterten Navigationsbereich auf der linken Seite](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Wir empfehlen die Navigation _links_, wenn Folgendes gilt:

- Du verfügst über fünf bis zehn Navigationskategorien der obersten Ebene mit gleicher Wichtigkeit.
- Du möchtest, dass die Navigationskategorien hervorgehoben sind und weniger Platz für anderen App-Inhalt vorhanden ist.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    Im Bereich werden bis zum Öffnen nur Symbole angezeigt, und dann wird er links vom Inhalt angeordnet.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für kompakten Navigationsbereich links](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Bis der Bereich geöffnet wird, wird nur die Menüschaltfläche angezeigt. Nach dem Öffnen wird er links vom Inhalt angeordnet.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für minimalen Navigationsbereich links](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

Standardmäßig ist „PaneDisplayMode“ auf „Auto“ festgelegt. Im Modus „Auto“ wird die Navigationsansicht von „LeftMinimal“ (wenn das Fenster schmal ist) in „LeftCompact“ und dann in „Left“ geändert, wenn das Fenster breiter wird. Weitere Informationen findest du im Abschnitt zum [adaptiven Verhalten](#adaptive-behavior).

![Standardnavigation links: adaptives Verhalten](images/displaymode-auto.png)<br/>
_Standardnavigationsansicht: adaptives Verhalten_

## <a name="anatomy"></a>Aufbau

In diesen Abbildungen ist das Layout des Bereichs, der Kopfzeile und des Inhaltsbereichs für das Steuerelement dargestellt, wenn es für die Navigation _oben_ oder _links_ konfiguriert ist.

![Layout für Navigationsansicht oben](images/topnav-anatomy.png)<br/>
_Layout für Navigation oben_

![Layout für Navigationsansicht links](images/leftnav-anatomy.png)<br/>
_Layout für Navigation links_

### <a name="pane"></a>Bereich

Du kannst die PaneDisplayMode-Eigenschaft verwenden, um den Bereich oberhalb des Inhalts oder links davon anzuordnen.

Der NavigationView-Bereich kann Folgendes enthalten:

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem)-Objekte. Navigationselemente für die Navigation auf bestimmte Seiten.
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)-Objekte. Trennzeichen zum Gruppieren von Navigationselementen. Lege die [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity)-Eigenschaft auf „0“ fest, um das Trennzeichen als Leerzeichen zu rendern.
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)-Objekte. Kopfzeile für Bezeichnungen von Elementgruppen.
- Ein optionales [AutoSuggestBox](auto-suggest-box.md)-Steuerelement für die Suche auf App-Ebene. Weise das Steuerelement der [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox)-Eigenschaft zu.
- Ein optionaler Einstiegspunkt für [App-Einstellungen](../app-settings/app-settings-and-data.md). Lege zum Ausblenden des Einstellungselements die [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)-Eigenschaft auf **false** fest.

Der linke Bereich enthält außerdem Folgendes:

- Eine Menüschaltfläche zum Öffnen und Schließen des Bereichs. Bei größeren App-Fenstern mit geöffnetem Bereich kannst du diese Schaltfläche mit der [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible)-Eigenschaft auch ausblenden.

Die Navigationsansicht verfügt über eine Zurück-Schaltfläche, die oben links im Bereich angeordnet ist. Hiermit wird die Navigation in Rückwärtsrichtung aber nicht automatisch verarbeitet und der Inhalt nicht dem Backstack hinzugefügt. Informationen zum Aktivieren der Rückwärtsnavigation findest du im Abschnitt [Rückwärtsnavigation](#backwards-navigation).

Hier ist der ausführliche Seitenaufbau für die Anordnung des Bereichs oben und links dargestellt.

#### <a name="top-navigation-pane"></a>Navigationsbereich oben

![Bereichsaufbau der Navigationsansicht oben](images/navview-pane-anatomy-horizontal.png)

1. Header
1. Navigationselemente
1. Trennzeichen
1. AutoSuggestBox (optional)
1. Schaltfläche „Einstellungen“ (optional)

#### <a name="left-navigation-pane"></a>Navigationsbereich links

![Bereichsaufbau der Navigationsansicht links](images/navview-pane-anatomy-vertical.png)

1. Menü-Taste
1. Navigationselemente
1. Trennzeichen
1. Header
1. AutoSuggestBox (optional)
1. Schaltfläche „Einstellungen“ (optional)

#### <a name="pane-footer"></a>Fußzeile des Bereichs

Du kannst den Freiforminhalt in der Fußzeile des Bereichs anordnen, indem du ihn der [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)-Eigenschaft hinzufügst.

:::row:::
    :::column:::
    ![Bereichsfußzeile für Navigation oben](images/navview-freeform-footer-top.png)<br>
     _Bereichsfußzeile für Navigation oben_<br>
    :::column-end:::
    :::column:::
    ![Bereichsfußzeile für Navigation links](images/navview-freeform-footer-left.png)<br>
    _Bereichsfußzeile für Navigation links_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Titel und Kopfzeile des Bereichs

Du kannst Textinhalt in der Kopfzeile des Bereichs anordnen, indem du die [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle)-Eigenschaft festlegst. Hierbei wird eine Zeichenfolge verwendet und der Text neben der Menüschaltfläche angezeigt.

Zum Hinzufügen von anderen Inhalten als Text, z. B. ein Bild oder Logo, kannst du ein beliebiges Element in der Kopfzeile des Bereichs anordnen, indem du es der [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)-Eigenschaft hinzufügst.

Wenn sowohl „PaneTitle“ als auch „PaneHeader“ festgelegt sind, wird der Inhalt horizontal neben der Menüschaltfläche gestapelt, wobei „PaneTitle“ der Menüschaltfläche am nächsten liegt.

:::row:::
    :::column:::
    ![Bereichskopfzeile für Navigation oben](images/navview-freeform-header-top.png)<br>
     _Bereichskopfzeile für Navigation oben_<br>
    :::column-end:::
    :::column:::
    ![Bereichskopfzeile für Navigation links](images/navview-freeform-header-left.png)<br>
    _Bereichskopfzeile für Navigation links_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Bereichsinhalt

Du kannst den Freiforminhalt im Bereich anordnen, indem du ihn der [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)-Eigenschaft hinzufügst.

:::row:::
    :::column:::
    ![Benutzerdefinierter Bereichsinhalt für Navigation oben](images/navview-freeform-pane-top.png)<br>
     _Benutzerdefinierter Bereichsinhalt für Navigation oben_<br>
    :::column-end:::
    :::column:::
    ![Benutzerdefinierter Bereichsinhalt für Navigation links](images/navview-freeform-pane-left.png)<br>
    _Benutzerdefinierter Bereichsinhalt für Navigation links_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Header

Du kannst einen Seitentitel hinzufügen, indem du die [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header)-Eigenschaft festlegst.

![Beispiel für Kopfzeilenbereich der Navigationsansicht](images/nav-header.png)<br/>
_Kopfzeile der Navigationsansicht_

Der Kopfzeilenbereich ist vertikal an der Position der Navigationsschaltfläche im linken Bereich ausgerichtet und befindet sich unterhalb des obersten Bereichs. Er hat eine feste Höhe von 52 Pixel. Er enthält jeweils den Seitentitel der ausgewählten Navigationskategorie. Die Kopfzeile ist am oberen Rand der Seite angedockt und dient als Scroll-Clipping-Punkt für den Inhaltsbereich.

Die Kopfzeile wird jedes Mal angezeigt, wenn sich „NavigationView“ im Anzeigemodus „Minimal“ befindet. Du kannst die Kopfzeile in anderen Modi ausblenden, die bei größeren Fensterbreiten verwendet werden. Lege zum Ausblenden der Kopfzeile die [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader)-Eigenschaft auf **false** fest.

### <a name="content"></a>Inhalt

![Beispiel für Inhaltsbereich der Navigationsansicht](images/nav-content.png)<br/>
_Inhalt der Navigationsansicht_

Im Inhaltsbereich werden die meisten Informationen für die ausgewählte Navigationskategorie angezeigt.

Wir empfehlen, Ränder für deinen Inhaltsbereich auf 12 Pixel festzulegen, wenn sich „NavigationView“ im Modus **Minimal** befindet. Verwende andernfalls 24 Pixel.

## <a name="adaptive-behavior"></a>Adaptives Verhalten

Standardmäßig ändert die Navigationsansicht je nach verfügbarem Platz auf dem Bildschirm automatisch den Anzeigemodus. Die Eigenschaften [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) und [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) geben die Haltepunkte an, an denen sich der Anzeigemodus ändert. Du kannst diese Werte ändern, um das Verhalten des adaptiven Anzeigemodus anzupassen.

### <a name="default"></a>Standard

Wenn PaneDisplayMode auf den Standardwert **Auto** festgelegt ist, wird beim adaptiven Verhalten Folgendes angezeigt:

- Ein erweiterter Bereich auf der linken Seite bei höheren Fensterbreiten (1008 Pixel oder mehr)
- Ein Navigationsbereich nur für Symbole auf der linken Seite (LeftCompact) bei mittleren Fensterbreiten (641 bis 1.007 Pixel)
- Nur eine Menüschaltfläche (LeftMinimal) bei kleinen Fensterbreiten (640 Pixel oder weniger)

Weitere Informationen zu Fenstergrößen für adaptives Verhalten findest du unter [Bildschirmgrößen und Haltepunkte](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Standardnavigation links: adaptives Verhalten](images/displaymode-auto.png)<br/>
_Standardnavigationsansicht: adaptives Verhalten_

### <a name="minimal"></a>Minimal

Ein zweites gängiges adaptives Muster ist die Verwendung eines erweiterten Bereichs auf der linken Seite bei höheren Fensterbreiten und nur einer Menüschaltfläche bei mittleren und geringen Fensterbreiten.

Dies empfehlen wir in folgenden Fällen:

- Du benötigst mehr Platz für App-Inhalt bei geringeren Fensterbreiten.
- Deine Navigationskategorien lassen sich durch Symbole nicht eindeutig darstellen.

![Navigation links „Minimal“: adaptives Verhalten](images/adaptive-behavior-minimal.png)<br/>
_Navigationsansicht „Minimal“: adaptives Verhalten_

Lege zum Konfigurieren dieses Verhaltens „CompactModeThresholdWidth“ auf die Breite fest, die für die Reduzierung des Bereichs verwendet werden soll. Hier wurde der Standardwert 640 in 1.007 geändert. Du kannst auch „ExpandedModeThresholdWidth“ festlegen, um sicherzustellen, dass es für die Werte nicht zu einem Konflikt kommt.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Kompakt

Ein drittes gängiges adaptives Muster ist die Verwendung eines erweiterten Bereichs auf der linken Seite bei höheren Fensterbreiten und eines Navigationsbereichs vom Typ „LeftCompact“ nur für Symbole bei mittleren und geringen Fensterbreiten.

Dies empfehlen wir in folgenden Fällen:

- Es ist wichtig, dass immer alle Navigationsoptionen auf dem Bildschirm angezeigt werden.
- Deine Navigationskategorien lassen sich durch Symbole eindeutig darstellen.

![Navigation links „Kompakt“: adaptives Verhalten](images/adaptive-behavior-compact.png)<br/>
_Navigationsansicht „Kompakt“: adaptives Verhalten_

Lege zum Konfigurieren dieses Verhaltens „CompactModeThresholdWidth“ auf „0“ fest.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Kein adaptives Verhalten

Lege „PaneDisplayMode“ auf einen anderen Wert als „Auto“ fest, um das automatische adaptive Verhalten zu deaktivieren. Hier ist „LeftMinimal“ festgelegt, sodass unabhängig von der Fensterbreite nur die Menüschaltfläche angezeigt wird.

![Navigation links: kein adaptives Verhalten](images/adaptive-behavior-none.png)<br/>
_Navigationsansicht mit Festlegung von „PaneDisplayMode“ auf „LeftMinimal“_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Wie oben im Abschnitt _Anzeigemodi_ beschrieben, kannst du für den Bereich „Immer oben“, „Immer erweitert“, „Immer kompakt“ oder „Immer minimal“ festlegen. Du kann die Anzeigemodi auch selbst in deinem App-Code verwalten. Ein Beispiel hierfür ist im nächsten Abschnitt enthalten.

### <a name="top-to-left-navigation"></a>Von Navigation oben zu Navigation links

Wenn du in deiner App die Navigation „oben“ verwendest, werden die Navigationselemente zu einem Überlaufmenü reduziert, wenn sich die Fensterbreite verringert. Falls dein App-Fenster schmal ist, kann die Benutzerfreundlichkeit durch das Umschalten von „PaneDisplayMode“ von „Top“ auf „LeftMinimal“ für die Navigation erhöht werden, anstatt zuzulassen, dass alle Elemente zu einem Überlaufmenü reduziert werden.

Wir empfehlen die Verwendung der Navigation „oben“ bei großen Fenstern und die Navigation „links“ bei kleinen Fenstern, wenn Folgendes gilt:

- Du verfügst über mehrere Navigationskategorien der obersten Ebene mit gleicher Wichtigkeit, die zusammen angezeigt werden sollen. Wenn eine Kategorie dieser Gruppe nicht auf den Bildschirm passt, wird eine Reduzierung auf den Navigationsbereich links durchgeführt, um die gleiche Wichtigkeit sicherzustellen.
- Du möchtest bei kleinen Fenstern für Inhalt so viel Platz wie möglich bereitstellen.

In diesem Beispiel wird gezeigt, wie du eine [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager)- und [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth)-Eigenschaft verwendest, um zwischen den Navigationstypen „Top“ und „LeftMinimal“ umzuschalten.

![Beispiel für adaptives Verhalten „oben“ oder „links“: 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> Wenn du „AdaptiveTrigger.MinWindowWidth“ verwendest, wird der visuelle Zustand ausgelöst, falls das Fenster breiter als die angegebene Mindestbreite ist. Dies bedeutet, dass mit dem XAML-Standardcode das schmale Fenster definiert wird und mit „VisualState“ die Änderungen definiert werden, die angewendet werden, wenn das Fenster breiter wird. Der Standardmodus „PaneDisplayMode“ für die Navigationsansicht lautet „Auto“. Wenn die Fensterbreite kleiner oder gleich „CompactModeThresholdWidth“ ist, wird die Navigation vom Typ „LeftMinimal“ verwendet. Wenn das Fenster breiter wird, wird der Standardwert mit „VisualState“ überschrieben und die Navigation oben verwendet.

## <a name="navigation"></a>Navigation

Für die Navigationsansicht werden keine Navigationsaufgaben automatisch durchgeführt. Wenn der Benutzer auf ein Navigationselement tippt, wird es in der Navigationsansicht als ausgewählt angezeigt und ein [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked)-Ereignis ausgelöst. Falls der Tippvorgang zur Auswahl eines neuen Elements führt, wird auch ein [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged)-Ereignis ausgelöst.

Du kannst beide Ereignisse verarbeiten, um Aufgaben für die angeforderte Navigation durchzuführen. Welches Ereignis verarbeitet werden sollte, hängt von dem Verhalten ab, das für deine App gelten soll. Normalerweise navigierst du zur angeforderten Seite und aktualisierst die Kopfzeile der Navigationsansicht als Reaktion auf diese Ereignisse.

**ItemInvoked** wird immer ausgelöst, wenn der Benutzer auf ein Navigationselement tippt – auch wenn es bereits ausgewählt ist. (Das Element kann auch mit einer gleichwertigen Aktion per Maus, Tastatur oder anderer Eingabemethode aufgerufen werden. Weitere Informationen findest du unter [Eingabe und Interaktionen](../input/index.md).) Wenn du im ItemInvoked-Handler navigierst, wird die Seite standardmäßig neu geladen, und dem Navigationsstapel wird ein doppelter Eintrag hinzugefügt. Für das Navigieren während eines Elementaufrufs solltest du das erneute Laden der Seite nicht zulassen oder sicherstellen, dass im Backstack der Navigation beim erneuten Laden der Seite kein doppelter Eintrag erstellt wird. (Siehe Codebeispiele.)

**SelectionChanged** kann von einem Benutzer ausgelöst werden, der ein derzeit nicht ausgewähltes Element aufruft, oder indem das ausgewählte Element programmgesteuert geändert wird. Wenn die Änderung der Auswahl erfolgt, weil ein Benutzer ein Element aufgerufen hat, tritt das ItemInvoked-Ereignis zuerst ein. Bei einer programmgesteuerten Änderung der Auswahl wird das ItemInvoked-Ereignis nicht ausgelöst.

### <a name="backwards-navigation"></a>Rückwärtsnavigation

„NavigationView“ verfügt über eine integrierte Zurück-Schaltfläche. Wie bei der Vorwärtsnavigation auch, wird die Rückwärtsnavigation aber nicht automatisch durchgeführt. Wenn der Benutzer auf die Zurück-Schaltfläche tippt, wird das [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested)-Ereignis ausgelöst. Du verarbeitest dieses Ereignis, um die Rückwärtsnavigation durchzuführen. Weitere Informationen und Codebeispiele findest du unter [Navigationsverlauf und Rückwärtsnavigation für UWP-Apps](../basics/navigation-history-and-backwards-navigation.md).

Im Modus „Minimal“ oder „Kompakt“ wird der Bereich der Navigationsansicht als Flyout geöffnet. In diesem Fall wird durch das Klicken auf die Zurück-Schaltfläche der Bereich geschlossen und stattdessen das **PaneClosing**-Ereignis ausgelöst.

Du kannst die Zurück-Schaltfläche ausblenden oder deaktivieren, indem du diese Eigenschaften festlegst:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): Dient zum Ein- und Ausblenden der Zurück-Schaltfläche. Für diese Eigenschaft wird ein Wert der [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible)-Enumeration verwendet, der standardmäßig auf **Auto** festgelegt ist. Wenn die Schaltfläche reduziert ist, wird im Layout dafür kein Platz reserviert.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): Dient zum Aktivieren und Deaktivieren der Zurück-Schaltfläche. Du kannst für diese Eigenschaft eine Datenbindung an die [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback)-Eigenschaft deines Navigationsrahmens durchführen. **BackRequested** wird nicht ausgelöst, wenn **IsBackEnabled** auf **false** festgelegt ist.

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Codebeispiel

In diesem Beispiel wird veranschaulicht, wie du „NavigationView“ sowohl mit einem oberen Navigationsbereich für große Fenster als auch mit einem linken Navigationsbereich für kleine Fenster verwenden kannst. Du kannst eine Umstellung durchführen, bei der nur die Navigation links verwendet wird, indem du in VisualStateManager die Navigationseinstellungen für _top_ entfernst.

Das Beispiel veranschaulicht eine empfohlene Vorgehensweise zum Einrichten von Navigationsdaten, die für viele gängige Szenarien geeignet ist. Darüber hinaus wird veranschaulicht, wie du die Rückwärtsnavigation mit der Zurück-Schaltfläche von „NavigationView“ und der Navigation über die Tastatur implementierst.

In diesem Code wird vorausgesetzt, dass deine App Seiten mit den folgenden Namen enthält, auf die navigiert werden kann: _HomePage_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_ und _SettingsPage_. Der Code für diese Seiten ist nicht angegeben.

> [!IMPORTANT]
> Informationen zu den Seiten der App werden in einem [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple)-Element gespeichert. Für diese Struktur muss als Mindestversion für dein App-Projekt das SDK 17763 oder höher verwendet werden. Wenn du die WinUI-Version von „NavigationView“ für frühere Versionen von Windows 10 verwendest, kannst du stattdessen das [NuGet-Paket „System.ValueTuple NuGet“](https://www.nuget.org/packages/System.ValueTuple/) nutzen.

> [!IMPORTANT]
> Dieser Code veranschaulicht, wie du die NavigationView-Version der [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) verwendest. Wenn du stattdessen die Plattformversion von „NavigationView“ nutzt, muss die Mindestversion für dein App-Projekt „SDK 17763“ oder höher lauten. Entferne zum Verwenden der Plattformversion alle Verweise auf `muxc:`.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> Dieser Code veranschaulicht, wie du die NavigationView-Version der [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) verwendest. Wenn du stattdessen die Plattformversion von „NavigationView“ nutzt, muss die Mindestversion für dein App-Projekt „SDK 17763“ oder höher lauten. Entferne zum Verwenden der Plattformversion alle Verweise auf `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

// NavView_SelectionChanged is not used in this example, but is shown for completeness.
// You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
// but not both.
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

Unten ist eine [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index)-Version des **NavView_ItemInvoked**-Handlers aus dem obigen C#-Codebeispiel angegeben. Das Verfahren im C++/WinRT-Handler umfasst als Erstes das Speichern des vollständigen Typnamens der Seite (im Tag des [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)-Elements), auf die du navigieren möchtest. Im Handler führst du das Unboxing für diesen Wert durch, wandelst ihn in ein [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename)-Objekt um und verwendest dieses Objekt, um zur Zielseite zu navigieren. Es ist nicht erforderlich, die Zuordnungsvariable mit dem Namen `_pages` aus dem C#-Beispiel zu nutzen. Du kannst Komponententests erstellen, um zu bestätigen, dass die Werte in deinen Tags einen gültigen Typ aufweisen. Weitere Informationen findest du unter [Boxing und Unboxing von Skalarwerten für „IInspectable“ mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="navigation-view-customization"></a>Anpassung der Navigationsansicht

### <a name="pane-backgrounds"></a>Bereichshintergründe

Standardmäßig wird je nach Anzeigemodus im NavigationView-Bereich ein anderer Hintergrund verwendet:

- Der Bereich ist grau, wenn er nach links erweitert wird (neben dem Inhalt, im Modus „Links“).
- Für den Bereich wird In-App-Acryl verwendet, wenn er als Überlagerung auf dem Inhalt geöffnet wird (im Modus „Oben“, „Minimal“ oder „Kompakt“).

Zum Ändern des Bereichshintergrunds kannst du die XAML-Designressourcen außer Kraft setzen, die in den einzelnen Modi zum Rendern des Hintergrunds verwendet werden. (Dieses Verfahren wird anstelle einer einzelnen PaneBackground-Eigenschaft genutzt, um unterschiedliche Hintergründe für unterschiedliche Anzeigemodi zu unterstützen.)

In dieser Tabelle ist angegeben, welche Designressource in den unterschiedlichen Anzeigemodi verwendet wird.

| Anzeigemodus | Designressource |
| ------------ | -------------- |
| Nach links | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Oben | NavigationViewTopPaneBackground |

In diesem Beispiel wird veranschaulicht, wie du die Designressourcen in „App.xaml“ außer Kraft setzt. Beim Außerkraftsetzen der Designressourcen solltest du immer mindestens die Ressourcenwörterbücher „Default“ und „HighContrast“ sowie je nach Bedarf Wörterbücher für Ressourcen vom Typ „Light“ oder „Dark“ bereitstellen. Weitere Informationen findest du unter [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Dieser Code veranschaulicht, wie du die AcrylicBrush-Version der [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) verwendest. Wenn du stattdessen die Plattformversion von „AcrylicBrush“ nutzt, muss die Mindestversion für dein App-Projekt „SDK 16299“ oder höher lauten. Entferne zum Verwenden der Plattformversion alle Verweise auf `muxm:`.

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

### <a name="top-whitespace"></a>Leerraum oben
Einige Apps [passen die Titelleiste ihres Fensters an](https://docs.microsoft.com/windows/uwp/design/shell/title-bar) und erweitern so den App-Inhalt auf den Bereich der Titelleiste. Wenn NavigationView das Stammelement in Apps ist, die ihren Inhalt **mithilfe der [ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar)-API** in die Titelleiste erweitern, passt das Steuerelement die Position seiner interaktiven Elemente automatisch an, um eine Überlappung mit dem [ziehbaren Bereich](https://docs.microsoft.com/windows/uwp/design/shell/title-bar#draggable-regions) zu vermeiden. 
![Eine App, die ihren Inhalt in die Titelleiste erweitert](images/navigation-view-with-titlebar-padding.png)

Wenn deine App den ziehbaren Bereich durch Aufrufen der [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar)-Methode angibt und du die Schaltflächen „Zurück“ und „Menü“ lieber oben im App-Fenster anordnen möchtest, lege `IsTitleBarAutoPaddingEnabled` auf FALSE fest.

![App, die ihren Inhalt in die Titelleiste erweitert, ohne Innenabstand](images/navigation-view-no-titlebar-padding.png)

```Xaml
<muxc:NavigationView x:Name="NavView" IsTitleBarAutoPaddingEnabled="False">
```

#### <a name="remarks"></a>Hinweise
Um die Position des Kopfzeilenbereichs von NavigationView genauer anzupassen, überschreibe die XAML-Themenressource *NavigationViewHeaderMargin* – z. B. in den Ressourcen für die Seite.

```Xaml
<Page.Resources>
    <Thickness x:Key="NavigationViewHeaderMargin">12,0</Thickness>
</Page.Resources>
```

Diese Themenressource ändert den Rand um [NavigationView.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.header).

## <a name="related-topics"></a>Verwandte Themen

- [NavigationView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Master/Details](master-details.md)
- [Navigationsgrundlagen](../basics/navigation-basics.md)
- [Übersicht über Fluent Design für UWP](/windows/apps/fluent-design-system)
