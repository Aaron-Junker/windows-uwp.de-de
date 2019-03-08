---
title: BatchRequest (JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
description: " BatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 51073c71700613edcd7c22e18cc0c00a9222d7e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654155"
---
# <a name="batchrequest-json"></a>BatchRequest (JSON)
Ein Array von Eigenschaften, mit denen Informationen über die Benutzerpräsenz, z. B. Benutzer, Geräte und Titeln zu filtern.
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

BatchRequest Objekt verfügt über den folgenden Spezifikationen.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| Benutzer| Array von Zeichenfolgen| Liste XUIDs von Benutzern, deren Anwesenheit, die Sie mit einem Maximum von 1100 XUIDs zu einem Zeitpunkt erfahren möchten.|
| deviceTypes| Array von Zeichenfolgen| Liste der Gerätetypen, die der Benutzer, die, denen Sie wissen möchten. Wenn das Array leer ist, wird standardmäßig alle möglichen Einheitentypen (d. h. keine werden herausgefiltert).|
| Titel| Array von 32-Bit-Ganzzahl ohne Vorzeichen| Liste mit Geräte-Typen, deren Benutzer Sie wissen möchten. Wenn das Array leer ist, wird standardmäßig alle möglichen Titel (d. h. keine werden herausgefiltert).|
| level| string| Mögliche Werte: <ul><li>Benutzer – Get-User-Knoten</li><li>Gerät - Get-Benutzer und Geräteknoten</li><li>Titel: Informieren Sie sich grundlegende Titel auf</li><li>All: umfassende Anwesenheitsinformationen, Informationen zu den Tasksequenzmedien oder beides zu erhalten</li></ul>Der Standardwert ist "Title".| 
| onlineOnly| Boolescher Wert| Wenn diese Eigenschaft auf "true" festgelegt ist, filtert der Batchvorgang Datensätze heraus für offline-Benutzer (einschließlich verdeckte werden). Wenn sie nicht angegeben ist, werden online und offline-Benutzer zurückgegeben.|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}


```


<a id="ID4EJD"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ELD"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Verweis   
