---
Description: Der Bericht "Reviews" im Partner Center können Sie die Bewertungen (Kommentare) anzuzeigen, die Kunden eingegeben werden, wenn Ihre app in den Store zu bewerten.
title: Bericht „Rezensionen“
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, Uwp, überprüfen Sie, Kommentar, reviewer
ms.localizationpriority: medium
ms.openlocfilehash: 7ec883e7bcb98d69673b520df918e085182d35ec
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621825"
---
# <a name="reviews-report"></a>Bericht „Rezensionen“


Die **überprüft** Bericht im [Partner Center](https://partner.microsoft.com/dashboard) können Sie die Überprüfungen (Kommentare), die Kunden eingegeben werden, wenn Ihre app in den Store zu bewerten.

Sie können diese Daten anzeigen, im Partner Center oder [Bericht herunterladen](download-analytic-reports.md) offline anzeigen. Alternativ können Sie diese Daten programmgesteuert abrufen, indem Sie mit der [Bewertungen zu Apps abrufen](../monetize/get-app-reviews.md) -Methode in der die [Microsoft Store Analytics-REST-API](../monetize/access-analytics-data-using-windows-store-services.md).

Sie können auch auf Überprüfungen durch den Kunden reagieren [direkt auf dieser Seite](respond-to-customer-reviews.md) oder programmgesteuert [über den Microsoft Store überprüft API](../monetize/submit-responses-to-app-reviews.md).

> [!TIP]
> Um Rezensionen, Bewertungen und Benutzerfeedback für alle Ihre Apps in den letzten 30 Tagen anzusehen, erweitern Sie **Einbeziehen** im linken Navigationsmenü und wählen Sie **Kritiken und Feedback** aus. 


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Rezensionen angezeigt werden sollen. Die Standardeinstellung ist **Lebensdauer**, aber Sie können Rezensionen für 30 Tage, 3, 6 oder 12 Monate anzeigen, oder für einen benutzerdefinierten Zeitraum, den Sie angeben.

Sie können **Filter** erweitern, um alle angezeigten Rezensionen auf dieser Seite mit den folgenden Optionen zu filtern. Diese Filter werden nicht auf die Diagramme **Rezensionsübersicht** und **Durchschnittliche Bewertung im Laufe der Zeit** angewandt.

-   **Bewertung**: Reviews mit allen sternbewertungen sind standardmäßig aktiviert, aber Sie können überprüfen, und deaktivieren Sie die spezifische Bewertungen (von 1 bis 5 Sterne), wenn Sie nur bestimmte sternbewertungen Rezensionen anzeigen möchten.
- **Überprüfen Sie Inhalt**: Die Standardeinstellung ist **Bewertungen mit Inhalt überprüfen**, was bedeutet, dass nur die Bewertungen mit Inhalt lesen wird angezeigt. Sie können auswählen, **alle** um alle Bewertungen anzeigen zu können, auch solche, die alle geschrieben enthalten Text überprüfen. Beachten Sie, dass die **Bewertungen Aufschlüsselung** Diagramm werden alle Bewertungen, unabhängig von Ihrer Auswahl immer angezeigt.
-   **Version des Betriebssystems**: Die Standardeinstellung ist **alle**. Sie können eine bestimmte Betriebssystemversion auswählen, wenn auf dieser Seite nur Rezensionen von Kunden angezeigt werden sollen, die diese Betriebssystemversion verwenden.
-   **Paketversion**: Die Standardeinstellung ist **alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein spezifisches Paket auswählen, um nur Rezensionen von Kunden anzuzeigen, die für die Rezension der App dieses Paket verwendet haben.
-   **Antworten**: Die Standardeinstellung ist **alle**. Sie können die Rezensionen so filtern, dass nur die Kundenrezensionen angezeigt werden, auf die Sie [geantwortet](respond-to-customer-reviews.md) haben, oder nur die, auf die Sie noch nicht geantwortet haben.
-   **Updates**: Die Standardeinstellung ist **alle**. Sie können die Rezensionen so filtern, dass nur diejenigen angezeigt werden, die vom Kunden aktualisiert wurden, nachdem Sie auf eine [Rezension geantwortet haben](respond-to-customer-reviews.md), oder nur die, die noch nicht vom Kunden aktualisiert wurden.
-   **Markt**: Die Standardeinstellung ist **alle Märkte**. Sie können einen bestimmten Markt auswählen, wenn auf dieser Seite nur Rezensionen der in diesem Markt ansässigen Kunden angezeigt werden sollen.
-   **Gerätetyp**: Der Standardfilter ist **alle Geräte**. Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Rezensionen von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
-   **Kategorienamen**: Der Standardfilter ist **alle**. Können Sie eine bestimmte [überprüfen Sie die Kategorie Insight](#review-insight-categories) an nur Bewertungen, dass wir diese Kategorie zugeordnet haben. 

> [!TIP]
> Wenn auf der Seite keine Rezensionen zu sehen sind, stellen Sie sicher, dass Sie mit Ihrer Filterauswahl nicht alle Rezensionen ausgeschlossen haben. Wenn Sie z. B. nach einem Zielbetriebssystem filtern, das von Ihrer App nicht unterstützt wird, werden keine Rezensionen angezeigt.


## <a name="ratings-breakdown"></a>Rezensionsübersicht

Die **Bewertungen Aufschlüsselung** Diagramm oben in diesem Bericht angezeigt wird, so, dass Sie einen kurzen Blick auf die folgenden abrufen können: 
- Den durchschnittlichen Bewertungsstern der Rezensionsübersicht für die App.
- Die Gesamtanzahl der Bewertungen Ihrer App in den letzten 12 Monaten.
- Die Gesamtanzahl der Bewertungen für jede Bewertung von Sternen.
- Die Anzahl der Bewertungen für jeden Bewertungstyp (neu oder überarbeitet) pro Sterne-Bewertung in den letzten 12 Monaten.
 - **Neue Bewertungen** sind Bewertungen, die Kunden abgegeben, aber nicht geändert haben.
 - **Überarbeitete Bewertungen** Bewertungen, die vom Kunden in keinster Weise geändert wurden, auch wenn es sich nur um Textänderungen handelt.

> [!TIP]
> Bei der durchschnittlichen Bewertung, die einem Kunden im Store angezeigt wird, werden der Markt und der Gerätetyp des Kunden berücksichtigt. Daher können die Inhalte in diesem Bericht davon abweichen.

Beachten Sie, dass in diesem Diagramm immer alle Ihre Bewertungen, enthält auch wenn Sie **Bewertungen mit Inhalt überprüfen** in die **Inhalt überprüfen** Seite zu filtern.

In diesem Diagramm sehen Sie auch in der [Bewertungen Bericht](ratings-report.md), sowie weitere Details zu Ihrer app-Bewertungen.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Insight-Kategorien

Die **Insight Kategorien** Diagramm gruppiert Ihre Berichte nach Kategorien, die wir ermittelt haben, mit der Überprüfung zugeordnet werden kann.

> [!NOTE]
> Rezensionen, die weniger als 24 Stunden alt sind bzw. in einer anderen Sprache als Englisch angegeben wurden, sind nicht darin enthalten, wenn Berichte nach Kategorien anzeigt werden.

Im oberen Bereich der Seite sehen Sie farbige Blöcke, die Berichte nach Kategorie darstellen. Wählen Sie eine der Kategorien aus, um nur Rezensionen anzeigen, die wir dieser Kategorie zugeordnet haben. Sie können auch die [Seite „Filter”](#apply-filters) nach Kategorie filtern.

Um eine Übersicht über die Anzahl der Rezensionen pro Kategorie anzuzeigen, wählen Sie **Details anzeigen**. 


## <a name="reviews"></a>Rezensionen

Jede Kundenrezension enthält Folgendes:

-   Den vom Kunden eingegebenen Titel und Rezensionstext. (Von Kunden unter Windows Phone 8.1 und früher verfasste Rezensionen haben keinen Titel.)
-   Das Datum der Rezension.
-   Der Name des Bearbeiters wie wird in den Microsoft Store angezeigt.
-   Das Land/die Region des Verfassers.
-   Die Paketversion der App, die auf dem Kundengerät installiert war, als die Rezension geschrieben wurde. (Diese Informationen ist nicht verfügbar für die Überprüfung online gesendet oder vom Kunden, die auf Windows 8.1 und früher übermittelt werden.)
-   Die Betriebssystemversion des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat.
-   Der Name des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat. (Diese Informationen ist nicht verfügbar für die Überprüfung online gesendet oder vom Kunden, die auf Windows 8.1 und früher übermittelt werden.)
-   Die von anderen Kunden, die die Rezension gelesen haben, abgegebene Bewertung für die Brauchbarkeit der Rezension. Sie wird in Form von zwei Zahlengruppen angezeigt: die erste Gruppe gibt an, wie viele Kunden die Rezension als hilfreich bewertet haben, und die zweite entspricht der Gesamtanzahl der Kunden, die die Rezension bewertet haben. 4/10 bedeutet z. B., dass vier von zehn Bewertern die Rezension hilfreich fanden und sechs nicht. (Wenn eine Rezension nicht bewertet wurde, ist auch kein Wert zur Brauchbarkeit angegeben.)

Beachten Sie, dass Kunden eine Bewertung für Ihre App abgeben können, ohne einen Kommentar zu schreiben. Daher sehen Sie normalerweise weniger Rezensionen als Bewertungen.

Sie können die Rezensionen auf der Seite nach Datum und/oder Bewertung in aufsteigender oder absteigender Reihenfolge sortieren. Klicken Sie auf die **sortieren, indem** Link zum Anzeigen von Optionen zum Sortieren nach **Datum** und/oder **Bewertung**.

Sie können auch im Suchfeld verwenden, um nach bestimmten Wörtern oder Ausdrücken in Ihrer app-Bewertungen zu suchen. Beachten Sie, dass nur der ursprüngliche Überprüfung Text vom Kunden geschriebene durchsucht wird, auch wenn die Überprüfung in einer anderen Sprache geschrieben wurde. Übersetzte Review Text wird nicht durchsucht.

> [!NOTE]
> Es kann vorkommen, dass Rezensionen in diesem Bericht nicht mehr angezeigt werden. Dies kann passieren, da Microsoft Rezensionen aus dem Store entfernt, die von Kunden mit bestimmten Vorabversionen und Insider-Builds von Windows 10 geschrieben werden. Wir tun dies, um die Möglichkeit einer negativen Rezension zu verringern, die durch ein Problem in einer Vorabversion des Windows-Builds verursacht wird. Unter Umständen entfernen wir auch Rezensionen aus dem Store, die als Spam erkannt wurden, unangemessene oder anstößige Inhalte enthalten oder anderweitig gegen die Richtlinien verstoßen. Diese Verfahrensweise soll die Benutzerfreundlichkeit für unsere Kunden erhöhen.


## <a name="translating-reviews"></a>Übersetzen von Rezensionen

Rezensionen, die nicht in Ihrer bevorzugten Sprache verfasst wurden, werden standardmäßig für Sie übersetzt. Falls gewünscht, können Sie die Übersetzung von Rezensionen deaktivieren, indem Sie das Kontrollkästchen **Rezensionen übersetzen** oben rechts über der Liste der Rezensionen deaktivieren.

Da die Rezensionen durch ein automatisches Übersetzungssystem übersetzt werden, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.

Wie oben erwähnt, wenn suchen Ihre Bewertungen, wird nur der ursprüngliche Text links vom Kunden durchsucht (und keine übersetzten Text), auch wenn Sie die **Reviews übersetzen** Feld aktiviert.


## <a name="responding-to-customer-reviews"></a>Reagieren auf Kundenrezensionen

Können Sie [Partner Center](https://partner.microsoft.com/dashboard) oder [Microsoft Store überprüft API](../monetize/submit-responses-to-app-reviews.md) zum Senden von Antworten auf viele Ihrer Kunden Überprüfungen. Weitere Informationen finden Sie unter [Reagieren auf Kundenrezensionen](respond-to-customer-reviews.md).

Im Folgenden finden Sie einige zusätzliche Aktionen, die Sie basierend auf den angezeigten Bewertungen und Rezensionen in Erwägung ziehen sollten.

-   Erhalten Sie zahlreiche Rezensionen, die eine neue oder geänderte Funktion vorschlagen oder ein Problem reklamieren, sollten Sie erwägen, eine neue Version zu veröffentlichen, in der die im Feedback angesprochenen Probleme behoben sind. (Achten Sie darauf, die [Beschreibung](create-app-descriptions.md) Ihrer App zu aktualisieren, um anzugeben, dass das Problem behoben wurde.)
-   Wenn die durchschnittliche Bewertung zwar hoch ist, die Anzahl der Downloads jedoch gering ausfällt, sollten Sie nach weiteren Methoden suchen, den [Bekanntheitsgrad Ihrer App zu steigern](attract-customers-and-promote-your-apps.md), da sie bei Benutzern, die sie getestet haben, gut angekommen ist.


 

 

 
