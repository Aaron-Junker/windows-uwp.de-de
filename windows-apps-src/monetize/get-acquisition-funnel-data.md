---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Erfassungs Trichter Daten für eine Anwendung in einem bestimmten Datumsbereich und anderen optionalen Filtern zu erhalten.
title: Abrufen von App-Erwerbstrichterdaten
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Erwerb, Trichter
ms.localizationpriority: medium
ms.openlocfilehash: 817ad85bae642d6b09cf77bb5e7b4bf71b08f594
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493555"
---
# <a name="get-app-acquisition-funnel-data"></a>Abrufen von App-Erwerbstrichterdaten

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Erfassungs Trichter Daten für eine Anwendung in einem bestimmten Datumsbereich und anderen optionalen Filtern zu erhalten. Diese Informationen finden Sie auch im Partner Center im [Bericht über Käufe](../publish/acquisitions-report.md#acquisition-funnel) .

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Erfassungs Trichter Daten abrufen möchten. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Abruf Trichter Daten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Abruf Trichter Daten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Weitere Informationen finden Sie unten im Abschnitt [Filterfelder](#filter-fields). | Nein   |

 
### <a name="filter-fields"></a>Filterfelder

Der Parameter *filter* der Anforderung enthält mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält ein Feld und einen Wert, das/der mit den Operatoren **eq** oder **ne** verknüpft ist. Anweisungen können mit **and** oder **or** kombiniert werden.

Die folgenden Filter Felder werden unterstützt. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden.

| Felder        |  BESCHREIBUNG        |
|---------------|-----------------|
| campaignId | Die ID-Zeichenfolge für eine [benutzerdefinierte App-Promotionkampagne](../publish/create-a-custom-app-promotion-campaign.md) , die mit dem Erwerb verknüpft ist. |
| market | Eine Zeichenfolge, die den ISO 3166-Ländercode des Markts enthält, in dem der Kauf erfolgte. |
| deviceType | Eine der folgenden Zeichen folgen, die den Gerätetyp angibt, auf dem der Erwerb erfolgt ist:<ul><li><strong>PC</strong></li><li><strong>Smartphone</strong></li><li><strong>Konsole-Xbox One</strong></li><li><strong>Konsole-Xbox Series X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unbekannt</strong></li></ul> |
| ageGroup | Eine der folgenden Zeichen folgen, die die Altersgruppe des Benutzers angibt, der die Übernahme abgeschlossen hat:<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 oder mehr</strong></li><li><strong>Unbekannt</strong></li></ul> |
| gender | Eine der folgenden Zeichen folgen, die das Geschlecht des Benutzers angibt, der die Übernahme abgeschlossen hat:<ul><li><strong>M</strong></li><li><strong>C</strong></li><li><strong>Unbekannt</strong></li></ul> |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel werden mehrere Anforderungen zum Abrufen von Erfassungsdaten für eine APP veranschaulicht. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Erfassungs Trichter Daten für die APP enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie im Abschnitt [Trichter Werte](#funnel-values) weiter unten.                  |
| TotalCount | INT    | Die Gesamtanzahl der Objekte im *Wert* Array.        |


### <a name="funnel-values"></a>Trichter Werte

Objekte im *Wertarray* enthalten die folgenden Werte.

| Wert               | type   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| Metrictype                | Zeichenfolge | Eine der folgenden Zeichen folgen, die den [Typ der Trichter Daten](../publish/acquisitions-report.md#acquisition-funnel) angibt, der in diesem-Objekt enthalten ist:<ul><li><strong>PageView</strong></li><li><strong>Erwerb</strong></li><li><strong>Installieren</strong></li><li><strong>Verwendung</strong></li></ul> |
| UserCount       | Zeichenfolge | Die Anzahl der Benutzer, die den durch den Wert *metrictype* angegebenen Trichter Schritt ausgeführt haben.             |


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
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von App-Käufen](get-app-acquisitions.md)
