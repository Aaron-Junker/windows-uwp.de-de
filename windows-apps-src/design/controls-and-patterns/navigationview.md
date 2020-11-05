---
description: NavigationView ist ein adaptives Steuerelement, mit dem Muster für die Navigation auf oberster Ebene für deine App implementiert werden.
title: Navigationsansicht
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 689f55393df5fc7af59af6ce1e51fb002f49b713
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031133"
---
# <a name="navigation-view"></a>Navigationsansicht

Mit dem NavigationView-Steuerelement wird die Navigation auf oberster Ebene für deine App bereitgestellt. Es ermöglicht die Anpassung an verschiedene Bildschirmgrößen und unterstützt sowohl den Navigationsstil _oben_ als auch _links_.

![Navigation „oben“](images/nav-view-header.png)<br/>
_Navigationsansicht unterstützt die Anordnung von Bereichen bzw. Menüs sowohl „oben“ als auch „links“_

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Das Steuerelement **NavigationView** ist in der Bibliothek „Windows UI“ enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Plattform-APIs:** [Windows.UI.Xaml.Controls.NavigationView-Klasse](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows-UI-Bibliotheks-APIs** : [Microsoft.UI.Xaml.Controls.NavigationView-Klasse](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Für einige Features von „NavigationView“, z. B. die Navigation _oben_ und die _hierarchische_ Navigation, ist Windows 10, Version 1809 ( [SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher oder die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) erforderlich.

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
<td><img src="images/XAML-controls-gallery-app-icon-sm.png" alt="XAML controls gallery" width="168"></img></td>
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

> Für die PaneDisplayMode-Eigenschaft ist Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) erforderlich.

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

Wir empfehlen die Navigation _oben_ , wenn Folgendes gilt:

- Du hast fünf oder weniger Navigationskategorien der obersten Ebene, die alle die gleiche Wichtigkeit aufweisen. Alle weiteren Navigationskategorien der obersten Ebene, die sich im Dropdown-Überlaufmenü befinden, werden als weniger wichtig angesehen.
- Du musst alle Navigationsoptionen auf dem Bildschirm anzeigen.
- Du benötigst mehr Platz für deinen App-Inhalt.
- Die Navigationskategorien deiner App lassen sich nicht eindeutig durch Symbole darstellen.

:::row:::
    :::column:::
    ### <a name="left"></a>Links
    Der Bereich wird erweitert und links vom Inhalt angeordnet.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für erweiterten Navigationsbereich auf der linken Seite](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Wir empfehlen die Navigation _links_ , wenn Folgendes gilt:

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

### <a name="auto"></a>Automatisch

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
- Ein optionaler Einstiegspunkt für [App-Einstellungen](../app-settings/guidelines-for-app-settings.md). Lege zum Ausblenden des Einstellungselements die [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)-Eigenschaft auf **false** fest.

Der linke Bereich enthält außerdem Folgendes:

- Eine Menüschaltfläche zum Öffnen und Schließen des Bereichs. Bei größeren App-Fenstern mit geöffnetem Bereich kannst du diese Schaltfläche mit der [IsPaneToggleButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible)-Eigenschaft auch ausblenden.

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

Du kannst den Freiforminhalt in der Fußzeile des Bereichs anordnen, indem du ihn der [PaneFooter](/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)-Eigenschaft hinzufügst.

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

Du kannst Textinhalt in der Kopfzeile des Bereichs anordnen, indem du die [PaneTitle](/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle)-Eigenschaft festlegst. Hierbei wird eine Zeichenfolge verwendet und der Text neben der Menüschaltfläche angezeigt.

Zum Hinzufügen von anderen Inhalten als Text, z. B. ein Bild oder Logo, kannst du ein beliebiges Element in der Kopfzeile des Bereichs anordnen, indem du es der [PaneHeader](/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)-Eigenschaft hinzufügst.

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

Du kannst den Freiforminhalt im Bereich anordnen, indem du ihn der [PaneCustomContent](/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)-Eigenschaft hinzufügst.

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

### <a name="default"></a>Standardwert

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
<Grid>
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

Im Modus „Minimal“ oder „Kompakt“ wird der Bereich der Navigationsansicht als Flyout geöffnet. In diesem Fall wird durch das Klicken auf die Zurück-Schaltfläche der Bereich geschlossen und stattdessen das **PaneClosing** -Ereignis ausgelöst.

Du kannst die Zurück-Schaltfläche ausblenden oder deaktivieren, indem du diese Eigenschaften festlegst:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): Dient zum Ein- und Ausblenden der Zurück-Schaltfläche. Für diese Eigenschaft wird ein Wert der [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible)-Enumeration verwendet, der standardmäßig auf **Auto** festgelegt ist. Wenn die Schaltfläche reduziert ist, wird im Layout dafür kein Platz reserviert.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): Dient zum Aktivieren und Deaktivieren der Zurück-Schaltfläche. Du kannst für diese Eigenschaft eine Datenbindung an die [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback)-Eigenschaft deines Navigationsrahmens durchführen. **BackRequested** wird nicht ausgelöst, wenn **IsBackEnabled** auf **false** festgelegt ist.

:::row:::
    :::column:::
        ![Schaltfläche „Zurück“ der Navigationsansicht im linken Navigationsbereich](images/leftnav-back.png)<br/>
        _Schaltfläche „Zurück“ im linken Navigationsbereich_
    :::column-end:::
    :::column:::
        ![Schaltfläche „Zurück“ der Navigationsansicht im oberen Navigationsbereich](images/topnav-back.png)<br/>
        _Schaltfläche „Zurück“ im oberen Navigationsbereich_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Codebeispiel

> [!IMPORTANT]
> Sie führen für jedes Projekt, bei dem das Toolkit für die WinUI-Bibliothek (Windows-Benutzeroberfläche) verwendet wird, die gleichen vorläufigen Setupschritte aus. Weitere Hintergrundinformationen sowie Details zu Einrichtung und Unterstützung findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](/uwp/toolkits/winui/getting-started).

In diesem Beispiel wird veranschaulicht, wie Sie **NavigationView** sowohl mit einem oberen Navigationsbereich für große Fenster als auch mit einem linken Navigationsbereich für kleine Fenster verwenden können. Sie können eine Umstellung durchführen, bei der nur die Navigation links verwendet wird, indem Sie in **VisualStateManager** die Navigationseinstellungen für *top* entfernen.

Das Beispiel veranschaulicht eine empfohlene Vorgehensweise zum Einrichten von Navigationsdaten, die für viele gängige Szenarien geeignet ist. Darüber hinaus wird veranschaulicht, wie Sie die Rückwärtsnavigation mit der Zurück-Schaltfläche von **NavigationView** und der Navigation über die Tastatur implementieren.

In diesem Code wird vorausgesetzt, dass deine App Seiten mit den folgenden Namen enthält, auf die navigiert werden kann: *HomePage* , *AppsPage* , *GamesPage* , *MusicPage* , *MyContentPage* und *SettingsPage*. Der Code für diese Seiten ist nicht angegeben.

> [!IMPORTANT]
> Informationen zu den Seiten der App werden in einem [ValueTuple](/dotnet/api/system.valuetuple)-Element gespeichert. Für diese Struktur muss als Mindestversion für dein App-Projekt das SDK 17763 oder höher verwendet werden. Wenn du die WinUI-Version von „NavigationView“ für frühere Versionen von Windows 10 verwendest, kannst du stattdessen das [NuGet-Paket „System.ValueTuple NuGet“](https://www.nuget.org/packages/System.ValueTuple/) nutzen.

> [!IMPORTANT]
> Dieser Code veranschaulicht, wie du die NavigationView-Version der [Windows-UI-Bibliothek](/uwp/toolkits/winui/) verwendest. Wenn du stattdessen die Plattformversion von „NavigationView“ nutzt, muss die Mindestversion für dein App-Projekt „SDK 17763“ oder höher lauten. Entferne zum Verwenden der Plattformversion alle Verweise auf `muxc:`.

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
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
                        MinWindowWidth="{x:Bind NavViewCompactModeThresholdWidth}"/>
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
</Page>
```

> [!IMPORTANT]
> Dieser Code veranschaulicht, wie du die NavigationView-Version der [Windows-UI-Bibliothek](/uwp/toolkits/winui/) verwendest. Wenn du stattdessen die Plattformversion von „NavigationView“ nutzt, muss die Mindestversion für dein App-Projekt „SDK 17763“ oder höher lauten. Entferne zum Verwenden der Plattformversion alle Verweise auf `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private double NavViewCompactModeThresholdWidth { get { return NavView.CompactModeThresholdWidth; } }

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
    NavView_Navigate("home", new Windows.UI.Xaml.Media.Animation.EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = Windows.System.VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = Windows.System.VirtualKey.Left,
        Modifiers = Windows.System.VirtualKeyModifiers.Menu
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

private void NavView_Navigate(
    string navItemTag,
    Windows.UI.Xaml.Media.Animation.NavigationTransitionInfo transitionInfo)
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

> [!NOTE]
> Erstellen Sie für die [C++/WinRT](../../cpp-and-winrt-apis/index.md)-Version dieses Codebeispiels zunächst ein neues Projekt, das auf der Projektvorlage **Leere App (C++/WinRT)** basiert. Fügen Sie den aufgeführten Quellcodedateien dann den Code in der Auflistung hinzu. Benennen Sie Ihr neues Projekt *NavigationViewCppWinRT* , um den Quellcode genau wie in der Auflistung gezeigt zu verwenden.

```cppwinrt
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    ...
    Double NavViewCompactModeThresholdWidth{ get; };
}

// pch.h
...
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Media.Animation.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace wuxc
{
    using namespace winrt::Windows::UI::Xaml::Controls;
};

namespace winrt::NavigationViewCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        double NavViewCompactModeThresholdWidth();
        void ContentFrame_NavigationFailed(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args);
        void NavView_Loaded(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::RoutedEventArgs const& /* args */);
        void NavView_ItemInvoked(
            Windows::Foundation::IInspectable const& /* sender */,
            muxc::NavigationViewItemInvokedEventArgs const& args);

        // NavView_SelectionChanged is not used in this example, but is shown for completeness.
        // You'll typically handle either ItemInvoked or SelectionChanged to perform navigation,
        // but not both.
        void NavView_SelectionChanged(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewSelectionChangedEventArgs const& args);
        void NavView_Navigate(
            std::wstring navItemTag,
            Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo);
        void NavView_BackRequested(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewBackRequestedEventArgs const& /* args */);
        void BackInvoked(
            Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */,
            Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args);
        bool On_BackRequested();
        void On_Navigated(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationEventArgs const& args);

    private:
        // Vector of std::pair holding the Navigation Tag and the relative Navigation Page.
        std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;
    };
}

namespace winrt::NavigationViewCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::NavigationViewCppWinRT::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"home", winrt::xaml_typename<NavigationViewCppWinRT::HomePage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"apps", winrt::xaml_typename<NavigationViewCppWinRT::AppsPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"games", winrt::xaml_typename<NavigationViewCppWinRT::GamesPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"music", winrt::xaml_typename<NavigationViewCppWinRT::MusicPage>()));
    }

    double MainPage::NavViewCompactModeThresholdWidth()
    {
        return NavView().CompactModeThresholdWidth();
    }

    void MainPage::ContentFrame_NavigationFailed(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args)
    {
        throw winrt::hresult_error(
            E_FAIL, winrt::hstring(L"Failed to load Page ") + args.SourcePageType().Name);
    }

    // List of ValueTuple holding the Navigation Tag and the relative Navigation Page
    std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;

    void MainPage::NavView_Loaded(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        // You can also add items in code.
        NavView().MenuItems().Append(muxc::NavigationViewItemSeparator());
        muxc::NavigationViewItem navigationViewItem;
        navigationViewItem.Content(winrt::box_value(L"My content"));
        navigationViewItem.Icon(wuxc::SymbolIcon(static_cast<wuxc::Symbol>(0xF1AD)));
        navigationViewItem.Tag(winrt::box_value(L"content"));
        NavView().MenuItems().Append(navigationViewItem);
        m_pages.push_back(
            std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(
                L"content", winrt::xaml_typename<NavigationViewCppWinRT::MyContentPage>()));

        // Add handler for ContentFrame navigation.
        ContentFrame().Navigated({ this, &MainPage::On_Navigated });

        // NavView doesn't load any page by default, so load home page.
        NavView().SelectedItem(NavView().MenuItems().GetAt(0));
        // If navigation occurs on SelectionChanged, then this isn't needed.
        // Because we use ItemInvoked to navigate, we need to call Navigate
        // here to load the home page.
        NavView_Navigate(L"home",
            Windows::UI::Xaml::Media::Animation::EntranceNavigationTransitionInfo());

        // Add keyboard accelerators for backwards navigation.
        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);

        // ALT routes here
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        goBack.Key(Windows::System::VirtualKey::Left);
        goBack.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(altLeft);
    }

    void MainPage::NavView_ItemInvoked(
        Windows::Foundation::IInspectable const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        if (args.IsSettingsInvoked())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.InvokedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.InvokedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    // NavView_SelectionChanged is not used in this example, but is shown for completeness.
    // You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
    // but not both.
    void MainPage::NavView_SelectionChanged(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewSelectionChangedEventArgs const& args)
    {
        if (args.IsSettingsSelected())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.SelectedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.SelectedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    void MainPage::NavView_Navigate(
        std::wstring navItemTag,
        Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo)
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        if (navItemTag == L"settings")
        {
            pageTypeName = winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>();
        }
        else
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.first == navItemTag)
                {
                    pageTypeName = eachPage.second;
                    break;
                }
            }
        }
        // Get the page type before navigation so you can prevent duplicate
        // entries in the backstack.
        Windows::UI::Xaml::Interop::TypeName preNavPageType =
            ContentFrame().CurrentSourcePageType();

        // Navigate only if the selected page isn't currently loaded.
        if (pageTypeName.Name != L"" && preNavPageType.Name != pageTypeName.Name)
        {
            ContentFrame().Navigate(pageTypeName, nullptr, transitionInfo);
        }
    }

    void MainPage::NavView_BackRequested(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewBackRequestedEventArgs const& /* args */)
    {
        On_BackRequested();
    }

    void MainPage::BackInvoked(
        Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        On_BackRequested();
        args.Handled(true);
    }

    bool MainPage::On_BackRequested()
    {
        if (!ContentFrame().CanGoBack())
            return false;

        // Don't go back if the nav pane is overlaid.
        if (NavView().IsPaneOpen() &&
            (NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Compact ||
                NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Minimal))
            return false;

        ContentFrame().GoBack();
        return true;
    }

    void MainPage::On_Navigated(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationEventArgs const& args)
    {
        NavView().IsBackEnabled(ContentFrame().CanGoBack());

        if (ContentFrame().SourcePageType().Name ==
            winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>().Name)
        {
            // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
            NavView().SelectedItem(NavView().SettingsItem().as<muxc::NavigationViewItem>());
            NavView().Header(winrt::box_value(L"Settings"));
        }
        else if (ContentFrame().SourcePageType().Name != L"")
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.second.Name == args.SourcePageType().Name)
                {
                    for (auto&& eachMenuItem : NavView().MenuItems())
                    {
                        auto navigationViewItem =
                            eachMenuItem.try_as<muxc::NavigationViewItem>();
                        {
                            if (navigationViewItem)
                            {
                                winrt::hstring hstringValue =
                                    winrt::unbox_value_or<winrt::hstring>(
                                        navigationViewItem.Tag(), L"");
                                if (hstringValue == eachPage.first)
                                {
                                    NavView().SelectedItem(navigationViewItem);
                                    NavView().Header(navigationViewItem.Content());
                                }
                            }
                        }
                    }
                    break;
                }
            }
        }
    }
}
```

### <a name="alternative-cwinrt-implementation"></a>Alternative C++/WinRT-Implementierung

Der oben gezeigte C#- und C++/WinRT-Code ist so konzipiert, dass Sie für beide Versionen dasselbe XAML-Markup verwenden können. Es gibt jedoch eine weitere Möglichkeit, die in diesem Abschnitt beschriebene C++/WinRT-Version zu implementieren, die Sie möglicherweise bevorzugen.

Im Folgenden finden Sie eine alternative Version des **NavView_ItemInvoked** -Handlers. Das Verfahren in dieser Version des Handlers umfasst als Erstes das Speichern des vollständigen Typnamens der Seite (im Tag des [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)-Elements), zu der Sie navigieren möchten. Im Handler führst du das Unboxing für diesen Wert durch, wandelst ihn in ein [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename)-Objekt um und verwendest dieses Objekt, um zur Zielseite zu navigieren. Es ist nicht erforderlich, die Zuordnungsvariable mit dem Namen `_pages` aus dem obigen Beispiel zu nutzen. Sie können Komponententests erstellen, um zu bestätigen, dass die Werte in den Tags einen gültigen Typ aufweisen. Weitere Informationen findest du unter [Boxing und Unboxing von Skalarwerten für „IInspectable“ mit C++/WinRT](../../cpp-and-winrt-apis/boxing.md).

```cppwinrt
void MainPage::NavView_ItemInvoked(
    Windows::Foundation::IInspectable const & /* sender */,
    Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
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

## <a name="hierarchical-navigation"></a>Hierarchische Navigation
Einige Apps verfügen möglicherweise über eine komplexere hierarchische Struktur, die mehr als nur eine flache Liste von Navigationselementen erfordert. Möglicherweise möchten Sie Navigationselemente auf oberster Ebene verwenden, um Seitenkategorien anzuzeigen, und untergeordnete Elemente, um bestimmte Seiten anzuzeigen. Dies ist auch nützlich, wenn Sie über Seiten im Hub-Format verfügen, die nur als Link zu anderen Seiten dienen. In solchen Fällen sollten Sie eine hierarchische Navigationsansicht erstellen.

Verwenden Sie entweder die [MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitems?view=winui-2.4)-Eigenschaft oder die [MenuItemsSource](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitemssource?view=winui-2.4)-Eigenschaft von **NavigationViewItem** , um eine hierarchische Liste von verschachtelten Navigationselementen im Fensterbereich anzuzeigen.
Jedes NavigationViewItem kann andere NavigationViewItems und Organisationselemente wie Elementheader und Trennzeichen enthalten. Um bei Verwendung von `MenuItemsSource` eine hierarchische Liste anzuzeigen, legen Sie `ItemTemplate` als NavigationViewItem fest, und binden Sie seine `MenuItemsSource`-Eigenschaft an die nächste Ebene der Hierarchie.

Obwohl NavigationViewItem beliebig viele verschachtelte Ebenen enthalten kann, empfehlen wir, die Navigationshierarchie Ihrer App flach zu halten. Wir glauben, dass zwei Ebenen zum Zwecke der Verwendbarkeit und des Verständnisses ideal sind.

NavigationView zeigt die Hierarchie in den Bereichsanzeigemodi „Top“, „Left“ und „LeftCompact“ an. Im folgenden sehen Sie, wie eine erweiterte Unterstruktur in den einzelnen Bereichsanzeigemodi aussieht:

![NavigationView mit Hierarchie](images/navigation-view-hierarchy-labeled.png)

### <a name="adding-a-hierarchy-of-items-in-markup"></a>Hinzufügen einer Hierarchie von Elementen im Markup
Deklarieren Sie die App-Navigationshierarchie im Markup.

```Xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:NavigationView>
    <muxc:NavigationView.MenuItems>
        <muxc:NavigationViewItem Content="Home" Icon="Home" ToolTipService.ToolTip="Home"/>
        <muxc:NavigationViewItem Content="Collections" Icon="Keyboard" ToolTipService.ToolTip="Collections">
            <muxc:NavigationViewItem.MenuItems>
                <muxc:NavigationViewItem Content="Notes" Icon="Page" ToolTipService.ToolTip="Notes"/>
                <muxc:NavigationViewItem Content="Mail" Icon="Mail" ToolTipService.ToolTip="Mail"/>
            </muxc:NavigationViewItem.MenuItems>
        </muxc:NavigationViewItem>
    </muxc:NavigationView.MenuItems>
</muxc:NavigationView>
```

### <a name="adding-a-hierarchy-of-items-using-data-binding"></a>Hinzufügen einer Hierarchie von Elementen mithilfe der Datenbindung

Fügen Sie NavigationView eine Hierarchie von Menüelementen hinzu, durch 
* Binden der MenuItemsSource-Eigenschaft an die hierarchischen Daten
* Definieren der Elementvorlage als NavigationViewMenuItem, wobei als sein Inhalt die Bezeichnung des Menüelements festgelegt ist und seine MenuItemsSource-Eigenschaft an die nächste Ebene der Hierarchie gebunden ist

Dieses Beispiel veranschaulicht auch das [Erweitern](/uwp/api/microsoft.ui.xaml.controls.navigationview.expanding?view=winui-2.4) und [Reduzieren](/uwp/api/microsoft.ui.xaml.controls.navigationview.collapsed?view=winui-2.4) von Ereignissen. Diese Ereignisse werden für ein Menüelement mit untergeordneten Elementen ausgelöst.

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}" MenuItemsSource="{x:Bind Children}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}" 
    ItemInvoked="{x:Bind OnItemInvoked}" 
    Expanding="OnItemExpanding" 
    Collapsed="OnItemCollapsed" 
    PaneDisplayMode="Left">
            <StackPanel Margin="10,10,0,0">
                <TextBlock Margin="0,10,0,0" x:Name="ExpandingItemLabel" Text="Last Expanding: N/A"/>
                <TextBlock x:Name="CollapsedItemLabel" Text="Last Collapsed: N/A"/>
            </StackPanel>
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon" },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon" }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon" },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon" }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon" }
    };

    private void OnItemInvoked(object sender, NavigationViewItemInvokedEventArgs e)
    {
        var clickedItem = e.InvokedItem;
        var clickedItemContainer = e.InvokedItemContainer;
    }
    private void OnItemExpanding(object sender, NavigationViewItemExpandingEventArgs e)
    {
        var nvib = e.ExpandingItemContainer;
        var name = "Last expanding: " + nvib.Content.ToString();
        ExpandingItemLabel.Text = name;
    }
    private void OnItemCollapsed(object sender, NavigationViewItemCollapsedEventArgs e)
    {
        var nvib = e.CollapsedItemContainer;
        var name = "Last collapsed: " + nvib.Content;
        CollapsedItemLabel.Text = name;
    }
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        String Name;
        String CategoryIcon;
        Windows.Foundation.Collections.IObservableVector<Category> Children;
    }
}

// Category.h
#pragma once
#include "Category.g.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct Category : CategoryT<Category>
    {
        Category();
        Category(winrt::hstring name,
            winrt::hstring categoryIcon,
            Windows::Foundation::Collections::
                IObservableVector<HierarchicalNavigationViewDataBinding::Category> children);

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
        winrt::hstring CategoryIcon();
        void CategoryIcon(winrt::hstring const& value);
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> Children();
        void Children(Windows::Foundation::Collections:
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> const& value);

    private:
        winrt::hstring m_name;
        winrt::hstring m_categoryIcon;
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_children;
    };
}

// Category.cpp
#include "pch.h"
#include "Category.h"
#include "Category.g.cpp"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    Category::Category()
    {
        m_children = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    }

    Category::Category(
        winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> children)
    {
        m_name = name;
        m_categoryIcon = categoryIcon;
        m_children = children;
    }

    hstring Category::Name()
    {
        return m_name;
    }

    void Category::Name(hstring const& value)
    {
        m_name = value;
    }

    hstring Category::CategoryIcon()
    {
        return m_categoryIcon;
    }

    void Category::CategoryIcon(hstring const& value)
    {
        m_categoryIcon = value;
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        Category::Children()
    {
        return m_children;
    }

    void Category::Children(
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            const& value)
    {
        m_children = value;
    }
}

// MainPage.idl
import "Category.idl";

namespace HierarchicalNavigationViewDataBinding
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Windows.Foundation.Collections.IObservableVector<Category> Categories{ get; };
    }
}

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            Categories();

        void OnItemInvoked(muxc::NavigationView const& sender, muxc::NavigationViewItemInvokedEventArgs const& args);
        void OnItemExpanding(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemExpandingEventArgs const& args);
        void OnItemCollapsed(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemCollapsedEventArgs const& args);

    private:
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_categories;
    };
}

namespace winrt::HierarchicalNavigationViewDataBinding::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

#include "Category.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        m_categories = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

        auto menuItem10 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 10", L"Icon", nullptr);

        auto menuItem9 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 9", L"Icon", nullptr);
        auto menuItem8 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 8", L"Icon", nullptr);
        auto menuItem7Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem7Children.Append(*menuItem9);
        menuItem7Children.Append(*menuItem8);

        auto menuItem7 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 7", L"Icon", menuItem7Children);
        auto menuItem6Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem6Children.Append(*menuItem7);

        auto menuItem6 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 6", L"Icon", menuItem6Children);

        auto menuItem5 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 5", L"Icon", nullptr);
        auto menuItem4 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 4", L"Icon", nullptr);
        auto menuItem3Children =
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem3Children.Append(*menuItem5);
        menuItem3Children.Append(*menuItem4);

        auto menuItem3 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 3", L"Icon", menuItem3Children);
        auto menuItem2Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem2Children.Append(*menuItem3);

        auto menuItem2 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 2", L"Icon", menuItem2Children);
        auto menuItem1Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem1Children.Append(*menuItem2);

        auto menuItem1 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 1", L"Icon", menuItem1Children);

        m_categories.Append(*menuItem1);
        m_categories.Append(*menuItem6);
        m_categories.Append(*menuItem10);
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        MainPage::Categories()
    {
        return m_categories;
    }

    void MainPage::OnItemInvoked(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        auto clickedItem = args.InvokedItem();
        auto clickedItemContainer = args.InvokedItemContainer();
    }

    void MainPage::OnItemExpanding(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemExpandingEventArgs const& args)
    {
        auto nvib = args.ExpandingItemContainer();
        auto name = L"Last expanding: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        ExpandingItemLabel().Text(name);
    }

    void MainPage::OnItemCollapsed(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemCollapsedEventArgs const& args)
    {
        auto nvib = args.CollapsedItemContainer();
        auto name = L"Last collapsed: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        CollapsedItemLabel().Text(name);
    }
}
```

### <a name="selection"></a>Auswahl

Standardmäßig kann jedes Element untergeordnete Elemente enthalten, aufgerufen oder ausgewählt werden.

Wenn Sie Benutzern eine hierarchische Struktur von Navigationsoptionen bereitstellen, können Sie festlegen, dass übergeordnete Elemente nicht auswählbar sind, z. B. wenn Ihre App über keine diesem übergeordneten Element zugeordnete Zielseite verfügt. Wenn Ihre übergeordneten Elemente _auswählbar_ sind, empfiehlt es sich, die Anzeigemodi „Left-Expanded“ oder „Top“ zu verwenden. Der Modus „LeftCompact“ veranlasst den Benutzer, zum übergeordneten Element zu navigieren, um bei jedem Aufruf die untergeordnete Teilstruktur zu öffnen.

Die Auswahlindikatoren ausgewählter Elemente werden im Modus „Left“ entlang des linken Rands bzw. im Modus „Top“ entlang des unteren Rands gezeichnet. Unten sehen Sie NavigationViews in den Modi „Left“ und „Top“ bei Auswahl eines übergeordneten Elements.

![NavigationView im Modus „Left“ mit ausgewähltem übergeordneten Element](images/navigation-view-selection.png)

![NavigationView im Modus „Top“ mit ausgewähltem übergeordneten Element](images/navigation-view-selection-top.png)

Das ausgewählte Element ist möglicherweise nicht immer sichtbar. Wenn ein untergeordnetes Element in einer reduzierten/nicht erweiterten Unterstruktur ausgewählt ist, wird der erste sichtbare Vorgänger als ausgewählt angezeigt. Der Auswahlindikator wechselt zurück zum ausgewählten Element, wenn/sobald die Unterstruktur erweitert wird.

Beispiel: Angenommen, in der Abbildung oben wird das Kalenderelement vom Benutzer ausgewählt, und der Benutzer reduziert dann die Unterstruktur. In diesem Fall wird der Auswahlindikator unterhalb des Kontoelements angezeigt, da „Konto“ der erste sichtbare Vorgänger von „Kalender“ ist. Der Auswahlindikator wechselt zurück zum Kalenderelement, wenn der Benutzer die Unterstruktur wieder erweitert. 

In der gesamten NavigationView wird nicht mehr als ein Auswahlindikator angezeigt.

Durch Klicken auf die Pfeile von NavigationViewItems in den Modi „Top“ und „Left“ wird die Unterstruktur erweitert oder reduziert. Durch Klicken oder Tippen _an anderer Stelle_ von NavigationViewItem wird das `ItemInvoked`-Ereignis ausgelöst und zudem die Teilstruktur reduziert oder erweitert.

Um zu verhindern, dass ein Element beim Aufruf den Auswahlindikator anzeigt, legen Sie dessen [SelectsOnInvoked](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.selectsoninvoked?view=winui-2.3)-Eigenschaft auf „False“ fest, wie unten dargestellt:

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}"
            MenuItemsSource="{x:Bind Children}"
            SelectsOnInvoked="{x:Bind IsLeaf}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}">
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
    public bool IsLeaf { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon", IsLeaf = true },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon", IsLeaf = true }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon", IsLeaf = true },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon", IsLeaf = true }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon", IsLeaf = true }
    };
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        ...
        Boolean IsLeaf;
    }
}

// Category.h
...
struct Category : CategoryT<Category>
{
    ...
    Category(winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
        bool isleaf = false);
    ...
    bool IsLeaf();
    void IsLeaf(bool value);

private:
    ...
    bool m_isleaf;
};

// Category.cpp
...
Category::Category(winrt::hstring name,
    winrt::hstring categoryIcon,
    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
    bool isleaf) : m_name(name), m_categoryIcon(categoryIcon), m_children(children), m_isleaf(isleaf) {}
...
bool Category::IsLeaf()
{
    return m_isleaf;
}

void Category::IsLeaf(bool value)
{
    m_isleaf = value;
}

// MainPage.h and MainPage.cpp
// Delete OnItemInvoked, OnItemExpanding, and OnItemCollapsed.

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    m_categories = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

    auto menuItem10 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 10", L"Icon", nullptr, true);

    auto menuItem9 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 9", L"Icon", nullptr, true);
    auto menuItem8 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 8", L"Icon", nullptr, true);
    auto menuItem7Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem7Children.Append(*menuItem9);
    menuItem7Children.Append(*menuItem8);

    auto menuItem7 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 7", L"Icon", menuItem7Children);
    auto menuItem6Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem6Children.Append(*menuItem7);

    auto menuItem6 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 6", L"Icon", menuItem6Children);

    auto menuItem5 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 5", L"Icon", nullptr, true);
    auto menuItem4 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 4", L"Icon", nullptr, true);
    auto menuItem3Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem3Children.Append(*menuItem5);
    menuItem3Children.Append(*menuItem4);

    auto menuItem3 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 3", L"Icon", menuItem3Children);
    auto menuItem2Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem2Children.Append(*menuItem3);

    auto menuItem2 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 2", L"Icon", menuItem2Children);
    auto menuItem1Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem1Children.Append(*menuItem2);

    auto menuItem1 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 1", L"Icon", menuItem1Children);

    m_categories.Append(*menuItem1);
    m_categories.Append(*menuItem6);
    m_categories.Append(*menuItem10);
}
...
```

### <a name="keyboarding-within-hierarchical-navigationview"></a>Tastaturunterstützung innerhalb einer hierarchischen NavigationView

Benutzer können den Fokus mithilfe ihrer [Tastatur](../input/keyboard-interactions.md) im der Navigationsansicht bewegen. Die Pfeiltasten machen innerhalb des Bereichs die „innere Navigation“ verfügbar und folgen den in der [Strukturansicht](./tree-view.md) bereitgestellten Interaktionen. Die Tastenaktionen ändern sich während der Navigation in der NavigationView oder dem zugehörigen Flyoutmenü, das in den Modi „Top“ und „Left-compact“ von HierarchicalNavigationView angezeigt wird. Im folgenden finden Sie die speziellen Aktionen, die jede Taste in einer hierarchischen NavigationView ausführen kann:

| Schlüssel      |      Im Modus „Left“      |  Im Modus „Top“ | Im Flyout  |
|----------|------------------------|--------------|------------|
| Nach oben |Verschiebt den Fokus auf das Element direkt oberhalb des Elements, das aktuell den Fokus hält. | Führt keine Aktion aus. |Verschiebt den Fokus auf das Element direkt oberhalb des Elements, das aktuell den Fokus hält.|
| Nach unten|Verschiebt den Fokus auf das Element direkt unterhalb des Elements, das aktuell den Fokus hält.* | Führt keine Aktion aus. | Verschiebt den Fokus auf das Element direkt unterhalb des Elements, das aktuell den Fokus hält.* |
| Rechts |Führt keine Aktion aus.  |Verschiebt den Fokus auf das Element direkt rechts von dem Element, das aktuell den Fokus hält. |Führt keine Aktion aus.|
| Links |Führt keine Aktion aus. | Verschiebt den Fokus auf das Element direkt links von dem Element, das aktuell den Fokus hält.  |Führt keine Aktion aus. |
| LEERTASTE/EINGABETASTE |Wenn das Element untergeordnete Elemente aufweist, wird das Element erweitert oder reduziert, und der Fokus wird nicht geändert.   | Wenn das Element über untergeordnete Elemente verfügt, werden die untergeordneten Elemente in ein Flyout erweitert, und der Fokus wird auf das erste Element im Flyout gesetzt. | Ruft ein Element auf oder wählt ein Element aus und schließt das Flyout. |
| ESC | Führt keine Aktion aus. | Führt keine Aktion aus. | Schließt das Flyout.|

Die LEERTASTE oder die EINGABETASTE bewirkt immer, dass ein Element aufgerufen bzw. ausgewählt wird.

*Beachten Sie, dass die Elemente nicht visuell aneinander angrenzend angeordnet sein müssen. Der Fokus wird vom letzten Element in der Liste des Bereichs auf das Einstellungselement verschoben. 

## <a name="navigation-view-customization"></a>Anpassung der Navigationsansicht

### <a name="pane-backgrounds"></a>Bereichshintergründe

Standardmäßig wird je nach Anzeigemodus im NavigationView-Bereich ein anderer Hintergrund verwendet:

- Der Bereich ist grau, wenn er nach links erweitert wird (neben dem Inhalt, im Modus „Links“).
- Für den Bereich wird In-App-Acryl verwendet, wenn er als Überlagerung auf dem Inhalt geöffnet wird (im Modus „Oben“, „Minimal“ oder „Kompakt“).

Zum Ändern des Bereichshintergrunds kannst du die XAML-Designressourcen außer Kraft setzen, die in den einzelnen Modi zum Rendern des Hintergrunds verwendet werden. (Dieses Verfahren wird anstelle einer einzelnen PaneBackground-Eigenschaft genutzt, um unterschiedliche Hintergründe für unterschiedliche Anzeigemodi zu unterstützen.)

In dieser Tabelle ist angegeben, welche Designressource in den unterschiedlichen Anzeigemodi verwendet wird.

| Anzeigemodus | Designressource |
| ------------ | -------------- |
| Links | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Oben | NavigationViewTopPaneBackground |

In diesem Beispiel wird veranschaulicht, wie du die Designressourcen in „App.xaml“ außer Kraft setzt. Beim Außerkraftsetzen der Designressourcen solltest du immer mindestens die Ressourcenwörterbücher „Default“ und „HighContrast“ sowie je nach Bedarf Wörterbücher für Ressourcen vom Typ „Light“ oder „Dark“ bereitstellen. Weitere Informationen findest du unter [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Dieser Code veranschaulicht, wie du die AcrylicBrush-Version der [Windows-UI-Bibliothek](/uwp/toolkits/winui/) verwendest. Wenn du stattdessen die Plattformversion von „AcrylicBrush“ nutzt, muss die Mindestversion für dein App-Projekt „SDK 16299“ oder höher lauten. Entferne zum Verwenden der Plattformversion alle Verweise auf `muxm:`.

```xaml
<Application ... xmlns:muxm="using:Microsoft.UI.Xaml.Media" ...>
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

> Für die `IsTitleBarAutoPaddingEnabled`-Eigenschaft ist die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) 2,2 oder höher erforderlich.

Einige Apps [passen die Titelleiste ihres Fensters an](../shell/title-bar.md) und erweitern so den App-Inhalt auf den Bereich der Titelleiste. Wenn NavigationView das Stammelement in Apps ist, die ihren Inhalt **mithilfe der [ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar)-API** in die Titelleiste erweitern, passt das Steuerelement die Position seiner interaktiven Elemente automatisch an, um eine Überlappung mit dem [ziehbaren Bereich](../shell/title-bar.md#draggable-regions) zu vermeiden.

![Eine App, die ihren Inhalt in die Titelleiste erweitert](images/navigation-view-with-titlebar-padding.png)

Wenn Ihre App den ziehbaren Bereich durch Aufrufen der [Window.SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar)-Methode angibt und Sie die Schaltflächen „Zurück“ und „Menü“ lieber oben im App-Fenster anordnen möchten, legen Sie [IsTitleBarAutoPaddingEnabled](/uwp/api/microsoft.ui.xaml.controls.navigationview.istitlebarautopaddingenabled) auf **false** fest.

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

Diese Themenressource ändert den Rand um [NavigationView.Header](/uwp/api/windows.ui.xaml.controls.navigationview.header).

## <a name="related-topics"></a>Zugehörige Themen

- [NavigationView-Klasse](/uwp/api/windows.ui.xaml.controls.navigationview)
- [Master/Details](master-details.md)
- [Navigationsgrundlagen](../basics/navigation-basics.md)
- [Fluent Design-Übersicht](/windows/apps/fluent-design-system)
