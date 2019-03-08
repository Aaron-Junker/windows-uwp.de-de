---
title: GET (/sessions/{sessionId}/scids/{scid})
assetID: 1feaceed-ba0d-0b0c-e809-44ba41f2e4ea
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidsscid-get.html
description: " GET (/sessions/{sessionId}/scids/{scid})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 8a0dcd24c57cbc7dce596961afcfd7e0eba476c3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650495"
---
# <a name="get-sessionssessionidscidsscid"></a>GET (/sessions/{sessionId}/scids/{scid})
Ruft Informationen zum Kontingent für diesen Speichertyp ab. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Autorisierung](#ID4ECB)
  * [Erforderlichen Anforderungsheader](#ID4ENB)
  * [Anforderungstext](#ID4EWC)
  * [HTTP-Statuscodes](#ID4EBD)
  * [Antworttext](#ID4E2H)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| sessionId| string| die ID der Sitzung zu suchen.| 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorisierung
 
Die Anforderung muss einen gültige Xbox LIVE Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst einen 403 Verboten-Antwort zurück. Wenn der Header ungültig ist oder nicht vorhanden ist, wird der Dienst eine nicht autorisierte 401-Antwort zurück. 
  
<a id="ID4ENB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| STS-Authentifizierungstoken. STSTokenString wird durch die von der Authentifizierungsanforderung zurückgegebene Token ersetzt. Weitere Informationen zu einem STS-Token abrufen und erstellen einen Authorization-Header finden Sie unter authentifizieren und Autorisieren von Xbox LIVE Services Anforderungen.| 
  
<a id="ID4EWC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EBD"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Anforderung war erfolgreich.| 
| 201| Erstellt| Die Entität erstellt wurde.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout| Die Anforderung dauerte zu lange dauert.| 
| 500| Interner Serverfehler.| Der Server hat eine unerwartete Bedingung die Anforderung konnte.| 
| 503| Dienst nicht verfügbar.| Anforderung wurde gedrosselt, und wiederholen Sie dann die Anforderung der Client-Retry-Wert in Sekunden (z. B. 5 Sekunden später).| 
  
<a id="ID4E2H"></a>

 
## <a name="response-body"></a>Antworttext
 
Der Dienst gibt zurück, wenn der Aufruf erfolgreich ist, eine [QuotaInfo (JSON)](../../json/json-quota.md) Objekt. 
 
<a id="ID4EKAAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
  "quotaInfo":
  {
    "usedBytes":619,
    "quotaBytes":16777216
  }
}
         
```

   
<a id="ID4EWAAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EYAAC"></a>

 
##### <a name="parent"></a>Parent 

[/sessions/{sessionId}/scids/{scid}](uri-sessionssessionidscidsscid.md)

  
<a id="ID4ECBAC"></a>

 
##### <a name="reference"></a>Verweis 

[quotaInfo (JSON)](../../json/json-quota.md)

   