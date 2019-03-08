---
title: POST (/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
assetID: 6e28d794-b5c6-0b70-6d46-957e8ae6e8ac
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersbatchscidssciddatapathandfilenametype-post.html
description: " POST (/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3acbefdec9af8c60c3a2bc7ece185c2b83e3b63b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631795"
---
# <a name="post-untrustedplatformusersbatchscidssciddatapathandfilenametype"></a>POST (/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Autorisierung](#ID4ECB)
  * [Anforderungstext](#ID4EPB)
  * [HTTP-Statuscodes](#ID4E3C)
  * [Erforderlichen Antwortheader](#ID4EPAAC)
  * [Optionale Antwortheader](#ID4ESBAC)
  * [Antworttext](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| pathAndFileName| string| Pfad und Dateiname für das Element zugegriffen werden kann. Gültige Zeichen für der Pfadteil (bis zur und einschließlich dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), und Schrägstrich (/). Der Pfadteil kann leer sein. Gültige Zeichen für den dateinamenteil (alles nach dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), Punkt (.) und Bindestriche (-). Der Dateiname kann nicht leer sein, auf einen Punkt enden oder zwei aufeinander folgende Punkte enthalten.| 
| type| string| Das Format der Daten. Mögliche Werte sind binäre Json.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorisierung 
 
Die Anforderung muss einen gültige Xbox LIVE Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst einen 403 Verboten-Antwort zurück. Wenn der Header ungültig ist oder nicht vorhanden ist, wird der Dienst eine nicht autorisierte 401-Antwort zurück. 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>Anforderungstext
 
| Eigenschaft| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| xuids| Array von 64-Bit-Ganzzahlen ohne Vorzeichen| Die Liste der XUIDs für das Herunterladen von Dateien.| 
 
<a id="ID4EQC"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
    "xuids" : 
    [
      12345,
      45678,
      78901
    ]
}
      
```

   
<a id="ID4E3C"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Disposition| Beschreibt den Inhalt des Parts. Die "Name" und "Filename" Teile des Headers sind die XUID des Benutzers, der diese Datei gehört.| 
| HttpStatusCode| Der HTTP-Statuscode, der im Zusammenhang mit dieser Datei werden abgerufen.| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>Optionale Antwortheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag ist ein nicht transparenter Bezeichner zugewiesen, die von einem Webserver, auf eine bestimmte Version einer Ressource finden Sie unter einer URL. Wenn der Ressourceninhalt unter dieser URL ändert, wird eine neue und andere ETag zugewiesen.| 
| Content-Type| Wenn die Datei erfolgreich abgerufen wurde, ist dies der Inhaltstyp der Datei.| 
| Inhaltsbereich| Wenn die Datei erfolgreich abgerufen wurde und ein unvollständiger Download, ist dies den Bereich von Bytes der Datei in der Antwort enthalten sind. | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst den Inhalt der angeforderten Dateien in einer mehrteiligen Antwort zurück.
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>Beispielantwort 
 

```cpp
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=c0a9fd75-d036-4294-8b7b-85fea15a31bb

228
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="123"; filename="123"
HttpStatusCode: 200
ETag: 0x8CF327717411C31
Content-Type: application/octet-stream

asdf123
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="456"; filename="456"
HttpStatusCode: 200
ETag: 0x8CF32771E954BB8
Content-Type: application/octet-stream

asdf456
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="789"; filename="789"
HttpStatusCode: 404


--c0a9fd75-d036-4294-8b7b-85fea15a31bb--

0

```

   
<a id="ID4EUDAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EWDAC"></a>

 
##### <a name="parent"></a>Parent 

[/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.md)

   