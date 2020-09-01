---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Fehler Berichterstattungs Daten für eine Desktop Anwendung für einen bestimmten Datumsbereich und andere optionale Filter zu erhalten.
title: Abrufen von Fehlerberichtsdaten für Ihre Desktopanwendung
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Fehler, Desktop Anwendung
ms.localizationpriority: medium
ms.openlocfilehash: bb9c0bd3162f603bd9965a98c5e75bad5aae9c54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167594"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>Abrufen von Fehlerberichtsdaten für Ihre Desktopanwendung

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Fehler Berichterstattungs Daten für eine Desktop Anwendung zu erhalten, die Sie dem [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)hinzugefügt haben. Diese Methode kann nur Fehler abrufen, die innerhalb der letzten 30 Tage aufgetreten sind. Diese Informationen sind auch im Integritäts [Bericht](/windows/desktop/appxpkg/windows-desktop-application-program) für Desktop Anwendungen in Partner Center verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die Produkt-ID der Desktop Anwendung, für die Sie Fehler Berichterstattungs Daten abrufen möchten. Wenn Sie die Produkt-ID einer Desktop Anwendung abrufen möchten, öffnen Sie einen beliebigen [Analysebericht für Ihre Desktop Anwendung im Partner Center](/windows/desktop/appxpkg/windows-desktop-application-program) (z. b. den Integritäts **Bericht**), und rufen Sie die Produkt-ID aus der URL ab. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Fehler Berichterstattungs Daten im Format ```mm/dd/yyyy``` . Als Standardeinstellung wird das aktuelle Datum festgelegt.<p/><p/>**Hinweis:** &nbsp; &nbsp; Diese Methode kann nur Fehler abrufen, die innerhalb der letzten 30 Tage aufgetreten sind.  |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der abzurufenden Fehler Berichterstattungs Daten im Format ```mm/dd/yyyy``` . Als Standardeinstellung wird das aktuelle Datum festgelegt.   |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter |Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>Tick</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>Marktforschungs</strong></li><li><strong>den DeviceType "</strong></li><li><strong>ProductName</strong></li><li><strong>date</strong></li></ul> | Nein   |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: **day**, **week** oder **month**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**. Wenn Sie **week** oder **month** angeben, sind die Werte *failureName* und *failureHash* auf 1000 Buckets begrenzt.<p/>  | Nein |
| orderby | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax lautet *OrderBy = Field [Order], Field [Order],...*. Der *Feld* Parameter kann eine der folgenden Zeichen folgen sein:<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>Tick</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>Marktforschungs</strong></li><li><strong>den DeviceType "</strong></li><li><strong>ProductName</strong></li><li><strong>date</strong></li></ul>Der Parameter *order* ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist **ASC**.</p><p>Hier ist ein Beispiel für eine *OrderBy* -Zeichenfolge: *OrderBy = Date, Market*</p> |  Nein  |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li>**failureName**</li><li>**failureHash**</li><li>**Tick**</li><li>**osVersion**</li><li>**eventType**</li><li>**Marktforschungs**</li><li>**den DeviceType "**</li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter *groupby* angegeben sind, sowie die folgenden:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>Der Parameter *groupby* kann mit dem Parameter *aggregationLevel* verwendet werden. Beispiel: * &amp; GroupBy = failurename, Market &amp; aggregationlevel = Week*</p></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Fehlerberichtsdaten. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID für Ihre Desktop Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ    | BESCHREIBUNG     |
|------------|---------|--------------|
| Wert      | array   | Ein Array von Objekten, die gesammelte Fehlerberichtsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Fehlerwerte](#error-values).     |
| @nextLink  | Zeichenfolge  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | integer | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.     |


### <a name="error-values"></a>Fehlerwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG        |
|-----------------|---------|---------------------|
| date            | Zeichenfolge  | Das erste Datum im Datumsbereich für die Fehler Daten im Format ```yyyy-mm-dd``` . Wenn die Anforderung einen einzelnen Tag angibt, ist dieser Wert dieses Datum. Wenn in der Anforderung ein längerer Datumsbereich angegeben wird, ist dieser Wert das erste Datum in diesem Datumsbereich. Bei Anforderungen, die einen *aggregationlevel* -Wert von **Hour**angeben, enthält dieser Wert auch einen Uhrzeitwert im Format ```hh:mm:ss``` .  |
| applicationId   | Zeichenfolge  | Die Produkt-ID der Desktop Anwendung, für die Sie Fehler Daten abgerufen haben.    |
| ProductName | Zeichenfolge  | Der Anzeige Name der Desktop Anwendung, die von den Metadaten der zugehörigen ausführbaren Dateien abgeleitet ist.   |
| appName | Zeichenfolge  |  TBD  |
| fileName | Zeichenfolge  | Der Name der ausführbaren Datei für die Desktop Anwendung.   |
| failureName     | Zeichenfolge  | Der Name des Fehlers, der aus vier Teilen besteht: mindestens eine Problemklasse, ein Ausnahme-/Fehlerprüfcode, der Name des Abbilds, in dem der Fehler aufgetreten ist, und der zugehörige Funktionsname.  |
| failureHash     | Zeichenfolge  | Der eindeutige Bezeichner des Fehlers.   |
| Symbol          | Zeichenfolge  | Das diesem Fehler zugewiesene Symbol. |
| osBuild       | Zeichenfolge  | Die vierteilige Buildnummer des Betriebssystems, auf dem der Fehler aufgetreten ist.  |
| osVersion       | Zeichenfolge  | Eine der folgenden Zeichen folgen, die die Betriebssystemversion angibt, auf der die Desktop Anwendung installiert wird:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unbekannt</strong></li></ul>   |   
| osRelease | Zeichenfolge  | Eine der folgenden Zeichen folgen, die das Betriebssystem Release oder den flighting-Ring (als untergeordnete Population innerhalb der Betriebssystemversion) angibt, auf dem der Fehler aufgetreten ist.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Releasevorschau</strong></li><li><strong>Insider fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Wenn das Betriebssystem Release oder der flighting-Ring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p> |
| eventType       | Zeichenfolge  | Eine der folgenden Zeichen folgen, die den Typ des Fehler Ereignisses angibt:<ul><li>**um**</li><li>**hängen**</li><li>**Gedenkens**</li><li>**JSE**</li></ul>       |
| market          | Zeichenfolge  | Der ISO 3166-Ländercode des Gerätemarkts.   |
| deviceType      | Zeichenfolge  | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem der Fehler aufgetreten ist:<p/><ul><li><strong>PCs</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>Unbekannt</strong></li></ul>    |
| applicationVersion     | Zeichenfolge  |   Die Version der ausführbaren Datei der Anwendung, in der der Fehler aufgetreten ist.    |
| eventCount      | number | Die Anzahl der Ereignisse, die diesem Fehler für die angegebene Aggregationsebene zugeordnet werden.      |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Zugehörige Themen

* [Integritätsbericht](../publish/health-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md)
* [Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung](download-the-cab-file-for-an-error-in-your-desktop-application.md)