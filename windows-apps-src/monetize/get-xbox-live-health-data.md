---
author: Xansky
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Xbox Live Integritätsdaten abzurufen.
title: Abrufen von Xbox Live Integritätsdaten
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, Uwp, Store-Diensten, Microsoft Store-Analyse-API, Xbox Live-Analyse, Integrität, Clientfehler
ms.localizationpriority: medium
ms.openlocfilehash: e7802db965ee4d1515090270125430871544d9f1
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "5407241"
---
# <a name="get-xbox-live-health-data"></a>Abrufen von Xbox Live Integritätsdaten


Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Integritätsdaten für Ihr [Xbox Live-fähiges Spiel](../xbox-live/index.md) abzurufen. Diese Informationen sind auch im [Xbox Analyse-Bericht](../publish/xbox-analytics-report.md) im Windows Dev Center-Dashboard verfügbar.

> [!IMPORTANT]
> Diese Methode unterstützt nur Spiele für Xbox oder Spiele, die Xbox Live-Dienste verwenden. Diese Spiele müssen den [Konzeptgenehmigungsprozess](../gaming/concept-approval.md) durchlaufen, der Spiele umfasst, die von [Microsoft-Partnern](../xbox-live/developer-program-overview.md#microsoft-partners) veröffentlicht wurden, sowie Spiele, die über das [ID@Xbox-Programm](../xbox-live/developer-program-overview.md#id) übermittelt wurden. Diese Methode unterstützt derzeit keine Spiele, die über das [Xbox Live Creators-Programm](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) eingereicht wurden.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter


| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | String | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie die Xbox Live Integritätsdaten abrufen möchten.  |  Ja  |
| metricType | String | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analysedaten angibt. Geben Sie für diese Methode den Wert **callingpattern** an.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der Integritätsdaten, die abgerufen werden sollen. Der Standardwert ist 30Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich der Integritätsdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. |  Nein  |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Wenn die Abfrage keine weiteren Zeilen enthält, entält der Antworttext den Link „Weiter“, den Sie verwenden können, um die nächste Seite mit Daten anzufordern. |  Nein  |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000Datenzeilen usw. |  Nein  |
| filter | string  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | Nein   |
| groupby | string | Eine Anweisung, die Datenaggregationen nur auf die angegebenen Felder anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>Wenn Sie eine oder mehrere *Groupby*-Felder angeben, haben alle anderen *Groupby* Felder, die Sie nicht angeben, den Wert **All** im Antworttext. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von Integritätsdaten für Ihr Xbox Live-fähiges Spiel an. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihres Spiels.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | Array  | Ein Array von Objekten, die Integritätsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.                                                                                                                      |
| @nextLink  | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10000 Zeilen mit Daten für die Abfrage gibt. |
| TotalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.   |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | String | Die Store-ID des Spiels, für das Sie Integritätsdaten abrufen.     |
| date                | String | Das erste Datum im Datumsbereich für die Integritätsdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| deviceType          | String | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, auf dem Ihr Spiel genutzt wurde:<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong> (dieser Wert gibt einen PC an)</li><li><strong>Unbekannt</strong></li></ul>  |
| sandboxId     | String |   Das Sandbox-ID, die für das Spiel erstellt wurde. Dies kann der Wert RETAIL sein oder die ID für die private Sandbox.   |
| packageVersion     | String |  Die vierteiligen Paketversion für das Spiel.  |
| callingPattern     | Objekt |  Ein [callingPattern](#callingpattern)-Objekt, das Dienstantworten, Geräte und Benutzerdaten für jeden Statuscode bereitstellt, der von jedem einzelnen Xbox Live Dienst zurückgegeben wird, der von Ihrem Spiel während des angegebenen Zeitraum genutzt wird.     |


### <a name="callingpattern"></a>callingPattern

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Dienst      | String  |   Der Name des Xbox Live Dienstes, auf den sich die Integritätsdaten beziehen.       |
| endpoint      | String  |   Der Endpunkt des Xbox Live Dienstes, auf den sich die Integritätsdaten beziehen.        |
| httpStatusCode      | String  |  Der HTTP-Statuscode für diesen Satz an Integritätsdaten.<p/><p/>**Hinweis:**&nbsp;&nbsp;Den Statuscode **429E** gibt an, dass der Aufruf des Dienstes nur deshalb erfolgreich war, weil [die differenzierte Begrenzung der Übertragungsrate](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md) während des Aufrufs als Ausnahme gehandhabt wurde. Differenzierte Begrenzung der Übertragungsrate könnte in der Zukunft erzwungen werden, wenn der Dienst ein hohes Aufkommen erfährt und in diesem Fall der Aufruf zu einem [429 HTTP-Statuscode](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object) führen könnte.         |
| serviceResponses      | number  | Die Anzahl der Dienstantworten, die den angegebenen Status zurückgegeben.         |
| uniqueDevices      | number  |  Die Anzahl der eindeutigen Geräte, die den Dienst aufgerufen und den angegebenen Statuscode empfangen haben.       |
| uniqueUsers      | number  |   Die Anzahl der eindeutigen Benutzer, die den angegebenen Statuscode erhalten haben.       |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung. Die Dienstnamen und Endpunkte, die in diesem Beispiel gezeigt werden, sind nicht real und dienen nur zu Beispielzwecken.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Zugreifen auf Analysedaten mit MicrosoftStore-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live Analysedaten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live Spielehubdaten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)
