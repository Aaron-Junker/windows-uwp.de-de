---
title: Entwerfen von Xbox Live-Erlebnisse
description: Erfahren Sie mehr zum Entwerfen von überzeugenden Anwendungen für Xbox Live-Elemente durch die Player-Statistiken und Bestenlisten Erfolge Titel Ihres planen.
ms.assetid: d2a85621-7d02-4b11-a7fa-9ebaee0092d5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Statistiken, Erfolge, Bestenlisten, Entwurf
ms.localizationpriority: medium
ms.openlocfilehash: 0f080593727ec6d7ddd529b2ce976708174250ba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600785"
---
# <a name="designing-xbox-live-experiences"></a>Entwerfen von Xbox Live-Erlebnisse

Dieses Thema führt Sie durch ein Beispiel denken und Hinzufügen von Statistiken, Erfolge und Bestenlisten an Ihrem Spiel mithilfe der Data-Plattform-Funktionen von Xbox Live.

## <a name="example"></a>Beispiel
Konfigurieren wir ein fiktives Spiel, das zeigt, wie Data Platform Ihnen helfen kann diagrammübergreifend überzeugende Xbox Live-Umgebungen im Xbox One-Dashboard die Xbox App unter Windows 10, und klicken Sie in Ihrem Spiel. In diesem Szenario arbeiten wir mit der eine gedachte racing Spiel mit dem Namen _Races 2015_.

Um den größtmöglichen Nutzen aus dieser Funktionen zu erhalten, werden wir diesen empfohlenen Planungsphasen befolgen:
1. Entwerfen Sie Ihre Erfahrungen XBL
2. Entwerfen Sie die Statistiken erforderlich, um Ihre Szenarien power
3. Konfigurieren Sie nach Bedarf – wichtige Statistiken, Bestenlisten oder Erfolge


## <a name="1-design-your-xbox-live-experiences"></a>1. Entwerfen Sie Ihre Xbox Live-Erfahrungen
Wir möchten _Race 2015_ um die optimale Nutzung der Xbox Live zu erhalten, damit die um Benutzer, die aufgerufen wird, innerhalb und außerhalb des Spiels zu halten. Der ersten Fragen sind:

1. Was tun wir möchten unsere GameHubs Erfolge Registerkarte Erfahrung wie? (Wichtige Statistiken und soziale Bestenlisten)
2. Welche Ziele und die Motivationen möchten wir auf die Spieler Geben Sie in's Spiel? (Erfolge)
3. Welche Ergebnisse oder Statistiken möchten wir Spielern zu verwenden, um sich mit anderen Spielern im Spiel Rangfolge? (Bestenlisten)


### <a name="gamehubs---featured-statistics-and-social-leaderboards"></a>GameHubs - wichtige Statistiken und soziale Bestenlisten
GameHubs sind Angebotsseiten, Umgebungen, in denen Benutzer alles, was zu einem Spiel erhalten. Als ein Entwickler und/oder Herausgeber, ist dies der ideale Ort für Sie _Showcase_ wie beeindruckender und umfassende ist Ihr Spiel Xbox-Benutzern nicht das Spiel noch gekauft haben. GameHubs dienen auch Spielern Fortschritt und ansprechende Metriken angezeigt, die bereits den Titel besitzen. Unter der Registerkarte "Ergebnisse" können Benutzer Spiel Status & Erfolge, Hero-Statistiken und soziale Bestenlisten finden.

Dieser Abschnitt des-Ziel ist, die am häufigsten ansprechender Metriken für das Spiel zu erfassen und verfügbar zu machen, um das Bereitstellen eines eigenen Bildes Erfahrung in der der Spieler das Spiel, und Stufen Sie sie mit ihren Freunden in sozialen Bestenliste. Beispielsweise kann die Registerkarte "Forza 6 Auszeichnung" etwa wie folgt aussehen:

![Gamehub-Bild](../images/omega/forza_gamehub.png)


Sie sehen einige der Statistiken, z. B. Meilen gesteuert und erste Stelle beendet wird goldene, silberne und Bronze Dekorationen aufweisen, die veranschaulichen, in dem der Spieler für seine Freunde Rangfolge bestimmt. Interagieren mit der diese statistischen Daten wird eine vollständige Bestenliste wie folgt angezeigt:

![Bestenliste](../images/omega/progress_gamehub_lb.png)

 Für _Race 2015_, wir sind der Meinung im folgenden ist eine gute Satz von Statistiken, die Erfahrung des Spielers, mit dem Titel darstellen sowie gute Rangfolge Metriken:
 * Schnellsten Rundenzeiten erreicht
 * Am 1. Ort wins
 * Meilen gesteuert


### <a name="achievements"></a>Erfolge
Leistungen sind eine systemweite Mechanismus zum Weiterleiten und Aktionen der Benutzer im Spiel auf einheitliche Weise über alle Spiele zu belohnen. Dies richtig zu entwerfen, kann Benutzer das Spiel mit vollständiger auftreten und erstreckt sich die Lebensdauer des Spiels.

Für _Races 2015_, hier ist ein Teil der Leistungen, die wir konfigurieren möchten:
* 1 Lap in weniger als 60 Sekunden abgeschlossen
* Beenden Sie 10 Races 1. vorhanden
* Führen Sie mindestens 1 Race in den einzelnen Spuren
* 5 Autos besitzen
* Laufwerk für 10.000 Meilen
* ...


###  <a name="leaderboards"></a>Bestenlisten
Leaderboards bieten eine Möglichkeit für die es Spielern ermöglichen, sich mit anderen Spielern für bestimmte Aktionen zu bewerten, die sie in's Spiel erreicht werden können. Neben der Bestenlisten von sozialer Netzwerke in der GameHubs konfigurieren wir die globale Bestenlisten im Spiel verwendet werden. Dies sind einige der Bestenlisten, wollen wir alle unsere Benutzer auf die Rangfolge:

* Am schnellsten Lap-Zeit
 * Metadaten: Nachverfolgen Sie, in dem dies geschah
 * Metadaten: Auto verwendet
 * Metadaten: Wetterbedingungen
* Am 1. Ort wins
* Die meisten Fahrzeuge gesammelt

## <a name="next-steps"></a>Nächste Schritte
Nun, Sie haben gelernt wie Statistiken, Leaderboards und Erfolge effektiven Entwurf, können Sie nun zu Beginn der Implementierung in der Titelleiste Ihres beginnen.  Die nächsten Abschnitten werden den End-to-End-Prozess, beginnend mit der Konfiguration im Partner Center beschrieben.

Finden Sie unter [Player-Statistiken](../leaderboards-and-stats-2017/player-stats.md)
