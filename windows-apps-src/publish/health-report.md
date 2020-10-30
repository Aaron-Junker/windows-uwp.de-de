---
description: Der Integritäts Bericht in Partner Center ermöglicht Ihnen das Abfangen von Daten in Bezug auf die Leistung und Qualität Ihrer APP, einschließlich abstürzen und nicht reagierender Ereignisse.
title: Integritätsbericht
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Health, Abstürze, nicht reagierende Ereignisse, App-Integrität, Integritäts Daten, Stapel Überwachung, CAB-Datei, Fehler, Fehler, PDB, Symbole
ms.localizationpriority: medium
ms.openlocfilehash: a345ad9fb6f2ed5c540c6e52ef0b95e43d1adc6d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032823"
---
# <a name="health-report"></a>Integritätsbericht

Der **Integritäts Bericht in** [Partner Center](https://partner.microsoft.com/dashboard) ermöglicht Ihnen das Abfangen von Daten in Bezug auf die Leistung und Qualität Ihrer APP, einschließlich abstürzen und nicht reagierender Ereignisse. Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen. Gegebenenfalls können Sie Stapel Überwachungen und/oder CAB-Dateien zum weiteren Debuggen anzeigen.

Alternativ dazu können Sie die Daten in diesem Bericht mithilfe der [Microsoft Store Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)Programm gesteuert abrufen.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, in dem Sie Daten anzeigen möchten. Die Standardauswahl ist **72H** (72 Stunden), aber Sie können stattdessen " **30D** " auswählen, um die Daten in den letzten 30 Tagen anzuzeigen. Beachten Sie, dass die Daten in der lokalen Zeitzone für die **72H** -Ansicht und in UTC für die **30D-** Ansicht angezeigt werden.

Sie können auch **Filter** erweitern, um alle Daten auf dieser Seite nach Paketversion, Markt und/oder Gerätetyp zu filtern.

-   **Paketversion** : Die Standardeinstellung ist **Alle** . Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes Paket auswählen.
-   **Market** : der Standardfilter ist **Alle Märkte** , aber Sie können die Daten auf einen oder mehrere Märkte beschränken.
-   **Gerätetyp** : Die Standardeinstellung ist **Alle** , Sie können jedoch festlegen, dass nur Daten für einen bestimmten Gerätetyp angezeigt werden. Beachten Sie, dass die **andere** Kategorie Geräte enthält, bei denen das Make/Model erkannt wird, jedoch nicht in eine der vordefinierten Kategorien eingeschlossen werden kann, die in diesem Filter angezeigt werden. Für diese Geräte kann das Gerätemodell im Abschnitt **Fehlerprotokoll** des Berichts **Fehlerdetails** angezeigt werden.  
-   **Betriebssystemversion** : Die Standardeinstellung lautet **Alle BS-Versionen** , Sie können jedoch eine bestimmte Version des Betriebssystems auswählen.
-   **Betriebssystem-Releaseversion** : die Standardeinstellung ist **alle Betriebssystem-Releaseversionen** , Sie können jedoch eine bestimmte Releaseversion der ausgewählten **Betriebssystemversion** auswählen.
-   **Sandbox** : der Standardwert ist " **Retail** ", aber bei Produkten, die mehrere Entwicklungs Sandboxes verwenden (z. b. Spiele, die in Xbox Live integriert sind), können Sie hier eine bestimmte auswählen. (Wenn Ihr Produkt keine Sandkästen verwendet, zeigt dieser Filter nur den **Einzelhandel** an und ist nicht anwendbar.)
-   **Architektur** : die Standardeinstellung ist **alle Architekturen** , Sie können jedoch einen bestimmten Systemtyp auswählen. Dieser Filter ist nur verfügbar, wenn **30D** ausgewählt ist.
-   " **Praid** ": die Standardeinstellung ist " **all** ", aber wenn Sie beim Erstellen des App-Pakets mehrere Paket relative App-IDs (app-IDs) definiert haben, können Sie auswählen, dass nur Daten angezeigt werden, die mit einer einzelnen App verknüpft sind. Dieser Filter wird nicht angezeigt, wenn Sie nicht mehrere "praids" definiert haben.

Die Informationen in allen unten aufgeführten Diagrammen spiegeln den Datumsbereich und alle Filter wider, die Sie ausgewählt haben. In einigen Abschnitten können Sie auch zusätzliche Filter anwenden.


## <a name="failure-hits"></a>Fehler Treffer

Das Diagramm " **Fehler Treffer** " zeigt die Anzahl der täglichen Abstürze und Ereignisse an, die Kunden bei der Verwendung Ihrer APP während des ausgewählten Zeitraums fest haben. Jeder Ereignistyp, der in Ihrer APP aufgetreten ist, wird separat nachverfolgt: Abstürze, Abstürze, JavaScript-Ausnahmen und Speicherfehler.

Wenn der Zeitraum von **30D** ausgewählt ist, werden möglicherweise Kreis Markierungen angezeigt. Diese stellen eine beträchtliche Zunahme oder Abnahme in einem bestimmten Wert dar, über den wir denken, dass Sie wissen möchten. Das Datum, an dem der Kreis angezeigt wird, steht für das Ende der Woche, an dem im Vergleich zur Woche vor der Woche eine beträchtliche Zunahme oder Abnahme festgestellt wurde. Zeigen Sie auf den Kreis, um weitere Details zu den Änderungen anzuzeigen.  

> [!TIP]
> Sie können in den letzten 30 Tagen im [Insights-Bericht](insights-report.md)weitere Einblicke in Bezug auf bedeutende Änderungen anzeigen.

## <a name="failure-hits-by-market"></a>Fehler Treffer nach Markt

Im Diagramm **Fehler Treffer nach Markt** wird die Gesamtzahl der Abstürze und Ereignisse im ausgewählten Zeitraum nach Markt angezeigt.

Sie können diese Daten in einem visuellen **Karten** Formular anzeigen oder die Einstellung umschalten, um Sie in **Tabellen** Form anzuzeigen. Das Tabellen Formular zeigt fünf Märkte gleichzeitig an, entweder alphabetisch oder mit der höchsten/niedrigsten Anzahl von Benutzersitzungen. Sie können die Daten auch herunterladen, um Informationen für alle Märkte anzuzeigen.


## <a name="package-version"></a>Paketversion

Das Diagramm **Paketversion** zeigt die Gesamtanzahl von Abstürzen und Ereignissen im ausgewählten Zeitraum nach Paketversion an. In der Standardeinstellung wird die Paketversion mit den meisten Treffern an oberster Stelle vor den Märkten mit weniger Treffern angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Treffer** des Diagramms klicken.

## <a name="failures"></a>Fehler

Das Diagramm **Fehler** zeigt die Gesamtanzahl von Abstürzen und Ereignissen im ausgewählten Zeitraum nach Fehlername an. Jeder Fehler Name besteht aus vier Teilen: einer oder mehreren Problemklassen, einem Ausnahme-/Fehlerprüfcode, dem Namen des Images/Treibers, in dem der Fehler aufgetreten ist, und dem zugeordneten Funktionsnamen. In der Standardeinstellung wird der Fehler mit den meisten Treffern an oberster Stelle vor den Fehlern mit weniger Treffern angezeigt. Sie können die Reihenfolge umkehren, indem Sie auf den Pfeil in der Spalte **Treffer** des Diagramms klicken. Außerdem wird für die einzelnen Fehler der Prozentsatz der Gesamtanzahl von Fehlern angezeigt.

> [!TIP]
> Es kann vorkommen, dass in diesem Abschnitt ein Eintrag für " **Unknown** " angezeigt wird. Dies tritt auf, wenn wir trotz unserer bestmöglichen Anstrengungen keine vollständigen Details für einen oder mehrere Fehler erfassen können, die alle unter " **Unknown** " gruppiert werden. Dies tritt meistens aufgrund von Speicher Einschränkungen auf, kann aber auch auf die Datenschutzeinstellungen eines Geräts, Netzwerk Verbindungsprobleme, partielle/schlechte Absturz Abbilder und andere Faktoren zurückzuführen sein.
>
> Wenn Sie im Rahmen eines Fehler namens **! Unknown** sehen, bedeutet dies, dass keine Symbole vorhanden waren, sodass der Fehler Name nicht identifiziert werden konnte. Stellen Sie sicher, dass Sie Symbole in das Paket einschließen, um eine genaue Fehleranalyse zu erhalten. Siehe [Konfigurieren eines App-Pakets](/windows/msix/package/packaging-uwp-apps#configure-an-app-package). Im Gegensatz dazu bedeuten Fehler Namen, die **! unknown_error_in_** und **! unknown_function** enthalten, dass wir keine kompletten Details aus verschiedenen anderen Gründen erfassen konnten.

Wählen Sie zum Anzeigen der **Fehlerdetails** für einen bestimmten Fehler den Fehlernamen aus. Wenn Sie Symbol Dateien eingeschlossen haben, enthält der Bericht " **Fehlerdetails** " die Anzahl der Fehler Treffer im Verlauf des letzten Monats sowie ein Fehlerprotokoll, in dem Details zu vorkommen (Datum, Paketversion, Gerätetyp, Gerätemodell, OS-Build) und ein Link zur Stapel Überwachung und/oder CAB-Datei (falls verfügbar) aufgelistet sind.

> [!TIP]
> CAB-Dateien sind nur verfügbar, wenn der Fehler auf einem Computer mithilfe eines Windows Insider-Builds aufgetreten ist, sodass nicht alle Fehler die CAB-Download Option enthalten. Wenn nur Fehler mit CAB-Dateien angezeigt werden sollen, wählen Sie im Abschnitt Filter die Option **Fehler mit Downloads** aus. Sie können auch auf die **Verknüpfungs** Kopfzeile im **Fehlerprotokoll** klicken, um die Ergebnisse zu sortieren, damit Fehler, die CAB-Dateien enthalten, oben in der Liste angezeigt werden.

Auf der Seite **Fehlerdetails** wird auch das Diagramm für die **Stapel Verbreitung** angezeigt, in dem die oberen Stapel angezeigt werden, die zu dem Fehler beigetragen haben, geordnet nach Prozentsatz, und das Diagramm der **Gerätekonfiguration (30D)** , das Details zur Konfiguration von Geräten enthält, bei denen der Fehler aufgetreten ist. 


## <a name="crash-free-sessions-and-devices-30d"></a>Absturz freie Sitzungen und Geräte (30D)

Das Diagramm " **Absturz freie Sitzungen und Geräte** " zeigt den Prozentsatz der Geräte oder Benutzersitzungen an, bei denen in den letzten 30 Tagen kein Absturz aufgetreten ist. Diese Informationen helfen Ihnen, zu verstehen, wie weitgehend die Abstürze Auswirkungen auf Ihre Benutzer haben. Beispielsweise kann eine APP 10.000 Abstürze in einem Tag aufweisen. Wenn 90% der Geräte betroffen sind, würden Sie diese wahrscheinlich als kritisch klassifizieren und reagieren, um das Problem sofort zu beheben. Wenn dies jedoch nur 5% der Geräte darstellt, die Ihre APP verwenden, kann die Priorität niedriger sein.

Dieses Diagramm verfügt über zwei Registerkarten:
- **Absturz freie Geräte** : zeigt den Prozentsatz der eindeutigen Geräte an, für die an jedem Tag (während der letzten 30 Tage) kein Fehler aufgetreten ist.
- **Absturz freie Sitzungen** : zeigt den Prozentsatz eindeutiger Benutzersitzungen an, für die an jedem Tag (während der letzten 30 Tage) kein Fehler aufgetreten ist.


 

 
