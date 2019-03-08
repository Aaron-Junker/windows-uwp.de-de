---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
description: " /media/{marketplaceId}/details"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: f58e5247c3fd52e84a3a9bab28c6926f74e864e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655705"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
Gibt Angebotsdetails und die Metadaten über ein oder mehrere Elemente. Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
Die API unterscheidet sich von den Durchsuchen-API und die zugehörigen API-Details (wenn Passin ID) wie diese APIs Zurückgeben von Informationen zu anderen Elementen, die mit der ID Fiven verknüpft, entweder explizit oder implizit sind, während die Details-API zusätzliche Informationen zurückgibt über das gleiche Element.
 
Mehrere verschiedene Medientypen für Element-IDs übergeben werden können, in einem einzelnen aufrufen (sofern sie nicht vom Typ ProviderContentID - finden Sie weiter unten stehen), aber diese alle die gleiche Media Group angehören müssen. Es gibt jedoch einige Clientszenarios, in denen der Aufrufer nicht der Gruppe "Medien" weiß. Die API unterstützt dies durch Zulassen der Beibehaltung einer Doppelbyte-"Unbekannt" für die Media-Gruppe in den folgenden Situationen:
 
   * ID-Typ = XboxHexTitle, der entweder "AppType" oder "GameType Elemente zurückgibt
   * idType = ProviderContentId, which will yield MovieType or TVType items
  
Das folgende Diagramm fasst die gesamte Zuordnung, die die ID Typen mit der Media-Gruppen bereitgestellt werden können:
 
| ID-Typ| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| Unbekannt| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| kanonische| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| N| 
| ZuneCatalog| N| N| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| N| N| 
| ZuneMediaInstance| N| N| „Y“ zugeordnet ist| N| „Y“ zugeordnet ist| „Y“ zugeordnet ist| N| N| 
| Angebot| „Y“ zugeordnet ist| „Y“ zugeordnet ist| „Y“ zugeordnet ist| N| „Y“ zugeordnet ist| „Y“ zugeordnet ist| N| N| 
| AMG| N| N| N| N| „Y“ zugeordnet ist| N| N| N| 
| MediaNet| N| N| N| N| „Y“ zugeordnet ist| N| N| N| 
| XboxHexTitle| „Y“ zugeordnet ist| „Y“ zugeordnet ist| N| N| N| N| N| „Y“ zugeordnet ist| 
| ProviderContentId| N| N| „Y“ zugeordnet ist| N| N| „Y“ zugeordnet ist| N| „Y“ zugeordnet ist| 
 
  * [Anmerkungen zu dieser Version Parameter](#ID4EEH)
  * [URI-Parameter](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>Anmerkungen zu dieser Version Parameter
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
Wird verwendet, um Suchanbieter bestimmten-ID z. B. Netflix oder Hulu-ID
 
Wenn ID-Typ ProviderContentId ist, wird nur ein einzelner Wert akzeptiert. Dies ist da ProviderContentIds der einzige Typ-ID sind, enthalten, kann, der '.' Zeichen. Da die '.' Zeichen ist ebenfalls das Trennzeichen, die wir zwischen IDs verwenden, es besteht eine Mehrdeutigkeit zwischen was eine Delimieter zwischen IDs ist und was ist Teil der ID selbst. Der Rest der API funktioniert auf dieselbe Weise für ProviderContentIds, mit Ausnahme von der Bulk-Lookup-Funktionalität.
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (/media/ {MarketplaceId} / details)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;Gibt Angebotsdetails und die Metadaten über ein oder mehrere Elemente. 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>Parent 

[Marketplace-URIs](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   