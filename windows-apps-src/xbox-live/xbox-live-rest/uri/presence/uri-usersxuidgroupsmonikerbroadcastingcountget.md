---
title: GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )
assetID: 2b2df42e-9b39-3906-36e4-fd9ff22add04
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcountget.html
description: " GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 219942838902381d7c9be91287056c422ea7957e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616815"
---
# <a name="get-usersxuidxuidgroupsmonikerbroadcastingcount-"></a>GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )
Ruft die Anzahl der Gruppen-Moniker im Zusammenhang mit der XUID, das in der URI wird angegebenen Broadcasting Benutzer ab. Die Domäne für diese URIs ist `userpresence.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4E5)
  * [Abfragezeichenfolgen-Parameter](#ID4EJB)
  * [Autorisierung](#ID4EKC)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EQD)
  * [Erforderlichen Anforderungsheader](#ID4EEH)
  * [Optionale Anforderungsheader](#ID4EMBAC)
  * [Anforderungstext](#ID4EMCAC)
  * [HTTP-Statuscodes](#ID4EXCAC)
  * [Erforderlichen Antwortheader](#ID4E3GAC)
  * [Optionale Antwortheader](#ID4EMJAC)
  * [Antworttext](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Ruft die Anzahl der Gruppen-Moniker im Zusammenhang mit der XUID, das in der URI wird angegebenen Broadcasting Benutzer ab.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Xbox User ID (XUID) des Benutzers im Zusammenhang mit der XUIDs in der Gruppe.| 
| moniker| string| Zeichenfolge, die die Gruppe von Benutzern zu definieren. Der einzige akzeptierte Moniker derzeit ist die "People", mit dem Großbuchstaben "P".| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| level| string| Gibt die Detailebene gemäß dieser Abfragezeichenfolge zurück. Gültige Optionen: "User", "Device", "Title" und "all". Der "User" ist das PresenceRecord-Objekt, ohne die DeviceRecord geschachtelten-Objekts. Die Ebene "Gerät" ist die PresenceRecord und DeviceRecord-Objekte, ohne die TitleRecord geschachtelten-Objekts. Die Ebene "Title" ist die PresenceRecord DeviceRecord und TitleRecord Objekte ohne die ActivityRecord geschachtelten-Objekts. Die Ebene "all" ist der gesamte Datensatz, einschließlich aller geschachtelten Objekte. Wenn dieser Parameter nicht angegeben wird, der Dienst standardmäßig der Title-Ebene (d. h. gibt Anwesenheit für diesen Benutzer zu den Details des Titels).| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>Autorisierung
 
Autorisierungsansprüche verwendet | Anspruch| Typ| Erforderlich?| Beispielwert| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>xuid</b>| 64-Bit-Ganzzahl mit Vorzeichen| Ja| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource
 
Der Dienst gibt stets gut 200 zurück, wenn die Anforderung selbst wohlgeformt ist. Allerdings herausgefiltert es Informationen aus der Antwort beim Datenschutz Überprüfungen nicht erfolgreich ist.
 
Auswirkungen der Datenschutzeinstellungen für Ressource | Anfordernden Benutzer| Ziel der Datenschutzeinstellungen des Benutzers| Verhalten| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Ich| -| Wie beschrieben.| 
| Friend| Alle Benutzer| Wie beschrieben.| 
| Friend| Nur Freunde| Wie beschrieben.| 
| Friend| gesperrt| Siehe - Filtern Daten des Diensts.| 
| nicht-Friend-Benutzer| Alle Benutzer| Wie beschrieben.| 
| nicht-Friend-Benutzer| Nur Freunde| Siehe - Filtern Daten des Diensts.| 
| nicht-Friend-Benutzer| gesperrt| Siehe - Filtern Daten des Diensts.| 
| Drittanbieter-Website| Alle Benutzer| Siehe - Filtern Daten des Diensts.| 
| Drittanbieter-Website| Nur Freunde| Siehe - Filtern Daten des Diensts.| 
| Drittanbieter-Website| gesperrt| Siehe - Filtern Daten des Diensts.| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Beispielwerte: 3, Vnext.| 
| Annehmen| string| Inhaltstypen, die zulässig sind. Ist der einzige Anwesenheit unterstützt <b>Application/Json</b>, aber dennoch muss angegeben werden in der Kopfzeile.| 
| Accept-Language| string| Zulässige Gebietsschema für Zeichenfolgen in der Antwort. Beispielwert: En-US.| 
| Host| string| Der Domänenname des Servers. Beispielwert: userpresence.xboxlive.com.| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 405| Methode, die nicht zulässig| HTTP-Methode wurde für einen nicht unterstützten Inhaltstyp verwendet.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 500| Anforderungstimeout| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 503| Anforderungstimeout| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Beispielwerte: 1, Vnext.| 
| Content-Type| string| Der MIME-Typ der Hauptteil der Anforderung. Beispielwert: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben. Beispielwerte: "No-Cache".| 
| X-XblCorrelationId| string| Dienst generierter Wert korreliert werden, was auf der Server zurückgibt und was vom Client empfangen wird. Beispielwert: "4106d0bc-1cb3-47bd-83fd-57d041c6febe".| 
| X-Content-Type-Option| string| Gibt den SDL-kompatiblen Wert. Beispielwert: "Nosniff".| 
| Datum| string| Datum/Uhrzeit, an die Nachricht gesendet wurde. Beispielwert: "Tue, 17 November 2012 10:33:31 GMT".| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>Optionale Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Retry-After| string| Bei HTTP-Fehler "503" zurückgegeben. Ermöglicht Clients, die wissen, wie lange warten soll, bevor der Aufruf wiederholt wird. Beispielwerte: "120".| 
| Content-Length| string| Länge des Antworttexts. Beispielwert: "527".| 
| Content-Encoding| string| Typ der Codierung der Antwort. Beispielwert: "Gzip".| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Diese API gibt die Anzahl der aktuellen Anzahl von Übertragungen anhand der Parameter zurück.
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
    "count":42
 }

         
```

   
<a id="ID4EQLAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ESLAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

   