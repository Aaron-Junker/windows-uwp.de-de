---
title: Reward (JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
description: " Reward (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1d58d34263e7e0e90091c41c1df4fd5e078f5055
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607495"
---
# <a name="reward-json"></a>Reward (JSON)
Der nutzen, die das Erreichen der Zertifikation zugeordnet wird.
<a id="ID4EN"></a>


## <a name="reward"></a>Belohnen

Reward Objekt verfügt über den folgenden Spezifikationen.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| name| string| Der Name der benutzerseitigen von der nutzen.|
| description| string| Die Benutzer gerichteten Beschreibung der nutzen.|
| value| string| Der Nutzen des Wert.|
| type| RewardType-enumeration| Der Typ des Reward: <ul><li>Ungültige (0): Eine unbekannte und nicht unterstützten Reward wurde konfiguriert.</li><li>Gamerscore (1): Der nutzen hinzugefügt des Spielers Gamerscore Punkte.</li><li>in-App (2): Die Belohnung definiert und bereitgestellt werden, durch den Titel.</li><li>Grafiken (3): Der Nutzen ist eine digitale Ressource.</li></ul> | 
| valueType| ProgressValueDataType-enumeration| Der Typ des Werts. Finden Sie unter [Anforderung (JSON)](json-requirement.md) für Weitere Informationen.|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
{
  "name":null,
  "description":null,
  "value":"10",
  "type":"Gamerscore",
  "valueType":"Int"
}

```


<a id="ID4EKD"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EMD"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
