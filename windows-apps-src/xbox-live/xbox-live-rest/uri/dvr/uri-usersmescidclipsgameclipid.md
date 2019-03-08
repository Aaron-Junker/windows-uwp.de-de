---
title: /users/me/scids/{scid}/clips/{gameClipId}
assetID: f5bead69-4fc9-f551-39cb-c8754645ac88
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipid.html
description: " /users/me/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3cbe2d2c996b466fd94287129f1add0868f05b05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661775"
---
# <a name="usersmescidsscidclipsgameclipid"></a>/users/me/scids/{scid}/clips/{gameClipId}
Spiele Clipdaten und Metadaten zuzugreifen. Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * [URI-Parameter](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| string| Dienst-Config-ID der Ressource, die auf den zugegriffen wird. Muss die SCID des authentifizierten Benutzers entsprechen.| 
| gameClipId| string| GameClip-ID der Ressource, die auf den zugegriffen wird.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[DELETE (/users/me/scids/{scid}/clips/{gameClipId})](uri-usersmescidclipsgameclipiddelete.md)

&nbsp;&nbsp;Spiele Clip löschen

[POST (/ Benutzer/me/Scids / {scid} /clips/ {GameClipId})](uri-usersmescidclipsgameclipidpost.md)

&nbsp;&nbsp;Spiele Clip-Metadaten für die Daten des Benutzers aktualisiert.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Game DVR-URIs](atoc-reference-dvr.md)

   