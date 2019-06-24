---
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Xbox Live Clubdaten abzurufen.
title: Abrufen von Xbox Live-Clubdaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, Uwp, Store-Diensten, Microsoft Store-Analyse-API, Xbox Live Analyse, Clubs
ms.localizationpriority: medium
ms.openlocfilehash: e5fc116c2b868ddf093aabea09d59934301f49ec
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321865"
---
# <a name="get-xbox-live-club-data"></a>Abrufen von Xbox Live-Clubdaten

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Clubdaten für Ihr [Xbox Live-fähiges Spiel](https://docs.microsoft.com/gaming/xbox-live/index.md) abzurufen. Diese Informationen sind auch verfügbar in der [Xbox-Analysebericht](../publish/xbox-analytics-report.md) im Partner Center.

> [!IMPORTANT]
> Diese Methode unterstützt nur Spiele für Xbox oder Spiele, die Xbox Live-Dienste verwenden. Diese Spiele müssen den [Konzeptgenehmigungsprozess](../gaming/concept-approval.md) durchlaufen, der Spiele umfasst, die von [Microsoft-Partnern](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners) veröffentlicht wurden, sowie Spiele, die über das [ID@Xbox-Programm](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id) übermittelt wurden. Diese Methode unterstützt derzeit keine Spiele, die über das [Xbox Live Creators-Programm](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) eingereicht wurden.

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
| applicationId | String | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie die Xbox Live Clubdaten abrufen möchten.  |  Ja  |
| metricType | String | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live-Analysedaten angibt. Geben Sie für diese Methode den Wert **communitymanagerclub** an.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Clubdaten, die abgerufen werden sollen. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Clubdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | ssNoversion | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | ssNoversion | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von Clubdaten für Ihr Xbox Live-fähiges Spiel an. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihres Spiels.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array mit einem [ProductData](#productdata)-Objekt, das Daten für Clubs enthält, die sich auf Ihr Spiel sowie auf ein [XboxwideData](#xboxwidedata)-Objekt beziehen, das Clubdaten für alle Xbox Live Kunden enthält. Diese Daten werden zu Vergleichszwecken mit den Daten für Ihr Spiel beigefügt.  |
| @nextLink  | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10000 Zeilen mit Daten für die Abfrage gibt. |
| TotalCount | ssNoversion    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |


### <a name="productdata"></a>ProductData

Diese Ressource enthält Clubdaten für Ihr Spiel.

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|------|
| date            |  String |   Das Datum für die Clubdaten.   |
|  applicationId               |    String     |  Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Clubdaten abgerufen haben.   |
|  clubsWithTitleActivity               |    ssNoversion     |  Die Anzahl der Clubs, die an Ihrem Spiel gemeinschaftlich beteiligt sind.   |     
|  clubsExclusiveToGame               |   ssNoversion      |  Die Anzahl der Clubs, die ausschließlich an Ihrem Spiel gemeinschaftlich beteiligt sind.   |     
|  clubFacts               |   array      |   Enthält ein oder mehrere [ClubFacts](#clubfacts)-Objekte zu den einzelnen Clubs, die an Ihrem Spiel gemeinschaftlich beteiligt sind.   |


### <a name="xboxwidedata"></a>XboxwideData

Diese Ressource enthält die durchschnittlichen Clubdaten über alle Xbox Live-Kunden.

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|------|
| date            |  String |   Das Datum für die Clubdaten.   |
|  applicationId  |    String     |   Im **XboxwideData**-Objekt hat diese Zeichenfolge ist immer den Wert **XBOXWIDE**.  |
|  clubsWithTitleActivity               |   ssNoversion     |  Durchschnittlich die Anzahl der Clubs mit Kunden, die an einem Xbox Live-fähigen Spiel gemeinschaftlich beteiligt sind.    |     
|  clubsExclusiveToGame               |   ssNoversion      |  Durchschnittlich die Anzahl der Clubs mit Kunden, die ausschließlich an einem Xbox Live-fähigen Spiel gemeinschaftlich beteiligt sind.   |     
|  clubFacts               |   object      |  Enthält ein [ClubFacts](#clubfacts)-Objekt. Dieses Objekt ist bedeutungslos im Kontext des **XboxwideData**-Objekts und verfügt über Standardwerte.  |


### <a name="clubfacts"></a>ClubFacts

Im **ProductData**-Objekt enthält dieses Objekt enthält Daten für einen bestimmten Club, der Aktivitäten im Bezug auf Ihr Spiel hat. Im **XboxwideData**-Objekt ist dieses Objekt bedeutungslos und verfügt über Standardwerte.

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|--------------------|
|  NAME            |  String  |   Im **ProductData**-Objekt ist dies der Name des Clubs. Im **XboxwideData**-Objekt hat dies immer den Wert **XBOXWIDE**.           |
|  memberCount               |    ssNoversion     | Im **ProductData**-Objekt ist dies die Anzahl der Clubmitglieder, mit Ausnahme der nicht-Mitglieder, die den Club nur besuchen. Im **XboxwideData**-Objekt ist dies immer 0.    |
|  titleSocialActionsCount               |    ssNoversion     |  Im **ProductData**-Objekt ist dies die Anzahl der gemeinschaftlichen Aktionen, die Mitglieder im Club im Bezug auf Ihr Spiel ausgeführt haben. Im **XboxwideData**-Objekt ist dies immer 0   |
|  isExclusiveToGame               |    Boolesch     |  Im **ProductData**-Objekt gibt dies an, ob der aktuelle Club ausschließlich nur an Ihrem Spiel gemeinschaftlich beteiligt ist. Im **XboxwideData**-Objekt ist dies immer wahr.  |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analytics-Daten](get-xbox-live-analytics.md)
* [Abrufen von Daten für Xbox Live Erfolge](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Health-Daten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live-Spiele-Hub-Daten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live Multiplayer-Daten](get-xbox-live-multiplayer-data.md)
