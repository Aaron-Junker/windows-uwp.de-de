---
title: Xbox Live, Multiplayer-Plattform
description: Erfahren Sie, bis die Multiplayer-Plattformen, die für die Xbox Live sind.
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: c71fc28a9bb8668d0253834c1a2c9c6ca7736fd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651155"
---
# <a name="xbox-live-multiplayer-platform"></a>Xbox Live, Multiplayer-Plattform

Die Xbox Live Multiplayer-Plattform können Ihr Spiel Xbox Live-Player über das Internet zusammenzuführen und den Lebenszyklus und die Nutzung eines Titels über typische Solo Play erheblich erweitern.

Erstellen einer überzeugenden Multiplayer-Umgebung für Ihre Zielgruppe, können Sie die große soziale Netzwerk Xbox Live Spieler die Benutzerbasis, die für Ihr Spiel zu erhöhen, und Stufen eine dauerhafte Community von dedizierten Fans zusammen spielen nutzen.


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>Was ist die Xbox Live Multiplayer-Plattform?

Die Xbox Live Multiplayer-Plattform ist ein Satz von Client-APIs, die Sie verwenden können, um in Echtzeit Multiplayer-Spiele zu implementieren. Die wichtigsten Subsysteme in der API-Suite sind:

-   Die **Xbox Live Multiplayer Sitzung Verzeichnis (MPSD)** Service. Der Dienst MPSD arbeitet mit integrierte Benutzeroberflächen, um Benutzern das Suchen und einladen von anderen für die Wiedergabe zu ermöglichen. Integration in Xbox Live-Dienste kann Kunden die Xbox Live-Chat Partei verwenden, um zusammenzustellen.
-   **Vermittlung von einfachen und erweiterten Funktionen.** Xbox Live stellt herkömmliche Quickmatch-Funktionen, aber auch Durchsuchen von Sitzungen und die Unterstützung für stark angepasste Vermittlung Szenarien bereit. Xbox Live für die Gruppe (LFG) Suche ermöglicht auch die Spieler, um miteinander zu suchen, in die Partei Chat rally und Spielen Sie Ihr Spiel.
-   **Peer-zu-Peer und dem Client / Server-Netzwerk-APIs** Bereitstellen sicherer Kommunikation in Echtzeit moderner Internetstandards nutzen und von Xbox Live aktiv überwacht. Standardisierung und Integration mit dem Xbox Live-Netzwerk, die zur Problembehandlung Erfahrungen können Benutzer schnell Konnektivitätsprobleme zu beheben.  
-   **Integrierte Sprache und Text-chat Lösungen** sicher in-Game-Kommunikation nutzen, die soziale Diagramm Xbox Live, Media Services und Spezialhardware Codierung für Xbox One-Geräte erleichtert.

Eine Übersicht über einige der am häufigsten verwendeten Multiplayer-Szenarien und die Xbox Live-Funktionalität helfen kann, denen diese Szenarien implementiert, finden Sie unter [Multiplayer-Ebenen](multiplayer-scenarios.md).

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>Werden Xbox Live Multiplayer-Spiele können wie in meinem Spiel implementiert?
Je nach Szenario bietet die Xbox Live Multiplayer-Plattform mehrere Methoden zum Implementieren der Xbox Live Multiplayer-Spiele in Ihrem Spiel.

### <a name="xbox-integrated-multiplayer-xim"></a>Xbox Integrated Multiplayer (XIM)
XIM ist eine schlüsselfertige Lösung für Ihr Spiel über die Leistungsfähigkeit von Xbox Live-Dienste in Echtzeit Multiplayer-Netzwerk und Kommunikation hinzugefügt. Das Ziel der XIM ist, es einfacher denn je, qualitativ hochwertige Peer-zu-Peer (P2P) Multiplayer-Spiele für Xbox One und Windows 10 zu erstellen.

XIM bietet die folgenden Funktionen:
- Unterstützung für Gamecontroller Einladungen und einfache Vermittlung.
- Eine einfache und sichere in Echtzeit Netzwerk, das dem Xbox Live Multiplayer-Relaydienst transparent erweitert wird.
- Einfache Sprache und Text-Chat, mit Funktionen zum Transkribieren und Erzählung zu Kommunikation für einen Zugriff auf Endbenutzer-Erfahrung.
- Hilfen für erkennen und Verwalten von Überlastung des Netzwerks sowie für die Migration Spielzustands.

XIM ist die einfachste Multiplayer-Lösung, die über die Xbox Live Multiplayer-Plattform, aber auch das am wenigsten anpassbare verfügbar und eignet sich nur für P2P-Spiele. Weitere Informationen zu XIM, finden Sie unter [Xbox integrierte Multiplayer](xbox-integrated-multiplayer.md).

### <a name="xbox-multiplayer-manager"></a>Xbox-Multiplayer-Manager
Xbox-Multiplayer-Manager (MPM) handelt es sich um eine Client-API, flexiblen Zugriff auf Xbox Live-Multiplayer-Sitzungsverzeichnis, Einladung und Vermittlung Dienste bereitstellt.

Viele allgemeine Multiplayer-Szenarien implementiert auf effiziente Weise, die bewährte Methoden befolgt, und auch verarbeitet einen Großteil der Xbox-Anforderungen (XRs), die Ihr Spiel implementieren muss, um zertifiziert wurde.

Xbox-Multiplayer-Manager implementiert kein Netzwerk- oder Chat-Ebene. MPM dient als eine flexible, aber vereinfachte und konsolidierte Multiplayer-Verwaltungs-API für Ihr Spiel zusammen mit einem sicheren Netzwerkebene über Windows.Networking.XboxLive implementiert. In-Game-Kommunikation kann hinzugefügt werden, mit der [Spiel, Chat 2](chat/game-chat-2-overview.md) API oder über XIM Chat Reservierungen. Der Netzwerke und Spiele Chat-2-APIs sind in der Xbox One XDK-und das Xbox Live-Plattform Extensions-SDK dokumentiert.

Wenn Sie dedizierte Server für Ihr Spiel Multiplayer-hosten, ist MPM die beste Wahl. Es ist auch MPM eignet sich ideal für Szenarios wie die Integration mit Xbox Live Turniere. Weitere Informationen zu MPM, finden Sie unter [Einführung in Multiplayer-Manager](multiplayer-manager/multiplayer-manager-api-overview.md).

Um Multiplayer-Manager zu verwenden, müssen Sie die Xbox Live-Dienst für Ihre Multiplayer-Szenarien konfigurieren. Weitere Informationen zu dieser Konfiguration finden Sie unter [konfigurieren Sie den Dienst Multiplayer-](service-configuration/configure-the-multiplayer-service.md).

>Multiplayer-Manager unterstützt das Durchsuchen von Sitzungen derzeit nicht. Weitere Informationen finden Sie unter [Multiplayer-Sitzung Durchsuchen](session-browse.md).

### <a name="api-capabilites"></a>API-Funktionen

Funktion | Xbox-integrierten Multiplayer-Spiele| Multiplayer-Manager
--  | -- | --
Sichtbarkeit |  XIM wird als kompilierte Bibliothek ohne Quelle bereitgestellt.  | MPM wird mit Quelle bereitgestellt, damit Sie Verhalten durch direkte Interaktion mit Xbox Live-Dienste oder XSAPI anpassen können.
Sitzung und Vermittlung | XIM einfache vorkonfiguriert, dass Vermittlung Regeln bereitstellt, und erfordert keine Multiplayer-Konfiguration. | MPM [erfordert das Konfigurieren des Multiplayer-Diensts](service-configuration/configure-the-multiplayer-service.md), wodurch anspruchsvolle Spezifikation der Vermittlung und der angegebenen Sitzung Verhalten.
Netzwerk | XIM bietet eine einfache und sichere Player auf spielernetzwerk, vom Xbox Live-Relay-Dienst im Bedarfsfall. | MPM wurde entwickelt, damit Sie Ihre eigene sichere Netzwerke-Lösung, die mithilfe von Windows.Networking.XboxLive Kommunikationsstapel verbinden können.
Spiele-chat | XIM bietet integrierte Sprache und Text-Chat. | In-Game-Kommunikation kann mit der Game-Chat-2-API oder XIM mit dem Out-of-Band-Reservierungen für eine MPM Chat zu verwaltet die Liste.
