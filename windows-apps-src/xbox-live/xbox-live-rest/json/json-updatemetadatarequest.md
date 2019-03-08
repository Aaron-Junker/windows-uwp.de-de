---
title: UpdateMetadataRequest (JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
description: " UpdateMetadataRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a76e4b12e0ffadb112913775b500ac0d39d413d5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597765"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest (JSON)
Die Metadaten, die für einen Clip aktualisiert werden sollen. 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
UpdateMetadataRequest Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| userCaption| string| Ändert die eingegebene Benutzer nicht-lokalisierte Zeichenfolge für den Clip spielen.| 
| visibility| [GameClipVisibility-Enumeration](../enums/gvr-enum-gameclipvisibility.md)| Ändert die Sichtbarkeit des Spiels Clips an, wie sie in das System veröffentlicht wurde.| 
| titleData| string| Der Titel-spezifische Eigenschaftensammlung. Maximale Größe: 10 KB.| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 
Clip-Benutzernamen und Sichtbarkeit ändern:
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Ändern nur die Title-Eigenschaften (Dies ist nur ein Beispiel, da das Schema der dieses Feld hängt vom Aufrufer ist):
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Verweis 

[POST (/ Benutzer/me/Scids / {scid} /clips/ {GameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   