---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen der app der monatlichen Nutzung zusammenfassen für einen bestimmten Zeitraum und anderen optionalen Filtern.
title: Abrufen der monatlichen App-Nutzung
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, Uwp, Store Services, Microsoft Store-Textanalyse-API, Nutzung
ms.localizationpriority: medium
ms.openlocfilehash: 48ad049b3f310f8b375a28d9695dd9280d686c43
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662925"
---
# <a name="get-monthly-app-usage"></a>Abrufen der monatlichen App-Nutzung

Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von Nutzungsdaten (nicht einschließlich Xbox Multiplayer-) im JSON-Format für eine Anwendung bei einem bestimmten Zeitraum (letzte 90 Tage nur) und andere optional Filter an. Diese Informationen sind auch verfügbar in der [Nutzungsbericht](../publish/usage-report.md) im Partner Center.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter     | Typ   |  Beschreibung                                                                                                    |  Erforderlich  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, für die Sie Rezensionsdaten abrufen möchten. |  Ja       |
| startDate     | date   | Das Startdatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.                   |  Nein        |
| endDate       | date   | Das Enddatum im Datumsbereich der Rezensionsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.                     |  Nein        |
| top           | int    | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können.                          |  Nein        |
| skip          | int    | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw.                         |  Nein        |  
| filter        |string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, der die Operatoren Eq "oder" Ne zugeordnet sind, und Anweisungen mit kombiniert werden können und bzw. oder. Zeichenfolgenwerte müssen in einfache Anführungszeichen im Parameter "Filter" gesetzt werden. Sie können die folgenden Felder aus dem Antworttext angeben: <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | Nein         |  
| orderby       | string | Eine Anweisung, die die Ergebnisdatenwerte anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Der Parameter <em>order</em> ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist **asc**.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein        |
| groupby       | string | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Nein        |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen der app der monatlichen Nutzung zusammenfassen. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die Nutzungsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle. |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Rezensionsdaten für die Abfrage gibt.                 |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                                                          |

 
### <a name="usage-values"></a>Ressourcenverwendung

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert                     | Typ    | Beschreibung                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | string  | Das erste Datum im Datumsbereich für die Verwendung von Daten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich.                          |
| applicationId             | string  | Die Store-ID der app für die Sie Nutzungsdaten abrufen.                            |
| applicationName           | string  | Der Anzeigename der App.                                                                |
| market                    | string  | Der ISO 3166-Ländercode des Markts, in denen Kunden Ihre app verwendet, werden soll.                   |
| packageVersion            | string  | Die Version des Pakets, in denen die Verwendung erfolgt ist.                                            |
| deviceType                | string  | Eine der folgenden Zeichenfolgen gibt an, dass der Typ des Geräts, in denen die Verwendung erfolgt ist:<ul><li>**PC**</li><li>**Phone**</li><li>**Verwaltungskonsole**</li><li>**Tablet**</li><li>**IoT**</li><li>**Server**</li><li>**Holographic**</li><li>**Unbekannt**</li></ul>                                                                                                                           |
| subscriptionName          | string  | Gibt an, ob es sich bei Verwendung über Xbox Game Pass erfolgte.                                              |
| monthlySessionCount       | lang    | Die Anzahl der benutzersitzungen für diesen Monat.                                              |
| engagementDurationMinutes | doppelt  | Die Minuten, in denen Benutzer aktiv mithilfe Ihrer app, gemessen an einer distinct Zeitspanne, die gestartet werden, während die app wird gestartet (Prozess starten) und endet, wenn es (Prozessende) beendet wird oder nach einem bestimmten Zeitraum der Inaktivität.                               |
| monthlyActiveUsers        | lang    | Die Anzahl der Kunden, die mithilfe der app diesen Monat.                                           |
| monthlyActiveDevices      | lang    | Die Anzahl von Geräten, die Ihre app ausgeführt wird, eine unterschiedliche längerer Zeit, gestartet werden, während die app wird gestartet (Prozess starten) und endet, wenn es sich bei (Prozessende) beendet wird oder nach einem Zeitraum der Inaktivität.                                                        |
| monthlyNewUsers           | lang    | Die Anzahl der Kunden, Ihre app zum ersten Mal diesen Monat verwendet, haben.                    |
| averageDailyActiveUsers   | doppelt  | Die durchschnittliche Anzahl von Kunden, die die app täglich verwenden.                             |
| averageDailyActiveDevices | doppelt  | Die durchschnittliche Anzahl von Geräten, die von allen Benutzern auf täglicher Basis für die Interaktion mit Ihrer app verwendet werden soll. |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Tägliche app Ussage abrufen](get-app-usage-daily.md)
* [Abrufen von app-Erwerb](get-app-acquisitions.md)
* [Add-On-Akquisitionen abrufen](get-in-app-acquisitions.md)
* [Abrufen von Daten für die Fehlerberichterstattung](get-error-reporting-data.md)
