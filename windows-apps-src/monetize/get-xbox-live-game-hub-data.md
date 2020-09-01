---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Xbox Live Game Hub-Daten zu erhalten.
title: Get Xbox Live Game Hub-Daten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Xbox Live Analytics, Game Hubs
ms.localizationpriority: medium
ms.openlocfilehash: aa714eafbc8357a66128045cf3b5f2effc9f067a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155554"
---
# <a name="get-xbox-live-game-hub-data"></a>Abrufen von Xbox Live Spielehubdaten


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Game Hub-Daten für das [Live-aktivierte Xbox-Spiel](/gaming/xbox-live/index.md)zu erhalten. Diese Informationen sind auch im Bericht " [Xbox Analytics](../publish/xbox-analytics-report.md) " im Partner Center verfügbar.

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
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Xbox Live Game Hub-Daten abrufen möchten.  |  Ja  |
| metrictype | Zeichenfolge | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analytics-Daten angibt. Geben Sie für diese Methode den Wert **communitymanagergamehub**an.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden spielhub-Daten. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der abzurufenden spielhub-Daten. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum erhalten von spielhub-Daten für Ihr Xbox Live-fähiges Spiel. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID für Ihr Spiel.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagergamehub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die spielhub Daten für jedes Datum im angegebenen Datumsbereich enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.                                                                                                                      |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10000 festgelegt ist, aber mehr als 10000 Daten Zeilen für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.  |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| date                | Zeichenfolge | Das Datum für die spielhub-Daten in diesem-Objekt. |
| applicationId       | Zeichenfolge | Die Speicher-ID des Spiels, für das Sie die spielhub-Daten abrufen.     |
| gamehublikecount     | number |   Die Anzahl der der spielhub Seite am angegebenen Datum hinzugefügten likes.   |
| gamehubcommentcount          | number |  Die Anzahl der Kommentare, die der spielhub Seite für Ihre APP am angegebenen Datum hinzugefügt werden.  |
| gamehubsharecount           | number | Gibt an, wie oft die spielhub Seite für Ihre APP an dem angegebenen Datum von Kunden gemeinsam genutzt wurde.   |
| gamehubfollowercount          | number | Die Anzahl aller Zeit-Follower für die spielhub Seite für Ihre APP.   |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2018-01-04",
      "applicationId": "9NBLGGGZ5QDR",
      "gameHubLikeCount": 10,
      "gameHubCommentCount": 1,
      "gameHubShareCount": 0,
      "gameHubFollowerCount": 15924
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "gameHubLikeCount": 12,
      "gameHubCommentCount": 1,
      "gameHubShareCount": 0,
      "gameHubFollowerCount": 15931
    }
  ],
  "@nextLink": null,
  "TotalCount": 26
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analysedaten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)