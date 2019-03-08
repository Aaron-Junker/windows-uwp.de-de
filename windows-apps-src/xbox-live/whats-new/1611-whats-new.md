---
title: Neuerungen für die Xbox Live SDK – November 2016
description: Neuerungen für die Xbox Live SDK – November 2016
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4a9efeec63399916444de9ba33d4e587a914f2a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658565"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>Neuerungen für die Xbox Live SDK – November 2016

Finden Sie unter den [– August 2016 Neuigkeiten](1608-whats-new.md) Artikel Neuheiten in der Version vom August 2016.

## <a name="xbox-services-api"></a>Xbox-Services-API

### <a name="simplified-achievements"></a>Vereinfachte Erfolge

* [Vereinfachte Erfolge](../achievements-2017/simplified-achievements.md) sind eine neue und einfachere Möglichkeit zum Konfigurieren und verwenden die Erfolge.  Sie müssen nicht mehr zum Senden von Ereignissen und bieten die Xbox Live-Dienste entscheiden, ob Ihre Auszeichnung entsperrt wird.  Titel des ist verantwortlich für diese Entscheidung.
* Wenn Sie Teil der frühen Pilotversion Erfolge vereinfacht wurde, haben wir auch offline-Unterstützung hinzugefügt.
* Es gibt ein neues Beispiel namens SimplifiedAchievements, die aufzeigen, wie einfach es ist, verwenden.

### <a name="multiplayer"></a>Multiplayer

* [Durchsuchen von Sitzungen](../multiplayer/session-browse.md) ist eine neue Möglichkeit für Ihre Benutzer ein Multiplayer-Spiels zu finden.  Durchsuchen von Sitzungen kann Spieler, um eine Liste der geöffneten Sitzungen für Multiplayer-Spiele durchsuchen, die bestimmte Kriterien erfüllen.
* Die [Multiplayer-Manager](../multiplayer/multiplayer-manager.md) hat nun Auto-Fill-Funktionen.  Wenn aktiviert, werden Multiplayer-Manager öffnen Slots während Gameplay füllen Mitglieder über die Vermittlung feststellen.
* Eine Version vor der Produktion von [XIM (Xbox, integrierte Multiplayer)](../multiplayer/xbox-integrated-multiplayer.md) ist jetzt für die Entwicklung von xdk-Version verfügbar.  XIM ist eine eigenständige Benutzeroberfläche für Multiplayer-Netzwerk und Chat Echtzeitkommunikation an Ihrem Spiel über die Leistungsfähigkeit von Xbox Live-Dienste auf einfache Weise hinzuzufügen.

### <a name="social-manager"></a>Soziale Medien-Manager

* Zahlreiche [Social Manager](../social-platform/intro-to-social-manager.md) API-Änderungen:
    * Social-Manager hat den experimentellen Namespace verlassen. Besonderer Dank denjenigen, die Erstanwender waren und Feedback haben!
    * `xbox_social_user`
        * `string_t` Objekte wurden geändert `char*` Objekte
        * Benutzerdefinierte Anwesenheit-Datensatz mit maximal sechs `social_manager_presence_title_record` pro `social_manager_presence_record`
    * `social_event`
        * Gibt eine `std::vector<xbox_user_id_container>` anstelle von `std::vector<xbox_social_user>`
        * Dieser Vektor kann an neue API übergeben werden, `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` API-gibt ein `std::vector<xbox_social_user*>`. Diese Zeiger werden beim nächsten Aufruf von ungültig `social_manager::do_work()`
        * `get_copy_of_users` Zurückgeben einer `std::vector<xbox_social_user>` als eine Kopie der aktuellen Benutzer in der Social_user_group an den Aufrufer. Aufruf dieser Funktion beeinträchtigen `do_work` Dauer.
* Soziale Medien-Manager kann jetzt nicht nach der Initialisierung nicht. Soziale Medien-Manager wiederholt RTA automatisch auf die Trennung der Verbindung. Die `error` und `rta_disconnect_error` Ereignisse als veraltet markiert oder entfernt wurden
* Titel kann benutzerdefinierte speicherzuordnungen angeben. Finden Sie in der neuen Dokumentation: [Soziale Medien-Manager-Speicher und Leistung](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Xbox One UWP
* TCUI APIs fügt Unterstützung für mehrere Benutzer für Xbox One-UWP-apps.  Die XSAPI C++ fügt neue Windows::System::User ^ Parameter geben Sie den Benutzer aus, und der WinRT-API für XSAPI ForUserAsync-APIs hinzugefügt.
* Aktualisierte UWP-Beispielen zur Unterstützung von mehreren Benutzern auf der Xbox

### <a name="other"></a>Andere

* C++ / WinRT-Unterstützung hinzugefügt.   Weitere Informationen finden Sie [hier](../introduction-to-xbox-live-apis.md)
* Neue Funktionen an, um zu Ihren eigenen Protokollierung Rückrufe hinzufügen und entfernen.  Die Diagnosestufe wird an den Rückruf übergeben werden, damit Sie Ihr Verhalten optimieren können.  Finden Sie unter `add_logging_handler` und `remove_logging_handler` in die `microsoft::xbox::services::system::xbox_live_services_settings` Namespace

## <a name="documentation"></a>Dokumentation
* Es gibt neue Dokumentation für die [Multiplayer-Manager](../multiplayer/multiplayer-manager.md), [XIM](../multiplayer/xbox-integrated-multiplayer.md), und [Multiplayer-Konzepte](../multiplayer/multiplayer-concepts.md) für Xbox Live.
* Die [Xbox Live-Einführung](../get-started-with-partner/get-started-with-xbox-live-partner.md) Abschnitte wurden neu geschrieben.  Wenn Sie einen neuen Xbox Live aktiviert Titel erstellen oder zum Einbinden von anderen Xbox Live-Funktionalität in Ihrem Spiel neugierig sind, sehen Sie die neuen Dokumentation [hier](../get-started-with-partner/get-started-with-xbox-live-partner.md).

## <a name="tools"></a>Tools
* Die Xbox Live-Trace-Analyzer finden Sie unter [ https://aka.ms/XboxLiveTools ](https://aka.ms/XboxLiveTools) verfügt jetzt über ein Zertifikat-Modus.  
* Die Xbox Live-Trace-Analyzer wird auch mit mehreren konsolenunterstützung.  Wenn Sie eine Fiddler-Ablaufverfolgung, die HTTP-Anforderungen für mehrere Konsolenfenster enthält übergeben, werden für jede separate Berichte generiert.
