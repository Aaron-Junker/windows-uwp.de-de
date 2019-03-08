---
title: Vorhandensein von Rich-Konfiguration
description: Erfahren Sie, wie Xbox Live umfassender vorhanden Zeichenfolgen konfigurieren.
ms.assetid: 7b08d46d-d3aa-42eb-93f2-ecd9338fccea
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, umfassender vorhanden
ms.localizationpriority: medium
ms.openlocfilehash: d69aaadaa1f64d1cb6eb40b52016a330d924b9b9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590475"
---
# <a name="rich-presence-configuration"></a>Vorhandensein von Rich-Konfiguration

Es wird ein Teil von der Xbox Developer Portal (XDP) speziell für umfangreiche Anwesenheit Konfigurationszeichenfolgen vorhanden sein. Bis dies in XDP verfügbar ist, muss die manuelle Konfiguration verwendet werden. Manuelle Konfiguration bedeutet, dass die Zeichenfolgen in eine Arbeitsblattvorlage bereitgestellt, um Sie von Ihrem Developer Account Manager, und das Senden an Ihren Developer Account Manager für die Erfassung in der Umgebung definieren.

-   **Strings**
-   **Enumerationen**
-   **Datenerfassung**
-   **Beispiel-Spiel**
-   **Konfigurieren von Statistiken für Rich-Präsenz**
-   **Konfigurationsbeispiel für Enumeration**
-   **Beispiel für Zeichenfolge-Konfiguration**


## <a name="strings"></a>Strings

Für die umfassende Zeichenfolgen vorhanden müssen Sie die Zeichenfolge-Sätze definieren. Jeder String-sollten einen Anzeigenamen ein, die als Bezeichner verwendet werden erteilt werden. Jede Zeichenfolge-Gruppe erstellt haben, muss eine Gruppe von Zeichenfolgen, die entsprechend für Gebietsschemas, die in denen der Titel veröffentlicht wird lokalisiert enthalten. Zusätzlich zu die lokalisierten Zeichenfolgen muss auch eine neutrale Kultur-Zeichenfolge angegeben werden. Diese Zeichenfolge wird verwendet, wenn eine Anforderung für eine lokale, die angegeben wird, ist nicht.

Stellen Sie jede Zeichenfolge-Gruppe kann außerdem Datenmengen im Spiel durch das Parametrisieren von Zeichenfolgen verwenden, um weiter zu verbessern. Ein Zeichenfolge-Satz haben so viele Parameter wie gewünscht, solange die resultierenden Zeichenfolgen zeichenbeschränkungen nicht besprochen werden. Finden Sie unter [Zeichenfolgen umfassender vorhanden: Richtlinien und Einschränkungen](rich-presence-strings-policies-and-limitations.md) Weitere Details.


## <a name="enumerations"></a>Enumerationen

Die unten beschriebenen Enumerationen werden durch die Ereignisse von der Data-Plattform definiert.


## <a name="statistics"></a>Statistiken

Sie können numerische vier einzelne statistischen Werte in den Rich-Anwesenheit Zeichenfolgen verwenden. Beispielsweise könnten in einer Shooter, Sie die Anwesenheit von Rich-Zeichenfolge, die wie viele Verlauf anzeigen der Spieler, bisherigen Kryptografietechnologie entwickelt hat. Die Konfiguration muss lediglich zeigen, dass der Parameter der Statistik "Anzahl von Kill" ersetzt werden soll. Die Statistik "Kill Count" wurde erstellt, durch das System Common Schema Ereignisse über die Datenplattform und muss definiert werden, über die Konfiguration der Statistiken. Wenn die Zeichenfolge, die von einem anderen Benutzer gelesen wird, wird der Dienst dann wechseln Sie und rufen den aktuellen Wert für die "Anzahl von Kill" Statistik können die Zeichenfolge, die immer auf dem neuesten Stand sein.


## <a name="state"></a>Status

Sie können auch Statistiken zu verwenden, die tatsächlichen Zustandswerte in Ihre Anwesenheit von Rich-Zeichenfolgen darstellen. Z. B. in einem Spiel Rennspiels können Sie eine umfassende Anwesenheitsfunktionen-Zeichenfolge, welches Auto Anzeigen der Player derzeit beeinflusst wird, oder die Zuordnung oder nachverfolgen, die der Spieler auf steht. Es gibt zwei Schritte zum Konfigurieren von Statusinformationen aus:

1.  Definieren Sie für Zustandsinformationen Enumerationen für in-Game-Zeichenfolgen. Z. B. Wenn Sie fünf Zuordnungen in Ihrem Spiel und die Zuordnungen Teil Ihrer Zeichenfolgen umfassender vorhanden sein soll, müssen die fünf Zuordnungen in lokalisierte Zeichenfolgen sowie aufgelistet werden sollen.
2.  Verknüpfen Sie die Enumerationen mit Statistiken an. Hier wird in diesem Fall der Dienst den aktuellen Wert aus der Datenplattform abrufen suchen Sie nach dem Wert der Statistik. Anschließend wird bestimmt der Enumerationswert, der in die Statistik übersetzt.


## <a name="ingestion"></a>Datenerfassung

Sobald Sie Ihr Arbeitsblatt abgeschlossen haben, senden sie an Ihren Developer Account Manager für die Erfassung. Das Vorhandensein von Rich-Team überprüfen Sie die Konfiguration und stellen Sie sicher, dass sie ordnungsgemäß konfiguriert ist, und klicken Sie dann haben sie im Dienst erfasst.


## <a name="example-game"></a>Beispiel-Spiel

Für den Rest des Beispiels nehmen wir an, dass ich den aktuellen Bucket aufgerufenen Spiel arbeite. Es gibt viele verschiedene Arten von Buckets in meinem Spiel wie holzsarg zur Buckets, Silber Buckets und goldene Buckets ein. Ich entscheiden, erstellen Sie eine Statistik für jeden Bucket gestartet. Es gibt auch verschiedene Speicherorte in meinem Spiel in den Buckets, wie die riesigen Mengen, die Wüste und Strand zu starten. Sobald ein Bucket gestartet wird, durchbricht es, daher müssen Sie immer mehr Buckets durchsuchen soll. Einige Buckets benötigen Sie spezielle Zahnradsymbol (Sie benötigen einen stark genug, um eine Kick toleriert Start), um den Bucket erfolgreich starten. Das Spiel enthält ebenfalls einen Multiplayer-Modus aus, sodass Wenn genügend Spieler den Bucket gleichzeitig starten möchten, können Sie den entsprechenden Start nicht benötigen.

Werfen wir einen Blick, welche Konfiguration umfassender vorhanden, der für dieses Spiel aussieht.


## <a name="configuring-statistics-for-rich-presence"></a>Konfigurieren von Statistiken für Rich-Präsenz

Um eine Statistik generiert, die für die Datenplattform in den Rich-Präsenz-Zeichenfolgen zu verwenden, muss der Dienst die Namen der Statistiken zu kennen. Sie müssen diese Informationen im Abschnitt der Arbeitsblattvorlage einschließen, die werden Sie aufgefordert, welche Statistiken Geben Sie in Zeichenfolgen verwenden.


## <a name="enumeration-configuration-example"></a>Konfigurationsbeispiel für Enumeration

Hier ist ein Beispiel für eine Reihe von Enumerationen zu definieren. Erstellen Sie zunächst eine Enumeration für die Zuordnungen in's Spiel. Das Spiel weist drei Zuordnungen, die einen Anzeigenamen zu erhalten, so dass wir ganz einfach auf sie verweisen können. Anschließend erstellen wir für jedes Gebietsschema, eine lokalisierte Zeichenfolge für diesen Zuordnungsnamen. Beachten Sie, dass eine gesamte Standard Lokalisierung wie auch eine neutrale Kultur vorhanden ist.

Anschließend beschäftigen wir uns auf die Waffen in's Spiel. Es gibt drei Waffen in meinem Spiel, wo sie leicht zu merkende Namen und lokalisierte Zeichenfolgen. Wir tun dies für alle Enumerationen.

Enumeration | Verwandte Statistikanweisungen | Angezeigter Name | Locale | Zeichenfolge
----------- | ----------------- | ------------- | ------ | ----
Zuordnung | CurrentMap | Map_Mountains | default | Berge
 |  |  |  | de | Berge
 |  |  |  | en-US | Berge
 |  |  |  | en-GB | Berge
 |  |  |  |  de | Gebirge
 | |  |  | usw. |
 | |  | Map_Desert | default | Desert
 |  ||  |  de | Desert
 |  ||  |  en-US | Desert
 |  ||  |   en-GB | Desert
 |  ||  |  de | Wuste
 |  ||  |  usw. |
| |  |  Map_Beach | default | Beach
 ||    |  | de | Beach
 ||    |  | en-US | Beach
 ||    |  | en-GB | Beach
 ||    |  | de | Strand
 | |   |  | usw. |
Start | CurrentWeapon | Boot_Light | default | Hell
 |  ||  |  de | Hell
 |  ||  |   en-US | Hell
 |  ||  |   en-GB | Hell
 |  ||  |   de | Leicht
 |  ||  |   usw.  |
 |  | |  Boot_Medium | default | Mittel
 |  |  |  | de | Mittel
 |  |  |  | en-US | Mittel
 |  |  |  | en-GB | Mittel
 |  |  |  | de | Mittel
 |  |  |  | usw. |
 |  | |  Boot_Strong | default | Starke
 |  |  |  | de | Starke
 |  |  |  | en-US | Starke
 |  |  |  | en-GB | Starke
 |  |  |  | de | Einen großen
 |  ||  | usw.

## <a name="string-configuration-example"></a>Beispiel für Zeichenfolge-Konfiguration

Nun, da die Enumerationen und die Statistik definiert wurden, können Sie Ihre Zeichenfolgen erstellen.

In diesem Beispieltabelle unten ist die erste Spalte einen Anzeigenamen ein, die mit Verweis auf einen Zeichenfolge-im Vergleich zu einem anderen Satz unterstützen. In diesem Beispiel haben wir entschieden, die Zeichenfolge "PlayingMap" für die erste Zeichenfolge-Gruppe ein, und klicken Sie dann "TotalKicked" für die zweite Zeichenfolge-Gruppe und anschließend auf "Multiplayer" für die dritte verwendet werden soll.

Die nächsten beiden Spalten können im zusammen. Hierbei handelt es sich um die gebietsschemazeichenfolge-Paare für die Anwesenheit von Rich-Zeichenfolge. Im ersten Beispiel ist die englische Zeichenfolge verwendet werden soll "auf {0}". Die {0} Token von einem Wert ersetzt wird. Wir lokalisieren, klicken Sie dann die Zeichenfolge in für andere Gebietsschemas. Wie die Enumerationen muss eine standardmäßige als auch neutrale Kultur-Zeichenfolgen, die verwendet werden, dass ein Benutzer ein Gebietsschema verwendet wird, dass Sie ein paar gebietsschemazeichenfolge nicht angegeben haben vorhanden sein. Gebietsschemas, die für Enumerationen verwendet, muss der Gebietsschemas, für die Zeichenfolgen übereinstimmen.

Die letzte Spalte ist für die Parameter, die zum Füllen die Leerstellen in der Anwesenheit von Rich-Zeichenfolge verwendet. Wenn Sie den Dienst vorhanden, um den Wert des Parameters in der Statistik Dienst suchen möchten, müssen Sie den Stat-Namen zum Verweisen auf den Parameter verwenden. Verwenden Ihre Statistiken in Ihrer Zeichenfolgen können Sie die Zeichenfolge festgelegt, und klicken Sie dann keine Gedanken machen es. Wenn die Zeichenfolge den Namen der enthält, und Sie die CurrentMap verwenden Stat in das leere Feld ausgefüllt, klicken Sie dann aktualisiert der Dienst die Zeichenfolge entsprechend während der Übertragung von Ihre Spieler, Zuordnung zum Zuordnen in's Spiel. Wenn Sie Statistiken nicht in Ihre Anwesenheit von Rich-Zeichenfolgen verwenden, können keine Parameter angegeben werden.

Im folgenden Beispiel möchten wir zur Verwendung der aktuellen Zuordnung von Statistiken in der ersten Zeichenfolge, und klicken Sie dann die BucketsKicked Statistik in der zweiten Zeichenfolge generiert werden. Beachten Sie, dass das dritte Beispiel alle Parameter enthalten, nicht. Die Zeichenfolge ist selbst definiert. Es bezieht werden Statistiken, sich nicht auf.

Angezeigter Name | Locale | Zeichenfolge | Parameter
--- | --- | --- | ---
playingMap | default | Wiedergabe auf Karte:{0} | CurrentMap
 |  | de | Wiedergabe auf Karte:{0} |
 |  | en-US | Wiedergabe auf Karte:{0} |
 |  | en-GB | Wiedergabe auf Karte:{0} |
 |  | de | Spielt Auf Karte: {0} |
 |  | Usw. | |
totalKicked | default | Start {0} Buckets! | BucketsKicked
 |  | de | Start {0} Buckets! |
 |  | en-US | Start {0} Buckets! |
 |  | en-GB | Start {0} Buckets! |
 |  | de | {0} Eimer Getreten! |
 |  | Usw. | |
Multiplayer | default | Multiplayer-Spielen |
 |  | de | Multiplayer-Spielen |
 |  | en-US | Multiplayer-Spielen |
 |  | en-GB | Multiplayer-Spielen |
 |  | de | Spielt Mehrspieler |
 |  | Usw. | 

Es gibt keine Beschränkung für wie viele Zeichenfolgen, die Sie erstellen können, aber Sie benötigen mindestens eine Zeichenfolge für den Titel.

"BucketsKicked" ist eine Zahl, die in der Zeichenfolge ersetzt werden kann. "CurrentMap" ist ein Beispiel für eine Statistik stammt, in dem die Zeichenfolge aus der Enumeration. Wenn der Titel des Rich-Präsenz auf eine der folgenden Zeichenfolgen festlegt, verwenden wir die Konfiguration wissen, welche Parameter erforderlich sind.
