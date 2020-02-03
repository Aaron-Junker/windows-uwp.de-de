---
Description: TwoPaneView ist ein Layoutsteuerelement, mit dem Sie die Anzeige von Apps mit zwei verschiedenen Inhaltsbereichen verwalten können.
title: Ansicht mit zwei Bereichen
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 67b97aec970cc655700729743f10c63c666ab0a6
ms.sourcegitcommit: 09571e1c6a01fabed773330aa7ead459a47d94f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929260"
---
# <a name="two-pane-view"></a>Ansicht mit zwei Bereichen

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) (Ansicht mit zwei Bereichen) ist ein Layoutsteuerelement, mit dem Sie die Anzeige von Apps mit zwei verschiedenen Inhaltsbereichen, wie beispielsweise einer Master- und einer Detailansicht, verwalten können.

> [!IMPORTANT]
> In diesem Artikel werden Funktionen und Anleitungen beschrieben, die sich in der öffentlichen Vorschau befinden und vor der allgemeinen Verfügbarkeit noch wesentlich geändert werden können. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.

Das TwoPaneView-Steuerelement funktioniert zwar auf allen Windows-Geräten, ist aber so konzipiert, dass Sie die Vorteile von Dual-Screen-Geräten automatisch und ohne spezielle Codierung voll ausschöpfen können. Bei Dual-Screen-Geräten stellt die Ansicht mit zwei Bereichen sicher, dass die Benutzeroberfläche sauber geteilt wird, wenn sie die Lücke zwischen den Bildschirmen umfasst, sodass die Inhalte auf beiden Seiten der Lücke dargestellt werden.

> **HINWEIS:** Ein _Dual-Screen-Gerät_ oder Doppelbildschirm-Gerät ist ein besonderer Gerätetyp mit einzigartigen Funktionen. Es ist nicht gleichbedeutend mit einem Desktopgerät mit mehreren Monitoren. Weitere Informationen zu Dual-Screen-Geräten finden Sie unter [Einführung zu Dual-Screen-Geräten](/dual-screen/introduction).
>
>In diesem Artikel werden Begriffe _Single-Screen-Gerät_ (Einzelbildschirm-Gerät) oder _Single-Screen-Display_ (Einzelbildschirm-Display) für alle Geräte verwendet, die keine Dual-Screen-Geräte sind, unabhängig davon, ob die Geräten einen einzigen Monitor besitzen oder Teil eines Setups mit mehreren Monitoren sind. Das TwoPaneView-Steuerelement verhält sich auf einem Einzelbildschirm auf dieselbe Weise wie andere XAML-Steuerelemente. Weitere Informationen zu Möglichkeiten, wie Sie Ihre App für mehrere Monitore optimieren können, finden Sie unter [Anzeigen mehrerer Ansichten](/windows/uwp/design/layout/show-multiple-views).

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

| **Plattform-APIs** | **Windows-UI-Bibliotheks-APIs** |
| - | - |
| [TwoPaneView-Klasse](/uwp/api/windows.ui.xaml.controls.twopaneview) | [TwoPaneView-Klasse](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt:

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt:

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie die Ansicht mit zwei Bereichen, wenn Sie 2 verschiedene Inhaltsbereiche haben und:

- Der Inhalt automatisch neu angeordnet und seine Größe angepasst werden soll, um optimal in das Fenster zu passen.
- Der sekundäre Inhaltsbereich basierend auf dem verfügbaren Platz angezeigt/ausgeblendet werden soll.
- Der Inhalt sauber auf die zwei Bildschirme eines Dual-Screen-Geräts aufgeteilt werden soll.

## <a name="examples"></a>Beispiele

Diese Abbildungen zeigen eine App, die auf einem Single-Screen-Gerät und einem Dual-Screen-Gerät ausgeführt wird. In der Ansicht mit zwei Bereichen wird die Benutzeroberfläche der App an verschiedene Bereichskonfigurationen auf jedem Gerät angepasst.

![tpv-single.png](images/two-pane-view/tpv-single.png)

_App auf einem Gerät mit einem Bildschirm._

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

_App auf einem Dual-Screen-Gerät im Querformat-Modus („Wide“)._

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

_App auf einem Dual-Screen-Gerät im Hochformat-Modus („Tall“)._

## <a name="how-it-works"></a>Funktionsweise

Die Ansicht mit zwei Bereichen verfügt über zwei getrennte Bereiche für Ihren Inhalt. Dabei werden Größe und Anordnung der Bereiche abhängig von dem im Fenster verfügbaren Platz angepasst. Die möglichen Bereichslayouts werden durch die [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode)-Enumeration definiert:

- **SinglePane** – Es wird nur ein Bereich angezeigt, wie durch die Eigenschaft [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) angegeben.
- **Wide** – Die Bereiche werden nebeneinander angezeigt, oder es wird ein einzelner Bereich angezeigt, wie in der Eigenschaft [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) angegeben.
- **Tall** – Die Bereiche werden untereinander angezeigt, oder es wird ein einzelner Bereich angezeigt, wie in der Eigenschaft [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) angegeben.

Sie konfigurieren die Ansicht mit zwei Bereichen, indem Sie mit dem Element [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) festlegen, welcher Bereich angezeigt wird, wenn nur Platz für einen Bereich vorhanden ist. Dann geben Sie an, ob Bereich 1 („Pane1“) bei Hochformat im oberen oder unteren bzw. bei Querformat im linken oder rechten Fenster angezeigt wird.

Die Ansicht mit zwei Bereichen steuert Größe und Anordnung der Bildhälften, aber Sie müssen dennoch dafür sorgen, dass der Inhalt innerhalb der Bereiche sich an die Größen- und Ausrichtungsänderungen anpasst. Weitere Informationen zum Erstellen einer adaptiven Benutzeroberfläche finden Sie unter [Dynamische Layouts mit XAML](/windows/uwp/design/layout/layouts-with-xaml) und [Layoutbereiche](/windows/uwp/design/layout/layout-panels).

Die Ansicht mit zwei Bereichen verwaltet die Anzeige der Bildhälften je nach Art des Geräts, auf dem die App ausgeführt wird:

- Auf einem Dual-Screen-Gerät

    Die Ansicht mit zwei Bereichen ist so konzipiert, dass die Optimierung der Benutzeroberfläche für Dual-Screen-Geräte vereinfacht wird. Die Größe des Fensters passt sich an den gesamten verfügbare Platz auf den Bildschirmen an. Wenn sich Ihre App nur auf einem Bildschirm des Geräts befindet, wird ein Bereich angezeigt, wie in der Eigenschaft [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) angegeben.

    Wenn Ihre App beide Bildschirme eines Dual-Screen-Geräts überspannt, wird in jedem Bildschirm der Inhalt eines der Bereiche angezeigt und er wird ordnungsgemäß über den Zwischenraum verteilt (Spanning). Die entsprechende Spanning-Erkennung ist integriert, wenn Sie die Ansicht mit zwei Bereichen verwenden. Sie müssen nur die Hoch/Quer-Konfiguration festlegen, um anzugeben, welcher Bereich auf welchem Bildschirm angezeigt wird. Die Ansicht mit zwei Bereichen erledigt den Rest.


- Auf einem Single-Screen-Gerät

    Bei der Ausführung auf einem Gerät mit einem Bildschirm, z. B. einem Laptop oder Desktop-PC, verhält sich die Ansicht mit zwei Bereichen so, wie Sie es von jedem XAML-Steuerelement erwarten würden. Wenn die Größe des Fensters verändert wird, werden Größe und Position der Bereiche basierend auf der Fenstergröße angepasst. Sie können zusätzliche Eigenschaften festlegen, um das Verhalten des Steuerelements zu definieren, wenn es auf einem Einzelbildschirm in einem Fenster mit veränderbarer Größe angezeigt wird. Diese Eigenschaften werden im nächsten Abschnitt näher erläutert.

## <a name="how-to-use-the-two-pane-view-control"></a>Verwenden des TwoPaneView-Steuerelements

Das [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview)-Steuerelements muss nicht das Stammelement Ihres Seitenlayouts sein. Tatsächlich werden Sie es oft innerhalb eines [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)-Steuerelements verwenden, das die Gesamtnavigation für Ihre App bereitstellt. Die Ansicht mit zwei Bereichen passt sich entsprechend an, unabhängig davon, wo sie sich in der XAML-Struktur befindet; wir empfehlen jedoch, dass Sie TwoPaneView-Steuerelemente nicht ineinander verschachteln.

### <a name="add-content-to-the-panes"></a>Hinzufügen von Inhalt zu Bereichen

Jeder Bereich einer Ansicht mit zwei Bereichen kann ein einzelnes XAML-Benutzeroberflächenelement enthalten. Um Inhalt hinzuzufügen, platzieren Sie normalerweise ein XAML-Layoutpanel in jedem Bereich und fügen dann weitere Steuerelemente und Inhalte zum Panel hinzu. Die Bereiche können ihre Größe ändern und zwischen Querformat- und Hochformat-Modus wechseln, daher müssen Sie sicherstellen, dass sich der Inhalt in jedem Bereich an diese Änderungen anpassen kann. Weitere Informationen zum Erstellen einer adaptiven Benutzeroberfläche finden Sie unter [Dynamische Layouts mit XAML](/windows/uwp/design/layout/layouts-with-xaml) und [Layoutbereiche](/windows/uwp/design/layout/layout-panels).

In diesem Beispiel wird die hier gezeigte einfache Bild/Info-App-Benutzeroberfläche erstellt. Wenn es Platz für zwei Bereiche gibt, werden das Bild und die Informationen in getrennten Bereichen angezeigt. (Wenn nur Platz für einen Bereich vorhanden ist, wird der Inhalt von Bereich 2 („Pane2“) in Bereich 1 („Pane1“) verschoben, und der Benutzer kann die ausgeblendeten Inhalte durch Scrollen anzeigen. Der entsprechende Code wird später im Abschnitt _Reagieren auf Modusänderungen_ dargestellt).

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
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

Die Größe der Bereiche auf einem Gerät mit einem Bildschirm wird durch die Eigenschaften [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) und [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length) bestimmt. Diese verwenden die [GridLength](/uwp/api/windows.ui.xaml.gridlength)-Werte _Auto_ und _\*_ (Stern) zur Größenanpassung. Eine Erläuterung der automatischen Größenanpassung und der Größenanpassung mit der Sternvariablen finden Sie unter [Dynamische Layouts mit XAML](/windows/uwp/design/layout/layouts-with-xaml#layout-properties) im Abschnitt _Layouteigenschaften_.

Standardmäßig ist „Paneel1Length“ auf **Auto** festgelegt und passt sich in seiner Größe automatisch dem Inhalt an. „Pane2Length“ ist auf „*“ festgelegt und verwendet den gesamten verbleibenden Platz.

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

_Bereiche mit Standardgrößenanpassung_

Die Standardwerte sind für ein typisches Master/Detail-Layout nützlich, bei dem eine Liste von Elementen in Bereich 1 („Pane1“) und viele Details in Bereich 2 („Pane2“) angezeigt werden. Je nach Inhalt möchten Sie den Platz jedoch möglicherweise anders aufteilen. Hier ist „Pane1Length“ auf „2*“ festgelegt, also doppelt so groß wie „Pane2“.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

_Bereiche mit der den Werten „2*“ und „*“ für die Größenanpassung_

> **HINWEIS:** Wie bereits erwähnt, werden diese Eigenschaften bei einem Dual-Screen-Gerät ignoriert, und die Bereiche werden in Größe und Anordnung automatisch auf der Grundlage der _Position_ des Geräts angepasst.

Wenn Sie einen Bereich für die automatische Größenanpassung festlegen, können Sie die Größe steuern, indem Sie die Höhe und Breite des Panels festlegen, das den Inhalt des Bereichs enthält. In diesem Fall müssen Sie möglicherweise das ModeChanged-Ereignis behandeln und die Höhen- und Breitenbeschränkungen des Inhalts entsprechend dem aktuellen Modus festlegen.

### <a name="display-in-wide-or-tall-mode"></a>Im Querformat- oder Hochformat-Modus anzeigen

Auf einem Desktopdisplay wird der [Modus](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) der Darstellung der Ansicht mit zwei Bereichen durch die Eigenschaften [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) und [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight) bestimmt. Beide Eigenschaften verfügen über den Standardwert „641px“, der identisch ist mit identisch mit [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

Wenn das Fenster:

- Breiter als „MinWideModeWidth“ ist, wird der **Wide**-Modus verwendet.
- Schmaler als „MinWideModeWidth“ und höher als „MinTallModeHeight“ ist, wird der **Tall**-Modus verwendet.
- Schmaler als „MinWideModeWidth“ und niedriger als „MinTallModeHeight“ ist, wird der **SinglePane**-Modus verwendet.

> **HINWEIS:** Wie bereits erwähnt, werden diese Eigenschaften bei einem Dual-Screen-Gerät ignoriert, und die Bereiche werden in Größe und Anordnung automatisch auf der Grundlage der _Position_ des Geräts angepasst.

#### <a name="wide-configuration-options"></a>Konfigurationsoptionen für Querformat („Wide“)

Die Ansicht mit zwei Bereichen wird im „Wide“-Modus angezeigt, wenn eine einzelne Anzeige vorhanden ist, die breiter als die MinWideModeWidth-Eigenschaft ist. MinWideModeWidth steuert, wann die Ansicht mit zwei Bereichen in den „Wide“-Modus wechselt. Der Standardwert ist „641px“, aber Sie können ihn in den gewünschten Wert ändern. Im Allgemeinen sollten Sie diese Eigenschaft auf den Wert festlegen, der der gewünschten minimalen Breite Ihres Bereichs entspricht.

Wenn die Ansicht mit zwei Bereichen im Querformat-Modus angezeigt wird, bestimmt die WideModeConfiguration-Eigenschaft, was angezeigt werden soll:

- **SinglePane** – Ein einzelnes Fenster (wie durch „PanePriority“ bestimmt). Der Bereich nimmt die gesamte Größe von „TwoPaneView“ in Anspruch (d. h. seine Größe wird in beiden Richtungen mit der Sternvariablen angepasst).
- **LeftRight** – Bereich 1 auf der linken, Bereich 2 auf der rechten Seite. Die Größe beider Bereiche wird vertikal mit der Sternvariablen angepasst, die Breite von Bereich 1 wird automatisch festgelegt und die Breite von Bereich 2 wird mit der Sternvariablen angepasst.
- **RightLeft** – Bereich 1 rechts, Bereich 2 links. Die Größe beider Bereiche wird vertikal mit der Sternvariablen angepasst, die Breite von Bereich 2 wird automatisch festgelegt und die Breite von Bereich 1 wird mit der Sternvariablen angepasst.

Der Standardwert ist **LeftRight**.

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **TIPP:** Wenn das Gerät eine Sprache mit Leserichtung von rechts nach links (RTL) verwendet, wird die Reihenfolge in der Ansicht mit zwei Bereichen automatisch getauscht: „RightLeft“ wird als „LeftRight“ und „LeftRight“ als „RightLeft“ gerendert.

#### <a name="tall-configuration-options"></a>Konfigurationsoptionen für Hochformat („Tall“)

Die Ansicht mit zwei Bereichen wird im „Tall“-Modus angezeigt, wenn eine einzelne Anzeige vorhanden ist, die schmaler als „MinWideModeWidth“ und höher als „MinTallModeHeight“ ist. Der Standardwert ist „641px“, aber Sie können ihn in den gewünschten Wert ändern. Im Allgemeinen sollten Sie diese Eigenschaft auf den Wert festlegen, der der gewünschten minimalen Höhe Ihres Bereichs entspricht.

Wenn die Ansicht mit zwei Bereichen im Querformat-Modus angezeigt wird, bestimmt die TallLayout-Eigenschaft, was angezeigt werden soll:

- **SinglePane** – Ein einzelnes Fenster (wie durch „PanePriority“ bestimmt). Der Bereich nimmt die gesamte Größe von „TwoPaneView“ in Anspruch (d. h. seine Größe wird in beiden Richtungen mit der Sternvariablen angepasst).
- **TopBottom** – Bereich 1 oben, Bereich 2 unten. Die Größe beider Bereiche wird horizontal mit der Sternvariablen angepasst, die Höhe von Bereich 1 wird automatisch festgelegt und die Höhe von Bereich 2 wird mit der Sternvariablen angepasst.
- **BottomTop** – Bereich 1 unten, Bereich 2 oben. Die Größe beider Bereiche wird horizontal mit der Sternvariablen angepasst, die Höhe von Bereich 2 wird automatisch festgelegt und die Höhe von Bereich 1 wird mit der Sternvariablen angepasst.

Die Standardeinstellung ist **TopBottom**.

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Spezielle Werte für „MinWideModeWidth“ und „MinTallModeHeight“

Mit der Eigenschaft „MinWideModeWidth“ können Sie verhindern, dass die Ansicht mit zwei Bereichen in den Wide-Modus wechselt. Legen Sie „MinWideModeWidth“ dazu auf [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) fest.

Wenn Sie „MinTallModeHeight“ auf [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) festlegen, verhindert dies, dass die Ansicht mit zwei Bereichen in den Tall-Modus wechselt.

Wenn Sie „MinTallModeHeight“ auf „0“ festlegen, verhindert dies, dass die Ansicht mit zwei Bereichen in den SinglePane-Modus wechselt.

#### <a name="responding-to-mode-changes"></a>Reagieren auf Modusänderungen

Sie können die schreibgeschützte Eigenschaft [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) verwenden, um den aktuellen Anzeigemodus abzurufen. Immer wenn die Ansicht mit zwei Bereichen ändert, welcher Bereiche oder welche Bereiche anzeigt wird/werden, tritt das Ereignis [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) ein, bevor der aktualisierte Inhalt gerendert wird. Sie können das Ereignis so behandelt, dass auf Änderungen im Anzeigemodus reagiert wird.

Eine Möglichkeit, dieses Ereignis zu nutzen, besteht darin, die Benutzeroberfläche Ihrer App zu aktualisieren, damit Benutzer den gesamten Inhalt im SinglePane-Modus anzeigen können. Die Beispiel-App verfügt über einen Primärbereich (das Bild) und einen Infobereich.

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

_Wide-Modus_

Wenn nur Platz zum Anzeigen eines Bereichs vorhanden ist, wird der Inhalt von Bereich 2 („Pane2“) in Bereich 1 („Pane1“) verschoben, damit der Benutzer alle Inhalte durch Scrollen anzeigen kann. Das sieht ungefähr wie folgt aus.

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

_Singlepane-Modus_

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwenden Sie die Ansicht mit zwei Bereichen, wann immer Sie können, damit Ihre App die Vorteile von Doppeldisplays und großen Bildschirmen nutzen kann.
- Platzieren Sie keine Ansicht mit zwei Bereichen in eine andere Ansicht mit zwei Bereichen.

## <a name="related-articles"></a>Verwandte Artikel

- [Übersicht über das Layout](../layout/index.md)
- [Dual-Screen-Entwicklung](/dual-screen)
- [Einführung in Dual-Screen-Geräte](/dual-screen/introduction)
