---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Xbox Live-Erstellungs Daten zu erhalten.
title: Abrufen von Xbox Live-Erfolgsdaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Xbox Live Analytics, Leistungen
ms.localizationpriority: medium
ms.openlocfilehash: 7d4c031d967c48c65f0f9be7b386c11c12b7366e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162374"
---
# <a name="get-xbox-live-achievements-data"></a>Abrufen von Xbox Live-Erfolgsdaten

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um die Anzahl der Kunden zu erhalten, die die einzelnen Leistungen für das [Xbox Live-fähige Spiel](/gaming/xbox-live/index.md) am letzten Tag, an dem die Daten verfügbar sind, und die letzten 30 Tage, die bis zu diesem Tag zur Verfügung stehen, entsperrt haben. Diese Informationen sind auch im Bericht " [Xbox Analytics](../publish/xbox-analytics-report.md) " im Partner Center verfügbar.

> [!IMPORTANT]
> Diese Methode unterstützt nur Spiele für Xbox oder Spiele, die Xbox Live-Dienste verwenden. Diese Spiele müssen den [Genehmigungsprozess des Konzepts](../gaming/concept-approval.md)durchlaufen, der Spiele umfasst, die von [Microsoft-Partnern](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) und über das [ ID@Xbox Programm](/gaming/xbox-live/developer-program-overview.md#id)gesendeten spielen veröffentlicht wurden. Diese Methode unterstützt zurzeit keine Spiele, die über das [Xbox Live Creators-Programm](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)veröffentlicht wurden.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter


| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Xbox Live-Erstellungs Daten abrufen möchten.  |  Ja  |
| metrictype | Zeichenfolge | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analytics-Daten angibt. Geben Sie für diese Methode den Wert " **Erfolge**" an.  |  Ja  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird eine Anforderung zum erhalten von Erstellungs Daten für Kunden veranschaulicht, die die ersten 10 Ergebnisse für das Xbox Live-fähige Spiel entsperrt haben. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID für Ihr Spiel.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Daten für die einzelnen Spiele in Ihrem Spiel enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.                                                                                                                      |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 100 festgelegt ist, aber mehr als 100 Daten Zeilen für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.  |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | Zeichenfolge | Die Speicher-ID des Spiels, für das Sie die Erstellungs Daten abrufen.     |
| reportdatetime     | Zeichenfolge |  Das Datum für die Erstellungs Daten.    |
| "achievementid"          | number |  Die ID der-Errungenschaft. |
| "achievementname"           | Zeichenfolge | Der Name der-Errungenschaft.  |
| Gamerscore           | number |  Die Gamerscore-Belohnung für die-Errungenschaft.  |
| dailyunlocks           | number |  Die Anzahl der Kunden, die die Bereitstellung an dem Tag entsperrt haben, der durch *reportdatetime*angegeben wurde.  |
| monthlyunlocks              | number |  Die Anzahl der Kunden, die die Auszeichnung in den 30 Tagen vor dem durch *reportdatetime*angegebenen Tag entsperrt haben.   |
| totalunlocks | number |  Die Anzahl der Kunden, die während der Lebensdauer des Spiels den Wert entsperrt haben, bis zu dem Tag, der durch *reportdatetime*festgelegt wurde.   |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analysedaten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live Spielehubdaten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)