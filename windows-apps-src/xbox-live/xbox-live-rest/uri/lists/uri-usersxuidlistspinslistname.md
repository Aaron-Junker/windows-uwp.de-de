---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 0f9610b400e9530f86e264cea30bfdfdd1b09c8d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627995"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
Greift auf die Elemente in einer Liste. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Xbox-Benutzer-ID (XUID).| 
| listtype| string| Der Typ der Liste (wie diese verwendet werden und wie sie fungiert). Bezieht sich "FIXIERT" diese immer Methoden.| 
| Listenname| string| Name der Liste (die Liste mit einer bestimmten Listentyp handeln). Elemente immer "XBLPins" für in Pins ein.| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[LÖSCHEN](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;Entfernt Elemente aus einer Liste an.

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;Gibt den Inhalt einer Liste zurück.

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;Fügt Elemente in der Liste am Index basierend auf dem Abfragezeichenfolgen-Parameter **InsertIndex**.

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;Aktualisiert die Elemente in Listen mit den Indizes, die für die einzelnen Elemente im Anforderungstext angegeben.
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   