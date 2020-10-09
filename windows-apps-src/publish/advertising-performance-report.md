---
Description: Zum Anzeigen von Leistungsdaten für die Ad-Einheiten in ihren Apps verwenden Sie den Leistungsbericht "Werbung" im Partner Center.
title: Bericht zur Anzeigenleistung
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ea0fe6f29059a2b9ef8e5ce728d883d5349d0468
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878423"
---
# <a name="advertising-performance-report"></a>Bericht zur Anzeigenleistung


Der **Leistungsbericht "Werbung** " im [Partner Center](https://partner.microsoft.com/dashboard) zeigt, wie Ihre [Ad-Einheiten](in-app-ads.md) , einschließlich der communitywerbung, funktioniert. Dieser Bericht enthält Daten von mehreren Ad-Anbietern in UWP-apps, die [AD-Vermittlung](in-app-ads.md#mediation)verwenden.

Um diesen Bericht anzuzeigen, erweitern Sie im linken Navigationsmenü die Option **analysieren** , und wählen Sie dann **Ad Performance**aus. Sie können diese Daten im Partner Center anzeigen oder die Berichtsdaten herunterladen, um Sie offline anzuzeigen, indem Sie auf die Pfeilsymbole auf der Seite klicken. Alternativ können Sie diese Daten Programm gesteuert abrufen, indem Sie die Methode zum Abrufen der [Leistungsdaten](../monetize/get-ad-performance-data.md) in unserer [Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)verwenden.

Beachten Sie beim Anzeigen der Leistungsberichte der Werbung, dass sich die Berichtsdaten der letzten drei Tage ändern können, wenn wir neue Daten aus verschiedenen Quellen empfangen und verarbeiten. Darüber hinaus können Daten Umrechnungen bis zu 90 Tage in der Vergangenheit vorkommen.

## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, in dem Sie Daten anzeigen möchten. Die Standardauswahl beträgt 30D (30 Tage), Sie können jedoch auswählen, dass Daten für 3, 6 oder 12 Monate oder für einen von Ihnen angegebenen benutzerdefinierten Datenbereich angezeigt werden.

Sie können auch **Filter** erweitern, um alle Daten auf dieser Seite nach Ad-Einheit, APP, AD-Anbieter und Gerätetyp zu filtern. Sie können eine der folgenden Optionen auswählen:

* **Aggregation**: Wählen Sie aus, wie die Berichtsdaten aggregiert werden, und wie Sie möglicherweise weiter gefiltert werden. Standardmäßig ist dieser Filter auf **Alle Ad-Einheiten**festgelegt. Optional können Sie diesen Filter in **alle apps** oder **alle AD-Anbieter**ändern, oder Sie können auswählen, ob Sie mit einer bestimmten app aggregieren möchten, in der Sie ADS verwenden.
* **AD-Anbieter**: Filtern Sie den Bericht nach Leistungsdaten für bestimmte [AD-Anbieter](in-app-ads.md#paid-networks). Standardmäßig zeigt der Bericht Daten von allen AD-Anbietern an. Diese Option wird deaktiviert, wenn Sie **alle AD-Anbieter** in der **Aggregations** -Dropdown-Option ausgewählt haben.
* **Gerät**: Filtern Sie den Bericht nach Leistungsdaten für bestimmte Gerätetypen. Standardmäßig zeigt der Bericht Daten für alle Gerätetypen an.

## <a name="overall-performance"></a>Gesamtleistung

In diesem Abschnitt werden AD-Leistungsmetriken in einer Diagramm-oder World Map-Ansicht für die Ad-Einheiten,-Apps und AD-Anbieter angezeigt, die Sie in den Berichts Filtern ausgewählt haben.

Um zu einer anderen Ansicht der Daten zu wechseln, klicken Sie oben im Abschnitt **allgemeine Leistung** auf **Diagramm** oder **Karte** . In der Kartenansicht stellen dunklere Schattierungen höhere Werte und hellere Schattierungen niedrigere Werte dar. Sie können mit der Maus auf ein bestimmtes Land oder eine Region auf der Karte zeigen, um den Wert der ausgewählten Metrik zu analysieren. Sie können auch einen beliebigen Bereich der Karte vergrößern, um Daten für kleinere Länder anzuzeigen.

Sie können die im Diagramm oder der Karte angezeigten Daten verfeinern, indem Sie auf das Filter Symbol neben der Dropdown Liste **Diagramm** oder **Karte** klicken. Mit diesem Filter können Sie zwischen bis zu sechs verschiedenen Ad-Einheiten, Apps oder AD-Anbietern wählen, die im Diagramm oder in der Kartenansicht verglichen werden sollen. Der Typ der Daten, aus denen Sie in diesem Filter auswählen können, hängt davon ab, was Sie für den **Aggregations** Filter der obersten Ebene am oberen Rand des Berichts ausgewählt haben.


## <a name="overall-performance-breakdown"></a>Aufschlüsselung der Gesamtleistung

In diesem Abschnitt wird eine Tabelle mit allen AD-Leistungsmetriken für das Dataset angezeigt, das in den Filtern angegeben ist, die Sie im Bericht ausgewählt haben.

## <a name="performance-metrics"></a>Leistungsmetriken

Der **Leistungs** Bericht "Werbung" enthält Daten für die folgenden Leistungsmetriken. Einige Metriken werden nur für bestimmte AD-Anbieter angezeigt.

|  Metrik  |  BESCHREIBUNG  |
|----------|---------------|
| Geschätzter Umsatz  |  Der geschätzte Betrag des Geldes, das Sie von den in Ihrer APP ausgelaufenden anzeigen erhalten haben. |
| eCPM  |  Effektive Kosten pro Tausendstel. |
| Requests  | Gibt an, wie oft eine Werbe Anforderung von Ihrer APP gesendet wurde.  |
| Aufrufe  | Gibt an, wie oft ein Ad in der App angezeigt wurde.  |
| Füllrate  | Der Prozentsatz der AD-Anforderungen, die von Ihrer APP gesendet wurden, in der eine Werbung angezeigt wurde.  |
| Clicks  |  Gibt an, wie oft jemand in der APP auf eine Werbeanzeige geklickt hat. |
| CTR  |  Die Click-through-Rate, d. h. die Häufigkeit, mit der auf eine Werbung geklickt wurde, dividiert durch die Anzahl von Eindrücken. |
| Sichtbarkeit | Der Prozentsatz der werbeeindrücke, die in ihrer App angezeigt werden können. Weitere Informationen zur Berechnung dieses Werts finden [Sie unter Optimieren der viewability ihrer Ad-Einheiten](../monetize/optimize-ad-unit-viewability.md). |
| Guthaben verdient  | Wenn Sie eine Community- [Werbe](../monetize/index.md) Kampagne ausführen, gibt dies die Anzahl der Gutschriften an, die Sie für Werbe Speicher Angebote erworben haben, indem Sie communitywerbung in ihrer App anzeigen.  |
| Guthaben aufgewendet  | Wenn Sie eine Community- [Werbe](../monetize/index.md) Kampagne ausführen, gibt dies die Anzahl der Gutschriften an, die Sie für Werbung für Ihre APP aufgewendet haben.  |

## <a name="related-topics"></a>Zugehörige Themen

* [In-App-Anzeigen](in-app-ads.md)
* [Anzeigen von Werbung mithilfe des Microsoft Advertising-SDK in der App](../monetize/display-ads-in-your-app.md)
* [Optimieren der Sichtbarkeit von Anzeigeneinheiten](../monetize/optimize-ad-unit-viewability.md)


 