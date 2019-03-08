---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
assetID: 221c440d-c448-11e9-f455-6d0f95fc16ef
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypesortorders.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: e807f1a29712f55fc2e404e3905c7c69eb50d166
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640925"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypesortorders"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders
Greift auf verfügbare Sortierreihenfolge für einen bestimmten Mediaitem-Typ und eine bestimmte Version von EDS. Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| string| Erforderlich. Einer der Werte von [abrufen (/media/ {MarketplaceId} / Metadata/MediaGroups / {Mediagroup} / MediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (/media/ {MarketplaceId} / Metadata/MediaItemTypes / {Mediaitemtype} / Erneutes Anordnen)](uri-medialocalemetadatamediaitemtypesortordersget.md)

&nbsp;&nbsp;Listet die verfügbaren Sortierreihenfolgen für einen bestimmten Mediaitem-Typ und eine bestimmte Version von EDS.
 
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

   