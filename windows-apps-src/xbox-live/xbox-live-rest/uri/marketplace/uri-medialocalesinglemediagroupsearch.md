---
title: /media/{marketplaceId}/singleMediaGroupSearch
assetID: f5599db7-4050-640e-db96-2df01a007c07
permalink: en-us/docs/xboxlive/rest/uri-medialocalesinglemediagroupsearch.html
description: " /media/{marketplaceId}/singleMediaGroupSearch"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b26b4c2dc51ef5591480372aa9908a49d2f8cbe2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661755"
---
# <a name="mediamarketplaceidsinglemediagroupsearch"></a>/media/{marketplaceId}/singleMediaGroupSearch
Ermöglicht die Suche nach Elementen innerhalb einer Gruppe für die einzelnen Mediensatz. Beachten Sie, dass Seiten mit Daten, die von dieser Suche zurückgegebene nicht sequenziell anstelle des SkipItems-Parameters mit dem Fortsetzungstoken zugegriffen werden können. Diese API lässt Abfrage Raffinierer.
 
Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (media/{marketplaceId}/singleMediaGroupSearch)](uri-medialocalesinglemediagroupsearchget.md)

&nbsp;&nbsp;Ermöglicht die Suche nach Elementen innerhalb einer Gruppe für die einzelnen Mediensatz. 
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace-URIs](atoc-reference-marketplace.md)

  
<a id="ID4EOC"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   