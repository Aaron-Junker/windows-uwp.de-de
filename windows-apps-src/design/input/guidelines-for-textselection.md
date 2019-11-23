---
Description: In diesem Thema wird die neue Windows-Benutzeroberfläche für das auswählen und Bearbeiten von Text, Bildern und Steuerelementen beschrieben und Richtlinien zur Benutzeroberfläche bereitstellt, die bei der Verwendung dieser neuen Auswahl-und manipulationsmechanismen in der UWP-App berücksichtigt werden sollten
title: Auswählen von Text und Bildern
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: Tastatur, Text, Eingabe, Benutzerinteraktionen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 56f09f2c903354159c63fa7226007cd65e57e69e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257918"
---
# <a name="selecting-text-and-images"></a>Auswählen von Text und Bildern


In diesem Artikel wird das Auswählen und Bearbeiten von Text, Bildern und Steuerelementen beschrieben. Außerdem enthält er Richtlinien für die Benutzeroberfläche, die Sie bei der Verwendung dieser Mechanismen in Ihren Apps berücksichtigen sollten.

> **Wichtige APIs**: [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Schriftartglyphen, wenn Sie Ihre eigene Ziehelement-UI implementieren. Das Ziehelement ist eine Kombination aus zwei systemweit verfügbaren Segoe UI-Schriftarten. Die Verwendung von Schriftressourcen vereinfacht das Rendering bei unterschiedlichen DPI-Einstellungen und funktioniert gut mit den verschiedenen UI-Skalierungsebenen. Ihre Ziehelemente sollten alle die folgenden Benutzeroberflächenmerkmale aufweisen:

    -   Kreisform
    -   Sichtbar auf jedem Hintergrund
    -   Einheitliche Größe
-   Fügen Sie um auswählbaren Inhalt einen Rand für die Ziehelement-UI hinzu. Falls Ihre App die Textauswahl in einem Bereich ohne Verschiebung/Bildlauf ermöglicht, sollten Sie einen Rand lassen, der an der linken und rechten Seite halb so groß ist wie das Ziehelement und an der oberen und unteren Seite so groß wie die Höhe des Zielelements (siehe folgende Abbildungen). Dadurch wird sichergestellt, dass die gesamte Ziehelement-UI für den Benutzer sichtbar ist, und unbeabsichtigte Interaktionen mit anderen randbasierten Benutzeroberflächen werden minimiert.

    ![Ränder des Textauswahlziehelements](images/textselection-gripper-margins.png)

-   Blenden Sie die Ziehelement-UI während der Interaktion aus. So können Sie verhindern, dass die Interaktion durch die Ziehelemente behindert wird. Dies ist hilfreich, wenn ein Ziehelement nicht vollständig vom Finger verdeckt wird oder mehrere Ziehelemente für die Textauswahl vorhanden sind. Dadurch werden visuelle Fehler beim Anzeigen untergeordneter Fenster verhindert.

-   Die Auswahl von UI-Elementen wie Steuerelementen, Beschriftungen, Bildern, geschützten Inhalten usw. sollte nicht möglich sein. In Windows-Apps ist die Auswahl normalerweise nur in bestimmten Steuerelementen möglich. Steuerelemente wie Schaltflächen, Beschriftungen und Logos sind nicht auswählbar. Beurteilen Sie, ob die Auswahl ein Problem für die App darstellt, und identifizieren Sie gegebenenfalls die Bereiche der UI, in denen keine Auswahl möglich sein sollte. 

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung


Für die Auswahl und Manipulation von Text ergeben sich durch Fingereingabeinteraktionen besonders leicht Probleme für die Benutzeroberfläche. Eingaben über Maus, Zeichen-/Tablettstift und Tastatur sind sehr präzise: Ein Mausklick oder ein Kontakt mit dem Zeichen- bzw. Tablettstift ist in der Regel einem einzigen Pixel zugeordnet, und eine Taste wird gedrückt oder nicht gedrückt. Die Fingereingabe ist weniger präzise. Es ist schwierig, die gesamte Oberfläche einer Fingerspitze einer bestimmten x-y-Position auf dem Bildschirm zuzuordnen, um ein Caretzeichen exakt im Text zu platzieren.

**Überlegungen und Empfehlungen**

Verwenden Sie die integrierten Steuerelemente, die über die sprach-Frameworks in Windows verfügbar gemacht werden, um apps zu erstellen, die die vollständige Benutzerinteraktion der Plattform ermöglichen, einschließlich Auswahl-und Bearbeitungs Verhalten Die Interaktionsfunktionen der integrierten Steuerelemente sollten daher für die meisten UWP-Apps ausreichen.

Bei Verwendung der standardmäßigen UWP-Textsteuerelemente können die in diesem Thema beschriebenen Auswahlverhalten und visuellen Elemente nicht angepasst werden.

**Textauswahl**

Wenn Ihre APP eine benutzerdefinierte Benutzeroberfläche erfordert, die die Textauswahl unterstützt, wird empfohlen, die hier beschriebenen Windows-Auswahl Verhalten zu befolgen.

**Bearbeitbarer und nicht bearbeitbarer Inhalt**


Bei der Fingereingabe werden Auswahlinteraktionen hauptsächlich durch Bewegungen ausgeführt, z. B. Tippen, um einen Einfügecursor zu setzen oder ein Wort auszuwählen, und Ziehen, um eine Auswahl zu ändern. Wie bei anderen Windows-touchinteraktionen sind zeitgesteuerte Interaktionen auf die Press-und Hold-Geste beschränkt, um die Informations Benutzeroberfläche anzuzeigen. Weitere Informationen finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

Windows erkennt zwei mögliche Zustände für Auswahl Interaktionen, bearbeitbar und nicht bearbeitbar und passt die Auswahl Benutzeroberfläche, das Feedback und die Funktionalität entsprechend an.

**Bearbeitbarer Inhalt**

Durch Tippen in der linken Hälfte eines Worts wird der Cursor links neben dem Wort platziert, durch Tippen in der rechten Hälfte wird er rechts neben dem Wort platziert.

Im folgenden Bild wird veranschaulicht, wie Sie einen anfänglichen Einfügecursor mit Ziehelement platzieren, indem Sie in die Nähe des Anfangs oder Endes eines Worts tippen.

![Tippen (oder drücken und halten) Sie links neben ein Wort, um ein Caretzeichen und ein Ziehelement am Anfang des Worts zu platzieren. Tippen (oder drücken und halten) Sie rechts neben ein Wort, um ein Caretzeichen und ein Ziehelement am Ende des Worts zu platzieren.](images/textselection-place-caret.png)

Im folgenden Bild wird veranschaulicht, wie Sie eine Auswahl durch Ziehen des Ziehelements anpassen.

![Ziehen Sie das Ziehelement in eine beliebige Richtung, um die Auswahl anzupassen (das erste Ziehelement bleibt verankert, und ein zweites Ziehelement wird angezeigt). Ziehen Sie eines der Ziehelemente, um weitere Anpassungen vorzunehmen.](images/adjust-selection.png)

Im folgenden Bild wird veranschaulicht, wie Sie das Kontextmenü aufrufen, indem Sie in die Auswahl oder auf ein Ziehelement tippen (Sie können auch Drücken und Halten verwenden).

![Tippen (oder drücken und halten) Sie in die Auswahl oder auf ein Ziehelement, um das Kontextmenü aufzurufen.](images/textselection-show-context.png)

**Beachten Sie**  diese Interaktionen im Fall eines falsch geschriebenen Worts etwas unterschiedlich sind. Wenn Sie auf ein Wort tippen, das als falsch geschrieben gekennzeichnet ist, wird das gesamte Wort hervorgehoben. Außerdem wird das Kontextmenü mit der vorgeschlagenen Schreibweise aufgerufen.

 

**Nicht bearbeitbarer Inhalt**

Im folgenden Bild wird veranschaulicht, wie Sie ein Wort auswählen, indem Sie in das Wort tippen (die anfängliche Auswahl enthält keine Leerzeichen).

![Tippen Sie in ein Wort, um es auszuwählen (die anfängliche Auswahl enthält keine Leerzeichen).](images/select-word.png)

Gehen Sie für bearbeitbaren Test genauso vor, um die Auswahl anzupassen und das Kontextmenü anzuzeigen.

**Objekt Bearbeitung**

Verwenden Sie beim Implementieren einer benutzerdefinierten Objektmanipulation in einer UWP-App nach Möglichkeit die gleichen (oder ähnliche) Ziehelementressourcen wie für die Textauswahl. Auf diese Weise kann ein einheitliches Interaktionsverhalten für die gesamte Plattform bereitgestellt werden.

Zielelemente können wie in den folgenden Abbildungen dargestellt z. B. auch in Bildverarbeitungs-Apps verwendet werden, die Größenänderungen und Zuschneiden unterstützen, oder in Media-Player-Apps mit anpassbaren Statusanzeigen.

![Media-Player mit Statusziehelement](images/gripper-mediaplayer.png)

*Media Player mit anpassbarer Statusanzeige.*

![Bild mit Ziehelementen zum Zuschneiden](images/gripper-imagemanip.png)

*Bildbearbeitung mit Zuschneiden Zieh stellern.*

## <a name="related-articles"></a>Verwandte Artikel



**Für Entwickler**
* [Benutzerdefinierte Benutzerinteraktionen](https://docs.microsoft.com/windows/uwp/design/layout/index)

**Beispiele**
* [Beispiel für eine einfache Eingabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Eingabe Beispiel mit niedriger Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Archivbeispiele**
* [Eingabe: Beispiel für XAML-Benutzereingabe Ereignisse](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Eingabe: Beispiel für Gerätefunktionen](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Eingabe: Beispiel für Berührungs Treffer Tests](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Eingabe: vereinfachtes Ink-Beispiel](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Eingabe: Beispiel für Windows 8-Gesten](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Eingabe: Manipulationen und Gesten (C++) (Beispiel)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Beispiel für DirectX-Fingereingabe](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




