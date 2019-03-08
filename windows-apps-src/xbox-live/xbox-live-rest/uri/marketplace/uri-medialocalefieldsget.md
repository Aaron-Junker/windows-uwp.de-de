---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
description: " GET (/media/{marketplaceId}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: cc3ae8abfe04b7a0b9728d07b9488f9ed7c27816
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606675"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
Ruft das Token für die Felder ab. Die Domäne für diese URIs ist `eds.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EGC)
  * [Abfragezeichenfolgen-Parameter](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Die Unterhaltung Discovery Services (EDS)-APIs in der Standardeinstellung gibt einen sehr kleinen minimalen Satz von Feldern für die einzelnen Elemente zurück:
 
   * Medientyp-Element
   * Medien-Gruppe
   * Id
   * Name
  
Weitere Informationen zu erhalten, die APIs akzeptieren einen **Felder** Parameter, der angibt, welche zusätzlichen Datenelemente zurückgegeben werden sollen. Da viele mögliche Felder vorhanden sind, würde die Anforderung ihren Namen angeben, vollen Zugriff auf jeden API-Aufruf erheblich aufzublähen. Stattdessen können die Namen in diese API übergeben werden, die einen viel niedrigeren Wert generiert wird, der in die andere APIs übergeben werden kann.
 
Für jede API, die dieser Parameter akzeptiert, muss der angegebene Wert die Obermenge aller Felder in der alle angegebenen Medien-Elementtypen. Es ist nicht möglich, verschiedene Sätze von Feldern für verschiedene Medientypen für Element angeben. Allerdings trifft ein Feld an eine Media-Elementtyp jedoch nicht eine andere, es wird nur angezeigt, auf dem Medium work Item Types, in dem Daten vorhanden sind (z. B., wenn im Aufruf von "AvatarBodyType" enthalten ist [abrufen (/media/ {MarketplaceId} / Felder)](uri-medialocalefields.md), nur AvatarItems enthält das Feld "").
 
Die von dieser API zurückgegebenen Werte werden in der Tat hohem Maße zwischenspeicherbar –, sie sollten nicht mit Ausnahme von ändern, zwischen den Bereitstellungen von EDS. Es wird empfohlen, dass die Zwischenspeicherung gewünscht ist, der Cache nicht mehr als die Sitzung des Benutzers zuletzt.
 
Zusätzlich zum Akzeptieren von der tatsächlichen Feldnamen, akzeptiert die API "all" als gültiger Wert. Dadurch wird einen Wert generiert, der jedes Feld, ist es möglich enthält, anzugeben. Verwendung der Wert "all" ist nur für die Entwicklung, Debuggen und Testzwecke empfohlen.
 
Sie können alternativ senden `desired={list of fields separated by a '.'}` an eine beliebige API, die akzeptiert die **Felder** token.
 
Sie können keine übergeben, in beiden **gewünschten** und **Felder** zusammen.
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| marketplaceId| string| Erforderlich. String-Wert, der vom die <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Gewünschter| string| Erforderlich. Die "." -getrennte Liste von Feldern, die zusätzlich zu den minimalen Satz zurückgegeben werden sollen. Nicht alle angegebenen Felder werden für jedes Objekt zurückgegeben werden (Daten möglicherweise einfach nicht für bestimmte Elemente vorhanden) garantiert, jedoch keine Felder außerhalb der minimale Satz, die hier angegeben, werden nicht zurückgegeben werden. | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Marketplace-URIs](atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   