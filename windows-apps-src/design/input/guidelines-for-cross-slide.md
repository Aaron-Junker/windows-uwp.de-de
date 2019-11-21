---
Description: Verwenden Sie Querziehen, um Auswahlinteraktionen mit einer Streifbewegung und Ziehinteraktionen (Verschieben) mit einer Ziehbewegung zu unterstützen.
title: Richtlinien für Querziehen
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 833949effd311c707de8dd1823ec6eee06e91e87
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257979"
---
# <a name="guidelines-for-cross-slide"></a>Richtlinien für Querziehen




**Wichtige APIs**

-   [**CrossSliding**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.crosssliding)
-   [**CrossSlideThresholds**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.crossslidethresholds)
-   [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

Verwenden Sie Querziehen, um Auswahlinteraktionen mit einer Streifbewegung und Ziehinteraktionen (Verschieben) mit einer Ziehbewegung zu unterstützen.

## <a name="span-iddos_and_don_tsspanspan-iddos_and_don_tsspanspan-iddos_and_don_tsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Dos and don'ts


-   Verwenden Sie das Querziehen für Listen oder Auflistungen, bei denen ein Bildlauf nur in eine Richtung möglich ist.
-   Verwenden Sie das Querziehen für die Elementauswahl, wenn die Tippinteraktion für andere Zwecke verwendet wird.
-   Verwenden Sie das Querziehen nicht, um Elemente zu einer Warteschlange hinzuzufügen.

## <a name="span-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanspan-idadditional_usage_guidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Additional usage guidance


Auswahl und Ziehen sind nur in Inhaltsbereichen möglich, die in eine Richtung (vertikal oder horizontal) verschoben werden können. Damit die Interaktion funktioniert, muss eine Verschiebungsrichtung arretiert sein, und die Bewegung muss senkrecht zur Verschiebungsrichtung ausgeführt werden.

Im Folgenden wird das Auswählen und Ziehen eines Objekts durch Querziehen veranschaulicht. Die linke Abbildung zeigt, wie ein Element ausgewählt wird, wenn eine Streifbewegung eine Distanzschwelle nicht überschreitet, bevor der Kontakt aufgehoben und das Objekt losgelassen wird. Die rechte Abbildung zeigt eine Ziehbewegung, die eine Distanzschwelle überschreitet und zum Ziehen des Objekts führt.

![Diagramm, das die Prozesse für Auswahl und Drag & Drop veranschaulicht](images/crossslide-mechanism.png)

Das folgende Diagramm zeigt Distanzschwellen, die bei der Interaktion des Querziehens verwendet werden.

![Screenshot der Prozesse für Auswahl und Drag & Drop](images/crossslide-threshold.png)

Damit die Verschiebungsfunktion erhalten bleibt, muss eine niedrige Schwelle von 2,7 mm (ca. 10 Pixel bei Zielauflösung) überschritten werden, bevor eine Auswahl- oder Ziehinteraktion aktiviert wird. Anhand dieser niedrigen Schwelle kann das System zwischen Querziehen und Verschieben unterscheiden. Außerdem kann so eine Tippbewegung von Querziehen und Verschieben unterschieden werden.

Die folgende Abbildung zeigt, wie der Benutzer ein UI-Element berührt, den Finger beim Kontakt aber leicht nach unten bewegt. Ohne Schwelle würde die Interaktion aufgrund der anfänglichen vertikalen Bewegung als Querziehen interpretiert. Dank der Schwelle wird die Bewegung korrekt als horizontales Verschieben interpretiert.

![Screenshot der Mehrdeutigkeitsvermeidungsschwelle für Auswahl und Drag & Drop](images/crossslide-threshold2.png)

Beachten Sie die folgenden Richtlinien, wenn Sie eine Querziehfunktion in Ihrer App bereitstellen.

Verwenden Sie das Querziehen für Listen oder Auflistungen, bei denen ein Bildlauf nur in eine Richtung möglich ist. Weitere Informationen finden Sie unter [Hinzufügen von ListView-Steuerelementen](https://docs.microsoft.com/previous-versions/windows/apps/hh465382(v=win.10)).

**Note**  In cases where the content area can be panned in two directions, such as web browsers or e-readers, the press-and-hold timed interaction should be used to invoke the context menu for objects such as images and hyperlinks.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![Horizontal verschiebbare zweidimensionale Liste](images/groupedlistview1.png)                | ![Vertikal verschiebbare eindimensionale Liste](images/listviewlistlayout.png)                |
| Hier sehen Sie eine horizontal verschiebbare zweidimensionale Liste. Ziehen Sie vertikal, um ein Element auszuwählen oder zu verschieben. | Hier sehen Sie eine vertikal verschiebbare eindimensionale Liste. Ziehen Sie horizontal, um ein Element auszuwählen oder zu verschieben. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**Selecting**

Beim Auswählen wird mindestens ein Objekt markiert, ohne es zu starten oder zu aktivieren. Diese Aktion entspricht einem einfachen Mausklick oder einem Mausklick mit gedrückter UMSCHALTTASTE auf mindestens ein Objekt.

Zum Auswählen durch Querziehen wird ein Element berührt und nach einer kurzen Interaktion des Ziehens wieder losgelassen. Bei dieser Methode entfallen sowohl der dedizierte Auswahlmodus als auch die zeitlich festgelegte Interaktion des Gedrückthaltens, die bei anderen Schnittstellen für die Fingereingabe erforderlich sind. Sie steht nicht in Konflikt mit der für die Aktivierung verwendeten Interaktion des Tippens.

Neben der Distanzschwelle gilt für das Auswählen durch Querziehen auch eine Beschränkung auf einen Schwellenbereich von 90°, wie im folgenden Diagramm veranschaulicht. Wenn das Objekt außerhalb dieses Bereichs gezogen wird, wird es nicht ausgewählt.

![Diagramm, das den Schwellenbereich für die Auswahl zeigt](images/crossslide-selection.png)

Die Interaktion des Querziehens wird durch eine zeitlich festgelegte Interaktion des Gedrückthaltens, auch als „Interaktion mit automatischem Einblenden“ bezeichnet, ergänzt. Durch diese zusätzliche Interaktion wird eine Animation aktiviert, die zeigt, welche Aktion für das Objekt ausgeführt werden kann. Weitere Informationen zur Mehrdeutigkeitsvermeidungs-UI finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

In den folgenden Screenshots wird die Funktionsweise der Animation mit automatischem Einblenden verdeutlicht.

1.  Halten Sie das Element gedrückt, um die Animation für die Interaktion mit automatischem Einblenden auszulösen. Der ausgewählte Zustand des Elements hat direkten Einfluss auf die durch die Animation eingeblendeten Inhalte: ein Häkchen bei aufgehobener Auswahl und kein Häkchen bei erfolgter Auswahl.

    ![Screenshot eines Zustands ohne Auswahl](images/crossslide-selfreveal1.png)

2.  Wählen Sie das Element mit einer Streifbewegung (nach oben oder unten) aus.

    ![Screenshot der Animation für die Auswahl](images/crossslide-selfreveal2.png)

3.  Das Element ist nun ausgewählt. Setzen Sie das Auswahlverhalten mit der Streifbewegung zum Verschieben des Elements außer Kraft.

    ![Screenshot der Animation für Drag & Drop](images/crossslide-selfreveal3.png)

Verwenden Sie in Apps, in denen das Auswählen die einzige Hauptaktion ist, einfaches Tippen für die Auswahl. Die Querziehanimation mit automatischem Einblenden wird angezeigt, um diese Funktion von der standardmäßigen Tippinteraktion für Aktivierung und Navigation zu unterscheiden.

**Selection basket**

Der Auswahlkorb ist eine visuell unverwechselbare, dynamische Darstellung von Elementen, die aus der primären Liste oder Auflistung in der Anwendung ausgewählt wurden. Dieses Feature ist hilfreich, um den Überblick über ausgewählte Elemente zu behalten, und es sollte in Anwendungen verwendet werden, auf die Folgendes zutrifft:

-   Elemente können an mehreren Orten ausgewählt werden.
-   Es können mehrere Elemente ausgewählt werden.
-   Einer Aktion oder einem Befehl wird die Auswahlliste zugrunde gelegt.

Der Inhalt des Auswahlkorbs bleibt über Aktionen und Befehle hinweg erhalten. Wenn Sie z. B. eine Serie von Fotos aus einer Galerie auswählen, in jedem Foto eine Farbkorrektur durchführen und die Fotos auf irgendeine Weise mit anderen teilen, bleibt die Auswahl der Elemente erhalten.

Wenn in einer Anwendung kein Auswahlkorb verwendet wird, sollte die aktuelle Auswahl nach einer Aktion oder einem Befehl gelöscht werden. Wenn Sie z. B. einen Musiktitel in einer Wiedergabeliste auswählen und bewerten, sollte die Auswahl gelöscht werden.

Auch wenn kein Auswahlkorb verwendet und ein anderes Element in der Liste oder Auflistung aktiviert wird, sollte die aktuelle Auswahl gelöscht werden. Wenn Sie z. B. eine Nachricht im Posteingang auswählen, wird das Vorschaufenster aktualisiert. Wählen Sie anschließend eine zweite Nachricht im Posteingang aus, wird die Auswahl der vorherigen Nachricht aufgehoben und das Vorschaufenster erneut aktualisiert.

**Queues**

Eine Warteschlange ist nicht mit der Liste im Auswahlkorb gleichzusetzen und sollte auch nicht so behandelt werden. Hier die wichtigsten Unterschiede:

-   Die Liste der Elemente im Auswahlkorb ist nur eine visuelle Darstellung, während die Elemente in einer Warteschlange im Hinblick auf eine bestimmte Aktion zusammengestellt werden.
-   Elemente können im Auswahlkorb nur einmal dargestellt werden, in einer Warteschlange hingegen mehrmals.
-   Die Reihenfolge der Elemente im Auswahlkorb entspricht der Reihenfolge, in der sie ausgewählt wurden. Die Reihenfolge der Elemente in einer Warteschlange hängt unmittelbar mit der Funktion zusammen.

Aus diesen Gründen sollte die Auswahlinteraktion durch Querziehen nicht verwendet werden, um Elemente zu einer Warteschlange hinzuzufügen. Zum Hinzufügen von Elementen zu einer Warteschlange sollte eine Ziehaktion ausgeführt werden.

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**Drag**

Verwenden Sie eine Ziehbewegung, um Objekte von einer Position an eine andere zu ziehen.

Wenn mehrere Objekte verschoben werden müssen, geben Sie dem Benutzer die Möglichkeit, mehrere Elemente auszuwählen und anschließend gleichzeitig zu ziehen.

## <a name="span-idrelated_topicsspanrelated-articles"></a><span id="related_topics"></span>Related articles


**Beispiele**
* [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
**Archivbeispiele**
* [Input: XAML user input events sample](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Input: Device capabilities sample](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Input: Touch hit testing sample](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML scrolling, panning, and zooming sample](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Input: Simplified ink sample](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Input: Windows 8 gestures sample](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Input: Manipulations and gestures (C++) sample](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX touch input sample](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




