---
author: Xansky
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Mittels dieser Methode in der Microsoft Store-Analyse-API können Sie Rezensionsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen.
title: Abrufen von App-Rezensionen
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Rezensionen
ms.localizationpriority: medium
ms.openlocfilehash: 656f00ec8a7711a43c44790b04ea01dace4c4ee2
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "6472081"
---
# <a name="get-app-reviews"></a>Abrufen von App-Rezensionen


Mit dieser Methode in der Microsoft Store-Analyse-API können Sie Rezensionsdaten im JSON-Format für einen bestimmten Zeitraum sowie weitere optionale Filter abrufen. Diese Informationen sind auch im [Bericht "Rezensionen"](../publish/reviews-report.md) im Partner Center verfügbar.

Nachdem Sie Rezensionen abgerufen haben, können Sie mithilfe der Methoden [Antwortinformationen für App-Rezensionen abrufen](get-response-info-for-app-reviews.md) und [Antworten auf App-Rezensionen übermitteln](submit-responses-to-app-reviews.md) der Microsoft Store-API für Rezensionen programmgesteuert auf Rezensionen reagieren.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|---------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, für die Sie Rezensionsdaten abrufen möchten.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Wenn die Abfrage keine weiteren Zeilen enthält, entält der Antworttext den Link „Weiter“, den Sie verwenden können, um die nächste Seite mit Daten anzufordern. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000Datenzeilen usw. |  Nein  |
| filter |string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Weitere Informationen finden Sie unten im Abschnitt [Filterfelder](#filter-fields). | Nein   |
| orderby | string | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax lautet <em>orderby=field [order],field [order],...</em>, wobei der Parameter <em>field</em> eine der folgenden Zeichenfolgen sein kann:<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>rating</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |


### <a name="filter-fields"></a>Filterfelder

Der Parameter *Filter* der Anforderung enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Einige Felder unterstützen darüber hinaus die Operatoren **contains**, **gt**, **lt**, **ge** und **le**. Anweisungen können mittels **and** oder **or** kombiniert werden.

Dies ist eine Beispielzeichenfolge für *filter*: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

Eine Liste der unterstützten Felder und Operatoren für die einzelnen Felder finden Sie in der folgenden Tabelle. Zeichenfolgenwerte im Parameter *Filter* müssen von einfachen Anführungszeichen eingeschlossen werden.

| Felder        | Unterstützte Operatoren   |  Beschreibung        |
|---------------|--------|-----------------|
| market | eq, ne | Eine Zeichenfolge, die den ISO 3166-Ländercode des Gerätemarkts enthält. |
| osVersion  | eq, ne  | Eine der folgenden Zeichenfolgen:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>  |
| deviceType  | eq, ne  | Eine der folgenden Zeichenfolgen:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>  |
| isRevised  | eq, ne  | Geben Sie <strong>true</strong> an, um nach Rezensionen zu filtern, die überprüft wurden. Geben Sie andernfalls <strong>false</strong> an.  |
| packageVersion  | eq, ne  | Die Version des App-Pakets, das überprüft wurde.  |
| deviceModel  | eq, ne  | Der Typ des Geräts, auf dem die App überprüft wurde.  |
| productFamily  | eq, ne  | Eine der folgenden Zeichenfolgen:<ul><li><strong>PC</strong></li><li><strong>Tablet</strong></li><li><strong>Phone</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>Collaborative</strong></li><li><strong>Other</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | Der physische Arbeitsspeicher (RAM) in MB.  |
| deviceScreenResolution  | eq, ne  | Die Auflösung des Gerätebildschirms im Format &quot;<em>Breite</em> x <em>Höhe</em>&quot;.   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | Die Kapazität des primären Datenspeichers in GB.  |
| isTouchEnabled  | eq, ne  | Geben Sie <strong>true</strong> an, um nach für die Toucheingabe aktivierten Geräten zu filtern; andernfalls <strong>false</strong>.   |
| reviewerName  | eq, ne  |  Der Name der Person, die die App rezensiert hat. |
| rating  | eq, ne, gt, lt, ge, le  | Die App-Bewertung in Sternen.  |
| reviewTitle  | eq, ne, contains  | Der Titel der Rezension.  |
| reviewText  | eq, ne, contains  |  Der Textinhalt der Rezension. |
| helpfulCount  | eq, ne  |  Die Häufigkeit, mit der die Rezension als nützlich markiert wurde. |
| notHelpfulCount  | eq, ne  | Die Häufigkeit, mit der die Rezension als nicht nützlich markiert wurde.  |
| responseDate  | eq, ne  | Das Datum, an dem die Antwort übermittelt wurde.  |
| responseText  | eq, ne, contains  | Der Textinhalt der Antwort.  |
| ID  | eq, ne  | Die ID der Rezension (dies ist eine GUID).        |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Rezensionsdaten. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung      |
|------------|--------|------------------|
| Wert      | array  | Ein Array von Objekten, die Rezensionsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Rezensionswerte](#review-values).       |
| @nextLink  | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Rezensionsdaten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.  |

 
### <a name="review-values"></a>Rezensionswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung       |
|-----------------|---------|-------------------|
| date            | string  | Das erste Datum im Datumsbereich für die Rezensionsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId   | string  | Die Store-ID der App, für die Sie Rezensionsdaten abrufen.         |
| applicationName | string  | Der Anzeigename der App.    |
| market          | string  | Der ISO 3166-Ländercode für den Markt, in dem die Rezension übermittelt wurde.        |
| osVersion       | string  | Die Version des Betriebssystems, auf dem die Rezension übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).            |
| deviceType      | string  | Der Typ des Geräts, auf dem die Rezension übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).            |
| isRevised       | Boolescher Wert | Der Wert **true** gibt an, dass die Rezension überprüft wurde; andernfalls **false**.   |
| packageVersion  | string  | Die Version des App-Pakets, das überprüft wurde.        |
| deviceModel        | string  |Der Typ des Geräts, auf dem die App überprüft wurde.     |
| productFamily      | string  | Der Name der Gerätefamilie. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).   |
| deviceRAM       | number  | Der physische Arbeitsspeicher (RAM) in MB.    |
| deviceScreenResolution       | string  | Die Auflösung des Gerätebildschirms im Format „*Breite* x *Höhe*“.    |
| deviceStorageCapacity | number | Die Kapazität des primären Datenspeichers in GB. |
| isTouchEnabled | Boolean | Der Wert **true** gibt an, dass die Toucheingabe aktiviert ist; andernfalls **false**. |
| reviewerName | string | Der Name der Person, die die App rezensiert hat. |
| rating | number | Die App-Bewertung in Sternen. |
| reviewTitle | string | Der Titel der Rezension. |
| reviewText | string | Der Textinhalt der Rezension. |
| helpfulCount | number | Die Häufigkeit, mit der die Rezension als nützlich markiert wurde. |
| notHelpfulCount | number | Die Häufigkeit, mit der die Rezension als nicht nützlich markiert wurde. |
| responseDate | string | Das Datum, an dem eine Antwort übermittelt wurde. |
| responseText | string | Der Textinhalt der Antwort. |
| ID | string | Die ID der Rezension (dies ist eine GUID). Sie können diese ID in den Methoden [Antwortinformationen für App-Rezensionen abrufen](get-response-info-for-app-reviews.md) und [Antworten auf App-Rezensionen übermitteln](submit-responses-to-app-reviews.md) verwenden. |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1",
      "id": "6be543ff-1c9c-4534-aced-af8b4fbe0316"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht „Rezensionen“](../publish/reviews-report.md)
* [Zugreifen auf Analysedaten mit MicrosoftStore-Diensten](access-analytics-data-using-windows-store-services.md)
* [Antwortinformationen für App-Rezensionen abrufen](get-response-info-for-app-reviews.md)
* [Antworten auf App-Rezensionen übermitteln](submit-responses-to-app-reviews.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)
