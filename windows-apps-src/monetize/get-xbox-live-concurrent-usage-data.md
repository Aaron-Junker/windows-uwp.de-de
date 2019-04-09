---
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Xbox Live gleichzeitige Nutzungsdaten abzurufen.
title: Abrufen von Xbox Live-Daten zur gleichzeitigen Nutzung
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, Uwp, Store-Diensten, Microsoft Store-Analyse-API, Xbox Live-Analyse, gleichzeitige Nutzung
ms.localizationpriority: medium
ms.openlocfilehash: e4ac2208ca5eca02e3007a88209aa26735e29612
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58162866"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Abrufen von Xbox Live-Daten zur gleichzeitigen Nutzung


Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um fast in Echtzeit die Nutzungsdaten (mit 5-15-Minuten-Latenz) über die durchschnittliche Anzahl der Kunden abzurufen, die Ihr [Xbox Live-fähiges Spiel](https://docs.microsoft.com/gaming/xbox-live//index.md) jede Minute, Stunde oder Tag während eines angegebenen Zeitraums spielen. Diese Informationen sind auch verfügbar in der [Xbox-Analysebericht](../publish/xbox-analytics-report.md) im Partner Center.

> [!IMPORTANT]
> Diese Methode unterstützt nur Spiele für Xbox oder Spiele, die Xbox Live-Dienste verwenden. Diese Spiele müssen den [Konzeptgenehmigungsprozess](../gaming/concept-approval.md) durchlaufen, der Spiele umfasst, die von [Microsoft-Partnern](https://docs.microsoft.com/gaming/xbox-live//developer-program-overview.md#microsoft-partners) veröffentlicht wurden, sowie Spiele, die über das [ID@Xbox-Programm](https://docs.microsoft.com/gaming/xbox-live//developer-program-overview.md#id) übermittelt wurden. Diese Methode unterstützt derzeit keine Spiele, die über das [Xbox Live Creators-Programm](https://docs.microsoft.com/gaming/xbox-live//get-started-with-creators/get-started-with-xbox-live-creators.md) eingereicht wurden.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter


| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | String | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie die Xbox Live gleichzeitigen Nutzungsdaten abrufen möchten.  |  Ja  |
| metricType | String | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live-Analysedaten angibt. Geben Sie für diese Methode den Wert **concurrency** an.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der gleichzeitigen Nutzungsdaten, die abgerufen werden sollen. Weitere Informationen finden Sie unter der *aggregationLevel*-Beschreibung für das Standardverhalten. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der gleichzeitigen Nutzungsdaten, die abgerufen werden sollen. Weitere Informationen finden Sie unter der *aggregationLevel*-Beschreibung für das Standardverhalten. |  Nein  |
| aggregationLevel | String | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: **minute**, **hour** oder **day**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**. <p/><p/>Wenn Sie kein *startDate* oder *endDate* angeben, ist der Antworttext standardmäßig wie folgt: <ul><li>**Minute**: Die letzten 60 Datensätze der verfügbaren Daten.</li><li>**Stunde**: Die letzten 24 Einträge der verfügbaren Daten.</li><li>**day**: Die letzte 7 Datensätze der verfügbaren Daten.</li></ul><p/>Die folgenden Aggregationsebenen haben Größenbegrenzungen hinsichtlich der Anzahl von Datensätzen, die zurückgegeben werden können. Die Datensätze werden abgeschnitten, wenn die angeforderte Zeitspanne zu groß ist. <ul><li>**Minute**: Bis zu 1440 Datensätze (24 Stunden Daten).</li><li>**Stunde**: Bis zu 720 Datensätze (Daten 30 Tage).</li><li>**day**: Bis zu 60 Datensätze (Daten 60 Tage).</li></ul>  |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von gleichzeitigen Nutzungsdaten für Ihr Xbox Live-fähiges Spiel an. Diese Anforderung ruft Daten für jede Minute zwischen dem 1. Februar 2018 und 2. Februar 2018 ab. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihres Spiels.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Der Antworttext enthält ein Array von Objekten, die jeweils einen Satz von gleichzeitigen Nutzungsdaten für eine angegebene Minute, Stunde oder Tag enthalten. Jedes Objekt enthält die folgenden Werte.

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Anzahl      | number  | Die durchschnittliche Anzahl der Kunden, die Ihr Xbox Live-fähiges Spiel für die angegebene Minute, Stunde oder Tag spielen. <p/><p/>**Hinweis:**&nbsp;&nbsp;Ein Wert von 0 gibt an, dass entweder keine gleichzeitigen Benutzer während des angegebenen Intervalls spielten, oder dass beim Sammeln von gleichzeitigen Benutzerdaten für das Spiel während des angegebenen Intervalls ein Fehler aufgetreten ist. |
| Datum  | String | Das Datum und Uhrzeit, die die Minute, Stunde oder Tag angeben, während dessen die gleichzeitigen Nutzungsdaten auftraten.  |
| SeriesName | String    | Dies hat immer den Wert **UserConcurrency**. |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung mit Datenaggregation pro Minute.

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

## <a name="related-topics"></a>Verwandte Themen

* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analytics-Daten](get-xbox-live-analytics.md)
* [Abrufen von Daten für Xbox Live Erfolge](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Health-Daten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live-Spiele-Hub-Daten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Club-Daten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayer-Daten](get-xbox-live-multiplayer-data.md)
