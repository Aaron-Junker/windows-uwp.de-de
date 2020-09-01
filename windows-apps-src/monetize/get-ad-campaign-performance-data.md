---
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um in einem bestimmten Datumsbereich und anderen optionalen Filtern aggregierte Leistungsdaten für die AD-Kampagne für die angegebene Anwendung zu erhalten.
title: Abrufen der Leistungsdaten einer Anzeigenkampagne
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Werbekampagnen
ms.localizationpriority: medium
ms.openlocfilehash: fd933f103bf8964997102731b653e125fae89e12
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162444"
---
# <a name="get-ad-campaign-performance-data"></a>Abrufen der Leistungsdaten einer Anzeigenkampagne


Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um eine aggregierte Zusammenfassung der Leistungsdaten der Werbekampagne für Ihre Anwendungen in einem bestimmten Datumsbereich und anderen optionalen Filtern zu erhalten. Diese Methode gibt die Daten im JSON-Format zurück.

Diese Methode gibt die gleichen Daten zurück, die vom [AD-Kampagnenbericht](/windows/uwp/publish/ad-campaign-report) im Partner Center bereitgestellt werden. Weitere Informationen zu Anzeigenkampagnen finden Sie unter [Erstellen einer Anzeigenkampagne für Ihre App](../publish/create-an-ad-campaign-for-your-app.md).

Zum Erstellen, aktualisieren oder Abrufen von Details für Ad-Kampagnen können Sie die Methoden zum [Verwalten von Ad-Kampagnen](manage-ad-campaigns.md) in der [Microsoft Store Promotions-API](run-ad-campaigns-using-windows-store-services.md)verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                |
|---------------|--------|---------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Um die Leistungsdaten einer Anzeigenkampagne für eine bestimmte App abzurufen, verwenden Sie den Parameter *applicationId*. Um Anzeigenleistungsdaten für alle Apps abzurufen, die Ihrem Entwicklerkonto zugeordnet sind, lassen Sie den Paramater *applicationId* aus.

| Parameter     | Typ   | BESCHREIBUNG     | Erforderlich |
|---------------|--------|-----------------|----------|
| applicationId   | Zeichenfolge    | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP, für die Sie Leistungsdaten der Werbekampagne abrufen möchten. |    Nein      |
|  startDate  |  date   |  Das Startdatum im Datumsbereich der abzurufenden Leistungsdaten einer Anzeigenkampagne im Format JJJJ/MM/TT. Der Standardwert ist das aktuelle Datum minus 30 Tage.   |   Nein    |
| endDate   |  date   |  Das Enddatum im Datumsbereich der abzurufenden Leistungsdaten einer Anzeigenkampagne im Format JJJJ/MM/TT. Der Standardwert ist das aktuelle Datum minus einen Tag.   |   Nein    |
| top   |  INT   |  Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können.   |   Nein    |
| skip   | INT    |  Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw.   |   Nein    |
| filter   |  Zeichenfolge   |  Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Der einzige unterstützte Filter ist **campaignId**. Alle Anweisungen können die Operatoren **eq** oder **ne** verwenden. Zudem können sie mit **and** oder **or** kombiniert werden.  Hier finden Sie ein Beispiel für den Parameter *filter*: ```filter=campaignId eq '100023'```.   |   Nein    |
|  aggregationLevel  |  Zeichenfolge   | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: <strong>day</strong>, <strong>week</strong> oder <strong>month</strong>. Wenn keine Angabe erfolgt, lautet der Standardwert <strong>day</strong>.    |   Nein    |
| orderby   |  Zeichenfolge   |  <p>Eine Anweisung, die die Ergebnisdatenwerte für die Leistungsdaten einer Anzeigenkampagne anfordert. Die Syntax lautet <em>OrderBy = Field [Order], Field [Order],...</em>. Der <em>Feld</em> Parameter kann eine der folgenden Zeichen folgen sein:</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p>Der Parameter <em>order</em> ist optional und kann <strong>asc</strong> oder <strong>desc</strong> sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist <strong>ASC</strong>.</p><p>Hier ist ein Beispiel für eine <em>OrderBy</em> -Zeichenfolge: <em>OrderBy = Date, campaignid</em></p>   |   Nein    |
|  groupby  |  Zeichenfolge   |  <p>Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben:</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p>Der Parameter <em>groupby</em> kann mit dem Parameter <em>aggregationLevel</em> verwendet werden. Beispiel: <em> &amp; GroupBy = ApplicationId &amp; aggregationlevel = Week</em></p>   |   Nein    |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt verschiedene Anforderungen für das Abrufen der Leistungsdaten einer Anzeigenkampagne auf.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | BESCHREIBUNG  |
|------------|--------|---------------|
| Wert      | array  | Ein Array von Objekten mit den aggregierten Leistungsdaten einer Anzeigenkampagne. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Kampagnenleistungsobjekt](#campaign-performance-object).          |
| @nextLink  | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 5 festgelegt ist, es jedoch mehr als fünf Datenelemente für die Abfrage gibt. |
| TotalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>Kampagnenleistungsobjekt

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | BESCHREIBUNG            |
|---------------------|--------|------------------------|
| date                | Zeichenfolge | Das erste Datum im Datumsbereich für die Leistungsdaten einer Anzeigenkampagne. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId       | Zeichenfolge | Die Store-ID der App, für die Sie die Leistungsdaten einer Anzeigenkampagne abrufen.                     |
| campaignId     | Zeichenfolge | Die ID der Anzeigenkampagne.           |
| LineID     | Zeichenfolge |    Die ID der Ad-Kampagnen Übermittlungs [Zeile](manage-delivery-lines-for-ad-campaigns.md) , die diese Leistungsdaten generiert hat.        |
| currencyCode              | Zeichenfolge | Der Währungscode für das Kampagnenbudget.              |
| spend          | Zeichenfolge |  Der Budgetbetrag, der in die Anzeigenkampagne investiert wurde.     |
| impressions           | long | Die Anzahl der Anzeigenaufrufe für die Kampagne.        |
| installs              | long | Die Anzahl der App-Installationen im Zusammenhang mit der Kampagne.   |
| clicks            | long | Die Anzahl der Klicks für die Kampagne.      |
| iapinstallationen            | long | Die Anzahl der Add-on-Installationen (auch als in-App-Käufe bezeichnet), die mit der Kampagne verknüpft sind.      |
| activeusers            | long | Die Anzahl der Benutzer, die auf eine Werbung geklickt haben, die Teil der Kampagne ist und an die APP zurückgegeben wurde.      |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "lineId": "0001",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8,
      "iapInstalls": 0,
      "activeUsers": 0
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "lineId": "0002",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5,
      "iapInstalls": 0,
      "activeUsers": 0
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen einer Anzeigenkampagne für Ihre App](../publish/create-an-ad-campaign-for-your-app.md)
* [Ausführen von Ad-Kampagnen mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)