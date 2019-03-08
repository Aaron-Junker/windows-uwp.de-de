---
title: /titles/{titleId}/sessions/{sessionId}/allocationStatus
assetID: 55611f4b-4ba4-fa9a-ce44-fcc4a6df1b35
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus.html
description: " /titles/{titleId}/sessions/{sessionId}/allocationStatus"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: aa66b81d1e363488a6969ab31d7091b695f09ae4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603305"
---
# <a name="titlestitleidsessionssessionidallocationstatus"></a>/titles/{titleId}/sessions/{sessionId}/allocationStatus
Rufen Sie für den angegebenen Titel-Id und die Sitzungs-Id Status der ticketanforderung. Die Domänen für diese URIs sind `gameserverds.xboxlive.com` und `gameserverms.xboxlive.com`.
 
  * [URI-Parameter](#ID4EU)
  * [Name des Hosts](#ID4EPB)
  * [Gültigen Methoden](#ID4EWB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Beschreibung| 
| --- | --- | 
| titleId| Die ID des Titels, der die Anforderung angewendet werden soll.| 
| sessionId| die ID der Sitzung zu suchen.| 
  
<a id="ID4EPB"></a>

 
## <a name="host-name"></a>Hostname
 
gameserverms.xboxlive.com
  
<a id="ID4EWB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden
  
[GET](uri-titlestitleidsessionssessionidallocationstatus-get.md)
 
&nbsp;&nbsp;Gibt den Zuordnungsstatus der der Sessionhost identifizierte die SessionId zurück.
   