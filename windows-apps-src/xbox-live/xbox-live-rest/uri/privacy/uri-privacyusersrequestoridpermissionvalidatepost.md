---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
description: " POST (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: edd91560ffb5d81b30da4b1453612cc5853a456f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623425"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
Ruft einen Satz von Ja oder Nein Antworten zu gibt an, ob der Benutzer bestimmte Aktionen mit einem Satz von Zielbenutzern ausführen darf.

  * ["Hinweise"](#ID4EQ)
  * [URI-Parameter](#ID4ECB)
  * [Autorisierung](#ID4ENB)
  * [Erforderlichen Anforderungsheader](#ID4ESC)
  * [Anforderungstext](#ID4E4D)
  * [HTTP-Statuscodes](#ID4ETE)
  * [Erforderlichen Antwortheader](#ID4EIG)
  * [Antworttext](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Hinweise

Der Anforderungstext nimmt eine Liste von Benutzern und eine Liste der Einstellungen, und das Ergebnis ist ein Ergebnis zugelassenen/blockierten für jedes Paar aus Benutzer-Einstellung.

In netzwerkübergreifende Multiplayer-Szenarien (wo Datenschutz Kommunikation muss ausgeführt werden zwischen Benutzern, die ein Xbox-Benutzer-ID (XUID) verfügen und Benutzer von außerhalb des Netzwerks, das nicht der Fall ist), finden Sie in [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md) für Typen.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| requestorId| string| Erforderlich. Der Bezeichner des Benutzers, die Aktion ausführt. Die möglichen Werte sind <code>xuid({xuid})</code> und <code>me</code>. Dies muss ein Benutzer angemeldet sein. Beispielwert: <code>xuid(0987654321)</code>.|

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorisierung

Autorisierungsansprüche verwendet | Anspruch| Typ| Erforderlich?| Beispielwert|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64-Bit-Ganzzahl mit Vorzeichen| Ja| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispielwert: 1.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>Anforderungstext

<a id="ID4EDE"></a>


### <a name="required-members"></a>Erforderliche Member

Finden Sie unter [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md).


```cpp
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ViewTargetGameHistory",
        "ViewTargetProfile"
    ]
}

```


<a id="ID4ETE"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 400| Die Anforderung ist ungültig.| Beispiele: IDs der falschen Einstellung, falschen URIs usw.|
| 404| Der im URI angegebene Benutzer ist nicht vorhanden.| Die angegebene Ressource konnte nicht gefunden werden.|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| Der MIME-Typ der Hauptteil der Anforderung. Beispielwert: <code>application/json</code>|
| Content-Length| string| Die Anzahl der Bytes, die in der Antwort gesendet werden. Beispielwert: 34|
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung vom Server zum Verhalten beim Zwischenspeichern angeben. Beispiel: <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>Antworttext

Finden Sie unter [PermissionCheckBatchResponse (JSON)](../../json/json-permissioncheckbatchresponse.md).

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRest", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}

```


<a id="ID4EVAAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EXAAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId-Enumeration](../../enums/privacy-enum-permissionid.md)
