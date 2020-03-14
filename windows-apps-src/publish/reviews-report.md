---
Description: Mit dem Bericht "Reviews" in Partner Center können Sie die Überprüfungen (Kommentare) anzeigen, die Kunden bei der Bewertung Ihrer APP im Store eingegeben haben.
title: Bericht „Rezensionen“
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, UWP, Review, comment, Reviewer
ms.localizationpriority: medium
ms.openlocfilehash: 7ec883e7bcb98d69673b520df918e085182d35ec
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210986"
---
# <a name="reviews-report"></a>Bericht „Rezensionen“


Mit dem Bericht " **Reviews** " in [Partner Center](https://partner.microsoft.com/dashboard) können Sie die Überprüfungen (Kommentare) anzeigen, die Kunden bei der Bewertung Ihrer APP im Store eingegeben haben.

Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen. Alternativ können Sie diese Daten mithilfe der [Get App Reviews](../monetize/get-app-reviews.md) -Methode in der [Microsoft Store Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)Programm gesteuert abrufen.

Sie können auch [direkt auf dieser Seite](respond-to-customer-reviews.md) oder Programm gesteuert [über die Microsoft Store Reviews-API](../monetize/submit-responses-to-app-reviews.md)auf Kunden Reviews reagieren.

> [!TIP]
> Um Rezensionen, Bewertungen und Benutzerfeedback für alle Ihre Apps in den letzten 30 Tagen anzusehen, erweitern Sie **Einbeziehen** im linken Navigationsmenü und wählen Sie **Kritiken und Feedback** aus. 


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Rezensionen angezeigt werden sollen. Die Standardeinstellung ist **Lebensdauer**, aber Sie können Rezensionen für 30 Tage, 3, 6 oder 12 Monate anzeigen, oder für einen benutzerdefinierten Zeitraum, den Sie angeben.

Sie können **Filter** erweitern, um alle angezeigten Rezensionen auf dieser Seite mit den folgenden Optionen zu filtern. Diese Filter werden nicht auf die Diagramme **Rezensionsübersicht** und **Durchschnittliche Bewertung im Laufe der Zeit** angewandt.

-   **Bewertung**: Standardmäßig ist die Rezension „Alle Sterne“ aktiviert, Sie können jedoch einzelne Bewertungen (von 1 bis 5 Sternen) aktivieren bzw. deaktivieren, wenn Sie nur Rezensionen mit einer bestimmten Sternebewertung anzeigen möchten.
- **Inhalt überprüfen**: die Standardeinstellung ist " **Bewertungen mit Inhalt überprüfen**", was bedeutet, dass nur Bewertungen mit Review-Inhalt angezeigt werden. Sie können **alle** auswählen, um alle Bewertungen anzuzeigen, auch solche, die keinen geschriebenen Überprüfungs Text enthalten. Beachten Sie, dass das Diagramm mit den **Bewertungs Aufteilungen** immer alle Reviews anzeigt, unabhängig von Ihrer Auswahl.
-   **Betriebssystemversion**: Die Standardeinstellung ist **Alle**. Sie können eine bestimmte Betriebssystemversion auswählen, wenn auf dieser Seite nur Rezensionen von Kunden angezeigt werden sollen, die diese Betriebssystemversion verwenden.
-   **Paketversion**: Die Standardeinstellung ist **Alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein spezifisches Paket auswählen, um nur Rezensionen von Kunden anzuzeigen, die für die Rezension der App dieses Paket verwendet haben.
-   **Antworten**: Die Standardeinstellung ist **Alle**. Sie können die Rezensionen so filtern, dass nur die Kundenrezensionen angezeigt werden, auf die Sie [geantwortet](respond-to-customer-reviews.md) haben, oder nur die, auf die Sie noch nicht geantwortet haben.
-   **Updates**: Die Standardeinstellung lautet **Alle**. Sie können die Rezensionen so filtern, dass nur diejenigen angezeigt werden, die vom Kunden aktualisiert wurden, nachdem Sie auf eine [Rezension geantwortet haben](respond-to-customer-reviews.md), oder nur die, die noch nicht vom Kunden aktualisiert wurden.
-   **Markt**: Die Standardeinstellung ist **Alle Märkte**. Sie können einen bestimmten Markt auswählen, wenn auf dieser Seite nur Rezensionen der in diesem Markt ansässigen Kunden angezeigt werden sollen.
-   **Gerätetyp**: Der Standardfilter ist **Alle Geräte**. Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Rezensionen von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
-   **Kategoriename**: Der Standardfilter ist **Immer**. Sie können eine bestimmte [Kategorie von Review Insights](#review-insight-categories) auswählen, damit nur die Überprüfungen angezeigt werden, die dieser Kategorie zugeordnet sind. 

> [!TIP]
> Wenn auf der Seite keine Rezensionen zu sehen sind, stellen Sie sicher, dass Sie mit Ihrer Filterauswahl nicht alle Rezensionen ausgeschlossen haben. Wenn Sie z. B. nach einem Zielbetriebssystem filtern, das von Ihrer App nicht unterstützt wird, werden keine Rezensionen angezeigt.


## <a name="ratings-breakdown"></a>Rezensionsübersicht

Das Diagramm mit den **Bewertungs Aufteilungen** wird am Anfang des Berichts angezeigt, sodass Sie sich die folgenden Punkte genauer ansehen können: 
- Den durchschnittlichen Bewertungsstern der Rezensionsübersicht für die App.
- Die Gesamtanzahl der Bewertungen Ihrer App in den letzten 12 Monaten.
- Die Gesamtanzahl der Bewertungen für jede Bewertung von Sternen.
- Die Anzahl der Bewertungen für jeden Bewertungstyp (neu oder überarbeitet) pro Sterne-Bewertung in den letzten 12 Monaten.
 - **Neue Bewertungen** sind Bewertungen, die Kunden abgegeben, aber nicht geändert haben.
 - **Überarbeitete Bewertungen** Bewertungen, die vom Kunden in keinster Weise geändert wurden, auch wenn es sich nur um Textänderungen handelt.

> [!TIP]
> Bei der durchschnittlichen Bewertung, die einem Kunden im Store angezeigt wird, werden der Markt und der Gerätetyp des Kunden berücksichtigt. Daher können die Inhalte in diesem Bericht davon abweichen.

Beachten Sie, dass in diesem Diagramm immer alle Ihre Überprüfungen enthalten sind, auch wenn Sie im Filter **Inhaltsseite überprüfen** die Option **Bewertungen mit Inhalt über** prüfen ausgewählt haben.

Dieses Diagramm kann auch im [Bewertungsbericht](ratings-report.md)zusammen mit weiteren Details zu den Bewertungen ihrer App eingesehen werden.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Insight-Kategorien

Das Diagramm mit den **Insight-Kategorien** gruppiert Ihre Überprüfungen gemäß den von uns ermittelten Kategorien, die mit der Überprüfung verknüpft werden können.

> [!NOTE]
> Rezensionen, die weniger als 24 Stunden alt sind bzw. in einer anderen Sprache als Englisch angegeben wurden, sind nicht darin enthalten, wenn Berichte nach Kategorien anzeigt werden.

Im oberen Bereich der Seite sehen Sie farbige Blöcke, die Berichte nach Kategorie darstellen. Wählen Sie eine der Kategorien aus, um nur Rezensionen anzeigen, die wir dieser Kategorie zugeordnet haben. Sie können auch die [Seite „Filter”](#apply-filters) nach Kategorie filtern.

Um eine Übersicht über die Anzahl der Rezensionen pro Kategorie anzuzeigen, wählen Sie **Details anzeigen**. 


## <a name="reviews"></a>Rezensionen

Jede Kundenrezension enthält Folgendes:

-   Den vom Kunden eingegebenen Titel und Rezensionstext. (Von Kunden unter Windows Phone 8.1 und früher verfasste Rezensionen haben keinen Titel.)
-   Das Datum der Rezension.
-   Der Name des Reviewers, wie er in der Microsoft Store angezeigt wird.
-   Das Land/die Region des Verfassers.
-   Die Paketversion der App, die auf dem Kundengerät installiert war, als die Rezension geschrieben wurde. (Diese Informationen sind nicht verfügbar für Überprüfungen, die Online übermittelt oder von Kunden an Windows 8.1 und früher übermittelt wurden.)
-   Die Betriebssystemversion des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat.
-   Der Name des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat. (Diese Informationen sind nicht verfügbar für Überprüfungen, die Online übermittelt oder von Kunden an Windows 8.1 und früher übermittelt wurden.)
-   Die von anderen Kunden, die die Rezension gelesen haben, abgegebene Bewertung für die Brauchbarkeit der Rezension. Sie wird in Form von zwei Zahlengruppen angezeigt: die erste Gruppe gibt an, wie viele Kunden die Rezension als hilfreich bewertet haben, und die zweite entspricht der Gesamtanzahl der Kunden, die die Rezension bewertet haben. 4/10 bedeutet z. B., dass vier von zehn Bewertern die Rezension hilfreich fanden und sechs nicht. (Wenn eine Rezension nicht bewertet wurde, ist auch kein Wert zur Brauchbarkeit angegeben.)

Beachten Sie, dass Kunden eine Bewertung für Ihre App abgeben können, ohne einen Kommentar zu schreiben. Daher sehen Sie normalerweise weniger Rezensionen als Bewertungen.

Sie können die Rezensionen auf der Seite nach Datum und/oder Bewertung in aufsteigender oder absteigender Reihenfolge sortieren. Klicken Sie auf den Link **Sortieren** nach, um die Optionen für das Sortieren nach **Datum** und/oder **Bewertung**anzuzeigen.

Sie können auch das Suchfeld verwenden, um in den Überprüfungen Ihrer APP nach bestimmten Wörtern oder Ausdrücken zu suchen. Beachten Sie, dass nur der ursprüngliche, vom Kunden geschriebene Überprüfungs Text durchsucht wird, auch wenn die Überprüfung in einer anderen Sprache geschrieben wurde. Der übersetzte Überprüfungs Text wird nicht durchsucht.

> [!NOTE]
> Es kann vorkommen, dass Rezensionen in diesem Bericht nicht mehr angezeigt werden. Dies kann passieren, da Microsoft Rezensionen aus dem Store entfernt, die von Kunden mit bestimmten Vorabversionen und Insider-Builds von Windows 10 geschrieben werden. Wir tun dies, um die Möglichkeit einer negativen Rezension zu verringern, die durch ein Problem in einer Vorabversion des Windows-Builds verursacht wird. Unter Umständen entfernen wir auch Rezensionen aus dem Store, die als Spam erkannt wurden, unangemessene oder anstößige Inhalte enthalten oder anderweitig gegen die Richtlinien verstoßen. Diese Verfahrensweise soll die Benutzerfreundlichkeit für unsere Kunden erhöhen.


## <a name="translating-reviews"></a>Übersetzen von Rezensionen

Rezensionen, die nicht in Ihrer bevorzugten Sprache verfasst wurden, werden standardmäßig für Sie übersetzt. Falls gewünscht, können Sie die Übersetzung von Rezensionen deaktivieren, indem Sie das Kontrollkästchen **Rezensionen übersetzen** oben rechts über der Liste der Rezensionen deaktivieren.

Da die Rezensionen durch ein automatisches Übersetzungssystem übersetzt werden, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.

Wie bereits erwähnt, wird beim Durchsuchen der Überprüfungen nur der ursprüngliche Text, der vom Kunden übrig bleibt, durchsucht (und nicht der übersetzte Text), selbst wenn Sie das Kontrollkästchen **übersetzen** überprüft haben.


## <a name="responding-to-customer-reviews"></a>Reagieren auf Kundenrezensionen

Sie können [Partner Center](https://partner.microsoft.com/dashboard) oder die [Microsoft Store Reviews-API](../monetize/submit-responses-to-app-reviews.md) verwenden, um Antworten an viele der Überprüfungen Ihrer Kunden zu senden. Weitere Informationen finden Sie unter [Reagieren auf Kundenrezensionen](respond-to-customer-reviews.md).

Im Folgenden finden Sie einige zusätzliche Aktionen, die Sie basierend auf den angezeigten Bewertungen und Rezensionen in Erwägung ziehen sollten.

-   Erhalten Sie zahlreiche Rezensionen, die eine neue oder geänderte Funktion vorschlagen oder ein Problem reklamieren, sollten Sie erwägen, eine neue Version zu veröffentlichen, in der die im Feedback angesprochenen Probleme behoben sind. (Achten Sie darauf, die [Beschreibung](create-app-descriptions.md) Ihrer App zu aktualisieren, um anzugeben, dass das Problem behoben wurde.)
-   Wenn die durchschnittliche Bewertung zwar hoch ist, die Anzahl der Downloads jedoch gering ausfällt, sollten Sie nach weiteren Methoden suchen, den [Bekanntheitsgrad Ihrer App zu steigern](attract-customers-and-promote-your-apps.md), da sie bei Benutzern, die sie getestet haben, gut angekommen ist.


 

 

 
