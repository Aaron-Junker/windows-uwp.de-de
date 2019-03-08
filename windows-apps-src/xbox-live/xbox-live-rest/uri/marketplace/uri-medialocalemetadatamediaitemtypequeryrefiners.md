---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners
assetID: 5a519314-1df1-cbdc-cb04-3a8b663003de
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefiners.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c0a891e7ba1b111d00593de55561c89988d2a4ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603165"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefiners"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners
Greift auf die Abfrage Raffinierer, für das angegebene Media Type-Element. Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| string| Erforderlich. Einer der Werte von [abrufen (/media/ {MarketplaceId} / Metadata/MediaGroups / {Mediagroup} / MediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (/media/ {MarketplaceId} / Metadata/MediaItemTypes / {Mediaitemtype} / Queryrefiners)](uri-medialocalemetadatamediaitemtypequeryrefinersget.md)

&nbsp;&nbsp;Listet die Raffinierer Abfrage für den angegebenen Medientyp des Elements an.
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace-URIs](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   