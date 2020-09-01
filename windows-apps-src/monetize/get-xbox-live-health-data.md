---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Xbox Live Health-Daten zu erhalten.
title: Abrufen von Xbox Live-Integritätsdaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Xbox Live Analytics, Health, Client Fehler
ms.localizationpriority: medium
ms.openlocfilehash: 3cf2432dfa9b4882f6fe878e3a1b832240735cb5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174994"
---
# <a name="get-xbox-live-health-data"></a>Abrufen von Xbox Live-Integritätsdaten


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Integritäts Daten für das [Live aktivierte Xbox-Spiel](/gaming/xbox-live/index.md)zu erhalten. Diese Informationen sind auch im Bericht " [Xbox Analytics](../publish/xbox-analytics-report.md) " im Partner Center verfügbar.

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
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie die Xbox Live Health-Daten abrufen möchten.  |  Ja  |
| metrictype | Zeichenfolge | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analytics-Daten angibt. Geben Sie für diese Methode den Wert **callingpattern**an.  |  Ja  |
| startDate | date | Das Startdatum im Datumsbereich der abzurufenden Integritäts Daten. Der Standardwert ist 30 Tage vor dem aktuellen Datum. |  Nein  |
| endDate | date | Das Enddatum im Datumsbereich von Integritäts Daten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. |  Nein  |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |
| filter | Zeichenfolge  | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter *filter* müssen von einfachen Anführungszeichen eingeschlossen werden. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>den DeviceType "</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | Nein   |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder aus dem Antworttext angeben:<p/><ul><li><strong>date</strong></li><li><strong>den DeviceType "</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>Wenn Sie ein oder mehrere *GroupBy* -Felder angeben, haben alle anderen *GroupBy* -Felder, die Sie nicht angeben, den Wert **alle** im Antworttext. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum erhalten von Integritäts Daten für Ihr Xbox Live-fähiges Spiel. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID für Ihr Spiel.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von-Objekten, die Integritäts Daten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.                                                                                                                      |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Dieser Wert wird z. b. zurückgegeben, wenn der **Top** -Parameter der Anforderung auf 10000 festgelegt ist, aber mehr als 10000 Daten Zeilen für die Abfrage vorhanden sind. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.   |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | BESCHREIBUNG                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | Zeichenfolge | Die Speicher-ID des Spiels, für das Sie Integritäts Daten abrufen.     |
| date                | Zeichenfolge | Das erste Datum im Datumsbereich für die Integritäts Daten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| deviceType          | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, auf dem das Spiel verwendet wurde:<p/><ul><li><strong>Xboxone</strong></li><li><strong>Windowsonecore</strong> (dieser Wert weist auf einen PC hin)</li><li><strong>Unbekannt</strong></li></ul>  |
| sandboxId     | Zeichenfolge |   Die Sandbox-ID, die für das Spiel erstellt wurde. Hierbei kann es sich um den Wert Retail oder die ID eines privaten Sandkastens handeln.   |
| packageVersion     | Zeichenfolge |  Die vierteilige Paketversion für das Spiel.  |
| callingpattern     | Objekt (object) |  Ein [callingpattern](#callingpattern) -Objekt, das Dienst Antworten, Geräte und Benutzerdaten für jeden Statuscode bereitstellt, der von jedem Xbox Live-Dienst zurückgegeben wird, der von Ihrem Spiel für den angegebenen Datumsbereich verwendet wird.     |


### <a name="callingpattern"></a>callingpattern

| Wert      | Typ   | BESCHREIBUNG                  |
|------------|--------|-------------------------------------------------------|
| Dienst      | Zeichenfolge  |   Der Name des Xbox Live-Dienstanbieter, auf den sich die Integritäts Daten beziehen.       |
| endpoint      | Zeichenfolge  |   Der Endpunkt des Xbox Live-Dienstanbieter, auf den sich die Integritäts Daten beziehen.        |
| HttpStatusCode      | Zeichenfolge  |  Der HTTP-Statuscode für diesen Satz von Integritäts Daten.<p/><p/>**Note** &nbsp; Hinweis &nbsp; Der Statuscode **429e** gibt an, dass der Dienst nur erfolgreich ausgeführt wurde, weil eine differenzierte [Raten Begrenzung](/gaming/xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md) während des Aufrufes ausgenommen war. Eine differenzierte Raten Beschränkung könnte in Zukunft erzwungen werden, wenn der Dienst ein hohes Volumen hat, und in diesem Fall würde der-Befehl zu einem [http 429-Statuscode](/gaming/xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object)führen.         |
| serviceresponses      | number  | Die Anzahl der Dienst Antworten, die den angegebenen Statuscode zurückgegeben haben.         |
| uniquedevices      | number  |  Die Anzahl eindeutiger Geräte, von denen der Dienst aufgerufen und der angegebene Statuscode empfangen wurde.       |
| uniqueUsers      | number  |   Die Anzahl der eindeutigen Benutzer, die den angegebenen Statuscode empfangen haben.       |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung. Die in diesem Beispiel gezeigten Dienstnamen und Endpunkte sind nicht real und dienen nur zu Beispiel Zwecken.

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

## <a name="related-topics"></a>Zugehörige Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analysedaten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live Spielehubdaten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)