---
description: Verwenden Sie diese REST-URI zum Abrufen von Daten für Block-Details für eine desktop-Anwendung bei einem bestimmten Zeitraum und andere optional Filter an.
title: Abrufen von Details zur Upgrade-Blockierung für die Desktopanwendung
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10 desktop-app-Blöcken, Windows-Desktop-Anwendung
localizationpriority: medium
ms.openlocfilehash: 66516c2bee58b3628542b66afcb0de24d0d9702b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600225"
---
# <a name="get-upgrade-block-details-for-your-desktop-application"></a>Abrufen von Details zur Upgrade-Blockierung für die Desktopanwendung

Verwenden Sie diese REST-URI, um für das Abrufen von Details für Windows 10-Geräte, auf denen eine bestimmte ausführbare Datei, in die desktop-Anwendung ein Windows 10-Upgrade ausgeführt wird blockiert. Sie können diesen URI verwenden, nur für desktop-Anwendungen, die Sie hinzugefügt haben die [Desktopanwendung, die Windows-Programm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Diese Informationen sind auch verfügbar in der [Anwendung blockiert Bericht](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) für desktop-Anwendungen im Partner Center.

Dieser URI ist ähnlich wie [Get Upgrade blockiert wird, für die desktop-Anwendung](get-desktop-block-data.md), Block Geräteinformationen für eine bestimmte ausführbare Datei in die desktop-Anwendung zurück.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails```|


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die Produkt-ID der Desktopanwendung, die für die Blockdaten abgerufen werden sollen. Rufen Sie die Produkt-ID, einer Desktopanwendung öffnen [Analytics zu senden, damit die desktop-Anwendung im Partner Center](https://msdn.microsoft.com/library/windows/desktop/mt826504) (z. B. die **blockiert Bericht**) und die Produkt-ID aus der URL abzurufen. |  Ja  |
| fileName | string | Der Name der ausführbaren Datei blockiert |
| startDate | date | Das Startdatum in den Datumsbereich für Blockdaten abgerufen werden soll. Der Standardwert ist 90 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum in den Datumsbereich für Blockdaten abgerufen werden soll. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li></ul> | Nein   |
| orderby | string | Eine Anweisung, die das Ergebnis von Datenwerten für jeden Block sortiert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eines der folgenden Felder aus dem Antworttext sein:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |
| groupby | string | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>deviceCount</strong></li></ul></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt mehrere Anforderungen zum Abrufen von desktop-Anwendung blockieren, dass Daten an. Ersetzen Sie die *ApplicationId* Wert mit der Produkt-ID für die desktop-Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die Daten aggregieren Sperren enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle. |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. B. zurückgegeben, wenn die **oben** Parameter der Anforderung ist auf 10.000 festgelegt, aber es gibt mehr als 10000 Zeilen von Daten mit Sperren für die Abfrage. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | Die Produkt-ID der Desktopanwendung, die für die Sie blockieren, dass Daten abgerufen werden soll. |
| date                | string | Das Datum, an dem Block Treffer Wert zugeordnet. |
| productName         | string | Der Anzeigename der Desktopanwendung, der aus den Metadaten der zugehörigen ausführbaren Datei(en) abgeleitet wurde. |
| fileName            | string | Die ausführbare Datei, die blockiert wurde. |
| applicationVersion  | string | Die Version der ausführbaren Datei der Anwendung, die blockiert wurde. |
| osVersion           | string | Eine der folgenden Zeichenfolgen, der die Betriebssystemversion angibt, auf der desktop-Anwendung, die derzeit ausgeführt wird:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unbekannt</strong></li></ul>   |
| osRelease           | string | Einer der folgenden Zeichenfolgen, die die Version des Betriebssystems oder der Test-flighting Ring (als eine Subpopulation in Version des Betriebssystems) angibt, auf dem desktop-Anwendung derzeit ausgeführt wird.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Preview-Version</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Servicepack 1</strong></li></ul><p>Wenn die Betriebssystemversion oder Verteilungsring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p> |
| market              | string | Der ISO 3166-Ländercode des Markts, in dem der desktop-Anwendung blockiert wird. |
| deviceType          | string | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, auf dem desktop-Anwendung blockiert wird:<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>Unbekannt</strong></li></ul> |
| blockType            | string | Eine der folgenden Zeichenfolgen, die den Typ des Blocks auf dem Gerät gefundenen angibt:<p/><ul><li><strong>Potenzielle Sediment</strong></li><li><strong>Temporäre Sediment</strong></li><li><strong>Laufzeitbenachrichtigungs</strong></li></ul><p/><p/>Weitere Informationen zu diesen Blocktypen und ihre Bedeutung für Entwickler und Benutzer finden Sie unter der Beschreibung des der [Anwendung blockiert Bericht](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report). |
| architecture        | string | Die Architektur des Geräts auf dem der Block vorhanden ist: <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | string | Eine der folgenden Zeichenfolgen, die Version des Windows 10-Betriebssystems angibt, auf der die Desktopanwendung Ausführung blockiert wird: <p/><ul><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li></ul> |
| deviceCount         | number | Die Anzahl von unterschiedlichen Geräten, die auf der Ebene des angegebenen Aggregation Blöcke enthalten. |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockdetails?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Windows-Desktop-Anwendung](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Rufen Sie ein Upgrade blockiert für die desktop-Anwendung](get-desktop-block-data.md)
