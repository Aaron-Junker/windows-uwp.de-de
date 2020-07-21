---
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um in einem bestimmten Datumsbereich und anderen optionalen Filtern aggregierte Erfassungsdaten für eine Anwendung zu erhalten.
title: Abrufen von App-Käufen
ms.date: 03/23/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, App-Käufe
ms.localizationpriority: medium
ms.openlocfilehash: 9d0b19ee837debfa7807de7c594ae076271d3537
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493345"
---
# <a name="get-app-acquisitions"></a>Abrufen von App-Käufen


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Erwerbs Daten im JSON-Format für eine Anwendung während eines bestimmten Datums Bereichs und anderen optionalen Filtern zu erhalten. Diese Informationen finden Sie auch im Partner Center im [Bericht über Käufe](../publish/acquisitions-report.md) .

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Erfassungsdaten abrufen möchten.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Beispiel: *Filter = Market EQ ' US ' und Geschlecht EQ 'm '*. <p/><p/>Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul> | Nein   |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Käufe anfordert. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eine der folgenden Zeichen folgen sein:<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p> |  Nein  |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em> &amp; GroupBy = AgeGroup, Market &amp; aggregationlevel = Week</em></p> |  Nein  |

### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt verschiedene Anforderungen für den Abruf von Kaufdaten für Apps. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | type   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die aggregierte Erfassungsdaten für die APP enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Kaufwerte](#acquisition-values).                                                                                                                      |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Kaufdaten für die Abfrage gibt. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                 |


### <a name="acquisition-values"></a>Kaufwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | type   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| date                | Zeichenfolge | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId       | Zeichenfolge | Die Store-ID der App, für die Sie Kaufdaten abrufen.     |
| applicationName     | Zeichenfolge | Der Anzeigename der App.   |
| deviceType          | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem der Erwerb erfolgt ist:<ul><li><strong>PC</strong></li><li><strong>Smartphone</strong></li><li><strong>Konsole-Xbox One</strong></li><li><strong>Konsole-Xbox Series X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>    |
| orderName           | Zeichenfolge | Der Name der Bestellung.  |
| storeClient         | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Version des Speicher Orts angibt, in dem der Erwerb erfolgt ist:<ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (Client)** (oder **Windows Store (Client)** bei der Abfrage von Daten vor dem 23. März 2018)</li><li>**Microsoft Store (Web)** (oder **Windows Store (Web)** , wenn vor dem 23. März 2018 Daten abgefragt werden sollen)</li><li>**Volume purchase by organizations**</li><li>**Andere**</li></ul>                                                                                            |
| osVersion           | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Betriebssystemversion angibt, auf der der Erwerb erfolgt ist:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unbekannt</strong></li></ul>  |
| market              | Zeichenfolge | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte.  |
| gender              | Zeichenfolge | Eine der folgenden Zeichen folgen, die das Geschlecht des Benutzers angibt, der die Übernahme durchgeführt hat:<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unbekannt</strong></li></ul>    |
| ageGroup            | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Altersgruppe des Benutzers angibt, der die Übernahme durchgeführt hat:<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unbekannt</strong></li></ul>  |
| acquisitionType     | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Erwerbs angibt:<ul><li><strong>Free</strong></li><li><strong>Testversion</strong></li><li><strong>Bezahlt</strong></li><li><strong>Aktions Code</strong></li><li><strong>IAP</strong></li><li><strong>Abonnement-IAP</strong></li><li><strong>Private Zielgruppe</strong></li><li><strong>Vorab anordnen</strong></li><li><strong>Xbox Game Pass</strong> (oder <strong>Game Pass</strong> bei der Abfrage von Daten vor dem 23. März 2018)</li><li><strong>Disk</strong></li><li><strong>Prepaid-Code</strong></li></ul>   |
| acquisitionQuantity | number | Die Anzahl der Käufe, die während der angegebenen Aggregationsebene erfolgten.    |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht „Käufe“](../publish/acquisitions-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von App-Erwerbstrichterdaten](get-acquisition-funnel-data.md)
* [Abrufen von App-Konvertierungen nach Kanal](get-app-conversions-by-channel.md)
* [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
