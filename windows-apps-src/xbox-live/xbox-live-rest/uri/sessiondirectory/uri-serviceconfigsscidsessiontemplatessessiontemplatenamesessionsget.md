---
title: GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
assetID: 9daac964-0b25-3430-fcfd-0f8658aceee1
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.html
description: " GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1020c9d9c378a95070a7b0bf3faeb9d2c6751d51
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627475"
---
# <a name="get-serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)
Ruft die Vorlagendokumente Sitzung ab.

> [!IMPORTANT]
> Diese URI-Methode erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EKB)
  * [HTTP-Statuscodes](#ID4EXB)
  * [Anforderungstext](#ID4EAC)
  * [Antworttext](#ID4EKC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode ruft die Sitzungsinformationen der Vorlage für die angegebenen Filter ab. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**.


> [!NOTE] 
> Diese Methode wird aufgerufen für 2015 Multiplayer-Spiele, indem <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>.  



> [!NOTE] 
> Jeder Aufruf dieser Methode muss entweder ein Schlüsselwort, ein Xbox-Benutzer-ID-Filter oder beides enthalten. Wenn der Aufrufer keinen korrekte Berechtigungen für die <i>private</i> und <i>Reservierungen</i> Parameter, der Methodenrückgabe Fehlercode "403 Forbidden", unabhängig davon, ob eine solche Sitzung tatsächlich vorhanden sind.  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 von der Sitzungs-ID.|
| keyword| string| Ein Schlüsselwort zum Filtern der Ergebnisse auf nur Sitzungen, die mit dieser Zeichenfolge identifiziert.|
| xuid| GUID| Xbox-Benutzer-IDs für die Benutzer für die Sitzungen abzurufen. Der Benutzer müssen in den Sitzungen aktiv sein. |
| Reservierungen| string| Der Wert, der angibt, wenn die Liste der Sitzungen die enthält, die die Benutzer nicht akzeptiert haben. Dieser Parameter kann nur festgelegt werden auf "true". Diese Einstellung muss der Aufrufer auf die Sitzung auf Serverebene zugreifen, oder des Aufrufers XUID Anspruch mit den Xbox-Benutzer-ID-Filter übereinstimmen. |
| inaktiv| string| Der Wert, der angibt, wenn die Liste der Sitzungen die enthält, die der Benutzer zugestimmt haben, aber keine aktiven Audiowiedergabe. Dieser Parameter kann nur festgelegt werden auf "true". |
| Private| string| Der Wert, der angibt, wenn die Liste der Sitzungen private Sitzungen enthält. Dieser Parameter kann nur festgelegt werden auf "true". Es gilt nur, wenn Ihre eigenen Sitzungen Abfragen oder beim Abfragen von Server-zu-Server. Dieser Parameter auf "true" festlegen, muss der Aufrufer auf die Sitzung auf Serverebene zugreifen, oder des Aufrufers XUID Anspruch mit den Xbox-Benutzer-ID-Filter übereinstimmen. |
| visibility| string| Ein Enumerationswert, der angibt, Sichtbarkeitsstatus zum Filtern der Ergebnisse verwendet wird. Derzeit kann diesen Parameter nur zu öffnen festgelegt werden, offene Sitzungen enthalten. Finden Sie unter <b>MultiplayerSessionVisibility</b>. |
| version| string| Eine positive ganze Zahl, der angibt, die wichtige Sitzung-Version oder eine niedrigere Sitzungen einschließen. Der Wert muss kleiner als oder gleich der Anforderung vertragsversion modulo 100 sein. |
| Take| string| Eine positive ganze Zahl, der angibt, der maximalen Anzahl von Sitzungen abgerufen.|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EAC"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4EKC"></a>


## <a name="response-body"></a>Antworttext

Die Rückgabe dieser Methode ist ein JSON-Array von Sitzung verweisen, mit einigen Sitzung enthaltenen Daten Inline.


```cpp
{
    "results": [ {
            "xuid": "9876",    // If the session was found from a xuid, that xuid.
            "startTime": "2009-06-15T13:45:30.0900000Z",
            "sessionRef": {
                "scid": "foo",
                "templateName": "bar",
                "name": "session-seven"
            },
            "accepted": 4,    // Approximate number of non-reserved members.
            "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
            "visibility": "open",    // or "private", "visible", or "full"
            "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
            "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
            "keywords": [ "one", "two" ]
        }
    ]
}

```


<a id="ID4EUC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EWC"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)
