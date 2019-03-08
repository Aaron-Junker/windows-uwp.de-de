---
title: Bewährte Methoden für offline
description: Erfahren Sie mehr über bewährten Methoden für die Behandlung von offline-Szenarien mit Titeln Xbox Live aktiviert.
ms.assetid: 6290dd67-1145-4fe2-8ada-c3a29a9ad29a
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, offline
ms.localizationpriority: medium
ms.openlocfilehash: 5680b045873ebf6eed1422cb915f3f53df748790
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618725"
---
# <a name="best-practices-for-offline"></a>Bewährte Methoden für offline

Xbox One dient als verbundenes Gerät, und die beste Benutzeroberflächen, wie z. B. multiplayer-Spiele und Streamen von Videos, erfordern Konnektivität. Allerdings unterstützt Xbox One viele Formen von offline-Wiedergabe. In diesem Thema wird erläutert, wie offline Play behandelt wird.

## <a name="overview"></a>Übersicht

Eine ständig wachsende Anzahl von Kunden weltweit haben Zugriff auf das Internet. Es gibt jedoch noch stellen in der ganzen Welt, in denen Konnektivität ist unvorhersehbar, und es gibt Situationen, wenn die Router fehlschlägt, Fiber cut ist, Server stürzt ab oder Drahtlosdienste löschen.

Um den umfassendsten Satz von Kunden und Erfahrungen zu unterstützen, können für allgemeine Fälle, in dem Internetkonnektivität von Zeit zu Zeit verloren gegangen ist oder nicht verfügbar ist vollständig, Xbox One. Ihr Spiel wird darüber informiert, von Verbindungsfehlern, und Sie, wie Sie reagieren möchten –, ob die, die vollständige Gameplay fortgesetzt wird, Downgrades auf einer offline-Modus oder Spiel vollständig zu beenden.

## <a name="normal-online-operation"></a>Normale Onlinevorgang

Im Allgemeinen arbeiten Xbox One-Konsolen, vollständig verbunden sind, in denen der Benutzer eine stetige Internetverbindung und einen vollständigen Zugriff auf Xbox Live und Diensten von Drittanbietern verfügt. Dieser verbundenen Zustand ermöglicht, Xbox Live-Dienst, um die Verwaltungskonsole, Status, Updates bereitstellen und anderen Diensten im Hintergrund auszuführen, die Ihr Spiel und die Benutzer profitieren in regelmäßigen Abständen zu überprüfen.

Es wird empfohlen, dass Sie davon ausgehen, dass Benutzer onlineverbindung den Großteil der Zeit.

## <a name="offline-play-principles"></a>Offline-Wiedergabe-Prinzipien

Es gibt Fälle, in denen eine online-Verbindung nicht verfügbar ist. Für die offline-Wiedergabe Xbox One mit den folgenden Prinzipien Bedenken dient:

-   Besonders darauf, dass Benutzer, die trotz Konnektivitätsprobleme zu spielen.

-   Benutzer weiter spielen werden, auch wenn sie keine Verbindung zu haben.

-   Stellen Sie offline Play einfachen und vorhersagbaren für Benutzer, während gleichzeitig die Intention des Anwendungsperformance immer verbunden.

## <a name="offline-modes"></a>Offlinemodus

Es gibt zwei allgemeine *Verlust der Konnektivität* Szenarien:

-   Vollständige dienstunterbrechung Internet

-   Verlust von ein oder mehrere online-Dienste

In jedem dieser Modi gibt es eine Vielzahl von Situationen, die auftreten können. Diese werden unten mit Beispielen für die allgemeinen offline-Szenarien erläutert, die sich Spiele auswirken.

## <a name="offline-scenario-no-internet-service-upon-game-start"></a>Offlineszenario: keine Internet-Dienst beim Spiel starten

Spiele können sich als einer der drei Typen deklarieren:

-   *Xbox Live erforderlich*: Alle Modi der Gameplay erfordert eine Internetverbindung.

-   *Xbox Live Gold erforderlich*: allen Gaming-Modi müssen über Internetkonnektivität sowie über eine Xbox Live Gold-Mitgliedschaft.

-   *Xbox Live nicht erforderlich,*: das Spiel weist mindestens einen Modus von Play, die keine Internetverbindung erforderlich. Technisch gesehen ist dieser Typ nicht explizit im Anwendungsmanifest deklariert. Eine app, die nicht selbst deklariert, wie einer der ersten beiden Typen "Xbox Live nicht erforderlich," betrachtet wird oder offline unterstützt.

Wenn ein Spiel, die vom Benutzer gestartet wird, und die Konsole offline ist, überprüft das System die Deklaration der game-Konnektivität im Manifest Anwendung. Wenn das Spiel Konnektivität (entweder von der ersten beiden oben genannten Fälle) erforderlich ist, wird das System automatisch zeigt eine Meldung an den Benutzer und den Titel wird nicht gestartet.

Wenn die Konsole offline ist, startet das System nur Titel, die mindestens ein Modus von Play verfügen, die keine Verbindung erfordert. Das heißt, startet das System nicht "Xbox Live erforderlich" oder "Xbox Live Gold-required" Titel.

## <a name="offline-scenario-connectivity-lost-during-gameplay"></a>Offlineszenario: Konnektivität verloren, während der Spiele

Wenn Sie ein Spiel bereits ausgeführt wird, wenn eine Verbindung unterbrochen ist, werden vom System der Titel benachrichtigt. Wenn das Spiel nicht online-Dienste verwendet wird, die Sitzung ohne Unterbrechung fortgesetzt werden. Wenn das Spiel Onlinedienste aktiv verwendet wird, wechseln Sie in einem Modus, in denen diese Dienste nicht mehr benötigt, oder informieren Sie dem Spieler, dass die Gaming-Sitzung aufgrund des offline-Status beendet wird.

Titel, die deklariert werden, dass "Xbox Live-required" oder "Xbox Live Gold erforderlich" automatisch angehalten wird durch das System, wenn die Konsole alle Netzwerkverbindungen für eine bestimmte Zeitspanne verliert, und das System stellt automatisch eine Fehlermeldung an dem Benutzer bereit.

Mit allen anderen Szenarien im Zusammenhang mit Gameplay anhalten, Zustand speichern sollten, damit der Benutzer keine Daten verloren gehen und schnell können auf diesen Zustand nach dem Verbindung wieder zurück.

## <a name="offline-scenario-when-a-single-xbox-live-service-is-down"></a>Offlineszenario: Wenn ein einzelner Xbox Live-Dienst ist ausgefallen

Es gibt andere Situationen, in denen bestimmte online-Dienste möglicherweise nicht verfügbar, obwohl Internetkonnektivität in Ordnung ist.

Ein einzelner Xbox Live-Dienst kann z. B. für einen kurzen Zeitraum offline sein. In diesem Fall wird der Aufruf an einen bestimmten service erfolgt ein Timeout oder meldet einen Fehler, um das Spiel. Sie sollte-Dienst, der offline-Status richtig verarbeiten, so wie Sie Situationen, die auf der Xbox 360 oder auf Windows behandeln.

Das Spiel muss zumindest nicht abstürzt oder aufhängen führt. Wenn Gameplay ohne dass der Dienst kann nicht fortgesetzt werden, dann melden Sie dies für den Benutzer und ermöglicht dem Benutzer, die in einem anderen Bereich des Spiels fortfahren, in dem der Dienst nicht erforderlich ist.

Im besten Fall weiterhin Spiel und die Cache-Daten, die später (wenn das Spiel beim Schreiben in den Dienst) senden, oder angemessene Annahmen über die Daten zu gestalten, (wenn das Spiel aus dem Dienst Lesen wurde).

## <a name="offline-scenario-when-a-third-party-service-is-down"></a>Offlineszenario: Wenn ein Drittanbieter-Dienst ist nicht verfügbar

Wenn Ihr Spiel auf einem Drittanbieter-online-Dienst basiert, muss Ihr Spiel auch sein kann, wenn dieser Dienst nicht. Wenn der Dienst nicht, klicken Sie dann Ihr Spiel muss nicht abstürzt oder aufhängen führt. Sie können den Dienstfehler für den Benutzer melden, wenn Spiel nicht fortgesetzt werden kann. Im Idealfall Gameplay sollten weiterhin, oder Sie sollten in einem Bereich des Spiels fortgesetzt werden, die den Onlinedienst erfordern nicht zulassen.

## <a name="offline-scenario-when-xbox-live-cloud-compute-is-down"></a>Offlineszenario: beim Xbox Live-Cloud-Computing ist nicht verfügbar

Eines der Xbox One stellt ist die Power der Cloud. Einige Spiele können ein Dienst immer verbunden, wie Xbox Live Compute für die Cloud, vollständig hängen die ermöglicht den Zugriff auf Weitere Compute-Funktionen oder stets verfügbare gameservern. Diese immer verbundenen Modus ist sowohl zulässig und wird empfohlen, in dem sie die Erfahrung für Spieler verbessert.

Wenn Ihr Spiel verwendet diesen Modus, es wird empfohlen, Ihr Spiel dienstunterbrechungen ausfallsicher sein muss (mehrere Sekunden und bis zu eine minute), die aufgrund von entweder insgesamt Verlust der Internetverbindung oder eine bestimmte Cloud-Diensts sind. Das Spiel ist jedoch nicht erforderlich, um eine offline-Modus zu erhalten. Wenn das Spiel benötigt wirklich eine Verbindung und Konnektivität nicht verfügbar ist, die informieren Sie Benutzer darüber, und beenden Sie die Gaming-Sitzung.

## <a name="xbox-requirements"></a>Xbox-Anforderungen

Die wichtigste Anforderung beim Umgang mit offline-Szenarien ist Spiel Stabilität. Ob es vollständig getrennt oder nur den Verlust eines bestimmten Diensts für die online ist, muss Ihr Spiel nicht hängen, stürzt ab, oder dazu führen, dass der Benutzer Zustand verloren gehen. Ihr Spiel muss ein robustes System für die Behandlung von Szenarien für Netzwerk-Timeout und zur Behandlung von Fehlern, die von jeder API zurückgegeben, die auf einen online-Dienst zugreift.

Spiele sind nicht erforderlich, um offline Play zu unterstützen. Wenn Ihr Spiel einfach aufgrund einer Unterbrechung der dienstkonnektivität fortgesetzt werden kann, klicken Sie dann Benutzer benachrichtigen, die Gaming-Sitzung zu beenden und zurück zu den im Hauptmenü oder interaktiven Ausgangszustand.

## <a name="best-practices"></a>Empfohlene Methoden

Hier finden bewährte Methoden für den Umgang mit offline Situationen:

-   Entwerfen Sie Spiele davon ausgehen, dass die Benutzer-onlineverbindung den Großteil der Zeit haben.

-   Wenn es für Ihre Spiele sinnvoll ist, sollten Sie entwerfen Modi Gaming, mit denen die Benutzer eine angenehme Erfahrung verfügen, auch wenn die Konsole in einem Offlinestatus ist.

-   Dienste können nicht mehr verfügbar sind. Verbindungen können Fehler auftreten. Erstellen Sie stabile Fehlerbehandlung für APIs, die Timeouts können fehlerbedingungen Bericht, wenn ein Onlinedienst ist nicht verfügbar oder wenn Internet unterbrochen wird. Wann immer möglich, lassen Sie Benutzer, die trotz dieser Probleme.

-   Befolgen Sie die Xbox-Anforderungen (XRs). Keine hängt oder abstürzt.

-   Speichern Sie nach dem Empfang einer PLM Titel Unterbrechung-benachrichtigungs, Status, damit der Benutzer keine Daten verloren gehen und schnell auf diesen Zustand zurückkehren kann, wenn das Spiel fortsetzen.

-   Flag zum Titel des entsprechend im Manifest Anwendung. Nur markieren Sie den Titel wie "Xbox Live-required", wenn alle Modi von Gaming-Verbindungen erfordern.

-   Xbox One Spiele sind zulässig und online-Dienste in allen Spielmodi abhängig. Wenn das Spiel einfach aufgrund einer Unterbrechung der dienstkonnektivität fortgesetzt werden kann, klicken Sie dann Benutzer benachrichtigen, die Gaming-Sitzung zu beenden und zurück zu den im Hauptmenü oder interaktiven Ausgangszustand.

-   Verlassen Sie sich nicht auf der Xbox-Hilfe-Dienst Fehlermeldungen vorliegen und Hilfe im Zusammenhang mit offline-Status. Der Xbox-Hilfe-Dienst erfordert eine Verbindung zu Xbox Live.

## <a name="conclusion"></a>Abschluss

Xbox One ist für eine immer vernetzten Welt konzipiert. Allerdings ermöglicht der Konsole auf dem Weg zu dieser Vision, Spieler, um Offlineszenarios zu nutzen. Spiele muss stabil gegenüber Verbindungsfehlern. Können Sie für Spiele mit offline-Wiedergabe-Erfahrungen Ihre Spieler, um diese Funktionen optimal zu nutzen. Für Spiele, die immer online sind, wenn Sie über eine Netzwerkverbindung verloren geht, müssen Sie den Benutzer ordnungsgemäß in den Offlinemodus zurück.
