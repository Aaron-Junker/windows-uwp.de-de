---
ms.assetid: 99DB5622-3700-4FB2-803B-DA447A1FD7B7
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um die täglichen Nutzungsdaten für die APP für einen bestimmten Datumsbereich und andere optionale Filter zu erhalten.
title: Abrufen der täglichen App-Nutzung
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Verwendung
ms.localizationpriority: medium
ms.openlocfilehash: 4ddc741d13568ba6860902ebc5b6e375ec43d369
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493335"
---
# <a name="get-daily-app-usage"></a>Abrufen der täglichen App-Nutzung

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Verwendungs Daten (ohne Xbox-Multiplayer) im JSON-Format für eine Anwendung in einem bestimmten Datumsbereich (nur letzte 90 Tage) und anderen optionalen Filtern zu erhalten. Diese Informationen sind auch im [Nutzungsbericht](../publish/usage-report.md) von Partner Center verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                               |
|--------|---------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter     | type   |  BESCHREIBUNG                                                                                                    |  Erforderlich  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Überprüfungs Daten abrufen möchten. |  Ja       |
| startDate     | date   | Das Startdatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt.                   |  Nein        |
| endDate       | date   | Das Enddatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt.                     |  Nein        |
| top           | INT    | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können.                          |  Nein        |
| skip          | INT    | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw.                         |  Nein        |  
| filter        |Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den EQ-oder ne-Operatoren zugeordnet sind, und-Anweisungen können mithilfe von and oder or kombiniert werden. Zeichenfolgenwerte im Parameter filter müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben: <ul><li>**Marktforschungs**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | Nein         |  
| orderby       | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eine der folgenden Zeichen folgen sein:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**Marktforschungs**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**dailysessioncount**</li><li>**engagementdurationminutes**</li><li>**dailyactiveusers**</li><li>**dailyactivedevices**</li><li>**dailynewusers**</li><li>**monthlyactiveusers**</li><li>**monthlyactivedevices**</li><li>**monthlynewusers**</li></ul><p>Der Parameter <em>order</em> ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist **ASC**.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, Market</em></p>                                                                                                   |  Nein        |
| groupby       | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben: <ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**Marktforschungs**</li><li>**date**</li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**dailysessioncount**</li><li>**engagementdurationminutes**</li><li>**dailyactiveusers**</li><li>**dailyactivedevices**</li><li>**dailynewusers**</li><li>**monthlyactiveusers**</li><li>**monthlyactivedevices**</li><li>**monthlynewusers**</li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em> &amp; GroupBy = AgeGroup, Market &amp; aggregationlevel = Week</em></p>                                                                                                             |  Nein        |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Anfordern täglicher App-Verwendungs Daten. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily?applicationId=XXXXXXXXXXXX&startDate=2018-08-10&endDate=2018-08-14 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort



### <a name="response-body"></a>Antworttext

| Wert      | type   | BESCHREIBUNG                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Aggregat Verwendungs Daten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle. |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Rezensionsdaten für die Abfrage gibt.                 |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                                                          |

 
### <a name="usage-values"></a>Verwendungs Werte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert                     | type    | BESCHREIBUNG                                                               |
|---------------------------|---------|---------------------------------------------------------------------------|
| date                      | Zeichenfolge  | Das erste Datum im Datumsbereich der Verwendungs Daten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich.        |
| applicationId             | Zeichenfolge  | Die Speicher-ID der APP, für die Sie Verwendungs Daten abrufen.          |
| applicationName           | Zeichenfolge  | Der Anzeigename der App.                                              |
| deviceType                | Zeichenfolge  | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem die Verwendung aufgetreten ist:<ul><li>**PC**</li><li>**Smartphone**</li><li>**Konsole-Xbox One**</li><li>**Konsole-Xbox Series X**</li><li>**Tablet**</li><li>**IoT**</li><li>**Server**</li><li>**Holographic**</li><li>**Unbekannt**</li></ul>                                                                                                         |
| packageVersion            | Zeichenfolge  | Die Version des Pakets, in dem die Verwendung aufgetreten ist.                          |
| market                    | Zeichenfolge  | Der ISO 3166-Ländercode des Markts, an dem der Kunde Ihre APP verwendet hat. |
| subscriptionName          | Zeichenfolge  | Gibt an, ob die Verwendung über das Xbox-Spiel bestanden wurde.                            |
| dailysessioncount         | long    | Die Anzahl der Benutzersitzungen an diesem Tag.                                  |
| engagementdurationminutes | double  | Die Minuten, in denen Benutzer Ihre APP aktiv mit einem bestimmten Zeitraum messen, beginnend beim Starten der APP (Prozessstart) und beenden, wenn Sie beendet wird (Prozess Ende) oder nach einer gewissen Zeit der Inaktivität.             |
| dailyactiveusers          | long    | Die Anzahl der Kunden, die die APP an einem Tag verwenden.                           |
| dailyactivedevices        | long    | Die Anzahl der täglichen Geräte, die von allen Benutzern für die Interaktion mit Ihrer APP verwendet werden.  |
| dailynewusers             | long    | Die Anzahl der Kunden, die Ihre APP zum ersten Mal verwendet haben.    |
| monthlyactiveusers        | long    | Die Anzahl der Kunden, die die app in diesem Monat nutzen.                         |
| monthlyactivedevices      | long    | Die Anzahl der Geräte, die Ihre APP für einen bestimmten Zeitraum ausführen, beginnend mit dem Start der APP (Prozessstart) und dem Beenden, wenn Sie beendet wird (Prozess Ende) oder nach einem Zeitraum der Inaktivität.                                      |
| monthlynewusers           | long    | Die Anzahl der Kunden, die Ihre APP zum ersten Mal im Monat verwendet haben.  |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2018-08-10",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 17718,
      "engagementDurationMinutes": 582540.2,
      "dailyActiveUsers": 7078,
      "dailyActiveDevices": 6964,
      "dailyNewUsers": 1727,
      "monthlyActiveUsers": 123609,
      "monthlyActiveDevices": 116723,
      "monthlyNewUsers": 72271
    },
    {
      "date": "2018-08-11",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 19899,
      "engagementDurationMinutes": 655379.7,
      "dailyActiveUsers": 7877,
      "dailyActiveDevices": 7789,
      "dailyNewUsers": 2062,
      "monthlyActiveUsers": 123666,
      "monthlyActiveDevices": 116770,
      "monthlyNewUsers": 72276
    },
    {
      "date": "2018-08-12",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 21349,
      "engagementDurationMinutes": 735640.5,
      "dailyActiveUsers": 8456,
      "dailyActiveDevices": 8309,
      "dailyNewUsers": 2433,
      "monthlyActiveUsers": 124241,
      "monthlyActiveDevices": 117289,
      "monthlyNewUsers": 72732
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Monatliche App-ussage erhalten](get-app-usage-monthly.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
* [Abrufen von Add-On-Käufen](get-in-app-acquisitions.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
