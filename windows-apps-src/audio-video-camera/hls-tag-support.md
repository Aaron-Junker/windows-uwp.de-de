---
ms.assetid: 66a9cfe2-b212-4c73-8a36-963c33270099
description: In diesem Artikel finden Sie eine Liste der für UWP-Apps unterstützten Tags für das HLS-Protokoll (HTTP Live Streaming).
title: Unterstützung von HLS-Tags (HTTP Live Streaming)
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7c6664d13e76a5774172094d632de9db25109fdc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613085"
---
# <a name="http-live-streaming-hls-tag-support"></a>Unterstützung von HLS-Tags (HTTP Live Streaming)
In der folgenden Tabelle sind die HLS-Tags aufgeführt, die für UWP-Apps unterstützt werden.

> [!NOTE] 
> Auf benutzerdefinierte Tags, die mit „X-” beginnen, kann wie im Artikel zu [Medienelementen, Wiedergabelisten und Titeln](media-playback-with-mediasource.md) beschrieben als zeitgesteuerte Metadaten zugegriffen werden.

|Tag |Eingeführt in HLS-Protokollversion|Entwurfsversion des HLS-Protokolldokuments|Erforderlich auf dem Client|Juliversion von Windows 10|Windows 10, Version 1511|Windows 10, Version 1607 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  Grundlegende Tags                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|ERFORDERLICH|Unterstützt|Unterstützt|Unterstützt|
| 4.3.1.2.  EXT-X-VERSION |2|3|ERFORDERLICH|Unterstützt|Unterstützt|Unterstützt
|4.3.2.  Mediensegmenttags                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|ERFORDERLICH|Unterstützt|Unterstützt|Unterstützt
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|OPTIONAL|Unterstützt|Unterstützt|Unterstützt|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|OPTIONAL|Unterstützt|Unterstützt|Unterstützt|
| 4.3.2.4.  EXT-X-KEY |1|0|OPTIONAL|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp; METHODE|1|0|Attribut|„NONE, AES-128”|„NONE, AES-128”|„NONE, AES-128, SAMPLE-AES”|
|&nbsp;&nbsp;&nbsp; URI|1|0|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp; IV|2|3|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
| 4.3.2.5.  EXT-X-MAP |5|9|OPTIONAL|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp; URI|5|9|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|OPTIONAL|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|4.3.3.  Medienwiedergabelisten-Tags                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|ERFORDERLICH|Unterstützt|Unterstützt|Unterstützt|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|OPTIONAL|Unterstützt|Unterstützt|Unterstützt|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|OPTIONAL|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|OPTIONAL|Unterstützt|Unterstützt|Unterstützt|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|OPTIONAL|Unterstützt|Unterstützt|Unterstützt|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|OPTIONAL|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|4.3.4.  Masterwiedergabelisten-Tags                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|OPTIONAL|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  TYP|4|7|Attribut|„AUDIO, VIDEO”|„AUDIO, VIDEO”|„AUDIO, VIDEO, SUBTITLES”|
|&nbsp;&nbsp;&nbsp;  URI|4|7|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  GRUPPEN-ID|4|7|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  SPRACHE|4|7|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  ASSOC-SPRACHE|6|13|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp;  NAME|4|7|Attribut|Nicht unterstützt|Nicht unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  STANDARDWERT|4|7|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp;  AUTOMATISCH AUSWÄHLEN|4|7|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp;  ERZWUNGEN|5|9|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp;  MERKMALE|5|9|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|ERFORDERLICH|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  BANDBREITE|1|0|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  PROGRAMM-ID|1|0|Attribut|Nicht verfügbar|Nicht verfügbar|Nicht verfügbar|
|&nbsp;&nbsp;&nbsp;  DURCHSCHNITTLICHE BANDBREITE|7|14|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  AUFLÖSUNG|2|3|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  FRAMERATE|7|15|Attribut|Nicht verfügbar|Nicht verfügbar|Nicht verfügbar|
|&nbsp;&nbsp;&nbsp;  AUDIO|4|7|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  VIDEO|4|7|Attribut|Unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  UNTERTITEL|5|9|Attribut|Nicht unterstützt|Nicht unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  UNTERTITEL|6|12|Attribut|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|OPTIONAL|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|OPTIONAL|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|OPTIONAL|Nicht unterstützt|Nicht unterstützt|Nicht unterstützt|
|4.3.5.  Tags für Medien- oder Masterwiedergabelisten                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|OPTIONAL|Nicht unterstützt|Unterstützt|Unterstützt|
| 4.3.5.2.  EXT-X-START  |6|12|OPTIONAL|Nicht unterstützt|Teilweise unterstützt|Teilweise unterstützt|
|&nbsp;&nbsp;&nbsp;  OFFSET DER ZEIT|6|12|Attribut|Nicht unterstützt|Unterstützt|Unterstützt|
|&nbsp;&nbsp;&nbsp;  PRÄZISE|6|12|Attribut|Nicht unterstützt|„NO“ standardmäßig unterstützt|„NO“ standardmäßig unterstützt|



## <a name="related-topics"></a>Verwandte Themen

* [Medienwiedergabe](media-playback.md)
* [Adaptives streaming](adaptive-streaming.md)
 

 




