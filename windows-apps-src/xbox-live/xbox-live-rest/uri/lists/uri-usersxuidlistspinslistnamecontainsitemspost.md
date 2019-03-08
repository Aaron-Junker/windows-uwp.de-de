---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
assetID: 86ee6d1a-fb1f-b918-f605-a9b494c0e787
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamecontainsitemspost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c18bf70773de60d4c831d4be891255f98750efa8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636875"
---
# <a name="post-usersxuidxuidlistspinslistnamecontainsitems"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/ContainsItems
Bestimmt, ob eine Liste einen Satz von Elementen (angegeben durch die Element-ID) enthält, ohne die gesamte Liste abzurufen. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EAB)
  * [Abfragezeichenfolgen-Parameter](#ID4EJC)
  * [Anforderungstext](#ID4EUC)
  * [HTTP-Statuscodes](#ID4E6C)
  * [Erforderlichen Anforderungsheader](#ID4EVAAC)
  * [Antworttext](#ID4ELCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise 
 
Dieser Endpunkt ermöglicht den Aufrufer, zu bestimmen, ob ein oder mehrere Elemente in der angegebenen Liste vorhanden sein, ohne tatsächlich Abrufen der Liste, und suchen für sich selbst. Der Satz der Elemente werden durch Element-ID (oder Anbieter/ProviderId-Kombinationsfeld) identifiziert, und die Rückgabedaten werden die Daten, die mit ein boolescher Wert, der "true" oder "false" nzeige übergeben wird, ob die Liste jedes Element enthält. 
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| XUID| string| XUID des Benutzers.| 
| Listenname| string| Der Name der Liste zu bearbeiten.| 
  
<a id="ID4EJC"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter 
 
Abfrageparameter werden nicht unterstützt.
  
<a id="ID4EUC"></a>

 
## <a name="request-body"></a>Anforderungstext 
 

```cpp
{
  "Items":
  [{"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }]
}


    
```

  
<a id="ID4E6C"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK | Die Anforderung wurde erfolgreich abgeschlossen. Der Antworttext sollte die angeforderte Ressource (für eine GET) enthalten. POST- und PUT-Anforderungen erhalten auf dem neuesten Stand listenmetadaten (Version auflisten, Anzahl usw.).| 
| 201| Erstellt | Eine neue Liste wurde erstellt. Dies wird auf dem ersten Einfügen in eine Liste zurückgegeben. Die Antwort enthält, auf dem neuesten Stand Metadaten in der Liste aus, und der Location-Header enthält den URI für die Liste.| 
| 304| Nicht geändert.| Ruft zurückgegeben. Gibt an, dass der Client die neueste Version der Liste ist. Der Dienst vergleicht den Wert in der <b>If-Match</b> Header auf die Listenversion. Wenn sie gleich sind, ruft dann 304 ohne Daten zurückgegeben. | 
| 400| Ungültige Anforderung. | Die Anforderung war falsch formatiert. Eine wurde ein ungültiger Wert oder Typ für einen URI oder Abfrageparameter Zeichenfolge-Parameter kann sein. Das Ergebnis ein fehlender erforderlicher Parameter kann auch sein, oder Datenwert oder der Anforderung angegebene eine fehlende oder ungültige Version der API. Finden Sie unter den <b>X-XBL-Contract-Version</b> Header. | 
| 401| Nicht autorisiert | Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten | Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden | Der Aufrufer verfügt nicht über die Zugriffsrechte auf die Ressource. Dies gibt an, dass keine solche Liste erstellt wurde.| 
| 412| Vorbedingung nicht erfüllt | Gibt an, dass die Version der Liste geändert wurde und eine Ändern der Anforderung ist fehlgeschlagen. Eine Anforderung ändern ist eine INSERT-, Update-, oder entfernen. Der Dienst sucht die <b>If-Match</b> -Header für die Listenversion. Wenn sie nicht über die aktuelle Version der Liste übereinstimmt, klicken Sie dann der Vorgang schlägt fehl, und wird zusammen mit den aktuellen listenmetadaten (einschließlich die aktuelle Version) zurückgegeben. | 
| 500| Interner Serverfehler. | Der Dienst wird die Anforderung aufgrund eines Fehlers serverseitige vom abgewiesen.| 
| 501| Nicht implementiert. | Vom Aufrufer abgefragten einen URI, der nicht auf dem Server implementiert wurde. (Derzeit nur verwendet, wenn eine Anforderung für einen Namen ein, die nicht in der Whitelist enthalten ist erfolgt.)| 
| 503| Dienst nicht verfügbar. | Der Server verarbeitet die Anforderung, in der Regel aufgrund übermäßiger Auslastung. Nach einer Verzögerung (finden Sie unter den <b>Retry-after</b> Header), die Anforderung wiederholt werden kann. | 
  
<a id="ID4EVAAC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Enthält das STS-Token zum Authentifizieren und Autorisieren der Anforderungs verwendet. Muss ein Token vom XSTS-Dienst, und schließen Sie die XUID als einer der Ansprüche. | 
| X-XBL-Contract-Version| Gibt an, welche API-Version. angeforderte (positive Ganzzahlen) wird. Pins unterstützt, Version 2. Wenn dieser Header nicht vorhanden ist oder der Wert nicht unterstützt wird, der Dienst – 400 zurückgegeben werden mit "nicht unterstützt oder fehlender Vertrag Version-Header" in der statusbeschreibung ungültige Anforderung. | 
| Content-Type| Gibt an, ob der Inhalt der Anforderung/Antwort-Textkörper in Json oder Xml werden. Unterstützte Werte sind "Application/Json" und "Application/Xml"| 
| If-Match| Dieser Header muss die aktuelle Versionsnummer einer Liste enthalten, bei Anfragen zu verändern. Ändern von Anforderungen mit PUT, POST, oder Löschen von Verben. Wenn die Version im Header "If-Match" der Liste die aktuelle Version nicht übereinstimmt, wird die Anforderung abgelehnt werden, eine HTTP 412 – vorbedingungsfehler Rückgabecode. (optional) Kann auch für Get verwendet werden, wenn vorhanden und die übergebene Version übereinstimmt, die aktuelle Listenversion dann keine Daten und eine HTTP-304 nicht geändert-Code zurückgegeben werden. | 
  
<a id="ID4ELCAC"></a>

 
## <a name="response-body"></a>Antworttext 
 
Wenn der Aufruf erfolgreich ist, wird die Liste der Elemente zurückgegeben werden, sowie einen booleschen Wert für jedes Element, das angibt, ob das Element in der Liste oder nicht ist. 
 
<a id="ID4EVCAC"></a>

 
### <a name="sample-response"></a>Beispielantwort 
 

```cpp
{
  "ContainedItems":
  [{"Contained": "true",
    "Item":
   {"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId": null,
    "Provider": null
   }
  }]
}


      
```

   
<a id="ID4EBDAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EDDAC"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   