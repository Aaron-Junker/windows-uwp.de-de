---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Fehler Berichterstattungs Daten für einen bestimmten Datumsbereich und andere optionale Filter zu erhalten.
title: Fehlerberichts Daten für Ihre APP erhalten
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Fehler
ms.localizationpriority: medium
ms.openlocfilehash: b1ecfad64598bb74ead0a912fa67b9e1c5c5d0c2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492795"
---
# <a name="get-error-reporting-data-for-your-app"></a>Fehlerberichts Daten für Ihre APP erhalten

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um für einen bestimmten Datumsbereich und andere optionale Filter aggregierte Fehler Berichterstattungs Daten für Ihre APP im JSON-Format zu erhalten. Diese Methode kann nur Fehler abrufen, die innerhalb der letzten 30 Tage aufgetreten sind. Diese Informationen sind auch im Abschnitt " **Fehler** " des Integritäts [Berichts](../publish/health-report.md) im Partner Center verfügbar.

Sie können zusätzliche Fehlerinformationen abrufen, indem Sie die Methoden [Get Error Details](get-details-for-an-error-in-your-app.md), [Get Stack Trace](get-the-stack-trace-for-an-error-in-your-app.md)und [Download CAB file](download-the-cab-file-for-an-error-in-your-app.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die Store-ID der App, für die Fehlerberichtsdaten abgerufen werden sollen. Die Store-ID ist auf der [Seite "App-Identität](../publish/view-app-identity-details.md) " im Partner Center verfügbar. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Fehlerberichtsdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. Wenn *aggregationlevel* " **Day**", " **Week**" oder " **Month**" ist, sollte dieser Parameter ein Datum im Format angeben ```mm/dd/yyyy``` . Wenn *aggregationlevel* auf **Hour**festgelegt ist, kann dieser Parameter ein Datum im Format ```mm/dd/yyyy``` oder ein Datum und eine Uhrzeit im Format angeben ```yyyy-mm-dd hh:mm:ss``` .<p/><p/>**Hinweis:** &nbsp; &nbsp; Diese Methode kann nur Fehler abrufen, die innerhalb der letzten 30 Tage aufgetreten sind.  |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Fehlerberichtsdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. Wenn *aggregationlevel* " **Day**", " **Week**" oder " **Month**" ist, sollte dieser Parameter ein Datum im Format angeben ```mm/dd/yyyy``` . Wenn *aggregationlevel* auf **Hour**festgelegt ist, kann dieser Parameter ein Datum im Format ```mm/dd/yyyy``` oder ein Datum und eine Uhrzeit im Format angeben ```yyyy-mm-dd hh:mm:ss``` . |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter |Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**Tick**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**Marktforschungs**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | Nein   |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Kann eine der folgenden Zeichen folgen sein: **Stunde**, **Tag**, **Woche**oder **Monat**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**. Wenn Sie **week** oder **month** angeben, sind die Werte *failureName* und *failureHash* auf 1000 Buckets begrenzt.<p/><p/>**Hinweis:** &nbsp; &nbsp; Wenn Sie **Hour**angeben, können Sie Fehler Daten nur aus den vorherigen 72 Stunden abrufen. Zum Abrufen von Fehler Daten, die älter als 72 Stunden sind, geben Sie **Tag** oder eine der anderen Aggregations Ebenen an.  | Nein |
| orderby | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax lautet *OrderBy = Field [Order], Field [Order],...*. Der *Feld* Parameter kann eine der folgenden Zeichen folgen sein:<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**Tick**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**Marktforschungs**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>Der Parameter *order* ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist **ASC**.</p><p>Hier ist ein Beispiel für eine *OrderBy* -Zeichenfolge: *OrderBy = Date, Market*</p> |  Nein  |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li>**failureName**</li><li>**failureHash**</li><li>**Tick**</li><li>**osVersion**</li><li>**eventType**</li><li>**Marktforschungs**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter *groupby* angegeben sind, sowie die folgenden:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**devicecount**</li><li>**eventCount**</li></ul><p>Der Parameter *groupby* kann mit dem Parameter *aggregationLevel* verwendet werden. Beispiel: * &amp; GroupBy = failurename, Market &amp; aggregationlevel = Week*</p></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Fehlerberichtsdaten. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | type    | BESCHREIBUNG     |
|------------|---------|--------------|
| Wert      | array   | Ein Array von Objekten, die gesammelte Fehlerberichtsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Fehlerwerte](#error-values).     |
| @nextLink  | Zeichenfolge  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | integer | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.     |


### <a name="error-values"></a>Fehlerwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | type    | BESCHREIBUNG        |
|-----------------|---------|---------------------|
| date            | Zeichenfolge  | Das erste Datum im Datumsbereich für die Fehler Daten im Format ```yyyy-mm-dd``` . Wenn die Anforderung einen einzelnen Tag angibt, ist dieser Wert dieses Datum. Wenn in der Anforderung ein längerer Datumsbereich angegeben wird, ist dieser Wert das erste Datum in diesem Datumsbereich. Bei Anforderungen, die einen *aggregationlevel* -Wert von **Hour**angeben, enthält dieser Wert auch einen Uhrzeitwert im Format ```hh:mm:ss``` .  |
| applicationId   | Zeichenfolge  | Die Store-ID der App, für die Fehlerdaten abgerufen werden sollen.   |
| applicationName | Zeichenfolge  | Der Anzeigename der App.   |
| failureName     | Zeichenfolge  | Der Name des Fehlers, der aus vier Teilen besteht: mindestens eine Problemklasse, ein Ausnahme-/Fehlerprüfcode, der Name des Abbilds, in dem der Fehler aufgetreten ist, und der zugehörige Funktionsname.  |
| failureHash     | Zeichenfolge  | Der eindeutige Bezeichner des Fehlers.   |
| Symbol          | Zeichenfolge  | Das diesem Fehler zugewiesene Symbol. |
| osVersion       | Zeichenfolge  | Eine der folgenden Zeichen folgen, die die Betriebssystemversion angibt, auf der der Fehler aufgetreten ist:<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**Unbekannt**</li></ul>  |
| osRelease       | Zeichenfolge  |  Eine der folgenden Zeichen folgen, die das Betriebssystem Release oder den flighting-Ring (als untergeordnete Population innerhalb der Betriebssystemversion) angibt, auf dem der Fehler aufgetreten ist.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Releasevorschau</strong></li><li><strong>Insider fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Wenn das Betriebssystem Release oder der flighting-Ring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p>    |
| eventType       | Zeichenfolge  | Eine der folgenden Zeichenfolgen:<ul><li>**um**</li><li>**hängen**</li><li>**Gedenkens**</li><li>**JSE**</li></ul>      |
| market          | Zeichenfolge  | Der ISO 3166-Ländercode des Gerätemarkts.   |
| deviceType      | Zeichenfolge  | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem der Fehler aufgetreten ist:<ul><li>**PC**</li><li>**Smartphone**</li><li>**Konsole-Xbox One**</li><li>**Konsole-Xbox Series X**</li><li>**IoT**</li><li>**Holographic**</li><li>**Unbekannt**</li></ul>    |
| packageName     | Zeichenfolge  | Der eindeutige Name des App-Pakets, das mit diesem Fehler verknüpft ist.      |
| packageVersion  | Zeichenfolge  | Die Version des App-Pakets, das mit diesem Fehler verknüpft ist.   |
| deviceCount     | number | Die Anzahl der eindeutigen Geräte, die diesem Fehler für die angegebene Aggregationsebene entsprechen.  |
| eventCount      | number | Die Anzahl der Ereignisse, die diesem Fehler für die angegebene Aggregationsebene zugeordnet werden.      |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Verwandte Themen

* [Integritätsbericht](../publish/health-report.md)
* [Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md)
* [Abrufen der Stapelüberwachung für einen Fehler in Ihrer App](get-the-stack-trace-for-an-error-in-your-app.md)
* [Herunterladen der CAB-Datei bei einem Fehler in der App](download-the-cab-file-for-an-error-in-your-app.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
* [Abrufen von App-Bewertungen](get-app-ratings.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)
