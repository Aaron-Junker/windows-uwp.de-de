---
title: Erstellen Sie eine Bestenliste in Unity
description: Führen Sie zum Erstellen eigener Bestenliste in Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Unity, Bestenlisten
ms.openlocfilehash: a62cb2b3bd4afebcc7aa9060db60ac167f052977
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657825"
---
# <a name="leaderboards-data-in-unity"></a>Bestenlisten-Daten in Unity

Für diejenigen unter Ihnen, die eine benutzerdefinierte Leaderboard-Umgebung wünschen, wird in diesem Artikel die eigene Bestenliste zu implementieren, indem Sie über die verfügbaren APIs zu Unity-Entwickler-Tools angezeigt. Nachdem Sie erfahren, wie Leaderboard-Daten per pull werden Sie sie auf der Benutzeroberfläche Ihrer Wahl anwenden können.

[!IMPORTANT]
> Dieser Artikel bezieht sich auf eine Version des Plug-Ins vor einem Update im Mai 2018 (Version 1804). Wenn Sie die Xbox Live-Plug-in nach dieser Zeit installiert haben oder noch nicht heruntergeladen müssen Sie eine neuere Version möglicherweise die verschiedenen Aufrufe verwendet, um das Leaderboard-Daten zu sammeln. Darüber hinaus werden Sie feststellen, dass die Screenshots in diesem-Plug-in die von der neuesten Version nicht übereinstimmen. Stattdessen finden Sie in der [neuere Unity Bestenlisten von Grund auf neue Artikel](unity-leaderboard-from-scratch.md).


## <a name="call-for-leaderboard-data"></a>Aufruf für Leaderboard-Daten

Um das Leaderboard-Daten aus dem Xbox Live-Dienst abzurufen, die Sie aufrufen müssen  `void GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)`

Um erfolgreich auf diesen Aufruf vorzunehmen Sie zum Abrufen müssen, einer `XboxLiveUser` aus [-Anmeldung](unity-prefabs-and-sign-in.md), haben eine [Stat konfiguriert](add-stats-and-leaderboards-in-unity.md) mit dem Wert für mindestens ein einziger Player und Formular eine `LeaderboardQuery`. Sie können die verknüpften Artikel lesen, wenn Sie nicht bereits wissen, wie Sie melden sich einen Benutzer oder eine Statistik für die Bestenliste initialisiert werden muss. Nachdem Sie einen initialisierten haben eine Statistik die einfachste Möglichkeit, Ihre Leaderboard-Skript zugeordnet ist, eine der Statistik prefabs (Vorlagen) umfasst die: `IntegerStat`, `DoubleStat`, oder `StringStat` als eine öffentliche Variable. Ihre Stat benötigen sie die Eigenschaft "ID" zumindest als konfiguriert wird was wir für verwendet werden, die **StatName** Parameter an, wenn wir für unsere Leaderboard-Daten aufrufen. Schließlich müssen Sie dem Formular eine `LeaderboardQuery` Objekt.
Ein `LeaderboardQuery` hat einige Attribute, die festgelegt werden, können sich Auswirkungen auf die Daten, die zurückgegeben werden:

- **StatName(Required):** Dies ist die ID des der Stat Ihre Bestenliste Ruft Daten ab, wenn dies nicht festgelegt ist, nicht auf einen Stats-ID-Wert festgelegt, werden keine Daten zurückgegeben werden.
- **SocialGroup:** abhängig vom Wert dieser Zeichenfolge werden Ihre zurückgegebenen Leaderboard-Daten gefiltert werden Ihre Freunde, Ihre Favoriten Freunden oder, wenn nicht festgelegt ist, wird eine ungefilterte globalen Rangliste abgerufen. Der Wert "" wird eine ungefilterte Liste, den Wert "all" zurückgegeben wird, eine Liste mit nur Ihren Freunden in "Favoriten" Es gibt eine Liste mit nur Ihre Favoriten Freunden vorhanden zurück gibt.
- **SkipResultToRank:** Wenn festgelegt, diese Variable "uint" bestimmen, welche die Rangfolge der Leaderboard-Daten mit gestartet wird, bei der Rückgabe. Rangfolgen beginnen bei Rang 1.
- **SkipResultToMe:** Wenn legen Sie auf dieser booleschen Wert "true" die Leaderboard-Daten zurückgegeben, die zum Starten verursacht, die `XboxLiveUser` in verwendet die `GetLeaderboard()` aufrufen.
- **Bestellung:** Enumerationen des Typs `Microsoft.Xbox.Services.Leaderboard.SortOrder` zwei mögliche Werte auf- und absteigenden haben. Diese Variable für die Abfrage bestimmt die Sortierreihenfolge von Ihrem Bestenliste. Standardmäßig werden Daten von Bestenlisten in absteigender Reihenfolge zurückzugeben.
- **"MaxItems":** Diese "uint" bestimmt die maximale Anzahl von Zeilen, die pro Aufruf zurückgegeben `GetLeaderboard()`.

Erstellen Ihre LeaderboardQuery kann wie folgt aussehen:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

LeaderboardQuery query = new LeaderboardQuery
        {
            StatName = stat.ID,
            SkipResultToRank = 100,
            MaxItems = 5
        };
```

Mithilfe einer Abfrage gibt zurück, geordnet fünf Zeilen die Bestenliste beginnend ab der 100 einzelne für die angegebene Statistik.

> [!WARNING]
> SkipResultToRank höher als die Anzahl der Spieler die Bestenliste enthaltenen Einstellung bewirkt, dass die Bestenliste Daten mit 0 (null) Zeilen zurückgeben.

Nun, wir alle Bestandteile zusammen haben können wir Aufrufen der `GetLeaderboard(XboxLiveUser user,  LeaderboardQuery query)` -Funktion, in dem wir unsere mit Vorzeichen in verwenden `XboxLiveUser` (wahrscheinlich eine `XboxLiveUserInfo.User`) von Anmeldung als unser Benutzer abgerufen und die `LeaderboardQuery` wir gerade erstellt haben, als der Abfrage.

Ihre Funktion aufrufen Bestenliste kann wie folgt aussehen:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

public void LoadLeaderboard()
    {
        // check to make sure you have a valid stat
        if (this.stat == null)
        {
            // TO DO: Display "Stat not specified" error message!
            return;
        }

        // check to make sure you have an XboxLiveUser
        if (this.xboxLiveUser == null)
        {
            if (XboxLiveUserManager.Instance.UserForSingleUserMode != null
                && XboxLiveUserManager.Instance.UserForSingleUserMode.User != null)
            {
                // set the XboxLiveUser if one is available
                this.xboxLiveUser = XboxLiveUserManager.Instance.UserForSingleUserMode.User;
            }
            else // if you don't have a user signed in display an error message and exit. 
            {
                // TO DO: Display "User Not logged In" error message
                return;
            }
        }

        LeaderboardQuery query;

        query = new LeaderboardQuery
        {
            StatName = this.stat.ID,
            MaxItems = this.maxItems,

        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
    }
```

Jetzt haben Sie vielleicht bemerkt, die unseren Bestenlisten Funktion abrufen `GetLeaderboard()` "void" zurückgibt und daher keine Daten zurückgibt, die Bestenliste wir suchen. Wir werden die Leaderboard-Daten in eine Ereignisfunktion, die im nächsten Abschnitt erläutert tatsächlich abgerufen.

## <a name="receive-the-leaderboard-data"></a>Empfangen der Leaderboard-Daten

Um die Bestenliste Daten abzurufen, Sie eine lauschende Funktion zum hinzufügen müssen, der `StatsManagerComponent` -Instanz für den Titel. Sie sollten die folgende Codezeile zum Hinzufügen der `Awake()` Funktion Ihres Codes: `StatsManagerComponent.Instance.GetLeaderboardCompleted += this.MyGetLeaderboardCompletedFunction`.

```csharp
private void Awake()
    {
        this.EnsureEventSystem();
        XboxLiveServicesSettings.EnsureXboxLiveServicesSettings();

        StatsManagerComponent.Instance.GetLeaderboardCompleted += this.GetLeaderboardCompleted;
    }
```

Die `StatsManagerComponent` Leaderboard-Beendigungsereignisse überwacht. Durch Ausführen dieser Codezeile, fügen Sie eine Funktion, die Liste der Funktionen, die immer dann aufgerufen werden, wenn ein Leaderboard-Abschlussereignis tritt auf. In diesem Beispiel, dass die Funktion aufgerufen wird, `MyGetLeaderBoardCompletedFunction()`, Sie können die Funktion benennen, wie Sie in Ihr eigenes Skript verwenden möchten. Die Funktion "MyGetLeaderboardCompletedFunction" müssen zwei Parameter: ein Objekt, das den Absender darstellt und einen `Microsoft.Xbox.Services.Client.StatEventArgs` Parameter. Die Shell Ihre Funktion kann etwa wie folgt aussehen:

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        //Do Something;
    }
```

Das erste, was tun, diese Funktion ist Überprüfung auf Fehler, die im befinden die `StatEventArgs` Parameter StatArgs. StatArgs enthält eine `StatisticEvent` namens EventData enthält die `System.Exception` ErrorInfo aufgerufen. Wenn Fehler beim Abrufen der Leaderboard-Daten finden Sie es in statArgs.EventData.ErrorInfo. Sie können eine einfache If hinzufügen-Anweisung des vorhergehenden Codes zum Fehler zu prüfen.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }
    }
```

Nach der Bestätigung, dass keine Fehler vorliegen, speichern Sie die Ergebnisse der Leaderboard-Anforderung die im Ordner sind `statArgs.EventData.EventArgs.Result`. `Result` ist eine `LeaderBoardResult` Objekt, das die Daten enthält, Sie Ihre Bestenliste aufzufüllen müssen. In unserem Beispiel wir diese Daten zu extrahieren und an eine andere Funktion wird aufgerufen, LoadResult zu senden.

```csharp
using Microsoft.Xbox.Services.Statistics.Manager;

private void MyGetLeaderBoardCompletedFunction(object sender, StatEventArgs statArgs)
    {
        if (e.EventData.ErrorInfo != null && e.EventData.ErrorInfo.Message == "")
        {
            // TO DO: Display error message
            return;
        }

        LeaderboardResultEventArgs leaderboardArgs = (LeaderboardResultEventArgs)statArgs.EventData.EventArgs;
        this.LoadResult(leaderboardArgs.Result);
    }
```

Die `LeaderboardResult` Ergebnis, das wir für die LoadResult-Funktion zu senden, müssen alle müssen wir sowohl lesen Sie die Bestenliste-Daten, die zurückgegeben wurde als auch zusätzliche Aufrufe zum Abrufen der Ränge, die noch nicht von den ursprünglichen Aufruf zurückgegebenen Daten. Sie möchten die Ergebnisse in eine Klassenvariable für das Leaderboard-Skript zu speichern wie folgt:

```csharp
using Microsoft.Xbox.Services.Leaderboard;

void LoadResult(LeaderboardResult result)
    {
        this.leaderboardData = result;
    }
```

Dies ist wichtig, da die `LeaderboardResult` enthält eine HasNext-Eigenschaft, die bestimmt, ob es wird weiter unten die Bestenliste, die abgerufen werden können. Die `LeaderboardResult` speichert auch eine Variable für die Summe der Zeilen, die die Bestenliste bilden. Diese Eigenschaften werden für die Navigation in Ihrem Bestenliste wichtig sein. Um Daten aus Ihrer `LeaderBoardResult` einfach implementieren eine für die Schleife verwenden, die `LeaderboardResults` Liste der `LeaderboardRow` namens `Rows`. In unserem Beispielcode werden einfach wir zum Verketten der Werte in den einzelnen `LeaderboardRow` in eine Zeichenfolge, die angezeigt werden.

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

In diesem Beispiel wir die Rang, Gamertag und-Werte Eigenschaften verwendet `LeaderBoardResult` zum Auffüllen der angezeigten Zeichenfolgen als auch der "DisplayName", der die Stat die Bestenliste zugeordnet.

Ich bin sicher, dass Sie etwas kreativere mit all diesem Leaderboard-Daten können müssen.

## <a name="navigating-the-leaderboard-data"></a>Navigieren Sie in der Leaderboard-Daten

In den am häufigsten verwendeten Instanzen Sie lädt keine jedes einzelnen Rang in Ihre Bestenliste gleichzeitig und müssen die Ränge zum Anzeigen der verschiedenen Abschnitte der die Bestenliste für den Benutzer navigieren können. Angenommen Sie, dass Sie die Bestenliste mit - 100 Spieler eingestuft haben. Der erste Aufruf `GetLeaderboard()` Sie abrufen, zehn `LeaderboardRows` und für den Player angezeigt. Der Spieler möchten möglicherweise mehr als die obersten zehn Spieler finden Sie unter. Die einfachste Möglichkeit zum Abrufen des nächsten Satzes von zehn Benutzern ist, um zu bestimmen, ob die `LeaderboardResult` Sie ab Ihrem letzten gespeichert Abfrage verfügt über mehrere Zeilen mehr abgerufen und zum Aufrufen `GetLeaderboard()` mit diesem LeaderboardResult des `NextQuery` Eigenschaft. Der folgende Code wird die nächste Gruppe von Leaderboard-Daten abgerufen werden.

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Leaderboard;

void GetNextLeaders()
    {
        if(this.leaderboardData.HasNext)
        {
            LeaderboardQuery query = this.leaderboardData.NextQuery;
            XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query);
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
        StatName = stat.ID,
        SkipResultToRank = targetRank,
        MaxItems = this.maxItems
    };

    XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
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
            StatName = stat.ID,
            SkipResultToMe = true,
            MaxItems = this.maxItems
        };

        XboxLive.Instance.StatsManager.GetLeaderboard(this.xboxLiveUser, query); // call the GetLeaderboard() function with the new query
    }
```

Wenn Sie ein ausführlicheres Leaderboard-Beispiel beschäftigen möchten Sie immer lesen können das Skript Leaderboard.cs XboxLive Plugin unter im Ordner Ressourcen >> XboxLive >> Skripts >> Leaderboard.cs.