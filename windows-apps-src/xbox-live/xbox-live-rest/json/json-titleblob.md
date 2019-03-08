---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
description: " TitleBlob (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 51a0b17a46d1c71ffdf9098d4637ca59d840c90a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612585"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
Enthält Informationen über einen Titel aus dem Speicher. 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
TitleBlob Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| clientFileTime| DateTime| [optional] Datum und Uhrzeit des letzten Hochladens der Datei.| 
| displayName| string| [optional] Der Name der Datei, die dem Benutzer angezeigt wird.| 
| etag| string| Tag für die Datei im herunterladen und Hochladen von Anforderungen.| 
| fileName| string| Der Name der Datei.| 
| size| 64-Bit-Ganzzahl mit Vorzeichen| Die Größe der Datei in Bytes.| 
| smartBlobType| string| [optional] Typ der Daten. Mögliche Werte sind: Konfigurationsdatei binary Json.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "fileName":"foo\bar\blob.txt,binary",
    "clientFileTime":"2012-01-01T01:02:03.1234567Z",
    "displayName":"Friendly Name",
    "size":12,
    "etag":"0x8CEB3E4F8F3A5BF",
    "smartBlobType":"binary"
}
      
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   