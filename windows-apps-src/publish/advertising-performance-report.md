---
Description: Zum Anzeigen von Leistungsdaten für die Ad-Einheiten in Ihren apps verwenden Sie den Leistungsbericht für die Werbung im Partner Center.
title: Bericht zur Anzeigenleistung
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a96f6f6593a8ccc6714f67b6f825a6416750b432
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640275"
---
# <a name="advertising-performance-report"></a>Bericht zur Anzeigenleistung


Die **ankündigen Leistungsbericht** in [Partner Center](https://partner.microsoft.com/dashboard) zeigt, wie Ihre [Werbeeinheiten](in-app-ads.md) durchführen, einschließlich der Community anzeigen. Dieser Bericht enthält Daten mehrerer Anzeigenanbieter in UWP-Apps, die die [Anzeigenvermittlung](in-app-ads.md#mediation) verwenden.

Erweitern Sie zum Anzeigen dieses Berichts im linken Navigationsmenü **Analysieren** und wählen Sie dann **Anzeigenleistung** aus. Sie können diese Daten im Partner Center anzeigen oder Herunterladen der Berichtsdaten offline anzeigen, indem Sie auf den Pfeilsymbole auf der Seite. Sie können diese Daten alternativ auch programmgesteuert mit der Methode [Abrufen von Anzeigenleistungsdaten](../monetize/get-ad-performance-data.md) unserer [Analyse-REST-API](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

Wenn Sie die Anzeigenvermittlungsberichte ansehen, ist es wichtig zu beachten, dass sich die Berichtsdaten für die letzten drei Tage sich unter Umständen ändern, wenn wir neue Daten aus verschiedenen Quellen erhalten und verarbeiten. Datenanpassungen können außerdem rückwirkend für bis zu 90 Tage vorgenommen werden.

## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist 30D (30 Tage), aber Sie können Daten für 3, 6 oder 12 Monate anzeigen, oder für einen benutzerdefinierten Zeitraum, den Sie angeben.

Sie können ebenfalls **Filter** erweitern, um alle Daten auf dieser Seite nach Anzeigeneinheit, App, Anzeigenanbieter und Gerätetyp zu filtern. Folgende Optionen stehen zur Auswahl:

* **Aggregation**: Wählen Sie wie die Daten des Berichts aggregiert werden und wie Daten weiter gefiltert werden können. Standardmäßig ist dieser Filter auf **allen Anzeigeeinheiten** festgelegt. Sie können optional dieses Filter auf **alle Apps** oder **alle Anzeigenanbieter** festlegen, oder Sie können das Aggregieren nach einer bestimmten App auswählen, in der Sie Werbung verwenden.
* **AD-Anbietern**: Filtern Sie den Bericht aus, um auf die Daten für bestimmte [Ad Anbieter](in-app-ads.md#paid-networks). In der Standardeinstellung zeigt der Bericht Daten von allen Ad-Anbietern an. Diese Option wird deaktiviert, wenn Sie **Alle Ad-Anbieter** aus der Dropdownliste **Aggregation** auswählen.
* **Gerät**: Filtern Sie den Bericht aus, um auf die Daten für bestimmte Gerätetypen. In der Standardeinstellung zeigt der Bericht Daten für alle Gerätetypen an.

## <a name="overall-performance"></a>Gesamtleistung

In diesem Abschnitt werden Anzeigenleistungsmetriken in n einer Diagramm- oder Weltkartenansicht für Anzeigeeinheiten, Apps und Anzeigenanbieter angezeigt, die Sie im Berichtsfilter ausgewählt haben.

Um zu einer anderen Ansicht der Daten zu wechseln, klicken Sie auf **Diagramm** oder **Karte** am oberen Rand des Abschnitts **Gesamtleistung**. In der Kartenansicht stellen dunklere Schattierungen höhere Werte und hellere Schattierungen niedrigere Werte dar. Sie können mit der Maus auf ein bestimmtes Land oder eine Region auf der Karte zeigen, um den Wert der ausgewählten Metrik zu analysieren. Sie können auch einen beliebigen Bereich der Karte vergrößern, um Daten für kleinere Länder anzuzeigen.

Die im Diagramm oder in der Karte angezeigten Daten können durch Klicken auf das Filtersymbol neben der Dropdownliste **Diagramm** oder **Karte** optimiert werden. Mit diesem Filter können Sie bis zu sechs verschiedene Anzeigeeinheiten, Apps oder Anzeigenanbieter in der Ansicht Diagramm oder Karte vergleichen. Der ausgewählte Datentyp in diesem Filter hängt von Ihrer für Auswahl des Filters **Aggregation** Filter am oberen Rand des Berichts der obersten Ebene ab.


## <a name="overall-performance-breakdown"></a>Gesamtleistungsübersicht

Dieser Abschnitt zeigt eine Tabelle mit allen Anzeigeneistungsmetriken für die angegebenen Daten mithilfe der Filter, die Sie im Bericht ausgewählt haben, an.

## <a name="performance-metrics"></a>Leistungsmetriken

Der Bericht **Anzeigenleistung** enthält Daten für die folgenden Leistungsmetriken. Einige Metriken werden nur für bestimmte Anzeigenanbieter angezeigt.

|  Metrik  |  Beschreibung  |
|----------|---------------|
| Geschätzter Umsatz  |  Die geschätzten Einnahmen, die Sie mit den Anzeigen in Ihrer App erzielen. |
| eCPM  |  Die effektiven Kosten pro tausend Anzeigenaufrufen. |
| Requests  | Die Anzahl der von Ihrer App gesendeten Anzeigenanforderungen.  |
| Aufrufe  | Gibt an, wie häufig eine Anzeige in Ihrer App angezeigt wurde.  |
| Füllrate  | Gibt den Prozentsatz der von Ihrer App gesendeten Anzeigenanforderungen an, bei denen eine Anzeige angezeigt wurde.  |
| Klicks  |  Gibt an, wie häufig in Ihrer App auf eine Anzeige geklickt wurde. |
| CTR  |  Gibt an, wie häufig auf eine Anzeige geklickt wurde – geteilt durch die Anzahl von Anzeigenaufrufen. |
| Viewability | Der Prozentsatz der anzeigenaufrufe, die in Ihrer app angezeigt werden. Weitere Informationen dazu, wie dieser Wert berechnet wird, finden Sie unter [optimieren die Viewability Ihre Ad-Einheiten](../monetize/optimize-ad-unit-viewability.md). |
| Erworbenes Guthaben  | Bei [Community](https://docs.microsoft.com/windows/uwp/publish/about-community-ads)-Kampagnen gibt dies das Guthaben an, das Sie durch Anzeigen von Community-Anzeigen in Ihrer App für Werbefläche erworben haben.  |
| Beanspruchtes Guthaben  | Bei [Community](https://docs.microsoft.com/windows/uwp/publish/about-community-ads)-Kampagnen gibt dies das für Ihre App beanspruchte Anzeigenguthaben an.  |

## <a name="related-topics"></a>Verwandte Themen

* [In-app-Werbung](in-app-ads.md)
* [Anzeigen von Werbung in Ihrer app mit dem Microsoft Advertising SDK](../monetize/display-ads-in-your-app.md)
* [Optimieren der Viewability Ihre Ad-Einheiten](../monetize/optimize-ad-unit-viewability.md)


 
