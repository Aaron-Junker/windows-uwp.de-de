---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Insights-Daten für Ihre APP zu erhalten.
title: Insights-Daten erhalten
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Einblicke
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cebd415b00268c9a5e8febbe175347345d830c23
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349720"
---
# <a name="get-insights-data"></a>Insights-Daten erhalten

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um in einem bestimmten Datumsbereich und anderen optionalen Filtern Einblicke in Bezug auf die Übernahmen, Integritäts-und nutzungsmetriken für eine APP zu erhalten. Diese Informationen sind auch im Insights- [Bericht](../publish/insights-report.md) im Partner Center verfügbar.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Insights-Daten abrufen möchten. Wenn Sie diesen Parameter nicht angeben, enthält der Antworttext Insights-Daten für alle apps, die für Ihr Konto registriert sind.  |  Nein  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Insights-Daten. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der abzurufenden Insights-Daten. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Beispiel: *Filter = DataType EQ ' Acquisition '*. <p/><p/>Sie können die folgenden Filter Felder angeben:<p/><ul><li><strong>Kauf</strong></li><li><strong>gesundheitliche</strong></li><li><strong>ungs</strong></li></ul> | Ja   |

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird eine Anforderung zum erhalten von Insights-Daten veranschaulicht. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert      | type   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Insights-Daten für die APP enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Insight Values](#insight-values) .                                                                                                                      |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                 |


### <a name="insight-values"></a>Insight-Werte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | type   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | Zeichenfolge | Die Speicher-ID der APP, für die Sie Insights-Daten abrufen.     |
| insightdate                | Zeichenfolge | Das Datum, an dem die Änderung in einer bestimmten Metrik festgelegt wurde. Dieses Datum ist das Ende der Woche, an dem wir eine beträchtliche Zunahme oder Abnahme einer Metrik im Vergleich zur Woche vor dieser Woche festgestellt haben. |
| dataType     | Zeichenfolge | Eine der folgenden Zeichen folgen, die den allgemeinen Analysebereich angibt, der in diesem Einblick beschrieben wird:<p/><ul><li><strong>Kauf</strong></li><li><strong>gesundheitliche</strong></li><li><strong>ungs</strong></li></ul>   |
| insightdetail          | array | Ein oder mehrere [insightdetail-Werte](#insightdetail-values) , die die Details für den aktuellen Einblick darstellen.    |


### <a name="insightdetail-values"></a>Insightdetail-Werte

| Wert               | type   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| Factname           | Zeichenfolge | Einer der folgenden Werte, der die Metrik angibt, die von der aktuellen Insight-oder Current-Dimension basierend auf dem **DataType** -Wert beschrieben wird.<ul><li>Aus **Integritäts Gründen ist dieser** Wert immer " **HitCount**".</li><li>Für den **Erwerb** ist dieser Wert immer **acquisitionmenge**.</li><li>Zur **Verwendung** kann es sich bei diesem Wert um eine der folgenden Zeichen folgen handeln:<ul><li><strong>Dailyactiveusers</strong></li><li><strong>Engagementdurationminutes</strong></li><li><strong>Dailyactivedevices</strong></li><li><strong>Dailynewusers</strong></li><li><strong>Dailysessioncount</strong></li></ul></ul>  |
| Unterdimensionen         | array |  Mindestens ein-Objekt, das eine einzelne Metrik für den Einblick beschreibt.   |
| Prozentuumänderung            | Zeichenfolge |  Der Prozentsatz, in dem sich die Metrik für die gesamte Kundenbasis geändert hat.  |
| DimensionName           | Zeichenfolge |  Der Name der Metrik, die in der aktuellen Dimension beschrieben wird. Beispiele hierfür sind **eventType**, **Market**, **DeviceType**, **packageversion**, **acquisitiontype**, **AgeGroup** und **Geschlecht**.   |
| Dimensionvalue              | Zeichenfolge | Der Wert der Metrik, die in der aktuellen Dimension beschrieben wird. Wenn **dimensionname** z. b. **eventType** ist, könnte **dimensionvalue** **abstürzen** oder nicht **hängen**.   |
| Factvalue     | Zeichenfolge | Der absolute Wert der Metrik für das Datum, an dem der Einblick erkannt wurde.  |
| Direction | Zeichenfolge |  Die Richtung der Änderung (**positiv** oder **negativ**).   |
| Datum              | Zeichenfolge |  Das Datum, an dem die Änderung im Zusammenhang mit dem aktuellen Einblick oder der aktuellen Dimension festgelegt wurde.   |

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

## <a name="related-topics"></a>Zugehörige Themen

* [Datenbericht](../publish/insights-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
