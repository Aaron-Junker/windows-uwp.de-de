---
title: Lesen eines Konfigurations-BLOBs
description: Erfahren Sie, wie einen Konfigurations-Blob im Xbox Live Titel Speicher zu lesen.
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 71607f05f07ee4a98f73a19c28c85dc325041a10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599395"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>Lesen eines Konfigurations-BLOBs im Speicher von Xbox Live-Titel

*Konfiguration Blobs* sind Dateien im Speicher für Xbox Live Titel, die Spieledaten enthalten. Die Daten sind JSON-Objekte, die virtuelle Knoten enthalten, die gefiltert werden können, bevor Sie dem Spiel übermittelt werden. Weitere Informationen zur Konfiguration von Blobs finden Sie unter **Titel Storage URIs**.

### <a name="to-read-a-configuration-blob"></a>Um einen Konfigurations-Blob lesen

1.  Senden Sie eine Anforderung mithilfe der unten Methode, um die Daten aus dem Title-Speicher zu lesen.

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   Der Benutzer muss sich in der Sitzung zu aktualisieren.
-   STSTokenString ist ein Platzhalter aus Gründen der Übersichtlichkeit und ersetzt werden soll, mit dem Token von die Authentifizierungsanforderung zurückgegeben.

#### <a name="reference"></a>Verweis

**/ global/Scids / {scid} / Data / {PathAndFileName}, {Type}**
**abrufen (/ global/Scids / {scid} / Data / {PathAndFileName}, {Type})**
