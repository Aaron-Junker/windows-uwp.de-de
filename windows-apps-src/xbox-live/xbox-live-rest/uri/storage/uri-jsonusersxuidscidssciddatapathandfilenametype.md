---
title: /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
assetID: d004d715-d73c-693e-2a0b-ce64f482642b
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype.html
description: " /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ee2f21426f0fad1cdb84adba864aeebd0d8e1edc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614025"
---
# <a name="jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
Downloads, hochgeladen, oder löscht eine Datei. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Der Xbox-Benutzer-ID (XUID) des Spielers, die die Anforderung.| 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| pathAndFileName| string| Pfad und Dateiname für das Element zugegriffen werden kann. Gültige Zeichen für der Pfadteil (bis zur und einschließlich dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), und Schrägstrich (/). Der Pfadteil kann leer sein. Gültige Zeichen für den dateinamenteil (alles nach dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), Punkt (.) und Bindestriche (-). Der Dateiname kann nicht leer sein, auf einen Punkt enden oder zwei aufeinander folgende Punkte enthalten.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[LÖSCHEN](uri-jsonusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;Löscht eine Datei. 

[GET](uri-jsonusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Lädt eine Datei herunter.

[PUT](uri-jsonusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;Lädt eine Datei hoch. Mit mehreren Block hochladen, wird für Daten vom Typ Json nicht unterstützt. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 

[Titel-Speicher-URIs](atoc-reference-storagev2.md)

   