---
title: Property (JSON)
assetID: 93de547e-d936-6fcc-92cb-e4dd284dd609
permalink: en-us/docs/xboxlive/rest/json-property.html
description: " Property (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7e2a721886509c49c60d663d491f8d49bc3c95e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659705"
---
# <a name="property-json"></a>Property (JSON)
Enthält die Eigenschaftendaten für die Kriterien der Vermittlung-Anforderung vom Client bereitgestellt.
<a id="ID4EN"></a>


## <a name="property"></a>Eigenschaft

Das Eigenschaftenobjekt hat den folgenden Spezifikationen.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| id| string| Eine Id für diese Eigenschaft.|
| type| 32-Bit-Ganzzahl mit Vorzeichen | Der Typ der Eigenschaft. Werte werden unterstützt: <ul><li>0 = ganze Zahl</li><li>1 = string</li></ul>| 
| value| string| Der Wert dieser Eigenschaft.|

<a id="ID4EGC"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
{
    "id":"1",
    "value":"20",
    "type":0
}

```


<a id="ID4EPC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ERC"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
