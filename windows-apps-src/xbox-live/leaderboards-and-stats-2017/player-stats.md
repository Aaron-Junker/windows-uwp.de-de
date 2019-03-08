---
title: Spielerstatistiken
description: Informationen Sie zum Einrichten von Player-Statistiken in Xbox Live.
ms.assetid: 5ec7cec6-4296-483d-960d-2f025af6896e
ms.date: 07/30/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Player-Statistiken, Bestenlisten
ms.localizationpriority: medium
ms.openlocfilehash: e8b88f3db93f98789ada3c3b3dfe74195925657e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597305"
---
# <a name="player-stats"></a>Player-Statistiken

Siehe [Übersicht über Data-Plattform](../data-platform/data-platform.md), Statistiken sind die wichtigsten Informationen, die Sie über einen Player nachverfolgen möchten. Z. B. *Head Aufnahmen* oder *Spielstufe Lap*. Statistiken werden verwendet, um Bestenlisten in eine Reihe von Szenarien zu generieren, mit dem Spieler, um ihren Aufwand und die Fähigkeiten, mit dessen Freunden zu vergleichen und alle anderen Spieler einen Titel für die Community. Konfiguriert Sie Statistiken angezeigt wird, im Titels eines [Spiel Hub](../data-platform/designing-xbox-live-experiences.md) Bestenliste anzeigen, in denen ein Spieler sehen wie sie sich für seine Freunde Rangfolge auch den Titel gespielt haben. Die Statistiken, die im Spiel-Hub des Titels werden angezeigt, heißen **Feature Statistiken**, auch bekannt als **Hero-Statistiken**. Neben der im Hub-Spiel angezeigt, kann der Xbox One 1806 Aktualisierung wichtige Statistiken in regelmäßigen Abständen in angehefteten Inhaltsblöcke angezeigt, die Benutzer auf ihre Homepage-Ansicht hinzufügen können. Dadurch können Schnellzugriff soziale Bestenlisten für direkt aus der Home-Sicht. Es ist wichtig zu beachten, dass wichtige Stats-Erstellung ist eines von mehreren möglichen Elementen, die in Blöcken angeheftete Inhalt angezeigt wird, damit Benutzern nicht immer angezeigt möglicherweise wenn Inhalte von Microsoft-Dienste feststellen, dass es sich bei einem anderen Inhaltselement mehr interessant sein könnte.

## <a name="stats-2013-and-2017"></a>Statistiken, 2013 und 2017

Es gibt derzeit zwei Implementierungen für Statistiken für die Xbox Live, 2013 von Statistiken und Statistiken 2017. Beide stehen für ID@Xbox und Partner, Entwickler verwaltet. Xbox Live Creators-Programm-Entwickler können nur Statistiken 2017 verwenden, und daher können Statistiken 2013 ignorieren.

Entwickler für Xbox Live Creators-Programm können fahren Sie mit der [Statistiken 2017 Dokument](stats2017.md).

Diese zwei Implementierungen werden für grundlegend Prinzipien. Wenn Statistiken 2013 verwenden, die Sie senden **Ereignisse** auf Xbox Live-Dienst, enthält bestimmte Informationen über eine Aktion, die ein Benutzer ausgeführt. Die Informationen in den folgenden **Ereignisse** wird verwendet, um Statistiken entsprechend aktualisieren. Statistiken 2013 wird der Dienst verfolgt und Aktualisieren aller Ihrer Statistiken-Werte, die die Quelle der Wahrheit für vier einzelne statistischen Werte für einen Player oder eine Gruppe von Spielern erleichtert. Im Gegensatz dazu, in der Statistik 2017 senden Sie den tatsächlichen Stat-Wert für den zu verwendenden Server selbst. Statistiken 2017 des Servers, die nur wenige oder gar keine Validierung für den Wert an ihn gesendet, und daher ist es bis zu Ihrem Titel, verfolgt die richtigen Stat-Werte, und die Quelle der Wahrheit für vier einzelne statistischen Werte werden. Es wird empfohlen, die Sie nachverfolgen, und speichern Ihre Statistiken in der Cloud mit dem [Xbox Live-Speicherplattform](../storage-platform/storage-platform.md) Statistiken 2017 implementieren möchten. Sie können Statistiken 2017 als einen Dienst vorstellen. Senden Sie die richtige Statistik für Ihr Spiel an den Server, stat wird Klicken Sie dann auf dem Server befinden und warten, bis die Anforderung angezeigt oder aktualisiert werden.

## <a name="a-brief-history"></a>Eine kurze Geschichte

Mit der Einführung der Xbox One eingeführt, Xbox Live neue Statistiken ereignisgesteuerten Modell Player Statistiken 2013. Dies sorgte für eine Vielzahl von Vorteilen: ein einzelnes Ereignis von einem Spiel kann Daten für mehrere Xbox Live-Features wie Bestenlisten und Erfolge aktualisieren. Xbox Live-Konfiguration befindet sich auf dem Server statt auf dem Client; und vieles mehr. In den Jahren, die nach der Veröffentlichung der Xbox One wir hat eng Spieleentwickler Feedback reagiert, und Entwickler konsistent eine angefordert optimierte Statistiken, die würde können sie die Komplexität zu umgehen, die in eine ereignisgesteuerte System enthalten ist, sowie Sie verwenden alle Statistiken, die Methoden, mit denen, die Sie bereits mehrmals wurden, nachverfolgen. Basierend auf Feedback der Entwickler, die eine neue, vereinfachte Version von Statistiken erstellt wurde, die Kontrolle über Statistiken Logik in die Hände der Entwickler wiederhergestellt werden sollen. Dass System Statistiken 2017 einen Dienst ist einfach den Wert, an sie übergeben, durch den Titel, die Kontrolle für Entwickler von der Logik der wie ein Stat Wert bestimmt wird.

## <a name="how-stats-are-handled-in-2013-and-2017"></a>Behandlung von Statistiken in 2013 und 2017

Werfen wir einen Blick auf die zum Konfigurieren und Aktualisieren von Statistiken für 2013 und 2017. Angenommen Sie, wir wollen Statistiken von einigen generischen RPG-Datentypen zu stellen und zum Nachverfolgen Erlegen beendet werden soll.

Statistiken 2013 Ihrem Titel senden würde eine *Ereignis* , enthält Informationen über eine Aktion, die von einem Player ausgeführt. In diesem Fall wird die Aktion ein Orc beseitigen einfach die überflüssigen solange der Spieler ein Schwert ausgestattet war. Einige der Informationen in diesem Fall enthalten möglicherweise, dass eine Aktion Slay erstellt wurde, die Sache Slayed ein Orc wurde, die bekämpfen Melee war und die Waffe verwendet, eine "sword wurde". Statistiken 2013 führt diese Informationen über eine Reihe von Regeln, die auf von Ihnen, den Entwickler konfiguriert die [Xbox-Entwickler-Portal (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) oder [Partner Center](https://partner.microsoft.com/dashboard), und Statistiken zu aktualisieren, auch konfiguriert werden, indem Sie auf der Grundlage das Ereignis. Statistiken 2013 der Dienst wird von verfolgt werden wie der Wert für die Statistik slaying werden soll. Die Slay Orc mit "sword" Ereignis konnte mehrere Statistiken wie, Anzahl der Verlauf, Anzahl der Orcs slain und Anzahl der "sword" Verlauf aktualisiert werden.

Statistiken 2017 werden Ihnen die tatsächlichen Werte für die Statistik senden. Für die Slay Orc "sword"-Beispiel werden Ihre Titel nachverfolgen die Anzahl der gesamte Verlauf und "sword" Verlauf Orcs slain einzeln, und senden dem Dienst die aktualisierte Anzahl für jede Statistik. Der Dienst hat die minimalen Überprüfungen, um sicherzustellen, dass Sie eine Zahl gesendet, die sinnvoll ist, daher ist es absolut bis zu den Titel, um die richtige Statistik senden. Während Sie die Stats-2017-Dienst verwenden können, um die Werte von Statistiken zu Beginn einer Spiele-Sitzung abzurufen, sollten Sie die Stats-2017-Dienst nicht verwenden, bestätigen Sie den Wert des Status, während die Sitzung ausgeführt wird.

Im folgenden sehen Sie eine Abbildung der zwei Arten von Statistiken wie ausgeführt werden.
![Statistiken Unterschied Abbildung](../images/stats/Stats2013-7DiagramColored.jpg)

## <a name="a-few-more-notes"></a>Einige Weitere Hinweise

Für ID@Xbox und verwalteten Partner-Entwickler gibt es einige weitere Unterschiede, die Ihnen bei der Entscheidung zwischen Statistiken 2013 "und" Stats 2017 Titel Ihres bekannt sein sollten.

Statistiken 2013 ermöglicht Weitere Leaderboard-Ansichten.
Statistiken 2013 ermöglicht, dass Sie problemlos einen Drilldown auf die Metadaten der Statistiken ausführen. In unserem Orc Monster Beispiel, dass die gesamte Statistik verfolgen die Anzahl der Verlauf von wurde. Statistiken 2013 können Sie einen Drilldown ausführen, auf die Statistik, was beendet wurde und was Waffe sowie alle anderen Kill definieren die Informationen, die Sie konfigurieren können.

Statistiken 2013 verfügt über eine systemeigene spielminuten Stat. Statistiken 2013 ermöglicht, dass Sie auf die des Spielers Play-Zeit in Spiel zugreifen, ohne die Statistik zu konfigurieren. Alle bei Statistiken müsste nach Ihrem Titel in Statistiken 2017 nachverfolgt werden.

Statistiken 2013 verfügt über eine near Real-Time-Aktualisierungsintervall.
Das Ereignissystem Statistiken 2013 aktualisiert Ihre Statistiken nahezu sofort an. In den Statistiken 2017 ist eine Wartezeit fünf Minuten, bevor aktualisierte Statistiken, an den Dienst geleert werden. Entwickler kann die Statistiken manuell leeren, aber immer noch auf ein Minimum von 30 Sekunden zwischen protokollleerungen gedrosselt.

Statistiken 2013 kann andere Xbox Live Services unterstützen.
Mit Statistiken 2013 können Sie Statistiken zum Entsperren Erfolge und Vermittlung Entscheidungen treffen. Statistiken 2017 wird nur verwendet, um Bestenlisten zu erzeugen.

## <a name="further-reading"></a>Weitere Informationen

Eine ausführlichere Erläuterung von Statistiken 2013 finden Sie die [partner nur Dokumentation Statistiken 2013](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/user-stats).
Eine ausführlichere Erläuterung von Statistiken 2017 finden Sie die [Statistiken 2017-Dokumentation](stats2017.md).