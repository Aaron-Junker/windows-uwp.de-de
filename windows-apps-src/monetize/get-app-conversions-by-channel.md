---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um in einem bestimmten Datumsbereich und anderen optionalen Filtern Aggregat Konvertierungen nach Channeldaten für eine Anwendung zu erhalten.
title: Abrufen von App-Konvertierungen nach Kanal
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, App-Konvertierungen, Channel
ms.localizationpriority: medium
ms.openlocfilehash: 58eda007ad82431734028231e8f5ae02cc704fab
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493535"
---
# <a name="get-app-conversions-by-channel"></a>Abrufen von App-Konvertierungen nach Kanal

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um in einem bestimmten Datumsbereich und anderen optionalen Filtern Aggregat Konvertierungen nach Kanal für eine Anwendung zu erhalten.

* Eine *Konvertierung* bedeutet, dass ein Kunde (angemeldet mit einem Microsoft-Konto) eine Lizenz für Ihre APP abgerufen hat (unabhängig davon, ob Sie Geld abgerechnet haben oder Sie kostenlos angeboten haben).
* Der *Kanal* ist die Methode, bei der ein Kunde auf der Auflistungs Seite Ihrer APP angekommen ist (z. b. über den Store oder eine [benutzerdefinierte App-Promotionkampagne](../publish/create-a-custom-app-promotion-campaign.md)).

Diese Informationen finden Sie auch im Partner Center im [Bericht über Käufe](../publish/acquisitions-report.md#app-page-views-and-conversions-by-channel) .

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Konvertierungsdaten abrufen möchten. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich von Konvertierungsdaten, die abgerufen werden sollen. Der Standardwert ist 1/1/2016. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich von Konvertierungsdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | Zeichenfolge  | Eine oder mehrere-Anweisungen, die den Antworttext filtern. Alle Anweisungen können die Operatoren **eq** oder **ne** verwenden. Zudem können sie mit **and** oder **or** kombiniert werden. Sie können die folgenden Zeichen folgen in den Filter Anweisungen angeben. Beschreibungen finden Sie im Abschnitt [Konvertierungs Werte](#conversion-values) in diesem Artikel. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customcampaignid</strong></li><li><strong>referenreruridomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>Marktforschungs</strong></li></ul><p>Hier ist ein Beispiel für einen *Filter* Parameter: <em>Filter = Geräte-ype EQ ' PC '</em>.</p> | Nein   |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | Zeichenfolge | Eine-Anweisung, die die Ergebnisdaten Werte für jede Konvertierung anordnet. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eine der folgenden Zeichen folgen sein:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customcampaignid</strong></li><li><strong>referenreruridomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>Marktforschungs</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p> |  Nein  |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customcampaignid</strong></li><li><strong>referenreruridomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>Marktforschungs</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>Anzahl von Komponenten</strong></li><li><strong>ClickCount</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em>GroupBy = AgeGroup, Market &amp; aggregationlevel = Week</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel werden mehrere Anforderungen zum erhalten von App-Konvertierungsdaten veranschaulicht. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | type   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die aggregierte Konvertierungsdaten für die APP enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Konvertierungs Werte](#conversion-values) .                                                                                                                      |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10 festgelegt ist, aber mehr als 10 Zeilen von Konvertierungsdaten für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.      |


### <a name="conversion-values"></a>Konvertierungs Werte

Objekte im *Wertarray* enthalten die folgenden Werte.

| Wert               | type   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| date                | Zeichenfolge | Das erste Datum im Datumsbereich für die Konvertierungsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId       | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Konvertierungsdaten abrufen.     |
| applicationName     | Zeichenfolge | Der Anzeige Name der APP, für die Sie Konvertierungsdaten abrufen.        |
| appType          | Zeichenfolge |  Der Typ des Produkts, für das Sie Konvertierungsdaten abrufen. Bei dieser Methode ist der einzige unterstützte Wert " **App**".            |
| customcampaignid           | Zeichenfolge |  Die ID-Zeichenfolge für eine [benutzerdefinierte App-Promotionkampagne](../publish/create-a-custom-app-promotion-campaign.md) , die der APP zugeordnet ist.   |
| referenreruridomain           | Zeichenfolge |  Gibt die Domäne an, in der die APP-Auflistung mit der ID der benutzerdefinierten App-herauf Stufung aktiviert wurde.   |
| channelType           | Zeichenfolge |  Eine der folgenden Zeichen folgen, die den Kanal für die Konvertierung angibt:<ul><li><strong>Customcampaignid</strong></li><li><strong>Datenverkehr speichern</strong></li><li><strong>Andere</strong></li></ul>    |
| storeClient         | Zeichenfolge | Die Version des Stores, in der die Konvertierung aufgetreten ist. Der einzige unterstützte Wert ist **sfc**.    |
| deviceType          | Zeichenfolge | Eine der folgenden Zeichenfolgen:<ul><li><strong>PC</strong></li><li><strong>Smartphone</strong></li><li><strong>Konsole-Xbox One</strong></li><li><strong>Konsole-Xbox Series X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>            |
| market              | Zeichenfolge | Der ISO 3166-Ländercode des Markts, an dem die Konvertierung erfolgt ist.    |
| ClickCount              | number  |     Die Anzahl der Kunden klickt auf den Link der APP-Auflistung.      |           
| Anzahl von Komponenten            | number  |   Die Anzahl der Kunden Konvertierungen.         |          


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "App",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "US",
      "clickCount": 7,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht „Käufe“](../publish/acquisitions-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
