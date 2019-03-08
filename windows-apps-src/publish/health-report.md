---
Description: Der Bericht zur Integrität im Partner Center können Sie die Daten, die im Zusammenhang mit Leistung und Qualität Ihrer App, einschließlich abstürzen und nicht reagierenden Ereignissen abzurufen.
title: Integritätsbericht
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Integrität, Abstürze, reagiert nicht, App-Integrität, Integritätsdaten, Stapelüberwachung, CAB-Datei, Fehler, Fehler, Pdb, Symbole
ms.localizationpriority: medium
ms.openlocfilehash: c547cc933247e69fd208e8d3c297572815a5f2ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635835"
---
# <a name="health-report"></a>Integritätsbericht

Die **Integrität** Bericht im [Partner Center](https://partner.microsoft.com/dashboard) können Sie Daten, die im Zusammenhang mit Leistung und Qualität Ihrer App, einschließlich abstürzen und nicht reagierenden Ereignissen abrufen. Sie können diese Daten anzeigen, im Partner Center oder [Bericht herunterladen](download-analytic-reports.md) offline anzeigen. Bei Bedarf können Sie Stapelüberwachungen und/oder CAB-Dateien für weitere Debugzwecke anzeigen.

Sie können die Daten in diesem Bericht aber auch programmgesteuert mit der [Microsoft Store-REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **72 H** (72 Stunden), Sie können stattdessen aber auch **30D** auswählen, um die Daten der letzten 30 Tage anzuzeigen. Beachten Sie, dass die Daten in der lokalen Zeitzone für dargestellt ist die **72H** anzeigen und im UTC-Zeit für die **30D** anzeigen.

Sie können ebenfalls **Filter** erweitern, um alle Daten auf dieser Seite nach Paketversion, Markt und/oder Gerätetyp zu filtern.

-   **Paketversion**: Die Standardeinstellung ist **alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes Paket auswählen.
-   **Markt**: Der Standardfilter ist **alle Märkte**, aber Sie können die Daten auf eine oder mehrere einschränken.
-   **Gerätetyp**: Die Standardeinstellung ist **alle**, aber Sie können auswählen, um Daten für nur ein bestimmtes Gerät anzuzeigen. Beachten Sie, dass die Kategorie **Andere** Geräte enthält, für die der Hersteller/ das Modell erkannt wird, wir diese allerdings nicht in eine der vordefinierten Kategorien einbeziehen, die in diesem Filter angezeigt werden. Für diese Geräte kann das Modells im Abschnitt **Fehlerprotokoll** des Berichts **Fehlerdetails** angezeigt werden.  
-   **Version des Betriebssystems**: Der Standardwert ist **alle Betriebssystemversionen**, aber Sie können eine bestimmte Betriebssystemversion auswählen.
-   **Betriebssystem-Releaseversion**: Der Standardwert ist **aller OS-Versionen**, aber Sie können eine bestimmte Version von den ausgewählten **Betriebssystemversion**.
-   **Sandbox**: Der Standardwert ist **Retail**, aber für Produkte, die mehrere Entwicklung Sandboxes (z. B. Spiele Xbox Live integriert) verwenden, können Sie hier ein bestimmtes Abonnement auswählen. (Wenn Ihr Produkt keine Sandboxes verwendet, zeigt dieser Filter nur **Einzelhandel** an und ist nicht anwendbar.)
-   **Architektur**: Der Standardwert ist **alle Architekturen**, aber Sie können einen bestimmte Architektur Systemtyp auswählen. Dieser Filter ist nur verfügbar, wenn **30D** ausgewählt ist.
-   **PRAID**: Die Standardeinstellung ist **alle**, aber wenn Sie mehrere relative Paket-app IDs (PRAIDs) beim Erstellen des app-Pakets definiert haben, können Sie nur Daten, die im Zusammenhang mit einem PRAID angezeigt werden. Dieser Filter wird nicht angezeigt, wenn Sie nicht mehrere PRAIDs definiert haben.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den ausgewählten Zeitraum und alle von Ihnen ausgewählten Filter. In einigen Abschnitten können Sie zusätzliche Filter anwenden.


## <a name="failure-hits"></a>Fehlertreffer

Das Diagramm **Fehlertreffer** zeigt die Anzahl von täglichen Abstürzen und Ereignissen, die Kunden bei der Nutzung Ihrer App im ausgewählten Zeitraum festgestellt haben. Jeder Ereignistyp, der in der App aufgetreten ist, wird separat überwacht: Abstürze, Blockaden, JavaScript-Ausnahmen und Speicherfehler.

Wenn die **30D** Zeitraum ausgewählt ist, wird möglicherweise Kreis Marker wird angezeigt. Dies eine erhebliche Leistungssteigerung darstellen, oder verringern Sie in einem angegebenen Wert, den wir glauben, dass Sie wissen möchten. Das Datum, die auf dem der Kreis angezeigt wird, stellt das Ende der Woche, die in der wir eine beträchtliche Zunahme oder verringern, die im Vergleich zu die Woche vor, die erkannt dar. Zeigen Sie auf den Kreis, um weitere Details zu Änderungen anzuzeigen.  

> [!TIP]
> Sehen Sie weitere Informationen zu wesentlichen Änderungen in den letzten 30 Tagen in den [Insights-Bericht](insights-report.md).

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
> Wenn **!unbekannt** im Rahmen seines Fehlernamens angezeigt wird, bedeutet dies, dass keine Symbole vorhanden waren und wir den Fehlernamen daher nicht identifizieren konnten. Stellen Sie sicher, dass Ihr Paket Symbole zum Abrufen des genauen Fehlers bei der Analyse enthält. Weitere Informationen finden Sie unter [Konfigurieren eines App-Pakets](../packaging/packaging-uwp-apps.md#configure-an-app-package). Im Gegensatz dazu bedeuten Fehlernamen, die **!unknown_error_in_** und **!unknown_function** enthalten, dass das Sammeln der Informationen aus verschiedenen anderen Gründen nicht durchgeführt werden konnte.

Wählen Sie zum Anzeigen der **Fehlerdetails** für einen bestimmten Fehler den Fehlernamen aus. Wenn Sie Symboldateien eingefügt haben, beinhaltet der Bericht **Fehlerdetails** die Anzahl der Fehlertreffer im letzten Monat sowie ein Fehlerprotokoll, in dem die entsprechenden Details (Datum, Paketversion, Gerätetyp, Gerätemodell, Betriebssystembuild) sowie ein Link zu der Ablaufverfolgung und/oder einer CAB-Datei, wenn verfügbar, aufgeführt werden.

> [!TIP]
> CAB-Dateien sind nur verfügbar, wenn Fehler auf einem Computer mit einer Windows-Insider-Build auftreten, daher haben nicht alle Fehler die Option zum Herunterladen einer CAB-Datei. Wählen Sie zum Anzeigen, die CAB-Dateien nur der Fehlschläge **Fehler mit Downloads** im Abschnitt Filter. Sie können auch klicken die **Links** -Header in der **Fehlerprotokoll** zum Sortieren der Ergebnisse, sodass Fehler aufgetreten, die CAB-Dateien sind am oberen Rand der Liste angezeigt werden.

Auf der **Fehlerdetails** Seite, und Sie sehen auch die **Stack Verbreitung** Diagramms, das die Top-Stapel, die auf den Fehler zeigt, sortiert nach Prozent beigetragen haben und die **Gerät Konfiguration (30D)** Diagramm, das den Fehler aufgetreten. Details zur Konfiguration von Geräten bereitstellt. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sitzungen ohne Abstürze und Geräten (30D)

Die **Sitzungen ohne Abstürze und Geräte** Diagramm zeigt den Prozentsatz der Geräte oder benutzersitzungen, die nicht in der letzten 30 Tage abgestürzt. Diese Informationen können Sie nachvollziehen, grob wie Ihre Abstürze Ihre Benutzer auswirken. Beispielsweise könnte eine app 10.000 Abstürze an einem Tag enthalten. Wenn 90 % der Geräte betroffen sind, würde dann wahrscheinlich klassifizieren, die als wichtig und reagieren, um es unverzüglich zu beseitigen. Wenn, die nur auf 5 % der Geräte, die Ihre App darstellt, kann die Priorität jedoch niedriger sein.

In diesem Diagramm verfügt über zwei Registerkarten:
- **Geräte ohne Abstürze**: Zeigt den Prozentsatz der eindeutigen Geräte, die jedoch an jedem Tag (während der letzten 30 Tage) tritt kein Fehler auf.
- **Ohne Sitzungen durch Abstürze**: Zeigt den Prozentsatz der eindeutigen Benutzer-Sitzungen, die jedoch an jedem Tag (während der letzten 30 Tage) tritt kein Fehler auf.


 

 
