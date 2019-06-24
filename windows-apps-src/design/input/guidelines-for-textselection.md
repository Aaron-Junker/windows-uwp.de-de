---
Description: Dieses Thema beschreibt die neuen Windows-Benutzeroberfläche zum auswählen und Bearbeiten von Text, Bildern und Steuerelementen sowie Richtlinien zur benutzerfreundlichkeit, die berücksichtigt werden sollten, wenn diese neuen Auswahl und Manipulation Mechanismen in Ihre UWP-app verwenden.
title: Auswählen von Text und Bildern
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: Tastatur, Text, Eingabe, Benutzerinteraktionen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8dab8d26436d312601b749bed7e97048ed5805bb
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317283"
---
# <a name="selecting-text-and-images"></a>Auswählen von Text und Bildern


In diesem Artikel wird das Auswählen und Bearbeiten von Text, Bildern und Steuerelementen beschrieben. Außerdem enthält er Richtlinien für die Benutzeroberfläche, die Sie bei der Verwendung dieser Mechanismen in Ihren Apps berücksichtigen sollten.

> **Wichtige APIs:** [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)
 


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

Verwenden Sie die integrierten Steuerelemente, die über die Sprachen-Frameworks in Windows verfügbar gemacht werden, um apps zu erstellen, die die vollständige Plattform Interaktion benutzerfreundlichkeit, einschließlich der Auswahl und-Bearbeitung Verhalten bereitstellen. Die Interaktionsfunktionen der integrierten Steuerelemente sollten daher für die meisten UWP-Apps ausreichen.

Bei Verwendung der standardmäßigen UWP-Textsteuerelemente können die in diesem Thema beschriebenen Auswahlverhalten und visuellen Elemente nicht angepasst werden.

**Textauswahl**

Wenn Ihre app eine benutzerdefinierte Benutzeroberfläche, die Textauswahl unterstützt erfordert, empfehlen wir, dass Sie die hier beschriebenen Windows-Auswahlverhalten befolgen.

**Bearbeitbare und nicht-bearbeitbaren Inhalt**


Bei der Fingereingabe werden Auswahlinteraktionen hauptsächlich durch Bewegungen ausgeführt, z. B. Tippen, um einen Einfügecursor zu setzen oder ein Wort auszuwählen, und Ziehen, um eine Auswahl zu ändern. Wie mit anderen Windows touch Interaktionen, wird zeitgesteuerte Interaktionen sind auf das Drücken und halten die Eingabeaktion, die nur zu Informationszwecken Benutzeroberfläche angezeigt. Weitere Informationen finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

Windows erkennt zwei mögliche Zustände für die Auswahl Interaktionen, bearbeitbare und nicht bearbeitbare und passt die Benutzeroberfläche zur Auswahl, Feedback und Funktionen entsprechend.

**Bearbeitbarer Inhalt**

Durch Tippen in der linken Hälfte eines Worts wird der Cursor links neben dem Wort platziert, durch Tippen in der rechten Hälfte wird er rechts neben dem Wort platziert.

Im folgenden Bild wird veranschaulicht, wie Sie einen anfänglichen Einfügecursor mit Ziehelement platzieren, indem Sie in die Nähe des Anfangs oder Endes eines Worts tippen.

![Tippen (oder drücken und halten) Sie links neben ein Wort, um ein Caretzeichen und ein Ziehelement am Anfang des Worts zu platzieren. Tippen (oder drücken und halten) Sie rechts neben ein Wort, um ein Caretzeichen und ein Ziehelement am Ende des Worts zu platzieren.](images/textselection-place-caret.png)

Im folgenden Bild wird veranschaulicht, wie Sie eine Auswahl durch Ziehen des Ziehelements anpassen.

![Ziehen Sie das Ziehelement in eine beliebige Richtung, um die Auswahl anzupassen (das erste Ziehelement bleibt verankert, und ein zweites Ziehelement wird angezeigt). Ziehen Sie eines der Ziehelemente, um weitere Anpassungen vorzunehmen.](images/adjust-selection.png)

Im folgenden Bild wird veranschaulicht, wie Sie das Kontextmenü aufrufen, indem Sie in die Auswahl oder auf ein Ziehelement tippen (Sie können auch Drücken und Halten verwenden).

![Tippen (oder drücken und halten) Sie in die Auswahl oder auf ein Ziehelement, um das Kontextmenü aufzurufen.](images/textselection-show-context.png)

**Beachten Sie**  diese Interaktionen, unterscheiden sich geringfügig bei ein falsch geschriebenes Wort. Wenn Sie auf ein Wort tippen, das als falsch geschrieben gekennzeichnet ist, wird das gesamte Wort hervorgehoben. Außerdem wird das Kontextmenü mit der vorgeschlagenen Schreibweise aufgerufen.

 

**Nicht-bearbeitbaren Inhalt**

Im folgenden Bild wird veranschaulicht, wie Sie ein Wort auswählen, indem Sie in das Wort tippen (die anfängliche Auswahl enthält keine Leerzeichen).

![Tippen Sie in ein Wort, um es auszuwählen (die anfängliche Auswahl enthält keine Leerzeichen).](images/select-word.png)

Gehen Sie für bearbeitbaren Test genauso vor, um die Auswahl anzupassen und das Kontextmenü anzuzeigen.

**Bearbeitung des Objekts**

Verwenden Sie beim Implementieren einer benutzerdefinierten Objektmanipulation in einer UWP-App nach Möglichkeit die gleichen (oder ähnliche) Ziehelementressourcen wie für die Textauswahl. Auf diese Weise kann ein einheitliches Interaktionsverhalten für die gesamte Plattform bereitgestellt werden.

Zielelemente können wie in den folgenden Abbildungen dargestellt z. B. auch in Bildverarbeitungs-Apps verwendet werden, die Größenänderungen und Zuschneiden unterstützen, oder in Media-Player-Apps mit anpassbaren Statusanzeigen.

![Media-Player mit Statusziehelement](images/gripper-mediaplayer.png)

*Media-Player mit anpassbaren Statusanzeige angezeigt.*

![Bild mit Ziehelementen zum Zuschneiden](images/gripper-imagemanip.png)

*Bild-Editor mit ziehelemente zuschneiden.*

## <a name="related-articles"></a>Verwandte Artikel



**Für Entwickler**
* [Benutzerdefinierte Benutzerinteraktionen](https://docs.microsoft.com/windows/uwp/design/layout/index)

**Beispiele**
* [Grundlegende Eingabebeispiel](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: XAML-benutzerbeispiel Eingabeereignisse](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Funktionen-gerätebeispiel](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel zu Leistungstests in Touch Treffer](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML Bildlauf, schwenken und Zoomen Beispiel](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Vereinfachte Freihand-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Beispiel für Windows 8-Gesten](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Manipulationen und Beispiel für Bewegungen (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX-Touch-Eingabe-Beispiel](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




