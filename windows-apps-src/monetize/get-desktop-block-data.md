---
description: Verwenden Sie diesen Rest-URI, um in einem bestimmten Datumsbereich und anderen optionalen Filtern Blockdaten für eine Desktop Anwendung zu erhalten.
title: Abrufen von Upgradeblockierungen für Ihre Desktopanwendung
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10, Desktop-App-Blöcke, Windows-Desktop-Anwendungsprogramm
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 65cb156d313f398517e174b12520411d5586e22c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158554"
---
# <a name="get-upgrade-blocks-for-your-desktop-application"></a>Abrufen von Upgradeblockierungen für Ihre Desktopanwendung

Mit diesem Rest-URI erhalten Sie Informationen zu Windows 10-Geräten, auf denen Ihre Desktop Anwendung das Ausführen eines Windows 10-Upgrades blockiert. Sie können diesen URI nur für Desktop Anwendungen verwenden, die Sie dem [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)hinzugefügt haben. Diese Informationen sind auch im Bericht " [Anwendungs Blöcke](/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) " für Desktop Anwendungen in Partner Center verfügbar.

Informationen zu Geräte Blöcken für eine bestimmte ausführbare Datei in der Desktop Anwendung finden Sie unter [Get Upgrade Block Details for your Desktop Application](get-desktop-block-data-details.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits```|


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die Produkt-ID der Desktop Anwendung, für die Sie Blockdaten abrufen möchten. Wenn Sie die Produkt-ID einer Desktop Anwendung abrufen möchten, öffnen Sie einen beliebigen [Analysebericht für Ihre Desktop Anwendung im Partner Center](/windows/desktop/appxpkg/windows-desktop-application-program) (z. b. den **Bericht "Blöcke**"), und rufen Sie die Produkt-ID aus der URL ab. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Blockdaten. Der Standardwert ist 90 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich von Blockdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>den DeviceType "</strong></li><li><strong>fileName</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>ProductName</strong></li><li><strong>targetOs</strong></li></ul> | Nein   |
| orderby | Zeichenfolge | Eine-Anweisung, die die Ergebnisdaten Werte für jeden Block anordnet. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eines der folgenden Felder aus dem Antworttext sein:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>den DeviceType "</strong></li><li><strong>fileName</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>ProductName</strong></li><li><strong>targetOs</strong></li><li><strong>devicecount</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p> |  Nein  |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>den DeviceType "</strong></li><li><strong>fileName</strong></li><li><strong>Marktforschungs</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>ProductName</strong></li><li><strong>devicecount</strong></li></ul></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel werden mehrere Anforderungen zum erhalten von Daten für Desktop Anwendungs Blöcke veranschaulicht. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID für Ihre Desktop Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Aggregat Blockdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle. |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10000 festgelegt ist, aber mehr als 10000 Zeilen mit Blockdaten für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | Zeichenfolge | Die Produkt-ID der Desktop Anwendung, für die Sie Blockdaten abgerufen haben. |
| date                | Zeichenfolge | Das Datum, das dem Block Treffer Wert zugeordnet ist. |
| ProductName         | Zeichenfolge | Der Anzeige Name der Desktop Anwendung, die von den Metadaten der zugehörigen ausführbaren Dateien abgeleitet ist. |
| fileName            | Zeichenfolge | Die ausführbare Datei, die blockiert wurde. |
| applicationVersion  | Zeichenfolge | Die Version der ausführbaren Datei der Anwendung, die blockiert wurde. |
| osVersion           | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Betriebssystemversion angibt, auf der die Desktop Anwendung derzeit ausgeführt wird:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unbekannt</strong></li></ul>   |
| osRelease           | Zeichenfolge | Eine der folgenden Zeichen folgen, die das Betriebssystem Release oder den flighting-Ring (als untergeordnete Population innerhalb der Betriebssystemversion) angibt, auf dem die Desktop Anwendung derzeit ausgeführt wird.<p/><p>Für Windows 10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Releasevorschau</strong></li><li><strong>Insider fast</strong></li><li><strong>Insider langsam</strong></li></ul><p/><p>Für Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Für Windows Server 2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Für Windows 8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Für Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Wenn das Betriebssystem Release oder der flighting-Ring unbekannt ist, hat dieses Feld den Wert <strong>Unknown</strong>.</p> |
| market              | Zeichenfolge | Der ISO 3166-Ländercode des Markts, auf dem die Desktop Anwendung blockiert wird. |
| deviceType          | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem die Desktop Anwendung blockiert ist:<p/><ul><li><strong>PCs</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>Unbekannt</strong></li></ul> |
| blockType            | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des auf dem Gerät gefundenen Blocks angibt:<p/><ul><li><strong>Potenzieller Sediment</strong></li><li><strong>Temporäres Sediment</strong></li><li><strong>Lauf Zeit Benachrichtigung</strong></li></ul><p/><p/>Weitere Informationen zu diesen Blocktypen und deren Bedeutung für Entwickler und Benutzer finden Sie in der Beschreibung des Berichts zu [Anwendungs Blöcken](/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report).  |
| Architektur        | Zeichenfolge | Die Architektur des Geräts, auf dem der Block vorhanden ist: <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Version des Windows 10-Betriebssystems angibt, auf der die Ausführung der Desktop Anwendung blockiert ist: <p/><ul><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li></ul> |
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
  "@nextLink": "desktop/blockhits?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)
* [Abrufen von Details zur Upgradeblockierung für Ihre Desktopanwendung](get-desktop-block-data-details.md)