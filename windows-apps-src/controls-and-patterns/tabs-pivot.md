---
author: Jwmsft
Description: "Registerkarten und Pivots ermöglichen Benutzern die Navigation zwischen häufig verwendeten Inhalten."
title: Registerkarten und Pivots
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a652189d0595bb6dffbc5228a91a68261eb7e5ef
ms.openlocfilehash: 865969a7c965a502acd9fe24df28128399efd380

---
# <a name="pivot-and-tabs"></a>Pivot und Registerkarten

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Das Pivot-Steuerelement und das verwandte Registerkartenmuster werden für die Navigation zwischen häufig genutzten verschiedenen Inhaltskategorien verwendet. Pivots ermöglichen die Navigation zwischen mindestens zwei Inhaltsbereichen und nutzen Textheader, um die verschiedenen Inhaltsabschnitte zu gliedern.

![Beispiel für Registerkarten](images/pivot_Hero_main.png)

Registerkarten stellen eine visuelle Variante eines Pivots dar, die eine Kombination aus Symbolen und Text oder nur Symbole verwenden, um Abschnittsinhalte zu gliedern. Registerkarten werden mit dem [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)-Steuerelement erstellt. Das [**Pivotbeispiel**](http://go.microsoft.com/fwlink/p/?LinkId=619903) zeigt, wie das Pivot-Steuerelement an das Registerkartenmuster angepasst wird.

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li>[**Pivot-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn608241)</li>
</ul>
</div>


## <a name="the-pivot-pattern"></a>Pivotmuster

Wenn Sie eine App mit dem Pivot erstellen, sind einige wichtige Designelemente zu berücksichtigen.

- **Headerbeschriftungen.**  Header können über ein Symbol mit Text, nur über ein Symbol oder nur über Text verfügen.
- **Headerausrichtung.**  Header können links ausgerichtet oder zentriert werden.
- **Navigation auf oberster oder auf einer untergeordneten Ebene.**  Pivots können für eine der Navigationsebenen verwendet werden. Optional kann der [Navigationsbereich](nav-pane.md) als primäre Ebene dienen und das Pivot als sekundäre.
- **Unterstützung für Touchbewegungen.**  Für Geräte, die Touchbewegungen unterstützen, können Sie zur Navigation zwischen Inhaltskategorien eine von zwei Interaktionen verwenden:
    1. Tippen Sie auf einen Registerkarten-/Pivotheader, um zur gewünschten Kategorie zu navigieren.
    2. Wischen Sie über dem Inhaltsbereich nach links oder rechts, um zur benachbarten Kategorie zu navigieren.

## <a name="examples"></a>Beispiele

Pivot-Steuerelement auf dem Handy

![Beispiel für ein Pivot](images/pivot_example.png)

Registerkartenmuster in der App „Alarm & Uhr“

![Beispiel für ein Registerkartenmuster in „Alarm & Uhr“](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>Erstellen eines Pivot-Steuerelements

Das [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)-Steuerelement enthält die in diesem Abschnitt beschriebenen grundlegenden Funktionen.

Dieser XAML-Code erzeugt ein grundlegendes Pivot-Steuerelement mit 3 Inhaltsbereichen.

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Pivot-Elemente

Das Pivot-Steuerelement ist ein [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), daher kann es eine Sammlung von Elementen jeden Typs enthalten. Jedes zum Pivot-Steuerelement hinzugefügte Element, das nicht ausdrücklich ein [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx)-Element ist, wird implizit in ein PivotItem eingeschlossen. Da ein Pivot-Element häufig zum Navigieren zwischen Inhaltsseiten verwendet wird, ist es üblich, die Sammlung von [**Elementen**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) direkt mit XAML-UI-Elementen zu füllen. Alternativ können Sie die Eigenschaft [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) auf eine Datenquelle festlegen. In der ItemsSource gebundene Elemente können beliebigen Typs sein, wenn es sich jedoch nicht explizit um PivotItems handelt, müssen Sie eine [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)- und eine [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx)-Eigenschaft definieren, um festzulegen, wie die Elemente angezeigt werden sollen.

Mit der Eigenschaft [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) können Sie das aktive Element des Pivot-Steuerelements abrufen oder festlegen. Mit der Eigenschaft [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) können Sie den Index des aktiven Elements abrufen oder festlegen.

### <a name="pivot-headers"></a>Pivotheader

Sie können die Eigenschaften [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) und [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) verwenden, um weitere Steuerelemente zum Pivotheader hinzuzufügen.

### <a name="pivot-interaction"></a>Pivot-Interaktion

Das Steuerelement erkennt die folgenden Touchgesteninteraktionen:

-   Durch Tippen auf den Header eines Pivotelements wird zum Abschnittsinhalt dieses Headers navigiert.
-   Durch Wischen nach links oder rechts auf dem Header eines Pivotelements wird zum benachbarten Abschnitt navigiert.
-   Durch Wischen nach links oder rechts auf einem Abschnittsinhalt wird zum benachbarten Abschnitt navigiert.
![Beispiel für eine Wischbewegung nach links auf dem Abschnittsinhalt](images/pivot_w_hand.png)

Das Steuerelement ist in zwei Modi verfügbar:

**Stationär**

-   Pivots werden nicht verschoben, wenn alle Pivotheader in den zulässigen Bereich passen.
-   Durch Tippen auf eine Pivotbeschriftung wird zur entsprechenden Seite navigiert. Das Pivot selbst wird jedoch nicht verschoben. Das aktive Pivot wird hervorgehoben.

<div class="microsoft-internal-note">
Es wird dringen davon abgeraten, Elemente in einer 10-Fuß-Umgebung im Karussellmodus zu verwenden. Legen Sie die neue `IsHeaderItemsCarouselEnabled`-Eigenschaft auf „False“ fest, wenn Ihre App auf der Xbox ausgeführt wird.
</div>

**Karussell**

-   Falls nicht alle Pivotheader in den verfügbaren Bereich passen, werden Pivots in einer Karussellansicht dargestellt.
-   Durch Tippen auf eine Pivotbeschriftung wird die entsprechende Seite aufgerufen, und die aktive Pivotbeschriftung rückt an die erste Position.
-   Pivotelemente in einer Karussellschleife wechseln vom letzten zum ersten Pivotabschnitt.

<div class="microsoft-internal-note">
### Pivotfokus

Der Tastaturfokus wird bei einem Pivotheader standardmäßig mit einem Unterstrich dargestellt.

![Standardfokus als Unterstrich des ausgewählten Headers](images/pivot_focus_selectedHeader.png)

Apps, die über ein benutzerdefiniertes Pivot verfügen und den Unterstrich in visuelle Objekte zur Headerauswahl integrieren, können die neue `HeaderFocusVisualPlacement`-Eigenschaft verwenden, um den Standardwert zu ändern. Wenn `HeaderFocusVisualPlacement=\"ItemHeaders\"` lautet, wird der Fokus um den gesamten Headerbereich gezeichnet.

![Bei der ItemsHeader-Option wird das Fokusrechteck um alle Pivotheader gezeichnet.](images/pivot_focus_headers.png)
</div>

## <a name="recommendations"></a>Empfehlungen

-   Die Ausrichtung der Registerkarten-/Pivotheader sollte auf der Bildschirmgröße basieren. Bei Bildschirmbreiten unter 720 epx ist die Zentrierung in der Regel besser geeignet. Bei Bildschirmbreiten über 720 epx ist in den meisten Fällen die Linksausrichtung empfehlenswert.
-   Vermeiden Sie mehr als 5 Header bei Verwendung des Karussell-Modus (Roundtrip), da mehr als Schleifen verwirrend sein können.
-   Verwenden Sie das Registerkartenmuster nur, wenn Ihre Pivotelemente unterschiedliche Symbole besitzen.
-   Fügen Sie Text in Pivotelementheader ein, damit Benutzer die Bedeutung der einzelnen Pivotabschnitte verstehen. Symbole sind nicht unbedingt für alle Benutzer selbsterklärend.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen
- [Pivotbeispiel](http://go.microsoft.com/fwlink/p/?LinkId=619903)<br/>
    So passen Sie das Pivot-Steuerelement an das Registerkartenmuster an.
- [Einfaches Beispiel für XAML-Benutzeroberfläche](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Verwandte Themen
- [Navigationsdesigngrundlagen](../layout/navigation-basics.md)
- [**Pivotbeispiel**](http://go.microsoft.com/fwlink/p/?LinkId=619903)



<!--HONumber=Dec16_HO1-->


