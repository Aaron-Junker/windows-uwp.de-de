---
title: URIs für Absenderzuverlässigkeit
assetID: d1cb76c0-86a4-8c51-19f6-5f223b517d46
permalink: en-us/docs/xboxlive/rest/atoc-reference-reputation.html
description: " URIs für Absenderzuverlässigkeit"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 93d6d6e6acfd8fa39bd9d26c87ed99362d2c88d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614005"
---
# <a name="reputation-uris"></a>URIs für Absenderzuverlässigkeit
 
Dieser Abschnitt enthält Informationen zu Universal Resource Identifier (URI)-Adressen und zugeordneten Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für die **Microsoft.Xbox.Services.Social.ReputationService** . Die Domäne für den Ruf URIs ist reputation.xboxlive.com. Eine typische URI-Darstellung möglicherweise https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback. 
 
Der Reputation Service verwendet Feedback Siehe [Feedback (JSON)](../../json/json-feedback.md), um eine Bewertung der Bewertung zu berechnen. Dieses Ergebnis wird in den Bereich der Statistiken für den Benutzer unter dem Schlüssel ReputationOverall gespeichert. Weitere Informationen zum Abrufen von Statistiken finden Sie unter [abrufen (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md). 
 
Spiele für alle Plattformen können Reputation Service verwenden.
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;Von Ihrem Titel verwendet, wenn Sie wünschen, Option "Feedback" in Ihrem Spiel, im Gegensatz zur Verwendung der Shell hinzufügen.

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;Von der Titel des Diensts verwendet zum Senden von Feedback im Batch-Format außerhalb des Titels-Benutzeroberfläche.

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;Ermöglicht es das Team Erzwingung des aktuellen Benutzers Reputation Bewertungen den Zugriff auf.

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;Der Ruf-Daten für einen Testbenutzer zurückgesetzt vollständig. Nur zu Testzwecken.

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;Ermöglicht es das Team Erzwingung des angegebenen Benutzers Reputation Bewertungen den Zugriff auf.
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   