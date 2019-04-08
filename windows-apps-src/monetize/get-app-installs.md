---
ms.assetid: FAD033C7-F887-4217-A385-089F09242827
description: Verwenden Sie diese Methode der Microsoft Store-Analyse-API, um die aggregierten Installationsdaten für eine Anwendung während eines bestimmten Zeitraums und andere optionale Filter abzurufen.
title: Abrufen von App-Installationen
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-Analyse-API, App-Installationen
ms.localizationpriority: medium
ms.openlocfilehash: e2dc604e15180296e84d6177aa3d9b71a0bf82dc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594795"
---
# <a name="get-app-installs"></a>Abrufen von App-Installationen


Verwenden Sie diese Methode der Microsoft Store-Analyse-API, um die aggregierten Installationsdaten (im JSON-Format) für eine Anwendung während eines bestimmten Zeitraums und andere optionale Filter abzurufen. Diese Informationen sind auch verfügbar in der [Akquisitionen Bericht](../publish/acquisitions-report.md) im Partner Center.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, für die Sie Installationsdaten abrufen möchten.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Installationsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Installationsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>Markt</strong></li><li><strong>"osversion"</strong></li><li><strong>"DeviceType"</strong></li><li><strong>PackageVersion</strong></li></ul> | Nein   |
| aggregationLevel | string | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | string | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Installationen anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eines der folgenden Felder aus dem Antworttext sein:<p/><ul><li><strong>ApplicationName</strong></li><li><strong>Datum</strong><li><strong>"DeviceType"</strong></li><li><strong>Markt</strong></li><li><strong>"osversion"</strong></li><li><strong>PackageVersion</strong></li><li><strong>successfulInstallCount</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |
| groupby | string | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>ApplicationName</strong></li><li><strong>Datum</strong><li><strong>"DeviceType"</strong></li><li><strong>Markt</strong></li><li><strong>"osversion"</strong></li><li><strong>PackageVersion</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>Datum</strong></li><li><strong>applicationId</strong></li><li><strong>successfulInstallCount</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Nein  |

 
### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt verschiedene Anforderungen für das Abrufen von Installationsdaten für Apps. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die gesammelte Installationsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.                                                                                                                      |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10000 Zeilen mit Installationsdaten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.    |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Das erste Datum im Datumsbereich für die Installationsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId       | string | Die Store-ID der App, für die Sie Installationsdaten abrufen.     |
| applicationName     | string | Der Anzeigename der App.     |
| deviceType          | string | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, über das die Installation vorgenommen wurde:<p/><ul><li><strong>PC</strong></li><li><strong>Telefon</strong></li><li><strong>Verwaltungskonsole</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>  |
| packageVersion           | string | Die Version des Pakets, das installiert wurde.  |
| osVersion           | string | Eine der folgenden Zeichenfolgen, die die Version des Betriebssystems angibt, auf dem die Installation vorgenommen wurde:<p/><ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unbekannt</strong></li></ul>   |
| market              | string | Die ISO 3166-Ländercode des Markts, in dem die Installation erfolgte.    |
| successfulInstallCount | number | Die Anzahl der erfolgreichen Installationen, die während der angegebenen Aggregationsebene erfolgten.     |


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

## <a name="related-topics"></a>Verwandte Themen

* [Installationen-Bericht](../publish/installs-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
