---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
description: " POST ({itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 877986ce9d48269295a68dbfd644f14785916b88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640855"
---
# <a name="post-itemid"></a>POST ({itemID})
Gibt an, dass alle oder einen Teil einer nutzbar Lagerartikel verwendet wurde, und verringert die Menge des durch den angeforderten Betrag der verwendet werden können.
Die Domäne für diese URIs ist `inventory.xboxlive.com`.

  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EQB)
  * [Anforderungstext](#ID4E2B)
  * [Antworttext](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Hinweise

   * Wenn die Menge, die der Aufrufer aufgefordert, nutzen die verbleibende erneut mit Strom des Elements überschreitet, werden der Aufruf abgelehnt.
   * Die Menge, die der Aufrufer verwenden, muss eine positive ganze Zahl über 0 sein. Aufrufe mit dem Verbrauch-Wert von 0 oder niedriger werden zurückgewiesen.
   * Wenn der Aufrufer eine leere Transaktions-ID bereitstellt, wird die Anforderung zurückgewiesen werden.
   * Gegebenenfalls werden, damit es möglich ist, zu bestimmen, welche Titel meldet die Nutzung der Title-Anspruch protokolliert.
   * Weitere Beiträge mit derselben Transaktions-ID werden für einen bestimmten Zeitraum ignoriert.


> [!NOTE]
> Die <b>Header X-Xbl-Contract-Version</b> für diese API "4" ist.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| itemID| string| die ID für jeden Benutzer für eine einzelne Lagerartikel eindeutig|

<a id="ID4E2B"></a>


## <a name="request-body"></a>Anforderungstext

<a id="ID4EBC"></a>


### <a name="sample-request"></a>Beispiel für eine Anforderung


```cpp
{
  "transactionId": String
  "removeQuantity": Int
}

```


Remove Quantity-Felds kann der Aufrufer die Menge der verwendet werden können anzugeben, die sie aus der verwendet werden können die verbleibende Menge entfernen möchten. Das Transaktions-ID-Feld enthält, den Aufrufer mit der Möglichkeit, versuchen Sie es mit verständlichen inhaltsvorgänge und schränkt gleichzeitig die in Bezug auf die gleiche Verwendung zweimal gezählt wird.

<a id="ID4ENC"></a>


## <a name="response-body"></a>Antworttext

Die Antwort an das POSTING, vorausgesetzt, es Authentifizierung erfolgreich ist und den entsprechenden Autorisierungskontext ist ein ein Acknolodgement Empfangsbestätigung mit dem gleichen Transaktions-ID, die an den Dienst in der POST-Anforderung, der nutzbar URL des Elements und des Elements neu Wert für die Menge.

<a id="ID4EVC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
  "transactionId": String
  "url": String
  "newQuantity": Int
}

```


<a id="ID4E6C"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EBD"></a>


##### <a name="parent"></a>Parent

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>Weitere Informationen

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Marketplace-URIs](atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)
