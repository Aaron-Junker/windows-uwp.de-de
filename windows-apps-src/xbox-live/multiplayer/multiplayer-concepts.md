---
title: Xbox Live-Multiplayer-Konzepte
description: Informationen Sie zu allgemeinen Multiplayer-Konzepte, die von Xbox Live Multiplayer-Systemen verwendet.
ms.assetid: 1e765f19-1530-4464-b5cf-b00259807fd3
ms.date: 08/25/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: 0e2ba32b2eaff757cfa6401952567f6b0163f632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623795"
---
# <a name="xbox-live-multiplayer-concepts"></a>Xbox Live-Multiplayer-Konzepte

Dieses Thema behandelt eine Reihe von wichtige Multiplayer-Begriffe und Konzepte, die häufig in der Dokumentation zu Xbox Live verwendet werden. Müssen vertraut dieser Konzepte hilft Ihnen zu verstehen, wie Xbox Live Multiplayer-funktioniert.

## <a name="multiplayer-session"></a>Multiplayer-Sitzung

Eine Multiplayer-Sitzung stellt eine Gruppe von Xbox Live-Benutzer und zugeordneter Eigenschaften dar. Die Sitzung erstellt und von Titeln, verwaltet und wird dargestellt als sichere JSON-Dokument, das die Xbox Live-Cloud befindet. Das Session-Dokument selbst enthält Informationen über die Xbox Live-Benutzer, die mit der Sitzung, wie viele Stellen zur Verfügung, benutzerdefinierte Metadaten (für die Sitzung auch für jede Sitzung-Element) sind verbunden sind, und andere Informationen in Bezug auf die Spiele-Sitzung.

Jede Sitzung basiert auf einer sitzungsvorlage für, werden durch die Spieleentwickler definiert und in der Xbox Live-Dienstkonfiguration für eine Instanz des Title konfiguriert sind.

Während ein Titels erstellen und aktualisieren eine Sitzung kann, kann nicht direkt eine-Sitzung gelöscht werden.  Stattdessen, nachdem alle Spieler in einer Sitzung entfernt wurden, wird die Xbox Live Multiplayer-Dienst automatisch die Sitzung nach einem festgelegten Timeout gelöscht werden. Ausführliche Informationen zu Sitzungen finden Sie unter [MPSD Sitzungsdetails](multiplayer-appendix/mpsd-session-details.md).

Titel können auch mehrere Sitzungen zu verwenden, aber eine typische Multiplayer-Implementierung verwendet zwei Sitzungen:

* Lobby-Sitzung – Dies ist eine Sitzung, die eine Gruppe von Freunden darstellt, die zusammen in mehreren rundet, Ebenen, Zuordnungen usw. des Spiels bleiben soll.
* Spiele-Sitzung – ist dies eine Sitzung, die den Benutzer darstellt, die in einer bestimmten Sitzungsinstanz eines Spiels, z. B. eine runde, Match, Ebene usw. wiedergegeben werden. In dieser Sitzung kann Elemente aus mehreren Lobby-Sitzungen enthalten, die die Sitzungsinstanz, in der Regel über einen Vermittlungsdienst verknüpft haben.

Hier ist ein Beispielszenario: Sally möchte einige Multiplayer-Spiele mit ihren Freunden, John und Claudia wiedergeben. Sally ein Spiel gestartet, und lädt in ihrem Spiel John und Claudia. Nachdem sie hinzugefügt haben, sind in einer Sitzung Lobby Sally, John und Claudia. In dieser Sitzung möchten sie in einer online-Übereinstimmung mit anderen Personen spielen. Das Spiel erstellt eine Spiele-Sitzung und die Xbox Live-Vermittlungsdienst zum Ausfüllen der verbleibenden Player Slots mit anderen Spielern Xbox Live verwendet.

Nehmen wir an, dass Bob und Joe werden mit diesen abgestimmt, und diese fünf die Runde zusammen spielen. Nach Beendigung des round mit Sally, John und Claudia verlassen die game-Sitzung jedoch weiterhin gemeinsam in der Sitzung Lobby (ohne Bob "und" Joe ") sind, und spielen eine andere round oder zu anderen Spielmodus wechseln können.

### <a name="session-member"></a>In der Sitzung Element

Eine Sitzung-Element ist ein Xbox Live-Benutzer, der einer Sitzung gehört.

### <a name="arbiter"></a>Arbiter

Ein Arbiter ist eine Konsole oder ein Gerät, das den Zustand der Sitzung eines Spiels verwaltet. Beispielsweise wäre der Arbiter verantwortlich für die Ankündigung einer spielen-Sitzungs mit Vermittlung, um mehr Spieler finden.

Der Arbiter, die durch den Titel festgelegt ist. Es kann als Host für das Spiel identisch sein und muss nicht identisch sein.

### <a name="session-host"></a>Sitzungshost

Der Sitzungshost ist die Konsole oder Gerät, das Spiel ausgeführt wird, spielen Simulation für Titel, basiert auf einer Peer-zu-Peer hostbasierte Netzwerkarchitektur. Diese Konsole oder das Gerät ist in der Regel identisch mit dem Arbiter, aber es muss nicht identisch sein.

## <a name="multiplayer-service-session-directory"></a>Multiplayer-Service-Sitzungsverzeichnis

Der Xbox Live-Multiplayer-Dienst in der Xbox Live-Cloud ausgeführt wird, und zentralisiert die Multiplayer-Systemmetadaten des Spiels über mehrere Clients. Das System, das diese Metadaten verfolgt wird als Multiplayer-Sitzungsverzeichnis oder MPSD kurz bezeichnet. Sie können als eine Bibliothek mit aktiven Spiel Sitzungen MPSD vorstellen. Hinzufügen Ihres Spiels durchsuchen Sie, ändern Sie oder entfernen Sie die aktive Sitzungen, die im Zusammenhang mit dem Titel des. Die MPSD auch die Verwaltung des Sitzungszustands und Sitzungen aktualisiert bei Bedarf.

MPSD kann Titel für die wichtigsten Informationen für eine Gruppe von Benutzern die Verbindung freigeben. Dadurch wird sichergestellt, dass die Funktionalität der Sitzung synchronisiert und konsistent ist. Er koordiniert sich mit der Shell und Xbox One-Konsole Betriebssystem Einladungen senden/übernehmen und über die Spieler Karte verknüpft wird.

### <a name="session-handles"></a>Sitzung handles

Eine Sitzung wird eindeutig durch eine Kombination von Daten in MPSD identifiziert:

* Die Dienstkonfiguration ID (SCID) des Titels
* Der Name der sitzungsvorlage, die zum Erstellen der Sitzung verwendet wurde
* Der Name der Sitzung

Ein Sitzungshandle ist ein jsonobjekt, das einen Verweis auf eine bestimmte Sitzung enthält, die in MPSD vorhanden ist. Sitzung Handles ermöglichen den Xbox Live-Mitgliedern, vorhandene Sitzungen aufzurufen.

Jede Sitzungshandle enthält eine Guid, die das Handle eindeutig, das welche die Titel identifiziert für die Sitzung zu verweisen, indem Sie eine einzelne Guid ermöglicht.

Es gibt mehrere Typen von Handles der Sitzung aus:

* Handle einladen
* Suchhandle
* Aktivität-handle
* Korrelationshandle
* Handle der Übertragung

#### <a name="invite-handle"></a>Handle einladen

Ein Handle für die Einladung wird auf einen Member übergeben, wenn sie gefragt werden, um ein Spiel zu verknüpfen. Das Handle für die INVITE-Nachricht enthält Informationen, die die eingeladenes Mitglied Spiel Join in der richtigen Sitzung kann.

#### <a name="search-handle"></a>Suchhandle

Ein Handle für die Suche umfasst zusätzliche Metadaten über die Sitzung, und können Titel für Sitzungen zu suchen, die die ausgewählten Kriterien erfüllen.

#### <a name="activity-handle"></a>Aktivität-handle

Ein Handle für die Aktivität kann Member finden Sie unter was andere Mitglieder in ihrem sozialen Netzwerk sind, spielen und können verwendet werden, einen Freund Spiel zu verknüpfen.

#### <a name="correlation-handle"></a>Korrelationshandle

Ein Korrelationshandle arbeitet effektiv als Alias für eine Sitzung ermöglicht ein Spiel zu einer Sitzung zu verweisen, indem Sie nur die Id des das Korrelationshandle.

### <a name="transfer-handle"></a>Handle zu übertragen

Ein Handle für die Übertragung wird zum Verschieben von Spielern aus einer Sitzung mit einer anderen Sitzung verwendet.

### <a name="invites"></a>Einladungen

Xbox Live stellt eine INVITE-Nachricht-System, das vom Multiplayer-Dienst unterstützt wird. Dadurch werden Spieler, andere Spieler, um ihre spielen Sitzungen einzuladen. Eingeladene Spieler erhalten eine Einladung spielen und ein Titel verwendet diese Informationen, um eine vorhandene Sitzung, und Multiplayer-Benutzeroberfläche zu verknüpfen. Titel steuern Einladung Flow "und" wann Einladungen gesendet werden können.

Einladungen können über die Shell vom Benutzer oder direkt aus dem Titel gesendet werden. Der Benachrichtigungstext für eine Einladung kann dynamisch durch einen Titel festgelegt werden, um weitere Informationen an dem eingeladenen Spieler bereitzustellen. Einladungen können auch zusätzliche Daten für den Titel, der für den Spieler nicht sichtbar ist enthalten und können verwendet werden, um zusätzliche Informationen enthalten.

### <a name="join-in-progress"></a>Join-in-Bearbeitung

Neben der Einladungen bietet das Xbox Live auch eine Shell-minimaloption für Spieler, um eine aktive Gaming-Sitzung von Freunden zu verknüpfen oder andere bekannte Spieler. Dies ermöglicht einen anderen Pfad in eine aktive Sitzung des Spiels und wird auch von der MPSD gesteuert. Titel steuern, wann Sitzungen beigetreten sind und welche Sitzung, für die Join-in-Bearbeitung verfügbar zu machen.

### <a name="protocol-activation"></a>Protokollaktivierung

Wenn Sally eine Einladung an Claudia verknüpfen Sie ihr Spiel sendet, empfängt Claudia eine Benachrichtigung auf dem Gerät, die sie auswählen können, zum annehmen oder ablehnen.

Wenn sie die Einladung annimmt, versucht das Betriebssystem das Spiel, wenn das Spiel ist nicht bereits ausgeführt und löst ein Activation-Ereignis, das Informationen über enthält, warum das Spiel aktiviert wurde, und alle weiteren Details zu starten (im Fall von eine Einladung, z. B. die Details enthalten die ID des Players, der, sowie die Sitzung eingeladen, die das Element zur eingeladen wurde.)

Der Prozess der Behandlung dieses Ereignisses wird als protokollaktivierung bezeichnet, und gibt an, dass das Spiel automatisch in einem bestimmten Zustand wechseln sollte die in den Aktivierungsargumenten für das Ereignis beschrieben wird. Wenn der Member ein Multiplayer-Spiels verknüpft wird, wird die Handle-Id der Sitzung als eines der Argumente angegeben.

In Claudias Groß-/Kleinschreibung sollten die Einladung akzeptieren automatisch starten des Spiels (falls erforderlich), und fügen Sie ihr Spiel derselben Sitzung als Sally, ohne Claudia weiteren Aktionen ausführen müssen.

Protokoll-Aktivierung kann ausgelöst durch das Akzeptieren der Einladung, verknüpfen einen anderen Member der Spieler über ihre Profilkarte sein, oder klicken Sie auf einer tiefen verknüpft Leistung.

## <a name="smartmatch-matchmaking"></a>SmartMatch Vermittlung

SmartMatch ist der Name des Xbox Live-Diensts für anonyme Vermittlung. Dieser Dienst gleicht Akteure der dasselbe Spiel konfigurierbare anhand ein Regelsatzes Übereinstimmung.

Die Vermittlungsdienst arbeitet eng mit der MPSD und Vermittlung ein- und Ausgabe Sitzungen verwendet. Vermittlung erfolgt für den Dienst, dem Titel für die anderen Benutzeroberflächen ganz einfach während des Ausführungsflusses Vermittlung, z. B. einzelne-Player in den Titel angeben können.

Einzelpersonen oder Gruppen, die Vermittlung eingeben möchten eine Match-Ticket-Sitzung erstellen und dann die Vermittlungsdienst Weitere Entwicklungen, mit denen zum Einrichten einer Übereinstimmung zu finden. Dies führt zur Erstellung eines temporären "Übereinstimmung Ticket" sich innerhalb der Vermittlungsdienst (auf einer Übereinstimmung Hopper) befinden, für eine bestimmte Zeitdauer.

Wählt die Vermittlungsdienst Sitzungen zur Wiedergabe Konfiguration von Regeln, anhand Statistiken, die für jeden Spieler und keine weiteren Informationen angegeben, die zum Zeitpunkt der Anforderung überein. Der Dienst erstellt anschließend eine Match-Ziel-Sitzung, die alle Spieler, die zugeordnet wurden enthält, und der Benutzer die Titel der Übereinstimmung benachrichtigt.

Wenn die zielsitzung bereit ist, können Titel Quality Service (QoS)-Überprüfungen, um zu bestätigen, dass die Gruppe Zusammenspiel kann, bzw. Fügen Sie der Sitzung, um Spiele zu beginnen Spieler ausführen. Während der QoS-Prozess und Matchmade Spiels Titeln Stand des Sitzungszustands in MPSD, und sie Benachrichtigungen von MPSD über Änderungen an der Sitzung. Solche Änderungen, dass Benutzer beitreten oder diese verlassen, und Änderungen an der Sitzung Arbiter.

### <a name="match-ticket-session"></a>Ticket-Sitzung überein

Eine Match-Ticket-Sitzung stellt dar, die Clients für die Spieler mit der eine Übereinstimmung vornehmen möchten. Sie wird in der Regel erstellt basierend auf einer Gruppe von Spielern, die zusammen in einem Lobby sind oder auf anderen Titel-spezifische Gruppierungen der Spieler. In einigen Fällen möglicherweise die Ticket-Sitzung eine Spiele-Sitzung bereits in Bearbeitung befindlichen, die für mehrere Spieler sucht.

### <a name="match-ticket"></a>Match-ticket

Senden eine Sitzung Ticket Vermittlung führt zur Erstellung eines Match-Tickets, die den Vermittlung Versuch nachverfolgt. Das Ticket, z. B. Spiele Zuordnung oder Spielerebene können Attribute hinzugefügt werden. Diese zusammen mit Attributen, die der Player in der Sitzung Ticket werden verwendet, um die Übereinstimmung zu ermitteln.

### <a name="hoppers"></a>Hoppers

Hoppers sind logische stellen, in dem Match-Tickets werden gesammelt und am Anfang des Vermittlung angegeben. Nur die Tickets in der gleichen Hopper können zugeordnet werden. Ein Titel kann mehrere Hoppers jedoch Vermittlung kann nur auf einem zu einem Zeitpunkt gestartet werden. Beispielsweise können Sie durch ein Titel ein Hopper erstellen, für welche Spieler Fähigkeit das wichtigste Element für den Abgleich ist. Sie können einen anderen Hopper verwenden, in dem Spieler nur verglichen werden, wenn sie den gleichen herunterladbaren Inhalt erworben haben.

Sie konfigurieren die Hoppers für die Vermittlung, in der Dienstkonfiguration. NOCH FESTZULEGEN.

## <a name="quality-of-service-qos"></a>Quality of Service (QoS) für

Wenn Spieler online ein Multiplayer-Spiel spielen, ist die Qualität des Spiels der servicequalitätsanforderungen die Netzwerkkommunikation zwischen den Geräten betroffen, der die Spiele hostet sind. Ein schlechter Netzwerk kann unerwünschte Spiel Umgebungen wie fällt der Verzögerung oder eine Verbindung aufgrund von nicht genügend Bandbreite oder die Latenz führen.

QoS bezieht sich auf die Stärke einer online-Verbindung (Bandbreite und Wartezeit) zwischen den Beteiligten, um sicherzustellen, dass alle Spieler einen ausreichender Qualität der Netzwerk-Verbindung messen. Dies ist insbesondere wichtig für Spieler, die bei der Vermittlung, garantiert eine gute Erfahrung aufgrund von Net abgeglichen werden. Es ist weniger geeignet für Einladungen, in denen Freunde Zusammenspiel und in der Regel bereit sind, die Folgen einer schlechten Verbindung akzeptiert.

Sie können konfigurieren, dass die Sitzung, um QoS automatisch basierend auf bestimmten Kriterien zu verarbeiten, oder Ihr Spiel verarbeiten kann, die QoS messen, wenn alle Benutzer der Sitzung beitritt.
