---
description: Mit dem Bericht "Add-on-Akquisitionen" im Partner Center können Sie sehen, wie viele Add-ons Sie verkauft haben, sowie demografische und Platt Form Details.
title: Bericht zu Add-On-Käufen
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Add-on-Verkäufe, Add-on-Akquisitionen, IAP-Verkäufe, in-App-Produkte, IAPS, Add-ons
ms.localizationpriority: medium
ms.openlocfilehash: f996fdcdff8664564379d99e529398e0f998e813
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033953"
---
# <a name="add-on-acquisitions-report"></a>Bericht zu Add-On-Käufen


Mit dem Bericht " **Add-on-Akquisitionen** " in [Partner Center](https://partner.microsoft.com/dashboard) können Sie sehen, wie viele Add-ons Sie verkauft haben, zusammen mit den demografischen und Platt Form Details und Informationen zur Konvertierung für Kunden unter Windows 10 (einschließlich Xbox) anzeigen. Sie können auch Near Real-Time Erwerbs Daten für die letzte Stunde oder den Zeitraum von 72 Stunden anzeigen.

Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen. Alternativ können Sie diese Daten Programm gesteuert abrufen, indem Sie die [Get Add-on-Übernahme](../monetize/get-in-app-acquisitions.md) Methode in der [Microsoft Store Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)verwenden.

In diesem Bericht bedeutet ein Add-on-Erwerb, dass ein Kunde ein Add-on von Ihnen gekauft hat (oder es ohne Zahlung erworben hat, wenn Sie es kostenlos angeboten haben). Wenn ein Kunde mehrere Käufe desselben konsumierbaren Add-Ons getätigt hat, werden diese als separate Add-On-Käufe aufgeführt.

> [!IMPORTANT]
> Der Bericht zu den **Add-on-Akquisitionen** enthält keine Daten über Erstattungen, Umleitungen, Rückstände usw. Um die APP zu schätzen, besuchen Sie die [Auszahlungs Zusammenfassung](payout-summary.md). Klicken Sie im Abschnitt **Reserviert** auf den Link **Reservierte Transaktionen herunterladen** .


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, in dem Sie Daten anzeigen möchten. Die Standardauswahl beträgt **30D** (30 Tage), Sie können jedoch auswählen, dass Daten für 3, 6 oder 12 Monate oder für einen von Ihnen angegebenen benutzerdefinierten Datenbereich angezeigt werden. Sie können auch die Option **1 h** oder **72H** auswählen, um Erwerbs Daten für eine Stunde oder 72 Stunden nahezu in Echtzeit anzuzeigen. diese Zeiträume gelten nur für die Registerkarte " **Add-on Daily** " des Diagramms " **Add-on-Akquisitionen** " und für die Registerkarte " **Akquisitionen** " des **Markt** Diagramms. 

Sie können auch **Filter** erweitern, um alle Daten auf dieser Seite nach bestimmten Add-ons, nach Markt und/oder nach Gerätetyp zu filtern.

-   **Add-on** : der Standardfilter ist **alle Add-ons** , aber Sie können die Daten auf ein oder mehrere der Add-ons der APP beschränken.
-   **Market** : der Standardfilter ist **Alle Märkte** , aber Sie können die Daten auf einen oder mehrere Märkte beschränken.
-   **Gerätetyp** : Die Standardeinstellung ist **Alle Geräte** . Wenn Sie nur Daten für Akquisitionen von einem bestimmten Gerätetyp anzeigen möchten (z. b. PC, Konsole oder Tablet), können Sie hier eine bestimmte auswählen.

Die Informationen in allen unten aufgeführten Diagrammen spiegeln den Datumsbereich und alle Filter wider, die Sie ausgewählt haben. In einigen Abschnitten können Sie auch zusätzliche Filter anwenden.


## <a name="add-on-acquisitions"></a>Add-On-Käufe

Das Diagramm **Add-On-Käufe** zeigt, wie oft Ihre Add-Ons im ausgewählten Zeitraum pro Tag oder Woche gekauft wurde. (Wenn Sie **Filter anwenden** verwenden, um Daten für einen längeren Zeitraum anzuzeigen, werden die Erwerbs Daten nach Woche gruppiert.)

Sie können auch die Lebensdauer Anzahl von Übernahmen für Ihre App anzeigen, indem Sie die Option **Kumulatives Add-on** auswählen. Zeigt den kumulierten Gesamtwert aller Käufe an (ab der ersten Veröffentlichung Ihrer App).

Optional können Sie die Ergebnisse filtern, indem Sie vom Client oder vom webbasierten Speicher und/oder von der Betriebssystemversion stammen.


## <a name="customer-demographic"></a>Kundendemografie

Das **Demo grafikdiagramm für Kunden** zeigt demografische Informationen über die Personen an, die Ihre Add-ons erworben haben. Sie können sehen, wie viele Käufe (im ausgewählten Zeitraum) von Personen einer bestimmten Altersgruppe getätigt wurden und welches Geschlecht die Käufer hatten.

> [!NOTE]
> Einige Kunden haben sich entschieden, diese Informationen nicht freizugeben. Falls die Altergruppe oder das Geschlecht nicht ermittelt werden konnten, wird der Kauf als **Unbekannt** kategorisiert.


## <a name="markets"></a>Märkte

Das **Markt** Diagramm zeigt die Gesamtzahl der Add-on-Akquisitionen im ausgewählten Zeitraum für jeden Markt, in dem Ihre app verfügbar ist. 

Sie können diese Daten in einem visuellen **Karten** Formular anzeigen oder die Einstellung umschalten, um Sie in **Tabellen** Form anzuzeigen. Das Tabellen Formular zeigt fünf Märkte gleichzeitig an, entweder alphabetisch oder mit der höchsten/niedrigsten Anzahl von Akquisitionen oder Installationen. Sie können die Daten auch herunterladen, um Informationen für alle Märkte anzuzeigen.


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>Add-on-Seitenaufrufe und Konvertierungen nach Kampagnen-ID

Im Diagramm " **Add-on page views and Konvertierungen by Kampagnen-ID** " wird die Gesamtzahl der Add-on-Konvertierungen (Akquisitionen) pro Kampagnen-ID im ausgewählten Zeitraum angezeigt, sodass Sie Konvertierungen und Seitenaufrufe von Kunden unter Windows 10 (einschließlich Xbox) für die einzelnen [benutzerdefinierten Promotionkampagnen](create-a-custom-app-promotion-campaign.md)nachverfolgen können. In diesem Diagramm werden nur Add-on-Konvertierungen angezeigt.

> [!NOTE]
> Kunden können Ihre APP-Auflistung erreichen, indem Sie auf eine benutzerdefinierte Kampagne klicken, die nicht von Ihnen erstellt wurde. Wir Stempeln jede Seitenansicht innerhalb einer Sitzung mit der Kampagnen-ID, aus der der Kunde zuerst in den Store gewechselt hat. Anschließend ordnen wir dieser Kampagnen-ID für alle Käufe, die innerhalb von 24 Stunden getätigt werden, Konversionen zu. Aus diesem Grund wird möglicherweise eine größere Anzahl von Konvertierungen als bei den gesamten Konvertierungen für Ihre Kampagnen-IDs angezeigt, und Sie haben möglicherweise Konvertierungen oder Add-on-Konvertierungen, die keine Seitenaufrufe haben. 


## <a name="conversions-breakdown-by-campaign-id"></a>Konvertierungen werden nach Kampagnen-ID Aufschlüsselung

Mithilfe des Diagramms " **Konvertierungen nach Kampagnen-ID** " können Sie Konvertierungen und Seitenaufrufe von Kunden unter Windows 10 für jede Ihrer [benutzerdefinierten Promotionkampagnen](create-a-custom-app-promotion-campaign.md)nachverfolgen. Sowohl App-als auch Add-on-Konvertierungen werden pro Kampagnen-ID angezeigt.

In diesem Diagramm bedeutet eine *Seitenansicht* , dass ein Kunde die Store-Auflistung der App angezeigt hat. Eine *Konvertierung* bedeutet, dass ein Kunde eine Lizenz für die APP oder das Add-on abgerufen hat (unabhängig davon, ob Sie Geld abgerechnet haben oder Sie kostenlos angeboten haben).

Beachten Sie, dass es sich bei diesen Seitenansichten und Konvertierungs Nummern nicht um eindeutige Kunden handelt. 


## <a name="top-add-ons"></a>Wichtigste Add-Ons

Im **oberen Add-on-** Diagramm wird die Gesamtzahl der Käufe für die einzelnen Add-ons im ausgewählten Zeitraum angezeigt, sodass Sie sehen können, welche ihrer Add-ons am beliebtesten sind. 



 

 
