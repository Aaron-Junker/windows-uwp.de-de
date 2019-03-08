---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
description: " /handles/{handleId}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3a312d3744e96755a899d73307a47c01e3dc79fd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632635"
---
# <a name="handleshandleid"></a>/handles/{handleId}
Unterstützt die DELETE und GET-Operationen für die Sitzung verarbeitet, die vom Bezeichner angegebene. 

> [!NOTE] 
> Dieser URI wird von 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es ist für die Verwendung mit Vorlage Vertrag 104/105 oder höher vorgesehen.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | 
| handleId| GUID| Die eindeutige ID des Handles für die Sitzung.| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;Löscht die Handles, die gemäß der Handle-ID an.

[GET (/handles/ {Handle-Id})](uri-handleshandleidget.md)

&nbsp;&nbsp;Ruft ab, Handles, die gemäß der Handle-ID an.
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)

   