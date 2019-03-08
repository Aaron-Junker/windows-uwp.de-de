---
title: /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
assetID: fc096def-ac64-76c6-09f8-8f33a6bb47a0
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediagroupsmediaitemtypes.html
description: " /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: d014cbcf4e42e2f07c3cc32ceeb557a3c8537871
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637065"
---
# <a name="mediamarketplaceidmetadatamediagroupsmediagroupmediaitemtypes"></a>/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
Greift auf die verfügbaren MediaItemTypes pro Media-Gruppe für die angegebene Version der EDS. Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediagroup| string| Erforderlich. Einer der Werte von [GET (/media/ {MarketplaceId} / Metadata/MediaGroups)](uri-medialocalemetadatamediagroupsget.md).| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)

&nbsp;&nbsp;Listet die verfügbaren MediaItemTypes pro Media-Gruppe für die angegebene Version der EDS an.
 
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

   