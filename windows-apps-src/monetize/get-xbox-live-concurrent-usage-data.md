---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um gleichzeitige Xbox Live-Verwendungs Daten zu erhalten.
title: Abrufen von Xbox Live-Daten zur gleichzeitigen Nutzung
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Xbox Live Analytics, gleichzeitige Verwendung
ms.localizationpriority: medium
ms.openlocfilehash: 9e8d36ce9336d5fc65a73a19233b8a1f0a05a8bf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167584"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Abrufen von Xbox Live-Daten zur gleichzeitigen Nutzung


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Nutzungsdaten nahezu in Echtzeit (mit einer Latenz von 5-15 Minuten) für die durchschnittliche Anzahl der Kunden zu erhalten, die das [Xbox Live-fähige Spiel](/gaming/xbox-live/index.md) alle Minuten, Stunden oder Tage innerhalb eines bestimmten Zeitraums spielen. Diese Informationen sind auch im Bericht " [Xbox Analytics](../publish/xbox-analytics-report.md) " im Partner Center verfügbar.

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
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie die Daten zur gleichzeitigen Verwendung von Xbox Live abrufen möchten.  |  Ja  |
| metrictype | Zeichenfolge | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analytics-Daten angibt. Geben Sie für diese Methode **den Wert**Parallelität an.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Daten der gleichzeitigen Verwendung. Das Standardverhalten finden Sie in der *aggregationlevel* -Beschreibung. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der abzurufenden Daten der gleichzeitigen Verwendung. Das Standardverhalten finden Sie in der *aggregationlevel* -Beschreibung. |  Nein  |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Kann eine der folgenden Zeichen folgen sein: **Minute**, **Hour**oder **Day**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**. <p/><p/>Wenn Sie *StartDate* oder *EndDate*nicht angeben, lautet der Antworttext standardmäßig wie folgt: <ul><li>**Minute**: die letzten 60 Datensätze verfügbarer Daten.</li><li>**Hour**: die letzten 24 Datensätze verfügbarer Daten.</li><li>**Day**: die letzten 7 Datensätze verfügbarer Daten.</li></ul><p/>Die folgenden Aggregations Ebenen haben Größenbeschränkungen hinsichtlich der Anzahl von Datensätzen, die zurückgegeben werden können. Die Datensätze werden abgeschnitten, wenn die angeforderte Zeitspanne zu groß ist. <ul><li>**Minute**: bis zu 1440 Datensätze (24 Stundendaten).</li><li>**Hour**: bis zu 720 Datensätze (Daten von 30 Tagen).</li><li>**Day**: bis zu 60 Datensätze (60 Tage Daten).</li></ul>  |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird eine Anforderung zum erhalten von Daten zur gleichzeitigen Verwendung für das Xbox Live-fähige Spiel veranschaulicht. Mit dieser Anforderung werden Daten für jede Minute zwischen Februar 1 2018 und dem 2 2018. Februar abgerufen. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID für Ihr Spiel.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Der Antworttext enthält ein Array von-Objekten, die jeweils einen Satz gleichzeitiger Verwendungs Daten für eine angegebene Minute, Stunde oder einen bestimmten Tag enthalten. Jedes-Objekt enthält die folgenden Werte:

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Anzahl      | number  | Die durchschnittliche Anzahl der Kunden, die Ihre Xbox Live-Funktion für die angegebene Minute, Stunde oder den angegebenen Tag spielen. <p/><p/>**Note** &nbsp; Hinweis &nbsp; Der Wert 0 gibt entweder an, dass während des angegebenen Intervalls keine gleichzeitigen Benutzer vorhanden waren, oder dass während des angegebenen Intervalls während der Erfassung gleichzeitiger Benutzerdaten für das Spiel ein Fehler aufgetreten ist. |
| Date  | Zeichenfolge | Das Datum und die Uhrzeit der Angabe von Minute, Stunde oder Tag, in der die Daten zur gleichzeitigen Verwendung aufgetreten sind.  |
| Seriesname | Zeichenfolge    | Dies hat immer den Wert **userparallelcurrency**. |


### <a name="response-example"></a>Antwortbeispiel

Im folgenden Beispiel wird ein Beispiel für einen JSON-Antworttext für diese Anforderung mit Datenaggregation pro Minute veranschaulicht.

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>Zugehörige Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analysedaten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)
* [Get Xbox Live Game Hub-Daten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)