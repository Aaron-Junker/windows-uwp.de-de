---
title: Aktivitätsüberwachung in Echtzeit-Dienst
description: Erfahren Sie, bis des Diensts für die Xbox Live-Real-Time-Aktivität.
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Aktivität Real-Time-Dienst.
ms.localizationpriority: medium
ms.openlocfilehash: 36389fac3bd6dea2d2e24c0935087781118d8046
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592085"
---
# <a name="real-time-activity-service"></a>Aktivitätsüberwachung in Echtzeit-Dienst

Der Real-Time-Aktivität (RTA)-Dienst kann eine Anwendung auf jedem Gerät Daten, Statistiken und Vorhandensein abonnieren. Das System ermöglicht Abonnements aus, um eigene Daten und anderen Daten in jeder Titel basierend auf ihre datenschutzeinstellungen. Dies ermöglicht einen Fluss von Informationen, ohne ständig abruft, um die neuesten Daten abzurufen.


## <a name="developer-scenarios"></a>Entwicklerszenarios

Es gibt viele Szenarien, die RTA unterstützt. Nur einige davon sind hier aufgeführt, aber die wirkliche Stärke der RTA ist, die viele Szenarios, denen Sie mit gestartet werden, dass wir noch gestaltete noch nicht. Sie können die nächste Generation von Gaming definieren, in denen Benutzer häufig an über ein Microsoft Surface-oder Apple iPad ausgeführt werden muss, während sie mit dem Konsolentitel interagieren. RTA verwendet WebSocket-Technologie, damit die verschiedenen Unterthema exemplarischen Vorgehensweisen enthält eine Übersicht über die Implementierung, die mithilfe der WebSockets-API von Windows bereitgestellt.

Im folgenden finden einige einfachen Szenarien, die Sie für den Titel des RTA mit erstellen können:

-   Erfolge, Fortschritt-app
-   Spiele-Hilfe-app
-   Team-Viewer-app
-   Statistiken-viewer
-   Präsenz-Viewer


## <a name="achievements-progress-app"></a>Erfolge, Fortschritt-app

Benutzer sind fast immer wissen, was ihre Fortschritte im Hinblick auf bestimmte Erfolge, insbesondere die Leistungen, die erfordern eine Aktion eine bestimmte Anzahl von Malen. Mit in Echtzeit Zugriff eines Benutzers-Statistiken (dies im Xbox Live Player Statistics-Dienst aggregiert werden), können Sie echtzeitfortschritts für Spieler und seine Freunde Erfolge und Meilensteine, die auf der Xbox One oder auf ein begleitgerät, während die Benutzer darstellen. Es werden Ihre Titel wiedergegeben werden.


## <a name="game-help-app"></a>Spiele-Hilfe-app

Wie Benutzer durch den Titel navigieren, kann Zugriff auf Daten in Echtzeit Sie Spiele Hilfe neben der Umgebung auf der Xbox One oder auf jedem Gerät Companion bieten. Benutzer können jedoch auch eine neue Zuordnung, neue nachverfolgen oder eine Herausforderung Chef Kampf, und Ihr Spiel Hilfe-Benutzerhandbuch benutzergeneriertem oder Entwickler generierte Videos und Text, der den Benutzer über die Benutzeroberfläche in Ihre Titel anzeigen.


## <a name="squad-viewer-app"></a>Team-Viewer-app

In der Co-op-Multiplayer-Spiele gemeinsam einen Spieler und seine Teamkollegen für eine gemeinsame Ziel verfolgen. Mit so vielen Playern kann es schwierig, sich ein besseres Bild von sein. Zugriff auf Daten in Echtzeit können Sie eine Begleit-app erstellen, die zeigt, die auf hoher Ebene Map und Heat Maps, in dem die Aktion sein kann.


## <a name="statistics-viewer"></a>Statistiken-viewer

Obwohl es typisch, bei der Betrachtung RTA Begleit-apps zu betrachten ist, können Sie auch RTA in der Titelleiste Ihres Core. RTA können Sie z. B. einen Player einen Multiplayer-Spiels eine Anzeige der aktuellen Statistiken innerhalb des Spiels, z. B. durch den Player, drücken einfach die Schaltfläche "Ansicht" auf dem Controller bei der Multiplayer-Übereinstimmung bereit.


## <a name="presence-viewer"></a>Präsenz-Viewer

In einem jedoch auch festlegen, ist es sinnvoll, eine aktuelle Ansicht die Freunde online sind und wenn sie denselben Titel wiedergegeben werden. Sie können abonnieren Freunde eines Benutzers vorhanden und anzeigen, welche Freunde stammen online, wenn sie Titel, komplett in Echtzeit experimentieren.


## <a name="subscription-privacy-and-authorization"></a>Abonnement-Datenschutz und -Autorisierung

Die neueste Version von RTA enthält Überprüfungen für Datenschutz und die Autorisierung/Content-Isolation. Solange Datenschutz und autorisierungsüberprüfungen aufweisen erfüllt sind, kann Ihre app alle Statistiken abonnieren, die als RTA-fähigen markiert ist. (Weitere Informationen zum Stellen einer Statistik RTA-fähig ist, finden Sie unter [registrieren für Benachrichtigungen von Stat](register-for-stat-notifications.md). Es gibt keine Einschränkungen hinsichtlich der Arten von Statistiken, die RTA-fähigen vorgenommen werden können, es liegt an Ihnen als Entwickler. Es ist jedoch begrenzt die *Anzahl* von Statistiken, die ein Benutzer pro app-Sitzung abonnieren kann. Wenn ein Benutzer diesen Grenzwert erreicht, wird er oder sie Fehler bei der nächsten Abonnements erhalten.


## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[Registrieren Sie sich für den Player-Stat-änderungsbenachrichtigungen](register-for-stat-notifications.md)  
Beschreibt, wie Real-Time-Aktivität (RTA) für Statistiken oder Statusinformationen zu aktivieren.

[Programmierung des Real-Time-Aktivität (rta)-Diensts mithilfe von Winrt-apis](programming-the-real-time-activity-service.md)  
Beschreibt, wie den Real-Time-Aktivität (RTA)-Dienst mithilfe von WinRT-APIs programmiert.

[Programmierung des Real-Time-Aktivität (rta)-Diensts mithilfe der restful-Schnittstelle](programming-the-real-time-activity-service.md)  
Beschreibt, wie den Real-Time-Aktivität (RTA)-Dienst mithilfe der RESTful-Schnittstelle zu programmieren.

[Real-Time-Aktivität (rta) bewährte Methoden](rta-best-practices.md)  
Bewährte Methoden für die Verwendung der Xbox-Services von Echtzeit-Aktivität (RTA) zum Abonnieren von Statistiken und Statusdaten aus dem Xbox-Datenplattform.
