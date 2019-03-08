---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
description: " POST (/users/{ownerId}/people/xuids)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1cb160f3276d215e3aba5dfd671c67fa17d883b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589865"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
Ruft Personen nach XUID von des Aufrufers Personen-Auflistung ab. Die Domäne für diese URIs ist `social.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4E5)
  * [Autorisierung](#ID4EJB)
  * [Erforderlichen Anforderungsheader](#ID4ERC)
  * [Optionale Anforderungsheader](#ID4EBE)
  * [Anforderungstext](#ID4EHF)
  * [HTTP-Statuscodes](#ID4EKH)
  * [Erforderlichen Antwortheader](#ID4ENBAC)
  * [Antworttext](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Vorgänge wird nicht nach Ressourcen ändern, damit diese die gleichen Ergebnisse erzielt werden, wenn einmal oder mehrmals ausgeführt.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| ownerId| string| Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Muss den authentifizierten Benutzer übereinstimmen. Die möglichen Werte sind "me", xuid({xuid}) oder gt({gamertag}).| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Autorisierung
 
| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Ja| Aufrufer hat des Benutzers Xbox Benutzer-ID (XUID).| 401 nicht autorisiert| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Eine Zeichenfolge. Der Autorisierungsdaten für Xbox LIVE. Dies ist normalerweise ein verschlüsseltes XSTS-Token. Beispielwert: <b>XBL3.0 x=&lt;userhash>;&lt;token></b>.| 
| Content-Length| 32-Bit-Ganzzahl ohne Vorzeichen. Länge in Bytes, des Anforderungstexts. Beispielwert: 22.| 
| Content-Type| Eine Zeichenfolge. MIME-Typ des Anforderungstexts. Dies muss <b>Application/Json</b>.| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Standardwert: 1.| 
| Annehmen| Eine Zeichenfolge. Inhaltstypen, die der Aufrufer in der Antwort akzeptiert. Alle Antworten bleiben <b>Application/Json</b>.| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>Erforderliche Member
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| Array von XUIDs, die identifizieren, die Personen aus des Aufrufers Personen Auflistung zurückgegeben werden. Finden Sie unter [XuidList (JSON)](../../json/json-xuidlist.md).| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>Optionale Elemente
 
Es gibt keine optionalen Elemente für diese Anforderung.
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>Unzulässige Mitglieder
 
Alle anderen Elemente sind in einer Anforderung nicht zulässig.
  
<a id="ID4EAH"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
      
```

   
<a id="ID4EKH"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Erfolgreich, wenn die Methode ist "get".| 
| 204| Kein Inhalt| Erfolgreich, wenn die Methode ist "add" oder "entfernen".| 
| 400| Ungültige Anforderung.| Methodenparameter angegeben wurde, fehlt oder ist fehlerhaft, oder Benutzer-IDs wurden falsch formatiert.| 
| 403| Verboten| XUID Anspruch konnte nicht aus dem Autorisierungsheader analysiert werden.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32-Bit-Ganzzahl ohne Vorzeichen| Länge in Bytes, des Antworttexts. Beispielwert: 22.| 
| Content-Type| string| MIME-Typ des Antworttexts. Immer <b>Application/Json</b>.| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Ein Antworttext wird nur gesendet, wenn die Anforderungsmethode ist "get". Es ist kein Antworttext für "add" oder "entfernen" aus.
 
Wenn der Aufruf einer "Get"-Methode erfolgreich ist, gibt der Dienst die Gesamtanzahl der Mitarbeiter in des Aufrufers Mitarbeiter, Auflistung und ein Array, das der Aufrufer Personen Auflistung enthält. Für die Methoden "add" und "remove" wird keine Antwort zurückgegeben. Finden Sie unter [PeopleList (JSON)](../../json/json-peoplelist.md).
 
<a id="ID4EHDAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
         
```

   
<a id="ID4ERDAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ETDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   