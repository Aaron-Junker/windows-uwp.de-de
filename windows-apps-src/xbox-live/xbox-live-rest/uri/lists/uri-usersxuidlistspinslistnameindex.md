---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b56b563c72c206340aa2c1ce9f73aa8dfe50809d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641225"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Verschiebt ein Element in einer Liste. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| XUID| string| XUID des Benutzers.| 
| Listenname| string| Der Name der Liste zu bearbeiten.| 
| index| string| Gibt den aktuellen Index des Elements, das verschoben werden. Wenn der Indexwert 0 (null) oder eine positive ganze Zahl ist, bezieht sich auf den aktuellen Index des Elements, und der Anforderungstext des Aufrufs sollte leer sein. Wenn der Indexwert "-1" ist, muss das Element, das verschoben werden von Element-ID oder der Anbieter/ProviderID im Hauptteil Anforderung des Aufrufs angegeben werden. | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;Verschiebt ein Element in einer Liste an eine andere Position in der Liste an.
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   