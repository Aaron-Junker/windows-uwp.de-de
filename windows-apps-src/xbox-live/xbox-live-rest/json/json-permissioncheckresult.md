---
title: PermissionCheckResult (JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
description: " PermissionCheckResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: dc13826be1b6f81201d069f5ade7ea5ba6668cd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598445"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult (JSON)
Die Ergebnisse einer Überprüfung von einem einzelnen Benutzer für eine einzelne Berechtigung-Einstellung für einen einzelnen Zielbenutzer. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
PermissionCheckResult Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| reason| string| Optional. Ein <b>PermissionResultCode</b> Wert, der angibt, warum die Berechtigung verweigert wurde Wenn <b>IsAllowed</b> wurde "false".| 
| restrictedSetting| string| Optional. Wenn die <b>PermissionResultCode</b> Wert in der <b>Grund</b> Member gibt an, dass eine Überprüfung der Berechtigungen für den anfordernden, dies gibt an, welche Berechtigungen Fehler.| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "reason": "MissingPrivilege",
    "restrictedSetting": "VideoCommunications"
}
    
```

  
<a id="ID4EIC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EKC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   