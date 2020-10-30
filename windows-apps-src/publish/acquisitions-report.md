---
description: Mit dem Bericht über Käufe in Partner Center können Sie sehen, wer Ihre APP erworben und installiert hat, zusammen mit demografischen und Platt Form Details.
title: Bericht „Käufe“
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Akquisitionen, App-Verkäufe, App-Downloads, Installationen, Trichter, Erwerb, Konvertierungen, Channel, App-Seitenaufrufe
ms.localizationpriority: medium
ms.openlocfilehash: cabdbd1433da64c927fe5e8326b7906095573497
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033283"
---
# <a name="acquisitions-report"></a>Bericht „Käufe“


Mit dem Bericht über **Käufe** in [Partner Center](https://partner.microsoft.com/dashboard) können Sie sehen, wer Ihre APP erworben und installiert hat, zusammen mit den demografischen und Platt Form Details. Außerdem erhalten Sie Informationen darüber, wie Kunden auf Windows 10 (einschließlich Xbox) in der Liste Ihrer Apps angekommen sind. Sie können auch Near Real-Time Erwerbs Daten für die letzte Stunde oder den Zeitraum von 72 Stunden anzeigen. 

Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen. Alternativ können Sie diese Daten mithilfe unserer [Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)Programm gesteuert abrufen.

In diesem Bericht bedeutet eine **Anschaffung** , dass ein neuer Kunde eine Lizenz für Ihre APP erhalten hat (egal, ob Sie Geld abgerechnet haben oder Sie kostenlos angeboten haben). Eine **Installation** bezieht sich auf die APP, die auf einem Windows 10-Gerät installiert wird.

> [!IMPORTANT]
> Der Bericht " **Akquisitionen** " enthält keine Daten über Erstattungen, Umleitungen, Rückstände usw. Um die APP zu schätzen, besuchen Sie die [Auszahlungs Zusammenfassung](payout-summary.md). Klicken Sie im Abschnitt **Reserviert** auf den Link **Reservierte Transaktionen herunterladen** .
>
> Mit Ausnahme der Seiten Ansichts Daten (wie nachstehend beschrieben) enthält dieser Bericht keine Daten, die sich auf Kunden beziehen, die eine App abrufen, ohne bei einem Microsoft-Konto angemeldet zu sein.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, in dem Sie Daten anzeigen möchten. Die Standardauswahl beträgt **30D** (30 Tage), Sie können jedoch auswählen, dass Daten für 3, 6 oder 12 Monate oder für einen von Ihnen angegebenen benutzerdefinierten Datenbereich angezeigt werden. Nahezu in Echtzeit werden Daten für alle Optionen angezeigt (außer in **kumulativen App** -Daten). Die Zeiträume von **1 Stunde** und **72H** gelten nur für die Registerkarte **app (täglich** ) des **anerwerbs** Diagramms und für die Registerkarte über **nahmen** des **Markt** Diagramms. 

Sie können auch **Filter** erweitern, um alle Daten auf dieser Seite nach Markt und/oder nach Gerätetyp zu filtern.

-   **Market** : der Standardfilter ist **Alle Märkte** , aber Sie können die Daten auf einen oder mehrere Märkte beschränken.
-   **Gerätetyp** : Die Standardeinstellung ist **Alle Geräte** . Wenn Sie nur Daten für Akquisitionen von einem bestimmten Gerätetyp anzeigen möchten (z. b. PC, Konsole oder Tablet), können Sie hier eine bestimmte auswählen.

Die Informationen in allen unten aufgeführten Diagrammen spiegeln den Datumsbereich und alle Filter wider, die Sie ausgewählt haben. In einigen Abschnitten können Sie auch zusätzliche Filter anwenden.


## <a name="acquisitions"></a>Käufe

Das **Erstellungs Diagramm zeigt** die Anzahl täglicher oder wöchentlicher Akquisitionen (ein neuer Kunde erhält eine Lizenz für Ihre APP) über den ausgewählten Zeitraum an. (Wenn Sie **Filter anwenden** verwenden, um Daten für einen längeren Zeitraum anzuzeigen, werden die Erwerbs Daten nach Woche gruppiert.) In diesem Diagramm sind nur Übernahmen von Kunden, die mit einem gültigen Microsoft-Konto angemeldet sind, enthalten. 

Standardmäßig wird die tägliche Ansicht der **App** angezeigt, die Daten nahezu in Echtzeit enthält. Sie können auch die Lebensdauer Anzahl von Akquisitionen für Ihre App anzeigen, indem Sie **App kumulativ** auswählen. Zeigt den kumulierten Gesamtwert aller Käufe an (ab der ersten Veröffentlichung Ihrer App).

Der **Bruttoumsatz** für Ihre APP (ab dem 2016-Stand Oktober) ist auch in diesem Diagramm verfügbar und zeigt den Gesamtbetrag der APP-Verkäufe (in USD). Beachten Sie, dass dieser Betrag keine Rückerstattungen, Umleitungen, Rück Belastungs Kosten usw. berücksichtigt.

Optional können Sie die Ergebnisse filtern, indem Sie vom Client oder vom webbasierten Speicher und/oder von der Betriebssystemversion stammen.

> [!NOTE]
> Sie können diese Daten auch Programm gesteuert abrufen, indem Sie die Methode [Get App Acquisitions](../monetize/get-app-acquisitions.md) in unserer [Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)verwenden.

Wenn in der täglichen Ansicht der **App** der Zeitraum von **30D** ausgewählt ist, werden möglicherweise Kreis Markierungen angezeigt. Diese stellen eine beträchtliche Zunahme oder Abnahme in einem bestimmten Wert dar, über den wir denken, dass Sie wissen möchten. Das Datum, an dem der Kreis angezeigt wird, steht für das Ende der Woche, an dem im Vergleich zur Woche vor der Woche eine beträchtliche Zunahme oder Abnahme festgestellt wurde. Zeigen Sie auf den Kreis, um weitere Details zu den Änderungen anzuzeigen.  

> [!TIP]
> Sie können in den letzten 30 Tagen im [Insights-Bericht](insights-report.md)weitere Einblicke in Bezug auf bedeutende Änderungen anzeigen.

## <a name="installs"></a>Video

Das Diagramm " **Installationen** " zeigt an, wie oft festgestellt wurde, dass Kunden Ihre APP über den ausgewählten Zeitraum auf Windows 10-Geräten (einschließlich Xbox One-Konsolen) erfolgreich installiert haben. Die Gesamtzahl wird angezeigt, zusammen mit einem Diagramm, das Installationen nach Tag oder Woche anzeigt (abhängig von der von Ihnen ausgewählten Dauer). Optional können Sie die Ergebnisse nach einer bestimmten Paketversion filtern.

Die Installations Summe umfasst Folgendes:
-   **Wird auf mehreren Windows 10-Geräten installiert.** Wenn derselbe Kunde beispielsweise die APP auf zwei Windows 10-PCs und einer Xbox One-Konsole installiert, zählt dies als drei Installationen.
-   **Neu installiert.** Wenn beispielsweise ein Kunde Ihre APP noch heute installiert, deinstalliert ihre App Morgen und installiert dann den nächsten Monat Ihrer APP neu, was als zwei Installationen zählt.

Die Installations Summe umfasst weder noch Folgendes:
-   **Wird auf Geräten ohne Windows 10 installiert.** Wenn Ihre APP frühere Betriebssystemversionen wie Windows 8. x oder Windows Phone 8. x unterstützt, werden auf diesen Geräten keine Installationen gezählt.
-   **Deinstalliert.** Wenn ein Kunde Ihre APP von seinem Gerät deinstalliert, wird dies von der Gesamtzahl der Installationen nicht subtrahiert.
-   **Updates.** Wenn z. b. ein Kunde Ihre APP heute installiert und ein App-Update eine Woche später installiert, zählt diese nur als eine Installation.
-   **Bereits vorinstalliert.** Wenn ein Kunde ein Gerät kauft, auf dem Ihre APP vorinstalliert ist, wird dies nicht als Installation gezählt.
-   **Vom System initiierte Installationen.** Wenn Windows Ihre APP aus irgendeinem Grund automatisch installiert, wird dies nicht als Installation gezählt.

> [!NOTE]
> Sie können diese Daten auch Programm gesteuert abrufen, indem Sie die Methode [Get App-Installationen](../monetize/get-app-installs.md) in unserer [Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)verwenden.

## <a name="acquisition-funnel"></a>Erfassungs Trichter

Der **Erfassungs Trichter** zeigt Ihnen, wie viele Kunden die einzelnen Schritte des Trichters abgeschlossen haben, von der Anzeige der Store-Seite bis hin zur Verwendung der APP und der Konvertierungsrate. Diese Daten können Ihnen helfen, Bereiche zu identifizieren, in denen Sie vielleicht mehr investieren möchten, um Ihre Akquisitionen, Installationen oder Nutzung zu erhöhen.

> [!IMPORTANT]
> Der **Erfassungs Trichter** zeigt Daten nur für Kunden in Windows 10 (einschließlich Xbox) während der letzten 90 Tage.

Die Schritte im Trichter sind:

- **Seitenaufrufe** : Diese Zahl stellt die Gesamtzahl der Ansichten der Store-Auflistung Ihrer APP dar, einschließlich Personen, die nicht mit einem Microsoft-Konto angemeldet sind. Dabei handelt es sich nicht um Daten von Kunden, die sich für die Bereitstellung dieser Informationen an Microsoft entschieden haben.
- **Akquisitionen** : die Anzahl der neuen Kunden, die eine Lizenz für Ihre APP erhalten haben (bei der Anmeldung mit Ihrem Microsoft-Konto) innerhalb von 48 Stunden nach dem Anzeigen der Store-Auflistung.
- **Installiert** : die Anzahl der Kunden, die die APP nach dem Erwerb installiert haben.
- **Verwendung** : die Anzahl der Kunden, die die APP nach der Installation verwendet haben.

Optional können Sie die Ergebnisse nach Geschlecht und/oder Altersgruppe sowie nach benutzerdefinierter Kampagnen-ID filtern.

> [!NOTE]
> Sie können diese Daten auch Programm gesteuert abrufen, indem Sie in unserer [Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)die Methode [Get App Acquisition Trichter Data](../monetize/get-acquisition-funnel-data.md) verwenden.

## <a name="markets"></a>Märkte

Das Diagramm " **Märkte** " zeigt die Gesamtanzahl von Akquisitionen oder Installationen für jeden Markt, in dem Ihre app verfügbar ist, über den ausgewählten Zeitraum an. Sie können auswählen, ob Daten für **Akquisitionen** oder **Installationen** angezeigt werden sollen.

Sie können diese Daten in einem visuellen **Karten** Formular anzeigen oder die Einstellung umschalten, um Sie in **Tabellen** Form anzuzeigen. Das Tabellen Formular zeigt fünf Märkte gleichzeitig an, entweder alphabetisch oder mit der höchsten/niedrigsten Anzahl von Akquisitionen oder Installationen. Sie können die Daten auch herunterladen, um Informationen für alle Märkte anzuzeigen.


## <a name="customer-demographic"></a>Kundendemografie

Das Diagramm **Kundendemografie** zeigt demografische Informationen zu den Personen, die Ihre App erworben haben. Sie können sehen, wie viele Käufe (im ausgewählten Zeitraum) von Personen einer bestimmten Altersgruppe getätigt wurden und welches Geschlecht die Käufer hatten.

> [!NOTE]
> Einige Kunden haben sich entschieden, diese Informationen nicht freizugeben. Falls die Altergruppe oder das Geschlecht nicht ermittelt werden konnten, wird der Kauf als **Unbekannt** kategorisiert.

 

## <a name="app-page-views-and-conversions-by-channel"></a>App-Seitenaufrufe und Konvertierungen nach -Kanal

Mithilfe der **App-Seitenansichten und-Konvertierungen nach Kanal** Diagramm können Sie sehen, wie Kunden auf Windows 10 über den ausgewählten Zeitraum in der Liste Ihrer Apps eingetroffen sind.

In diesem Diagramm bezieht sich ein *Kanal* auf die Methode, in der ein Kunde auf der Auflistungs Seite Ihrer APP angekommen ist (z. b. durchsuchen und suchen im Store, ein Link von einer externen Website, eine Verknüpfung von einer Ihrer benutzerdefinierten Kampagnen usw.). Die folgenden Kanaltypen sind enthalten:

-   **Store-Verkehr:** Der Kunde hat den Store durchsucht und ist dabei auf den Eintrag Ihrer App aufmerksam geworden.
-   **Benutzerdefinierte Kampagne:** Der Kunde ist einem Link gefolgt, der eine [benutzerdefinierte Kampagnen-ID](create-a-custom-app-promotion-campaign.md) verwendet.
-   **Sonstige:** Der Kunde hat eine externe Verknüpfung (ohne benutzerdefinierte Kampagnen-ID) von einer Website zur Liste Ihrer APP befolgt, oder der Kunde hat einen Link von einer Suchmaschine zum Auflisten Ihrer APP befolgt.

Ein *Seitenaufruf* bedeutet, dass ein Kunde die Store-Eintragsseite für Ihre App über den webbasierten Store oder innerhalb der Store-App auf Windows 10 angezeigt hat. Dies schließt Sichten von Personen ein, die nicht mit einem Microsoft-Konto angemeldet sind. Einige Kunden haben diese Informationen nicht für Microsoft bereitgestellt.

Eine *Konvertierung* bedeutet, dass ein Kunde (angemeldet mit einem Microsoft-Konto) eine Lizenz für Ihre APP abgerufen hat (unabhängig davon, ob Sie Geld abgerechnet haben oder Sie kostenlos angeboten haben).

Die Seitenansicht und die Konvertierungs Nummern zählen nicht zu den eindeutigen Kunden. Informationen zu den Konvertierungsraten Informationen finden Sie im Trichter Diagramm für den [Erwerb](#acquisition-funnel) .

> [!NOTE]
> Kunden können Ihre APP-Auflistung erreichen, indem Sie auf eine benutzerdefinierte Kampagne klicken, die nicht von Ihnen erstellt wurde. Wir Stempeln jede Seitenansicht innerhalb einer Sitzung mit der Kampagnen-ID, aus der der Kunde zuerst in den Store gewechselt hat. Anschließend ordnen wir dieser Kampagnen-ID für alle Käufe, die innerhalb von 24 Stunden getätigt werden, Konversionen zu. Aus diesem Grund wird möglicherweise eine größere Anzahl von Konvertierungen als bei den gesamten Konvertierungen für Ihre Kampagnen-IDs angezeigt, und Sie haben möglicherweise Konvertierungen oder Add-on-Konvertierungen, die keine Seitenaufrufe haben.

> [!NOTE]
> Sie können diese Daten auch Programm gesteuert abrufen, indem Sie die Methode [Get App Konvertierungen by Channel](../monetize/get-app-conversions-by-channel.md) in unserer [Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)verwenden.

## <a name="app-page-views-and-conversions-by-campaign-id"></a>App-Seitenaufrufe und -Konvertierungen nach Kampagnen-ID

Mit dem Diagramm **App-Seitenaufrufe und-Konvertierungen nach Kampagnen-ID** können Sie Konvertierungen und Seitenaufrufe wie oben beschrieben für die einzelnen [benutzerdefinierten Promotionkampagnen](create-a-custom-app-promotion-campaign.md)nachverfolgen. Die Top-Kampagnen-IDs werden angezeigt, und Sie können die Filter zum ausschließen oder einschließen spezifischer Kampagnen-IDs verwenden.

## <a name="total-campaign-conversions"></a>Kampagnen Konvertierungen Gesamt

Das Diagramm **Gesamt-Kampagnen Konvertierungen** zeigt die Gesamtzahl der APP-und Add-on-Konvertierungen aus allen benutzerdefinierten Kampagnen innerhalb des ausgewählten Zeitraums.





 

 
