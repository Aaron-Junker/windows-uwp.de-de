---
Description: NavigationView ist eine adaptive-Steuerelement, das Muster der Navigation auf oberster Ebene für Ihre app implementiert.
title: Navigationsansicht
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4ba3a45701d82ad0b43591469bf390190ec18db0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642225"
---
# <a name="navigation-view"></a>Navigationsansicht

Das NavigationView-Steuerelement stellt die Navigation auf oberster Ebene für Ihre app bereit. Es passt sich an eine Vielzahl von Bildschirmgrößen und unterstützt _oben_ und _linken_ navigationsformaten.

![oben im Navigationsbereich](images/nav-view-header.png)<br/>
_Navigationsansicht unterstützt sowohl oben und links im Navigationsbereich oder im Menü_

> **Plattform-APIs**: [Windows.UI.Xaml.Controls.NavigationView-Klasse](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows-APIs für UI-Bibliothek**: [Microsoft.UI.Xaml.Controls.NavigationView-Klasse](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Einige Features von NavigationView, z. B. _oben_ Navigation erfordert Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

NavigationView handelt es sich um eine adaptive Navigationssteuerelement, das für gut funktioniert:

- Bereitstellen einer konsistenten Navigieren in der gesamten app.
- Platz auf dem Bildschirm auf kleineres wird beibehalten.
- Organisieren den Zugriff auf viele Navigationskategorien an.

Andere navigationsmuster finden Sie unter [Grundlagen des Berichtsentwurfs Navigation](../basics/navigation-basics.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/NavigationView">die App zu öffnen und NavigationView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Anzeigemodi

> Die PaneDisplayMode-Eigenschaft erfordert Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

Sie können mithilfe der Eigenschaft PaneDisplayMode verschiedene navigationsformaten konfigurieren oder Anzeigemodi für die NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Oben
    Im Bereich befindet sich über den Inhalt.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für die oben im Navigationsbereich](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Es wird empfohlen _oben_ Navigation beim:

- Sie haben 5 oder weniger Kategorien der Navigation auf oberster Ebene, die gleichermaßen wichtig sind und weitere Navigation auf oberster Ebene, die Kategorien, die im Überlaufmenü Dropdownmenü am Ende weniger als wichtig betrachtet zu werden.
- Sie müssen alle Navigationsoptionen für die auf dem Bildschirm anzuzeigen.
- Sie möchten mehr Speicherplatz für Ihre app-Inhalte.
- Symbole können nicht klar Ihrer app Navigationskategorien beschreiben.

:::row:::
    :::column:::
    ### <a name="left"></a>Nach links
    Der Bereich wird erweitert und positioniert auf der linken Seite des Inhalts.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für die erweiterte links im Navigationsbereich](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Es wird empfohlen _linken_ Navigation beim:

- Sie haben die Navigation für ebenso wichtig auf oberster Ebene Kategorien 5 bis 10.
- Von Navigationskategorien zu sehr gut sichtbaren, weniger Platz für andere app-Inhalte werden sollen.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    Der Bereich zeigt nur die Symbole erst geöffnet und, auf der linken Seite des Inhalts positioniert ist.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für kompakte links im Navigationsbereich](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Nur die Menüschaltfläche wird angezeigt, bis der Bereich geöffnet ist. Beim Öffnen, wird es auf der linken Seite des Inhalts positioniert.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Beispiel für die minimale links im Navigationsbereich](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

Standardmäßig wird die PaneDisplayMode auf Auto festgelegt. Im Auto-Modus die Navigationsansicht passt zwischen LeftMinimal, wenn das Fenster schmal sein, um LeftCompact ist, und klicken Sie dann auf die Links im Fenster vergrößert wird. Weitere Informationen finden Sie in der [adaptive Verhalten](#adaptive-behavior) Abschnitt.

![Adaptive Standardverhalten linken Navigationsbereich](images/displaymode-auto.png)<br/>
_Navigationsbereich anzeigen adaptive Standardverhalten_

## <a name="anatomy"></a>Aufbau

Diese Abbildungen zeigen das Layout der Bereich, Header und Content-Bereiche des Steuerelements, wenn für konfiguriert _oben_ oder _linken_ Navigation.

![Layout von oben im Navigationsbereich anzeigen](images/topnav-anatomy.png)<br/>
_Layout der oberen Navigationsleiste_

![Linken Ansichtslayout](images/leftnav-anatomy.png)<br/>
_Layout der linken Navigationsleiste_

### <a name="pane"></a>Bereich

Sie können die PaneDisplayMode-Eigenschaft verwenden, um den Bereich oberhalb des Inhalts oder auf der linken Seite des Inhalts zu positionieren.

Im Bereich von NavigationView kann Folgendes enthalten:

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) Objekte. Der Navigationselemente zu bestimmten Seiten zu navigieren.
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) Objekte. Trennzeichen für die Navigationselemente zu gruppieren. Legen Sie die [Deckkraft](/uwp/api/windows.ui.xaml.uielement.opacity) Eigenschaft auf 0, um das Trennzeichen als Leerzeichen rendern.
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) Objekte. Die Header für die Bezeichnung von Gruppen von Elementen.
- Eine optionale [AutoSuggestBox](auto-suggest-box.md) Steuerelement für die Suche von app-Ebene ermöglichen. Weisen Sie das Steuerelement, das die [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) Eigenschaft.
- Ein optionaler Einstiegspunkt für [App-Einstellungen](../app-settings/app-settings-and-data.md). Um das Settings-Element auszublenden, legen die [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) Eigenschaft **"false"**.

Der linke Bereich enthält auch:

- Eine Menüschaltfläche zum Umschalten der Bereich geöffnet und geschlossen werden soll. Sie können bei größeren App-Fenstern mit geöffnetem Bereich diese Schaltfläche mit der Eigenschaft [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) ausblenden.

Die Navigationsansicht verfügt über eine Schaltfläche "zurück", die in der oberen linken Ecke des Bereichs platziert wird. Es jedoch nicht automatisch Behandeln der Navigation rückwärts und Inhalt in den Backstack hinzufügen. Um die Navigation rückwärts zu aktivieren, finden Sie unter den [rückwärts Navigation](#backwards-navigation) Abschnitt.

So sieht die Struktur der Bereich ' detaillierte ' für die Position des oberen und linken Bereich aus.

#### <a name="top-navigation-pane"></a>Oben im Navigationsbereich

![Navigationsbereich anzeigen im oberen Bereich Aufbau](images/navview-pane-anatomy-horizontal.png)

1. Header
1. Navigationselementen
1. Trennzeichen
1. AutoSuggestBox (optional)
1. Schaltfläche "Einstellungen" (optional)

#### <a name="left-navigation-pane"></a>Im linken Navigationsbereich

![Links im Bereich Aufbau Navigationsansicht](images/navview-pane-anatomy-vertical.png)

1. Menü-Taste
1. Navigationselementen
1. Trennzeichen
1. Header
1. AutoSuggestBox (optional)
1. Schaltfläche "Einstellungen" (optional)

#### <a name="pane-footer"></a>Bereich Fußzeile

Sie können Freiform-Inhalt in den Bereich der Fußzeile platzieren, durch das Hinzufügen, damit die [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) Eigenschaft.

:::row:::
    :::column:::
    ![Bereich Fußzeile Top nav](images/navview-freeform-footer-top.png)<br>
     _Im oberen Bereich Fußzeile_<br>
    :::column-end:::
    :::column:::
    ![Bereich Fußzeile linke Navigationsleiste](images/navview-freeform-footer-left.png)<br>
    _Fußzeile für Links_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Titel und header

Sie können Text-Inhalt in den Headerbereich Bereich einfügen, durch Festlegen der [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) Eigenschaft. Es nimmt eine Zeichenfolge und zeigt den Text neben der Menüschaltfläche.

Zum Hinzufügen von nicht-Text-Inhalt, z. B. ein Bild oder Logo, können Sie jedes Element in den Bereich des Headers platzieren, fügen es zu den [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) Eigenschaft.

Wenn sowohl für PaneTitle PaneHeader festgelegt werden, wird der Inhalt neben der Menüschaltfläche, mit der auf die Menüschaltfläche am nächsten PaneTitle horizontal gestapelt.

:::row:::
    :::column:::
    ![Bereich Header Top nav](images/navview-freeform-header-top.png)<br>
     _Im oberen Bereich-header_<br>
    :::column-end:::
    :::column:::
    ![Bereich Header linke Navigationsleiste](images/navview-freeform-header-left.png)<br>
    _Im linken Bereich-header_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Im Bereich Inhalt

Sie können Freiform-Inhalte in den Bereich platzieren, durch das Hinzufügen, damit die [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) Eigenschaft.

:::row:::
    :::column:::
    ![Bereich benutzerdefinierten Content Top nav](images/navview-freeform-pane-top.png)<br>
     _Im oberen Bereich benutzerdefinierten Inhalt_<br>
    :::column-end:::
    :::column:::
    ![Bereich benutzerdefinierten Inhalt Linker Navigationsbereich](images/navview-freeform-pane-left.png)<br>
    _Benutzerdefinierten Inhalt für Links_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Header

Sie können den Titel einer Seite hinzufügen, durch Festlegen der [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) Eigenschaft.

![Beispiel für die Navigation Ansicht-Kopfzeilenbereich](images/nav-header.png)<br/>
_Header der Navigation_

Der Headerbereich die Navigationsschaltfläche "an der Position der linken Seite vertikal ausgerichtet ist, und unten in die Position im oberen Bereich liegt. Es hat eine feste Höhe von 52 px. Er dient dazu, den Seitentitel der ausgewählten Navigationskategorie aufrechtzuerhalten. Die Kopfzeile ist an den oberen Rand der Seite angedockt und dient als Scroll-Clipping-Punkt für den Inhaltsbereich.

Die Kopfzeile ist jedes Mal, wenn NavigationView im minimalen Anzeigemodus ist sichtbar. Sie können die Kopfzeile in anderen Modi ausblenden, die bei größeren Fensterbreiten verwendet werden. Um die Header auszublenden, legen die [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) Eigenschaft **"false"**.

### <a name="content"></a>Inhalt

![Beispiel des Inhaltsbereichs des Navigations-anzeigen](images/nav-content.png)<br/>
_Navigationsbereich anzeigen von Inhalten_

Im Inhaltsbereich werden die meisten Informationen für die ausgewählte Navigationskategorie angezeigt.

Es 12px Ränder für Ihre Inhaltsbereich wird empfohlen, wenn NavigationView wird **minimale** Modus und 24px Ränder andernfalls.

## <a name="adaptive-behavior"></a>Adaptives Verhalten

Standardmäßig wird die Navigationsansicht automatisch der Anzeigemodus, der basierend auf der Menge der verfügbaren Platz auf dem Bildschirm geändert. Die [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) und [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) Eigenschaften angeben, die Haltepunkte, an dem der Anzeigemodus geändert wird. Sie können diese Werte zum Anpassen des Verhaltens im adaptive Anzeige ändern.

### <a name="default"></a>Standard

Wenn PaneDisplayMode festgelegt ist, auf den Standardwert des **automatisch**, wird das Verhalten das adaptive erläutert:

- Eine erweiterte links auf großen Fensters breiten (1008px oder höher).
- Eine verlassen, nur das Symbol, Navigationsbereich (LeftCompact) auf der mittleren Fenster breiten (641px zu 1007px).
- Nur eine Menüschaltfläche (LeftMinimal) auf kleinen Fensters breiten (640px oder weniger).

Weitere Informationen zu Fenstergrößen für adaptive Verhalten, finden Sie unter [Bildschirm Größen und Haltepunkte](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Adaptive Standardverhalten linken Navigationsbereich](images/displaymode-auto.png)<br/>
_Navigationsbereich anzeigen adaptive Standardverhalten_

### <a name="minimal"></a>Minimiert

Ein zweites adaptive häufiges ist die Verwendung einen erweiterten linken Bereich auf großen Fensters Breiten und nur eine Menüschaltfläche am vorzusehen mittelständische und kleine Fenster.

Es wird empfohlen, diese Lösung, wenn:

- Mehr Speicherplatz möchten für den Inhalt der app auf kleineren Fenster breiten.
- Ihre Navigationskategorien werden nicht eindeutig durch Symbole dargestellt.

![Linken minimale adaptive Verhalten](images/adaptive-behavior-minimal.png)<br/>
_Ansicht "minimal" adaptive Navigationsverhalten_

Um dieses Verhalten zu konfigurieren, legen Sie CompactModeThresholdWidth, der die Breite unter der den Bereich reduziert werden soll. Hier wird es vom Standardwert von 640 auf 1007 geändert. Sie sollten auch festlegen, dass ExpandedModeThresholdWidth, um sicherzustellen, dass die Werte, die keine Konflikte auftreten.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Kompakt

Ein dritter adaptive häufiges ist die Verwendung einen erweiterten linken Bereich auf großen Fensters Breiten und eine LeftCompact, Symbol ausschließlich Navigationsbereich auf vorzusehen mittelständische und kleine Fenster.

Es wird empfohlen, diese Lösung, wenn:

- Es ist wichtig, immer alle Navigationsoptionen auf dem Bildschirm anzeigen.
- Ihre Navigationskategorien können deutlich durch Symbole dargestellt werden.

![Linken compact adaptive Verhalten](images/adaptive-behavior-compact.png)<br/>
_Ansicht "komprimieren" adaptive Navigationsverhalten_

Um dieses Verhalten zu konfigurieren, CompactModeThresholdWidth auf 0 festgelegt.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Kein adaptive Verhalten

Um das automatische adaptive Verhalten zu deaktivieren, legen Sie PaneDisplayMode auf einen anderen Wert als automatisch ein. Hier wird festgelegt, LeftMinimal, also nur auf die Menüschaltfläche angezeigt wird, unabhängig davon die Fensterbreite.

![Linke Navigationsleiste kein adaptive Verhalten](images/adaptive-behavior-none.png)<br/>
_Navigationsansicht mit PaneDisplayMode LeftMinimal festgelegt_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Wie zuvor beschrieben in der _Anzeigemodi_ Abschnitt können Sie den Bereich, um immer am oberen Rand immer erweitert, immer compact oder immer minimal sein festlegen. Sie können auch die Anzeigemodi selbst in Ihrem app-Code verwalten. Ein Beispiel dafür wird im nächsten Abschnitt angezeigt.

### <a name="top-to-left-navigation"></a>Oben auf der linken Navigationsleiste

Wenn Sie oben im Navigationsbereich in Ihrer app verwenden, reduzieren das Fenster sich Breite verringert Navigationselementen in ein Überlaufmenü. Wenn Ihre app-Fenster schmaler ist, erhalten sie möglicherweise eine bessere benutzererfahrung zum Wechseln der PaneDisplayMode von oben auf LeftMinimal Navigation, anstatt zuzulassen, dass alles, was die Elemente in der Überlauf zu reduzieren.

Es wird empfohlen, oben im Navigationsbereich zur Größe des großen Fensters und linken Navigationsbereich auf kleinen Fenster Größen, wenn:

- Sie haben einen Satz von gleichermaßen wichtig der obersten Ebene Navigationskategorien zusammen angezeigt werden, wenn eine Kategorie in diesem Satz nicht auf den Bildschirm passt, zum linken Navigationsbereich, um ihnen einen gleicher Wichtigkeit gewähren Sie reduzieren.
- Möchten Sie so viel Inhalt Speicherplatz wie möglich in kleinen Fenstergrößen zu erhalten.

Dieses Beispiel zeigt, wie Sie mit einem [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) und [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) Eigenschaft, um zwischen den oberen und LeftMinimal Navigation zu wechseln.

![Beispiel für die oberen oder linken adaptive Verhalten 1](images/navigation-top-to-left.png)

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
> Bei Verwendung von AdaptiveTrigger.MinWindowWidth wird der visuelle Zustand ausgelöst, wenn das Fenster breiter als die angegebene minimale Breite ist. Dies bedeutet, dass der Standard-XAML definiert das schmale Fenster und VisualState definiert die Änderungen, die angewendet werden, wenn das Fenster vergrößert wird. Der Standardwert PaneDisplayMode für die Navigationsansicht automatisch ist, sodass, wenn die Fensterbreite kleiner als oder gleich CompactModeThresholdWidth, ist LeftMinimal Navigation verwendet wird. Wenn das Fenster vergrößert wird, VisualState überschreibt den Standardnamen, und oben im Navigationsbereich wird verwendet.

## <a name="navigation"></a>Navigation

Alle Navigationsaufgaben nicht automatisch die Navigationsansicht ausgeführt werden. Wenn der Benutzer auf ein Element tippt, wird die Navigationsansicht zeigt das Element als aktiviert und löst eine [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) Ereignis. Wenn das Tap in ein neues Element ausgewählt wird, führt eine [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) Ereignis wird auch ausgelöst.

Sie können entweder Ereignis aus, um Aufgaben im Zusammenhang mit der angeforderten Navigation behandeln. Sie behandeln soll hängt das Verhalten, das Sie für Ihre app verwenden möchten. Normalerweise navigieren Sie zu die angeforderte Seite und aktualisieren die Ansicht Navigationsheader als Reaktion auf diese Ereignisse.

**ItemInvoked** wird jedes Mal, die der Benutzer tippt auf einem Navigationselement ausgelöst, selbst wenn sie bereits ausgewählt ist. (Das Element kann auch mit der entsprechenden Aktion mit der Maus, Tastatur oder andere Eingaben aufgerufen werden. Weitere Informationen finden Sie unter [Eingabe- und Interaktionen](../input/index.md).) Wenn Sie im Ereignishandler ItemInvoked navigieren, wird standardmäßig die Seite neu geladen, und ein duplizierten Eintrag wird von dem Navigationsstapel hinzugefügt. Wenn Sie navigieren, wenn ein Element aufgerufen wird, sollten Sie nicht zulassen, die Seite erneut zu laden oder stellen Sie sicher, dass ein duplizierten Eintrag nicht in der Navigation BackStack-erstellt wird, wenn die Seite erneut geladen wird. (Siehe Beispiele für Authentifizierungscode.)

**SelectionChanged** ausgelöst werden kann, von einem Benutzer ein Element, das derzeit ausgewählt ist nicht aufrufen oder das ausgewählte Element programmgesteuert zu ändern. Wenn die Änderung der Auswahl tritt auf, da ein Benutzer ein Element aufgerufen wird, Eintritt des ItemInvoked-Ereignisses. Wenn die Änderung der Auswahl programmgesteuerte ist, wird die ItemInvoked nicht ausgelöst.

### <a name="backwards-navigation"></a>Rückwärtsnavigation

NavigationView ist eine integrierte Schaltfläche "zurück"; aber wie bei der Navigationsverlauf vor, es nicht rückwärts Navigation automatisch ausgeführt. Wenn der Benutzer die zurück-Schaltfläche tippt der [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) Ereignis wird ausgelöst. Sie behandeln dieses Ereignis, um die Navigation rückwärts durchgeführt. Weitere Informationen und Codebeispiele finden Sie unter [Navigationsverlauf und Abwärtskompatibilität Navigation](../basics/navigation-history-and-backwards-navigation.md).

Im Modus mit minimaler oder Compact für die Navigationsansicht Bereich als ein Flyout geöffnet ist. In diesem Fall auf die Schaltfläche "zurück" schließt den Bereich und Auslösen der **PaneClosing** Ereignis stattdessen.

Sie können ausblenden oder deaktivieren die Schaltfläche "zurück", durch Festlegen dieser Eigenschaften:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): ein-und Ausblenden der Schaltfläche "zurück" verwenden. Diese Eigenschaft akzeptiert den Wert der [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) -Enumeration und nastaven NA hodnotu **automatisch** standardmäßig. Wenn die Schaltfläche mit der reduziert ist, ist kein Speicherplatz im Layout dafür reserviert.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): Verwenden Sie zum Aktivieren oder deaktivieren die Schaltfläche "zurück". Sie können eine Datenbindung dieser Eigenschaft auf die [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) Ihre Navigationsframe Eigenschaft. **BackRequested** wird nicht ausgelöst, wenn **IsBackEnabled** ist **"false"**.

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

Dieses Beispiel zeigt, wie Sie mit einem oberen Navigationsbereich auf großen Fenstergrößen und einen linken Navigationsbereich auf kleine Fenstergrößen NavigationView verwenden können. Möglich durch das Entfernen Navigation mit Links an den _oben_ navigationseinstellungen für die in der VisualStateManager.

Das Beispiel veranschaulicht die Navigation einrichten, die für viele allgemeine Szenarien funktioniert am besten. Es veranschaulicht auch Abwärtskompatibilität Navigation mithilfe NavigationViews Rückwärtsnavigation Schaltfläche und die Tastatur zu implementieren.

Dieser Code setzt voraus, dass Ihre app Seiten mit den folgenden Namen, zu dem navigiert enthält: _HomePage_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_, und _SettingsPage_ . Code für diese Seiten wird nicht angezeigt.

> [!IMPORTANT]
> Informationen zu den Seiten der app befindet sich in einem [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Diese Struktur ist erforderlich, dass die Mindestversion für Ihr app-Projekt SDK 17763 oder größer sein muss. Wenn Sie die Version WinUI von NavigationView zu frühere Versionen von Windows 10 als Ziel verwenden, können Sie die [System.ValueTuple-NuGet-Paket](https://www.nuget.org/packages/System.ValueTuple/) stattdessen.

> [!IMPORTANT]
> Dieser Code zeigt, wie Sie mit der [Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) Version von NavigationView. Wenn Sie stattdessen die Version der Plattform von NavigationView verwenden, muss die Mindestversion für Ihr app-Projekt SDK 17763 oder höher sein. Um die Plattformversion zu verwenden, entfernen Sie alle Verweise auf `muxc:`.

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
> Dieser Code zeigt, wie Sie mit der [Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) Version von NavigationView. Wenn Sie stattdessen die Version der Plattform von NavigationView verwenden, muss die Mindestversion für Ihr app-Projekt SDK 17763 oder höher sein. Um die Plattformversion zu verwenden, entfernen Sie alle Verweise auf `muxc`.

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

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
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

Im folgenden finden Sie eine [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/index) Version der **NavView_ItemInvoked** -Handler aus der C# obenstehenden Codebeispiel wird. Die Technik in C++ / WinRT-Ereignishandler meldet man zuerst speichern (im Tag der [ **NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) der vollständige Name der Seite auf die Sie navigieren möchten. In den Handler auf, können Sie auch diesen Wert mittels Unboxing zu konvertieren, aktivieren Sie ihn in eine [ **TypeName** ](/uwp/api/windows.ui.xaml.interop.typename) -Objekt und verwenden, um zu der Seite "Ziel" zu navigieren. Besteht keine Notwendigkeit für die Mapping-Variable, die mit dem Namen `_pages` , die Sie sehen, der C# Beispiel; und Sie werden zum Erstellen von Komponententests, die bestätigt, dass die Werte in die Tags einen gültigen Typ aufweisen. Siehe auch [Boxing und unboxing Skalare Werte für IInspectable mit C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

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

## <a name="navigation-view-customization"></a>Anpassung der Navigation-Ansicht

### <a name="pane-backgrounds"></a>Im Bereich von Hintergründen

Standardmäßig verwendet der NavigationView-Bereich einen anderen Hintergrund je nach den Anzeigemodus an:

- der Bereich ist eine solide graue Farbe, wenn auf der linken Seite-an-Seite mit dem Inhalt (im linken Modus) erweitert.
- im Bereich wird in-app-Blog beim Öffnen als Overlay über Inhalt (im oben, minimale oder Compact-Modus).

Um den Hintergrund zu ändern, können Sie die XAML-Design-Ressourcen verwendet, um den Hintergrund in jedem Modus Rendern überschreiben. (Diese Technik werden anstatt einer einzelnen PaneBackground-Eigenschaft verwendet, um andere Hintergründe für verschiedene Anzeigemodi zu unterstützen.)

Diese Tabelle zeigt die Design-Ressourcen in jeder Anzeigemodus verwendet wird.

| Anzeigemodus | Design-Ressourcen |
| ------------ | -------------- |
| Nach links | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Oben | NavigationViewTopPaneBackground |

Dieses Beispiel zeigt, wie die Designressourcen in "App.xaml" überschrieben wird. Wenn Sie Designressourcen überschreiben, sollten Sie immer "Default" und "Hohem Kontrast" Ressourcenverzeichnisse auf ein Minimum und Wörterbücher für "Hell" oder "Dunkel" Ressourcen bereitstellen nach Bedarf. Weitere Informationen finden Sie unter [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Dieser Code zeigt, wie Sie mit der [Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) AcrylicBrush Version. Wenn Sie stattdessen die Version der Plattform der AcrylicBrush verwenden, muss die Mindestversion für Ihr app-Projekt SDK 16299 oder höher sein. Um die Plattformversion zu verwenden, entfernen Sie alle Verweise auf `muxm:`.

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

## <a name="related-topics"></a>Verwandte Themen

- [NavigationView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Master/Details](master-details.md)
- [Navigationsgrundlagen](../basics/navigation-basics.md)
- [Fluent Design für UWP-Übersicht](../fluent-design-system/index.md)
