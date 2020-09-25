---
Description: Mit dem Verwendungs Bericht in Partner Center können Sie sehen, wie Kunden Ihre APP verwenden.
title: Nutzungsbericht
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Verwendung, benutzerdefiniertes Ereignis, Bericht, Telemetrie, Benutzersitzungen
ms.localizationpriority: medium
ms.openlocfilehash: a794018b2fcdd07017ee3441e65c6200e7436304
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220203"
---
# <a name="usage-report"></a>Nutzungsbericht


Der **Verwendungs** Bericht in [Partner Center](https://partner.microsoft.com/dashboard) zeigt Ihnen, wie Kunden auf Windows 10 (einschließlich Xbox) Ihre APP verwenden, und zeigt Informationen über benutzerdefinierte Ereignisse an, die Sie definiert haben. Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, in dem Sie Daten anzeigen möchten. Die Standardauswahl beträgt **30D** (30 Tage), Sie können jedoch auswählen, dass Daten für 3, 6 oder 12 Monate oder für einen von Ihnen angegebenen benutzerdefinierten Datenbereich angezeigt werden.

Sie können auch **Filter** erweitern, um die Daten auf dieser Seite nach Paketversion, Markt und/oder Gerätetyp zu filtern.

-   **Paketversion**: Die Standardeinstellung ist **Alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes Paket auswählen.
-   **Market**: der Standardfilter ist **Alle Märkte**, aber Sie können die Daten auf einen oder mehrere Märkte beschränken.
-   **Gerätetyp**: die Standardeinstellung ist " **alle**", aber Sie können auswählen, dass nur Daten für einen bestimmten Gerätetyp (PC, Konsole, Tablet usw.) angezeigt werden.

Die Informationen in allen unten aufgeführten Diagrammen spiegeln den Datumsbereich und alle ausgewählten Filter wider (mit Ausnahme von **neuen Benutzern** im **Verwendungs** Diagramm, die nicht angezeigt werden, wenn Filter ausgewählt werden). In einigen Abschnitten können Sie auch zusätzliche Filter anwenden.

> [!IMPORTANT]
> Dieser Bericht enthält nur Nutzungsdaten von Kunden unter Windows 10 (einschließlich Xbox), die keine telemetrieinformationen bereitgestellt haben. Die Nutzungsdaten für Xbox Games sind hier enthalten, unabhängig davon, ob der Kunde bei Xbox Live angemeldet war. 


## <a name="usage"></a>Verwendung

Das **Verwendungs** Diagramm zeigt Details dazu, wie Ihre Kunden Ihre APP über den ausgewählten Zeitraum verwenden. Beachten Sie, dass in diesem Diagramm keine eindeutigen Benutzer für Ihre APP oder eindeutige Benutzersitzungen nachverfolgt werden (d. h. ein Benutzer wird in diesem Diagramm dargestellt, unabhängig davon, ob Sie die app nur einmal oder mehrmals verwendet haben).

Dieses Diagramm enthält separate Registerkarten, die Sie anzeigen können, wobei die Nutzung nach Tag oder Woche (abhängig von der von Ihnen ausgewählten Dauer) angezeigt wird.

- **Benutzer**: zeigt die Gesamtanzahl der **Benutzersitzungen** im ausgewählten Zeitraum an. Jede Benutzersitzung stellt einen bestimmten Zeitraum dar, beginnend beim Start der APP (Prozessstart) und beim Beenden (Prozess Ende) oder nach einem Zeitraum der Inaktivität. Aus diesem Grund kann ein einzelner Kunde über denselben Tag oder eine Woche über mehrere Benutzersitzungen verfügen. Die Gesamtanzahl **aktiver Benutzer** (alle Kunden, die die APP für den Tag oder die Woche verwenden) und **neue Benutzer** (ein Kunde, der Ihre APP zum ersten Mal diesen Tag oder jede Woche verwendet) wird angezeigt. Beachten Sie, dass in diesem Diagramm keine **neuen Benutzer** angezeigt werden, wenn Sie auf der Seite Filter angewendet haben.
- **Geräte**: zeigt die Anzahl der täglichen Geräte an, die von allen Benutzern für die Interaktion mit Ihrer APP verwendet werden.
- **Dauer**: zeigt die Gesamtzahl der Engagement-Stunden (Stunden, in denen ein Benutzer Ihre APP aktiv verwendet) an.
- **Engagement**: zeigt die durchschnittlichen Engagement-Minuten pro Benutzer an (durchschnittliche Dauer aller Benutzersitzungen). 
- **Aufbewahrung**: zeigt die Gesamtzahl von **Dau/Mau** (tägliche aktive Benutzer/monatliche aktive Benutzer) im ausgewählten Zeitraum an.

Wenn der Zeitraum von **30D** ausgewählt ist, werden möglicherweise Kreis Markierungen angezeigt, wenn die Registerkarten **Benutzer**, **Geräte**oder **Dauer** angezeigt werden. Diese stellen eine beträchtliche Zunahme oder Abnahme in einem bestimmten Wert dar, über den wir denken, dass Sie wissen möchten. Das Datum, an dem der Kreis angezeigt wird, steht für das Ende der Woche, an dem im Vergleich zur Woche vor der Woche eine beträchtliche Zunahme oder Abnahme festgestellt wurde. Zeigen Sie auf den Kreis, um weitere Details zu den Änderungen anzuzeigen.  

> [!TIP]
> Sie können in den letzten 30 Tagen im [Insights-Bericht](insights-report.md)weitere Einblicke in Bezug auf bedeutende Änderungen anzeigen.


## <a name="user-sessions"></a>Benutzersitzungen

Im Diagramm **Benutzersitzungen** wird die Gesamtzahl der Benutzersitzungen für Ihre APP pro Markt über den ausgewählten Zeitraum angezeigt.

Wie bei den **Benutzer** Sitzungsinformationen im **Verwendungs** Diagramm stellt eine Benutzersitzung einen unterschiedlichen Zeitraum dar, in dem ein Kunde mit Ihrer APP interagiert hat, und in diesem Diagramm werden keine eindeutigen Benutzer für Ihre APP nachverfolgt.

Sie können diese Daten in einem visuellen **Karten** Formular anzeigen oder die Einstellung umschalten, um Sie in **Tabellen** Form anzuzeigen. Das Tabellen Formular zeigt fünf Märkte gleichzeitig an, entweder alphabetisch oder mit der höchsten/niedrigsten Anzahl von Benutzersitzungen. Sie können die Daten auch herunterladen, um Informationen für alle Märkte anzuzeigen.


## <a name="package-version"></a>Paketversion

Im Diagramm **Benutzersitzungen** wird die Gesamtzahl der täglichen Benutzersitzungen für Ihre APP pro Paketversion im ausgewählten Zeitraum angezeigt.

Wie beim **Benutzer** Sitzungs Diagramm stellt eine Benutzersitzung einen eindeutigen Zeitraum dar, in dem ein Kunde mit Ihrer APP interagiert, und dieses Diagramm verfolgt keine eindeutigen Benutzer für Ihre APP.


## <a name="custom-events"></a>Benutzerdefinierte Ereignisse

Das Diagramm für **benutzerdefinierte Ereignisse** zeigt die Gesamtanzahl der Vorkommen für benutzerdefinierte Ereignisse an, die Sie für Ihre APP definiert haben. Dazu können mehrere Vorkommen für denselben Kunden gehören. Mit den Filtern können Sie die spezifischen benutzerdefinierten Ereignisse auswählen, für die Sie diese Daten anzeigen möchten.

Benutzerdefinierte Ereignisse werden unter Verwendung der [StoreServicesCustomEventLogger.Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Methode im [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md) implementiert.

Weitere Informationen finden Sie unter [Protokollieren von benutzerdefinierten Ereignissen für dev Center](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Aufschlüsselung von benutzerdefinierten Ereignissen

Das Diagramm für **benutzerdefinierte Ereignis Aufgliederung** zeigt weitere Details dazu, wie oft jedes benutzerdefinierte Ereignis aufgetreten ist. Dies kann Ihnen dabei helfen, zu bestimmen, ob Ereignisse für einen bestimmten Markt, Gerätetyp oder eine Paketversion häufiger auftreten.

Für jedes Ereignis werden der Ereignis Name und eine Ereignis Anzahl angezeigt, die einer bestimmten Kombination aus Markt, Gerätetyp und Paketversion des Benutzers entsprechen. In der Regel sehen Sie, dass ein Ereignis mehrmals aufgeführt wird und verschiedene Kombinationen dieser Faktoren vorhanden sind. 




 
