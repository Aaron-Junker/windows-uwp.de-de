---
title: URIs für Session Directory
assetID: e3ba951d-b21f-0014-c358-2603d549d118
permalink: en-us/docs/xboxlive/rest/atoc-reference-sessiondirectory.html
description: " URIs für Session Directory"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 9492ff3272af830404a546c9b01d62178adbac96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651195"
---
# <a name="session-directory-uris"></a>URIs für Session Directory

Dieser Abschnitt enthält Informationen zu Universal Resource Identifier (URI)-Adressen und zugehörigen Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für die Multiplayer-Sitzung-Verzeichnis (MPSD).


> [!NOTE] 
> Nur einen Titel für Spiele, die ausgeführt wird, auf eine Xbox 360, auf einem Windows Phone-Gerät oder auf "Xbox.com" können das Sitzungsverzeichnis URIs.  


  * [Domain](#ID4EUB)
  * [Service-version](#ID4EZB)
  * [Systemobjekten und Eigenschaften](#ID4EAC)
  * [Handles](#ID4EBE)

<a id="ID4EUB"></a>


## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4EZB"></a>


## <a name="service-version"></a>Service-version

Aufrufer dieser REST-URIs müssen den Wert 104/105 oder höher für die X-Xbl-Contract-Version, die HTTP-Header übergeben, die angibt, die Dienstversion der Unterhaltung Discovery Services (EDS).

<a id="ID4EAC"></a>


## <a name="system-objects-and-properties"></a>Systemobjekten und Eigenschaften

Für die Konfiguration der Sitzungen und Vorlagen verwendet der MPSD eine Reihe von Sitzung JSON-Objekten, die entsprechen, mit festen Schemas, die das Verzeichnis erzwingt und interpretiert. Beim Aufrufen der Methoden, die von den verschiedenen Sitzungsverzeichnisdienst URIs unterstützt werden werden diese Objekte überprüft und zusammengeführt, basierend auf den unterstützten Schemas. Die wichtigsten JSON-Objekte, Multiplayer-Konfiguration zugeordnet sind:

   *  [MultiplayerActivityDetails (JSON)](../../json/json-multiplayeractivitydetails.md)
   *  [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)
   *  [MultiplayerSessionReference (JSON)](../../json/json-multiplayersessionreference.md)
   *  [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)


Zugeordnete JSON-Objekte, die speziell mit Titeln zu befürchten sind:

   *  [GameMessage (JSON)](../../json/json-gamemessage.md)
   *  [GameResult (JSON)](../../json/json-gameresult.md)
   *  [GameSession (JSON)](../../json/json-gamesession.md)
   *  [GameSessionSummary (JSON)](../../json/json-gamesessionsummary.md)


<a id="ID4EBE"></a>


## <a name="handles"></a>Handles

Für 2015 Multiplayer-Spiele nur können die Sitzungen durch Sitzung Handles zugegriffen werden. Mehrere URIs wurde eine Funktionalität zur Unterstützung von Handles hinzugefügt.  
<a id="ID4EFE"></a>


## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/handles](uri-handles.md)

&nbsp;&nbsp;Unterstützt einen POST-Vorgang zum Festlegen der Sitzungs für die aktuelle Aktivität des Benutzers, in der Benutzeroberfläche für Xbox One-Dashboard angezeigt werden, und Sitzung-Mitglieder einladen, falls erforderlich.

[/handles/{handleId}](uri-handleshandleid.md)

&nbsp;&nbsp;Unterstützt die DELETE und GET-Operationen für die Sitzung verarbeitet, die vom Bezeichner angegebene.

[/handles/{handleId}/session](uri-handleshandleidsession.md)

&nbsp;&nbsp;Unterstützt die PUT- und GET-Vorgänge für eine Sitzung mithilfe von Handle zu dereferenzieren.

[/handles/query](uri-handlesquery.md)

&nbsp;&nbsp;Unterstützt die POST-Vorgänge zum Erstellen von Abfragen für die Sitzung verarbeitet.

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

&nbsp;&nbsp;Unterstützt einen POST-Vorgang für eine Batchabfrage auf der Dienstebene des Konfigurations-ID an.

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

&nbsp;&nbsp;Unterstützt einen GET-Vorgang um eine Reihe von Dokumenten der Sitzung abzurufen.

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

&nbsp;&nbsp;Unterstützt einen GET-Vorgang um einen Satz von Vorlagen für ereignissitzungen MPSD abzurufen.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)

&nbsp;&nbsp;Unterstützt einen GET-Vorgang um einen Vorlagennamen für die Sitzung abzurufen.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)

&nbsp;&nbsp;Unterstützt einen POST-Vorgang zum Erstellen einer Batchabfrage auf der Sitzungsebene für die Vorlage an.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)

&nbsp;&nbsp;Unterstützt einen GET-Vorgang um einen Satz von Vorlagen für ereignissitzungen mit den Namen der angegebenen Vorlage abzurufen.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

&nbsp;&nbsp;Unterstützt die PUT- und GET-Vorgänge zum Erstellen und Abrufen von Sitzungen.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

&nbsp;&nbsp;Unterstützt einen Löschvorgang an, um die angegebene Sitzungs-Element zu entfernen.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)

&nbsp;&nbsp;Unterstützt einen DELETE-Vorgang zum Entfernen von Mitgliedern der Sitzung.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)

&nbsp;&nbsp;Unterstützt einen Löschvorgang an, um den angegebenen Server einer Sitzung entfernen.

<a id="ID4ESF"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EUF"></a>

   [Vermittlung-URIs](../matchtickets/atoc-reference-matchtickets.md)


<a id="ID4E1F"></a>


##### <a name="parent"></a>Parent

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)
