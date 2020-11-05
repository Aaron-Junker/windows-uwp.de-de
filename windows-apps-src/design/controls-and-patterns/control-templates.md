---
description: Durch Erstellen einer Steuerelementvorlage im XAML-Framework können Sie die visuelle Struktur und das visuelle Verhalten eines Steuerelements anpassen.
MS-HAID: dev\_ctrl\_layout\_txt.control\_templates
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Steuerelementvorlagen
ms.assetid: 6E642626-A1D6-482F-9F7E-DBBA7A071DAD
label: Control templates
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 720104c9dc27e258307b307a466477a5ca14f3e3
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033424"
---
# <a name="control-templates"></a>Steuerelementvorlagen

Durch Erstellen einer Steuerelementvorlage im XAML-Framework können Sie die visuelle Struktur und das visuelle Verhalten eines Steuerelements anpassen. Steuerelemente besitzen zahlreiche Eigenschaften wie [**Background**](/uwp/api/windows.ui.xaml.controls.control.background), [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) und [**FontFamily**](/uwp/api/windows.ui.xaml.controls.control.fontfamily). Damit können Sie verschiedene Aspekte der Steuerelementdarstellung angeben. Die Anpassungen, die Sie mit diesen Eigenschaften vornehmen können, sind jedoch begrenzt. Weitere Anpassungen können Sie durch Erstellen einer Vorlage mit der [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Klasse angeben. Hier erfahren Sie, wie Sie eine **ControlTemplate** erstellen, um das Erscheinungsbild eines [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Steuerelements anzupassen.

> **Wichtige APIs:** [**ControlTemplate-Klasse**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate), [**Control.Template-Eigenschaft**](/uwp/api/windows.ui.xaml.controls.control.template)

## <a name="custom-control-template-example"></a>Beispiel für eine benutzerdefinierte Steuerelementvorlage

Standardmäßig wird der Inhalt eines [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Steuerelements (also die Zeichenfolge oder das Objekt neben dem **CheckBox** -Element) rechts neben dem Auswahlfeld platziert. Ein Häkchen gibt an, dass das **CheckBox** -Element von einem Benutzer aktiviert wurde. Diese Merkmale stellen die visuelle Struktur und das visuelle Verhalten des **CheckBox** -Elements dar.

Hier sehen Sie ein [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Element mit standardmäßiger [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Klasse und den Zuständen `Unchecked`, `Checked` und `Indeterminate`.

![Standardvorlage für CheckBox-Steuerelemente](images/templates-checkbox-states-default.png)

Sie können diese Merkmale ändern, indem Sie eine [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) für das [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Element erstellen. Ein Beispiel: Sie möchten den Inhalt des Kontrollkästchens unter dem Auswahlfeld platzieren und durch ein **X** angeben, dass das Kontrollkästchen vom Benutzer aktiviert wurde. Diese Merkmale geben Sie dann in der **ControlTemplate** -Klasse des **CheckBox** -Elements an.

Wenn Sie eine benutzerdefinierte Vorlage für ein Steuerelement verwenden möchten, weisen Sie die [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) der [**Template**](/uwp/api/windows.ui.xaml.controls.control.template)-Eigenschaft des Steuerelements zu. Hier sehen Sie ein [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Element mit der **ControlTemplate** -Klasse `CheckBoxTemplate1`. Den XAML (Extensible Application Markup Language)-Code für die **ControlTemplate** zeigen wir im nächsten Abschnitt.

```XAML
<CheckBox Content="CheckBox" Template="{StaticResource CheckBoxTemplate1}" IsThreeState="True" Margin="20"/>
```

So sieht dieses [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Element in den Zuständen `Unchecked`, `Checked`, and `Indeterminate` aus, nachdem die Vorlage angewendet wurde.

![Benutzerdefinierte Vorlage für CheckBox-Steuerelemente](images/templates-checkbox-states.png)

## <a name="specify-the-visual-structure-of-a-control"></a>Angeben der visuellen Struktur von Steuerelementen

Wenn Sie eine [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Klasse erstellen, kombinieren Sie [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement)-Objekte, um ein einzelnes Steuerelement zu erstellen. Eine **ControlTemplate** -Klasse kann nur ein **FrameworkElement** als Stammelement besitzen. Das Stammelement enthält in der Regel weitere **FrameworkElement** -Objekte. Die Kombination aus Objekten ergibt die visuelle Struktur des Steuerelements.

Dieser XAML-Code erstellt eine [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Klasse für ein [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Element, die festlegt, dass sich das Steuerelement unter dem Auswahlfeld befinden soll. Das Stammelement ist eine [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border)-Klasse. Das Beispiel gibt einen [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)-Wert an, um das **X** zu erstellen, mit darauf hinweist, dass ein Benutzer das **CheckBox** -Element aktiviert hat. Eine [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)-Klasse kennzeichnet einen unbestimmten Zustand. Beachten Sie, dass [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) für **Path** und **Ellipse** auf 0 festgelegt ist, sodass beide standardmäßig nicht angezeigt werden.

[TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) stellt eine spezielle Bindung dar, die den Wert einer Eigenschaft in einer Steuerelementvorlage mit dem Wert einer anderen Eigenschaft verknüpft, die im Steuerelement mit Vorlagen verfügbar gemacht wird. TemplateBinding kann nur in einer ControlTemplate-Definition in XAML verwendet werden. Weitere Informationen finden Sie unter [TemplateBinding-Markuperweiterung](../../xaml-platform/templatebinding-markup-extension.md).

> [!NOTE]
> Ab Windows 10, Version 1809 ( [SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) können Sie [**x:Bind**](../../xaml-platform/x-bind-markup-extension.md)-Markuperweiterungen an Stellen verwenden, an denen Sie [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) verwenden. Weitere Informationen finden Sie unter [TemplateBinding-Markuperweiterung](../../xaml-platform/templatebinding-markup-extension.md).

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

## <a name="specify-the-visual-behavior-of-a-control"></a>Angeben des visuellen Verhaltens von Steuerelementen

Das visuelle Verhalten gibt das Erscheinungsbild eines Steuerelements in verschiedenen Zuständen an. Das [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)-Steuerelement besitzt 3 Aktivierungszustände: `Checked`, `Unchecked` und `Indeterminate`. Der Wert der [**IsChecked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)-Eigenschaft bestimmt den Zustand des **CheckBox** , der wiederum festlegt, was im Feld angezeigt wird.

Diese Tabelle enthält die möglichen Werte von [**IsChecked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked), die zugehörigen Zustände von [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) und das Erscheinungsbild des **CheckBox** -Steuerelements.

|                     |                    |                         |
|---------------------|--------------------|-------------------------|
| **IsChecked** -Wert | **CheckBox** -Zustand | **CheckBox** -Darstellung |
| **true**            | `Checked`          | Enthält ein „X“.        |
| **false**           | `Unchecked`        | Leer.                  |
| **Null**            | `Indeterminate`    | Enthält einen Kreis.      |


Das Erscheinungsbild eines Steuerelements in verschiedenen Zuständen wird mithilfe von [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState)-Objekten angegeben. Eine **VisualState** -Klasse enthält eine [**Setter**](/uwp/api/Windows.UI.Xaml.Setter)- oder [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.BeginStoryboard)-Eigenschaft, mit der die Darstellung der Elementen in der [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Klasse geändert wird. Wenn das Steuerelement in den Zustand übergeht, den die [**VisualState.Name**](/uwp/api/windows.ui.xaml.visualstate.name) angibt, werden die Änderungen in der **Setter** - oder [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)-Eigenschaft angewendet. Verlässt das Steuerelement den Zustand wieder, werden die Änderungen entfernt. **VisualState** -Objekte werden [**VisualStateGroup**](/uwp/api/Windows.UI.Xaml.VisualStateGroup)-Objekten hinzugefügt. **VisualStateGroup** -Objekte werden der angefügten [**VisualStateManager.VisualStateGroups**](/dotnet/api/system.windows.visualstatemanager)-Eigenschaft hinzugefügt. Diesel legen Sie im [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement)-Stammelement der **ControlTemplate** -Klasse fest.

Dieser XAML-Code zeigt die [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState)-Objekte für die Zustände `Checked`, `Unchecked` und `Indeterminate`. Das Beispiel legt die angefügte [**VisualStateManager.VisualStateGroups**](/dotnet/api/system.windows.visualstatemanager)-Eigenschaft in der [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border)-Klasse fest. Dies ist das Stammelement der [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Klasse. Der Zustand `Checked` der **VisualState** -Eigenschaft gibt an, dass die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft des [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)-Werts namens `CheckGlyph` (der im vorherigen Beispiel gezeigt wurde) gleich 1 ist. Der Zustand `Indeterminate` der **VisualState** -Eigenschaft gibt an, dass die **Opacity** -Eigenschaft des [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)-Werts namens `IndeterminateGlyph` gleich 1 ist. Da der Zustand `Unchecked` der **VisualState** -Eigenschaft keine [**Setter**](/uwp/api/Windows.UI.Xaml.Setter)- oder [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)-Eigenschaft besitzt, wird [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) auf die Standarddarstellung zurückgesetzt.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CheckStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Target="CheckGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="CheckGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
                <VisualState x:Name="Unchecked"/>
                <VisualState x:Name="Indeterminate">
                    <VisualState.Setters>
                        <Setter Target="IndeterminateGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="IndeterminateGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

Zum besseren Verständnis der Funktionsweise von [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState)-Objekten sollten Sie berücksichtigen, was passiert, wenn das [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) vom `Unchecked`-Zustand in den `Checked`-Zustand, dann in den `Indeterminate`-Zustand und anschließend zurück in den `Unchecked`-Zustand wechselt. Hier sehen Sie die Übergänge.

| Zustandsübergang | Folgendes passiert: | CheckBox-Darstellung nach Abschluss des Übergangs |
| ---------------- | ------------ | ------------------------------------------------- |
| Von `Unchecked` in `Checked`.       | Der [**Setter**](/uwp/api/Windows.UI.Xaml.Setter)-Wert des `Checked`- [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) wird angewendet. Daher hat die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft von `CheckGlyph` den Wert 1.                                                                                                                                                          | Es wird ein „X“ angezeigt.                                |
| Von `Checked` in `Indeterminate`.   | Der [**Setter**](/uwp/api/Windows.UI.Xaml.Setter)-Wert des `Indeterminate`- [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) wird angewendet. Daher hat die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft von `IndeterminateGlyph` den Wert 1. Der **Setter** -Wert von `Checked`- **VisualState** wird entfernt. Daher hat die [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity)-Eigenschaft von `CheckGlyph` den Wert 0. | Ein Kreis wird angezeigt.                            |
| Von `Indeterminate` in `Unchecked`. | Der [**Setter**](/uwp/api/Windows.UI.Xaml.Setter)-Wert von `Indeterminate`- [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) wird entfernt. Daher hat die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft von `IndeterminateGlyph` den Wert 0.                                                                                                                                           | Es wird nichts angezeigt.                             |

 
Weitere Informationen zum Erstellen visueller Zustände für Steuerelemente und insbesondere zum Verwenden der [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)-Klasse und der Animationstypen finden Sie unter [Storyboardanimationen für visuelle Zustände](/previous-versions/windows/apps/jj819808(v=win.10)).

## <a name="use-tools-to-work-with-themes-easily"></a>Verwenden Sie für die Arbeit mit Designs Tools.

Wenn Sie schnell Designs auf Ihre Steuerelemente anwenden möchten, klicken Sie in der Microsoft Visual Studio- **Dokumentgliederung** mit der rechten Maustaste auf ein Steuerelement und wählen **Design bearbeiten** oder **Stil bearbeiten** aus (je nach Steuerelement). Anschließend können Sie ein vorhandenes Design anwenden, indem Sie **Ressource übernehmen** auswählen, oder ein neues erstellen, indem Sie **Create Empty** auswählen.

## <a name="controls-and-accessibility"></a>Steuerelemente und Barrierefreiheit

Beim Erstellen einer neuen Vorlage für ein Steuerelement können Sie zusätzlich zur eventuellen Änderung des Verhaltens und der visuellen Darstellung des Steuerelements auch die Art und Weise ändern, wie sich das Steuerelement selbst für Barrierefreiheits-Frameworks repräsentiert. Die Windows-App unterstützt das Benutzeroberflächenautomatisierungs-Framework von Microsoft für die Barrierefreiheit. Alle Standardsteuerelemente und zugehörigen Vorlagen bieten Unterstützung für allgemeine Steuerelementtypen zur Benutzeroberflächenautomatisierung und Muster, die für den Zweck und die Funktion des Steuerelements geeignet sind. Diese Steuerelementtypen und Muster werden von Benutzeroberflächenautomatisierungs-Clients interpretiert, beispielsweise von Hilfstechnologien, und dies ermöglicht die Zugriffsmöglichkeit auf ein Steuerelement als Bestandteil einer barrierefreien größeren App-UI.

Um die grundlegende Steuerlogik zu trennen und auch einige architekturbezogene Anforderungen der Benutzeroberflächenautomatisierung zu erfüllen, ist die Unterstützung der Barrierefreiheit von Steuerelementklassen in einer separaten Klasse in Form eines Automatisierungspeers enthalten. Die Automatisierungspeers interagieren mitunter mit den Steuerelementvorlagen, weil die Peers von der Existenz bestimmter benannter Teile in den Vorlagen ausgehen, sodass Funktionen wie die Aktivierung von Hilfstechnologien zum Aufrufen von Schaltflächenaktionen möglich sind.

Beim Erstellen eines vollständig neuen, benutzerdefinierten Steuerelements sollten Sie mitunter auch einen neuen Automatisierungspeer für das Steuerelement erstellen. Weitere Informationen finden Sie unter [Benutzerdefinierte Automatisierungspeers](../accessibility/custom-automation-peers.md).

## <a name="learn-more-about-a-controls-default-template"></a>Weitere Informationen zur Standardvorlage eines Steuerelements

Die Themen, in denen die Stile und Vorlagen für XAML-Steuerelemente dokumentiert werden, enthalten Auszüge des XAML-Anfangscodes, der genutzt wird, wenn Sie die bereits erläuterten Verfahren zum **Bearbeiten von Designs** oder zum **Bearbeiten von Stilen** verwenden. In jedem Thema sind die Namen der Ansichtszustände, die Designressourcen und der vollständige XAML-Code für den Stil aufgeführt, der die Vorlage enthält. Die Themen können eine nützliche Hilfe darstellen, wenn Sie bereits mit der Änderung einer Vorlage begonnen haben und sehen möchten, wie die Vorlage im Originalzustand ausgesehen hat. Außerdem können Sie sicherstellen, dass die neue Vorlage über alle erforderlichen benannten Ansichtszustände verfügt.

## <a name="theme-resources-in-control-templates"></a>Designressourcen in Steuerelementvorlagen

Möglicherweise sind Ihnen bei einigen Attributen in den XAML-Beispielen Ressourcenverweise aufgefallen, für die die [{ThemeResource}-Markuperweiterung](../../xaml-platform/themeresource-markup-extension.md) verwendet wird. Mit diesem Verfahren kann eine einzelne Steuerelementvorlage Ressourcen nutzen, bei denen es sich um unterschiedliche Werte handeln kann. Dies hängt davon ab, welches Design gerade aktiv ist. Besonders wichtig ist dies für Pinsel und Farben, da der Hauptzweck der Designs darin besteht, den Benutzern die Auswahl eines dunklen Designs, hellen Designs oder Designs mit hohem Kontrast zu ermöglichen, das auf das gesamte System angewendet wird. Apps, für die das XAML-Ressourcensystem verwendet wird, können einen für das jeweilige Design geeigneten Ressourcensatz nutzen. Die Designauswahl in der UI einer App spiegelt dann die systemweite Designauswahl des Benutzers wider.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

* [Beispiel für XAML-Steuerelementekatalog](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)
* [Beispiel für ein benutzerdefiniertes Textbearbeitungssteuerelement](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)
