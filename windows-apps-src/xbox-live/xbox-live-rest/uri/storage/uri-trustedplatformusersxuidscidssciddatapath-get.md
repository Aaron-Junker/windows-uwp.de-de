---
title: GET (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path})
assetID: 0ab5de2f-35cd-1712-8c07-2049f5f27daf
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapath-get.html
description: " GET (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a045faf4cd81c4a7e0f1c9ca51ba454bb972b9f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599155"
---
# <a name="get-trustedplatformusersxuidxuidscidssciddatapath"></a>GET (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path})
Listet die Informationen in einem angegebenen Pfad. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Optionale Abfragezeichenfolgen-Parameter](#ID4ECB)
  * [Autorisierung](#ID4EWC)
  * [Erforderlichen Anforderungsheader](#ID4EDD)
  * [Anforderungstext](#ID4EME)
  * [HTTP-Statuscodes](#ID4EZE)
  * [Antworttext](#ID4EMCAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Der Xbox-Benutzer-ID (XUID) des Spielers, die die Anforderung.| 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| path| string| Der Pfad zu die Datenelemente zurückgegeben. Alle entsprechenden Verzeichnissen und Unterverzeichnissen zurückgegeben werden. Gültige Zeichen sind Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstrich (_) und Schrägstrich (/). Kann leer sein. Die Maximallänge von 256.| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>Optionale Abfragezeichenfolgen-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| int| Zurück, dass die Elemente, beginnend mit N + 1 in der Auflistung, z. B. N Elemente überspringen.| 
| continuationToken| string| Gibt die Elemente, die beginnend ab des angegebenen Fortsetzungstokens zurück. Der ContinuationToken-Parameter hat Vorrang vor SkipItems Wenn beides angegeben werden. Das heißt, wird der SkipItems-Parameter ignoriert, wenn ContinuationToken-Parameter vorhanden ist.| 
| "MaxItems"| int| Maximale Anzahl von Elementen, die aus der Auflistung zurück, die mit SkipItems und "continuationtoken", um einen Bereich von Elementen zurückzugeben, kombiniert werden können. Der Dienst kann einen Standardwert bereitstellen, wenn "MaxItems" nicht vorhanden ist und kann weniger als "MaxItems", zurückgeben, selbst wenn die letzte Seite der Ergebnisse noch nicht zurückgegeben wurde. | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>Autorisierung 
 
Die Anforderung muss einen gültige Xbox LIVE Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst einen 403 Verboten-Antwort zurück. Wenn der Header ungültig ist oder nicht vorhanden ist, wird der Dienst eine nicht autorisierte 401-Antwort zurück. 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| STS-Authentifizierungstoken. STSTokenString wird durch die von der Authentifizierungsanforderung zurückgegebene Token ersetzt. Weitere Informationen zu einem STS-Token abrufen und erstellen einen Authorization-Header finden Sie unter authentifizieren und Autorisieren von Xbox LIVE Services Anforderungen.| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK | Die Anforderung war erfolgreich.| 
| 201| Erstellt | Die Entität erstellt wurde.| 
| 400| Ungültige Anforderung. | Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert | Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten | Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden | Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel | Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout | Die Anforderung dauerte zu lange dauert.| 
| 500| Interner Serverfehler. | Der Server hat eine unerwartete Bedingung die Anforderung konnte.| 
| 503| Dienst nicht verfügbar. | Anforderung wurde gedrosselt, und wiederholen Sie dann die Anforderung der Client-Retry-Wert in Sekunden (z. B. 5 Sekunden später).| 
  
<a id="ID4EMCAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst ein Array von [TitleBlob](../../json/json-titleblob.md) Objekte.
 
<a id="ID4E1CAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
"blobs":
[
    {
        "fileName":"foo\bar\blob.txt,binary",
        "clientFileTime":"2012-01-01T01:02:03.1234567Z",
        "displayName":"Friendly Name",
        "size":12,
        "etag":"0x8CEB3E4F8F3A5BF"
    },
    {
        "fileName":"foo\bar\blob2.txt,binary",
        "displayName":"Blob 2",
        "size":4,
        "etag":"0x8CEB3FE57F1A142"
    },
    {
        "fileName":"foo\jsonblob.txt,json",
        "size":15,
        "etag":"0x8CEB40152B4A6F8"
    }
],
"pagingInfo":
    {
        "continuationToken":"54",
    }
}
         
```

   
<a id="ID4EGDAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIDAC"></a>

 
##### <a name="parent"></a>Parent  

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-trustedplatformusersxuidscidssciddatapath.md)

  
<a id="ID4EUDAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Verweis [TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

   