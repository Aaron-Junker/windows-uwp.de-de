---
title: Ein binäres Blob lesen
description: Erfahren Sie, ein binäres Blob im Xbox Live Titel Speicher lesen.
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Title-Speicher
ms.localizationpriority: medium
ms.openlocfilehash: e2a0be72142a3e11c7c680cfc998287f396c2afc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641655"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>Lesen Sie ein binäres Blob im Speicher von Xbox Live-Titel

1.  Senden einer Anforderung mit der *erhalten* Methode, um die Daten aus dem Title-Speicher zu lesen. Dieses Beispiel verwendet die globale Title-Speicher.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   Der Benutzer muss sich in der Sitzung zu aktualisieren.

-   STSTokenString ist ein Platzhalter aus Gründen der Übersichtlichkeit und ersetzt werden soll, mit dem Token von die Authentifizierungsanforderung zurückgegeben.

#### <a name="reference"></a>Verweis

**/global/scids/{scid}/data/{pathAndFileName},{type}**
