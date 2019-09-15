---
description: Reveal ist ein Lichteffekt, der die interaktiven Elemente Ihrer App mit Tiefe und Fokus versehen kann.
title: Reveal Highlight
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0810365eeb0023a31862d31213862e2b3bce8db8
ms.sourcegitcommit: 5687e5340f8d78da95c3ac28304d1c9b8960c47d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70930348"
---
# <a name="reveal-highlight"></a>Reveal Highlight

![Herobild](images/header-reveal-highlight.svg)

Reveal Highlight ist ein Lichteffekt, der interaktive Elemente wie z.B. Befehlsleisten hervorhebt, wenn der Benutzer den Mauszeiger in deren Nähe bewegt. 

> **Wichtige APIs:** [„RevealBrush“-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [„RevealBackgroundBrush“-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [„RevealBorderBrush“-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [„RevealBrushHelper“-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [„VisualState“-Klasse](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>Funktionsweise
Reveal Highlight hebt interaktive Elemente hervor, indem der Container des Elements hervorgehoben wird, wenn sich der Mauszeiger nähert, wie in der folgenden Abbildung gezeigt wird:

![Visuelle Einblendung](images/Nav_Reveal_Animation.gif)

Da durch Einblendungen die ausgeblendeten Rahmen um Objekte herum angezeigt werden, entwickeln Benutzer dank Reveal ein besseres Verständnis von dem Raum, mit dem sie interagieren. Darüber hinaus erfahren sie auf diese Weise, welche Aktionen verfügbar sind. Dies ist besonders wichtig bei Listensteuerelementen und Gruppen von Schaltflächen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/Reveal">die App zu öffnen und Reveal in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Videozusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>Verwendung

„Reveal” funktioniert bei einigen Steuerelementen automatisch. Bei anderen Steuerelementen können Sie „Reveal” aktivieren, indem Sie dem Steuerelement ein spezielles Format zuweisen, wie im Abschnitt [Aktivieren von „Reveal” für andere Steuerelemente](#enabling-reveal-on-other-controls) und [Aktivieren von „Reveal” für allgemeine Steuerelemente](#enabling-reveal-on-custom-controls) dieses Artikels beschrieben wird.

## <a name="controls-that-automatically-use-reveal"></a>Steuerelemente, die „Reveal” automatisch verwenden

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

Diese Abbildungen zeigen Reveal Highlight bei verschiedenen Steuerelementen:

![Beispiele für „Reveal”](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>Aktivieren von „Reveal” für andere Steuerelemente

Für Szenarien, in denen „Reveal“ angewendet werden sollte (diese Steuerelemente sind Hauptinhalt und/oder werden in einer Liste oder Sammlungsausrichtung verwendet), haben wir optionale Ressourcenformate bereitgestellt, mit denen Sie „Reveal“ für solche Situationen aktivieren können.

Diese Steuerelemente verfügen standardmäßig nicht über „Reveal“, da sie kleinere Steuerelemente sind, die in der Regel als Hilfssteuerelemente für die zentralen Punkte Ihrer Anwendung dienen. Alle Apps sind jedoch verschieden, und wenn diese Steuerelemente in Ihrer App am häufigsten verwendet werden, stehen Ihnen einige Formate als Hilfe zur Verfügung:

| Name des Steuerelements   | Ressourcenname |
|----------|:-------------:|
| Schaltfläche |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem („Reveal“ über dem Inhalt) | GridViewItemRevealBackgroundShowsAboveContentStyle |

Um diese Formate anzuwenden, legen Sie die Eigenschaft [Style](/uwp/api/Windows.UI.Xaml.Style) folgendermaßen fest:

```xaml
<Button Content="Button Content" Style="{ThemeResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>„Reveal“ in Designs

„Reveal“ ändert sich geringfügig je nach dem angeforderten Design des Steuerelements, der App oder der Einstellung des Benutzers. Beim Design „Dark“ ist das Licht für „Border“ und „Hover“ weiß; beim Design „Light“ dagegen werden nur die Rahmen hellgrau angezeigt.

![Reveal mit „Dark“ und „Light“](images/Dark_vs_LightReveal.png)

Wenn Sie im Design „Light“ weiße Rahmen anzeigen möchten, legen Sie das angeforderte Design für das Steuerelement einfach auf „Dark“ fest.

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

Oder ändern Sie den Wert für „TargetTheme” bei „RevealBorderBrush” auf „Dark“. Bitte beachten Sie Folgendes! Wenn „TargetTheme” auf „Dark“ festgelegt ist, wird „Reveal“ weiß angezeigt. Wenn es aber auf „Light” festgelegt ist, werden die Rahmen von „Reveal“ grau angezeigt.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>Aktivieren von „Reveal” für benutzerdefinierte Steuerelemente

Sie können „Reveal” für benutzerdefinierte Steuerelemente hinzufügen. Bevor Sie dies tun, ist es hilfreich, etwas mehr über die Funktionsweise des Effekts „Reveal” zu erfahren. „Reveal“ besteht aus zwei getrennten Effekten: **Reveal Border** und **Reveal Hover**.

- **Border** zeigt die Rahmen der interaktiven Elemente an, wenn sich ein Zeiger nähert. Dadurch können Objekte in der Nähe ähnliche Aktionen wie das gerade fokussierte Objekt ausführen.
- Durch **Hover** wird das Element, auf das gezeigt oder das fokussiert wird, mit einem leichten Schein umgeben, und beim Anklicken wird eine gedrückte Animation angezeigt. 

![Ebenen von „Reveal“](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


Diese Effekte werden durch zwei Pinsel definiert: 
* „Reveal” für Rahmen wird durch **RevealBorderBrush** definiert.
* „Reveal” für Hover wird durch **RevealBackgroundBrush** definiert.

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
In den meisten Fällen wird „Reveal” für bestimmte Steuerelemente von uns automatisch aktiviert. Andere Steuerelemente müssen jedoch über das Anwenden eines Formats oder das Ändern der zugehörigen Vorlagen direkt aktiviert werden.

### <a name="when-to-add-reveal"></a>Wann sollte „Reveal” hinzugefügt werden
Sie können „Reveal” Ihren benutzerdefinierten Steuerelementen hinzufügen. Allerdings sollten Sie vorher den Typ des Steuerelements und sein Verhalten festlegen. 
* Wenn Ihr benutzerdefiniertes Steuerelement ein einzelnes interaktives Element ist und keine ähnlichen Steuerelemente (wie z.B. Menüelemente) auf derselben Oberfläche angezeigt werden, benötigt Ihr benutzerdefiniertes Steuerelement „Reveal“ wahrscheinlich nicht.  
* Besitzen Sie eine Gruppe von verwandten interaktiven Inhalten oder Elementen, benötigt dieser Bereich Ihrer App wahrscheinlich „Reveal“ – dies wird häufig als [Steuerung](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding) der Oberfläche bezeichnet.

So sollte beispielsweise eine allein angezeigte Schaltfläche „Reveal“ nicht verwenden, während eine Reihe von Schaltflächen in einer Befehlsleiste es verwenden sollte.

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>Hinzufügen von „Reveal” mithilfe der Steuerelementvorlage 
Um „Reveal“ für benutzerdefinierte Steuerelemente oder Steuerelemente mit neuen Vorlagen zu aktivieren, ändern Sie diese Vorlage für das Steuerelement. Die meisten Steuerelementvorlagen besitzen ein Raster im Stamm. Aktualisieren Sie [VisualState](/uwp/api/windows.ui.xaml.visualstate) dieses Stammrasters, um „Reveal“ verwenden zu können:

```xaml
<VisualState x:Name="PointerOver">
    <VisualState.Setters>
        <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
        <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
        <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
        <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
    </VisualState.Setters>
</VisualState>
```

Es ist wichtig zu beachten, dass für „Reveal“ in seinem visuellen Zustand sowohl der Pinsel als auch die Setter benötigt werden, damit es einwandfrei funktioniert. Durch einfaches Festlegen des Pinsels für ein Steuerelement auf eine unserer „Reveal“-Pinselressourcen allein wird „Reveal“ für dieses Steuerelement nicht aktiviert. Wenn aber nur die Ziele oder Einstellungen verwendet werden, ohne die Werte als „Reveal“-Pinsel festgelegt zu haben, wird Reveal ebenfalls nicht aktiviert.

Weitere Informationen zum Ändern von Steuerelementvorlagen finden Sie im Artikel [XAML-Steuerelementvorlagen](../controls-and-patterns/control-templates.md).

Wir haben eine Reihe von “Reveal“-Systempinseln erstellt, mit denen Sie Ihre Vorlage anpassen können. Sie können z.B. den Pinsel **ButtonRevealBackground** zum Erstellen eines Hintergrunds für eine benutzerdefinierte Schaltfläche oder den Pinsel **ListViewItemRevealBackground** für benutzerdefinierte Listen usw. verwenden. (Informationen zur Funktionsweise von Ressourcen in XAML finden Sie im Artikel [Xaml-Ressourcenverzeichnis](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).)

### <a name="full-template-example"></a>Beispiel für eine vollständige Vorlage

Hier sehen Sie eine gesamte Vorlage für das gewünschte Erscheinungsbild einer „Reveal“-Schaltfläche :

```xaml
<Style TargetType="Button" x:Key="ButtonStyle1">
    <Setter Property="Background" Value="{ThemeResource ButtonRevealBackground}" />
    <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
    <Setter Property="BorderBrush" Value="{ThemeResource ButtonRevealBorderBrush}" />
    <Setter Property="BorderThickness" Value="2" />
    <Setter Property="Padding" Value="8,4,8,4" />
    <Setter Property="HorizontalAlignment" Value="Left" />
    <Setter Property="VerticalAlignment" Value="Center" />
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="UseSystemFocusVisuals" Value="True" />
    <Setter Property="FocusVisualMargin" Value="-3" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid x:Name="RootGrid" Background="{TemplateBinding Background}">

                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="PointerOver">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Pressed">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="Pressed" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerDownThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundDisabled}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushDisabled}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundDisabled}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>

              </VisualStateManager.VisualStateGroups>
                    <ContentPresenter x:Name="ContentPresenter"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    Content="{TemplateBinding Content}"
                    ContentTransitions="{TemplateBinding ContentTransitions}"
                    ContentTemplate="{TemplateBinding ContentTemplate}"
                    Padding="{TemplateBinding Padding}"
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                    AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>Optimieren des Effekts „Reveal” für ein benutzerdefiniertes Steuerelement 

Wenn Sie „Reveal” für ein benutzerdefiniertes Steuerelement oder ein Steuerelement mit neuer Vorlage oder aber eine benutzerdefinierte Befehlsoberfläche aktivieren, können Ihnen diese Tipps beim Optimieren des Effekts helfen:
 
* Auf benachbarten Elementen mit einer Größe, die in Höhe oder Breite (insbesondere in Listen) nicht ausgerichtet ist: Entfernen Sie das Verhalten des Rahmens, und aktivieren Sie die Rahmen nur für das Hovern.
* Bei Befehlselementen, die häufig aktiviert oder deaktiviert werden: Platzieren Sie den Pinsel für den Rahmen auf die Backplates der Elemente sowie deren Rahmen, um ihren Zustand zu betonen.
* Bei benachbarten Steuerelementen, die sich fast berühren: Fügen Sie einen Rand von 1 Pixel zwischen den beiden Elementen hinzu. 

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen
### <a name="do"></a>Empfohlen:
- Verwenden Sie „Reveal” für Elemente, bei denen der Benutzer viele Aktionen (CommandBars, Navigationsmenüs) ausführen kann.
- Verwenden Sie „Reveal” in Gruppierungen von interaktiven Elementen, die standardmäßig keine visuellen Trennlinien haben (Listen, Menübänder).
- Verwenden Sie „Reveal” in Bereichen mit vielen interaktiven Elementen (Befehlsszenarien).
- Fügen Sie einen Rand von 1 Pixel zwischen „Reveal“-Elementen hinzu.

### <a name="dont"></a>Nicht empfohlen
- Verwenden Sie „Reveal” nicht auf statischen Inhalten (Hintergrund, Text).
- Verwenden Sie „Reveal” nicht auf Popups, Flyouts oder Dropdownlisten.
- Verwenden Sie „Reveal” nicht in einzelnen, isolierten Situationen.
- Verwenden Sie „Reveal“ nicht auf sehr großen Elementen (größer als 500 Epx).
- Verwenden Sie „Reveal” nicht in sicherheitsbezogenen Entscheidungen, da es den Benutzer beim Lesen der Nachricht, die Sie ihm übermitteln müssen, ablenken kann.


## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="reveal-and-the-fluent-design-system"></a>„Reveal” und das Fluent Design-System

 Mit dem Fluent Design-System können Sie moderne, klare Benutzeroberflächen erstellen, die Licht, Tiefe, Bewegung, Material und Skalierung enthalten. „Reveal” ist eine Komponente des Fluent Design-Systems, die Ihrer App Lichteffekte hinzufügt. Weitere Informationen finden Sie in der [Übersicht über Fluent Design für UWP](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Verwandte Artikel

- [RevealBrush-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylic](acrylic.md)
- [Kompositionseffekte](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design für UWP](/windows/apps/fluent-design-system)
- [Wissenschaft im System: Fluent Design und Tiefe](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Wissenschaft im System: Fluent Design und Licht](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
