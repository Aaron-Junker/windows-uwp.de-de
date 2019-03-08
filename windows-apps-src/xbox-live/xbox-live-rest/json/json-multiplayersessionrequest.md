---
title: MultiplayerSessionRequest (JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
description: " MultiplayerSessionRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 18929c060adeae47f0305422dd312e7410f93981
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628345"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest (JSON)
Die Anforderung JSON-Objekt übergeben, für einen Vorgang für eine **MultiplayerSession** Objekt. 
<a id="ID4EQ"></a>

  
 
Das MultiplayerSessionRequest JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| Konstanten| object| Nur-Lese Einstellungen, die zusammengeführt werden, mit der sitzungsvorlage, um die Konstanten für die Sitzung zu erstellen. | 
| Eigenschaften | object | Änderungen an den Eigenschaften von leistungssitzungen zusammengeführt werden soll.| 
| Members.Me | object| Konstanten und Eigenschaften, die ähnlich funktionieren wie ihre äquivalente der obersten Ebene. Eine PUT-Methode muss der Benutzer ein Mitglied der Sitzung sein, und fügt den Benutzer hinzu, falls erforderlich. Wenn Sie "me" als null angegeben wird, wird das Element, das die Anforderung aus der Sitzung entfernt. | 
| Elemente | object| Andere Objekte, die Benutzer auf die Sitzung hinzufügen durch einen nullbasierten Index als Schlüssel darstellen. Die Anzahl der Elemente in einer Anforderung beginnt immer mit 0 (null), auch wenn die Sitzung bereits Mitglieder enthält. Elemente werden für die Sitzung in der Reihenfolge hinzugefügt, in denen sie in der Anforderung angezeigt werden. Elementeigenschaften können nur vom Benutzer festgelegt werden, zu denen sie gehören. | 
| -Server | object| Werte, der angibt, Updates und Ergänzungen für die Sitzung die von Teilnehmern für zugeordnete Server festgelegt. Wenn ein Server als Null angegeben ist, wird dieser Eintrag aus der Sitzung entfernt. | 
  
<a id="ID4EZ"></a>

 
## <a name="request-structure"></a>Anforderung-Struktur
 

```json
{
  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },
  "members": {
    // Requires a service principal. Existing members can be deleted by index.
    // Not available on large sessions.
    "5": null,

    // Reservation requests must start with zero. New users will get added in order to the end of the session's member list.
    // Large sessions don't support reservations.
    "reserve_0": {
      "constants": { /* Property Bag */ }
    },
    "reserve_1": {
      "constants": { /* Property Bag */ }
    },

    // Requires a user principal with a xuid claim. Can be 'null' to remove myself from the session.
    "me": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
    }
  },

  // Requires a server principal.
  "servers": {
    // Can be any name. The value can be 'null' to remove the server from the session.
    "name": {
      "constants": {  /* Property Bag */ },
      "properties": {  /* Property Bag */ }
    }
  }
}
```

  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>Verweis 

[MultiplayerSession (JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/ {scid} /sessiontemplates/ {SessionTemplateName} /sessions/ {SessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   