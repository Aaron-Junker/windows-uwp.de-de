---
title: DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
assetID: ad049340-f752-e05e-8b34-62adb8e4771f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameremoveitemsdelete.html
description: " DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: d26d8eaf54dcc14de3e31d7c2b54cd4454f2029f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594095"
---
# <a name="delete-usersxuidxuidlistspinslistnameremoveitems"></a>DELETE /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
Entfernt Elemente aus einer Liste anhand des Indexes an. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4ECB)
  * [Abfragezeichenfolgen-Parameter](#ID4ELC)
  * [Anforderungstext](#ID4END)
  * [HTTP-Statuscodes](#ID4EYD)
  * [Erforderlichen Anforderungsheader](#ID4EOBAC)
  * [Antworttext](#ID4EEDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise 
 
Entfernen von Elementen aus einer Liste. Zu entfernenden Elemente werden anhand ihres Indexes in der Liste und in den Abfragezeichenfolgen-Parameter "Indizes" identifiziert werden. Die Liste der Indizes muss eine durch Trennzeichen getrennt, und nur 100 Indizes in einem einzigen Aufruf übergeben werden können. Jedoch keine Indizes (leere Abfragezeichenfolgen-Parameter) übergeben werden dann die gesamte Liste der Inhalt gelöscht werden (Liste bleibt aber leer ist, Versionsnummer wird weiterhin erhöht werden). Nachdem die Elemente entfernt wurden, wird die Liste so, dass keine Sicherheitslücken übersehen übrig sind, in der Reihenfolge von Indizes "komprimiert". 
 
Für diesen Aufruf muss eine "Wenn-Übereinstimmung: VersionNumber" Header in der Anforderung eingeschlossen werden, in denen ist VersionNumber die aktuelle Versionsnummer der Datei. Falls es nicht enthalten ist, oder nicht der aktuellen entspricht Version auflisten, die der Zahl und anschließend eine HTTP 412-Vorbedingung Fehlerstatuscode zurückgegeben wird und der Text der Antwort enthält die neuesten Metadaten der Liste die aktuelle Versionsnummer enthält. Dies erfolgt zum Schutz gegen Updates von unterschiedlichen Clients trampling voneinander. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| XUID| string| XUID des Benutzers.| 
| Listenname| string| Der Name der Liste zu bearbeiten.| 
  
<a id="ID4ELC"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Indizes| string| 0 (null) oder eine positive ganze Zahl. Die Zahlen müssen nicht zusammenhängend sein. Zum Beispiel die Abfrage parameterindizes = 1, 8, entfernt die Elemente in den Indizes, 1 und 8. <b>Wenn kein Index angegeben ist, wird die gesamte Liste gelöscht.</b>| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Der Anforderungstext muss für diesen Aufruf leer sein.
  
<a id="ID4EYD"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes 
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EOBAC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Enthält das STS-Token zum Authentifizieren und Autorisieren der Anforderungs verwendet. Muss ein Token vom XSTS-Dienst, und schließen Sie die XUID als einer der Ansprüche. | 
| X-XBL-Contract-Version| Gibt an, welche API-Version. angeforderte (positive Ganzzahlen) wird. Pins unterstützt, Version 2. Wenn dieser Header nicht vorhanden ist oder der Wert nicht unterstützt wird, der Dienst – 400 zurückgegeben werden mit "nicht unterstützt oder fehlender Vertrag Version-Header" in der statusbeschreibung ungültige Anforderung. | 
| Content-Type| Gibt an, ob der Inhalt der Anforderung/Antwort-Textkörper in Json oder Xml werden. Unterstützte Werte sind "Application/Json" und "Application/Xml"| 
| If-Match| Dieser Header muss die aktuelle Versionsnummer einer Liste enthalten, bei Anfragen zu verändern. Ändern von Anforderungen mit PUT, POST, oder Löschen von Verben. Wenn die Version im Header "If-Match" der Liste die aktuelle Version nicht übereinstimmt, wird die Anforderung abgelehnt werden, eine HTTP 412 – vorbedingungsfehler Rückgabecode. (optional) Kann auch für Get verwendet werden, wenn vorhanden und die übergebene Version übereinstimmt, die aktuelle Listenversion dann keine Daten und eine HTTP-304 nicht geändert-Code zurückgegeben werden. | 
  
<a id="ID4EEDAC"></a>

 
## <a name="response-body"></a>Antworttext 
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst die neueste Metadaten für die Liste an. 
 
<a id="ID4EODAC"></a>

 
### <a name="sample-response"></a>Beispielantwort 
 

```cpp
{
        "ListVersion":  1,
        "ListCount":  1,
        "MaxListSize": 200,
        "AllowDuplicates": "true",
        "AccessSetting":  "OwnerOnly"
        }

      
```

   
<a id="ID4E1DAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E3DAC"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   