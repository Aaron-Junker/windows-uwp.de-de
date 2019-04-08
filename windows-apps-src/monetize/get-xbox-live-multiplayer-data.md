---
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Xbox Live Multiplayerdaten abzurufen.
title: Abrufen von Xbox Live Multiplayerdaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, Uwp, Store-Diensten, Microsoft Store-Analyse-API, Xbox Live-Analyse, Multiplayer
ms.localizationpriority: medium
ms.openlocfilehash: 74f1a64bde32fe68a51527527a0b049d811d0853
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662035"
---
# <a name="get-xbox-live-multiplayer-data"></a>Abrufen von Xbox Live Multiplayerdaten


Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Multiplayerdaten für Ihr [Xbox Live-fähiges Spiel](../xbox-live/index.md) auf einer täglichen oder monatlichen Basis abzurufen. Diese Informationen sind auch verfügbar in der [Xbox-Analysebericht](../publish/xbox-analytics-report.md) im Partner Center.

> [!IMPORTANT]
> Diese Methode unterstützt nur Spiele für Xbox oder Spiele, die Xbox Live-Dienste verwenden. Diese Spiele müssen den [Konzeptgenehmigungsprozess](../gaming/concept-approval.md) durchlaufen, der Spiele umfasst, die von [Microsoft-Partnern](../xbox-live/developer-program-overview.md#microsoft-partners) veröffentlicht wurden, sowie Spiele, die über das [ID@Xbox-Programm](../xbox-live/developer-program-overview.md#id) übermittelt wurden. Diese Methode unterstützt derzeit keine Spiele, die über das [Xbox Live Creators-Programm](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) eingereicht wurden.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter


| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie die Xbox Live Multiplayerdaten abrufen möchten.  |  Ja  |
| metricType | string | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live-Analysedaten angibt. Geben Sie für diese Methode den Wert **multiplayerdaily** zum Abrufen von täglichen Multiplayerdaten an oder **multiplayermonthly**, um monatliche Multiplayerdaten zu erhalten.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Multiplayerdaten, die abgerufen werden sollen. Für **multiplayerdaily** ist der Standardwert 3 Monate vor dem aktuellen Datum Für **multiplayermonthly** ist der Standardwert 1 Jahr vor dem aktuellen Datum |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Multiplayerdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>"DeviceType"</strong></li><li><strong>PackageVersion</strong></li><li><strong>Markt</strong></li><li><strong>Abonnementname</strong></li></ul> | Nein   |
| groupby | string | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>Datum</strong></li><li><strong>"DeviceType"</strong></li><li><strong>PackageVersion</strong></li><li><strong>Markt</strong></li><li><strong>Abonnementname</strong></li></ul><p/>Wenn Sie eine oder mehrere *Groupby*-Felder angeben, haben alle anderen *Groupby* Felder, die Sie nicht angeben, den Wert **All** im Antworttext. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von Multiplayerdaten für Ihr Xbox Live-fähiges Spiel an. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihres Spiels.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die Multiplayerdaten enthalten, wobei jedes Objekt einen Satz von Daten für den angegebenen täglichen oder monatlichen Zeitraum darstellt, die nach den angegebenen **Filter**- und **Groupby**-Werten organisiert sind. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unter den Abschnitten zur [täglichen Multiplayer-Analyse](#daily-multiplayer-analytics) und [monatlichen Multiplayer-Analyse](#monthly-multiplayer-analytics).     |
| @nextLink  | string | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10000 Zeilen mit Daten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |


### <a name="daily-multiplayer-analytics"></a>Tägliche Multiplayer-Analyse

Elemente in dem *Wert*-Array enthalten die folgenden Werte, wenn Sie tägliche Multiplayer-Analysedaten anfordern (d. h., wenn Sie **multiplayerdaily** für den **metricType**-Parameter angeben).

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Das Datum für die Multiplayerdaten. |
| applicationId       | string | Die Store-ID des Spiels, für das Sie Multiplayerdaten abrufen.     |
| applicationName       | string |  Der Name des Spiels, für das Sie Multiplayerdaten abrufen.     |
| market       | string | Der aus zwei Buchstaben bestehende ISO 3166-Ländercode des Markts, von dem die Multiplayerdaten stammen.       |
| packageVersion     | string |  Die vierteiligen Paketversion für das Spiel.  |
| deviceType          | string | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, von dem die Multiplayerdaten stammen:<p/><ul><li><strong>Verwaltungskonsole</strong></li><li><strong>PC</strong></li><li>**Unbekannt**</li></ul>  |
| subscriptionName     | string |  Der Name des Abonnements, das für die Multiplayerdaten verwendet wird. Mögliche Werte sind **Xbox Game Pass** und **""** (für kein Abonnement).  |
| dailySessionCount     | number |  Die Anzahl der Multiplayersitzungen für das Spiel zum angegebenen Datum.  |
| engagementDurationMinutes     | number |  Die Gesamtanzahl der Minuten, die Kunden mit Multiplayersitzungen für das Spiel zum angegebenen Datum beschäftigt waren.  |
| dailyActiveUsers     | number |  Die Gesamtzahl der aktiven Multiplayerbenutzer des Spiels zum angegebenen Datum.  |
| dailyActiveDevices     | number |  Die Gesamtzahl der aktiven Geräte, auf denen Multiplayersitzungen des Spiels zum angegebenen Datum gespielt wurden.  |
| dailyNewUsers     | number |  Die Gesamtzahl der neuen Multiplayerbenutzer des Spiels zum angegebenen Datum.  |
| monthlyActiveUsers     | number |  Die Gesamtzahl der aktiven Multiplayerbenutzer des Spiels in dem Monat, in dem das angegebene Datum eintrat.  |
| monthlyActiveDevices     | number | Die Gesamtzahl der aktiven Geräte, auf denen Multiplayersitzungen des Spiels in dem Monat gespielt wurden, in dem das angegebene Datum eintrat.   |
| monthlyNewUsers     | number |  Die Gesamtzahl der neuen Multiplayerbenutzer des Spiels in dem Monat, in dem das angegebene Datum eintrat.  |


### <a name="monthly-multiplayer-analytics"></a>Monatliche Multiplayer-Analyse

Elemente in dem *Wert*-Array enthalten die folgenden Werte, wenn Sie monatliche Multiplayer-Analysedaten anfordern (d. h., wenn Sie **multiplayermonthly** für den **metricType**-Parameter angeben).

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Das erste Datum des Monats für die Multiplayerdaten. |
| applicationId       | string | Die Store-ID des Spiels, für das Sie Multiplayerdaten abrufen.     |
| applicationName       | string |  Der Name des Spiels, für das Sie Multiplayerdaten abrufen.     |
| market       | string | Der aus zwei Buchstaben bestehende ISO 3166-Ländercode des Markts, von dem die Multiplayerdaten stammen.       |
| packageVersion     | string |  Die vierteiligen Paketversion für das Spiel.  |
| deviceType          | string | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, von dem die Multiplayerdaten stammen:<p/><ul><li><strong>Verwaltungskonsole</strong></li><li><strong>PC</strong></li><li>**Unbekannt**</li></ul>  |
| subscriptionName     | string |  Der Name des Abonnements, das für die Multiplayerdaten verwendet wird. Mögliche Werte sind **Xbox Game Pass** und **""** (für kein Abonnement).  |
| monthlySessionCount     | number |  Die Anzahl der Multiplayersitzungen für das Spiel während des angegebenen Monats.   |
| engagementDurationMinutes     | number |  Die Gesamtanzahl der Minuten, die Kunden mit Multiplayersitzungen des Spiels während des angegebenen Monats beschäftigt waren.  |
| monthlyActiveUsers     | number | Die Gesamtzahl der aktiven Multiplayerbenutzer während des angegebenen Monats.   |
| monthlyActiveDevices     | number | Die Gesamtzahl der aktiven Geräte, auf denen Multiplayersitzungen des Spiels während des angegebenen Monats gespielt wurden.   |
| monthlyNewUsers     | number |  Die Gesamtzahl der neuen Multiplayerbenutzer des Spiels während des angegebenen Monats.  |
| averageDailyActiveUsers     | number |  Die Durchschnittsanzahl der täglichen, aktiven Multiplayerbenutzer des Spiels während des angegebenen Monats.  |
| averageDailyActiveDevices     | number |  Die Durchschnittsanzahl der aktiven Geräte, auf denen Multiplayersitzungen des Spiels während des angegebenen Monats gespielt wurden.  |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für den JSON-Antworttext für die tägliche Metrik-Variante dieser Anforderung (d. h., wenn Sie **multiplayerdaily** für den **MetricType**-Parameter angeben).

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analytics-Daten](get-xbox-live-analytics.md)
* [Abrufen von Daten für Xbox Live Erfolge](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Health-Daten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live-Spiel Hub-Daten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Club-Daten](get-xbox-live-club-data.md)
