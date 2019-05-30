---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von Insights-Daten für die desktop-Anwendung.
title: Abrufen von internen Daten für die Desktopanwendung
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, Uwp, Store Services, Microsoft Store-Textanalyse-API, Einblicke
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f6f4b2df1cda14bc1f363a1f9100e416f26489b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372466"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Abrufen von internen Daten für die Desktopanwendung

Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von Insights-Daten im Zusammenhang mit der integritätsmetriken für eine desktop-Anwendung, die Sie hinzugefügt haben die [Desktopanwendung, die Windows-Programm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Diese Daten werden auch in der [Integritätsbericht](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) für desktop-Anwendungen im Partner Center.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | String | Die Produkt-ID der Desktopanwendung, die für das Insights-Daten abgerufen werden sollen. Rufen Sie die Produkt-ID, einer Desktopanwendung öffnen [Analytics zu senden, damit die desktop-Anwendung im Partner Center](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (z. B. die **Integritätsbericht**) und die Produkt-ID aus der URL abzurufen. Wenn Sie diesen Parameter nicht angeben, enthält der Antworttext die Insights-Daten für alle apps, die mit Ihrem Konto registriert.  |  Nein  |
| startDate | date | Das Startdatum in den Datumsbereich für Insights-Daten abgerufen werden soll. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum in den Datumsbereich für Insights-Daten abgerufen werden soll. Der Standardwert ist das aktuelle Datum. |  Nein  |
| filter | String  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Z. B. *Filter = DataType-Eq "Übernahme"* . <p/><p/>Diese Methode unterstützt derzeit nur den Filter **Integrität**.  | Nein   |

### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von Insights-Daten. Ersetzen Sie die *ApplicationId* Wert mit den entsprechenden Wert für die desktop-Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die für die app Insights-Daten enthalten. Weitere Informationen zu den Daten in jedem Objekt finden Sie unter der [Insight Werte](#insight-values) Abschnitt weiter unten.                                                                                                                      |
| TotalCount | ssNoversion    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                 |


### <a name="insight-values"></a>Insight-Werte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | String | Die Produkt-ID der Desktopanwendung, die für das Insights-Daten abgerufen werden soll.     |
| insightDate                | String | Das Datum, an dem die Änderung in einer bestimmten Metrik erkannt wurden. Dieses Datum stellt das Ende der Woche, die in der wir eine erhebliche Leistungssteigerung erkannt oder verringern Sie in einer Metrik im Vergleich zu einer Woche davor. |
| dataType     | String | Eine Zeichenfolge, die den allgemeinen Analytics Bereich angibt, den diese Information werden soll. Diese Methode unterstützt derzeit nur die **Integrität**.    |
| insightDetail          | array | Eine oder mehrere [InsightDetail Werte](#insightdetail-values) , die die Details für den aktuellen Insight darstellen.    |


### <a name="insightdetail-values"></a>InsightDetail Werte

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| FactName           | String | Eine Zeichenfolge, die die Metrik gibt an, der den aktuellen Insight oder die aktuelle Dimension beschreibt. Diese Methode unterstützt derzeit nur den Wert **Trefferanzahl**.  |
| SubDimensions         | array |  Ein oder mehrere Objekte, die eine einzelne Metrik für die Einblicke zu beschreiben.   |
| PercentChange            | String |  Der Prozentsatz, den die Metrik für Ihre gesamte Kundenbasis geändert.  |
| DimensionName           | String |  Der Name der Metrik in der aktuellen Dimension beschrieben. Beispiele hierfür sind **EventType**, **Markt**, **"DeviceType"** , und **PackageVersion**.   |
| DimensionValue              | String | Der Wert der Metrik, die in der aktuellen Dimension beschrieben wird. Z. B. wenn **DimensionName** ist **EventType**, **DimensionValue** möglicherweise **Absturz** oder **hängen** .   |
| FactValue     | String | Der Absolute Wert der Metrik auf das Datum, an das der Einblick erkannt wurde.  |
| Richtung | String |  Die Richtung der Änderung (**Positive** oder **Negative**).   |
| date              | String |  Das Datum, an dem die Änderung im Zusammenhang mit der aktuellen Einblick oder die aktuelle Dimension erkannt wurden.   |

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

* [Windows-Desktop-Anwendung](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Bericht zur Integrität](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
