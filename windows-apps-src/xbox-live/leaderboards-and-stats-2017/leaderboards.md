---
title: Bestenlisten
description: Erfahren Sie, wie Bestenlisten Xbox Live verwenden, um Spieler zu vergleichen.
ms.assetid: 132604f9-6107-4479-9246-f8f497978db7
ms.date: 09/28/2018
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 8fd7e30b99418fda614a888d9269548cdc57a88a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662025"
---
# <a name="leaderboards"></a>Bestenlisten

## <a name="introduction"></a>Einführung

Siehe [Übersicht über Data-Plattform](../data-platform/data-platform.md), Bestenlisten sind eine hervorragende Möglichkeit zum Wettbewerb zwischen Ihre Spieler, fördern und Spieler jederzeit fesseln bei dem Versuch, ihre vorherigen beste Ergebnisse als auch mit Freunden zu besiegen.

Bestenlisten für [wichtige Statistiken](stats2017.md#configured-stats-and-featured-leaderboards) sind immer im Spiel-Hub des Titels angezeigt und manchmal auch als Teil der Benutzeroberfläche für einen Titel angezeigt, wenn er auf die Startseite angeheftet ist. Ihre konfigurierten wichtige Statistiken können auch um Bestenlisten in den Titel zu erstellen.

## <a name="choosing-good-leaderboards"></a>Gute Bestenlisten auswählen

Siehe [Player Statistiken](player-stats.md), entspricht die Bestenliste Status, die Sie definiert haben.  Sie sollten Bestenlisten auswählen, die eine Neuerung zu, die ein Player können entsprechen, zu verbessern verwenden.

Beispielsweise liegt Lap Bestzeit in einem autorennspiel gute Bestenliste, Spieler zur Verbesserung ihrer Lap Bestzeit arbeiten möchten.  Weitere Beispiele für sind Kill/Tod Verhältnis für Shooter oder Max. Größe des Kombinationsfelds in einem Kampf gegen Spiel.

## <a name="when-to-display-leaderboards"></a>Wann Bestenlisten anzeigen

Sie haben die Möglichkeit, Bestenlisten jederzeit in den Titel anzuzeigen.  Sie sollten einen Zeitraum auswählen, wenn die Bestenliste nicht mit dem Spiel oder den Fluss der Titel des beeinträchtigt.  Zwischen dem runden und beide tolle Zeiten übereinstimmen.

## <a name="how-to-display-leaderboards"></a>Vorgehensweise beim Anzeigen von Bestenlisten

Es gibt zahlreiche Optionen zum Anzeigen von Bestenlisten im Xbox Live SDK bereitgestellt.  Wenn Sie Unity mit dem Xbox Live Creators-Programm verwenden, können Sie beginnen mit einer Leaderboard-Prefab zum Anzeigen Ihrer Leaderboard-Daten.  Finden Sie unter den [konfigurieren Xbox Live in Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) Weitere Einzelheiten.

Wenn Sie direkt mit dem Xbox Live SDK programmiert, lesen sich beim Lernen Sie die APIs, die Sie verwenden können.

## <a name="programming-guide"></a>Programmierhandbuch

Es gibt mehrere Leaderboard-APIs, die Sie verwenden können, um den aktuellen Status der Bestenliste zu erhalten.  Alle APIs sind asynchron und werden nicht blockiert.  Sie würden stellen eine Anforderung zum Abrufen von Daten der Bestenliste und weiterhin Ihre Spiele wie üblich verarbeitet.  Wenn die bestenlistenergebnisse vom Dienst zurückgegeben werden, können Sie die Ergebnisse zum richtigen Zeitpunkt anzeigen.

Sie sollten die Leaderboard-Daten aus dem Dienst, vor etwas anfordern, wenn angezeigt werden soll, sodass Spieler nicht blockiert werden, warten, bis die Bestenliste angezeigt.

## <a name="leaderboards-2013-apis"></a>Bestenlisten-2013-APIs

Sie sehen die `leaderboard_service` Namespace-URI für alle Statistiken 2013 Leaderboard-API.

<table>

<tr>
<td>C++ API</td><td>Beschreibung</td>
</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
        const string_t& scid,
        const string_t& name
        );
```

</td>

<td>Die meisten grundlegenden Version der API.  Dadurch wird die Leaderboard-Werte für die angegebene Bestenliste, beginnend mit der Spieler am oberen Rand die Bestenliste zurückgegeben.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
        _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName
        ) 
```

</td>

<td>WinRT C# Code – rufen Sie die Bestenliste für eine einzelne Leaderboard erhalten eine Dienst-Konfigurations-ID und einen Namen für die Bestenliste.</td>

</tr>

<tr>
<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ uint32_t skipToRank,
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>Diese API bietet einige mehr Flexibilität, Sie können angeben, den Rang (Position), den Sie anzeigen möchten, sowie einen maximalen Wert der zurückzugebenden Elemente.  Beispielsweise würden Sie diese API verwenden, wenn Sie die Bestenliste beginnend an Position 1000 anzeigen möchten.</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_ uint32 skipToRank,
         _In_ uint32 maxItems
        ) 
```

</td>

<td>WinRT C# code – Bestenliste Seitensuche für eine einzelne Bestenliste Geben Sie eine Dienst-Konfigurations-ID und ein Leaderboard-Namen, Bestenliste Ergebnisse mit dem Rang "SkipToRank beginnt" zu erhalten.</td>

</tr>

<tr>

<td markdown="block">

```cpp

pplx::task<xbox_live_result<leaderboard_result>> get_leaderboard_skip_to_xuid(
    _In_ const string_t& scid,
    _In_ const string_t& name,
    _In_ const string_t& skipToXuid = string_t(),
    _In_ uint32_t maxItems = 0
    );

```

</td>

<td>

Verwenden Sie diese Option, wenn Sie die Bestenliste für einen bestimmten Benutzer überspringen möchten.  Ein `XUID` ist ein eindeutiger Bezeichner für jeden Benutzer Xbox.  Sie können für den angemeldeten Benutzer oder einen ihrer Freunde zu erhalten und übergeben Sie ihn an dieser Funktion.

</td>

</tr>

<tr>

<td markdown="block">

```csharp
Windows::Foundation::IAsyncOperation< LeaderboardResult^> ^  GetLeaderboardWithSkipToUserAsync (
         _In_ Platform::String^ serviceConfigurationId,
         _In_ Platform::String^ leaderboardName,
         _In_opt_ Platform::String^ skipToXboxUserId,
         _In_ uint32 maxItems
        )
```

</td>

<td>WinRT C# code – beginnend mit einem angegebenen Player, unabhängig davon, Rang oder die Bewertung, geordnet nach des Spielers Quantilsrang des Spielers Bestenliste abrufen</td>

</tr>

</table>

## <a name="2013-c-example"></a>2013 C++-Beispiel

Bei Verwendung die C++-API-Ebene können Sie festlegen, einen Rückruf, der aufgerufen werden, nachdem das Leaderboard-Ergebnisse vom Dienst zurückgegeben werden.  Wir zeigen ein Beispiel hierzu finden Sie nachstehend.

Wenn Sie keine Erfahrung mit sind die `pplx::task` von diesen APIs zurückgegeben wird, dies ist ein asynchroner Task-Objekt aus der Microsoft Parallel Programming Library (PPL).  Weitere Informationen finden Sie unter [ https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks ](https://github.com/Microsoft/cpprestsdk/wiki/Programming-with-Tasks).

Der folgende Abschnitt zeigt, wie Sie Leaderboard-Ergebnisse abrufen und diese verwenden können.

### <a name="1-create-an-async-task-to-retrieve-leaderboard-results"></a>1. Erstellen Sie eine asynchrone Aufgabe zum Abrufen von bestenlistenergebnisse

Der erste Schritt ist zum Aufrufen des Diensts Bestenlisten zum Abrufen der Ergebnisse für einen bestimmten Bestenliste anzeigen.

```cpp
pplx::task<xbox_live_result<leaderboard_result>> asyncTask;
auto& leaderboardService = xboxLiveContext->leaderboard_service();

asyncTask = leaderboardService.get_leaderboard(m_liveResources->GetServiceConfigId(), LeaderboardIdEnemyDefeats);
```

### <a name="2-setup-a-callback"></a>2. Einrichten eines Rückrufs

Sie können einrichten, eine [Fortsetzungsaufgabe](https://msdn.microsoft.com/en-us/library/dd492427(v=vs.110).aspx#continuations) aufgerufen werden, sobald die bestenlistenergebnisse zurückgegeben werden.  Sie tun, wie folgt unten.

```cpp
asyncTask.then([this](xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result)
{
    // Handle result here
});
```

Diese Fortsetzungsaufgabe heißt im Kontext des Objekts, das ursprünglich es aufgerufen und empfängt die ```leaderboard_result``` die kann in einer Weise, die den Titel passt angezeigt werden.


### <a name="3-display-leaderboard"></a>3. Anzeigen der Bestenliste

Die Bestenliste-Daten sind enthalten ```leaderboard_result``` und die Felder sind Erläuterung erforderlich.  Ein Beispiel finden Sie unten.

```cpp
auto leaderboard = result.payload();

for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
{
    string_t colValues;
    for (auto columnValue : row.column_values())
    {
        colValues = colValues + L" ";
        colValues = colValues + columnValue;
    }
    m_console->Format(L"%18s %8d %14f %10s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
}

```

## <a name="2013-winrt-c-example"></a>2013 WinRT C# Beispiel

Bei Verwendung der WinRT C# Ebene wird nicht müssen Sie einen separaten Rückruf-Aufgabe auf und wird einfach erforderlich, verwenden die `await` Schlüsselwort Aufrufen des Leaderboard-Diensts.

### <a name="1-access-the-leaderboardservice"></a>1. Zugriff auf die LeaderboardService

Die `LeaderboardService` abgerufen werden können, aus der `XboxLiveContext` erstellt, beim Anmelden bei einem Benutzer das Spiel, Sie benötigen für Leaderboard-Daten aufrufen.

```csharp
XboxLiveContext xboxLiveContext = idManager.xboxLiveContext;
LeaderboardService boardService = xboxLiveContext.LeaderboardService;
```

### <a name="2-call-the-leaderboardservice"></a>2. Rufen Sie die LeaderboardService

```csharp
LeaderboardResult boardResult = await boardService.GetLeaderboardAsync(
     xboxLiveConfig.ServiceConfigurationId,
     leaderboardName
     );
```

### <a name="3-retrieve-leaderboard-data"></a>3. Abrufen von Daten der Bestenliste

`GetLeaderboardAsync()` Gibt eine `LeaderboardResult` enthält die Statistiken, die die benannte Bestenliste auffüllen.

`LeaderboardResult` bietet mehrere Funktionen und Eigenschaften, die das Lesen der Leaderboard-Daten zu ermöglichen.

|Eigenschaft  |Beschreibung  |
|---------|---------|
|Öffentliche IAsyncOperation<LeaderboardResult> GetNextAsync (Uint "MaxItems");     |Ruft den nächsten Satz von Rängen bis zur Anzahl der Parameter "MaxItems" ab. Dies ist im Wesentlichen einen anderen Aufruf `GetLeaderboard()`         |
|public LeaderboardQuery GetNextQuery();     |Ruft ab, der LeaderboardQuery, die zum Treffen des Leaderboard-Aufrufs zum Abrufen von des nächsten Satz von Daten verwendet werden kann.         |
|public bool HasNext { get; }    |Legt fest, und zwar unabhängig davon, ob weitere Bestenliste Zeilen abrufen         |
|Öffentliche IReadOnlyList<LeaderboardRow> Zeilen {Get;}     | Zeilen mit Daten der Bestenliste pro Rang        |
|Öffentliche IReadOnlyList<LeaderboardColumn> Spalten {Get;}     | Die Liste der Spalten, aus denen die Bestenliste        |
|Öffentliche Uint TotalRowCount {Get;}     | Gesamtmenge der Zeilen in die Bestenliste        |
|public string DisplayName { get; }     | Name, der für die Bestenliste angezeigt werden       |

Leaderboard-Daten werden eine Seite zu einem Zeitpunkt bereitgestellt. Sie können die Schleife durchlaufen die `LeaderboardResult` Zeilen und Spalten aus, um die Daten abzurufen.  
Verwenden der `HasNext` booleschen und `GetNextAsync()` Funktion zum späteren Leaderboard-Datenseiten abrufen.

```csharp
if (boardResult != null)
{
    foreach (LeaderboardRow row in boardResult.Rows)
    {
        Debug.Write(string.Format("Rank: {0} | Gamertag: {1} | {2}\n", row.Rank, row.Gamertag, row.Values.ToString()));
    }
}
```

## <a name="leaderboard-2017"></a>Leaderboard-2017

Um die Aufrufe an die Stats 2017 Leaderboard-Dienst ausführen, Sie verwenden, die `StatisticManager` Leaderboard-apis anstelle von der `LeaderboardService` Leaderboard-apis.  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_leaderboard (
     _In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ leaderboard::leaderboard_query query
     ) 
```  

`xbox::services::stats:manager::stats_manager::get_leaderboard`  

```cpp
xbox_live_result< void >  get_social_leaderboard (_In_ const xbox_live_user_t &user,
     _In_ const string_t &statName,
     _In_ const string_t &socialGroup,
     _In_ leaderboard::leaderboard_query query
)
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetLeaderboard`  

```csharp
public void GetLeaderboard(
    XboxLiveUser user,
    string statName,
    LeaderboardQuery query
    )
```  

`Microsoft.Xbox.Services.Statistics.Manager.StatisticManager.GetSocialLeaderboard`  

```csharp
public void GetSocialLeaderboard(
    XboxLiveUser user,
    string statName,
    string socialGroup,
    LeaderboardQuery query
    )
```  

## <a name="2017-c-example"></a>2017 C++-Beispiel

### <a name="1-get-a-singleton-instance-of-the-statsmanager"></a>1. Erhalten Sie eine Singleton-Instanz, der die stats_manager

Bevor Sie aufrufen können, die `stats_manager` Funktionen, die Sie benötigen eine Variable für die Singleton-Instanz festlegen.

```csharp
m_statsManager = stats_manager::get_singleton_instance();
```

### <a name="2-create-a-leaderboardquery"></a>2. Erstellen Sie eine LeaderboardQuery

Die `leaderboard_query` wird bestimmt, wie viel, Reihenfolge und Ausgangspunkt der Daten aus dem Leaderboard-Aufruf zurückgegeben.

Ein `leaderboard_query` hat einige Attribute, die festgelegt werden, können sich Auswirkungen auf die Daten, die zurückgegeben werden:

|Eigenschaft |Beschreibung  |
|---------|---------|
|m_skipResultToRank     |Diese Variable "uint" bestimmt, welche die Rangfolge der Leaderboard-Daten mit gestartet wird, bei der Rückgabe. Rangfolgen beginnen bei Rang 1.         |
|m_skipResultToMe     |Wenn das Festlegen auf diesen booleschen Wert "true" die Leaderboard-Daten zurückgegeben, die zum Starten verursacht, die `XboxLiveUser` in verwendet die `get_leaderboard()` aufrufen.  |
|m_order     |Enumerationen des Typs `xbox::services::leaderboard::sort_order` zwei mögliche Werte auf- und absteigenden haben. Diese Variable für die Abfrage bestimmt die Sortierreihenfolge von Ihrem Bestenliste.        |
|m_maxItems     |Diese "uint" bestimmt die maximale Anzahl von Zeilen, die pro Aufruf zurückgegeben `get_leaderboard` oder `get_social_leaderboard()`.         |

`leaderboard_query` verfügt über mehrere Set-Funktion, die Sie verwenden können, diese Eigenschaften Wert zuweisen. Der folgende Code zeigt Ihnen zum Einrichten Ihrer `leaderboard_query`

```cpp
leaderboard::leaderboard_query leaderboardQuery;
leaderboardQuery.set_skip_result_to_rank(10);
leaderboardQuery.set_max_items(10);
leaderboardQuery.set_order(sort_order::descending);
```

Diese Abfrage würde einzelne zurückgeben zehn Zeilen die Bestenliste, beginnend mit dem 100 eingestuft.

> [!WARNING]
> SkipResultToRank höher als die Anzahl der Spieler die Bestenliste enthaltenen Einstellung bewirkt, dass die Bestenliste Daten mit 0 (null) Zeilen zurückgeben.

### <a name="3-call-getleaderboard"></a>3. Rufen Sie get_leaderboard

```cpp
leaderboard::leaderboard_query leaderboardQuery;
m_statsManager->get_leaderboard(user, statName, leaderboardQuery);
```

> [!IMPORTANT]
> Die `statName` in verwendet die `GetLeaderboard()` Aufruf muss identisch mit den Namen des Status für Ihre Titel im konfiguriert [Partner Center](https://partner.microsoft.com/dashboard), die Groß-/Kleinschreibung beachtet wird.

### <a name="4-read-the-leaderboard-data"></a>4. Lesen Sie die Bestenliste-Daten

Um das Leaderboard-Daten zu lesen, Sie zum Aufrufen benötigen, der `stats_manager::do_work()` Funktion, die eine Liste der zurückgegeben wird `stat_event` Werte. Leaderboard-Daten sind in einem `stat_event` des Typs `stat_event_type::get_leaderboard_complete`. Wenn Sie ein Ereignis dieses Typs in der Liste der stoßen `stat_event`s, die Sie durchsuchen können die `leaderboard_result` innerhalb der `stat_event` für den Datenzugriff.

Beispiel `do_work()` Handler

```cpp
void Sample::UpdateStatsManager()
{
    // Process events from the stats manager
    // This should be called each frame update

    auto statsEvents = m_statsManager->do_work();
    std::wstring text;

    for (const auto& evt : statsEvents)
    {
        switch (evt.event_type())
        {
            case stat_event_type::local_user_added: 
                text = L"local_user_added"; 
                break;

            case stat_event_type::local_user_removed: 
                text = L"local_user_removed"; 
                break;

            case stat_event_type::stat_update_complete: 
                text = L"stat_update_complete"; 
                break;

            case stat_event_type::get_leaderboard_complete: //leaderboard data is read here
                text = L"get_leaderboard_complete";
                auto getLeaderboardCompleteArgs = std::dynamic_pointer_cast<leaderboard_result_event_args>(evt.event_args());
                ProcessLeaderboards(evt.local_user(), getLeaderboardCompleteArgs->result());
                break;
        }

        stringstream_t source;
        source << _T("StatsManager event: ");
        source << text;
        source << _T(".");
        m_console->WriteLine(source.str().c_str());
    }
}
```

Lesen der Leaderboard-Daten aus dem Ergebnis der Bestenliste  

```cpp
void Sample::PrintLeaderboard(const xbox::services::leaderboard::leaderboard_result& leaderboard)
{
    if (leaderboard.rows().size() > 0)
    {
        m_console->Format(L"%16s %6s %12s %12s\n", L"Gamertag", L"Rank", L"Percentile", L"Values");
    }

    for (const xbox::services::leaderboard::leaderboard_row& row : leaderboard.rows())
    {
        string_t colValues;
        for (auto columnValue : row.column_values())
        {
            colValues = colValues + L" ";
            colValues = colValues + columnValue;
        }
        m_console->Format(L"%16s %6d %12f %12s\n", row.gamertag().c_str(), row.rank(), row.percentile(), colValues.c_str());
    }
}
```  

Rufen Sie aus der Bestenliste Datenseiten weiter ab.  

```cpp
void Sample::ProcessLeaderboards(
    _In_ std::shared_ptr<xbox::services::system::xbox_live_user> user,
    _In_ xbox::services::xbox_live_result<xbox::services::leaderboard::leaderboard_result> result
    )
{
    if (!result.err())
    {
        auto leaderboardResult = result.payload();
        PrintLeaderboard(leaderboardResult);

        // Keep processing if there is more data in the leaderboard
        if (leaderboardResult.has_next())
        {
            if (!leaderboardResult.get_next_query().err())
            {               
                auto lbQuery = leaderboardResult.get_next_query().payload();
                if (lbQuery.social_group().empty())
                {
                    m_statsManager->get_leaderboard(user, lbQuery.stat_name(), lbQuery);
                }
                else
                {
                    m_statsManager->get_social_leaderboard(user, lbQuery.stat_name(), lbQuery.social_group(), lbQuery);
                }
            }
        }
    }
}
```  

## <a name="2017-winrt-c-example"></a>2017 WinRT C# Beispiel

### <a name="1-get-a-singleton-instance-of-the-statisticmanager"></a>1. Erhalten Sie eine Singletoninstanz, der die StatisticManager

Bevor Sie aufrufen können, die `StatisticManager` Funktionen, die Sie benötigen eine Variable für die Singleton-Instanz festlegen.

```csharp
statManager = StatisticManager.SingletonInstance;
```

### <a name="2-create-a-leaderboardquery"></a>2. Erstellen Sie eine LeaderboardQuery

Die `LeaderboardQuery` wird bestimmt, wie viel, Reihenfolge und Ausgangspunkt der Daten aus dem Leaderboard-Aufruf zurückgegeben.  

```csharp
public sealed class LeaderboardQuery : __ILeaderboardQueryPublicNonVirtuals
    {
        [Overload("CreateInstance1")]
        public LeaderboardQuery();

        public bool HasNext { get; }
        public string SocialGroup { get; }
        public string StatName { get; }
        public SortOrder Order { get; set; }
        public uint MaxItems { get; set; }
        public uint SkipResultToRank { get; set; }
        public bool SkipResultToMe { get; set; }
    }
```

Ein `LeaderboardQuery` hat einige Attribute, die festgelegt werden, können sich Auswirkungen auf die Daten, die zurückgegeben werden:

|Eigenschaft |Beschreibung  |
|---------|---------|
|SkipResultToRank     |Diese Variable "uint" bestimmt, welche die Rangfolge der Leaderboard-Daten mit gestartet wird, bei der Rückgabe. Rangfolgen beginnen bei Rang 1.         |
|SkipResultToMe     |Wenn das Festlegen auf diesen booleschen Wert "true" die Leaderboard-Daten zurückgegeben, die zum Starten verursacht, die `XboxLiveUser` in verwendet die `GetLeaderboard()` aufrufen.  |
|Reihenfolge     |Enumerationen des Typs `Microsoft.Xbox.Services.Leaderboard.SortOrder` zwei mögliche Werte auf- und absteigenden haben. Diese Variable für die Abfrage bestimmt die Sortierreihenfolge von Ihrem Bestenliste.        |
|"MaxItems"     |Diese "uint" bestimmt die maximale Anzahl von Zeilen, die pro Aufruf zurückgegeben `GetLeaderboard()` oder `GetSocialLeaderboard()`.         |

Und so Ihre `LeaderboardQuery` kann wie folgt aussehen:

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

### <a name="3-call-getleaderboard"></a>3. Rufen Sie GetLeaderboard()

Sie können nun Aufrufen `GetLeaderboard()` mit Ihrem `XboxLiveUser`, den Namen der Statistik, und ein `LeaderboardQuery`.

```csharp
statManager.GetLeaderboard(xboxLiveUser, statName, leaderboardQuery);
```

> [!IMPORTANT]
> Die `statName` in verwendet die `GetLeaderboard()` Aufruf muss identisch mit den Namen des Status für Ihre Titel im konfiguriert [Partner Center](https://partner.microsoft.com/dashboard), die Groß-/Kleinschreibung beachtet wird.

### <a name="4-read-leaderboard-data"></a>4. Read-Leaderboard-Daten

Um das Leaderboard-Daten zu lesen, Sie zum Aufrufen benötigen, der `StatisticManager.DoWork()` Funktion, die eine Liste der zurückgegeben wird `StatisticEvent` Werte. Leaderboard-Daten sind in einem `StatisticEvent` des Typs `GetLeaderboardComplete`. Wenn Sie ein Ereignis dieses Typs in der Liste der stoßen `StatisticEvent`s, die Sie durchsuchen können die `LeaderboardResult` innerhalb der `StatisticEvent` für den Datenzugriff.

```csharp
IReadOnlyList<StatisticEvent> statEvents = statManager.DoWork(); //In practice this should be called every update frame

foreach(StatisticEvent statEvent in statEvents)
{
    if(statEvent.EventType == StatisticEventType.GetLeaderboardComplete
        && statEvent.ErrorCode == 0)
    {
        LeaderboardResultEventArgs leaderArgs = (LeaderboardResultEventArgs)statEvent.EventArgs;
        LeaderboardResult leaderboardResult = leaderArgs.Result;
        foreach(LeaderboardRow leaderRow in leaderboardResult.Rows)
        {
            Debug.WriteLine(string.Format("Rank: {0} | Gamertag: {1} | {2}:{3}\n\n", leaderRow.Rank, leaderRow.Gamertag, "test", leaderRow.Values[0]));
        }
    }
}
```

In Ihrem Code Titel `StatisticManager.DoWork()` sollte verwendet werden, um alle eingehenden Statistik-Manager-Ereignisse behandeln, und nicht nur für Bestenlisten. 

> [!NOTE]
> Zum Abrufen der `LeaderboardResultEventArgs` umwandeln müssen Sie die `StatisticEvent.EventArgs` als eine `LeaderboardResultEventArgs` Variable.

### <a name="5-retrieve-more-leaderboard-data"></a>5. Abrufen von Daten für weitere Bestenliste

Um höhere Datenseiten Bestenliste abzurufen, Sie verwenden müssen, die `LeaderboardResult.HasNext` Eigenschaft und die `LeaderboardResult.GetNextQuery()` Funktion zum Abrufen der `LeaderboardQuery` wird, die Sie die nächste Seite der Daten angezeigt.

```csharp
while (leaderboardResult.HasNext)
{
    leaderboardQuery = leaderboardResult.GetNextQuery();
    statManager.GetLeaderboard(xboxLiveUser, leaderboardQuery.StatName, leaderboardQuery)
    // StatisticManager.DoWork() is called
    // Leaderboard results are read
}
```