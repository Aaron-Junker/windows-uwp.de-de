---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
description: " GET (/users/{ownerId}/people/{targetid})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 408b4df30f53e27b04e2a1e654e9686d2b359637
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632675"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
Ruft eine Person von Ziel-ID aus Personen-Auflistung des Aufrufers ab. Die Domäne für diese URIs ist `social.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4E5)
  * [Autorisierung](#ID4EJB)
  * [Erforderlichen Anforderungsheader](#ID4ERC)
  * [Optionale Anforderungsheader](#ID4EQD)
  * [Anforderungstext](#ID4EWE)
  * [HTTP-Statuscodes](#ID4EBF)
  * [Erforderlichen Antwortheader](#ID4EDH)
  * [Antworttext](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
GET-Vorgängen wird keine Ressourcen ändern, damit diese die gleichen Ergebnisse erzielt werden, wenn einmal oder mehrmals ausgeführt.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| ownerId| string| Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Muss den authentifizierten Benutzer übereinstimmen. Die möglichen Werte sind "me", xuid({xuid}) oder gt({gamertag}).| 
| targetid| string| Der Bezeichner des Benutzers, dessen Daten aus Kontaktliste des Besitzers, entweder ein Xbox-Benutzer-ID (XUID) oder Gamertag abgerufen wird. Beispielwerte: xuid(2603643534573581), gt(SomeGamertag).| 
  
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
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Standardwert: 1.| 
| Annehmen| Eine Zeichenfolge. Inhaltstypen, die der Aufrufer in der Antwort akzeptiert. Alle Antworten bleiben <b>Application/Json</b>.| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Erfolg.| 
| 400| Ungültige Anforderung.| Benutzer-IDs wurden falsch formatiert.| 
| 403| Verboten| XUID Anspruch konnte nicht aus dem Autorisierungsheader analysiert werden.| 
| 404| Nicht gefunden| Zielbenutzer in der Kontaktliste des Besitzers nicht gefunden.| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32-Bit-Ganzzahl ohne Vorzeichen| Länge in Bytes, des Antworttexts. Beispielwert: 22.| 
| Content-Type| string| MIME-Typ des Antworttexts. Immer <b>Application/Json</b>.| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst die Zielperson. Finden Sie unter [Person (JSON)](../../json/json-person.md).
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   