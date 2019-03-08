---
title: PermissionCheckResponse (JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
description: " PermissionCheckResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7623a45fa21e30a015bd5c6a7c1f5add19cc189b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623365"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse (JSON)
Die Ergebnisse einer Überprüfung von einem einzelnen Benutzer für eine einzelne Berechtigung-Einstellung für einen einzelnen Zielbenutzer. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
PermissionCheckResponse Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| IsAllowed| Boolescher Wert| Erforderlich. Dieser Member ist <b>"true"</b> , wenn der anfordernde Benutzer die angeforderte Aktion mit dem Zielbenutzer ausführen darf.| 
| Ergebnisse| Array von [PermissionCheckResult (JSON)](json-permissioncheckresult.md)| Optional. Wenn <b>IsAllowed</b> wurde "false", und die Überprüfung wurde abgelehnt von etwas an die anfordernde Person beziehen, gibt an, warum die Berechtigung verweigert wurde.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   