---
Description: Menüs und Kontextmenüs zeigen auf Anforderung des Benutzers eine Liste von Befehlen oder Optionen an.
title: Menüs und Kontextmenüs
label: Menus and context menus
template: detail.hbs
ms.date: 01/08/2019
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d3ea8e2bff2455340a1183dbe5c1840fdb599d46
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59247188"
---
# <a name="menus-and-context-menus"></a>Menüs und Kontextmenüs

Menüs und Kontextmenüs zeigen auf Anforderung des Benutzers eine Liste von Befehlen oder Optionen an. Verwenden Sie ein menüflyout, um ein einziges, Inline-Menü anzuzeigen. Verwenden Sie eine Menüleiste, um eine Reihe von Menüs in einer horizontalen Zeile, in der Regel am oberen Rand eines Fensters app anzuzeigen. Jedes Menü kann es sich um Menüelemente und Untermenüs haben.

![Beispiel für ein typisches Kontextmenü](images/contextmenu_rs2_icons.png)

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist Bestandteil der Windows-UI-Bibliothek, eines NuGet-Pakets, die neuen Steuerelemente und Funktionen der Benutzeroberfläche für UWP-apps enthält. Weitere Informationen einschließlich installationsanweisungen finden Sie in der [Übersicht über die Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **Plattform-APIs** | **Windows UI-Bibliothek-APIs** |
| - | - |
| [MenuFlyout Klasse](/uwp/api/windows.ui.xaml.controls.menuflyout), [MenuBar-Klasse](/uwp/api/windows.ui.xaml.controls.menubar), [ContextFlyout Eigenschaft](/uwp/api/windows.ui.xaml.uielement.contextflyout), [FlyoutBase.AttachedFlyout-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) | [MenuBar-Klasse](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Menüs und Kontextmenüs sparen Platz, indem Befehle angeordnet und ausgeblendet werden, bis der Benutzer sie benötigt. Wenn ein bestimmter Befehl häufig verwendet wird und Sie genügend Platz haben, sollten Sie ihn direkt als eigenes Element und nicht in einem Menü platzieren, damit Benutzer das Menü nicht durchlaufen müssen, um ihn aufzurufen.

Menüs und Kontextmenüs sind Befehle zu organisieren. Verwenden Sie zum Anzeigen von beliebigen Inhalts, wie eine Benachrichtigung oder einer Bestätigung, einem [Dialogfeld oder ein Flyout](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>Menüleiste im Vergleich zu MenuFlyout

Um ein Menü in ein Flyout, angefügt an ein Element der Benutzeroberfläche im Zeichenbereich anzuzeigen, verwenden Sie das MenuFlyout-Steuerelement, um den Menüelementen zu hosten. Sie können ein menüflyout als reguläre Menü oder als ein Kontextmenü aufrufen. Ein menüflyout hostet, ein einzelnes Hauptebenen-Menüelement (und optional Untermenüs).

Um eine Reihe von Menüs der obersten Ebene in einer horizontalen Zeile anzuzeigen, verwenden Sie eine Menüleiste. Sie positionieren in der Regel die Menüleiste am oberen Rand der app-Fensters.

### <a name="menubar-vs-commandbar"></a>Menüleiste im Vergleich zu CommandBar

Menüleiste "und" CommandBar repräsentieren beide Oberflächen, die Sie verwenden können, um Befehle aus, um Ihre Benutzer verfügbar zu machen. Die Menüleiste bietet eine schnelle und einfache Möglichkeit, eine Reihe von Befehlen für apps verfügbar zu machen, die möglicherweise weitere Organisation oder Gruppierung, als ein CommandBar zulässt.

Sie können auch in Verbindung mit einer Befehlsleiste eine Menüleiste verwenden. Verwenden Sie die Menüleiste, um den größten Teil der Befehle und CommandBar, markieren Sie die am häufigsten verwendeten Befehle bereitzustellen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/MenuFlyout">die App zu öffnen und MenuFlyout in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Vergleich zwischen Menüs und Kontextmenüs

Menüs und Kontextmenüs ähneln im Aussehen und was sie enthalten können. In der Tat können Sie das gleiche Steuerelement [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), um sie zu erstellen. Der Unterschied ist, wie Sie den Benutzer darauf zugreifen können.

Wann sollten Sie ein Menü und wann ein Kontextmenü verwenden?

- Wenn das Hostelement ist eine Schaltfläche oder ein anderes Befehlselement, dessen primäre Rolle zusätzliche Befehle vorhanden ist, verwenden Sie ein Menü.
- Wenn das übergeordnete Element eine andere Art von Element ist, das hauptsächlich einem anderen Zweck dient (z. B. der Anzeige von Text oder einem Bild), verwenden Sie ein Kontextmenü.

Verwenden Sie z. B. ein Menü auf eine Schaltfläche, um die stellen Filter- und Sortieroptionen für eine Liste. Der Hauptzweck des Schaltflächen-Steuerelements ist in diesem Szenario, auf ein Menü zuzugreifen.

![Beispiel des Menüs in Mail](images/Mail_Menu.png)

Wenn Sie Befehle (z. B. Ausschneiden, Kopieren und Einfügen) hinzufügen möchten, verwenden Sie ein Kontextmenü anstelle eines Menüs. In diesem Szenario ist die Hauptfunktion des Textelements Text zur Ansicht und zum Bearbeiten anzuzeigen. Weitere Befehle (z. B. Ausschneiden, Kopieren und Einfügen) sind sekundär und gehören in ein Kontextmenü.

![Beispiel des Kurzbefehlmenüs in der Fotogalerie](images/ContextMenu_example.png)

### <a name="menus"></a>Menüs

- Haben Sie einen einzigen Einstiegspunkt (am oberen Bildschirmrand, z. B. über ein Menü „Datei“), der immer angezeigt wird
- Werden in der Regel an eine Schaltfläche oder ein übergeordnetes Menüelement angehängt
- Werden mit der linken Maustaste (oder einer entsprechenden Aktion, z. B. durch Tippen) aufgerufen
- Einem Element zugeordnet sind die [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) oder [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) Eigenschaften oder auf einer Menüleiste am oberen Rand des Fensters app gruppiert.

### <a name="context-menus"></a>Kontextmenüs

- Sind mit einem einzelnen Element verknüpft und zeigen Sekundärbefehle an.
- Werden mit der rechten Maustaste (oder einer entsprechenden Aktion, z. B. durch Tippen und Halten) aufgerufen.
- Werden Elementen über ihre [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)-Eigenschaft zugeordnet.

## <a name="icons"></a>Symbole

Erwägen Sie in folgenden Fällen Symbole für die Menüpunkte einzurichten:

- Die am häufigsten verwendeten Elemente.
- Menüelemente, deren Symbol Standard- oder bekannt ist.
- Menüelemente, deren Symbol auch veranschaulicht, wie der Befehl bewirkt.

Fühlen Sie sich nicht dazu verpflichtet, Befehle mit einem Symbol zu versehen, für die keine Standardsymbole vorhanden sind. Kryptische Symbole sind nicht hilfreich, machen das Menü unübersichtlich und hindern Benutzer daran, wichtige Menüpunkte einfach aufzufinden.

![Beispiel für ein Kontextmenü mit Symbolen](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````

> [!TIP]
> Die Größe des Symbols in einer MenuFlyoutItem ist 16x16px. Wenn Sie SymbolIcon FontIcon oder PathIcon verwenden, wird das Symbol an die richtige Größe ohne Verlust der Genauigkeit automatisch skaliert. Achten Sie bei der Verwendung von BitmapIcon darauf, dass Ihr Element 16x16px groß sein muss.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Erstellen Sie ein menüflyout oder ein Kontextmenü

Um ein menüflyout oder ein Kontextmenü zu erstellen, verwenden Sie die [MenuFlyout Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout). Definieren Sie den Inhalt des Menüs, indem hinzufügen [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem), [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)und [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) Objekte, die MenuFlyout.

Diese Objekte erfüllen folgende Zwecke:

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem): Ausführen einer sofortigen Aktion
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem)–, enthält eine hierarchische Liste von Menüelementen.
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem): Ein- und Ausschalten einer Option
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem): Wechseln zwischen gegenseitig Menüelemente.
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator)—Optisches Trennen von Menüelementen

In diesem Beispiel wird eine [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) und verwendet die [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) -Eigenschaft, die eine Eigenschaft, die für die meisten Steuerelemente, um die MenuFlyout als ein Kontextmenü anzuzeigen.

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

Das nächste Beispiel ist nahezu identisch, aber anstelle der [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)-Eigenschaft zur Anzeige der [MenuFlyout-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) als Kontextmenü wird die [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout)-Eigenschaft verwendet, um die Klasse als Menü anzuzeigen.

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

### <a name="light-dismiss"></a>Light schließen

Light dismiss-Steuerelemente wie z. B. Menüs, Kontextmenüs und andere Flyouts, Abfangen von Tastatur und Gamepad Fokus innerhalb der vorübergehenden Benutzeroberfläche bis geschlossen. Um dieses Verhalten optisch zu kennzeichnen, werden diese Steuerelemente auf der Xbox als Überlagerung gezeichnet, wobei Helligkeit bzw. Sichtbarkeit der umgebenden Benutzeroberfläche reduziert wird. Dieses Verhalten lässt sich mit der Eigenschaft  [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) anpassen. Standardmäßig werden vorübergehende Benutzeroberflächen die light-Dismiss-Überlagerung auf der Xbox gezeichnet (**automatisch**), aber nicht für andere gerätefamilien. Sie können auch erzwingen, dass das Overlay an, dass Sie immer **auf** oder immer **aus**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Erstellen einer Menüleiste

> [!IMPORTANT]
> Menüleiste erfordert Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

Sie verwenden die gleichen Elemente zum Erstellen von Menüs in einer Menüleiste, wie ein menüflyout. Allerdings gruppieren statt MenuFlyoutItem gruppierungsobjekte in einem MenuFlyout, Sie in einem MenuBarItem-Element. Jede MenuBarItem wird auf der Menüleiste als ein Menü der obersten Ebene hinzugefügt.

![Beispiel für eine Menüleiste](images/menu-bar-submenu.png)

> [!NOTE]
> In diesem Beispiel wird veranschaulicht, wie nur die Benutzeroberflächenautomatisierungs-Struktur erstellen, aber Implementierung der Befehle nicht angezeigt.

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für XAML-Kontextmenü](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Verwandte Artikel

- [MenuFlyout-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)
- [MenuBar-Klasse](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)
