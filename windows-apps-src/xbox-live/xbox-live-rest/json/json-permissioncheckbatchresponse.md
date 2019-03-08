---
title: PermissionCheckBatchResponse (JSON)
assetID: f157ed8d-7483-1b34-bc3d-e3fcf6a7d055
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchresponse.html
description: " PermissionCheckBatchResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 6ac79d3361e83993c372b1d651e97e900788d39f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613345"
---
# <a name="permissioncheckbatchresponse-json"></a>PermissionCheckBatchResponse (JSON)
Überprüfen Sie die Ergebnisse einer Batch-Berechtigung für eine Liste der Berechtigungswerte für mehrere Benutzer. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchresponse"></a>PermissionCheckBatchResponse
 
PermissionCheckBatchResponse Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| Responses| Array von [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)| Erforderlich. Ein [PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md) Objekt für jede Berechtigung, die für die in der ursprünglichen Anforderung, in der gleichen Reihenfolge wie die Anforderung gestellt wurde.| 
  
<a id="ID4EQB"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRestrictsTarget", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}
    
```

  
<a id="ID4EZB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E2B"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   