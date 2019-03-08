---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
description: " GET (/users/me/inventory)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 31c787108fad84f06b02ded3958f9d2f99727cbe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632205"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
Enthält den Satz von Inventur, die derzeit mit dem angegebenen Benutzer zurück an den Aufrufer zugeordnet sind.
Die Domäne für diese URIs ist `inventory.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [Abfragezeichenfolgen-Parameter](#ID4EHB)
  * [Beispiel für eine Anforderung](#ID4EDE)
  * [Antworttext](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Keine Richtlinie eincheckt, Erzwingung oder Filterung erfolgt als Teil dieses Aufrufs. Aufrufer können Abfrageparameter zu übergeben, um die zurückgegebenen Ergebnisse einzugrenzen.

Aufrufer können über die Ergebnisse mit einem Fortsetzungstoken Seite, wie durch eine vorherige Antwort bereitgestellt: **/users/me/inventory? ContinuationToken = ContinuationTokenString**.

Der Aufrufer kann einen Aufruf in den Details-API mit einem bestimmten Element-URL, um Informationen zu einem bestimmten Element finden Sie unter machen.

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| Verfügbarkeit| string| Die aktuelle Verfügbarkeit der zurückzugebenden Elemente. Der Standardwert ist "Verfügbar", welche gibt-Elemente, die für die das aktuelle Datum zwischen dem Startdatum und dem Ende fällt Bereich Datum. Andere Werte sind "All", gibt alle Elemente aus, und "Nicht verfügbar" die Elemente zurückgibt, für die das aktuelle Datum liegt außerhalb der Datumsbereich für den Start Startdatum und Enddatum, und es daher derzeit nicht verfügbar. |
| Container| string| Optional. Wenn Sie den Wert auf der Produkt-ID für ein Spiel festlegen, enthalten die Ergebnisse aus dem Inventar nur Elemente, die im Zusammenhang mit diesem Spiel. Dies ist besonders nützlich, beim Aufrufen der Inventurzyklus von Ihrem Server zum Filtern der Ergebnisse bis hinab zu einem bestimmten Spiel-Produkte.|
| expandSatisfyingEntitlements| string| Ein Flag, das angibt, ob die Antwort auf alle zufriedenstellendes Berechtigungen enthält, die der Benutzer in den Ergebnissen zurückgegeben wurden. Der Standardwert ist "false". Wenn dieser Parameter verwendet wird, mit dem Wert "True", die alle Produkte, die gewährt werden, für dem Benutzer über erfüllen Elemente der Berechtigungen, z. B. gebündelt, Xbox 360-Käufe in Deiner Xbox One,-Abonnementvorteilen, migriert werden, werden mit den Ergebnissen usw. hinzugefügt. Wenn dieser Wert auf "false" festgelegt ist, werden nur die übergeordneten Elemente wie z. B. des Bündels "ProductID" in den Ergebnissen, und nicht die einzelnen enthaltenen Elemente zurückgegeben. **Hinweis:** Mithilfe dieses Parameters mit dem Wert "True" wird nur unterstützt, wenn der ItemType-Parameter nicht im URI enthalten ist, andernfalls tritt einen HTTP 400-Fehler. |  
  | productIds | string |  Eine Auflistung von Produkt-IDs, die speziell aus dem Lager des Benutzers abgerufen werden sollen, getrennt durch ",".  Wenn der Benutzer nicht über eine angegebene Produkt-ID in die Ergebnisse der Anwendungsinventur verfügt, wird dieses Element aus der API-Aufruf nicht in den Ergebnissen angezeigt. Wenn Sie die "ProductID" eines Pakets sowie die ExpandSatisfyingEntitlements Parameter auf "true" übergeben, werden alle Elemente, die im Paket enthaltenen in die Ergebnisse des Funktionsaufrufs zurückgegeben (ob Sie die Produkt-IDs in der Abfragezeichenfolge oder nicht angegeben).   |
  | Status | string | Der Status der Elemente, die zurückgegeben werden soll. Der Standardwert ist "all", die alle Elemente zurückgibt. Andere Werte sind "aktiviert", gibt an, nur Itemsthat aktiviert zurückgegeben werden soll, "Angehalten", was bedeutet, dass nur Elemente, die angehalten wurden zurückgegeben werden soll, "Abgelaufen", der angibt, dass ausschließlich Elemente, die abgelaufen sind, die zurückgegeben werden soll, " "Wird abgebrochen, der angibt, dass nur Elemente, die abgebrochen werden zurückgegeben werden sollen, und"Renewed", der gibt an, dass nur Elemente, die erneuert wurden zurückgegeben werden sollen.  |

Zusätzlich zu diesen unterstützt die Ressource für die standard-Paging-Mechanismen.

<a id="ID4EDE"></a>


## <a name="sample-request"></a>Beispiel für eine Anforderung

Wird von der vollständig qualifizierten Domänennamen für diese URI-Methode `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> Welche Benutzer gelten hängt das Token bereitgestellt, die mehrere Benutzer enthalten kann. Hardwareinventur für einen einzelnen Benutzer können müssen Sie auch den Hash für einen bestimmten Benutzer angeben, die Sie ausschließlich berücksichtigen möchten.

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>Antworttext

Wenn der Aufruf erfolgreich ist, gibt der Dienst ein Array von Artikeln im Bestand. Finden Sie unter [InventoryItem (JSON)](../../json/json-inventoryitem.md).

<a id="ID4E4E"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
  "pagingInfo": {
    "continuationToken": string,
    "totalItems": int
  },
  "items":
  {
    "url": string,
    "itemType": "Music",
    "titleId": string,
    "containers": string,
    "obtained": DateTime,
    "startDate": DateTime,
    "endDate": DateTime,
    "state": "Enabled"  
}

```


<a id="ID4EHF"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EJF"></a>


##### <a name="parent"></a>Parent

[/Users/Me/Inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>Weitere Informationen

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Marketplace-URIs](atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)
