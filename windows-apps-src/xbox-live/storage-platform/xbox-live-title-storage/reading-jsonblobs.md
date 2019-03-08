---
title: Lesen von JSON-blob
description: Erfahren Sie, wie einen JSON-Blob im Xbox Live Titel Speicher zu lesen.
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 30d2b9f6539e73df1c5ea5ae18b3d1a6ca061d60
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613485"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>Lesen von JSON-Blob im Speicher von Xbox Live-Titel

1.  Senden einer Anforderung mit der *erhalten* Methode, um die Daten aus dem Title-Speicher zu lesen. Dieses Beispiel verwendet die globale Title-Speicher.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   Der Benutzer muss sich in der Sitzung zu aktualisieren.

-   STSTokenString ist ein Platzhalter aus Gründen der Übersichtlichkeit und ersetzt werden soll, mit dem Token von die Authentifizierungsanforderung zurückgegeben.

#### <a name="reference"></a>Verweis

**/global/scids/{scid}/data/{pathAndFileName},{type}**
