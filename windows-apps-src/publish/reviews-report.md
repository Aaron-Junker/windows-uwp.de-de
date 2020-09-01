---
Description: Mit dem Bericht "Reviews" in Partner Center können Sie die Überprüfungen (Kommentare) anzeigen, die Kunden bei der Bewertung Ihrer APP im Store eingegeben haben.
title: Bericht „Rezensionen“
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, UWP, Review, comment, Reviewer
ms.localizationpriority: medium
ms.openlocfilehash: feec3577dde9144892ea1ff9f9755074d2af9429
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167334"
---
# <a name="reviews-report"></a>Bericht „Rezensionen“


Mit dem Bericht " **Reviews** " in [Partner Center](https://partner.microsoft.com/dashboard) können Sie die Überprüfungen (Kommentare) anzeigen, die Kunden bei der Bewertung Ihrer APP im Store eingegeben haben.

Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen. Alternativ können Sie diese Daten mithilfe der [Get App Reviews](../monetize/get-app-reviews.md) -Methode in der [Microsoft Store Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)Programm gesteuert abrufen.

Sie können auch [direkt auf dieser Seite](respond-to-customer-reviews.md) oder Programm gesteuert [über die Microsoft Store Reviews-API](../monetize/submit-responses-to-app-reviews.md)auf Kunden Reviews reagieren.

> [!TIP]
> Um in den letzten 30 Tagen die Überprüfungen, BEWERTUNGEN und das Benutzer Feedback für alle Ihre apps anzuzeigen, erweitern Sie im linken Navigationsmenü die Option " **einbinden** ", und wählen Sie über **Prüfungen und Feedback aus.** 


## <a name="apply-filters"></a>Anwenden von Filtern

Am oberen Rand der Seite können Sie den Zeitraum auswählen, in dem Sie die Überprüfungen anzeigen möchten. Die Standardauswahl ist die Gültigkeits **Dauer**, aber Sie können die Überprüfungen für 30 Tage, 3 Monate, 6 Monate oder 12 Monate oder für einen von Ihnen angegebenen benutzerdefinierten Datenbereich anzeigen.

Sie können **Filter** erweitern, um die auf dieser Seite angezeigten Überprüfungen anhand der folgenden Optionen zu filtern. Diese Filter gelten nicht für die **Bewertungs Aufschlüsselung** und die **durchschnittliche Bewertung im Zeit** Diagramm.

-   **Bewertung**: Standardmäßig werden Reviews mit allen Stern Bewertungen überprüft, Sie können jedoch bestimmte Bewertungen überprüfen und deaktivieren (von 1 bis 5 Sternen), wenn Sie nur Überprüfungen sehen möchten, die mit bestimmten Stern Bewertungen verknüpft sind.
- **Inhalt überprüfen**: die Standardeinstellung ist " **Bewertungen mit Inhalt überprüfen**", was bedeutet, dass nur Bewertungen mit Review-Inhalt angezeigt werden. Sie können **alle** auswählen, um alle Bewertungen anzuzeigen, auch solche, die keinen geschriebenen Überprüfungs Text enthalten. Beachten Sie, dass das Diagramm mit den **Bewertungs Aufteilungen** immer alle Reviews anzeigt, unabhängig von Ihrer Auswahl.
-   **Betriebssystemversion**: die Standardeinstellung ist " **alle**". Sie können eine bestimmte Betriebssystemversion auswählen, wenn Sie möchten, dass auf dieser Seite nur die von Kunden in dieser Betriebssystemversion verbleibenden Reviews angezeigt werden.
-   **Paketversion**: Die Standardeinstellung ist **Alle**. Wenn Ihre APP mehr als ein Paket umfasst, können Sie hier ein bestimmtes Paket auswählen, um nur die von Kunden, die dieses Paket bei der Überprüfung der APP hatten, Links anzuzeigen.
-   **Antworten**: Die Standardeinstellung ist **Alle**. Sie können die Rezensionen so filtern, dass nur die Kundenrezensionen angezeigt werden, auf die Sie [geantwortet](respond-to-customer-reviews.md) haben, oder nur die, auf die Sie noch nicht geantwortet haben.
-   **Updates**: Die Standardeinstellung lautet **Alle**. Sie können die Überprüfungen so filtern, dass nur die vom Kunden aktualisierten Überprüfungen angezeigt werden, da Sie [auf eine Überprüfung geantwortet](respond-to-customer-reviews.md)haben, oder nur auf diejenigen, die noch nicht vom Kunden aktualisiert wurden.
-   **Markt**: Die Standardeinstellung ist **Alle Märkte**. Sie können einen bestimmten Markt auswählen, wenn auf dieser Seite nur Rezensionen der in diesem Markt ansässigen Kunden angezeigt werden sollen.
-   **Gerätetyp**: Der Standardfilter ist **Alle Geräte**. Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Rezensionen von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
-   **Kategoriename**: der Standardfilter ist **all**. Sie können eine bestimmte [Kategorie von Review Insights](#review-insight-categories) auswählen, damit nur die Überprüfungen angezeigt werden, die dieser Kategorie zugeordnet sind. 

> [!TIP]
> Wenn auf der Seite keine Reviews angezeigt werden, stellen Sie sicher, dass Ihre Filter nicht alle Ihre Überprüfungen ausgeschlossen haben. Wenn Sie z. B. nach einem Zielbetriebssystem filtern, das von Ihrer App nicht unterstützt wird, werden keine Rezensionen angezeigt.


## <a name="ratings-breakdown"></a>Bewertungs Aufschlüsselung

Das Diagramm mit den **Bewertungs Aufteilungen** wird am Anfang des Berichts angezeigt, sodass Sie sich die folgenden Punkte genauer ansehen können: 
- Die durchschnittliche Bewertungs-Stern-Bewertung für die app.
- Die Gesamtanzahl der Bewertungen ihrer app in den letzten 12 Monaten.
- Die Gesamtanzahl der Bewertungen für jede Stern Bewertung.
- Die Anzahl der Bewertungen für jeden Typ der Bewertung (neu oder überarbeitet) pro Stern Bewertung in den letzten 12 Monaten.
 - **Neue Bewertungen** sind Bewertungen, die Kunden übermittelt, aber überhaupt nicht geändert haben.
 - **Überarbeitete Bewertungen** sind Bewertungen, die vom Kunden in beliebiger Weise geändert wurden, auch wenn nur der Text der Überprüfung geändert wurde.

> [!TIP]
> Die durchschnittliche Bewertung, die ein Kunde im Geschäft sieht, berücksichtigt den Markt-und Gerätetyp des Kunden, sodass er sich von den in diesem Bericht angezeigten Werten unterscheiden kann.

Beachten Sie, dass in diesem Diagramm immer alle Ihre Überprüfungen enthalten sind, auch wenn Sie im Filter **Inhaltsseite überprüfen** die Option **Bewertungen mit Inhalt über** prüfen ausgewählt haben.

Dieses Diagramm kann auch im [Bewertungsbericht](ratings-report.md)zusammen mit weiteren Details zu den Bewertungen ihrer App eingesehen werden.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Insight-Kategorien

Das Diagramm mit den **Insight-Kategorien** gruppiert Ihre Überprüfungen gemäß den von uns ermittelten Kategorien, die mit der Überprüfung verknüpft werden können.

> [!NOTE]
> Überprüfungen, die älter als 24 Stunden alt und/oder in einer anderen Sprache als Englisch sind, sind nicht enthalten, wenn Sie Reviews nach Kategorien anzeigen.

Am oberen Rand der Seite werden farbige Blöcke angezeigt, die die Überprüfungen nach Kategorie darstellen. Wählen Sie eine dieser Kategorien aus, um nur Überprüfungen anzuzeigen, die dieser Kategorie zugeordnet sind. Sie können auch die [Seiten Filter](#apply-filters) verwenden, um nach Kategorie zu filtern.

Wählen Sie **Details anzeigen**aus, um eine Aufschlüsselung der Anzahl von Überprüfungen pro Kategorie anzuzeigen. 


## <a name="reviews"></a>Überprüfungen

Jede Kundenrezension enthält Folgendes:

-   Den vom Kunden eingegebenen Titel und Rezensionstext. (Von Kunden unter Windows Phone 8.1 und früher verfasste Rezensionen haben keinen Titel.)
-   Das Datum der Rezension.
-   Der Name des Reviewers, wie er in der Microsoft Store angezeigt wird.
-   Das Land/die Region des Verfassers.
-   Die Paketversion der App, die auf dem Kundengerät installiert war, als die Rezension geschrieben wurde. (Für Rezensionen, die online abgegebenen oder von Kunden mit Windows 8.1 oder einem früheren Betriebssystem geschrieben wurden, ist diese Information nicht verfügbar.)
-   Die Betriebssystemversion des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat.
-   Der Name des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat. (Für Rezensionen, die online abgegebenen oder von Kunden mit Windows 8.1 oder einem früheren Betriebssystem geschrieben wurden, ist diese Information nicht verfügbar.)
-   Die von anderen Kunden, die die Rezension gelesen haben, abgegebene Bewertung für die Brauchbarkeit der Rezension. Sie wird in Form von zwei Zahlengruppen angezeigt: die erste Gruppe gibt an, wie viele Kunden die Rezension als hilfreich bewertet haben, und die zweite entspricht der Gesamtanzahl der Kunden, die die Rezension bewertet haben. 4/10 bedeutet z. B., dass vier von zehn Bewertern die Rezension hilfreich fanden und sechs nicht. (Wenn eine Rezension nicht bewertet wurde, ist auch kein Wert zur Brauchbarkeit angegeben.)

Beachten Sie, dass Kunden eine Bewertung für Ihre App abgeben können, ohne einen Kommentar zu schreiben. Daher sehen Sie normalerweise weniger Rezensionen als Bewertungen.

Sie können die Rezensionen auf der Seite nach Datum und/oder Bewertung in aufsteigender oder absteigender Reihenfolge sortieren. Klicken Sie auf den Link **Sortieren** nach, um die Optionen für das Sortieren nach **Datum** und/oder **Bewertung**anzuzeigen.

Sie können auch das Suchfeld verwenden, um in den Überprüfungen Ihrer APP nach bestimmten Wörtern oder Ausdrücken zu suchen. Beachten Sie, dass nur der ursprüngliche, vom Kunden geschriebene Überprüfungs Text durchsucht wird, auch wenn die Überprüfung in einer anderen Sprache geschrieben wurde. Der übersetzte Überprüfungs Text wird nicht durchsucht.

> [!NOTE]
> Gelegentlich bemerken Sie, dass die Überprüfungen in diesem Bericht verschwinden. Dies kann passieren, da Microsoft Rezensionen aus dem Store entfernt, die von Kunden mit bestimmten Vorabversionen und Insider-Builds von Windows 10 geschrieben werden. Wir tun dies, um die Möglichkeit einer negativen Rezension zu verringern, die durch ein Problem in einer Vorabversion des Windows-Builds verursacht wird. Unter Umständen entfernen wir auch Rezensionen aus dem Store, die als Spam erkannt wurden, unangemessene oder anstößige Inhalte enthalten oder anderweitig gegen die Richtlinien verstoßen. Diese Verfahrensweise soll die Benutzerfreundlichkeit für unsere Kunden erhöhen.


## <a name="translating-reviews"></a>Übersetzen von Rezensionen

Rezensionen, die nicht in Ihrer bevorzugten Sprache verfasst wurden, werden standardmäßig für Sie übersetzt. Falls gewünscht, können Sie die Übersetzung von Rezensionen deaktivieren, indem Sie das Kontrollkästchen **Rezensionen übersetzen** oben rechts über der Liste der Rezensionen deaktivieren.

Da die Rezensionen durch ein automatisches Übersetzungssystem übersetzt werden, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.

Wie bereits erwähnt, wird beim Durchsuchen der Überprüfungen nur der ursprüngliche Text, der vom Kunden übrig bleibt, durchsucht (und nicht der übersetzte Text), selbst wenn Sie das Kontrollkästchen **übersetzen** überprüft haben.


## <a name="responding-to-customer-reviews"></a>Reagieren auf Kundenrezensionen

Sie können [Partner Center](https://partner.microsoft.com/dashboard) oder die [Microsoft Store Reviews-API](../monetize/submit-responses-to-app-reviews.md) verwenden, um Antworten an viele der Überprüfungen Ihrer Kunden zu senden. Weitere Informationen finden Sie unter [Reagieren auf Kundenrezensionen](respond-to-customer-reviews.md).

Im Folgenden finden Sie einige zusätzliche Aktionen, die Sie basierend auf den angezeigten Bewertungen und Rezensionen in Erwägung ziehen sollten.

-   Erhalten Sie zahlreiche Rezensionen, die eine neue oder geänderte Funktion vorschlagen oder ein Problem reklamieren, sollten Sie erwägen, eine neue Version zu veröffentlichen, in der die im Feedback angesprochenen Probleme behoben sind. (Achten Sie darauf, die [Beschreibung](./create-app-store-listings.md) Ihrer App zu aktualisieren, um anzugeben, dass das Problem behoben wurde.)
-   Wenn die durchschnittliche Bewertung zwar hoch ist, die Anzahl der Downloads jedoch gering ausfällt, sollten Sie nach weiteren Methoden suchen, den [Bekanntheitsgrad Ihrer App zu steigern](attract-customers-and-promote-your-apps.md), da sie bei Benutzern, die sie getestet haben, gut angekommen ist.


 

 

 