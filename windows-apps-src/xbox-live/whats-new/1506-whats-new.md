---
title: Neuerungen für die Xbox Live SDK – Juni 2015
description: Neuerungen für die Xbox Live SDK – Juni 2015
ms.assetid: 354bcd47-2564-4dd5-89e3-242bca462b35
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a42d0fb0a3cb457a60a0542bfc5966893d00f18b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627875"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2015"></a>Neuerungen für die Xbox Live SDK – Juni 2015

Der Juni-Ausgabe des Xbox Live SDK umfasst die folgenden Updates:

## <a name="os-and-tool-support"></a>Unterstützung für Betriebssystem und Tools ##
Das Xbox Live-SDK unterstützt jetzt den neuesten Insider-Build von Windows 10 und Visual Studio 2015 RC.

## <a name="title-callable-ui-apis"></a>Titel aufrufbare UI-APIs

| Hinweis |
|------|
| Dieser Abschnitt gilt nur für UWP-Titel, xdk-Version Titel sollte finden Sie auf die Klasse Windows.Xbox.UI.SystemUI TCUI  |

Das Xbox Live SDK enthält jetzt Wrapper-APIs, die Titel aufrufbare UI (TCUI) für die Anzeige von Stock UI auf einem Windows 10-PC/Mobile-Gerät zu unterstützen.

Diese APIs sind nur für UWP-apps verfügbar.

Sie können die TCUI-Wrapper aus der TitleCallableUI (WinRT) und die Title_callable_ui (C++)-Klasse aufrufen.

Die Aktie umfassen Benutzeroberflächen:
* Ein Player-Auswahl-UI
* Ein Spiel Einladung-Auswahl-UI
* Ein Player Profilkarte UI
* Ein hinzufügen/entfernen Friend-Benutzeroberfläche
* Ein anzeigen Titel des Spiels Erfolge UI

Finden Sie im neuen *TCUI* Beispiel z. B. die Verwendung der neuen APIs. Sie finden das Beispiel unter {*SDK Quellstamm*} \Samples\TCUI.

## <a name="new-authentication-model-for-uwp-apps"></a>Neues Authentifizierungsmodell für UWP-apps
Das Xbox Live-SDK unterstützt jetzt mit einem Microsoft-Konto (MSA) zum Identifizieren des Xbox Live-Players auf einem Windows 10-PC/Mobile-Gerät.

Sie können jetzt in einem vom Benutzer anmelden, mit dem XboxLiveUser (WinRT) und Xbox_live_user (C++)-Klassen.

## <a name="new-api-for-writing-events-in-uwp-apps"></a>Neue API für das Schreiben von Ereignissen in UWP-apps

| Hinweis |
|------|
| Dieser Abschnitt gilt nur für UWP-Titel verwendet werden.  Entwickler von xdk-Version finden Sie in der [Game-Events](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/game-events) xdk-Version bestimmte Informationen  |

Die neue EventsService (WinRT) und die Events_service (C++)-Klassen können Sie die in-Game-Ereignisse zu schreiben, die Benutzer-Statistiken, Erfolge, Bestenlisten usw. aktualisiert werden können. Diese neuen Klassen sind für nur für UWP-apps.

## <a name="breaking-change-to-event-handlers"></a>Wichtige Änderung an Ereignishandler ##
Alle Ereignishandler in der C++-SDK von einem einzigen geändert wurden `void set_*_handler()` Methode, um ein Paar von `function_context add_*_handler()` und `void remove_*_handler(function_context context)` Methoden.

Jede `add_*_handler()` Methode gibt eine `function_context` Objekt. Wenn Sie den Ereignishandler zu entfernen, müssen Sie übergeben, der `function_context` Objekt.

Zum Beispiel:
```
function_context subscriptionLostContext = xbox_live_context()->multiplayer_service().add_multiplayer_subscription_lost_handler(...);

localUser->xbox_live_context()->multiplayer_service().remove_multiplayer_subscription_lost_handler(subscriptionLostContext);
```

## <a name="new-apis-for-managing-multiplayer-scenarios"></a>Neue APIs für die Verwaltung von multiplayer-Szenarien
Das Xbox Live SDK enthält jetzt einen Satz von C++-APIs für die Verwaltung von allgemeinen multiplayer-Szenarien. Die APIs sind in den xbox.services.experimental.multiplayer-Namespace enthalten.

Diese APIs sind in der experimentellen-Namespace der bedeutet, die dass sie wahrscheinlich ändern, sind abhängig von Feedback.  Wir empfehlen Entwicklern, die sie verwenden und Feedback geben.

Finden Sie im neuen *MultiplayerManager* Musterverwendung z. B. zu diesen neuen APIs. Sie finden das Beispiel unter {*SDK Quellstamm*} \Samples\MultiplayerManager.
