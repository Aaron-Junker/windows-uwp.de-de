---
Description: Über Befehlsleisten können Benutzer komfortabel auf häufig verwendete Befehle in Ihrer App zugreifen.
title: Befehlsleiste
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 461d6d135838a5141e6606d4c77ce21972a45fe1
ms.sourcegitcommit: aeebfe35330aa471d22121957d9b510f6ebacbcf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58901648"
---
# <a name="command-bar"></a>Befehlsleiste

Über Befehlsleisten können Benutzer komfortabel auf häufig verwendete Befehle in Ihrer App zugreifen. Befehlsleisten ermöglichen den Zugriff auf App-Ebene oder seitenspezifische Befehle und können mit jedem Navigationsmuster verwendet werden.

> **Wichtige APIs:** [CommandBar-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), [AppBarButton Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx), [AppBarSeparator-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

![Beispiel für eine Befehlsleiste mit Symbolen](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Das CommandBar-Steuerelement ist ein allgemeines, flexibles und kleines Steuerelement, mit dem komplexe Inhalte wie Bilder oder Textblöcke sowie einfache Befehle wie [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)-, [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx)- und [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)-Steuerelemente angezeigt werden können.

> [!NOTE]
> XAML stellt das [AppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar)-Steuerelement und das [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar)-Steuerelement bereit. Sie sollten AppBar nur verwenden, wenn Sie eine universelle Windows 8-App aktualisieren, in der AppBar verwendet wird, und möglichst wenige Änderungen vornehmen möchten. Für neue Apps in Windows 10 sollten Sie stattdessen das CommandBar-Steuerelement verwenden. In diesem Dokument wird davon ausgegangen, dass Sie das CommandBar-Steuerelement verwenden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CommandBar">die App zu öffnen und CommandBar in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Eine erweiterte Befehlsleiste in der Microsoft Fotos-App:

![Befehlsleiste in der Microsoft Fotos-App](images/control-examples/command-bar-photos.png)

Eine Befehlsleiste im Outlook-Kalender auf einem Windows Phone:

![Befehlsleiste in der Outlook-Kalender-App](images/control-examples/command-bar-calendar-phone.png)

## <a name="anatomy"></a>Aufbau

Standardmäßig zeigt die Befehlsleiste eine Zeile mit Symbolschaltflächen und eine optionale "Weitere Informationen" Schaltfläche, die durch ein Auslassungszeichen dargestellt wird \[• • • anzuzeigen\]. Dies ist die Befehlsleiste, die mit dem weiter unten gezeigten Beispielcode erstellt wird. Hier ist der geschlossene, kompakte Zustand zu sehen.

![Geschlossene Befehlsleiste](images/command-bar-compact.png)

Die Befehlsleiste kann auch im geschlossenen, minimierten Zustand angezeigt werden, die wie folgt aussieht. Weitere Informationen finden Sie im Abschnitt [Geöffneter und geschlossener Zustand](#open-and-closed-states).

![Geschlossene Befehlsleiste](images/command-bar-minimal.png)

Dies ist die gleiche Befehlsleiste im geöffneten Zustand. Die Beschriftungen zeigen die wichtigsten Elemente des Steuerelements.

![Geschlossene Befehlsleiste](images/commandbar_anatomy_open.png)

Die Befehlsleiste ist in 4 Hauptbereiche unterteilt:
- Der Inhaltsbereich wird an der linken Seite der Leiste ausgerichtet. Er wird angezeigt, wenn die Eigenschaft [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) befüllt ist.
- Der Bereich für primäre Befehle wird an der rechten Seite der Leiste ausgerichtet. Er wird angezeigt, wenn die Eigenschaft [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) gefüllt ist.  
- Die "finden Sie weitere" \[• • • anzuzeigen\] Schaltfläche auf der rechten Seite der Leiste angezeigt wird. Drücken die "finden Sie weitere" \[• • • anzuzeigen\] Schaltfläche zeigt primären Befehl "Bezeichnungen" und das Überlaufmenü wird geöffnet, wenn sekundäre Befehle vorhanden sind. Die Schaltfläche wird nicht angezeigt, wenn keine primären oder sekundären Befehlsbezeichnungen vorhanden sind. Dieses Standardverhalten können Sie mit der [OverflowButtonVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx)-Eigenschaft ändern.
- Das Überlaufmenü wird nur angezeigt, wenn die Befehlsleiste geöffnet ist und die Eigenschaft [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) befüllt ist. Wenn der Platz begrenzt ist, werden primäre Befehle in den SecondaryCommands-Bereich verschoben. Dieses Standardverhalten können Sie mit der [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx)-Eigenschaft ändern.

Das Layout wird umgekehrt, wenn [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) auf **RightToLeft** festgelegt wurde.

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
Das CommandBar-Steuerelement verfügt über 3 Eigenschaften, die Sie verwenden können, um Befehle und Inhalt hinzuzufügen: [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx), [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx), und [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx).


### <a name="commands"></a>Befehle

Befehlsleistenelemente werden standardmäßig der **PrimaryCommands**-Sammlung hinzugefügt. Sie sollten Befehle nach ihrer Wichtigkeit hinzufügen, damit die wichtigsten Befehle immer sichtbar sind. Wenn die Breite der Befehlsleiste geändert wird, z. B. wenn Benutzer die Größe des App-Fensters ändern, werden primäre Befehle dynamisch zwischen der Befehlsleiste und dem Überlaufmenü an Haltepunkten verschoben. Dieses Standardverhalten können Sie mit der [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx)-Eigenschaft ändern. 

Auf sehr kleinen Bildschirmen (Breite: 320 epx) passen maximal vier primäre Befehle in die Befehlsleiste. 

Sie können der **SecondaryCommands**-Sammlung auch Befehle hinzufügen, die im Überlaufmenü angezeigt werden.

![Beispiel für eine Befehlsleiste mit einem Bereich für weitere Optionen inklusive Symbolen](images/appbar_rs2_overflow_icons.png)

Befehle können programmgesteuert nach Bedarf zwischen „PrimaryCommands“ und „SecondaryCommands“ verschoben werden.

- *Ein Befehl, der einheitlich über mehrere Seiten hinweg angezeigt wird, muss immer an der gleichen Stelle platziert werden.*
- *Es wird empfohlen, die Befehle „Akzeptieren“, „Ja“ und „OK“ links von den Befehlen „Ablehnen“, „Nein“ und „Abbrechen“ zu platzieren. Konsistenz trägt dazu bei, dass Benutzern die App vertraut ist und sie ihre App-Navigationskenntnisse von einer App auf andere Apps übertragen können.*

### <a name="app-bar-buttons"></a>App-Leistenschaltflächen

„PrimaryCommands“ und „SecondaryCommands“ können nur mit Befehlselementen vom Typ [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx), and [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) gefüllt werden. 

Die Steuerelemente für die App-Leistenschaltfläche zeichnen sich durch ein Symbol und eine Textbeschriftung aus. Diese Steuerelemente sind für die Verwendung in Befehlsleisten optimiert, und ihr Erscheinungsbild verändert sich abhängig davon, ob das Steuerelement in der Befehlsleiste oder im Überlaufmenü verwendet wird.

Die Symbole im Überlaufmenü sind 16 × 16 px groß und damit kleiner als die Symbole im Bereich für primäre Befehle (20 × 20 px). Wenn Sie „SymbolIcon“, „FontIcon“ oder „PathIcon“ verwenden, wird das Symbol automatisch und ohne Qualitätsverlust auf die richtige Größe skaliert, sobald der Befehl in den Bereich für sekundäre Befehle verschoben wird. 

### <a name="button-labels"></a>Schaltflächenbeschriftungen
Mithilfe der Appbarbutton- Eigenschaft [IsCompact](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) wird festgelegt, ob die Bezeichnung angezeigt wird. In einem CommandBar-Steuerelement überschreibt die Befehlsleiste automatisch die IsCompact-Eigenschaft der Schaltfläche, wenn die Befehlsleiste geöffnet und geschlossen wird.

Verwenden Sie zum Positionieren von App-Leistenschaltflächen die [DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx)-Eigenschaft von CommandBar.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![Befehlsleiste mit Beschriftungen auf der rechten Seite](images/app-bar-labels-on-right.png)

In größeren Fenstern können Sie zur Verbesserung der Lesbarkeit Beschriftungen rechts neben die Symbole der App-Leistenschaltflächen verschieben. Bei Beschriftungen, die sich unten befinden, müssen Benutzer die Befehlsleiste öffnen, um die Beschriftungen anzuzeigen. Beschriftungen auf der rechten Seite werden auch angezeigt, wenn die Befehlsleiste geschlossen ist.

In Überlaufmenüs werden Beschriftungen standardmäßig rechts von Symbolen positioniert und **LabelPosition** wird ignoriert. Sie können das Format anpassen, indem Sie in der Eigenschaft [CommandBarOverflowPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) ein Format für die Klasse [CommandBarOverflowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter) angeben. 

Schaltflächenbeschriftungen sollten kurz sein (vorzugsweise ein einzelnes Wort). Längere Beschriftungen unter einem Symbol werden auf mehrere Zeilen umgebrochen, wodurch die geöffnete Befehlsleiste insgesamt höher wird. Sie können im Text für eine Beschriftung einen bedingten Trennstrich (0x00AD) einfügen, um die Stelle anzugeben, an der der Zeilenumbruch erfolgen soll. In XAML können Sie dies wie folgt mit einer Escapesequenz ausdrücken:

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

Wenn die Beschriftung an der angegebenen Stelle umgebrochen wird, sieht dies wie folgt aus:

![App-Leistenschaltfläche mit umgebrochener Beschriftung](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>Flyouts auf einer Befehlsleiste

Ziehen Sie logische Gruppierungen für die Befehle in Erwägung. Platzieren Sie z. B. die Befehle „Antworten“, „Allen antworten“ und „Weiterleiten“ im Menü „Antworten“. Mit einer App-Leistenschaltfläche wird zwar üblicherweise ein einzelner Befehl aktiviert, sie kann jedoch auch zum Anzeigen eines [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx)- oder [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx)-Elements mit benutzerdefiniertem Inhalt verwendet werden.

![Beispiel für Flyouts auf einer Befehlsleiste](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>Andere Inhalte

Sie können dem Inhaltsbereich beliebige XAML-Elemente hinzufügen, indem Sie die **Content**-Eigenschaft festlegen. Wenn Sie mehrere Elemente hinzufügen möchten, müssen Sie sie in einem Panel-Container platzieren und das Panel als einziges untergeordnetes Element der Content-Eigenschaft festlegen.

Wenn der dynamische Überlauf aktiviert ist, wird der Inhalt nicht abgeschnitten, da die primären Befehle in das Überlaufmenü verschoben werden können. Andernfalls haben primäre Befehle Vorrang und können dazu führen, dass der Inhalt abgeschnitten wird.

Wenn [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) auf **Compact** festgelegt wurde, kann der Inhalt abgeschnitten werden, wenn er größer als die kompakte Größe der Befehlsleiste ist. Behandeln Sie das [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx)-Ereignis und das [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx)-Ereignis, um Teile der Benutzeroberfläche im Inhaltsbereich anzuzeigen oder auszublenden, damit sie nicht beschnitten werden. Weitere Informationen finden Sie im Abschnitt [Geöffneter und geschlossener Zustand](#open-and-closed-states).


## <a name="open-and-closed-states"></a>Geöffneter und geschlossener Zustand

Die Befehlsleiste kann geöffnet oder geschlossen sein. Wenn es geöffnet ist, werden die primären Schaltflächen mit Beschriftungen und (falls vorhanden sekundäre Befehle sind) geöffnet Überlaufmenü wird.
Die Befehlsleiste wird geöffnet, das Überlaufmenü immer (über die primären Befehle) oder nach unten (siehe unten die primären Befehle). Die standardrichtung ist nach oben, jedoch wird es nicht genügend Speicherplatz für das Überlaufmenü aufwärts zu öffnen, öffnet der Befehlsleiste sie nach unten. 

Benutzer kann zwischen diesen Zuständen durch Drücken von "finden Sie weitere" wechseln \[• • • anzuzeigen\] Schaltfläche. Sie können programmgesteuert zwischen den Zuständen wechseln, indem Sie einen Wert für die Eigenschaft [IsOpen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx) festlegen. 

Sie können die Ereignisse [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx), [Opened](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx), [Closing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) und [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) nutzen, um auf das Öffnen und Schließen der Befehlsleiste zu reagieren.  
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

Wenn ein Benutzer bei geöffneter Befehlsleiste mit anderen Teilen einer App interagiert, wird die Befehlsleiste automatisch geschlossen. Dies wird als *einfaches Ausblenden* bezeichnet. Sie können das Verhalten für einfaches Ausblenden steuern, indem Sie einen Wert für die Eigenschaft [IsSticky](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx) festlegen. Wenn `IsSticky="true"`, die Leiste bleibt geöffnet, bis der Benutzer "finden Sie weitere" drückt \[• • • anzuzeigen\] Schaltfläche oder ein Element aus das Überlaufmenü ausgewählt. 

Es wird empfohlen, persistente Befehlsleisten vermeiden, da sie nicht den Erwartungen für Benutzer entsprechen [Licht zu verwerfen und Fokusverhalten der Tastatur](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#light-dismiss).

### <a name="display-mode"></a>Anzeigemodus

Sie können steuern, wie die Befehlsleiste im geschlossenen Zustand angezeigt wird, indem Sie einen Wert für die Eigenschaft [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) festlegen. Sie können aus drei Anzeigemodi für den geschlossenen Zustand auswählen:
- **Compact**: Der Standardmodus. Zeigt die Inhalte, primäre Befehlssymbole ohne Bezeichnung und der "finden Sie unter" Weitere \[• • • anzuzeigen\] Schaltfläche.
- **Minimal**: Zeigt nur eine dünne Leiste, als die "Weitere Informationen fungiert" \[• • • anzuzeigen\] Schaltfläche. Der Benutzer kann auf eine beliebige Stelle auf der Leiste tippen, um sie zu öffnen.
- **Versteckt**: Die Befehlsleiste wird nicht angezeigt, wenn es geschlossen wird. Dies kann hilfreich beim Anzeigen von Kontextbefehlen mit einer Inlinebefehlsleiste sein. In diesem Fall müssen Sie die Befehlsleiste programmgesteuert öffnen, indem Sie die **IsOpen**-Eigenschaft festlegen oder ClosedDisplayMode auf **Minimal** oder **Compact** festlegen.

Hier enthält eine Befehlsleiste einfache Formatierungsbefehle für eine [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx). Wenn das Bearbeitungsfeld nicht den Fokus besitzt, können die Formatierungsbefehle störend sein, daher werden sie ausgeblendet. Wenn das Bearbeitungsfeld verwendet wird, wird ClosedDisplayMode für die Befehlsleiste in „Compact“ geändert, sodass die Formatierungsbefehle angezeigt werden.

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
Befehlsleisten können am oberen Rand des App-Fensters, am unteren Rand des App-Fensters und inline platziert werden.

![Beispiel 1 für die Platzierung der App-Leiste](images/AppbarGuidelines_Placement1.png)

-   Bei kleinen Handheld-Geräten empfiehlt es sich, Befehlsleisten am unteren Bildschirmrand zu platzieren, da sie dort besser erreichbar sind.
-   Bei Geräten mit größeren Bildschirmen hat es sich bewährt, Befehlsleisten im oberen Bereich des Fensters zu platzieren.

Zur Ermittlung der Größe des physischen Bildschirms können Sie die API [DiagonalSizeInInches](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) verwenden.

Befehlsleisten können auf Bildschirmen mit einzelner Ansicht (linkes Beispiel) und auf Bildschirmen mit mehreren Ansichten (rechtes Beispiel) in folgenden Bildschirmbereichen platziert werden: Inlinebefehlsleisten können überall im Aktionsbereich platziert werden.

![Beispiel 2 für die Platzierung der App-Leiste](images/AppbarGuidelines_Placement2.png)

>**Touch-Geräte**: Wenn die Befehlsleiste sichtbar ist, zu einem Benutzer bleiben muss, wenn die Bildschirmtastatur oder Soft Input Panel (SIP) angezeigt wird, und klicken Sie dann Sie auf die Befehlsleiste zuweisen können die ["bottomappbar"](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) Eigenschaft einer Seite, und es werden verschoben, sichtbar bleiben, wenn das SIP vorhanden ist. . Andernfalls müssen Sie die Befehlsleiste inline relativ zum App-Inhalt platzieren.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für XAML-Befehle](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Verwandte Artikel

* [Befehlsdesigngrundlagen für UWP-Apps](../basics/commanding-basics.md)
* [CommandBar-Klasse](https://msdn.microsoft.com/library/windows/apps/dn279427)
