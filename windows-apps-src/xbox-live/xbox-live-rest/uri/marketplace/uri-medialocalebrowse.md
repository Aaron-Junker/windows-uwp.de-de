---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
description: " /media/{marketplaceId}/browse"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: f692fb66580e20ffeefb3595b8cf9d795f504311
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589685"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
Ermöglicht die Suche nach Elementen innerhalb einer Gruppe für die einzelnen Mediensatz. Die Durchsuchen-API ermöglicht Clients durch Elemente in einer einzelnen Mediengruppe durchsuchen. Seiten mit Daten können zugegriffen werden, nicht sequenziell anstelle des SkipItems Parameters das Fortsetzungstoken verwenden.
 
Diese API ermöglicht auch das Durchsuchen in die untergeordneten Elemente eines angegebenen Elements. Beispielsweise ermöglicht das Übergeben einer ID und einen MediaItemType-Parameter für eine Xbox 360-Spiele, dies durchsuchen und Diltering in den untergeordneten Elementen dieses Elements, z. B. Avatar Elemente oder DLC für das Spiel.
 
Diese API lässt Abfrage Raffinierer.
 
Einige Szenarien für die untergeordneten Elemente abgerufen werden:
 
   * Album zu Tracks
   * Reihe zu staffeln
   * Staffeln, Episoden
   * REO nachverfolgen
   * Interpreten, Alben
   * Spiele spielen-Add-Ons (DLC, Avatar, Designs usw.)
  
Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * [URI-Parameter](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (Media / {MarketplaceId} / durchsuchen)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;Ermöglicht die Suche nach Elementen innerhalb einer Gruppe für die einzelnen Mediensatz. 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace-URIs](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   