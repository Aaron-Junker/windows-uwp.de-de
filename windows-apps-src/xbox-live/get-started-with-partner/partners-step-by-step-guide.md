---
title: Schrittweise Anleitung für Xbox Live-Partner
description: Stellen eine Richtlinie der Schritte zum Integrieren von Xbox Live für verwalteten Partner.
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 9a2d0b9e7d97adfb02281e7bae34ed51afd44f7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660845"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>Schrittweise Anleitung zum Integrieren von Xbox Live für verwaltete Partner und ID@Xbox Mitglieder

In diesem Abschnitt helfen Ihnen die mit Xbox Live einsatzbereit zu machen:

## <a name="1-choose-a-platform"></a>1. Wählen Sie eine Plattform
Entscheiden Sie zwischen einer von einer Xbox Development Kit (xdk-Version), universelle Windows-Plattform (UWP) oder Cross-Spielen des Spiels.

- Xdk-Version-basierte Spiele, die nur auf der Xbox One-Konsole ausgeführt werden
- UWP-Spiele können alle Windows-Plattform, z. B. Windows-PC, Windows Phone oder Xbox One abzielen.
  - Xbox One finden Sie unter [UWP für Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) und insbesondere [Systemressourcen für UWP-apps und Spiele für Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Cross-Play-Spiele sind in der Regel Spiele, die Xbox One und Windows-PCs, die mit der Pfade der xdk-Version und die UWP als Ziel.

## <a name="2-ensure-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Stellen Sie sicher, dass Sie einen Titel, der in Partner Center oder XDP erstellt haben
Vor der Anmeldung und Xbox Live-Dienst-Aufrufe können, muss jeder Titel Xbox Live in Partner Center oder die Xbox-Entwickler-Portal (XDP) definiert werden.  [Erstellt ein neues Spiel](create-a-new-title.md) wird gezeigt, wie Sie dies tun.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Führen Sie das entsprechende Handbuch zum Einrichten Ihrer IDE oder Ihrem Spiel-Engine
Sie können führen Sie das entsprechende Handbuch Erste Schritte für Ihre Plattform und -Engine und die Grundlagen von Xbox Live während des Vorgangs.

* [Erste Schritte mit Visual Studio für UWP-Spiele](get-started-with-visual-studio-and-uwp.md) wird gezeigt, wie Sie Visual Studio-Projekt mit der Xbox Live-Konfiguration im Partner Center zu verknüpfen.

* [Erste Schritte mit dem Unity für UWP-Spiele](partner-add-xbox-live-to-unity-uwp.md) wird angezeigt, Sie wie zum Erstellen einer neuen Xbox Live Unity-Titel, aktiviert Hinzufügen von Funktionen wie z. B. Bestenlisten zu Ihrem Titel und generieren ein systemeigenes Visual Studio-Projekt.

* [Erste Schritte mit Visual Studio für xdk-Version-basierte Spiele](xdk-developers.md) erfahren Sie, wie ein, um Ihre Installation von Visual Studio-Projekt zu erhalten, wenn Sie einen Xbox One Titel mit der xdk-Version vornehmen.

* [Erste Schritte vornehmen ein Cross-Play-Spiel](get-started-with-cross-play-games.md) wird erläutert, wie Sie ein Produkt, das ist ein Spiel basierend xdk-Version für Xbox One und eine UWP-basierte Spiele für Windows 10-PCs.

## <a name="4-xbox-live-concepts"></a>4. Xbox Live-Konzepte
Wenn Sie einen Titel erstellt haben, sollten Sie mehr über die Xbox Live-Konzepte erfahren, die sich auf Ihre Erfahrung bei der Entwicklung von Titeln auswirken.

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

ID@Xbox Titeln und vom Herausgeber freigegeben werden, ist dies ein Microsoft-Partner einen Zertifizierungsprozess vor Version durchlaufen muss.  Dies ist, da diese Titel Zugriff auf zusätzliche Features wie Multiplayer-Spiele, Erfolge und Rich-Präsenz verfügen.  Sobald Sie für die Veröffentlichung der Titels des bereit sind, wenden Sie sich an Ihren Ansprechpartner bei Microsoft, Weitere Informationen zum Senden und Version.
