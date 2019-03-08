---
title: VerifyStringResult (JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
description: " VerifyStringResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b01793222be80efccdca1f24f5226a2e9ff78064
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599485"
---
# <a name="verifystringresult-json"></a>VerifyStringResult (JSON)
Führen Sie Codes für jede Zeichenfolge, die an gesendet [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md).
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

VerifyStringResult Objekt verfügt über den folgenden Spezifikationen.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| resultCode| 32-Bit-Ganzzahl ohne Vorzeichen| Erforderlich. HResult Code entspricht übermittelt Zeichenfolge.|
| offendingString| string| Erforderlich. String-Wert, der eine Zeichenfolge, die zurückgewiesen werden verursacht hat.|

<a id="ID4EXB"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
{
    "verifyStringResult":
    [
        {"resultCode": "1", "offendingString": "badword"},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
    ]
}

```


#### <a name="common-hresult-values"></a>Allgemeine HResult-Werte

| Wert| Fehlerbezeichnung|
| --- | --- | --- | --- | --- |
| 0| Möglich|
| 1| Anstößige Zeichenfolge|
| 2| Zeichenfolge ist zu lang|
| 3| Unbekannter Fehler|

<a id="ID4ELD"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4END"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Verweis

[POST (/ System/Zeichenfolgen/validate)](../uri/stringserver/uri-systemstringsvalidatepost.md)
