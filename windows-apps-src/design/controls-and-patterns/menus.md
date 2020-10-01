---
Description: Menüs und Kontextmenüs zeigen auf Anforderung des Benutzers eine Liste von Befehlen oder Optionen an.
title: Menüs und Kontextmenüs
label: Menus and context menus
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
ms.custom: RS5, 19H1
keywords: Windows 10, UWP
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e44da32b5eec603ed9a334b4b870ca1176047a97
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218653"
---
# <a name="menus-and-context-menus"></a>Menüs und Kontextmenüs

Menüs und Kontextmenüs zeigen auf Anforderung des Benutzers eine Liste von Befehlen oder Optionen an. Verwenden Sie ein Menü-Flyout, um ein einzelnes Inlinemenü anzuzeigen. Verwenden Sie eine Menüleiste, um eine Gruppe von Menüs in einer horizontalen Zeile anzuzeigen, in der Regel am oberen Rand eines App-Fensters. Jedes Menü kann Menüelemente und Untermenüs enthalten.

![Beispiel für ein typisches Kontextmenü](images/contextmenu_rs2_icons.png)

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Das Steuerelement **MenuBar** ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Windows-UI-Bibliotheks-APIs:** [MenuBar-Klasse](/uwp/api/microsoft.ui.xaml.controls.menubar)
>
> **Plattform-APIs:** [MenuFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.menuflyout), [MenuBar-Klasse](/uwp/api/windows.ui.xaml.controls.menubar), [ContextFlyout-Eigenschaft](/uwp/api/windows.ui.xaml.uielement.contextflyout), [FlyoutBase.AttachedFlyout-Eigenschaft](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Menüs und Kontextmenüs sparen Platz, indem Befehle angeordnet und ausgeblendet werden, bis der Benutzer sie benötigt. Wenn ein bestimmter Befehl häufig verwendet wird und Sie genügend Platz haben, sollten Sie ihn direkt als eigenes Element und nicht in einem Menü platzieren, damit Benutzer das Menü nicht durchlaufen müssen, um ihn aufzurufen.

Menüs und Kontextmenüs dienen dazu, Befehle strukturiert anzuordnen. Um beliebige Inhalte anzuzeigen, z. B. eine Benachrichtigung oder eine Anfragebestätigung, verwenden Sie ein [Dialogfeld oder Flyout](./dialogs-and-flyouts/index.md).

### <a name="menubar-vs-menuflyout"></a>MenuBar im Vergleich zu MenuFlyout

Um ein Menü in einem Flyout anzuzeigen, das einem Benutzeroberflächenelement im Zeichenbereich angefügt ist, verwenden Sie das MenuFlyout-Steuerelement, um Ihre Menüelemente aufzunehmen. Sie können ein Menü-Flyout entweder als normales Menü oder als Kontextmenü aufrufen. Ein Menü-Flyout anthält ein einzelnes Menü der obersten Ebene (und optionale Untermenüs).

Um eine Gruppe von mehreren Menüs der obersten Ebene in einer horizontalen Reihe anzuzeigen, verwenden Sie eine Menüleiste. Die Menüleiste positionieren Sie normalerweise am oberen Rand des App-Fensters.

### <a name="menubar-vs-commandbar"></a>MenuBar im Vergleich zu CommandBar

MenuBar und CommandBar repräsentieren beide Oberflächen, die Sie verwenden können, um Ihren Benutzern Befehle verfügbar zu machen. MenuBar bietet eine schnelle und einfache Möglichkeit, um eine Reihe von Befehlen für Apps verfügbar zu machen, für die ggf. ein höherer Grad an Organisation und Gruppierung als bei CommandBar erforderlich ist.

Sie können eine MenuBar auch in Verbindung mit einer CommandBar verwenden. Verwenden Sie die MenuBar, um den größten Teil der Befehle bereitzustellen, und CommandBar, um die am häufigsten verwendeten Befehle hervorzuheben.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/MenuFlyout">die App zu öffnen und MenuFlyout in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Vergleich zwischen Menüs und Kontextmenüs

Menüs und Kontextmenüs sind ähnlich, was ihr Erscheinungsbild und ihren Inhalt angeht. Tatsächlich werden beide mit demselben Steuerelement erstellt, [MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout). Der Unterschied besteht darin, wie Sie den Benutzer darauf zugreifen lassen.

Wann sollten Sie ein Menü und wann ein Kontextmenü verwenden?

- Ist das übergeordnete Element eine Schaltfläche oder ein anderes Befehlselement, dessen primäre Funktion es ist, weitere Befehle zu präsentieren, verwenden Sie ein Menü.
- Wenn das übergeordnete Element eine andere Art von Element ist, das hauptsächlich einem anderen Zweck dient (z. B. der Anzeige von Text oder einem Bild), verwenden Sie ein Kontextmenü.

Verwenden Sie z. B. ein Menü auf einer Schaltfläche, um Filter- und Sortieroptionen für eine Liste bereitzustellen. Der Hauptzweck des Schaltflächen-Steuerelements ist in diesem Szenario, auf ein Menü zuzugreifen.

![Beispiel für ein Menü in Mail](images/Mail_Menu.png)

Wenn Sie Befehle (z. B. Ausschneiden, Kopieren und Einfügen) hinzufügen möchten, verwenden Sie ein Kontextmenü anstelle eines Menüs. In diesem Szenario ist die Hauptfunktion des Textelements Text zur Ansicht und zum Bearbeiten anzuzeigen. Weitere Befehle (z. B. Ausschneiden, Kopieren und Einfügen) sind sekundär und gehören in ein Kontextmenü.

![Beispiel für ein Kontextmenü in der Fotogalerie](images/ContextMenu_example.png)

### <a name="menus"></a>Menüs

- Haben Sie einen einzigen Einstiegspunkt (am oberen Bildschirmrand, z. B. über ein Menü „Datei“), der immer angezeigt wird
- Werden in der Regel an eine Schaltfläche oder ein übergeordnetes Menüelement angehängt
- Werden mit der linken Maustaste (oder einer entsprechenden Aktion, z. B. durch Tippen) aufgerufen
- Werden Elementen über ihre [Flyout](/uwp/api/windows.ui.xaml.controls.button.flyout)- oder [FlyoutBase.AttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties)-Eigenschaft zugeordnet oder in einer Menüleiste am oberen Rand des App-Fensters gruppiert.

### <a name="context-menus"></a>Kontextmenüs

- Sind mit einem einzelnen Element verknüpft und zeigen Sekundärbefehle an.
- Werden mit der rechten Maustaste (oder einer entsprechenden Aktion, z. B. durch Tippen und Halten) aufgerufen.
- Werden Elementen über ihre [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout)-Eigenschaft zugeordnet.

## <a name="icons"></a>Symbole

Erwägen Sie in folgenden Fällen Symbole für die Menüpunkte einzurichten:

- Am häufigsten verwendete Elemente.
- Menüpunkte, für die allgemein bekannte oder Standardsymbole vorhanden sind.
- Menüpunkte, deren Funktion auf einfache Weise mit einem Symbol veranschaulicht werden kann.

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
> Die Größe der Symbole in einem MenuFlyoutItem ist 16 x 16 px. Wenn Sie SymbolIcon, FontIcon oder PathIcon verwenden, wird das Symbol automatisch verlustfrei auf die richtige Größe skaliert. Achten Sie bei der Verwendung von BitmapIcon darauf, dass Ihr Element 16 x 16 px groß sein muss.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Erstellen eines Menü-Flyouts oder eines Kontextmenüs

Um ein Menü-Flyout oder ein Kontextmenü zu erstellen, verwenden Sie die [MenuFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.menuflyout). Sie definieren den Inhalt des Menüs, indem Sie dem MenuFlyout [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.menuflyoutitem)-, [MenuFlyoutSubItem](/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem)-, [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)-, [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) and [MenuFlyoutSeparator](/uwp/api/windows.ui.xaml.controls.menuflyoutseparator)-Objekte hinzufügen.

Diese Objekte erfüllen folgende Zwecke:

- [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.menuflyoutitem): Ausführen einer sofortigen Aktion
- [MenuFlyoutSubItem](/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem) – Aufnehmen einer hierarchischen Liste von Menüelementen.
- [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem): Ein- und Ausschalten einer Option
- [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) – Wechseln zwischen sich gegenseitig ausschließenden Menüelementen.
- [MenuFlyoutSeparator](/uwp/api/windows.ui.xaml.controls.menuflyoutseparator)—Optisches Trennen von Menüelementen

In diesem Beispiel wird ein [MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout) erstellt, und anschließend wird mit der [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout)-Eigenschaft, die für die meisten Steuerelemente verfügbar ist, das MenuFlyout als Kontextmenü angezeigt.

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

Das nächste Beispiel ist nahezu identisch, aber anstelle der [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout)-Eigenschaft zur Anzeige der [MenuFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.menuflyout) als Kontextmenü wird die [FlyoutBase.ShowAttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout)-Eigenschaft verwendet, um die Klasse als Menü anzuzeigen.

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

### <a name="light-dismiss"></a>Einfaches Ausblenden

Einfach ausblendbare Steuerelemente wie Menüs, Kontextmenüs und andere Flyouts erhalten in der vorübergehenden Benutzeroberfläche den Tastatur- bzw. Gamepad-Fokus, bis sie nicht mehr angezeigt werden. Um dieses Verhalten optisch zu kennzeichnen, werden diese Steuerelemente auf der Xbox als Überlagerung gezeichnet, wobei Helligkeit bzw. Sichtbarkeit der umgebenden Benutzeroberfläche reduziert wird. Dieses Verhalten lässt sich mit der Eigenschaft [LightDismissOverlayMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode) anpassen. Standardmäßig erhalten kurzlebige Benutzeroberflächen auf der Xbox (**Auto**), jedoch nicht auf anderen Gerätefamilien eine einfach ausgeblendete Überlagerung. Apps können jedoch durchsetzen, dass die Überlagerung stets **On** oder stets **Off** ist.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Erstellen einer Menüleiste

> [!IMPORTANT]
> MenuBar erfordert Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher oder die [Windows UI-Bibliothek](/uwp/toolkits/winui/).

Sie verwenden dieselben Elemente zum Erstellen von Menüs in einer Menüleiste wie im einem Menü-Flyout. Anstatt allerdings MenuFlyoutItem-Objekte in einem MenuFlyout zu gruppieren, gruppieren Sie sie in einem MenuBarItem-Element. Jedes MenuBarItem wird der MenuBar als ein Menü der obersten Ebene hinzugefügt.

![Beispiel für eine Menüleiste](images/menu-bar-submenu.png)

> [!NOTE]
> In diesem Beispiel wird nur gezeigt, wie die Benutzeroberflächenstruktur erstellt wird, aber nicht, wie einer der Befehle implementiert wird.

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

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für XAML-Kontextmenü](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Verwandte Artikel

- [MenuFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.menuflyout)
- [MenuBar-Klasse](/uwp/api/microsoft.ui.xaml.controls.menubar)
