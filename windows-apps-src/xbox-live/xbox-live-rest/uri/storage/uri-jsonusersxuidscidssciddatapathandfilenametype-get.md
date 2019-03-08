---
title: GET (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
assetID: 229cabc6-3c5c-89e1-47e3-96a7f54094c9
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype-get.html
description: " GET (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 78933c206111a63a1a928b6d9fcdae43cb4192d4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622685"
---
# <a name="get-jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>GET (/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)
Lädt eine Datei herunter. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Autorisierung](#ID4ECB)
  * [Optionale Abfragezeichenfolgen-Parameter](#ID4EPB)
  * [Erforderlichen Anforderungsheader](#ID4EQC)
  * [Optionale Anforderungsheader](#ID4EZD)
  * [Anforderungstext](#ID4EDF)
  * [HTTP-Statuscodes](#ID4EQF)
  * [Antwortheader](#ID4EDDAC)
  * [Antworttext](#ID4EGEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Der Xbox-Benutzer-ID (XUID) des Spielers, die die Anforderung.| 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| pathAndFileName| string| Pfad und Dateiname für das Element zugegriffen werden kann. Gültige Zeichen für der Pfadteil (bis zur und einschließlich dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), und Schrägstrich (/). Der Pfadteil kann leer sein. Gültige Zeichen für den dateinamenteil (alles nach dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), Punkt (.) und Bindestriche (-). Der Dateiname kann nicht leer sein, auf einen Punkt enden oder zwei aufeinander folgende Punkte enthalten.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorisierung 
 
Die Anforderung muss einen gültige Xbox LIVE Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst einen 403 Verboten-Antwort zurück. Wenn der Header ungültig ist oder nicht vorhanden ist, wird der Dienst eine nicht autorisierte 401-Antwort zurück.
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>Optionale Abfragezeichenfolgen-Parameter 
 
Variiert je nach den Blob-Typ. Binäre Blobs unterstützen keine Abfrageparameter.
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| select| string| Nur verwendet, wenn der Typ Json ist. Gibt an, dass die Antwort nur einen bestimmte Eigenschaft/Wert der JSON-, enthalten soll, die durch diesen Parameter festgelegt. Verwenden einen "Punkt" (.) an untergeordnete Eigenschaften und eckigen Klammern ("[' und ']') Arrayindizes angeben. Z. B. "Matrix1 [4] .prop2" gibt an, die "prop2"-Eigenschaft des Index 4 des Arrays "Matrix1".| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| STS-Authentifizierungstoken. STSTokenString wird durch die von der Authentifizierungsanforderung zurückgegebene Token ersetzt. Weitere Informationen zu einem STS-Token abrufen und erstellen einen Authorization-Header finden Sie unter authentifizieren und Autorisieren von Xbox LIVE Services Anforderungen.| 
  
<a id="ID4EZD"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Gibt an, ein ETag, das einem vorhandenen-Element zum Abschließen des Vorgangs entsprechen muss.| 
| If-None-Match| Gibt an, ein ETag, die keine vorhandenen Elemente zum Abschließen des Vorgangs entsprechen muss.| 
| Bereich| Gibt den Bereich von Bytes zum Herunterladen an. Folgt den standard-HTTP-bereichsheaderformat an.| 
  
<a id="ID4EDF"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EQF"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EDDAC"></a>

 
## <a name="response-headers"></a>Antwortheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag ist ein nicht transparenter Bezeichner zugewiesen, die von einem Webserver, auf eine bestimmte Version einer Ressource finden Sie unter einer URL. Wenn der Ressourceninhalt unter dieser URL ändert, wird eine neue und andere ETag zugewiesen.| 
| Inhaltsbereich| Wenn dies ein unvollständiger Download war, gibt dieser Header den Bereich von Bytes, die heruntergeladen.| 
  
<a id="ID4EGEAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst den Inhalt der Datei zurück.
  
<a id="ID4EREAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ETEAC"></a>

 
##### <a name="parent"></a>Parent  

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

  
<a id="ID4E6EAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Verweis [TitleBlob (JSON)](../../json/json-titleblob.md)

   