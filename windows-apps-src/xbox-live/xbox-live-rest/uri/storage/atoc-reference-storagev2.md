---
title: URIs für Titelspeicher
assetID: 32bba1e4-0980-785e-c098-a96cd88a8e5f
permalink: en-us/docs/xboxlive/rest/atoc-reference-storagev2.html
description: " URIs für Titelspeicher"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: e0296eff0937ea5075630db0e049c86e2ea2c8ce
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622675"
---
# <a name="title-storage-uris"></a>URIs für Titelspeicher
 
Dieser Abschnitt enthält Informationen zu Universal Resource Identifier (URI)-Adressen und zugeordneten Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für *Titel Storage*.
 
Spiele, die auf allen Plattformen ausgeführt wird, können diesen Dienst verwenden.
 
Die Domäne für diese URIs ist titlestorage.xboxlive.com.
 
<a id="ID4EFB"></a>

 
## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/global/scids/{scid}](uri-globalscidsscid.md)

&nbsp;&nbsp;Ruft Informationen zum Kontingent für diesen Speichertyp ab.

[/global/scids/{scid}/data/{path}](uri-globalscidssciddatapath.md)

&nbsp;&nbsp;Listet die Informationen in einem angegebenen Pfad. 

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Lädt eine Datei herunter.

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen.

[/json/users/xuid({xuid})/scids/{scid}](uri-jsonusersxuidscidsscid.md)

&nbsp;&nbsp;Ruft Informationen zum Kontingent für diesen Speichertyp ab.

[/json/users/xuid({xuid})/scids/{scid}/data/{path}](uri-jsonusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Listet die Informationen in einem angegebenen Pfad. 

[/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Downloads, hochgeladen, oder löscht eine Datei.

[/sessions/{sessionId}/scids/{scid}](uri-sessionssessionidscidsscid.md)

&nbsp;&nbsp;Ruft Informationen zum Kontingent für diesen Speichertyp ab.

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

&nbsp;&nbsp;Listet die Informationen in einem angegebenen Pfad. 

[/sessions/{sessionId}/scids/{scid}/data/{pathAndFileName},{type}](uri-sessionssessionidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Lädt eine Datei herunter.

[/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen.

[/trustedplatform/users/xuid({xuid})/scids/{scid}](uri-trustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Ruft Informationen zum Kontingent für diesen Speichertyp ab.

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-trustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Listet die Informationen in einem angegebenen Pfad. 

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Downloads, hochgeladen, oder löscht eine Datei.

[/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Mehrere Dateien heruntergeladen mehrerer Benutzer mit dem gleichen Dateinamen.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}](uri-untrustedplatformusersxuidscidsscid.md)

&nbsp;&nbsp;Ruft Informationen zum Kontingent für diesen Speichertyp ab.

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{path}](uri-untrustedplatformusersxuidscidssciddatapath.md)

&nbsp;&nbsp;Listet die Informationen in einem angegebenen Pfad. 

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

&nbsp;&nbsp;Downloads, hochgeladen, oder löscht eine Datei.
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   