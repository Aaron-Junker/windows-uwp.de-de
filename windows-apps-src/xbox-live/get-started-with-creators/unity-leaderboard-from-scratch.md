---
title: Skripterstellung für die Bestenliste in Unity
description: Führen Sie zum Erstellen eigener Bestenliste in Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Unity, Bestenlisten
ms.openlocfilehash: 6e73ffd9b55f3638eb3cf4245c6f7943fe92dc48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608755"
---
# <a name="script-a-leaderbaord-gameobject"></a>Skript für eine Leaderbaord "gameobject"

Für diejenigen unter Ihnen, die eine benutzerdefinierte Leaderboard-Umgebung wünschen, wird in diesem Artikel die eigene Bestenliste zu implementieren, indem Sie über die verfügbaren APIs zu Unity-Entwickler-Tools angezeigt. Nachdem Sie erfahren, wie Leaderboard-Daten per pull werden Sie sie auf der Benutzeroberfläche Ihrer Wahl anwenden können.

## <a name="call-for-leaderboard-data"></a>Aufruf für Leaderboard-Daten

Es gibt zwei API-Aufrufe zum Abrufen von Daten der Bestenliste an.

- `void GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)`
- `void GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)`

Um erfolgreich eine der machen diese Aufrufe Rückgabedaten Sie erwerben müssen ein `XboxLiveUser` von [Anmeldung](unity-prefabs-and-sign-in.md), haben eine [Stat konfiguriert](add-stats-and-leaderboards-in-unity.md) mit dem Wert für mindestens ein einziger Player und Formular eine `LeaderboardQuery`. Sie können die verknüpften Artikel lesen, wenn Sie nicht bereits wissen, wie Sie melden sich einen Benutzer oder eine Statistik für die Bestenliste initialisiert werden muss. Nachdem Sie die einfachste Möglichkeit zum Zuordnen einer initialisierten Statistik haben das Leaderboard-Skript werden eine der Statistik Prefabs umfasst: `IntegerStat`, `DoubleStat`, oder `StringStat` als eine öffentliche Variable. Ihre Stat benötigen sie die Eigenschaft "ID" zumindest als konfiguriert wird was wir für verwendet werden, die **StatName** Parameter an, wenn wir für unsere Leaderboard-Daten aufrufen. Schließlich müssen Sie dem Formular eine `LeaderboardQuery` Objekt.
Ein `LeaderboardQuery` hat einige Attribute, die festgelegt werden, können sich Auswirkungen auf die Daten, die zurückgegeben werden:

- **SkipResultToRank**: Wenn festgelegt, diese Variable "uint" bestimmen, welche die Rangfolge der Leaderboard-Daten mit gestartet wird, bei der Rückgabe. Rangfolgen beginnen bei Rang 1.
- **SkipResultToMe**: Wenn das Festlegen auf diesen booleschen Wert "true" die Leaderboard-Daten zurückgegeben, die zum Starten verursacht, die `XboxLiveUser` in verwendet die `GetLeaderboard()` aufrufen.
- **Reihenfolge**: Enumerationen des Typs `Microsoft.Xbox.Services.Leaderboard.SortOrder` zwei mögliche Werte auf- und absteigenden haben. Diese Variable für die Abfrage bestimmt die Sortierreihenfolge von Ihrem Bestenliste.
- **"MaxItems"**: Diese "uint" bestimmt die maximale Anzahl von Zeilen, die pro Aufruf zurückgegeben `GetLeaderboard()` oder `GetSocialLeaderboard()`.

Erstellen Ihre LeaderboardQuery kann wie folgt aussehen:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

Diese Abfrage würde einzelne zurückgeben fünf Zeilen die Bestenliste, beginnend mit dem 100 eingestuft.

> [!WARNING]
> SkipResultToRank höher als die Anzahl der Spieler die Bestenliste enthaltenen Einstellung bewirkt, dass die Bestenliste Daten mit 0 (null) Zeilen zurückgeben.

Nun, wir alle Bestandteile zusammen haben rufen wir die `GetLeaderboard(XboxLiveUser user, string statName, LeaderboardQuery query)` Funktion.

Die `GetSocialLeaderboard(XboxLiveUSer user, string statName, string socialGroup, LeaderboardQuery query)` Funktion verfügt über einen zusätzlichen Parameter SocialGroup aufgerufen. Diese Zeichenfolge dient als Filter Beziehung für die zurückgegebenen Daten. Die zulässigen Werte für SocialGroup sind wie folgt aus:

- "all": Ergebnis erhalten Sie die Bestenliste gefiltert, dass die XboxLiveUsers Freunde
- "Favoriten": Ergebnis erhalten Sie die Bestenliste gefiltert, dass die XboxLiveUsers Favoriten Freunden

Können Sie die `LeaderboardTypes` Enumeration in die `Microsoft.Xbox.Services.Client` Namespace, um Ihre Bestenlisten SocialGroup Bezeichnung, und verwenden Sie dann die `LeaderboardHelper` Klasse Funktion `GetSocialGroupFromLeaderboardType(LeaderboardTypes leaderboardType)` zum Abrufen der entsprechenden Zeichenfolge.

> [!NOTE]
> Eine leere Zeichenfolge übergeben, für die gleichen Ergebnisse wie das Aufrufen der SocialGroup-Parameter zurückgibt der `GetLeaderboard()` Funktion. Sie erhalten eine ungefilterte *globale* Bestenliste mit jeder mit einer fensterrangfunktion im Leaderboard, der das Spiel gespielt wurde.

```csharp
using Microsoft.Xbox.Services.Leaderboard;
using Microsoft.Xbox.Services.Statistics.Manager;
using Microsoft.Xbox.Services;

public void LoadLeaderboard()
{

    if (this.stat == null)
    {
        // TO DO: Display "Stat not specified" error message!
        return;
    }

    if (this.xboxLiveUser == null)
    {
        if (SignInManager.Instance.GetCurrentNumberOfPlayers() > 0)
        {
            this.xboxLiveUser = SignInManager.Instance.GetPlayer(1);
            this.isLocalUserAdded = true;
        }
        else
        {
            // TO DO: Display "No user signed-in" error message!
            return;
        }
    }

    LeaderboardQuery query = new LeaderboardQuery
    {
        MaxItems = 5,
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, otherQuery);
}

```

Nachdem Sie bemerkt haben, dass unsere zwei Bestenliste Funktionen beim Abrufen von "void" zurückgeben und daher keine der Leaderboard-Daten, die wir suchen zurück. Wir werden die Leaderboard-Daten in eine Ereignisfunktion, die im nächsten Abschnitt erläutert tatsächlich abgerufen.

## <a name="receive-the-leaderboard-data"></a>Empfangen der Leaderboard-Daten

Um die Bestenliste Daten abzurufen, Sie eine lauschende Funktion zum hinzufügen müssen, der `StatsManagerComponent` -Instanz für den Titel. Sie sollten die folgende Codezeile zum Hinzufügen der `Awake()` Funktion Ihres Codes: `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`. Die `StatsManagerComponent` in die `Microsoft.Xbox.Services.Client` Namespace auf Leaderboard-Abschluss-Ereignisse lauscht. Durch Ausführen dieser Codezeile, fügen Sie eine Funktion, die Liste der Funktionen, die immer dann aufgerufen werden, wenn ein Leaderboard-Abschlussereignis tritt auf. In diesem Beispiel, dass die Funktion MyGetLeaderBoardCompletedFunction aufgerufen wird können Sie die Funktion benennen, wie Sie in Ihr eigenes Skript verwenden möchten. Die Funktion "MyGetLeaderboardCompletedFunction" müssen zwei Parameter: ein Objekt, das den Absender darstellt und einen `Microsoft.Xbox.Services.Client.StatEventArgs` Parameter. Die Shell Ihre Funktion kann etwa wie folgt aussehen:

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

Das erste, was tun, diese Funktion ist Überprüfung auf Fehler, die im befinden die `StatEventArgs` Parameter StatArgs. StatArgs enthält eine `StatisticEvent` EventData die Fehlerdaten enthält. Wenn Fehler beim Abrufen der Leaderboard-Daten finden Sie in `statArgs.EventData.ErrorCode` oder `statArgs.EventData.ErrorMessage`. Wenn kein Fehler der ErrorCode 0, und die Fehlermeldung wird die leere Zeichenfolge "". Sie können eine einfache If hinzufügen-Anweisung des vorhergehenden Codes zum Fehler zu prüfen.

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0) //if there is an error
        {
            // TO DO: Display error message
            return;
        }
    }
```

Nach der Bestätigung, dass keine Fehler vorliegen, speichern Sie die Ergebnisse der Leaderboard-Anforderung die im Ordner sind `statArgs.EventData.EventArgs.Result`. `Result` ist eine `LeaderBoardResult` Objekt, das die Daten enthält, Sie Ihre Bestenliste aufzufüllen müssen. In unserem Beispiel werden wir diese Daten zu extrahieren und an eine andere Funktion wird aufgerufen, gesendet `LoadResult()`.

```csharp
using Microsoft.Xbox.Services.Client;
using Microsoft.Xbox.Services.Statistics.Manager;

private void GetLeaderboardCompleted(object sender, StatEventArgs statArgs)
    {
        if (statArgs.EventData.ErrorCode != 0)
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

Die `LeaderboardResult` Ergebnis, das wir zum Senden der `LoadResult()` Funktion müssen alle müssen wir sowohl lesen Sie die Bestenliste-Daten, die zurückgegeben wurde als auch zusätzliche Aufrufe zum Abrufen der Ränge, die noch nicht von den ursprünglichen Aufruf zurückgegebenen Daten. Sie möchten die Ergebnisse in eine Klassenvariable für das Leaderboard-Skript zu speichern wie folgt:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

Dies ist wichtig, da die `LeaderboardResult` enthält eine `HasNext` Eigenschaft, die bestimmt, ob es wird weiter unten die Bestenliste, die abgerufen werden können, enthält das Ergebnis auch Gesamtzahl der Zeilen, die die Bestenliste bilden. Diese Eigenschaften werden für die Navigation in Ihrem Bestenliste wichtig sein. Um Daten aus Ihrer `LeaderBoardResult` einfach implementieren eine für die Schleife verwenden, die `LeaderboardResults` Liste der `LeaderboardRow` namens `Rows`. In unserem Beispielcode werden einfach wir zum Verketten der Werte in den einzelnen `LeaderboardRow` in eine Zeichenfolge, die angezeigt werden.


```csharp
using Microsoft.Xbox.Services.Leaderboard

void LoadResult(LeaderboardResult result)
{

    this.leaderboardData = result;

    this.leaderBoardText.text = "" // leaderBoardText is a UnityEngine.UI text box.

    foreach (LeaderboardRow row in this.leaderboardData.Rows)
    {
        leaderBoardText.text += string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", row.Rank, row.Gamertag, this.stat.DisplayName, row.Values[0]);
    }
}
```

In unserem Beispiel verwendet haben wir die Eigenschaften LeaderBoardResult Rang, Gamertag und Werte zum Auffüllen der angezeigten Zeichenfolgen als auch der "DisplayName", der der zugeordnete der Bestenliste Stat.

Ich bin sicher, dass Sie etwas kreativere mit all diesem Leaderboard-Daten können müssen.

## <a name="navigating-the-leaderboard-data"></a>Navigieren Sie in der Leaderboard-Daten

In den am häufigsten verwendeten Instanzen Sie lädt keine jedes einzelnen Rang in Ihre Bestenliste gleichzeitig und müssen die Ränge zum Anzeigen der verschiedenen Abschnitte der die Bestenliste für den Benutzer navigieren können. Angenommen Sie, Sie haben die Bestenliste mit 100 Rang Playern. Der erste Aufruf `GetLeaderboard()` oder `GetSocialLeaderboard` Sie 10 abrufen `LeaderboardRows` und für den Player angezeigt. Der Spieler möchten möglicherweise mehr als die obersten zehn Spieler finden Sie unter. Die einfachste Möglichkeit zum Abrufen des nächsten Satzes von zehn Benutzern ist, um zu bestimmen, ob die `LeaderboardResult` Sie ab Ihrem letzten gespeichert Abfrage verfügt über mehrere Zeilen mehr abgerufen und zum Aufrufen `GetLeaderboard()` mit dieser LeaderboardResults nächste Abfrage. Verwenden Sie eine LeaderBoardResult des *NextQuery* verwenden Sie die Funktion `LeaderBoardResult.GetNextQuery()`. Der Code zum Abrufen des nächsten Satzes von Rängen würde wie folgt aussehen.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.GetNextQuery();
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query);
        }
        else
        {
            //TO DO: Display an error message or go back to the beginning of the leaderboard as the situation demands.
        }
    }
```

Rückwärts verschieben, in die Bestenliste ist etwas schwieriger, da keine Funktion zum vorherigen X Anzahl der Ränge von Ihrem Bestenliste abrufen vorhanden ist. Um vorherige Rangfolgen abzurufen, müssen Sie Ihre eigene Logik schreiben. Eine Methode wäre zum Speichern Ihrer `MaxItems` pro `LeaderboardQuery` und berechnen Sie welche Rang, Sie mit dem überspringen müssen, der `SkipToRank` Attribut Ihrer `LeaderboardQuery`. Dieser Code könnte folgendermaßen aussehen:

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetPreviousLeader()
{
    if(leaderboardData == null || leaderboardData.Rows.Count < 1)
    {
        return;
    }

    uint topRank = leaderboardData.Rows[0].Rank; //get the first rank of the leaderboard.
    uint targetRank = topRank - this.maxItems > 0 ? topRank - this.maxItems : 0; //set your targetRank equal to the current top rank of your leaderboard minus the configured number of rows to display at a time.

    LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
    {
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
}
```

Das letzte, am häufigsten verwendete Szenario ist, dass ein Spieler einfach möchten möglicherweise ihre Stelle auf die Bestenliste angezeigt. Dies erfolgt einfach durch Aufrufen der `GetLeaderboard()` -Funktion mit einer Abfrage, in denen die `SkipResultToMe` -Attribut festgelegt ist auf "true".

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

    void GetRankForTag()
    {

        LeaderboardQuery query = new LeaderboardQuery // make a new query with the calculated targetRank
        {
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, this.stat.ID, query); // call the GetLeaderboard() function with the new query
    }
```

Wenn Sie ein ausführlicheres Leaderboard-Beispiel beschäftigen möchten Sie immer lesen können das Skript Leaderboard.cs XboxLive Plugin unter im Ordner Ressourcen >> XboxLive >> Skripts >> Leaderboard.cs.