---
author: knicholasa
description: Z-Tiefe oder relative Tiefe und Schatten sind zwei Möglichkeiten, Tiefeninformationen in Ihre App einzubeziehen, um den Benutzern ein natürliches und effizientes Fokussieren zu ermöglichen.
title: Z-Tiefe und Schatten für UWP-Apps
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081387"
---
# <a name="z-depth-and-shadow"></a>Z-Tiefe und Schatten

![Ein GIF, das vier graue Rechtecke zeigt, die diagonal übereinander gestapelt sind. Das GIF ist animiert, sodass die Schatten erscheinen und verschwinden.](images/elevation-shadow/shadow.gif)

Das Erstellen einer visuellen Hierarchie in Ihrer Benutzeroberfläche macht es einfach, die Benutzeroberfläche zu durchsuchen und transportiert die Information, was wichtig ist und beachtet werden muss. Hervorhebung, das In-den-Vordergrund-Stellen bestimmter Elemente Ihrer Benutzeroberfläche, wird häufig verwendet, um eine solche Hierarchie in Software zu erreichen. In diesem Artikel wird das Erstellen von Hervorhebung in einer UWP-App mithilfe von Z-Tiefe und Schatten erörtert.

Z-Tiefe ist ein Begriff, der von 3D-App-Entwicklern verwendet wird, um den Abstand zwischen zwei Oberflächen entlang der Z-Achse anzugeben. Er veranschaulicht, wie nah ein Objekt dem Beobachter ist. Stellen Sie es sich als ein ähnliches Konzept wie x/y-Koordinaten vor, aber in der Z-Richtung.

## <a name="why-use-z-depth"></a>Gründe für die Verwendung von Z-Tiefe

In der physischen Welt neigen wir dazu, auf die Objekte zu fokussieren, die uns näher sind. Wir können diesen räumlichen Instinkt ebenfalls auf digitale Benutzeroberflächen anwenden. Wenn Sie beispielsweise ein Element näher an den Benutzer heranbringen, wird der Benutzer instinktiv auf das Element fokussieren. Durch das nähere Heranrücken von Elementen der Benutzeroberfläche auf der Z-Achse können Sie eine visuelle Hierarchie unter Objekten erstellen, was es Benutzern erleichtert, Aufgaben in deiner App natürlich und effizient abzuschließen.

## <a name="what-is-shadow"></a>Was ist Schatten?

Schatten sind eine Art, wie Benutzer Hervorhebung wahrnehmen. Licht oberhalb eines hervorgehobenen Objekts schafft einen Schatten auf der Oberfläche darunter. Je höher das Objekt, desto größer und weicher wird der Schatten. Hervorgehobene Objekte auf Ihrer Benutzeroberfläche müssen keine Schatten aufweisen, sie unterstützen aber den Eindruck von Hervorhebung.

In UWP-Apps sollten Schatten eher in zweckmäßiger statt in ästhetischer Weise verwendet werden. Die Verwendung zu vieler Schatten schwächt die Funktion von Schatten, den Benutzer zu leiten, oder beseitigt sie gar ganz.

Wenn Sie Standardsteuerelemente verwenden, werden Schatten automatisch in Ihre Benutzeroberfläche einbezogen. Sie können Schatten jedoch manuell in Ihre Benutzeroberfläche einbeziehen, indem Sie die APIs ThemeShadow oder DropShadow verwenden. 

## <a name="themeshadow"></a>ThemeShadow

Der [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow)-Typ kann auf jedes XAML-Element angewendet werden, um auf der Grundlage der x-, y- und z-Koordinaten passende Schatten zu zeichnen. ThemeShadow passt sich außerdem automatisch an andere Umgebungsspezifikationen an:

- Passt sich an Änderungen von Beleuchtung, Benutzerdesign, App-Umgebung und Shell an.
- Wendet Schatten automatisch auf Elemente auf der Grundlage ihrer Z-Tiefe an. 
- Hält Elemente bei Bewegungen und Änderungen der Hervorhebung synchron.
- Hält Schatten innerhalb der Anwendung und anwendungsübergreifend konsistent.

Hier sehen Sie, wie ThemeShadow für ein MenuFlyout implementiert wurde. MenuFlyout weist eine integrierte Benutzererfahrung auf, bei der die Hauptoberfläche auf 32 Pixel angehoben ist und jedes weitere kaskadierende Menü 8 Pixel oberhalb des Menüs geöffnet wird, aus dem es sich öffnet.

![Screenshot von ThemeShadow, angewendet auf ein MenuFlyout mit drei geöffneten verschachtelten Menüs. Das erste Menü ist auf 32 Pixel angehoben, und jedes nachfolgende Menü, das sich aus dem vorherigen Menü öffnet, ist um weitere 8 Pixel hervorgehoben, sodass es einen deutlichen Schatten auf dem Hintergrund hinterlässt.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow in allgemeinen Steuerelementen

Die folgenden allgemeinen Steuerelemente verwenden automatisch ThemeShadow, um Schatten von 32 Pixel Tiefe zu werfen, sofern nichts anderes angegeben ist:

- [Kontextmenü](../controls-and-patterns/menus.md), [Befehlsleiste](../controls-and-patterns/app-bars.md), [Befehlsleisten-Flyout](../controls-and-patterns/command-bar-flyout.md), [MenuBar](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Dialogfelder und Flyouts](../controls-and-patterns/dialogs.md) (Dialogfeld bei 64 Pixel)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Auswahlsteuerelemente für Kalender, Datum und Uhrzeit](../controls-and-patterns/date-and-time.md)
- [QuickInfo](../controls-and-patterns/tooltips.md) (16 Pixel)
- [Medientransport-Steuerelement](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Verbundene Animationen](../motion/connected-animation.md)

Hinweis: Für Flyouts wird ThemeShadow nur beim Kompilieren für Windows 10, Version 1903, oder ein neueres SDK angewendet.

### <a name="themeshadow-in-popups"></a>ThemeShadow in Popups

Häufig verwenden Benutzeroberflächen von Apps ein Popup in Szenarien, in denen die Aufmerksamkeit des Benutzers und ein schnelles Eingreifen erforderlich sind. Dies sind tolle Beispiele dafür, wann Schatten verwendet werden sollten, um eine Hierarchie in der Benutzeroberfläche Ihrer App zu erstellen.

ThemeShadow wirft automatisch Schatten, wenn es auf ein beliebiges XAML-Element in einem [Popup](/uwp/api/windows.ui.xaml.controls.primitives.popup) angewendet wird. Es wirft Schatten auf die Hintergrundinhalte der App hinter ihm und alle anderen geöffneten Popups unter ihm.

Um ThemeShadow für Popups einzusetzen, verwenden Sie die `Shadow`-Eigenschaft, um einen ThemeShadow auf ein XAML-Element anzuwenden. Heben Sie anschließend das Element gegenüber anderen Elementen hinter ihm hervor, beispielsweise mithilfe der z-Komponente der `Translation`-Eigenschaft.
Für den größten Teil von Popup-UI beträgt die empfohlene Standardhervorhebung relativ zum Inhalt des App-Hintergrunds 32 effektive Pixel.

Dieses Beispiel zeigt ein Rechteck in einem Popup, das einen Schatten auf den App-Hintergrundinhalt und alle anderen Popups hinter ihm wirft:

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

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Deaktivieren von standardmäßigem ThemeShadow für benutzerdefinierte Flyout-Steuerelemente

Steuerelemente, die auf [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) oder [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) basieren, verwenden automatisch ThemeShadow, um einen Schatten zu werfen.

Wenn der Standardschatten auf dem Inhalt Ihres Steuerelements nicht richtig aussieht, können Sie es deaktivieren, indem Sie die [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled)-Eigenschaft für den zugeordneten FlyoutPresenter auf `false` festlegen:

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>ThemeShadow in anderen Elementen

Im Allgemeinen empfehlen wir Ihnen, sich sorgfältig mit der Verwendung von Schatten zu beschäftigen und ihren Einsatz auf Fälle zu beschränken, in denen sie zu einer sinnvollen visuellen Hierarchie führen. Wir bieten aber eine Möglichkeit, jedes beliebige UI-Element einen Schatten werfen zu lassen, falls das für erweiterte Szenarien erforderlich sein sollte.

Um einen Schatten von einem XAML-Element werfen zu lassen, das sich nicht in einem Popup befindet, müssen Sie explizit die anderen Elemente angeben, die den Schatten in der `ThemeShadow.Receivers`-Sammlung empfangen können. Als Empfänger kommt kein Vorgänger des werfenden Elements in der visuellen Baumstruktur in Frage.

Dieses Beispiel zeigt zwei Rechtecke, die Schatten auf ein Raster hinter ihnen werfen:

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

![Zwei türkisfarbene Rechtecke nebeneinander, beide mit Schatten.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Bewährte Methoden zur Leistungssteigerung für ThemeShadow

1. Für das System ist ein Grenzwert von 5 Werfer-Empfänger-Paaren festgelegt, und Schatten werden deaktiviert, wenn dieser Wert überschritten wird. Halten Sie sich an den vom System durchgesetzten Grenzwert von 5 Werfer-Empfänger-Paaren.

2. Schränken Sie die Anzahl der benutzerdefinierten Empfängerelemente auf das erforderliche Minimum ein.

3. Wenn sich mehrere Empfängerelemente auf der gleichen Hervorhebungsstufe befinden, versuchen Sie, sie zu kombinieren, indem Sie stattdessen ein einzelnes übergeordnetes Element anzielen.

4. Wenn mehrere Elemente die gleiche Art Schatten auf die gleichen Empfängerelemente werfen, fügen Sie den Schatten als freigegebene Ressource hinzu, und verwenden Sie ihn erneut.

## <a name="drop-shadow"></a>Schlagschatten

DropShadow reagiert nicht automatisch auf seine Umgebung und verwendet keine Lichtquellen. Beispielimplementierungen finden Sie in der [DropShadow-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Welchen Schatten soll ich verwenden?

| Eigenschaft | ThemeShadow | DropShadow |
| - | - | - |
| **Min SDK** | Windows 10, Version 1903 | 14393 |
| **Anpassungsfähigkeit** | Ja | Nein |
| **Benutzerdefinierte Anpassung** | Nein | Ja |
| **Lichtquelle** | Automatisch (standardmäßig global, kann jedoch pro App überschrieben werden) | Keine |
| **In 3D-Umgebungen unterstützt** | Ja | Nein |

- Beachten Sie, dass der Zweck von Schatten darin besteht, eine sinnvolle Hierarchie zu schaffen, keine einfache visuelle Bereicherung.
- Im allgemeinen empfehlen wir die Verwendung von ThemeShadow, das sich automatisch an seine Umgebung anpasst.
- Bei Bedenken hinsichtlich der Leistung schränken Sie die Anzahl der Schatten ein, verwenden Sie eine andere visuelle Darstellung, oder verwenden Sie DropShadow.
- Wenn Sie visuelle Hierarchie in komplexeren Szenarien schaffen müssen, erwägen Sie die Verwendung anderer visueller Darstellungen (beispielsweise Farbe). Wenn Schatten erforderlich ist, verwenden Sie DropShadow.
