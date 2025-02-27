---
description: Erfahren Sie, wie Sie das flexible XAML-Layoutsystem verwenden, mit automatischer Größenanpassung, Layoutbereichen, visuellen Zuständen und getrennten UI-Definitionen verwenden, um eine reaktionsfähige Benutzeroberfläche zu erstellen.
title: Dynamische Layouts mit XAML
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: contperf-fy21q2
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: e2bc29acc63a5363891ad78b873db6c8311ac970
ms.sourcegitcommit: 06d59b59a95aad009acb947a0dac7432116bdb60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100544650"
---
# <a name="responsive-layouts-with-xaml"></a>Dynamische Layouts mit XAML

Das XAML-Layoutsystem bietet automatische Größenanpassung von Elementen, Layoutpanels und visuelle Zustände, um eine reaktionsfähige Benutzeroberfläche zu erstellen. Mit einem dynamischen Layout können Sie für Ihre App ein großartiges Erscheinungsbild auf Bildschirmen mit unterschiedlichen Größen, Auflösungen, Pixeldichten und Ausrichtungen von App-Fenstern erstellen. Sie können XAML auch verwenden, um die Benutzeroberfläche Ihrer Anwendung neu zu positionieren, die Größe zu ändern, neu zu ordnen, anzuzeigen/auszublenden, zu ersetzen oder neu zu strukturieren, wie in [Reaktionsfähige Design-Techniken](responsive-design.md) beschrieben. Hier wird besprochen, wie man dynamische Layouts mit XAML realisiert.

## <a name="fluid-layouts-with-properties-and-panels"></a>Fluid-Layouts mit Eigenschaften und Panels

Die Grundlage für ein dynamisches Layout ist die richtige Verwendung von XAML-Layouteigenschaften und -panels, um die Position und Größe von Inhalten zu ändern und Inhalte neu anzuordnen.

Das XAML-Layoutsystem unterstützt sowohl statische als auch dynamische Layouts. In einem statischen Layout weisen Sie Steuerelementen feste Größen in Pixel und Positionen zu. Wenn der Benutzer die Auflösung oder Ausrichtung seines Geräts ändert, bleibt die UI unverändert. Statische Layouts können auf bestimmte Formfaktoren und Anzeigegrößen beschnitten werden. Dynamische Layouts passen sich andererseits durch Vergrößerung, Verkleinerung und Umbrüche an den auf einem Gerät verfügbaren Platz für die Anzeige an.

In der Praxis verwenden Sie eine Kombination aus statischen und dynamischen Elemente zum Erstellen der Benutzeroberfläche. Statische Elemente und Werte kommen an bestimmten Stellen immer noch vor, es sollte jedoch darauf geachtet werden, dass die Gesamt-UI reaktionsfähig ist und sich an verschiedene Auflösungen, Bildschirmgrößen und Ansichten anpasst.

Hier erörtern wir die Verwendung von XAML-Eigenschaften und Layoutpanels zur Erstellung eines Fluid-Layouts für Ihre App.

### <a name="layout-properties"></a>Layouteigenschaften
Zum Bestimmen der Größe und Position eines Elements legen Sie seine Layouteigenschaften fest. Um ein dynamisches Layout zu erstellen, verwenden Sie die automatische oder proportionale Größenanpassung für Elemente, und lassen Sie die Layoutpanels die Position für ihre untergeordneten Elemente wie erforderlich positionieren.

Hier sind einige allgemeine Layout-Eigenschaften und wie man sie zur Erstellung von Fluid-Layouts verwendet.

**Höhe und Breite**

Die Eigenschaften [**Height**](/uwp/api/windows.ui.xaml.frameworkelement.height) und [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.width) geben die Größe eines Elements an. Sie können feste Werte verwenden, die in effektiven Pixeln gemessen werden, oder Sie können eine automatische oder proportionale Größenanpassung verwenden.

Verwenden Sie die automatische Größenanpassung, damit die Größe von UI-Elementen entsprechend ihren Inhalten oder der Größe des übergeordneten Containers geändert wird. Sie können die automatische Größenanpassung auch für die Zeilen und Spalten eines Rasters verwenden. Um die automatische Größenanpassung zu verwenden, legen Sie für „Height“ und/oder „Width“ von UI-Elementen **Auto** fest.

> [!NOTE]
> Ob die Größe eines Elements an seinen Inhalt oder seinen Container angepasst wird, ist abhängig davon, wie der übergeordnete Container die Größenanpassung seiner untergeordneten Elemente behandelt. Weitere Informationen finden Sie unter [Layoutpanels](#layout-panels) weiter unten in diesem Artikel.

Die proportionale Größenanpassung, die auch als *Größenanpassung mit Sternvariable* bezeichnet wird, wird zum gleichmäßigen Aufteilen des verfügbaren Platzes auf die Zeilen und Spalten eines Rasters verwendet. In XAML werden Sternwerte als \* ausgedrückt (bzw. *n*\* für gleichmäßige Größenanpassung mit Sternvariable). Möchten Sie also z. B. angeben, dass eine Spalte fünfmal breiter als die zweite Spalte eines zweispaltigen Layouts ist, verwenden Sie "5\*" und "\*" für die [**Width**](/uwp/api/windows.ui.xaml.controls.columndefinition.width)-Eigenschaften der [**ColumnDefinition**](/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition)-Elemente.

In diesem Beispiel wird die feste, automatische und proportionale Größenanpassung in einem [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) mit 4 Spalten kombiniert.

| Spalte | Festlegen der Größe | BESCHREIBUNG |
| ------ | ------ | ----------- |
Column_1 | **Auto** | Die Breite der Spalte wird entsprechend ihres Inhalts angepasst.
Column_2 | * | Nach dem Berechnen der Spalten mit automatischer Breite erhält diese Spalte einen Teil der verbleibenden Breite. Column_2 ist nur halb so breit wie Column_4.
Column_3 | **44** | Die Spalte ist 44 Pixel breit.
Column_4 | **2**\* | Nach dem Berechnen der Spalten mit automatischer Breite erhält diese Spalte einen Teil der verbleibenden Breite. Column_4 ist doppelt so breit wie Column_2.

Die standardmäßige Spaltenbreite beträgt "*", daher Sie müssen diesen Wert für die zweite Spalte nicht explizit festlegen.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its content." FontSize="24"/>
</Grid>
```

Im Visual Studio-XAML-Designer sieht das Ergebnis wie folgt aus.

![Ein Raster mit vier Spalten im Visual Studio-Designer](images/xaml-layout-grid-in-designer.png)

Verwenden Sie zum Abrufen der Größe eines Elements zur Laufzeit die schreibgeschützten Eigenschaften [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) und [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) anstelle von „Height“ und „Width“.

**Größenbeschränkungen**

Wenn Sie die automatische Größenanpassung in Ihrer Benutzeroberfläche verwenden, müssen Sie möglicherweise dennoch Einschränkungen für die Größe eines Elements festlegen. Sie können die Eigenschaften [**MinWidth**](/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) und [**MinHeight**](/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](/uwp/api/windows.ui.xaml.frameworkelement.maxheight) festlegen, um Werte anzugeben, die die Größe eines Elements begrenzen, während weiterhin eine dynamische Größenanpassung möglich ist.

In einem „Grid“ kann „MinWidth/MaxWidth“ auch mit Spaltendefinitionen verwendet werden, und „MinHeight/MaxHeight“ kann mit Zeilendefinitionen verwendet werden.

**Ausrichtung**

Verwenden Sie die Eigenschaften [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) und [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment), um anzugeben, wie ein Element innerhalb seines übergeordneten Containers positioniert werden soll.
- Die Werte für **HorizontalAlignment** sind **Left**, **Center**, **Right** und **Stretch**.
- Die Werte für **VerticalAlignment** sind **Top**, **Center**, **Bottom** und **Stretch**.

Bei der **Stretch**-Ausrichtung füllen die Elemente den Bereich, der für sie im übergeordneten Container bereitgestellt wird. „Stretch“ ist der Standardwert für beide Ausrichtungseigenschaften. Einige Steuerelemente, z. B. [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) überschreiben diesen Wert jedoch in ihrem Standardstil.
Alle Elemente, die untergeordnete Elemente haben können, können den Wert „Stretch“ für die Eigenschaften „HorizontalAlignment“ und „VerticalAlignment“ eindeutig behandeln. Angenommen, ein Element, das die standardmäßigen „Stretch“-Werte verwendet und in einem „Grid“ platziert ist, wird so gestreckt, dass es die Zelle füllt, die es enthält. Dasselbe Element in einem Canvas-Panel wird an die Größe seines Inhalts angepasst. Weitere Informationen dazu, wie die einzelnen Panels den „Stretch“-Wert behandeln, finden Sie unter im Artikel [Layoutpanels](layout-panels.md).

Weitere Informationen finden Sie im Artikel [Ausrichtung, Rand und Abstand](alignment-margin-padding.md) und auf den Referenzseiten [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) und [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment).

**Sichtbarkeit**

Sie können ein Element ein- oder ausblenden, indem Sie seine [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility)-Eigenschaft auf einen der [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility)-Enumerationswerte festlegen: **Visible** oder **Collapsed**. Wenn ein Element „Collapsed“ ist, verbraucht es keinen Platz im Benutzeroberflächenlayout.

Sie können die Visibility-Eigenschaft eines Elements im Code oder in einem visuellen Zustand ändern. Wenn die Visibility eines Elements geändert wird, werden auch alle untergeordneten Elemente geändert. Sie können Abschnitte der Benutzeroberfläche ersetzen, indem Sie ein Panel einblenden, während Sie ein anderes reduzieren.

> [!Tip]
> Wenn die Benutzeroberfläche Elemente enthält, die standardmäßig **Collapsed** sind, werden die Objekte beim Start dennoch erstellt, obwohl sie nicht sichtbar sind. Sie können das Laden dieser Elemente bis zu ihrer Anzeige verzögern, indem Sie das **x:DeferLoadStrategy attribute** auf „Lazy“ festlegen. Dies kann die Leistung beim Starten verbessern. Weitere Informationen finden Sie unter [x: DeferLoadStrategy-Attribut](../../xaml-platform/x-deferloadstrategy-attribute.md).

### <a name="style-resources"></a>Stilressourcen

Sie müssen nicht jeden Eigenschaftswert einzeln für ein Steuerelement festlegen. Es ist in der Regel effizienter, Eigenschaftswerte in einer [**Style**](/uwp/api/Windows.UI.Xaml.Style)-Ressource zu gruppieren und den Stil auf ein Steuerelement anzuwenden. Dies gilt insbesondere dann, wenn Sie die gleichen Eigenschaftswerte auf viele Steuerelemente anwenden müssen. Weitere Informationen zum Verwenden von Stilen finden Sie unter [Formatieren von Steuerelementen](../controls-and-patterns/xaml-styles.md).

### <a name="layout-panels"></a>Layoutpanels

Sie müssen visuelle Objekte in einem Panel oder einem anderen Containerobjekt platzieren, um sie zu positionieren. Das XAML-Framework bietet verschiedene Panel-Klassen, z. B. [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas), [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) und [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel), die als Container dienen und Ihnen das Platzieren und Anordnen der darin enthaltenen UI-Elemente ermöglichen.

Die Hauptaspekte, die es bei der Wahl eines Layoutpanels zu berücksichtigen gilt, sind die Positionierung und Anpassung der Größe von untergeordneten Elementen durch das Panel. Gegebenenfalls müssen Sie auch die Schichtung untergeordneter Elemente berücksichtigen, die sich überschneiden.

Es folgt ein Vergleich der Hauptfunktionen der Panelsteuerelemente, die im XAML-Framework bereitgestellt werden.

Panelsteuerelement | Beschreibung
--------------|------------
[**Zeichenbereich**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) | **Canvas** unterstützt keine dynamische Benutzeroberfläche; Sie steuern alle Aspekte der Positionierung und der Anpassung der Größe von untergeordneten Elementen. Sie verwenden dieses Panel in der Regel für spezielle Fälle wie das Erstellen von Grafiken oder das Definieren kleiner statischer Bereiche einer größeren adaptiven Benutzeroberfläche. Sie können Code oder visuelle Zustände verwenden, um Elemente zur Laufzeit neu zu positionieren.<li>Die Elemente werden mithilfe der angefügten Eigenschaften Canvas.Top und Canvas.Left absolut positioniert.</li><li>Die Schichtung kann mithilfe der angefügten Eigenschaft Canvas.ZIndex explizit angegeben werden.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden ignoriert. Wenn die Größe des Elements nicht explizit festgelegt wird, wird die Größe an den Inhalt angepasst.</li><li>Untergeordnete Inhalte werden nicht visuell abgeschnitten, wenn sie größer sind als das Panel. </li><li>Untergeordnete Inhalte werden nicht durch die Grenzen des Panels beschränkt.</li>
[**Raster**](/uwp/api/Windows.UI.Xaml.Controls.Grid) | **Grid** unterstützt das dynamische Ändern der Größe von untergeordneten Elementen. Sie können Code oder visuelle Zustände verwenden, um die Position von Elementen zu ändern und sie dynamisch zu umbrechen.<li>Mithilfe der angefügten Eigenschaften Grid.Row und Grid.Column können Elemente in Zeilen und Spalten angeordnet werden.</li><li>Mithilfe der angefügten Eigenschaften „Grid.RowSpan“ und „Grid.ColumnSpan“ können sich die Elemente über mehrere Spalten und Zeilen erstrecken.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden berücksichtigt. Wird die Größe eines Elements nicht explizit festgelegt, wird das Element gestreckt, sodass es den zur Verfügung stehenden Platz in der Rasterzelle ausfüllt.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe von Inhalten wird durch die Grenzen des Panels beschränkt, für bildlauffähige Inhalte werden daher bei Bedarf Bildlaufleisten angezeigt.</li>
[**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | <li>Elemente werden in Bezug auf den Rand oder die Mitte des Panels und im Verhältnis zueinander angeordnet.</li><li>Elemente werden unter Verwendung verschiedener angefügter Eigenschaften positioniert, die die Ausrichtung des Panels sowie von gleichgeordneten Elementen und die Position gleichgeordneter Elemente steuern. </li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden ignoriert, es sei denn, die angefügten RelativePanel-Eigenschaften für die Ausrichtung verursachen eine Streckung (ein Element wird zum Beispiel sowohl am rechten als auch am linken Rand des Panels ausgerichtet). Wenn die Größe eines Elements nicht explizit festgelegt und es nicht gestreckt wird, wird die Größe an den Inhalt angepasst.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe von Inhalten wird durch die Grenzen des Panels beschränkt, für bildlauffähige Inhalte werden daher bei Bedarf Bildlaufleisten angezeigt.</li>
[**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) |<li>Die Elemente werden in einer Linie gestapelt – entweder vertikal oder horizontal.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden in der entgegengesetzten Richtung der „Orientation“-Eigenschaft berücksichtigt. Wird die Größe eines Elements nicht explizit festgelegt, wird das Element gestreckt, sodass es die zur Verfügung stehende Breite (oder Höhe, falls die Ausrichtung auf „Horizontal“ festgelegt ist) ausfüllt. In der von der „Orientation“-Eigenschaft angegebenen Richtung wird ein Element an seinen Inhalt angepasst.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe des Inhalts wird nicht durch die Grenzen des Panels in der von der „Orientation“-Eigenschaft angegebenen Richtung beschränkt; bildlauffähige Inhalte werden daher über die Panelgrenzen hinaus gestreckt und weisen keine Bildlaufleisten auf. Sie müssen die Höhe (oder Breite) des untergeordneten Inhalts explizit auf die Bildlaufleisten beschränken, damit die Bildlaufleisten angezeigt werden.</li>
[**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) |<li>Die Elemente werden in Zeilen oder Spalten angeordnet, die automatisch auf eine neue Zeile oder Spalte umgebrochen werden, wenn der „MaximumRowsOrColumns“-Wert erreicht ist.</li><li>Ob die Elemente in Zeilen oder Spalten angeordnet werden, hängt von der „Orientation“-Eigenschaft ab.</li><li>Mithilfe der angefügten Eigenschaften „VariableSizedWrapGrid.RowSpan“ und „VariableSizedWrapGrid.ColumnSpan“ können sich die Elemente über mehrere Spalten und Zeilen erstrecken.</li><li>Streckungswerte für „HorizontalAlignment“/„VerticalAlignment“ werden ignoriert. Die Größe der Elemente wird gemäß den Eigenschaften „ItemHeight“ und „ItemWidth“ angepasst. Wenn diese Eigenschaften nicht festgelegt sind, übernehmen sie ihre Werte von der Größe der ersten Zelle.</li><li>Untergeordnete Inhalte werden visuell abgeschnitten, wenn sie größer sind als das Panel.</li><li>Die Größe von Inhalten wird durch die Grenzen des Panels beschränkt, für bildlauffähige Inhalte werden daher bei Bedarf Bildlaufleisten angezeigt.</li>

Ausführliche Informationen und Beispiele für diese Panels finden Sie unter [Layoutpanels](layout-panels.md). Weitere Informationen finden Sie auch im [Beispiel für reaktionsfähige Designtechniken](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques).

Mit Layoutpanels können Sie die Benutzeroberfläche als logische Steuerelementgruppen strukturieren. Wenn Sie diese mit den entsprechenden Eigenschaftseinstellungen verwenden, erhalten Sie Unterstützung für die automatische Größenanpassung, die Neupositionierung und die Neuformatierung von UI-Elementen. Die meisten Benutzeroberflächenlayouts müssen jedoch weiter bearbeitet werden, wenn erhebliche Änderungen an der Fenstergröße vorgenommen werden. Hierzu können Sie visuelle Zustände verwenden.

## <a name="adaptive-layouts-with-visual-states-and-state-triggers"></a>Adaptive Layouts mit visuellen Zuständen und Zustandsauslösern
Verwenden Sie visuelle Zustände, um wesentliche Änderungen an Ihrer UI basierend auf der Fenstergröße oder andere Änderungen vorzunehmen.

Wenn sich die Größe Ihres App-Fensters über einen bestimmten Punkt vergrößert oder verkleinert, können Sie Layouteigenschaften ändern, um die Größe und Position von Abschnitten Ihrer UI zu ändern oder diese neu anzuordnen, einzublenden oder zu ersetzen. Sie können verschiedene visuelle Zustände für Ihre Benutzeroberfläche definieren und diese anwenden, wenn die Fensterbreite oder Fensterhöhe einen bestimmten Schwellenwert überschreitet.

Ein [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) definiert Eigenschaftswerte, die auf ein Element angewendet werden, wenn es sich in einem bestimmten Zustand befindet. Gruppieren Sie visuelle Zustände in einem [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager), der den entsprechenden „VisualState“ anwendet, wenn die angegebenen Bedingungen erfüllt werden. Ein [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) bietet eine einfache Möglichkeit, um einen Schwellenwert (auch „Haltepunkt“ genannt) festzulegen, wenn ein Zustand in XAML angewendet wird. Oder Sie können die [**VisualStateManager.GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate)-Methode aufrufen, um einen visuellen Zustand aus Code anzuwenden. Beispiele für beide Möglichkeiten werden in den nächsten Abschnitten gezeigt.

### <a name="set-visual-states-in-code"></a>Festlegen von visuellen Zuständen im Code

Um einen visuellen Zustand aus Code anzuwenden, rufen Sie die [**VisualStateManager.GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate)-Methode auf. Um beispielsweise einen Zustand anzuwenden, wenn das App-Fenster eine bestimmte Größe erreicht, behandeln Sie das [**SizeChanged**](/uwp/api/windows.ui.xaml.window.sizechanged)-Ereignis, und rufen Sie **GoToState** auf, um den entsprechenden Zustand anzuwenden.

Hier enthält eine [**VisualStateGroup**](/uwp/api/Windows.UI.Xaml.VisualStateGroup) zwei VisualState-Definitionen. Die erste Definition, `DefaultState`, ist leer. Wenn sie angewendet wird, werden die in der XAML-Seite definierten Werte angewendet. Die zweite Definition, `WideState`, ändert die [**DisplayMode**](/uwp/api/windows.ui.xaml.controls.splitview.displaymode)-Eigenschaft der [**SplitView**](/uwp/api/Windows.UI.Xaml.Controls.SplitView) in **Inline** und öffnet den Bereich. Dieser Zustand wird im SizeChanged-Ereignis-Handler angewendet, wenn die Fensterbreite mehr als 640 effektive Pixel beträgt.

> [!NOTE]
> Windows bietet keine Möglichkeit für Ihre App zum Erkennen des Geräts, auf dem sie ausgeführt wird. Sie kann die Gerätefamilie des Geräts (mobil, Desktop usw.), auf dem sie ausgeführt wird, die effektive Auflösung und den für die App verfügbaren Bildschirmbereich (Größe des App-Fensters) erkennen. Wir empfehlen, visuelle Zustände für [Bildschirmgrößen und Breakpoints](screen-sizes-and-breakpoints-for-responsive-design.md) zu definieren.

```xaml
<Page ...
    SizeChanged="CurrentWindow_SizeChanged">
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width > 640)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

```cppwinrt
// YourPage.h
void CurrentWindow_SizeChanged(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::SizeChangedEventArgs const& e);

// YourPage.cpp
void YourPage::CurrentWindow_SizeChanged(IInspectable const& sender, SizeChangedEventArgs const& e)
{
    if (e.NewSize.Width > 640)
        VisualStateManager::GoToState(*this, "WideState", false);
    else
        VisualStateManager::GoToState(*this, "DefaultState", false);
}

```

### <a name="set-visual-states-in-xaml-markup"></a>Festlegen von visuellen Zuständen im XAML-Markup

Vor Windows 10 erforderten „VisualState“-Definitionen [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)-Objekte für Eigenschaftsänderungen, und Sie mussten **GoToState** im Code aufrufen, um den Zustand anzuwenden. Dies wird im vorherigen Beispiel veranschaulicht. Sie werden immer noch viele Beispiele finden, in denen diese Syntax verwendet wird, oder Sie verfügen möglicherweise über vorhandenen Code, der sie verwendet.

Ab Windows 10 können Sie die hier dargestellte vereinfachte [**Setter**](/uwp/api/Windows.UI.Xaml.Setter)-Syntax verwenden, und Sie können [**StateTrigger**](/uwp/api/Windows.UI.Xaml.StateTrigger)-Elemente im XAML-Markup verwenden, um den Zustand anzuwenden. Sie verwenden „StateTrigger“-Elemente, um einfache Regeln zu erstellen, die automatisch Änderungen des visuellen Zustands als Reaktion auf ein App-Ereignis auslösen.

In diesem Beispiel wird die gleiche Aufgabe wie im vorherigen Beispiel ausgeführt, es wird jedoch die vereinfachte **Setter**-Syntax anstelle eines Storyboard zum Definieren von Eigenschaftsänderungen verwendet. Anstatt „GoToState“ aufzurufen, wird der integrierte [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)-Zustandsauslöser verwendet, um den Zustand anzuwenden. Bei Verwendung von Zustandsauslösern müssen Sie keinen leeren `DefaultState` definieren. Die Standardeinstellungen werden automatisch erneut angewendet, wenn die Bedingungen des Zustandsauslösers nicht mehr erfüllt sind.

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=640 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="640" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> [!Important]
> Im vorherigen Beispiel wird die angefügte „VisualStateManager.VisualStateGroups“-Eigenschaft im **Grid**-Element festgelegt. Bei Verwendung von „StateTrigger“-Elementen müssen Sie immer sicherstellen, dass „VisualStateGroups“ an das erste untergeordnete Element des Stamms angefügt wird, damit die Auslöser automatisch wirksam werden. (Hier ist **Grid** das erste untergeordnete Element des Stammelements **Page**.)

### <a name="attached-property-syntax"></a>Syntax von angefügten Eigenschaften

In einem „VisualState“ wird in der Regel ein Wert für eine Steuerelementeigenschaft oder für eine der angefügten Eigenschaften des Panels festgelegt, das das Steuerelement enthält. Wenn Sie eine angefügte Eigenschaft festlegen, verwenden Sie Klammern um den Namen der angefügten Eigenschaft.

In diesem Beispiel wird veranschaulicht, wie die angefügte [ **„RelativePanel.AlignHorizontalCenterWithPanel“-Eigenschaft für ein**](/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty)TextBox`myTextBox` mit dem Namen festgelegt wird. Das erste XAML-Markup verwendet [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)-Syntax und das zweite die **Setter**-Syntax.

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### <a name="custom-state-triggers"></a>Benutzerdefinierte Zustandsauslöser

Sie können die [**StateTrigger**](/uwp/api/Windows.UI.Xaml.StateTrigger)-Klasse erweitern, um benutzerdefinierte Auslöser für eine Vielzahl von Szenarien zu erstellen. Sie können z. B. ein „StateTrigger“-Element erstellen, um die verschiedenen Zustände basierend auf dem Eingabetyp auszulösen, und dann die Ränder um ein Steuerelement herum vergrößern, wenn der Eingabetyp „Toucheingabe“ ist. Alternativ können Sie ein „StateTrigger“-Element erstellen, um unterschiedliche Zustände auf der Grundlage der Gerätefamilie anzuwenden, in der die App ausgeführt wird. Beispiele zum Erstellen von benutzerdefinierten Auslösern und zum Verwenden der Auslöser, um optimale UI-Ergebnisse in einer einzelnen XAML-Ansicht zu erzielen, finden Sie im [Beispiel für Zustandsauslöser](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers).

### <a name="visual-states-and-styles"></a>Visuelle Zustände und Stile

Sie können Stilressourcen in visuellen Zuständen verwenden, um eine Reihe von Eigenschaftsänderungen auf mehrere Steuerelemente anzuwenden. Weitere Informationen zum Verwenden von Stilen finden Sie unter [Formatieren von Steuerelementen](../controls-and-patterns/xaml-styles.md).

In diesem vereinfachten XAML-Markup aus dem Beispiel für Zustandsauslöser wird eine Stilressource auf ein Button-Element angewendet, um die Größe und die Ränder für die Maus- oder Toucheingabe anzupassen. Den vollständigen Code und die Definition des benutzerdefinierten Zustandsauslösers finden Sie im [Beispiel für Zustandsauslöser](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers).

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## <a name="related-topics"></a>Zugehörige Themen

- [Tutorial: Erstellen von adaptiven Layouts](../basics/xaml-basics-adaptive-layout.md)
- [Beispiel zu dynamischen Designtechniken (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)
- [Beispiel zu Zustandsauslösern (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)
- [Beispiel zu maßgeschneiderten Ansichten (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlTailoredMultipleViews)
