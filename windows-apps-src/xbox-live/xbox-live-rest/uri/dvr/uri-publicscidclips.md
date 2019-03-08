---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
description: " /public/scids/{scid}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: db279d546780ed40158894f73ecb84687ef35ba6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627975"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
Öffentlichen Zugriff-Clips. Dieser URI tatsächlich kann angegeben werden in zwei Formen `/public/scids/{scid}/clips` und `/public/titles/{titleId}/clips`. Weitere Informationen finden Sie weiter unten. Die Datendomäne für diesen URI besteht `gameclipsmetadata.xboxlive.com`.
 
  * [URI-Parameter](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| string| Der primäre Dienst-Bezeichner der Konfiguration der öffentlichen Clips.| 
| titleid| string| Die TitleId der öffentlichen Clips. Kann nicht in den gleichen URI die SCID angegeben werden. Wenn angegeben, wird verwendet, um die primäre SCID zu suchen.| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (/ public/Scids / {scid} / schneidet)](uri-publicscidclipsget.md)

&nbsp;&nbsp;Listet die öffentlichen Clips.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace-URIs](../marketplace/atoc-reference-marketplace.md)

   