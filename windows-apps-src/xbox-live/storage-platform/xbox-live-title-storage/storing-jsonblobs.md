---
title: Speichern von JSON-blob
description: Erfahren Sie, wie Sie einen JSON-Blob in Xbox Live Titel Speicher zu speichern.
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Title-Speicher
ms.localizationpriority: medium
ms.openlocfilehash: dabcd0634bb20bc6b82ae302c77338b5bb726a63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595065"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>Speichern von JSON-Blob im Speicher von Xbox Live-Titel

1.  Senden einer Anforderung mit der *PUT* Methode, um die Daten an den Titel Speicher zu senden.

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   Der Benutzer muss sich in der Sitzung zu aktualisieren.

-   STSTokenString ist ein Platzhalter aus Gründen der Übersichtlichkeit und ersetzt werden soll, mit dem Token von die Authentifizierungsanforderung zurückgegeben.

2.  Senden Sie eine JSON-Objekt.

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>Verweis

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
