---
Description: Akquisitionen Bericht im Partner Center können Sie sehen, wer erhalten und Ihre app, demografische Informationen installiert hat und plattformdetails.
title: Bericht „Käufe“
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Käufe, App-Verkäufe, App-Downloads, Installationen, Trichter, Käufe, Konvertierungen, Kanal, App-Seitenaufrufe
ms.localizationpriority: medium
ms.openlocfilehash: af8fce64f96626e2842c3b99622af21bd286c2e4
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63770342"
---
# <a name="acquisitions-report"></a>Bericht „Käufe“


Die **Akquisitionen** Bericht im [Partner Center](https://partner.microsoft.com/dashboard) können Sie anzeigen, die erhalten und Ihre app, demografische Informationen installiert hat und plattformdetails und zeigt Informationen dazu, wie Kunden mit Windows 10 (einschließlich Xbox) bei Ihrer app-Liste erreicht haben. Sie können auch near Real-Time Kaufdaten für den letzten oder 70-zwei-Stunden-Zeitraum anzeigen. 

Sie können diese Daten anzeigen, im Partner Center oder [Bericht herunterladen](download-analytic-reports.md) offline anzeigen. Sie können diese Daten aber auch programmgesteuert mit unseren [REST-API für Analysen](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

In diesem Bericht steht **Kauf** für einen neuen Kunden, der eine Lizenz Ihrer App erworben hat (entweder für eine kostenpflichtige oder eine kostenlose App). **Installieren** bezieht sich auf die App, die auf einem Windows 10-Gerät installiert wird.

> [!IMPORTANT]
> Die **Akquisitionen** Bericht enthält keine Daten über Rückerstattungen, Rückbuchungen, eine verbrauchsbasierte kostenzuteilung. Um Ihre app-Erlöse schätzen zu können, finden Sie unter [auszahlungszusammenfassung](payout-summary.md). Klicken Sie im Abschnitt **Reserviert** auf den Link **Reservierte Transaktionen herunterladen**.
>
> Mit Ausnahme von Informationen zu den Seitenaufrufen (wie unten beschrieben) enthält dieser Bericht keine Daten im Zusammenhang mit Kunden, die eine App erwerben, ohne auf einem Microsoft-Konto angemeldet zu sein.


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **30D** (30 Tage), aber Sie können Daten für 3, 6 oder 12 Monate anzeigen, oder für einen benutzerdefinierten Zeitraum, den Sie angeben. Daten werden nahezu in Echtzeit für alle Optionen angezeigt werden (außer in **Kumulativer App** Daten). Die **1H** und **72H** Zeiträume gelten nur für die **App täglich** Registerkarte die **Akquisitionen** Diagramm und die  **Akquisitionen** Registerkarte die **Märkte** Diagramm. 

Sie können ebenfalls **Filter** erweitern, um alle Daten auf dieser Seite nach Markt und/oder Gerätetyp zu filtern.

-   **Markt**: Der Standardfilter ist **alle Märkte**, aber Sie können die Daten, Übernahmen ein oder mehrere Märkte einschränken.
-   **Gerätetyp**: Die Standardeinstellung ist **alle Geräte**. Wenn Daten für Käufe nur für einen bestimmten Gerätetyp angezeigt werden sollen (beispielsweise PCs, Konsolen oder Tablets), können Sie hier einen bestimmten Typ auswählen.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den ausgewählten Zeitraum und alle von Ihnen ausgewählten Filter. In einigen Abschnitten können Sie zusätzliche Filter anwenden.


## <a name="acquisitions"></a>Käufe

Das Diagramm **Käufe** zeigt, wie oft Ihre Käufe (ein neuer Kunde, der eine Lizenz für Ihre App ausgewählt hat) im ausgewählten Zeitraum pro Tag oder Woche gekauft wurde. (Wenn Sie die Daten über einen längeren Zeitraum mithilfe von **Filter anwenden** anzeigen, werden die Erwerbsdaten nach Woche gruppiert.) Nur die Übernahmen, die von Kunden, die mit einem gültigen Microsoft-Konto angemeldet sind, sind in diesem Diagramm enthalten. 

Standardmäßig wird die **App täglich** Ansicht, die nahe-Echtzeit-Daten enthält. Sie können auch anzeigen, wie oft die App während ihrer gesamten Lebensdauer gekauft wurde, indem Sie **App Insgesamt** auswählen. Zeigt den kumulierten Gesamtwert aller Käufe an (ab der ersten Veröffentlichung Ihrer App).

**Bruttoumsatz** für Ihre app (ab Oktober 2016 - vorhanden) finden Sie auch in diesem Diagramm, zeigt die Gesamtmenge von app-Verkäufe (in US-Dollar), die durch. Beachten Sie, dass diese Menge nicht für alle Rückerstattungen, Rückbuchungen, Chargeback usw. berücksichtigt wird.

Sie können optional die Ergebnisse danach filtern, ob die Übernahme vom Client oder einem webbasierten Store und/oder Betriebssystemversion stammt.

> [!NOTE]
> Sie können diese Daten auch programmgesteuert mit der Methode [Abrufen von App-Käufen](../monetize/get-app-acquisitions.md) unserer [Analyse-REST-API](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

In der **App täglich** anzeigen, wenn die **30D** Zeitraum ausgewählt ist, wird möglicherweise Kreis Marker wird angezeigt. Dies eine erhebliche Leistungssteigerung darstellen, oder verringern Sie in einem angegebenen Wert, den wir glauben, dass Sie wissen möchten. Das Datum, die auf dem der Kreis angezeigt wird, stellt das Ende der Woche, die in der wir eine beträchtliche Zunahme oder verringern, die im Vergleich zu die Woche vor, die erkannt dar. Zeigen Sie auf den Kreis, um weitere Details zu Änderungen anzuzeigen.  

> [!TIP]
> Sehen Sie weitere Informationen zu wesentlichen Änderungen in den letzten 30 Tagen in den [Insights-Bericht](insights-report.md).

## <a name="installs"></a>Installiert

Das Diagramm **Installiert** zeigt an, wie häufig festgestellt wurde, dass ein Kunde Ihre App über den ausgewählten Zeitraum erfolgreich auf Windows 10-Geräten (einschließlich Xbox One Konsolen) installiert hat. Es wird die Gesamtanzahl zusammen mit einem Diagramm dargestellt, das die Installation pro Tag oder Woche anzeigt (abhängig von der Dauer, die Sie ausgewählt haben). Optional können Sie die Ergebnisse nach Paketversion filtern.

Zur Gesamtübersicht der Installationen gehören:
-   **Auf mehreren Windows 10-Geräten installiert werden.** Wenn derselbe Kunde Ihre App beispielsweise aus zwei Windows 10-PCs und einer Xbox One Konsole installiert, werden drei Installationen gezählt.
-   **Neu installiert werden.** Wenn eine Kunde Ihre App beispielsweise heute installiert, morgen deinstalliert und im nächsten Monat erneut installiert, werden zwei Installationen gezählt.

Von der Gesamtansicht der Installationen sind ausgenommen:
-   **Auf nicht - Windows 10-Geräten installiert werden.** Wenn Ihre App frühere Betriebssystemversionen z. B. Windows 8.x oder Windows Phone 8.x unterstützt, zählen wir keine Installationen auf diesen Geräten.
-   **Wird deinstalliert.** Wenn ein Kunde Ihre App vom Gerät deinstalliert, wird diese nicht von der Gesamtanzahl der Installationen abgezogen.
-   **Aktualisiert.** Wenn ein Kunde Ihre App beispielsweise heute installiert und eine Woche später eine App-Aktualisierung installiert, zählt dies nur als eine Installation.
-   **Sind bereits vorinstalliert.** Wenn ein Kunde ein Gerät kauft, auf dem Ihre App vorinstalliert wurde, zählt dies nicht als Installation.
-   **Vom System initiierte Installationen.** Wenn Windows aus irgendeinem Grund Ihre App automatisch installiert, zählt dies nicht als Installation.

> [!NOTE]
> Sie können diese Daten auch programmgesteuert mit der Methode [Abrufen von App-Installationen](../monetize/get-app-installs.md) unserer [Analyse-REST-API](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

## <a name="acquisition-funnel"></a>App-Erwerbstrichter

Der **Erwerbstrichter** zeigt an, wie viele Kunden jeden Schritt des Trichters ausgeführt hat, von der Store-Seite mit der App bis hin zur Anzeige des Wechselkurses. Diese Daten helfen Ihnen Bereiche zu identifizieren, in denen Sie mehr investieren möchten, um die Käufe, Installationen oder Nutzung zu erhöhen.

> [!IMPORTANT]
> Der **Erwerbstrichter** zeigt nur Daten für Kunden auf Windows 10 (einschließlich Xbox) über die letzten 90 Tage an.

Die Schritte im Trichter sehen folgendermaßen aus:

- **Seitenaufrufe**: Dieser Wert gibt die Gesamtanzahl der Aufrufe Ihrer app Store-Auflistung an, z. B. Personen, die nicht mit einem Microsoft-Konto angemeldet sind. Dies umfasst nicht die Daten von Kunden, die diese Informationen nicht an Microsoft weitergeben möchten.
- **Akquisitionen**: Die Anzahl von neuen Kunden, die eine Lizenz für Ihre app (Wenn Sie sich mit ihrem Microsoft-Konto mit Vorzeichen) innerhalb von 48 Stunden zum Anzeigen der Store-Liste abgerufen.
- **Installiert**: Die Anzahl der Kunden, die die app nach dem Erwerb von es installiert.
- **Nutzung**: Die Anzahl der Kunden, die app nach der Installation verwendet, haben.

Sie können optional die Ergebnisse nach Geschlecht und/oder Altersgruppe filtern, sowie durch benutzerdefinierte Kampagnen-ID

> [!NOTE]
> Sie können diese Daten auch programmgesteuert mit der Methode [Abrufen von App-Erwerbstrichterdaten](../monetize/get-acquisition-funnel-data.md) unserer [Analyse-REST-API](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

## <a name="markets"></a>Märkte

Das Diagramm **Märkte** zeigt die Gesamtanzahl von Käufen für die Installationen im ausgewählten Zeitraum für jeden Markt an, auf dem Ihre App erhältlich ist. Sie können auswählen, ob Sie Daten für **Käufe** oder **Installiert** anzeigen möchten.

Sie können diese Daten in einer visuellen **Karte** anzeigen, oder die Einstellung auf eine Anzeige in Form einer **Tabelle** festlegen. Die Tabellenform zeigt jeweils fünf Märkten an, die alphabetisch oder nach der höchsten/niedrigst möglichen Anzahl von Käufen oder Installationen sortiert sind. Sie können auch die Daten zum Anzeigen von Informationen für alle Märkte zusammen herunterladen.


## <a name="customer-demographic"></a>Kundendemografie

Das Diagramm **Kundendemografie** zeigt demografische Informationen zu den Personen, die Ihre App erworben haben. Sie können sehen, wie viele Käufe (im ausgewählten Zeitraum) von Personen einer bestimmten Altersgruppe getätigt wurden und welches Geschlecht die Käufer hatten.

> [!NOTE]
> Einige Kunden haben festgelegt, dass sie diese Informationen nicht freigeben möchten. Falls die Altergruppe oder das Geschlecht nicht ermittelt werden konnten, wird der Kauf als **Unbekannt** kategorisiert.

 

## <a name="app-page-views-and-conversions-by-channel"></a>App-Seitenaufrufe und Konvertierungen nach -Kanal

Die **App Seitenaufrufe und Konvertierungen vom Kanal** Diagramm können Sie sehen, wie Kunden mit Windows 10 auf Ihrer app-Liste über den ausgewählten Zeitraum empfangen.

In diesem Diagramm bezieht sich ein *Kanal* auf die Methode, über die ein Kunde zu der Eintragsseite für Ihre App gelangt ist (z. B. durch Browsen und Suchen im Store, über einen Link von einer externen Website oder einen Link aus einer Ihrer benutzerdefinierten Kampagnen usw.). Die folgenden Kanaltypen sind enthalten:

-   **Store-Datenverkehr:** Der Kunde war durchsuchen oder Suchen in den Store, wenn sie Ihrer app-Liste angezeigt.
-   **Benutzerdefinierte Kampagne:** Der Kunde folgen, einen Link, der verwendet eine [benutzerdefinierte Kampagnen-ID](create-a-custom-app-promotion-campaign.md).
-   **Andere:** Der Kunde gefolgt einen externen Link (ohne alle benutzerdefinierten Kampagnen-ID) auf einer Website zu Ihrer app-Angebot oder der Kunde gefolgt einen Link von einer Suchmaschine für eine Auflistung von Ihrer app.

Ein *Seitenansicht* bedeutet, dass ein Kunde Angebotsseite Ihrer app-Store,, entweder über die Web-basierte Store oder in angezeigt der Store-app auf Windows 10. Dies umfasst Kontakte, die nicht mit einem Microsoft-Konto angemeldet sind. Einige Kunden haben festgelegt, das sie Microsoft diese Informationen nicht zur Verfügung stellen möchten.

*Konvertierung* bedeutet, dass ein Kunde (der mit einem Microsoft-Konto angemeldet ist) eine Lizenz für Ihre App (entweder für eine kostenpflichtige oder eine kostenlose App) neu erworben hat.

Die Seitenansicht und Konvertierungszahlen geben nicht die Anzahl an eindeutigen Kunden wieder. Informationen für Konvertierungszahlen finden Sie im [Erwerbstrichter](#acquisition-funnel)-Diagramm.

> [!NOTE]
> Der Kunden gelangt möglicherweise zu dem Eintrag Ihrer App durch Klicken auf eine nicht von Ihnen erstellte, benutzerdefinierte Kampagne. Wir zählen jeden Seitenaufruf innerhalb einer Sitzung mit der Kampagnen-ID, die der Kunde zuerst im Store eingibt. Wir fügen der Kampagnen-ID anschließend Konvertierungen für alle Käufe innerhalb von 24 Stunden hinzu. Aus diesem Grund sehen Sie möglicherweise eine höhere Anzahl an Gesamtkonvertierungen als die Konvertierungen für Ihre Kampagnen-ID anzeigt, und Sie haben möglicherweise Konvertierungen oder Add-On-Konvertierungen, die keine Seitenansichten haben.

> [!NOTE]
> Sie können diese Daten auch programmgesteuert mit der Methode [Abrufen von App-Konvertierungen nach Kanal](../monetize/get-app-conversions-by-channel.md) unserer [Analyse-REST-API](../monetize/access-analytics-data-using-windows-store-services.md) abrufen.

## <a name="app-page-views-and-conversions-by-campaign-id"></a>App-Seitenaufrufe und -Konvertierungen nach Kampagnen-ID

Mit dem Diagramm **App-Seitenaufrufe und -Konvertierungen nach Kampagnen-ID** können Sie Konvertierungen und Seitenaufrufe wie oben beschrieben für jede Ihrer [benutzerdefinierten Werbekampagnen](create-a-custom-app-promotion-campaign.md) nachverfolgen,. Die höchsten Kampagnen-IDs werden angezeigt, und mithilfe der Filter können Sie bestimmte Kampagnen-ID einbeziehen oder ausschließen.

## <a name="total-campaign-conversions"></a>Gesamtanzahl der Konvertierungen pro Kampagne

Das Diagramm der **Gesamtanzahl der Konvertierungen pro Kampagne** enthält die Gesamtanzahl der App- und Add-On-Konvertierungen aus allen Ihren benutzerdefinierten Kampagnen im ausgewählten Zeitraum.





 

 
