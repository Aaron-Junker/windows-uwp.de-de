---
title: EDS Reverse-Lookup für Video
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
description: " EDS Reverse-Lookup für Video"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: d535dec8a95eba4d486bfc6946e187e2da66ae49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598435"
---
# <a name="eds-reverse-lookup-for-video"></a>EDS Reverse-Lookup für Video
 
  * [Reverse-Lookup-Schritte](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>Reverse-Lookup-Schritte
 
Unterhaltung Discovery Services (EDS)-Reverse-Lookup wird für alle Videos Medientypen unterstützt (**MediaItemType.Movie**, **MediaItemType.TVSeries**, **MediaItemType.TVEpisode**, **MediaItemType.TVSeason**, und **MediaItemType.TVShow**), als auch **MediaItemType.Unknown**.
 
Reverse-Lookup erfordert 4 übergeben von Parametern an: 
   * `idType=ScopedMediaId`
   * `ids=` die Anbieter-Medien-ID
   * `ScopeIdType=Title`
   * `ScopeId=` die Title-ID
 
 
Das reverse-Lookup erfordert in der Regel 2 Schritte aus: 
   * Abgerufen Sie Anbieter-Medien-Id (z. B. von einem Aufruf des Details) werden, wenn sie nicht verfügbar ist. 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * Geben Sie den Aufruf für die Verwendung von reverse-Lookup der **ProviderMediaId** Feld aus der vorherigen Antwort: 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
Wenn die **ProviderMediaId** Feld nicht von EDS abgerufen wurde, und klicken Sie dann das Feld muss URL-codierte EDS ordnungsgemäß übergeben werden soll.
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Marketplace-URIs](../uri/marketplace/atoc-reference-marketplace.md)

   