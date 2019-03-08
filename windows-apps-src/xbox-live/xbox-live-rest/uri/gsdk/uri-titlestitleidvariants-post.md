---
title: POST (/titles/{titleId}/variants)
assetID: 84303448-5a11-d96f-907d-77f57f859741
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidvariants-post.html
description: " POST (/titles/{titleId}/variants)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 17974ddf7dec26abac18ccee9fda5249bc9d656f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618495"
---
# <a name="post-titlestitleidvariants"></a>POST (/titles/{titleId}/variants)
URI bezeichnet, die von einem Client, der eine Liste der game Varianten für den angegebenen Titel ID abruft. Die Domänen für diese URIs sind `gameserverds.xboxlive.com` und `gameserverms.xboxlive.com`.
 
  * [URI-Parameter](#ID4EZ)
  * [Erforderlichen Anforderungsheader](#ID4EIB)
  * [Optionale Anforderungsheader](#ID4EED)
  * [Autorisierung](#ID4E3D)
  * [Anforderungstext](#ID4EEE)
  * [Erforderlichen Antwortheader](#ID4ELF)
  * [Optionale Antwortheader](#ID4EMG)
  * [Antworttext](#ID4EEH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Beschreibung| 
| --- | --- | 
| titleid| Die ID des Titels, der die Anforderung angewendet werden soll.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Hostname

gameserverds.xboxlive.com
 
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
Wenn eine Anforderung ausführen, müssen die Header in der folgenden Tabelle gezeigt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | 
| Content-Type| Anwendung/JSON| Typ der Daten, die gesendet wird.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Die Länge des Anforderungsobjekts.| 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| Authentifizierungstoken.| 
  
<a id="ID4EED"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
Wenn eine Anforderung ausführen, sind die in der folgenden Tabelle gezeigten Header optional.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | Der MIME-Typ der Hauptteil der Anforderung.| 
  
<a id="ID4E3D"></a>

 
## <a name="authorization"></a>Autorisierung

Die Anforderung muss einen gültige Xbox Live Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst "403 Forbidden" in der Antwort. Wenn der Header ungültig ist oder nicht vorhanden ist, gibt der Dienst 401 nicht autorisiert in Antwort zurück.
 
<a id="ID4EEE"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Die Anforderung muss ein JSON-Objekt für die folgenden Elemente enthalten.
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| locale| Die lokale Varianten zurückgegeben werden soll.| 
| maxVariants| Die maximale Anzahl von Varianten, die zurückgegeben werden soll.| 
| publisherOnly|  | 
| Geräteeinschränkungen|  | 
 
<a id="ID4EDF"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
  "locale": "en-us",
  "maxVariants": "100",
  "publisherOnly": "false",
  "restriction": null
}

```

   
<a id="ID4ELF"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
Eine Antwort enthält immer die Header in der folgenden Tabelle gezeigt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| Anwendung/JSON| Typ der Daten im Antworttext.| 
| Content-Length|  | Die Länge des Antworttexts.| 
  
<a id="ID4EMG"></a>

 
## <a name="optional-response-headers"></a>Optionale Antwortheader
 
Eine Antwort möglicherweise enthalten die Header, die im folgenden dargestellt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-XblCorrelationId|  | Der MIME-Typ des Antworttexts.| 
  
<a id="ID4EEH"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst ein JSON-Objekt mit den folgenden Elementen zurück.
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Varianten| Ein Array von Varianten.| 
| variantId| Die Id einer Variante.| 
| name| Der Name einer Variante.| 
| isPublisher|  | 
| Rang|  | 
| gameVariantSchemaId|  | 
| variantSchemas| Ein Array von variant-Schemas.| 
| variantSchemaId| Die Id des Schemas.| 
| schemaContent| Der Inhalt eines Schemas| 
| name| Name des Schemas| 
| gsiSets| Ein Array von GSI legt diese fest.| 
| minRequiredPlayers| Die minimale Anzahl der Spieler für die Variante.| 
| maxAllowedPlayers| Die maximale Anzahl der Spieler für die Variante.| 
| gsiSetId| Die Id des Satzes GSI.| 
| gsiSetName| Der Name des Satzes GSI.| 
| selectionOrder|  | 
| variantSchemaId| Die ID des Schemas, die in der GSI verwendet Varaint festgelegt.| 
 
<a id="ID4EYBAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
 "variants": [
     { 
       "variantId": "8B6EF8A0-7807-42C4-9CB0-1D9B8B8CE742", 
       "name": "tankWarsV2.0",
       "isPublisher": "true",
       "rank": "1",
       "gameVariantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }],
  "variantSchemas": [
     {
        "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB",
        "schemaContent": "&lt;?xml version=\"1.0\" encoding=\"UTF-8\" ?>&lt;xs:schema xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">&lt;xs:element name=\"root\">&lt;/xs:element>&lt;/xs:schema>"
        "name": "tanksSchema"
     }],
     "gsiSets":
     [{ 
          "minRequiredPlayers": "5", 
          "maxAllowedPlayers": "10", 
          "gsiSetId": "B28047F5-B52F-477E-97C2-4C1C39E31D42",
          "gsiSetName": "TanksGSISet",
          "selectionOrder": "1",
          "variantSchemaId": "9742DBA5-23FD-4760-9D74-6CFA211B9CFB"
     }]
 }

  

```

   
<a id="ID4ERCAC"></a>

 
## <a name="see-also"></a>Siehe auch
 [/titles/{titleId}/variants](uri-titlestitleidvariants.md)

  