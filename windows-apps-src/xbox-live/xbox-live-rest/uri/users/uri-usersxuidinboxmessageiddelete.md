---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 80ec2a462648177cc6bfc846b9c84278821b0e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594105"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
Löscht eine Benutzernachricht im Posteingang des Benutzers. Die Domäne für diese URIs ist `msg.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4ECB)
  * [Autorisierung](#ID4EPB)
  * [Anforderungstext](#ID4E1B)
  * [HTTP-Statuscodes](#ID4EHC)
  * [JavaScript Object Notation (JSON)-Antwort](#ID4EAE)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise 
 
Der Delete-Vorgang ist Idempotent.
 
Der nur Inhaltstyp, die, den diese API unterstützt, ist "Application/Json", der in der HTTP-Header, der bei jedem Aufruf erforderlich ist. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid | 64-Bit-Ganzzahl ohne Vorzeichen | Die Xbox User ID (XUID) des Spielers, die die Anforderung stammt. | 
| messageId | string[50] | Die ID der Nachricht, die abgerufen oder gelöscht. | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Autorisierung 
 
Sie müssen Ihre eigenen Benutzer, die vorgeben, löschen Sie eine Meldung für den Benutzer verfügen.
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Beschreibung| 
| --- | --- | --- | --- | --- | 
| 204| Erfolg.| 
| 403| Die XUID kann nicht konvertiert werden, oder ein gültigen XUID Anspruch kann nicht gefunden werden.| 
| 404| Nachrichten-ID im URI kann nicht analysiert werden, oder ein XUID fehlt im URI.| 
| 500| Allgemeiner serverseitigen Fehler.| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON)-Antwort 
 
Im Fall eines Fehlers kann der Dienst ein ErrorResponse-Objekt zurück, die Werte aus der Umgebung des Diensts enthalten kann.
 
| Eigenschaft| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| string| Ein Hinweis darauf, wo der Fehler verursacht wurde.| 
| errorCode| int| Numerischen Code für den Fehler (kann null sein).| 
| errorMessage| string| Details des Fehlers, wenn Sie so konfiguriert, dass die Details anzuzeigen.| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource 
 
Sie können nur eigene benutzermeldungen löschen. 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Verweis [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md)

   