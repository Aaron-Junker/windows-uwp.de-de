---
title: URIs für Marketplace
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
description: " URIs für Marketplace"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 9fd8112c6e16b3e9d9fb70c34381e88ba5aa6273
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593875"
---
# <a name="marketplace-uris"></a>URIs für Marketplace

Dieser Abschnitt enthält Informationen zu Universal Resource Identifier (URI)-Adressen und zugeordneten Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für *Marketplace* Dienste, auch bekannt als Unterhaltung Ermittlungsdienste (EDS).

Nur Spiele, die ausgeführt wird, auf eine Xbox 360, auf einem Windows Phone-Gerät oder auf "Xbox.com" können diesen Dienst verwenden.

Die Domänen für diese URIs sind eds.xboxlive.com und inventory.xboxlive.com.

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/Users/Me/Inventory](uri-inventory.md)

&nbsp;&nbsp;Greift auf den Satz von Inventur, die derzeit mit dem angegebenen Benutzer zugeordnet sind.

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;Greift auf der vollständigen Satz an Details zu einem bestimmten Lagerartikel für genutzt werden.

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;Greift auf die vollständigen Details zu einem bestimmten Lagerartikel festgelegt.

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;Greift auf Elemente aus mehreren Mediengruppen von anderen.

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;Ermöglicht die Suche nach Elementen innerhalb einer Gruppe für die einzelnen Mediensatz.

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;Die Einstufung des Inhalts-Zugriffstoken.

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;Gibt Angebotsdetails und die Metadaten über ein oder mehrere Elemente.

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;Greift auf das Token für die Felder aus.

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;Listet alle unterstützten MediaGroups für die angegebene EDS-Version.

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;Greift auf die verfügbaren MediaItemTypes pro Media-Gruppe für die angegebene Version der EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;Greift auf Felder, die von denen eine Daten für einen bestimmten Mediaitemtype und eine bestimmte Version von EDS erwarten kann.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;Greift auf die Abfrage Raffinierer, für das angegebene Media Type-Element.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;Greift auf die zulässigen Werte für den Namen der angegebenen Abfrage Farbänderungsfeld und der angegebenen Medientyp-Element.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;Zugriff auf die Liste der untergeordneten Werte für eine bestimmte Abfrage Farbänderungsfeld-Wert (z. B. "Subgenres in einem bestimmten Genre").

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;Greift auf alle unterstützten MediaItemTypes für die angegebene EDS-Version.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;Greift auf verfügbare Sortierreihenfolge für einen bestimmten Mediaitem-Typ und eine bestimmte Version von EDS.

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;Ermöglicht die Suche nach Elementen innerhalb einer Gruppe für die einzelnen Mediensatz.

<a id="ID4EFD"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EHD"></a>


##### <a name="parent"></a>Parent

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>Weitere Informationen

[Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)
