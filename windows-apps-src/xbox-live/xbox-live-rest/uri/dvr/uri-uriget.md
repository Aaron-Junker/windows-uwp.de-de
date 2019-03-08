---
title: GET (/{uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
description: " GET (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 757d84c9ad5a005e042b42d699ada08504dc57ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650615"
---
# <a name="get-uri"></a>GET (/{uri})
Spiele Clip herunterladen. Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EDB)
  * [Erforderlichen Anforderungsheader](#ID4EEC)
  * [Optionale Anforderungsheader](#ID4EQE)
  * [Anforderungstext](#ID4EZF)
  * [Erforderlichen Antwortheader](#ID4EEG)
  * [HTTP-Statuscodes](#ID4EYAAC)
  * [Optionale Antwortheader](#ID4EOFAC)
  * [Antworttext](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Hinweise
 
Der Client können alle Clip oder eine Miniaturansicht, die den Veröffentlichungsstatus erreicht hat und die herunterladbare Typ, gemäß einem **GameClipUri** Objekt. Der URI für das Anfordern der Datei ist im Antworttext enthalten, beim Abrufen einer Liste der Benutzer oder eine öffentliche Clips.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| <b>uri</b>| string| Die <b>Uri</b> Feld innerhalb der <b>GameClipUri</b> Objekt.| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Zulässige komprimierungscodierungen. Beispielwerte: Gzip, deflate, Identität.| 
| ETag| string| Für Cache-Optimierung verwendet. Beispielwert: "686897696a7c876b7e".| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Retry-After| string| Weist der Client im Fall einer nicht verfügbaren Server es später erneut versuchen.| 
| Variieren| string| Weist downstream-Proxys, wie zum Zwischenspeichern von Antworten.| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.| 
| 301| Permanent verschoben| Der Dienst wurde auf einen anderen URI verschoben.| 
| 307| Temporäre Umleitung| Der Dienst wurde auf einen anderen URI verschoben.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout| Die Anforderung dauerte zu lange dauert.| 
| 410| Aufgetreten| Die angeforderte Ressource ist nicht mehr verfügbar.| 
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>Optionale Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Für Cache-Optimierung verwendet. Beispiel: "686897696a7c876b7e".| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EUGAC"></a>

  
 
Bei Erfolg gibt der Server den Videoclip, möglicherweise abgeschnitten, gemäß der Range-Header für Anforderung zurück. Für einen abgeschnittenen Clip wird die Antwort Teilinhalt (206) sein. Wenn der Server die gesamte Datei zurückgegeben wird, reagiert er OK (200). Bei einem Fehler eine **GameClipsServiceErrorResponse** Objekt zusammen mit den entsprechenden HTTP-Statuscode (z. B. 416, Angeforderter Bereich nicht erfüllbar) zurückgegeben werden kann.
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>Parent 

[/{uri}](uri-uri.md)

   