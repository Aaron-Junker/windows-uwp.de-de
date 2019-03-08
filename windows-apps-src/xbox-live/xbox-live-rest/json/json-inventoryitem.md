---
title: InventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
description: " InventoryItem (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 240e527258923cff146009810c190e401e0574d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628495"
---
# <a name="inventoryitem-json"></a>InventoryItem (JSON)
Das Core-Inventar-Element stellt dar, das standard-Element auf dem eine Berechtigung erteilt werden kann.
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

InventoryItem Objekt verfügt über den folgenden Spezifikationen.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| URL| string| Eindeutiger Bezeichner für diesen bestimmten Lagerartikel.|
| itemType| string| Der Typ des Elements. Aktuelle Werte <ul><li><b>Unbekannt</b></li><li><b>Spiel</b></li><li><b>Film</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>Poster</b></li><li><b>Podcast</b></li><li><b>Image</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>Design</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>Album</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>Paket</b></li><li><b>XnaCommunityGame</b></li><li><b>Promotional</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>App</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>Abonnement</b><br/><br/> **Hinweis:** Spiele sind vom angegebenen **GameV2**, Verbrauchsmaterialien sind **GameConsumable**, und durable-DLC- **GameContent**. |
  | Container | string | Dies ist der Satz von "Container", die diesem Element enthalten. Für Elemente, die zu einem bestimmten Container gehören, kann eines Benutzers Inventur abgefragt werden. Diese Container werden festgelegt, wenn das Element mit dem Bestand von Bestellung hinzugefügt wird. |
  | abgerufen | DateTime | Datum und Uhrzeit, die das Element so erfassen Sie des Benutzers hinzugefügt wurde. |
  | startDate | DateTime | Datum und Uhrzeit, die das Element wurde oder für die Verwendung zur Verfügung stehen. |
  | endDate | DateTime | Datum und Uhrzeit, die das Element wurde oder unbrauchbar wird. |
  | Status | string | Der Zustand des Elements. Zulässige Werte sind **aktiviert**, **Suspended**, **abgelaufen**, **Canceled**, **Renewed**.  |
  | trial | Boolescher Wert | Erforderlich. True, wenn diese Berechtigung eine Testversion ist. andernfalls "false". Wenn Sie die Testversion von eine Berechtigung erwerben, und erwerben Sie anschließend auf die Vollversion, erhalten Sie beide. |
  | trialTimeRemaining | TimeSpan | NULL-Werte zulässt. Wie viel Zeit wird in der Testversion in wenigen Minuten verbleiben. |
  | Verwendet werden können | array | Wenn die Elemente ist genutzt werden, enthält eine inlinedarstellung der der eindeutige Bezeichner (Link) für den verwendbaren Lagerartikel als auch die aktuelle Menge. |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
inventoryItem {
  "url": string,
  "itemType": "Music" | "Video" | "Game" | "AvatarItem" | "Subscription" | "DLC" | "Consumable" | ...,
  "obtained": DateTime,
  "beginDate": DateTime,
  "endDate": DateTime,
  "state": "Unavailable" | "Available" | "Suspended" | "Expired",
  "trial": true,
  "trialTimeRemaining":"23:12:14",
  ("consumable": {"url": string, "quantity": int})
}

```


<a id="ID4EVAAC"></a>


## <a name="consumable-inventory-item"></a>Nutzbar Lagerartikel

Die verwendbaren Entität stellt den minimalen Satz von Eigenschaften für ein Element genutzt werden.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| URL| string| Eindeutiger Bezeichner für den bestimmten Lagerartikel genutzt werden.|
| quantity| 32-Bit-Ganzzahl mit Vorzeichen| Die aktuelle Menge dieses Artikels Bestand.|


```json
consumableInventoryItem {
  "url": string,
  "quantity": int
}

```


<a id="ID4E4BAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E6BAC"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>Verweis

[/Users/Me/Inventory](../uri/marketplace/uri-inventory.md)

 [/inventory/consumables/{itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)
