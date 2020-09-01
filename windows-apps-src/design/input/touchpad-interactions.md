---
Description: Erstellen Sie Windows-apps mit intuitiver und unterschiedlichen Benutzerinteraktionen, die für Touchpad optimiert sind, aber funktionell auf Eingabegeräten einheitlich sind.
title: Touchpad-Interaktionen
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
keywords: Touchpad, PTP, Touch, Zeiger, Eingabe, Benutzerinteraktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9e83cb1ceca96e5c7b51e71cb419b86b0395ea99
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165864"
---
# <a name="touchpad-design-guidelines"></a>Touchpad-Designrichtlinien


Gestalten Sie Ihre App so, dass Benutzer über ein Touchpad mit ihr interagieren können. Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigegeräts (beispielsweise eine Maus). Durch diese Kombination ist das Touchpad sowohl für eine toucheingabeoptimierte Benutzeroberfläche als auch für die kleineren Ziele von Produktivitäts-Apps geeignet.

 

![Touchpad](images/input-patterns/input-touchpad.jpg)


Interaktionen per Touchpad erfordern drei Dinge:

-   Ein standardmäßiges Touchpad oder ein Windows-Präzisionstouchpad.

    Genauigkeits Touchpads sind für Windows-App-Geräte optimiert. Sie ermöglichen es dem System, bestimmte Aspekte der Touchpadverwendung (etwa die Fingerverfolgung und Handflächenerkennung) nativ zu behandeln, um geräteübergreifend eine konsistentere Umgebung zu bieten.

-   Der direkte Kontakt einzelner oder mehrerer Finger auf dem Touchpad.
-   Bewegung der Berührungskontakte (oder das Fehlen der Bewegung basierend auf einem Zeitschwellenwert).

Mögliche Eingabedaten des Touchpadsensors:

-   Interpretation als physische Geste für die direkte Manipulation von einem oder mehreren UI-Elementen (z. B. Schwenken, Drehen, Vergrößern/Verkleinern oder Verschieben). Die Interaktion mit einem Element über das zugehörige Eigenschaftenfenster oder ein anderes Dialogfeld gilt dagegen als indirekte Manipulation.
-   Erkennung als alternative Eingabemethode, z. B. Maus oder Stift.
-   Wird zum Ergänzen oder Ändern von Aspekten anderer Eingabemethoden verwendet, z. B. zum Verwischen eines mit einem Stift gezeichneten Freihandstrichs.

Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigergeräts (etwa eine Maus). Dadurch ist das Touchpad sowohl für die berührungsoptimierte Benutzeroberfläche als auch die kleineren Ziele der Produktivitäts-Apps und der Desktopumgebung geeignet. Optimieren Sie Ihren Windows-App-Entwurf für Touch input, und holen Sie sich standardmäßig Touchpad-Unterstützung

Aufgrund der Konvergenz der Interaktionsformen, die von Touchpads unterstützt werden, empfehlen wir die Verwendung des [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)-Ereignisses, um zusätzlich zur integrierten Unterstützung für die Toucheingabe Benutzeroberflächenbefehle für die Mauseingabe bereitzustellen. Verwenden Sie beispielsweise Zurück- und Weiter-Schaltflächen, mit denen Benutzer sowohl Inhaltsseiten durchblättern als auch Inhalte verschieben können.

Die in diesem Thema beschriebenen Gesten und Richtlinien können dabei helfen, die Unterstützung der Touchpadeingabe nahtlos und mit minimalem Programmieraufwand in Ihre App zu integrieren.

## <a name="the-touchpad-language"></a>Sprache für Eingabe per Touchpad


Ein kompakter Satz von Touchpadinteraktionen wird durchgängig im ganzen System verwendet. Indem Sie Ihre App für die Touch- und Mauseingabe optimieren, sorgen Sie dafür, dass sich Benutzer sofort in Ihrer App zurechtfinden. So erleichtern Sie Benutzern den Einstieg und die Verwendung Ihrer App.

Benutzer können viel mehr Gesten und Interaktionsverhalten für Präzisionstouchpads festlegen als für ein standardmäßiges Touchpad. Diese beiden Bilder zeigen die verschiedenen Touchpad-Einstellungsseiten (unter Einstellungen &gt; Geräte &gt; Maus und Touchpad) für ein standardmäßiges Touchpad und ein Präzisionstouchpad.

![Einstellungen für ein standardmäßiges Touchpad](images/mouse-touchpad-settings-standard.png)

<sup>Standard \\ Einstellungen für Touchpad \\</sup>

![Einstellungen für ein Windows-Präzisionstouchpad](images/mouse-touchpad-settings-ptp.png)

<sup>\\ \\ Touchpad- \\ Einstellungen für Windows Precision</sup>

Im Anschluss folgen einige Beispiele für touchpadoptimierte Gesten zum Ausführen allgemeiner Aufgaben.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Begriff</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Tippen mit drei Fingern</p></td>
<td align="left"><p>Benutzereinstellung für die Suche mit <strong>Cortana</strong> oder die Anzeige des Info-Centers<strong></strong>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Ziehbewegung mit drei Fingern</p></td>
<td align="left"><p>Benutzereinstellung zum Öffnen der Aufgabenansicht des virtuellen Desktops, zum Anzeigen des Desktops oder zum Wechseln zwischen geöffneten Apps.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Tippen mit einem Finger: Aufrufen der primären Aktion</p></td>
<td align="left"><p>Durch Tippen mit einem Finger auf ein Element wird dessen primäre Aktion aufgerufen (z. B. das Starten einer App oder das Ausführen eines Befehls).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Tippen mit zwei Fingern: Ausführen eines Rechtsklicks</p></td>
<td align="left"><p>Tippen Sie mit zwei Fingern gleichzeitig auf ein Element, um es auszuwählen und Kontextbefehle anzuzeigen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Ziehen mit zwei Fingern: Verschieben</p></td>
<td align="left"><p>Das Ziehen wird in erster Linie für Verschiebungen verwendet, kann jedoch auch zum Schieben, Zeichnen oder Schreiben genutzt werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Zusammendrücken/Aufziehen: Zoom</p></td>
<td align="left"><p>Die Zusammendrück- und Aufziehbewegungen werden üblicherweise für Größenänderungen und zum Ausführen des semantischen Zooms verwendet.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Drücken und Ziehen mit einem Finger: Neuanordnen</p></td>
<td align="left"><p>Ziehen eines Elements.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Drücken und Ziehen mit einem Finger: Textauswahl</p></td>
<td align="left"><p>Durch Drücken und Ziehen in auswählbarem Text wird Text ausgewählt. Durch Doppeltippen wird ein einzelnes Wort ausgewählt.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Zonen für Links- und Rechtsklick</p></td>
<td align="left"><p>Mit diesen Zonen emulieren Sie die Links- und Rechtsklickfunktion einer Maus.</p></td>
</tr>
</tbody>
</table>

 

## <a name="hardware"></a>Hardware


Fragen Sie die Funktionen des Mausgeräts ([**MouseCapabilities**](/uwp/api/Windows.Devices.Input.MouseCapabilities)) ab, um zu ermitteln, auf welche Elemente der Benutzeroberfläche Ihrer App die Touchpad-Hardware direkt zugreifen kann. Wir empfehlen die Bereitstellung einer Benutzeroberfläche, die sowohl Touch- als auch Mauseingabe ermöglicht.

Weitere Informationen zum Abfragen von Gerätefunktionen finden Sie unter [Identifizieren von Eingabegeräten](identify-input-devices.md).

## <a name="visual-feedback"></a>Visuelles Feedback


-   Blenden Sie die für Touchpadinteraktionen spezifische Benutzeroberfläche ein, sobald ein Touchpad-Cursor erkannt wird (durch Bewegungs- oder Zeigeereignisse), um die Funktionalität des Elements verfügbar zu machen. Wenn der Touchpad-Cursor für eine bestimmte Zeit nicht bewegt wird oder der Benutzer eine Toucheingabeinteraktion auslöst, blenden Sie die für Touchpad-Interaktionen spezifische Benutzeroberfläche schrittweise aus. Somit bleibt die Benutzeroberfläche sauber und aufgeräumt.
-   Verwenden Sie den Cursor nicht, um Feedback zu senden, das vom-Element bereitgestellte Feedback ist ausreichend (Weitere Informationen finden Sie im Abschnitt "Cursor" unten).
-   Lassen Sie kein visuelles Feedback anzeigen, wenn ein Element keine Interaktionen unterstützt (z. B. statischer Text).
-   Verwenden Sie keine Fokusrechtecke für Interaktionen per Touchpad. Diese sind ausschließlich für Tastaturinteraktionen vorgesehen.
-   Zeigen Sie für alle Elemente, die das gleiche Eingabeziel darstellen, das gleiche visuelle Feedback an.

Allgemeine Informationen zum visuellen Feedback finden Sie unter [Richtlinien für visuelles Feedback](./guidelines-for-visualfeedback.md).

## <a name="cursors"></a>Cursor


In Windows Store-Apps sind einige Standardcursor verfügbar, die als Touchpad-Zeiger verwendet werden können. Diese Cursor werden verwendet, um die primäre Aktion eines Elements anzugeben.

Jedem Standardcursor ist ein entsprechendes Standardbild zugewiesen. Benutzer einer App können das einem Standardcursor zugewiesene Standardbild jederzeit ändern. UWP-Apps geben ein Cursor Bild über die [**pointercursor**](/uwp/api/windows.ui.core.corewindow.pointercursor) -Funktion an.

Beachten Sie beim Anpassen des Mauszeigers Folgendes:

-   Verwenden Sie immer den Pfeilcursor (![Pfeilcursor](images/cursor-arrow.png)) für klickbare Elemente. Verwenden Sie den Zeigefingercursor (![Zeigefingercursor](images/cursor-pointinghand.png)) nicht für Links oder andere interaktive Elemente. Verwenden Sie stattdessen Zeigeeffekte (wie bereits beschrieben).
-   Verwenden Sie den Textcursor (![Textcursor](images/cursor-text.png)) für auswählbaren Text.
-   Verwenden Sie den Bewegungscursor (![Bewegungscursor](images/cursor-move.png)), wenn die primäre Aktion eine Bewegung ist (etwa beim Ziehen oder Zuschneiden). Verwenden Sie den Bewegungscursor nicht, wenn die primäre Aktion eine Navigationsaktion ist (etwa bei Start-Kacheln).
-   Verwenden Sie die Cursor für horizontale, vertikale und diagonale Größenänderung (![Cursor für vertikale Größenänderung](images/cursor-vertical.png), ![Cursor für horizontale Größenänderung](images/cursor-horizontal.png), ![Cursor für diagonale Größenänderung (unten links, oben rechts)](images/cursor-diagonal2.png), ![Cursor für diagonale Größenänderung (oben links, unten rechts)](images/cursor-diagonal1.png)), wenn die Größe eines Objekts geändert werden kann.
-   Verwenden Sie den Handcursor (![Handcursor (offen)](images/cursor-pan1.png), ![Handcursor (geschlossen)](images/cursor-pan2.png)) beim Verschieben von Inhalt innerhalb einer Canvas (etwa bei einer Karte).

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