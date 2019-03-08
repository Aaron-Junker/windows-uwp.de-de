---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
description: " GET (/media/{marketplaceId}/contentRating)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 8d1cb9d09de8671d4cd3d61e96a8335412237e5c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641585"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
Rufen Sie das Token für die Einstufung des Inhalts. Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4ELB)
  * [Abfragezeichenfolgen-Parameter](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Durchsetzung von zugangssteuerungen über die Inhalte, die untergeordneten Elemente zulässig sind, finden Sie unter ist eine komplizierte Aufgabe. Nicht nur besitzt jedes Element Medientyp eigene Bewertungssystem, sondern diese Bewertungssystemen können von Land zu Land unterscheiden. Dies bedeutet, dass es mehrere verschiedene Teile der Daten, die müssen angegeben werden, um alle Elemente ordnungsgemäß zu filtern.
 
Anstatt alle Parameter in allen API-aufrufen, generiert dieser API einen Wert für die Übergabe in **CombinedContentRating** Parameter in anderen APIs und kommunizieren Sie weiterhin die gleiche Informationen. Dies soll erleichtern die APIs verwenden und verwalten, da mehrere Parameter übergeben, die in diese API für die anderen APIs in einen einzelnen, wieder verwendbaren Wert reduziert werden.
 
Obwohl die genauen Werte, die von dieser API zurückgegebenen schließlich geändert werden können, sollten sie nur sehr selten ändern (z. B. zwischen Versionen der Unterhaltung Discovery Services (EDS)) und somit für längere Zeit zwischengespeichert werden kann. Jede API akzeptiert eine **CombinedContentRating** Parameter erhalten eine sinnvolle Fehlermeldung, wenn der übergebene Wert ungültig ist. Dies ist eine Angabe über das der Aufrufer lediglich zum Aufrufen dieser API erneut aus, um eine aktualisierte Wert zu erhalten muss. Wenn eine API akzeptiert eine **CombinedContentRating** -Parameter, aber leider nicht angegeben wird, keine Filterung von Inhalten basierend auf Jugendschutz stattfinden soll. 

> [!NOTE] 
> Dies bedeutet nicht, dass nur "sicher" Inhalt zurückgegeben werden – dies bedeutet, dass der gesamte Inhalt einschließlich potenziell anstößiger Inhalt zurückgegeben wird,. 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| Boolescher Wert| Optional. Filtert die explizite Musik.| 
| filterFamilyOnlyApps| Boolescher Wert| Optional. Apps, die nicht als Familie Anzeigenamen zu filtern.| 
| filterUnrated| Boolescher Wert| Optional. Bestimmt, ob der Inhalt, der nicht bewertete ist oder nicht in der Antwort enthalten sein sollten.| 
| maxGameRating| 32-Bit-Ganzzahl mit Vorzeichen| Optional. Filtert Spiele.| 
| maxMovieRating| 32-Bit-Ganzzahl mit Vorzeichen| Optional. Filtert Filme oberhalb einer bestimmten Ebene an.| 
| maxTVRating| 32-Bit-Ganzzahl mit Vorzeichen| Optional. TV-Filter.| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Marketplace-URIs](atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   