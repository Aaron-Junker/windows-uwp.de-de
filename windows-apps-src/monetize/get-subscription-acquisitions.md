---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Erfassungsdaten für ein Add-on-Abonnement in einem bestimmten Datumsbereich und anderen optionalen Filtern zu erhalten.
title: Abrufen von Add-On-Käufen für Abonnements
ms.date: 01/25/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Abonnements
ms.openlocfilehash: 4c307dbf7d17251e3d3c5f8792d8ea3d049924d9
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493545"
---
# <a name="get-subscription-add-on-acquisitions"></a>Abrufen von Add-On-Käufen für Abonnements

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Erfassungsdaten für Add-on-Abonnements für Ihre APP in einem bestimmten Datumsbereich und anderen optionalen Filtern zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG          |
|---------------|--------|--------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Abonnement Daten des Abonnement-Add-Ins abrufen möchten. |  Ja  |
| Abonnement ProductID  | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Abonnement-Add-Ins, für das Sie Erwerbs Daten abrufen möchten. Wenn Sie diesen Wert nicht angeben, gibt diese Methode Erwerbs Daten für alle Abonnement-Add-ons für die angegebene app zurück.  | Nein  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Abruf Daten für Abonnement-Add-on. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Abruf Daten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Höchstwert und der Standardwert, falls nicht angegeben, ist 100. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | Zeichenfolge  | Eine oder mehrere-Anweisungen, die den Antworttext filtern. Alle Anweisungen können die Operatoren **eq** oder **ne** verwenden. Zudem können sie mit **and** oder **or** kombiniert werden. Sie können die folgenden Zeichen folgen in den Filter Anweisungen angeben (diese entsprechen den [Werten im Antworttext](#subscription-acquisition-values)): <ul><li><strong>date</strong></li><li><strong>Abonnement Name</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>Marktforschungs</strong></li><li><strong>deviceType</strong></li></ul><p>Hier ist ein Beispiel für einen *Filter* Parameter: <em>Filter = Date EQ ' 2017-07-08 '</em>.</p> | Nein   |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | Zeichenfolge | Eine-Anweisung, die die Ergebnisdaten Werte für die einzelnen Abonnement-Add-on-Käufe anordnet. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eine der folgenden Zeichen folgen sein:<ul><li><strong>date</strong></li><li><strong>Abonnement Name</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>Marktforschungs</strong></li><li><strong>deviceType</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p> |  Nein  |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li><strong>date</strong></li><li><strong>Abonnement Name</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>Marktforschungs</strong></li><li><strong>deviceType</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em>GroupBy = Market &amp; aggregationlevel = Week</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

In den folgenden Beispielen wird veranschaulicht, wie Sie Abonnement-Add-on-Erwerbs Daten abrufen. Ersetzen Sie den Wert *ApplicationId* durch die entsprechende Speicher-ID für Ihre APP.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | type   | BESCHREIBUNG         |
|------------|--------|------------------|
| Wert      | array  | Ein Array von-Objekten, die Erfassungsdaten für das aggregierte Abonnement enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie im Abschnitt [Abonnement Erwerbs Werte](#subscription-acquisition-values) weiter unten.             |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 100 festgelegt ist, aber mehr als 100 Zeilen mit Abonnement-Add-on-Erfassungsdaten für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>Abonnement Erfassungs Werte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | type    | BESCHREIBUNG        |
|---------------------|---------|---------------------|
| date                | Zeichenfolge  | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| Abonnement ProductID      | Zeichenfolge  | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Abonnement-Add-Ins, für das Sie Erfassungsdaten abrufen.    |
| Abonnement Name    | Zeichenfolge  | Der Anzeige Name des Abonnement-Add-Ins.         |
| applicationId       | Zeichenfolge  | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Abonnement-Add-on-Erwerbs Daten abrufen.   |
| applicationName     | Zeichenfolge  | Der Anzeigename der App.     |
| skuId     | Zeichenfolge  | Die ID der [SKU](in-app-purchases-and-trials.md#products-skus) des Abonnement-Add-Ins, für das Sie Erfassungsdaten abrufen.     |
| deviceType          | Zeichenfolge  |  Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, das den Erwerb abgeschlossen hat:<ul><li><strong>PC</strong></li><li><strong>Smartphone</strong></li><li><strong>Konsole-Xbox One</strong></li><li><strong>Konsole-Xbox Series X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>       |
| market           | Zeichenfolge  | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte.     |
| currencyCode         | Zeichenfolge  | Der Währungscode im ISO 4217-Format für Brutto Umsätze vor Steuern.       |
| Gross Vertriebs beforetax           | integer  | Der Bruttoumsatz in der lokalen Währung, die durch den Wert "Currency *Code* " angegeben wird.     |
| totalactivecount             | integer  | Die Gesamtanzahl der aktiven Abonnements innerhalb des angegebenen Zeitraums. Dies entspricht der Summe der Werte von " *goodstandingactivecount*", " *pdinggraceactivecount*", " *graceactivecount*" und " *lockedactivecount* ".  |
| totalchurncount              | integer  | Die Gesamtanzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden. Dies entspricht der Summe der Werte " *billingchurncount*", " *nonrenewalchurncount*", " *refundchanncount*", " *chargebacktschncount*", " *earlychurncount*" und " *otherchanncount* ".   |
| newcount            | integer  | Die Anzahl der neuen Abonnement Überprüfungen innerhalb des angegebenen Zeitraums, einschließlich der Testversionen.    |
| erneuungsanzahl     | integer  | Die Anzahl der Abonnement Verlängerungen innerhalb des angegebenen Zeitraums, einschließlich der vom Benutzer initiierten Erneuerungen und automatischer Erneuerungen.        |
| goodstandingactivecount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums aktiv waren, und wobei das Ablaufdatum >= der *EndDate* -Wert für die Abfrage ist.    |
| "pdinggraceactivecount" | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums aktiv waren, bei denen ein Abrechnungsfehler aufgetreten ist und für die das Ablaufdatum des Abonnements >= der *EndDate* -Wert für die Abfrage ist.     |
| graceactivecount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums aktiv waren, bei denen ein Abrechnungsfehler aufgetreten ist und bei dem Folgendes gilt:<ul><li>Das Ablaufdatum des Abonnements ist < den *EndDate* -Wert für die Abfrage.</li><li>Das Ende der Toleranz Periode ist >= der *EndDate* -Wert.</li></ul>        |
| lockedactivecount | integer | Die Anzahl der Abonnements, die sich in *Dunning* befanden (d. h., das Abonnement läuft bald ab, und Microsoft versucht, für die automatische Verlängerung des Abonnements Geld zu erwerben) während des angegebenen Zeitraums und in folgendem Zeitraum:<ul><li>Das Ablaufdatum des Abonnements ist < den *EndDate* -Wert für die Abfrage.</li><li>Das Ende der Toleranz Periode ist <= der *EndDate* -Wert.</li></ul>        |
| billingchurncount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, weil eine Abrechnungsgebühr nicht verarbeitet werden konnte und in der sich die Abonnements zuvor in der Dämmerung befanden.        |
| nonrenewalchurncount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, weil Sie nicht erneuert wurden.        |
| refundchanncount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, weil Sie zurückerstattet wurden.        |
| chargebacktschncount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums aufgrund einer Rück Belastung deaktiviert wurden.        |
| earlychurncount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums deaktiviert wurden, während Sie sich in einem ordnungsgemäßen Zustand befanden.        |
| otherchanncount | integer | Die Anzahl der Abonnements, die während des angegebenen Zeitraums aus anderen Gründen deaktiviert wurden.        |


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
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)

 

 
