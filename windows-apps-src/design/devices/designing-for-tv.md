---
Description: Entwerfen Sie Ihre App so, dass sie auf Fernsehgeräten gut aussieht und ordnungsgemäß funktioniert.
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
ms.openlocfilehash: 491b67322c8b328c21446d50951daad61f15ad3d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175574"
---
# <a name="designing-for-xbox-and-tv"></a>Entwerfen für Xbox und Fernsehgeräte

Entwerfen Sie Ihre Windows-APP so, dass Sie gut aussieht und gut auf Xbox One-und TV-Bildschirmen funktioniert.

Unter [Gamepad-und Remote Steuerungs Interaktionen](../input/gamepad-and-remote-interactions.md) finden Sie Anleitungen zur Interaktion bei UWP-Anwendungen mit der *10-Fuß-* Erfahrung.

## <a name="overview"></a>Übersicht

Dank der universellen Windows-Plattform können Sie großartige Benutzeroberflächen für verschiedene Windows 10-Geräte erstellen.
Die Mehrzahl der durch das UWP Framework bereitgestellten Funktionen ermöglichen Apps, auf diesen Geräten ohne zusätzlichen Aufwand die gleiche Benutzeroberfläche (UI) zu verwenden.
Das Anpassen und Optimieren Ihrer App an Xbox One- und Fernsehbildschirme erfordert jedoch besondere Überlegungen.

Die Erfahrung, die Sie machen, wenn Sie auf dem Sofa sitzen und mittels eines Gamepads oder einer Fernbedienung mit Ihrem Fernsehgerät interagieren, wird als **3-Meter-Erfahrung** (10-Fuß-Erfahrung) bezeichnet.
Der Name kommt daher, dass sich der Benutzer im Allgemeinen ungefähr 3 Meter (10 Fuß) vom Bildschirm entfernt befindet.
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

_**Alle im Screenshot gezeigten Filme sind in Microsoft Movies & TV verfügbar.**_  

### <a name="captivating"></a>Fesselnd

Der große Bildschirm bietet äußerst faszinierende Erfahrungen, ähnlich wie ein Kino. Rand-Rand-Design, elegante Bewegungen und brillante Farben und Typografien eröffnen Ihren Apps neue Dimensionen. Seien Sie mutig, und bieten Sie ein ansprechendes Design.

![Xbox One Avatar-App](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>Optimierungen für die 10-Fuß-Erfahrung

Da Sie nun mit den Grundsätzen eines guten UWP-App-Designs für die 10-Fuß-Erfahrung vertraut sind, lesen Sie die folgenden Übersicht über die verschiedenen Arten, wie Sie Ihre App optimieren und eine hervorragende Benutzerumgebung bereitstellen können.

| Funktion        | BESCHREIBUNG           |
| -------------------------------------------------------------- |--------------------------------|
| [Anpassen von Benutzeroberflächenelementen](#ui-element-sizing)  | Die Universelle Windows-Plattform verwendet [Skalierung und effektive Pixel](../basics/design-and-ui-intro.md#effective-pixels-and-scaling), um die Benutzeroberfläche gemäß dem Anzeigeabstand zu skalieren. Wenn Sie verstehen, wie Sie Größen anpassen und auf Ihre Benutzeroberfläche anwenden, hilft Ihnen dies, Ihre App für die 10-Fuß-Umgebung zu optimieren.  |
|  [Fernsehsicherer Bereich](#tv-safe-area) | Die UWP vermeidet automatisch und standardmäßig die Anzeige von Benutzeroberflächenelementen in nicht fernsehsicheren Bereichen (nahe dem Bildschirmrand). Dies führt jedoch zu einem „Schachteleffekt“, so dass die Benutzeroberfläche einem Briefkastenschlitz ähnelt. Damit Ihre App auf Fernsehgeräten wirklich immersiv ist, müssen Sie diese so anpassen, dass sie sich auf Fernsehgeräten, die dies unterstützen, bis zu den Rändern erweitert wird. |
| [Farben](#colors)  |  Die UWP unterstützt Farbdesigns. Daher wird eine App, die das Systemdesign berücksichtigt, auf Xbox One standardmäßig auf **dark** festgelegt. Wenn Ihre App ein bestimmtes Farbdesign verwendet, sollten Sie daran denken, dass sich einige Farben nicht gut für Fernsehbildschirme eignen und daher vermieden werden sollten. |
| [Sound](../style/sound.md)    | Sounds spielen bei der 10 Fuß-Erfahrung eine wichtige Rolle. Sie helfen den Benutzern sich zu vertiefen und liefern Feedback. Die UWP bietet Funktionen, mit denen Sounds für allgemeine Steuerelemente automatisch aktiviert werden, wenn die App auf Xbox One ausgeführt wird. Erfahren Sie mehr über die in der Universellen Windows-Plattform integrierte Unterstützung von Sound, und erfahren Sie, wie Sie davon profitieren.    |
| [Richtlinien für Benutzeroberflächensteuerelemente](#guidelines-for-ui-controls)  |  Es gibt mehrere Benutzeroberflächen-Steuerelemente, die auf mehreren Geräten gut funktionieren. Wenn diese jedoch auf Fernsehgeräten verwendet werden, müssen bestimmte Aspekte berücksichtigt werden. Informieren Sie sich über einige bewährte Methoden für die Verwendung dieser Steuerelemente beim Entwerfen für die 10 Fuß-Erfahrung. |
| [Benutzerdefinierter visueller Zustandsauslöser für Xbox](#custom-visual-state-trigger-for-xbox) | Um Ihre UWP-App an die 10-Fuß-Erfahrung anzupassen, empfehlen wir Ihnen, einen benutzerdefinierten *visuellen Zustandsauslöser* zu verwenden, um das Layout zu ändern, wenn die App erkennt, dass sie auf einer Xbox-Konsole gestartet wurde. |

Zusätzlich zu den vorangehenden Entwurfs-und layoutüberlegungen gibt es eine Reihe von Optimierungs Optimierungen für [Gamepad und Remote Steuerung](../input/gamepad-and-remote-interactions.md) , die Sie beim Erstellen Ihrer APP berücksichtigen sollten.

| Funktion        | BESCHREIBUNG           |
| -------------------------------------------------------------- |--------------------------------|
| [XY-Fokusnavigation und -interaktion](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | Die **XY-Fokus Navigation** ermöglicht dem Benutzer, um die Benutzeroberfläche Ihrer APP zu navigieren. Dies begrenzt Benutzer jedoch auf eine Navigation nach oben, unten, links und rechts. In diesem Abschnitt finden Sie Empfehlungen für den Umgang mit diesen und anderen Überlegungen. |
| [Mausmodus](../input/gamepad-and-remote-interactions.md#mouse-mode)|Die XY-Fokus Navigation ist für einige Arten von Anwendungen, z. b. Karten oder das Zeichnen und Zeichnen von apps, nicht praktikabel oder sogar möglich. In diesen Fällen ermöglicht der **Maus Modus** Benutzern, mit einem Gamepad oder einer Remote Steuerung wie der Maus auf einem PC frei zu navigieren.|
| [Fokusanzeige](../input/gamepad-and-remote-interactions.md#focus-visual)  | Das visuelle Fokus Element ist ein Rahmen, der das aktuell fokussierte Benutzeroberflächen Element hervorhebt. Dadurch kann der Benutzer schnell die Benutzeroberfläche identifizieren, durch die Sie navigieren oder mit der Sie interagieren.  |
| [Fokus Einbindung](../input/gamepad-and-remote-interactions.md#focus-engagement) | Der Fokus Engagement erfordert, dass der Benutzer die **A/Select-** Schaltfläche in einem Gamepad oder der Remote Steuerung drückt, wenn ein Benutzeroberflächen Element den Fokus besitzt, um damit zu interagieren. |
| [Hardwaretasten](../input/gamepad-and-remote-interactions.md#hardware-buttons) | Das Gamepad und die Remote Steuerung bieten sehr unterschiedliche Schaltflächen und Konfigurationen. |

> [!NOTE]
> Die meisten Codeausschnitte in diesem Thema wurden in XAML/C# verfasst. Die Grundsätze und Konzepte gelten jedoch für alle UWP-Apps. Wenn Sie eine HTML/JavaScript-UWP-App für Xbox entwickeln, steht Ihnen die hervorragende [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki)-Bibliothek auf GitHub zur Verfügung.

## <a name="ui-element-sizing"></a>Anpassen von Benutzeroberflächenelementen

Da Benutzer von Apps in 10-Fuß-Umgebungen Fernbedienungen oder Gamepads verwenden und sich mehrere Meter vom Bildschirm entfernt befinden, gibt es einige Überlegungen in Bezug auf die Benutzeroberfläche, die Sie für Ihren Entwurf berücksichtigen müssen.
Stellen Sie sicher, dass die Benutzeroberfläche über eine geeignete Inhaltsdichte verfügt und nicht zu überladen ist, damit Benutzer einfach navigieren und Elemente auswählen können. Denken Sie daran: Einfachheit ist der Schlüssel.

### <a name="scale-factor-and-adaptive-layout"></a>Skalierungsfaktor und adaptives Layout

Der **Skalierungsfaktor** trägt dazu bei, dass Benutzeroberflächenelemente in der richtigen Größe für das Gerät angezeigt werden, auf dem die App ausgeführt wird.
Auf Desktops befindet sich diese Einstellung in **Einstellungen > System > Anzeige** als gleitender Wert.
Diese Einstellung ist auch auf Mobiltelefonen vorhanden, wenn das Gerät diese unterstützt.

![Ändern der Größe von Text, Apps und anderen Elementen](images/designing-for-tv/ui-scaling.png)

Auf Xbox One gibt es diese Systemeinstellung nicht. UWP-UI-Elemente, die für Fernsehgeräte angepasst werden, werden jedoch standardmäßig auf **200 %** für XAML-Apps und **150 %** für HTML-Apps skaliert.
Solange Benutzeroberflächenelemente für andere Geräte korrekt angepasst werden, werden sie auch für Fernsehgeräte angepasst.
Xbox One rendert Ihre App mit 1080p (1920 x 1080 Pixel). Stellen Sie daher mittels [adaptiver Techniken](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) sicher, dass die UI bei 960 x 540 px und einer Skalierung von 100 % (oder bei 1280 x 720 px und einer Skalierung von 100 % für HTML-Apps) gut aussieht, wenn Sie eine App von anderen Geräten wie PCs übertragen.

Das Entwerfen für Xbox unterscheidet sich etwas vom Entwerfen für PCs, da Sie lediglich eine Auflösung von 1920 x 1080 berücksichtigen müssen.
Es spielt keine Rolle, ob der Benutzer ein Fernsehgerät mit einer besseren Auflösung besitzt&mdash;UWP-Apps werden stets auf 1080p skaliert.

Bei der Ausführung auf Xbox One werden darüber hinaus korrekte Ressourcengrößen aus dem Satz mit 200 % (oder 150 % für HTML-Apps) für Ihre App aufgerufen, unabhängig von der Auflösung des Fernsehgeräts.

### <a name="content-density"></a>Inhaltsdichte

Denken Sie beim Entwerfen Ihrer App daran, dass Benutzer die Benutzeroberfläche aus der Entfernung sieht und mittels einer Fernbedienung oder einem Gamecontroller mit dieser interagiert. Dies dauert länger als die Verwendung von Maus- oder Toucheingaben.

#### <a name="sizes-of-ui-controls"></a>Größen von Benutzeroberflächensteuerelementen

Interaktive Benutzeroberflächenelemente sollten eine Mindesthöhe von 32 epx (effektive Pixel) besitzen. Dies ist die Standardeinstellung für allgemeine UWP-Steuerelemente. Wenn diese bei einer Skalierung von 200 % verwendet wird, ist sichergestellt, dass Benutzeroberflächenelemente aus der Entfernung erkennbar sind. Darüber hinaus trägt dies zur Reduzierung der Inhaltsdichte bei.

![UWP-Schaltfläche bei einer Skalierung von 100 % und 200 %](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>Anzahl der Klicks

Um Ihre Benutzeroberfläche zu vereinfachen, sollten Benutzer nicht mehr als **sechs Klicks** benötigen, wenn sie von einem Rand des Fernsehbildschirms zum anderen navigieren. Auch hier gilt der Grundsatz der **Einfachheit**. 

![6 Symbole insgesamt](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>Textgrößen

Wenden Sie die folgenden Faustregeln an, damit Ihre Benutzeroberfläche aus der Entfernung erkennbar ist:

* Haupttext und Lesen von Inhalten: mindestens 15 epx
* Nicht kritische Texte und ergänzende Inhalte: mindestens 12 epx

Wenn Sie in der Benutzeroberfläche einen größeren Text verwenden, sollten Sie eine Größe wählen, die die verfügbare Bildschirmfläche nicht zu sehr begrenzt, indem sie Platz beansprucht, der potenziell von anderen Inhalten ausgefüllt werden kann.

### <a name="opting-out-of-scale-factor"></a>Deaktivieren des Skalierungsfaktors

Wir empfehlen Ihnen, für Ihre App die Vorteile der Skalierungsfaktorunterstützung zu nutzen. Dies trägt dazu bei, dass sie auf allen Geräten korrekt ausgeführt wird, indem sie für jeden Gerätetyp skaliert wird.
Es ist jedoch möglich, dieses Verhalten zu deaktivieren und alle Benutzeroberflächenelemente für eine Skalierung von 100 % zu entwerfen. Beachten Sie, dass Sie den Skalierungsfaktor nicht in einen anderen Wert als 100 % ändern können.

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

Wir empfehlen Ihnen, bestimmte Benutzeroberflächenelemente zu verwenden, um die Benutzeroberfläche bis an die Ränder des Bildschirms zu erweitern und Benutzern eine immersivere Umgebung zu bieten. Dazu gehören [ScrollViewers](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [Navigationsbereiche](../controls-and-patterns/navigationview.md) und [CommandBars](/uwp/api/Windows.UI.Xaml.Controls.CommandBar).

Es ist jedoch auch wichtig, dass interaktive Elemente und Texte stets die Bildschirmränder vermeiden, um sicherzustellen, dass sie auf bestimmten Fernsehgeräten nicht abgeschnitten werden. Wir empfehlen Ihnen, nur nicht essentielle visuelle Elemente bis zu 5 % von den Rändern des Bildschirms entfernt zu zeichnen. Wie in [Anpassen von Benutzeroberflächenelementen](#ui-element-sizing) bereits erwähnt, nutzt eine UWP-App, die den Xbox One-Standardskalierungsfaktor von 200 % verwendet, einen Bereich von 960 x 540 epx. Sie sollten es daher vermeiden, in der Benutzeroberfläche Ihrer App essentielle Benutzerflächenelemente in den folgenden Bereichen zu platzieren:

- 27 epx vom oberen und unteren Rand
- 48 epx vom linken und rechten Rand

In den folgenden Abschnitten wird beschrieben, wie Sie Ihr UI bis an die Ränder des Bildschirms erweitern.

#### <a name="core-window-bounds"></a>Kernfenstergrenzen

Im Fall von UWP-Apps, die ausschließlich für die 10-Fuß-Erfahrung entwickelt werden, stellt die Verwendung von Kernfenstergrenzen die einfachere Option dar.

Fügen Sie in der `OnLaunched`-Methode von `App.xaml.cs` den folgenden Code hinzu:

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Mit dieser Codezeile wird das App-Fenster bis zu den Rändern des Bildschirms erweitert. Daher müssen Sie alle interaktiven und essentiellen Benutzeroberflächenelemente in den bereits beschriebenen fernsehsicheren Bereich verschieben. Kurzlebige Benutzeroberflächenelemente wie Kontextmenüs und geöffnete [Kombinationsfelder](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) bleiben automatisch im fernsehsicheren Bereich.

![Kernfenstergrenzen](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Bereichshintergründe

Navigationsbereiche werden in der Regel nahe am Rand des Bildschirms dargestellt. Daher sollte sich der Hintergrund in den nicht fernsehsicheren Bereich erstrecken, um hässliche Lücken zu vermeiden. Hierzu können Sie einfach die Hintergrundfarbe des Navigationsbereichs in der Hintergrundfarbe der App ändern.

Mithilfe der oben beschriebenen Kernfenstergrenzen können Sie Ihr UI an den Rändern des Bildschirms darstellen. Sie sollten dann jedoch positive Ränder für die [SplitView](/uwp/api/Windows.UI.Xaml.Controls.SplitView)-Inhalte nutzen, um diese innerhalb des fernsehsicheren Bereichs zu halten.

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

[CommandBar](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) ist ein weiteres Beispiel für einen Bereich, der häufig in der Nähe eines oder mehrerer Ränder der App positioniert ist. Daher sollte dessen Hintergrund auf Fernsehbildschirmen bis an die Ränder des Bildschirms erweitert werden. In der Regel gibt es auf der rechten Seite die Schaltfläche **Mehr** (dargestellt durch „...“), die weiter im fernsehsicheren Bereich angezeigt werden sollte. Im Folgenden finden Sie einige unterschiedliche Strategien, um die gewünschten Interaktionen und visuellen Effekte zu erzielen.

**Option 1**: Ändern der `CommandBar`-Hintergrundfarbe in transparent oder in die Farbe des Seitenhintergrunds:

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

Hierdurch sieht die `CommandBar` aus, als ob sie auf dem gleichen Hintergrund wie der Rest der Seite angezeigt wird, sodass sich der Hintergrund nahtlos bis an den Rand des Bildschirms erstreckt.

**Option 2**: Hinzufügen eines Hintergrund-Rechtecks, dessen Füllung die gleiche Farbe wie der `CommandBar`-Hintergrund hat und das unten der `CommandBar` und über dem Rest der Seite liegt:

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

Die UWP verfügt über Funktionen, die dafür sorgen, dass die Fokusanzeige weiter innerhalb der [VisibleBounds](/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds) angezeigt wird. Sie müssen jedoch Abstand hinzufügen, um sicherzustellen, dass für die Listen-/Rasterelemente ein Bildlauf in den Anzeigebereich des sicheren Bereichs durchgeführt werden kann. Genauer gesagt, müssen Sie für [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) oder [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) deren [ItemsPresenter](/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter) einen positiven Rand hinzufügen, wie im folgenden Codeausschnitt gezeigt:

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
> Dieser Codeausschnitt gilt speziell für `ListView`-Elemente. Legen Sie bei einem `GridView`-Stil das [TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype)-Attribut für [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) und [Style](/uwp/api/Windows.UI.Xaml.Style) auf `GridView` fest.

Wenn Ihre Anwendung auf Version 1803 oder höher ausgerichtet ist, können Sie das [UIElement. bringindeviewangeforderten-Ereignis](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)verwenden, um eine präzisere Kontrolle darüber zu erhalten, wie Elemente in die Ansicht aufgenommen werden. Sie können Sie in [ItemsPanel](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) ablegen, damit die **ListView**- / **GridView** Sie vor dem internen **ScrollViewer** abfängt, wie in den folgenden Code Ausschnitten:

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

Standardmäßig skaliert das universelle Windows-Plattform die Farben Ihrer APP auf den TV-sicheren Bereich. (Weitere Informationen finden Sie unter [TV-sichere Farben](#tv-safe-colors) ), sodass Ihre APP in jedem Fernsehgerät gut aussieht. Außerdem gibt es Verbesserungen, die Sie an der Gruppe von Farben vornehmen können, die Ihre APP verwendet, um die visuelle Darstellung im Fernsehen zu verbessern.

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

Solange Ihre APP eine Pinsel Ressource wie z. b. **systemcontrolforegroundaccentbrush**oder eine Color-Ressource (**systemaccentcolor**) verwendet oder stattdessen Akzentfarben direkt über die [uicolortype. Akzent *](/uwp/api/Windows.UI.ViewManagement.UIColorType) -API aufruft, werden diese Farben durch die auf Xbox One verfügbaren Akzentfarben ersetzt. Pinsel Farben mit hohem Kontrast werden ebenfalls auf der gleichen Weise wie auf einem PC und Telefon vom System abgerufen.

Weitere Informationen zu Akzentfarben im Allgemeinen finden Sie unter [Akzentfarbe](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Farbabweichungen zwischen Fernsehgeräten

Beachten Sie bei der Entwicklung von Apps für Fernsehgeräte, dass Farben sehr unterschiedlich dargestellt werden, abhängig von dem Fernsehgerät, auf dem sie gerendert werden. Gehen Sie nicht davon aus, dass die Farben genau wie auf Ihrem Bildschirm angezeigt werden. Wenn Ihre App geringfügige Farbunterschiede nutzt, um Teile der Benutzeroberfläche zu unterscheiden, könnten sich die Farben mischen und die Benutzer könnten verwirrt werden. Verwenden Sie Farben, die unterschiedlich genug sind, dass Benutzer sie unabhängig vom verwendeten Fernsehgerät deutlich unterscheiden können.

### <a name="tv-safe-colors"></a>Fernsehsichere Farben

Die RGB-Werte einer Farbe stellen die Intensität für Rot, Grün und Blau dar. Die Fernsehgeräte behandeln keine extrem Intensitäten sehr gut &mdash; , Sie können zu einem ungeraden geblenden Effekt führen oder auf bestimmten Fernsehgeräten ausgewaschen angezeigt werden. Darüber hinaus verursachen Farben mit hoher Intensität möglicherweise ein „Blooming“, d. h., Pixel in der Nähe beginnen, die gleichen Farben aufzurufen. Die Ansichten darüber, was als fernsehsichere Farben gelten kann, gehen zwar auseinander. Im Allgemeinen können Farben mit RGB-Werten zwischen 16 und 235 (oder 10-EB hexadezimal) jedoch sicher für Fernsehgeräte verwendet werden.

![Fernsehsicherer Farbbereich](images/designing-for-tv/tv-safe-colors-2.png)

In der Vergangenheit mussten apps auf der Xbox Ihre Farben so anpassen, dass Sie in diesen "TV-Safe"-Farbbereich fallen. ab dem Fall Creators Update skaliert Xbox One den vollständigen Inhalts Bereich jedoch automatisch in den Bereich TV-sicher. Dies bedeutet, dass sich die meisten App-Entwickler nicht mehr um TV-sichere Farben kümmern müssen.

> [!IMPORTANT]
> Bei Video Inhalten, die sich bereits im TV-sicheren Farbbereich befinden, wird dieser Farb Skalierungs Effekt bei der Wiedergabe mit [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk)nicht angewendet.

Wenn Sie eine APP unter Verwendung von DirectX 11 oder DirectX 12 entwickeln und eine eigene SwapChain zum Rendering der Benutzeroberfläche oder des Videos erstellen, können Sie den von Ihnen verwendeten Farbraum angeben, indem Sie [IDXGISwapChain3:: SetColorSpace1](/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1)aufrufen. Dadurch wird dem System mitgeteilt, ob Farben skaliert werden müssen.

## <a name="guidelines-for-ui-controls"></a>Richtlinien für Benutzeroberflächensteuerelemente

Es gibt mehrere Benutzeroberflächen-Steuerelemente, die auf mehreren Geräten gut funktionieren. Wenn diese jedoch auf Fernsehgeräten verwendet werden, müssen bestimmte Aspekte berücksichtigt werden. Informieren Sie sich über einige bewährte Methoden für die Verwendung dieser Steuerelemente beim Entwerfen für die 10 Fuß-Erfahrung.

### <a name="pivot-control"></a>Pivotsteuerelement

Ein [Pivot](/uwp/api/Windows.UI.Xaml.Controls.Pivot) ermöglicht über verschiedene Header oder Registerkarten eine schnelle Navigation für Ansichten in einer App. Das Steuerelement unterstreicht jeweils den Header, der den Fokus hat. So wird bei der Nutzung von Gamepads/Remotesteuerungen deutlicher, welcher Header zurzeit ausgewählt ist.

![Pivotunterstreichung](images/designing-for-tv/pivot-underline.png)

Sie können die [Pivot.IsHeaderItemsCarouselEnabled](/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty)-Eigenschaft auf `true` festlegen, damit Pivots stets die gleiche Position haben und die Kopfzeile des ausgewählten Pivots nicht stets an die erste Position verschoben wird. Dies ist besser für große Geräte mit großen Bildschirmanzeigen wie Fernsehgeräte geeignet, da Kopfzeilenumbrüche Benutzer stark ablenken können. Wenn nicht alle Pivotkopfzeilen gleichzeitig auf den Bildschirm passen, wird eine Bildlaufleiste angezeigt, damit Kunden die restlichen Kopfzeilen sehen. Sie sollten jedoch sicherstellen, dass alle Kopfzeilen auf den Bildschirm passen, um eine optimale Erfahrung bereitzustellen. Weitere Informationen finden Sie unter [Registerkarten und Pivots](../controls-and-patterns/pivot.md).

### <a name="navigation-pane"></a>Navigationsbereich <a name="navigation-pane" />

Ein Navigationsbereich (auch *Hamburger-Menü* genannt) ist ein Navigationssteuerelement, das häufig in UWP-Apps verwendet wird. In der Regel handelt es sich um einen Bereich mit mehreren Optionen im Stil eine Liste, mit denen die Benutzer zu anderen Seiten wechseln können. Im Allgemeinen ist dieser Bereich zu Beginn reduziert, um Platz zu sparen. Der Benutzer kann ihn durch Klicken auf eine Schaltfläche öffnen.

Während Nav-Bereiche leicht über die Maus- und Touch-Bedienung genutzt werden können, ist die Bedienung über Gamepads/Fernbedienungen weniger praktisch. Der Benutzer muss hier immer erst zu einer Schaltfläche navigieren, um den jeweiligen Bereich zu öffnen. Aus diesem Grund empfiehlt es sich, den Navigationsbereich über die **Ansicht**-Schaltfläche zu öffnen und dem Benutzer das Öffnen über das Navigieren zum linken Rand der Seite zu ermöglichen. Ein Code Beispiel zum Implementieren dieses Entwurfs Musters finden Sie Unterprogramm gesteuertes [Fokus Navigations](../input/focus-navigation-programmatic.md#split-view-code-sample) Dokument. So steht dem Benutzer ein sehr einfacher Zugriff auf die Inhalte des Bereichs zur Verfügung. Weitere Informationen zum Verhalten von Navigationsbereichen auf unterschiedlichen Bildschirmgrößen sowie zu bewährten Vorgehensweisen für die Navigation mit Gamepads/Fernbedienungen finden Sie unter [Navigationsbereiche](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>CommandBar-Beschriftungen

Es empfiehlt sich, die Beschriftungen rechts neben den Symbolen auf einer [CommandBar](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) zu platzieren. So bleibt dessen Höhe minimiert und konsistent. Sie erreichen dies, indem Sie die Eigenschaft [CommandBar.DefaultLabelPosition](/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) auf `CommandBarDefaultLabelPosition.Right` festlegen.

![CommandBar Beschriftungen rechts von Symbolen](images/designing-for-tv/commandbar.png)

Durch das Festlegen dieser Eigenschaft werden die Beschriftungen immer angezeigt. Dies ist bei einer 10-Fuß-Erfahrung vorteilhaft, denn es minimiert die Anzahl der durch den Benutzer erforderlichen Klicks. Auch für andere Gerätetypen ist dies ein hervorragendes Konzept.

### <a name="tooltip"></a>QuickInfo

Das Steuerelement [QuickInfo](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) wurde eingeführt, um zusätzliche Informationen in der Benutzeroberfläche anzeigen zu können, wenn der Benutzer mit der Maus auf ein Element zeigt oder mit dem Finger auf ein Element tippt und den Finger darauf hält. Für Gamepad und Remote wird `Tooltip` kurz nachdem das Element den Fokus erhält angezeigt, bleibt für einen kurzen Zeitraum auf dem Bildschirm und verschwindet dann. Dieses Verhalten könnte ablenkend wirken, wenn zu viele `Tooltip`-Elemente verwendet werden. Versuchen Sie, `Tooltip`-Elemente bei Entwürfen für Fernsehgeräte zu vermeiden.

### <a name="button-styles"></a>Schaltflächenstile

Zwar funktionieren die Standard-UWP-Schaltflächen sehr gut auf TV-Bildschirmen, es gibt jedoch einige visuelle Stile für Schaltflächen, die noch besser auf die Benutzeroberfläche aufmerksam machen. Sie sollten diese für alle Plattformen in Erwägung ziehen, besonders für die 10-Fuß-Umgebung, bei der es sehr vorteilhaft ist, die Fokusposition klar und deutlich darzustellen. Weitere Informationen zu diesen Stilen finden Sie unter [Schaltflächen](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Geschachtelte UI-Elemente

Eine geschachtelte Benutzeroberfläche (User Interface, UI) verfügt über geschachtelte Elemente mit ausführbaren Aktionen, die in einem Container eingeschlossen sind, sodass sowohl die geschachtelten Elemente als auch die Container unabhängig voneinander den Fokus erhalten können.

Geschachtelte UI eignet sich für einige Eingabetypen, jedoch nicht immer für Gamepads und Fernbedienungen, da diese eine XY-Navigation erfordern. Beachten Sie die unter diesem Thema angeführten Richtlinien, um sicherzustellen, dass die Benutzeroberfläche für die 10-Fuß-Umgebung optimiert ist, und dass die Benutzer mühelos auf alle interaktiven Elemente zugreifen können. Eine gängige Lösung besteht darin, die in einer eingefügten Benutzeroberflächen Elemente zu platzieren `ContextFlyout` .

Weitere Informationen zur geschachtelten UI finden Sie unter [Geschachtelte UI bei Listenelementen](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

Das [MediaTransportControls](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls)-Element ermöglicht Benutzern die Interaktion mit ihren Medien. Hierzu stellt es eine standardmäßige Wiedergabeumgebung bereit, in der Benutzer unter anderem die Wiedergabe starten und anhalten sowie Untertitel aktivieren können. Dieses Steuerelement ist eine Eigenschaft von [MediaPlayerElement](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) und unterstützt zwei Layoutoptionen: *einzeilig* und *zweizeilig*. Beim einzeiligen Layout befinden sich der Schieberegler und die Wiedergabeschaltflächen alle in einer Zeile, und die Schaltfläche für Wiedergabe/Pause wird links neben dem Schieberegler angezeigt. Beim zweizeiligen Layout befindet sich der Schieberegler in einer eigenen Zeile, und die Wiedergabeschaltflächen werden in einer Zeile darunter angezeigt. Bei Designs für die 10-Fuß-Erfahrung empfiehlt sich die Verwendung des zweizeiligen Layouts, da es eine bessere Gamepadnavigation ermöglicht. Wenn Sie das zweizeilige Layout aktivieren möchten, legen Sie in der [TransportControls](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols)-Eigenschaft von `MediaPlayerElement` für das `MediaTransportControls`-Element Folgendes fest: `IsCompact="False"`.

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

> ![HINWEIS:] `MediaPlayerElement` steht erst ab der Windows 10-Version 1607 zur Verfügung. Bei Apps für niedrigere Windows 10-Versionen muss stattdessen [MediaElement](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) verwendet werden. Die hier angegebenen Empfehlungen gelten auch für `MediaElement`, und der Zugriff auf die `TransportControls`-Eigenschaft erfolgt auf die gleiche Weise.

### <a name="search-experience"></a>Suchoberfläche

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

Anschließend können Sie im Codeblock nach der Überprüfung die entsprechenden Anpassungen für Ihr UI vornehmen. 

## <a name="summary"></a>Zusammenfassung

Beim Entwerfen für die 10 Fuß-Erfahrung müssen einige besondere Punkte berücksichtigt werden, die sich vom Entwerfen für andere Plattformen unterscheiden. Sie können durchaus Ihre UWP-App einfach zu Xbox One portieren, dies funktioniert. Die App wird jedoch nicht unbedingt für die 10-Fuß-Erfahrung optimiert und kann für den Benutzer mit Frustration verbunden sein. Wenn Sie die Richtlinien in diesem Artikel befolgen, stellen Sie so sicher, dass Ihre App genauso gut auf Fernsehgeräten funktioniert.

## <a name="related-articles"></a>Verwandte Artikel

- [Geräte Einführung für Windows-apps](index.md)
- [Interaktionen mit Gamepad und Fernbedienung](../input/gamepad-and-remote-interactions.md)
- [Sound in UWP-Apps](../style/sound.md)