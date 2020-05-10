---
Description: Gute Symbole harmonieren mit der Typografie und der übrigen Gestaltungssprache. Sie verwenden keine Metaphern und geben einfach und schnell nur die erforderlichen Informationen weiter.
title: Symbole
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7c44baee7d3201e2e554604405afe337007dd510
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970955"
---
# <a name="icons-for-windows-apps"></a>Symbole für Windows-Apps

![Headerbild für Symbole](images/icons/header-icons.png)

Symbole dienen zur visuellen Darstellung einer Aktion, eines Konzepts oder eines Produkts. Dank der in einem Symbolbild komprimierten Bedeutung können Symbole Sprachhürden überwinden und dazu beitragen, eine äußerst wertvolle Ressource zu sparen: Platz auf dem Bildschirm. 

Symbole können sowohl innerhalb als auch außerhalb von Apps vorkommen: 

:::row:::
    :::column:::
        **Symbole in der App**

        ![Symbole in der App](images/icons/inside-icons.png) Innerhalb Ihrer App stellen Symbole eine Aktion dar – etwa das Kopieren von Text oder das Aufrufen der Einstellungsseite.
    :::column-end:::
    :::column:::
**Symbole außerhalb der App**

        ![Symbole außerhalb der App](images/icons/outside-icons.jpg) Außerhalb Ihrer App verwendet Windows ein Symbol, um Ihre App im Startmenü und auf der Taskleiste darzustellen. Wenn der Benutzer deine App an das Startmenü anheftet, kann die Startkachel deiner App das Symbol deiner App enthalten. Das Symbol deiner App wird auf der Titelleiste angezeigt, und du kannst einen Begrüßungsbildschirm mit dem Logo deiner App erstellen.
    :::column-end:::
:::row-end:::

In diesem Artikel werden Symbole innerhalb deiner App beschrieben. Informationen zu Symbolen außerhalb deiner App (App-Symbole) findest du im [Artikel zu App- und Kachelsymbolen](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Wann Symbole verwendet werden sollten

Symbole können Platz sparen, aber wann ist eine Verwendung sinnvoll? 

:::row:::
    :::column:::
        ![Empfohlen](images/do.svg) ![Standardbild für Symbole](images/icons/icons-standard.svg)<br>

Verwende ein Symbol für Aktionen wie Ausschneiden, Kopieren, Einfügen und Speichern oder für Navigationselemente in einem Navigationsmenü.
    :::column-end:::
    :::column:::
        ![Nicht empfohlen](images/dont.svg) ![Konzeptbild für Symbole](images/icons/icons-concept.svg)<br>

Verwende ein Symbol, wenn für das gewünschte Konzept bereits eines vorhanden ist. (Ob bereits ein Symbol vorhanden ist, erfährst du in der Segoe-Symbolliste.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Empfohlen](images/do.svg) ![Symbol für Einkaufswagen](images/icons/icon-shopping-cart.svg)<br>

Verwende ein Symbol, wenn dessen Bedeutung für den Benutzer leicht nachvollziehbar und es auch bei geringer Größe gut erkennbar ist.
    :::column-end:::
    :::column:::
        ![Nicht empfohlen](images/dont.svg) ![Konzeptbild für Symbole](images/icons/icon-bad-example.png)<br>

Verwende kein Symbol, wenn dessen Bedeutung nicht eindeutig oder zur Verdeutlichung eine komplexe Form erforderlich ist.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Verwenden der richtigen Art von Symbol

Symbole können auf unterschiedliche Weise erstellt werden. Du kannst eine Symbolschriftart wie Segoe MDL2 Assets verwenden. Du kannst ein eigenes vektorbasiertes Bild erstellen. Du kannst sogar ein Bitmap-Bild verwenden. (Davon raten wir allerdings ab.) Im Anschluss findest du eine Übersicht über die verschiedenen Methoden, mit denen du deiner App ein Symbol hinzufügen kannst: 

### <a name="use-a-predefined-icon"></a>Verwenden eines vordefinierten Symbols
:::row:::
    :::column:::
In der Schriftart „Segoe MDL2 Assets“ von Microsoft stehen über 1.000 Symbole zur Verfügung. Die Verwendung eines Symbols aus einer Schriftart ist zwar möglicherweise nicht intuitiv, dank unserer Anzeigetechnologie für Schriftarten werden diese Symbole jedoch auf jedem Anzeigegerät, bei jeder Auflösung und in jeder Größe klar und scharf dargestellt. Eine entsprechende Anleitung findest du unter [Segoe MDL2-Symbole](segoe-ui-symbol-font.md).
    :::column-end:::
    :::column:::
        ![Vordefiniertes Symbolbild](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Verwenden einer Schriftart
:::row:::
    :::column:::
Es muss nicht unbedingt „Segoe MDL2 Assets“ sein: Du kannst jede beliebige Schriftart verwenden, die der Benutzer auf seinem System installiert hat – beispielsweise „Wingdings“ oder „Webdings“.
    :::column-end:::
    :::column:::
        ![Wingdings-Bild](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Verwenden einer SVG-Datei (Scalable Vector Graphics)
:::row:::
    :::column:::
SVG-Ressourcen sind ideal für Symbole, da sie in jeder Größe und Auflösung gestochen scharf dargestellt werden. Die meisten Zeichenanwendungen verfügen über eine SVG-Exportfunktion. Eine entsprechende Anleitung findest du unter [SvgImageSource Class](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource) (Klasse „SvgImageSource“).
    :::column-end:::
    :::column:::
        ![SVG-Bild](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Verwenden geometrischer Objekte
:::row:::
    :::column:::
Bei geometrischen Objekten handelt es sich genau wie bei SVG-Dateien um vektorbasierte Ressourcen, die immer gestochen scharf dargestellt werden. Die Erstellung eines geometrischen Objekts ist allerdings kompliziert, da du jeden Punkt und jede Kurve einzeln angeben musst. Diese Option ist eigentlich nur empfehlenswert, wenn du das Symbol ändern musst, während deine App ausgeführt wird (um es beispielsweise zu animieren). Eine entsprechende Anleitung findest du unter [Move and draw commands syntax](../../xaml-platform/move-draw-commands-syntax.md) (Syntax für Befehle zum Verschieben und Zeichnen). 
    :::column-end:::
    :::column:::
        ![Bild geometrischer Objekte](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Du kannst auch ein Bitmap-Bild (beispielsweise eine PNG-, GIF- oder JPEG-Datei) verwenden. Davon raten wir allerdings ab.
:::row:::
    :::column:::
Bitmap-Bilder werden in einer bestimmten Größe erstellt und müssen daher je nach Bildschirmauflösung und gewünschter Symbolgröße vergrößert oder verkleinert werden. Verkleinert wirkt das Bild möglicherweise unscharf; vergrößert kann es kantig und verpixelt aussehen. Solltest du ein Bitmap-Bild verwenden müssen, verwende nach Möglichkeit eine PNG- oder GIF-Datei (anstelle einer JPEG-Datei). 
    :::column-end:::
    :::column:::
        ![Nicht empfohlen](images/dont.svg) ![Bitmapbild](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Festlegen einer Aktion für das Symbol

Der nächste Schritt darin, deinem Symbol einen Befehl oder eine Navigationsaktion zuzuordnen. Dazu fügst du das Symbol am besten einer Schaltfläche oder Befehlsleiste hinzu. 

![Abbildung: Befehlsleiste](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Erstellen einer Symbolschaltfläche

Du kannst ein Symbol einer Standardschaltfläche hinzufügen. Da Schaltflächen an verschiedensten Orten verwendet werden können, kannst du dadurch etwas flexibler bestimmen, wo dein Aktionssymbol angezeigt werden soll. 

Ein Symbol kann einer Schaltfläche auf unterschiedliche Weise hinzugefügt werden:

:::row:::
    :::column span="2":::
        <b>Schritt 1</b><br>
Lege die Schriftfamilie der Schaltfläche auf `Segoe MDL2 Assets` und die Inhaltseigenschaft auf den Unicode-Wert der gewünschten Glyphe fest:
    :::column-end:::
    :::column:::
        ![Erstellen einer Symbolschaltfläche, Schritt 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Schritt 2</b><br>
Du kannst eines der Symbolelementobjekte verwenden: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) oder [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Dadurch hast du mehr Symboltypen zur Auswahl, und du kannst auf Wunsch Symbole mit anderen Arten von Inhalten (beispielsweise Text) kombinieren:
    :::column-end:::
    :::column:::
        ![Erstellen einer Symbolschaltfläche, Schritt 2](images/icons/icon-text-step-2.svg)
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

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Erstellen einer Reihe von Symbolen auf einer Befehlsleiste

:::row:::
    :::column span:::
Zusammengehörige Befehle wie „Ausschneiden“, „Kopieren“ und „Einfügen“ oder eine Reihe von Zeichenbefehlen für ein Fotobearbeitungsprogramm können auf einer [Befehlsleiste](../controls-and-patterns/app-bars.md) zusammengefasst werden. Eine Befehlsleiste enthält einzelne oder mehrere Schaltflächen oder Umschaltflächen der App-Leiste, die jeweils eine Aktion darstellen. Jede Schaltfläche verfügt über eine Eigenschaft vom Typ [Icon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon), die steuert, welches Symbol angezeigt wird. Das Symbol kann auf unterschiedliche Weise angegeben werden. 
    :::column-end:::
    :::column:::
        ![Beispiel für eine Befehlsleiste mit Symbolen](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

Die komfortabelste Methode ist die Verwendung der von uns bereitgestellten Liste vordefinierter Symbole: Gib einfach den Namen des Symbols an (beispielsweise „Back“ oder „Stop“), und das System zeichnet das entsprechende Symbol: 

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
Die vollständige Liste mit Symbolnamen findest du in der [Symbolenumeration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol). 

Es gibt auch noch andere Möglichkeiten, um Symbole für eine Schaltfläche auf einer Befehlsleiste bereitzustellen:

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon): Das Symbol basiert auf einer Glyphe aus der angegebenen Schriftfamilie.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon): Das Symbol basiert auf einer Bitmap-Bilddatei mit dem angegebenen **URI**.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon): Das Symbol basiert auf Pfaddaten ([Path](/uwp/api/windows.ui.xaml.shapes.path)).

Weitere Informationen zu Befehlsleisten findest du in [diesem Artikel](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Verwandte Artikel

* [App-Symbole und Logos](app-icons-and-logos.md)
