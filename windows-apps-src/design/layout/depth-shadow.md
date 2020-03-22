---
author: knicholasa
description: Z-Tiefe, relative Tiefe und Schatten sind zwei Möglichkeiten, um Tiefe in Ihre APP zu integrieren, damit Benutzer sich auf natürliche und effiziente Weise fokussieren können.
title: Z-Tiefe und Schatten für UWP-apps
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081387"
---
# <a name="z-depth-and-shadow"></a>Z-Tiefe und Schatten

![Ein GIF, das vier graue Rechtecke anzeigt, die diagonal gestapelt sind. Der GIF wird so animiert, dass Schatten angezeigt und ausgeblendet werden.](images/elevation-shadow/shadow.gif)

Durch das Erstellen einer visuellen Hierarchie von Elementen in der Benutzeroberfläche ist die Benutzeroberfläche leicht zu scannen und zu überprüfen, worauf Sie sich konzentrieren müssen. Die Erhöhung der Rechte für die Verwendung von SELECT-Elementen Ihrer Benutzeroberfläche wird häufig verwendet, um eine solche Hierarchie in Software zu erzielen. In diesem Artikel wird erläutert, wie Sie mithilfe von z-Tiefe und Schatten Erweiterungen in einer UWP-app erstellen.

Z-Tiefe ist ein Begriff, der bei 3D-App-Erstellern verwendet wird, um den Abstand zwischen zwei Oberflächen entlang der z-Achse anzugeben. Es zeigt, wie ein Objekt für den Viewer geschlossen wird. Betrachten Sie es als ähnliches Konzept für x/y-Koordinaten, aber in der z-Richtung.

## <a name="why-use-z-depth"></a>Gründe für die Verwendung von z-Tiefe

In der physischen Welt konzentrieren wir uns auf Objekte, die sich näher an uns befinden. Wir können diesen räumlichen Instinkt auch auf die digitale Benutzeroberfläche anwenden. Wenn Sie z. b. ein Element näher an den Benutzer weitergeben, konzentriert sich der Benutzer instinktiv auf das-Element. Durch das Verschieben von UI-Elementen in der z-Achse können Sie eine visuelle Hierarchie zwischen Objekten einrichten, sodass Benutzer die Aufgaben auf natürliche und effiziente Weise in der app ausführen können.

## <a name="what-is-shadow"></a>Was ist Schatten?

Shadow ist eine Methode, mit der ein Benutzer eine Erhöhung der Rechte wahrnimmt Ein Licht oberhalb eines erhöhten Objekts erstellt einen Schatten auf der-Oberfläche unten. Je höher das Objekt ist, desto größer und weicher der Schatten wird. Erweiterte Objekte in der Benutzeroberfläche benötigen keine Schatten, aber Sie können die Darstellung der Erhöhung erhöhen.

In UWP-apps sollten Schatten in gezielter und nicht in ästhetischer Weise verwendet werden. Durch die Verwendung zu vieler Schatten wird die Fähigkeit des Schattens, den Benutzer zu fokussieren, verringert oder vermieden.

Wenn Sie Standard Steuerelemente verwenden, werden die themeshadow-Schatten automatisch in die Benutzeroberfläche integriert. Sie können jedoch mit der themeshadow-oder der DropShadow-API Schatten in Ihre Benutzeroberfläche einschließen. 

## <a name="themeshadow"></a>ThemeShadow

Der [themeshadow](/uwp/api/windows.ui.xaml.media.themeshadow) -Typ kann auf jedes XAML-Element angewendet werden, um die Schatten entsprechend der x-, y-und z-Koordinaten entsprechend zu zeichnen. Themeshadow passt sich auch automatisch an andere Umgebungs Spezifikationen an:

- Passt sich an die Änderungen an Beleuchtung, Benutzer Design, App-Umgebung und Shell an.
- Wendet Schatten auf Elemente automatisch basierend auf Ihrer z-Tiefe an. 
- Behält Elemente beim Verschieben und Ändern der Erhöhung der Rechte bei.
- Hält Schatten in der gesamten und in allen Anwendungen einheitlich.

Im folgenden wird erläutert, wie themeshadow für ein menuflyout implementiert wurde. Menuflyout verfügt über eine integrierte Oberfläche, bei der die Haupt Oberfläche auf 32 px erhöht wird, und jedes zusätzliche kaskadierende Menü wird geöffnet + 8px über dem Menü, von dem es geöffnet wird.

![Ein Screenshot von themeshadow, der auf ein menuflyout mit drei geöffneten, geöffnetten Menüs angewendet wird. Im ersten Menü wird der Wert 32px erhöht, und jedes nachfolgende Menü, das im vorherigen Menü geöffnet wird, wird um 8px erhöht, sodass es einen eindeutigen Schatten im Hintergrund hinterlässt.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>Themeshadow in allgemeinen Steuerelementen

Die folgenden allgemeinen Steuerelemente verwenden automatisch themeshadow zum Umwandeln von Shadowing aus 32px, sofern nicht anders angegeben:

- [Kontextmenü](../controls-and-patterns/menus.md), [Befehlsleiste](../controls-and-patterns/app-bars.md), [Befehls leisten-Flyout](../controls-and-patterns/command-bar-flyout.md), [Menüleiste](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Dialoge und Flyouts](../controls-and-patterns/dialogs.md) (Dialog bei 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [Dropdown Button, SplitButton,](../controls-and-patterns/buttons.md) UMSCHALT Fläche
- [Lehr Tipp](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Kalender/Datum/Uhrzeit-Picker](../controls-and-patterns/date-and-time.md)
- [QuickInfo (](../controls-and-patterns/tooltips.md) 16px)
- [Medien Transport-Steuer](../controls-and-patterns/media-playback.md#media-transport-controls)Element, [inktoolbar](../controls-and-patterns/inking-controls.md)
- [Verbundene Animationen](../motion/connected-animation.md)

Hinweis: Flyouts wenden bei der Kompilierung für Windows 10, Version 1903 oder ein neueres SDK nur themeshadow an.

### <a name="themeshadow-in-popups"></a>Themeshadow in Popups

Häufig ist es der Fall, dass die Benutzeroberfläche Ihrer APP ein Popup für Szenarien verwendet, in denen Sie die Aufmerksamkeit des Benutzers und die schnelle Aktion benötigen. Diese Beispiele sind hervorragend, wenn Schatten verwendet werden soll, um die Hierarchie in der Benutzeroberfläche Ihrer APP zu erstellen.

Themeshadow wandelt Schatten in einem beliebigen XAML-Element in einem [Popup](/uwp/api/windows.ui.xaml.controls.primitives.popup)automatisch um. Sie wandelt Schatten in den Hintergrund Inhalt der APP und alle anderen geöffneten Popups darunter um.

Um themeshadow mit Popups zu verwenden, verwenden Sie die `Shadow`-Eigenschaft, um einen themeshadow auf ein XAML-Element anzuwenden. Erhöhen Sie dann das Element aus anderen dahinter liegenden Elementen, z. b. mithilfe der z-Komponente der `Translation`-Eigenschaft.
Bei der meisten Popup-Benutzeroberfläche ist die empfohlene Standard Erhöhung relativ zum Inhalt der APP im Hintergrund 32 effektive Pixel.

Dieses Beispiel zeigt ein Rechteck in einem Popup, das einen Schatten in den Hintergrund Inhalt der APP und alle anderen Popups hinter ihm wirft:

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![Ein einzelnes rechteckiges Popup mit einem Schatten.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Deaktivieren von Standard-themeshadow für benutzerdefinierte Flyout-Steuerelemente

Steuerelemente, die auf [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [datepickerflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [menuflyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) oder [timepickerflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) basieren, verwenden automatisch themeshadow zum Umwandeln eines Schattens.

Wenn der Standard Schatten im Inhalt des Steuer Elements nicht korrekt aussieht, können Sie ihn deaktivieren, indem Sie die [isdefaultshadowenabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) -Eigenschaft auf `false` für den zugehörigen flyoutpresenter festlegen:

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>Themeshadow in anderen Elementen

Im Allgemeinen empfehlen wir Ihnen, sich sorgfältig mit der Verwendung von Schatten zu beschäftigen und deren Verwendung auf Fälle zu beschränken, in denen es eine sinnvolle visuelle Hierarchie einführt. Wir bieten jedoch eine Möglichkeit, einen Schatten von einem beliebigen Benutzeroberflächen Element zu umwandeln, wenn Sie über erweiterte Szenarien verfügen, die dies erfordern.

Zum Umwandeln eines Schattens aus einem XAML-Element, das sich nicht in einem Popup befindet, müssen Sie explizit die anderen Elemente angeben, die den Schatten in der `ThemeShadow.Receivers` Auflistung empfangen können. Empfänger dürfen in der visuellen Struktur kein Vorgänger des-Zauberers sein.

Dieses Beispiel zeigt zwei Rechtecke, die Schatten in ein Raster hinter Ihnen umwandeln:

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![Zwei Türkis Rechtecke nebeneinander, beides mit Schatten.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Bewährte Methoden für die Leistung von themeshadow

1. Das System legt ein Limit von 5 "Caster-Empfänger"-Paaren fest und deaktiviert den Schatten, wenn dies überschritten wird. Halten Sie sich an das vom System erzwungene Limit von 5 "Caster-Empfänger"-Paaren.

2. Beschränken Sie die Anzahl der benutzerdefinierten Empfänger Elemente auf den minimal erforderlichen Wert.

3. Wenn sich mehrere Empfänger Elemente in derselben Höhe befinden, versuchen Sie, diese zu kombinieren, indem Sie stattdessen ein einzelnes übergeordnetes Element als Ziel verwenden.

4. Wenn mehrere Elemente denselben Typ von Schatten in dieselben Empfänger Elemente umwandeln, fügen Sie den Schatten als freigegebene Ressource hinzu, und verwenden Sie Sie erneut.

## <a name="drop-shadow"></a>Schlagschatten

"DropShadow" reagiert nicht automatisch auf seine Umgebung und verwendet keine Lichtquellen. Beispiele für Implementierungen finden Sie in der [DropShadow-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Welchen Schatten sollte ich verwenden?

| Eigenschaft | ThemeShadow | DropShadow |
| - | - | - |
| **Min SDK** | Windows 10, Version 1903 | 14393 |
| **Anpassbarkeit** | Ja | Nein |
| **Instan** | Nein | Ja |
| **Helle Quelle** | Automatisch (standardmäßig Global, kann jedoch pro App überschrieben werden) | Keine |
| **In 3D-Umgebungen unterstützt** | Ja | Nein |

- Beachten Sie, dass der Zweck von Shadow darin besteht, eine sinnvolle Hierarchie und nicht als einfache visuelle Behandlung bereitzustellen.
- Im Allgemeinen wird die Verwendung von themeshadow empfohlen, das sich automatisch an seine Umgebung anpasst.
- Wenn Sie Bedenken bezüglich der Leistung haben, begrenzen Sie die Anzahl der Schatten, verwenden Sie eine andere visuelle Behandlung, oder verwenden Sie DropShadow.
- Wenn Sie über komplexere Szenarien verfügen, um eine visuelle Hierarchie zu erreichen, sollten Sie die Verwendung anderer visueller Behandlungen (z. b. Farbe) in Erwägung gezogen Wenn Schatten erforderlich ist, verwenden Sie DropShadow.
