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
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64564539"
---
# <a name="icons-for-uwp-apps"></a>Symbole für UWP-Apps

![Headerbild für Symbole](images/icons/header-icons.png)

Symbole bieten eine visuelle Kurzform für eine Aktion, ein Konzept oder ein Produkt. Durch das Komprimieren der Bedeutung in ein symbolisches Bild können Symbole Sprachhürden überwinden und dazu beitragen, eine sehr wertvolle Ressource zu sparen: Platz auf dem Bildschirm. 

Symbole können in Apps angezeigt werden – und außerhalb von Apps: 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
In Ihre app verwenden Sie die Symbole zur Darstellung einer Aktion, z. B. Text zu kopieren, oder Navigieren auf der Seite "Einstellungen".
    :::column-end:::
    :::column:::
**Symbole, die außerhalb der app**

        ![icons outside the app](images/icons/outside-icons.jpg)
Außerhalb der app verwendet Windows ein Symbol für Ihre app im Startmenü und auf der Taskleiste. Wenn der Benutzer entscheidet, die Ihre app an das Startmenü anheften, kann Ihrer app-Start-Kachel das Symbol Ihrer app bieten. Das Symbol Ihrer app, die in der Titelleiste angezeigt wird, und Sie können auch einen Begrüßungsbildschirm mit das Logo Ihrer app zu erstellen.
    :::column-end:::
:::row-end:::

In diesem Artikel werden Symbole in Ihrer App beschrieben. Informationen zu Symbolen außerhalb Ihrer App (App-Symbole) finden Sie im [Artikel zu App- und Kachelsymbolen](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Wann Symbole verwendet werden sollten

Symbole können Platz sparen, aber wann ist eine Verwendung sinnvoll? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

Verwenden Sie ein Symbol für Aktionen wie Ausschneiden, kopieren, einfügen, und speichern oder für die Navigationselemente im Navigationsmenü aus.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

Verwenden Sie ein Symbol, wenn bereits eine weitere Informationen zum Konzept vorhanden ist, die Sie darstellen möchten. (Um festzustellen, ob ein Symbol vorhanden ist, überprüfen Sie die Liste der Segoe-Symbol.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

Verwenden Sie ein Symbol aus, wenn für den Benutzer zu verstehen, was bedeutet, dass das Symbol ist es einfach, und es einfach genug ist, um das Löschen bei kleiner Größe sein.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

Verwenden Sie ein Symbol nicht seine Bedeutung nicht klar ist, oder machen deutlich eine komplexe Form.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Verwendung der richtigen Art von Symbol

Es gibt viele Möglichkeiten, ein Symbol zu erstellen. Sie können eine Symbolschriftart wie Segoe MDL2 Assets verwenden. Sie können Ihre eigenen vektorbasiertes Bild erstellen. Sie können sogar ein Bitmap-Bild verwenden, auch wenn das nicht empfohlen wird. Hier ist eine Übersicht über die verschiedenen Möglichkeiten zum Hinzufügen eines Symbols zu Ihrer App. 

### <a name="use-a-predefined-icon"></a>Verwenden eines vordefinierten Symbols
:::row:::
    :::column:::
Microsoft bietet mehr als 1.000 Symbole in der Form der Schriftart Segoe MDL2 Bestand. Möglicherweise ist es nicht intuitiv, ein Symbol aus einer Schriftart zu nehmen, aber unsere Schriftanzeigetechnologie bedeutet, dass diese Symbole klar und scharf auf jedem Bildschirm, bei jeder Auflösung und in jeder Größe angezeigt werden. Anweisungen hierzu finden Sie unter [Segoe MDL2 Symbole](segoe-ui-symbol-font.md).
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Verwenden einer Schriftart
:::row:::
    :::column:::
Sie müssen keine verwenden Sie die Schriftart Segoe MDL2 Assets – können Sie alle Schriftarten, die der Benutzer hat auf ihrem System, z. B. Wingdings oder Webdings installiert.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Verwenden einer SVG-Datei (Scalable Vector Graphics)
:::row:::
    :::column:::
SVG-Ressourcen sind ideal für Symbole, da sie immer auf jeder Größe und Auflösung scharf aussehen. Die meisten Zeichenanwendungen können in das SVG-Format exportieren. Anweisungen hierzu finden Sie unter [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource).
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Verwenden geometrischer Objekte
:::row:::
    :::column:::
Geometrien sind eine vektorbasierte-Ressource, wie SVG-Dateien sodass sie immer Spitzen gesucht werden. Das Erstellen einer Geometrie ist jedoch kompliziert, da Sie jeden Punkt und jede Kurve einzeln angeben müssen. Es ist wirklich nur eine gute Option, wenn Sie das Symbol ändern müssen, während Ihre App ausgeführt wird (z. B. um es zu animieren). Anweisungen finden Sie unter [Befehle zum Verschieben und Zeichnen von Geometrien](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Sie können auf ein Bitmap-Bild wie PNG, GIF oder JPEG verwenden, auch wenn das nicht empfohlen wird.
:::row:::
    :::column:::
Sie haben nach oben oder unten skaliert werden, je nachdem, wie groß das Symbol sein soll und die Auflösung des Bildschirms werden Bitmapbilder an eine bestimmte Größe erstellt. Wenn das Bild verkleinert wird, kann es verschwommen angezeigt werden. Wenn es vergrößert wird, kann es eckig und verpixelt aussehen. Wenn Sie ein Bitmap-Bild verwenden müssen, empfehlen wir die Verwendung einer PNG- oder GIF-Datei anstelle des JPEG-Formats. 
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
Legen Sie auf der Schaltfläche Schriftfamilie `Segoe MDL2 Assets` und der Content-Eigenschaft auf den Unicode-Wert des Symbols für das Sie verwenden möchten:
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
Sie können eines der Objekte die Symbol-Element verwenden: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), oder [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Dies bietet Ihnen weitere Arten von Symbolen, die zur Auswahl und können Sie Symbole und andere Arten von Inhalten, z. B. Text, kombiniert werden, wenn Sie möchten:
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
Wenn Sie eine Reihe von Befehlen, die zusammengehören, haben, wie z. B. Ausschneiden/Kopieren/Einfügen oder eine Gruppe von Zeichnen-Befehle für ein Programm bearbeiten, speichern Sie sie zusammen in einem [Befehlsleiste](../controls-and-patterns/app-bars.md). Eine Befehlsleiste enthält eine oder mehrere Schaltflächen oder Umschaltflächen der App-Leiste, die jeweils eine Aktion darstellen. Jede Schaltfläche verfügt über eine [Symbol](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon)-Eigenschaft, mit der Sie steuern, welches Symbol angezeigt wird. Es gibt eine Vielzahl von Möglichkeiten, um das Symbol anzugeben. 
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
