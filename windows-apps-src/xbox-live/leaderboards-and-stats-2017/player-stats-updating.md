---
title: Aktualisieren von Statistiken 2017
description: Erfahren Sie, wie Xbox Live Player Statistiken zu aktualisieren, indem Sie Statistiken 2017.
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.date: 08/24/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Player Statistik, Statistiken 2017
ms.localizationpriority: medium
ms.openlocfilehash: d5b37c008e6aa719b641321c5e5a1c3360b20786
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631825"
---
# <a name="updating-stats-2017"></a>Aktualisieren von Statistiken 2017

Sie aktualisieren Statistiken durch Senden den letzten Wert für die Xbox Live-Dienst mithilfe der `StatsManager` APIs, die unten erläutert wird.

Es obliegt dem Titel des Player Statistiken von, und rufen Sie `StatsManager` diese nach Bedarf zu aktualisieren.  `StatsManager` werden alle Änderungen zu puffern und diese an den Dienst in regelmäßigen Abständen zu leeren.  Titel des kann auch manuell leeren.

> [!NOTE]
> Statistiken nicht allzu oft entleert.  Andernfalls werden Ihre Titel Ratenlimit.  Eine bewährte Methode wird höchstens einmal alle 5 Minuten geleert.

### <a name="multiple-devices"></a>Mehrere Geräte

Es ist möglich, dass ein Player zum Wiedergeben von Ihrem Titels auf mehreren Geräten.  In diesem Fall müssen Sie bestimmte bemühen, Dinge synchron zu halten.

Wenn beispielsweise ein Spieler 15 Headshots auf ihre Xbox zu Hause haben.  Haben sie später 10 weitere Headshots auf ihre Xbox am Haus eines Freundes.  Sie müssen den Stat-Wert von 25 auf dem zweiten Gerät senden.  Jedoch keine Möglichkeit, dies ohne Synchronisierung aus irgendeinem Grund diese Informationen kennen müssen.

Es gibt einige Möglichkeiten, wie Sie dies tun können:

1. Diese Store mit [verbundenen Speicher](../storage-platform/connected-storage/connected-storage-technical-overview.md).  Verwenden Sie in der Regel verbundenen Speicher für benutzerspezifische Daten speichern.  Diese Daten werden auf verschiedenen Geräten für einen bestimmten Benutzer synchronisiert.
2. Verwenden Sie Ihren eigenen Webdienst, um die Statistiken synchron zu halten, wenn Sie bereits, eine verfügen für das temporäre Aufgaben für den Titel.

### <a name="offline"></a>Offline

Wie oben bereits erwähnt, dient dem Titel des zum Nachverfolgen Player-Statistiken und somit Responsbile um Offlineszenarios zu unterstützen. 

### <a name="examples"></a>Beispiele

Wir werden die anhand eines Beispiels, um diese Konzepte zu verbinden.

Allgemeine Status in einen autorennspiel ist der Lap Zeit.  In der Regel niedriger ist besser für diese Statistiken.  Um eine Stat und die zugehörigen Bestenliste erstellen würden, in denen senken ist besser.  Das heißt, würden diese Bestenliste in aufsteigender Reihenfolge sortiert werden.

Titel des würde Verfolgen eines Benutzers Rundenzeiten während einer Sitzung für die Wiedergabe der.  Sie würden die Stats-Manager aktualisieren, nur dann, wenn eine runde Zeit, die niedriger als die vorherigen optimal hatten.

Sie können ihre vorherigen am besten nachverfolgen, in einem der folgenden Methoden:
1. Aus der Datei mithilfe von verbundenen Speicher.
2. Ihren eigenen Webdienst.

Der Dienst wird unabhängig davon, was die Stat-Wert ersetzen.  Also, würden Sie mit der Zeit Runde zu aktualisieren, die größer als die vorherigen am besten ist, würde dann vorherige optimal überschrieben werden.

So stellen Sie sicher Ihre Position, dass Sie nur die richtigen Stat-Werte, die basierend auf Ihrem Gaming-Szenario senden.  In einigen Fällen niedrigere Werte möglicherweise besser, die in anderen Fällen höher möglicherweise besser oder doch etwas anderes vollständig.

## <a name="programming-guide"></a>Programmierhandbuch

In der Regel wird der Flow für die Verwendung von Statistiken:

1. Initialisieren der `StatsManager` API durch die Übergabe in einen lokalen Benutzer.
1. Ein Benutzer über den Titel wiedergegeben wird, aktualisieren Stat Werte, die mithilfe der `set_stat` Funktionen.
1. Diese Stat Updates werden in regelmäßigen Abständen geleert und Xbox Live geschrieben werden.  Sie können auch manuell so vorgehen.

### <a name="initialization"></a>Initialisierung

Rufen Sie die `StatsManager` mit einem lokalen Benutzer auf die API mit den erforderlichen Informationen zu initialisieren.

Ein Beispiel finden Sie unter

```cpp
std::shared_ptr<stats_manager> statsManager = stats_manager::get_singleton_instance();
statsManager->add_local_user(user);
statsManager->do_work();  // returns stat_event_type::local_user_added
```

```csharp
Microsoft.Xbox.Services.Statistics.Manager.StatisticManager statManager = StatisticManager.SingletonInstance;
statManager.AddLocalUser(user);
statEvent = statManager.DoWork();
```

### <a name="writing-stats"></a>Schreiben von Statistiken

Schreiben Sie Statistiken mit der `stats_manager::set_stat` Funktionsreihe.  Es gibt drei Varianten von dieser Funktion für jeden Datentyp:

* `set_stat_number` für Gleitkommazahlen.
* `set_stat_integer` für ganze Zahlen.
* `set_stat_string` für Zeichenfolgen.

Wenn Sie diese aufrufen, werden die Stat-Updates auf dem Gerät lokal zwischengespeichert.  Diese werden in regelmäßigen Abständen zu Xbox Live geleert werden.

Sie haben die Möglichkeit, manuell leeren Statistiken über die `stats_manager::request_flush_to_service` API.  Bitte beachten Sie, dass Sie die Rate begrenzt, wenn Sie diese Funktion zu häufig aufrufen, sein werden.  Dies bedeutet nicht, dass die Stat nie aktualisiert wird.  Es bedeutet lediglich, dass das Update erfolgt, wenn das Timeout abläuft.

```cpp
statsManager->set_stat_integer(user, L"numHeadshots", 20);
statsManager->request_flush_to_service(user); // requests flush to service, performs a do_work
statsManager->do_work();  // applies the stat changes, returns stat_update_complete after flush to service
```

```csharp
statManager.SetStatisticIntegerData(user, statName, (long)statValue);
statManager.RequestFlushToService(user);
statManager.DoWork();
```

#### <a name="example"></a>Beispiel

Nehmen wir an, dass Sie eine Ego-Shooter verfügen.  Während eines Abgleichvorgangs können Sie die folgenden Statistiken sammeln:

| STAT-Name | Format |
|-----------|--------|
| Beste Verlauf pro Roundrobin | Ganze Zahl |
| Verlauf der Lebensdauer | Ganze Zahl |
| Lebensdauer Todesfälle eintragen | Ganze Zahl |
| Lebensdauer Kill/Tod Verhältnis | Number |

Ähnlich wie die Übereinstimmung der Spieler durchläuft, würden Sie erhöhen die *pro Runde beendet*, *Lebensdauer beendet* und *Lebensdauer Todesfälle* lokal.

Am Ende der Übereinstimmung würden Sie Folgendes tun:
1. Vergleichen Sie den Verlauf, die sie in der "Round", mit der vorherigen optimal ist.  Wenn sie größer ist, aktualisieren Sie dann `StatsManager`.
2. Deren Lebensdauer beendet und Todesfälle mit den neuen Werten und update `StatsManager`.
3. Verlauf/Todesfälle berechnen und zu aktualisieren `StatsManager`

Beachten Sie, dass bei 1 und 2, Sie ihre vorherigen Stat Werte kennen müssen.  Finden Sie in den Abschnitten oben genannten bewährten Methoden zum Abrufen dieser.

Keines dieser Statistiken konnte die Bestenliste, entsprechen die im nächsten Artikel erläutert wird.

### <a name="flushing-stats"></a>Das leeren von Statistiken

Sie können manuell Statistiken, die mithilfe von leeren `stats_manager::request_flush_to_service`.  Möglicherweise möchten dies tun, wenn Sie die Bestenliste anzeigen möchten.

Angenommen, mussten Sie für die Bestenliste `Lifetime Kills` im obigen Beispiel, Sie möchten sicherstellen, dass die Stat-Updates für diese Statistik vor dem Anzeigen der Bestenliste mit dem Server gelöscht wurde hatte.  Auf diese Weise die Bestenlisten spiegelt wider, aktuellen Status des Spielers.

### <a name="cleanup"></a>Bereinigen
Wenn der Titel wird, und geschlossen entfernen Sie den Benutzer aus Stats-Manager. Dadurch wird die neueste Stat-Werte an den Dienst geleert.

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```
