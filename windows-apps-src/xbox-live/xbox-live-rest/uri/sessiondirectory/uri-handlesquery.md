---
title: /handles/query
assetID: e00d31ad-b9ba-8e52-1333-83192eab0446
permalink: en-us/docs/xboxlive/rest/uri-handlesquery.html
description: " /handles/query"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: eaa148972ce1e65056470a6c4082cb4e50de3f09
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598565"
---
# <a name="handlesquery"></a>/handles/query
Unterstützt die POST-Vorgänge zum Erstellen von Abfragen für die Sitzung verarbeitet. 

> [!NOTE] 
> Dieser URI wird von 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es ist für die Verwendung mit Vorlage Vertrag 104/105 oder höher vorgesehen.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
Dieser URI unterstützt Abfragen für Handles. Anders als bei Sitzung Abfragen, die Zeichenfolge "und" Batch-Abfragen sind, verwenden die Handle-Abfragen einen Abfrageprozessor Stil. Bis zu 100 Handles werden unterstützt.  
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
Keine   
<a id="ID4EEB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[POST (/ Handles/Abfrage)](uri-handlesquerypost.md)

&nbsp;&nbsp;Erstellt Abfragen für die Sitzung verarbeitet.

[POST (Abfrage/Handles /? enthalten RelatedInfo =)](uri-handlesqueryincludepost.md)

&nbsp;&nbsp;Erstellt Abfragen für die Sitzung verarbeitet, die verwandten Sitzungs-Informationen enthalten.
 
<a id="ID4EQB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ESB"></a>

 
##### <a name="parent"></a>Parent 

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)

   