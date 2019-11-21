---
Description: Reagieren Sie in Ihren Apps auf Mauseingaben, indem Sie die gleichen einfachen Zeigerereignisse behandeln, die Sie für Touch- und Stifteingaben verwenden.
title: Mausinteraktionen
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0d54ab79926ef20bb54a83346c12df2ea383167d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258344"
---
# <a name="mouse-interactions"></a>Mausinteraktionen

Optimieren Sie das Design Ihrer UWP-Apps für die Toucheingabe, und freuen Sie sich über die standardmäßige allgemeine Unterstützung von Mausgeräten. 

![Maus](images/input-patterns/input-mouse.jpg)

Die Mauseingabe eignet sich am besten für Benutzerinteraktionen, die Präzision beim Zeigen und Klicken erfordern. Naturgemäß unterstützt die Benutzeroberfläche von Windows diese Präzision, auch wenn sie für die ungenaue Toucheingabe optimiert wurde.

Die Maus- und Toucheingabe unterscheiden sich dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (z. B. Streifen, Ziehen, Drehen usw.) emuliert werden kann. Manipulationen mit der Maus erfordern in der Regel einigen UI-Aufwand, wie z. B. die Verwendung von Handles für das Anpassen der Größe oder Drehen eines Objekts.

In diesem Thema werden Designüberlegungen für Mausinteraktionen behandelt.

## <a name="the-uwp-app-mouse-language"></a>Die UWP-App-Sprache für Mauseingaben

Ein kompakter Satz von Mausinteraktionen wird durchgängig im ganzen System verwendet.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Begriff</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Lernen durch Zeigen</p></td>
<td align="left"><p>Zeigen Sie auf ein Element, um weitere Informationen oder visuelle Hinweise (wie etwa QuickInfos) aufzurufen, ohne eine Aktion auszuführen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Linksklick, um primäre Aktion auszuführen</p></td>
<td align="left"><p>Klicken Sie mit der linken Maustaste auf ein Element, um dessen primäre Aktion aufzurufen (z. B. das Starten einer App oder das Ausführen eines Befehls).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Bildlauf, um Ansicht zu ändern</p></td>
<td align="left"><p>Zeigen Sie Bildlaufleisten an, damit Benutzer in einem Inhaltsbereich nach oben, unten, links und rechts navigieren können. Benutzer können durch Klicken auf Bildlaufleisten oder Drehen des Mausrads einen Bildlauf durchführen. Auf Bildlaufleisten kann die Position der aktuellen Ansicht innerhalb des Inhaltsbereichs angezeigt werden (durch das Schwenken bei der Fingereingabe wird eine ähnliche Benutzeroberfläche angezeigt).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Rechtsklick, um Auswahl zu treffen und Befehl auszuwählen</p></td>
<td align="left"><p>Klicken Sie mit der rechten Maustaste, um die Navigationsleiste (sofern verfügbar) und die App-Leiste mit globalen Befehlen anzuzeigen. Klicken Sie mit der rechten Maustaste auf ein Element, um es auszuwählen und die App-Leiste mit Kontextbefehlen für das ausgewählte Element anzuzeigen.</p>
<div class="alert">
<strong>Note</strong>  Right-click to display a context menu if selection or app bar commands are not appropriate UI behaviors. Wir empfehlen jedoch ausdrücklich, die App-Leiste für alle Befehlsverhalten zu verwenden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>Benutzeroberflächenbefehle zum Zoomen</p></td>
<td align="left"><p>Zeigen Sie Benutzeroberflächenbefehle auf der App-Leiste an (z. B. "+" und "-"), oder drücken Sie STRG und drehen Sie das Mausrad, um Zusammendrück- und Aufziehbewegungen zum Zoomen zu emulieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Benutzeroberflächenbefehle zum Drehen</p></td>
<td align="left"><p>Zeigen Sie Benutzeroberflächenbefehle auf der App-Leiste an, oder drücken Sie STRG+UMSCHALTTASTE, und drehen Sie das Mausrad, um die Drehbewegung zum Drehen zu emulieren. Drehen Sie das Gerät selbst, um den ganzen Bildschirm zu drehen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Linksklick und ziehen, um neu anzuordnen</p></td>
<td align="left"><p>Klicken Sie mit der linken Maustaste auf ein Element, und ziehen Sie, um es zu bewegen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Linksklick und ziehen, um Text auszuwählen</p></td>
<td align="left"><p>Klicken Sie mit der linken Maustaste auf auswählbaren Text, und ziehen Sie, um Text auszuwählen. Doppelklicken Sie, um ein Wort auszuwählen.</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-input-events"></a>Mouse input events

Most mouse input can be handled through the common routed input events supported by all [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) objects. Dazu zählen:

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Drop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefound)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

However, you can take advantage of the specific capabilities of each device (such as mouse wheel events) using the pointer, gesture, and manipulation events in [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input).

**Samples:** See our [BasicInput sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput), for .

## <a name="guidelines-for-visual-feedback"></a>Richtlinien für visuelles Feedback

- Wenn eine Maus erkannt wird (durch Bewegungs- oder Daraufzeigen-Ereignisse), zeigen Sie eine für Mausinteraktionen spezifische Benutzeroberfläche an, um auf vom Element verfügbar gemachte Funktionen hinzuweisen. Wenn die Maus für eine bestimmte Zeit nicht bewegt wird oder der Benutzer eine Fingereingabeinteraktion auslöst, blenden Sie die für Mausinteraktionen spezifische Benutzeroberfläche schrittweise aus. Somit bleibt die Benutzeroberfläche sauber und aufgeräumt.
- Verwenden Sie nicht den Cursor für Zeigefeedback, das Feedback des Elements reicht aus (siehe Cursor unten).
- Zeigen Sie kein visuelles Feedback an, wenn ein Element keine Interaktionen unterstützt (z. B. statischer Text).
- Verwenden Sie keine Fokusrechtecke für Mausinteraktionen. Diese sind ausschließlich für Tastaturinteraktionen vorgesehen.
- Zeigen Sie für alle Elemente, die das gleiche Eingabeziel darstellen, das gleiche visuelle Feedback an.
- Stellen Sie Schaltflächen (z. B. „+“ und „-“) zur Verfügung, um fingereingabebasierte Manipulationen wie etwa Schwenken, Drehen, Zoomen usw. zu emulieren.

Allgemeine Informationen zum visuellen Feedback finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

## <a name="cursors"></a>Cursor

Für einen Mauszeiger ist eine Reihe von Standardcursor verfügbar. Diese geben die primäre Aktion eines Elements an.

Jedem Standardcursor ist ein entsprechendes Standardbild zugewiesen. Benutzer einer App können das Standardbild, das einem Standardcursor zugewiesen ist, jederzeit ändern. Geben Sie über die [**PointerCursor**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointercursor)-Funktion die Abbildung eines Cursors an.

Beachten Sie beim Anpassen des Mauszeigers Folgendes:

- Verwenden Sie immer den Pfeilcursor (![Pfeilcursor](images/cursor-arrow.png)) für klickbare Elemente. Verwenden Sie den Zeigefingercursor (![Zeigefingercursor](images/cursor-pointinghand.png)) nicht für Links oder andere interaktive Elemente. Verwenden Sie stattdessen Zeigeeffekte (wie bereits beschrieben).
- Verwenden Sie den Textcursor (![Textcursor](images/cursor-text.png)) für auswählbaren Text.
- Verwenden Sie den Bewegungscursor (![Bewegungscursor](images/cursor-move.png)), wenn die primäre Aktion eine Bewegung ist (etwa beim Ziehen oder Zuschneiden). Verwenden Sie den Bewegungscursor nicht, wenn die primäre Aktion eine Navigationsaktion ist (etwa bei Start-Kacheln).
- Verwenden Sie die Cursor für horizontale, vertikale und diagonale Größenänderung (![Cursor für vertikale Größenänderung](images/cursor-vertical.png), ![Cursor für horizontale Größenänderung](images/cursor-horizontal.png), ![Cursor für diagonale Größenänderung (unten links, oben rechts)](images/cursor-diagonal2.png), ![Cursor für diagonale Größenänderung (oben links, unten rechts)](images/cursor-diagonal1.png)), wenn die Größe eines Objekts geändert werden kann.
- Verwenden Sie den Handcursor (![Handcursor (offen)](images/cursor-pan1.png), ![Handcursor (geschlossen)](images/cursor-pan2.png)) beim Verschieben von Inhalt innerhalb einer Canvas (etwa bei einer Karte).

## <a name="related-articles"></a>Verwandte Artikel

- [Behandeln von Zeigereingaben](handle-pointer-input.md)
- [Identifizieren von Eingabegeräten](identify-input-devices.md)
- [Übersicht über Ereignisse und Routingereignisse](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

### <a name="samples"></a>Beispiele

- [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)