---
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Überprüfungs Daten für einen bestimmten Datumsbereich und andere optionale Filter zu erhalten.
title: Abrufen von App-Rezensionen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Reviews
ms.localizationpriority: medium
ms.openlocfilehash: 01a22d22245882454fce6eb53b67c4fec0f8072b
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493105"
---
# <a name="get-app-reviews"></a>Abrufen von App-Rezensionen


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um für einen bestimmten Datumsbereich und andere optionale Filter Überprüfungs Daten im JSON-Format zu erhalten. Diese Informationen sind auch im [Bericht Reviews](../publish/reviews-report.md) in Partner Center verfügbar.

Nachdem Sie die Überprüfungen abgerufen haben, können Sie die [Get Response Info for App Reviews](get-response-info-for-app-reviews.md) und die [Übermittlung von Antworten an App Reviews](submit-responses-to-app-reviews.md) -Methoden in der Microsoft Store Reviews-API verwenden, um Programm gesteuert auf Überprüfungen zu reagieren.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|---------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Überprüfungs Daten abrufen möchten.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter |Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Weitere Informationen finden Sie unten im Abschnitt [Filterfelder](#filter-fields). | Nein   |
| orderby | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eine der folgenden Zeichen folgen sein:<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>Marktforschungs</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>"DeviceModel"</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewtext</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>Leistung</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p> |  Nein  |


### <a name="filter-fields"></a>Filterfelder

Der Parameter *filter* der Anforderung enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Einige Felder unterstützen darüber hinaus die Operatoren **contains**, **gt**, **lt**, **ge** und **le**. Anweisungen können mittels **and** oder **or** kombiniert werden.

Dies ist eine Beispielzeichenfolge für *filter*: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

Eine Liste der unterstützten Felder und Operatoren für die einzelnen Felder finden Sie in der folgenden Tabelle. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden.

| Felder        | Unterstützte Operatoren   |  BESCHREIBUNG        |
|---------------|--------|-----------------|
| market | eq, ne | Eine Zeichenfolge, die den ISO 3166-Ländercode des Gerätemarkts enthält. |
| osVersion  | eq, ne  | Eine der folgenden Zeichenfolgen:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unbekannt</strong></li></ul>  |
| deviceType  | eq, ne  | Eine der folgenden Zeichenfolgen:<ul><li><strong>PC</strong></li><li><strong>Smartphone</strong></li><li><strong>Konsole-Xbox One</strong></li><li><strong>Konsole-Xbox Series X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>  |
| isRevised  | eq, ne  | Geben Sie <strong>true</strong> an, um nach Rezensionen zu filtern, die überprüft wurden. Geben Sie andernfalls <strong>false</strong> an.  |
| packageVersion  | eq, ne  | Die Version des App-Pakets, das überprüft wurde.  |
| deviceModel  | eq, ne  | Der Typ des Geräts, auf dem die App überprüft wurde.  |
| productFamily  | eq, ne  | Eine der folgenden Zeichenfolgen:<ul><li><strong>PC</strong></li><li><strong>Tablet</strong></li><li><strong>Smartphone</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>Gemeinsames</strong></li><li><strong>Andere</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | Der physische Arbeitsspeicher (RAM) in MB.  |
| deviceScreenResolution  | eq, ne  | Die Bildschirmauflösung des Geräts im Format &quot; <em>Breite</em> x <em>Höhe</em> &quot; .   |
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
| id  | eq, ne  | Die ID der Überprüfung (Hierbei handelt es sich um eine GUID).        |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Rezensionsdaten. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | type   | BESCHREIBUNG      |
|------------|--------|------------------|
| Wert      | array  | Ein Array von Objekten, die Rezensionsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Rezensionswerte](#review-values).       |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Rezensionsdaten für die Abfrage gibt. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.  |

 
### <a name="review-values"></a>Rezensionswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | type    | BESCHREIBUNG       |
|-----------------|---------|-------------------|
| date            | Zeichenfolge  | Das erste Datum im Datumsbereich für die Rezensionsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId   | Zeichenfolge  | Die Store-ID der App, für die Sie Rezensionsdaten abrufen.         |
| applicationName | Zeichenfolge  | Der Anzeigename der App.    |
| market          | Zeichenfolge  | Der ISO 3166-Ländercode für den Markt, in dem die Rezension übermittelt wurde.        |
| osVersion       | Zeichenfolge  | Die Version des Betriebssystems, auf dem die Rezension übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).            |
| deviceType      | Zeichenfolge  | Der Typ des Geräts, auf dem die Rezension übermittelt wurde. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).            |
| isRevised       | Boolean | Der Wert **true** gibt an, dass die Rezension überprüft wurde; andernfalls **false**.   |
| packageVersion  | Zeichenfolge  | Die Version des App-Pakets, das überprüft wurde.        |
| deviceModel        | Zeichenfolge  |Der Typ des Geräts, auf dem die App überprüft wurde.     |
| productFamily      | Zeichenfolge  | Der Name der Gerätefamilie. Eine Liste der unterstützten Zeichenfolgen finden Sie oben im Abschnitt [Filterfelder](#filter-fields).   |
| deviceRAM       | number  | Der physische Arbeitsspeicher (RAM) in MB.    |
| deviceScreenResolution       | Zeichenfolge  | Die Bildschirmauflösung des Geräts im Format "*Width* x *height*" (Breite x Höhe).    |
| deviceStorageCapacity | number | Die Kapazität des primären Datenspeichers in GB. |
| isTouchEnabled | Boolean | Der Wert **true** gibt an, dass die Toucheingabe aktiviert ist; andernfalls **false**. |
| reviewerName | Zeichenfolge | Der Name der Person, die die App rezensiert hat. |
| rating | number | Die App-Bewertung in Sternen. |
| reviewTitle | Zeichenfolge | Der Titel der Rezension. |
| reviewText | Zeichenfolge | Der Textinhalt der Rezension. |
| helpfulCount | number | Die Häufigkeit, mit der die Rezension als nützlich markiert wurde. |
| notHelpfulCount | number | Die Häufigkeit, mit der die Rezension als nicht nützlich markiert wurde. |
| responseDate | Zeichenfolge | Das Datum, an dem eine Antwort übermittelt wurde. |
| responseText | Zeichenfolge | Der Textinhalt der Antwort. |
| id | Zeichenfolge | Die ID der Überprüfung (Hierbei handelt es sich um eine GUID). Sie können diese ID in der [Get Response Info for App Reviews](get-response-info-for-app-reviews.md) und [Submit Response to App Reviews](submit-responses-to-app-reviews.md) Methods verwenden. |


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

* [Rezensionsbericht](../publish/reviews-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Antwortinformationen für App-Reviews erhalten](get-response-info-for-app-reviews.md)
* [Senden von Antworten an App-Reviews](submit-responses-to-app-reviews.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)
