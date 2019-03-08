---
title: GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)
assetID: 1bbfdfd7-84e0-68e0-49e8-ba1c60fabaa3
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediagroupsmediaitemtypesget.html
description: " GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c48079e57d4d490ab75e8120eff81c77af855923
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611445"
---
# <a name="get-mediamarketplaceidmetadatamediagroupsmediagroupmediaitemtypes"></a>GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)
Listet die verfügbaren MediaItemTypes pro Media-Gruppe für die angegebene Version der EDS an. Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediagroup| string| Erforderlich. Einer der Werte von [GET (/media/ {MarketplaceId} / Metadata/MediaGroups)](uri-medialocalemetadatamediagroupsget.md).| 
  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

  
<a id="ID4EMB"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Marketplace-URIs](atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   