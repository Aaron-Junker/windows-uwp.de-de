---
author: knicholasa
description: Z-Tiefe, oder einen Pfad relativ Tiefe und Schattenkopien sind zwei Möglichkeiten, um die Integration von Tiefe in Ihre app, die Benutzern auf natürliche Weise und effizient zu unterstützen.
title: Z-Tiefe und Schattenkopien für UWP-apps
template: detail.hbs
ms.author: nichola
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, UWP
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: ab49f13d3938e55750ce523f9e0d4ae241304763
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817710"
---
# <a name="z-depth-and-shadow"></a>Z-Tiefe und Schatten

![Eine GIF-Datei mit vier hellgrauen Rechtecke, die ein gestapeltes diagonal, untereinander. Das GIF-Bild wird animiert, sodass Schatten angezeigt und ausgeblendet werden.](images/elevation-shadow/shadow.gif)

Erstellen einer visuellen Hierarchie von Elementen auf der Benutzeroberfläche erleichtert die Benutzeroberfläche überprüft und übermittelt wichtige zu konzentrieren. Eine Erhöhung der Rechte für den Einsatz von select-Elemente der Benutzeroberfläche weiterleiten, Act wird häufig verwendet, um solche Hierarchie in der Software zu erreichen. Dieser Artikel beschreibt, wie Sie die Erhöhung der Rechte in einer UWP-app mithilfe von Z-Tiefe und Schatten zu erstellen.

Z-Tiefe ist ein Ausdruck, für 3D app-Ersteller verwendet, um anzugeben, die Entfernung zwischen zwei Oberflächen entlang der z-Achse. Es wird veranschaulicht, wie weit ein Objekt in der Ereignisanzeige ist. Betrachten Sie sie als ein ähnliches Konzept in X / y-Koordinaten, aber in der Z-Richtung.

## <a name="why-use-z-depth"></a>Gründe für die Verwendung der Z-Tiefe

In der realen Welt tendenziell auf Objekte zu konzentrieren, die näher an uns. Wir können dieses räumlichen instinkthafte Verhalten auf digitale Benutzeroberfläche sowie anwenden. Z. B. Wenn Sie ein Element ist näher an dem Benutzer bereitstellen, konzentriert klicken Sie dann der Benutzer instinktiv Elements sich auf. Durch Verschieben UI-Elemente näher in z-Achse können Sie visuelle Hierarchie zwischen den Objekten, die Unterstützung von Benutzern auf natürliche Weise und effizient in Ihrer app Aufgaben einrichten.

## <a name="what-is-shadow"></a>Was ist die Schattenkopie?

Schattenkopien ist eine Möglichkeit, die ein Benutzer über erhöhte Rechte wahrnimmt. LED über ein Objekt mit erhöhten Rechten erstellt einen Schatten auf der Oberfläche, die weiter unten. Je höher das Objekt, das größer und weicher wird der Schatten. Mit erhöhten Rechten Objekte auf der Benutzeroberfläche benötigen Schatten, aber sie können die Darstellung der Erhöhung der Rechte zu erstellen.

In UWP-apps sollte die Shadows in einer zweckmäßigen anstelle von ästhetischen Weise verwendet werden. Zu viele Schatten wird verringern oder beseitigen die Möglichkeit des Schattens den Benutzer zu konzentrieren.

Wenn Sie die standardmäßigen Steuerelemente verwenden, werden ThemeShadow Shadows automatisch in die Benutzeroberfläche integriert werden. Allerdings können Sie auf der Benutzeroberfläche auch manuell Shadows einschließen, mithilfe der ThemeShadow oder die DropShadow-APIs. 

## <a name="themeshadow"></a>ThemeShadow

Die ThemeShadow, die Typ kann, auf ein XAML-Element zum Zeichnen angewendet werden führt Shadowing für entsprechend basierend auf X, y, Z-Koordinaten. ThemeShadow passt sich automatisch auch für andere environmental Spezifikationen:

- Anpassung an datenbankänderungen Beleuchtung, die Benutzer-Design, app-Umgebung und -Shell.
- Zeichnen von Schatten und Elemente, die automatisch basierend auf deren Z-Tiefe gilt. 
- Hält Elemente synchron, wenn sie verschieben oder ändern die Erhöhung der Rechte.
- Hält Schatten konsistent im gesamten und Anwendungen.

Hier ist, wie auf eine MenuFlyout ThemeShadow implementiert wurde. MenuFlyout verfügt über einen integrierten Umgebung, in dem die main-Oberfläche auf 32 Pixel erhöht wird und jede zusätzliches hierarchisches Menü geöffnet wird, und 8px oben im Menü aus öffnen.

![Ein Screenshot, der ThemeShadow auf eine MenuFlyout mit drei öffnen, verschachtelte Menüs angewendet werden soll. Das erste Menü mit erhöhten Rechten 32 Pixel und jedes nachfolgende Menü, das im vorherigen Menü geöffnet wird mit erhöhten Rechten 8px Weitere so, dass es im Hintergrund einen unterschiedlichen Schatten lässt.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow gemeinsam steuert.

Die folgenden allgemeinen Steuerelemente verwenden automatisch ThemeShadow zum Zeichnen von Schatten von 32 Pixel Tiefe umgewandelt werden, sofern nicht anders angegeben:

- [Kontextmenü](../controls-and-patterns/menus.md), [Befehlsleiste](../controls-and-patterns/app-bars.md), [Flyout Befehlsleisten](../controls-and-patterns/command-bar-flyout.md), [Menüleiste](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Dialogfelder und Flyouts](../controls-and-patterns/dialogs.md) (Dialogfeld 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Kalender/Datum/Uhrzeit-Farbauswahl](../controls-and-patterns/date-and-time.md)
- [Tooltip](../controls-and-patterns/tooltips.md) (16px)
- [Medientransport Steuerelement](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Verbundene animation](../motion/connected-animation.md)

Hinweis: Flyouts wird ThemeShadow bei der Kompilierung für Windows 10, Version 1903 sein oder eine neuere SDK nur angewendet werden.

### <a name="themeshadow-in-popups"></a>ThemeShadow in Popups

Es ist häufig der Fall, dass UIs Ihrer app ein Popup für Szenarien verwendet, in denen Sie benötigen Aufmerksamkeit und schnelle Aktion des Benutzers. Dies sind gute Beispiele für, wenn Schatten verwendet werden soll, können Sie die Hierarchie in Ihrer app Benutzeroberfläche zu erstellen.

ThemeShadow wandelt automatisch Shadows bei Anwendung auf ein XAML-Element in einem [Popup](/uwp/api/windows.ui.xaml.controls.primitives.popup). Er wird Schatten auf dem Inhalt des app-Hintergrund hinter diese und andere open Popups darunter umgewandelt.

Um ThemeShadow mit Popups verwenden möchten, verwenden Sie die `Shadow` Eigenschaft, um eine ThemeShadow für ein XAML-Element gelten. Klicken Sie dann erhöhen das Element von anderen Elementen vorhanden, und es z. B. mithilfe der Z-Komponente von der `Translation` Eigenschaft.
Für die meisten Popup-Benutzeroberfläche ist die empfohlene Standardvorgehensweise-Höhe relativ zu den app-Hintergrund-Inhalten 32 effektive Pixel.

Dieses Beispiel zeigt ein Rechteck in einem Popupfenster einen Schatten auf der app-Hintergrund-Inhalt und andere Popups dahinter verwendet wird:

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

![Einen einzelnen rechteckigen Popupfenster mit einem Schatten.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Deaktivieren die standardmäßige steuert ThemeShadow auf benutzerdefinierte Flyout

Steuerelemente auf der Grundlage [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) oder [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) verwenden automatisch ThemeShadow zum Umwandeln von einem Schatten.

Wenn der Standard-Schatten nicht richtig Aussehen Ihrer Kontrolle den Inhalt dann können Sie es durch Festlegen von Deaktivieren der [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) Eigenschaft `false` auf den zugeordneten FlyoutPresenter:

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

Im Allgemeinen empfehlen wir Ihnen, über die Verwendung von Schattenkopien mit Vorsicht und dessen Verwendung in Fällen, in dem sie aussagekräftige visuelle Hierarchie führt, zu beschränken. Allerdings bieten wir eine Möglichkeit, einen Schatten beliebiges Benutzeroberflächenelement umgewandelt werden, für den Fall, dass Sie erweiterte Szenarien, die es erfordern.

Um einen Schatten von einem XAML-Element umzuwandeln, die nicht in einem Popupfenster, müssen Sie explizit angeben die anderen Elemente, die die Schattenkopie in empfangen kann die `ThemeShadow.Receivers` Auflistung. Empfänger darf nicht über ein Vorgänger des der Caster in der visuellen Struktur sein.

Dieses Beispiel zeigt zwei Rechtecke, die Schatten in einem Raster zu ihnen zugrunde liegenden umgewandelt:

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

![Zwei türkises Rechtecke nebeneinander, sowohl mit Schatten.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Bewährte Methoden zur leistungsverbesserung von ThemeShadow

1. Das System legt einen Grenzwert von 5 Caster-Empfänger-Paaren fest und deaktiviert Schatten Wenn diese überschritten wird. Bleiben Sie auf das System erzwungen-Limit von 5 Caster-Empfänger-Paaren.

2. Die Anzahl der Elemente der benutzerdefinierten Empfänger auf ein Minimum zu beschränken.

3. Wenn mehrere Empfänger-Elemente auf die gleiche Höhe sind, versuchen Sie sie durch eine Ausrichtung auf ein einzelnes übergeordnetes Element stattdessen kombiniert.

4. Wenn mehrere Elemente den gleichen Typ von Schatten auf die gleiche Receiver-Elementen umgewandelt werden den Schatten als eine freigegebene Ressource hinzufügen, und wiederzuverwenden.

## <a name="drop-shadow"></a>Schlagschatten

DropShadow-Steuerelements ist nicht auf ihre Umgebung automatisch reagieren und Lichtquellen nicht verwendet. Implementierungen, z. B. finden Sie unter den [DropShadow-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Die Schattenkopie sollte ich verwenden?

| Eigenschaft | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | Windows 10, Version 1903 | 14393 |
| **Anpassungsfähigkeit** | Ja | Nein |
| **Anpassung** | Nein | Ja |
| **Lichtquelle** | Automatische (standardmäßig, global aber können pro app überschreiben) | Keine |
| **In 3D Umgebungen unterstützt** | Ja | Nein |

- Beachten Sie, dass der Zweck des Schattens sinnvollen Hierarchie, und nicht als eine einfache visuelle Erläuterung zu halten.
- Im Allgemeinen empfiehlt ThemeShadow, die automatisch mit ihrer Umgebung passt.
- Für Bedenken hinsichtlich Leistung die Anzahl der Schatten, verwenden Sie andere visuelle Erläuterung oder DropShadow-Steuerelements verwenden.
- Wenn Sie mehr Szenarien zum visuellen Hierarchie zu erreichen erweitert haben, sollten erwägen Sie, andere visual behandelt (z. B. Farbe) zu verwenden. Wenn Schattenkopien erforderlich ist, verwenden Sie DropShadow-Steuerelements.
