---
title: Ein binäres Blob speichern
description: Erfahren Sie, wie ein binäres Blob im Xbox Live Titel Speicher zu speichern.
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Title-Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 70e620f2e992dfdd240f71b5c73165f13d4ef5cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660095"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>Speichern ein binäres Blob im Speicher von Xbox Live-Titel

1.  Eine Anforderung mithilfe der unter-Methode zum Senden der Daten in den Speicher des Titels.

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   Der Benutzer muss sich in der Sitzung zu aktualisieren.

-   STSTokenString ist ein Platzhalter aus Gründen der Übersichtlichkeit und ersetzt werden soll, mit dem Token von die Authentifizierungsanforderung zurückgegeben.

2.  Die binären Daten zu senden. Da die Daten über HTTP übertragen werden, müssen die Daten auf den zulässige Zeichensatz eingeschränkt werden. Informationen wie z. B. Bild oder audio-Daten muss codiert werden. Sie können eine beliebige Codierungsmethode auswählen, die HTTP-kompatible Zeichen generiert.
d
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>Verweis

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
