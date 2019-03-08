---
title: MediaAsset (JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
description: " MediaAsset (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 308a65b7c049a6aba0405865bab63fb9d28b8506
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623475"
---
# <a name="mediaasset-json"></a>MediaAsset (JSON)
Die Medienobjekte, die das Erreichen der Zertifikation oder ihren Lohn zugeordnet wird.
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

MediaAsset Objekt verfügt über den folgenden Spezifikationen.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| name| string| Der Name des der MediaAsset, z. B. "tile01".|
| type| MediaAssetType-enumeration| Der Typ des dem videomedienobjekt: <ul><li>icon (0): Das Symbol "Leistung".</li><li>Grafiken (1): Das digitale ClipArt-Objekt.</li></ul> | 
| URL| string| Die URL des der MediaAsset.|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"https://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
