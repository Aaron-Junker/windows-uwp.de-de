---
description: Verwenden Sie diesen Rest-URI, um in einem bestimmten Datumsbereich und anderen optionalen Filtern Block Detaildaten für eine Desktop Anwendung zu erhalten.
title: Aktualisierungs Block Details für Ihre APP erhalten
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10, Desktop-App-Blöcke, Windows-Desktop-Anwendungsprogramm
localizationpriority: medium
ms.openlocfilehash: b30594ff3943d1ed9183190a0b5a43824816c080
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735115"
---
# <a name="get-upgrade-block-details-for-your-desktop-application"></a>Abrufen von Details zur Upgradeblockierung für Ihre Desktopanwendung

Verwenden Sie diesen Rest-URI, um Details für Windows 10-Geräte zu erhalten, auf denen eine bestimmte ausführbare Datei in der Desktop Anwendung die Ausführung eines Windows 10-Upgrades blockiert. Sie können diesen URI nur für Desktop Anwendungen verwenden, die Sie dem [Windows-Desktop Anwendungsprogramm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)hinzugefügt haben. Diese Informationen sind auch im Bericht " [Anwendungs Blöcke](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) " für Desktop Anwendungen in Partner Center verfügbar.

Dieser URI ähnelt dem [erzielen von upgradeblöcken für Ihre Desktop Anwendung](get-desktop-block-data.md), gibt jedoch Geräte Block Informationen für eine bestimmte ausführbare Datei in der Desktop Anwendung zurück.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails```|


### <a name="request-header"></a>Anforderungsheader

| Kopfzeile        | Geben Sie in das Suchfeld auf der Taskleiste   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD Zugriffs Token **im Formular** Bearer&lt;*Token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Geben Sie in das Suchfeld auf der Taskleiste   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | String | Die Produkt-ID der Desktop Anwendung, für die Sie Blockdaten abrufen möchten. Wenn Sie die Produkt-ID einer Desktop Anwendung abrufen möchten, öffnen Sie einen beliebigen [Analysebericht für Ihre Desktop Anwendung im Partner Center](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (z. b. den **Bericht "Blöcke**"), und rufen Sie die Produkt-ID aus der URL ab. |  „Ja“  |
| fileName | String | Der Name der gesperrten ausführbaren Datei. |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Blockdaten. Der Standardwert ist 90 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich von Blockdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Wenn die Abfrage keine weiteren Zeilen enthält, entält der Antworttext den Link „Weiter“, den Sie verwenden können, um die nächste Seite mit Daten anzufordern. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | String  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>Architektur</strong></li><li><strong>blockType</strong></li><li><strong>den DeviceType "</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>ProductName</strong></li><li><strong>targetOs</strong></li></ul> | Nein   |
| orderby | String | Eine-Anweisung, die die Ergebnisdaten Werte für jeden Block anordnet. Die Syntax lautet <em>orderby=field [order],field [order],...</em>, wobei der Parameter <em>field</em> eines der folgenden Felder aus dem Antworttext sein kann:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>Architektur</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>den DeviceType "</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>ProductName</strong></li><li><strong>targetOs</strong></li><li><strong>devicecount</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |
| groupby | String | Eine Anweisung, die Datenaggregationen nur auf die angegebenen Felder anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>Architektur</strong></li><li><strong>blockType</strong></li><li><strong>den DeviceType "</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>ApplicationId</strong></li><li><strong>date</strong></li><li><strong>ProductName</strong></li><li><strong>devicecount</strong></li></ul></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel werden mehrere Anforderungen zum erhalten von Daten für Desktop Anwendungs Blöcke veranschaulicht. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID für Ihre Desktop Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Antworttext

| Value      | Geben Sie in das Suchfeld auf der Taskleiste   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Value      | Array  | Ein Array von-Objekten, die Aggregat Blockdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle. |
| @nextLink  | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10000 festgelegt ist, aber mehr als 10000 Zeilen mit Blockdaten für die Abfrage vorhanden sind. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |


Elemente im Array *Value* enthalten die folgenden Werte.

| Value               | Geben Sie in das Suchfeld auf der Taskleiste   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | String | Die Produkt-ID der Desktop Anwendung, für die Sie Blockdaten abgerufen haben. |
| date                | String | Das Datum, das dem Block Treffer Wert zugeordnet ist. |
| productName         | String | Der Anzeigename der Desktopanwendung, der aus den Metadaten der zugehörigen ausführbaren Datei(en) abgeleitet wurde. |
| fileName            | String | Die ausführbare Datei, die blockiert wurde. |
| applicationVersion  | String | Die Version der ausführbaren Datei der Anwendung, die blockiert wurde. |
| osVersion           | String | Eine der folgenden Zeichen folgen, die die Betriebssystemversion angibt, auf der die Desktop Anwendung derzeit ausgeführt wird:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unbekannter</strong></li></ul>   |
| osRelease           | String | Eine der folgenden Zeichen folgen, die das Betriebssystem Release oder den flighting-Ring (als untergeordnete Population innerhalb der Betriebssystemversion) angibt, auf dem die Desktop Anwendung derzeit ausgeführt wird.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Releasevorschau</strong></li><li><strong>Insider fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Wenn die Betriebssystemversion oder Verteilungsring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p> |
| market              | String | Der ISO 3166-Ländercode des Markts, auf dem die Desktop Anwendung blockiert wird. |
| deviceType          | String | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem die Desktop Anwendung blockiert ist:<p/><ul><li><strong>PCs</strong></li><li><strong>Servers</strong></li><li><strong>Tablet</strong></li><li><strong>Unbekannter</strong></li></ul> |
| blockType            | String | Eine der folgenden Zeichen folgen, die den Typ des auf dem Gerät gefundenen Blocks angibt:<p/><ul><li><strong>Potenzieller Sediment</strong></li><li><strong>Temporäres Sediment</strong></li><li><strong>Lauf Zeit Benachrichtigung</strong></li></ul><p/><p/>Weitere Informationen zu diesen Blocktypen und deren Bedeutung für Entwickler und Benutzer finden Sie in der Beschreibung des Berichts zu [Anwendungs Blöcken](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report). |
| architecture        | String | Die Architektur des Geräts, auf dem der Block vorhanden ist: <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | String | Eine der folgenden Zeichen folgen, die die Version des Windows 10-Betriebssystems angibt, auf der die Ausführung der Desktop Anwendung blockiert ist: <p/><ul><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li></ul> |
| deviceCount         | number | Die Anzahl der unterschiedlichen Geräte, die Blöcke auf der angegebenen Aggregations Ebene aufweisen. |


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

* [Windows-Desktop Anwendungsprogramm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Aktualisieren Sie upgradeblöcke für Ihre Desktop Anwendung.](get-desktop-block-data.md)
