---
title: Einführung in sozialen Netzwerken-Manager
description: Erfahren Sie, bis die Xbox Live Social-Manager-API zum Nachverfolgen online-Freunde.
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.date: 03/26/2018
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5dff3dcfd79fe43ff8af1513a4358bd0ff98b8d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609005"
---
# <a name="introduction-to-social-manager"></a>Einführung in sozialen Netzwerken-Manager

## <a name="description"></a>Beschreibung

Xbox Live bietet umfassende social Graph, die Titel für verschiedene Szenarien verwenden können.

Mithilfe der social APIs in den Xbox Live-API (XSAPI) abrufen und Verwalten von Informationen zu social Graph ist komplex, und diese Informationen auf dem neuesten Stand bleiben kann kompliziert sein.  Dies nicht richtig durchgeführt, kann dies zu Leistungsproblemen, veraltete Daten oder Drosselung aufgrund von Aufrufen von Diensten Social Xbox Live häufiger als nötig führen.

Der US-Sozialversicherungsnummern Manager löst dieses Problem durch:

* Erstellen eine einfache API aufrufen
* Erstellen von auf dem neuesten Stand mit den Real-Time-aktivitätsdienst hinter den Kulissen Informationen
* Entwickler können die Social-Manager-API, ohne jede zusätzliche Belastung für den Dienst synchron aufrufen.

Der US-Sozialversicherungsnummern Manager verbirgt die Komplexität des Umgangs mit mehreren RTA-Abonnements, und Aktualisieren von Daten für Benutzer und ermöglicht es Entwicklern, die einfach auf dem neuesten Stand Diagramm erhalten, das Sie interessante Szenarios erstellen möchten.

Um einen Überblick über die Social-Manager-Speicher und Leistung Merkmale betrachten [Social-Manager-Speicher und Leistung (Übersicht)](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>Features

Der US-Sozialversicherungsnummern-Manager bietet die folgenden Features:

* Vereinfachte Social-API
* Auf dem neuesten Stand social graph
* Die Kontrolle über den Ausführlichkeitsgrad der angezeigten Informationen
* Reduzieren der Anzahl der Aufrufe von Xbox Live-Dienste
  * Dies entspricht direkt insgesamt eine Verringerung der Wartezeiten in der Datenerfassung
* Thread-sicher
* Effizient Daten, die auf dem neuesten Stand halten

## <a name="core-concepts"></a>Core-Konzepte

**Soziale Diagramm**: Ein *social Graph* für einen lokalen Benutzer auf dem Gerät erstellt wird. Dies erstellt eine Struktur, die Informationen für alle von einem Benutzer Freunden auf dem neuesten Stand hält.

> [!NOTE]
> Auf Windows möglich gibt es nur einen lokaler Benutzer

**Xbox-Social Benutzer**: Ein *Xbox soziale Benutzer* ein vollständiger Satz von Daten aus sozialen Medien mit einem Benutzer aus einer Gruppe verknüpft ist

**Xbox Social-Benutzergruppe**: Eine Gruppe ist eine Sammlung von Benutzern, die für Aufgaben wie das Auffüllen der Benutzeroberfläche verwendet wird. Es gibt zwei Arten von Gruppen

* **Filtergruppen**: Eine Filtergruppe wird eine (aufrufen) des lokalen Benutzers *social Graph* und gibt einen konsistent neuen Satz von Benutzern basierend auf den angegebenen Filter-Parameter
* **Benutzergruppen**: Eine Benutzergruppe akzeptiert eine Liste von Benutzern und gibt eine ständig aktualisierte Ansicht dieser Benutzer. Diese Benutzer können außerhalb eines Benutzers Freundesliste sein.

Damit bleibt ein *soziale Benutzergruppe* bis zu die Funktion date `social_manager::do_work()` muss jedes Bild aufgerufen werden.

## <a name="api-overview"></a>API-Übersicht

Sie können die am häufigsten die folgenden wichtigsten Klassen:

### <a name="social-manager"></a>Soziale Medien-Manager

* C++-API-Klassenname: Social_manager
* WinRT (C#) API-Klassenname: [SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

Dies ist eine Singletonklasse, die verwendet werden kann, zum Abrufen **Xbox soziale Benutzergruppen** die Ansichten oben beschrieben werden.

Der US-Sozialversicherungsnummern Manager bleibt Xbox soziale Benutzergruppen auf dem neuesten Stand und Benutzergruppen durch Vorhandensein oder Beziehung, die dem Benutzer filtern.  Beispielsweise könnte eine Xbox social-Benutzergruppe, die alle für die Freunde des Benutzers, die online sind und diese wiedergeben des Titels der aktuellen erstellt werden.  Dies würde auf dem neuesten Stand aufzubewahren wie Freunde starten oder Beenden der Wiedergabe des Titels.

### <a name="xbox-social-user-group"></a>Soziale Xbox-Benutzergruppe

* C++-API-Klassenname: Xbox_social_user_group
* WinRT (C#) API-Klassenname: [XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

Eine Gruppe von Benutzern, die bestimmten Kriterien entsprechen, wie oben beschrieben. Soziale Benutzergruppen Xbox verfügbar machen, welche Art von einer Gruppe, sie sind, welche Benutzer nachverfolgt werden, oder wie der Filter so festgelegt, und den lokalen Benutzer ab, der die Gruppe zugehörig, ist.

Sie finden eine vollständige Beschreibung sozialen-Manager-APIs in der [Xbox Live-API-Referenz](https://aka.ms/xboxliveuwpdocs).
Sie finden auch die WinRT-APIs in der [Microsoft.Xbox.Services.Social.Manager.Namespace-Dokumentation](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01)

## <a name="usage"></a>Verwendungszweck

### <a name="creating-a-social-user-group-from-filters"></a>Erstellen eine soziale Benutzergruppe von Filtern

In diesem Szenario möchten Sie eine Liste der Benutzer von einem Filter ein, z. B. die Personen, die dieser Benutzer folgt oder als Favoriten gekennzeichnet sind.

#### <a name="source-example-using-the-c-api"></a>Source-Beispiel mit der C++-API

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Datenquelle wird mit der C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}

```

#### <a name="events-returned"></a>Zurückgegebene Ereignisse

`local_user_added`(C++) | `LocalUserAdded`(C#) – Wird ausgelöst, wenn das Laden des social Graph Benutzer abgeschlossen ist. Wird angezeigt, ob der Fehler während der Initialisierung aufgetreten sind.

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#) – Wird ausgelöst, wenn es sich bei sozialen Benutzergruppe erstellt wurde

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#) – Wird ausgelöst, wenn Benutzer in geladen werden

#### <a name="additional-details"></a>Zusätzliche Details

Im obige Beispiel zeigt, wie Social-Manager für einen Benutzer initialisiert, erstellen Sie eine soziale Benutzergruppe für diesen Benutzer, und bewahren Sie sie auf dem neuesten Stand.

Filteroptionen finden Sie in der `presence_filter` und `relationship_filter` Enumerationen

In die Spieleschleife die `do_work` Funktion aktualisiert alle erstellte Ansichten mit der neuesten Momentaufnahme in dieser Gruppe Benutzer.

Die Benutzer in der Sicht abgerufen werden können, durch den Aufruf der `xbox_social_user_group::get_users()` Funktion, die eine Liste der zurückgibt `xbox_social_user` Objekte.  Die `xbox_social_user` der soziale Informationen wie z. B. Gamertag, Gamerpic Uri usw. enthält.

### <a name="create-and-update-a-social-user-group-from-list"></a>Erstellen Sie und aktualisieren Sie eine soziale Benutzergruppe aus

In diesem Szenario möchten Sie die soziale Informationen eine Liste der Benutzer z. B. Benutzer in einer Multiplayer-Sitzung.

#### <a name="source-example-using-the-c-api"></a>Source-Beispiel mit der C++-API

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_list(
    xboxLiveContext->user(),
    userList  // this is a std::vector<string_t> (list of xuids)
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>Datenquelle wird mit der C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromList(
     xboxLiveContext.User,
     userList //this is a IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
    );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Zurückgegebene Ereignisse

`local_user_added`(C++) | `LocalUserAdded`(C#) – Wird ausgelöst, wenn das Laden des social Graph Benutzer abgeschlossen ist. Wird angezeigt, ob der Fehler während der Initialisierung aufgetreten sind.

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#) – Wird ausgelöst, wenn es sich bei sozialen Benutzergruppe erstellt wurde

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#) – Wird ausgelöst, wenn Benutzer in geladen werden

### <a name="updating-social-user-group-from-list"></a>Social-Benutzergruppe aus der Liste wird aktualisiert.

Sie können auch die Liste der nachverfolgten Benutzer in der Benutzergruppe für soziale Netzwerke ändern, durch den Aufruf von update_social_user_group()

#### <a name="source-example-using-the-c-api"></a>Source-Beispiel mit der C++-API

```cpp
//#include "Social.h"

socialManager->update_social_user_group(
    xboxSocialUserGroup,
    newUserList    // std::vector<string_t> (list of xuids)
    );

    while(true)
    {
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
    }
```

#### <a name="source-example-using-the-c-api"></a>Datenquelle wird mit der C# API

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.UpdateSocialUserGroup(
     xboxSocialUserGroup,
     newUserList //IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>Zurückgegebene Ereignisse

`social_user_group_updated`(C++) | `SocialUserGroupUpdated`(C#) – Wird ausgelöst, wenn es sich bei sozialen User Group Aktualisierung abgeschlossen ist.

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#) – Wird ausgelöst, wenn Benutzer in geladen werden. Wenn Benutzer über der Liste hinzugefügt wurden bereits im Diagramm vorhanden sind, wird dieses Ereignis nicht ausgelöst.

### <a name="using-social-manager-events"></a>Verwenden Social-Manager-Ereignisse

Soziale Medien-Manager wird auch informieren, was in Form von Ereignissen aufgetreten ist.  Sie können diese Ereignisse verwenden, aktualisieren die Benutzeroberfläche oder andere Logik ausführen.

#### <a name="source-example-using-the-c-api"></a>Source-Beispiel mit der C++-API

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();
socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::no_extra_detail
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

socialManager->do_work();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    auto events = socialManager->do_work();
    for(auto evt : events)
    {
        auto affectedUsersFromGraph = socialUserGroup->get_users_from_xbox_user_ids(evt.users_affected());
        // TODO: render the changes to the friends list using game UI, passing in affectedUsersFromGraph
    }
}
```

##### <a name="source-example-using-the-c-api"></a>Datenquelle wird mit der C# API

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

socialManager.DoWork();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    IReadOnlyList<SocialEvent> Events = socialManager.DoWork();
    IReadOnlyList<XboxSocialUser> affectedUsersFromGraph;
    foreach(SocialEvent managerEvent in Events)
    {
        affectedUsersFromGraph = socialUserGroup.GetUsersFromXboxUserIds(managerEvent.UsersAffected);
    }
}

```

#### <a name="events-returned"></a>Zurückgegebene Ereignisse

`local_user_added`(C++) | `LocalUserAdded`(C#) – Wird ausgelöst, wenn das Laden des social Graph Benutzer abgeschlossen ist. Wird angezeigt, ob der Fehler während der Initialisierung aufgetreten sind.

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#) – Wird ausgelöst, wenn es sich bei sozialen Benutzergruppe erstellt wurde

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#) – Wird ausgelöst, wenn Benutzer in geladen werden

#### <a name="additional-details"></a>Zusätzliche Details

Dieses Beispiel zeigt einige zusätzliche Kontrolle angeboten.  Statt auf der sozialen benutzergruppenfilter Benutzerliste neue während der die spielschleife angeben, wird die soziale Diagramm außerhalb die spielschleife initialisiert.  Und klicken Sie dann auf der Titel beruht die `events` zurückgegebenes der `socialManager->do_work()` Funktion.  `events` eine Liste der `social_event`, und jede `social_event` enthält eine Änderung an soziale Diagramm, die während der letzten Frame aufgetreten sind.  Z. B. `profiles_changed`, `users_added`usw.  Weitere Informationen finden Sie der `social_event` -API-Dokumentation.

### <a name="cleanup"></a>Bereinigen

#### <a name="cleaning-up-social-user-groups"></a>Bereinigen von Benutzergruppen für soziale Netzwerke

```cpp
//#include "Social.h"

socialManger->destroy_social_user_group(
    socialUserGroup
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.DestroySocialUserGroup(
     socialUserGroup
     );
```

Löscht die sozialen Benutzergruppe, die erstellt wurde. Aufrufer sollten auch alle Verweise entfernen, die sie für jede erstellte social Benutzergruppe haben, da sie nun veraltete Daten enthält.

#### <a name="cleaning-up-local-users"></a>Bereinigen von lokale Benutzer

```cpp
//#include "Social.h"

socialManger->remove_local_user(
    xboxLiveContext->user()
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.RemoveLocalUser(
     xboxLiveContext.User
     );
```

Entfernen lokaler Benutzer entfernt Benutzer geladen soziale Diagramm als auch alle sozialen Benutzergruppen, die mit diesem Benutzer erstellt wurden.

#### <a name="events-returned"></a>Zurückgegebene Ereignisse

`local_user_removed`(C++) | `LocalUserRemoved`(C#) – Wird ausgelöst, wenn ein lokaler Benutzer wurde erfolgreich entfernt wurde