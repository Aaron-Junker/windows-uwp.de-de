---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
description: " GET (/users/{ownerId}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7c52daf4a07914c34f1aadc84a7238771669d65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623205"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
Rufen Sie die Liste der Benutzer des Clips.
Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.

  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EEB)
  * [Abfragezeichenfolgen-Parameter](#ID4EPB)
  * [Anforderungstext](#ID4EPE)
  * [Erforderlichen Antwortheader](#ID4E1E)
  * [Optionale Antwortheader](#ID4ENH)
  * [Antworttext](#ID4EOAAC)
  * [Verwandte URIs](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Hinweise

Mit dieser API können verschiedene Möglichkeiten zur Liste eines Benutzers eigenen Clips für sowie andere Benutzer schneidet, die im Dienst gespeichert werden. Mehrere Einstiegspunkte Zurückgeben von Daten aus verschiedenen Ebenen und Filtern über Abfrageparameter ermöglichen. Wenn die XUID im Anspruch, den im URI angegebenen Besitzer übereinstimmt, werden die Benutzers eigenen Clips zurückgegeben, nachdem inhaltsisolation überprüft. Wenn der Besitzer im URI nicht den Anspruch XUID übereinstimmt, werden des angegebenen Benutzers Clips basierend auf den Datenschutz-Überprüfungen und Überprüfungen im Hinblick auf die anfordernde XUID inhaltsisolation zurückgegeben.

Abfragen werden pro Benutzer pro Dienst-Konfigurations-Id (scid) optimiert. Angeben weiter filtern oder Sortierreihenfolgen als die Standards wie unten beschrieben in einigen Fällen zurückzugebenden länger dauert. Dies ist offensichtlich für größere Gruppen von Videos pro Benutzer.

Es gibt keine Batch-API zum Abrufen der Liste von mehreren Benutzern innerhalb der gleichen API-Aufrufs. Das empfohlene Muster (derzeit) aus den SLS Architekten ist wiederum für jeden Benutzer Abfragen.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| ownerId| string| Die Benutzeridentität des Benutzers, dessen Ressource zugegriffen wird. Unterstützte Formate: "me" oder "xuid(123456789)". Maximale Länge: 16.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| skipItems| 32-Bit-Ganzzahl mit Vorzeichen| Optional. Gibt die Elemente, beginnend mit N + 1 in der Auflistung (d. h. Skip N Elemente) zurück.|
| continuationToken| string| Optional. Gibt die Elemente, die beginnend ab des angegebenen Fortsetzungstokens zurück. Der ContinuationToken-Parameter hat Vorrang vor SkipItems Wenn beides angegeben werden. Das heißt, wird der SkipItems-Parameter ignoriert, wenn ContinuationToken-Parameter vorhanden ist. Maximale Größe: 36.|
| "MaxItems"| 32-Bit-Ganzzahl mit Vorzeichen| Optional. Maximale Anzahl von Elementen, die aus der Auflistung zurückgegeben (kann mit SkipItems und "continuationtoken", um einen Bereich von Elementen zurückzugeben kombiniert werden). Der Dienst kann einen Standardwert bereitstellen, wenn "MaxItems" nicht vorhanden ist und weniger als "MaxItems" zurückgeben kann, (selbst wenn die letzte Seite der Ergebnisse noch nicht zurückgegeben).|
| Reihenfolge| Unicode-Zeichen| Optional. Gibt an, ob die Liste in (D) Escending zurückgegeben wird (höchste Wert an erster Stelle) oder (A) sortieren (niedrigste Wert an erster Stelle) Reihenfolge. Standardwert: D.|
| type| GameClipTypes| Optional. Durch Trennzeichen getrennten Liste des Typs des Clips zurückgegeben. Standardwert: Alle.|
| eventId| string| Optional. Durch Trennzeichen getrennten Liste von EventIDs zum Filtern von Ergebnissen durch. Standardwert: NULL ist.|
| qualifer| string| Optional. Gibt an, der Order-Qualifizierer, der zum Abrufen von Clips verwendet werden. <ul><li>erstellt – gibt an, die Clips in der Reihenfolge des Datums in das System zurückgegeben werden</li><li>Rating - [erstklassiger Bewertung]: Gibt an, dass die Clips, die von ihren Rating-Wert zurückgegeben werden</li><li>Ansichten - [am beliebtesten] – Gibt an, dass die Clips von der Anzahl der Aufrufe zurückgegeben werden</li></ul><br/> Maximale Größe: 12. Standardwert: "erstellt".| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>Anforderungstext

Es gibt keine erforderliche Member für diese Anforderung.

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.|
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.|
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.|
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.|
| Retry-After| string| Weist der Client im Fall einer nicht verfügbaren Server es später erneut versuchen.|
| Variieren| string| Weist downstream-Proxys, wie zum Zwischenspeichern von Antworten.|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>Optionale Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| string| Für Cache-Optimierung verwendet. Beispiel: "686897696a7c876b7e".|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
       "gameClips":
       [
           {
               "xuid": "2716903703773872",
               "clipName": "[en-US] Localized Greatest Moment Name",
               "titleName": "[en-US] Localized Title Name",
               "gameClipLocale": "en-US",
               "gameClipId": "6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
               "state": "Published",
               "dateRecorded": "2013-06-14T01:02:55.4918465Z",
               "lastModified": "2013-06-14T01:05:41.3652693Z",
               "userCaption": "Set by user!",
               "type": "DeveloperInitiated",
               "source": "Console",
               "visibility": "Public",
               "durationInSeconds": 30,
               "scid": "00000000-0000-0012-0023-000000000070",
               "titleId": 354975,
               "rating": 3.75,
               "ratingCount": 245,
               "views": 7453,
               "titleData": "",
               "systemProperties": "",
               "savedByUser": false,
               "achievementId": "AchievementId",
               "greatestMomentId": "GreatestMomentId",
               "thumbnails": [
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/large",
                       "fileSize": 637293,
                       "thumbnailType": "Large"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/small",
                       "fileSize": 163998,
                       "thumbnailType": "Small"
                   }
               ],
               "gameClipUris": [
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest",
                       "uriType": "SmoothStreaming",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest(format=m3u8-aapl)",
                       "uriType": "Ahls",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
                       "fileSize": 88820,
                       "uriType": "Download",
                       "expiration": "2999-12-31T11:59:40Z"
                   }
               ]
           }
       ],
   "pagingInfo":
       {
           "continuationToken": null,
           "totalItems": 1
       }
   }

```


<a id="ID4EABAC"></a>


## <a name="related-uris"></a>Verwandte URIs

Der folgende URI ist identisch mit dem primären in diesem Dokument, jedoch mit einer zusätzlichen Pfad-Parameter, um eine SCID anzugeben. Nur dieses Benutzers Clips für diese SCID werden zurückgegeben. Der anfordernde Benutzer Zugriff auf die angeforderte SCID benötigen, werden andernfalls HTTP 403-Fehler zurückgegeben.

   * **/users/{ownerId}/scids/{scid}/clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/clips](uri-usersowneridclips.md)
