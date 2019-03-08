---
title: Requirement (JSON)
assetID: 74faee8d-42e3-cfcf-22b3-9dcd9227de6b
permalink: en-us/docs/xboxlive/rest/json-requirement.html
description: " Requirement (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4e8ccd2d38c6683ef54ad1576f47a8d3e5197d4e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612605"
---
# <a name="requirement-json"></a>Requirement (JSON)
Die Unlock-Kriterien für das Erreichen der Zertifikation und wie weit der Benutzer auf die Erfüllung werden. 
<a id="ID4EN"></a>

 
## <a name="requirement"></a>Anforderung
 
Das Objekt für die Anforderung hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| id| string| Die ID der Anforderung.| 
| Aktuelle| string| Der aktuelle Wert der Fortschritt in Richtung der Anforderung.| 
| target| string| Der Zielwert der Anforderung.| 
| operationType| string| Der Vorgangstyp der Anforderung. Gültige Werte sind die Summe, Minimum, Maximum.| 
| ruleParticipationType| string| Der Typ der Beteiligung der Anforderung. Gültige Werte sind die Person, Gruppe.| 
  
<a id="ID4ETC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EVC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   