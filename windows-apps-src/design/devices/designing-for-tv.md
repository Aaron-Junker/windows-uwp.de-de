---
Description: Design your app so that it looks good and functions well on your television.
title: Entwerfen für Xbox und Fernsehgeräte
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, 10-Fuß-Erfahrung, Gamepad, Fernbedienung, Eingabe, Interaktion
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7b09e90ea71872421a0caf1edcc1a03ce25fef42
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "8209359"
---
# <a name="designing-for-xbox-and-tv"></a>Entwerfen für Xbox und Fernsehgeräte

Entwerfen Sie Ihre App für die Universelle Windows-Plattform (UWP) so, dass sie auf Xbox One- und Fernsehbildschirmen gut aussieht und optimal funktioniert.

## <a name="overview"></a>Übersicht

Dank der universellen Windows-Plattform können Sie großartige Benutzeroberflächen für verschiedene Windows10-Geräte erstellen.
Die Mehrzahl der durch das UWP Framework bereitgestellten Funktionen ermöglichen Apps, auf diesen Geräten ohne zusätzlichen Aufwand die gleiche Benutzeroberfläche (UI) zu verwenden.
Das Anpassen und Optimieren Ihrer App an Xbox One- und Fernsehbildschirme erfordert jedoch besondere Überlegungen.

Die Erfahrung, die Sie machen, wenn Sie auf dem Sofa sitzen und mittels eines Gamepads oder einer Fernbedienung mit Ihrem Fernsehgerät interagieren, wird als **3-Meter-Erfahrung** (10-Fuß-Erfahrung) bezeichnet.
Der Name kommt daher, dass sich der Benutzer im Allgemeinen ungefähr 3Meter (10Fuß) vom Bildschirm entfernt befindet.
Dies stellt eine besondere Herausforderung dar, die beispielsweise bei einer *50-cm-Erfahrung* (2-Fuß-Erfahrung) oder bei der Interaktion mit einem PC nicht vorhanden ist.
Wenn Sie eine App für Xbox One oder ein anderes Gerät entwickeln, das Inhalte auf einem Fernsehbildschirm ausgibt und eine Steuerung verwendet, sollten Sie dies stets bedenken.

Nicht alle Schritte in diesem Artikel sind erforderlich, damit Ihre App gut in 10 Fuß-Umgebungen funktioniert. Wenn Sie diese jedoch kennen und für Ihre App die entsprechenden Entscheidungen treffen, führt dies zu einer besseren 10-Fuß-Erfahrung, die an die spezifischen Anforderungen Ihrer App angepasst ist.
Berücksichtigen Sie die folgenden Designrichtlinien, wenn Sie eine App für eine 10-Fuß-Umgebung entwickeln.

### <a name="simple"></a>Einfach

Ein Entwurf für eine 10-Fuß-Umgebung bedeutet einen einzigartigen Satz von Herausforderungen. Auflösung und Anzeigeabstand kann es Menschen schwer machen, zu viele Informationen zu verarbeiten.
Versuchen Sie, Ihr Design klar zu halten und auf die einfachsten Komponenten zu reduzieren, die möglich sind. Die Menge der auf einem Fernseher angezeigten Informationen sollte mit der vergleichbar sein, die Ihnen auf einem Mobiltelefon angezeigt werden, nicht auf einem Desktop.

![Xbox One-Startseite](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>Einheitlich

UWP-Apps in 10-Fuß-Umgebungen sollten intuitiv und benutzerfreundlich sein. Sorgen Sie für einen klaren und unverkennbaren Fokus.
Ordnen Sie Inhalte so an, dass Verschiebungen auf dem Bildschirm konsistent und vorhersagbar sind. Stellen Sie Menschen den kürzesten Weg zu dem bereit, was sie tun möchten.

![Xbox One-Film-App](images/designing-for-tv/xbox-movies-app.png)

_**Alle im Screenshot gezeigten Filme sind auf Microsoft Filme & TV verfügbar.**_  

### <a name="captivating"></a>Fesselnd

Der große Bildschirm bietet äußerst faszinierende Erfahrungen, ähnlich wie ein Kino. Rand-Rand-Design, elegante Bewegungen und brillante Farben und Typografien eröffnen Ihren Apps neue Dimensionen. Seien Sie mutig, und bieten Sie ein ansprechendes Design.

![Xbox One Avatar-App](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>Optimierungen für die 10-Fuß-Erfahrung

Da Sie nun mit den Grundsätzen eines guten UWP-App-Designs für die 10-Fuß-Erfahrung vertraut sind, lesen Sie die folgenden Übersicht über die verschiedenen Arten, wie Sie Ihre App optimieren und eine hervorragende Benutzerumgebung bereitstellen können.

| Feature        | Beschreibung           |
| -------------------------------------------------------------- |--------------------------------|
| [Gamepad und Fernbedienung](#gamepad-and-remote-control)      | Der wichtigste Schritt bei der Optimierung für 10-Fuß-Umgebungen besteht darin, sicherzustellen, dass Ihre App gut mit Gamepads und Fernbedienungen funktioniert. Es gibt spezifische Verbesserungen für Gamepads und Fernbedienungen, mit denen Sie die Interaktionserfahrungen von Benutzern auf Geräten optimieren können, auf denen ihre Aktionen eher eingeschränkt sind. |
| [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) | Die UWP stellt eine **XY Fokusnavigation** bereit, mit deren Hilfe Benutzer in der Benutzeroberfläche Ihrer App navigieren können. Dies begrenzt Benutzer jedoch auf eine Navigation nach oben, unten, links und rechts. In diesem Abschnitt finden Sie Empfehlungen für den Umgang mit diesen und anderen Überlegungen. |
| [Mausmodus](#mouse-mode)|In einigen Benutzeroberflächen, beispielsweise Karten und Zeichenoberflächen, ist es nicht möglich oder praktisch, eine XY-Fokusnavigation zu verwenden. Für diese Schnittstellen stellt die UWP den **Mausmodus** bereit, damit das Gamepad/die Fernbedienung ähnlich einer Maus auf einem Desktopcomputer frei navigieren kann.|
| [Fokusanzeige](#focus-visual)  | Die Fokusanzeige ist der Rahmen um das Benutzeroberflächenelement, das zurzeit den Fokus besitzt. Dies hilft Benutzern, sich zu orientieren, damit sie in Ihrer Benutzeroberfläche einfach navigieren können, ohne die Orientierung zu verlieren. Wenn der Fokus nicht klar erkennbar ist, könnten Benutzer in der Benutzeroberfläche die Orientierung verlieren und keine gute Erfahrung mit Ihrer App machen.  |
| [Fokusaktivierung](#focus-engagement) | Das Einrichten der Fokusaktivierung für ein Benutzeroberflächenelement erfordert, dass der Benutzer die **A/Auswahl**-Taste drückt, um mit diesem zu interagieren. Dies kann helfen, eine bessere Erfahrung für den Benutzer beim Navigieren in der Benutzeroberfläche der App zu ermöglichen.
| [Anpassen von Benutzeroberflächenelementen](#ui-element-sizing)  | Die Universelle Windows-Plattform verwendet [Skalierung und effektive Pixel](../basics/design-and-ui-intro.md#effective-pixels-and-scaling), um die Benutzeroberfläche gemäß dem Anzeigeabstand zu skalieren. Wenn Sie verstehen, wie Sie Größen anpassen und auf Ihre Benutzeroberfläche anwenden, hilft Ihnen dies, Ihre App für die 10-Fuß-Umgebung zu optimieren.  |
|  [Fernsehsicherer Bereich](#tv-safe-area) | Die UWP vermeidet automatisch und standardmäßig die Anzeige von Benutzeroberflächenelementen in nicht fernsehsicheren Bereichen (nahe dem Bildschirmrand). Dies führt jedoch zu einem „Schachteleffekt“, so dass die Benutzeroberfläche einem Briefkastenschlitz ähnelt. Damit Ihre App auf Fernsehgeräten wirklich immersiv ist, müssen Sie diese so anpassen, dass sie sich auf Fernsehgeräten, die dies unterstützen, bis zu den Rändern erweitert wird. |
| [Farben](#colors)  |  Die UWP unterstützt Farbdesigns. Daher wird eine App, die das Systemdesign berücksichtigt, auf Xbox One standardmäßig auf **dark** festgelegt. Wenn Ihre App ein bestimmtes Farbdesign verwendet, sollten Sie daran denken, dass sich einige Farben nicht gut für Fernsehbildschirme eignen und daher vermieden werden sollten. |
| [Sound](../style/sound.md)    | Sounds spielen bei der 10 Fuß-Erfahrung eine wichtige Rolle. Sie helfen den Benutzern sich zu vertiefen und liefern Feedback. Die UWP bietet Funktionen, mit denen Sounds für allgemeine Steuerelemente automatisch aktiviert werden, wenn die App auf Xbox One ausgeführt wird. Erfahren Sie mehr über die in der Universellen Windows-Plattform integrierte Unterstützung von Sound, und erfahren Sie, wie Sie davon profitieren.    |
| [Richtlinien für Benutzeroberflächensteuerelemente](#guidelines-for-ui-controls)  |  Es gibt mehrere Benutzeroberflächen-Steuerelemente, die auf mehreren Geräten gut funktionieren. Wenn diese jedoch auf Fernsehgeräten verwendet werden, müssen bestimmte Aspekte berücksichtigt werden. Informieren Sie sich über einige bewährte Methoden für die Verwendung dieser Steuerelemente beim Entwerfen für die 10 Fuß-Erfahrung. |
| [Benutzerdefinierter visueller Zustandsauslöser für Xbox](#custom-visual-state-trigger-for-xbox) | Um Ihre UWP-App an die 10-Fuß-Erfahrung anzupassen, empfehlen wir Ihnen, einen benutzerdefinierten *visuellen Zustandsauslöser* zu verwenden, um das Layout zu ändern, wenn die App erkennt, dass sie auf einer Xbox-Konsole gestartet wurde.

> [!NOTE]
> Die meisten Codeausschnitte in diesem Thema wurden in XAML/C# verfasst. Die Grundsätze und Konzepte gelten jedoch für alle UWP-Apps. Wenn Sie eine HTML/JavaScript-UWP-App für Xbox entwickeln, steht Ihnen die hervorragende [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki)-Bibliothek auf GitHub zur Verfügung.

## <a name="gamepad-and-remote-control"></a>Gamepad und Remotesteuerung

Gamepads und Remotesteuerungen sind die Haupteingabegeräte für die 10-Fuß-Erfahrung (vergleichbar Tastatur und Maus bei PCs und Toucheingabe bei Smartphones und Tablets).
In diesem Abschnitt werden die Hardwaretasten und ihre Funktionen vorgestellt.
In [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) und [Mausmodus](#mouse-mode) erfahren Sie, wie Sie Ihre App für die Verwendung dieser Eingabegeräte optimieren.

Die Qualität des Gamepad- und Fernbedienungsverhaltens, das Sie direkt erhalten, ist davon abhängig, wie gut die Tastatur in Ihrer App unterstützt wird. Eine gute Möglichkeit, um sicherzustellen, dass Ihre App gut mit Gamepads/Fernbedienungen funktioniert, besteht darin, sicherzustellen, dass sie gut mit PC-Tastaturen funktioniert, und sie anschließend mit Gamepads/Fernbedienungen zu testen, um die Schwachstellen in der Benutzeroberfläche zu finden.

### <a name="hardware-buttons"></a>Hardwaretasten

In diesem Dokument werden Schaltflächen mit den Namen bezeichnet, die im folgenden Diagramm angegeben werden.

![Diagramm für Gamepad- und Fernbedienungstasten](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Wie Sie im Diagramm erkennen können, werden einige Schaltflächen auf Gamepads unterstützt, die nicht auf Fernbedienungen unterstützt werden, und umgekehrt. Sie können zwar Tasten verwenden, die nur auf einer Art von Eingabegerät unterstützt werden, um die Navigation in der Benutzeroberfläche zu beschleunigen. Denken Sie jedoch daran, dass deren Verwendung für kritische Interaktionen zu Situationen führen kann, in den der Benutzer nicht mit bestimmten Teilen der Benutzeroberfläche interagieren kann.

Die folgende Tabelle enthält eine Liste aller Hardwaretasten, die von UWP-Apps unterstützt werden, sowie Angaben dazu, welche Eingabegeräte diese unterstützen.

| Taste                    | Gamepad   | Fernbedienung    |
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
| Stumm-Taste               | Nein        | Ja               |

### <a name="built-in-button-support"></a>Integrierte Tastenunterstützung

Die UWP ordnet automatisch das vorhandene Tastatureingabeverhalten den Gamepad- und Fernbedienungseingaben zu. Die folgende Tabelle zeigt diese integrierten Zuordnungen.

| Tastatur              | Gamepad/Fernbedienung                        |
|-----------------------|---------------------------------------|
| Pfeiltasten            | Steuerkreuz (D-Pad; auch linker Stick auf Gamepads)    |
| Leertaste              | A/Auswahl-Taste                       |
| Eingabe                 | A/Auswahl-Taste                       |
| Escape                | B/Zurück-Taste*                        |

\*Wenn für die B-Taste weder das [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941.aspx)- noch das [KeyUp](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx)-Ereignis von der App behandelt werden, wird das [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx)-Ereignis ausgelöst, das zu einer Rückwärtsnavigation innerhalb der App führen sollte. Dieses Verhalten müssen Sie allerdings selbst implementieren, wie im folgenden Codeausschnitt gezeigt:

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
> Wenn die Taste B verwendet wird, um zurück zu gehen, dann zeigen Sie keine Zurück-Schaltfläche in der Benutzeroberfläche an. Wenn Sie eine [Navigationsansicht](../controls-and-patterns/navigationview.md) verwenden, wird die Schaltfläche Zurück automatisch ausgeblendet. Weitere Informationen zur Rückwärtsnavigation finden Sie unter [Navigationsverlauf und Rückwärtsnavigation für UWP-Apps](../basics/navigation-history-and-backwards-navigation.md).

UWP-Apps auf Xbox One unterstützen außerdem das Öffnen des Kontextmenüs durch Drücken der **Menü**-Taste. Weitere Informationen finden Sie unter [CommandBar und ContextFlyout](#commandbar-and-contextflyout).

### <a name="accelerator-support"></a>Unterstützung von Beschleunigertasten

Beschleunigertasten sind Tasten, die für die schnellere Navigation in einer Benutzeroberfläche verwendet werden können. Diese Tasten sind jedoch möglicherweise für ein bestimmtes Eingabegerät spezifisch. Denken Sie daher daran, dass nicht alle Benutzer diese Funktionen verwenden können. Tatsächlich sind zurzeit Gamepads die einzigen Eingabegeräte, die Beschleunigerfunktionen für UWP-Apps auf Xbox One unterstützen.

Die folgende Tabelle zeigt die in die UWP integrierte Beschleunigerunterstützung sowie die Unterstützung, die von Ihnen selbst implementiert werden kann. Nutzen Sie diese Verhaltensweisen in Ihrer benutzerdefinierten Benutzeroberfläche, um eine konsistente und benutzerfreundliche Benutzeroberfläche bereitzustellen.

| Interaktion   | Tastatur/Maus   | Gamepad      | Integriert für:  | Empfohlen für: |
|---------------|------------|--------------|----------------|------------------|
| Bild auf/Bild ab  | Bild auf/Bild ab | Linker/rechter Trigger | [CalendarView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.calendarview.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [ComboBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Ansichten, die den vertikalen Bildlauf unterstützen
| Seite nach links/rechts | Keine | Linker/rechter Bumper | [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Ansichten, die den horizontalen Bildlauf unterstützen
| Vergrößern/Verkleinern        | STRG +/- | Linker/rechter Trigger | Keine | `ScrollViewer`, Ansichten, die das Vergrößern/Verkleinern unterstützen |
| Navigationsbereich öffnen/schließen | Keine | Ansicht | Keine | Navigationsbereiche |
| [Suche](#search-experience) | Keine | Y-Taste | Keine | Verknüpfung mit der Hauptsuchfunktion in der App |
| [Öffnen Sie das Kontextmenü](#commandbar-and-contextflyout) | Klicken Sie mit der rechten Maustaste auf | Menü-Taste | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Kontextmenüs |

## <a name="xy-focus-navigation-and-interaction"></a>XY-Fokusnavigation und -interaktion

Wenn Ihre App die korrekte Fokusnavigation für Tastaturen unterstützt, wird dies gut auf Gamepads und Fernbedienungen übertragen.
Die Pfeiltastennavigation ist dem **Steuerkreuz** und dem **linken Stick** auf Gamepads zugeordnet. Interaktionen mit Benutzeroberflächenelementen sind der Taste **Eingabe/Auswahl** zugeordnet (siehe [Gamepad und Fernbedienung](#gamepad-and-remote-control)).

Viele Ereignisse und Eigenschaften werden von der Tastatur und dem Gamepad verwendet. Beide lösen die Ereignisse `KeyDown` und `KeyUp` aus, und beide navigieren nur zu Steuerelementen mit den Eigenschaften `IsTabStop="True"` und `Visibility="Visible"`. Designanleitungen für die Tastaturnutzung finden Sie unter [Tastaturinteraktionen](../input/keyboard-interactions.md).

Wenn die Tastaturunterstützung ordnungsgemäß implementiert ist, wird Ihre App angemessen funktionieren. Es sind jedoch möglicherweise zusätzliche Arbeiten erforderlich, um jedes Szenario zu unterstützen. Bedenken Sie die spezifischen Anforderungen Ihrer App, um die bestmögliche Benutzererfahrung bereitzustellen.

> [!IMPORTANT]
> Der Mausmodus ist bei UWP-Apps auf Xbox One standardmäßig aktiviert. Um den Mausmodus zu deaktivieren und die XY-Fokusnavigation zu aktivieren, legen Sie `Application.RequiresPointerMode=WhenRequested` fest.

### <a name="debugging-focus-issues"></a>Debuggen von Fokusproblemen

Die Methode [FocusManager.GetFocusedElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.focusmanager.getfocusedelement.aspx) informiert Sie darüber, welches Element gerade den Fokus besitzt. Dies ist in Situationen hilfreich, in denen die Fokusanzeige nicht direkt sichtbar ist. Mit dem folgenden Code können Sie die entsprechenden Informationen im Visual Studio-Ausgabefenster protokollieren:

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

* Die [IsTabStop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.istabstop.aspx)- oder [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx)-Eigenschaft ist falsch festgelegt.
* Das Steuerelement mit dem Fokus ist größer, als Sie denken. Die XY-Navigation arbeitet mit der Gesamtgröße des Steuerelements ([ActualWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualwidth.aspx) und [ActualHeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualheight.aspx)) und nicht nur mit dem Teil des Steuerelements, das etwas darstellt.
* Ein Steuerelement, das den Fokus erhalten kann, befindet sich über einem anderen Steuerelement. Die XY-Navigation unterstützt keine überlappenden Steuerelemente.

Wenn die XY-Navigation nach dem Beheben dieser drei Probleme noch immer nicht wie erwartet funktioniert, kann das Element, das den Fokus erhalten soll, mit der in [Überschreiben der Standardnavigation](#overriding-the-default-navigation) beschriebenen Methode manuell festgelegt werden.

Wenn die XY-Navigation wie beabsichtigt funktioniert, die Fokusanzeige jedoch nicht sichtbar ist, kann dies von einem der folgenden Probleme verursacht werden:

* Sie haben eine neue Vorlage auf das Steuerelement angewendet und keine Fokusanzeige einbezogen. Legen Sie die Eigenschaft `UseSystemFocusVisuals="True"` fest, oder fügen Sie manuell eine Fokusanzeige hinzu.
* Sie haben den Fokus über den Aufruf von `Focus(FocusState.Pointer)` verschoben. Der [FocusState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.focusstate.aspx)-Parameter steuert, was mit der Fokusanzeige geschieht. Im Allgemeinen sollten Sie den Parameter auf `FocusState.Programmatic` festlegen. So bleibt die Fokusanzeige sichtbar, wenn sie vorher sichtbar war, und wird ausgeblendet, wenn sie vorher ausgeblendet war.

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

Im obigen Beispiel gilt: Wenn `Button`2 den Fokus besitzt und der Benutzer nach rechts navigiert, ist `Button`4 der beste Fokuskandidat. Der Fokus wechselt jedoch zu `Button`3, da das übergeordnete `UserControl`-Element dies erzwingt, wenn die visuelle Struktur verlassen wird.

### <a name="path-of-least-clicks"></a>Pfad der wenigsten Klicks

Ermöglichen Sie Benutzern, häufig ausgeführte Aktionen mit möglichst wenigen Klicks auszuführen. Im folgenden Beispiel befindet sich [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) zwischen der **Play**-Schaltfläche (die zunächst den Fokus erhält) und einem häufig verwendeten Element, sodass sich zwischen zwei Aktionen mit Priorität ein nicht notwendiges Element befindet.

![Bewährte Methoden für die Navigation stellen den Pfad der wenigsten Klicks bereit](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Im folgenden Beispiel befindet sich [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) stattdessen oberhalb der **Play**-Schaltfläche.
Durch das einfache Neuanordnen der Benutzeroberfläche, sodass sich nicht notwendige Elemente nicht zwischen Aktionen mit Priorität befinden, wird die Verwendbarkeit Ihrer App erheblich verbessert.

![TextBlock an eine Stelle oberhalb der Play-Schaltfläche verschoben, sodass sie sich nicht mehr zwischen Aktionen mit Priorität befindet](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar und ContextFlyout

Bei Verwendung einer [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) müssen Sie das Problem hinsichtlich Bildläufen durch Listen berücksichtigen, wie in [Problem: Benutzeroberflächenelemente nach langen bildlauffähigen Listen/Rastern](#problem-ui-elements-located-after-long-scrolling-list-grid) beschrieben. Die folgende Abbildung zeigt ein Benutzeroberflächenlayout mit `CommandBar` am Ende einer Liste/eines Rasters. Der Benutzer müsste einen Bildlauf ganz nach unten durch die Liste/das Rasters ausführen, um zu `CommandBar` zu gelangen.

![CommandBar am unteren Ende einer Liste/eines Rasters](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Was geschieht, wenn Sie `CommandBar` an einer Stelle *oberhalb* der Liste/des Rasters platzieren? Auch wenn ein Benutzer, der einen Bildlauf nach unten durch die Liste/das Raster ausführt, einen Bildlauf zurück nach oben ausführen müsste, um zu `CommandBar` zu gelangen, bedeutet dies etwas weniger Navigationsaufwand als bei der vorherigen Konfiguration. Beachten Sie, dass dies voraussetzt, dass sich der anfängliche Fokus Ihrer App neben oder oberhalb von `CommandBar` befindet. Dieser Ansatz funktioniert weniger gut, wenn sich der anfängliche Fokus unterhalb der Liste/des Rasters befindet. Wenn diese `CommandBar`-Elemente globale Aktionselemente sind, auf die nicht sehr häufig zugegriffen werden muss (beispielsweise eine **Sync**-Schaltfläche), ist es möglicherweise zulässig, diese oberhalb der Liste/des Rasters zu platzieren.

Zwar ist das vertikale Stapeln der `CommandBar`-Elemente nicht möglich, die Platzierung gegen die Bildlaufrichtung (etwa links oder rechts von einer vertikal laufenden Liste oder über/unter einer horizontal laufenden Liste) ist eine weitere Option, die Sie nutzen können, wenn dies gut zu Ihrem Benutzeroberflächenlayout passt.

Wenn Ihre App eine `CommandBar` umfasst, auf deren Elemente die Benutzer zugreifen müssen, sollten Sie diese Elemente möglicherweise innerhalb einer [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)-Eigenschaft platzieren und sie aus der `CommandBar` entfernen. `ContextFlyout` ist eine Eigenschaft von [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx) und stellt das dem Element zugeordnete [Kontextmenü](../controls-and-patterns/dialogs-and-flyouts/index.md) dar. Wenn Sie auf einem PC mit der rechten Maustaste auf ein Element mit einem `ContextFlyout` klicken, wird das Kontextmenü eingeblendet. Auf Xbox One geschieht dies beim Drücken der **Menü**-Taste, während ein entsprechendes Element den Fokus hat.

### <a name="ui-layout-challenges"></a>Herausforderungen beim UI-Layout

Einige UI-Layouts sind aufgrund der Natur der XY-Fokusnavigation anspruchsvoller und sollten jeweils einzeln für sich beurteilt werden. Es gibt zwar keine einzige „richtige“ Lösung, und Ihre Entscheidung muss auf den Anforderungen Ihrer App basieren, es stehen jedoch einige Techniken zur Verfügung, die Ihnen dabei helfen können, ein hervorragendes TV-Erlebnis bereitzustellen.

Um dies zu verdeutlichen, sehen wir uns eine imaginäre App an, die einige dieser Herausforderungen sowie die entsprechenden Techniken zur Behebung illustriert.

> [!NOTE]
> Diese fiktive App dient dazu, UI-Probleme und mögliche Lösungen zu veranschaulichen. Die Benutzerfreundlichkeit wird dabei nicht berücksichtigt.

Nachfolgend sehen Sie eine fiktive Immobilien-App, die eine Liste zum Verkauf stehender Häuser, eine Karte, Beschreibungen von Immobilien sowie weitere Informationen anzeigt. Diese App stellt Sie vor drei Herausforderungen, denen Sie mit den folgenden Techniken begegnen können:

- [Neuanordnung der UI](#ui-rearrange)
- [Fokusaktivierung](#engagement)
- [Mausmodus](#mouse-mode)

![Fiktive Immobilien-App](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problem: UI-Elemente, die sich hinter einer langen Bildlaufliste oder einem Raster befinden <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

Die [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) der Eigenschaften in der folgenden Abbildung ist eine sehr lange Bildlaufliste. Wenn [Engagement](#focus-engagement) *nicht* für die `ListView` erforderlich ist, wenn der Benutzer zu der Liste navigiert, wird der Fokus auf das erste Element in der Liste gesetzt. Damit der Benutzer zur Schaltfläche **Zurück** oder **Weiter** gelangen kann, muss er alle Elemente der Liste durchlaufen. In Fällen wie diese, bei denen der Benutzer die gesamte Liste mühsam durchlaufen muss – d.h. wenn die Liste ganz einfach zu lang ist, um noch akzeptablen Benutzerkomfort zu ermöglichen – sollten Sie andere Optionen erwägen.

![Immobilien-App: Eine Liste mit 50 Elementen erfordert 51 Klicks, bis die Schaltflächen am Ende erreicht sind](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Lösungen

**Neuanordnung der UI <a name="ui-rearrange"></a>**

Sofern nicht der anfängliche Fokus auf das Ende der Seite gesetzt ist, sind über einer langen Bildlaufliste platzierte UI-Elemente typischerweise einfacher erreichbar, als solche unterhalb einer solchen Liste.
Wenn dieses neue Layout für andere Geräte funktioniert, kann das Ändern des Layouts für alle Gerätefamilien kostengünstiger sein als das Vornehmen spezieller UI-Änderungen nur für die Xbox One.
Weiterhin gilt, dass die Platzierung von UI-Elementen gegen die Bildlaufrichtung (d.h. horizontal bei einer vertikal laufenden Liste oder vertikal bei einer horizontal laufenden Liste) den Benutzerkomfort noch weiter erhöht.

![Immobilien-App: Platzieren von Schaltflächen oberhalb einer langen Bildlaufliste](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Fokusaktivierung <a name="engagement"></a>**

Ist die Aktivierung *erforderlich*, wird die gesamte `ListView` zu einem einzigen Fokusziel. Der Benutzer kann die Inhalte der Liste übergehen, um zum nächsten fokussierbaren Element zu gelangen. Erfahren Sie mehr darüber, welche Steuerelemente die Aktivierung unterstützen, und wie Sie sie in der [Fokusaktivierung](#focus-engagement) verwenden können.

![Immobilien-App: Einstellen der Aktivierung als erforderlich, damit die Schaltflächen „Zurück/Weiter“ mit nur einem Klick erreicht werden können](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problem: ScrollViewer ohne fokussierbare Elemente

Da die XY-Fokusnavigation darauf basiert, dass jeweils zu einem fokussierbaren UI-Element navigiert wird, kann ein [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) ohne fokussierbare Elemente (etwa einer, der, wie der in diesem Beispiel, nur Text enthält) ein Szenario verursachen, in dem der Benutzer nicht alle Inhalte in dem `ScrollViewer` anzeigen kann.
Lösungen für dieses und ähnliche Szenarien finden Sie unter [Fokusaktivierung](#focus-engagement).

![Immobilien-App: ScrollViewer mit lediglich Text](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problem: UI mit freiem Bildlauf

Wenn Ihre App eine UI mit freiem Bildlauf benötigt, etwa eine Zeichenoberfläche oder, wie in diesem Beispiel, eine Karte, ist die XY-Fokus-Navigation ganz einfach nicht geeignet.
In solchen Fällen können Sie den [Mausmodus](#mouse-mode) aktivieren, damit der Benutzer in einem UI-Element frei navigieren kann.

![Karten-UI-Element im Mausmodus](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Mausmodus

Wie in [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) beschrieben, wird der Fokus auf Xbox One mittels eines XY-Navigationssystems verschoben, sodass Benutzer den Fokus von Steuerelement zu Steuerelement verschieben können, indem sie nach oben, unten, links und rechts navigieren.
Einige Steuerelemente wie [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) und [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx) erfordern jedoch eine mausähnliche Interaktion, bei der Benutzer mit dem Zeiger auf eine beliebige Stelle innerhalb der Grenzen des Steuerelements zeigen können.
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

Verwenden Sie die [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer)-Eigenschaft für ein Steuerelement oder eine Seite, um den Mausmodus zu aktivieren. Die Eigenschaft hat drei mögliche Werte: `Never` (Standardwert), `WhenEngaged` und `WhenFocused`.

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

Wenn sich ein Steuerelement im Mausmodus befindet, befinden sich dessen verschachtelte Steuerelemente ebenfalls im Mausmodus. Der angeforderte Modus der untergeordneten Elemente wird ignoriert; es ist nicht möglich, dass sich ein übergeordnetes Element im Mausmodus befindet, jedoch nicht dessen untergeordnete Elemente.

Darüber hinaus wird der angeforderte Modus eines Steuerelements nur untersucht, wenn es den Fokus erhält. Daher kann der Modus nicht dynamisch geändert werden, während es den Fokus besitzt.

### <a name="activating-mouse-mode-on-a-page"></a>Aktivieren des Mausmodus auf einer Seite

Wenn eine Seite die Eigenschaft `RequiresPointer="WhenFocused"` besitzt, wird der Mausmodus für die gesamte Seite aktiviert, wenn sie den Fokus erhält. Der folgende Codeausschnitt zeigt, wie Sie einer Seite diese Eigenschaft zuweisen:

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> Der Wert `WhenFocused` wird nur für [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)-Objekte unterstützt. Wenn Sie versuchen, diesen Wert für ein Steuerelement festzulegen, wird eine Ausnahme ausgelöst.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>Deaktivieren des Mausmodus für Vollbildinhalte

Beim Anzeigen von Videos oder anderen Arten von Vollbildinhalten empfiehlt es sich in der Regel, den Cursor auszublenden, um den Benutzer nicht abzulenken. Dieses Szenario liegt vor, wenn der Rest der App den Mausmodus verwendet, dieser aber beim Anzeigen von Vollbildinhalten deaktiviert werden soll. Platzieren Sie hierzu den Vollbildinhalt in einem eigenen `Page`-Objekt, und führen Sie die folgenden Schritte aus.

1. Legen Sie im `App`-Objekt Folgendes fest: `RequiresPointerMode="WhenRequested"`.
2. Legen Sie in jedem `Page`-Objekt (*mit Ausnahme des Vollbild-`Page`-Objekts*) Folgendes fest: `RequiresPointer="WhenFocused"`.
3. Legen Sie für das Vollbild-`Page`-Objekt Folgendes fest: `RequiresPointer="Never"`.

Dadurch wird der Cursor nicht angezeigt, wenn Inhalte im Vollbildmodus angezeigt werden.

## <a name="focus-visual"></a>Fokusanzeige

Die Fokusanzeige ist der Rahmen um das Benutzeroberflächenelement, das zurzeit den Fokus besitzt. Dies hilft Benutzern, sich zu orientieren, damit sie in Ihrer Benutzeroberfläche einfach navigieren können, ohne die Orientierung zu verlieren.

Dank der Hinzufügung einer Anzeigenaktualisierung und zahlreicher Anpassungsoptionen zur Fokusanzeige können Entwickler darauf vertrauen, dass eine einzelne Fokusanzeige gut auf PCs und Xbox One und allen anderen Windows10-Geräten funktioniert, die Tastaturen und/oder Gamepads/Fernbedienungen unterstützen.

Während die gleiche Fokusanzeige auf verschiedenen Plattformen verwendet werden kann, unterscheidet sich der Kontext, in dem der Benutzer diese antrifft, für die 10-Fuß-Erfahrung etwas. Sie sollten davon ausgehen, dass der Benutzer nicht dem gesamten Fernsehbildschirm seine volle Aufmerksamkeit schenkt und es daher wichtig ist, dass das aktuell fokussierte Element für den Benutzer jederzeit deutlich erkennbar ist, um eine frustrierende Suche nach der Anzeige zu vermeiden.

Darüber hinaus ist es wichtig, zu beachten, dass die Fokusanzeige standardmäßig angezeigt wird, wenn ein Gamepad oder eine Fernbedienung verwendet wird, jedoch *nicht*, wenn eine Tastatur verwendet wird. Auch wenn Sie keine Fokusanzeige implementieren, wird sie daher angezeigt, wenn Sie Ihre App auf Xbox One ausführen.

### <a name="initial-focus-visual-placement"></a>Anfängliche Platzierung der Fokusanzeige

Platzieren Sie beim Starten einer App oder Navigieren zu einer Seite den Fokus auf einem Benutzeroberflächenelement, das sinnvollerweise als erstes Element betrachtet werden kann, für das Benutzer eine Aktion ausführen würden. Beispielsweise könnte eine Foto-App den Fokus auf das erste Element im Katalog platzieren, und eine Musik-App, in der zur detaillierten Ansicht eines Musiktitels navigiert wurde, könnte den Fokus auf die Abspieltaste setzen, um eine einfache Wiedergabe der Musik zu ermöglichen.

Platzieren Sie den anfänglichen Fokus in Ihrer App möglichst in den Bereich oben links (oder oben rechts bei einem Lesefluss von rechts nach links). Die meisten Benutzer neigen dazu, sich zunächst auf diesen Bereich zu konzentrieren, da der Inhaltsfluss einer App im Allgemeinen dort beginnt.

### <a name="making-focus-clearly-visible"></a>Klare Erkennbarkeit des Fokus

Eine Fokusanzeige sollte stets auf dem Bildschirm sichtbar sein, damit Benutzer Vorgänge an der Stelle fortsetzen können, an der sie aufgehört haben, ohne nach dem Fokus suchen zu müssen. Entsprechend sollte sich stets ein fokussierbares Element auf dem Bildschirm befinden. Verwenden Sie beispielsweise keine Popups, die nur Text und keine fokussierbaren Elemente enthalten.

Eine Ausnahme von dieser Regel wären die Vollbild-Funktionen wie das Abspielen von Videos oder das Anzeigen von Bildern. In diesen Fällen sollte die Fokusanzeige nicht sichtbar sein.

### <a name="reveal-focus"></a>Einblendungen mit Fokus

Reveal-focus sind Lichteffekte, die den Rahmen des fokussierbaren Elementes animieren, wenn der Benutzer den Fokus des Gamepad oder der Tastatur darauf lenken. Durch die Animation des Scheins um den Rand der fokussierten Elemente, gibt Reveal-Focus dem Benutzer ein besseres Verständnis, wo der Fokus liegt und wohin der Fokus geht.

Reveal-focus ist standardmäßig deaktiviert Für 10-Fuß-Erlebnisse sollten Sie sich dafür entscheiden, den Fokus zu aufzuzeigen, indem Sie die [Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind)-Eigenschaft in Ihrem App-Konstruktor setzen.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Weitere Informationen finden Sie in der Anleitung zu [Einblendungen mit Fokus](/windows/uwp/design/style/reveal-focus).

### <a name="customizing-the-focus-visual"></a>Anpassen der Fokusanzeige

Wenn Sie die Fokusanzeige anpassen möchten, können Sie hierzu die Eigenschaften für die Fokusanzeige in den einzelnen Steuerelementen bearbeiten. Es gibt mehrere entsprechende Eigenschaften, über die Sie die App personalisieren können.

Sie können sogar die systemeigenen Fokusanzeigen deaktivieren und eigene Fokusanzeigen darstellen. Weitere Informationen finden Sie unter [VisualState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.Aspx).

### <a name="light-dismiss-overlay"></a>Overlay für einfaches Ausblenden

Um die Aufmerksamkeit der Benutzer auf die Benutzeroberflächenelemente zu lenken, die sie gerade mit dem Gamecontroller oder der Fernbedienung bearbeiten, fügt UWP automatisch eine „Ausblendschicht“ ein, die Bereiche außerhalb der Popup-Benutzeroberfläche abdeckt, wenn die App auf Xbox One ausgeführt wird. Dies erfordert keinen zusätzlichen Aufwand. Sie sollten diese Funktionalität jedoch während der Entwicklung Ihrer Benutzeroberfläche berücksichtigen. Über die `LightDismissOverlayMode`-Eigenschaft können Sie die Ausblendschicht für jedes `FlyoutBase`-Element aktivieren oder deaktivieren. Der Standardwert `Auto` bedeutet, dass sie auf Xbox aktiviert und auf allen anderen Plattformen deaktiviert ist. Weitere Informationen finden Sie unter [Modales Ausblenden im Vergleich zu einfachem Ausblenden](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Fokusaktivierung

Die Fokusaktivierung soll die Verwendung eines Gamepads oder einer Fernbedienung für Interaktionen mit einer App vereinfachen.

> [!NOTE]
> Das Einrichten der Fokusaktivierung wirkt sich nicht auf Tastaturen oder andere Eingabegeräte aus.

Wenn die Eigenschaft `IsFocusEngagementEnabled` für ein [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) -Objekt auf `True` festgelegt wird, zeigt dies an, dass das Steuerelement Fokusaktivierung erfordert. Das bedeutet, dass der Benutzer die **A/Select**-Taste (Auswahl-Taste) drücken muss, um das Steuerelement zu „aktivieren“ und mit diesem zu interagieren. Anschließend können sie die **B/Zurück**-Taste drücken, um das Steuerelement zu deaktivieren und von diesem weg zu navigieren.

> [!NOTE]
> `IsFocusEngagementEnabled` ist eine neue API, die noch nicht dokumentiert ist.

### <a name="focus-trapping"></a>Fokustrapping

Fokustrapping bedeutet, dass ein Benutzer versucht, in der Benutzeroberfläche einer App zu navigieren, jedoch innerhalb eines Steuerelements „gefangen“ ist, wodurch es schwer oder sogar unmöglich wird, von diesem Steuerelement weg zu navigieren.

Das folgende Beispiel zeigt eine Benutzeroberfläche, die ein Fokustrapping verursacht.

![Schaltflächen links und rechts neben einem horizontalen Schieberegler](images/designing-for-tv/focus-engagement-focus-trapping.png)

Wenn der Benutzer von der linken zur rechten Schaltfläche navigieren möchte, wäre es logisch, anzunehmen, dass er hierzu auf dem Steuerkreuz (D-Pad) oder linken Stick lediglich zweimal rechts drücken muss.
Wenn das [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx)-Steuerelement jedoch keine Aktivierung erfordert, würde das folgende Verhalten eintreten; Wenn der Benutzer das erste Mal rechts drückt, würde der Fokus zum `Slider` verschoben. Wenn er das zweite Mal rechts drückt, würde der Ziehpunkt des `Slider` nach rechts verschoben. Der Benutzer würde den Ziehpunkt immer weiter nach rechts verschieben und könnte nicht zur Schaltfläche gelangen.

Es gibt verschiedene Methoden, um dieses Problem zu umgehen. Eine Möglichkeit besteht darin, ein anderes Layout ähnlich wie im Beispiel für eine Immobilien-App in [XY-Fokusnavigation und -interaktion](#xy-focus-navigation-and-interaction) zu entwerfen. Dort wurden die Schaltflächen **Zurück** und **Weiter** oberhalb der `ListView` platziert. Eine vertikale anstelle einer horizontalen Anordnung der Steuerelemente wie in der folgenden Abbildung würde das Problem lösen.

![Schaltflächen ober- und unterhalb eines horizontalen Schiebereglers](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

Nun kann der Benutzer zu jedem der Steuerelemente navigieren, indem er auf dem Steuerkreuz (D-Pad/linken Stick) nach oben und nach unten drückt. Wenn der `Slider` den Fokus hat, kann er links und rechts drücken, um den Ziehpunkt des `Slider` zu verschieben, wie erwartet.

Ein anderer Ansatz zur Lösung dieses Problems besteht darin, für den `Slider` Aktivierung zu erfordern. Wenn Sie `IsFocusEngagementEnabled="True"` festlegen, führt dies zum folgenden Verhalten.

![Anfordern einer Fokusaktivierung für den Schieberegler, damit Benutzer zur Schaltfläche auf der rechten Seite navigieren können](images/designing-for-tv/focus-engagement-slider.png)

Wenn der `Slider` Fokusaktivierung erfordert, kann der Benutzer einfach zur Schaltfläche auf der rechten Seite gelangen, indem er auf dem Steuerkreuz (D-Pad)/dem linken Stick zweimal rechts drückt. Diese Lösung ist hervorragend geeignet, da sie keine Benutzeroberflächenanpassung erfordert und das erwartete Verhalten erzeugt.

### <a name="items-controls"></a>Elementsteuerelemente

Abgesehen vom [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx)-Steuerelement gibt es weitere Steuerelemente, für die Sie möglicherweise eine Aktivierung anfordern sollten, wie beispielsweise:

- [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

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

[ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) unterscheidet sich etwas von diesen Steuerelementen. Es weist spezifische Besonderheiten auf, die Sie berücksichtigen müssen. Wenn Sie ein `ScrollViewer`-Steuerelement mit fokussierbarem Inhalt besitzen, können Sie standardmäßig durch Navigieren zum `ScrollViewer`-Steuerelement durch dessen fokussierbare Elemente navigieren. Wie in einer `ListView`, müssen Sie einen Bildlauf über alle Elemente ausführen, um von `ScrollViewer` weg zu navigieren.

Wenn `ScrollViewer` *keine* fokussierbaren Inhalte besitzt, also beispielsweise lediglich Text enthält, können Sie `IsFocusEngagementEnabled="True"` festlegen, sodass Benutzer `ScrollViewer` mithilfe der **A/Auswahl**-Taste aktivieren können. Nach der Aktivierung können sie mit dem **Steuerkreuz (D-Pad)/linken Stick** einen Bildlauf durch den Text ausführen und anschließend die **B/Back**-Taste (Zurück-Taste) drücken, um das Steuerelement zu deaktivieren, wenn sie den Vorgang abgeschlossen haben.

Ein weiterer Ansatz besteht darin, `IsTabStop="True"` für `ScrollViewer` festzulegen, damit der Benutzer das Steuerelement nicht aktivieren muss. Er kann in diesem Fall einfach den Fokus auf das Steuerelement setzen und anschließend mit dem **Steuerkreuz (D-Pad)/linken Stick** einen Bildlauf ausführen, wenn es innerhalb von `ScrollViewer` keine fokussierbaren Elemente gibt..

### <a name="focus-engagement-defaults"></a>Standardeinstellungen in Bezug auf die Fokusaktivierung

Einige Steuerelemente führen häufig genug dazu, dass Benutzer in einem Steuerelement gefangen werden, um die Fokusaktivierung als Standardeinstellung zu rechtfertigen. Im Fall anderer Steuerelemente, für die die Fokusaktivierung standardmäßig deaktiviert ist, kann es nützlich sein, die Fokusaktivierung zu aktivieren. Die folgende Tabelle listet diese Steuerelemente und deren Standardverhalten in Bezug auf die Fokusaktivierung auf.

| Steuerelement               | Standardeinstellung in Bezug auf die Fokusaktivierung  |
|-----------------------|---------------------------|
| CalendarDatePicker    | Ein                        |
| FlipView              | Aus                       |
| GridView              | Aus                       |
| ListBox               | Aus                       |
| ListView              | Aus                       |
| ScrollViewer          | Aus                       |
| SemanticZoom          | Aus                       |
| Slider                | Ein                        |

Alle anderen UWP-Steuerelemente zeigen keine Änderungen in Bezug auf Verhalten oder Anzeige, wenn `IsFocusEngagementEnabled="True"` festgelegt wird.

## <a name="ui-element-sizing"></a>Anpassen von Benutzeroberflächenelementen

Da Benutzer von Apps in 10-Fuß-Umgebungen Fernbedienungen oder Gamepads verwenden und sich mehrere Meter vom Bildschirm entfernt befinden, gibt es einige Überlegungen in Bezug auf die Benutzeroberfläche, die Sie für Ihren Entwurf berücksichtigen müssen.
Stellen Sie sicher, dass die Benutzeroberfläche über eine geeignete Inhaltsdichte verfügt und nicht zu überladen ist, damit Benutzer einfach navigieren und Elemente auswählen können. Denken Sie daran: Einfachheit ist der Schlüssel.

### <a name="scale-factor-and-adaptive-layout"></a>Skalierungsfaktor und adaptives Layout

Der **Skalierungsfaktor** trägt dazu bei, dass Benutzeroberflächenelemente in der richtigen Größe für das Gerät angezeigt werden, auf dem die App ausgeführt wird.
Auf Desktops befindet sich diese Einstellung in **Einstellungen > System > Anzeige** als gleitender Wert.
Diese Einstellung ist auch auf Mobiltelefonen vorhanden, wenn das Gerät diese unterstützt.

![Ändern der Größe von Text, Apps und anderen Elementen](images/designing-for-tv/ui-scaling.png)

Auf Xbox One gibt es diese Systemeinstellung nicht. UWP-UI-Elemente, die für Fernsehgeräte angepasst werden, werden jedoch standardmäßig auf **200%** für XAML-Apps und **150%** für HTML-Apps skaliert.
Solange Benutzeroberflächenelemente für andere Geräte korrekt angepasst werden, werden sie auch für Fernsehgeräte angepasst.
Xbox One rendert Ihre App mit 1080p (1920 x 1080 Pixel). Stellen Sie daher mittels [adaptiver Techniken](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) sicher, dass die UI bei 960x540px und einer Skalierung von 100% (oder bei 1280x720px und einer Skalierung von 100% für HTML-Apps) gut aussieht, wenn Sie eine App von anderen Geräten wie PCs übertragen.

Das Entwerfen für Xbox unterscheidet sich etwas vom Entwerfen für PCs, da Sie lediglich eine Auflösung von 1920x1080 berücksichtigen müssen.
Es spielt keine Rolle, ob der Benutzer ein Fernsehgerät mit einer besseren Auflösung besitzt&mdash;UWP-Apps werden stets auf 1080p skaliert.

Bei der Ausführung auf Xbox One werden darüber hinaus korrekte Ressourcengrößen aus dem Satz mit 200% (oder 150% für HTML-Apps) für Ihre App aufgerufen, unabhängig von der Auflösung des Fernsehgeräts.

### <a name="content-density"></a>Inhaltsdichte

Denken Sie beim Entwerfen Ihrer App daran, dass Benutzer die Benutzeroberfläche aus der Entfernung sieht und mittels einer Fernbedienung oder einem Gamecontroller mit dieser interagiert. Dies dauert länger als die Verwendung von Maus- oder Toucheingaben.

#### <a name="sizes-of-ui-controls"></a>Größen von Benutzeroberflächensteuerelementen

Interaktive Benutzeroberflächenelemente sollten eine Mindesthöhe von 32epx (effektive Pixel) besitzen. Dies ist die Standardeinstellung für allgemeine UWP-Steuerelemente. Wenn diese bei einer Skalierung von 200% verwendet wird, ist sichergestellt, dass Benutzeroberflächenelemente aus der Entfernung erkennbar sind. Darüber hinaus trägt dies zur Reduzierung der Inhaltsdichte bei.

![UWP-Schaltfläche bei einer Skalierung von 100% und 200%](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>Anzahl der Klicks

Um Ihre Benutzeroberfläche zu vereinfachen, sollten Benutzer nicht mehr als **sechs Klicks** benötigen, wenn sie von einem Rand des Fernsehbildschirms zum anderen navigieren. Auch hier gilt der Grundsatz der **Einfachheit**. Weitere Informationen finden Sie unter [Pfad der wenigsten Klicks](#path-of-least-clicks).

![6Symbole insgesamt](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>Textgrößen

Wenden Sie die folgenden Faustregeln an, damit Ihre Benutzeroberfläche aus der Entfernung erkennbar ist:

* Haupttext und Lesen von Inhalten: mindestens 15epx
* Nicht kritische Texte und ergänzende Inhalte: mindestens 12epx

Wenn Sie in der Benutzeroberfläche einen größeren Text verwenden, sollten Sie eine Größe wählen, die die verfügbare Bildschirmfläche nicht zu sehr begrenzt, indem sie Platz beansprucht, der potenziell von anderen Inhalten ausgefüllt werden kann.

### <a name="opting-out-of-scale-factor"></a>Deaktivieren des Skalierungsfaktors

Wir empfehlen Ihnen, für Ihre App die Vorteile der Skalierungsfaktorunterstützung zu nutzen. Dies trägt dazu bei, dass sie auf allen Geräten korrekt ausgeführt wird, indem sie für jeden Gerätetyp skaliert wird.
Es ist jedoch möglich, dieses Verhalten zu deaktivieren und alle Benutzeroberflächenelemente für eine Skalierung von 100 % zu entwerfen. Beachten Sie, dass Sie den Skalierungsfaktor nicht in einen anderen Wert als 100% ändern können.

Sie können den Skalierungsfaktor für HTML-Apps deaktivieren, indem Sie den folgenden Codeausschnitt verwenden:

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` informiert Sie, ob die Deaktivierung erfolgreich ausgeführt wurde.

Weitere Informationen, einschließlich HTML/JavaScript-Beispielcode, finden Sie unter [So deaktivieren Sie die Skalierung](../../xbox-apps/disable-scaling.md).

Stellen Sie sicher, dass Sie entsprechenden Größen der Benutzeroberflächenelemente berechnen, indem Sie die in diesem Thema genannten *effektiven* Pixelwerte auf die *tatsächlichen* Pixelwerte verdoppeln (oder für HTML-Apps mit 1,5 multiplizieren).

## <a name="tv-safe-area"></a>Fernsehsicherer Bereich

Nicht alle Fernsehgeräte zeigen die Inhalte von Rand zu Rand an. Dies hat historische und technische Gründe. Standardmäßig vermeidet die UWP die Anzeige von Benutzeroberflächeninhalten in nicht fernsehsicheren Bereichen und zeichnet stattdessen lediglich den Seitenhintergrund.

Der nicht fernsehsichere Bereich wird in der folgenden Abbildung durch den blauen Bereich dargestellt.

![Nicht fernsehsicherer Bereich](images/designing-for-tv/tv-unsafe-area.png)

Sie können für den Hintergrund eine statische Farbe, eine Designfarbe oder ein Bild verwenden, wie in den folgenden Codeausschnitten gezeigt.

### <a name="theme-color"></a>Designfarbe

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>Bild

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

So sieht Ihre App ohne zusätzlichen Aufwand aus.

![Fernsehsicherer Bereich](images/designing-for-tv/tv-safe-area.png)

Dies ist nicht optimal, da dies der App einen „Schachteleffekt“ verleiht. Teile der Benutzeroberfläche werden scheinbar abgeschnitten, beispielsweise der Navigationsbereich und das Raster. Sie können jedoch Optimierungen durchführen, um Teile der Benutzeroberfläche bis an die Ränder des Bildschirms zu erweitern, um der App einen kinofilmähnlicheren Effekt zu verleihen.

### <a name="drawing-ui-to-the-edge"></a>Zeichnen der Benutzeroberfläche bis zum Rand

Wir empfehlen Ihnen, bestimmte Benutzeroberflächenelemente zu verwenden, um die Benutzeroberfläche bis an die Ränder des Bildschirms zu erweitern und Benutzern eine immersivere Umgebung zu bieten. Dazu gehören [ScrollViewers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx), [Navigationsbereiche](../controls-and-patterns/navigationview.md) und [CommandBars](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx).

Es ist jedoch auch wichtig, dass interaktive Elemente und Texte stets die Bildschirmränder vermeiden, um sicherzustellen, dass sie auf bestimmten Fernsehgeräten nicht abgeschnitten werden. Wir empfehlen Ihnen, nur nicht essentielle visuelle Elemente bis zu 5% von den Rändern des Bildschirms entfernt zu zeichnen. Wie in [Anpassen von Benutzeroberflächenelementen](#ui-element-sizing) bereits erwähnt, nutzt eine UWP-App, die den Xbox One-Standardskalierungsfaktor von 200% verwendet, einen Bereich von 960 x 540epx. Sie sollten es daher vermeiden, in der Benutzeroberfläche Ihrer App essentielle Benutzerflächenelemente in den folgenden Bereichen zu platzieren:

- 27epx vom oberen und unteren Rand
- 48epx vom linken und rechten Rand

In den folgenden Abschnitten wird beschrieben, wie Sie Ihr UI bis an die Ränder des Bildschirms erweitern.

#### <a name="core-window-bounds"></a>Kernfenstergrenzen

Im Fall von UWP-Apps, die ausschließlich für die 10-Fuß-Erfahrung entwickelt werden, stellt die Verwendung von Kernfenstergrenzen die einfachere Option dar.

Fügen Sie in der `OnLaunched`-Methode von `App.xaml.cs` den folgenden Code hinzu:

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Mit dieser Codezeile wird das App-Fenster bis zu den Rändern des Bildschirms erweitert. Daher müssen Sie alle interaktiven und essentiellen Benutzeroberflächenelemente in den bereits beschriebenen fernsehsicheren Bereich verschieben. Kurzlebige Benutzeroberflächenelemente wie Kontextmenüs und geöffnete [Kombinationsfelder](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) bleiben automatisch im fernsehsicheren Bereich.

![Kernfenstergrenzen](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Bereichshintergründe

Navigationsbereiche werden in der Regel nahe am Rand des Bildschirms dargestellt. Daher sollte sich der Hintergrund in den nicht fernsehsicheren Bereich erstrecken, um hässliche Lücken zu vermeiden. Hierzu können Sie einfach die Hintergrundfarbe des Navigationsbereichs in der Hintergrundfarbe der App ändern.

Mithilfe der oben beschriebenen Kernfenstergrenzen können Sie Ihr UI an den Rändern des Bildschirms darstellen. Sie sollten dann jedoch positive Ränder für die [SplitView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.splitview.aspx)-Inhalte nutzen, um diese innerhalb des fernsehsicheren Bereichs zu halten.

![Navigationsbereich bis an die Ränder des Bildschirms erweitert](images/designing-for-tv/tv-safe-areas-2.png)

Hier wurde der Hintergrund des Navigationsbereichs bis an die Ränder des Bildschirms erweitert, während die Navigationselemente weiterhin im fernsehsicheren Bereich angezeigt werden.
Der Inhalt des `SplitView`-Elements (in diesem Fall ein Raster mit Elementen) wurde bis an den unteren Rand des Bildschirms erweitert. So sieht es aus, als würde es fortgesetzt und nicht abgeschnitten. Der obere Bereich des Rasters befindet sich nach wie vor im fernsehsicheren Bereich. (Weitere Informationen hierzu finden Sie unter [Bildlaufenden von Listen und Rastern](#scrolling-ends-of-lists-and-grids)).

Mit dem folgenden Codeausschnitt wird dieser Effekt erzielt:

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) ist ein weiteres Beispiel für einen Bereich, der häufig in der Nähe eines oder mehrerer Ränder der App positioniert ist. Daher sollte dessen Hintergrund auf Fernsehbildschirmen bis an die Ränder des Bildschirms erweitert werden. In der Regel gibt es auf der rechten Seite die Schaltfläche **Mehr** (dargestellt durch „...“), die weiter im fernsehsicheren Bereich angezeigt werden sollte. Im Folgenden finden Sie einige unterschiedliche Strategien, um die gewünschten Interaktionen und visuellen Effekte zu erzielen.

**Option1**: Festlegen der `CommandBar`-Hintergrundfarbe auf transparent oder auf die Farbe des Seitenhintergrunds:

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

Hierdurch sieht die `CommandBar` aus, als ob sie auf dem gleichen Hintergrund wie der Rest der Seite angezeigt wird, sodass sich der Hintergrund nahtlos bis an den Rand des Bildschirms erstreckt.

**Option2**: Hinzufügen eines Hintergrundrechtecks, dessen Füllung die gleiche Farbe wie der `CommandBar`-Hintergrund hat und das unter der `CommandBar` und über dem Rest der Seite liegt:

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> Wenn Sie sich für diesen Ansatz entscheiden, sollten Sie berücksichtigen, dass die Schaltfläche **Mehr** wenn notwendig die Höhe der geöffneten `CommandBar` ändert, um die Beschriftungen der `AppBarButton`-Schaltflächen unterhalb der Symbole anzuzeigen. Wir empfehlen Ihnen, die Beschriftungen *rechts* neben ihren Symbolen anzuzeigen, um diese Größenanpassung zu vermeiden. Weitere Informationen finden Sie unter [CommandBar-Beschriftungen](#commandbar-labels).

Beide Vorgehensweisen gelten auch für die anderen in diesem Abschnitt aufgeführten Arten von Steuerelementen.

#### <a name="scrolling-ends-of-lists-and-grids"></a>Bildlaufenden von Listen und Rastern

Meist enthalten Listen und Raster mehr Elemente, als gleichzeitig auf den Bildschirm passen. Wenn dies der Fall ist, sollten Sie die Liste oder das Raster bis zum Rand des Bildschirms erweitern. Listen und Raster mit horizontalem Bildlauf sollten bis zum rechten Rand erweitert werden. Listen und Raster mit vertikalem Bildlauf sollten bis zum unteren Rand erweitert werden.

![Abschneiden eines Rasters im fernsehsicheren Bereich](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

Wenn eine Liste oder ein Raster wie hier beschrieben erweitert wird, ist es wichtig, dass die Fokusanzeige und das mit dieser verknüpfte Element weiter im fernsehsicheren Bereich angezeigt werden.

![Bildlauffokus sollte weiter im fernsehsicheren Bereich angezeigt werden](images/designing-for-tv/scrolling-grid-focus.png)

Die UWP verfügt über Funktionen, die dafür sorgen, dass die Fokusanzeige weiter innerhalb der [VisibleBounds](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) angezeigt wird. Sie müssen jedoch Abstand hinzufügen, um sicherzustellen, dass für die Listen-/Rasterelemente ein Bildlauf in den Anzeigebereich des sicheren Bereichs durchgeführt werden kann. Genauer gesagt, müssen Sie für [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) oder [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) deren [ItemsPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx) einen positiven Rand hinzufügen, wie im folgenden Codeausschnitt gezeigt:

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Sie platzieren den zuvor angezeigten Codeausschnitt entweder in die Seitenressourcen oder in den App-Ressourcen und greifen anschließend wie folgt auf diesen zu:

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> Dieser Codeausschnitt gilt speziell für `ListView`-Elemente. Legen Sie bei einem `GridView`-Stil das [TargetType](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx)-Attribut für [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) und [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.aspx) auf `GridView` fest.

Für eine genauere Kontrolle über das sind Elemente eingeblendet, wenn die Anwendung, Version 1803 abzielt oder höher, können Sie das [Ereignis UIElement.BringIntoViewRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)verwenden. Für die **ListView**auf [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) platzieren/**GridView** abzufangen, bevor die internen **ScrollViewer** , wie in den folgenden Codeausschnitten ist:

```xaml
<GridView x:Name="gridView">
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid Orientation="Horizontal"
                           BringIntoViewRequested="ItemsWrapGrid_BringIntoViewRequested"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

```cs
// The BringIntoViewRequested event is raised by the framework when items receive keyboard (or Narrator) focus or 
// someone triggers it with a call to UIElement.StartBringIntoView.
private void ItemsWrapGrid_BringIntoViewRequested(UIElement sender, BringIntoViewRequestedEventArgs args)
{
    if (args.VerticalAlignmentRatio != 0.5)  // Guard against our own request
    {
        args.Handled = true;
        // Swallow this request and restart it with a request to center the item.  We could instead have chosen
        // to adjust the TargetRect’s Y and Height values to add a specific amount of padding as it bubbles up, 
        // but if we just want to center it then this is easier.

        // (Optional) Account for sticky headers if they exist
        var headerOffset = 0.0;
        var itemsWrapGrid = sender as ItemsWrapGrid;
        if (gridView.IsGrouping && itemsWrapGrid.AreStickyGroupHeadersEnabled)
        {
            var header = gridView.GroupHeaderContainerFromItemContainer(args.TargetElement as GridViewItem);
            if (header != null)
            {
                headerOffset = ((FrameworkElement)header).ActualHeight;
            }
        }

        // Issue a new request
        args.TargetElement.StartBringIntoView(new BringIntoViewOptions()
        {
            AnimationDesired = true,
            VerticalAlignmentRatio = 0.5, // a normalized alignment position (0 for the top, 1 for the bottom)
            VerticalOffset = headerOffset, // applied after meeting the alignment ratio request
        });
    }
}
```

## <a name="colors"></a>Farben

Standardmäßig skaliert die Universelle Windows-Plattform die Farben Ihrer Anwendung auf den TV-sicheren Bereich (siehe [TV-sichere Farben](#tv-safe-colors) für weitere Informationen), so dass Ihre App auf jedem Fernseher gut aussieht. Zusätzlich gibt es jedoch Verbesserungen, die für den Satz der von Ihrer App verwendeten Farben durchführen können, um die visuelle Erfahrung auf Fernsehgeräten zu verbessern.

### <a name="application-theme"></a>Anwendungsdesign

Sie können ein **Anwendungsdesign** (dunkel oder hell) wählen, je nach Ihrer App, oder Designs deaktivieren. Weitere allgemeine Empfehlungen für Designs finden Sie in [Farbdesigns](../style/color.md).

Die UWP ermöglicht Apps darüber hinaus, das Design dynamisch festzulegen, basierend auf den Systemeinstellungen, die von den Geräten bereitgestellt werden, auf denen sie ausgeführt werden.
Während die UWP stets die vom Benutzer angegebenen Designeinstellungen beachtet, stellt jedes Gerät auch ein entsprechendes Standarddesign bereit.
Aufgrund des Zwecks der Xbox One, die eher eine *Medien*- als eine *Produktivitäts*umgebung bereitstellen soll, ist das Systemdesign standardmäßig dunkel.
Wenn das Design Ihrer App auf den Systemeinstellungen basiert, sollten Sie berücksichtigen, dass es auf Xbox One standardmäßig dunkel ist.

### <a name="accent-color"></a>Akzentfarbe

Die UWP bietet eine bequeme Möglichkeit, die **Akzentfarbe** verfügbar zu machen, die der Benutzer aus den Systemeinstellungen ausgewählt hat.

Auf Xbox One können Benutzer eine Benutzerfarbe auswählen – genauso, wie sie auf einem PC eine Akzentfarbe auswählen können.
Solange Ihre App diese Akzentfarben über Pinsel oder Farbressourcen aufruft, wird die vom jeweiligen Benutzer in den Systemeinstellungen ausgewählte Farbe verwendet. Beachten Sie, dass Akzentfarben auf Xbox One benutzerbasiert und nicht systembasiert angewendet werden.

Beachten Sie auch, dass der Benutzerfarbensatz auf Xbox One nicht identisch mit dem Benutzerfarbensatz auf PCs, Smartphones und anderen Geräten ist.

Solange Ihre App eine Pinselressource wie **SystemControlForegroundAccentBrush** oder eine Farbressource (**SystemAccentColor**) verwendet oder stattdessen Akzentfarben direkt über die [UIColorType.Accent*](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx)-API aufruft, werden diese Farben durch Akzentfarben ersetzt, die für Xbox One geeignet sind. Pinselfarben mit hohem Kontrast werden wie im Fall von PCs und Telefonen aus dem System abgerufen.

Weitere Informationen zu Akzentfarben im Allgemeinen finden Sie unter [Akzentfarbe](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Farbabweichungen zwischen Fernsehgeräten

Beachten Sie bei der Entwicklung von Apps für Fernsehgeräte, dass Farben sehr unterschiedlich dargestellt werden, abhängig von dem Fernsehgerät, auf dem sie gerendert werden. Gehen Sie nicht davon aus, dass die Farben genau wie auf Ihrem Bildschirm angezeigt werden. Wenn Ihre App geringfügige Farbunterschiede nutzt, um Teile der Benutzeroberfläche zu unterscheiden, könnten sich die Farben mischen und die Benutzer könnten verwirrt werden. Verwenden Sie Farben, die unterschiedlich genug sind, dass Benutzer sie unabhängig vom verwendeten Fernsehgerät deutlich unterscheiden können.

### <a name="tv-safe-colors"></a>Fernsehsichere Farben

Die RGB-Werte einer Farbe stellen die Intensität für Rot, Grün und Blau dar. Fernseher kommen mit extremen Intensitäten nicht sehr gut zurecht. Sie können einen seltsamen Bandeffekt erzeugen oder auf bestimmten Fernsehern verwaschen erscheinen. Darüber hinaus verursachen Farben mit hoher Intensität möglicherweise ein „Blooming“, d.h., Pixel in der Nähe beginnen, die gleichen Farben aufzurufen. Die Ansichten darüber, was als fernsehsichere Farben gelten kann, gehen zwar auseinander. Im Allgemeinen können Farben mit RGB-Werten zwischen 16 und 235 (oder 10-EB hexadezimal) jedoch sicher für Fernsehgeräte verwendet werden.

![Fernsehsicherer Farbbereich](images/designing-for-tv/tv-safe-colors-2.png)

In der Vergangenheit mussten Apps auf der Xbox ihre Farben so anpassen, dass sie in diesen „TV-sicheren” Farbbereich fallen. Ab dem Fall Creators Update skaliert Xbox One jedoch automatisch alle Inhalte in den TV-sicheren Bereich. Das bedeutet, dass sich die meisten App-Entwickler nicht mehr um TV-sichere Farben kümmern müssen.

> [!IMPORTANT]
> Videoinhalte, die sich bereits im TV-sicheren Farbbereich befinden, haben diesen Farbskalierungseffekt bei der Wiedergabe mit [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) nicht.

Wenn Sie eine App mit DirectX 11 oder DirectX 12 entwickeln und eine eigene Swap-Kette zum Rendern von UI- oder Video-Inhalten erstellen, können Sie den verwendeten Farbraum angeben, indem Sie [IDXGISwapChain3::SetColorSpace1](https://msdn.microsoft.com/library/windows/desktop/dn903676) aufrufen.

## <a name="guidelines-for-ui-controls"></a>Richtlinien für Benutzeroberflächensteuerelemente

Es gibt mehrere Benutzeroberflächen-Steuerelemente, die auf mehreren Geräten gut funktionieren. Wenn diese jedoch auf Fernsehgeräten verwendet werden, müssen bestimmte Aspekte berücksichtigt werden. Informieren Sie sich über einige bewährte Methoden für die Verwendung dieser Steuerelemente beim Entwerfen für die 10 Fuß-Erfahrung.

### <a name="pivot-control"></a>Pivotsteuerelement

Ein [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) ermöglicht über verschiedene Header oder Registerkarten eine schnelle Navigation für Ansichten in einer App. Das Steuerelement unterstreicht jeweils den Header, der den Fokus hat. So wird bei der Nutzung von Gamepads/Remotesteuerungen deutlicher, welcher Header zurzeit ausgewählt ist.

![Pivotunterstreichung](images/designing-for-tv/pivot-underline.png)

Sie können die [Pivot.IsHeaderItemsCarouselEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabled.aspx)-Eigenschaft auf `true` festlegen, damit Pivots stets die gleiche Position haben und die Kopfzeile des ausgewählten Pivots nicht stets an die erste Position verschoben wird. Dies ist besser für große Geräte mit großen Bildschirmanzeigen wie Fernsehgeräte geeignet, da Kopfzeilenumbrüche Benutzer stark ablenken können. Wenn nicht alle Pivotkopfzeilen gleichzeitig auf den Bildschirm passen, wird eine Bildlaufleiste angezeigt, damit Kunden die restlichen Kopfzeilen sehen. Sie sollten jedoch sicherstellen, dass alle Kopfzeilen auf den Bildschirm passen, um eine optimale Erfahrung bereitzustellen. Weitere Informationen finden Sie unter [Registerkarten und Pivots](../controls-and-patterns/tabs-pivot.md).

### <a name="navigation-pane-a-namenavigation-pane"></a>Navigationsbereich <a name="navigation-pane">

Ein Navigationsbereich (auch *Hamburger-Menü* genannt) ist ein Navigationssteuerelement, das häufig in UWP-Apps verwendet wird. In der Regel handelt es sich um einen Bereich mit mehreren Optionen im Stil eine Liste, mit denen die Benutzer zu anderen Seiten wechseln können. Im Allgemeinen ist dieser Bereich zu Beginn reduziert, um Platz zu sparen. Der Benutzer kann ihn durch Klicken auf eine Schaltfläche öffnen.

Während Nav-Bereiche leicht über die Maus- und Touch-Bedienung genutzt werden können, ist die Bedienung über Gamepads/Fernbedienungen weniger praktisch. Der Benutzer muss hier immer erst zu einer Schaltfläche navigieren, um den jeweiligen Bereich zu öffnen. Aus diesem Grund empfiehlt es sich, den Navigationsbereich über die **Ansicht**-Schaltfläche zu öffnen und dem Benutzer das Öffnen über das Navigieren zum linken Rand der Seite zu ermöglichen. Ein Codebeispiel zum Implementieren dieses Entwurfsmusters finden Sie im Dokument [Programmgesteuerte Fokusnavigation](../input/focus-navigation-programmatic.md#split-view-code-sample). So steht dem Benutzer ein sehr einfacher Zugriff auf die Inhalte des Bereichs zur Verfügung. Weitere Informationen zum Verhalten von Navigationsbereichen auf unterschiedlichen Bildschirmgrößen sowie zu bewährten Vorgehensweisen für die Navigation mit Gamepads/Fernbedienungen finden Sie unter [Navigationsbereiche](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>CommandBar-Beschriftungen

Es empfiehlt sich, die Beschriftungen rechts neben den Symbolen auf einer [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) zu platzieren. So bleibt dessen Höhe minimiert und konsistent. Sie erreichen dies, indem Sie die Eigenschaft [CommandBar.DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) auf `CommandBarDefaultLabelPosition.Right` festlegen.

![CommandBar Beschriftungen rechts von Symbolen](images/designing-for-tv/commandbar.png)

Durch das Festlegen dieser Eigenschaft werden die Beschriftungen immer angezeigt. Dies ist bei einer 10-Fuß-Erfahrung vorteilhaft, denn es minimiert die Anzahl der durch den Benutzer erforderlichen Klicks. Auch für andere Gerätetypen ist dies ein hervorragendes Konzept.

### <a name="tooltip"></a>QuickInfo

Das Steuerelement [QuickInfo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) wurde eingeführt, um zusätzliche Informationen in der Benutzeroberfläche anzeigen zu können, wenn der Benutzer mit der Maus auf ein Element zeigt oder mit dem Finger auf ein Element tippt und den Finger darauf hält. Für Gamepad und Remote wird `Tooltip` kurz nachdem das Element den Fokus erhält angezeigt, bleibt für einen kurzen Zeitraum auf dem Bildschirm und verschwindet dann. Dieses Verhalten könnte ablenkend wirken, wenn zu viele `Tooltip`-Elemente verwendet werden. Versuchen Sie, `Tooltip`-Elemente bei Entwürfen für Fernsehgeräte zu vermeiden.

### <a name="button-styles"></a>Schaltflächenstile

Zwar funktionieren die Standard-UWP-Schaltflächen sehr gut auf TV-Bildschirmen, es gibt jedoch einige visuelle Stile für Schaltflächen, die noch besser auf die Benutzeroberfläche aufmerksam machen. Sie sollten diese für alle Plattformen in Erwägung ziehen, besonders für die 10-Fuß-Umgebung, bei der es sehr vorteilhaft ist, die Fokusposition klar und deutlich darzustellen. Weitere Informationen zu diesen Stilen finden Sie unter [Schaltflächen](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Geschachtelte UI-Elemente

Eine geschachtelte Benutzeroberfläche (User Interface, UI) verfügt über geschachtelte Elemente mit ausführbaren Aktionen, die in einem Container eingeschlossen sind, sodass sowohl die geschachtelten Elemente als auch die Container unabhängig voneinander den Fokus erhalten können.

Geschachtelte UI eignet sich für einige Eingabetypen, jedoch nicht immer für Gamepads und Fernbedienungen, da diese eine XY-Navigation erfordern. Beachten Sie die unter diesem Thema angeführten Richtlinien, um sicherzustellen, dass die Benutzeroberfläche für die 10-Fuß-Umgebung optimiert ist, und dass die Benutzer mühelos auf alle interaktiven Elemente zugreifen können. Eine gängige Lösung besteht darin, geschachtelte UI-Elemente in einem `ContextFlyout` zu platzieren (siehe [CommandBar und ContextFlyout](#commandbar-and-contextflyout)).

Weitere Informationen zur geschachtelten UI finden Sie unter [Geschachtelte UI bei Listenelementen](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

Das [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols.aspx)-Element ermöglicht Benutzern die Interaktion mit ihren Medien. Hierzu stellt es eine standardmäßige Wiedergabeumgebung bereit, in der Benutzer unter anderem die Wiedergabe starten und anhalten sowie Untertitel aktivieren können. Dieses Steuerelement ist eine Eigenschaft von [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.aspx) und unterstützt zwei Layoutoptionen: *einzeilig* und *zweizeilig*. Beim einzeiligen Layout befinden sich der Schieberegler und die Wiedergabeschaltflächen alle in einer Zeile, und die Schaltfläche für Wiedergabe/Pause wird links neben dem Schieberegler angezeigt. Beim zweizeiligen Layout befindet sich der Schieberegler in einer eigenen Zeile, und die Wiedergabeschaltflächen werden in einer Zeile darunter angezeigt. Bei Designs für die 10-Fuß-Erfahrung empfiehlt sich die Verwendung des zweizeiligen Layouts, da es eine bessere Gamepadnavigation ermöglicht. Wenn Sie das zweizeilige Layout aktivieren möchten, legen Sie in der [TransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.transportcontrols.aspx)-Eigenschaft von `MediaPlayerElement` für das `MediaTransportControls`-Element Folgendes fest: `IsCompact="False"`.

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

Weitere Informationen zum Hinzufügen von Medien zu Ihrer App finden Sie unter [Medienwiedergabe](../controls-and-patterns/media-playback.md).

> ![HINWEIS:] `MediaPlayerElement` steht erst ab der Windows10-Version1607 zur Verfügung. Bei Apps für niedrigere Windows10-Versionen muss stattdessen [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) verwendet werden. Die hier angegebenen Empfehlungen gelten auch für `MediaElement`, und der Zugriff auf die `TransportControls`-Eigenschaft erfolgt auf die gleiche Weise.

### <a name="search-experience"></a>Suchvorgang

Die Suche nach Inhalten ist eine der am häufigsten ausgeführten Funktionen in der 10-Fuß-Erfahrung. Wenn Ihre App eine Suchfunktion zur Verfügung stellt, sollte der Benutzer auf diese schnell über die **Y**-Taste auf dem Gamepad zugreifen können.

Die meisten Kunden sollten mit dieser Beschleunigungsfunktion bereits vertraut sein. Sie können aber auch der Benutzeroberfläche eine visuelle **Y**-Glyphe hinzufügen, um anzuzeigen, dass der Benutzer mit dieser Taste auf die Suchfunktion zugreifen kann. In diesem Fall müssen Sie das Symbol aus der Schriftart **Segoe Xbox MDL2 Symbol** (`&#xE3CC;` für XAML-Apps, `\E426` für HTML-Apps) verwenden, um die Konsistenz sicherzustellen.

> [!NOTE]
> Da die Schriftart **Segoe Xbox MDL2 Symbol** nur auf Xbox verfügbar ist, wird das Symbol auf Ihrem PC nicht ordnungsgemäß angezeigt. Es wird jedoch auf dem Fernseher angezeigt, nachdem Sie auf Xbox bereitstellen.

Da die **Y**-Taste nur auf dem Gamepad verfügbar ist, müssen Sie auch andere Möglichkeiten für den Zugriff auf die Suche zur Verfügung stellen, wie Schaltflächen in der Benutzeroberfläche. Andernfalls können einige Kunden möglicherweise nicht auf diese Funktion zugreifen.

In der 10-Fuß-Erfahrung ist es für Kunden oft einfacher, eine Vollbildschirm-Suche durchzuführen, da der Platz auf dem Display begrenzt ist. Unabhängig davon, ob Sie eine Voll- oder Teilbildschirm-Direktsuche haben, empfehlen wir, dass die Bildschirmtastatur bereits geöffnet ist, wenn der Benutzer die Suchfunktion öffnet, damit sofort Suchbegriffe eingegeben werden können.

## <a name="custom-visual-state-trigger-for-xbox"></a>Benutzerdefinierter visueller Zustandsauslöser für Xbox

Um Ihre UWP-App an die 10-Fuß-Erfahrung anzupassen, empfehlen wir Ihnen, das Layout zu ändern, wenn die App erkennt, dass sie auf einer Xbox-Konsole gestartet wurde. Eine Möglichkeit, um dies zu erreichen ist die Verwendung eines benutzerdefinierten *visuellen Zustandsauslösers*. Visuelle Zustandsauslöser sind besonders dann nützlich, wenn Sie in **Blend für Visual Studio** arbeiten möchten. Der folgende Codeausschnitt zeigt, wie ein visueller Zustandsauslöser für Xbox erstellt wird:

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Um den Auslöser zu erstellen, fügen Sie Ihrer App die folgende Klasse hinzu. Dies ist die Klasse, die in dem XAML-Code oben referenziert wird:

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

Nachdem Sie den benutzerdefinierten Auslöser hinzugefügt haben, wird Ihre App automatisch die Layoutänderungen ausführen, die Sie im XAML-Code angegeben haben, wenn sie erkennt, dass sie auf einer Xbox One-Konsole ausgeführt wird.

Sie können außerdem über Programmcode erkennen, ob die App auf Xbox ausgeführt wird, und dann entsprechende Anpassungen durchführen. Mit der folgenden einfachen Variable können Sie überprüfen, ob Ihre App auf Xbox ausgeführt wird:

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

Anschließend können Sie im Codeblock nach der Überprüfung die entsprechenden Anpassungen für Ihr UI vornehmen. Ein Beispiel hierfür finden Sie im [Beispiel für UWP-Farbe](#uwp-color-sample).

## <a name="summary"></a>Zusammenfassung

Beim Entwerfen für die 10 Fuß-Erfahrung müssen einige besondere Punkte berücksichtigt werden, die sich vom Entwerfen für andere Plattformen unterscheiden. Sie können durchaus Ihre UWP-App einfach zu Xbox One portieren, dies funktioniert. Die App wird jedoch nicht unbedingt für die 10-Fuß-Erfahrung optimiert und kann für den Benutzer mit Frustration verbunden sein. Wenn Sie die Richtlinien in diesem Artikel befolgen, stellen Sie so sicher, dass Ihre App genauso gut auf Fernsehgeräten funktioniert.

## <a name="related-articles"></a>Verwandte Artikel

- [Einführung der Geräte für UWP-Apps (Universelle Windows-Plattform)](index.md)
- [Interaktionen mit Gamepad und Fernbedienung](../input/gamepad-and-remote-interactions.md)
- [Sound in UWP-Apps](../style/sound.md)
