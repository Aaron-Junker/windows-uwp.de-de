---
title: /users/{ownerId}/clips
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
description: " /users/{ownerId}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: cd711777bcdf0b073dd0821222049b03aa35a23c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599515"
---
# <a name="usersowneridclips"></a>/users/{ownerId}/clips
Zugriffsliste des Benutzers Clips. Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * [URI-Parameter](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| ownerId| string| Die Benutzeridentität des Benutzers, dessen Ressource zugegriffen wird. Unterstützte Formate: "me" oder "xuid(123456789)". Maximale Länge: 16.| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/users/{ownerId}/clips)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;Rufen Sie die Liste der Benutzer des Clips.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[Game DVR-URIs](atoc-reference-dvr.md)

   