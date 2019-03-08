---
title: /users/{ownerId}/scids/{scid}/clips/{gameClipId}
assetID: 49b68418-71f1-c5a2-3a9b-869fd1fa663c
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipid.html
description: " /users/{ownerId}/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: e7ea92e89d54df17e8d82084d840a7ee9ef7d032
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658335"
---
# <a name="usersowneridscidsscidclipsgameclipid"></a>/users/{ownerId}/scids/{scid}/clips/{gameClipId}
Zugriff auf einen einzelnen Spiel Clip aus dem System, wenn alle IDs erleichtern bekannt sind. Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * [URI-Parameter](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| ownerId| string| Die Benutzeridentität des Benutzers, dessen Ressource zugegriffen wird. Unterstützte Formate: "me" oder "xuid(123456789)". Maximale Länge: 16.| 
| scid| string| Dienst-Config-ID der Ressource, die auf den zugegriffen wird. Muss die SCID des authentifizierten Benutzers entsprechen.| 
| gameClipId| string| GameClip-ID der Ressource, die auf den zugegriffen wird.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})](uri-usersowneridscidclipsgameclipidget.md)

&nbsp;&nbsp;Erhalten Sie einen spielen Clip aus dem System, wenn alle IDs erleichtern bekannt sind.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[Game DVR-URIs](atoc-reference-dvr.md)

   