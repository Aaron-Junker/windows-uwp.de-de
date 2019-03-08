---
title: Pagingparameter
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
description: " Pagingparameter"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: fe80e1666f9eab8caf7e0bbdb2b537bd7661c9a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627295"
---
# <a name="paging-parameters"></a>Pagingparameter
 
Einige Xbox Live Services-URIs werden Auflistungen von JavaScript Objekt Notation (JSON)-Objekten zurückgeben. Diese Sammlungen können über ausgelagert werden, durch das Angeben von Paging-Parameter als Teil der Abfragezeichenfolge an den URI angefügt. Eine vollständige Liste der Parameter Paging folgt. Alle URIs ermöglichen das Paging-Parametern mit verknüpft sind am unteren Rand dieser Seite.
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter 
 
| Parameter| Erforderlich| Typ| Beschreibung| 
| --- | --- | --- | --- | 
| continuationToken| Nein| string| Gibt die Elemente, die beginnend ab des angegebenen Fortsetzungstokens zurück. | 
| "MaxItems"| Nein| 32-Bit-Ganzzahl mit Vorzeichen| Maximale Anzahl von Elementen aus der Auflistung zurückgegeben, die mit kombiniert werden können <b>SkipItems</b> und <b>"continuationtoken"</b> um einen Bereich von Elementen zurückzugeben. Der Dienst kann einen Standardwert angeben, wenn <b>"MaxItems"</b> ist nicht vorhanden, und möglicherweise weniger als zurück <b>"MaxItems"</b>, auch wenn die letzte Seite der Ergebnisse noch nicht zurückgegeben wurde. | 
| skipItems| Nein| 32-Bit-Ganzzahl mit Vorzeichen| Zurückgeben Sie Elemente, die ab dem die angegebene Anzahl von Elementen. Z. B. <b>SkipItems = "3"</b> ruft Elemente ab, mit dem vierten Element abgerufen. | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>Verweis [GET (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   