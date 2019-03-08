---
title: /global/scids/{scid}/data/{path}
assetID: d6353cd3-9127-98d4-bb99-5df690e07022
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapath.html
description: " /global/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b8788ae6773c53c2f86b3f51ee9023876416feeb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618865"
---
# <a name="globalscidssciddatapath"></a>/global/scids/{scid}/data/{path}
Listet die Informationen in einem angegebenen Pfad. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| path| string| Der Pfad zu die Datenelemente zurückgegeben. Alle entsprechenden Verzeichnissen und Unterverzeichnissen zurückgegeben werden. Gültige Zeichen sind Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstrich (_) und Schrägstrich (/). Kann leer sein. Die Maximallänge von 256.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET](uri-globalscidssciddatapath-get.md)

&nbsp;&nbsp;Listet die Informationen in einem angegebenen Pfad.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Titel-Speicher-URIs](atoc-reference-storagev2.md)

   