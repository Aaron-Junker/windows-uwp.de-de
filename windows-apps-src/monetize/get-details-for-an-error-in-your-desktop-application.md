---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um ausführliche Daten für einen bestimmten Fehler für Ihre Desktop Anwendung zu erhalten.
title: Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Fehler, Details, Desktop Anwendung
ms.localizationpriority: medium
ms.openlocfilehash: 5fb904caf6e7cda0cba43d11b94aeb0811e8a504
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155614"
---
# <a name="get-details-for-an-error-in-your-desktop-application"></a>Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um ausführliche Daten für einen bestimmten Fehler für Ihre APP im JSON-Format zu erhalten. Diese Methode kann nur Details zu Fehlern abrufen, die in den letzten 30 Tagen aufgetreten sind. Ausführliche Fehler Daten sind auch im Integritäts [Bericht](/windows/desktop/appxpkg/windows-desktop-application-program) für Desktop Anwendungen in Partner Center verfügbar.

Bevor Sie diese Methode verwenden können, müssen Sie zuerst die Methode [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md) verwenden, um die ID des Fehlers abzurufen, zu dem Sie detaillierte Informationen erhalten möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID des Fehlers ab, zu dem Sie detaillierte Informationen erhalten möchten. Um diese ID zu erhalten, verwenden Sie die Methode für das [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md) und verwenden im Antworttext dieser Methode den Wert **FailureHash**.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die Produkt-ID der Desktop Anwendung, für die Fehlerdetails abgerufen werden sollen. Wenn Sie die Produkt-ID einer Desktop Anwendung abrufen möchten, öffnen Sie einen beliebigen [Analysebericht für Ihre Desktop Anwendung im Partner Center](/windows/desktop/appxpkg/windows-desktop-application-program) (z. b. den Integritäts **Bericht**), und rufen Sie die Produkt-ID aus der URL ab. |  Ja  |
| failureHash | Zeichenfolge | Die eindeutige ID des Fehlers, zu dem Sie detaillierte Informationen erhalten möchten. Um diesen Wert für den Fehler zu erhalten, an dem Sie interessiert sind, verwenden Sie die Methode für das [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md) und verwenden im Antworttext dieser Methode den Wert **FailureHash**. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der detaillierten Fehlerdaten, die abgerufen werden sollen. Der Standardwert ist 30 Tage vor dem aktuellen Datum.<p/><p/>**Hinweis:** &nbsp; &nbsp; Mit dieser Methode können nur Details zu Fehlern abgerufen werden, die in den letzten 30 Tagen aufgetreten sind. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der detaillierten Fehlerdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10“ und „skip=0“ die ersten 10 Datenzeilen ab, „top=10“ und „skip=10“ die nächsten 10 Datenzeilen usw. |  Nein  |
| filter |Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>Marktforschungs</strong></li><li><strong>date</strong></li><li><strong>cabidhash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>den DeviceType "</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul> | Nein   |
| orderby | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eine der folgenden Zeichen folgen sein:<ul><li><strong>Marktforschungs</strong></li><li><strong>date</strong></li><li><strong>cabidhash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>den DeviceType "</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen detaillierter Fehlerdaten. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID für Ihre Desktop Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ    | BESCHREIBUNG    |
|------------|---------|------------|
| Wert      | array   | Ein Array von Objekten, die detaillierte Fehlerdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Fehlerdetailwerte](#error-detail-values).          |
| @nextLink  | Zeichenfolge  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10 festgelegt ist, es jedoch mehr als 10 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | integer | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Fehlerdetailwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG     |
|-----------------|---------|----------------------------|
| applicationId   | Zeichenfolge  | Die Produkt-ID der Desktop Anwendung, für die Sie Fehlerdetails abgerufen haben.      |
| failureHash     | Zeichenfolge  | Der eindeutige Bezeichner des Fehlers.     |
| failureName     | Zeichenfolge  | Der Name des Fehlers, der aus vier Teilen besteht: mindestens eine Problemklasse, ein Ausnahme-/Fehlerprüfcode, der Name des Abbilds, in dem der Fehler aufgetreten ist, und der zugehörige Funktionsname.           |
| date            | Zeichenfolge  | Das erste Datum im Datumsbereich für die Fehlerdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| cabidhash           | Zeichenfolge  | Der eindeutige ID-Hash der CAB-Datei, die diesem Fehler zugeordnet ist.   |
| cabExpirationTime  | Zeichenfolge  | Datum und Uhrzeit im Format ISO 8601, an dem/der die CAB-Datei abgelaufen ist und nicht mehr heruntergeladen werden kann.   |
| market          | Zeichenfolge  | Der ISO 3166-Ländercode des Gerätemarkts.     |
| osBuild         | Zeichenfolge  | Die Buildnummer des Betriebssystems, auf dem der Fehler aufgetreten ist.       |
| applicationVersion         | Zeichenfolge  |   Die Version der ausführbaren Datei der Anwendung, in der der Fehler aufgetreten ist.     |
| deviceModel           | Zeichenfolge  | Eine Zeichenfolge, die das Modell des Geräts angibt, auf dem die App ausgeführt wurde, als der Fehler aufgetreten ist.   |
| osVersion       | Zeichenfolge  | Eine der folgenden Zeichen folgen, die die Betriebssystemversion angibt, auf der die Desktop Anwendung installiert wird:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unbekannt</strong></li></ul>    |
| osRelease       | Zeichenfolge  |  Eine der folgenden Zeichen folgen, die das Betriebssystem Release oder den flighting-Ring (als untergeordnete Population innerhalb der Betriebssystemversion) angibt, auf dem der Fehler aufgetreten ist.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Releasevorschau</strong></li><li><strong>Insider fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Wenn das Betriebssystem Release oder der flighting-Ring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p>    |
| deviceType      | Zeichenfolge  | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem der Fehler aufgetreten ist: <p/><ul><li><strong>PCs</strong></li><li><strong>Server</strong></li><li><strong>Unbekannt</strong></li></ul>     |
| cabDownloadable           | Boolean  | Gibt an, ob die CAB-Datei durch den Benutzer heruntergeladen werden kann.   |
| fileName           | Zeichenfolge  | Der Name der ausführbaren Datei für die Desktop Anwendung, für die Sie Fehlerdetails abgerufen haben.  |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "applicationId": "10238467886765136388",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "NULL_CLASS_PTR_WRITE_c0000005_contoso.exe!unknown_error_in_process",
      "date": "2018-01-28 23:55:29",
      "cabIdHash": "54ffb83a-e159-41d2-8158-f36f306cc01e",
      "cabExpirationTime": "2018-02-27 23:55:29",
      "market": "US",
      "osBuild": "10.0.10240",
      "applicationVersion": "2.2.2.0",
      "deviceModel": "Contoso All-in-one",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "deviceType": "PC",
      "cabDownloadable": false,
      "fileName": "contosodemo.exe"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Integritätsbericht](../publish/health-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Fehlerberichtsdaten für Ihre Desktopanwendung](get-desktop-application-error-reporting-data.md)
* [Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung](download-the-cab-file-for-an-error-in-your-desktop-application.md)