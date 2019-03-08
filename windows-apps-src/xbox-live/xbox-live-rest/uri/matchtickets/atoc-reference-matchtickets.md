---
title: URIs für Matchmaking
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
description: " URIs für Matchmaking"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c52abca7ed49d4a5e14520095ae944938b86f093
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590465"
---
# <a name="matchmaking-uris"></a>URIs für Matchmaking
 
Dieser Abschnitt enthält Informationen zu Universal Resource Identifier (URI)-Adressen und zugehörigen Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für die Vermittlungsdienst. 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>Domäne
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>Service-version
 
Aufrufer dieser HTTP-REST-URIs müssen den Wert 103 oder höher für die X-Xbl-Contract-Version, die HTTP-Header übergeben, die angibt, die Dienstversion der Unterhaltung Discovery Services (EDS). 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>Systemobjekten und Eigenschaften
 
Derzeit die gesamte Konfiguration die Vermittlungsdienst tritt auf, manuell den Dienst Configuration Teil mithilfe der [Xbox-Entwickler-Portal (XDP)](https://xdp.xboxlive.com) oder [Partner Center](https://partner.microsoft.com/dashboard). Einige Informationen für die Vermittlung wird auch in den Objekten, die für die MPSD definierten übernommen. 
 
Sind die wichtigsten JSON-Objekte, die zum Konfigurieren der Vermittlung in definiert [MatchTicket (JSON)](../../json/json-matchticket.md) und [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md). Beachten Sie, dass alle Tickets übereinstimmen muss definieren eine **TicketSessionRef** Objekt, geben Sie einen Verweis auf eine Multiplayer-Sitzung mit dem Player oder Spieler, die mit anderen verglichen werden soll. 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;Unterstützt einen POST-Vorgang zum Erstellen von Tickets für Übereinstimmung an. 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;Unterstützt einen GET-Vorgang zum Abrufen von Statistiken für eine Hopper an.

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;Unterstützt einen Löschvorgang für ein Ticket für die Übereinstimmung an.
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [Sitzung Directory URIs](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   