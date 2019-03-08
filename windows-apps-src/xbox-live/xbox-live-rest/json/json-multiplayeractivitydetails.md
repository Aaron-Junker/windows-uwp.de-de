---
title: MultiplayerActivityDetails (JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
description: " MultiplayerActivityDetails (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 188bcebb8d6bff879f30dcc83d7039fbcbfae0b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658105"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails (JSON)
Ein JSON-Objekt, das **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**. 

> [!NOTE] 
> Dieses Objekt wird implementiert, indem die 2015 Multiplayer-Spiele und gilt nur für diese Multiplayer-Version und höher. Es ist für die Verwendung mit Vorlage Vertrag 104/105 oder höher vorgesehen.  

 
<a id="ID4ES"></a>

  
 
Das MultiplayerActivityDetails JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| Ein <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b> Objekt, das identifizierende Informationen für die Sitzung darstellt.| 
| HandleId| 64-Bit-Ganzzahl ohne Vorzeichen| Die Handle-ID der Aktivität entspricht.| 
| TitleId| 32-Bit-Ganzzahl ohne Vorzeichen| Die Title-ID, die zum Verknüpfen der Aktivitäts gestartet werden soll.| 
| Sichtbarkeit| MultiplayerSessionVisibility| Ein <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b> Wert, der den Sichtbarkeitsstatus der Sitzung.| 
| JoinRestriction| MultiplayerSessionJoinRestriction| Ein <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b> Wert, der angibt, der Join-Einschränkung für die Sitzung. Diese Einschränkung gilt, wenn das Feld "Sichtbarkeit" auf "Öffnen" festgelegt ist.| 
| Geschlossen| Boolescher Wert| True, wenn die Sitzung vorübergehend für die Teilnahme an, und "false" andernfalls geschlossen wird.| 
| OwnerXboxUserId| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox-Benutzer-ID des Mitglieds, das die Aktivität besitzt.| 
| MaxMembersCount| 32-Bit-Ganzzahl ohne Vorzeichen| Anzahl der insgesamt Steckplätze.| 
| MembersCount| 32-Bit-Ganzzahl ohne Vorzeichen| Anzahl der Slots belegt wird.| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
  "results": [{
    "id": "11111111-ebe0-42da-885f-033860a818f6",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "party",
      "name": "e3a836aeac6f4cbe9bcab985494d3175"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": true,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  },
  {
    "id": "11111111-ebe0-42da-885f-033860a818f7",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "TitleStorageTestDefault",
      "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": false,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  }]
}
    
```

  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   