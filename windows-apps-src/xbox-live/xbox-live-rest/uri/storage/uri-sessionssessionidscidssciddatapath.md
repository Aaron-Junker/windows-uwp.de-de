---
title: /sessions/{sessionId}/scids/{scid}/data/{path}
assetID: 932459b4-24b4-5b09-8146-ed214de0083a
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapath.html
description: " /sessions/{sessionId}/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1af8befe28c407948dfa03d706f476458bb77c14
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655075"
---
# <a name="sessionssessionidscidssciddatapath"></a>/sessions/{sessionId}/scids/{scid}/data/{path}
Listet die Informationen in einem angegebenen Pfad. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| sessionId| string| die ID der Sitzung zu suchen.| 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| path| string| Der Pfad zu die Datenelemente zurückgegeben. Alle entsprechenden Verzeichnissen und Unterverzeichnissen zurückgegeben werden. Gültige Zeichen sind Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstrich (_) und Schrägstrich (/). Kann leer sein. Die Maximallänge von 256.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET](uri-sessionssessionidscidssciddatapath-get.md)

&nbsp;&nbsp;Listet die Informationen in einem angegebenen Pfad.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[Titel-Speicher-URIs](atoc-reference-storagev2.md)

   