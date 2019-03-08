---
title: DELETE (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: f390cc37-6a30-962c-ccd5-e1528a91d30b
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-delete.html
description: " DELETE (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4af180d758d14a2726046a6221889acc33efc897
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657655"
---
# <a name="delete-untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>DELETE (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
Löscht eine Datei. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Autorisierung](#ID4EEB)
  * [Erforderlichen Anforderungsheader](#ID4ERB)
  * [Optionale Anforderungsheader](#ID4E1C)
  * [Anforderungstext](#ID4EWD)
  * [HTTP-Statuscodes](#ID4EDE)
  * [Antworttext](#ID4EUBAC)
 
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

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| STS-Authentifizierungstoken. STSTokenString wird durch die von der Authentifizierungsanforderung zurückgegebene Token ersetzt. Weitere Informationen zu einem STS-Token abrufen und erstellen einen Authorization-Header finden Sie unter authentifizieren und Autorisieren von Xbox LIVE Services Anforderungen.| 
  
<a id="ID4E1C"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Gibt an, ein ETag, das einem vorhandenen-Element zum Abschließen des Vorgangs entsprechen muss.| 
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK | Die Datei wurde erfolgreich gelöscht oder ist nicht vorhanden.| 
| 201| Erstellt | Die Entität erstellt wurde.| 
| 400| Ungültige Anforderung. | Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert | Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten | Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden | Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel | Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout | Die Anforderung dauerte zu lange dauert.| 
| 500| Interner Serverfehler. | Der Server hat eine unerwartete Bedingung die Anforderung konnte.| 
| 503| Dienst nicht verfügbar. | Anforderung wurde gedrosselt, und wiederholen Sie dann die Anforderung der Client-Retry-Wert in Sekunden (z. B. 5 Sekunden später).| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>Antworttext 
 
Es werden keine Objekte im Text der Antwort gesendet.
  
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Parent  

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

   