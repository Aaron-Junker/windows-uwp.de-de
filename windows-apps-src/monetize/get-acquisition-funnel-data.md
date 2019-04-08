---
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die Erwerbstrichterdaten für eine Anwendung während eines bestimmten Zeitraums und andere optionale Filter abzurufen.
title: Abrufen von App-Erwerbstrichterdaten
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Kauf, Trichter
ms.localizationpriority: medium
ms.openlocfilehash: d9ccbb081ef6f39ad795105ee2449de4d8442ab3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646195"
---
# <a name="get-app-acquisition-funnel-data"></a>Abrufen von App-Erwerbstrichterdaten

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die Erwerbstrichterdaten für eine Anwendung während eines bestimmten Zeitraums und andere optionale Filter abzurufen. Diese Informationen sind auch verfügbar in der [Akquisitionen Bericht](../publish/acquisitions-report.md#acquisition-funnel) im Partner Center.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, für die Sie die Erwerbstrichterdaten abrufen möchten. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Erwerbstrichterdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Erwerbstrichterdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Weitere Informationen finden Sie unten im Abschnitt [Filterfelder](#filter-fields). | Nein   |

 
### <a name="filter-fields"></a>Filterfelder

Der Parameter *filter* der Anforderung enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Anweisungen können mit **and** oder **or** kombiniert werden.

Die folgenden Filterfelder werden unterstützt. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden.

| Felder        |  Beschreibung        |
|---------------|-----------------|
| campaignId | Die ID-Zeichenfolge für einen [Werbekampagne für benutzerdefinierte Apps](../publish/create-a-custom-app-promotion-campaign.md), die dem Kauf zugeordnet ist. |
| market | Eine Zeichenfolge, die den ISO 3166-Ländercode des Markts enthält, in dem der Kauf erfolgte. |
| deviceType | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, auf der Kauf aufgetreten ist:<ul><li><strong>PC</strong></li><li><strong>Telefon</strong></li><li><strong>Verwaltungskonsole</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul> |
| ageGroup | Eine der folgenden Zeichenfolgen, die den Typ der Age-Group des Benutzer angibt, der den Kauf abgeschlossen hat:<ul><li><strong>0 – 17</strong></li><li><strong>18 bis 24</strong></li><li><strong>25: 34</strong></li><li><strong>35: 49</strong></li><li><strong>50 oder höher</strong></li><li><strong>Unbekannt</strong></li></ul> |
| gender | Eine der folgenden Zeichenfolgen, die das Geschlecht des Benutzer angibt, der den Kauf abgeschlossen hat:<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>Unbekannt</strong></li></ul> |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt verschiedene Anforderungen für den Abruf von Erwerbstrichterdaten für Apps. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die Erwerbstrichterdaten für die App enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Trichterwerte](#funnel-values).                  |
| TotalCount | int    | Die Gesamtanzahl der Objekte im *Value* Array.        |


### <a name="funnel-values"></a>Trichterwerte

Objekte im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | Eine der folgenden Zeichenfolgen, die angibt, die [Trichter Datentyp](../publish/acquisitions-report.md#acquisition-funnel), die in diesem Objekt enthalten ist:<ul><li><strong>PageView</strong></li><li><strong>Erwerb</strong></li><li><strong>Installieren</strong></li><li><strong>Verwendung</strong></li></ul> |
| UserCount       | string | Die Anzahl der Benutzer, die vom angegebenen Trichter Schritt ausgeführt hat, die *MetricType* Wert.             |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Bericht „Käufe“](../publish/acquisitions-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von app-Erwerb](get-app-acquisitions.md)
