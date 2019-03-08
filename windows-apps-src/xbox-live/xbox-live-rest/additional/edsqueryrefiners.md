---
title: EDS-Abfrageneinschränkungen
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
description: " EDS-Abfrageneinschränkungen"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c00ff971e05003ec88c47d3803e565f6e9406c47
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611465"
---
# <a name="eds-query-refiners"></a>EDS-Abfrageneinschränkungen
 
<a id="ID4EO"></a>

  
 
Die folgenden Parameter können verwendet werden, um eine Unterhaltung Discovery Services (EDS)-Abfrage an einen besser abgestimmten Satz von Elementen zu verfeinern. Keine der folgenden Parameter müssen in einer API, aber sie akzeptiert haben, in einer API, die Abfrage Raffinierer akzeptiert.
 
Die Parameternamen können als Werte in jeder "QueryRefiners"-Parameter übergeben werden. Dieser wird dann zurückgegeben, die die Anzahl der Elemente, die zurückgegeben werden, wenn die Anforderung mit der Abfrage Farbänderungsfeld wiederholt wurden angewendet, aufgeschlüsselt nach jeder Wert, der die Abfrage Farbänderungsfeld.
 
Hier ist, wie dies in der Praxis funktionieren kann:
 
   * Erfolgt ein Aufruf an die Durchsuchen-API, einschließlich des Parameters "QueryRefiners =" Genre "".
   * Die API gibt acht Spiele zurück. Zusätzlich zu den Elementen wird eine Liste von einzelnen "Genre", die Elemente zurückgegeben werden zusammen mit der Anzahl der Elemente, "Genre" angehören. Für ein Spiel, dies ist möglicherweise "Shooter: 3, Rätsel: 5".
   * Eine zweite Abfrage erfolgt. Es ist mit dem ersten identisch, außer dass "" Genre "= Shooter" wird hinzugefügt.
   * Die Antwort enthält jetzt nur drei Spiele, die in der Kategorie "Shooter" gehören.
  
| Parameter| Datentyp| Beschreibung| 
| --- | --- | --- | 
| <b>decade</b>| string| Die zehn Jahren, in dem alle Elemente veröffentlicht wurden müssen.| 
| <b>genre</b>| Array von Zeichenfolgen| Die Liste der Genres, die alle Elemente aufweisen muss.| 
| <b>labelOwner</b>| string| Die Music-Bezeichnung zugeordnet, die Interpreten, Alben oder nachverfolgen.| 
| <b>network</b>| Array von Zeichenfolgen| Das Netzwerk, das die Aufgaben erstellt hat.| 
| <b>studio</b>| Array von Zeichenfolgen| Der Studio, die die Aufgaben erstellt hat.| 
| <b>xboxAppCategories</b>| Array von Zeichenfolgen| Die Liste der Kategorien, die alle Xbox-Anwendungen benötigen.| 
| <b>xboxAvatarClothes</b>| Array von Zeichenfolgen| Die Liste der einzelnen Typen müssen alle Xbox Avatar-Elemente.| 
| <b>xboxAvatarStores</b>| Array von Zeichenfolgen| Die Liste der Speicher, der alle, die Xbox Avatar Elemente gehören müssen.| 
| <b>xboxGamePublisherBits</b>| Array von Zeichenfolgen| Die Liste der game-Publisher-Bits, die auf alle GameType Elemente oder der AppType Elemente festgelegt werden muss.| 
| <b>xboxIsBrowsable</b>| Boolescher Wert| Wenn <b>"true"</b>, vollständige Spiele, die nicht direkt neben der anwendbare Inhalte handlungsrelevant sind zurück. Standardmäßig <b>"false"</b>.| 
| <b>xboxHasChildMediaItemTypes</b>| Array von Zeichenfolgen| Alle zurückgegebenen Elemente mit einer Media-Gruppe des Spiels müssen untergeordnete Elemente verfügen, dessen Elementtyp der Medien eines der bereitgestellten Werte.| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Marketplace-URIs](../uri/marketplace/atoc-reference-marketplace.md)

   