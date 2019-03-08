---
title: Neuerungen für die Xbox Live SDK – Juni 2016
description: Neuerungen für die Xbox Live SDK – Juni 2016
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1d984d054d9e5fd7f9d34b42c1a224d53632e719
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595285"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>Neuerungen für die Xbox Live SDK – Juni 2016

Informieren Sie sich die [– April 2016 Neuigkeiten](1604-whats-new.md) Artikel was hinzugefügt wurde in Version April 2016.

> **Hinweis:** Bei Verwendung der Xbox Entwickler Kit (xdk-Version), und klicken Sie dann die *– April 2016 Neuigkeiten* Artikel wird beschrieben, weitere neue Funktionen, die seit der März-xdk-Version Version hinzugefügt wurde.

## <a name="os-and-tool-support"></a>Unterstützung für Betriebssystem und Tools
Das Xbox Live-SDK unterstützt Windows 10 RTM [Version 10.0.10240] und Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="contextual-search"></a>Kontextbezogene Suche
Die folgenden neuen Klassen hinzugefügt wurden die `contextual_search` -API, um das Spiel Clips abzurufen:

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

Finden Sie in der Referenzdokumentation für Weitere Informationen.

## <a name="networking"></a>Netzwerk
Die Xbox Live SDK enthält nun ein neues bestätigen, dass es sich bei, der erkennt, wenn mindestens 5 Websockets pro Benutzer in einer einzelnen Titel-Instanz erstellt werden.

Deaktivieren Sie diese durch Aufrufen von assert `disable_asserts_for_maximum_number_of_websockets_activated()`.

> **Hinweis:** US-Sozialversicherungsnummern und Multiplayer-Manager verwenden ein einzelnes kombiniertes Websocket, wenn Sie diese Features in Ihrem Titel verwenden.

## <a name="tools"></a>Tools
* Die Xbox Live Resilienz-Plug-In für Fiddler ist jetzt in das Xbox Live-SDK enthalten.  
Es ist ein Add-on von Fiddler, die Entwicklern ermöglicht, Aufrufe von Xbox Live selektiv zu blockieren.
Verwenden dieses Plug-in, können Entwickler über teilweise dienstunterbrechungen in ihre Spiele Titel resilienz testen.
Das Tool enthält eine Anzahl von Xbox Live-Dienst-Endpunkte integrierte und andere Fehlertypen für den Test.
Alle Xbox One und UWP-Titel werden unterstützt.
