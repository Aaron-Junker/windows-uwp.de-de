---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Verwenden Sie diese Methode in der Microsoft Store-Angebots-API, um Werbeanzeigenkampagnen zu erstellen, zu bearbeiten und abzurufen.
title: Verwalten von Anzeigenkampagnen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Werbungs-API, Anzeigenkampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 6529c1a21865b2997d36e9b254b19f971f620490
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633225"
---
# <a name="manage-ad-campaigns"></a>Verwalten von Anzeigenkampagnen

Verwenden Sie diese Methoden in der [Microsoft Store-Angebots-API-](run-ad-campaigns-using-windows-store-services.md), um werbende Anzeigenkampagnen zu erstellen, zu bearbeiten und abzurufen. Jede Kampagne, die Sie erstellen, mit dieser Methode kann nur eine App zugeordnet werden.

>**Beachten Sie**&nbsp;&nbsp;können auch erstellen und Verwalten von Ad-Kampagnen über Partner Center und Kampagnen, die Sie programmgesteuert erstellen im Partner Center erfolgen können. Weitere Informationen zum Verwalten von Ad-Kampagnen in Partner Center finden Sie unter [erstellen Sie eine Ad-Kampagne für Ihre app](../publish/create-an-ad-campaign-for-your-app.md).

Wenn Sie eine Kampagne mit dieser Methode erstellen oder aktualisieren, rufen Sie in der Regel auch eine oder mehrere der folgenden Methoden zum Verwalten der *Lieferpositionen*, *Zielgruppenprofile*, und *Werbemittel* ab, die der Kampagne zugeordnet sind. Weitere Informationen über die Beziehung zwischen Kampagnen, Lieferpositionen, Zielgruppenprofilen und Werbemitteln finden Sie unter [Anzeigenkampagnen mit Microsoft Store-Diensten ausführen](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

* [Übermittlung Zeilen für Ad-Kampagnen verwalten](manage-delivery-lines-for-ad-campaigns.md)
* [Für die Zielgruppenadressierung Profile für Ad-Kampagnen verwalten](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von Werbemitteln für Ad-Kampagnen](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methoden sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store-Werbungs-API.

  >**Beachten Sie**&nbsp;&nbsp;als Teil der erforderlichen Komponenten, achten Sie darauf, die Sie [erstellen Sie mindestens eine kostenpflichtige Ad Kampagne im Partner Center](../publish/create-an-ad-campaign-for-your-app.md) und Sie mindestens eine Zahlungsmethode für die Ad-Kampagne Partner hinzufügen Im Mittelpunkt. Übermittlung von Zeilen für Ad-Kampagnen, die Sie erstellen diese API verwenden berechnet automatisch die standardmäßigen Zahlungsmethode, die ausgewählt werden, auf die **Anzeigenkampagnen** Seite im Partner Center.

* [Rufen Sie ein Azure AD-Zugriffstoken ab](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token), das in der Anforderungskopfzeile für diese Methoden verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.


## <a name="request"></a>Anfordern

Diese Methoden haben die folgenden URIs.

| Methodentyp | Anforderungs-URI                                                      |  Beschreibung  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Erstellt eine neue Anzeigenkampagne.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Bearbeitet die mit der *Kampagnen-ID* näher bezeichnete Anzeigenkampagne.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Ruft die mit der *Kampagnen-ID* näher bezeichnete Anzeigenkampagne ab.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Abfragen für Anzeigenkampagnen. Siehe [Parameter](#parameters) Abschnitt zu den unterstützten Abfrageparametern.  |


### <a name="header"></a>Header

| Header        | Typ   | Beschreibung         |
|---------------|--------|---------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |
| Tracking-ID   | GUID   | Optional. Eine ID, die den Abfrageablauf verfolgt.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>Parameter

Die GET-Methode zum Abfragen für Anzeigenkampagnen unterstützt die folgenden optionalen Abfrageparameter.

| Name        | Typ   |  Beschreibung      |    
|-------------|--------|---------------|------|
| skip  |  int   | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „fetch=10“ und „skip=0“ die ersten 10 Datenzeilen ab, „top=10“ und „skip=10“ die nächsten 10 Datenzeilen usw.    |       
| fetch  |  int   | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen.    |       
| campaignSetSortColumn  |  string   | Sortiert die Objekte der [Kampagne](#campaign) im Antworttext nach dem angegebenen Feld. Die Syntax lautet <em>CampaignSetSortColumn = field</em>, wobei der Parameter <em>field</em> eine der folgenden Zeichenfolgen sein kann:</p><ul><li><strong>ID</strong></li><li><strong>CreatedDateTime:</strong></li></ul><p>Der Standardwert ist **createdDateTime**.     |     
| isDescending  |  Boolesch   | Sortiert die Objekte der [Kampagne](#campaign) im Antworttext in absteigender oder aufsteigender Reihenfolge.   |         
| storeProductId  |  string   | Verwenden Sie diesen Wert, um nur die Anzeigenkampagnen zurückgegeben, die der App mit der angegebenen ID zugeordnet sind [Store-ID](in-app-purchases-and-trials.md#store-ids). Ein Beispiel für eine Store-ID eines Produkts ist 9nblggh42cfd.   |         
| label  |  string   | Verwenden Sie diesen Wert nur Anzeigenkampagnen zurückgegeben, die der angegebenen enthalten *Bezeichnung* in der [Kampagne](#campaign) Objekt.    |       |    


### <a name="request-body"></a>Anforderungstext

Die POST und PUT-Methoden erfordern einen JSON-Anforderungstext mit den erforderlichen Feldern für ein Objekt einer [Kampagne](#campaign) und alle zusätzlichen Felder, die Sie festlegen oder ändern möchten.


### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie die POST-Methode zum Erstellen einer Anzeigenkampagne aufgerufen wird.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

Im folgenden Beispiel wird veranschaulicht, wie die GET-Methode zum Abrufen einer bestimmten Anzeigenkampagne aufgerufen wird.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie die GET-Methode zur Abfrage einer nach dem Erstellungsdatum sortierten Gruppe von Anzeigenkampagnen aufgerufen wird.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Antwort

Diese Methoden geben je nach aufgerufener Methode einen JSON-Antworttext mit einem oder mehreren [Kampagne](#campaign)-Objekten zurück. Das folgende Beispiel zeigt eine Antworttext für die GET-Methode für eine bestimmte Kampagne.

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>Kampagnenobjekt

Die Anforderungs- und Antworttexte für diese Methoden enthalten die folgenden Felder. Die folgende Tabelle zeigt, welche Felder schreibgeschützt sind (d. h. sie können in der PUT-Methode nicht geändert werden) und welche Felder in dem Anforderungstext für die POST-Methode erforderlich sind.

| Feld        | Typ   |  Beschreibung      |  Schreibgeschützt  | Standard  | Erforderlich für POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  Ganzzahl   |  Die ID der Anzeigenkampagne.     |   Ja    |      |  Nein     |       
|  name   |  string   |   Der Name der Anzeigenkampagne.    |    Nein   |      |  Ja     |       
|  configuredStatus   |  string   |  Einer der folgenden Werte, der den Status der vom Entwickler angegebenen Anzeigenkampagne angibt: <ul><li>**aktiv**</li><li>**inaktiv**</li></ul>     |  Nein     |  Aktiv    |   Ja    |       
|  effectiveStatus   |  string   |   Einer der folgenden Werte, der den effektiven Status der Anzeigenkampagne basierend auf der Systemüberprüfung angibt: <ul><li>**aktiv**</li><li>**inaktiv**</li><li>**Verarbeitung**</li></ul>    |    Ja   |      |   Nein      |       
|  effectiveStatusReasons   |  array   |  Einer oder mehrere der folgenden Werte, die den Grund für den effektive Status der Anzeigenkampagne angeben: <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**Fehlgeschlagen**</li></ul>      |  Ja     |     |    Nein     |       
|  storeProductId   |  string   |  Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, der diese Anzeigenkampagne zugeordnet ist. Ein Beispiel für eine Store-ID eines Produkts ist 9nblggh42cfd.     |   Ja    |      |  Ja     |       
|  Beschriftungen   |  array   |   Eine oder mehrere Zeichenfolgen, die benutzerdefinierte Beschriftungen für die Kampagne darstellen. Diese Beschriftungen werden für das Suchen und Kennzeichnen von Kampagnen verwendet.    |   Nein    |  Null    |    Nein     |       
|  type   | string    |  Einer der folgenden Werte, der den Kampagnentyp angibt: <ul><li>**Kostenpflichtige**</li><li>**Haus**</li><li>**Community**</li></ul>      |   Ja    |      |   Ja    |       
|  Ziel   |  string   |  Einer der folgenden Werte, der das Ziel der Kampagne angibt: <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   Nein    |  DriveInstall    |   Ja    |       
|  Zeilen   |  array   |   Ein oder mehrere Objekte, die die [Lieferpositionen](manage-delivery-lines-for-ad-campaigns.md#line) identifizieren, die der Kampagne zugeordnet sind. Jedes Objekt in diesem Feld umfasst ein Feld *ID* und *Name*, das die ID und den Namen der Lieferposition angibt.     |   Nein    |      |    Nein     |       
|  createdDate   |  string   |  Das Datum und die Uhrzeit, zu dem die Anzeigenkampagne erstellt wurde (im ISO 8601-Format).     |  Ja     |      |     Nein    |       |


## <a name="related-topics"></a>Verwandte Themen

* [Ausführen von Ad-Kampagnen, die mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Übermittlung Zeilen für Ad-Kampagnen verwalten](manage-delivery-lines-for-ad-campaigns.md)
* [Für die Zielgruppenadressierung Profile für Ad-Kampagnen verwalten](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von Werbemitteln für Ad-Kampagnen](manage-creatives-for-ad-campaigns.md)
* [Ad-Kampagne-Leistungsdaten abrufen](get-ad-campaign-performance-data.md)
