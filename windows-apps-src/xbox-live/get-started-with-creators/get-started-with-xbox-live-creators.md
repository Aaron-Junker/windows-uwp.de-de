---
title: Erste Schritte – Xbox Live Creators-Programm
description: Enthält Links, um Sie beim Einstieg mit dem Xbox Live Creators-Programm zu unterstützen.
ms.assetid: 2a744405-7ee4-42b4-8f36-9916e8c3a530
ms.date: 12/13/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: cce9d34679884d48475b7a7ae0fa8286204a6289
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615875"
---
# <a name="get-started-with-the-xbox-live-creators-program"></a>Erste Schritte – Xbox Live Creators-Programm
 
Das Xbox Live Creators-Programm eignet sich dank dem vereinfachten Zertifizierungsprozess hervorragend für die direkte und schnelle Veröffentlichung Ihrer Spiele auf Xbox One und Windows 10. Eine Konzeptgenehmigung ist nicht erforderlich. Wenn Ihr Spiel Xbox Live integriert und unseren [Standardrichtlinien des Stores](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx) entspricht, sind Sie für die Veröffentlichung bereit. In diesem Artikel werden die erforderlichen Schritte aufgeführt, um Ihr Spiel mit Xbox Live-Integration zu verwenden. 

Spiele im Xbox Live Creators-Programm müssen eine universelle Windows Platform (UWP)-Anwendung sein. Informationen über Xbox One finden Sie unter [UWP auf Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) und speziell [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation). Spiele, die über das Xbox Live Creators-Programm veröffentlicht werden, haben keinen Zugriff auf die Erfolge oder Online-Multiplayer-Dienste. Eine vollständige Liste der unterstützten Dienste finden Sie unter [Übersicht über das Entwickler-Programm: Tabelle der Features](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview#feature-table).

## <a name="1-ensure-you-have-a-title-created-in-partner-center"></a>1. Stellen Sie sicher, dass Sie einen Titel, der in Partner Center erstellt haben
Jeder Titel Xbox Live muss definiert werden, [Partner Center](https://partner.microsoft.com/dashboard) vor der Anmeldung und Xbox Live-Dienst-Aufrufe können.  [Erstellen einen neuen Creator-Titels](create-and-test-a-new-creators-title.md) – hier erfahren Sie, wie das geht.

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. Führen Sie das entsprechende Handbuch zum Einrichten Ihrer IDE oder Ihrem Spiel-engine
Führen Sie die unter „Erste Schritte-Leitfaden” für die Plattform und das Modul aus und lernen Sie die Grundlagen von Xbox Live kennen:

* [Entwickeln Sie einen Titel Entwickler mit Visual Studio](develop-creators-title-with-visual-studio.md) wird gezeigt, wie Sie Visual Studio-Projekt mit der Xbox Live-Konfiguration im Partner Center zu verknüpfen.
* [Entwickeln Sie einen Creators-Titel mit Unity](develop-creators-title-with-unity.md) – hier wird das Erstellen eines neuen Xbox Live Unity-Spiels gezeigt, das Behandeln der Anmeldung für einzelne Benutzer und mehrerer Benutzer, das Hinzufügen von Features wie Ranglisten und Statistiken und das Generieren eines nativen Visual Studio-Projekts.

Zwar ist Unity die einzige Drittanbieter-Spielengine, für die wir eine Dokumentation bereitstellen, doch [Construct (2 und 3)](https://www.scirra.com/construct2) und [Game Maker Studio](https://www.yoyogames.com/gamemaker) verfügen auch über die Dokumentation über Xbox Live für Spiele in Construct oder Game Maker Studio.

* [Die Game Maker Studio 2-UWP unterstützt jetzt das Xbox Live Creators-Programm](https://www.yoyogames.com/gamemaker/xblc) – hier erfahren Sie, wie Sie Ihr Game Maker Studio-Projekt auf Xbox One und Windows 10-PC Spiele exportieren.
* [Verwenden von Xbox Live in UWP-Apps – Construct](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps) – hier wird gezeigt, wie Sie Xbox Live in Construct 2 und 3-Spielen verwenden.

Für andere Spieleentwicklungsengines ohne Dokumentation über eine Xbox Live-Integration, können Sie die Xbox Live-APIs verwenden, um Ihrem Titel Xbox Live hinzuzufügen. Um die Xbox Live-API aus Ihrem Projekt zu verwenden, können Sie Verweise auf die Binärdateien mit NuGet-Paketen oder die API-Quelle hinzufügen. Das Hinzufügen von NuGet-Paketen kann die Kompilierung erleichtern, während das Hinzufügen der Quelle das Debuggen vereinfacht.

Support von Xbox Live-Diensten für die Verwendung von Drittanbieter-Spielengines, die nicht Unity sind, finden Sie beim entsprechenden Spielengine-Team, das Ihre Fragen beantwortet.

## <a name="3-xbox-live-concepts--testing"></a>3. Konzepte von Xbox Live & testen
Wenn Sie einen Titel erstellt haben, sollten Sie mehr über die Xbox Live-Konzepte erfahren, die sich auf Ihre Erfahrung bei der Entwicklung von Titeln auswirken. Es ist auch wichtig, dass Spiel auf allen Plattformen zu testen, die unterstützt werden, um sicherzustellen, dass es sich wie erwartet verhält.

- [Xbox Live-Dienstkonfiguration für das Creators-Programm](xbox-live-service-configuration-creators.md)
- [Xbox Live-testumgebung](../xbox-live-sandboxes.md)
- [Autorisieren von Xbox Live-Konten](authorize-xbox-live-accounts.md)

## <a name="4-enable-xbox-live-sign-in"></a>4. Xbox Live-Anmeldung aktivieren
Alle Xbox Live Creators-Programm-Spiele müssen mit Xbox Live-Anmeldung integriert werden und die Identität des Benutzers (Gamertag, Spielerbild usw.) anzeigen. Sie können sich auch automatisch als Benutzer anmelden oder eine Taste drücken, um dies zu initiieren. Der Gamertag muss nach der Anmeldung angezeigt werden, damit der Spieler überprüfen kann, dass er das richtige Profil verwendet.

- [Xbox Live sozialen Plattform - Profil, Freunde, Anwesenheit](../social-platform/social-platform.md)

## <a name="5-add-optional-xbox-live-features"></a>5. Hinzufügen von optionalen Xbox Live-Funktionen

Das Xbox Live Creators-Programm bietet eine Reihe von Features, die entwickelt wurden, um Werbung für Ihr Spiel zu ermöglichen und die Spieler einzubinden:

- [Xbox Live-Daten-Plattform – Statistiken, Bestenlisten](../data-platform/data-platform.md) – hilft Ihnen dabei, die Aufmerksamkeit des Spiels zu erhöhen, damit Spieler gegen ihre Freunde gewinnen und den Rang zu erhöhen.
- [Xbox Live-Speicherplattform – Onlinespeicher, Titelspeicher](../storage-platform/storage-platform.md) bietet kostenloses Speichern des Spielroamings zwischen Geräten, damit Spieler ganz leicht den Spielstatus zwischen Xbox One und Windows-PC fortsetzen können.
- [Xbox Live soziale Plattform – Profil, Freunde, Anwesenheit](../social-platform/social-platform.md) – hilft Spielern, mit Freunden in Kontakt zu treten und über das Spiel zu sprechen.

Es ist wichtig zu beachten, dass das Xbox Live Creators-Programm keine Online-Multiplayer, Erfolge und Gamerscore unterstützt.

## <a name="6-release-your-game"></a>6. Lassen Sie Ihr Spiel

Wenn Sie das Xbox Live Creators-Programm verwenden, wird der Prozess genauso wie jede andere UWP-Anwendung veröffentlicht:

- [Microsoft Store-Richtlinien](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx), einschließlich [Spiele und Xbox-Richtlinien](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_13)
- [Veröffentlichen von Windows-apps](https://developer.microsoft.com/en-us/store/publish-apps)