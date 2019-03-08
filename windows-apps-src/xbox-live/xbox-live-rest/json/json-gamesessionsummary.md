---
title: GameSessionSummary (JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
description: " GameSessionSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 8dace19404ae7c8b1d1ef296a21c874e4dd14c6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613385"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary (JSON)
Ein JSON-Objekt, die Zusammenfassungsdaten für eine Spiele-Sitzung darstellt. 
<a id="ID4EN"></a>

  
 
Das GameSessionSummary JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| creationTime| DateTime| Das Datum und Zeit, wenn die Sitzung, in UTC angegeben erstellt wurde. | 
| customData| Array von 8-Bit-Ganzzahlen ohne Vorzeichen| 1024 Bytes der spezifischen Daten. Dieser Wert ist für den Server nicht transparent ist. | 
| displayName| string| Die Anzeige Namen des Spiels Sitzung mit einer maximalen Länge von 128 Zeichen lang sein. Dieser Wert ist für den Server nicht transparent ist. | 
| hasEnded| Boolescher Wert| True, wenn die Sitzung beendet wurde, und false, andernfalls. Wenn dieses Feld auf "true" markiert die Spiele-Sitzung als schreibgeschützt ist, verhindert, dass weitere Daten an die Sitzung übermittelt werden. | 
| sessionId| Die Sitzungs-ID-Zeichenfolge | 
| titleId| 32-Bit-Ganzzahl ohne Vorzeichen| Die ID des Titels die game-Sitzung erstellen.| 
| Variant| 32-Bit-Ganzzahl mit Vorzeichen| Die Spiele-Variante. Dieser Wert ist für den Server nicht transparent ist.| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "hasEnded": false,
}
    
```

  
<a id="ID4ERD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ETD"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Verweis 

[GameSession (JSON)](json-gamesession.md)

   