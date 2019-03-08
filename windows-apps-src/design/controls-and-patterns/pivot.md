---
Description: Das Pivot-Steuerelement ermöglicht, zwischen einem kleinen Satz von Inhaltsabschnitten Touch Wischen.
title: Pivot
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 56079bc51d3efa8f7ecaaee21379a6e9caf7d440
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642925"
---
# <a name="pivot"></a>Pivot

Die [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) -Steuerelement können zwischen einem kleinen Satz von Inhaltsabschnitten Touch Wischen.

> **Wichtige APIs**: [Pivot-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [NavigationView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/Pivot">öffnen Sie die app und das Pivot-Steuerelement in Aktion sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Abrufen der XAML-Steuerelemente Katalog-app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Das Pivot-Element zu steuern, wie [NavigationView](navigationview.md), unterstreicht ausgewählten Elements.

![Standardfokus als Unterstrich des ausgewählten Headers](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Um allgemeine oben im Navigationsbereich und Registerkarten-Muster zu erreichen, es wird empfohlen, [NavigationView](navigationview.md), die automatisch passt sich an verschiedene Bildschirmgrößen und größere Anpassung ermöglicht.

Aber wenn die Navigation ein Touch-Lesegerät erforderlich ist, empfehlen wir mithilfe von Pivot.

Die wichtigsten Unterschiede zwischen der NavigationView und Pivot-Steuerelemente sind das Standardverhalten für den Überlauf und die Navigation-API:

- Pivotieren Sie Laufbänder Überlauf, die Elemente, die während der NavigationView eine Menü-Dropdownliste verwendet überlaufen, damit Benutzer alle Elemente angezeigt werden können.
- Pivot behandelt die Navigation zwischen Inhaltsabschnitte, während mehr Kontrolle über Navigationsverhalten NavigationView ermöglicht.

## <a name="use-navigationview-instead-of-pivot"></a>Verwenden von NavigationView anstelle von PowerPivot

Wenn UIs Ihrer app das Pivot-Steuerelement verwendet wird, können Sie Pivot in NavigationView durch den folgenden Code konvertieren.

Dieses XAML erstellt eine NavigationView mit 3 Abschnitte des Inhalts, wie im Beispiel Pivot in [erstellt ein Pivot-Steuerelement](#create-a-pivot-control).

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView bietet mehr Kontrolle über die Anpassung der Navigation und entsprechenden Code-Behind muss. Der obige XAML begleitenden, verwenden Sie folgende Code-Behind:

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

Dieser Code imitiert das Pivot-Steuerelement integrierte navigationsumgebung minus die Touch-Wischen Erfahrung zwischen Inhaltsabschnitten. Wie Sie sehen können, können Sie mehrere Aspekte, wie z.B. der animierten Übergang Navigation-Parameter und Stack-Funktionen auch anpassen.

## <a name="create-a-pivot-control"></a>Erstellen eines Pivot-Steuerelements

Dieser Code erstellt eine grundlegende Pivot-Steuerelement mit 3 Abschnitte des Inhalts.

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Pivot-Elemente

Das Pivot-Steuerelement ist ein [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx), daher kann es eine Sammlung von Elementen jeden Typs enthalten. Jedes zum Pivot-Steuerelement hinzugefügte Element, das nicht ausdrücklich ein [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx)-Element ist, wird implizit in ein PivotItem eingeschlossen. Da ein Pivot-Element häufig zum Navigieren zwischen Inhaltsseiten verwendet wird, ist es üblich, die Sammlung von [Elementen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) direkt mit XAML-UI-Elementen zu füllen. Alternativ können Sie die Eigenschaft [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) auf eine Datenquelle festlegen. In der ItemsSource gebundene Elemente können beliebigen Typs sein, wenn es sich jedoch nicht explizit um PivotItems handelt, müssen Sie eine [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)- und eine [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx)-Eigenschaft definieren, um festzulegen, wie die Elemente angezeigt werden sollen.

Mit der Eigenschaft [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) können Sie das aktive Element des Pivot-Steuerelements abrufen oder festlegen. Mit der Eigenschaft [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) können Sie den Index des aktiven Elements abrufen oder festlegen.

### <a name="pivot-headers"></a>Pivotheader

Sie können die Eigenschaften [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) und [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) verwenden, um weitere Steuerelemente zum Pivotheader hinzuzufügen.

Sie können dem RightHeader des Pivot-Steuerelements z. B. eine [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars) hinzufügen.

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>Pivot-Interaktion

Das Steuerelement erkennt die folgenden Touchgesteninteraktionen:

- Durch Tippen auf den Header eines Pivotelements wird zum Abschnittsinhalt dieses Headers navigiert.
- Durch Wischen nach links oder rechts auf dem Header eines Pivotelements wird zum benachbarten Abschnitt navigiert.
- Durch Wischen nach links oder rechts auf einem Abschnittsinhalt wird zum benachbarten Abschnitt navigiert.

Das Steuerelement ist in zwei Modi verfügbar:

**Stationary**

- Pivots werden nicht verschoben, wenn alle Pivotheader in den zulässigen Bereich passen.
- Durch Tippen auf eine Pivotbeschriftung wird zur entsprechenden Seite navigiert. Das Pivot selbst wird jedoch nicht verschoben. Das aktive Pivot wird hervorgehoben.

**Karussellsicht**

- Falls nicht alle Pivotheader in den verfügbaren Bereich passen, werden Pivots in einer Karussellansicht dargestellt.
- Durch Tippen auf eine Pivotbeschriftung wird die entsprechende Seite aufgerufen, und die aktive Pivotbeschriftung rückt an die erste Position.
- Pivotelemente in einer Karussellschleife wechseln vom letzten zum ersten Pivotabschnitt.

> **Hinweis** Pivotheader sollten in einer [10-Fuß-Umgebung](../devices/designing-for-tv.md) nicht in einer Karussellansicht dargestellt werden. Legen Sie die [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled)-Eigenschaft auf **false** fest, wenn Ihre App auf der Xbox ausgeführt wird.

## <a name="recommendations"></a>Empfehlungen

- Vermeiden Sie mehr als 5 Header bei Verwendung des Karussell-Modus (Roundtrip), da mehr als Schleifen verwirrend sein können.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Verwandte Themen

- [Pivot-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Grundlagen des Berichtsentwurfs Navigation](../basics/navigation-basics.md)