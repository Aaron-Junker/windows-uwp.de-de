---
Description: Mit dem Verwendungs Bericht in Partner Center können Sie sehen, wie Kunden Ihre APP verwenden.
title: Nutzungsbericht
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, Uwp, Verwendung, benutzerdefiniertes Ereignis, Bericht, Telemetrie, Benutzersitzungen
ms.localizationpriority: medium
ms.openlocfilehash: 45f42b7cf31eb0c22ef68ef9191d773f9044421e
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685112"
---
# <a name="usage-report"></a>Nutzungsbericht


Der **Verwendungs** Bericht in [Partner Center](https://partner.microsoft.com/dashboard) zeigt Ihnen, wie Kunden auf Windows 10 (einschließlich Xbox) Ihre APP verwenden, und zeigt Informationen über benutzerdefinierte Ereignisse an, die Sie definiert haben. Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **30D** (30 Tage), aber Sie können Daten für 3, 6 oder 12 Monate anzeigen, oder für einen benutzerdefinierten Zeitraum, den Sie angeben.

Sie können ebenfalls **Filter** erweitern, um Daten auf dieser Seite nach Paketversion, Markt und/oder Gerätetyp zu filtern.

-   **Paketversion**: Die Standardeinstellung ist **Alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes Paket auswählen.
-   **Markt**: der Standardfilter lautet **Alle Märkte**, aber Sie können die Daten auf einen oder mehrere Märkte begrenzen.
-   **Gerätetyp**: Die Standardeinstellung ist **Alle**, Sie können jedoch festlegen, dass nur Daten für einen bestimmten Gerätetyp (PC, Konsole, Tablet etc.) angezeigt werden.

Die Informationen in allen unten angezeigten Diagrammen spiegelt den Zeitraum und die ausgewählten Filter wider (mit Ausnahme von **Neue Benutzer** im Diagramm **Nutzung**, das nicht angezeigt wird, wenn keine Filter ausgewählt sind). In einigen Abschnitten können Sie zusätzliche Filter anwenden.

> [!IMPORTANT]
> Dieser Bericht enthält nur Nutzungsdaten von Kunden unter Windows 10 (inklusive Xbox), die die Bereitstellung von Telemetriedaten nicht deaktiviert haben. Die Nutzungsdaten für Xbox-Spiele werden unabhängig von der Anmeldung des Kunden bei Xbox Live angezeigt. 


## <a name="usage"></a>Usage

Das Diagramm **Nutzung** zeigt im Detail, wie Kunden Ihre App über den ausgewählten Zeitraum verwenden. In diesem Diagramm werden individuelle Benutzer Ihrer App oder Benutzersitzungen nicht einzeln nachverfolgt (d. h., ein Benutzer wird in diesem Diagramm unabhängig davon dargestellt, ob er Ihre App nur einmal oder mehrmals verwendet hat).

Dieses Diagramm enthält separate Registerkarten, die Sie anzeigen können, wobei die Nutzung nach Tag oder Woche (abhängig von der von Ihnen ausgewählten Dauer) angezeigt wird.

- **Benutzer**: Zeigt die Gesamtanzahl der **Benutzersitzungen** über den ausgewählten Zeitraum an. Jede Benutzersitzung stellt einen unterschiedlichen Zeitraum dar und beginnt, wenn die App gestartet wird (Prozessbeginn) und endet bei Prozessende oder nach einer bestimmten Zeit der Inaktivität. Aus diesem Grund kann ein einzelner Kunde mehrere Benutzersitzungen am gleichen Tag oder in der gleichen Woche haben. Die Gesamtanzahl der **aktiven Benutzer** (alle Kunden, die die App an diesem Tag oder in dieser Woche nutzen) und **neuen Benutzer** (ein Kunde, der Ihre App das erste Mal an diesem Tag oder in der Woche nutzt) wird ebenfalls angezeigt. Wenn Sie Filter auf der Seite angewendet haben, werden **neue Benutzer** in diesem Diagramm nicht angezeigt.
- **Geräte**: Zeigt die Anzahl der täglichen Geräte an, die von allen Benutzern zur Interaktion mit Ihrer App verwendet werden.
- **Dauer**: Zeigt die Gesamtanzahl der aktiven Stunden an (Stunden, in denen ein Benutzer aktiv Ihrer App verwendet).
- **Engagement**: zeigt die durchschnittlichen Engagement-Minuten pro Benutzer an (durchschnittliche Dauer aller Benutzersitzungen). 
- **Beibehaltung**: Zeigt die Gesamtanzahl der **DAU/MAU** (tägliche aktive Benutzer/monatliche aktive Benutzer) über den ausgewählten Zeitraum an.
- **Vorhersage**der Abwanderung: zeigt an, wie viele Benutzer Ihre APP wahrscheinlich bald nicht mehr verwenden, basierend auf Ihrer aktuellen Nutzung.

Wenn der Zeitraum von **30D** ausgewählt ist, werden möglicherweise Kreis Markierungen angezeigt, wenn die Registerkarten **Benutzer**, **Geräte**oder **Dauer** angezeigt werden. Diese stellen eine beträchtliche Zunahme oder Abnahme in einem bestimmten Wert dar, über den wir denken, dass Sie wissen möchten. Das Datum, an dem der Kreis angezeigt wird, steht für das Ende der Woche, an dem im Vergleich zur Woche vor der Woche eine beträchtliche Zunahme oder Abnahme festgestellt wurde. Zeigen Sie auf den Kreis, um weitere Details zu den Änderungen anzuzeigen.  

> [!TIP]
> Sie können in den letzten 30 Tagen im [Insights-Bericht](insights-report.md)weitere Einblicke in Bezug auf bedeutende Änderungen anzeigen.


## <a name="user-sessions"></a>Benutzersitzungen

Das Diagramm **Benutzersitzungen** zeigt die Gesamtzahl der Benutzersitzungen pro Markt für die App über den ausgewählten Zeitraum an.

Genau wie in den Infos für die **Benutzersitzungen** stellen die Infos im Diagramm **Nutzung** eine Sitzung des Benutzers für einen bestimmten Zeitraum dar, wenn der Kunde mit der App interagiert. Dieses Diagramm verfolgt keine individuellen Benutzer Ihrer App.

Sie können diese Daten in einer visuellen **Karte** anzeigen, oder die Einstellung auf eine Anzeige in Form einer **Tabelle** festlegen. Die Tabellenform zeigt jeweils fünf Märkten an, die alphabetisch oder nach der höchsten/niedrigst möglichen Anzahl von Anwendersitzungen sortiert sind. Sie können auch die Daten zum Anzeigen von Informationen für alle Märkte zusammen herunterladen.


## <a name="package-version"></a>Paketversion

Das Diagramm **Benutzersitzungen** zeigt die Gesamtzahl täglicher Benutzersitzungen pro Paketversion für die App über den ausgewählten Zeitraum an.

Genau wie im Diagramm **Benutzersitzungen** stellen die Infos eine Sitzung des Benutzers für einen bestimmten Zeitraum dar, wenn der Kunde mit der App interagiert. Dieses Diagramm verfolgt keine individuellen Benutzer Ihrer App.


## <a name="custom-events"></a>Benutzerdefinierte Ereignisse

Das Diagramm **Benutzerdefinierte Ereignisse** zeigt, wie häufig die für Ihre App definierten benutzerdefinierten Ereignisse insgesamt aufgetreten sind. Dazu können mehrere Vorkommen für denselben Kunden gehören. Mithilfe der Filter können Sie die benutzerdefinierten Ereignisse auswählen, für die Daten angezeigt werden sollen.

Benutzerdefinierte Ereignisse werden unter Verwendung der [StoreServicesCustomEventLogger.Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Methode im [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md) implementiert.

Weitere Informationen finden Sie unter [Protokollieren benutzerdefinierter Ereignisse für Dev Center](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Übersicht über benutzerdefinierte Ereignisse

Das Diagramm **Übersicht über benutzerdefinierte Ereignisse** enthält weitere Details, wie oft jedes Ihre benutzerdefinierten Ereignisse aufgetreten ist. Dadurch können Sie ermitteln, ob Ereignisse für einen bestimmten Markt, Gerätetyp oder eine Paketversionen häufiger auftreten.

Für jedes Ereignis wird der Name des Ereignisses und ein Ereignisanzahl angezeigt, die einer bestimmte Kombination von Markt, Gerätetyp und Paketversion des Benutzers entsprechen. In der Regel wird ein Ereignis mehrmals angezeigt, zusammen mit verschiedenen Kombinationen dieser Faktoren. 




 
