---
title: /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
assetID: c2909e75-e108-3555-c157-f2d552f10e9f
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.html
description: " /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: bc57ce4cd80a33b831be3eb3615489cec5a633b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599735"
---
# <a name="untrustedplatformusersbatchscidssciddatapathandfilenametype"></a>/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| pathAndFileName| string| Pfad und Dateiname für das Element zugegriffen werden kann. Gültige Zeichen für der Pfadteil (bis zur und einschließlich dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), und Schrägstrich (/). Der Pfadteil kann leer sein. Gültige Zeichen für den dateinamenteil (alles nach dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), Punkt (.) und Bindestriche (-). Der Dateiname kann nicht leer sein, auf einen Punkt enden oder zwei aufeinander folgende Punkte enthalten.| 
| type| string| Das Format der Daten. Mögliche Werte sind binäre Json.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[POST](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[Titel-Speicher-URIs](atoc-reference-storagev2.md)

   