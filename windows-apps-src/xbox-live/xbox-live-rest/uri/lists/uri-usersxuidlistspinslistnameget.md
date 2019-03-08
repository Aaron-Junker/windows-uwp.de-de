---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1a31e6a6b541276d3191ba5d40767a1929c70168
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641625"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
Gibt den Inhalt einer Liste zurück. Die Domäne für diese URIs ist `eplists.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EIB)
  * [Abfragezeichenfolgen-Parameter](#ID4ETB)
  * [Autorisierung](#ID4ESD)
  * [Erforderlichen Anforderungsheader](#ID4E6D)
  * [Anforderungstext](#ID4EVF)
  * [HTTP-Statuscodes](#ID4EAG)
  * [Antworttext](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Die **ListCount** Feld in den zurückgegebenen Daten gibt an, wie viele Elemente in der gesamten Liste verwaltet durch den Dienst – daher sind, sondern die verwendet werden kann, um zu ermitteln, in dem das Ende der Liste, und dies ist möglicherweise eine unterschiedliche Anzahl von wie vielen bestimmte Elemente wurden von der Anforderung zurückgegeben.
 
Enthalten keine Listenelemente, die Ergebnisse, wenn die Liste ist noch nicht vorhanden. die **ListCount** werden 0 (null) und die **ListVersion** wird 0 (null) sein.
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Xbox-Benutzer-ID (XUID).| 
| listtype| string| Der Typ der Liste (wie diese verwendet werden und wie sie fungiert). Bezieht sich "FIXIERT" diese immer Methoden.| 
| Listenname| string| Name der Liste (die Liste mit einer bestimmten Listentyp handeln). Elemente immer "XBLPins" für in Pins ein.| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| 32-Bit-Ganzzahl mit Vorzeichen| Optional. Anzahl der Elemente in der Enumeration vor der Rückgabe der Ergebnisse übersprungen werden sollen. Standardwert: 0.| 
| "MaxItems"| 32-Bit-Ganzzahl mit Vorzeichen| Optional. Maximale Anzahl der zurückzugebenden Elemente. Der Standardwert ist 25 Elemente auf, wenn kein Maximum in der Anforderung angegeben ist. Der Dienst wird keine maximal auf diesem Wert platziert; Wenn der Wert größer als die Anzahl der Elemente in der Liste ist, werden alle Elemente und kein Fehler zurückgegeben werden.| 
| filterItemId| string| Optional. Gibt das Element, in der Liste gesucht. Gibt alle Instanzen des Elements in der Liste zurück. Ermöglicht dem Client können Sie schnell feststellen, wenn und wo ein Element in einer Liste ist. Nützlich bei großen Listen, die alle Instanzen eines Elements zu bestimmen, ohne die gesamte Liste durchlaufen. Standardwert: null.| 
| filterContentType| string| Optional. Gibt eine durch Trennzeichen getrennte Liste der Inhaltstypen zurückzugebenden (werden keine Rückgabetypen nicht in der Liste). Verwendet, um nur bestimmte Inhaltstypen aus einer Liste abrufen. Eine durch Trennzeichen getrennte Liste der Inhaltstypen den Typ wird für diesen Filter verwendet. (Mehrere Inhaltstypen können in einem einzigen Aufruf abgefragt werden.) Unterstützten Inhaltstypen umfassen alle Medientypen, die von der Unterhaltung Discovery Services (EDS) definiert. Standardwert: Null (alle Inhaltstypen).| 
| filterDeviceType| string| Optional. Gibt eine durch Trennzeichen getrennte Liste von Gerätetypen an zurückgeben (werden keine Rückgabetypen nicht in der Liste). Filtert die zurückgegebenen Menge, die nur zurückzugeben, die aus einem bestimmten Satz von Gerätetypen eingefügt wurden. Eine durch Trennzeichen getrennte Liste von Gerätetypen wird für diesen Filter verwendet (es können in einem Aufruf mehrere Gerätetypen abgefragt werden). Mögliche Werte: XboxOne, MCapensis, WindowsPhone, WindowsPhone7, Web, PC, MoLive. Standardwert: Null (alle Inhaltstypen).| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>Autorisierung
 
Dieser Aufruf erwartet, dass ein XSTS SAML-Token in der **Autorisierung** Header. Ein Anspruch Xuid muss in diese SAML-Token zum Identifizieren des Aufrufers vorhanden sein. Dieser Wert wird verwendet, um festzustellen, ob der Aufrufer über die Zugriffsrechte für die betreffenden Daten verfügt. Die Liste selbst wird ebenfalls die Xuid identifiziert werden und wird in der URI für die Liste enthalten sein. Anhand dieser wir möglicherweise in Zukunft unterstützt freigegebene Zugriff auf Listen, aber das ist keine Funktion zu diesem Zeitpunkt. Derzeit alle sind die, die ein Benutzer greift auf ihre eigenen und es gibt keinen gemeinsamer Zugriff. Daher muss die Xuid im URI der Xuid im SAML-Token Ansprüchen übereinstimmen. 

> [!NOTE] 
> Sowohl XBL Auth 2.0 und 3.0-Token werden derzeit unterstützt. 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Enthält das STS-Token zum Authentifizieren und Autorisieren der Anforderungs verwendet. Muss ein Token vom XSTS-Dienst, und schließen Sie die XUID als einer der Ansprüche. | 
| X-XBL-Contract-Version| Gibt an, welche API-Version. angeforderte (positive Ganzzahlen) wird. Pins unterstützt, Version 2. Wenn dieser Header nicht vorhanden ist oder der Wert nicht unterstützt wird, der Dienst – 400 zurückgegeben werden mit "nicht unterstützt oder fehlender Vertrag Version-Header" in der statusbeschreibung ungültige Anforderung.| 
| Content-Type| Gibt an, ob der Inhalt der Anforderung/Antwort-Textkörper in Json oder Xml werden. Unterstützte Werte sind "Application/Json" und "Application/Xml"| 
| If-Match| Dieser Header muss die aktuelle Versionsnummer einer Liste enthalten, bei Anfragen zu verändern. Ändern von Anforderungen mit PUT, POST, oder Löschen von Verben. Wenn die Version im Header "If-Match" der Liste die aktuelle Version nicht übereinstimmt, wird die Anforderung abgelehnt werden, eine HTTP 412 – vorbedingungsfehler Rückgabecode. (optional) Kann auch für Get verwendet werden, wenn vorhanden und die übergebene Version übereinstimmt, die aktuelle Listenversion dann keine Daten und eine HTTP-304 nicht geändert-Code zurückgegeben werden. | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EAG"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Marketplace-URIs](../marketplace/atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   