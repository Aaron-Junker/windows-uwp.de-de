---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von Daten der aggregate-Add-On-Übernahme.
title: Xbox One-Add-On-Käufe abrufen
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, Uwp, Store Services, Microsoft Store-Textanalyse-API, Xbox One-Add-On-Übernahmen
ms.localizationpriority: medium
ms.openlocfilehash: 1387e9adc5d6ef3e7a76b6b2e898c863e8434a9b
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57822905"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>Xbox One-Add-On-Käufe abrufen

Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API aggregieren-Add-On Kaufdaten werden im JSON-Format für eine Xbox One Spiel abgerufen, die über die Xbox-Entwickler-Portal (XDP) erfasste und im XDP Analytics Partner Center-Dashboard verfügbar war.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung          |
|---------------|--------|--------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Die *ApplicationId* oder *AddonProductId* -Parameter ist erforderlich. Um Kaufdaten für alle für die App registrierten Add-Ons abzurufen, geben Sie den Parameter *applicationId* an. Um die Kaufdaten für eine einzelne Add-on abzurufen, geben die *AddonProductId* Parameter. Wenn Sie beide Parameter angeben, wird der Parameter *applicationId* ignoriert.

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | String | Die *"ProductID"* des Xbox One-Spiels für die Sie Kaufdaten abrufen. Beachten Sie, dass dies die Store-ID und nicht die XDP-Produkt-ID Zum Abrufen der *"ProductID"* Ihres Spiels, navigieren Sie zu Ihr Spiel im Programm Analytics XDP und Abrufen der *"ProductID"* aus der URL. Sie können auch, wenn Sie die Übernahmen Daten aus den Partner Center-Analysebericht Herunterladen der *"ProductID"* befindet sich im TSV-Datei. |  Ja  |
| addonProductId | String | Die *"ProductID"* das Add-On für die Kaufdaten werden abgerufen werden sollen.  | Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Add-On-Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Add-On-Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter |String  | <p>Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, der die Operatoren Eq "oder" Ne zugeordnet sind, und Anweisungen mit kombiniert werden können und bzw. oder. Zeichenfolgenwerte müssen in einfache Anführungszeichen im Parameter "Filter" gesetzt werden. Z. B. Filtern = Markt Eq 'US' und Geschlecht Eq bin ".</p> <p>Sie können die folgenden Felder aus dem Antworttext angeben:</p> <ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| Nein   |
| aggregationLevel | String | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | String | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Add-On-Käufe anfordert. Die Syntax ist <em>Orderby = Feld [Reihenfolge], [Order]...</em> Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |
| groupby | String | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Zum Beispiel:  <em>&amp;Groupby = Age, Markt&amp;AggregationLevel = Woche</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Add-On-Kaufdaten. Ersetzen Sie die *AddonProductId* und *ApplicationId* Werte mit die entsprechende Store-ID für Ihre-Add-On oder die app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung         |
|------------|--------|------------------|
| Wert      | array  | Ein Array von Objekten, die aggregierte Add-On-Kaufdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Add-On-Kaufwerte](#add-on-acquisition-values).                                                                                                              |
| @nextLink  | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Add-On-Kaufdaten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Add-On-Kaufwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ    | Beschreibung        |
|---------------------|---------|---------------------|
| date                | String  | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| addonProductId      | String  | Die *"ProductID"* das Add-On für den Sie Kaufdaten abrufen.                                                                                                                                                                 |
| addonProductName    | String  | Der Anzeigename des Add-Ons. Dieser Wert wird nur in der Antwort angezeigt, wenn die *AggregationLevel* Parametersatz zu **Tag**, es sei denn, Sie geben die **AddonProductName** -Feld in der *Groupby* Parameter.                                                                                                                                                                                                            |
| applicationId       | String  | Die *"ProductID"* der app für das Add-on Kaufdaten werden abgerufen werden sollen. Beachten Sie, dass dies die Store-ID und nicht die XDP-Produkt-ID                                                                                                                                                           |
| applicationName     | String  | Der Anzeigename des Spiels.                                                                                                                                                                                                             |
| deviceType          | String  | <p>Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, mit dem der Kauf getätigt wurde:</p> <ul><li>"PC"</li><li>"Phone"</li><li>"Console"</li><li>"IoT"</li><li>"Server"</li><li>"Tablet"</li><li>"Holographic"</li><li>"Unknown"</li></ul>                                                                                                  |
| storeClient         | String  | <p>Eine der folgenden Zeichenfolgen, die die Version des Store anzeigt, wo der Kauf getätigt wurde:</p> <ul><li>"Windows Phone Store (Client)"</li><li>"Microsoft Store (Client)" (oder "Windows Store (Client)" beim Abfragen von Daten vor dem 23. März 2018)</li><li>"Microsoft Store (Web)" (oder "Windows Store (Web)" beim Abfragen von Daten vor dem 23. März 2018)</li><li>"Volume-Purchase von Unternehmen"</li><li>"Other"</li></ul>                                                                                            |
| osVersion           | String  | Die Version des Betriebssystems, auf dem der Kauf ausgeführt wurde. Für diese Methode ist dieser Wert immer "Windows 10".                                                                                                   |
| market              | String  | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte.                                                                                                                                                                  |
| gender              | String  | <p>Eine der folgenden Zeichenfolgen, die das Geschlecht des Benutzer angibt, der den Kauf getätigt hat:</p> <ul><li>"m"</li><li>"f"</li><li>"Unknown"</li></ul>                                                                                                    |
| Alter            | String  | <p>Eine der folgenden Zeichenfolgen, die die Altersgruppe des Benutzers anzeigt, der den Kauf getätigt hat:</p> <ul><li>"kleiner als"13 "</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"größer als 55"</li><li>"Unknown"</li></ul>                                                                                                 |
| acquisitionType     | String  | <p>Eine der folgenden Zeichenfolgen, die den Typ des Kaufes angibt:</p> <ul><li>"Free"</li><li>"Trial"</li><li>"Kostenpflichtiges"</li><li>"Angebotscode"</li><li>"Iap"</li><li>"Abonnement Iap"</li><li>"Audience" Private "</li><li>"Order" vor "</li><li>"Xbox Game Pass" (oder "Spiel übergeben", wenn Abfragen für Daten vor dem 23. März 2018)</li><li>"Disk"</li><li>"Im Voraus bezahlten Code"</li><li>"Vor Reihenfolge berechnet"</li><li>"Vor Auftrag abgebrochen"</li><li>"Fehler bei der Pre-Auftrag"</li></ul>                                                                                                    |
| acquisitionQuantity | Ganzzahl | Die Anzahl der Käufe, die ausgeführt wurden.                        |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
