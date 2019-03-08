---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: dab43079dbba3729ff39f3a2116c377c3b73142a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604065"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
Der Ruf-Daten für einen Testbenutzer zurückgesetzt vollständig. Nur zu Testzwecken.

  * ["Hinweise"](#ID4EQ)
  * [URI-Parameter](#ID4E5)
  * [Autorisierung](#ID4EJB)
  * [Erforderlichen Anforderungsheader](#ID4E3B)
  * [HTTP-Statuscodes](#ID4EHC)
  * [Antworttext](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Hinweise

Aufrufen dieser API werden alle feedbackelemente und reputationsdaten von einem Benutzer entfernen. Partner können diese API für alle Sandbox mit Ausnahme von Retail aufgerufen werden. Das Team für die Erzwingung kann diese API mit jeder Sandkasten-ID aufrufen.

Die Domäne für diese URIs ist `reputation.xboxlive.com`. Dieser URI wird immer an Port 10443 aufgerufen.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Benutzers, dessen Daten gelöscht wird.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorisierung

Für den Sandkasten Retail **PartnerClaim** vom Team erzwingen.

Für alle anderen Sandboxes **PartnerClaim** und **SandboxIdClaim**.

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

**Inhaltstyp: Application/Json** und **X-Xbl-Contract-Version** (aktuelle Version ist 101).

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.|
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.|
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.|
| 500| Interner Serverfehler.| Der Server hat eine unerwartete Bedingung die Anforderung konnte.|
| 503| Dienst nicht verfügbar.| Anforderung wurde gedrosselt, und wiederholen Sie dann die Anforderung der Client-Retry-Wert in Sekunden (z. B. 5 Sekunden später).|

<a id="ID4EJF"></a>


## <a name="response-body"></a>Antworttext

None bei Erfolg; andernfalls ein [ServiceError (JSON)](../../json/json-serviceerror.md) Dokument.

<a id="ID4EWF"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EYF"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
