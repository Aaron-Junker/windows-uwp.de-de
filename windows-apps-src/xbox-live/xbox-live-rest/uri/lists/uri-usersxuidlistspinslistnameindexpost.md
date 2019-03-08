---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: df61be42-c229-7408-5e4c-dbf4ae95b52b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindexpost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7711beee6551c40afe1afcb031278484a3dc5820
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627465"
---
# <a name="post-usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Verschiebt ein Element in einer Liste an eine andere Position in der Liste an. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EEB)
  * [Abfragezeichenfolgen-Parameter](#ID4EWC)
  * [Anforderungstext](#ID4EVD)
  * [HTTP-Statuscodes](#ID4EEE)
  * [Erforderlichen Anforderungsheader](#ID4E1BAC)
  * [Antworttext](#ID4EQDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise 
 
Dieser Aufruf wird bereitgestellt, um die einfache Verschieben der ein Element in einen anderen Index in der Liste in einem einzigen Vorgang zuzulassen. Nur ein Element kann zu einem Zeitpunkt verschoben werden. Wenn der Index des zu verschiebenden Elements nicht vorhanden ist, wird HTTP 400 zurückgegeben werden. Der Index für die Einfügemarke folgt denselben Regeln wie das Einfügen einer standard. Wird standardmäßig auf 0 – der Anfang der Liste aus, und eine beliebige Anzahl größer als die Anzahl der Elemente in der Liste wird das Element am Ende der Liste erneut einfügen. Auf ähnliche Weise kann am Ende der Liste angegeben werden, indem Sie "End" für InsertIndex übergeben wird. 
 
Dieser Aufruf können Sie zum Identifizieren des Elements vom Element-ID (oder Anbieter/ProviderId-Kombinationsfeld) verschoben werden soll. In diesem Fall die Element-ID muss im Text der Anforderung übergeben werden, und es muss ein vorhandenes Element in der Liste entsprechen. Wenn dies nicht der Fall ist, wird HTTP 400-Fehler in der Beschreibung mit IndexOutOfRange zurückgegeben werden soll. Verwenden Sie "-1" für diese spezielle Version des Aufrufs wie der Index des Elements verschoben werden soll. 
 
Für diesen Aufruf muss eine "Wenn-Übereinstimmung: VersionNumber" Header in der Anforderung berücksichtigt werden sollen, wenn das Element angegeben Index. Wenn Element-ID verwenden, die zu verschiebende Element angeben, ist der Header "If-Match" optional. Falls vorhanden, wird der Header "If-Match" immer überprüft werden. Im Header "If-Match" ist VersionNumber die aktuelle Versionsnummer der Liste. Wenn es nicht enthalten (und erforderlich ist), oder entspricht nicht der aktuellen Version auflisten, die der Zahl und anschließend eine HTTP 412-Vorbedingung Fehlerstatuscode zurückgegeben wird und der Text der Antwort enthält die neuesten Metadaten, der die Liste enthält die aktuelle Versionsnummer . Dies erfolgt zum Schutz gegen Updates von unterschiedlichen Clients trampling voneinander. 
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| XUID| string| XUID des Benutzers.| 
| Listenname| string| Der Name der Liste zu bearbeiten.| 
| index| string| Gibt den aktuellen Index des Elements, das verschoben werden. Wenn der Indexwert 0 (null) oder eine positive ganze Zahl ist, bezieht sich auf den aktuellen Index des Elements, und der Anforderungstext des Aufrufs sollte leer sein. Wenn der Indexwert "-1" ist, muss das Element, das verschoben werden von Element-ID oder der Anbieter/ProviderID im Hauptteil Anforderung des Aufrufs angegeben werden.| 
  
<a id="ID4EWC"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| string| Gibt den Listenspeicherort, um das Element einzufügen. Zulässige Werte sind 0 (null), positive ganze Zahlen und "End". "End" platziert das Element am Ende der aktuellen Liste. Wenn der angegebene Wert nach dem Ende der Liste ist, wird das Element am Ende der Liste eingefügt. | 
  
<a id="ID4EVD"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Ein Anforderungstext wird gesendet, nur, wenn das Element angegeben wird, durch die Element-ID oder Anbieter/ProviderId verschieben.
 
<a id="ID4E6D"></a>

  
Beispiel für eine Anforderung 

```cpp
{
  "Item":
  {
    "ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }
}
    
```

  
<a id="ID4EEE"></a>

 
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
  
<a id="ID4E1BAC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Enthält das STS-Token zum Authentifizieren und Autorisieren der Anforderungs verwendet. Muss ein Token vom XSTS-Dienst, und schließen Sie die XUID als einer der Ansprüche. | 
| X-XBL-Contract-Version| Gibt an, welche API-Version. angeforderte (positive Ganzzahlen) wird. Pins unterstützt, Version 2. Wenn dieser Header nicht vorhanden ist oder der Wert nicht unterstützt wird, der Dienst – 400 zurückgegeben werden mit "nicht unterstützt oder fehlender Vertrag Version-Header" in der statusbeschreibung ungültige Anforderung.| 
| Content-Type| Gibt an, ob der Inhalt der Anforderung/Antwort-Textkörper in Json oder Xml werden. Unterstützte Werte sind "Application/Json" und "Application/Xml"| 
| If-Match| Dieser Header muss die aktuelle Versionsnummer einer Liste enthalten, bei Anfragen zu verändern. Ändern von Anforderungen mit PUT, POST, oder Löschen von Verben. Wenn die Version im Header "If-Match" der Liste die aktuelle Version nicht übereinstimmt, wird die Anforderung abgelehnt werden, eine HTTP 412 – vorbedingungsfehler Rückgabecode. (optional) Kann auch für Get verwendet werden, wenn vorhanden und die übergebene Version übereinstimmt, die aktuelle Listenversion dann keine Daten und eine HTTP-304 nicht geändert-Code zurückgegeben werden. | 
  
<a id="ID4EQDAC"></a>

 
## <a name="response-body"></a>Antworttext 
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst die neueste Metadaten für die Liste an. 
 
<a id="ID4E1DAC"></a>

 
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

   
<a id="ID4EIEAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EKEAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

   