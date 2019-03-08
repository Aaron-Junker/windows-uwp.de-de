---
title: Konfigurieren Sie die Statistiken und Bestenlisten 2017
description: Erfahren Sie, wie Xbox Live – wichtige Statistiken und Bestenlisten im Partner Center mit Data-Plattform 2017 zu konfigurieren.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ea2baf4bc27e6d1cfd5beb9ef0386acda72a39d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598855"
---
# <a name="configuring-featured-stats-or-leaderboards-in-partner-center-with-data-platform-2017"></a>Konfigurieren wichtige Statistiken oder Bestenlisten im Partner Center mit Datenplattform 2017

Mit Data-Plattform 2017 müssen Sie nur eine Stat in beiden Fällen zu konfigurieren:

* Die Stat dient als Grundlage für eine globale Bestenlisten, oder
* Die Stat ist eine empfohlene Player-Stat, die auf der Seite des Spiels angezeigt werden.

In beiden Fällen müssen Sie Statistiken und Bestenlisten zusammen konfigurieren. Jede Bestenliste basieren auf den Status, und Sie können nicht Status ohne Konfiguration auch eine zugeordnete globale Bestenlisten, konfigurieren, selbst wenn Sie nicht, verwenden Sie die Bestenliste für ein ausgewähltes Player-Stat möchten.

Sie müssen nicht so konfigurieren Sie Statistiken, die nicht ausgewählte Player-Statistiken und nicht von einer globalen Rangliste verwendet wird.

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>Konfigurieren einer globalen Rangliste und einen zugeordneten Player-stat

Wechseln Sie zunächst Player-Statistiken im Abschnitt mit den Titel des wie unten dargestellt.

![](../images/omega/dev_center_player_stats_creators.png)

Klicken Sie dann auf die `Create your first featured stat/leaderboard` Schaltfläche, und füllen Sie dann auf dieses Dialogfeld, und klicken Sie auf Speichern.

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

Die `Display name` Feld ist, was Benutzern in der GameHub angezeigt werden.  Diese Zeichenfolge kann lokalisiert werden, der `Localize strings` Abschnitt.  Klicken Sie auf `Show Options` in die `Localize strings` Abschnitt aus, um Details dazu, wie diese Zeichenfolgen lokalisiert anzuzeigen.

Die `ID` Feld ist die Stat-Name und wie Sie auf Ihre Stat verweisen werden wenn aus dem Title-Code aktualisiert werden.   Finden Sie unter den [Aktualisieren von Statistiken](player-stats-updating.md) ausführlicher im nachfolgenden Abschnitt.

Die `Format` wird das Datenformat der Statistik.

Die `Display Logic` bietet die Wahl zwischen `Player progression`, `Personal best`, und `Counter`:
- Player-Fortschritt: Eine einzelne Spielerebene oder der Fortschritt im Spiel darstellt.  Der letzte festgelegte Wert wird Benutzern angezeigt werden.  Positionieren Sie z. B. in der aktuellen Rundung.
- Persönliche am besten geeignet: Stellt die aktuelle beste Bewertung, die ein Player veröffentlicht wurde. Die Max. oder Min. Wert abhängig von der Sortierreihenfolge festgelegt ist, was Benutzern angezeigt werden.  Z. B. schnellsten Rundenzeiten erreicht.
- Zähler: Andere berechnet einen kumulierten Wert des Spielers hinzugefügt werden können.  

Die `Sort` Feld können Sie die Sortierreihenfolge für die Bestenliste zu ändern.

Sie können auch diese Stat stellen auch eine `Featured Stat`, aber auf `Feature on players' profiles`.  

## <a name="configure-featured-stats"></a>Konfigurieren Sie wichtige Statistiken.

Wenn Sie einen Player-Stat definieren, werden Sie haben die Möglichkeit der Überprüfung `Featured Stat`.  Bitte beachten Sie die folgenden Anforderungen

| Entwickler-Typ | Anforderung |
|----------------|-------------|
| Xbox Live Creators-Programm | Es ist nicht erforderlich, um alle Statistiken als wichtige Statistiken zu kennzeichnen.  Wenn Sie auf ein Maximum von 10 beschränkt sind. |
| ID@Xbox und Microsoft-Partner | Sie müssen zwischen 3 und 10 wichtige Statistiken festlegen. |

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes müssen Sie die Stat aus Titel Code aktualisieren.  Finden Sie unter [Aktualisieren von Statistiken](player-stats-updating.md) Weitere Details.