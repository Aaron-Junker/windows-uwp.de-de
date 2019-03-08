---
title: URIs für Game DVR
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
description: " URIs für Game DVR"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b4bfd6e51efce4c6ec85db99a10a44a776dcb840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617845"
---
# <a name="game-dvr-uris"></a>URIs für Game DVR
 
Dieser Abschnitt enthält Informationen zu Universal Resource Identifier (URI)-Adressen und zugehörigen Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für *game DVR*.
 
Nur Konsolen können einen spielen Clip aufzeichnen, aber jedes Gerät, das auf Sie zugreifen kann, kann einen Clip anzeigen.
 
Sind abhängig von der Funktion des betreffenden URIS die Domänen für diese URIs:
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;Öffentlichen Zugriff-Clips. Dieser URI tatsächlich kann angegeben werden in zwei Formen `/public/scids/{scid}/clips` und `/public/titles/{titleId}/clips`. Weitere Informationen finden Sie weiter unten.

[/{uri}](uri-uri.md)

&nbsp;&nbsp;Zugreifen auf Spiele Clip-Daten.

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;Zugriff einen anfänglichen hochladen Anforderung.

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;Spiele Clipdaten und Metadaten zuzugreifen.

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;Zugriffsliste des Benutzers Clips.

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;Zugriff auf einen einzelnen Spiel Clip aus dem System, wenn alle IDs erleichtern bekannt sind.
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   