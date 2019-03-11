---
title: Durchsuchen von Multiplayer-Sitzungen
description: Informationen Sie zum Durchsuchen von Multiplayer-Sitzungen zu implementieren, indem Sie das Xbox Live Multiplayer.
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.date: 10/16/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 579c71ef9266fb9a1ee4ef0538d1beffec0bb4ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660675"
---
# <a name="multiplayer-session-browse"></a>Durchsuchen von Multiplayer-Sitzungen

Durchsuchen eine neue Funktion, die im November 2016 eingeführten ist, Multiplayer-Sitzung, die einen Titel ein, die Abfrage für eine Liste der geöffneten Sitzungen für Multiplayer-Spiele ermöglicht, die die angegebenen Kriterien erfüllen.

## <a name="what-is-session-browse"></a>Was ist die Sitzung durchsuchen?

In einem Szenario mit Sitzung durchsuchen kann ein Spieler in einem Spiel zum Abrufen einer Liste von beigetreten game-Sitzungen. Jeder Sitzung-Eintrag in dieser Liste enthält einige zusätzliche Metadaten über das Spiel, das ein Player verwenden können, wählen Sie die gewünschte Sitzung erweitern können.  Sie können auch die Liste der Sitzungen, die auf der Grundlage der Metadaten filtern. Sobald der Spieler eine Spiele-Sitzung wird, die Ihnen hostcomputerkonfigurationen attraktiv angezeigt, können sie die Sitzung beitreten.

Ein Player kann auch eine neue Sitzung für die Spiele erstellen und Durchsuchen von Sitzungen zusätzliche Spieler, statt die standardremotedatenbank Vermittlung beschaffen.

Durchsuchen von Sitzungen unterscheidet sich von herkömmlichen Vermittlung Szenarien bei, dass der Spieler die Spiele-Sitzung Self sie in der Vermittlung verknüpfen möchten wählt, der Spieler in der Regel "finden ein Spiel" auf eine Schaltfläche klickt, die versucht, automatisch der Spieler in einem entsprechende Spiel-Sitzung. Beim Durchsuchen von Sitzungen ein manueller und langsamer Vorgang, der nicht immer das Objektiv bewährte Spiel auswählen können ist, bietet mehr Kontrolle für dem Player und wahrgenommen werden können, bestimmen, die die subjectively bessere Spiele zu entwickeln.

Es ist üblich, Spiele sowohl Sitzung durchsuchen und Vermittlung Szenarien einschließt. In der Regel Vermittlung dient häufig Spielmodi, wiedergegeben, während für benutzerdefinierte-Spiele Durchsuchen von Sitzungen verwendet wird.

**Beispiel:** John möglicherweise interessieren, spielen eine Hero-Schlacht Spielbereich Stil Multiplayer-Spiels, möchte jedoch ein Spiel zu spielen, in denen alle Beteiligten ihre Hero nach dem Zufallsprinzip ausgewählt. Er Abrufen einer Liste der offenen game-Sitzungen und finden, die "zufällige Helden" in ihrer Beschreibung enthalten kann oder wenn die Benutzeroberfläche des Spiele es zulässt, er wählen Sie die "zufälligen Hero" Spielmodus und abgerufen werden kann nur die Sitzungen, die markiert sind, um anzugeben, dass sie "RandomHero" Gam sind Es ist.

Wenn er ein Spiel, die er gerne findet, verbindet er das Spiel. Wenn genügend Mitarbeiter die-Sitzung beigetreten sind, kann der Host der game-Sitzung das Spiel zu starten.

### <a name="roles"></a>Rollen

Ein Spiel in Durchsuchen von Sitzungen können Spieler für bestimmte Rollen zu werben. Beispielsweise kann ein Spieler eine Spiele-Sitzung erstellen möchten, die angibt, dass die Sitzung nicht mehr als 5 davon Klassen enthält, aber es muss mindestens 2 Healer Rollen und mindestens 1 Tank enthalten.

Wenn Sie einen anderen Player für die Sitzung angewendet wird, sie können ihre Rolle im Voraus auswählen, und der Dienst lässt nicht zu der Sitzung beizutreten, wenn es sind keine geöffneten Slots für die Rolle, die sie ausgewählt haben.

Ein weiteres Beispiel wäre, wenn ein Player 2 Steckplätze für seine Freunde zum join zu reservieren möchte, das Spiel kann eine Rolle "Friends" geben an, und nur Player, die friends mit dem Sitzungshost können sich füllen die 2 Steckplätze, die speziell für die Rolle "Friends".

Weitere Informationen zu Rollen finden Sie unter [Multiplayer-Rollen](multiplayer-roles.md).



## <a name="how-does-session-browse-work"></a>Wie funktioniert das Durchsuchen von Sitzungen?

Durchsuchen von Sitzungen kann in erster Linie für die Verwendung bei der Suche. Ein Handle für die Suche ist ein Datenpaket, das einen Verweis auf die Sitzung als auch zusätzliche Metadaten über die Sitzung, nämlich die Search-Attribute enthält.

Wenn erstellt ein Titel suchen eine neue Spiele-Sitzung, die für die Sitzung berechtigt ist, ein Handle für die Suche für die Sitzung erstellt. Das Suchhandle wird in Multiplayer Service Verzeichnis (MPSD) gespeichert, und die Suche Handles für den Titel verwaltet.

Wenn Sie ein Titel muss zum Abrufen einer Liste von Sitzungen, kann der Titel eine Suchabfrage an MPSD, senden, hierbei eine Liste der bei der Suche zurückgegeben, die den Suchkriterien entsprechen. Der Titel kann dann die Liste der Sitzungen verwenden, eine Liste der beigetreten werden kann, Spiele, die der Player angezeigt.

Wenn eine Sitzung voll ist, oder kann nicht verknüpft werden, kann ein Titel das Suchhandle MPSD aufheben, damit die Sitzung nicht mehr in Sitzung durchsuchen Abfragen angezeigt werden.

>[!NOTE]
> Bei der Suche werden bei der Anzeige einer Liste von Sitzungen verwendet werden, um zu einem Benutzer angezeigt werden sollen. Verwenden Sie bei der Suche für die Vermittlung der Hintergrund ist nicht gültig, und erwägen Sie stattdessen die Verwendung [SmartMatch](multiplayer-manager/play-multiplayer-with-matchmaking.md)

## <a name="set-up-a-session-for-session-browse"></a>Einrichten einer Sitzungs für das Durchsuchen von Sitzungen

Um bei der Suche für eine Sitzung zu verwenden, müssen die Sitzung die folgenden Funktionen auf "true" festgelegt ist:

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> Die `userAuthorizationStyle` -Funktion ist nur für UWP-Spiele erforderlich sind, wird jedoch empfohlen implementieren diese für alle Xbox Live Spiele, insbesondere Spiele xdk-Version, da zukünftige Portabilität gewährleistet wird.

>[!NOTE]
> Festlegen der `userAuthorizationStyle` Funktion Standardwerte der `readRestriction` und `joinRestriction` Sitzungsverhalten `local` anstelle von `none`. Dies bedeutet, dass der Titel müssen bei der Suche verwenden oder übertragen Handles Spiele Sitzung nicht beitreten.

Sie können diese Funktionen in der sitzungsvorlage festlegen, wenn Sie Ihre Xbox Live-Dienste konfigurieren.

Für das Durchsuchen von Sitzungen sollten Sie bei der Suche nur in Sitzungen erstellen, die für die eigentliche Spiel, nicht für Lobby Sitzungen verwendet werden.

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>Was bedeutet es, ein Besitzer einer Sitzung sein?

Während viele Spiel Sitzung Typen, z. B. SmartMatch oder ein nur Spiele, Freunde nicht Besitzer erforderlich sind, für Sitzungen, die Sitzung durchsuchen möchten Sie möglicherweise keinen Besitzer haben. 

Müssen eine Besitzer verwaltete Sitzung hat einige Vorteile für den Besitzer. Besitzer können andere Mitglieder aus der Sitzung entfernen oder ändern Sie den Besitzstatus von anderen Elementen.

Um Besitzern für eine Sitzung zu verwenden, müssen die Sitzung die folgenden Funktionen auf "true" festgelegt ist:

* `hasOwners`

Wenn Sie einen Besitzer einer Sitzung einen Xbox Live-Member blockiert hat, kann nicht diesen Member der Sitzung beizutreten.

Bei Verwendung [Multiplayer-Rollen](multiplayer-roles.md), Sie können festlegen, es also nur der Besitzer von Benutzern Rollen zuweisen können.

Wenn alle Besitzer lassen Sie eine Sitzung, und der Dienst führt die Aktionen in der Sitzung, die basierend der `ownershipPolicy.migration` Richtlinie, die für die Sitzung definiert ist. Wenn die Richtlinie "ältesten" ist, wird der Player, der in der Sitzung die am längsten wurde der neue Besitzer werden festgelegt. Wenn die Richtlinie "Endsession" (die Standardeinstellung, wenn keine Angabe erfolgt) ist, kann der Dienst wird die Sitzung beendet und entfernt alle verbleibenden Spieler aus der Sitzung.


## <a name="search-handles"></a>Bei der Suche

Ein Handle für die Suche wird in MSPD als eine JSON-Datenstruktur gespeichert. Zusätzlich zu, die einen Verweis auf die Sitzung enthält, enthalten die bei der Suche auch zusätzliche Metadaten für die Suche, Suche Attribute genannt.

Eine Sitzung kann nur eine Suchhandle, die zu einem beliebigen Zeitpunkt erstellt haben.

Um ein Handle für die Suche mithilfe der Xbox Live-APIs für eine Sitzung in zu erstellen, erstellen Sie zuerst eine `multiplayer::multiplayer_search_handle_request` Objekt, und übergeben Sie dann das Objekt mit der `multiplayer::multiplayer_service::set_search_handle()` Methode.

### <a name="search-attributes"></a>Durchsuchen

Durchsuchen umfasst die folgenden Komponenten:

`tags` -Tags sind Zeichenfolgendeskriptoren, die von Personen verwendet werden kann, um eine Spiele-Sitzung, ähnlich einem Hashtag zu kategorisieren. Tags müssen mit einem Buchstaben beginnen, darf keine Leerzeichen enthalten und müssen weniger als 100 Zeichen sein.
Beispiel-Tags: "ProRankOnly", "norocketlaunchers", "cityMaps".

`strings` -Zeichenfolgen werden Variablen vom Typ Text, und die Zeichenfolgennamen müssen mit einem Buchstaben beginnen, darf keine Leerzeichen enthalten und müssen weniger als 100 Zeichen sein.

Beispiel für Zeichenfolge-Metadaten: "Waffen"="Messer + abgefeuert + als auch", "MapName" = "UrbanCityAssault", "Beschreibung"="viel Spaß beim freizeitspiel, neue Mitarbeiter Willkommen."

`numbers` -Zahlen sind numerische Variablen, und die Anzahl Namen müssen mit einem Buchstaben beginnen, darf keine Leerzeichen enthalten und müssen weniger als 100 Zeichen sein. Die Xbox Live-APIs abgerufen werden numerische Werte als Typ "float".

Beispiel-Number-Metadaten: "MinLevel" = 25, "MaxRank" = 10.

>**Hinweis:** Groß-oder Kleinbuchstaben von Tags und Zeichenfolgenwerten im Dienst beibehalten wird, aber Sie müssen die ToLower()-Funktion verwenden, beim Abfragen von Tags. Dies bedeutet, dass es sich bei Tags und Zeichenfolgenwerte derzeit alle behandelt, als Kleinbuchstaben sind, auch wenn sie Großbuchstaben enthalten.

In den Xbox Live-APIs können Sie die Suche Attribute festlegen, mit der `set_tags()`, `set_stringsmetadata()`, und `set_numbers_metadata()` Methoden eine `multiplayer_search_handle_request` Objekt.


### <a name="additional-details"></a>Zusätzliche Details

Wenn Sie ein Handle für die Suche abrufen, enthalten die Ergebnisse auch weitere nützliche Daten über die Sitzung an, z. B. als ob die Sitzung geschlossen wird, gibt es Join Einschränkungen auf die Sitzung usw.

In den Xbox Live-APIs sind in diesen Informationen können zusammen mit den Attributen suchen, enthalten die `multiplayer_search_handle_details` nach einer Abfrage zurückgegeben werden.

### <a name="remove-a-search-handle"></a>Entfernen Sie ein Suchhandle

Wenn Sie die Sitzung Durchsuchen "," z. B. wenn die Sitzung voll ist, wird eine Sitzung aufheben möchten, oder wenn die Sitzung geschlossen wird, können Sie das Suchhandle löschen.

In den Xbox Live-APIs können Sie die `multiplayer_service::clear_search_handle()` Methode, um ein Handle für die Suche zu entfernen.

### <a name="example-create-a-search-handle-with-metadata"></a>Beispiel: Erstellen Sie ein Handle für die Suche mit Metadaten

Der folgende Code zeigt, wie Sie ein Handle für die Suche mithilfe der C++ Xbox Live Multiplayer-APIs für eine Sitzung zu erstellen.

```cpp
auto searchHandleReq = multiplayer_search_handle_request(sessionBrowseRef);

searchHandleReq.set_tags(std::vector<string_t> val);
searchHandleReq.set_numbers_metadata(std::unordered_map<string_t, double> metadata);
searchHandleReq.set_strings_metadata(std::unordered_map<string_t, string_t> metadata);

auto result = xboxLiveContext->multiplayer_service().set_search_handle(searchHandleReq)
.then([](xbox_live_result<void> result)
{
  if (result.err())
  {
    // handle error
  }
});
```


## <a name="create-a-search-query-for-sessions"></a>Erstellen Sie eine Suchabfrage für Sitzungen

Wenn Sie eine Liste der bei der Suche abrufen zu können, können Sie eine Suchabfrage, um die Ergebnisse an die Sitzungen zu beschränken, die bestimmte Kriterien erfüllen.

Die Suchsyntax für die Abfrage ist ein [OData](https://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) formatieren Sie die Syntax, mit nur den folgenden Operatoren unterstützt:

 Operator | Beschreibung
 --- | ---
 eq | Ist gleich
 ne | Ungleich
 gt | Größer als
 ge | Größer als oder gleich
 lt | Kleiner als
 LE | Kleiner oder gleich
 und | Logisches AND
 oder | logisches oder (siehe Hinweis unten)

Sie können auch Lambda-Ausdrücke und die `tolower` Funktion (kanonisch). Derzeit werden keine anderen OData-Funktionen unterstützt.

Bei der Suche nach Tags oder Zeichenfolgenwerte, müssen Sie die Funktion "Tolower" in die Suchabfrage verwenden, wie der Dienst nur aktuell unterstützt das Suchen nach Zeichenfolgen mit Kleinbuchstaben.

Die Xbox Live-Dienst gibt nur die ersten 100 Ergebnisse, die mit die Suchabfrage übereinstimmen. Ihr Spiel sollte Spieler, um ihre Suchabfrage verfeinern, wenn die Ergebnisse zu weit gefasst sind, ermöglichen.

>[!NOTE]
>  Logisches OR werden in Filterabfragen-Zeichenfolge unterstützt. aber jeweils nur ein OR zulässig ist, und es muss sich im Stammverzeichnis Ihrer Abfrage handeln. Sie können nicht mehrere or haben, in der Abfrage, noch können Sie eine Abfrage erstellen, der ergibt, oder wird nicht oben in den meisten Maß an die Abfragestruktur.

### <a name="search-handle-query-examples"></a>Beispiele für die Suche handle

In einer restful-Aufruf ist "Filter", in dem Sie eine Sprachenzeichenfolge OData-Filter angeben würden, die in der Abfrage für alle Suchhandler ausgeführt wird.  
In den Multiplayer-2015-APIs, können Sie die Suchfilterzeichenfolge im Angeben der *SearchFilter* Parameter der `multiplayer_service.get_search_handles()` Methode.  

Derzeit werden die folgenden filterszenarien unterstützt:

 Filtern nach | Suchfilterzeichenfolge
 --- | ---
 Ein einzelnes Element Xuid "1234566" | "MemberXuids/Sitzung/any(d:d eq '1234566')"
 Ein Benutzer im Besitz Xuid "1234566" | "OwnerXuids/Sitzung/any(d:d eq '1234566')"
 Eine Zeichenfolge "Forzacarclass' gleich"Classb" | "tolower(strings/forzacarclass) Eq"Classb""
 Eine Zahl 'Forzaskill' gleich 6 | "Zahlen/Forzaskill Eq 6"
 Eine Zahl 'Halokdratio"größer als 1.5 | "Zahlen/Halokdratio Gt 1.5"
 Ein Tag "Coolpeopleonly" | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 Sitzungen, die das Tag 'Cursingallowed' nicht enthalten | "tags/any(d:tolower(d) ne 'cursingallowed')"
 Sitzungen, die eine Zahl, die "rank" nicht enthalten ist gleich 0 | "Zahlen/des Rangs Ne 0"
 Sitzungen, die eine Zeichenfolge 'Forzacarclass' nicht enthalten, die ist gleich 'Classa' | "tolower(strings/forzacarclass) Ne 'Classa'"
 Ein Tag "Coolpeopleonly" und eine Reihe "Halokdratio' gleich 7.5 | "tags/any(d:tolower(d) Eq 'Coolpeopleonly') Eq" true ", und Zahlen/Halokdratio Eq 7.5"
 Eine Zahl 'Halodkratio"größer als oder gleich 1.5, eine Reihe"rank"weniger als 60 und eine Anzahl 'Customnumbervalue" kleiner als oder gleich 5 | "Zahlen/Halokdratio ge 1.5 und Zahlen/des Rangs Lt 60 und Zahlen/Customnumbervalue le 5"
 Eine Auszeichnung-Id "123456" | "AchievementIds/any(d:d eq '123456')"
 Der Sprachcode "En" | "Sprache Eq 'En'"
 Gibt alle geplanten geplanten Zeitpunkt ein Timeout kleiner oder gleich der angegebenen Zeit | "session/scheduledTime le '2009-06-15T13:45:30.0900000Z'"
 Bereitgestellte Zeit, gibt alle gebucht Mal kleiner als die angegebene Zeit | "Sitzung /" postedtime "Lt" 2009-06-15T13:45:30.0900000Z ""
 Sitzungsstatus-Registrierung | "session/registrationState eq 'registered'"
 Wenn die Anzahl der Elemente der Sitzung, die gleich 5 ist | "Sitzung/MembersCount Eq 5"
 In denen die Zielanzahl der Session-Element größer als 1 ist. | "Sitzung/TargetMembersCount Gt 1"
 Wobei kleiner als 3 ist die maximale Anzahl von Elementen der Sitzung | "Sitzung/MaxMembersCount Lt 3"
 Unterscheiden sich die Anzahl der Sitzungen-Member-Ziel und die Anzahl der Elemente der Sitzung kleiner als oder gleich 5 ist | "Sitzung/TargetMembersCountRemaining le 5"
 In denen der Unterschied zwischen die maximale Anzahl von Elementen der Sitzung und die Anzahl der Elemente der Sitzung größer als 2 ist. | "Sitzung/MaxMembersCountRemaining Gt 2"
 Wobei unterscheiden sich die Anzahl der Sitzungen-Member-Ziel und die Anzahl der Elemente der Sitzung kleiner als oder gleich 15 ist.</br> Wenn die Rolle nicht über ein angegebenes Ziel besitzt, werden diese Abfrage für unterscheiden sich die maximale Anzahl von Elementen der Sitzung und die Anzahl der Elemente der Sitzung gefiltert. | "Sitzung/benötigt le 15"
 Rolle "bestätigt", der die, in dem die Anzahl der Elemente mit dieser Rolle gleich 5 ist, Rolle Typ "Lfg" | "/ Rollen/Lfg/bestätigt/Anzahl der Sitzungen Eq 5"
 Die Rolle "bestätigt", der die Rolle Typ "Lfg auf", in dem das Ziel dieser Rolle größer als 1 ist.</br> Wenn die Rolle nicht über ein angegebenes Ziel besitzt, wird die maximale Anzahl der Rolle stattdessen verwendet. | "/ Rollen/Lfg/bestätigt/Sitzungsziel Gt 1"
 Rolle "bestätigt" der Rolle geben Sie "Lfg", in denen der Unterschied zwischen dem Ziel der Rolle und die Anzahl der Elemente mit dieser Rolle kleiner als oder gleich 15 ist.</br> Wenn die Rolle nicht über ein angegebenes Ziel besitzt, werden diese Abfrage anhand der Unterschied zwischen den Maximalwert für die Rolle und die Anzahl der Elemente mit dieser Rolle gefiltert. | "/ Rollen/Lfg/bestätigt/sitzungsanforderungen le 15"
 Alle suchen Handles, die auf eine Sitzung mit einem bestimmten Schlüsselwort verweisen | "session/keywords/any(d:tolower(d)-Eq"level2")"
 Alle suchen Handles, die auf eine Sitzung, die zu einem bestimmten scid gehören verweisen | "session/scid eq '151512315'"
 Alle suchen-Handles, die auf eine Sitzung zu verweisen, die einen bestimmten Namen verwendet. | "session/templateName eq 'mytemplate1'"
 Alle suchen Handles, die das Tag 'Elite' oder verfügen über eine Reihe "Vorgabe" größer als 15 und "Clan" gleich "Violett" Zeichenfolge | "tags/any(a:tolower(a) Eq"Elite") oder Anzahl/Vorgabe Gt 15 und Zeichenfolge /" Clan "-Eq"Violett""

### <a name="refreshing-search-results"></a>Aktualisieren der Suchergebnisse

 Ihr Spiel sollten vermeiden, wird eine Liste der Sitzungen automatisch aktualisiert, sondern stattdessen bereitstellen Benutzeroberfläche, die einen Player, um die Liste manuell zu aktualisieren, (ggf. nach verfeinern die Suchkriterien zum Filtern Sie die Ergebnisse besser) kann.

 Wenn ein Spieler versucht, die eine Sitzung beitreten, aber diese Sitzung vollständige oder geschlossen ist, sollte Ihr Spiel die Suchergebnisse auch aktualisieren.

 Zu viele Aktualisierungen der Suche können zu Drosselung des Diensts führen, damit Ihre Titel die Rate beschränken soll, an der die Abfrage aktualisiert werden kann.

 Um Service Anrufaufkommen zu reduzieren, sind bei der Suche benutzerdefinierte Eigenschaften, die zum Speichern und Abfragen von schnell veränderlichen Attribute der Sitzung verwendet werden kann. Solche Attribute sollten nicht in Search Attributen gespeichert werden.

### <a name="example-query-for-search-handles"></a>Beispiel:-Abfrage für Suchhandler

 Der folgende Code zeigt, wie bei der Suche abgefragt wird. Die API gibt eine Auflistung von `multiplayer_search_handle_details` Objekte, die alle Suchhandler darstellen, die der Abfrage entsprechen.

```cpp
 auto result = multiplayer_service().get_search_handles(scid, template, orderBy, orderAscending, searchFilter)
 .then([](xbox_live_result<std::vector<multiplayer_search_handle_details>> result)
 {
   if (result.err())
   {
      // handle error
   }
   else
   {
      // parse result.payload
   }
 });

 /* Payload element properties

 multiplayer_search_handle_details
 {
   string_t& handle_id();
   multiplayer_session_reference& session_reference();
   std::vector<string_t>& session_owner_xbox_user_ids();
   std::vector<string_t>& tags();
   std::unordered_map<string_t, double>& numbers_metadata();
   std::unordered_map<string_t, string_t>& strings_metadata();
   std::unordered_map<string_t, multiplayer_role_type>& role_types();
 }
 */
```

## <a name="join-a-session-by-using-a-search-handle"></a>Beitreten Sie zu einer Sitzung mit einem Suchhandle

Nachdem Sie ein Handle für die Suche für eine Sitzung, die Sie hinzufügen möchten abgerufen haben, sollten Titel verwenden `MultiplayerService::WriteSessionByHandleAsync()` oder `multiplayer_service::write_session_by_handle()` selbst zur Sitzung hinzufügen.

> [!NOTE]
> Die `WriteSessionAsync()` und `write_session()` Methoden können nicht verwendet werden, um eine Sitzung durchsuchen-Sitzung teilnehmen.

Der folgende Code veranschaulicht, wie eine Sitzung nach dem Abrufen eines Handles für die Suche zu verknüpfen.

```cpp
void Sample::BrowseSearchHandles()
{
    auto context = m_liveResources->GetLiveContext();
    context->multiplayer_service().get_search_handles(...)
    .then([this](xbox_live_result<std::vector<multiplayer_search_handle_details>> searchHandles)
    {
        if (searchHandles.err())
        {
            LogErrorFormat( L"BrowseSearchHandles failed: %S\n", searchHandles.err_message().c_str() );
        }
        else
        {
            m_searchHandles = searchHandles.payload();

            // Join the game session

            auto handleId = m_searchHandles.at(0).handle_id();
            auto sessionRef = multiplayer_session_reference(m_searchHandles.at(0).session_reference());
            auto gameSession = std::make_shared<multiplayer_session>(m_liveResources->GetLiveContext()->xbox_live_user_id(), sessionRef);
            gameSession->join(web::json::value::null(), false, false, false);

            context->multiplayer_service().write_session_by_handle(gameSession, multiplayer_session_write_mode::update_existing, handleId)
            .then([this, sessionRef](xbox_live_result<std::shared_ptr<multiplayer_session>> writeResult)
            {
                if (!writeResult.err())
                {
                    // Join the game session via MPM
                    m_multiplayerManager->join_game(sessionRef.session_name(), sessionRef.session_template_name());
                }
            });
        }
    });
}
```
