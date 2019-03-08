---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
description: " POST (/titles/{titleId}/clusters)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 91d7c49628914f887c5d2243942e10e47d47b095
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608905"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
URI, der einen Client eine Xbox Live Compute-Server-Instanz erstellen kann. Die Domäne für diese URIs ist `gameserverms.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Erforderlichen Anforderungsheader](#ID4EGB)
  * [Autorisierung](#ID4ELD)
  * [Anforderungstext](#ID4EWD)
  * [Erforderlichen Antwortheader](#ID4EZE)
  * [Antworttext](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Beschreibung| 
| --- | --- | 
| titleId| Die ID des Titels, der die Anforderung angewendet werden soll.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Hostname

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
Wenn eine Anforderung ausführen, müssen die Header in der folgenden Tabelle gezeigt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | 
| Benutzer-Agent|  | Informationen über den Benutzer-Agent, der die Anforderung.| 
| Content-Type| Anwendung/JSON| Typ der Daten, die gesendet wird.| 
| Host| gameserverms.xboxlive.com|  | 
| Content-Length|  | Die Länge des Anforderungsobjekts.| 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| Authentifizierungstoken.| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>Autorisierung
 
Die Anforderung muss einen gültige Xbox Live Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst "403 Forbidden" in der Antwort. Wenn der Header ungültig ist oder nicht vorhanden ist, gibt der Dienst 401 nicht autorisiert in Antwort zurück.
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Die Anforderung muss ein JSON-Objekt für die folgenden Elemente enthalten.
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| Sitzungs-ID aus der MPSD.| 
| abortIfQueued| Optionaler Parameter, die bei Festlegung auf "true" teilt GSMS nicht für diese Sitzung für eine Ressource in die Warteschlange, wenn er nicht sofort erfüllt werden kann. Wenn die Anforderung abgebrochen wird, da dieser Wert true ist, enthält das Antwortobjekt <code>"fulfillmentState" : "Aborted"</code>. | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
Eine Antwort enthält immer die Header in der folgenden Tabelle gezeigt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Cache-Control|  | Anweisungen, die von allen Zwischenspeicherungsmechanismen in der Anforderungs-/antwortkette antwortkette befolgt werden müssen.| 
| Content-Type| Anwendung/JSON| Typ der Daten in der Antwort.| 
| Content-Length|  | Die Länge des Antworttexts.| 
| X-Content-Type-Options|  |  | 
| X-XblCorrelationId|  | Der MIME-Typ des Antworttexts.| 
| Datum|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst ein JSON-Objekt mit den folgenden Elementen zurück.
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| Das empfohlene Intervall in Millisekunden für den Abruf von Abschluss. Beachten Sie, dass dies keine Schätzung der Wenn der Cluster bereit ist, aber stattdessen eine Empfehlung für die Häufigkeit der Aufrufer für eine statusaktualisierung erhält den aktuellen Pool von Abonnements und die Anforderung und Fulfillment-Preise abrufen soll.| 
| fulfillmentState| Gibt an, ob die angegebene Sitzung sofort eine Ressource zugewiesen wurde, "Erfüllt", in die Warteschlange für die Verfügbarkeit einer zukünftigen Ressource hinzugefügt "in der Warteschlange", oder wurde abgebrochen, "Abgebrochen", aufgrund der Unfähigkeit zur Verarbeitung der Anforderung sofort, wenn die Anforderung angegebene AbortIfQueued als "True". | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Hinweise
 
Ein Titel sollte nur wiederholt den Aufruf an den Dienst, wenn die folgenden Antwortcodes empfangen werden:
 
   * 408-Servertimeout
   * 429 – zu viele Anforderungen
   * 500 – Serverfehler
   * 502 – Fehlerhaftes Gateway
   * 503 – Dienst nicht verfügbar.
   * 504 – GatewayTimeout
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>Siehe auch
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  