---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Mittels dieser Methode in der Microsoft Store-Analyse-API können Sie gesammelte Fehlerberichtsdaten für einen bestimmten Zeitraum und andere optionale Filter abrufen.
title: Abrufen von Fehlerberichtsdaten für Ihre App
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store-Dienst, Microsoft Store-Analyse-API, Fehler
ms.localizationpriority: medium
ms.openlocfilehash: 69504fd8d670158a35e5afd5dcd035acd296f084
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590595"
---
# <a name="get-error-reporting-data-for-your-app"></a>Abrufen von Fehlerberichtsdaten für Ihre App

Mittels dieser Methode in der Microsoft Store-Analyse-API können Sie aggregierte Fehlerberichtsdaten für Ihre App (im JSON-Format) für einen bestimmten Zeitraum und andere optionale Filter abrufen. Diese Methode kann nur Fehler abrufen, die in den letzten 30 Tagen aufgetreten sind. Diese Informationen sind auch verfügbar in der **Fehler** Teil der [Integritätsbericht](../publish/health-report.md) im Partner Center.

Sie können zusätzliche Fehlerinformationen abrufen, indem Sie die Methoden [get error details](get-details-for-an-error-in-your-app.md), [get stack trace](get-the-stack-trace-for-an-error-in-your-app.md) und [download CAB file](download-the-cab-file-for-an-error-in-your-app.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die Store-ID der App, für die Fehlerberichtsdaten abgerufen werden sollen. Die Store-ID ist verfügbar, auf die [Seite "App-Identität"](../publish/view-app-identity-details.md) im Partner Center. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Fehlerberichtsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. Wenn *aggregationLevel***day**, **week** oder **month** ist, sollte dieser Parameter ein Datum im Format ```mm/dd/yyyy``` angeben. Wenn *aggregationLevel***hour** ist, kann dieser Parameter ein Datum im Format ```mm/dd/yyyy``` oder ein Datum und Uhrzeit im Format ```yyyy-mm-dd hh:mm:ss``` angeben.<p/><p/>**Hinweis:**&nbsp;&nbsp;dieser Methode kann nur in den letzten 30 Tagen aufgetretenen Fehler abgerufen werden.  |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Fehlerberichtsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. Wenn *aggregationLevel***day**, **week** oder **month** ist, sollte dieser Parameter ein Datum im Format ```mm/dd/yyyy``` angeben. Wenn *aggregationLevel***hour** ist, kann dieser Parameter ein Datum im Format ```mm/dd/yyyy``` oder ein Datum und Uhrzeit im Format ```yyyy-mm-dd hh:mm:ss``` angeben. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter |string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li>**ApplicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**Symbol**</li><li>**"osversion"**</li><li>**osRelease**</li><li>**EventType**</li><li>**Markt**</li><li>**"DeviceType"**</li><li>**Paketname**</li><li>**PackageVersion**</li><li>**Datum**</li></ul> | Nein   |
| aggregationLevel | string | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: **hour**, **day**, **week** oder **month**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**. Wenn Sie **week** oder **month** angeben, sind die Werte *failureName* und *failureHash* auf 1000 Buckets begrenzt.<p/><p/>**Hinweis:**&nbsp;&nbsp;Wenn Sie **hour** angeben, können Sie nur die Fehlerdaten der letzten 72 Stunden abrufen. Um Fehler anzurufen, die älter als 72 Stunden sind, geben Sie **day** oder eine der anderen Aggregationsebenen an.  | Nein |
| orderby | string | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax ist *orderby=field [order],field [order],...*. Der Parameter *field* kann eine der folgenden Zeichenfolgen sein:<ul><li>**ApplicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**Symbol**</li><li>**"osversion"**</li><li>**osRelease**</li><li>**EventType**</li><li>**Markt**</li><li>**"DeviceType"**</li><li>**Paketname**</li><li>**PackageVersion**</li><li>**Datum**</li></ul><p>Der Parameter *order* ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist **asc**.</p><p>Dies ist eine Beispielzeichenfolge für *orderby*: *orderby=date,market*</p> |  Nein  |
| groupby | string | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<ul><li>**failureName**</li><li>**failureHash**</li><li>**Symbol**</li><li>**"osversion"**</li><li>**EventType**</li><li>**Markt**</li><li>**"DeviceType"**</li><li>**Paketname**</li><li>**PackageVersion**</li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter *groupby* angegeben sind, sowie die folgenden:</p><ul><li>**Datum**</li><li>**applicationId**</li><li>**ApplicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>Der Parameter *groupby* kann mit dem Parameter *aggregationLevel* verwendet werden. Beispiel: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  Nein  |


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

| Wert      | Typ    | Beschreibung     |
|------------|---------|--------------|
| Wert      | array   | Ein Array von Objekten, die gesammelte Fehlerberichtsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Fehlerwerte](#error-values).     |
| @nextLink  | string  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | Ganzzahl | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.     |


### <a name="error-values"></a>Fehlerwerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|---------------------|
| date            | string  | Das erste Datum im Datumsbereich für die Fehlerdaten im Format ```yyyy-mm-dd```. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung einen längeren Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. Für Anforderungen, die einen *aggregationLevel*-Wert von **hour** angeben, enthält dieser Wert auch einen Zeitwert im Format ```hh:mm:ss```.  |
| applicationId   | string  | Die Store-ID der App, für die Fehlerdaten abgerufen werden sollen.   |
| applicationName | string  | Der Anzeigename der App.   |
| failureName     | string  | Der Name des Fehlers, der aus vier Teilen besteht: eine oder mehrere Problemklassen, ein Ausnahme/Fehlerprüfcode, der Name des Image, in dem der Fehler aufgetreten ist, und der zugehörige Funktionsname.  |
| failureHash     | string  | Der eindeutige Bezeichner des Fehlers.   |
| symbol          | string  | Das diesem Fehler zugewiesene Symbol. |
| osVersion       | string  | Eine der folgenden Zeichenfolgen, die die Version des Betriebssystems angibt, auf dem der Fehler aufgetreten ist:<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**Unbekannt**</li></ul>  |
| osRelease       | string  |  Eine der folgenden Zeichenfolgen, die die Betriebssystemversion oder den Verteilungsring (als eine Subpopulation innerhalb der Betriebssystemversion) angibt, in der bzw. dem der Fehler aufgetreten ist.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Preview-Version</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Servicepack 1</strong></li></ul><p>Wenn die Betriebssystemversion oder Verteilungsring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p>    |
| eventType       | string  | Eine der folgenden Zeichenfolgen:<ul><li>**Absturz**</li><li>**Einfrieren**</li><li>**Arbeitsspeicher**</li><li>**JSE**</li></ul>      |
| market          | string  | Der ISO 3166-Ländercode des Gerätemarkts.   |
| deviceType      | string  | Eine der folgenden Zeichenfolgen, die den Typ des Geräts anzeigt, auf dem der Fehler aufgetreten ist:<ul><li>**PC**</li><li>**Telefon**</li><li>**Verwaltungskonsole**</li><li>**IoT**</li><li>**Holographic**</li><li>**Unbekannt**</li></ul>    |
| packageName     | string  | Der eindeutige Name des App-Pakets, das mit diesem Fehler verknüpft ist.      |
| packageVersion  | string  | Die Version des App-Pakets, das mit diesem Fehler verknüpft ist.   |
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

* [Bericht zur Integrität](../publish/health-report.md)
* [Abrufen von Details für einen Fehler in Ihrer app](get-details-for-an-error-in-your-app.md)
* [Die stapelüberwachung für einen Fehler in Ihrer app abrufen](get-the-stack-trace-for-an-error-in-your-app.md)
* [Laden Sie die CAB-Datei für einen Fehler in Ihrer app](download-the-cab-file-for-an-error-in-your-app.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von app-Erwerb](get-app-acquisitions.md)
* [Add-On-Akquisitionen abrufen](get-in-app-acquisitions.md)
* [Abrufen von app-Bewertungen](get-app-ratings.md)
* [Abrufen von app-Bewertungen](get-app-reviews.md)
