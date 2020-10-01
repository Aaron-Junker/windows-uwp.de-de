---
Description: Das Pivot-Steuerelement ermöglicht Touch-Wischen zwischen einem kleinen Satz von Inhaltsabschnitten.
title: Pivot
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d4e8d86c46144531e423ed514b2865a90d78e051
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217853"
---
# <a name="pivot"></a>Pivot

Das [Pivot](/uwp/api/Windows.UI.Xaml.Controls.Pivot)-Steuerelement ermöglicht Touch-Wischen zwischen einem kleinen Satz von Inhaltsabschnitten.

![Standardfokus als Unterstrich des ausgewählten Headers](images/pivot_focus_selectedHeader.png)

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Die Bibliothek „Windows UI“ enthält ab Version 2.2 eine neue Vorlage für dieses Steuerelement, die abgerundete Ecken verwendet. Weitere Informationen finden Sie unter [Eckradius](../style/rounded-corner.md). WinUI ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Plattform-APIs:** [Pivot-Klasse](/uwp/api/Windows.UI.Xaml.Controls.Pivot), [NavigationView-Klasse](/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn du die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert hast, klicke hier, um <a href="xamlcontrolsgallery:/item/Pivot">die App zu öffnen und Pivot-Steuerlement in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Das Pivot-Steuerelement, ebenso wie [NavigationView](navigationview.md), unterstreicht das ausgewählte Element.

![Standardfokus als Unterstrich des ausgewählten Headers](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Um gängige Muster für obere Navigation und Registerkarten zu erhalten, wird empfohlen, [NavigationView](navigationview.md) zu verwenden, das sich automatisch an verschiedene Bildschirmgrößen anpasst und größere Anpassungsmöglichkeiten bietet.

Wenn Ihre Navigation jedoch Touch-Wischen erfordert, empfehlen wir die Verwendung von Pivot.

Die anderen Hauptunterschiede zwischen dem NavigationView- und dem Pivot-Steuerelement sind das Standardverhalten bei Überlauf und die Navigations-API:

- Pivot verwendet für überlaufende Elemente ein Karussell, während NavigationView für Überlauf ein Menüdropdown verwendet, sodass Benutzer alle Elemente sehen können.
- Pivot verarbeitet die Navigation zwischen Inhaltsabschnitten, während NavigationView stärkere Kontrolle über das Navigationsverhalten ermöglicht.

## <a name="use-navigationview-instead-of-pivot"></a>Verwenden von NavigationView anstelle von PowerPivot

Wenn die Benutzeroberfläche Ihrer App das Pivot-Steuerelement verwendet, können Sie mit dem folgenden Code Pivot in NavigationView konvertieren.

Dieses XAML erstellt eine NavigationView mit 3 Inhaltabschnitten, wie das Beispiel-Pivot in [Erstellen eines Pivot-Steuerelements](#create-a-pivot-control).

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

NavigationView bietet stärkere Kontrolle über Anpassungen der Navigation und erfordert entsprechenden CodeBehind. Verwenden Sie den folgenden CodeBehind zusammen mit dem oben stehenden XAML:

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

Dieser Code imitiert die integrierte Navigationsumgebung des Pivot-Steuerelements ohne die Touch-Wischen-Erfahrung zwischen Inhaltsabschnitten. Wie Sie sehen können, könnten Sie aber auch mehrere Aspekte anpassen, einschließlich des animierten Übergangs, der Navigationsparameter und der Stapelfunktionen.

## <a name="create-a-pivot-control"></a>Erstellen eines Pivot-Steuerelements

Dieser Code erzeugt ein grundlegendes Pivot-Steuerelement mit 3 Inhaltsabschnitten.

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

Pivot ist ein [ItemsControl](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl), daher kann es eine Sammlung von Elementen jeden Typs enthalten. Jedes zum Pivot-Steuerelement hinzugefügte Element, das nicht ausdrücklich ein [PivotItem](/uwp/api/Windows.UI.Xaml.Controls.PivotItem)-Element ist, wird implizit in ein PivotItem eingeschlossen. Da ein Pivot-Element häufig zum Navigieren zwischen Inhaltsseiten verwendet wird, ist es üblich, die Sammlung von [Elementen](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) direkt mit XAML-UI-Elementen zu füllen. Alternativ können Sie die Eigenschaft [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) auf eine Datenquelle festlegen. In der ItemsSource gebundene Elemente können beliebigen Typs sein, wenn es sich jedoch nicht explizit um PivotItems handelt, müssen Sie eine [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)- und eine [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.pivot.headertemplate)-Eigenschaft definieren, um festzulegen, wie die Elemente angezeigt werden sollen.

Mit der Eigenschaft [SelectedItem](/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) können Sie das aktive Element des Pivot-Steuerelements abrufen oder festlegen. Mit der Eigenschaft [SelectedIndex](/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) können Sie den Index des aktiven Elements abrufen oder festlegen.

### <a name="pivot-headers"></a>Pivotheader

Sie können die Eigenschaften [LeftHeader](/uwp/api/windows.ui.xaml.controls.pivot.leftheader) und [RightHeader](/uwp/api/windows.ui.xaml.controls.pivot.rightheader) verwenden, um weitere Steuerelemente zum Pivotheader hinzuzufügen.

Sie können dem RightHeader des Pivot-Steuerelements z. B. eine [CommandBar](./app-bars.md) hinzufügen.

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

**Stationär**

- Pivots werden nicht verschoben, wenn alle Pivotheader in den zulässigen Bereich passen.
- Durch Tippen auf eine Pivotbeschriftung wird zur entsprechenden Seite navigiert. Das Pivot selbst wird jedoch nicht verschoben. Das aktive Pivot wird hervorgehoben.

**Karussell**

- Falls nicht alle Pivotheader in den verfügbaren Bereich passen, werden Pivots in einer Karussellansicht dargestellt.
- Durch Tippen auf eine Pivotbeschriftung wird die entsprechende Seite aufgerufen, und die aktive Pivotbeschriftung rückt an die erste Position.
- Pivotelemente in einer Karussellschleife wechseln vom letzten zum ersten Pivotabschnitt.

> **Hinweis** Pivotheader sollten in einer [10-Fuß-Umgebung](../devices/designing-for-tv.md) nicht in einer Karussellansicht dargestellt werden. Legen Sie die [IsHeaderItemsCarouselEnabled](/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled)-Eigenschaft auf **false** fest, wenn Ihre App auf der Xbox ausgeführt wird.

## <a name="recommendations"></a>Empfehlungen

- Vermeiden Sie mehr als 5 Header bei Verwendung des Karussell-Modus (Roundtrip), da mehr als Schleifen verwirrend sein können.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Zugehörige Themen

- [Pivot-Klasse](/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Navigationsdesigngrundlagen](../basics/navigation-basics.md)
