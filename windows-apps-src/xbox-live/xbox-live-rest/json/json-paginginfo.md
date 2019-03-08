---
title: PagingInfo (JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
description: " PagingInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 0e773d73499e79fe23f736a536027932ca1a07b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651215"
---
# <a name="paginginfo-json"></a>PagingInfo (JSON)
Enthält Informationen für Ergebnisse, die in Seiten mit Daten zurückgegeben werden. 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| continuationToken| string| Es wurde eine opake Fortsetzungstoken für den Zugriff auf die nächste Seite der Ergebnisse. Maximal 32 Zeichen. Aufrufer können diesen Wert im Angeben der <b>"continuationtoken"</b> Abfrageparameter, um den nächsten Satz von Elementen in der Auflistung abzurufen. Wenn diese Eigenschaft <b>null</b>, und klicken Sie dann in der Auflistung keine weiteren Elemente vorhanden sind. Diese Eigenschaft ist erforderlich und wird bereitgestellt, selbst wenn die Auflistung mit ausgelagerte <b>SkipItems</b>.| 
| totalItems| 32-Bit-Ganzzahl mit Vorzeichen| Die Gesamtanzahl der Elemente in der Auflistung. Dieser Wert nicht angegeben, wenn der Dienst nicht in Echtzeit anzeigen in der Größe der Auflistung bereitstellen kann.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   