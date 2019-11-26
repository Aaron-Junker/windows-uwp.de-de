---
description: Beschreibt die Schritte, mit denen Sie die Verwendbarkeit Ihrer UWP-App (Universelle Windows-Plattform) sicherstellen können, wenn ein Design mit hohem Kontrast aktiviert ist.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Designs mit hohem Kontrast
template: detail.hbs
ms.date: 09/28/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 617511d23bcabe342abe7477f617d81cd01182e6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257785"
---
# <a name="high-contrast-themes"></a>Designs mit hohem Kontrast  

Windows unterstützt Designs mit hohem Kontrast für das Betriebssystem und Apps, die von Benutzern aktiviert werden können. Designs mit hohem Kontrast verwenden eine kleine Palette von Farbkombinationen mit hohem Farbkontrast, durch die die Benutzeroberfläche leichter zu erkennen ist.

![Rechner im hellen Design und im Design „Hoher Kontrast (Schwarz)”](images/high-contrast-calculators.png)

*Rechner, der im hellen Design angezeigt wird, und hoher Kontrast schwarzes Design.*

Sie können über *Einstellungen > Erleichterte Bedienung > Hoher Kontrast* zu einem Design mit hohem Kontrast wechseln.

> [!NOTE]
> Designs mit hohem Kontrast sind nicht mit hellen und dunklen Designs zu verwechseln. Helle und dunkle Designs unterstützen eine deutlich größere Farbpalette ohne Farbkombinationen mit hohem Kontrast. Weitere helle und dunkle Designs finden Sie im Artikel zu [Farben](../style/color.md).

Während bei allgemeinen Steuerelementen vollständige Unterstützung für hohen Kontrast automatisch verfügbar ist, ist beim Anpassen der Benutzeroberfläche Vorsicht geboten. Die Ursache für den häufigsten Fehler bei der Verwendung von Designs mit hohem Kontrast ist die Inlinehartcodierung einer Steuerelementfarbe.

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Wird die Farbe `#E6E6E6` im ersten Beispiel inline festgelegt, behält das Raster diese Hintergrundfarbe in allen Designs bei. Wenn der Benutzer zum Design „Hoher Kontrast (Schwarz)” wechselt, erwartet er, dass Ihre App einen schwarzen Hintergrund hat. Da `#E6E6E6` fast weiß ist, sind einige Benutzer möglicherweise nicht in der Lage, mit Ihrer App zu interagieren.

Im zweiten Beispiel wird die [ **{ThemeResource}-Markuperweiterung**](../../xaml-platform/themeresource-markup-extension.md) verwendet, um auf eine Farbe in der [**ThemeDictionaries**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)-Sammlung zu verweisen, bei der es sich um eine dedizierte Eigenschaft eines [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)-Elements handelt. Die **ThemeDictionaries**-Sammlung ermöglicht es XAML, Farben automatisch basierend auf dem aktuellen Design des Benutzers auszutauschen.

## <a name="theme-dictionaries"></a>Designverzeichnisse

Wenn Sie die Systemstandardfarbe ändern müssen, erstellen Sie eine ThemeDictionaries-Sammlung für Ihre App.

1. Erstellen Sie zunächst die richtige Grundstruktur (sofern nicht bereits vorhanden). Erstellen Sie in „App.xaml” eine **ThemeDictionaries**-Sammlung, die mindestens **Default** und **HighContrast** enthält.
2. Erstellen Sie unter **Default** den gewünschten [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush)-Typ (in der Regel **SolidColorBrush**). Vergeben Sie einen *x:Key*-Namen, der für die Verwendung spezifisch ist.
3. Weisen Sie das gewünschte **Color**-Element zu.
4. Kopieren Sie das **Brush**-Objekt in **HighContrast**.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Im letzten Schritt bestimmen Sie, welche Farbe für hohen Kontrast verwendet werden soll. Dies wird im nächsten Abschnitt erläutert.

> [!NOTE]
> **HighContrast** ist nicht der einzige verfügbare Schlüsselname. Es gibt auch noch **HighContrastBlack**, **HighContrastWhite** und **HighContrastCustom**. In den meisten Fällen ist **HighContrast** ausreichend.

## <a name="high-contrast-colors"></a>Farben mit hohem Kontrast

Auf der Seite *Einstellungen > Erleichterte Bedienung > Hoher Kontrast* sind standardmäßig vier Designs mit hohem Kontrast verfügbar. 


![Einstellungen für hohen Kontrast](images/high-contrast-settings.png)  

*Nachdem der Benutzer eine Option ausgewählt hat, wird auf der Seite eine Vorschau angezeigt.*  

![Ressourcen mit hohem Kontrast](images/high-contrast-resources.png)  

*Sie können auf jedes Farbmuster in der Vorschau klicken, um den Wert zu ändern. Jedes Muster wird auch direkt einer XAML-Farb Ressource zugeordnet.*  

Jede **SystemColor*Color**-Ressource ist eine Variable, die die Farbe automatisch aktualisiert, wenn der Benutzer Designs mit hohem Kontrast wechselt. Im Folgenden finden Sie Richtlinien für die Verwendung der einzelnen Ressourcen.

Ressource | Verwendung |
|--------|-------|
**Systemcolorwindowtextcolor** | Textkörper, Überschriften, Listen, beliebiger Text, mit dem nicht interagiert werden kann |
| **Systemcolorhotlightcolor** | Hyperlinks |
| **Systemcolorgraytextcolor** | Deaktivierte Benutzeroberflächenelemente |
| **Systemcolorhighlighttextcolor** | Vordergrundfarbe für Text oder Benutzeroberflächenelemente, die sich in Bearbeitung befinden, die ausgewählt sind oder mit denen gegenwärtig interagiert wird |
| **Systemcolorhighlightcolor** | Hintergrundfarbe für Text oder Benutzeroberflächenelemente, die sich in Bearbeitung befinden, die ausgewählt sind oder mit denen gegenwärtig interagiert wird |
| **Systemcolorbuttontextcolor** | Vordergrundfarbe für Schaltflächen und beliebige Benutzeroberflächenelemente, mit denen interagiert werden kann |
| **Systemcolorbuttonfakecolor** | Hintergrundfarbe für Schaltflächen und beliebige Benutzeroberflächenelemente, mit denen interagiert werden kann |
| **Systemcolorwindowcolor** | Hintergrund von Seiten, Bereichen, Popups und Leisten |

Häufig ist es hilfreich, sich in vorhandenen Apps, Startseiten oder allgemeinen Steuerelementen anzusehen, wie andere Entwickler ähnliche Probleme beim Entwerfen für hohen Kontrast gelöst haben.

**Können**

* Beachten Sie nach Möglichkeit die Hintergrund-/Vordergrundpaare.
* Testen Sie alle vier Designs mit hohem Kontrast, während die App ausgeführt wird. Der Benutzer sollte die App bei einem Wechsel des Designs nicht neu starten müssen.
* Achten Sie auf Einheitlichkeit.

**Tue nicht**

* Hartcodierung einer Farbe im Design **HighContrast** – verwenden Sie die **SystemColor*Color**-Ressourcen.
* Berücksichtigen Sie bei der Auswahl einer Farbressource die Ästhetik. Denken Sie daran, dass sich die Farbressourcen mit dem Design ändern.
* Verwenden Sie **SystemColorGrayTextColor** nicht für sekundären oder als Hinweis fungierenden Textkörper.


Im vorherigen Beispiel müssen Sie eine Ressource für **BrandedPageBackgroundBrush** auswählen. Da der Name bereits besagt, dass dieser Pinsel für einen Hintergrund verwendet wird, ist **SystemColorWindowColor** eine gute Wahl.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Später können Sie dann in Ihrer App den Hintergrund festlegen.

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Beachten Sie, wie **\{themeresource-\}** zweimal verwendet wird, einmal, um auf **systemcolorwindowcolor** zu verweisen, und wieder, um auf **brandblogebackgroundbrush**zu verweisen. Beide Verweise sind erforderlich, damit zur Laufzeit das korrekte Design in Ihrer App verwendet wird. Dies ist ein guter Zeitpunkt, um die Funktionalität in Ihrer App zu testen. Der Hintergrund des Rasters wird automatisch aktualisiert, wenn Sie zu einem Design mit hohem Kontrast wechseln. Beim Wechseln zwischen verschiedenen Designs mit hohem Kontrast wird er ebenfalls aktualisiert.

## <a name="when-to-use-borders"></a>Wann sollten Rahmen verwenden werden?

Für den Hintergrund von Seiten, Bereichen, Popups und Leisten sollte im Design mit hohem Kontrast **SystemColorWindowColor** verwendet werden. Fügen Sie bei Bedarf einen Rahmen nur für hohen Kontrast hinzu, um wichtige Grenzen in Ihrer Benutzeroberfläche beizubehalten.

![Ein vom Rest der Seite abgegrenzter Navigationsbereich](images/high-contrast-actions-content.png)  

*Der Navigationsbereich und die Seite haben im hohen Kontrast dieselbe Hintergrundfarbe. Es ist von entscheidender Bedeutung, einen Rahmen mit hohem Kontrast zu teilen.*


## <a name="list-items"></a>Listenelemente

Im Design mit hohem Kontrast wird der Hintergrund von Elementen in ein [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) auf **SystemColorHighlightColor** festgelegt, wenn der Benutzer auf sie zeigt, sie drückt oder auswählt. Ein häufiger Fehler bei komplexen Listenelementen ist, dass die Farbe des Inhalts des Listenelements beim Zeigen, Drücken oder Auswählen nicht umgekehrt wird. Dies führt dazu, dass das Element nicht gelesen werden kann.

![Einfache Liste im hellen Design und im Design „Hoher Kontrast (Schwarz)”](images/high-contrast-list1.png)

*Eine einfache Liste im hellen Design (links) und hoher Kontrast schwarzes Design (rechts). Das zweite Element ist ausgewählt. Beachten Sie, dass die Textfarbe im hohen Kontrast invertiert wird.*


### <a name="list-items-with-colored-text"></a>Listenelemente mit farbigem Text

Das Festlegen von „TextBlock.Foreground” in der [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) des ListView-Steuerelements kann Probleme verursachen. Diese Einstellung wird meist vorgenommen, um eine visuelle Hierarchie zu erzeugen. Die Foreground-Eigenschaft ist für das [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) festgelegt, und TextBlock-Elemente in der „DataTemplate” erben die richtige Vordergrundfarbe, wenn auf das Element gezeigt, es gedrückt oder ausgewählt wird. Durch das Festlegen der Foreground-Eigenschaft funktioniert die Vererbung jedoch nicht.

![Komplexe Liste im hellen Design und im Design „Hoher Kontrast (Schwarz)”](images/high-contrast-list2.png)

*Komplexe Liste im hellen Design (links) und hoher Kontrast schwarzes Design (rechts). Im hohen Kontrast konnte die zweite Zeile des ausgewählten Elements nicht umkehren.*  

Sie können dieses Problem umgehen, indem Sie die Foreground-Eigenschaft bedingt über einen Stil in einer **ThemeDictionaries**-Sammlung festlegen. Da die **Foreground**-Eigenschaft nicht durch **SecondaryBodyTextBlockStyle** in **HighContrast** festgelegt wird, wird die Farbe korrekt umgekehrt.

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>Erkennen von hohem Kontrast

Sie können mithilfe von Membern der [**AccessibilitySettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings)-Klasse programmgesteuert überprüfen, ob das aktuelle Design ein Design mit hohem Kontrast ist.

> [!NOTE]
> Achten Sie darauf, dass Sie den **AccessibilitySettings**-Konstruktor in einem Bereich aufrufen, in dem die App initialisiert ist und bereits Inhalt anzeigt.

## <a name="related-topics"></a>Verwandte Themen  
* [Bedienungshilfen](accessibility.md)
* [Beispiel für UI-Kontraste und-Einstellungen](https://code.msdn.microsoft.com/windowsapps/High-Contrast-UI-Settings-9a310961)
* [XAML-Barrierefreiheits Beispiel](https://code.msdn.microsoft.com/windowsapps/XAML-accessibility-sample-d63e820d)
* [XAML-Beispiel für hohe Kontraste](https://code.msdn.microsoft.com/windowsapps/XAML-high-contrast-style-c0ce936f)
* [**Accessibilitysettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings)
