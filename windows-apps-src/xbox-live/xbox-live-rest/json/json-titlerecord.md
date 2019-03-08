---
title: TitleRecord (JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
description: " TitleRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 89baf7e9a737428d492246f1647a561a4a8170cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603985"
---
# <a name="titlerecord-json"></a>TitleRecord (JSON)
Informationen über einen Titel, einschließlich des Namens und einen Zeitstempel letzten Änderung. 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
Eine TitleRecord muss eine DeviceRecord oder eine LastSeenRecord enthalten, aber möglicherweise nicht enthalten.
 
TitleRecord Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| id| 32-Bit-Ganzzahl ohne Vorzeichen| TitleId des Datensatzes.| 
| name| string| Lokalisierte Name des Titels.| 
| Aktivität| [ActivityRecord](json-activityrecord.md)| Die Aktivität des Benutzers im Titel. Nur zurückgegeben, wenn "all" ist.| 
| lastModified| DateTime| UTC-Zeitstempel, wenn der Datensatz zuletzt aktualisiert wurde.| 
| Platzierung| string| Der Speicherort der app in der Benutzeroberfläche. Sie können beispielsweise "Fill", "full", "angedockt" oder "Hintergrund". Der Standardwert ist "full" für Geräte ohne die Möglichkeit, apps zu platzieren.| 
| Status| string| Der Status des Titels. Kann es sich "aktiv" oder "inaktiv" (Standard). Der Titel legt den Zustand basierend auf seine eigenen Kriterien für die Aktivität und inaktivitätszeiträume fest.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>Verweis 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   