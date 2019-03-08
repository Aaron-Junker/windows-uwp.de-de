---
title: PUT (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: ed037b64-6525-99a2-b7f6-050fdf345de8
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype-put.html
description: " PUT (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: f04deef3a7c9eba494fdce8e7df16d9815cfc1ba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594125"
---
# <a name="put-trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>PUT (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
Lädt eine Datei hoch. Die Daten können in einer vollständigen Upload hochgeladen werden, in denen die Daten und Metadaten gesendet werden, in eine einzelne Nachricht oder als ein Multi-Block hochladen, die in der die Daten und Metadaten in einer Reihe von kleineren Blöcken gesendet werden. Nur Dateien, die kleiner als 4 MB sind, können als einzelne Nachricht gesendet werden. Mit mehreren Block hochladen, wird für Daten vom Typ Json nicht unterstützt. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Autorisierung](#ID4EEB)
  * [Optionale Abfragezeichenfolgen-Parameter](#ID4ERB)
  * [Erforderlichen Anforderungsheader](#ID4EQE)
  * [Optionale Anforderungsheader](#ID4EZF)
  * [Anforderungstext](#ID4E3G)
  * [HTTP-Statuscodes](#ID4EHH)
  * [Antworttext](#ID4E1EAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Der Xbox-Benutzer-ID (XUID) des Spielers, die die Anforderung.| 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| pathAndFileName| string| Pfad und Dateiname für das Element zugegriffen werden kann. Gültige Zeichen für der Pfadteil (bis zur und einschließlich dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), und Schrägstrich (/). Der Pfadteil kann leer sein. Gültige Zeichen für den dateinamenteil (alles nach dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), Punkt (.) und Bindestriche (-). Der Dateiname kann nicht leer sein, auf einen Punkt enden oder zwei aufeinander folgende Punkte enthalten.| 
| type| string| Das Format der Daten. Mögliche Werte sind binär- oder Json.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Autorisierung 
 
Die Anforderung muss einen gültige Xbox LIVE Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst einen 403 Verboten-Antwort zurück. Wenn der Header ungültig ist oder nicht vorhanden ist, wird der Dienst eine nicht autorisierte 401-Antwort zurück. 
  
<a id="ID4ERB"></a>

 
## <a name="optional-query-string-parameters"></a>Optionale Abfragezeichenfolgen-Parameter 
 
Für einzelne Nachricht Hochladen werden die Abfragezeichenfolgen-Parameter:
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| Datum/Uhrzeit der Datei auf den Client zuletzt die Datei hochgeladen.| 
| displayName| string| Der Name der Datei, die dem Benutzer angezeigt werden soll.| 
 
Für Multi-Block Hochladen werden die Abfragezeichenfolgen-Parameter:
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| Datum/Uhrzeit der Datei auf den Client zuletzt die Datei hochgeladen.| 
| displayName| string| Der Name der Datei, die dem Benutzer angezeigt werden soll.| 
| continuationToken| string| Das Fortsetzungstoken aus der Antwort der vorherigen Anforderung hochladen. Wenn dies der erste Block ist, sollte dies nicht angegeben werden. | 
| finalBlock| bool| Klicken Sie auf "true" für den letzten Block der Datei festgelegt ist. Legen Sie auf "false" für alle anderen Blöcken.| 
  
<a id="ID4EQE"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| STS-Authentifizierungstoken. STSTokenString wird durch die von der Authentifizierungsanforderung zurückgegebene Token ersetzt. Weitere Informationen zu einem STS-Token abrufen und erstellen einen Authorization-Header finden Sie unter authentifizieren und Autorisieren von Xbox LIVE Services Anforderungen.| 
  
<a id="ID4EZF"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Gibt an, ein ETag, das einem vorhandenen-Element zum Abschließen des Vorgangs entsprechen muss.| 
| If-None-Match| Gibt an, ein ETag, die keine vorhandenen Elemente zum Abschließen des Vorgangs entsprechen muss.| 
  
<a id="ID4E3G"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Der Anforderungstext enthält den Inhalt der Datei, die hochgeladen wird. Für einzelne Nachricht Hochladen ist der Text der vollständige Inhalt der Datei. Für Multi-Block Hochladen ist der Text der Teil der Datei, die in die Abfragezeichenfolgen-Parameter angegeben. 
  
<a id="ID4EHH"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4E1EAC"></a>

 
## <a name="response-body"></a>Antworttext 
 
Wenn der Aufruf eine Anforderung mit mehreren Block ist erfolgreich wurde, gibt der Dienst ein Token Continution, die mit dem nächsten Block übergeben zurück.
 
<a id="ID4EGFAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
    "continuationToken":"abcd1234-1111-2222-3333-abcd12345678-1"
}
         
```

   
<a id="ID4ESFAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EUFAC"></a>

 
##### <a name="parent"></a>Parent  

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

   