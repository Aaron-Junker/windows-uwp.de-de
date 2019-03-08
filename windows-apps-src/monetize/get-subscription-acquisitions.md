---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API, um Kaufdaten für ein Add-On-Abonnement bei einem bestimmten Zeitraum und andere optionale Filter abzurufen.
title: Abrufen von Add-On-Käufen für Abonnements
ms.date: 01/25/18
ms.topic: article
keywords: Windows 10, Uwp, Store Services, Microsoft Store-Textanalyse-API, Abonnements
ms.openlocfilehash: e33a3ded219fb4d223137b40ebe871f66589addf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594775"
---
# <a name="get-subscription-add-on-acquisitions"></a>Abrufen von Add-On-Käufen für Abonnements

Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von aggregierten Kaufdaten für Add-On-Abonnements für Ihre app bei einem bestimmten Zeitraum und andere optionale Filter.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung          |
|---------------|--------|--------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store ID](in-app-purchases-and-trials.md#store-ids) der app für die Abonnement-Add-On Kaufdaten werden abgerufen werden sollen. |  Ja  |
| subscriptionProductId  | string | Die [Store ID](in-app-purchases-and-trials.md#store-ids) der Abonnement-Add-On für die Kaufdaten werden abgerufen werden sollen. Wenn Sie diesen Wert nicht angeben, gibt diese Methode die Kaufdaten für alle Abonnements Add-ons für die angegebene Anwendung.  | Nein  |
| startDate | date | Das Startdatum in den Datumsbereich für die Abonnement-Add-On Kaufdaten werden abgerufen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| endDate | date | Das Enddatum in den Datumsbereich für die Abonnement-Add-On Kaufdaten werden abgerufen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der maximale Wert und der Standardwert, wenn nicht angegeben ist 100. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen im Antworttext filtert. Alle Anweisungen können die Operatoren **eq** oder **ne** verwenden. Zudem können sie mit **and** oder **or** kombiniert werden. Sie können die folgenden Zeichenfolgen angeben, in den filteranweisungen (diese entsprechen [Werte im Antworttext](#subscription-acquisition-values)): <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Es folgt ein Beispiel *Filter* Parameter: <em>Filter = Datum-Eq "2017-07-08"</em>.</p> | Nein   |
| aggregationLevel | string | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | string | Eine Anweisung, die das Ergebnis der Datenwerte für jede Abonnement-Add-On-Abruf sortiert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |
| groupby | string | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Zum Beispiel: <em>Groupby = Markt&amp;AggregationLevel = Woche</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

In den folgenden Beispielen wird veranschaulicht, wie Abonnement-Add-On Kaufdaten werden abgerufen. Ersetzen Sie die *ApplicationId* Wert mit dem entsprechenden Store-ID für Ihre app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung         |
|------------|--------|------------------|
| Wert      | array  | Ein Array von Objekten, die aggregate-Abonnement-Add-On-Kaufdaten enthalten. Weitere Informationen zu den Daten in jedem Objekt finden Sie unter der [Übernahme abonnementwerte](#subscription-acquisition-values) Abschnitt weiter unten.             |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. B. zurückgegeben, wenn die **oben** Parameter der Anforderung auf 100 festgelegt ist, aber es gibt mehr als 100 Zeilen der Abonnement-Add-On-Kaufdaten für die Abfrage. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>Übernahme der abonnementwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ    | Beschreibung        |
|---------------------|---------|---------------------|
| date                | string  | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| subscriptionProductId      | string  | Die [Store ID](in-app-purchases-and-trials.md#store-ids) von der Abonnement-Add-On, Sie für die Kaufdaten rufen.    |
| subscriptionProductName    | string  | Der Anzeigename des Abonnements Add-Ons.         |
| applicationId       | string  | Die [Store ID](in-app-purchases-and-trials.md#store-ids) der app Sie für die Abonnement-Add-On-Kaufdaten rufen.   |
| applicationName     | string  | Der Anzeigename der App.     |
| skuId     | string  | Die ID des dem [SKU](in-app-purchases-and-trials.md#products-skus) von der Abonnement-Add-On, Sie für die Kaufdaten rufen.     |
| deviceType          | string  |  Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, mit dem der Kauf getätigt wurde:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Verwaltungskonsole</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>       |
| market           | string  | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte.     |
| currencyCode         | string  | Der Currency-Code nach ISO 4217-Format für Bruttoumsätze vor Steuern.       |
| grossSalesBeforeTax           | Ganzzahl  | Der Bruttoumsatz in der lokalen Währung, die gemäß der *CurrencyCode* Wert.     |
| totalActiveCount             | Ganzzahl  | Die Anzahl der gesamten aktiven Abonnements während des angegebenen Zeitraums. Dies entspricht der Summe aus der *GoodStandingActiveCount*, *PendingGraceActiveCount*, *GraceActiveCount*, und *LockedActiveCount* Werte.  |
| totalChurnCount              | Ganzzahl  | Die Gesamtzahl von Abonnements, die während des angegebenen Zeitraums deaktiviert wurden. Dies entspricht der Summe aus der *BillingChurnCount*, *NonRenewalChurnCount*, *RefundChurnCount*, *ChargebackChurnCount*, *EarlyChurnCount*, und *OtherChurnCount* Werte.   |
| newCount            | Ganzzahl  | Die Anzahl der neuen Abonnement Akquisitionen während des angegebenen Zeitraums, einschließlich Testversionen.    |
| renewCount     | Ganzzahl  | Die Anzahl der abonnementerneuerungen während des angegebenen Zeitraums, einschließlich der vom Benutzer initiierte erneuerungen und automatische Erneuerung.        |
| goodStandingActiveCount | Ganzzahl | Die Anzahl der Abonnements, die im angegebenen Zeitraum aktiv waren, und wo das Ablaufdatum > = die *"EndDate"* Wert für die Abfrage.    |
| pendingGraceActiveCount | Ganzzahl | Die Anzahl der Abonnements, die im angegebenen Zeitraum aktiv waren, enthielt jedoch einen Abrechnungen Fehler, und wo das Ablaufdatum des Abonnements > = die *"EndDate"* Wert für die Abfrage.     |
| graceActiveCount | Ganzzahl | Die Anzahl der Abonnements, die im angegebenen Zeitraum aktiv waren, aber musste ein Abrechnung auftritt, und wo:<ul><li>Das Ablaufdatum des Abonnements ist < der *"EndDate"* Wert für die Abfrage.</li><li>Das Ende der Toleranzperiode > = die *"EndDate"* Wert.</li></ul>        |
| lockedActiveCount | Ganzzahl | Die Anzahl der Abonnements, die in waren *dunning* (, also das Abonnement wird demnächst ablaufen, und Microsoft versucht, Finanzierung, um das Abonnement automatisch verlängern abrufen) während der angegebenen Zeit, Zeitraum und an:<ul><li>Das Ablaufdatum des Abonnements ist < der *"EndDate"* Wert für die Abfrage.</li><li>Das Ende der Toleranzperiode ist < = die *"EndDate"* Wert.</li></ul>        |
| billingChurnCount | Ganzzahl | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, aufgrund eines Fehlers beim Verarbeiten der Rechnungen einer Gebühr an, und, in dem die Abonnements wurden zuvor in dunning.        |
| nonRenewalChurnCount | Ganzzahl | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, da sie nicht verlängert wurden.        |
| refundChurnCount | Ganzzahl | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, da sie erstattet wurden.        |
| chargebackChurnCount | Ganzzahl | Die Anzahl der Abonnements, die während des angegebenen Zeitraums, da eine verbrauchsbasierte kostenzuteilung ereignet deaktiviert wurden.        |
| earlyChurnCount | Ganzzahl | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, während Sie sich keine Probleme haben.        |
| otherChurnCount | Ganzzahl | Die Anzahl der Abonnements, die während des angegebenen Zeitraums aus anderen Gründen deaktiviert wurden.        |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht zu Add-On-Käufen](../publish/add-on-acquisitions-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)

 

 
