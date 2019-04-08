---
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um aggregierte Konversionen nach Kanal für ein Add-On während eines bestimmten Zeitraums und andere optionale Filter abzurufen.
title: Abrufen von Add-On-Konvertierungen nach Kanal
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Add-On-Konversionen, Kanäle
ms.localizationpriority: medium
ms.openlocfilehash: 1b1cbc33b2ce53ea7f851e78433b74b103e5a035
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603895"
---
# <a name="get-add-on-conversions-by-channel"></a>Abrufen von Add-On-Konvertierungen nach Kanal

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um aggregierte Konversionen nach Kanal für ein Add-On während eines bestimmten Zeitraums und andere optionale Filter abzurufen.

* *Konvertierung* bedeutet, dass ein Kunde (der mit einem Microsoft-Konto angemeldet ist) eine Lizenz für Ihr Add-On (entweder für eine kostenpflichtige oder eine kostenlose App) neu erworben hat.
* Der *Channel* ist die Methode, über die ein Kunde auf der Eintragsseite Ihrer App gelangt ist (z. B. über den Store oder einer [Werbekampagne für benutzerdefinierte Apps](../publish/create-a-custom-app-promotion-campaign.md)).

Diese Informationen sind auch verfügbar in der [-Add-On Akquisitionen Bericht](../publish/add-on-acquisitions-report.md#add-on-page-views-and-conversions-by-campaign-id) im Partner Center.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, für die Sie die Add-On-Konversionsdaten abrufen möchten. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| inAppProductId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Add-Ons, für das Sie Konversionsdaten abrufen möchten.  | Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Konversionsdaten, die abgerufen werden sollen. Der Standardwert ist 1/1/2016 |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Konversionsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen im Antworttext filtert. Alle Anweisungen können die Operatoren **eq** oder **ne** verwenden. Zudem können sie mit **and** oder **or** kombiniert werden. Sie können die folgenden Zeichenfolgen in der Filteranweisung angeben.  Beschreibung finden Sie im [Konvertierungswerte](#conversion-values) Abschnitt in diesem Artikel. <ul><li><strong>ApplicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>"DeviceType"</strong></li><li><strong>Markt</strong></li></ul><p>Hier ist ein Beispiel *Filter*-Parameter: <em>filter=deviceType eq 'PC'</em>.</p> | Nein   |
| aggregationLevel | string | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>. | Nein |
| orderby | string | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Konversionen anfordert. Die Syntax ist <em>orderby=field [order],field [order],...</em>. Der Parameter <em>field</em> kann eine der folgenden Zeichenfolgen sein:<ul><li><strong>Datum</strong></li><li><strong>ApplicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>"DeviceType"</strong></li><li><strong>Markt</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist <strong>asc</strong>.</p><p>Dies ist eine Beispielzeichenfolge für <em>orderby</em>: <em>orderby=date,market</em></p> |  Nein  |
| groupby | string | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:<p/><ul><li><strong>Datum</strong></li><li><strong>ApplicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>"DeviceType"</strong></li><li><strong>Markt</strong></li></ul><p>Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter <em>groupby</em> angegeben sind, sowie die folgenden:</p><ul><li><strong>Datum</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>inAppProductName</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em>groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt verschiedene Anforderungen für den Abruf von Konversionsdaten für Apps. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die aggregierte Konversionsdaten für das Add-On enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Konversionswerte](#conversion-values).                     |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10 festgelegt ist, es jedoch mehr als 10 Zeilen mit Konversionsdaten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                                                                                                                                                                                                             |

### <a name="conversion-values"></a>Konversionswerte

Objekte im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Das erste Datum im Datumsbereich für die Konversionsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| inAppProductId      | string  | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Add-Ons, für das Sie Konversionsdaten abrufen.     |
| inAppProductName    | string  | Der Anzeigename des Add-Ons, für das Sie Konversionsdaten abrufen.   |
| applicationId       | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, für die Sie Konversionsdaten abrufen.     |
| applicationName     | string | Der Anzeigename der App, für das Sie Konversionsdaten abrufen.        |
| appType          | string |  Die Type ID des Produkts, für die Sie Konversionsdaten abrufen. Für diese Methode wird nur der Wert **Add-On** unterstützt.            |
| customCampaignId           | string |  Die ID-Zeichenfolge für einen [Werbekampagne für benutzerdefinierte Apps](../publish/create-a-custom-app-promotion-campaign.md), die der App zugeordnet ist.   |
| referrerUriDomain           | string |  Gibt die Domäne, in denen die App-Eintrag mit der benutzerdefinierten App Promotion Kampagnen-ID aktiviert wurde.   |
| channelType           | string |  Einer der folgenden Zeichenfolgen, der den Kanal für die Konversion angibt:<ul><li><strong>customCampaignId</strong></li><li><strong>Store-Datenverkehr</strong></li><li><strong>Andere</strong></li></ul>    |
| storeClient         | string | Die Version des Store, in dem der Konversion erfolgte. Derzeit wird als einziger Wert **SFC** unterstützt.    |
| deviceType          | string | Eine der folgenden Zeichenfolgen:<ul><li><strong>PC</strong></li><li><strong>Telefon</strong></li><li><strong>Verwaltungskonsole</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul>            |
| market              | string | Die ISO 3166-Ländercode des Markts, in dem die Konversion erfolgte.    |
| clickCount              | number  |     Die Anzahl der Kundenklicks auf den Link zum App-Eintrag.      |           
| conversionCount            | number  |   Die Anzahl der Kundenkonvertierungen.         |         


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso Add-On",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "Add-On",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "CN",
      "clickCount": 1,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht zu Add-On-Käufen](../publish/add-on-acquisitions-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Add-On-Akquisitionen abrufen](get-in-app-acquisitions.md)
