---
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um detaillierte Daten zu einem spezifischen Fehler für Ihre App zu erhalten.
title: Abrufen von Details zu einem Fehler in Ihrer App
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Fehler, Details
ms.localizationpriority: medium
ms.openlocfilehash: 5176e123d57b8bcc5d4981acc91b22329ad4c643
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662105"
---
# <a name="get-details-for-an-error-in-your-app"></a>Abrufen von Details zu einem Fehler in Ihrer App

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um detaillierte Daten zu einem spezifischen Fehler für Ihre App im JSON-Format zu erhalten. Diese Methode kann nur Details zu Fehlern abrufen, die in den letzten 30 Tagen aufgetreten sind. Ausführliche Daten werden auch in der **Fehler** Teil der [Integritätsbericht](../publish/health-report.md) im Partner Center.

Bevor Sie diese Methode verwenden können, müssen Sie zuerst die Methode [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md) verwenden, um die ID des Fehlers abzurufen, zu dem Sie detaillierte Informationen erhalten möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID des Fehlers ab, zu dem Sie detaillierte Informationen erhalten möchten. Um diese ID zu erhalten, verwenden Sie die Methode für das [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md) und verwenden im Antworttext dieser Methode den Wert **FailureHash**.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die Store-ID der App, für die detaillierte Fehlerdaten abgerufen werden sollen. Die Store-ID ist verfügbar, auf die [Seite "App-Identität"](../publish/view-app-identity-details.md) im Partner Center. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| failureHash | string | Die eindeutige ID des Fehlers, zu dem Sie detaillierte Informationen erhalten möchten. Um diesen Wert für den Fehler zu erhalten, an dem Sie interessiert sind, verwenden Sie die Methode für das [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md) und verwenden im Antworttext dieser Methode den Wert **FailureHash**. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der detaillierten Fehlerdaten, die abgerufen werden sollen. Der Standardwert ist 30 Tage vor dem aktuellen Datum.<p/><p/>**Hinweis:**&nbsp;&nbsp;dieser Methode kann nur Informationen über Fehler, die in den letzten 30 Tagen erfolgt abgerufen. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der detaillierten Fehlerdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10“ und „skip=0“ die ersten 10 Datenzeilen ab, „top=10“ und „skip=10“ die nächsten 10 Datenzeilen usw. |  Nein  |
| filter |string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Nein   |
| orderby | string | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen detaillierter Fehlerdaten. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ    | Beschreibung    |
|------------|---------|------------|
| Wert      | array   | Ein Array von Objekten, die detaillierte Fehlerdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Fehlerdetailwerte](#error-detail-values).          |
| @nextLink  | string  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10 festgelegt ist, es jedoch mehr als 10 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | Ganzzahl | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Fehlerdetailwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung     |
|-----------------|---------|----------------------------|
| applicationId   | string  | Die Store-ID der App, für die detaillierte Fehlerdaten abgerufen wurden.      |
| failureHash     | string  | Der eindeutige Bezeichner des Fehlers.     |
| failureName     | string  | Der Name des Fehlers, der aus vier Teilen besteht: eine oder mehrere Problemklassen, ein Ausnahme/Fehlerprüfcode, der Name des Image, in dem der Fehler aufgetreten ist, und der zugehörige Funktionsname.           |
| date            | string  | Das erste Datum im Datumsbereich für die Fehlerdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| cabId           | string  | Die eindeutige ID der CAB-Datei, die mit diesem Fehler verknüpft ist.   |
| cabExpirationTime  | string  | Datum und Uhrzeit im Format ISO 8601, an dem/der die CAB-Datei abgelaufen ist und nicht mehr heruntergeladen werden kann.   |
| market          | string  | Der ISO 3166-Ländercode des Gerätemarkts.     |
| osBuild         | string  | Die Buildnummer des Betriebssystems, auf dem der Fehler aufgetreten ist.       |
| packageVersion  | string  | Die Version des App-Pakets, das mit diesem Fehler verknüpft ist.    |
| deviceModel           | string  | Eine Zeichenfolge, die das Modell des Geräts angibt, auf dem die App ausgeführt wurde, als der Fehler aufgetreten ist.   |
| osVersion       | string  | Eine der folgenden Zeichenfolgen, die die Version des Betriebssystems angibt, auf dem der Fehler aufgetreten ist:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unbekannt</strong></li></ul>    |
| osRelease       | string  |  Eine der folgenden Zeichenfolgen, die die Betriebssystemversion oder den Verteilungsring (als eine Subpopulation innerhalb der Betriebssystemversion) angibt, in der bzw. dem der Fehler aufgetreten ist.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Preview-Version</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Servicepack 1</strong></li></ul><p>Wenn die Betriebssystemversion oder Verteilungsring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p>    |
| deviceType      | string  | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, auf dem die App ausgeführt wurde, als der Fehler aufgetreten ist:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Verwaltungskonsole</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>     |
| cabDownloadable           | Boolesch  | Gibt an, ob die CAB-Datei durch den Benutzer heruntergeladen werden kann.   |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "deviceType": "PC",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht zur Integrität](../publish/health-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Daten für die Fehlerberichterstattung](get-error-reporting-data.md)
* [Die stapelüberwachung für einen Fehler in Ihrer app abrufen](get-the-stack-trace-for-an-error-in-your-app.md)
* [Laden Sie die CAB-Datei für einen Fehler in Ihrer app](download-the-cab-file-for-an-error-in-your-app.md)
