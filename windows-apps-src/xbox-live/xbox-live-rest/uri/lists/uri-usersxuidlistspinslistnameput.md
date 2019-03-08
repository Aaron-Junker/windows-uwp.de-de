---
title: PUT (/users/xuid(xuid)/lists/PINS/{listname})
assetID: f7964d2e-a8c8-2caa-ca97-e0d76ef586f4
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameput.html
description: " PUT (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 35527df28c2ca482d0c5ae2fe25637b3bc29f6ca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622925"
---
# <a name="put-usersxuidxuidlistspinslistname"></a>PUT (/users/xuid(xuid)/lists/PINS/{listname})
Aktualisiert die Elemente in Listen mit den Indizes, die für die einzelnen Elemente im Anforderungstext angegeben. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4E1B)
  * [Autorisierung](#ID4EFC)
  * [HTTP-Statuscodes](#ID4ESC)
  * [Erforderlichen Anforderungsheader](#ID4EPH)
  * [Anforderungstext](#ID4EGBAC)
  * [Antworttext](#ID4EWBAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Dieser Aufruf aktualisiert die Elemente in Listen mit den Indizes, die für die einzelnen Elemente im Anforderungstext angegeben. Dieser Aufruf fügt keine Elemente in der Liste aus, und wenn Elemente nicht an den angegebenen Indizes vorhanden sind, klicken Sie dann den Aufruf zurück, eine 400 Bad Request-Status. Mehrere Elemente in einem einzigen Aufruf aktualisiert werden können, jedoch müssen alle in der aktuellen Liste vorhanden sein. D. h. alle aktualisiert oder keine aktualisiert.
 
Dieser Aufruf kann das Element, das aktualisiert werden, um von bestimmt die **ItemId** anstelle von **Index**. Zu diesem Zweck verwenden Sie einfach "-1" für den Index in die **IndexedItems** -Struktur, die an den Dienst gesendet wird. Offensichtlich in diesem Fall die **ItemId** kann nicht als Teil des Updates geändert werden, damit es nur für Änderungen an anderen Metadatenfelder funktioniert. Die **Anbieter**/**ProviderId** Kombinationsfeld kann verwendet werden, anstelle von **ItemId** zum Identifizieren des Elements. Intern sucht der Dienst die Liste für diese Elemente und bestimmt, die richtigen Indizes zu aktualisieren. Wenn das Element bzw. die Elemente nicht gefunden werden können, klicken Sie dann einen 400 Bad Request-Status zurückgegeben und keine Elemente aktualisiert werden.
 
Dieser Aufruf erfordert eine **Wenn-Übereinstimmung: VersionNumber** Header in der Anforderung enthalten sein, wenn Indizes zu verwenden, um Elemente zu identifizieren. Wenn mit der Artikel IDs fest, welche Elemente (und die Liste Duplikate sind unzulässig), und klicken Sie dann die **If-Match** -Header ist optional. Falls vorhanden, die **If-Match** Header wird immer überprüft werden. In der Kopfzeile der **VersionNumber** ist die aktuelle Versionsnummer der Liste. Wenn dabei handelt es sich nicht enthalten (erforderlich), oder ist nicht entspricht, gibt die aktuelle Versionsnummer für die Liste, und klicken Sie dann auf einen Fehler bei HTTP 412-Vorbedingung-Statuscode zurückgegeben wird und der Text der Antwort enthält die neuesten Metadaten der Liste, die die aktuelle Versionsnummer enthält. Dies ist zum Schutz vor Updates von unterschiedlichen Clients trampling voneinander abhängig.
  
<a id="ID4E1B"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Xbox-Benutzer-ID (XUID).| 
| listtype| string| Der Typ der Liste (wie diese verwendet werden und wie sie fungiert). Bezieht sich "FIXIERT" diese immer Methoden.| 
| Listenname| string| Name der Liste (die Liste mit einer bestimmten Listentyp handeln). Elemente immer "XBLPins" für in Pins ein.| 
  
<a id="ID4EFC"></a>

 
## <a name="authorization"></a>Autorisierung
 
Dieser Aufruf erwartet, dass ein XSTS SAML-Token in der **Autorisierung** Header. Ein Anspruch Xuid muss in diese SAML-Token zum Identifizieren des Aufrufers vorhanden sein. Dieser Wert wird verwendet, um festzustellen, ob der Aufrufer über die Zugriffsrechte für die betreffenden Daten verfügt. Die Liste selbst wird ebenfalls die Xuid identifiziert werden und wird in der URI für die Liste enthalten sein. Anhand dieser wir möglicherweise in Zukunft unterstützt freigegebene Zugriff auf Listen, aber das ist keine Funktion zu diesem Zeitpunkt. Derzeit alle sind die, die ein Benutzer greift auf ihre eigenen und es gibt keinen gemeinsamer Zugriff. Daher muss die Xuid im URI der Xuid im SAML-Token Ansprüchen übereinstimmen. 

> [!NOTE] 
> Sowohl XBL Auth 2.0 und 3.0-Token werden derzeit unterstützt. 


  
<a id="ID4ESC"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Anforderung wurde erfolgreich abgeschlossen. Der Antworttext sollte die angeforderte Ressource (für eine GET) enthalten. POST- und PUT-Anforderungen erhalten auf dem neuesten Stand listenmetadaten (Version auflisten, Anzahl usw.).| 
| 201| Erstellt| Eine neue Liste wurde erstellt. Dies wird auf dem ersten Einfügen in eine Liste zurückgegeben. Die Antwort enthält, auf dem neuesten Stand Metadaten in der Liste aus, und der Location-Header enthält den URI für die Liste.| 
| 304| Nicht geändert.| Ruft zurückgegeben. Gibt an, dass der Client die neueste Version der Liste ist. Der Dienst vergleicht den Wert in der <b>If-Match</b> Header auf die Listenversion. Wenn sie gleich sind, ruft dann 304 ohne Daten zurückgegeben.| 
| 400| Ungültige Anforderung.| Die Anforderung war falsch formatiert. Eine wurde ein ungültiger Wert oder Typ für einen URI oder Abfrageparameter Zeichenfolge-Parameter kann sein. Das Ergebnis ein fehlender erforderlicher Parameter kann auch sein, oder Datenwert oder der Anforderung angegebene eine fehlende oder ungültige Version der API. Finden Sie unter den <b>X-XBL-Contract-Version</b> Header.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Der Aufrufer verfügt nicht über die Zugriffsrechte auf die Ressource. Dies gibt an, dass keine solche Liste erstellt wurde.| 
| 412| Vorbedingung nicht erfüllt| Gibt an, dass die Version der Liste geändert wurde und eine Ändern der Anforderung ist fehlgeschlagen. Eine Anforderung ändern ist eine INSERT-, Update-, oder entfernen. Der Dienst sucht die <b>If-Match</b> -Header für die Listenversion. Wenn sie nicht über die aktuelle Version der Liste übereinstimmt, klicken Sie dann der Vorgang schlägt fehl, und wird zusammen mit den aktuellen listenmetadaten (einschließlich die aktuelle Version) zurückgegeben.| 
| 500| Interner Serverfehler.| Der Dienst wird die Anforderung aufgrund eines Fehlers serverseitige vom abgewiesen.| 
| 501| Nicht implementiert.| Vom Aufrufer abgefragten einen URI, der nicht auf dem Server implementiert wurde. (Derzeit nur verwendet, wenn eine Anforderung für einen Namen ein, die nicht in der Whitelist enthalten ist erfolgt.)| 
| 503| Dienst nicht verfügbar.| Der Server verarbeitet die Anforderung, in der Regel aufgrund übermäßiger Auslastung. Nach einer Verzögerung (finden Sie unter den <b>Retry-after</b> Header), die Anforderung wiederholt werden kann.| 
  
<a id="ID4EPH"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Enthält das STS-Token zum Authentifizieren und Autorisieren der Anforderungs verwendet. Muss ein Token vom XSTS-Dienst, und schließen Sie die XUID als einer der Ansprüche. | 
| X-XBL-Contract-Version| Gibt an, welche API-Version. angeforderte (positive Ganzzahlen) wird. Pins unterstützt, Version 2. Wenn dieser Header nicht vorhanden ist oder der Wert nicht unterstützt wird, der Dienst – 400 zurückgegeben werden mit "nicht unterstützt oder fehlender Vertrag Version-Header" in der statusbeschreibung ungültige Anforderung.| 
| Content-Type| Gibt an, ob der Inhalt der Anforderung/Antwort-Textkörper in Json oder Xml werden. Unterstützte Werte sind "Application/Json" und "Application/Xml"| 
| If-Match| Dieser Header muss die aktuelle Versionsnummer einer Liste enthalten, bei Anfragen zu verändern. Ändern von Anforderungen mit PUT, POST, oder Löschen von Verben. Wenn die Version im Header "If-Match" der Liste die aktuelle Version nicht übereinstimmt, wird die Anforderung abgelehnt werden, eine HTTP 412 – vorbedingungsfehler Rückgabecode. (optional) Kann auch für Get verwendet werden, wenn vorhanden und die übergebene Version übereinstimmt, die aktuelle Listenversion dann keine Daten und eine HTTP-304 nicht geändert-Code zurückgegeben werden. | 
  
<a id="ID4EGBAC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
<a id="ID4EMBAC"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{"IndexedItems":
 [{ "Index": 0, 
     "Item": 
     {
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": null,
    "Provider": null,
    "ImageUrl": "https://www.bing.com/thumb/...",
    "Title": "The Dark Knight",
    "SubTitle":null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
}
}]}      
      
```

   
<a id="ID4EWBAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4E3BAC"></a>

 
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

   
<a id="ID4EGCAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EICAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   