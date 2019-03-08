---
Description: Der Bericht zu den im Partner Center können Sie sehen, wie Kunden Ihre app verwenden.
title: Nutzungsbericht
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, Uwp, Verwendung, benutzerdefiniertes Ereignis, Bericht, Telemetrie, Benutzersitzungen
ms.localizationpriority: medium
ms.openlocfilehash: 0d0be1399ebc00ffda57ecf27a72be994fa994ce
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610905"
---
# <a name="usage-report"></a>Nutzungsbericht


Die **Nutzung** Bericht im [Partner Center](https://partner.microsoft.com/dashboard) können Sie sehen, wie Kunden unter Windows 10 (einschließlich Xbox) Ihrer app verwenden, und zeigt Informationen zu benutzerdefinierten Ereignissen, die Sie definiert haben. Sie können diese Daten anzeigen, im Partner Center oder [Bericht herunterladen](download-analytic-reports.md) offline anzeigen.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **30D** (30 Tage), aber Sie können Daten für 3, 6 oder 12 Monate anzeigen, oder für einen benutzerdefinierten Zeitraum, den Sie angeben.

Sie können ebenfalls **Filter** erweitern, um Daten auf dieser Seite nach Paketversion, Markt und/oder Gerätetyp zu filtern.

-   **Paketversion**: Die Standardeinstellung ist **alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes Paket auswählen.
-   **Markt**: Der Standardfilter ist **alle Märkte**, aber Sie können die Daten auf eine oder mehrere einschränken.
-   **Gerätetyp**: Die Standardeinstellung ist **alle**, aber Sie können auch Daten für nur einen bestimmten Gerätetyp (PC, Konsole, Tablet, usw.) angezeigt.

Die Informationen in allen unten angezeigten Diagrammen spiegelt den Zeitraum und die ausgewählten Filter wider (mit Ausnahme von **Neue Benutzer** im Diagramm **Nutzung**, das nicht angezeigt wird, wenn keine Filter ausgewählt sind). In einigen Abschnitten können Sie zusätzliche Filter anwenden.

> [!IMPORTANT]
> Dieser Bericht enthält nur Nutzungsdaten von Kunden unter Windows 10 (inklusive Xbox), die die Bereitstellung von Telemetriedaten nicht deaktiviert haben. Die Nutzungsdaten für Xbox-Spiele werden unabhängig von der Anmeldung des Kunden bei Xbox Live angezeigt. 


## <a name="usage"></a>Verwendungszweck

Das Diagramm **Nutzung** zeigt im Detail, wie Kunden Ihre App über den ausgewählten Zeitraum verwenden. In diesem Diagramm werden individuelle Benutzer Ihrer App oder Benutzersitzungen nicht einzeln nachverfolgt (d. h., ein Benutzer wird in diesem Diagramm unabhängig davon dargestellt, ob er Ihre App nur einmal oder mehrmals verwendet hat).

In diesem Diagramm verfügt über separate Registerkarten, die Sie anzeigen können, mit der Nutzung von Tag oder Woche (je nach der Dauer, die Sie ausgewählt haben).

- **Benutzer**: Zeigt die Gesamtanzahl der **benutzersitzungen** über den ausgewählten Zeitraum an. Jede Benutzersitzung stellt einen unterschiedlichen Zeitraum dar und beginnt, wenn die App gestartet wird (Prozessbeginn) und endet bei Prozessende oder nach einer bestimmten Zeit der Inaktivität. Aus diesem Grund kann ein einzelner Kunde mehrere Benutzersitzungen am gleichen Tag oder in der gleichen Woche haben. Die Gesamtanzahl der **aktiven Benutzer** (alle Kunden, die die App an diesem Tag oder in dieser Woche nutzen) und **neuen Benutzer** (ein Kunde, der Ihre App das erste Mal an diesem Tag oder in der Woche nutzt) wird ebenfalls angezeigt. Wenn Sie Filter auf der Seite angewendet haben, werden **neue Benutzer** in diesem Diagramm nicht angezeigt.
- **Geräte**: Zeigt die Anzahl der täglichen Geräte, die von allen Benutzern Interaktion mit Ihrer app verwendet.
- **Dauer**: Zeigt die gesamten Engagement-Stunden (Stunden, in denen ein Benutzer Ihre app aktiv verwendet wird).
- **Engagement**: Zeigt die durchschnittliche Engagement Minuten pro Benutzer (durchschnittliche Dauer der Sitzung des Benutzers). 
- **Aufbewahrung**: Zeigt die Gesamtanzahl der **täglich aktive Benutzer/MAU** (aktive Benutzer/monatlich aktive Benutzer/Tag) über den ausgewählten Zeitraum an.
- **Churn Prediction**: Zeigt, wie viele Benutzer, die wir Vorhersagen wahrscheinlich beenden werden, anhand Ihrer app in Kürze mit ihre letzte Nutzung.

Wenn die **30D** Zeitraum ausgewählt ist, wird möglicherweise beim Anzeigen von Kreis Marker angezeigt der **Benutzer**, **Geräte**, oder **Dauer** Registerkarten. Dies eine erhebliche Leistungssteigerung darstellen, oder verringern Sie in einem angegebenen Wert, den wir glauben, dass Sie wissen möchten. Das Datum, die auf dem der Kreis angezeigt wird, stellt das Ende der Woche, die in der wir eine beträchtliche Zunahme oder verringern, die im Vergleich zu die Woche vor, die erkannt dar. Zeigen Sie auf den Kreis, um weitere Details zu Änderungen anzuzeigen.  

> [!TIP]
> Sehen Sie weitere Informationen zu wesentlichen Änderungen in den letzten 30 Tagen in den [Insights-Bericht](insights-report.md).


## <a name="user-sessions"></a>Benutzersitzungen

Das Diagramm **Benutzersitzungen** zeigt die Gesamtzahl der Benutzersitzungen pro Markt für die App über den ausgewählten Zeitraum an.

Genau wie in den Infos für die **Benutzersitzungen** stellen die Infos im Diagramm **Nutzung** eine Sitzung des Benutzers für einen bestimmten Zeitraum dar, wenn der Kunde mit der App interagiert. Dieses Diagramm verfolgt keine individuellen Benutzer Ihrer App.

Sie können diese Daten in einer visuellen **Karte** anzeigen, oder die Einstellung auf eine Anzeige in Form einer **Tabelle** festlegen. Die Tabellenform zeigt jeweils fünf Märkten an, die alphabetisch oder nach der höchsten/niedrigst möglichen Anzahl von Anwendersitzungen sortiert sind. Sie können auch die Daten zum Anzeigen von Informationen für alle Märkte zusammen herunterladen.


## <a name="package-version"></a>Paketversion

Das Diagramm **Benutzersitzungen** zeigt die Gesamtzahl täglicher Benutzersitzungen pro Paketversion für die App über den ausgewählten Zeitraum an.

Genau wie im Diagramm **Benutzersitzungen** stellen die Infos eine Sitzung des Benutzers für einen bestimmten Zeitraum dar, wenn der Kunde mit der App interagiert. Dieses Diagramm verfolgt keine individuellen Benutzer Ihrer App.


## <a name="custom-events"></a>Benutzerdefinierte Ereignisse

Das Diagramm **Benutzerdefinierte Ereignisse** zeigt, wie häufig die für Ihre App definierten benutzerdefinierten Ereignisse insgesamt aufgetreten sind. Dazu können mehrere Vorkommen für denselben Kunden gehören. Mithilfe der Filter können Sie die benutzerdefinierten Ereignisse auswählen, für die Daten angezeigt werden sollen.

Benutzerdefinierte Ereignisse werden unter Verwendung der [StoreServicesCustomEventLogger.Log](https://docs.microsoft.com/en-us/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Methode im [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md) implementiert.

Weitere Informationen finden Sie unter [Protokollieren benutzerdefinierter Ereignisse für Dev Center](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Übersicht über benutzerdefinierte Ereignisse

Das Diagramm **Übersicht über benutzerdefinierte Ereignisse** enthält weitere Details, wie oft jedes Ihre benutzerdefinierten Ereignisse aufgetreten ist. Dadurch können Sie ermitteln, ob Ereignisse für einen bestimmten Markt, Gerätetyp oder eine Paketversionen häufiger auftreten.

Für jedes Ereignis wird der Name des Ereignisses und ein Ereignisanzahl angezeigt, die einer bestimmte Kombination von Markt, Gerätetyp und Paketversion des Benutzers entsprechen. In der Regel wird ein Ereignis mehrmals angezeigt, zusammen mit verschiedenen Kombinationen dieser Faktoren. 




 
