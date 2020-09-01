---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Xbox Live Club-Daten zu erhalten.
title: Abrufen von Xbox Live-Clubdaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Xbox Live Analytics, Clubs
ms.localizationpriority: medium
ms.openlocfilehash: 155e8a48f9e9e207709af4e55bb8e052b4419867
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162364"
---
# <a name="get-xbox-live-club-data"></a>Abrufen von Xbox Live-Clubdaten

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Club Daten für das [Live-aktivierte Xbox-Spiel](/gaming/xbox-live/index.md)zu erhalten. Diese Informationen sind auch im Bericht " [Xbox Analytics](../publish/xbox-analytics-report.md) " im Partner Center verfügbar.

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
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Xbox Live Club-Daten abrufen möchten.  |  Ja  |
| metrictype | Zeichenfolge | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analytics-Daten angibt. Geben Sie für diese Methode den Wert **communitymanagerclub**an.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Club Daten. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich von Club Daten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum erhalten von Club Daten für Ihr Xbox Live-fähiges Spiel. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID für Ihr Spiel.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array, das ein [productdata](#productdata) -Objekt enthält, das Daten für mit Ihrem Spiel gehörige-und ein [xboxwidedata](#xboxwidedata) -Objekt enthält, das Club Daten für alle Xbox Live-Kunden enthält. Diese Daten sind für Vergleichszwecke mit den Daten für Ihr Spiel enthalten.  |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10000 festgelegt ist, aber mehr als 10000 Daten Zeilen für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |


### <a name="productdata"></a>Productdata

Diese Ressource enthält Club Daten für Ihr Spiel.

| Wert           | Typ    | BESCHREIBUNG        |
|-----------------|---------|------|
| date            |  Zeichenfolge |   Das Datum der Club Daten.   |
|  applicationId               |    Zeichenfolge     |  Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Club Daten abgerufen haben.   |
|  clubswithtitleactivity               |    INT     |  Die Anzahl der mit Ihrem Spiel in sozialen spielen.   |     
|  clubsexclusivewegame               |   INT      |  Die Anzahl der Clubs, die sich ausschließlich mit Ihrem Spiel befassen.   |     
|  Club Fakten               |   array      |   Enthält ein oder mehrere [Club Facts](#clubfacts) -Objekte zu den einzelnen vereinen, die gesellschaftlich mit Ihrem Spiel beschäftigt sind.   |


### <a name="xboxwidedata"></a>Xboxwidedata

Diese Ressource enthält die durchschnittlichen Club Daten für alle Xbox Live-Kunden.

| Wert           | Typ    | BESCHREIBUNG        |
|-----------------|---------|------|
| date            |  Zeichenfolge |   Das Datum der Club Daten.   |
|  applicationId  |    Zeichenfolge     |   Im **xboxwidedata** -Objekt entspricht diese Zeichenfolge immer dem Wert **xboxwide**.  |
|  clubswithtitleactivity               |   INT     |  Im Durchschnitt die Anzahl der Clubs mit Kunden, die sich in einem Live-fähigen Xbox-Spiel befinden.    |     
|  clubsexclusivewegame               |   INT      |  Im Durchschnitt ist dies die Anzahl von Clubs, die sich ausschließlich mit einem Live-fähigen Xbox-Spiel befassen.   |     
|  Club Fakten               |   Objekt (object)      |  Enthält ein [Club Facts](#clubfacts) -Objekt. Dieses Objekt ist im Kontext des **xboxwidedata** -Objekts bedeutungslos und verfügt über Standardwerte.  |


### <a name="clubfacts"></a>Club Fakten

Im **productdata** -Objekt enthält dieses Objektdaten für einen bestimmten Club, der über Aktivitäten im Zusammenhang mit Ihrem Spiel verfügt. Im **xboxwidedata** -Objekt ist dieses Objekt bedeutungslos und verfügt über Standardwerte.

| Wert           | Typ    | BESCHREIBUNG        |
|-----------------|---------|--------------------|
|  name            |  Zeichenfolge  |   Im **productdata** -Objekt ist dies der Name des-Clubs. Im **xboxwidedata** -Objekt ist dies immer der Wert **xboxwide**.           |
|  memberCount               |    INT     | Im **productdata** -Objekt ist dies die Anzahl der Mitglieder im Club, ausgenommen nicht-Member, die nur den Club besuchen. Im **xboxwidedata** -Objekt ist dies immer 0.    |
|  titlesocialaktionscount               |    INT     |  Im **productdata** -Objekt ist dies die Anzahl der sozialen Aktionen, die Mitglieder des Clubs durchgeführt haben, die mit Ihrem Spiel in Zusammenhang stehen. Im **xboxwidedata** -Objekt ist dies immer 0.   |
|  isexclusivewegame               |    Boolean     |  Im **productdata** -Objekt gibt dies an, ob der aktuelle Club ausschließlich mit Ihrem Spiel verknüpft ist. Im **xboxwidedata** -Objekt ist dies immer "true".  |


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

## <a name="related-topics"></a>Zugehörige Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analysedaten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)
* [Get Xbox Live Game Hub-Daten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)