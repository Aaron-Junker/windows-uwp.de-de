---
title: Erfolge 2017
description: Erfolge 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: bfc67f6aca27abf095a89c451111e6429bca82e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590985"
---
# <a name="achievements-2017"></a>Erfolge 2017

Das System Auszeichnungen 2017 ermöglicht eine direkte Aufrufmodell verwenden, um Erfolge für neue Xbox Live-Spiele für Xbox One, Windows 10, Windows 10 Phone, Android und iOS zu entsperren.

## <a name="introduction"></a>Einführung

Mit der Xbox One wurde ein neues Cloud-Powered Erfolge-System, mit dem Spieleentwickler, um die Daten für ihre Xbox Live-Funktionen, z. B. Statistiken Benutzerzugriff, Erfolge, umfassender vorhanden und Multiplayer-Spiele, zu steuern, indem Sie einfach die Ereignisse für in-Game-Telemetriedaten senden kann. Dies ist eine Vielzahl von neuen Vorteilen geöffnet – ein einzelnes Ereignis kann Daten für mehrere Funktionen auf Xbox Live aktualisieren; Xbox Live-Konfiguration befindet sich auf dem Server statt auf dem Client; und vieles mehr.

In den Jahren, die nach der Veröffentlichung der Xbox One wir haben eng datenkanalports, auf Feedback der Entwickler von Spielen, und Entwickler konsistent die folgenden freigegeben haben:

1.  **Verlangen Sie, Erfolge, die über eine direkte Aufrufmuster zu entsperren.** Viele Entwickler erstellen Sie Spiele für verschiedene Plattformen, einschließlich früherer Versionen von Xbox, und die Auszeichnung-ähnliche Systeme auf diesen Plattformen verwenden, eine direkte aufrufende Methode. Unterstützt direkte Aufrufe auf der Xbox One entsperren und anderen Plattformen der aktuellen Generation Xbox würde ihre plattformübergreifenden Spieleentwicklung Anforderungen und die Entwicklungskosten für die Zeit zu erleichtern.

2.  **Minimieren Sie die Komplexität der Konfiguration.** Mit dem System Cloud-Powered Erfolge Entsperren eines Erfolg der Logik muss in Xbox Live konfiguriert sein, damit die Dienste wissen, wie mit dem Titel gehörigen Statistiken Daten interpretiert und wann das Erreichen der Zertifikation für einen Benutzer zu entsperren. Dies erfolgte über einen neuen Auszeichnung Regeln-Abschnitt einen Erfolg der Konfiguration, die nicht bereits vorhanden ist. Während Logik in der Cloud zu entsperren müssen sehr leistungsstark sein können, fügt diese Anforderung zusätzliche Konfiguration Komplexität in das Design und die Erstellung des Titels Erfolge hinzu.

3.  **Schwere Fehlerbehebung.** Während das System Cloud-Powered Erfolge eine Vielzahl von hilfreiche Funktionen eingeführt werden, es kann auch sein für das Spiel schwieriger Entwickler, um zu überprüfen und Behandeln von Problemen mit ihren Ergebnissen aus, da die Auszeichnung entsperrt werden indirekt durch Regeln ausgelöst die für den Dienst live, anstatt direkt durch das Spiel selbst gesteuert.

Es ist erwähnenswert, dass Spieleentwickler auch wiederholt freigegeben haben Feedback, das sie zu schätzen wissen und Wert bestimmte Funktionen, die zusammen mit dem System Cloud-Powered Erfolge eingeführt wurden:

1.  Neue Features für benutzererfahrung wie z. B. Auszeichnung Fortschritt, Aktualisierungen in Echtzeit, Konzept Art Boni und Beiträge entsperrt in Aktivitätsfeed.

2.  Configuration-Verbesserungen, z.B. eine Dienstkonfiguration statt einer lokalen Konfiguration, die im Spiel-Paket enthalten sein muss (d. h. Gameconfig, XLAST, SPA usw.) und die Möglichkeit, problemlos Auszeichnung Bearbeiten von Zeichenfolgen & images nach der Auslieferung des Spiels.

Auszeichnungen 2017, erstellen wir eine Ersetzung von der vorhandenen Cloud-Powered Erfolge System für die zukünftige Titeln zu verwenden, es sogar noch einfacher für Spieleentwickler Xbox ist, Erfolge zu konfigurieren, integrieren, Auszeichnung entsperrt, und Sie Softwareupdates in der Spielen Sie Code, und überprüfen Sie, dass die Ergebnisse wie erwartet funktionieren.

## <a name="whats-different-with-achievements-2017"></a>Worin besteht der Unterschied mit Auszeichnungen 2017

|                          | Auszeichnungen 2017-system        | Cloud-gestützte Erfolge system      |
|--------------------------|---------------------------------------|----------------------------------------|
| Entsperren von Trigger           | Direkt über API-Aufruf                 | Indirekt über die Telemetrie-Ereignissen        |
| Entsperren Sie Besitzer             | Titel                                 | Xbox Live                              |
| Konfiguration            | Zeichenfolgen, Bilder und Boni              | Zeichenfolgen, Bilder, Boni, entsperren Regeln \[+ Statistiken und Ereignisse\]                    |
| Fortschritt              | Unterstützt <br>*direkt über API-Aufruf*                | Unterstützt <br> *indirekt über die Telemetrie-Ereignissen*       |
| Aktivitätsüberwachung in Echtzeit (RTA) | Unterstützt                             | Unterstützt                              |
| Herausforderungen               | Nicht unterstützt   | Unterstützt                      |

## <a name="title-requirements"></a>Titel-Anforderungen

Im folgenden werden die Anforderungen der alle Titel, der das System Auszeichnungen 2017 verwendet.

1.  **Sie muss ein neuer Titel ein (freigegebene).** Titel, die bereits veröffentlicht wurden, und verwenden das System Cloud-Powered Erfolge zur datenmodifikation sind. Weitere Informationen finden Sie unter [Warum können keine vorhandenen Titel "migrieren" auf das neue Auszeichnungen 2017-System?](#_Why_can’t_existing)

2.  **Verwenden, müssen vom August 2016 xdk-Version oder höher.** Die Update_Achievement-API wurde im August 2016 xdk-Version veröffentlicht.

3.  **Ein xdk-Version oder UWP-Titel muss sein.** Das System Auszeichnungen 2017 ist nicht verfügbar für legacy-Plattformen, einschließlich Xbox 360, Windows 8.x oder höher durchführen, und Windows Phone 8 oder ältere.

## <a name="updateachievement-api"></a>Update_Achievement API

Wenn Sie Ihre Erfolge über XDP konfiguriert haben oder [UDC](../configure-xbl/dev-center/achievements-in-udc.md) und veröffentlicht in Ihrer Dev-Sandbox, Ihre Titel kann Entsperren durch Aufrufen der API Update_Achievement.

Die API ist in der xdk-Version und das Xbox Live SDK verfügbar.

### <a name="api-signature"></a>API-Signatur

Die API-Signatur lautet wie folgt aus:

```c++
/// <summary>
    /// Allow achievement progress to be updated and achievements to be unlocked.  
    /// This API will work even when offline. On PC and Xbox One, updates will be posted by the system when connection is re-established even if the title isn't running
    /// </summary>
    /// <param name="xboxUserId">The Xbox User ID of the player.</param>
    /// <param name="titleId">The title ID.</param>
    /// <param name="serviceConfigurationId">The service configuration ID (SCID) for the title.</param>
    /// <param name="achievementId">The achievement ID as defined by XDP or Partner Center.</param>
    /// <param name="percentComplete">The completion percentage of the achievement to indicate progress.
    /// Valid values are from 1 to 100. Set to 100 to unlock the achievement.  
    /// Progress will be set by the server to the highest value sent</param>
    /// <remarks>
    /// Returns a task<T> object that represents the state of the asynchronous operation.
    ///
    /// This method calls V2 GET /users/xuid({xuid})/achievements/{scid}/update
    /// </remarks>
    _XSAPIIMP pplx::task<xbox::services::xbox_live_result<void>> update_achievement(
        _In_ const string_t& xboxUserId,
        _In_ uint32_t titleId,
        _In_ const string_t& serviceConfigurationId,
        _In_ const string_t& achievementId,
        _In_ uint32_t percentComplete
        );
```

`xbox::services::xbox_live_result<T>` ist der zurückgebenden Aufruf für alle C++ Xbox Live Services-API-Aufrufe.

Weitere Informationen finden Sie in die Rede Xfest 2015 "XSAPI: C++, keine Ausnahmen!"<br>
[video](https://go.microsoft.com/?linkid=9888207) |  [slides](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>Entsperren per Update_Achievement API

Um einen Erfolg zu entsperren, legen die *PercentComplete* bis 100.

Wenn der Benutzer online ist, wird die Anforderung wird sofort an den Xbox Live Erfolge-Dienst gesendet werden, und löst die folgenden Benutzeroberflächen:

-   Der Benutzer erhält eine Auszeichnung entsperrt Benachrichtigung;

-   In der Liste des Benutzers Auszeichnung für den Titel wird entsperrt die angegebenen Auszeichnung dargestellt.

-   Die Auszeichnung entsperrt wird die Aktivität des Benutzers feed hinzugefügt werden.

> *Hinweis: Es werden keinen sichtbaren Unterschied in der Benutzeroberflächen für Fortschritte, die vom System Auszeichnungen 2017 verwendet und die Cloud-Powered Erfolge.*

Wenn der Benutzer offline ist, wird der entsperranforderung lokal auf dem Gerät des Benutzers in die Warteschlange eingereiht werden. Wenn das Gerät des Benutzers die Netzwerkverbindung getrennt wird, die Anforderung wird automatisch wiederhergestellt hat, die an den Erfolge-Dienst gesendet werden – Beachten Sie: ist keine Aktion erforderlich, von dem Spiel zum Auslösen dieses Feature und die oben genannten Benutzeroberflächen erfolgt wie beschrieben.

### <a name="updating-completion-progress-via-updateachievement-api"></a>Abschluss der Bearbeitung über Update_Achievement-API wird aktualisiert.

Legen Sie zum Aktualisieren eines Benutzers Fortschritte bei der Aufhebung der Sperre eines Erfolg der *PercentComplete* auf die entsprechende ganze Zahl zwischen 1 und 100.

Einen Erfolg des Status kann nur erhöhen. Wenn *PercentComplete* festgelegt ist, um eine Zahl kleiner als das Erreichen der Zertifikation dem letzten *PercentComplete* Wert, das Update wird ignoriert. Z. B. wenn erreichen der Zertifikation *PercentComplete* hatte zuvor festgelegt wurde, 75 ist, senden ein Update mit einem Wert von 25 wird ignoriert, und das Erreichen der Zertifikation wird weiterhin als 75 % abgeschlossen angezeigt.

Wenn *PercentComplete* festgelegt ist auf 100 erhöht, wird das Erreichen der Zertifikation entsperrt.

Wenn *PercentComplete* festgelegt ist in eine Zahl größer als 100 ist, die API verhält, als ob Sie genau 100 festlegen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>Können mein Titel mit der Auszeichnungen 2017 noch werden geliefert?

Absolut. Alle neuen Titel sind willkommene und wird empfohlen, die Auszeichnungen 2017 verwenden System durch das System Cloud-Powered Erfolge.

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>Warum werden Herausforderungen im System Auszeichnungen 2017 nicht unterstützt?

Von Nutzungsdaten über die Xbox-Spiele ergab, dass die aktuelle Implementierung und die Darstellung der Herausforderungen für die meisten Spiele-Entwickler müssen nicht erfüllt. Wir weiterhin, Erfassen der Eingabe der Developer und im Feedback in diesem Bereich und sich bemühen, zukünftige Features zu liefern, die mehr auf den Punkt mit entwickleranforderungen gerecht zu werden. Der neu veröffentlichten Xbox Spielbereich ist ein Beispiel für eine Funktion, die neue Wettbewerbsfähigkeit für Xbox führt eine neue, jedoch ähnliche, Richtung Spiele.

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>Kann ich trotzdem Hinzufügen neuer Erfolge jedem Kalenderquartal wenn mein Titel der Auszeichnungen 2017-System verwendet wird?

Ja. Die Erfolge-Richtlinie bleibt unverändert.

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>Warum migrieren können nicht vorhandenen Titel"auf das neue Auszeichnungen 2017 System"?

Für die überwiegende Mehrheit der vorhandenen Titel "Migration" an das System Auszeichnungen 2017 wäre nicht auf ihre Dienstkonfigurationen aktualisieren, und Aufrufe Ereignis Schreibvorgänge für energieeffizienzleistungen austauschen entsperrt werden, obwohl diese Änderungen, die allein sehr wäre kostspielige und würde erheblich in Bezug auf Fehler- und unerwartetem Verhalten, die in die Erfolge irreparabel beschädigt wird führen können. Vielmehr haben die meisten vorhandenen Titel auch Benutzer mit vorhandenen Daten. Versuchen, ein live-Spiel zu konvertieren, die bereits die Erfolge Cloud-Powered verwendet System würde nicht nur eine sehr teuer Aufwand für Entwickler und Xbox, sondern würde erheblich gefährden Sie, vorhandene Benutzer Profile und/oder spielen Erfahrungen.

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>Wenn mein Titel mit dem System Cloud-Powered Erfolge veröffentlicht wurde, kann alle zukünftigen DLC für den Titel in Auszeichnungen 2017 wechseln?

Alle Leistungen, eines Titels müssen das gleiche Erfolge-System verwenden. Welche Erfolge-System von der Basis des Spiels Erfolge verwendet wird, ist das System, das für alle zukünftigen Erfolge für den Titel verwendet werden muss.

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>Können Sie beim Ausführen der Tests Erfolge in meinem Dev-Sandkasten ich und kombinieren zwischen der Verwendung der Auszeichnungen 2017-System und das System Cloud-Powered Erfolge?

Nein. Alle Leistungen, eines Titels müssen das gleiche Erfolge-System verwenden.

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>Auszeichnungen 2017 auch umfasst offline entsperrt?

Wenn Sie der Titel einer Auszeichnung entsperrt, während das Gerät offline ist, die Update_Achievement-API automatisch in die Warteschlange die offline sperraufhebungsanforderungen und zu Xbox Live automatisch synchronisiert werden, wenn das Gerät wieder eine Netzwerkverbindung getrennt wird, ähnlich wie die aktuelle hergestellt hat Cloud-gestützte Erfolge des Systems die offline-Erfahrung. Erfolge entsperrt wird nicht ausgeführt, wenn der Benutzer offline ist.

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>Ich sehe es sich um ein neues "AchievementUpdate"-Ereignis im XDP. Wenn mein Titel dieses Ereignisses verwendet wird, bedeutet, dass Auszeichnungen 2017 wurden?

Die *AchievementUpdate* Basisereignis ist von Xbox Live für Back-End-Zwecke erforderlich. Sie können es ignorieren. Wenn Ihre Titel ein Ereignis mit diesem Typ Basisereignis konfiguriert, werden Ereignis Schreibvorgänge von Xbox Live ignoriert. Titel, die auf dem System Cloud-Powered Erfolge erstellt werden sollten weiterhin die Ereignisse zu konfigurieren, indem Sie mithilfe der anderen Basisereignis. Titel, die auf dem System Auszeichnungen 2017 erstellt wurden, müssen nicht konfigurieren *alle* Ereignisse aus Gründen der Leistung.
