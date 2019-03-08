---
title: Beginnen Sie mit der Xbox Live als partner
description: Enthält Links, mit denen verwalteten Partner oder ID@Xbox Member beginnen mit der Xbox Live-Entwicklung.
ms.assetid: 69ab75d1-c0e7-4bad-bf8f-5df5cfce13d5
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Partner, ID@Xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1a219ee6f4e1dc80ead893c4e230d70e8caa2400
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659825"
---
# <a name="get-started-with-xbox-live-as-a-managed-partner-or-an-idxbox-developer"></a>Erste Schritte mit der Xbox Live, als verwaltete Partner oder einem ID@Xbox Developer

Dieser Abschnitt enthält erste Schritte mit der Xbox Live als verwaltete Partner oder als ein ID@Xbox Entwickler. Partner und ID-Entwickler können die vollständige Palette von Xbox Live-Features, einschließlich Erfolge, Multiplayer-Spiele und mehr zugreifen.

Verwaltete Partner und ID@Xbox Entwickler können Titel Xbox Live für die universelle Windows-Plattform (UWP) und die Xbox Developer Kit (xdk-Version)-Plattform entwickeln.

Zusätzlich zu dem Inhalt zur Verfügung, gibt es auch zusätzlicher Dokumentation, der für Partner mit einer autorisierten Partner Center-Konto verfügbar ist. Sie können, dass der Inhalt hier zugreifen: [Xbox Live partnerinhalt](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/partner-content).

## <a name="why-should-you-use-xbox-live"></a>Warum sollten Sie mit der Xbox Live verwenden?

Xbox Live stellt ein Array von Funktionen zur Förderung Ihres Spiels und Beibehalten von Spielern beteiligt:

- Xbox Live sozialen Plattform kann Spieler mit Ihren Freunden und sprechen über die Ihr Spiel.
- Xbox Live Erfolge hilft dabei, Ihr Spiel beliebte abrufen, indem Sie Ihr Spiel kostenlose Höherstufung auf Xbox Live soziale Diagramm erhalten, wenn Spieler Erfolge entsperren.
- Xbox Live Leaderboards kann fördern der Interaktion Ihres Spiels, indem Sie Spieler konkurrieren zu besiegen. seine Freunde, und verschieben Sie die Ränge lassen.
- Xbox Live, Multiplayer-Spiele kann Spieler mit Freunden oder Get Spielen mit anderen Spielern im Wettbewerb stehen, oder in Ihrem Spiel kooperieren abgeglichen.
- Xbox Live verbundenen Speicher zur Verfügung stellt Spielstände roaming zwischen Geräten, damit es Spielern ermöglichen problemlos Spiel Status zwischen Xbox One und Windows-PC fortgesetzt werden können.

## <a name="1-choose-a-platform"></a>1. Wählen Sie eine Plattform
Entscheiden Sie zwischen einer von einer Xbox Development Kit (xdk-Version), universelle Windows-Plattform (UWP) oder Cross-Spielen des Spiels ein:

- Xdk-Version-basierte Spiele, die nur auf der Xbox One-Konsole ausgeführt werden
- UWP-Spiele können alle Windows-Plattform, z. B. Windows-PC, Windows Phone oder Xbox One abzielen.
  - Xbox One finden Sie unter [UWP für Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) und insbesondere [Systemressourcen für UWP-apps und Spiele für Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Cross-Play-Spiele sind in der Regel Spiele, die Xbox One und Windows-PCs, die mit der Pfade der xdk-Version und die UWP als Ziel.

## <a name="2-ensure-that-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Stellen Sie sicher, dass Sie einen Titel, der in Partner Center oder XDP erstellt haben
Vor der Anmeldung und Xbox Live-Dienst-Aufrufe können, muss jeder Titel Xbox Live in Partner Center oder die Xbox-Entwickler-Portal (XDP) definiert werden.  [Erstellt ein neues Spiel](create-a-new-title.md) wird gezeigt, wie Sie dies tun.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Führen Sie das entsprechende Handbuch zum Einrichten Ihrer IDE oder Ihrem Spiel-engine
Sie können führen Sie das entsprechende Handbuch Erste Schritte für Ihre Plattform und -Engine und die Grundlagen von Xbox Live während des Vorgangs.

* [Erste Schritte mit Visual Studio für UWP-Spiele](get-started-with-visual-studio-and-uwp.md) wird gezeigt, wie Sie Visual Studio-Projekt mit der Xbox Live-Konfiguration im Partner Center zu verknüpfen.
* [Erste Schritte mit dem Unity für UWP-Spiele](partner-add-xbox-live-to-unity-uwp.md) wird angezeigt, Sie wie zum Erstellen einer neuen Xbox Live Unity-Titel, aktiviert Hinzufügen von Funktionen wie z. B. Bestenlisten zu Ihrem Titel und generieren ein systemeigenes Visual Studio-Projekt.
* [Erste Schritte mit Visual Studio für xdk-Version-basierte Spiele](xdk-developers.md) erfahren Sie, wie ein, um Ihre Installation von Visual Studio-Projekt zu erhalten, wenn Sie einen Xbox One Titel mit der xdk-Version vornehmen.
* [Erste Schritte vornehmen ein Cross-Play-Spiel](get-started-with-cross-play-games.md) wird erläutert, wie Sie ein Produkt, das ist ein Spiel basierend xdk-Version für Xbox One und eine UWP-basierte Spiele für Windows 10-PCs.

## <a name="4-xbox-live-concepts"></a>4. Xbox Live-Konzepte
Nachdem Sie einen Titel erstellt haben, sollten Sie über die Xbox Live-Konzepte machen, die Ihrer Erfahrung mit der Entwicklung von Titeln auswirken:

- [Xbox Live-sandboxes](../xbox-live-sandboxes.md)
- [Xbox Live-Testkonten](../xbox-live-test-accounts.md)
- [Einführung in die Xbox Live-APIs](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. Hinzufügen von Xbox Live-Funktionen

Fügen Sie Ihr Spiel Xbox Live-Funktionen hinzu:

- [Xbox Live sozialen Plattform - Profil, Freunde, Anwesenheit](../social-platform/social-platform.md)
- [Xbox Live-Daten-Plattform: Statistik, Bestenlisten, Erfolge](../data-platform/data-platform.md)
- [Xbox Live-einladen, Vermittlung, Turniere Multiplayer-Plattform:](../multiplayer/multiplayer-intro.md)
- [Xbox Live-Plattform: verbundenen Speicher, Titel Speicher](../storage-platform/storage-platform.md)
- [Kontextsuche](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. Titel des Release

ID@Xbox und verwaltete Partner müssen über einen Zertifizierungsprozess vor dem Freigeben von deren Titel.  Dies ist da die Titel Zugriff auf zusätzliche Features wie z. B. online Multiplayer-Spiele, Erfolge und umfassender vorhanden.  Sobald Sie für die Veröffentlichung der Titels des bereit sind, wenden Sie sich an Ihren Ansprechpartner bei Microsoft Weitere Informationen zum Senden und Version.
