---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Xbox Live-multiplayerdaten zu erhalten.
title: Abrufen von Xbox Live Multiplayerdaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Xbox Live Analytics, Multiplayer
ms.localizationpriority: medium
ms.openlocfilehash: 546151e1cf95adc8ecbb64513a66671067869143
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171384"
---
# <a name="get-xbox-live-multiplayer-data"></a>Abrufen von Xbox Live Multiplayerdaten


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um für Ihr [Xbox Live-fähiges Spiel](/gaming/xbox-live/index.md) täglich oder monatlich multiplayerdaten zu erhalten. Diese Informationen sind auch im Bericht " [Xbox Analytics](../publish/xbox-analytics-report.md) " im Partner Center verfügbar.

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
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Xbox Live-multiplayerdaten abrufen möchten.  |  Ja  |
| metrictype | Zeichenfolge | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analytics-Daten angibt. Geben Sie für diese Methode den Wert **multiplayerdaily** an, um tägliche multiplayerdaten und multiplayermonatlich zu erhalten, um monatliche **multiplayerdaten** zu erhalten.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich von multiplayerdaten, die abgerufen werden sollen. Für **multiplayerdaily**beträgt der Standardwert drei Monate vor dem aktuellen Datum. Bei **multiplayermonthly**beträgt der Standardwert 1 Jahr vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich von multiplayerdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>den DeviceType "</strong></li><li><strong>packageVersion</strong></li><li><strong>Marktforschungs</strong></li><li><strong>subscriptionName</strong></li></ul> | Nein   |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>date</strong></li><li><strong>den DeviceType "</strong></li><li><strong>packageVersion</strong></li><li><strong>Marktforschungs</strong></li><li><strong>subscriptionName</strong></li></ul><p/>Wenn Sie ein oder mehrere *GroupBy* -Felder angeben, haben alle anderen *GroupBy* -Felder, die Sie nicht angeben, den Wert **alle** im Antworttext. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum erhalten von multiplayerdaten für Ihr Xbox Live-fähiges Spiel. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID für Ihr Spiel.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die multiplayerdaten enthalten, wobei jedes-Objekt eine Menge von Daten für den angegebenen Tages-oder Monats Zeitraum darstellt und nach dem angegebenen **Filter** und den **GroupBy** -Werten angeordnet ist. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in den Abschnitten zu den [täglichen multiplayeranalysen](#daily-multiplayer-analytics) und den [monatlichen multiplayeranalysen](#monthly-multiplayer-analytics) .     |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10000 festgelegt ist, aber mehr als 10000 Daten Zeilen für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |


### <a name="daily-multiplayer-analytics"></a>Tägliche multiplayeranalysen

Elemente im *Wertarray* enthalten die folgenden Werte, wenn Sie tägliche multiplayeranalysedaten anfordern (d. h., wenn Sie **multiplayerdaily** für den **metrictype** -Parameter angeben).

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| date                | Zeichenfolge | Das Datum der multiplayerdaten. |
| applicationId       | Zeichenfolge | Die Speicher-ID des Spiels, für das Sie multiplayerdaten abrufen.     |
| applicationName       | Zeichenfolge |  Der Name des Spiels, für das Sie multiplayerdaten abrufen.     |
| market       | Zeichenfolge | Der aus zwei Buchstaben bestehende ISO 3166-Ländercode des Markts, von dem die multiplayerdaten stammen.       |
| packageVersion     | Zeichenfolge |  Die vierteilige Paketversion für das Spiel.  |
| deviceType          | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, von dem die multiplayerdaten stammen:<p/><ul><li><strong>Konsole</strong></li><li><strong>PCs</strong></li><li>**Unbekannt**</li></ul>  |
| subscriptionName     | Zeichenfolge |  Der Name des Abonnements, das für die multiplayerdaten verwendet wird. Mögliche Werte sind " **Xbox Game Pass** **" und ""** (für kein Abonnement).  |
| dailysessioncount     | number |  Die Anzahl der multiplayersitzungen für das Spiel am angegebenen Datum.  |
| engagementdurationminutes     | number |  Die Gesamtanzahl der Minuten, die Kunden an dem angegebenen Datum mit multiplayersitzungen für das Spiel beschäftigt waren.  |
| dailyactiveusers     | number |  Die Gesamtanzahl aktiver multiplayerbenutzer für das Spiel am angegebenen Datum.  |
| dailyactivedevices     | number |  Die Gesamtanzahl aktiver Geräte, die für das Spiel an dem angegebenen Datum multiplayersitzungen durchgespielt haben.  |
| dailynewusers     | number |  Die Gesamtanzahl der neuen multiplayerbenutzer für das Spiel am angegebenen Datum.  |
| monthlyactiveusers     | number |  Die Gesamtanzahl aktiver multiplayerbenutzer für den Monat, in dem das angegebene Datum aufgetreten ist.  |
| monthlyactivedevices     | number | Die Gesamtanzahl aktiver Geräte, die Multiplayer-Sitzungen für das Spiel für den Monat abgespielt haben, in dem das angegebene Datum aufgetreten ist.   |
| monthlynewusers     | number |  Die Gesamtanzahl der neuen multiplayerbenutzer für das Spiel für den Monat, in dem das angegebene Datum aufgetreten ist.  |


### <a name="monthly-multiplayer-analytics"></a>Monatliche multiplayeranalysen

Elemente im *Wertarray* enthalten die folgenden Werte, wenn Sie monatliche multiplayeranalysedaten anfordern (d. h., wenn Sie **multiplayermonatlich** für den **metrictype** -Parameter angeben).

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| date                | Zeichenfolge | Das erste Datum des Monats für die multiplayerdaten. |
| applicationId       | Zeichenfolge | Die Speicher-ID des Spiels, für das Sie multiplayerdaten abrufen.     |
| applicationName       | Zeichenfolge |  Der Name des Spiels, für das Sie multiplayerdaten abrufen.     |
| market       | Zeichenfolge | Der aus zwei Buchstaben bestehende ISO 3166-Ländercode des Markts, von dem die multiplayerdaten stammen.       |
| packageVersion     | Zeichenfolge |  Die vierteilige Paketversion für das Spiel.  |
| deviceType          | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, von dem die multiplayerdaten stammen:<p/><ul><li><strong>Konsole</strong></li><li><strong>PCs</strong></li><li>**Unbekannt**</li></ul>  |
| subscriptionName     | Zeichenfolge |  Der Name des Abonnements, das für die multiplayerdaten verwendet wird. Mögliche Werte sind " **Xbox Game Pass** **" und ""** (für kein Abonnement).  |
| monthlysessioncount     | number |  Die Anzahl der multiplayersitzungen für das Spiel innerhalb des angegebenen Monats.   |
| engagementdurationminutes     | number |  Die Gesamtanzahl der Minuten, die Kunden während des angegebenen Monats mit multiplayersitzungen für das Spiel beschäftigt waren.  |
| monthlyactiveusers     | number | Die Gesamtanzahl aktiver multiplayerbenutzer innerhalb des angegebenen Monats.   |
| monthlyactivedevices     | number | Die Gesamtanzahl aktiver Geräte, die während des angegebenen Monats multiplayersitzungen für das Spiel abgespielt haben.   |
| monthlynewusers     | number |  Die Gesamtanzahl der neuen multiplayerbenutzer für das Spiel innerhalb des angegebenen Monats.  |
| averagedailyactiveusers     | number |  Die durchschnittliche Anzahl der täglichen aktiven multiplayerbenutzer für das Spiel innerhalb des angegebenen Monats.  |
| averagedailyactivedevices     | number |  Die durchschnittliche Anzahl aktiver Geräte, die während des angegebenen Monats multiplayersitzungen für das Spiel abgespielt haben.  |


### <a name="response-example"></a>Antwortbeispiel

Im folgenden Beispiel wird ein Beispiel für einen JSON-Antworttext für die Variante der täglichen Metriken dieser Anforderung veranschaulicht (d. h., wenn Sie **multiplayerdaily** für den **metrictype** -Parameter angeben).

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

## <a name="related-topics"></a>Zugehörige Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analysedaten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live Spielehubdaten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)