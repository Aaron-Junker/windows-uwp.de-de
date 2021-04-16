---
description: Erfahren Sie, wie Sie mit den Steuerelementen der Befehlsleiste den Benutzern einfachen Zugriff auf die häufigsten Befehle und Aufgaben Ihrer App ermöglichen.
title: Befehlsleiste
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f3d21f60a26f7a1c6a63d678c60e1d9538580d55
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107219043"
---
# <a name="command-bar"></a>Befehlsleiste

Über Befehlsleisten können Benutzer auf häufig verwendete Befehle in Ihrer App komfortabel zugreifen. Befehlsleisten ermöglichen den Zugriff auf Befehle auf App-Ebene oder seitenspezifische Befehle und können bei jedem Navigationsmuster verwendet werden.

> **Plattform-APIs:** [CommandBar-Klasse](/uwp/api/windows.ui.xaml.controls.commandbar), [AppBarButton-Klasse](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton-Klasse](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [AppBarSeparator-Klasse](/uwp/api/windows.ui.xaml.controls.appbarseparator)

![Beispiel für eine Befehlsleiste mit Symbolen](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Das CommandBar-Steuerelement ist ein allgemeines, flexibles und kleines Steuerelement, mit dem komplexe Inhalte wie Bilder oder Textblöcke sowie einfache Befehle wie [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)-, [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)- und [AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)-Steuerelemente angezeigt werden können.

> [!NOTE]
> XAML stellt das [AppBar](/uwp/api/windows.ui.xaml.controls.appbar)-Steuerelement und das [CommandBar](/uwp/api/windows.ui.xaml.controls.commandbar)-Steuerelement bereit. Sie sollten AppBar nur verwenden, wenn Sie eine universelle Windows 8-App aktualisieren, in der AppBar verwendet wird, und möglichst wenige Änderungen vornehmen möchten. Für neue Apps in Windows 10 sollten Sie stattdessen das CommandBar-Steuerelement verwenden. In diesem Dokument wird davon ausgegangen, dass Sie das CommandBar-Steuerelement verwenden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CommandBar">die App zu öffnen und „CommandBar“ in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Eine erweiterte Befehlsleiste.

![Erweiterte Befehlsleiste](images/control-examples/command-bar-photos.png)

## <a name="anatomy"></a>Aufbau

Standardmäßig werden auf der Befehlsleiste eine Reihe von Symbolschaltflächen und eine optionale, durch drei Punkte (\[•••\]) dargestellte Schaltfläche für weitere Optionen angezeigt. Dies ist die Befehlsleiste, die mit dem weiter unten gezeigten Beispielcode erstellt wird. Hier ist der geschlossene, kompakte Zustand zu sehen.

![Screenshot, der eine geschlossene Befehlsleiste zeigt.](images/command-bar-compact.png)

Die Befehlsleiste kann auch im geschlossenen, minimierten Zustand angezeigt werden, die wie folgt aussieht. Weitere Informationen finden Sie im Abschnitt [Geöffneter und geschlossener Zustand](#open-and-closed-states).

![Screenshot, der eine Befehlsleiste in einem geschlossenen minimierten Zustand zeigt.](images/command-bar-minimal.png)

Dies ist die gleiche Befehlsleiste im geöffneten Zustand. Die Beschriftungen zeigen die wichtigsten Elemente des Steuerelements.

![Screenshot, der eine Befehlsleiste im geöffneten Zustand zeigt.](images/commandbar_anatomy_open.png)

Die Befehlsleiste ist in 4 Hauptbereiche unterteilt:
- Der Inhaltsbereich wird an der linken Seite der Leiste ausgerichtet. Er wird angezeigt, wenn die [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)-Eigenschaft gefüllt wird.
- Der primäre Befehlsbereich wird an der rechten Seite der Leiste ausgerichtet. Er wird angezeigt, wenn die [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)-Eigenschaft gefüllt wird.  
- Die Schaltfläche für weitere Optionen (\[•••\]) wird rechts auf der Leiste angezeigt. Durch Klicken auf die Schaltfläche für weitere Informationen (\[•••\]) werden primäre Befehlsbezeichnungen angezeigt und wird das Überlaufmenü geöffnet, wenn sekundäre Befehle vorhanden sind. Die Schaltfläche wird nicht angezeigt, wenn keine primären oder sekundären Befehlsbezeichnungen vorhanden sind. Dieses Standardverhalten können Sie mit der [OverflowButtonVisibility](/uwp/api/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility)-Eigenschaft ändern.
- Das Überlaufmenü wird nur angezeigt, wenn die Befehlsleiste geöffnet ist und die [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands)-Eigenschaft gefüllt wird. Wenn der Platz begrenzt ist, werden primäre Befehle in den Bereich „SecondaryCommands“ verschoben. Dieses Standardverhalten können Sie mit der [IsDynamicOverflowEnabled](/uwp/api/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled)-Eigenschaft ändern.

Das Layout wird umgekehrt, wenn [FlowDirection](/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) auf **RightToLeft** festgelegt wurde.

## <a name="create-a-command-bar"></a>Erstellen einer Befehlsleiste
Mit folgendem Beispiel wird die oben gezeigte Befehlsleiste erstellt.

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>Befehle und Inhalt
Das CommandBar-Steuerelement verfügt über 3 Eigenschaften, mit denen Sie Befehle und Inhalt hinzufügen können: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands), [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) und [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content).


### <a name="commands"></a>Befehle

Befehlsleistenelemente werden standardmäßig der **PrimaryCommands**-Sammlung hinzugefügt. Sie sollten Befehle nach ihrer Wichtigkeit hinzufügen, damit die wichtigsten Befehle immer sichtbar sind. Wenn die Breite der Befehlsleiste geändert wird, z.B. wenn Benutzer die Größe des App-Fensters ändern, werden primäre Befehle dynamisch zwischen der Befehlsleiste und dem Überlaufmenü an Haltepunkten verschoben. Dieses Standardverhalten können Sie mit der [IsDynamicOverflowEnabled](/uwp/api/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled)-Eigenschaft ändern. 

Auf sehr kleinen Bildschirmen (Breite: 320 epx) passen maximal vier primäre Befehle in die Befehlsleiste. 

Sie können der **SecondaryCommands**-Sammlung auch Befehle hinzufügen, die im Überlaufmenü angezeigt werden.

![Beispiel für eine Befehlsleiste mit einem Bereich für weitere Optionen inklusive Symbolen](images/appbar_rs2_overflow_icons.png)

Befehle können programmgesteuert nach Bedarf zwischen „PrimaryCommands“ und „SecondaryCommands“ verschoben werden.

- *Ein Befehl, der einheitlich über mehrere Seiten hinweg angezeigt wird, sollte immer an der gleichen Stelle platziert werden.*
- *Es wird empfohlen, die Befehle „Akzeptieren“, „Ja“ und „OK“ links neben den Befehlen „Ablehnen“, „Nein“ und „Abbrechen“ zu platzieren. Konsistenz trägt dazu bei, dass die App Benutzern vertraut ist und sie ihre App-Navigationskenntnisse von einer App auf andere Apps übertragen können.*

### <a name="app-bar-buttons"></a>App-Leistenschaltflächen

„PrimaryCommands“ und „SecondaryCommands“ können nur mit Befehlselementen vom Typ [AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) und [AppBarSeparator](/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) gefüllt werden. 

Die Steuerelemente für die App-Leistenschaltfläche zeichnen sich durch ein Symbol und eine Textbeschriftung aus. Diese Steuerelemente sind für die Verwendung in Befehlsleisten optimiert, und ihr Erscheinungsbild verändert sich abhängig davon, ob das Steuerelement in der Befehlsleiste oder im Überlaufmenü verwendet wird.

#### <a name="icons"></a>Symbole

Die Größe der Symbole, wenn sie im primären Befehlsbereich angezeigt werden, beträgt 20 × 20 Pixel; im Überlaufmenü werden die Symbole mit 16 x 16 Pixel angezeigt. Wenn Sie [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon), [FontIcon](/uwp/api/windows.ui.xaml.controls.fonticon) oder [PathIcon](/uwp/api/windows.ui.xaml.controls.pathicon) verwenden, wird das Symbol automatisch und ohne Qualitätsverlust auf die richtige Größe skaliert, sobald der Befehl in den Bereich für sekundäre Befehle verschoben wird.

Weitere Informationen und Beispiele zum Festlegen des Symbols finden Sie in der Dokumentation für die [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)-Klasse.

#### <a name="labels"></a>Bezeichnungen

Mithilfe der „AppBarbutton“-Eigenschaft [IsCompact](/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) wird festgelegt, ob die Bezeichnung angezeigt wird. In einem „CommandBar“-Steuerelement überschreibt die Befehlsleiste automatisch die „IsCompact“-Eigenschaft der Schaltfläche, wenn die Befehlsleiste geöffnet und geschlossen wird.

Verwenden Sie zum Positionieren von App-Leistenschaltflächen die [DefaultLabelPosition](/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelposition)-Eigenschaft von „CommandBar“.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![Befehlsleiste mit Beschriftungen auf der rechten Seite](images/app-bar-labels-on-right.png)

In größeren Fenstern können Sie zur Verbesserung der Lesbarkeit Beschriftungen rechts neben die Symbole der App-Leistenschaltflächen verschieben. Bei Beschriftungen, die sich unten befinden, müssen Benutzer die Befehlsleiste öffnen, um die Beschriftungen anzuzeigen. Beschriftungen auf der rechten Seite werden auch angezeigt, wenn die Befehlsleiste geschlossen ist.

In Überlaufmenüs werden Beschriftungen standardmäßig rechts neben Symbolen positioniert, und **LabelPosition** wird ignoriert. Sie können das Format anpassen, indem Sie die [CommandBarOverflowPresenterStyle](/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle)-Eigenschaft auf einen „Style“-Wert für [CommandBarOverflowPresenter](/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter)festlegen. 

Schaltflächenbeschriftungen sollten kurz sein (vorzugsweise ein einzelnes Wort). Längere Beschriftungen unter einem Symbol werden auf mehrere Zeilen umbrochen, wodurch die geöffnete Befehlsleiste insgesamt höher wird. Sie können im Text für eine Beschriftung einen bedingten Trennstrich (0x00AD) einfügen, um die Stelle anzugeben, an der der Zeilenumbruch erfolgen soll. In XAML können Sie dies wie folgt mit einer Escapesequenz ausdrücken:

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

Wenn die Beschriftung an der angegebenen Stelle umgebrochen wird, sieht dies wie folgt aus:

![App-Leistenschaltfläche mit umgebrochener Beschriftung](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>Flyouts auf einer Befehlsleiste

Ziehen Sie logische Gruppierungen für die Befehle in Erwägung. Platzieren Sie z. B. die Befehle „Antworten“, „Allen antworten“ und „Weiterleiten“ im Menü „Antworten“. Mit einer App-Leistenschaltfläche wird zwar üblicherweise ein einzelner Befehl aktiviert, sie kann jedoch auch zum Anzeigen eines [MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout)- oder [Flyout](/uwp/api/windows.ui.xaml.controls.flyout)-Elements mit benutzerdefiniertem Inhalt verwendet werden.

![Beispiel für Flyouts auf einer Befehlsleiste](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>Andere Inhalte

Sie können dem Inhaltsbereich beliebige XAML-Elemente hinzufügen, indem Sie die **Content**-Eigenschaft festlegen. Wenn Sie mehrere Elemente hinzufügen möchten, müssen Sie sie in einem Panel-Container platzieren und das Panel als einziges untergeordnetes Element der Content-Eigenschaft festlegen.

Wenn der dynamische Überlauf aktiviert ist, wird der Inhalt nicht abgeschnitten, da die primären Befehle in das Überlaufmenü verschoben werden können. Andernfalls haben primäre Befehle Vorrang und können dazu führen, dass der Inhalt abgeschnitten wird.

Wenn [ClosedDisplayMode](/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode) auf **Compact** festgelegt wurde, kann der Inhalt abgeschnitten werden, wenn er größer als die kompakte Größe der Befehlsleiste ist. Behandeln Sie das [Opening](/uwp/api/windows.ui.xaml.controls.appbar.opening)-Ereignis und das [Closed](/uwp/api/windows.ui.xaml.controls.appbar.closed)-Ereignis, um Teile der Benutzeroberfläche im Inhaltsbereich anzuzeigen oder auszublenden, damit sie nicht beschnitten werden. Weitere Informationen finden Sie im Abschnitt [Geöffneter und geschlossener Zustand](#open-and-closed-states).


## <a name="open-and-closed-states"></a>Geöffneter und geschlossener Zustand

Die Befehlsleiste kann geöffnet oder geschlossen sein. Wenn sie geöffnet ist, zeigt sie primäre Befehlsschaltflächen mit Beschriftungen an und öffnet das Überlaufmenü (sofern es sekundäre Befehle gibt).
Die Befehlsleiste öffnet das Überlaufmenü nach oben (über die primären Befehle) oder nach unten (unter die primären Befehle). Die Standardrichtung ist nach oben, doch wenn der Platz zum Öffnen des Überlaufmenüs nach oben nicht ausreicht, öffnet die Befehlsleiste das Menü nach unten. 

Der Benutzer kann mit der Schaltfläche für weitere Optionen (\[•••\]) zwischen diesen Zuständen wechseln. Sie können programmgesteuert zwischen den Zuständen wechseln, indem Sie einen Wert für die [IsOpen](/uwp/api/windows.ui.xaml.controls.appbar.isopen)-Eigenschaft festlegen. 

Mit den Ereignissen [Opening](/uwp/api/windows.ui.xaml.controls.appbar.opening), [Opened](/uwp/api/windows.ui.xaml.controls.appbar.opened), [Closing](/uwp/api/windows.ui.xaml.controls.appbar.closing) und [Closed](/uwp/api/windows.ui.xaml.controls.appbar.closed) können Sie auf das Öffnen oder Schließen der Befehlsleiste reagieren.  
- Das Opening-Ereignis und das Closing-Ereignis treten vor Beginn der Übergangsanimation ein.
- Das Opened-Ereignis und das Closed-Ereignis treten nach Abschluss des Übergangs ein.

In diesem Beispiel wird mit dem Opening-Ereignis und dem Closing-Ereignis die Deckkraft der Befehlsleiste geändert. Im geschlossenen Zustand ist die Befehlsleiste halbtransparent, sodass der Hintergrund der App durchscheint. Wenn die Befehlsleiste geöffnet wird, wird sie undurchsichtig, damit der Benutzer sich auf die Befehle konzentrieren kann.

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### <a name="issticky"></a>IsSticky

Wenn ein Benutzer bei geöffneter Befehlsleiste mit anderen Teilen einer App interagiert, wird die Befehlsleiste automatisch geschlossen. Dies wird als *einfaches Ausblenden* bezeichnet. Sie können das Verhalten für einfaches Ausblenden steuern, indem Sie einen Wert für die [IsSticky](/uwp/api/windows.ui.xaml.controls.appbar.issticky)-Eigenschaft festlegen. Wenn sie `IsSticky="true"` lautet, bleibt die Leiste geöffnet, bis der Benutzer auf die Schaltfläche für weitere Optionen (\[•••\]) klickt oder ein Menüelement im Überlaufmenü auswählt. 

Es wird empfohlen, eingerastete Befehlsleisten zu vermeiden, da sie den Benutzererwartungen an das [Verhalten für einfaches Ausblenden und Tastaturfokus](./menus.md#light-dismiss) nicht entsprechen.

### <a name="display-mode"></a>Anzeigemodus

Sie können steuern, wie die Befehlsleiste im geschlossenen Zustand angezeigt wird, indem Sie die [ClosedDisplayMode](/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode)-Eigenschaft festlegen. Sie können aus drei Anzeigemodi für den geschlossenen Zustand auswählen:
- **Kompakt**: Der Standardmodus. Hiermit werden der Inhalt, primäre Befehlssymbole ohne Beschriftungen und die Schaltfläche für weitere Optionen (\[•••\]) angezeigt.
- **Minimal**: Hiermit wird nur eine dünne Leiste angezeigt, die als Schaltfläche für weitere Optionen (\[•••\]) fungiert. Der Benutzer kann auf eine beliebige Stelle auf der Leiste tippen, um sie zu öffnen.
- **Versteckt**: Die Befehlsleiste wird nicht angezeigt, wenn sie geschlossen ist. Dies kann hilfreich beim Anzeigen von Kontextbefehlen mit einer Inlinebefehlsleiste sein. In diesem Fall müssen Sie die Befehlsleiste programmgesteuert öffnen, indem Sie die **IsOpen**-Eigenschaft festlegen oder ClosedDisplayMode auf **Minimal** oder **Compact** festlegen.

Hier enthält eine Befehlsleiste einfache Formatierungsbefehle für eine [RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). Wenn das Bearbeitungsfeld nicht den Fokus besitzt, können die Formatierungsbefehle störend sein, daher werden sie ausgeblendet. Wenn das Bearbeitungsfeld verwendet wird, wird ClosedDisplayMode für die Befehlsleiste in „Compact“ geändert, sodass die Formatierungsbefehle angezeigt werden.

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**Hinweis**&nbsp;&nbsp;Die Implementierung von Bearbeitungsbefehlen geht über den Rahmen dieses Beispiels hinaus. Weitere Informationen finden Sie im Artikel zu [RichEditBox](rich-edit-box.md).

Auch wenn die Modi „Minimal“ und „Hidden“ in einigen Situationen nützlich sind, beachten Sie, dass es für die Benutzer verwirrend sein kann, wenn alle Aktionen ausgeblendet werden.

Das Ändern von ClosedDisplayMode, um mehr oder weniger Informationen für die Benutzer bereitzustellen, hat Auswirkungen auf das Layout der umgebenden Elemente. Das Wechseln zwischen dem geschlossenen und geöffneten Zustand von CommandBar hat hingegen keine Auswirkungen auf das Layout von anderen Elementen.

## <a name="placement"></a>Platzierung
Befehlsleisten können am oberen Rand des App-Fensters, am unteren Rand des App-Fensters und inline platziert werden, indem sie in ein Layoutsteuerelement wie ```Grid.row``` eingebettet werden.

![Beispiel 1 für die Platzierung der App-Leiste](images/AppbarGuidelines_Placement1.png)

-   Bei kleinen Handheld-Geräten empfiehlt es sich, Befehlsleisten am unteren Bildschirmrand zu platzieren, da sie dort besser erreichbar sind.
-   Bei Geräten mit größeren Bildschirmen hat es sich bewährt, Befehlsleisten im oberen Bereich des Fensters zu platzieren.

Die Größe des physischen Bildschirms können Sie mithilfe der [DiagonalSizeInInches](/uwp/api/windows.graphics.display.displayinformation.diagonalsizeininches)-API ermitteln.

Befehlsleisten können auf Bildschirmen mit einzelner Ansicht (linkes Beispiel) und auf Bildschirmen mit mehreren Ansichten (rechtes Beispiel) in folgenden Bildschirmbereichen platziert werden: Inlinebefehlsleisten können überall im Aktionsbereich platziert werden.

![Beispiel 2 für die Platzierung der App-Leiste](images/AppbarGuidelines_Placement2.png)

>**Toucheingabegeräte:** Wenn die Befehlsleiste für den Benutzer sichtbar bleiben muss, während die Bildschirmtastatur (oder ein Soft Input Panel, SIP) angezeigt wird, können Sie die Befehlsleiste der [BottomAppBar](/uwp/api/windows.ui.xaml.controls.page.bottomappbar)-Eigenschaft einer Seite zuweisen. Sie wird dann verschoben und bleibt sichtbar, wenn die Bildschirmtastatur eingeblendet ist. Andernfalls müssen Sie die Befehlsleiste inline relativ zum App-Inhalt platzieren.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für XAML-Befehle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>Verwandte Artikel

* [Grundlagen des Befehlsdesigns für Windows-Apps](../basics/commanding-basics.md)
* [CommandBar-Klasse](/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
