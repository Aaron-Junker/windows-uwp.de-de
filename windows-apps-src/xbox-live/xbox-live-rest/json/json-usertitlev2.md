---
title: UserTitle (JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
description: " UserTitle (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 33901a5bde25fd17072c2b45d587a33209424378
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623025"
---
# <a name="usertitle-json"></a>UserTitle (JSON)
Enthält Daten zu Softwaretiteln. 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
UserTitle Objekt verfügt über den folgenden Spezifikationen. Alle Eigenschaften, die erforderlich sind.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| lastUnlock| DateTime| Die Zeit wurde zuletzt eine Auszeichnung erworben werden.| 
| titleId| 32-Bit-Ganzzahl ohne Vorzeichen| Der eindeutige Bezeichner für den Titel.| 
| titleVersion| string| Die Version des Titels.| 
| serviceConfigId| string| ID des primären Diensts Config dem Titel zugeordnet.| 
| titleType| string| Der Titeltyp.| 
| Plattform| string| Die unterstützte Plattform.| 
| name| string| Der Textname für diesen Titel. Die maximale Länge 22.| 
| earnedAchievements| 32-Bit-Ganzzahl ohne Vorzeichen| Die Anzahl der Erfolge für den Titel, einschließlich entsperrt Erfolge erworben und Herausforderungen erfolgreich abgeschlossen.| 
| currentGamerscore| 32-Bit-Ganzzahl ohne Vorzeichen| Die gesamte Gamerscore, die dieser Benutzer in diesem Titel erhalten hat.| 
| maxGamerscore| 32-Bit-Ganzzahl ohne Vorzeichen| Die gesamte mögliche Gamerscore für diesen Titel.| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   