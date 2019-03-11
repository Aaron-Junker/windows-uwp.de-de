---
title: GET (/users/{requestorId}/permission/validate)
assetID: 8d22c668-af9a-1d24-8d65-830c2ce913d7
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidateget.html
description: " GET (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 07ccac63b3e6681ea35405314b0cd8b93aa60f9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594755"
---
# <a name="get-usersrequestoridpermissionvalidate"></a>GET (/users/{requestorId}/permission/validate)
Ruft ab eine Ja / Nein Antwort zu gibt an, ob der Benutzer die angegebene Aktion mit einem Zielbenutzer ausführen darf.

  * [URI-Parameter](#ID4EQ)
  * [Abfragezeichenfolgen-Parameter](#ID4E2)
  * [Autorisierung](#ID4EDC)
  * [Erforderlichen Anforderungsheader](#ID4EID)
  * [Anforderungstext](#ID4ETE)
  * [HTTP-Statuscodes](#ID4E5E)
  * [Erforderlichen Antwortheader](#ID4ETG)
  * [Antworttext](#ID4EKAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| requestorId| string| Erforderlich. Der Bezeichner des Benutzers, die Aktion ausführt. Die möglichen Werte sind <code>xuid({xuid})</code> und <code>me</code>. Dies muss ein Benutzer angemeldet sein. Beispielwert: <code>xuid(0987654321)</code>.|

<a id="ID4E2"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| Einstellung| zeichenfolgenenumeration| Der PermissionId-Wert, auf die geprüft werden soll. Beispielwert: "CommunicateUsingText".|
| target| string| Bezeichner des Benutzers, auf denen die Aktion ist, ausgeführt werden. Die möglichen Werte sind <code>xuid({xuid})</code>. Beispielwerte: <code>xuid(0987654321)</code>|

<a id="ID4EDC"></a>


## <a name="authorization"></a>Autorisierung

Autorisierungsansprüche verwendet | Anspruch| Typ| Erforderlich?| Beispielwert|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| xuid| 64-Bit-Ganzzahl mit Vorzeichen| Ja| 1234567890|

<a id="ID4EID"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispielwert: 1.|

<a id="ID4ETE"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 400| Die Anforderung ist ungültig.| Beispiele: IDs der falschen Einstellung, falschen URIs usw.|
| 404| Der im URI angegebene Benutzer ist nicht vorhanden.| Die angegebene Ressource konnte nicht gefunden werden.|

<a id="ID4ETG"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| Der MIME-Typ der Hauptteil der Anforderung. Beispielwert: <code>application/json</code>|
| Content-Length| string| Die Anzahl der Bytes, die in der Antwort gesendet werden. Beispielwert: 34|
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung vom Server zum Verhalten beim Zwischenspeichern angeben. Beispiel: <code>no-cache, no-store</code>|

<a id="ID4EKAAC"></a>


## <a name="response-body"></a>Antworttext

Finden Sie unter [PermissionCheckResponse (JSON)](../../json/json-permissioncheckresponse.md).

<a id="ID4EWAAC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}

```


<a id="ID4EABAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ECBAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId-Enumeration](../../enums/privacy-enum-permissionid.md)
