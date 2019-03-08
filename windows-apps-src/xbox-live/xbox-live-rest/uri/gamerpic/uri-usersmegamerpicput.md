---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
description: " PUT (/users/me/gamerpic)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7aedc7cbd8366c9cb8d3a60e2cb1f5e843b24a8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661655"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
Lädt eine 1080 x 1080 Gamerpic hoch. 
  * [Anforderungstext](#ID4EQ)
  * [HTTP-Statuscodes](#ID4EZ)
  * [Antworttext](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Der Anforderungstext ist eine Gamerpic (1080 x 1080 PNG-Datei).
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | 
| 200| OK| VERSCHAFFEN Sie sich erfolgreich.| 
| 201| Erstellt.| Es wurde erfolgreich hochgeladen.| 
| 403| Verboten| Die Berechtigung aufgehoben wird.| 
| 500| Fehler| Da hat etwas nicht geklappt.| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>Antworttext
 
Es werden keine Objekte im Text der Antwort gesendet.
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   