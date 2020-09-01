---
ms.assetid: FAD033C7-F887-4217-A385-089F09242827
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um in einem bestimmten Datumsbereich und anderen optionalen Filtern Aggregat Installationsdaten für eine Anwendung zu erhalten.
title: Abrufen von App-Installationen
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, App-Installationen
ms.localizationpriority: medium
ms.openlocfilehash: f6a22d8f7763b6f6048992e546a70a7b747e771e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164684"
---
# <a name="get-app-installs"></a>Abrufen von App-Installationen


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um in einem bestimmten Datumsbereich und anderen optionalen Filtern Aggregat Installationsdaten im JSON-Format für eine Anwendung zu erhalten. Diese Informationen finden Sie auch im Partner Center im [Bericht über Käufe](../publish/acquisitions-report.md) .

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Installationsdaten abrufen möchten.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Installationsdaten. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der abzurufenden Installationsdaten. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>Marktforschungs</strong></li><li><strong>osVersion</strong></li><li><strong>den DeviceType "</strong></li><li><strong>packageVersion</strong></li></ul> | Nein   |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | Zeichenfolge | Eine-Anweisung, die die Ergebnisdaten Werte für jede Installation anordnet. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eines der folgenden Felder aus dem Antworttext sein:<p/><ul><li><strong>applicationName</strong></li><li><strong>date</strong><li><strong>den DeviceType "</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>erfolgreiche installcount</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p> |  Nein  |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>applicationName</strong></li><li><strong>date</strong><li><strong>den DeviceType "</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>erfolgreiche installcount</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em> &amp; GroupBy = AgeGroup, Market &amp; aggregationlevel = Week</em></p> |  Nein  |

 
### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel werden mehrere Anforderungen zum erhalten von App-Installationsdaten veranschaulicht. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Aggregat Installationsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.                                                                                                                      |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10000 festgelegt ist, aber mehr als 10000 Zeilen mit Installationsdaten für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.    |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| date                | Zeichenfolge | Das erste Datum im Datumsbereich für die Installationsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId       | Zeichenfolge | Die Speicher-ID der APP, für die Sie Installationsdaten abrufen.     |
| applicationName     | Zeichenfolge | Der Anzeigename der App.     |
| deviceType          | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, das die Installation abgeschlossen hat:<p/><ul><li><strong>PCs</strong></li><li><strong>Smartphone</strong></li><li><strong>Konsole-Xbox One</strong></li><li><strong>Konsole-Xbox Series X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>  |
| packageVersion           | Zeichenfolge | Die Version des Pakets, das installiert wurde.  |
| osVersion           | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Betriebssystemversion angibt, auf der die Installation aufgetreten ist:<p/><ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unbekannt</strong></li></ul>   |
| market              | Zeichenfolge | Der ISO 3166-Ländercode des Markts, an dem die Installation aufgetreten ist.    |
| erfolgreiche installcount | number | Die Anzahl der erfolgreichen Installationen, die während der angegebenen Aggregations Ebene aufgetreten sind.     |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "packageVersion": "1.0.0.0",
      "deviceType": "Phone",
      "market": "IT",
      "osVersion": "Windows Phone 8.1",
      "successfulInstallCount": 1
    }
  ],
  "@nextLink": "installs?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount":23012
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Installiert den Bericht.](../publish/acquisitions-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)