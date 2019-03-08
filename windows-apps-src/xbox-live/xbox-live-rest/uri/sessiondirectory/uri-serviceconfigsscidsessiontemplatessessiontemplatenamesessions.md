---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
assetID: 8d55818f-99fd-146a-896b-0f100e78799f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 99d9a82cb419e6598fc1113692b031487950aa8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656095"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
Unterstützt einen GET-Vorgang um einen Satz von Vorlagen für ereignissitzungen mit den Namen der angegebenen Vorlage abzurufen. 
<a id="ID4EO"></a>

 
## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 von der Sitzungs-ID.| 
| keyword| string| Ein Schlüsselwort zum Filtern der Ergebnisse auf nur Sitzungen, die mit dieser Zeichenfolge identifiziert.| 
| xuid| GUID| Xbox-Benutzer-IDs für die Benutzer für die Sitzungen abzurufen. Der Benutzer müssen in den Sitzungen aktiv sein. | 
| Reservierungen| string| Der Wert, der angibt, wenn die Liste der Sitzungen die enthält, die die Benutzer nicht akzeptiert haben. Dieser Parameter kann nur festgelegt werden auf "true". Diese Einstellung muss der Aufrufer auf die Sitzung auf Serverebene zugreifen, oder des Aufrufers XUID Anspruch mit den Xbox-Benutzer-ID-Filter übereinstimmen. | 
| inaktiv| string| Der Wert, der angibt, wenn die Liste der Sitzungen die enthält, die der Benutzer zugestimmt haben, aber keine aktiven Audiowiedergabe. Dieser Parameter kann nur festgelegt werden auf "true". | 
| Private| string| Der Wert, der angibt, wenn die Liste der Sitzungen private Sitzungen enthält. Dieser Parameter kann nur festgelegt werden auf "true". Es gilt nur, wenn Ihre eigenen Sitzungen Abfragen oder beim Abfragen von Server-zu-Server. Dieser Parameter auf "true" festlegen, muss der Aufrufer auf die Sitzung auf Serverebene zugreifen, oder des Aufrufers XUID Anspruch mit den Xbox-Benutzer-ID-Filter übereinstimmen. | 
| visibility| string| Ein Enumerationswert, der angibt, Sichtbarkeitsstatus zum Filtern der Ergebnisse verwendet wird. Derzeit kann diesen Parameter nur zu öffnen festgelegt werden, offene Sitzungen enthalten. Finden Sie unter <b>MultiplayerSessionVisibility</b>. | 
| version| string| Eine positive ganze Zahl, der angibt, die wichtige Sitzung-Version oder eine niedrigere Sitzungen einschließen. Der Wert muss kleiner als oder gleich der Anforderung vertragsversion modulo 100 sein. | 
| Take| string| Eine positive ganze Zahl, der angibt, der maximalen Anzahl von Sitzungen abgerufen.| 
  
<a id="ID4EZD"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (/serviceconfigs/ {scid} /sessiontemplates/ {SessionTemplateName} / Sitzungen)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.md)

&nbsp;&nbsp;Ruft die Vorlagendokumente Sitzung ab.
 
<a id="ID4EDE"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EFE"></a>

 
##### <a name="parent"></a>Parent 

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)

   