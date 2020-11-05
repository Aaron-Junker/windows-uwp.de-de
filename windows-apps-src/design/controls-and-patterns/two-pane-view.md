---
description: TwoPaneView ist ein Layoutsteuerelement, mit dem Sie die Anzeige von Apps mit zwei verschiedenen Inhaltsbereichen verwalten können.
title: Ansicht mit zwei Bereichen
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 76a6264a8ce1704e9bd209a6246c81ba9665265f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034433"
---
# <a name="two-pane-view"></a>Ansicht mit zwei Bereichen

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) (Ansicht mit zwei Bereichen) ist ein Layoutsteuerelement, mit dem Sie die Anzeige von Apps mit zwei verschiedenen Inhaltsbereichen, wie beispielsweise einer Master- und einer Detailansicht, verwalten können.

> [!IMPORTANT]
> In diesem Artikel werden Funktionen und Anleitungen beschrieben, die sich in der öffentlichen Vorschau befinden und vor der allgemeinen Verfügbarkeit noch wesentlich geändert werden können. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.

Das TwoPaneView-Steuerelement funktioniert zwar auf allen Windows-Geräten, ist aber so konzipiert, dass Sie die Vorteile von Dual-Screen-Geräten automatisch und ohne spezielle Codierung voll ausschöpfen können. Bei Dual-Screen-Geräten stellt die Ansicht mit zwei Bereichen sicher, dass die Benutzeroberfläche sauber geteilt wird, wenn sie die Lücke zwischen den Bildschirmen umfasst, sodass die Inhalte auf beiden Seiten der Lücke dargestellt werden.

> [!NOTE]
> Ein _Dual-Screen-Gerät_ oder Doppelbildschirm-Gerät ist ein besonderer Gerätetyp mit einzigartigen Funktionen. Es ist nicht gleichbedeutend mit einem Desktopgerät mit mehreren Monitoren. Weitere Informationen zu Dual-Screen-Geräten finden Sie unter [Einführung zu Dual-Screen-Geräten](/dual-screen/introduction). (Weitere Informationen zu Möglichkeiten, wie du deine App für mehrere Monitore optimieren kannst, findest du unter [Anzeigen mehrerer Ansichten](../layout/show-multiple-views.md).)

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Das Steuerelement **TwoPaneView** ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows-UI-Bibliotheks-APIs:** [TwoPaneView-Klasse](/uwp/api/microsoft.ui.xaml.controls.twopaneview)

> [!TIP]
> In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt: `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using** -Anweisung hinzugefügt: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie die Ansicht mit zwei Bereichen, wenn Sie 2 verschiedene Inhaltsbereiche haben und:

- Der Inhalt automatisch neu angeordnet und seine Größe angepasst werden soll, um optimal in das Fenster zu passen.
- Der sekundäre Inhaltsbereich basierend auf dem verfügbaren Platz angezeigt/ausgeblendet werden soll.
- Der Inhalt sauber auf die zwei Bildschirme eines Dual-Screen-Geräts aufgeteilt werden soll.

## <a name="examples"></a>Beispiele

Diese Abbildungen zeigen eine App, die auf einem Einzelbildschirm und einem Doppelbildschirm ausgeführt wird. In der Ansicht mit zwei Bereichen wird die Benutzeroberfläche der App an verschiedene Bereichskonfigurationen angepasst.

![App in der Ansicht mit zwei Bereichen auf einem einzelnen Bildschirm](images/two-pane-view/tpv-single.png)

> _App auf einem einzelnen Bildschirm._

![App in der Ansicht mit zwei Bereichen auf Doppelbildschirmen im Querformat-Modus](images/two-pane-view/tpv-dual-wide.png)

> _App auf einem Dual-Screen-Gerät im Querformat-Modus („Wide“)._

![App in der Ansicht mit zwei Bereichen auf Doppelbildschirmen im Hochformat-Modus](images/two-pane-view/tpv-dual-tall.png)

> _App auf einem Dual-Screen-Gerät im Hochformat-Modus („Tall“)._

## <a name="how-it-works"></a>Funktionsweise

Die Ansicht mit zwei Bereichen verfügt über zwei getrennte Bereiche für Ihren Inhalt. Dabei werden Größe und Anordnung der Bereiche abhängig von dem im Fenster verfügbaren Platz angepasst. Die möglichen Bereichslayouts werden durch die [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode)-Enumeration definiert:

| Enum-Wert&nbsp; | Beschreibung |
| - | - |
| `SinglePane` | Es wird nur ein Bereich angezeigt, wie durch die Eigenschaft [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) angegeben. |
| `Wide` | Die Bereiche werden nebeneinander angezeigt, oder es wird ein einzelner Bereich angezeigt, wie in der Eigenschaft [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) angegeben. |
| `Tall` | Die Bereiche werden untereinander angezeigt, oder es wird ein einzelner Bereich angezeigt, wie in der Eigenschaft [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) angegeben. |

Sie konfigurieren die Ansicht mit zwei Bereichen, indem Sie mit dem Element [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) festlegen, welcher Bereich angezeigt wird, wenn nur Platz für einen Bereich vorhanden ist. Dann geben Sie an, ob `Pane1` bei Hochformat im oberen oder unteren bzw. bei Querformat im linken oder rechten Fenster angezeigt wird.

Die Ansicht mit zwei Bereichen steuert Größe und Anordnung der Bildhälften, aber Sie müssen dennoch dafür sorgen, dass der Inhalt innerhalb der Bereiche sich an die Größen- und Ausrichtungsänderungen anpasst. Weitere Informationen zum Erstellen einer adaptiven Benutzeroberfläche finden Sie unter [Dynamische Layouts mit XAML](../layout/layouts-with-xaml.md) und [Layoutbereiche](../layout/layout-panels.md).

Das Steuerelement [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) verwaltet die Anzeige der Fensterbereiche basierend auf dem Aufteilungsstatus der Anwendung.

- Auf einem einzelnen Bildschirm

    Wenn Ihre App nur auf einem einzelnen Bildschirm angezeigt wird, passt das Steuerelement `TwoPaneView` die Größe und Position der Fensterbereiche auf der Grundlage der von Ihnen angegebenen Eigenschaftseinstellungen an. Diese Eigenschaften werden im nächsten Abschnitt näher erläutert. Der einzige Unterschied zwischen Geräten besteht darin, dass die Fenster auf einigen Geräten, wie z. B. Desktop-PCs, in der Größe veränderbar sind und auf anderen Geräten nicht.

- Auf Doppelbildschirme aufgeteilt

    Das Steuerelement `TwoPaneView` ist so konzipiert, dass die Optimierung der Benutzeroberfläche für die Aufteilung auf Dual-Screen-Geräten vereinfacht wird. Die Größe des Fensters wird an den gesamten verfügbare Platz auf den Bildschirmen angepasst. Wenn Ihre App beide Bildschirme eines Dual-Screen-Geräts überspannt, wird in jedem Bildschirm der Inhalt eines der Bereiche angezeigt und er wird ordnungsgemäß über den Zwischenraum verteilt (Spanning). Die entsprechende Spanning-Erkennung ist integriert, wenn Sie die Ansicht mit zwei Bereichen verwenden. Sie müssen nur die Hoch/Quer-Konfiguration festlegen, um anzugeben, welcher Bereich auf welchem Bildschirm angezeigt wird. Die Ansicht mit zwei Bereichen erledigt den Rest.

## <a name="how-to-use-the-two-pane-view-control"></a>Verwenden des TwoPaneView-Steuerelements

Das [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview)-Steuerelements muss nicht das Stammelement Ihres Seitenlayouts sein. Tatsächlich werden Sie es oft innerhalb eines [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)-Steuerelements verwenden, das die Gesamtnavigation für Ihre App bereitstellt. `TwoPaneView` mit zwei Bereichen passt sich entsprechend an, unabhängig davon, wo sie sich in der XAML-Struktur befindet; wir empfehlen jedoch, dass Sie `TwoPaneView`-Steuerelemente nicht ineinander verschachteln. (Wenn Sie dies tun, ist lässt sich nur das äußere `TwoPaneView`-Steuerelement aufteilen.)

### <a name="add-content-to-the-panes"></a>Hinzufügen von Inhalt zu Bereichen

Jeder Bereich einer Ansicht mit zwei Bereichen kann ein einzelnes XAML `UIElement` enthalten. Um Inhalt hinzuzufügen, platzieren Sie normalerweise ein XAML-Layoutpanel in jedem Bereich und fügen dann weitere Steuerelemente und Inhalte zum Panel hinzu. Die Bereiche können ihre Größe ändern und zwischen Querformat- und Hochformat-Modus wechseln, daher müssen Sie sicherstellen, dass sich der Inhalt in jedem Bereich an diese Änderungen anpassen kann. Weitere Informationen zum Erstellen einer adaptiven Benutzeroberfläche finden Sie unter [Dynamische Layouts mit XAML](../layout/layouts-with-xaml.md) und [Layoutbereiche](../layout/layout-panels.md).

In diesem Beispiel wird die im Abschnitt _Beispiele_ weiter oben gezeigte einfache Bild/Info-App-Benutzeroberfläche erstellt. Wenn sich die App über Doppelbildschirme erstreckt, werden das Bild und die Informationen auf separaten Bildschirmen angezeigt. Auf einem Einzelbildschirm kann der Inhalt in zwei Fenstern angezeigt oder in einem einzigen Fenster kombiniert werden, je nachdem, wie viel Platz zur Verfügung steht. (Wenn nur Platz für einen Bereich vorhanden ist, wird der Inhalt von Bereich 2 („Pane2“) in Bereich 1 („Pane1“) verschoben, und der Benutzer kann die ausgeblendeten Inhalte durch Scrollen anzeigen. Der entsprechende Code wird später im Abschnitt _Reagieren auf Modusänderungen_ dargestellt).

![Kleines Bild einer Beispiel-App auf Doppelbildschirmen](images/two-pane-view/tpv-left-right.png)

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" MinHeight="40"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <CommandBar DefaultLabelPosition="Right">
        <AppBarButton x:Name="Share" Icon="Share" Label="Share" Click="Share_Click"/>
        <AppBarButton x:Name="Print" Icon="Print" Label="Print" Click="Print_Click"/>
    </CommandBar>

    <muxc:TwoPaneView
        x:Name="MyTwoPaneView"
        Grid.Row="1"
        MinWideModeWidth="959"
        MinTallModeHeight="863"
        ModeChanged="TwoPaneView_ModeChanged">

        <muxc:TwoPaneView.Pane1>
            <Grid x:Name="Pane1Root">
                <ScrollViewer>
                    <StackPanel x:Name="Pane1StackPanel">
                        <Image Source="Assets\LandscapeImage8.jpg"
                               VerticalAlignment="Top" HorizontalAlignment="Center"
                               Margin="16,0"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane1>

        <muxc:TwoPaneView.Pane2>
            <Grid x:Name="Pane2Root">
                <ScrollViewer x:Name="DetailsContent">
                    <StackPanel Padding="16">
                        <TextBlock Text="Mountain.jpg" MaxLines="1"
                                       Style="{ThemeResource HeaderTextBlockStyle}"/>
                        <TextBlock Text="Date Taken:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="8/29/2019 9:55am"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Dimensions:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="800x536"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Resolution:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="96 dpi"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Description:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere, magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna."
                                       Style="{ThemeResource SubtitleTextBlockStyle}"
                                       TextWrapping="Wrap"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane2>
    </muxc:TwoPaneView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="TwoPaneViewStates">
            <VisualState x:Name="Normal"/>
            <VisualState x:Name="Wide">
                <VisualState.Setters>
                    <Setter Target="MyTwoPaneView.Pane1Length"
                            Value="2*"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="specify-which-pane-to-display"></a>Festlegen, welcher Bereich angezeigt werden soll

Wenn nur ein einziger Bereich angezeigt werden kann, wird die Eigenschaft [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) des TwoPaneView-Steuerelements verwendet, um zu bestimmen, welcher Bereich angezeigt werden soll. Standardmäßig ist „PanePriority“ auf **Pane1** eingestellt. So legen Sie diese Eigenschaft in XAML oder im Code fest.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>Anpassen der Bereichsgröße

Auf einem Doppelbildschirm wird die Größe der Bereiche durch die Eigenschaften [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) und [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) bestimmt. Diese verwenden die [GridLength](/uwp/api/windows.ui.xaml.gridlength)-Werte _Auto_ und _\*_ (Stern) zur Größenanpassung. Eine Erläuterung der automatischen Größenanpassung und der Größenanpassung mit der Sternvariablen finden Sie unter [Dynamische Layouts mit XAML](../layout/layouts-with-xaml.md#layout-properties) im Abschnitt _Layouteigenschaften_.

Standardmäßig ist `Pane1Length` auf `Auto` festgelegt und passt sich in seiner Größe automatisch dem Inhalt an. `Pane2Length` ist auf `*` festgelegt und verwendet den gesamten verbleibenden Platz.

![Ansicht mit zwei Bereichen, dessen Bereiche auf Standardgrößen festgelegt sind](images/two-pane-view/tpv-size-default.png)

> _Bereiche mit Standardgrößenanpassung_

Die Standardwerte sind für ein typisches Master/Detail-Layout nützlich, bei dem eine Liste von Elementen in Bereich 1 (`Pane1`) und viele Details in Bereich 2 (`Pane2`) angezeigt werden. Je nach Inhalt möchten Sie den Platz jedoch möglicherweise anders aufteilen. Hier ist die Länge von Bereich 1 (`Pane1Length`) auf `2*` festgelegt, sodass er doppelt so viel Platz erhält wie Bereich 2 (`Pane2`).

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![Ansicht mit zwei Bereichen, wobei Bereich 1 zwei Drittel des Bildschirms nutzt, und Bereich 2 ein Drittel](images/two-pane-view/tpv-size-2.png)

> _Bereiche mit der den Werten „2*“ und „*“ für die Größenanpassung_

> [!NOTE]
> Wie bereits erwähnt, werden diese Eigenschaften ignoriert, wenn sich die App auf zwei Bildschirme aufteilt, und jeder Bereich füllt einen der Bildschirme aus.

Wenn Sie einen Bereich für die automatische Größenanpassung festlegen, können Sie die Größe steuern, indem Sie die Höhe und Breite des Panels festlegen, das den Inhalt des Bereichs enthält. In diesem Fall müssen Sie möglicherweise das ModeChanged-Ereignis behandeln und die Höhen- und Breitenbeschränkungen des Inhalts entsprechend dem aktuellen Modus festlegen.

### <a name="display-in-wide-or-tall-mode"></a>Im Querformat- oder Hochformat-Modus anzeigen

Auf einem Einzelbildschirm wird der [Modus](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) der Darstellung der Ansicht mit zwei Bereichen durch die Eigenschaften [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) und [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) bestimmt. Beide Eigenschaften verfügen über den Standardwert „641px“, der identisch ist mit identisch mit [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

Diese Tabelle zeigt, wie die Höhe (Height) und Breite (Width) des TwoPaneView-Steuerelements bestimmen, welcher Anzeigemodus verwendet wird.

| TwoPaneView-Bedingung  | Modus |
|---------|---------|
| `Width` > `MinWideModeWidth` | `Wide`-Modus wird verwendet |
| `Width` <= `MinWideModeWidth` und `Height` > `MinTallModeHeight` | `Tall`-Modus wird verwendet |
| `Width` <= `MinWideModeWidth` und `Height` <= `MinTallModeHeight` | `SinglePane`-Modus wird verwendet |


> [!NOTE]
> Wie bereits erwähnt, werden diese Eigenschaften ignoriert, wenn sich die App auf zwei Bildschirme aufteilt, und der Anzeigemodus wird basierend auf der _Ausrichtung_ des Geräts bestimmt.

#### <a name="wide-configuration-options"></a>Konfigurationsoptionen für Querformat („Wide“)

Die Ansicht mit zwei Bereichen wechselt in den `Wide`-Modus, wenn ein einzelnes Display vorhanden ist, das breiter als die `MinWideModeWidth`-Eigenschaft ist. `MinWideModeWidth` steuert, wann die Ansicht mit zwei Bereichen in den Querformat-Modus wechselt. Der Standardwert ist „641px“, aber Sie können ihn in den gewünschten Wert ändern. Im Allgemeinen sollten Sie diese Eigenschaft auf den Wert festlegen, der der gewünschten minimalen Breite Ihres Bereichs entspricht.

Wenn die Ansicht mit zwei Bereichen im Querformat-Modus angezeigt wird, bestimmt die [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration)-Eigenschaft, was angezeigt werden soll:

| [Enum-Wert](/uwp/api/microsoft.ui.xaml.controls.twopaneviewwidemodeconfiguration) | Beschreibung |
|---------|---------|
| `SinglePane` | Ein einzelner Bereich (wie durch `PanePriority` bestimmt). Der Bereich nimmt die gesamte Größe von `TwoPaneView` in Anspruch (d. h. seine Größe wird in beiden Richtungen mit der Sternvariablen angepasst). |
| `LeftRight` | `Pane1` auf der linken und `Pane2` auf der rechten Seite. Die Größe beider Bereiche wird vertikal mit der Sternvariablen angepasst, die Breite von Bereich 1 (`Pane1`) wird automatisch festgelegt und die Breite von Bereich 2 (`Pane2`) wird mit der Sternvariablen angepasst. |
| `RightLeft` | `Pane1` auf der rechten und `Pane2` auf der linken Seite. Die Größe beider Bereiche wird vertikal mit der Sternvariablen angepasst, die Breite von Bereich 1 (`Pane2`) wird automatisch festgelegt und die Breite von Bereich 2 (`Pane1`) wird mit der Sternvariablen angepasst. |

Der Standardwert für diese Einstellung ist `LeftRight`.

| LeftRight | RightLeft |
| - | - |
| ![Ansicht mit zwei Bereichen, von links nach rechts konfiguriert](images/two-pane-view/tpv-left-right.png)  | ![Ansicht mit zwei Bereichen, von rechts nach links konfiguriert](images/two-pane-view/tpv-right-left.png)  |

> **TIPP:** Wenn das Gerät eine Sprache mit Leserichtung von rechts nach links (RTL) verwendet, wird die Reihenfolge in der Ansicht mit zwei Bereichen automatisch getauscht: `RightLeft` wird als `LeftRight` und `LeftRight` als `RightLeft` gerendert.

#### <a name="tall-configuration-options"></a>Konfigurationsoptionen für Hochformat („Tall“)

Die Ansicht mit zwei Bereichen wechselt in den `Tall`-Modus, wenn ein einzelnes Display vorhanden ist, das schmaler als die `MinWideModeWidth`-Eigenschaft und höher als die `MinTallModeHeight`-Eigenschaft ist. Der Standardwert ist „641px“, aber Sie können ihn in den gewünschten Wert ändern. Im Allgemeinen sollten Sie diese Eigenschaft auf den Wert festlegen, der der gewünschten minimalen Höhe Ihres Bereichs entspricht.

Wenn die Ansicht mit zwei Bereichen im Hochformat-Modus angezeigt wird, bestimmt die Eigenschaft [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration), was angezeigt werden soll:

| [Enum-Wert](/uwp/api/microsoft.ui.xaml.controls.twopaneviewtallmodeconfiguration) | Beschreibung |
|---------|---------|
| `SinglePane` | Ein einzelner Bereich (wie durch `PanePriority` bestimmt). Der Bereich nimmt die gesamte Größe von `TwoPaneView` in Anspruch (d. h. seine Größe wird in beiden Richtungen mit der Sternvariablen angepasst). |
| `TopBottom` | `Pane1` oben und `Pane2` unten. Die Größe beider Bereiche wird horizontal mit der Sternvariablen angepasst, die Höhe von Bereich 1 (`Pane1`) wird automatisch festgelegt und die Höhe von Bereich 2 (`Pane2`) wird mit der Sternvariablen angepasst. |
| `BottomTop` | `Pane1` unten und `Pane2` oben. Die Größe beider Bereiche wird horizontal mit der Sternvariablen angepasst, die Höhe von Bereich 2 (`Pane2`) wird automatisch festgelegt und die Höhe von Bereich 1 (`Pane1`) wird mit der Sternvariablen angepasst. |

Der Standardwert ist `TopBottom`.

| TopBottom | BottomTop |
| - | - |
| ![Ansicht mit zwei Bereichen, von oben nach unten konfiguriert](images/two-pane-view/tpv-top-bottom.png)  | ![Ansicht mit zwei Bereichen, von unten nach oben konfiguriert](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Spezielle Werte für „MinWideModeWidth“ und „MinTallModeHeight“

Mit der Eigenschaft `MinWideModeWidth` können Sie verhindern, dass die Ansicht mit zwei Bereichen in den Querformat-Modus („Wide“) wechselt. Legen Sie `MinWideModeWidth` dazu auf [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) fest.

Wenn Sie `MinTallModeHeight` auf [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) festlegen, verhindert dies, dass die Ansicht mit zwei Bereichen in den Hochformat-Modus („Tall“) wechselt.

Wenn Sie `MinTallModeHeight` auf 0 festlegen, verhindert dies, dass die Ansicht mit zwei Bereichen in den `SinglePane`-Modus wechselt.

#### <a name="responding-to-mode-changes"></a>Reagieren auf Modusänderungen

Sie können die schreibgeschützte Eigenschaft [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) verwenden, um den aktuellen Anzeigemodus abzurufen. Immer wenn die Ansicht mit zwei Bereichen ändert, welcher Bereiche oder welche Bereiche anzeigt wird/werden, tritt das Ereignis [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) ein, bevor der aktualisierte Inhalt gerendert wird. Sie können das Ereignis so behandelt, dass auf Änderungen im Anzeigemodus reagiert wird.

> [!TIP]
> Das `ModeChanged`-Ereignis tritt beim ersten Laden der Seite nicht auf, daher sollte Ihre Standard-XAML die Benutzeroberfläche so darstellen, wie sie beim ersten Laden angezeigt werden soll.

Eine Möglichkeit, dieses Ereignis zu nutzen, besteht darin, die Benutzeroberfläche Ihrer App zu aktualisieren, damit Benutzer den gesamten Inhalt im `SinglePane`-Modus anzeigen können. Die Beispiel-App verfügt über einen Primärbereich (das Bild) und einen Infobereich.

![Kleines Bild einer Beispiel-App im Hochformat-Modus](images/two-pane-view/tpv-top-bottom.png)

> _Hochformat-Modus_

Wenn nur Platz zum Anzeigen eines Bereichs vorhanden ist, kann der Inhalt von Bereich 2 (`Pane2`) in Bereich 1 (`Pane1`) verschoben werden, damit der Benutzer alle Inhalte durch Scrollen anzeigen kann. Das sieht ungefähr wie folgt aus.

![Bild der Beispiel-App auf einem Bildschirm, wobei der gesamte Inhalt in einem einzigen Bereich angezeigt wird und gescrollt werden kann](images/two-pane-view/tpv-single-pane.png)

> _Singlepane-Modus_

Denken Sie daran, dass die Eigenschaften `MinWideModeWidth` und `MinTallModeHeight` bestimmen, wann sich der Anzeigemodus ändert, Somit können Sie ändern, wann der Inhalt zwischen den Bereichen verschoben wird, indem Sie die Werte dieser Eigenschaften anpassen.

Hier ist der `ModeChanged`-Ereignishandlercode, der den Inhalt zwischen `Pane1` und `Pane2` verschiebt. Außerdem wird ein [VisualState](/uwp/api/windows.ui.xaml.visualstate)-Steuerelement festgelegt, um die Breite des Bilds im Querformat-Modus einzuschränken.

```csharp
private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
{
    // Remove details content from it's parent panel.
    ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
    // Set Normal visual state.
    Windows.UI.Xaml.VisualStateManager.GoToState(this, "Normal", true);

    // Single pane
    if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
    {
        // Add the details content to Pane1.
        Pane1StackPanel.Children.Add(DetailsContent);
    }
    // Dual pane.
    else
    {
        // Put details content in Pane2.
        Pane2Root.Children.Add(DetailsContent);

        // If also in Wide mode, set Wide visual state
        // to constrain the width of the image to 2*.
        if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
        {
            Windows.UI.Xaml.VisualStateManager.GoToState(this, "Wide", true);
        }
    }
}
```

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwenden Sie die Ansicht mit zwei Bereichen, wann immer Sie können, damit Ihre App die Vorteile von Doppelbildschirmen und großen Bildschirmen nutzen kann.
- Platzieren Sie keine Ansicht mit zwei Bereichen in eine andere Ansicht mit zwei Bereichen.

## <a name="related-articles"></a>Verwandte Artikel

- [Übersicht über das Layout](../layout/index.md)
- [Dual-Screen-Entwicklung](/dual-screen)
- [Einführung in Dual-Screen-Geräte](/dual-screen/introduction)
