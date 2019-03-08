---
title: Wichtige Statistiken und Bestenlisten 2017
description: Erfahren Sie, wie Xbox Live – wichtige Statistiken und Bestenlisten 2017 in Partner Center zu konfigurieren.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, wichtige Statistiken und Bestenlisten, Bestenlisten, Statistiken 2017 Partner Center
ms.openlocfilehash: e152ea8aa745536f0b7843f4f7d372a79dc3223f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655105"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>Konfigurieren wichtige Statistiken und Bestenlisten 2017 im Partner Center

Status eines Spiels für die Interaktion mit den Statistiken-Dienst muss in definiert werden [Partner Center](https://partner.microsoft.com/dashboard). Alle wichtige Statistiken werden auf die GameHub ausgewiesen, dadurch wird es automatisch als eine Bestenliste fungieren. Wir werden den Rohwert speichern, allerdings besitzen das Spiel wird die Logik zum bestimmen, ob ein neuer Wert bereitgestellt werden sollen.

![Screenshot der Seite Erfolge auf dem Spiel Hub](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png) die Abbildung oben zeigt, wie wichtige Statistiken in der Titelleiste Ihres GameHub aussieht. Die wichtige Statistiken sind innerhalb-Rot dargestellt.

Mit Data-Plattform 2017 müssen Sie nur eine Statistik zu konfigurieren, die für eine social Bestenliste verwendet wird, die auf GameHub des Spielers empfohlen wird.

Sie können die Partner Center verwenden, so konfigurieren Sie eine ausgewählte Stat und Bestenlisten, die Ihr Spiel zugeordnet ist. Fügen Sie Konfiguration wie folgt hinzu:

1. Navigieren Sie zu der **wichtige Statistiken und Bestenlisten** Titel, befindet sich im Abschnitt **Services** > **Xbox Live**  >  **Wichtige Statistiken und Bestenlisten**.
2. Klicken Sie auf die **neu** Schaltfläche ein modales Formular geöffnet wird. Nachdem ausgefüllt haben, klicken Sie auf **speichern**.

![Abbildung des Dialogfeld "Neues ausgewähltes Stat/Leaderboard"](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

Die **Anzeigenamen** Feld ist, was Benutzern in der GameHub angezeigt werden. Diese Zeichenfolge kann lokalisiert werden, der **Lokalisieren von Zeichenfolgen** der Dienstkonfiguration Xbox Live.

Die **ID** Feld den Stat-Namen und wie Sie auf Ihre Stat verweisen werden, wenn es im Code aktualisieren. Finden Sie unter den [Aktualisieren von Statistiken](../../leaderboards-and-stats-2017/player-stats-updating.md) Weitere Details.

Die **Format** wird das Datenformat der Statistik. Unter anderem Integer, Decimal, Prozentsatz, kurze Zeitspanne, langen Zeitraum und Zeichenfolge.

Jede **Format** Option erhalten einige Informationen zu den zulässigen Werten oder Formatierungen in der Dropdownliste, nach unten, wenn ausgewählt.

* Ganze Zahl Statistiken akzeptieren ganze Zahlen wie 1, 2 oder 100.
* Decimal-Statistiken akzeptieren sollen Bruchzahlen mit zwei Dezimalstellen an, wie z. B. 1,05 oder 12.00
* Prozentsatz Statistiken akzeptieren ganze Zahlen zwischen 0 und 100 liegen. "%" wird am Ende die ganze Zahl angefügt. (z. B. 0 %, 100 %)
* Kurze Timespan-Statistiken befolgen Sie die im Format hh: mm: 02:10:30, und fordert Sie auf eine Zeiteinheit für die Stat bereitzustellen.   Die Einheiten für die verfügbare Zeit sind Millisekunden, Sekunden, Minuten, Stunden und Tage.
* Lange Timespan-Stat folgen der Xd Xh Xm-Format wie z. B. 1d 2h 10m, fordert diese Stat auch Sie auf eine Zeiteinheit für die Stat bereitzustellen.

Die **Sortierreihenfolge** Feld können Sie die Bestenliste, entweder aufsteigend oder absteigend sortiert werden die Sortierreihenfolge zu ändern.

Beachten Sie beim Konfigurieren einer ausgewählte Stat und Bestenlisten die folgenden Anforderungen:

| Entwickler-Typ | Anforderung | Grenze |
|----------------|-------------|-------|
| Xbox Live Creators-Programm | Es ist nicht erforderlich, um alle Statistiken als wichtige Statistiken zu kennzeichnen. | 20 |
| ID@Xbox und Microsoft-Partner | Sie müssen mindestens 3 – wichtige Statistiken festlegen. | 20 |

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes müssen Sie die teamstatistik aus dem Code zu aktualisieren.  Finden Sie unter [Aktualisieren von Statistiken](../../leaderboards-and-stats-2017/player-stats-updating.md) Weitere Details.
