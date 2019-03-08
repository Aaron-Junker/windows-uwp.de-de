---
title: POST (/users/me/resetreputation)
assetID: 1a4fed45-f608-dac2-3384-2ee493112f7b
permalink: en-us/docs/xboxlive/rest/uri-usersmeresetreputationpost.html
description: " POST (/users/me/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: fc770673ac7e4034004f58032c7fa66cb28413e7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632685"
---
# <a name="post-usersmeresetreputation"></a>POST (/users/me/resetreputation)
Ermöglicht es das Team Erzwingung zum Festlegen des aktuellen Benutzers Reputation Bewertungen auf eine beliebige Werte nach (z. B.) eine Konto-Hijacking. Die Domäne für diese URIs ist `reputation.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [Autorisierung](#ID4E5)
  * [Erforderlichen Anforderungsheader](#ID4ETB)
  * [Anforderungstext](#ID4END)
  * [HTTP-Statuscodes](#ID4EDE)
  * [Antworttext](#ID4EFH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Diese Methode kann auch von anderen Partnern für alle Sandboxes außer im Einzelhandel und von Benutzern in allen Sandboxes außer Einzelhandel, zu Testzwecken aufgerufen werden. Beachten Sie, dass diese Anforderung, eines Benutzers festlegt "Ruf Bewertungen base" und seine positive rückmeldungen Gewichtungen alle werden auf NULL werden gesetzt werden. Tatsächliche Ruf des Benutzers, nach diesem Aufruf wird dieser Basis Bewertungen sowie seine botschafter Bonus sowie seine Follower Bonus.
  
<a id="ID4E5"></a>

 
## <a name="authorization"></a>Autorisierung
 
Vom Partner: Für den Sandkasten Retail **PartnerClaim** vom Team Erzwingung; für alle anderen Sandboxes **PartnerClaim**.
 
Benutzer: Für alle Sandboxes außer Einzelhandel **XuidClaim** und **TitleClaim**.
  
<a id="ID4ETB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
Von allen: **Inhaltstyp: Application/Json**.
 
Vom Partner: **X-Xbl-Contract-Version** (aktuelle Version ist 101), **X-Xbl-Sandbox**.
 
Benutzer: **X-Xbl-Contract-Version** (aktuelle Version ist 101).
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 101.| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>Anforderungstext
 
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 
Der Anforderungstext ist eine einfache [ResetReputation (JSON)](../../json/json-resetreputation.md) Dokument.
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK| OK.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 500| Interner Serverfehler.| Der Server hat eine unerwartete Bedingung die Anforderung konnte.| 
| 503| Dienst nicht verfügbar.| Anforderung wurde gedrosselt, und wiederholen Sie dann die Anforderung der Client-Retry-Wert in Sekunden (z. B. 5 Sekunden später).| 
  
<a id="ID4EFH"></a>

 
## <a name="response-body"></a>Antworttext
 
Bei Erfolg ist der Antworttext leer. Bei einem Fehler eine [ServiceError (JSON)](../../json/json-serviceerror.md) -Dokument wird zurückgegeben.
 
<a id="ID4ERH"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4E2H"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E4H"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/resetreputation](uri-usersmeresetreputation.md)

   