---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Verwenden Sie diese Methode in der Microsoft Store-Werbungs-API, um Lieferpositionen für Werbeanzeigenkampagnen zu verwalten.
title: Verwalten von Lieferpositionen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Werbungs-API, Anzeigenkampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 363f7034d7e353d9ee110637971e7b848dbca1bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625635"
---
# <a name="manage-delivery-lines"></a>Verwalten von Lieferpositionen

Erstellen Sie mit diesen Methoden in Microsoft Store-Angebote API eine oder mehrere *Lieferpositionen*, um Inventar zu kaufen und Ihre Anzeigen für eine Werbekampagne zu übermitteln. Für jede Lieferposition können Sie Zielgruppen und Ihren Angebotspreis festlegen sowie entscheiden, wie viel Sie ausgeben möchten, indem Sie ein Budget festlegen und Verknüpfungen zu den Werbemitteln, die Sie verwenden möchten, erstellen.

Weitere Informationen zu der Beziehung zwischen Lieferpositionen und Anzeigenkampagnen, Zielgruppenprofilen und Werbemitteln finden Sie unter [Anzeigenkampagnen mit Microsoft Store-Diensten ausführen](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Beachten Sie**&nbsp;&nbsp;, bevor Sie die Übermittlung von Zeilen für Ad-Kampagnen, die mit dieser API erfolgreich erstellen können, müssen Sie zuerst [erstellen Sie eine kostenpflichtige Ad Kampagne mit der **Anzeigenkampagnen** auf der Seite Partner Center](../publish/create-an-ad-campaign-for-your-app.md), und Sie müssen mindestens ein Zahlungsmittel hinzufügen, auf dieser Seite. Danach können Sie mithilfe dieser API gebührenpflichtige Lieferpositionen für Anzeigenkampagnen erstellen. AD-Kampagnen, die Sie erstellen mithilfe der API berechnet automatisch die standardmäßigen Zahlungsmethode, die ausgewählt werden, auf die **Anzeigenkampagnen** Seite im Partner Center.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methoden sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store-Werbungs-API.

  > [!NOTE]
  > Als Teil der erforderlichen Komponenten, achten Sie darauf, die Sie [erstellen Sie mindestens eine kostenpflichtige Ad Kampagne im Partner Center](../publish/create-an-ad-campaign-for-your-app.md) und Sie mindestens eine Zahlungsmethode für die Ad-Kampagne im Partner Center hinzufügen. Delivery-Zeilen, die Sie erstellen mit dieser API berechnet automatisch die standardmäßigen Zahlungsmethode, die ausgewählt werden, auf die **Anzeigenkampagnen** Seite im Partner Center.

* [Rufen Sie ein Azure AD-Zugriffstoken ab](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token), das in der Anforderungskopfzeile für diese Methoden verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern

Diese Methoden haben die folgenden URIs.

| Methodentyp | Anforderungs-URI         |  Beschreibung  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Erstellt eine neue Übermittlung Linie.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Ändert die durch *lineId* angegebene Lieferposition.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Ändert die durch *lineId* angegebene Lieferposition.  |


### <a name="header"></a>Header

| Header        | Typ   | Beschreibung         |
|---------------|--------|---------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |
| Tracking-ID   | GUID   | Optional. Eine ID, die den Abfrageablauf verfolgt.                                  |


### <a name="request-body"></a>Anforderungstext

Die POST- und PUT-Methoden erfordern einen JSON-Anforderungstext mit den erforderlichen Feldern für ein [Lieferpositionen](#line)-Objekt und alle zusätzlichen Felder, die Sie festlegen oder ändern möchten.


### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie die POST-Methode zum Erstellen einer Lieferposition aufgerufen wird.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

Im folgenden Beispiel wird veranschaulicht, wie die GET-Methode zum Abrufen einer Lieferposition aufgerufen wird.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Diese Methoden geben einen JSON-Antworttext mit einem [Lieferpositionen](#line)-Objekt zurück, das Informationen über die Lieferposition enthält, die erstellt, aktualisiert oder abgerufen wurde. Das folgende Beispiel zeigt einen Antworttext für diese Methoden.

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```


<span id="line"/>

## <a name="delivery-line-object"></a>Lieferpositionen-Objekt

Die Anforderungs- und Antworttexte für diese Methoden enthalten die folgenden Felder. Die folgende Tabelle zeigt, welche Felder schreibgeschützt sind (d. h. sie können in der PUT-Methode nicht geändert werden) und welche Felder in dem Anforderungstext für die POST- oder PUT-Methode erforderlich sind.

| Feld        | Typ   |  Beschreibung      |  Schreibgeschützt  | Standard  | Erforderlich für POST/PUT |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  Ganzzahl   |  Die ID der Lieferposition.     |   Ja    |      |  Nein      |    
|  name   |  string   |   Der Name der Lieferposition.    |    Nein   |      |  POST     |     
|  configuredStatus   |  string   |  Einer der folgenden Werte, der den Status der vom Entwickler angegebenen Lieferposition angibt: <ul><li>**aktiv**</li><li>**inaktiv**</li></ul>     |  Nein     |      |   POST    |       
|  effectiveStatus   |  string   |   Einer der folgenden Werte, der den effektiven Status der Lieferposition basierend auf der Systemüberprüfung angibt: <ul><li>**aktiv**</li><li>**inaktiv**</li><li>**Verarbeitung**</li><li>**Fehlgeschlagen**</li></ul>    |    Ja   |      |  Nein      |      
|  effectiveStatusReasons   |  array   |  Einer oder mehrere der folgenden Werte, die den Grund für den effektive Status der Lieferposition angeben: <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Ja     |     |    Nein    |           
|  startDatetime   |  string   |  Startdatum und -uhrzeit der Lieferposition im ISO 8601-Format. Dieser Wert kann nicht geändert werden, wenn er bereits in der Vergangenheit liegt.     |    Nein   |      |    POST, PUT     |
|  endDatetime   |  string   |  Enddatum und -uhrzeit der Lieferposition im ISO 8601-Format. Dieser Wert kann nicht geändert werden, wenn er bereits in der Vergangenheit liegt.     |   Nein    |      |  POST, PUT     |
|  createdDatetime   |  string   |  Erstellungsdatum und -uhrzeit der Lieferposition im ISO 8601-Format.      |    Ja   |      |  Nein      |
|  bidType   |  string   |  Ein Wert, der den Angebotstyp der Lieferposition angibt. Derzeit wird als einziger Wert **CPM** unterstützt.      |    Nein   |  CPM    |  Nein     |
|  bidAmount   |  decimal   |  Der Betrag für jede Anzeigenanforderung bieten verwendet werden.      |    Nein   |  Der durchschnittliche CPM-Wert basierend auf den Zielmärkten (dieser Wert wird in regelmäßigen Abständen überarbeitet).    |    Nein    |  
|  dailyBudget   |  decimal   |  Das tägliche Budget für die Lieferposition. Entweder *DailyBudget* oder *lifetimeBudget* muss festgelegt werden.      |    Nein   |      |   POST, PUT (wenn *lifetimeBudget* nicht festgelegt ist)       |
|  lifetimeBudget   |  decimal   |   Das Lebensdauer-Budget für die Lieferposition. Entweder lifetimeBudget* oder *dailyBudget* muss festgelegt werden.      |    Nein   |      |   POST, PUT (wenn *dailyBudget* nicht festgelegt ist)    |
|  targetingProfileId   |  object   |  Objekt, das das [Zielgruppenprofil](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) identifiziert, das die Benutzer, Regionen und Bestandstypen beschreibt, die Sie für diese Lieferposition ansprechen möchten. Dieses Objekt umfasst ein einzelnes *ID*-Feld, das die ID des Zielgruppenprofils angibt.     |    Nein   |      |  Nein      |  
|  Werbemittel   |  array   |  Ein oder mehrere Objekte, die die [Werbemittel](manage-creatives-for-ad-campaigns.md#creative) darstellen, die der Lieferposition zugeordnet sind. Jedes Objekt in diesem Feld umfasst ein einzelnes *ID*-Feld, das die ID eines Werbemittels angibt.      |    Nein   |      |   Nein     |  
|  campaignId   |  Ganzzahl   |  Die ID der übergeordneten Anzeigenkampagne.      |    Nein   |      |   Nein     |  
|  minMinutesPerImp   |  Ganzzahl   |  Gibt das minimale Zeitintervall (in Minuten) zwischen zwei Aufrufen von Anzeigen an, die demselben Benutzer dieser Lieferposition angezeigt werden.      |    Nein   |  4000    |  Nein      |  
|  pacingType   |  string   |  Einer der folgenden Werte, die den Geschwindigkeitstyp angeben: <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    Nein   |  SpendEvenly    |  Nein      |
|  currencyId   |  Ganzzahl   |  Die ID der Währung der Kampagne.      |    Ja   |  Die Währung des Entwicklerkontos (Sie müssen dieses Feld nicht im POST- oder PUT-Aufrufen angeben)    |   Nein     |      |


## <a name="related-topics"></a>Verwandte Themen

* [Ausführen von Ad-Kampagnen, die mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Ad-Kampagnen verwalten](manage-ad-campaigns.md)
* [Für die Zielgruppenadressierung Profile für Ad-Kampagnen verwalten](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von Werbemitteln für Ad-Kampagnen](manage-creatives-for-ad-campaigns.md)
* [Ad-Kampagne-Leistungsdaten abrufen](get-ad-campaign-performance-data.md)
