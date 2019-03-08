---
title: Standard-HTTP-Anforderung und Antwort-Header
assetID: a5f8fd96-9393-5234-04ad-837e5c117c92
permalink: en-us/docs/xboxlive/rest/httpstandardheaders.html
description: " Standard-HTTP-Anforderung und Antwort-Header"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ca97e82365eab40266b3ffdd84924f71289eede6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636515"
---
# <a name="standard-http-request-and-response-headers"></a>Standard-HTTP-Anforderung und Antwort-Header
 
<a id="ID4ES"></a>

 
## <a name="request-headers"></a>Anforderungsheader
 
Die folgende Tabelle enthält die standardmäßigen HTTP-Headern, die beim Herstellen von Xbox Live Services-Anforderungen verwendet.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | 
| x-xbl-contract-version| 1| API-Vertrag-Version. Für alle Anforderungen von Xbox Live Services erforderlich.| 
| Autorisierung| STSTokenString| STS-Authentifizierungstoken. Der Wert für diesen Header wird abgerufen, von der <b>GetTokenAndSignatureResult.Token</b> Eigenschaft. | 
| Content-Type| Application/Xml, Application/Json, Multipart/Form-Data oder Application/X-www-form-urlencoded| Gibt den Typ des Inhalts, die mit einer Anforderung gesendet wird.| 
| Content-Length| Ganzzahliger Wert| Gibt die Länge der Daten in eine POST-Anforderung gesendet wird.| 
| Accept-Language | Zeichenfolge| Gibt an, wie zum Lokalisieren von Zeichenfolgen zurückgegeben. Finden Sie unter <a href="https://msdn.microsoft.com/en-us/library/bb975829.aspx">Xbox 360-Programmierung erweiterter</a> für eine Liste der gültigen Sprache/Gebietsschema Kombinationen.| 
  
<a id="ID4E6C"></a>

 
## <a name="response-headers"></a>Antwortheader
 
Die folgende Tabelle enthält den HTTP-Standardheader in Xbox Live Services-Antworten verwendet.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/xml, application/json| Gibt den Typ des Inhalts zurückgegeben werden.| 
| Content-Length| Ganzzahliger Wert| Gibt die Länge der zurückgegebenen Daten.| 
  
<a id="ID4EEE"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EGE"></a>

 
##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

   