---
Description: Der Integritäts Bericht in Partner Center ermöglicht Ihnen das Abfangen von Daten in Bezug auf die Leistung und Qualität Ihrer APP, einschließlich abstürzen und nicht reagierender Ereignisse.
title: Integritätsbericht
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Integrität, Abstürze, reagiert nicht, App-Integrität, Integritätsdaten, Stapelüberwachung, CAB-Datei, Fehler, Fehler, Pdb, Symbole
ms.localizationpriority: medium
ms.openlocfilehash: 9b6795673959510d0e4f5452a68ffced6c43ced1
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682504"
---
# <a name="health-report"></a>Integritätsbericht

Der **Integritäts Bericht in** [Partner Center](https://partner.microsoft.com/dashboard) ermöglicht Ihnen das Abfangen von Daten in Bezug auf die Leistung und Qualität Ihrer APP, einschließlich abstürzen und nicht reagierender Ereignisse. Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen. Bei Bedarf können Sie Stapelüberwachungen und/oder CAB-Dateien für weitere Debugzwecke anzeigen.

Sie können die Daten in diesem Bericht aber auch programmgesteuert mit der [Microsoft Store-REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **72 H** (72 Stunden), Sie können stattdessen aber auch **30D** auswählen, um die Daten der letzten 30 Tage anzuzeigen. Beachten Sie, dass die Daten in der lokalen Zeitzone für die **72H** -Ansicht und in UTC für die **30D-** Ansicht angezeigt werden.

Sie können ebenfalls **Filter** erweitern, um alle Daten auf dieser Seite nach Paketversion, Markt und/oder Gerätetyp zu filtern.

-   **Paketversion**: Die Standardeinstellung ist **alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes Paket auswählen.
-   **Markt**: Der Standardfilter ist **Alle Märkte**, aber Sie können die Daten auf einen oder mehrere Märkte beschränken.
-   **Gerätetyp**: Die Standardeinstellung ist " **alle**", aber Sie können auswählen, dass nur Daten für einen bestimmten Gerätetyp angezeigt werden. Beachten Sie, dass die Kategorie **Andere** Geräte enthält, für die der Hersteller/ das Modell erkannt wird, wir diese allerdings nicht in eine der vordefinierten Kategorien einbeziehen, die in diesem Filter angezeigt werden. Für diese Geräte kann das Modells im Abschnitt **Fehlerprotokoll** des Berichts **Fehlerdetails** angezeigt werden.  
-   **Betriebssystemversion**: Der Standardwert ist **Alle Betriebssystemversionen**, Sie können jedoch eine bestimmte Betriebssystemversion auswählen.
-   **Betriebssystem-Releaseversion**: Der Standardwert ist **Alle Betriebssystemversionen**, Sie können jedoch eine bestimmte Releaseversion der ausgewählten **Betriebssystemversion**auswählen.
-   **Sandbox**: Der Standardwert ist " **Retail**", aber bei Produkten, die mehrere Entwicklungs Sandboxes verwenden (z. b. Spiele, die in Xbox Live integriert sind), können Sie hier eine bestimmte auswählen. (Wenn Ihr Produkt keine Sandboxes verwendet, zeigt dieser Filter nur **Einzelhandel** an und ist nicht anwendbar.)
-   **Architektur**: Der Standardwert ist **alle Architekturen**, Sie können jedoch einen bestimmten Systemtyp auswählen. Dieser Filter ist nur verfügbar, wenn **30D** ausgewählt ist.
-   " **PRAID**": Die Standardeinstellung ist **all**, aber wenn Sie beim Erstellen des App-Pakets mehrere Paket relative App-IDs (app-IDs) definiert haben, können Sie auswählen, dass nur Daten angezeigt werden sollen, die mit einer einzelnen App verknüpft sind. Dieser Filter wird nicht angezeigt, wenn Sie nicht mehrere PRAIDs definiert haben.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den ausgewählten Zeitraum und alle von Ihnen ausgewählten Filter. In einigen Abschnitten können Sie zusätzliche Filter anwenden.


## <a name="failure-hits"></a>Fehlertreffer

Das Diagramm **Fehlertreffer** zeigt die Anzahl von täglichen Abstürzen und Ereignissen, die Kunden bei der Nutzung Ihrer App im ausgewählten Zeitraum festgestellt haben. Jeder Ereignistyp, der in der App aufgetreten ist, wird separat überwacht: Abstürze, Blockaden, JavaScript-Ausnahmen und Speicherfehler.

Wenn der Zeitraum von **30D** ausgewählt ist, werden möglicherweise Kreis Markierungen angezeigt. Diese stellen eine beträchtliche Zunahme oder Abnahme in einem bestimmten Wert dar, über den wir denken, dass Sie wissen möchten. Das Datum, an dem der Kreis angezeigt wird, steht für das Ende der Woche, an dem im Vergleich zur Woche vor der Woche eine beträchtliche Zunahme oder Abnahme festgestellt wurde. Zeigen Sie auf den Kreis, um weitere Details zu den Änderungen anzuzeigen.  

> [!TIP]
> Sie können in den letzten 30 Tagen im [Insights-Bericht](insights-report.md)weitere Einblicke in Bezug auf bedeutende Änderungen anzeigen.

## <a name="failure-hits-by-market"></a>Fehlertreffer nach Markt

Das Diagramm **Fehlertreffer nach Markt** zeigt die Gesamtanzahl von Abstürzen und Ereignissen im ausgewählten Zeitraum nach Markt an.

Sie können diese Daten in einer visuellen **Karte** anzeigen, oder die Einstellung auf eine Anzeige in Form einer **Tabelle** festlegen. Die Tabellenform zeigt jeweils fünf Märkten an, die alphabetisch oder nach der höchsten/niedrigst möglichen Anzahl von Anwendersitzungen sortiert sind. Sie können auch die Daten zum Anzeigen von Informationen für alle Märkte zusammen herunterladen.


## <a name="package-version"></a>Paketversion

Das Diagramm **Paketversion** zeigt die Gesamtanzahl von Abstürzen und Ereignissen im ausgewählten Zeitraum nach Paketversion an. In der Standardeinstellung wird die Paketversion mit den meisten Treffern an oberster Stelle vor den Märkten mit weniger Treffern angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Treffer** des Diagramms klicken.

## <a name="failures"></a>Fehler

Das Diagramm **Fehler** zeigt die Gesamtanzahl von Abstürzen und Ereignissen im ausgewählten Zeitraum nach Fehlername an. Jeder Fehlername besteht aus vier Teilen: eine oder mehrere Problemklassen, Ausnahme/Fehlerprüfcode, der Name des Image/Treibers, in dem der Fehler aufgetreten ist, und der zugehörige Funktionsname. In der Standardeinstellung wird der Fehler mit den meisten Treffern an oberster Stelle vor den Fehlern mit weniger Treffern angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Treffer** des Diagramms klicken. Außerdem wird für die einzelnen Fehler der Prozentsatz der Gesamtanzahl von Fehlern angezeigt.

> [!TIP]
> Von Zeit zu Zeit sehen Sie in diesem Abschnitt einen Eintrag **Unbekannt**. Dies passiert, wenn wir trotz aller Bemühungen, keine vollständigen Detailinformationen für einen oder mehrere Fehler erfassen können. Diese werden alle zusammen unter **Unbekannt** gruppiert. In den meisten Fällen tritt dies aufgrund von Speichereinschränkungen auf. Dies kann jedoch auch aufgrund von Datenschutzeinstellungen des Geräts, Netzwerkverbindungsproblemen, teilweise/beschädigten Absturzabbildern und anderen Faktoren auftreten.
>
> Wenn **!unbekannt** im Rahmen seines Fehlernamens angezeigt wird, bedeutet dies, dass keine Symbole vorhanden waren und wir den Fehlernamen daher nicht identifizieren konnten. Stellen Sie sicher, dass Ihr Paket Symbole zum Abrufen des genauen Fehlers bei der Analyse enthält. Weitere Informationen finden Sie unter [Konfigurieren eines App-Pakets](/windows/msix/package/packaging-uwp-apps#configure-an-app-package). Im Gegensatz dazu bedeuten Fehlernamen, die **!unknown_error_in_** und **!unknown_function** enthalten, dass das Sammeln der Informationen aus verschiedenen anderen Gründen nicht durchgeführt werden konnte.

Wählen Sie zum Anzeigen der **Fehlerdetails** für einen bestimmten Fehler den Fehlernamen aus. Wenn Sie Symboldateien eingefügt haben, beinhaltet der Bericht **Fehlerdetails** die Anzahl der Fehlertreffer im letzten Monat sowie ein Fehlerprotokoll, in dem die entsprechenden Details (Datum, Paketversion, Gerätetyp, Gerätemodell, Betriebssystembuild) sowie ein Link zu der Ablaufverfolgung und/oder einer CAB-Datei, wenn verfügbar, aufgeführt werden.

> [!TIP]
> CAB-Dateien sind nur verfügbar, wenn Fehler auf einem Computer mit einer Windows-Insider-Build auftreten, daher haben nicht alle Fehler die Option zum Herunterladen einer CAB-Datei. Wenn nur Fehler mit CAB-Dateien angezeigt werden sollen, wählen Sie im Abschnitt Filter die Option **Fehler mit Downloads** aus. Sie können auch auf die **Verknüpfungs** Kopfzeile im **Fehlerprotokoll** klicken, um die Ergebnisse zu sortieren, damit Fehler, die CAB-Dateien enthalten, oben in der Liste angezeigt werden.

Auf der Seite **Fehlerdetails** wird auch das Diagramm für die **Stapel Verbreitung** angezeigt, in dem die obersten Stapel angezeigt werden, die zu dem Fehler beigetragen haben, geordnet nach Prozentsatz, und das Diagramm für die **Gerätekonfiguration (30D)** , das Details über die Konfiguration von Geräten, bei denen der Fehler aufgetreten ist. 


## <a name="crash-free-sessions-and-devices-30d"></a>Absturz freie Sitzungen und Geräte (30D)

Das Diagramm " **Absturz freie Sitzungen und Geräte** " zeigt den Prozentsatz der Geräte oder Benutzersitzungen an, bei denen in den letzten 30 Tagen kein Absturz aufgetreten ist. Diese Informationen helfen Ihnen, zu verstehen, wie weitgehend die Abstürze Auswirkungen auf Ihre Benutzer haben. Beispielsweise kann eine APP 10.000 Abstürze in einem Tag aufweisen. Wenn 90% der Geräte betroffen sind, würden Sie diese wahrscheinlich als kritisch klassifizieren und reagieren, um das Problem sofort zu beheben. Wenn dies jedoch nur 5% der Geräte darstellt, die Ihre APP verwenden, kann die Priorität niedriger sein.

Dieses Diagramm verfügt über zwei Registerkarten:
- **Absturz freie Geräte**: Zeigt den Prozentsatz der eindeutigen Geräte an, für die an jedem Tag (während der letzten 30 Tage) kein Fehler aufgetreten ist.
- **Absturz freie Sitzungen**: Zeigt den Prozentsatz eindeutiger Benutzersitzungen an, für die an jedem Tag (während der letzten 30 Tage) kein Fehler aufgetreten ist.


 

 
