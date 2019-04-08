---
description: Einblendungen sind neue Lichteffekte, welche die interaktiven Elemente in Ihrer App mit Tiefe und Fokus versehen kann.
title: Markierung anzeigen
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: aeba4dbd734ea4b521033726968e90c232c154cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628665"
---
# <a name="reveal-highlight"></a>Markierung anzeigen

![Favoritenbild](images/header-reveal-highlight.svg)

Zeigen Sie, dass die Hervorhebung eines Effekts Beleuchtung ist, das interaktive Elemente, z. B. Befehlsleisten, hervorgehoben, wenn der Benutzer den Zeiger in der Nähe sie. 

> **Wichtige APIs:** [RevealBrush Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [RevealBackgroundBrush Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [RevealBorderBrush Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [RevealBrushHelper Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [VisualState Klasse](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>Funktionsweise
Zeigen Sie Hervorhebung Aufrufe Aufmerksamkeit interaktiven Elementen von des Elements Container offenlegen, wenn der Zeiger in der Nähe, wie in dieser Abbildung gezeigt wird:

![Reveal Visual](images/Nav_Reveal_Animation.gif)

Da durch Einblendungen die ausgeblendeten Rahmen um Objekte herum angezeigt werden, entwickeln Benutzer ein besseres Verständnis von dem Raum, mit dem diese Objekte interagieren. Darüber hinaus erfahren sie dadurch, welche Aktionen verfügbar sind. Dies ist besonders bei Listensteuerelementen und Gruppen von Schaltflächen wichtig.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/Reveal">die App zu öffnen und Einblendungen in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Video-Zusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>Verwendung

„Reveal” funktioniert automatisch bei einigen Steuerelementen. Für andere Steuerelemente, Sie können anzeigen durch Aktivieren einer speziellen Format zuzuweisen, an das Steuerelement, wie in beschrieben die [aktivieren anzuzeigen, für andere Steuerelemente](#enabling-reveal-on-other-controls) und [aktivieren anzuzeigen, zu benutzerdefinierten Steuerelementen](#enabling-reveal-on-custom-controls) Abschnitten dieses Artikel.

## <a name="controls-that-automatically-use-reveal"></a>Steuerelemente, die „Reveal” automatisch verwenden

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

Diese Abbildungen zeigen markieren anzuzeigen, klicken Sie auf verschiedene Steuerelemente:

![Beispiele für „Reveal”](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>Aktivieren von „Reveal” für andere Steuerelemente

Für Szenarien, in denen Einblendungen angewendet werden sollten (diese Steuerelemente sind Hauptinhalt und/oder werden in einer Liste oder Auflistungsausrichtung verwendet), haben wir optionale Ressourcenformate bereitgestellt, mithilfe derer Sie Einblendungen für solche Situationen aktivieren können.

Diese Steuerelemente verfügen nicht standardmäßig über Einblendungen, da sie kleinere Steuerelemente sind, die in der Regel als Hilfssteuerelemente für die zentralen Punkte Ihrer Anwendung dienen. Alle Apps sind jedoch verschieden, und wenn diese Steuerelemente in Ihrer App am meisten verwendet werden, stehen Ihnen einige Formate als Hilfe zur Verfügung:

| Name des Steuerelements   | Ressourcenname |
|----------|:-------------:|
| Schaltfläche |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (Anzeigen über dem Inhalt) | GridViewItemRevealBackgroundShowsAboveContentStyle |

Um diese Formate anzuwenden, legen Sie die Eigenschaft [Style](/uwp/api/Windows.UI.Xaml.Style) folgendermaßen fest:

```xaml
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>Einblenden in Designs

Einblenden ändert sich je nach angefordertem Design des Steuerelements, der App oder der Einstellung des Benutzers. Im dunklen Design ist das Licht des Rahmens und der Anzeige weiß, währen in hellem Design nur der Rahmen hellgrau angezeigt wird.

![helles und dunkles Einblenden](images/Dark_vs_LightReveal.png)

Um weiße Rahmen in einem hellen Design anzuzeigen, setzen Sie einfach das angeforderte Design für das Steuerelement auf dunkel fest.

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

Oder ändern Sie das „TargetTheme” des „RevealBorderBrush” auf Dunkel. Beachten Sie Folgendes! Wenn „TargetTheme” auf dunkel festgelegt ist, wird Einblenden weiß angezeigt. Wenn es auf „Light” festgelegt ist, werden die Rahmen grau angezeigt.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>Aktivieren von „Reveal” für benutzerdefinierte Steuerelemente

Sie können „Reveal” für benutzerdefinierte Steuerelemente hinzufügen. Bevor Sie dies tun, ist es hilfreich, ein wenig mehr wissen über die Funktionsweise des anzeigen-Effekt. Anzeigen besteht aus zwei separaten Auswirkungen: **Anzeigen von Rahmen** und **Hover anzeigen**.

- **Rahmen** zeigt die Rahmen der interaktiven Elemente an, wenn sich ein Zeiger nähert. Dadurch können Objekte in der Nähe ähnliche Aktionen wie das aktuell fokussierte Objekt durchführen.
- Durch **Draufzeigen** wird die angedeutete oder fokussierte Form mit einem leichten Schein umgeben und beim Anklicken wird eine gedrückte Animation angezeigt. 

![Ebenen einblenden](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


Diese Effekte werden durch zwei Pinselelemente definiert: 
* Rahmen anzeigen wird definiert, indem **RevealBorderBrush**
* Zeigen Sie anzeigen wird definiert, indem **RevealBackgroundBrush**

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
In den meisten Fällen wird „Reveal” für bestimmte Steuerelemente von uns automatisch aktiviert. Andere Steuerelemente müssen jedoch über das Anwenden eines Stils oder das Ändern der Vorlagen direkt aktiviert.

### <a name="when-to-add-reveal"></a>Wann sollte „Reveal” hinzugefügt werden
Sie können „Reveal” auf Ihre benutzerdefinierten Steuerelemente hinzufügen – es empfiehlt allerdings, den Typ des Steuerelements und sein Verhalten vorher festzulegen. 
* Wenn Ihr benutzerdefiniertes Steuerelement ein interaktives Element ist und keine ähnlichen Steuerelemente auf der gleichen Oberfläche angezeigt werden (wie z. B. Menüelemente), benötigt das benutzerdefinierte Steuerelement wahrscheinlich keine Einblendung.  
* Besitzen Sie eine Gruppe von verwandten interaktiven Inhalten oder Elementen, benötigen die Bereiche der App wahrscheinlich die Einblendung – dies wird häufig als [Steuerung](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding) der Oberfläche bezeichnet.

Eine allein angezeigte Schaltfläche muss keine Einblendung verwenden, aber eine Reihe von Schaltflächen in einer Befehlsleiste sollte „Reveal” verwenden.

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>Hinzufügen von „Reveal” mithilfe der Steuerelementvorlage 
Um die Einblendung für benutzerdefinierte Steuerelemente oder Steuerelemente mit neuen Vorlagen zu aktivieren, ändern Sie die Steuerelementvorlage für das Steuerelement. Die meisten Steuerelementvorlagen besitzen ein Raster im Stammverzeichnis. Aktualisieren Sie [VisualState](/uwp/api/windows.ui.xaml.visualstate) des Stamm-Rasters, um die Einblendung zu verwenden:

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

Es ist wichtig zu beachten, dass zur Einblendung sowohl ein Pinsel als auch Setter in Visual State benötigt werden, um einwandfrei zu arbeiten. Das einfache Festlegen eines Steuerelements für Reveal-Pinselressourcen alleine aktivieren das Steuerelement nicht. Ziele oder Einstellungen zu verwenden ohne die Werte als Reveal-Pinsel festgelegt zu haben, aktiviert die Einblendung auch nicht.

Weitere Informationen zum Ändern von Steuerelementvorlagen finden Sie im Artikel [XAML-Steuerelementvorlagen](../controls-and-patterns/control-templates.md) Artikel.

Wir haben eine Reihe von System-Reveal-Pinsels erstellt, mit denen Sie die Vorlagen anpassen können. Sie können z. B. den Pinsel **ButtonRevealBackground** zum Erstellen eines Hintergrunds für eine benutzerdefinierte Schaltfläche oder den Pinsel **ListViewItemRevealBackground** für benutzerdefinierte Listen und so weiter verwenden. (Informationen zur Funktionsweise von Ressourcen in XAML finden Sie im Artikel [Xaml-Ressourcenverzeichnis](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).)

### <a name="full-template-example"></a>Vollständiges Vorlagenbeispiel

Hier sehen Sie eine gesamte Vorlage und wie eine Schaltfläche zum Einblenden aussehen würde:

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

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>Optimieren des Effekts von „Reveal” für ein benutzerdefiniertes Steuerelement 

Wenn Sie auf das benutzerdefinierte oder Re-Vorlagen-Steuerelement oder eine benutzerdefinierte Eingabeereignisse Oberfläche anzeigen aktivieren, können Sie die Auswirkungen zu optimieren diese Tipps helfen:
 
* Benachbarte Elemente mit Größen, die nicht in die Höhe oder Breite (insbesondere in Listen) aneinander ausgerichtet: Entfernen Sie das Verhalten des Rahmens Ansatz, und halten Sie die Rahmen, wenn darauf gezeigt wird nur angezeigt.
* Eingabeereignisse Elemente finden Sie unter, die häufig ein-und den deaktivierten Zustand: Ort der Rahmenpinsel Ansatz, der Elemente Backplates als auch der Rahmen um den Zustand zu bezeichnen.
* Tippen Sie sie an für angrenzende sind Elemente, die so dicht beieinander liegen: Fügen Sie eine von 1px-Rand zwischen den beiden Elementen hinzu. 

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen
### <a name="do"></a>Führen Sie aus:
- Verwenden Sie „Reveal” für Elemente, in denen der Benutzer viele Aktionen (CommandBars, Navigationsmenüs) ausführt
- Verwenden Sie „Reveal” bei der Gruppierung von interaktiven Elementen, die nicht standardmäßig visuelle Trennzeichen haben (Listen, Menübänder)
- Verwenden Sie „Reveal” in Bereichen mit vielen interaktiven Elementen (Befehlszenarios)
- Fügen Sie einen Rand von einem Pixel zwischen Einblendungselementen hinzu

### <a name="dont"></a>Nicht empfohlen
- Verwenden Sie „Reveal” nicht auf statischen Inhalten (Hintergrund, Text)
- Verwenden Sie „Reveal” nicht auf Popups, Flyouts oder Dropdownlisten
- Verwenden Sie „Reveal” nicht in einzelnen, isolierten Situationen
- Verwenden Sie Einblendungen nicht auf sehr großen Elementen (größer als 500epx)
- Verwenden Sie „Reveal” nicht bei sicherheitsbezogenen Entscheidungen, da es die Aufmerksamkeit von der Nachricht, die Sie an die Benutzer übermitteln möchten, weglenken kann.


## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="reveal-and-the-fluent-design-system"></a>„Reveal” und das Fluent Design-System

 Mit dem Fluent Design-System erstellen Sie moderne Oberflächen, die Licht, Tiefe, Bewegung, Material und Skalierungsmöglichkeiten beinhalten. „Einblendungen” ist eine Komponente des Fluent Design-Systems, die Lichteffekte in Ihrer App ermöglicht. Weitere Informationen finden Sie in der [Fluent Design für UWP-Übersicht](../fluent-design-system/index.md).

## <a name="related-articles"></a>Verwandte Artikel

- [RevealBrush-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Blog](acrylic.md)
- [Auswirkungen der Komposition](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design für UWP](../fluent-design-system/index.md)
- [Die Wissenschaft im System: Fluent Design und der Tiefe](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Die Wissenschaft im System: Fluent Design und Licht](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
