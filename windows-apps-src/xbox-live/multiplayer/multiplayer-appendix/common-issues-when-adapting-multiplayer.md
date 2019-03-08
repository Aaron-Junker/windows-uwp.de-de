---
title: Häufige Multiplayer-2015-Migrationsprobleme
description: Erfahren Sie etwas über häufige Probleme, die, denen Sie stoßen eventuell bei der Anpassung Ihrer Multiplayer-2014-Titels, 2015 Multiplayer-Spiele.
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a04370ee2f45534c88467700b9523c5a4ad11094
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615815"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>Bekannte Probleme beim Anpassen der Titel des Multiplayer-2014 für mehrere Spieler 2015

Dieses Thema beschreibt die Probleme, die Sie beim Anpassen Ihrer 2014 Multiplayer-Titeln für 2015 Multiplayer-Spiele berücksichtigen müssen.


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>Konfigurationsänderungen an Stellen für 2015 Multiplayer-Spiele

Dieser Abschnitt beschreibt die Änderungen, wenn Sie Ihre Sitzungen und Vorlagen für 2015 Multiplayer-Spiele konfigurieren bewusst sein sollten. Beispiele für die einzelnen Elemente, die erläutert, finden Sie unter [Vorlagen für Ereignissitzungen MPSD](multiplayer-session-directory.md).

### <a name="add-a-capability-for-active-member-connection"></a>Fügen Sie eine Funktion für aktives Mitglied Verbindung hinzu.

Die ConnectionRequiredForActiveMembers-Funktion ermöglicht die Erkennung trennen, und Sitzung ändern, Abonnementfeatures von 2015 Multiplayer-Spiele. Diese Funktion dem /constants/system/capabilities-Objekt für alle Vorlagen für ereignissitzungen hinzufügen.


### <a name="add-a-system-constant-for-invite-protocol"></a>Fügen Sie eine System-Konstante für Einladung-Protokoll hinzu.

Die InviteProtocol-System-Konstante kann den Empfänger eine Einladung, um ein Popup zu erhalten, wenn die Aufrufe des Absenders Titel der **MultiplayerService.SendInvitesAsync Methode** oder **SystemUI.ShowSendGameInvitesAsync Methode (IUser, IMultiplayerSessionReference, String)**. Fügen Sie dieser Konstante hinzu, legen Sie auf ", auf das Objekt /constants/system für alle Vorlagen für Sitzungen, der der Titel Spieler Einladungen, Knacken".


## <a name="runtime-considerations-for-2015-multiplayer"></a>Laufzeitaspekten für 2015 Multiplayer-Spiele

Titel für 2015 muss es sich um Multiplayer-Spiele:   Rufen Sie immer die **MultiplayerService.EnableMultiplayerSubscriptions Methode** vor dem Multiplayer-Bereich des Codes Titel eingeben. Dieser Aufruf können beide Abonnements auf Änderungen der Sitzung, und Trennen von Erkennung.
-   Achten Sie darauf, dass Sie die gleiche **XboxLiveContext Klasse** Objekt für alle Aufrufe vom gleichen Benutzer. Der Kontext enthält den Status, die im Zusammenhang mit der Verwaltung von der Verbindung für mehrere Spieler Abonnements verwendet, und Trennen von Erkennung.
-   Wenn mehrere lokale Benutzer vorhanden sind, verwenden Sie eine Separate **XboxLiveContext** Objekt für jeden Benutzer.


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>Migrieren von einer Sitzungsvorlage von Vertragsversion 104/105, 107

Die aktuelle Sitzung Vertrag Vorlagenversion ist 107, mit einigen Änderungen auf das Schema für MPSD verwendet. Vorlagen, die Sie für die Sitzung Vertrag Vorlagenversion 104/105 geschrieben haben müssen aktualisiert werden, wenn sie erneut veröffentlicht werden, um Version 107 zu verwenden. In diesem Thema werden die Änderungen bei der Migration Ihrer Vorlagen auf die neueste Version zusammengefasst. Die Vorlagen selbst werden in beschrieben [Vorlagen für Ereignissitzungen MPSD](multiplayer-session-directory.md).

| Wichtig                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Funktionen, die über eine Vorlage festgelegt ist kann nicht durch Schreibvorgänge in MPSD geändert werden. Um die Werte ändern zu können, müssen Sie erstellen und übermitteln eine neue Vorlage mit den erforderlichen Änderungen. Alle Elemente, die nicht über eine Vorlage festgelegt werden können durch Schreibvorgänge auf den MPSD geändert werden. |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>Änderungen an der /constants/system/managedInitialization-Objekt

Das Objekt /constants/system/managedInitialization wurde in /constants/system/memberInitialization umbenannt. Hier sind die Änderungen an Name/Wert-Paare für dieses Objekt vornehmen: AutoEvaluate ExternalEvaluation umbenannt wird. Die Polarität Änderungen, es sei denn, "false" weiterhin der Standard.
-   Der Standardwert von MembersNeededToStart ändert sich von 2 auf 1.
-   Der Standardwert von JoinTimeout ändert sich von 5 Sekunden auf 10 Sekunden.
-   Der Standardwert von MeasurementTimeout ändert sich von 5 Sekunden auf 30 Sekunden.


### <a name="changes-to-the-constantssystemtimeouts-object"></a>Änderungen an der /constants/system/timeouts-Objekt

Das Objekt /constants/system/timeouts wird entfernt, und die Timeouts umbenannt und unter /constants/system verschoben werden. Hier sind die Änderungen an Name/Wert-Paare für dieses Objekt vornehmen:   Das reservierte Timeout wird ReservedRemovalTimeout.
-   Das inaktive-Timeout wird InactiveRemovalTimeout. Der neue Standardwert ist 0 (Stunden).
-   Das Timeout bereit wird ReadyRemovalTimeout.
-   Das Timeout SessionEmpty wird SessionEmptyTimeout.

Details von Timeouts werden angezeigt, [Sitzungstimeouts](mpsd-session-details.md).


### <a name="change-to-the-game-play-capability"></a>Ändern Sie für die Spiele-Funktion

Das Spiel spielen Funktion ermöglicht das letzte Spieler und Reputation reporting. Sie müssen jetzt das Feld "/constants/system/capabilities/gameplay" als "true" für Sitzungen angeben, die tatsächliche Spiels, im Gegensatz zu Helper-Sitzungen darstellen, z. B. übereinstimmen und lobby-Sitzungen wird.


## <a name="see-also"></a>Siehe auch

[Vorlagen für Ereignissitzungen MPSD](mpsd-session-details.md)
