---
ms.assetid: 235EBA39-8F64-4499-9833-4CCA9C737477
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um aggregierte Anzeigenleistungsdaten für eine Anwendung während eines bestimmten Zeitraums und andere optionale Filter abzurufen.
title: Abrufen von Anzeigenleistungsdaten
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Anzeigen, Leistung
ms.localizationpriority: medium
ms.openlocfilehash: c6bec86929284e49e4e882597422d316276c0a33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627885"
---
# <a name="get-ad-performance-data"></a>Abrufen von Anzeigenleistungsdaten


Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um aggregierte Anzeigenleistungsdaten für Ihre Anwendungen während eines bestimmten Zeitraums und andere optionale Filter abzurufen. Diese Methode gibt die Daten im JSON-Format zurück.

Diese Methode gibt zurück, die gleichen Daten, die von bereitgestellte der [ankündigen Leistungsbericht](../publish/advertising-performance-report.md) im Partner Center.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

Weitere Informationen finden Sie unter [Zugreifen auf Analysedaten mit Microsoft Store-Diensten](access-analytics-data-using-windows-store-services.md).

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung           |
|---------------|--------|--------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Um Anzeigenleistungsdaten für eine bestimmte App abzurufen, verwenden Sie den Parameter *applicationId*. Um Anzeigenleistungsdaten für alle Apps abzurufen, die Ihrem Entwicklerkonto zugeordnet sind, lassen Sie den Paramater *applicationId* aus.

| Parameter     | Typ   | Beschreibung     | Erforderlich |
|---------------|--------|-----------------|----------|
| applicationId   | string    | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, für die Sie Anzeigenleistungsdaten abrufen möchten.  |    Nein      |
| startDate   | date    | Das Startdatum im Datumsbereich der abzurufenden Anzeigenleistungsdaten im Format JJJJ/MM/TT. Der Standardwert ist das aktuelle Datum minus 30 Tage. |    Nein      |
| endDate   | date    | Das Enddatum im Datumsbereich der abzurufenden Anzeigenleistungsdaten im Format JJJJ/MM/TT. Der Standardwert ist das aktuelle Datum minus einen Tag. |    Nein      |
| top   | int    | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |    Nein      |
| skip   | int    | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |    Nein      |
| filter   | string    | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Weitere Informationen finden Sie unten im Abschnitt [Filterfelder](#filter-fields). |    Nein      |
| aggregationLevel   | string    | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. |    Nein      |
| orderby   | string    | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitId</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |    Nein      |
| groupby   | string    | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:</p><ul><li><strong>applicationId</strong></li><li><strong>applicationName</strong></li><li><strong>date</strong></li><li><strong>accountCurrencyCode</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitName</strong></li><li><strong>adUnitId</strong></li><li><strong>pubCenterAppName</strong></li><li><strong>adProvider</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p> |    Nein      |


### <a name="filter-fields"></a>Filterfelder

Der Parameter *Filter* des Anforderungstexts enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Anweisungen können mit **and** oder **or** kombiniert werden. Hier finden Sie ein Beispiel für den Parameter *filter*:

-   *Filter = Markt-Eq "De" und "DeviceType" Eq "Telefon"*

Die Liste der unterstützten Felder finden Sie in der folgenden Tabelle. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden.

| Feld | Beschreibung                                                              |
|--------|--------------------------------------------------------------------------|
| market    | Eine Zeichenfolge, die den ISO 3166-Ländercode des Markts enthält, in dem die Anzeigen platziert wurden. |
| deviceType    | Eine der folgenden Zeichenfolgen: <strong>PC/Tablet</strong> oder <strong>Phone</strong>. |
| adUnitId    | Eine Zeichenfolge, die eine Anzeigeneinheits-ID angibt, die auf den Filter angewendet werden soll. |
| pubCenterAppName    | Eine Zeichenfolge, die den PubCenter-Namen der aktuellen App angibt, der auf den Filter angewendet werden soll. |
| adProvider    | Eine Zeichenfolge, die einen Anzeigenanbieternamen angibt, der auf den Filter angewendet werden soll. |
| date    | Eine Zeichenfolge, die ein Datum im Format JJJJ/MM/TT-Filter angibt. |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt verschiedene Anforderungen für das Abrufen von Anzeigenleistungsdaten auf. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=8/1/2015&endDate=8/31/2015&skip=0&$filter=market eq 'US' and deviceType eq 'phone’ eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten mit aggregierten Anzeigenleistungsdaten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Anzeigenleistungswerte](#ad-performance-values).                                                                                                                      |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 5 festgelegt ist, es jedoch mehr als fünf Datenelemente für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                          |


### <a name="ad-performance-values"></a>Anzeigenleistungswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | string | Das erste Datum im Datumsbereich für die Anzeigenleistungsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId       | string | Die Store-ID der App, für die Sie Anzeigenleistungsdaten abrufen.     |
| applicationName     | string | Der Anzeigename der App.                         |
| adUnitId           | string | Die ID der Anzeigeneinheit.        |
| adUnitName           | string | Der Name der Ad-Komponente, laut der Entwickler im Partner Center.              |
| adProvider           |  string  |  Der Name des Anzeigenanbieters.   |
| deviceType          | string | Der Gerätetyp, auf dem die Anzeigen bereitgestellt wurden. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).                              |
| market              | string | Der ISO 3166-Ländercode des Markts, in dem die Anzeigen platziert wurden.             |
| accountCurrencyCode     | string | Der Währungscode für das Konto.        |
| pubCenterAppName       |  string  |   Der Name der PubCenter-app, die die app im Partner Center zugeordnet ist.   |
| adProviderRequests        | int | Die Anzahl der Anzeigenanforderungen für den angegebenen Anzeigenanbieter.                 |
| impressions           | int | Die Anzahl der Anzeigenaufrufe.        |
| clicks            | int | Die Anzahl der Anzeigenklicks.       |
| revenueInAccountCurrency       | number | Die Einnahmen in der Währung für das Land/die Region des Kontos.       |
| requests              | int | Die Anzahl der Anzeigenanforderungen.                 |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10765920",
      "adUnitName":"TestAdUnit",
      "revenueInAccountCurrency": 10.0,
      "impressions": 1000,
      "requests": 10000,
      "clicks": 1,
      "accountCurrencyCode":"USD"
    },
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10795110",
      "adUnitName":"TestAdUnit2",
      "revenueInAccountCurrency": 20.0,
      "impressions": 2000,
      "requests": 20000,
      "clicks": 3,
      "accountCurrencyCode":"USD"
    },
  ],
  "@nextLink": "adsperformance?applicationId=9NBLGGH4R315&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=2&skip=2",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
