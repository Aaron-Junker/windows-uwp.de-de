---
title: Hinzufügen von Player-Statistiken und Bestenlisten zu Ihrem Unity-Projekt
description: Erfahren Sie, wie auf der Xbox Live Unity-Plug-in zu verwenden, um die Player-Statistiken und Bestenlisten Ihr Unity-Projekt hinzugefügt werden.
ms.assetid: 756b3c31-a459-4ad2-97af-119adcd522b5
ms.date: 10/19/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Unity, Ersteller
ms.localizationpriority: medium
ms.openlocfilehash: 17af9afc8a9048e7222115d2afdc6108d25df1d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658135"
---
# <a name="add-player-stats-and-leaderboards-to-your-unity-project"></a>Hinzufügen von Player-Statistiken und Bestenlisten zu Ihrem Unity-Projekt

> [!IMPORTANT]
> Die Xbox Live Unity-Plug-in unterstützt keine Erfolge oder online Multiplayer-Spiele und wird nur empfohlen, für die [Xbox Live Creators-Programm](../developer-program-overview.md) Member.

Nachdem Sie hinzugefügt haben [Xbox Live anmelden](unity-prefabs-and-sign-in.md) Ihres Unity-Projekts der nächste Schritt darin Player-Statistiken und Bestenlisten anhand der die Player-Statistiken hinzufügen.

Mit der [Xbox Live Unity-Plug-Ins](https://github.com/Microsoft/xbox-live-unity-plugin), Sie können Player-Statistiken und Bestenlisten problemlos in Ihr Unity-Projekt hinzufügen. Ähnlich wie bei der Anmeldung über die Schritte aus, Sie können der enthalten prefabs (Vorlagen) verwenden, oder fügen Sie enthaltenen Skripts auf Ihre eigenen benutzerdefinierten Spielobjekte.

## <a name="prerequisites"></a>Voraussetzungen
1. [Konfigurieren von Xbox Live in Unity](configure-xbox-live-in-unity.md)
2. [Melden Sie sich beim Xbox Live in Unity](unity-prefabs-and-sign-in.md)

## <a name="player-stats"></a>Player-Statistiken

Ein Player-Stat ist alle interessanten Statistiken, die für Ihre Spieler nachverfolgt werden sollen. Die Statistiken, die Sie mit der Xbox Live verfolgen sollte Statistiken, die für den Player relevant sind, und auf irgendeine Weise angezeigt werden. Diese Statistiken Player werden am häufigsten verwendet, Bestenlisten, zu erstellen, die Spieler anzeigen können, um zu bestimmen, wie ihre Leistung mit anderen Spielern Rangfolge bestimmt. Statistiken für alle Spieler gelten "ausgewählte Statistik", was bedeutet, dass der Status auf der Seite GameHub für das Spiel angezeigt wird.

Player-Statistiken müssen es sich um einen einzelnen Wert darstellen. Die Xbox Live Unity-Plug-in enthält Prefabs für Integer "," double "," und "Zeichenfolge-Statistiken. Darüber hinaus wird ein Skript für ein Basisobjekt Stat bereitgestellt, die in andere Datentypen erweitert werden können.

Weitere Informationen zu Statistiken für Player, finden Sie unter [Player Statistiken](../leaderboards-and-stats-2017/player-stats.md).

> [!NOTE]
> Um die Player-Statistiken oder Bestenlisten mit der Xbox Live-Dienst verwenden, müssen Sie einen Benutzer erfolgreich angemeldeten verfügen, damit Sie alle Daten zugreifen können.

## <a name="using-the-player-stat-prefabs"></a>Verwenden den Player Stat prefabs (Vorlagen)

Es gibt mehrere prefabs (Vorlagen) bereitgestellt, die in den Xbox Live Unity-Plug-in, mit denen Sie in Bezug auf die Player-Statistiken:

* IntegerStat: Status, die als ganze Zahl, z. B. die Gesamtanzahl der Verlauf in einer ausgedrückt werden kann.
* DoubleStat: Status, die als Gleitkommawert dargestellt werden kann Wert, z. B. ein Verhältnis von Kill/Tod zeigen.
* StringStat: Status, die als Zeichenfolge zurück, in der Regel eine Enumeration, wie z. B. einen Rang für eine Rundung vergeben, z. B. "Gold", "Silver" oder "Bronze" ausgedrückt werden kann.
* StatPanel: Beispiel-UI, die Sie verwenden können, um den aktuellen Wert des Status anzuzeigen.

Wenn ein Player-Stat hinzufügen möchten, ziehen Sie einfach das Prefab, das den Datentyp, der die Stat auf die Szene übereinstimmt. In der Unity-Inspektors für die Statistik können Sie drei Werte angeben:

* Die ID der Statistik. Dies muss mit der konfigurierten im Partner Center-ID übereinstimmen und die Groß-/Kleinschreibung beachtet.
* Der Anzeigename, der die Stat (dieser Name wird in der StatPanel prefab-Benutzeroberfläche angezeigt).
* Der Anfangswert, der den Status zu Beginn die Szene.

Sie können die **StatPanel** Prefab, um den Wert des Status anzuzeigen. Ziehen Sie zu diesem Zweck eine **StatPanel** prefab auf die Szene. Können Sie die Stat um anzuzeigen, ziehen das Stat "gameobject" zum Angeben der **Stat** Feld der **StatPanel** Objekt in der Unity im Inspector.

### <a name="manipulating-the-player-stat-values"></a>Bearbeiten die Player-Stat-Werte

Die Player-Stat-Objekte haben eine Reihe von Funktionen, die Sie aufrufen können, den Wert des der Stat anpassen. Diese Funktionen können von anderen Routinen aufgerufen oder für UI-Elemente gebunden werden. Sehen Sie sich die **DoubleStat**, **IntegerStat**, und **StringStat** Skripts, um Beispiele für Funktionen zum Ändern des Werts, der den Status anzuzeigen. Sie können ändern, oder erstellen neue Skripts zur Darstellung von Statistiken sowie komplexere Funktionen und Logik. Neue Stat Klassen erweitern, sollte die `StatBase` in definierte Klasse die `StatBase` Skript.

Z. B. als ein einfacher Test, Sie können hinzufügen eine UI-Schaltfläche zu Ihrer Szene, und klicken Sie in der `OnClick` Ereignis der Schaltfläche in der Unity im Inspector, Hinzufügen einer **IntegerStat** Objekt aus, und rufen Sie die `Increment()` Funktion, um den Wert für die Stat erhöhen von einem jedes Mal, wenn Sie auf die Schaltfläche klicken.

Wenn Sie die Stat auch an gebunden haben eine **StatPanel** Objekt sehen Sie die Stat-Wert jedes Mal, wenn Sie auf die Schaltfläche klicken zu aktualisieren.

Bei jeder Aktualisierung die Statistiken (erhöhen, verringern, usw.), die Werte lokal aktualisiert. Um diese Stat-Updates zu erhalten, die in den Xbox Live, geschieht zweierlei müssen, wiedergegeben werden. Zunächst müssen Sie die Stats-Wert, mit dem Wert der StatisticManager.SetStatistic Funktionen festlegen. Es gibt drei `StatisticManager` Funktionen festzulegende Statistiken `StatisticManager.SetStatisticIntegerData(XboxLiveUser user, String statName, Int64 value)`, `StatisticManager.SetStatisticNumberData(XboxLiveUser user, String statName, Double value)`, und `StatisticManager.SetStatisticStringData(XboxLiveUser user, String statName, String value)`. Jede dieser Funktionen wird verwendet, legen Sie den entsprechenden Wert für den Datentyp, der Ihre Stat. Der zweite Schritt ist erforderlich, um Ihre Statistiken auf dem Server zu aktualisieren ist *leeren* die lokalen Daten. Die Daten ruft automatisch alle fünf Minuten von geleert der `StatManagerComponent` Skript.  Wenn Ihr Spiel vor der letzten 5 Minuten beendet wird, müssen Sie sicherstellen, dass die Daten manuell leeren zuerst sicherstellen, nicht verloren gehen, wird ausgeführt. Zu diesem Zweck müssen Sie zum Aufrufen der `statManagerComponent.RequestFlushToService()` -Methode aufrufen, damit Sie dabei die **XboxLiveUser** für die Stat geschrieben wird.

> [!TIP]
> Es wird empfohlen, die Daten immer zu leeren, bevor Sie Ihr Spiel ist beendet, um sicherzustellen, dass Sie nicht verlieren, den Status angesehen.

### <a name="checking-and-verifying-stats"></a>Überprüft, und Überprüfen von Statistiken

Die `StatisticManager` -Klasse verfügt über zwei Funktionen sind hilfreich, wenn überprüft wird, auf die Statistiken für konfiguriert eine `XboxLiveUser`, `StatisticManager.GetStatisticNames(XboxLiveUser user)` und `StatisticManager.GetStatistic(XboxLiveUser user, String statName)`. `GetStatisticNames()` Stellt eine `list<string>` nach den Namen der die Statistiken für die bereitgestellten XboxLiveUser aufgefüllt. Diese Namen können verwendet werden, um für den aktuellen Wert, der eine Statistik mit einem Aufruf auf die `GetStatistic()` Funktion. Es ist wichtig zu beachten, dass Sie Statistiken aus dem Xbox Live-Statistik-Dienst Lesen nicht empfohlen wird, dass Sie sie für die Spiellogik verwenden, sondern überprüfen Sie auf den Zustand der Statistik, nachdem es veröffentlicht wurde. Der Dienst nur sollen andere Dienste wie Bestenlisten ausgeführt und dient kein Quelle der Wahrheit für Statistiken im Spiel werden soll. Es ist wichtig, dass der Titel des Logik für alle Statistiken zu verarbeiten, keine Überprüfungen erfolgen auf Ihre Statistik des Xbox-Diensts, und er akzeptiert lediglich den Wert, der an es übergeben wird, als den aktuellen Status.

## <a name="leaderboards"></a>Bestenlisten

Die Bestenliste stellt eine geordnete, nummerierte Liste der Spieler, die die "besten" Wert des Status erreicht wurde. Möglicherweise die Bestenliste listet z.B. Personen, die minimale Zeitspanne, die auf eine runde Race erzielt haben Spieler bewährte Race Zeit für die besten Race Zeiten erreicht, indem Weitere Entwicklungen vergleichen zu können.

Leaderboards basiert auf die Player-Statistiken, die durch das Spiel mit dem Xbox Live-Dienst gesendet werden. Leaderboard-Daten werden daher nur gelesen, wie Sie sie nicht direkt ändern können.

Die Xbox Live Unity-Plug-in bietet eine Beispiel-Leaderboard-Prefab, die Sie verwenden können, um zu verstehen, wie Bestenlisten in einem Spiel zu implementieren.

Weitere Informationen zu Bestenlisten, finden Sie unter [Bestenlisten](../leaderboards-and-stats-2017/leaderboards.md).

## <a name="using-the-leaderboard-prefabs"></a>Verwenden die Bestenliste prefabs (Vorlagen)

Die Xbox Live Unity-Plug-in enthält zwei Prefabs für Bestenlisten:

* Leaderboard: Ein Objekt, das einfache Benutzeroberfläche zum Anzeigen der Werte in die Bestenliste enthält und die Bestenliste darstellt.
* LeaderboardEntry: Ein Objekt, das eine einzelne Zeile mit einem Bestenliste darstellt.

Ziehen Sie eine **Bestenliste** prefab auf die Szene. In der Unity im Inspector können Sie die folgenden Attribute festlegen:

* Stat: Die Stat "gameobject", dem diese Bestenliste zugeordnet ist.
* Leaderboard-Typ: Der Bereich der Ergebnisse, die für die Bestenliste Einträge zurückgegeben werden soll.
* Anzahl der Einträge: Die Anzahl von Zeilen mit Daten, die pro Seite angezeigt.

> [!NOTE]
> Die Stat Teil der Leaderboard-Prefab ist zunächst leer. Versuchen Sie, ziehen eine der oben genannten, in das Einschubfach "gameobject" zum Testen der Stat prefabs (Vorlagen).

Im Unity-Editor die **Bestenliste** Prefab wird stets die gleichen simulierten Daten unabhängig von den Einstellungen für die Eigenschaftenanalyse angezeigt. Sie müssen erstellen und Exportieren Ihr Projekt in Visual Studio, und melden Sie sich ein autorisierter Benutzer auf die tatsächlichen Datenwerte finden Sie unter. Weitere Informationen finden Sie unter [konfigurieren Xbox Live in Unity](configure-xbox-live-in-unity.md).

## <a name="see-also"></a>Siehe auch

* [Anmeldung bei Xbox Live in Unity](unity-prefabs-and-sign-in.md)
* [Konfigurieren von Xbox Live in Unity](configure-xbox-live-in-unity.md)
* [Die Szene Leaderboard-Beispiel](setup-leaderboard-example-scene.md)
* [Abrufen von Daten der Bestenliste](unity-leaderboard-from-scratch.md)
