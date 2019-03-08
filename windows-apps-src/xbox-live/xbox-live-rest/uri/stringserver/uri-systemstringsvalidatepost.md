---
title: POST (/system/strings/validate)
assetID: 6a59bc0b-8edd-87bf-efaf-f16efa3bedf7
permalink: en-us/docs/xboxlive/rest/uri-systemstringsvalidatepost.html
description: " POST (/system/strings/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 70e86567f449674c7a046e072437d9ee715dc6d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595505"
---
# <a name="post-systemstringsvalidate"></a>POST (/system/strings/validate)
Akzeptiert ein Array von Zeichenfolgen für die Überprüfung aus, und gibt ein Array von Ergebnissen gleicher Größe. Die Domäne für diese URIs ist `client-strings.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [Erforderlichen Anforderungsheader](#ID4EIB)
  * [Anforderungstext](#ID4ELC)
  * [HTTP-Statuscodes](#ID4E4C)
  * [Antworttext](#ID4ETF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Jedes Ergebnis gibt an, ob die entsprechende Zeichenfolge auf Xbox LIVE akzeptabel ist, und die betreffende Zeichenfolge enthält, sofern zutreffend.
 
Identische Zeichenfolgen erhalten immer dieselben Ergebnisse. Wenn Sie ein nicht erfolgreiche Ergebnis angezeigt wird, wird analysieren Sie das Ergebnis, und ändern Sie die Zeichenfolge entsprechend.
 
 

> [!NOTE] 
> Die resultierende <b>VerifyStringResult</b> wird nur das erste problematische Wort in der Zeichenfolge. Es gibt möglicherweise weitere problematische Wörter in der Zeichenfolge. Wenn Sie beabsichtigen, ersetzen Sie die problematische Wörter, damit die Zeichenfolge verwendet werden kann, Sie die problematische Wort oder eine Teilzeichenfolge ersetzen, und klicken Sie dann erneut überprüfen, ob die Zeichenfolge, die zusätzliche problematische Teilzeichenfolgen gesucht.  

 
  
<a id="ID4EIB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | 
| Autorisierung| Authentifizierungstoken. Beispiel: XBL3.0 x = [Hash]; [Token].| 
| x-xbl-contract-version| Integer-API-Vertrag-Version. 1 oder 2 muss für diese API sein.| 
  
<a id="ID4ELC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Der Anforderungstext ist ein Array von Zeichenfolgen, wobei keine Beschränkungen hinsichtlich der Größe des Arrays und 512 Zeichen pro Zeichenfolge.
 
<a id="ID4ETC"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
    "stringstoVerify":
    [
        "Poppycock",
        "The quick brown fox jumped over the lazy dog.",
        "Hey, want to hang out?",
        "oh noes"
    ]
}
      
```

   
<a id="ID4E4C"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| 200| OK| Alle Zeichenfolgen wurden erfolgreich verarbeitet. Dies bedeutet nicht unbedingt, dass alle Zeichenfolgen positive HResults hatten.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 406| Nicht akzeptabel| Fehlende <b>Content-Type: Application/Json</b> Header.| 
| 408| Anforderungstimeout| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
  
<a id="ID4ETF"></a>

 
## <a name="response-body"></a>Antworttext
 
Gibt ein Array von [VerifyStringResult (JSON)](../../json/json-verifystringresult.md), der die gleiche Größe wie das Array der Anforderung.
  
<a id="ID4EAG"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ECG"></a>

 
##### <a name="parent"></a>Parent 

[/system/strings/validate](uri-systemstringsvalidate.md)

   