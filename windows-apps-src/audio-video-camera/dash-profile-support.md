---
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: Dieser Artikel enthält eine Liste der Dynamic Adaptive Streaming over HTTP (DASH)-Profile, die für UWP-Apps unterstützt werden.
title: Dynamic Adaptive Streaming over HTTP (DASH)-Profilunterstützung
ms.date: 02/15/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 268020c06be57d8ac300f1202046e52b6e1d2507
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867381"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Dynamic Adaptive Streaming over HTTP (DASH)-Profilunterstützung


## <a name="supported-dash-profiles"></a>Unterstützte DASH-Profile
In der folgenden Tabelle sind die DASH-Profile aufgeführt, die für UWP-Apps unterstützt werden.

|Tag | Manifesttyp | Hinweise|Juliversion von Windows 10|Windows 10, Version 1511|Windows 10, Version 1607 |Windows 10, Version 1607 |Windows 10, Version 1703| Windows 10, Version 1809
|----------------|------|-------|-----------|--------------|---------|-------|--------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Statisch |     |Unterstützt            |  Unterstützt              | Unterstützt        |Unterstützt| Unterstützt| Unterstützt|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | Beste Leistung | Unterstützt            |  Unterstützt              | Unterstützt        |Unterstützt| Unterstützt| Unterstützt|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Dynamic | $Time$ wird in Segmentvorlagen unterstützt, aber $Number$ nicht. | Nicht unterstützt            | Nicht unterstützt              | Nicht unterstützt        |Nicht unterstützt| Unterstützt| Unterstützt|
|urn:mpeg&#58;dash:profile:isoff-on-demand:2011 |        |  | Nicht unterstützt            |  Nicht unterstützt              | Nicht unterstützt        |Nicht unterstützt| Nicht unterstützt| Unterstützt|


## <a name="unsupported-dash-profiles"></a>Nicht unterstützte DASH-Profile
Profile, die nicht in der obigen Tabelle aufgeführt sind, werden nicht unterstützt, einschließlich, aber nicht beschränkt auf folgende:

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>Verwandte Themen

* [Medienwiedergabe](media-playback.md)
* [Adaptives Streaming](adaptive-streaming.md)
 

 




