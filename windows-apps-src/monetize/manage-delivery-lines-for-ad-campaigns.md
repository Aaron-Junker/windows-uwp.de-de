---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Verwenden Sie diese Methode in der Microsoft Store Promotions-API, um Übermittlungs Zeilen für Werbekampagnen zu verwalten.
title: Verwalten von Lieferpositionen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store promotionapi, Werbekampagnen
ms.localizationpriority: medium
ms.openlocfilehash: e7b370d8eea61033092d833cdf751f1dade86c6d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878563"
---
# <a name="manage-delivery-lines"></a>Verwalten von Lieferpositionen

Verwenden Sie diese Methoden in der Microsoft Store promotionapi, um eine oder mehrere Übermittlungs *Zeilen* zu erstellen und ihre Werbeeinblendungen für Werbekampagnen zu liefern. Für jede Übermittlungs Zeile können Sie die Zielgruppe festlegen, ihren Angebotspreis festlegen und entscheiden, wie viel Geld Sie aufwenden möchten, indem Sie ein Budget festlegen und mit den gewünschten Informationen verknüpfen.

Weitere Informationen über die Beziehung zwischen Übermittlungs Zeilen und Ad-Kampagnen, Ziel Profilen und Erstellungen finden [Sie unter Ausführen von Ad-Kampagnen mit Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Note** &nbsp; Hinweis &nbsp; Bevor Sie mithilfe dieser API Übermittlungs Zeilen für Ad-Kampagnen erstellen können, müssen Sie zunächst auf [der Seite mit den **Werbekampagnen** im Partner Center eine bezahlte Werbekampagne erstellen](./index.md), und Sie müssen mindestens ein Zahlungsinstrument auf dieser Seite hinzufügen. Nachdem Sie dies getan haben, können Sie mithilfe dieser API abrechenbare Übermittlungs Zeilen für Ad-Kampagnen erstellen. Mit Ad-Kampagnen, die Sie mithilfe der API erstellen, wird automatisch das standardmäßige Zahlungsinstrument abgerechnet, das auf der Seite mit den **Werbekampagnen** im Partner Center ausgewählt

## <a name="prerequisites"></a>Voraussetzungen

Um diese Methoden zu verwenden, müssen Sie zunächst die folgenden Schritte ausführen:

* Wenn Sie dies noch nicht getan haben, müssen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store promotionapi erfüllen.

  > [!NOTE]
  > Stellen Sie im Rahmen der Voraussetzungen sicher, dass Sie mindestens [eine bezahlte Werbekampagne in Partner Center erstellen](./index.md) und mindestens ein Zahlungsinstrument für die Werbekampagne in Partner Center hinzufügen. Durch die über diese API erstellten Übermittlungs Zeilen wird automatisch das standardmäßige Zahlungsinstrument abgerechnet, das auf der Seite mit den **Werbekampagnen** im Partner Center ausgewählt wurde.

* Rufen Sie [ein Azure AD Zugriffs Token](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) ab, das im-Anforderungs Header für diese Methoden verwendet werden soll. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methoden verfügen über die folgenden URIs.

| Methodentyp | Anforderungs-URI         |  BESCHREIBUNG  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Erstellt eine neue Übermittlungs Zeile.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Bearbeitet die von *LineID*angegebene Übermittlungs Zeile.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Ruft die von *LineID*angegebene Übermittlungs Zeile ab.  |


### <a name="header"></a>Header

| Header        | type   | BESCHREIBUNG         |
|---------------|--------|---------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |
| Nachverfolgungs-ID   | GUID   | Optional. Eine ID, die den aufrufflow nachverfolgt.                                  |


### <a name="request-body"></a>Anforderungstext

Die Post-und Put-Methoden erfordern einen JSON-Anforderungs Text mit den erforderlichen Feldern eines Übermittlungs [Zeilen](#line) Objekts und zusätzlichen Feldern, die Sie festlegen oder ändern möchten.


### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie die Post-Methode aufgerufen wird, um eine Übermittlungs Zeile zu erstellen.

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

Im folgenden Beispiel wird veranschaulicht, wie Sie die Get-Methode aufrufen, um eine Übermittlungs Zeile abzurufen.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Diese Methoden geben einen JSON-Antworttext mit einem Übermittlungs [Zeilen](#line) Objekt zurück, das Informationen über die erstellte, aktualisierte oder abgerufene Zustellungs Zeile enthält. Das folgende Beispiel zeigt einen Antworttext für diese Methoden.

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

## <a name="delivery-line-object"></a>Übermittlungs Zeilen Objekt

Die Anforderungs-und Antwort Texte für diese Methoden enthalten die folgenden Felder. Diese Tabelle zeigt, welche Felder schreibgeschützt sind (was bedeutet, dass Sie in der Put-Methode nicht geändert werden können) und welche Felder im Anforderungs Text für die Post-oder Put-Methoden erforderlich sind.

| Feld        | type   |  BESCHREIBUNG      |  Schreibgeschützt  | Standard  | Erforderlich für Post/Put |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  integer   |  Die ID der Übermittlungs Zeile.     |   Ja    |      |  Nein      |    
|  name   |  Zeichenfolge   |   Der Name der Übermittlungs Zeile.    |    Nein   |      |  POST     |     
|  Konfigurations Status   |  Zeichenfolge   |  Einer der folgenden-Werte, der den Status der vom Entwickler angegebenen Übermittlungs Zeile angibt: <ul><li>**Aktiv**</li><li>**Inaktiv**</li></ul>     |  Nein     |      |   POST    |       
|  effectivestatus   |  Zeichenfolge   |   Einer der folgenden Werte, der den effektiven Status der Übermittlungs Zeile basierend auf der Systemvalidierung angibt: <ul><li>**Aktiv**</li><li>**Inaktiv**</li><li>**Verarbeitung**</li><li>**Fehler**</li></ul>    |    Ja   |      |  Nein      |      
|  effectivestatus Reasons   |  array   |  Einer oder mehrere der folgenden Werte, die den Grund für den effektiven Status der Übermittlungs Zeile angeben: <ul><li>**Adkreativesininaktiv**</li><li>**ValidationFailed**</li></ul>      |  Ja     |     |    Nein    |           
|  startDatetime   |  Zeichenfolge   |  Das Startdatum und die Uhrzeit für die Übermittlungs Zeile im ISO 8601-Format. Dieser Wert kann nicht geändert werden, wenn er bereits in der Vergangenheit liegt.     |    Nein   |      |    Post, Put     |
|  endDatetime   |  Zeichenfolge   |  Das Enddatum und die Uhrzeit für die Übermittlungs Zeile im ISO 8601-Format. Dieser Wert kann nicht geändert werden, wenn er bereits in der Vergangenheit liegt.     |   Nein    |      |  Post, Put     |
|  "kreateddatetime"   |  Zeichenfolge   |  Das Datum und die Uhrzeit der Erstellung der Übermittlungs Zeile im ISO 8601-Format.      |    Ja   |      |  Nein      |
|  "Bid Type"   |  Zeichenfolge   |  Ein-Wert, der den anstellungstyp der Übermittlungs Zeile angibt. Derzeit ist der einzige unterstützte Wert " **CPM**".      |    Nein   |  CPM    |  Nein     |
|  bidamount   |  Decimal   |  Der Anforderungstyp, der zum Anfordern einer beliebigen AD-Anforderung verwendet werden soll.      |    Nein   |  Der durchschnittliche CPM-Wert, der auf den Zielmärkten basiert (dieser Wert wird regelmäßig überarbeitet).    |    Nein    |  
|  dailybudget   |  Decimal   |  Das tägliche Budget für die Übermittlungs Leitung. Entweder " *dailybudget* " oder " *lifetimebudget* " muss festgelegt werden.      |    Nein   |      |   Post, Put (wenn *lifetimebudget* nicht festgelegt ist)       |
|  lifetimebudget   |  Decimal   |   Das Lebensdauer Budget für die Übermittlungs Zeile. Lifetimebudget * oder *dailybudget* muss festgelegt werden.      |    Nein   |      |   Post, Put (wenn *dailybudget* nicht festgelegt ist)    |
|  targetingprofileid   |  Objekt (object)   |  Auf dem Objekt, das das Ziel [Profil](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) identifiziert, das die Benutzer, geografische Regionen und Inventur Typen beschreibt, die für diese Übermittlungs Leitung als Ziel verwendet werden sollen. Dieses Objekt besteht aus einem einzelnen *ID* -Feld, das die ID des Ziel Profils angibt.     |    Nein   |      |  Nein      |  
|  -kreative   |  array   |  Ein oder mehrere- [Objekte, die die der](manage-creatives-for-ad-campaigns.md#creative) Übermittlungs Zeile zugeordneten-Objekte darstellen. Jedes Objekt in diesem Feld besteht aus einem einzelnen *ID* -Feld, das die ID eines Creative-Objekts angibt.      |    Nein   |      |   Nein     |  
|  campaignId   |  integer   |  Die ID der übergeordneten Werbekampagne.      |    Nein   |      |   Nein     |  
|  minminutesperimp   |  integer   |  Gibt das minimale Zeitintervall (in Minuten) zwischen zwei Eindrücken an, die dem gleichen Benutzer in dieser Übermittlungs Zeile angezeigt werden.      |    Nein   |  4000    |  Nein      |  
|  pacingtype   |  Zeichenfolge   |  Einer der folgenden Werte, die den pacetyp angeben: <ul><li>**Spendezly**</li><li>**Spendasfastaspossible**</li></ul>      |    Nein   |  Spendezly    |  Nein      |
|  currencyId   |  integer   |  Die ID der Währung der Kampagne.      |    Ja   |  Die Währung des Entwickler Kontos (Sie müssen dieses Feld nicht in Post-oder Put-aufrufen angeben)    |   Nein     |      |


## <a name="related-topics"></a>Zugehörige Themen

* [Ausführen von Ad-Kampagnen mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Verwalten von Anzeigenkampagnen](manage-ad-campaigns.md)
* [Verwalten von Ziel Profilen für Werbekampagnen](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von kreativen für Werbekampagnen](manage-creatives-for-ad-campaigns.md)
* [Abrufen der Leistungsdaten einer Anzeigenkampagne](get-ad-campaign-performance-data.md)