---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von Insights-Daten für Ihre app ein.
title: Abrufen von Insights-Daten
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, Uwp, Store Services, Microsoft Store-Textanalyse-API, Einblicke
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1847f22f52eb066115b5681e745e74ec74f77f7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662845"
---
# <a name="get-insights-data"></a>Abrufen von Insights-Daten

Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von Insights-Daten bei einem bestimmten Zeitraum und andere optionale Filter Übernahmen, Integrität und nutzungsmetriken für eine app im Zusammenhang. Diese Informationen sind auch verfügbar in der [Insights-Bericht](../publish/insights-report.md) im Partner Center.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store ID](in-app-purchases-and-trials.md#store-ids) der app für das Insights-Daten abgerufen werden sollen. Wenn Sie diesen Parameter nicht angeben, enthält der Antworttext die Insights-Daten für alle apps, die mit Ihrem Konto registriert.  |  Nein  |
| startDate | date | Das Startdatum in den Datumsbereich für Insights-Daten abgerufen werden soll. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum in den Datumsbereich für Insights-Daten abgerufen werden soll. Der Standardwert ist das aktuelle Datum. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Z. B. *Filter = DataType-Eq "Übernahme"*. <p/><p/>Sie können die folgenden Filterfelder angeben:<p/><ul><li><strong>acquisition</strong></li><li><strong>health</strong></li><li><strong>usage</strong></li></ul> | Nein   |

### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von Insights-Daten. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die für die app Insights-Daten enthalten. Weitere Informationen zu den Daten in jedem Objekt finden Sie unter der [Insight Werte](#insight-values) Abschnitt weiter unten.                                                                                                                      |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                 |


### <a name="insight-values"></a>Insight-Werte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | Die Store-ID der app für die Sie Insights-Daten abgerufen werden.     |
| insightDate                | string | Das Datum, an dem die Änderung in einer bestimmten Metrik erkannt wurden. Dieses Datum stellt das Ende der Woche, die in der wir eine erhebliche Leistungssteigerung erkannt oder verringern Sie in einer Metrik im Vergleich zu einer Woche davor. |
| dataType     | string | Eine der folgenden Zeichenfolgen, die den Bereich Allgemeine Analytics gibt an, dem diese Information beschreibt:<p/><ul><li><strong>acquisition</strong></li><li><strong>health</strong></li><li><strong>usage</strong></li></ul>   |
| insightDetail          | array | Eine oder mehrere [InsightDetail Werte](#insightdetail-values) , die die Details für den aktuellen Insight darstellen.    |


### <a name="insightdetail-values"></a>InsightDetail Werte

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Die folgenden Werte, die die Metrik gibt an, die den aktuellen Insight oder die aktuelle Dimension beschreibt, auf der Grundlage der **DataType** Wert.<ul><li>Für **Integrität**, dieser Wert ist immer **Trefferanzahl**.</li><li>Für **Übernahme**, dieser Wert ist immer **AcquisitionQuantity**.</li><li>Für **Nutzung**, dieser Wert kann eine der folgenden Zeichenfolgen:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  Ein oder mehrere Objekte, die eine einzelne Metrik für die Einblicke zu beschreiben.   |
| PercentChange            | string |  Der Prozentsatz, den die Metrik für Ihre gesamte Kundenbasis geändert.  |
| DimensionName           | string |  Der Name der Metrik in der aktuellen Dimension beschrieben. Beispiele hierfür sind **EventType**, **Markt**, **"DeviceType"**, **PackageVersion**, **AcquisitionType**, **Altersgruppe** und **Geschlecht**.   |
| DimensionValue              | string | Der Wert der Metrik, die in der aktuellen Dimension beschrieben wird. Z. B. wenn **DimensionName** ist **EventType**, **DimensionValue** möglicherweise **Absturz** oder **hängen** .   |
| FactValue     | string | Der Absolute Wert der Metrik auf das Datum, an das der Einblick erkannt wurde.  |
| Direction | string |  Die Richtung der Änderung (**Positive** oder **Negative**).   |
| Datum              | string |  Das Datum, an dem die Änderung im Zusammenhang mit der aktuellen Einblick oder die aktuelle Dimension erkannt wurden.   |

### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Insights-Bericht](../publish/insights-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
