---
title: Standard-HTTP-Statuscodes
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
description: " Standard-HTTP-Statuscodes"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c77972e88dbf4950f716594ee61220d1734a7fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635555"
---
# <a name="standard-http-status-codes"></a>Standard-HTTP-Statuscodes
 
Der Transport HTTP (Hypertext Protocol) standard beschreibt eine Anzahl von Statuscodes, die vom Server als Antwort auf eine Clientanforderung zurückgegeben werden. Xbox Live Services-Methoden geben die HTTP-Protokoll kompatibel Statuscodes zum Beschreiben der Status der Anforderung zurück.
 
Hier ist eine Liste von Statuscodes, die von Xbox Live-Dienste und ihre typische Bedeutung zurückgegeben.
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Xbox Live Services-Statuscodes
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | 
| 200| OK| Die Anforderung war erfolgreich.| 
| 201| Erstellt| Die Entität erstellt wurde.| 
| 202| Akzeptiert| Die Anforderung wurde akzeptiert, jedoch wurde noch nicht abgeschlossen.| 
| 204| Kein Inhalt| Die Anforderung abgeschlossen wurde, jedoch keine Inhalte zurückgegeben.| 
| 301| Permanent verschoben| Der Dienst wurde auf einen anderen URI verschoben.| 
| 302| Gefunden.| Die angeforderte Ressource befindet sich temporär unter einem anderen URI.| 
| 307| Temporäre Umleitung| Der URI für diese Ressource wurde vorübergehend geändert werden.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout| Die Anforderung dauerte zu lange dauert.| 
| 407| Konflikt| Die Anforderung wurde aufgrund eines Konflikts mit dem aktuellen Zustand der Ressource nicht abgeschlossen werden.| 
| 410| Aufgetreten| Die angeforderte Ressource ist nicht mehr verfügbar.| 
| 412| Vorbedingung nicht erfüllt| Der Server erfüllt eine Vorbedingungen nicht, dass die anfordernde Person für die Anforderung eingefügt.| 
| 416| Angeforderter Bereich nicht erfüllbar| Der angeforderte Bereich ist nicht verfügbar.| 
| 500| Interner Serverfehler.| Der Server hat eine unerwartete Bedingung die Anforderung konnte.| 
| 501| Nicht implementiert.| Der Server unterstützt nicht die zur Verarbeitung der Anforderung erforderliche Funktionalität.| 
| 503| Dienst nicht verfügbar.| Anforderung wurde gedrosselt, und wiederholen Sie dann die Anforderung der Client-Retry-Wert in Sekunden (z. B. 5 Sekunden später).| 
 

> [!NOTE] 
> Einige Ressourcen und die Methoden geben bestimmte Informationen über die Bedeutung der bestimmte Statuscodes im Kontext der Ressource oder -Methode. Weitere Informationen finden Sie in der Dokumentation für die Ressourcen oder die Methoden, die Sie verwenden. 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>Verweis [Universal Resource Identifier (URI)-Referenz](../uri/atoc-xboxlivews-reference-uris.md)

 [Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpswwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>Externe Links [W3.org: HTTP/1.1-Statuscodedefinitionen](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   