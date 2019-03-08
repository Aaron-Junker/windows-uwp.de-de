---
title: /json/users/batch/scids/{scid}/data/{pathAndFileName},json
assetID: 06ae159f-7739-1330-df15-871d260e6ba1
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype.html
description: " /json/users/batch/scids/{scid}/data/{pathAndFileName},json"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 2b28b0faad1c321137344455bc7cd471f7cb6eba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598595"
---
# <a name="jsonusersbatchscidssciddatapathandfilenamejson"></a>/json/users/batch/scids/{scid}/data/{pathAndFileName},json
Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen. Die Domäne für diese URIs ist `titlestorage.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| guid| die ID von der Dienstkonfiguration zu suchen.| 
| pathAndFileName| string| Pfad und Dateiname für das Element zugegriffen werden kann. Gültige Zeichen für der Pfadteil (bis zur und einschließlich dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), und Schrägstrich (/). Der Pfadteil kann leer sein. Gültige Zeichen für den dateinamenteil (alles nach dem abschließenden Schrägstrich) sind, Großbuchstaben (A – Z), Kleinbuchstaben (a-Z), Zahlen (0-9), Unterstriche (_), Punkt (.) und Bindestriche (-). Der Dateiname kann nicht leer sein, auf einen Punkt enden oder zwei aufeinander folgende Punkte enthalten.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[POST](uri-jsonusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen. Die Datei heruntergeladen werden, wird durch den URI der Anforderung bestimmt. Der Hauptteil der Anforderung enthält die Liste der XUIDs der Benutzer, deren Dateien herunterladen. Der Text der Antwort werden eine mehrteilige MIME-Nachricht, wobei jeder Teil darstellen einer Datei für einen bestimmten Benutzer mit einem eigenen Satz von Headern. Es ist möglich, für die Teile der Antwort auf eine Kombination aus erfolgreichen und fehlgeschlagenen aufweisen.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Titel-Speicher-URIs](atoc-reference-storagev2.md)

   