---
description: Erstellen Sie Windows-apps mit intuitiver und ausgeprägter Benutzerinteraktion, die für die Berührungs Optimierung optimiert sind, aber funktional auf Eingabegeräten einheitlich sind.
title: Toucheingabe-Interaktionen
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: Touch, Zeiger, Eingabe, Benutzerinteraktion
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f173c90d43b7be795b8a87fe14dd3de9d6284da1
ms.sourcegitcommit: da44cb95946440cd06ff36254d42ecefcdd87ce2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93063012"
---
# <a name="touch-interactions"></a>Toucheingabe-Interaktionen


Gehen Sie beim Entwerfen Ihrer App davon aus, dass die Benutzer in erster Linie die Toucheingabe als Eingabemethode verwenden werden. Wenn Sie UWP-Steuerelemente verwenden, ist keine zusätzliche Programmierung für die Unterstützung von Touchpad, Maus und Zeichen-/Eingabestift erforderlich, da sie in UWP-Apps standardmäßig bereitgestellt wird.

Bedenken Sie dabei jedoch, dass eine für Toucheingaben optimierte Benutzeroberfläche einer herkömmlichen Benutzeroberfläche nicht in jedem Fall überlegen ist. Beide haben Vor- und Nachteile, die je nach Technologie oder Anwendung unterschiedlich sein können. Bei der Umstellung auf eine Touch-First-Benutzeroberfläche ist es wichtig, die wesentlichen Unterschiede zwischen Touch, Touchpad, Stift/Stift, Maus und Tastatureingaben zu verstehen.

> **Wichtige APIs** : [**Windows. UI. XAML. Input**](/uwp/api/Windows.UI.Xaml.Input), [**Windows. UI. Core**](/uwp/api/Windows.UI.Core), [**Windows. Devices. Input**](/uwp/api/Windows.Devices.Input)


Viele Geräte verfügen über Multitouch-Bildschirme, die eine Eingabe mit einem oder mehreren Fingern (oder Berührungskontakten) unterstützen. Die Berührungskontakte und ihre Bewegung werden als Touchbewegungen und Manipulationen interpretiert, um verschiedene Benutzerinteraktionen zu unterstützen.

Die Windows-app enthält eine Reihe von verschiedenen Mechanismen für die Handhabung von Finger Eingaben, sodass Sie eine immersive Benutzererfahrung erstellen können, die Ihre Benutzer zuverlässig durchsuchen können. Hier werden die Grundlagen der Verwendung von Berührungs Eingaben in einer Windows-App behandelt.

Interaktionen per Toucheingabe erfordern drei Dinge:

- Einen berührungsempfindlichen Bildschirm.
- Direkter Kontakt von einem oder mehreren Fingern auf dem Bildschirm (oder in der Nähe des Bildschirms, wenn er über Näherungssensoren verfügt und die Erkennung des Daraufzeigens unterstützt).
- Bewegung der Berührungskontakte (oder das Fehlen der Bewegung basierend auf einem Zeitschwellenwert).

Mögliche vom Berührungssensor bereitgestellte Eingabedaten:

- Interpretation als physische Geste für die direkte Manipulation von einem oder mehreren UI-Elementen (z. B. Schwenken, Drehen, Vergrößern/Verkleinern oder Verschieben). Die Interaktion mit einem Element über das zugehörige Eigenschaftenfenster, Dialogfeld oder ein anderes UI-Angebot gilt dagegen als indirekte Manipulation.
- Erkennung als alternative Eingabemethode, z. B. Maus oder Stift.
- Wird zum Ergänzen oder Ändern von Aspekten anderer Eingabemethoden verwendet, z. B. zum Verwischen eines mit einem Stift gezeichneten Freihandstrichs.

In der Regel beinhaltet die Toucheingabe die direkte Manipulation eines Elements auf dem Bildschirm. Das Element reagiert sofort auf alle Berührungen in seinem Treffertestbereich und reagiert entsprechend auf alle nachfolgenden Bewegungen des Touchkontakts, einschließlich dessen Entfernung.

Benutzerdefinierte Touchgesten und Interaktionen sollten sorgfältig entworfen werden. Sie sollten intuitiv, reaktionsfähig und leicht auffindbar sein und eine optimale Benutzererfahrung mit Ihrer App sicherstellen.

Stellen Sie sicher, dass die App-Funktionen konsistent auf allen unterstützten Eingabegerätetypen verfügbar sind. Verwenden Sie bei Bedarf eine Form von indirektem Eingabemodus, z. B. die Texteingabe für Tastaturinteraktionen oder UI-Angebote für Maus und Stift.

Bedenken Sie, dass herkömmliche Eingabegeräte (wie Maus und Tastatur) für viele Benutzer vertraut sind und gerne verwendet werden. Sie bieten häufig Faktoren wie Geschwindigkeit, Genauigkeit und klares Feedback, die bei der Toucheingabe unter Umständen nicht gegeben sind.

Mit den einzigartigen und speziellen Interaktionserfahrungen für alle Eingabegeräte unterstützen Sie eine breite Palette an Funktionen und Einstellungen und wenden sich an eine möglichst breite Zielgruppe. Auf diese Weise gewinnen Sie mehr Kunden für Ihre App.

## <a name="compare-touch-interaction-requirements"></a>Vergleich der Anforderungen für Toucheingabe-Interaktionen

In der folgenden Tabelle werden einige der Unterschiede zwischen Eingabegeräten angezeigt, die Sie beim Entwerfen von Touchscreen-optimierten Windows-apps berücksichtigen sollten.

<table>
<tbody><tr><th>Faktor</th><th>Toucheingabe-Interaktionen</th><th>Interaktionen über Maus, Tastatur oder Zeichen- bzw. Eingabestift</th><th>Touchpad</th></tr>
<tr><td rowspan="3">Precision</td><td>Der Kontaktbereich einer Fingerspitze ist größer als eine einzige xy-Koordinate. Dadurch erhöht sich die Wahrscheinlichkeit, dass Befehle unbeabsichtigt aktiviert werden.</td><td>Maus und Zeichen- oder Eingabestift liefern eine präzise xy-Koordinate.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Die Form des Kontaktbereichs ändert sich während der Bewegung.  </td><td>Mausbewegungen und Striche mit Zeichen- oder Eingabestift liefern eine präzise xy-Koordinate. Die Tastatur hat einen expliziten Fokus.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Es gibt keinen Mauscursor, der die Zielbestimmung unterstützt.</td><td>Der Fokus von Mauscursor, Zeichen- oder Eingabestiftcursor und Tastatur unterstützt die Zielbestimmung.</td><td>Identisch mit der Maus.</td></tr>
<tr><td rowspan="3">Menschliche Anatomie</td><td>Bewegungen mit den Fingerspitzen sind ungenau, da es schwierig ist, mit den Fingern eine lineare Bewegung auszuführen. Dies liegt an der Krümmung der Handgelenke und der Anzahl der an der Bewegung beteiligten Gelenke.</td><td>Es ist leichter, mit der Maus oder mit einem Zeichen- oder Eingabestift eine Bewegung in gerader Linie auszuführen, da die Hand, die diese Geräte steuert, eine kürzere physische Strecke zurücklegen muss als der Cursor auf dem Bildschirm.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Manche Bereiche der für die Fingereingabe vorgesehenen Oberfläche eines Anzeigegeräts sind aufgrund der Fingerhaltung und der Tatsache, dass der Benutzer das Gerät halten muss, möglicherweise schwer zu erreichen.</td><td>Maus und Zeichen- oder Eingabestift können alle Bereiche des Bildschirms erreichen, während über die Aktivierreihenfolge der Zugriff auf alle Steuerelemente möglich sein sollte. </td><td>Haltung und Druck der Finger spielen dabei eventuell auch eine Rolle.</td></tr>
<tr><td>Objekte werden möglicherweise durch mindestens eine Fingerspitze oder die Hand des Benutzers verdeckt. Dies wird als Okklusion bezeichnet.</td><td>Indirekte Eingabegeräte verursachen keine Okklusion.</td><td>Identisch mit der Maus.</td></tr>
<tr><td>Objektstatus</td><td>Bei der Fingereingabe wird ein Modell mit zwei Zuständen verwendet: Die für die Fingereingabe vorgesehene Oberfläche eines Anzeigegeräts wird entweder berührt (ein) oder nicht berührt (aus). Es gibt keinen Zeigezustand, der zusätzliches visuelles Feedback auslösen kann.</td><td>
<p>Maus, Zeichen- oder Eingabestift und Tastatur machen ein Modell mit drei Zuständen verfügbar: oben (aus), unten (ein) und Zeigen (Fokus).</p>
<p>Mit Zeigen können Benutzer QuickInfos zu UI-Elementen erforschen und kennenlernen. Zeigen und Fokus können vermitteln, welche Objekte interaktiv sind, und außerdem die Zielbestimmung erleichtern. 
</p>
</td><td>Identisch mit der Maus.</td></tr>
<tr><td rowspan="2">Umfangreiche Interaktionen</td><td>Unterstützt die Mehrfingereingabe: mehrere Eingabepunkte (Fingerspitzen) auf einer für die Fingereingabe vorgesehenen Oberfläche.</td><td>Unterstützt einen einzigen Eingabepunkt.</td><td>Identisch mit der Fingereingabe.</td></tr>
<tr><td>Unterstützt die direkte Manipulation von Objekten durch Bewegungen wie beispielsweise Tippen, Ziehen, Zusammendrücken und Drehen.</td><td>Keine Unterstützung für die direkte Manipulation, da Maus, Zeichen- oder Eingabestift und Tastatur indirekte Eingabegeräte sind.</td><td>Identisch mit der Maus.</td></tr>
</tbody></table>

> [!NOTE]
> Die indirekte Eingabe hat den Vorteil, dass sie über 25 Jahre optimiert wurde. Features wie durch Zeigen ausgelöste QuickInfos wurden als Lösung für die Erforschung der UI speziell für die Eingabe über Touchpad, Maus, Zeichen- oder Eingabestift und Tastatur entworfen. Solche UI-Funktionen wurden neu entworfen, um der umfassenden Toucheingabefunktion gerecht zu werden, ohne die Benutzererfahrung auf den anderen Geräten zu beeinträchtigen.

 

## <a name="use-touch-feedback"></a>Verwenden von Feedback für die Fingereingabe

Das entsprechende visuelle Feedback bei Interaktionen mit ihrer App hilft Benutzern dabei, die Art und Weise zu erkennen, zu erlernen und anzupassen, wie ihre Interaktionen von der APP und der Windows-Plattform interpretiert werden. Visuelles Feedback kann auf erfolgreiche Interaktionen hinweisen, über den Systemstatus informieren, das Gefühl der Kontrolle verstärken, Fehler verringern, Benutzern das Verständnis des Systems und des Eingabegeräts erleichtern und zu Interaktionen ermutigen.

Visuelles Feedback ist wichtig, wenn der Benutzer die Fingereingabe für Aktivitäten verwendet, bei denen Positionsgenauigkeit gefragt ist. Zeigen Sie immer Feedback an, wenn Toucheingabe erkannt wird, damit der Benutzer die von der App und den Steuerelementen definierten angepassten Zielbestimmungsregeln versteht.


## <a name="targeting"></a>Zielgruppenadressierung

Die Zielbestimmung wird durch Folgendes optimiert:

- Größe der Toucheingabeziele

    Klare Richtlinien für die Größe gewährleisten, dass Anwendungen eine angenehme UI bereitstellen, die Objekte und Steuerelemente enthält, die als Ziele einfach und sicher zu erreichen sind.

- Kontaktgeometrie

    Der gesamte Kontaktbereich des Fingers wird verwendet, um das wahrscheinlichste Zielobjekt zu ermitteln.

- Scrubbing (Bereinigung)

    Elemente in einer Gruppe können leicht erneut als Ziele bestimmt werden, indem der Finger zwischen ihnen gezogen wird (beispielsweise Optionsfelder). Das aktuelle Element wird aktiviert, wenn der Finger gehoben wird.

- Wackeln

    Eng angeordnete Elemente (beispielsweise Hyperlinks) können leicht erneut als Ziel bestimmt werden, indem Sie mit dem Finger auf das Element drücken und ohne Ziehen mit dem Finger leicht über den Elementen hin- und herwackeln. Aufgrund von Okklusion wird das aktuelle Element durch eine QuickInfo oder die Statusleiste identifiziert und aktiviert, wenn der Finger gehoben wird.

## <a name="accuracy"></a>Genauigkeit

Berücksichtigen Sie beim Design ungenaue Interaktionen, indem Sie Folgendes verwenden:

- Andockpunkte, die bei Interaktionen der Benutzer mit dem Inhalt das Anhalten an der gewünschten Position erleichtern.
- Direktionale "Rails", die beim vertikalen oder horizontalen Schwenken hilfreich sein können, auch wenn die Hand in einem kleinen Bogen bewegt wird. Weitere Informationen finden Sie unter [Richtlinien für das Schwenken](guidelines-for-panning.md).

## <a name="occlusion"></a>Okklusion

Okklusion durch Finger und Hand können Sie durch Folgendes vermeiden:

- Größe und Positionierung der UI

    Machen Sie die UI-Elemente groß genug, damit sie nicht vollständig von einem Fingerspitzen-Kontaktbereich verdeckt werden können.

    Positionieren Sie Menüs und Popups möglichst immer über dem Kontaktbereich.

- QuickInfos

    Wenn der Benutzer den Finger auf einem Objekt liegen lässt, sollten QuickInfos angezeigt werden. Dies dient der Beschreibung der Objektfunktionen. Der Benutzer kann verhindern, dass die QickInfo angezeigt wird, wenn er die Fingerspitze von Objekt herunterzieht.

    Versetzen Sie bei kleinen Objekten die QuickInfos, damit sie nicht vom Fingerspitzen-Kontaktbereich verdeckt werden. Dies ist hilfreich für die Zielbestimmung.

- Präzision durch Ziehpunkte

    Wenn Präzision erforderlich ist (beispielsweise bei der Textauswahl), stellen Sie versetzte Auswahlziehpunkte bereit, um die Genauigkeit zu verbessern. Weitere Informationen finden Sie unter [Richtlinien für die Auswahl von Text und Images (Windows-Runtime-Apps)](guidelines-for-textselection.md).

## <a name="timing"></a>Zeitliche Steuerung

Vermeiden Sie zeitlich festgelegte Modusänderungen und verwenden Sie stattdessen direkte Manipulationen. Bei der direkten Manipulation wird die direkte physische Handhabung eines Objekts in Echtzeit simuliert. Das Objekt reagiert, wenn die Finger bewegt werden.

Eine zeitlich festgelegte Interaktion dagegen tritt nach einer Fingereingabeinteraktion auf. Zeitlich festgelegte Interaktionen bestimmen normalerweise abhängig von unsichtbaren Schwellenwerten wie Zeit, Entfernung oder Geschwindigkeit, welcher Befehl ausgeführt werden soll. Bei zeitlich festgelegten Interaktionen erfolgt visuelles Feedback erst, wenn das System die Aktion ausführt.

Die direkte Manipulation bietet gegenüber zeitlich festgelegten Interaktionen eine Reihe von Vorteilen:

- Sofortiges visuelles Feedback bei Interaktionen weckt die Aufmerksamkeit der Benutzer und vermittelt ihnen ein Gefühl von Sicherheit und Kontrolle.
- Direkte Manipulationen sorgen für mehr Sicherheit beim Erforschen eines Systems, da sie umkehrbar sind, das heißt, Benutzer können ihre Aktionen leicht auf logische und intuitive Weise Schritt für Schritt rückgängig machen.
- Interaktionen, die direkte Auswirkungen auf Objekte haben und reale Interaktionen nachahmen, sind intuitiver und leichter zu finden und zu merken. Sie sind nicht auf schwer verständliche oder abstrakte Interaktionen angewiesen.
- Zeitlich festgelegte Interaktionen können schwer auszuführen sein, da die Benutzer willkürliche und unsichtbare Schwellenwerte erreichen müssen.

Darüber hinaus wird Folgendes dringend empfohlen:

- Manipulationen sollten nicht anhand der Anzahl der verwendeten Finger unterschieden werden.
- Die Interaktionen sollten zusammengesetzte Manipulationen unterstützen. Beispiel: Zoomen durch Zusammendrücken und gleichzeitiges Ziehen der Finger, um etwas zu verschieben.
- Die Interaktionen sollten nicht anhand der Zeit unterschieden werden. Eine Interaktion sollte unabhängig von der Ausführungsdauer immer zum gleichen Ergebnis führen. Zeitbasierte Aktivierungen führen zu obligatorischen Verzögerungen für Benutzer und beeinträchtigen die immersive Natur der direkten Manipulation und die Wahrnehmung der Reaktion des Systems.

    > [!NOTE]
    > Eine Ausnahme besteht darin, dass Sie bestimmte zeitgesteuerte Interaktionen verwenden, um das Erlernen und durchsuchen zu unterstützen (z. b. Drücken und halten).

- Entsprechende Beschreibungen und visuelle Hinweise haben einen großen Einfluss auf die Verwendung erweiterter Interaktionen.


## <a name="app-views"></a>App-Ansichten


Mithilfe der Einstellungen für Bewegungen/Bildläufe und Zoomstufen können Sie die Interaktionsmöglichkeiten für Benutzer Ihrer App-Ansichten optimieren. Die App-Ansicht bestimmt, wie ein Benutzer auf Ihre App und deren Inhalte zugreift und diese manipuliert. Ansichten stellen außerdem bestimmte Verhaltensweisen bereit, beispielsweise das Trägheitsverhalten, das „Springen“ an Inhaltsgrenzen und die Andockpunkte.

Die Einstellungen für Verschieben und Scrollen des [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)-Steuerelements legen fest, wie Benutzer in einer Ansicht navigieren können, wenn der Inhalt der Ansicht über den Viewport hinausgeht. Eine Einzelansicht kann beispielsweise eine Seite in einem Magazin oder Buch, die Ordnerstruktur eines Computers, eine Dokumentbibliothek oder ein Fotoalbum sein.

Zoomeinstellungen gelten sowohl für den optischen Zoom (unterstützt vom [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)-Steuerelement) als auch für das [**Semantic Zoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Steuerelement. Der semantische Zoom ist eine für Toucheingaben optimierte Technik, um große Bestände verwandter Daten oder Inhalte in einer einzelnen Ansicht darzustellen und in diesen zu navigieren. Dabei werden zwei unterschiedliche Klassifizierungsmodi (oder Zoomstufen) verwendet. Dies ist mit dem Verschieben und dem Durchführen von Bildläufen innerhalb einer einzelnen Ansicht vergleichbar. Verschieben und Bildläufe können in Verbindung mit dem semantischen Zoom verwendet werden.

Verwenden Sie App-Ansichten und -Ereignisse zum Ändern des Verhaltens für Schwenken/Bildlauf und Zoom. Dies kann für eine flüssigere Interaktion sorgen, als dies mit der Behandlung von Zeiger- und Gestikereignissen möglich ist.

Weitere Informationen zu app-Ansichten finden Sie unter Steuer [Elemente, Layouts und Text](../basics/index.md).

## <a name="custom-touch-interactions"></a>Benutzerdefinierte Touchinteraktionen


Wenn Sie eine eigene Interaktionsunterstützung implementieren, sollten Sie daran denken, dass die Benutzer eine intuitive Umgebung erwarten, die eine direkte Interaktion mit den UI-Elementen der App ermöglicht. Es empfiehlt sich, die benutzerdefinierten Interaktionen auf der Basis der Plattformsteuerelementbibliotheken zu modellieren, um auf diese Weise für eine konsistente und intuitive Benutzerumgebung zu sorgen. Die Steuerelemente in diesen Bibliotheken bieten umfassende Funktionen für Benutzerinteraktionen wie Standardinteraktionen, animierte Bewegungseffekte, visuelles Feedback und Barrierefreiheit. Erstellen Sie benutzerdefinierte Interaktionen nur dann, wenn ein eindeutiger, klar umrissener Bedarf besteht und es keine Basisinteraktion gibt, die das gewünschte Szenario unterstützt.

Zum Bereitstellen von angepasster Toucheingabeunterstützung können Sie verschiedene [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)-Ereignisse behandeln. Diese Ereignisse sind in drei verschiedene Abstraktionsschichten gruppiert.

- Statische Gestikereignisse werden ausgelöst, wenn eine Interaktion abgeschlossen ist. Gesten Ereignisse umfassen [**getippt**](/uwp/api/windows.ui.xaml.uielement.tapped), [**Double**](/uwp/api/windows.ui.xaml.uielement.doubletapped)-, [**righttippt**](/uwp/api/windows.ui.xaml.uielement.righttapped)und [**halten**](/uwp/api/windows.ui.xaml.uielement.holding).

    Sie können Gestikereignisse für bestimmte Elemente deaktivieren, indem Sie [**IsTapEnabled**](/uwp/api/windows.ui.xaml.uielement.istapenabled), [**IsDoubleTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isdoubletapenabled), [**IsRightTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isrighttapenabled) und [**IsHoldingEnabled**](/uwp/api/windows.ui.xaml.uielement.isholdingenabled) auf **false** festlegen.

- Zeiger Ereignisse, wie z. b. [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) und [**pointerverschiebe**](/uwp/api/windows.ui.xaml.uielement.pointermoved) , bieten ausführliche Details zu jedem Berührungs Kontakt, einschließlich Zeiger Bewegung und der Möglichkeit, Press-und releaseereignisse zu unterscheiden.

    Bei einem Zeiger handelt es sich um eine generische Eingabeart mit einem vereinheitlichten Ereignismechanismus. Er stellt grundlegende Informationen (wie die Bildschirmposition) für die aktive Eingabequelle (Touch, Touchpad, Maus oder Stift) zur Verfügung.

- Manipulationsgestikereignisse wie [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted) weisen auf eine fortlaufende Interaktion hin. Sie werden ausgelöst, wenn der Benutzer ein Element berührt, und bleiben so lange aktiv, bis der Benutzer den bzw. die Finger vom Element hebt oder die Manipulation abgebrochen wird.

    Manipulationsereignisse umfassen Multi-Touch-Interaktionen wie Zoomen, Schwenken und Drehen sowie Interaktionen, die Trägheits- und Geschwindigkeitsdaten nutzen (z. B. Ziehen). Die von den Bearbeitungsereignissen bereitgestellten Informationen identifizieren nicht die Form der ausgeführten Interaktion. Stattdessen enthalten sie Daten, z. B. zu Position, Übersetzungsdelta und Geschwindigkeit. Mit diesen Touchdaten können Sie die Art der auszuführenden Interaktion ermitteln.

Hier sehen Sie den grundlegenden Satz von Touchgesten, die von der UWP unterstützt werden.

| Name           | type                 | Description                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| Tippen            | Statische Geste       | Der Bildschirm wird mit einem Finger berührt, und der Finger wird wieder angehoben.                                            |
| Drücken und Halten | Statische Geste       | Ein Finger berührt den Bildschirm und bleibt auf dem Bildschirm.                                      |
| Slide (Folie)          | Manipulationsgeste | Mindestens ein Finger berührt den Bildschirm und bewegt sich in die gleiche Richtung.                   |
| Swipe          | Manipulationsgeste | Mindestens ein Finger berührt den Bildschirm und bewegt sich um eine kurze Distanz in die gleiche Richtung.  |
| Drehen           | Manipulationsgeste | Mindestens zwei Finger berühren den Bildschirm und führen eine Kreisbewegung im oder gegen den Uhrzeigersinn aus. |
| Zusammendrücken          | Manipulationsgeste | Mindestens zwei Finger berühren den Bildschirm und bewegen sich dichter zusammen.                         |
| Stretch        | Manipulationsgeste | Mindestens zwei Finger berühren den Bildschirm und bewegen sich weiter auseinander.                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>Gestikereignisse


In der [Steuerelementliste](../controls-and-patterns/index.md) finden Sie ausführliche Informationen zu einzelnen Steuerelementen.

## <a name="pointer-events"></a>Zeigerereignisse


Zeigerereignisse werden durch eine Vielzahl von aktiven Eingabequellen ausgelöst, darunter Toucheingabe, Touchpad, Stift und Maus (sie ersetzen herkömmliche Mausereignisse.)

Zeigerereignisse basieren auf einen einzigen Eingabepunkt (Finger, Stiftspitze, Mauszeiger) und unterstützen keine geschwindigkeitsbasierten Interaktionen.

Nachfolgend finden Sie eine Liste der Zeigerereignisse und der jeweiligen Ereignisargumente.

| Ereignis oder Klasse                                                       | Description                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)             | Tritt auf, wenn ein Finger den Bildschirm berührt.               |
| [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)           | Tritt auf, wenn dieser Berührungskontakt gelöst wird.                |
| [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)                 | Tritt auf, wenn der Mauszeiger über den Bildschirm gezogen wird.         |
| [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)             | Tritt auf, wenn der Zeiger in den Treffertestbereich eines Elements eintritt. |
| [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)               | Tritt auf, wenn der Zeiger aus dem Treffertestbereich eines Elements austritt.  |
| [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)           | Tritt auf, wenn ein Touchkontakt nicht normal verloren geht.               |
| [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)     | Tritt auf, wenn ein Zeiger durch ein anderes Element erfasst wird.    |
| [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)   | Tritt auf, wenn sich der Delta Wert eines Mausrades ändert und wenn das Touchpad fixiert wird.         |
| [**Pointerroutedebug-Elemente**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) | Stellt Daten für alle Zeigerereignisse bereit.                         |

 

Das folgende Beispiel zeigt, wie die Ereignisse [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) und [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) verwendet werden, um eine Tippinteraktion für ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Objekt zu behandeln ist.

Zuerst wird ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) mit dem Namen `touchRectangle` in Extensible Application Markup Language (XAML) erstellt.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
Als Nächstes werden Listener für die Ereignisse [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) und [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) festgelegt.

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerExited);
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

Schließlich vergrößert der [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)-Ereignishandler die [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) und [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) des [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), während die [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) und [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)-Ereignishandler die **Höhe** und die **Breite** wieder auf ihre Anfangswerte zurücksetzen.

```cpp
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```cs
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```vb
' Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Pointer moved outside Rectangle hit test area.
    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <a name="manipulation-events"></a>Bearbeitungsereignisse


Verwenden Sie Manipulationsereignisse, wenn Sie in Ihrer App Mehrfingereingabe-Interaktionen oder Interaktionen unterstützen müssen, die Geschwindigkeitsdaten erfordern.

Mithilfe von Manipulationsereignissen können Sie Interaktionen wie Ziehen, Zoomen und Halten erkennen.

> [!NOTE]
> Der Touchpad gibt keine Bearbeitungs Ereignisse aus. Stattdessen werden Zeiger Ereignisse für die Touchpad-Eingabe ausgelöst.

Nachfolgend finden Sie eine Liste der Manipulationsereignisse und der jeweiligen Ereignisargumente.

| Ereignis oder Klasse                                                                                               | Description                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**ManipulationStarting-Ereignis**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)                                   | Tritt bei der ersten Erstellung des Manipulationsprozessors auf.                                                                                  |
| [**ManipulationStarted-Ereignis**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)                                     | Tritt auf, wenn ein Eingabegerät eine Bearbeitung für das [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)beginnt.                                            |
| [**ManipulationDelta-Ereignis**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)                                         | Tritt ein, wenn sich die Position des Eingabegeräts während einer Bearbeitung ändert.                                                                      |
| [**ManipulationInertiaStarting-Ereignis**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastartingevent)                | Tritt auf, wenn das Eingabegerät während einer Bearbeitung Kontakt mit dem [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)-Objekt verliert und die Trägheit beginnt. |
| [**ManipulationCompleted-Ereignis**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)                                 | Tritt auf, wenn Bearbeitung und Trägheit für das [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) abgeschlossen sind.                                          |
| [**ManipulationStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartingRoutedEventArgs)               | Stelle Daten für das [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)-Ereignis bereit.                                         |
| [**ManipulationStartedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartedRoutedEventArgs)                 | Stellt Daten für das [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)-Ereignis bereit.                                           |
| [**ManipulationDeltaRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationDeltaRoutedEventArgs)                     | Stellt Daten für das [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)-Ereignis bereit.                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationInertiaStartingRoutedEventArgs) | Stellt Daten für das [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)-Ereignis bereit.                           |
| [**ManipulationVelocities**](/uwp/api/Windows.UI.Input.ManipulationVelocities)                                              | Beschreibt die Geschwindigkeit, mit der die Bearbeitung stattfindet.                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationCompletedRoutedEventArgs)             | Stellt Daten für das [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)-Ereignis bereit.                                       |

 

Eine Bewegung setzt sich aus einer Reihe von Bearbeitungsereignissen zusammen. Jede Geste beginnt mit einem [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted) (z. B., wenn ein Benutzer den Bildschirm berührt).

Anschließend wird mindestens ein [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)-Ereignis ausgelöst. Beispielsweise, wenn Sie den Bildschirm berühren und dann den Finger über den Bildschirm ziehen. Schließlich wird ein [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)-Ereignis ausgelöst, wenn die Interaktion beendet ist.

> [!NOTE]
> Wenn Sie nicht über einen Touchscreen verfügen, können Sie den Manipulations Ereignis Code im Simulator mithilfe einer Maus-und Mausrad Schnittstelle testen.

 

Das folgende Beispiel zeigt, wie die [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)-Ereignisse verwendet werden, um eine Ziehen-Interaktion für ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) zu behandeln und es über den Bildschirm zu bewegen.

Zuerst wird ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) mit dem Namen `touchRectangle` mit einer [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) und [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) von 200 in XAML erstellt.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

Anschließend wird ein globales [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-Element mit dem Namen `dragTranslation` erstellt, mit dem das [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Element übersetzt wird. Ein [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) -Ereignislistener ist für das **Rechteck** angegeben und `dragTranslation` wird der [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) des **Rechtecks** hinzugefügt.

```cpp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```cs
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```vb
' Global translation transform used for changing the position of 
' the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    ' New translation transform populated in 
    ' the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    ' Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

Schließlich wird im [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)-Ereignishandler die Position des [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) mit der [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-Eigenschaft für die [**Delta**](/uwp/api/windows.ui.xaml.input.manipulationdeltaroutedeventargs.delta)-Eigenschaft aktualisiert.

```cpp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```cs
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```vb
' Handler for the ManipulationDelta event.
' ManipulationDelta data Is loaded into the
' translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    ' Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <a name="routed-events"></a>Routingereignisse

Alle hier erwähnten Zeiger-, Gestik- und Manipulationsereignisse werden als *Routingereignisse* implementiert. Folglich kann das Ereignis potenziell auch von Objekten behandelt werden, bei denen es sich nicht um das Objekt handelt, von dem das Ereignis ursprünglich ausgelöst wurde. Von aufeinander folgenden übergeordneten Elementen in einer Objektstruktur (wie etwa den übergeordneten Containern eines [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)-Elements oder dem [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page)-Stammelement Ihrer App) können diese Ereignisse auch dann behandelt werden, wenn sie vom ursprünglichen Element nicht behandelt werden. Umgekehrt gilt: Ein Objekt, von dem das Ereignis nicht behandelt wird, kann das Ereignis als behandelt markieren, damit es keine übergeordneten Elemente mehr erreicht. Weitere Informationen zum Routing Ereignis Konzept und dessen Auswirkungen auf das Schreiben von Handlern für Routing Ereignisse finden Sie unter [Übersicht über Ereignisse und Routing Ereignisse](/previous-versions/windows/apps/hh758286(v=win.10)).

> [!Important]
> Wenn Sie Zeiger Ereignisse für ein [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) in einer Bild lauffähigen Ansicht (z. b. ScrollViewer oder ListView) behandeln müssen, müssen Sie die Unterstützung für Manipulations Ereignisse für das Element in der Ansicht explizit deaktivieren, indem Sie [UIElement. canceldirectmanipulation ()](/uwp/api/windows.ui.xaml.uielement.canceldirectmanipulations)aufrufen. Um Manipulations Ereignisse in der Ansicht erneut zu aktivieren, müssen Sie [UIElement. trystartdirectmanipulation ()](/uwp/api/windows.ui.xaml.uielement.trystartdirectmanipulation)aufrufen.

## <a name="dos-and-donts"></a>DOS und Donas

- Entwerfen Sie Anwendungen mit Toucheingabe als primär erwarteter Eingabemethode.
- Bieten Sie visuelles Feedback für alle Arten von Interaktionen (Berühren, Zeichenstift, Eingabestift, Maus usw.).
- Optimieren Sie die Zielbestimmung, indem Sie die Größe des Toucheingabeziels, die Kontaktgeometrie, das Scrubbing und das Wackeln anpassen.
- Optimieren Sie die Genauigkeit durch Verwendung von Andockpunkten und Führungsschienen.
- Stellen Sie QuickInfos und Handles bereit, um die Genauigkeit der Toucheingabe für eng aneinanderliegende UI-Elemente zu verbessern.
- Verwenden Sie nach Möglichkeit keine zeitlich festgelegten Interaktionen (Beispiel für eine richtige Verwendung: Berühren und Halten).
- Verwenden Sie nach Möglichkeit nicht die Anzahl der zu verwendenden Finger, um zwischen den Manipulationen zu unterscheiden.

## <a name="related-articles"></a>Verwandte Artikel

- [Behandeln von Zeigereingaben](handle-pointer-input.md)
- [Identifizieren von Eingabegeräten](identify-input-devices.md)

### <a name="samples"></a>Beispiele

- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für Gerätefunktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Eingabe: Gesten und Manipulationen mit GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
