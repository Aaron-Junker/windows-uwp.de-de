---
title: ServiceError (JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
description: " ServiceError (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: da3d682a1b66d25a12f21a93e9596d13afae7f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631705"
---
# <a name="serviceerror-json"></a>ServiceError (JSON)
Enthält Informationen zu einem Fehler wird zurückgegeben, wenn Fehler bei einem Aufruf an den Dienst an. 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
ServiceError Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| code| 32-Bit-Ganzzahl mit Vorzeichen | Der Typ des Fehlers. Siehe Tabelle unten für mögliche Werte. | 
| Quelle| string | Der Name des Diensts, der den Fehler ausgelöst hat. Z. B. den Wert <code>ReputationFD</code> gibt an, dass Fehler in der Reputation Service. | 
| description| string| Eine Beschreibung des Fehlers. | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>Fehlercodes
 
| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | 
| 0| Erfolg kein Fehler| 
| 4000| Ungültige Anforderung Text das JSON-Dokument mit einer Überprüfung der POST-Anforderung konnte nicht übermittelt. Finden Sie in das Feld "Beschreibung" Weitere Informationen. | 
| 4100| Benutzer ist nicht vorhanden ist die XUID im Anforderungs-URI enthalten sind, stellt keine gültigen Benutzer in XBOX Live dar.| 
| 4500| Fehler bei der der Aufrufer ist nicht autorisiert, auf den angeforderten Vorgang auszuführen.| 
| 5000| Es Fehler Service ist ein interner Fehler im Dienst| 
| 5300| Dienst nicht verfügbar ist der Dienst ist nicht verfügbar.| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
    
```

  
<a id="ID4EZE"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E2E"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   