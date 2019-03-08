---
Description: Gute Symbole harmonieren mit der Typografie und der übrigen Gestaltungssprache. Sie verwenden keine Metaphern und geben einfach und schnell nur die erforderlichen Informationen weiter.
title: Symbole
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fb97f69ccdffcec86dfdaf5fa6c5f817644ebd61
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612445"
---
# <a name="icons-for-uwp-apps"></a>Symbole für UWP-Apps

![Headerbild für Symbole](images/icons/header-icons.png)

Symbole bieten eine visuelle Kurzform für eine Aktion, ein Konzept oder ein Produkt. Durch das Komprimieren der Bedeutung in ein symbolisches Bild können Symbole Sprachhürden überwinden und dazu beitragen, eine sehr wertvolle Ressource zu sparen: Platz auf dem Bildschirm. 

Symbole können in Apps angezeigt werden – und außerhalb von Apps: 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::row-end:::

In diesem Artikel werden Symbole in Ihrer App beschrieben. Informationen zu Symbolen außerhalb Ihrer App (App-Symbole) finden Sie im [Artikel zu App- und Kachelsymbolen](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Wann Symbole verwendet werden sollten

Symbole können Platz sparen, aber wann ist eine Verwendung sinnvoll? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Verwendung der richtigen Art von Symbol

Es gibt viele Möglichkeiten, ein Symbol zu erstellen. Sie können eine Symbolschriftart wie Segoe MDL2 Assets verwenden. Sie können Ihre eigenen vektorbasiertes Bild erstellen. Sie können sogar ein Bitmap-Bild verwenden, auch wenn das nicht empfohlen wird. Hier ist eine Übersicht über die verschiedenen Möglichkeiten zum Hinzufügen eines Symbols zu Ihrer App. 

### <a name="use-a-predefined-icon"></a>Verwenden eines vordefinierten Symbols
:::row:::
    :::column:::
        Microsoft provides over 1000 icons in the form of the Segoe MDL2 Assets font. It might not be intuitive to get an icon from a font, but our font display technology means these icons will look crisp and sharp on any display, at any resolution, and at any size. 
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Verwenden einer Schriftart
:::row:::
    :::column:::
        You don't have to use the Segoe MDL2 Assets font--you can use any font the user has installed on their system, such as Wingdings or Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Verwenden einer SVG-Datei (Scalable Vector Graphics)
:::row:::
    :::column:::
        SVG resources are ideal for icons, because they always look sharp at any size or resolution. Most drawing applications can export to SVG. 
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Verwenden geometrischer Objekte
:::row:::
    :::column:::
        Like SVG files, geometries are a vector-based resource, so they always look sharp. However, creating a geometry is complicated because you have to individually specify each point and curve. It's really only a good choice if you need to modify the icon while your app is running (to animate it, for example). For instructions, see [Move and draw commands for geometries](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Sie können auf ein Bitmap-Bild wie PNG, GIF oder JPEG verwenden, auch wenn das nicht empfohlen wird.
:::row:::
    :::column:::
        Bitmap images are created at a specific size, so they have to be scaled up or down depending on how large you want the icon to be and the resolution of the screen. When the image is scaled down (shrunk), it can appear blurry; when it's scaled up, it can appear blocky and pixelated. If you have to use a bitmap image we recommend using a PNG or GIF over a JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Festlegen einer Aktion für das Symbol

Nachdem Sie ein Symbol haben, besteht der nächste Schritt, um einen Befehl oder einer Navigationsaktion zuordnen Zweck zu erleichtern. Die beste Möglichkeit hierzu ist das Symbol auf eine Schaltfläche oder eine Befehlsleiste hinzufügen. 

![Befehlsleistenbild](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Erstellen einer Symbolschaltfläche

Sie können ein Symbol in eine Standardschaltfläche einfügen. Da Sie Schaltflächen an vielfältigeren Orten verwenden können, erhalten Sie damit etwas mehr Flexibilität, welcher Stelle Ihr Aktionssymbol angezeigt wird. 

Es gibt verschiedene Möglichkeiten, ein Symbol zu einer Schaltfläche hinzuzufügen:

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
        Set the button's font family to `Segoe MDL2 Assets` and its content property to the unicode value of the glyph you want to use:
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
        You can use one of the icon element objects: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon),
        [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), 
        [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), or
        [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). This gives you more types of icons to choose from, and enables you to combine icons and other types of content, such as text, if you want:
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Erstellen einer Reihe von Symbolen in einer Befehlsleiste

:::row:::
    :::column span:::
        When you have a series of commands that go together, such as cut/copy/paste or a set of drawing commands for a photo-editing program, put them together in a [command bar](../controls-and-patterns/app-bars.md). A command bar takes one or more app bar buttons or app bar toggle buttons, each of which represents an action. Each button has an [Icon](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) property you use to control which icon it displays. There are a variety of ways to specify the icon. 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

Die einfachste Möglichkeit ist die Verwendung der von uns bereitgestellten Liste vordefinierter Symbole: Geben Sie einfach den Namen des Symbols an, z. B. „Zurück“ oder „Beenden“, und das System zeichnet das entsprechende Symbol: 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
Die vollständige Liste mit Symbolnamen finden Sie in der [Symbolenumeration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol). 

Es gibt andere Möglichkeiten zum Bereitstellen von Symbolen für eine Schaltfläche in einer Befehlsleiste:

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon): Das Symbol basiert auf einer Glyphe aus der angegebenen Schriftartfamilie.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon): Das Symbol basiert auf einer Bitmapbilddatei mit dem festgelegten **URI**.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon): Das Symbol basiert auf [Path](/uwp/api/windows.ui.xaml.shapes.path)-Daten.

Weitere Informationen zu Befehlszeilen finden Sie im [Artikel zu Befehlsleisten](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Verwandte Artikel

* [Richtlinien für die Kachel "und" Symbol-Objekte](../shell/tiles-and-notifications/app-assets.md)
