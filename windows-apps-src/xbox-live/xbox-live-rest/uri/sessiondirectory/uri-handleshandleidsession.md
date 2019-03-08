---
title: /handles/{handleId}/session
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
description: " /handles/{handleId}/session"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: e7b6990917437c22dd4d9282492e2a0eab37893b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628145"
---
# <a name="handleshandleidsession"></a>/handles/{handleId}/session
Unterstützt die PUT- und GET-Vorgänge für eine Sitzung mithilfe von Handle zu dereferenzieren. 

> [!NOTE] 
> Dieser URI wird von 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es ist für die Verwendung mit Vorlage Vertrag 104/105 oder höher vorgesehen.  

 

> [!NOTE] 
> Dieser URI ist derzeit nur ein Xbox-Konsolen und Servern, die mit einer Dienst-ID extern zugänglich.  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| Die eindeutige ID des Handles für die Sitzung.| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (/handles/ {HandleId} / Sitzung)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;Ruft ein Objekt für die angegebene Handle-ID ab. 

[Einfügen (/handles/ {Handle-Id} / Sitzung)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;Erstellt oder aktualisiert eine Sitzung durch die Dereferenzierung eines Handles.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)

   