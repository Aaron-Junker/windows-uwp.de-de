---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Insights-Daten für Ihre Desktop Anwendung zu erhalten.
title: Abrufen von internen Daten für die Desktopanwendung
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Einblicke
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd60425a5ec3c040417aded818c766db80eb59eb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172894"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Abrufen von internen Daten für die Desktopanwendung

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um mit den Integritäts Metriken für eine Desktop Anwendung, die Sie dem [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)hinzugefügt haben, Insights-Daten zu erhalten. Diese Daten sind auch im Integritäts [Bericht](/windows/desktop/appxpkg/windows-desktop-application-program#health-report) für Desktop Anwendungen in Partner Center verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die Produkt-ID der Desktop Anwendung, für die Sie Insights-Daten erhalten möchten. Wenn Sie die Produkt-ID einer Desktop Anwendung abrufen möchten, öffnen Sie einen beliebigen [Analysebericht für Ihre Desktop Anwendung im Partner Center](/windows/desktop/appxpkg/windows-desktop-application-program) (z. b. den Integritäts **Bericht**), und rufen Sie die Produkt-ID aus der URL ab. Wenn Sie diesen Parameter nicht angeben, enthält der Antworttext Insights-Daten für alle apps, die für Ihr Konto registriert sind.  |  Nein  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Insights-Daten. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der abzurufenden Insights-Daten. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Beispiel: *Filter = DataType EQ ' Acquisition '*. <p/><p/>Derzeit unterstützt diese Methode nur den Filter **Zustand**.  | Nein   |

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird eine Anforderung zum erhalten von Insights-Daten veranschaulicht. Ersetzen Sie den Wert *ApplicationId* durch den entsprechenden Wert für Ihre Desktop Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Insights-Daten für die APP enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Insight Values](#insight-values) .                                                                                                                      |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                 |


### <a name="insight-values"></a>Insight-Werte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | Zeichenfolge | Die Produkt-ID der Desktop Anwendung, für die Sie Insights-Daten abgerufen haben.     |
| insightdate                | Zeichenfolge | Das Datum, an dem die Änderung in einer bestimmten Metrik festgelegt wurde. Dieses Datum ist das Ende der Woche, an dem wir eine beträchtliche Zunahme oder Abnahme einer Metrik im Vergleich zur Woche vor dieser Woche festgestellt haben. |
| dataType     | Zeichenfolge | Eine Zeichenfolge, die den allgemeinen Analysebereich angibt, den dieser Einblick mitteilt. Diese **Methode unterstützt**derzeit nur die Integrität.    |
| insightdetail          | array | Ein oder mehrere [insightdetail-Werte](#insightdetail-values) , die die Details für den aktuellen Einblick darstellen.    |


### <a name="insightdetail-values"></a>Insightdetail-Werte

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| Factname           | Zeichenfolge | Eine Zeichenfolge, die die Metrik angibt, die von der aktuellen Insight-oder aktuellen Dimension beschrieben wird. Derzeit unterstützt diese Methode nur den Wert **HitCount**.  |
| Unterdimensionen         | array |  Mindestens ein-Objekt, das eine einzelne Metrik für den Einblick beschreibt.   |
| Prozentuumänderung            | Zeichenfolge |  Der Prozentsatz, in dem sich die Metrik für die gesamte Kundenbasis geändert hat.  |
| DimensionName           | Zeichenfolge |  Der Name der Metrik, die in der aktuellen Dimension beschrieben wird. Beispiele hierfür sind **eventType**, **Market**, **DeviceType**und **packageversion**.   |
| Dimensionvalue              | Zeichenfolge | Der Wert der Metrik, die in der aktuellen Dimension beschrieben wird. Wenn **dimensionname** z. b. **eventType**ist, könnte **dimensionvalue** **abstürzen** oder nicht **hängen**.   |
| Factvalue     | Zeichenfolge | Der absolute Wert der Metrik für das Datum, an dem der Einblick erkannt wurde.  |
| Direction | Zeichenfolge |  Die Richtung der Änderung (**positiv** oder **negativ**).   |
| Date              | Zeichenfolge |  Das Datum, an dem die Änderung im Zusammenhang mit dem aktuellen Einblick oder der aktuellen Dimension festgelegt wurde.   |

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

* [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)
* [Integritätsbericht](/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)