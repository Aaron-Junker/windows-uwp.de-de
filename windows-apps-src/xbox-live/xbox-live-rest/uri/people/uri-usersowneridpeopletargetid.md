---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
description: " /users/{ownerId}/people/{targetid}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 6238996abdeaca9b7a9a7a20d3f1ae9702e95a73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661745"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
Greift auf eine Person von Ziel-ID des Aufrufers Personen-Auflistung. Die Domäne für diese URIs ist `social.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| ownerId| string| Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Muss den authentifizierten Benutzer übereinstimmen. Die möglichen Werte sind "me", xuid({xuid}) oder gt({gamertag}).| 
| targetid| string| Der Bezeichner des Benutzers, dessen Daten aus Kontaktliste des Besitzers, entweder ein Xbox-Benutzer-ID (XUID) oder Gamertag abgerufen wird. Beispielwerte: xuid(2603643534573581), gt(SomeGamertag).| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;Ruft eine Person von Ziel-ID aus Personen-Auflistung des Aufrufers ab.
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   