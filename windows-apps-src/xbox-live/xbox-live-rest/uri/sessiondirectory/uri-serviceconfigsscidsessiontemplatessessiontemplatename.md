---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}
assetID: cc306ca0-57f8-f676-6d4b-f9ddd716dcc0
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatename.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 36b4034607fc6b5011491424551ed60bc3da5fa7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599265"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatename"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}
Unterstützt einen GET-Vorgang um einen Vorlagennamen für die Sitzung abzurufen. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| GUID| Dienst-Konfigurationsbezeichner (SCID). Teil 1 von der Sitzungs-ID.| 
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2 von der Sitzungs-ID. | 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/serviceconfigs/ {scid} /sessiontemplates/ {SessionTemplateName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenameget.md)

&nbsp;&nbsp;Ruft einen Satz von Vorlagennamen für die Sitzung ab.
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)

   