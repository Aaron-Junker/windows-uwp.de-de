---
title: Fehlerbehandlung
description: Erfahren Sie, wie Sie Fehler behandeln, beim Bereitstellen einer Xbox Live-Dienst aufrufen.
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Fehler, Dienstaufruf
ms.localizationpriority: medium
ms.openlocfilehash: 637b9da84ef3ae50ea6235ca86e6318ae62acd11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637005"
---
# <a name="error-handling"></a>Fehlerbehandlung

Entwickler sollten darauf achten, wenn ein Dienst-Aufrufs. Titel des sollte immer als Fehler behandelt, bei Netzwerkaufrufen. Sogar für Dienst-Aufrufe, die durchgängig während der Entwicklung gearbeitet haben, können Dienstaufrufe in realen Umgebungen für eine Reihe von aus verschiedenen Gründen fehlschlagen. Der Aufruf kann z. B. aufgrund von fehlschlagen:

* Das Netzwerk ist nicht verfügbar.
* Der Dienst ist zu stark ausgelastet, eine HTTP-Statuscode 503 zurückgeben.
* Die Anforderung ist nicht gültig ist, wird eine HTTP-Statuscode 400 zurückgeben.
* Der Benutzer besitzt nicht die richtigen Berechtigungen, ein 403-HTTP-Statuscode zurückgeben.
* Der Benutzer wurde ausgeschlossen, eine HTTP-Statuscode 401 zurückgegeben.
IXMLHTTPRequest2, die von der WinRT-Dienst-APIs aufgerufen wird, kann nicht zum Senden der Anforderung (ERROR_TIMEOUT, RPC_S_CALL_FAILED_DNE usw.).
* In der Liste oben ist nicht vollständig. Die meisten dieser Probleme führt zu einer Ausnahme, die von der Services-API-Ebene ausgelöst wird. Titel des sollten diese Ausnahmen zu erfassen und entsprechend behandelt.

Gibt es eine zwei verschiedene Formate für die Fehlerbehandlung in Xbox Services API (XSAPI) abhängig von der API verwenden Sie:

Finden Sie unter [C++-API-Fehlerbehandlung](error-handling-cpp.md).

Finden Sie unter [WinRT-API-Fehlerbehandlung](error-handling-winrt.md).
