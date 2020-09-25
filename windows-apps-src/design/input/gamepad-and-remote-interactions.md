---
title: Interaktionen mit Gamepad und Fernbedienung
description: Hier finden Sie Anleitungen, Empfehlungen und Vorschläge zum Optimieren Ihrer APP für die Eingabe von Xbox Gamepad und Remote Steuerung.
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7f11cde619b783292e4880927c68b6ae8ff38323
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217184"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interaktionen mit Gamepad und Fernbedienung

![Bild Tastatur und Gamepad](images/keyboard/keyboard-gamepad.jpg)

***Viele Interaktions Umgebungen werden von Gamepad, Remote Steuerung und Tastatur gemeinsam genutzt.***

Erstellen Sie Interaktionen in Ihren Windows-Anwendungen, die sicherstellen, dass Ihre APP sowohl über herkömmliche Eingabetypen von PCs, Laptops und Tablets (Maus, Tastatur, Toucheingabe usw.) als auch über die Eingabetypen, die typisch für die TV-und Xbox *-10-Fuß-* Erfahrung sind, genutzt werden können, wie z. b. Gamepad und Remote Steuerung.

Einen allgemeinen Entwurfs Leit Faden für Windows *-* Anwendungen finden Sie unter [Entwerfen für Xbox und TV](../devices/designing-for-tv.md) .

## <a name="overview"></a>Übersicht

In diesem Thema wird erläutert, was Sie in Ihrem Interaktions Entwurf berücksichtigen sollten (oder was Sie nicht tun sollten, wenn die Plattform für Sie nachgeschlagen ist) und Anleitungen, Empfehlungen und Vorschläge zum Erstellen von Windows-Anwendungen bereitstellen, die unabhängig von Gerät, Eingabetyp oder Benutzer Funktionen und-Voreinstellungen verwendet werden können.

In der untersten Zeile sollte Ihre Anwendung so intuitiv und einfach in der *2-Fuß-* Umgebung verwendet werden können, wie Sie sich in der *10-Fuß-* Umgebung befindet (und umgekehrt). Unterstützen Sie die bevorzugten Geräte des Benutzers, legen Sie den Schwerpunkt auf die Benutzeroberfläche fest, und ordnen Sie Inhalte so an, dass die Navigation konsistent und vorhersagbar ist, und geben Sie den Benutzern den kürzesten Weg, was Sie möchten.

> [!NOTE]
> Die meisten Code Ausschnitte in diesem Thema befinden sich in XAML/c#. die Prinzipien und Konzepte gelten jedoch für alle Windows-apps. Wenn Sie eine HTML/JavaScript-Windows-App für Xbox entwickeln, sehen Sie sich die ausgezeichnete [tvhilfsprogramme](https://github.com/Microsoft/TVHelpers/wiki) -Bibliothek auf GitHub an.


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>Optimieren von 2-und 10-Fuß-Umgebungen

Es wird mindestens empfohlen, dass Sie Ihre Anwendungen testen, um sicherzustellen, dass Sie sowohl in zwei-Fuß-als auch in 10-Fuß-Szenarien ordnungsgemäß funktionieren, und dass sämtliche Funktionen für Xbox [Gamepad und Remote Steuerung](#gamepad-and-remote-control)auffindbar und zugänglich sind.

Im folgenden finden Sie einige weitere Möglichkeiten zum Optimieren Ihrer APP für die Verwendung mit zwei-und 10-Fuß-Umgebungen und mit allen Eingabegeräten (jeweils Links zum entsprechenden Abschnitt in diesem Thema).

> [!NOTE]
> Da Xbox Gamepads und Remote Steuerelemente viele Windows-Tastatur Verhalten und-Erfahrungen unterstützen, sind diese Empfehlungen für beide Eingabetypen geeignet. Ausführlichere Informationen zur Tastatur finden Sie unter [Tastatur Interaktionen](keyboard-interactions.md) .

| Funktion        | BESCHREIBUNG           |
| -------------------------------------------------------------- |--------------------------------|
| [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) | Die **XY-Fokus Navigation** ermöglicht dem Benutzer, um die Benutzeroberfläche Ihrer APP zu navigieren. Dies begrenzt Benutzer jedoch auf eine Navigation nach oben, unten, links und rechts. In diesem Abschnitt finden Sie Empfehlungen für den Umgang mit diesen und anderen Überlegungen. |
| [Mausmodus](#mouse-mode)|Die XY-Fokus Navigation ist für einige Arten von Anwendungen, z. b. Karten oder das Zeichnen und Zeichnen von apps, nicht praktikabel oder sogar möglich. In diesen Fällen ermöglicht der **Maus Modus** Benutzern, mit einem Gamepad oder einer Remote Steuerung wie der Maus auf einem PC frei zu navigieren.|
| [Fokusanzeige](#focus-visual)  | Das visuelle Fokus Element ist ein Rahmen, der das aktuell fokussierte Benutzeroberflächen Element hervorhebt. Dadurch kann der Benutzer schnell die Benutzeroberfläche identifizieren, durch die Sie navigieren oder mit der Sie interagieren.  |
| [Fokus Einbindung](#focus-engagement) | Der Fokus Engagement erfordert, dass der Benutzer die **A/Select-** Schaltfläche in einem Gamepad oder der Remote Steuerung drückt, wenn ein Benutzeroberflächen Element den Fokus besitzt, um damit zu interagieren. |
| [Hardwaretasten](#hardware-buttons) | Das Gamepad und die Remote Steuerung bieten sehr unterschiedliche Schaltflächen und Konfigurationen. |

## <a name="gamepad-and-remote-control"></a>Gamepad und Remotesteuerung

Gamepads und Remotesteuerungen sind die Haupteingabegeräte für die 10-Fuß-Erfahrung (vergleichbar Tastatur und Maus bei PCs und Toucheingabe bei Smartphones und Tablets).
In diesem Abschnitt werden die Hardwaretasten und ihre Funktionen vorgestellt.
In [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) und [Mausmodus](#mouse-mode) erfahren Sie, wie Sie Ihre App für die Verwendung dieser Eingabegeräte optimieren.

Die Qualität des Gamepad- und Fernbedienungsverhaltens, das Sie direkt erhalten, ist davon abhängig, wie gut die Tastatur in Ihrer App unterstützt wird. Eine gute Möglichkeit, um sicherzustellen, dass Ihre App gut mit Gamepads/Fernbedienungen funktioniert, besteht darin, sicherzustellen, dass sie gut mit PC-Tastaturen funktioniert, und sie anschließend mit Gamepads/Fernbedienungen zu testen, um die Schwachstellen in der Benutzeroberfläche zu finden.

### <a name="hardware-buttons"></a>Hardwaretasten

In diesem Dokument werden Schaltflächen mit den Namen bezeichnet, die im folgenden Diagramm angegeben werden.

![Diagramm für Gamepad- und Fernbedienungstasten](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Wie Sie im Diagramm erkennen können, werden einige Schaltflächen auf Gamepads unterstützt, die nicht auf Fernbedienungen unterstützt werden, und umgekehrt. Sie können zwar Tasten verwenden, die nur auf einer Art von Eingabegerät unterstützt werden, um die Navigation in der Benutzeroberfläche zu beschleunigen. Denken Sie jedoch daran, dass deren Verwendung für kritische Interaktionen zu Situationen führen kann, in den der Benutzer nicht mit bestimmten Teilen der Benutzeroberfläche interagieren kann.

In der folgenden Tabelle werden alle Hardware Schaltflächen aufgelistet, die von Windows-Apps unterstützt werden, und welches Eingabegerät diese unterstützt.

| Schaltfläche                    | Gamepad   | Remotesteuerung    |
|---------------------------|-----------|-------------------|
| A/Auswahl-Taste           | Ja       | Ja               |
| B/Zurück-Taste             | Ja       | Ja               |
| Steuerkreuz (D-Pad)   | Ja       | Ja               |
| Menü-Taste               | Ja       | Ja               |
| Ansicht-Taste               | Ja       | Ja               |
| X- und Y-Tasten           | Ja       | Nein                |
| Linker Stick                | Ja       | Nein                |
| Rechter Stick               | Ja       | Nein                |
| Linke und rechte Schalter   | Ja       | Nein                |
| Linke und rechte Bumper    | Ja       | Nein                |
| OneGuide-Taste           | Nein        | Ja               |
| Lautstärke-Taste             | Nein        | Ja               |
| Kanal-Taste            | Nein        | Ja               |
| Mediensteuerungs-Tasten     | Nein        | Ja               |
| Taste zum Stummschalten               | Nein        | Ja               |

### <a name="built-in-button-support"></a>Integrierte Tastenunterstützung

UWP ordnet das vorhandene Tastatureingabe Verhalten automatisch den Gamepad-und Remote Steuerungs Eingaben zu. Die folgende Tabelle zeigt diese integrierten Zuordnungen.

| Tastatur              | Gamepad/Fernbedienung                        |
|-----------------------|---------------------------------------|
| Pfeiltasten            | Steuerkreuz (D-Pad; auch linker Stick auf Gamepads)    |
| LEERTASTE              | A/Auswahl-Taste                       |
| EINGABETASTE                 | A/Auswahl-Taste                       |
| Escape                | B/Zurück-Taste*                        |

\*Wenn weder das [KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown) -Ereignis noch das [KeyUp](/uwp/api/windows.ui.xaml.uielement.keyup) -Ereignis für die B-Schaltfläche von der APP behandelt wird, wird das [systemnavigationmanager. backangeforderten](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) -Ereignis ausgelöst, was zu einer rückwärts Navigation innerhalb der APP führen sollte. Dieses Verhalten müssen Sie allerdings selbst implementieren, wie im folgenden Codeausschnitt gezeigt:

```csharp
// This code goes in the MainPage class

public MainPage()
{
    this.InitializeComponent();

    // Handling Page Back navigation behaviors
    SystemNavigationManager.GetForCurrentView().BackRequested +=
        SystemNavigationManager_BackRequested;
}

private void SystemNavigationManager_BackRequested(
    object sender,
    BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = this.BackRequested();
    }
}

public Frame AppFrame { get { return this.Frame; } }

private bool BackRequested()
{
    // Get a hold of the current frame so that we can inspect the app back stack
    if (this.AppFrame == null)
        return false;

    // Check to see if this is the top-most page on the app back stack
    if (this.AppFrame.CanGoBack)
    {
        // If not, set the event to handled and go back to the previous page in the
        // app.
        this.AppFrame.GoBack();
        return true;
    }
    return false;
}
```

> [!NOTE]
> Wenn die Schaltfläche "B" verwendet wird, um zurückzukehren, zeigen Sie in der Benutzeroberfläche keine Schaltfläche "zurück" an. Wenn Sie eine [Navigationsansicht](../controls-and-patterns/navigationview.md)verwenden, wird die Schaltfläche "zurück" automatisch ausgeblendet. Weitere Informationen zur rückwärts Navigation finden Sie unter [Navigationsverlauf und rückwärts Navigation für Windows-apps](../basics/navigation-history-and-backwards-navigation.md).

Windows-apps auf Xbox One unterstützen auch das Drücken der **Menü** Schaltfläche zum Öffnen von Kontextmenüs. Weitere Informationen finden Sie unter [CommandBar und ContextFlyout](#commandbar-and-contextflyout).

### <a name="accelerator-support"></a>Unterstützung von Beschleunigertasten

Beschleunigertasten sind Tasten, die für die schnellere Navigation in einer Benutzeroberfläche verwendet werden können. Diese Tasten sind jedoch möglicherweise für ein bestimmtes Eingabegerät spezifisch. Denken Sie daher daran, dass nicht alle Benutzer diese Funktionen verwenden können. Tatsächlich ist Gamepad derzeit das einzige Eingabegerät, das Zugriffstasten Funktionen für Windows-apps auf Xbox One unterstützt.

Die folgende Tabelle zeigt die in die UWP integrierte Beschleunigerunterstützung sowie die Unterstützung, die von Ihnen selbst implementiert werden kann. Nutzen Sie diese Verhaltensweisen in Ihrer benutzerdefinierten Benutzeroberfläche, um eine konsistente und benutzerfreundliche Benutzeroberfläche bereitzustellen.

| Interaktion   | Tastatur/Maus   | Gamepad      | Integriert für:  | Empfohlen für: |
|---------------|------------|--------------|----------------|------------------|
| Bild auf/Bild ab  | Bild auf/Bild ab | Linker/rechter Trigger | [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Ansichten, die den vertikalen Bildlauf unterstützen
| Seite nach links/rechts | Keine | Linker/rechter Bumper | [Pivot](/uwp/api/Windows.UI.Xaml.Controls.Pivot), [ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Ansichten, die den horizontalen Bildlauf unterstützen
| Vergrößern/Verkleinern        | STRG +/- | Linker/rechter Trigger | Keine | `ScrollViewer`, Ansichten, die das Vergrößern/Verkleinern unterstützen |
| Navigationsbereich öffnen/schließen | Keine | Sicht | Keine | Navigationsbereiche |
| Suchen, | Keine | Y-Taste | Keine | Verknüpfung mit der Hauptsuchfunktion in der App |
| [Kontextmenü öffnen](#commandbar-and-contextflyout) | Klicken Sie mit der rechten Maustaste auf | Menü-Taste | [ContextFlyout](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Kontextmenüs |

## <a name="xy-focus-navigation-and-interaction"></a>XY-Fokusnavigation und -interaktion

Wenn Ihre App die korrekte Fokusnavigation für Tastaturen unterstützt, wird dies gut auf Gamepads und Fernbedienungen übertragen.
Die Pfeiltastennavigation ist dem **Steuerkreuz** und dem **linken Stick** auf Gamepads zugeordnet. Interaktionen mit Benutzeroberflächenelementen sind der Taste **Eingabe/Auswahl** zugeordnet (siehe [Gamepad und Fernbedienung](#gamepad-and-remote-control)).

Viele Ereignisse und Eigenschaften werden sowohl von der Tastatur als auch vom Gamepad verwendet &mdash; `KeyDown` , sodass beide Ereignisse und Ereignisse ausgelöst werden `KeyUp` . beide werden nur zu Steuerelementen navigiert, die über die Eigenschaften `IsTabStop="True"` und verfügen `Visibility="Visible"` . Designanleitungen für die Tastaturnutzung finden Sie unter [Tastaturinteraktionen](../input/keyboard-interactions.md).

Wenn die Tastaturunterstützung ordnungsgemäß implementiert ist, wird Ihre App angemessen funktionieren. Es sind jedoch möglicherweise zusätzliche Arbeiten erforderlich, um jedes Szenario zu unterstützen. Bedenken Sie die spezifischen Anforderungen Ihrer App, um die bestmögliche Benutzererfahrung bereitzustellen.

> [!IMPORTANT]
> Der Mausmodus ist für Windows-apps, die auf Xbox One ausgeführt werden, standardmäßig aktiviert. Um den Mausmodus zu deaktivieren und die XY-Fokusnavigation zu aktivieren, legen Sie `Application.RequiresPointerMode=WhenRequested` fest.

### <a name="debugging-focus-issues"></a>Debuggen von Fokusproblemen

Die Methode [FocusManager.GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) informiert Sie darüber, welches Element gerade den Fokus besitzt. Dies ist in Situationen hilfreich, in denen die Fokusanzeige nicht direkt sichtbar ist. Mit dem folgenden Code können Sie die entsprechenden Informationen im Visual Studio-Ausgabefenster protokollieren:

```csharp
page.GotFocus += (object sender, RoutedEventArgs e) =>
{
    FrameworkElement focus = FocusManager.GetFocusedElement() as FrameworkElement;
    if (focus != null)
    {
        Debug.WriteLine("got focus: " + focus.Name + " (" +
            focus.GetType().ToString() + ")");
    }
};
```

Es gibt drei Hauptursachen für die fehlerhafte Funktion der XY-Navigation:

* Die [IsTabStop](/uwp/api/windows.ui.xaml.controls.control.istabstop)- oder [Visibility](/uwp/api/windows.ui.xaml.uielement.visibility)-Eigenschaft ist falsch festgelegt.
* Das Steuerelement, das den Fokus erhält, ist tatsächlich größer als Sie denken &mdash; , dass die XY-Navigation die Gesamtgröße des Steuer Elements ([ActualWidth](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) und [ActualHeight](/uwp/api/windows.ui.xaml.frameworkelement.actualheight)) prüft, nicht nur den Teil des Steuer Elements, der etwas interessantes rendert.
* Ein Fokussier bares Steuerelement, das auf einer anderen &mdash; XY-Navigation basiert, unterstützt keine Steuerelemente, die überlappen.

Wenn die XY-Navigation nach dem Beheben dieser drei Probleme noch immer nicht wie erwartet funktioniert, kann das Element, das den Fokus erhalten soll, mit der in [Überschreiben der Standardnavigation](#overriding-the-default-navigation) beschriebenen Methode manuell festgelegt werden.

Wenn die XY-Navigation wie beabsichtigt funktioniert, die Fokusanzeige jedoch nicht sichtbar ist, kann dies von einem der folgenden Probleme verursacht werden:

* Sie haben eine neue Vorlage auf das Steuerelement angewendet und keine Fokusanzeige einbezogen. Legen Sie die Eigenschaft `UseSystemFocusVisuals="True"` fest, oder fügen Sie manuell eine Fokusanzeige hinzu.
* Sie haben den Fokus über den Aufruf von `Focus(FocusState.Pointer)` verschoben. Der [FocusState](/uwp/api/Windows.UI.Xaml.FocusState)-Parameter steuert, was mit der Fokusanzeige geschieht. Im Allgemeinen sollten Sie den Parameter auf `FocusState.Programmatic` festlegen. So bleibt die Fokusanzeige sichtbar, wenn sie vorher sichtbar war, und wird ausgeblendet, wenn sie vorher ausgeblendet war.

Im Rest dieses Abschnitts werden allgemeine Designprobleme bei der XY-Navigation sowie verschiedene Lösungswege besprochen.

### <a name="inaccessible-ui"></a>Nicht zugängliche Benutzeroberfläche

Da die XY-Fokusnavigation den Benutzer auf die Navigation nach oben, unten, links und rechts einschränkt, gibt es möglicherweise Szenarien, in denen auf Teile der Benutzeroberfläche nicht zugegriffen werden kann.
Das folgende Diagramm zeigt ein Beispiel für die Art von Benutzeroberflächenlayout, das die XY-Fokusnavigation nicht unterstützt.
Beachten Sie, dass das Element in der Mitte bei Verwendung von Gamepads/Fernbedienungen nicht zugänglich ist, da die vertikale und horizontale Navigation Priorität erhält und die Priorität des mittleren Elements nie hoch genug sein wird, um den Fokus erhalten.

![Elemente in vier Ecken mit nicht zugänglichem Element in der Mitte](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

Wenn die Elemente auf der Benutzeroberfläche aus irgendeinem Grund nicht neu angeordnet werden können, verwenden Sie eine der im nächsten Abschnitt erläuterten Techniken, um das Standardfokusverhalten zu überschreiben.

### <a name="overriding-the-default-navigation"></a>Überschreiben der Standardnavigation

Die universelle Windows-Plattform versucht zwar, dem Benutzer eine sinnvolle Navigation mit dem Steuerkreuz (D-Pad)/linken Stick zu bieten, sie kann jedoch kein optimales Verhalten für die Ziele Ihrer App garantieren.
Die beste Möglichkeit, um sicherzustellen, dass die Navigation für Ihre App optimiert ist, besteht im Testen mit einem Gamepad. So können Sie überprüfen, ob Benutzer auf alle Benutzeroberflächenelemente auf eine Weise zugreifen können, die für die Szenarien Ihrer App sinnvoll ist. Wenn die Szenarien Ihrer App Verhaltensweisen erfordern, die durch die vorhandene XY-Fokusnavigation nicht bereitgestellt werden können, ziehen Sie die Empfehlungen in den folgenden Abschnitten in Betracht und/oder überschreiben das Verhalten, um den Fokus auf ein logisches Element zu setzen.

Der folgende Codeausschnitt zeigt, wie Sie das Verhalten der XY-Fokusnavigation überschreiben können:

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft"
            Content="Search" />
    <Button x:Name="MyBtnRight"
            Content="Delete"/>
    <Button x:Name="MyBtnTop"
            Content="Update" />
    <Button x:Name="MyBtnDown"
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}"
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

Wenn sich der Fokus auf der `Home`-Schaltfläche und der Benutzer nach links navigiert, wird der Fokus zur `MyBtnLeft`-Schaltfläche verschoben. Wenn der Benutzer nach rechts navigiert, wird der Fokus zur `MyBtnRight`-Schaltfläche verschoben usw.

Um zu verhindern, dass der Fokus von einem Steuerelement in eine bestimmten Richtung verschoben wird, verwenden Sie die `XYFocus*`-Eigenschaft, um auf das gleiche Steuerelement zu zeigen:

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

Mithilfe dieser `XYFocus`-Eigenschaften kann ein übergeordnetes Steuerelement auch die Navigation der untergeordneten Elemente erzwingen, wenn sich der nächste Fokuskandidat außerhalb der visuellen Struktur befindet – es sei denn, das untergeordnete Element, das den Fokus besitzt, verwendet die gleiche `XYFocus`-Eigenschaft.

```xml
<StackPanel Orientation="Horizontal" Margin="300,300">
    <UserControl XYFocusRight="{x:Bind ButtonThree}">
        <StackPanel>
            <Button Content="One"/>
            <Button Content="Two"/>
        </StackPanel>
    </UserControl>
    <StackPanel>
        <Button x:Name="ButtonThree" Content="Three"/>
        <Button Content="Four"/>
    </StackPanel>
</StackPanel>
```

Im obigen Beispiel gilt: Wenn `Button` 2 den Fokus besitzt und der Benutzer nach rechts navigiert, ist `Button` 4 der beste Fokuskandidat. Der Fokus wechselt jedoch zu `Button` 3, da das übergeordnete `UserControl`-Element dies erzwingt, wenn die visuelle Struktur verlassen wird.

### <a name="path-of-least-clicks"></a>Pfad der wenigsten Klicks

Ermöglichen Sie Benutzern, häufig ausgeführte Aktionen mit möglichst wenigen Klicks auszuführen. Im folgenden Beispiel befindet sich [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) zwischen der **Play**-Schaltfläche (die zunächst den Fokus erhält) und einem häufig verwendeten Element, sodass sich zwischen zwei Aktionen mit Priorität ein nicht notwendiges Element befindet.

![Bewährte Methoden für die Navigation stellen den Pfad der wenigsten Klicks bereit](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Im folgenden Beispiel befindet sich [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) stattdessen oberhalb der **Play**-Schaltfläche.
Durch das einfache Neuanordnen der Benutzeroberfläche, sodass sich nicht notwendige Elemente nicht zwischen Aktionen mit Priorität befinden, wird die Verwendbarkeit Ihrer App erheblich verbessert.

![TextBlock an eine Stelle oberhalb der Play-Schaltfläche verschoben, sodass sie sich nicht mehr zwischen Aktionen mit Priorität befindet](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar und ContextFlyout

Bei Verwendung einer [CommandBar](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) müssen Sie das Problem hinsichtlich Bildläufen durch Listen berücksichtigen, wie in [Problem: Benutzeroberflächenelemente nach langen bildlauffähigen Listen/Rastern](#problem-ui-elements-located-after-long-scrolling-list-grid) beschrieben. Die folgende Abbildung zeigt ein Benutzeroberflächenlayout mit `CommandBar` am Ende einer Liste/eines Rasters. Der Benutzer müsste einen Bildlauf ganz nach unten durch die Liste/das Rasters ausführen, um zu `CommandBar` zu gelangen.

![CommandBar am unteren Ende einer Liste/eines Rasters](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Was geschieht, wenn Sie  an einer Stelle `CommandBar` *oberhalb* der Liste/des Rasters platzieren? Auch wenn ein Benutzer, der einen Bildlauf nach unten durch die Liste/das Raster ausführt, einen Bildlauf zurück nach oben ausführen müsste, um zu `CommandBar` zu gelangen, bedeutet dies etwas weniger Navigationsaufwand als bei der vorherigen Konfiguration. Beachten Sie, dass dies voraussetzt, dass sich der anfängliche Fokus Ihrer App neben oder oberhalb von `CommandBar` befindet. Dieser Ansatz funktioniert weniger gut, wenn sich der anfängliche Fokus unterhalb der Liste/des Rasters befindet. Wenn diese `CommandBar`-Elemente globale Aktionselemente sind, auf die nicht sehr häufig zugegriffen werden muss (beispielsweise eine **Sync**-Schaltfläche), ist es möglicherweise zulässig, diese oberhalb der Liste/des Rasters zu platzieren.

Zwar ist das vertikale Stapeln der `CommandBar`-Elemente nicht möglich, die Platzierung gegen die Bildlaufrichtung (etwa links oder rechts von einer vertikal laufenden Liste oder über/unter einer horizontal laufenden Liste) ist eine weitere Option, die Sie nutzen können, wenn dies gut zu Ihrem Benutzeroberflächenlayout passt.

Wenn Ihre App eine `CommandBar` umfasst, auf deren Elemente die Benutzer zugreifen müssen, sollten Sie diese Elemente möglicherweise innerhalb einer [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout)-Eigenschaft platzieren und sie aus der `CommandBar` entfernen. `ContextFlyout` ist eine Eigenschaft von " [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) " und ist das [Kontextmenü](../controls-and-patterns/dialogs-and-flyouts/index.md) , das diesem Element zugeordnet ist. Wenn Sie auf einem PC mit der rechten Maustaste auf ein Element mit einem `ContextFlyout` klicken, wird das Kontextmenü eingeblendet. Auf Xbox One geschieht dies beim Drücken der **Menü**-Taste, während ein entsprechendes Element den Fokus hat.

### <a name="ui-layout-challenges"></a>Herausforderungen beim UI-Layout

Einige UI-Layouts sind aufgrund der Natur der XY-Fokusnavigation anspruchsvoller und sollten jeweils einzeln für sich beurteilt werden. Es gibt zwar keine einzige „richtige“ Lösung, und Ihre Entscheidung muss auf den Anforderungen Ihrer App basieren, es stehen jedoch einige Techniken zur Verfügung, die Ihnen dabei helfen können, ein hervorragendes TV-Erlebnis bereitzustellen.

Um dies zu verdeutlichen, sehen wir uns eine imaginäre App an, die einige dieser Herausforderungen sowie die entsprechenden Techniken zur Behebung illustriert.

> [!NOTE]
> Diese fiktive App dient dazu, UI-Probleme und mögliche Lösungen zu veranschaulichen. Die Benutzerfreundlichkeit wird dabei nicht berücksichtigt.

Nachfolgend sehen Sie eine fiktive Immobilien-App, die eine Liste zum Verkauf stehender Häuser, eine Karte, Beschreibungen von Immobilien sowie weitere Informationen anzeigt. Diese App stellt Sie vor drei Herausforderungen, denen Sie mit den folgenden Techniken begegnen können:

- [Neuanordnung der UI ](#ui-rearrange)
- [Fokus Einbindung](#engagement)
- [Mausmodus](#mouse-mode)

![Fiktive Immobilien-App](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### <a name="problem-ui-elements-located-after-long-scrolling-listgrid"></a>Problem: UI-Elemente, die sich hinter einer langen Bildlaufliste oder einem Raster befinden <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

Die [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) der Eigenschaften in der folgenden Abbildung ist eine sehr lange Bildlaufliste. Wenn [Engagement](#focus-engagement)*nicht* für die `ListView` erforderlich ist, wenn der Benutzer zu der Liste navigiert, wird der Fokus auf das erste Element in der Liste gesetzt. Damit der Benutzer zur Schaltfläche **Zurück** oder **Weiter** gelangen kann, muss er alle Elemente der Liste durchlaufen. In solchen Fällen, in denen der Benutzer die gesamte Liste durchlaufen muss, ist es mühsam &mdash; , wenn die Liste nicht so kurz genug ist, dass diese Benutzerfunktion akzeptabel ist, &mdash; sollten Sie auch andere Optionen in Erwägung gezogen.

![Immobilien-App: Eine Liste mit 50 Elementen erfordert 51 Klicks, bis die Schaltflächen am Ende erreicht sind](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Lösungen

**Neuanordnung der Benutzeroberfläche <a name="ui-rearrange"></a>**

Sofern nicht der anfängliche Fokus auf das Ende der Seite gesetzt ist, sind über einer langen Bildlaufliste platzierte UI-Elemente typischerweise einfacher erreichbar, als solche unterhalb einer solchen Liste.
Wenn dieses neue Layout für andere Geräte funktioniert, kann das Ändern des Layouts für alle Gerätefamilien kostengünstiger sein als das Vornehmen spezieller UI-Änderungen nur für die Xbox One.
Weiterhin gilt, dass die Platzierung von UI-Elementen gegen die Bildlaufrichtung (d.h. horizontal bei einer vertikal laufenden Liste oder vertikal bei einer horizontal laufenden Liste) den Benutzerkomfort noch weiter erhöht.

![Immobilien-App: Platzieren von Schaltflächen oberhalb einer langen Bildlaufliste](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Fokus Einbindung <a name="engagement"></a>**

Ist die Aktivierung *erforderlich*, wird die gesamte `ListView` zu einem einzigen Fokusziel. Der Benutzer kann die Inhalte der Liste übergehen, um zum nächsten fokussierbaren Element zu gelangen. Erfahren Sie mehr darüber, welche Steuerelemente die Aktivierung unterstützen, und wie Sie sie in der [Fokusaktivierung](#focus-engagement) verwenden können.

![Immobilien-App: Einstellen der Aktivierung als erforderlich, damit die Schaltflächen „Zurück/Weiter“ mit nur einem Klick erreicht werden können](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problem: ScrollViewer ohne fokussierbare Elemente

Da die XY-Fokusnavigation darauf basiert, dass jeweils zu einem fokussierbaren UI-Element navigiert wird, kann ein [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) ohne fokussierbare Elemente (etwa einer, der, wie der in diesem Beispiel, nur Text enthält) ein Szenario verursachen, in dem der Benutzer nicht alle Inhalte in dem `ScrollViewer` anzeigen kann.
Lösungen für dieses und ähnliche Szenarien finden Sie unter [Fokusaktivierung](#focus-engagement).

![Immobilien-App: ScrollViewer mit lediglich Text](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problem: UI mit freiem Bildlauf

Wenn Ihre App eine UI mit freiem Bildlauf benötigt, etwa eine Zeichenoberfläche oder, wie in diesem Beispiel, eine Karte, ist die XY-Fokus-Navigation ganz einfach nicht geeignet.
In solchen Fällen können Sie den [Mausmodus](#mouse-mode) aktivieren, damit der Benutzer in einem UI-Element frei navigieren kann.

![Karten-UI-Element im Mausmodus](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Mausmodus

Wie in [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) beschrieben, wird der Fokus auf Xbox One mittels eines XY-Navigationssystems verschoben, sodass Benutzer den Fokus von Steuerelement zu Steuerelement verschieben können, indem sie nach oben, unten, links und rechts navigieren.
Einige Steuerelemente wie [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) und [MapControl](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) erfordern jedoch eine mausähnliche Interaktion, bei der Benutzer mit dem Zeiger auf eine beliebige Stelle innerhalb der Grenzen des Steuerelements zeigen können.
Es gibt auch einige Apps, für die es sinnvoll ist, wenn Benutzer den Zeiger über die gesamte Seite bewegen können, sodass sie mit Gamepads/Fernbedienungen eine Erfahrung ähnlich wie am PC mit einer Maus haben.

Für diese Szenarien sollten Sie einen Zeiger (Mausmodus) für die gesamte Seite oder ein Steuerelement auf einer Seite anfordern.
Ihre App könnte beispielsweise eine Seite mit einem `WebView`-Steuerelement haben, das den Mausmodus nur innerhalb des Steuerelements und überall sonst eine XY-Fokusnavigation verwendet.
Um einen Zeiger anzufordern, können Sie angeben, ob er verwendet werden soll, **wenn ein Steuerelement oder eine Seite aktiviert ist** oder **wenn eine Seite den Fokus hat**.

> [!NOTE]
> Das Anfordern eines Zeigers, wenn ein Steuerelement den Fokus erhält, wird nicht unterstützt.

Bei XAML und bei gehosteten Web-Apps, die auf Xbox One ausgeführt werden, ist der Mausmodus standardmäßig für die gesamte App aktiviert. Es wird dringend empfohlen, den Mausmodus zu deaktivieren und die App für die XY-Navigation zu optimieren. Legen Sie dazu die `Application.RequiresPointerMode`-Eigenschaft auf `WhenRequested` fest. So können Sie den Mausmodus nur dann aktivieren, wenn ein Steuerelement oder eine Seite diesen erfordert.

Nutzen Sie in Ihrer XAML-App hierfür den folgenden Code in Ihrer `App`-Klasse:

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Weitere Informationen, einschließlich HTML/JavaScript-Beispielcode, finden Sie unter [So deaktivieren Sie den Mausmodus](../../xbox-apps/how-to-disable-mouse-mode.md).

Das folgende Diagramm zeigt die Tastenzuordnungen für Gamepads/Remotesteuerungen im Mausmodus.

![Tastenzuordnungen für Gamepads/Fernbedienungen im Mausmodus](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> Der Mausmodus wird nur auf Xbox One mit Gamepad/Fernbedienung unterstützt. Bei anderen Gerätefamilien und Eingabetypen wird er stillschweigend ignoriert.

Verwenden Sie die Eigenschaft [requirespointer](/uwp/api/windows.ui.xaml.controls.requirespointer) auf einem Steuerelement oder einer Seite, um den Maus Modus dafür zu aktivieren. Diese Eigenschaft hat drei mögliche Werte: `Never` (Standardwert), `WhenEngaged` und `WhenFocused` .

### <a name="activating-mouse-mode-on-a-control"></a>Aktivieren des Mausmodus für ein Steuerelement

Wenn der Benutzer ein Steuerelement mit `RequiresPointer="WhenEngaged"` verwendet, wird der Mausmodus für das Steuerelement aktiviert, bis der Benutzer es nicht mehr verwendet. Der folgende Codeausschnitt zeigt ein einfaches `MapControl`-Steuerelement, das den Mausmodus aktiviert, wenn es verwendet wird:

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> Wenn ein Steuerelement bei Verwendung den Mausmodus aktiviert, muss es auch über `IsEngagementRequired="true"` eine Interaktion erfordern. Andernfalls wird der Mausmodus nie aktiviert.

Wenn sich ein Steuerelement im Mausmodus befindet, befinden sich dessen verschachtelte Steuerelemente ebenfalls im Mausmodus. Der angeforderte Modus der untergeordneten Elemente wird ignoriert &mdash; . es ist nicht möglich, dass sich ein übergeordnetes Element im Mausmodus befindet, aber ein untergeordnetes Element ist.

Darüber hinaus wird der angeforderte Modus eines Steuerelements nur untersucht, wenn es den Fokus erhält. Daher kann der Modus nicht dynamisch geändert werden, während es den Fokus besitzt.

### <a name="activating-mouse-mode-on-a-page"></a>Aktivieren des Mausmodus auf einer Seite

Wenn eine Seite die Eigenschaft `RequiresPointer="WhenFocused"` besitzt, wird der Mausmodus für die gesamte Seite aktiviert, wenn sie den Fokus erhält. Der folgende Codeausschnitt zeigt, wie Sie einer Seite diese Eigenschaft zuweisen:

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> Der Wert `WhenFocused` wird nur für [Page](/uwp/api/Windows.UI.Xaml.Controls.Page)-Objekte unterstützt. Wenn Sie versuchen, diesen Wert für ein Steuerelement festzulegen, wird eine Ausnahme ausgelöst.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>Deaktivieren des Mausmodus für Vollbildinhalte

Beim Anzeigen von Videos oder anderen Arten von Vollbildinhalten empfiehlt es sich in der Regel, den Cursor auszublenden, um den Benutzer nicht abzulenken. Dieses Szenario liegt vor, wenn der Rest der App den Mausmodus verwendet, dieser aber beim Anzeigen von Vollbildinhalten deaktiviert werden soll. Platzieren Sie hierzu den Vollbildinhalt in einem eigenen `Page`-Objekt, und führen Sie die folgenden Schritte aus.

1. Legen Sie im `App`-Objekt Folgendes fest: `RequiresPointerMode="WhenRequested"`.
2. Legen Sie in jedem `Page`-Objekt (*mit Ausnahme des Vollbild-`Page`-Objekts*) Folgendes fest: `RequiresPointer="WhenFocused"`.
3. Legen Sie für das Vollbild-`Page`-Objekt Folgendes fest: `RequiresPointer="Never"`.

Dadurch wird der Cursor nicht angezeigt, wenn Inhalte im Vollbildmodus angezeigt werden.

## <a name="focus-visual"></a>Fokusanzeige

Die Fokusanzeige ist der Rahmen um das Benutzeroberflächenelement, das zurzeit den Fokus besitzt. Dies hilft Benutzern, sich zu orientieren, damit sie in Ihrer Benutzeroberfläche einfach navigieren können, ohne die Orientierung zu verlieren.

Dank der Hinzufügung einer Anzeigenaktualisierung und zahlreicher Anpassungsoptionen zur Fokusanzeige können Entwickler darauf vertrauen, dass eine einzelne Fokusanzeige gut auf PCs und Xbox One und allen anderen Windows 10-Geräten funktioniert, die Tastaturen und/oder Gamepads/Fernbedienungen unterstützen.

Während die gleiche Fokusanzeige auf verschiedenen Plattformen verwendet werden kann, unterscheidet sich der Kontext, in dem der Benutzer diese antrifft, für die 10-Fuß-Erfahrung etwas. Sie sollten davon ausgehen, dass der Benutzer nicht dem gesamten Fernsehbildschirm seine volle Aufmerksamkeit schenkt und es daher wichtig ist, dass das aktuell fokussierte Element für den Benutzer jederzeit deutlich erkennbar ist, um eine frustrierende Suche nach der Anzeige zu vermeiden.

Darüber hinaus ist es wichtig, zu beachten, dass die Fokusanzeige standardmäßig angezeigt wird, wenn ein Gamepad oder eine Fernbedienung verwendet wird, jedoch *nicht*, wenn eine Tastatur verwendet wird. Auch wenn Sie keine Fokusanzeige implementieren, wird sie daher angezeigt, wenn Sie Ihre App auf Xbox One ausführen.

### <a name="initial-focus-visual-placement"></a>Anfängliche Platzierung der Fokusanzeige

Platzieren Sie beim Starten einer App oder Navigieren zu einer Seite den Fokus auf einem Benutzeroberflächenelement, das sinnvollerweise als erstes Element betrachtet werden kann, für das Benutzer eine Aktion ausführen würden. Beispielsweise könnte eine Foto-App den Fokus auf das erste Element im Katalog platzieren, und eine Musik-App, in der zur detaillierten Ansicht eines Musiktitels navigiert wurde, könnte den Fokus auf die Abspieltaste setzen, um eine einfache Wiedergabe der Musik zu ermöglichen.

Platzieren Sie den anfänglichen Fokus in Ihrer App möglichst in den Bereich oben links (oder oben rechts bei einem Lesefluss von rechts nach links). Die meisten Benutzer neigen dazu, sich zunächst auf diesen Bereich zu konzentrieren, da der Inhaltsfluss einer App im Allgemeinen dort beginnt.

### <a name="making-focus-clearly-visible"></a>Klare Erkennbarkeit des Fokus

Eine Fokusanzeige sollte stets auf dem Bildschirm sichtbar sein, damit Benutzer Vorgänge an der Stelle fortsetzen können, an der sie aufgehört haben, ohne nach dem Fokus suchen zu müssen. Ebenso sollte immer ein Fokus verwendender Element auf dem Bildschirm vorhanden sein. verwenden Sie beispielsweise keine &mdash; Popups mit nur Text und keinen Fokus nutzbaren Elementen.

Eine Ausnahme von dieser Regel wären die Vollbild-Funktionen wie das Abspielen von Videos oder das Anzeigen von Bildern. In diesen Fällen sollte die Fokusanzeige nicht sichtbar sein.

### <a name="reveal-focus"></a>Reveal Focus

"Fokus anzeigen" ist ein Beleuchtungs Effekt, der den Rahmen von Fokus verwendbaren Elementen animiert, z. b. eine Schaltfläche, wenn der Benutzer den Gamepad-oder Tastaturfokus auf diese verschiebt. Wenn Sie den Glanz um den Rahmen der fokussierten Elemente animieren, können Sie mit dem Fokus den Benutzern besser verstehen, wo der Fokus ist und wo der Fokus geht.

Der Fokus ist standardmäßig auf OFF eingestellt. Für 10-Fuß-Umgebungen sollten Sie sich für die Offenlegung des Fokus entscheiden, indem Sie die [Application. focevisualkind-Eigenschaft](/uwp/api/windows.ui.xaml.application.FocusVisualKind) im App-Konstruktor festlegen.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Weitere Informationen finden Sie in der Anleitung zum [Offenlegen des Fokus](../style/reveal-focus.md).

### <a name="customizing-the-focus-visual"></a>Anpassen der Fokusanzeige

Wenn Sie die Fokusanzeige anpassen möchten, können Sie hierzu die Eigenschaften für die Fokusanzeige in den einzelnen Steuerelementen bearbeiten. Es gibt mehrere entsprechende Eigenschaften, über die Sie die App personalisieren können.

Sie können sogar die systemeigenen Fokusanzeigen deaktivieren und eigene Fokusanzeigen darstellen. Weitere Informationen finden Sie unter [VisualState](/uwp/api/Windows.UI.Xaml.VisualState).

### <a name="light-dismiss-overlay"></a>Overlay für einfaches Ausblenden

Um die Aufmerksamkeit des Benutzers auf die Benutzeroberflächen Elemente, die der Benutzer gerade bearbeitet, mit dem Spielcontroller oder der Remote Steuerung aufzurufen, fügt UWP automatisch eine "Rauch"-Ebene hinzu, die Bereiche außerhalb der Popup-Benutzeroberfläche abdeckt, wenn die APP auf Xbox One ausgeführt wird. Dies erfordert keinen zusätzlichen Aufwand. Sie sollten diese Funktionalität jedoch während der Entwicklung Ihrer Benutzeroberfläche berücksichtigen. Über die `LightDismissOverlayMode`-Eigenschaft können Sie die Ausblendschicht für jedes `FlyoutBase`-Element aktivieren oder deaktivieren. Der Standardwert `Auto` bedeutet, dass sie auf Xbox aktiviert und auf allen anderen Plattformen deaktiviert ist. Weitere Informationen finden Sie unter [Modales Ausblenden im Vergleich zu einfachem Ausblenden](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Fokusaktivierung

Die Fokusaktivierung soll die Verwendung eines Gamepads oder einer Fernbedienung für Interaktionen mit einer App vereinfachen.

> [!NOTE]
> Das Einrichten der Fokusaktivierung wirkt sich nicht auf Tastaturen oder andere Eingabegeräte aus.

Wenn die Eigenschaft `IsFocusEngagementEnabled` für ein [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement) -Objekt auf `True` festgelegt wird, zeigt dies an, dass das Steuerelement Fokusaktivierung erfordert. Das bedeutet, dass der Benutzer die **A/Select**-Taste (Auswahl-Taste) drücken muss, um das Steuerelement zu „aktivieren“ und mit diesem zu interagieren. Anschließend können sie die **B/Zurück**-Taste drücken, um das Steuerelement zu deaktivieren und von diesem weg zu navigieren.

> [!NOTE]
> `IsFocusEngagementEnabled` ist eine neue API, die noch nicht dokumentiert ist.

### <a name="focus-trapping"></a>Fokustrapping

Fokustrapping bedeutet, dass ein Benutzer versucht, in der Benutzeroberfläche einer App zu navigieren, jedoch innerhalb eines Steuerelements „gefangen“ ist, wodurch es schwer oder sogar unmöglich wird, von diesem Steuerelement weg zu navigieren.

Das folgende Beispiel zeigt eine Benutzeroberfläche, die ein Fokustrapping verursacht.

![Schaltflächen links und rechts neben einem horizontalen Schieberegler](images/designing-for-tv/focus-engagement-focus-trapping.png)

Wenn der Benutzer von der linken zur rechten Schaltfläche navigieren möchte, wäre es logisch, anzunehmen, dass er hierzu auf dem Steuerkreuz (D-Pad) oder linken Stick lediglich zweimal rechts drücken muss.
Wenn das [Slider](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Steuerelement jedoch keine Aktivierung erfordert, würde das folgende Verhalten eintreten; Wenn der Benutzer das erste Mal rechts drückt, würde der Fokus zum `Slider` verschoben. Wenn er das zweite Mal rechts drückt, würde der Ziehpunkt des `Slider` nach rechts verschoben. Der Benutzer würde den Ziehpunkt immer weiter nach rechts verschieben und könnte nicht zur Schaltfläche gelangen.

Es gibt verschiedene Methoden, um dieses Problem zu umgehen. Eine Möglichkeit besteht darin, ein anderes Layout ähnlich wie im Beispiel für eine Immobilien-App in [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) zu entwerfen. Dort wurden die Schaltflächen **Zurück** und **Weiter** oberhalb der `ListView` platziert. Eine vertikale anstelle einer horizontalen Anordnung der Steuerelemente wie in der folgenden Abbildung würde das Problem lösen.

![Schaltflächen ober- und unterhalb eines horizontalen Schiebereglers](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

Nun kann der Benutzer zu jedem der Steuerelemente navigieren, indem er auf dem Steuerkreuz (D-Pad/linken Stick) nach oben und nach unten drückt. Wenn der `Slider` den Fokus hat, kann er links und rechts drücken, um den Ziehpunkt des `Slider` zu verschieben, wie erwartet.

Ein anderer Ansatz zur Lösung dieses Problems besteht darin, für den `Slider` Aktivierung zu erfordern. Wenn Sie `IsFocusEngagementEnabled="True"` festlegen, führt dies zum folgenden Verhalten.

![Anfordern einer Fokusaktivierung für den Schieberegler, damit Benutzer zur Schaltfläche auf der rechten Seite navigieren können](images/designing-for-tv/focus-engagement-slider.png)

Wenn der `Slider` Fokusaktivierung erfordert, kann der Benutzer einfach zur Schaltfläche auf der rechten Seite gelangen, indem er auf dem Steuerkreuz (D-Pad)/dem linken Stick zweimal rechts drückt. Diese Lösung ist hervorragend geeignet, da sie keine Benutzeroberflächenanpassung erfordert und das erwartete Verhalten erzeugt.

### <a name="items-controls"></a>Elementsteuerelemente

Abgesehen vom [Slider](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Steuerelement gibt es weitere Steuerelemente, für die Sie möglicherweise eine Aktivierung anfordern sollten, wie beispielsweise:

- [ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView)

Im Gegensatz zum `Slider`-Steuerelement, fangen diese Steuerelemente den Fokus nicht innerhalb ihrer selbst. Sie können jedoch Probleme hinsichtlich der Verwendbarkeit verursachen, wenn sie große Mengen an Daten enthalten. Im Folgenden finden Sie ein Beispiel für eine `ListView`, die eine große Menge an Daten enthält.

![ListView mit einer großen Menge an Daten und Schaltflächen ober- und unterhalb](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

Versuchen wir, ähnlich wie im Beispiel für das `Slider`-Steuerelement mit einem Gamepad/einer Fernbedienung von der Schaltfläche oben zur Schaltfläche unten zu navigieren.
Zunächst hat die Schaltfläche oben den Fokus. Wenn wir auf dem Steuerkreuz (D-Pad)/dem Stick nach unten drücken, wird der Fokus auf das erste Element in der `ListView` („Element 1“) platziert.
Wenn der Benutzer erneut nach unten drückt, erhält das nächste Element in der Liste den Fokus, nicht die Schaltfläche unten.
Um zur Schaltfläche zu gelangen, muss der Benutzer zunächst durch jedes Element in der `ListView` navigieren.
Wenn die `ListView` eine große Menge an Daten enthält, kann dies umständlich sein und stellt keine optimale Benutzererfahrung dar.

Um dieses Problem zu lösen, legen Sie die Eigenschaft `IsFocusEngagementEnabled="True"` für `ListView` fest, um Aktivierung zu erfordern.
Dadurch können Benutzer die `ListView` schnell überspringen, indem sie einfach nach unten drücken. Sie können jedoch keinen Bildlauf durch die Liste ausführen oder ein Element aus der Liste auswählen, wenn sie diese nicht durch Drücken der **A/Auswahl**-Taste aktivieren, wenn die Liste den Fokus hat. Anschließend können sie die **B/Zurück**-Taste drücken, um die Liste zu deaktivieren.

![ListView mit erforderlicher Aktivierung](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

[ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) unterscheidet sich etwas von diesen Steuerelementen. Es weist spezifische Besonderheiten auf, die Sie berücksichtigen müssen. Wenn Sie ein `ScrollViewer`-Steuerelement mit fokussierbarem Inhalt besitzen, können Sie standardmäßig durch Navigieren zum `ScrollViewer`-Steuerelement durch dessen fokussierbare Elemente navigieren. Wie in einer `ListView`, müssen Sie einen Bildlauf über alle Elemente ausführen, um von `ScrollViewer` weg zu navigieren.

Wenn der `ScrollViewer` über *keinen* Fokus verwendbaren Inhalt verfügt &mdash; , z. b., wenn er nur Text enthält, &mdash; können Sie festlegen, `IsFocusEngagementEnabled="True"` sodass der Benutzer den `ScrollViewer` mithilfe der Schaltfläche **A/Select** einbinden kann. Nach der Aktivierung können sie mit dem **Steuerkreuz (D-Pad)/linken Stick** einen Bildlauf durch den Text ausführen und anschließend die **B/Back**-Taste (Zurück-Taste) drücken, um das Steuerelement zu deaktivieren, wenn sie den Vorgang abgeschlossen haben.

Ein anderer Ansatz wäre die Festlegung von `IsTabStop="True"` `ScrollViewer` , sodass der Benutzer das Steuerelement nicht einbinden muss, um &mdash; ihn einfach in den Fokus zu versetzen, und dann einen Bildlauf durchführen, indem er das **D-Pad/den linken Strich** verwendet, wenn keine Fokus baren Elemente innerhalb der vorhanden sind `ScrollViewer` .

### <a name="focus-engagement-defaults"></a>Standardeinstellungen in Bezug auf die Fokusaktivierung

Einige Steuerelemente führen häufig genug dazu, dass Benutzer in einem Steuerelement gefangen werden, um die Fokusaktivierung als Standardeinstellung zu rechtfertigen. Im Fall anderer Steuerelemente, für die die Fokusaktivierung standardmäßig deaktiviert ist, kann es nützlich sein, die Fokusaktivierung zu aktivieren. Die folgende Tabelle listet diese Steuerelemente und deren Standardverhalten in Bezug auf die Fokusaktivierung auf.

| Control               | Standardeinstellung in Bezug auf die Fokusaktivierung  |
|-----------------------|---------------------------|
| CalendarDatePicker    | Andererseits                        |
| FlipView              | Aus                       |
| GridView              | Aus                       |
| ListBox               | Aus                       |
| ListView              | Aus                       |
| ScrollViewer          | Aus                       |
| SemanticZoom          | Aus                       |
| Schieberegler                | Andererseits                        |

Alle anderen Windows-Steuerelemente führen zu keinem Verhalten oder visuellen Änderungen, wenn dies der Fall ist `IsFocusEngagementEnabled="True"` .

## <a name="summary"></a>Zusammenfassung

Sie können Windows-Anwendungen erstellen, die für ein bestimmtes Gerät oder eine bestimmte Umgebung optimiert sind, aber das universelle Windows-Plattform ermöglicht Ihnen auch, apps zu erstellen, die auf verschiedenen Geräten erfolgreich verwendet werden können, und zwar mit zwei-und 10-Fuß-Erfahrungen und unabhängig von der Eingabe-oder Benutzer Fähigkeit. Mithilfe der Empfehlungen in diesem Artikel können Sie sicherstellen, dass Ihre APP so gut wie Sie sowohl auf dem Fernsehgerät als auch auf einem PC ist.

## <a name="related-articles"></a>Verwandte Artikel

- [Entwerfen für Xbox und Fernsehgeräte](../devices/designing-for-tv.md)
- [Geräte Einführung für Windows-apps](index.md)
