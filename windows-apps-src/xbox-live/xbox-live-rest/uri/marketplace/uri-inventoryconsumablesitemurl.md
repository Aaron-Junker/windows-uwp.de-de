---
title: /users/me/consumables/{itemID}
assetID: 45724827-5e35-326f-3f17-f49e606d9e08
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurl.html
description: RESTful-Endpunkt für Verbrauchsgüter Xbox, die für einen Benutzer.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4822180f11879417be67351f901474a38f89d82e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614085"
---
# <a name="usersmeconsumablesitemid"></a>/users/me/consumables/{itemID}
Greift auf der vollständigen Satz an Details zu einem bestimmten Lagerartikel für genutzt werden.
Die Domäne für diese URIs ist `inventory.xboxlive.com`.

  * [URI-Parameter](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| itemID| string| die ID für jeden Benutzer für eine einzelne Lagerartikel eindeutig|

<a id="ID4ERB"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[POST ({ItemID})](uri-inventoryconsumablesitemurlpost.md)

&nbsp;&nbsp;Gibt an, dass alle oder einen Teil einer nutzbar Lagerartikel verwendet wurde, und verringert die Menge des durch den angeforderten Betrag der verwendet werden können.

<a id="ID4E4B"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent

[Marketplace-URIs](atoc-reference-marketplace.md)


<a id="ID4EJC"></a>


##### <a name="further-information"></a>Weitere Informationen

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)
