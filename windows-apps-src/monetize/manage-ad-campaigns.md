---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Verwenden Sie diese Methode in der Microsoft Store Promotions-API, um Werbekampagnen zu erstellen, zu bearbeiten und zu erstellen.
title: Verwalten von Anzeigenkampagnen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store promotionapi, Werbekampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 0c985c9a53c233ae0c433567fcd0f88da1428542
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619626"
---
# <a name="manage-ad-campaigns"></a>Verwalten von Anzeigenkampagnen

Verwenden Sie diese Methoden in der [Microsoft Store Promotions-API](run-ad-campaigns-using-windows-store-services.md) , um Werbekampagnen für Ihre APP zu erstellen, zu bearbeiten und zu erhalten. Jede Kampagne, die Sie mit dieser Methode erstellen, kann nur einer APP zugeordnet werden.

> &nbsp; Hinweis &nbsp; Sie können auch Ad-Kampagnen mithilfe von Partner Center erstellen und verwalten, und auf Kampagnen, die Sie Programm gesteuert erstellen, können Sie im Partner Center zugreifen. Weitere Informationen zum Verwalten von Ad-Kampagnen in Partner Center finden [Sie unter Erstellen einer AD-Kampagne für Ihre APP](./index.md).

Wenn Sie diese Methoden verwenden, um eine Kampagne zu erstellen oder zu aktualisieren, rufen Sie in der Regel auch eine oder mehrere der folgenden Methoden auf, um die Übermittlungs Zeilen, *Ziel profile* *und die* der Kampagne zugeordneten *Erstellungs* Methoden zu verwalten. Weitere Informationen über die Beziehung zwischen Kampagnen, Übermittlungs Zeilen, Ziel Profilen und Erstellungen finden Sie unter [Ausführen von Ad-Kampagnen mit Microsoft Store-Diensten](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

* [Verwalten von Übermittlungs Zeilen für Werbekampagnen](manage-delivery-lines-for-ad-campaigns.md)
* [Verwalten von Ziel Profilen für Werbekampagnen](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von kreativen für Werbekampagnen](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>Voraussetzungen

Um diese Methoden zu verwenden, müssen Sie zunächst die folgenden Schritte ausführen:

* Wenn Sie dies noch nicht getan haben, müssen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store promotionapi erfüllen.

  > &nbsp; Hinweis &nbsp; Stellen Sie im Rahmen der Voraussetzungen sicher, dass Sie mindestens [eine bezahlte Werbekampagne in Partner Center erstellen](./index.md) und mindestens ein Zahlungsinstrument für die Werbekampagne in Partner Center hinzufügen. Mit Übermittlungs Zeilen für Ad-Kampagnen, die Sie mit dieser API erstellen, wird automatisch das standardmäßige Zahlungsinstrument abgerechnet, das auf der Seite mit den **Werbekampagnen** im Partner Center

* Rufen Sie [ein Azure AD Zugriffs Token](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) ab, das im-Anforderungs Header für diese Methoden verwendet werden soll. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.


## <a name="request"></a>Anforderung

Diese Methoden verfügen über die folgenden URIs.

| Methodentyp | Anforderungs-URI                                                      |  BESCHREIBUNG  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Erstellt eine neue Werbekampagne.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Bearbeitet die von *campaignid* angegebene Werbekampagne.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Ruft die von *campaignid* angegebene Werbekampagne ab.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Fragt nach Ad-Kampagnen ab. Informationen zu den unterstützten Abfrage Parametern finden Sie im Abschnitt " [Parameter](#parameters) ".  |


### <a name="header"></a>Header

| Header        | type   | BESCHREIBUNG         |
|---------------|--------|---------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |
| Nachverfolgungs-ID   | GUID   | Optional. Eine ID, die den aufrufflow nachverfolgt.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>Parameter

Die Get-Methode zum Abfragen von Ad-Kampagnen unterstützt die folgenden optionalen Abfrage Parameter.

| Name        | type   |  BESCHREIBUNG      |    
|-------------|--------|---------------|
| skip  |  INT   | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise ruft FETCH = 10 und Skip = 0 die ersten 10 Daten Zeilen ab, Top = 10 und Skip = 10 Ruft die nächsten 10 Daten Zeilen ab usw.    |       
| fetch  |  INT   | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen.    |       
| campaignsetsortcolumn  |  Zeichenfolge   | Ordnet die [Kampagnen](#campaign) Objekte im Antworttext mit dem angegebenen Feld an. Die Syntax ist " <em>campaignsetsortcolumn = Field</em>", wobei der <em>Feld</em> Parameter eine der folgenden Zeichen folgen sein kann:</p><ul><li><strong>id</strong></li><li><strong>"kreateddatetime"</strong></li></ul><p>Der Standardwert ist " **kreateddatetime**".     |     
| nicht absteigend  |  Boolean   | Sortiert die [Kampagnen](#campaign) Objekte im Antworttext in absteigender oder aufsteigender Reihenfolge.   |         
| storeproductid  |  Zeichenfolge   | Verwenden Sie diesen Wert, um nur die Ad-Kampagnen zurückzugeben, die der APP mit der angegebenen [Speicher-ID](in-app-purchases-and-trials.md#store-ids)zugeordnet sind. Eine Beispiel-Speicher-ID für ein Produkt ist 9nblggh42cfd.   |         
| label  |  Zeichenfolge   | Verwenden Sie diesen Wert, um nur die Werbekampagnen zurückzugeben, die die angegebene *Bezeichnung* im [Kampagnen](#campaign) Objekt enthalten.    |


### <a name="request-body"></a>Anforderungstext

Die Post-und Put-Methoden erfordern einen JSON-Anforderungs Text mit den erforderlichen Feldern eines [Kampagnen](#campaign) Objekts und zusätzlichen Feldern, die Sie festlegen oder ändern möchten.


### <a name="request-examples"></a>Beispiele für Anforderungen

Im folgenden Beispiel wird veranschaulicht, wie die Post-Methode aufgerufen wird, um eine Werbekampagne zu erstellen.

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

Im folgenden Beispiel wird veranschaulicht, wie Sie die Get-Methode aufrufen, um eine bestimmte Werbekampagne abzurufen.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie die Get-Methode aufgerufen wird, um eine Reihe von Ad-Kampagnen abzufragen, sortiert nach dem Erstellungsdatum.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Antwort

Diese Methoden geben einen JSON-Antworttext mit einem oder mehreren [Kampagnen](#campaign) Objekten zurück, abhängig von der Methode, die Sie aufgerufen haben. Im folgenden Beispiel wird ein Antworttext für die Get-Methode für eine bestimmte Kampagne veranschaulicht.

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

Die Anforderungs-und Antwort Texte für diese Methoden enthalten die folgenden Felder. Diese Tabelle zeigt, welche Felder schreibgeschützt sind (was bedeutet, dass Sie in der Put-Methode nicht geändert werden können) und welche Felder im Anforderungs Text für die Post-Methode erforderlich sind.

| Feld        | type   |  BESCHREIBUNG      |  Nur Lesezugriff  | Standard  | Für Post erforderlich |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  integer   |  Die ID der Anzeigenkampagne.     |   Ja    |      |  Nein     |       
|  name   |  Zeichenfolge   |   Der Name der Anzeigenkampagne.    |    Nein   |      |  Ja     |       
|  Konfigurations Status   |  Zeichenfolge   |  Einer der folgenden Werte, der den Status der vom Entwickler angegebenen AD-Kampagne angibt: <ul><li>**Aktiv**</li><li>**Inaktiv**</li></ul>     |  Nein     |  Aktiv    |   Yes    |       
|  effectivestatus   |  Zeichenfolge   |   Einer der folgenden Werte, der den effektiven Status der AD-Kampagne basierend auf der Systemvalidierung angibt: <ul><li>**Aktiv**</li><li>**Inaktiv**</li><li>**Verarbeitung**</li></ul>    |    Ja   |      |   Nein      |       
|  effectivestatus Reasons   |  array   |  Einer oder mehrere der folgenden Werte, die den Grund für den effektiven Status der AD-Kampagne angeben: <ul><li>**Adkreativesininaktiv**</li><li>**Billingfailed**</li><li>**Adlinesinaktiv**</li><li>**ValidationFailed**</li><li>**Fehler**</li></ul>      |  Ja     |     |    Nein     |       
|  storeproductid   |  Zeichenfolge   |  Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für die APP, mit der diese AD-Kampagne verknüpft ist. Eine Beispiel-Speicher-ID für ein Produkt ist 9nblggh42cfd.     |   Ja    |      |  Ja     |       
|  Bezeichnungen   |  array   |   Eine oder mehrere Zeichen folgen, die benutzerdefinierte Bezeichnungen für die Kampagne darstellen. Diese Bezeichnungen werden zum Suchen und Tagging von Kampagnen verwendet.    |   Nein    |  NULL    |    Nein     |       
|  type   | Zeichenfolge    |  Einer der folgenden-Werte, der den Kampagnen Typen angibt: <ul><li>**Kostenpflichtig**</li><li>**Pfarrhaus**</li><li>**Community**</li></ul>      |   Ja    |      |   Ja    |       
|  Ziel   |  Zeichenfolge   |  Einer der folgenden-Werte, der das Ziel der Kampagne angibt: <ul><li>**Drivein Stall**</li><li>**Drivereengagement**</li><li>**Drivanapppurchase**</li></ul>     |   Nein    |  Drivein Stall    |   Yes    |       
|  lines   |  array   |   Mindestens ein-Objekt, das die der Werbekampagne zugeordneten Übermittlungs [Zeilen](manage-delivery-lines-for-ad-campaigns.md#line) identifiziert. Jedes Objekt in diesem Feld besteht aus einem *ID* -und einem *namens* Feld, das die ID und den Namen der Übermittlungs Zeile angibt.     |   Nein    |      |    Nein     |       
|  createdDate   |  Zeichenfolge   |  Das Datum und die Uhrzeit der Erstellung der Werbekampagne im ISO 8601-Format.     |  Ja     |      |     Nein    |


## <a name="related-topics"></a>Zugehörige Themen

* [Ausführen von Ad-Kampagnen mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Verwalten von Übermittlungs Zeilen für Werbekampagnen](manage-delivery-lines-for-ad-campaigns.md)
* [Verwalten von Ziel Profilen für Werbekampagnen](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von kreativen für Werbekampagnen](manage-creatives-for-ad-campaigns.md)
* [Abrufen der Leistungsdaten einer Anzeigenkampagne](get-ad-campaign-performance-data.md)