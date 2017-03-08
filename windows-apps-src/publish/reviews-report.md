---
author: jnHs
Description: "Der Bericht „Rezensionen“ im Windows Dev Center-Dashboard gibt Aufschluss über die Kommentare, die Kunden beim Bewerten Ihrer App im Store eingegeben haben."
title: "Bericht „Rezensionen“"
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 54666642e67581bebbb35049f7779086d23939a0
ms.lasthandoff: 02/07/2017

---

# <a name="reviews-report"></a>Bericht „Rezensionen“


Der Bericht **Rezensionen** im Windows Dev Center-Dashboard gibt Aufschluss über die Kommentare, die Kunden beim Bewerten Ihrer App im Store eingegeben haben. Sie können diese Informationen in Ihrem Dashboard anzeigen oder [den Bericht herunterladen](download-analytic-reports.md), um Ihre Daten offline anzuzeigen. Sie können diese Daten auch programmgesteuert mit der [get app reviews](../monetize/get-app-reviews.md)-Methode der [Windows Store-REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

> **Hinweis:** Auf dieser Seite können Sie auch [auf Kundenrezensionen reagieren](respond-to-customer-reviews.md).

Dieser Bericht zeigt der Anzahl der Sterne, mit denen ein Kunde Ihre App beim Hinterlassen einer Rezension bewertet hat, ohne die Sterne-Bewertungen für die App zu analysieren. Statistiken zu Ihren Bewertungen finden Sie im [Bericht „Bewertungen“](ratings-report.md).

Beachten Sie, dass Kunden eine Bewertung für Ihre App abgeben können, ohne einen Kommentar zu schreiben. Daher sehen Sie normalerweise weniger Rezensionen als Bewertungen. Auf dieser Seite werden standardmäßig auch Bewertungen angezeigt, die keine Rezension enthalten. Mithilfe der **Seitenfilter** können Sie aber auch nur die Bewertungen anzeigen, die Rezensionen enthalten, wie unten beschrieben.

Jede Kundenrezension enthält Folgendes:

-   Den vom Kunden eingegebenen Titel und Rezensionstext. (Von Kunden unter Windows Phone 8.1 und früher verfasste Rezensionen haben keinen Titel.)
-   Das Datum der Rezension.
-   Der Name des Verfassers, wie im Windows Store angezeigt.
-   Das Land/die Region des Verfassers.
-   Die Paketversion der App, die auf dem Kundengerät installiert war, als die Rezension geschrieben wurde. (Für Rezensionen, die online abgegebenen oder von Kunden mit Windows 8.1 oder einem früheren Betriebssystem geschrieben wurden, ist diese Information nicht verfügbar.)
-   Die Betriebssystemversion des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat.
-   Der Name des Geräts, das der Kunde beim Hinterlassen der Rezension verwendet hat. (Für Rezensionen, die online abgegebenen oder von Kunden mit Windows 8.1 oder einem früheren Betriebssystem geschrieben wurden, ist diese Information nicht verfügbar.)
-   Die von anderen Kunden, die die Rezension gelesen haben, abgegebene Bewertung für die Brauchbarkeit der Rezension. Sie wird in Form von zwei Zahlengruppen angezeigt: die erste Gruppe gibt an, wie viele Kunden die Rezension als hilfreich bewertet haben, und die zweite entspricht der Gesamtanzahl der Kunden, die die Rezension bewertet haben. 4/10 bedeutet z. B., dass vier von zehn Bewertern die Rezension hilfreich fanden und sechs nicht. (Wenn eine Rezension nicht bewertet wurde, ist auch kein Wert zur Brauchbarkeit angegeben.)

> **Hinweis:** Es kann vorkommen, dass Rezensionen in diesem Bericht nicht mehr angezeigt werden. Dies kann passieren, da Microsoft Rezensionen aus dem Store entfernt, die von Kunden mit bestimmten Vorabversionen und Insider-Builds von Windows 10 geschrieben werden. Wir tun dies, um die Möglichkeit einer negativen Rezension zu verringern, die durch ein Problem in einer Vorabversion des Windows-Builds verursacht wird. Unter Umständen entfernen wir auch Rezensionen aus dem Store, die als Spam erkannt wurden, unangemessene oder anstößige Inhalte enthalten oder anderweitig gegen die Richtlinien verstoßen. Diese Verfahrensweise soll die Benutzerfreundlichkeit für unsere Kunden erhöhen.

## <a name="apply-filters"></a>Anwenden von Filtern


Im oberen Seitenbereich können Sie **Filter anwenden** erweitern, um alle Daten auf dieser Seite zu filtern.

>**Tipp** Wenn auf der Seite keine Rezensionen zu sehen sind, stellen Sie sicher, dass Sie mit Ihrer Filterauswahl nicht alle Rezensionen ausgeschlossen haben. Wenn Sie z. B. nach einem Zielbetriebssystem filtern, das von Ihrer App nicht unterstützt wird, werden keine Rezensionen angezeigt.

-   **Bewertung**: Standardmäßig ist die Bewertung „Alle Sterne“ aktiviert, Sie können jedoch einzelne Bewertungen (von 1 bis 5 Sternen) aktivieren bzw. deaktivieren, wenn Sie nur Rezensionen mit einer bestimmten Sternebewertung anzeigen möchten.
-   **Ab**: Der Standardwert (leer) zeigt Rezensionen ab der ersten Veröffentlichung Ihrer App an. Sie können ein anderes Datum auswählen, wenn Sie nur Rezensionen anzeigen möchten, die am oder nach dem ausgewählten Datum geschrieben wurden.
-   **Bis**: Der Standardwert (leer) zeigt Rezensionen bis zum aktuellen Datum an. Sie können ein anderes Datum auswählen, wenn Sie nur Rezensionen anzeigen möchten, die vor oder an dem ausgewählten Datum geschrieben wurden.
-   **Rezensionsinhalt**: Die Standardeinstellung ist **Alle** und schließt Bewertungen ohne Rezensionstext ein. Sie können **Bewertungen mit Rezensionsinhalt** auswählen, um nur Bewertungen mit geschriebenen Rezensionsinhalten anzuzeigen.
-   **Zielbetriebssystem**: Die Standardeinstellung ist **Alle**. Sie können ein bestimmtes Zielbetriebssystem auswählen, wenn auf dieser Seite nur Bewertungen von Kunden angezeigt werden sollen, die Ihre für das spezifische Betriebssystem ausgelegten Pakete verwenden.
-   **Antworten**: Die Standardeinstellung ist **Alle**. Sie können die Rezensionen so filtern, dass nur die Kundenrezensionen angezeigt werden, auf die Sie [geantwortet](respond-to-customer-reviews.md) haben, oder nur die, auf die Sie noch nicht geantwortet haben.
-   **Updates**: Die Standardeinstellung ist **Alle**. Sie können die Rezensionen so filtern, dass nur diejenigen angezeigt werden, die vom Kunden aktualisiert wurden, nachdem Sie auf eine [Rezension geantwortet haben](respond-to-customer-reviews.md), oder nur die, die noch nicht vom Kunden aktualisiert wurden.
-   **Markt**: Die Standardeinstellung ist **Alle Märkte**. Sie können einen bestimmten Markt auswählen, wenn auf dieser Seite nur Rezensionen der in diesem Markt ansässigen Kunden angezeigt werden sollen.
-   **Gerätetyp**: Der Standardfilter ist **Alle Geräte**. Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Rezensionen von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
-   **Paketversion**: Der Standardfilter ist **Alle Pakete**. Sie können ein bestimmtes Paket auswählen, wenn diese Seite nur Rezensionen von Kunden angezeigt werden sollen, die für die Rezension der App dieses Paket verwendet haben.

Die Informationen in allen unten aufgeführten Diagrammen beziehen sich auf den im Bereich **Filter anwenden** ausgewählten Zeitraum und entsprechen den von Ihnen hier ausgewählten Filtern.

> **Hinweis**: Bei der durchschnittlichen Bewertung, die einem Kunden im Store angezeigt wird, werden der Markt und der Gerätetyp des Kunden sowie die Bewertungen aus dem vergangenen Jahr berücksichtigt. Daher können die Inhalte in diesem Bericht davon abweichen. Um zu sehen, wie die durchschnittliche Bewertung für einen bestimmten Kunden im Store angezeigt wird, müssen Sie nach einem bestimmten Markt und Gerätetyp filtern und das **Datum** auf **Letzte 12 Monate** festlegen.

## <a name="translating-reviews"></a>Übersetzen von Rezensionen


Rezensionen, die nicht in Ihrer bevorzugten Sprache verfasst wurden, werden standardmäßig für Sie übersetzt. Falls gewünscht, können Sie die Übersetzung von Rezensionen deaktivieren, indem Sie das Kontrollkästchen **Rezensionen übersetzen** oben rechts über der Liste der Rezensionen deaktivieren.

Da die Rezensionen durch ein automatisches Übersetzungssystem übersetzt werden, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.

## <a name="sorting-reviews"></a>Sortieren von Rezensionen


Sie können die Rezensionen auf der Seite nach Datum und/oder Bewertung in aufsteigender oder absteigender Reihenfolge sortieren. Klicken Sie auf den Link **Sortieren nach**, um die Optionen zum Sortieren nach Datum und/oder Bewertung anzuzeigen. Wenn Sie im Abschnitt für Datum oder Bewertung auf ein Optionsfeld klicken, werden die Sortierkriterien angewendet und die Sortierbezeichnung neben der Überschrift **Sortieren nach** angezeigt. Sie können die Sortierkriterien vollständig entfernen, indem Sie auf das **X** klicken, das für jede Bezeichnung angezeigt wird.

## <a name="responding-to-customer-reviews"></a>Reagieren auf Kundenrezensionen

Mithilfe des Dev Center-Dashboards im Windows Store können Sie Antworten auf zahlreiche Kundenrezensionen senden. Weitere Informationen finden Sie unter [Reagieren auf Kundenrezensionen](respond-to-customer-reviews.md).

Im Folgenden finden Sie einige zusätzliche Aktionen, die Sie basierend auf den angezeigten Bewertungen und Rezensionen in Erwägung ziehen sollten.

-   Erhalten Sie zahlreiche Rezensionen, die eine neue oder geänderte Funktion vorschlagen oder ein Problem reklamieren, sollten Sie erwägen, eine neue Version zu veröffentlichen, in der die im Feedback angesprochenen Probleme behoben sind. (Achten Sie darauf, die [Beschreibung](create-app-descriptions.md) Ihrer App zu aktualisieren, um anzugeben, dass das Problem behoben wurde.)
-   Wenn die durchschnittliche Bewertung zwar hoch ist, die Anzahl der Downloads jedoch gering ausfällt, sollten Sie nach weiteren Methoden suchen, den [Bekanntheitsgrad Ihrer App zu steigern](app-promotion-and-customer-engagement.md), da sie bei Benutzern, die sie getestet haben, gut angekommen ist.


 

 

 

