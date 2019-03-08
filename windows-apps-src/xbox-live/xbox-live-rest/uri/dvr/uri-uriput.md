---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
description: " PUT (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 862b956e29222bb9d28f98510f13d42fd1a51b6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633115"
---
# <a name="put-uri"></a>PUT (/{uri})
Hochladen Sie Spiele Clipdaten.
Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.

  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EQB)
  * [Abfragezeichenfolgen-Parameter](#ID4ERC)
  * [Erforderlichen Anforderungsheader](#ID4EBE)
  * [Optionale Anforderungsheader](#ID4ENG)
  * [Anforderungstext](#ID4EWH)
  * [HTTP-Statuscodes](#ID4ECAAC)
  * [Erforderlichen Antwortheader](#ID4EYEAC)
  * [Optionale Antwortheader](#ID4ELHAC)
  * [Antworttext](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Hinweise

Nach der **InitialUploadResponse** zurückgegeben wird, wird der Upload erfolgt über die **UploadUri** in diesem Objekt zurückgegeben. Der Client sollte die Datei in Teilen **ExpectedBlocks** sequenzielle Blöcke, die nicht größer als 2 MB. Sie können parallel hochgeladen werden.

Wenn Sie die Datei in Blöcken hochladen, wird der Server HTTP-Statuscode akzeptiert (202) bei jeder Anforderung zurück, bis alle erwartete-Blöcken in empfangenen dem Fall führt einen Commit für alle Blöcke als eine Datei erstellt (201) zurückgeben. In diesen Fällen ist die Antwort enthält ein Objekt nicht, und der Server möglicherweise zusätzliche Verarbeitung planen. Bei einem Fehler eine **ServiceErrorResponse** Objekt zusammen mit den entsprechenden HTTP-Statuscode zurückgegeben werden kann.

Für ein behebbarer Fehlercode verwenden sollte der Client mit einem Wiederholungsmechanismus für standard Backoff-wiederholen.

> [!NOTE] 
> Auch wenn ein Upload erfolgreich abgeschlossen wurde, treten weitere Verarbeitung, kann ablehnen, die der Clip Gründen nicht das hoch- oder Metadaten zu ergänzen Prozess.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| <b>uri</b>| string| Die <b>UploadUri</b> Feld innerhalb der <b>InitialUploadResponse</b> Objekt.|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Erforderlich, wenn <b>ExpectedBlocks</b> festgelegt ist. 0 (null) indiziert Blocknummer bestimmt die Reihenfolge des Blocks in der Datei. Z. B. wenn <b>ExpectedBlocks</b> ist 7, klicken Sie dann <b>BlockNum</b> können Werte von 0 bis 6. |
| <b>uploadId</b>| string| Erforderlich. Nicht transparenter ID in <b>GameClipsServiceUploadResponse</b> Objekt.|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <b>Xauth=&lt;authtoken></b>|
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.|
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.|
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.|
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| string| Zulässige komprimierungscodierungen. Beispielwerte: Gzip, deflate, Identität.|
| ETag| string| Für Cache-Optimierung verwendet. Beispielwert: "686897696a7c876b7e".|

<a id="ID4EWH"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 301| Permanent verschoben| Der Dienst wurde auf einen anderen URI verschoben.|
| 307| Temporäre Umleitung| Der Dienst wurde auf einen anderen URI verschoben.|
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.|
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.|
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.|
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.|
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.|
| 408| Anforderungstimeout| Die Anforderung dauerte zu lange dauert.|
| 410| Aufgetreten| Die angeforderte Ressource ist nicht mehr verfügbar.|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.|
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.|
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.|
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.|
| Retry-After| string| Weist der Client im Fall einer nicht verfügbaren Server es später erneut versuchen.|
| Variieren| string| Weist downstream-Proxys, wie zum Zwischenspeichern von Antworten.|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>Optionale Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| string| Für Cache-Optimierung verwendet. Beispiel: "686897696a7c876b7e".|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>Antworttext

Es werden keine Objekte im Text der Antwort gesendet.

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>Parent

[/{uri}](uri-uri.md)
