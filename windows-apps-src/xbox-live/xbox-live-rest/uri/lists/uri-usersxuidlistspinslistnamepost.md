---
title: POST (/users/xuid(xuid)/lists/PINS/{listname})
assetID: 813c0bd2-aca9-a1f6-9e81-a84a4c897b1e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamepost.html
description: " POST (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: d30e5407be128032947f3f701f59ef25a16daaea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589975"
---
# <a name="post-usersxuidxuidlistspinslistname"></a>POST (/users/xuid(xuid)/lists/PINS/{listname})
Fügt Elemente in der Liste am Index basierend auf dem Abfragezeichenfolgen-Parameter **InsertIndex**. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * ["Hinweise"](#ID4EY)
  * [URI-Parameter](#ID4ETB)
  * [Abfragezeichenfolgen-Parameter](#ID4E5B)
  * [Autorisierung](#ID4EZC)
  * [HTTP-Statuscodes](#ID4EGD)
  * [Erforderlichen Anforderungsheader](#ID4EEAAC)
  * [Beispiel für eine Anforderung](#ID4E1BAC)
  * [Antworttext](#ID4EPCAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Hinweise
 
Dieser Aufruf fügt Elemente in der Liste am Index basierend auf dem Abfragezeichenfolgen-Parameter **InsertIndex** (standardmäßig auf 0 oder am Anfang der Liste). Alle Elemente im Hauptteil Anforderung werden an diesem Punkt in der Liste eingefügt werden. Wenn die **InsertIndex** ist größer als die Anzahl der Elemente in der vorhandenen Liste, die neuen Elemente am Ende eingefügt werden.
 
Elemente eingefügt werden soll, müssen die erforderlichen Felder in der Spezifikation für funktionale angegeben; Andernfalls wird HTTP 400 zurückgegeben werden. Wenn das Ergebnis der Einfügung die maximale Größe der Liste überschreitet (pro Listentyp definiert) klicken Sie dann eine HTTP 400 zurückgegeben werden und nichts eingefügt werden soll.
 
Wenn das Element *nicht* , am Anfang oder Ende der Liste eingefügt werden soll und klicken Sie dann die **Wenn-Übereinstimmung: VersionNumber** Header ist erforderlich, um in der Anforderung enthalten sein. Der Header ist optional, wenn die Einfügung für den Anfang oder Ende ist. Falls vorhanden, wird der Header, unabhängig davon, wo Insert überprüft werden. In der Kopfzeile **VersionNumber** ist die aktuelle Versionsnummer der Liste. Wenn es nicht erforderlich, und enthalten oder stimmt nicht überein, der die aktuelle Versionsnummer für die Liste, und klicken Sie dann auf eine HTTP 412 (Vorbedingung nicht erfüllt) Statuscode wird zurückgegeben, und der Text der Antwort enthält die neuesten Metadaten in der Liste, die die aktuelle Versionsnummer enthält. Dies ist zum Schutz vor Updates von unterschiedlichen Clients trampling voneinander abhängig.
 
Beachten Sie, dass dieser Aufruf nicht Idempotent ist. wiederholte Aufrufe mit denselben Daten können mehrere Einfügevorgänge führen. Aber da keine Liste derzeit Duplikate unterstützt, werden wiederholte Aufrufe sehr wahrscheinlich HTTP 400 Fehlercodes zurückgegeben wird.
  
<a id="ID4ETB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Xbox-Benutzer-ID (XUID).| 
| listtype| string| Der Typ der Liste (wie diese verwendet werden und wie sie fungiert). Bezieht sich "FIXIERT" diese immer Methoden.| 
| Listenname| string| Name der Liste (die Liste mit einer bestimmten Listentyp handeln). Elemente immer "XBLPins" für in Pins ein.| 
  
<a id="ID4E5B"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| string| Optional. Definiert die Position zum Einfügen von Elementen. Unterstützte Werte: 0, positive ganze Zahlen und "End". Jeder Indexwert, der größer als die Anzahl der Listenelemente wird das neue Element am Ende der Liste hinzugefügt, und "blank" Leerzeichen werden nicht in der Liste einfügen. Standardwert: 0.| 
  
<a id="ID4EZC"></a>

 
## <a name="authorization"></a>Autorisierung
 
Dieser Aufruf erwartet, dass ein XSTS SAML-Token in der **Autorisierung** Header. Ein Anspruch Xuid muss in diese SAML-Token zum Identifizieren des Aufrufers vorhanden sein. Dieser Wert wird verwendet, um festzustellen, ob der Aufrufer über die Zugriffsrechte für die betreffenden Daten verfügt. Die Liste selbst wird ebenfalls die Xuid identifiziert werden und wird in der URI für die Liste enthalten sein. Anhand dieser wir möglicherweise in Zukunft unterstützt freigegebene Zugriff auf Listen, aber das ist keine Funktion zu diesem Zeitpunkt. Derzeit alle sind die, die ein Benutzer greift auf ihre eigenen und es gibt keinen gemeinsamer Zugriff. Daher muss die Xuid im URI der Xuid im SAML-Token Ansprüchen übereinstimmen. 

> [!NOTE] 
> Sowohl XBL Auth 2.0 und 3.0-Token werden derzeit unterstützt. 


  
<a id="ID4EGD"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EEAAC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Enthält das STS-Token zum Authentifizieren und Autorisieren der Anforderungs verwendet. Muss ein Token vom XSTS-Dienst, und schließen Sie die XUID als einer der Ansprüche. | 
| X-XBL-Contract-Version| Gibt an, welche API-Version. angeforderte (positive Ganzzahlen) wird. Pins unterstützt, Version 2. Wenn dieser Header nicht vorhanden ist oder der Wert nicht unterstützt wird, der Dienst – 400 zurückgegeben werden mit "nicht unterstützt oder fehlender Vertrag Version-Header" in der statusbeschreibung ungültige Anforderung.| 
| Content-Type| Gibt an, ob der Inhalt der Anforderung/Antwort-Textkörper in Json oder Xml werden. Unterstützte Werte sind "Application/Json" und "Application/Xml"| 
| If-Match| Dieser Header muss die aktuelle Versionsnummer einer Liste enthalten, bei Anfragen zu verändern. Ändern von Anforderungen mit PUT, POST, oder Löschen von Verben. Wenn die Version im Header "If-Match" der Liste die aktuelle Version nicht übereinstimmt, wird die Anforderung abgelehnt werden, eine HTTP 412 – vorbedingungsfehler Rückgabecode. (optional) Kann auch für Get verwendet werden, wenn vorhanden und die übergebene Version übereinstimmt, die aktuelle Listenversion dann keine Daten und eine HTTP-304 nicht geändert-Code zurückgegeben werden. | 
  
<a id="ID4E1BAC"></a>

 
## <a name="sample-request"></a>Beispiel für eine Anforderung
 
**ContentType**, **ItemId** oder **ProviderId**, **Anbieter** und **Gebietsschema** müssen angegeben werden.
 

```cpp
{"Items":
  [{
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": "",
    "Provider": "",
    "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC", 
    "AltImageUrl": null, 
    "Title": "The Dark Knight", 
    "SubTitle": null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
  }]}
      
```

  
<a id="ID4EPCAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EVCAC"></a>

 
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

   
<a id="ID4E6CAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EBDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   