---
Description: Regelmäßige Benachrichtigungen – auch als abgerufene Benachrichtigungen bezeichnet – aktualisieren Kacheln und Signale in festgelegten Intervallen, indem sie Inhalte aus einem Clouddienst herunterladen.
title: Übersicht über regelmäßige Benachrichtigungen
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9936fd6b7210298bdce042b7848a75be8e044040
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175504"
---
# <a name="periodic-notification-overview"></a>Übersicht über regelmäßige Benachrichtigungen
 


Regelmäßige Benachrichtigungen – auch als abgerufene Benachrichtigungen bezeichnet – aktualisieren Kacheln und Signale in festgelegten Intervallen, indem sie Inhalte aus einem Clouddienst herunterladen. Zur Verwendung von regelmäßigen Benachrichtigungen muss Ihr Client-App-Code zwei Informationen bereitstellen:

-   Den URI (Uniform Resource Identifier) eines Webspeicherorts, von dem Windows Kachel- oder Signalupdates für Ihre App abfragt
-   Die Häufigkeit der URI-Abfrage

Regelmäßige Benachrichtigungen bieten Ihnen Live-Kachelaktualisierungen mit minimaler Investition in Clouddienst und Client. Sie stellen auch eine gute Methode zum Verteilen desselben Inhalts an eine große Zielgruppe dar.

**Hinweis**    Weitere Informationen erhalten Sie durch Herunterladen des Beispiels für [Pushbenachrichtigungen und periodische Benachrichtigungen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) für Windows 8.1 und wieder verwenden des Quellcodes in Ihrer Windows 10-app.

 

## <a name="how-it-works"></a>Funktionsweise


Regelmäßige Benachrichtigungen erfordern einen von Ihrer App gehosteten Clouddienst. Der Dienst wird regelmäßig von allen Benutzern abgefragt, die die App installiert haben. Bei jedem Abfrageintervall, wie z. B. nach einer Stunde, sendet Windows eine HTTP GET-Anforderung an den URI, lädt die angeforderten Inhalte, die als Reaktion auf die Anforderung zurückgegeben werden, für die Kachel oder das Signal herunter (als XML) und zeigt die Inhalte auf der App-Kachel an.

Beachten Sie, dass regelmäßige Benachrichtigungen nicht mit Popupbenachrichtigungen verwendet werden können. Popups werden am besten per [geplanter](/previous-versions/windows/apps/hh465417(v=win.10)) oder per [Push](/previous-versions/windows/apps/hh868252(v=win.10))benachrichtigung übermittelt.

## <a name="uri-location-and-xml-content"></a>URI-Speicherort und XML-Inhalt


Jede gültige HTTP- oder HTTPS-Webadresse kann als abzufragender URI verwendet werden.

Die Antwort des Cloudservers enthält die heruntergeladenen Inhalte. Die von dem URI zurückgegebenen Inhalte müssen mit den XML-Schemaspezifikationen für [Kacheln](adaptive-tiles-schema.md) oder [Signale](/uwp/schemas/tiles/badgeschema/schema-root) übereinstimmen, und sie müssen in UTF-8 codiert sein. Sie können definierte HTTP-Header verwenden, um [Ablaufzeit](#expiration-of-tile-and-badge-notifications) oder Tag für die Benachrichtigung anzugeben.

## <a name="polling-behavior"></a>Abfrageverhalten


Rufen Sie eine der folgenden Methoden auf, um die Abfrage zu starten:

-   [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (Kachel)
-   [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (Badge)
-   [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (Kachel)

Wenn Sie eine dieser Methoden aufrufen, wird der URI sofort abgefragt, und die Kachel oder das Signal wird mit den empfangenen Inhalten aktualisiert. Nach der ersten Abfrage stellt Windows weiter im angegebenen Intervall Updates bereit. Die Abfrage wird fortgeführt, bis Sie sie ausdrücklich (mit [**TileUpdater.StopPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate)) anhalten, die App deinstalliert wird oder, im Falle einer sekundären Kachel, die Kachel entfernt wird. Andernfalls fragt Windows weiterhin Updates für die Kachel oder das Signal ab, auch wenn die App nie wieder gestartet wird.

### <a name="the-recurrence-interval"></a>Das Wiederholungsintervall

Das Wiederholungsintervall wird als Parameter der oben aufgeführten Methoden angegeben. Obwohl Windows versucht, den Abruf wie angefordert durchzuführen, kann das Intervall nicht immer genau eingehalten werden. Das angeforderte Abfrageintervall kann um bis zu 15 Minuten verzögert werden, wobei hierfür Windows zuständig ist.

### <a name="the-start-time"></a>Die Startzeit

Optional können Sie eine bestimmte Uhrzeit angeben, zu der mit der Abfrage begonnen werden soll. Gehen wir von einer App aus, deren Kachelinhalt nur einmal täglich geändert wird. In diesem Fall empfehlen wir eine Abfrage nahe dem Zeitpunkt, zu dem auch Ihr Cloud-Dienst aktualisiert wird. Wenn z. B. auf einer Shoppingwebsite mit täglichen Angeboten die Tagesangebote um 08:00 Uhr veröffentlicht werden, sollten neue Kachelinhalte kurz nach 08:00 Uhr abgefragt werden.

Wenn Sie eine Startzeit angeben, werden beim ersten Aufruf der Methode sofort Inhalte abgefragt. Anschließend beginnt der regelmäßige Abruf innerhalb von 15 Minuten nach der angegebenen Startzeit.

### <a name="automatic-retry-behavior"></a>Automatisches Wiederholungsverhalten

Der URI wird nur abgefragt, wenn das Geräte online ist. Wenn das Netzwerk verfügbar ist, jedoch keine Verbindung mit dem URI hergestellt werden kann, wird diese Iteration des Abfrageintervalls übersprungen und der URI beim nächsten Intervall erneut abgefragt. Falls ein Abfrageintervall erreicht wird, während das Gerät ausgeschaltet ist oder sich im Standbymodus oder Ruhezustand befindet, wird der URI abgefragt, wenn das Gerät wieder verfügbar ist.

### <a name="handling-app-updates"></a>Behandeln von APP-Updates

Wenn Sie ein App-Update freigeben, das Ihren Abruf-URI ändert, sollten Sie einen Hintergrund Task für den täglichen [Zeit Trigger](../../../launch-resume/run-a-background-task-on-a-timer-.md) hinzufügen, der "startperiodicupdate" mit dem neuen URI aufruft, um sicherzustellen, dass Ihre Kacheln den neuen URI verwenden. Wenn Benutzer Ihr App-Update erhalten, aber die APP nicht starten, verwenden Ihre Kacheln weiterhin den alten URI, der möglicherweise nicht angezeigt wird, wenn der URI nun ungültig ist, oder wenn die zurückgegebene Nutzlast auf lokale Images verweist, die nicht mehr vorhanden sind.

## <a name="expiration-of-tile-and-badge-notifications"></a>Ablauf der Kachel- und Signalbenachrichtigungen


Standardmäßig laufen die regelmäßigen Kachel- und Signalbenachrichtigungen drei Tage, nachdem sie heruntergeladen wurden, ab. Wenn eine Benachrichtigung abläuft, wird der Inhalt aus dem Signal, der Kachel oder der Warteschlange entfernt und nicht mehr angezeigt. Es wird empfohlen, eine explizite Ablaufzeit für alle regelmäßigen Kachel- und Signalbenachrichtigungen festzulegen. Verwenden Sie dabei eine für Ihre App sinnvolle Zeit, durch die sichergestellt wird, dass der Inhalt nur so lange beibehalten wird, wie er relevant ist. Eine explizite Ablaufzeit ist für Inhalte mit definierter Lebensdauer von großer Bedeutung. Durch sie wird weiterhin sichergestellt, dass veraltete Inhalte entfernt werden, wenn Ihr Clouddienst nicht verfügbar ist oder der Benutzer die Verbindung mit dem Netzwerk für längere Zeit trennt.

Ihr Cloud-Dienst legt ein Ablaufdatum und eine Ablaufzeit für eine Benachrichtigung fest, indem der Antwortnutzlast der HTTP-Header "X-WNS-Expires" hinzugefügt wird. Der HTTP-Header „X-WNS-Expires” entspricht dem [HTTP-Datumsformat](https://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.3.1). Weitere Informationen finden Sie unter [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) oder [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_).

Beispielsweise können Sie während eines aktiven Börsenhandelstags die Gültigkeitsdauer für eine Aktienpreisaktualisierung gegenüber dem Abfrageintervall verdoppeln (z. B. auf eine Stunde nach Empfang bei einer Abfrage zu jeder halben Stunde). Als weiteres Beispiel dient eine News-App, bei der festgestellt wird, dass ein Intervall von einem Tag für eine tägliche Kachelaktualisierung angemessen ist.

## <a name="periodic-notifications-in-the-notification-queue"></a>Regelmäßige Benachrichtigungen in der Benachrichtigungswarteschlange


Sie können regelmäßige Benachrichtigungen mit [Benachrichtigungszyklen](/previous-versions/windows/apps/hh781199(v=win.10)) verwenden. Eine Kachel auf dem Startbildschirm zeigt standardmäßig den Inhalt einer einzelnen Benachrichtigung an, bis die aktuelle Benachrichtigung durch eine neue ersetzt wird. Bei aktivierten Benachrichtigungszyklen verbleiben bis zu fünf Benachrichtigungen in einer Warteschlange und werden nacheinander auf der Kachel angezeigt.

Hat die Warteschlange ihre maximale Kapazität von fünf Benachrichtigungen erreicht, ersetzt die nächste neue Benachrichtigung die älteste Benachrichtigung in der Warteschlange. Durch Festlegen von Tags für Ihre Benachrichtigungen können Sie die Ersetzungsrichtlinie der Warteschlange jedoch beeinflussen. Ein Tag ist eine app-spezifische Zeichenfolge von bis zu 16 alphanumerischen Zeichen (ohne Beachtung der Groß-/Kleinschreibung), die in der Antwortnutzlast im HTTP-Header [X-WNS-Tag](/previous-versions/windows/apps/hh465435(v=win.10)) angegeben wird. Windows vergleicht das Tag einer eingehenden Benachrichtigung mit den Tags aller bereits in der Warteschlange vorhandenen Benachrichtigungen. Wird eine Übereinstimmung gefunden, ersetzt die neue Benachrichtigung die Benachrichtigung in der Warteschlange mit demselben Tag. Wird keine Übereinstimmung gefunden, wird die Standardersetzungsregel angewendet und die neue Benachrichtigung ersetzt die älteste Benachrichtigung in der Warteschlange.

Sie können die Benachrichtigungswarteschlange und Tags verwenden, um eine Vielzahl von Benachrichtigungsszenarien mit großem Funktionsumfang zu implementieren. So kann beispielsweise eine Aktien-App fünf Benachrichtigungen senden – jede für eine andere Aktie und markiert mit dem Aktiennamen. Dadurch enthält die Warteschlange niemals zwei Benachrichtigungen für die gleiche Aktie, wovon eine zwangsläufig nicht mehr aktuell wäre.

Weitere Informationen finden Sie unter [Verwenden der Benachrichtigungs Warteschlange](/previous-versions/windows/apps/hh781199(v=win.10)).

### <a name="enabling-the-notification-queue"></a>Aktivieren der Benachrichtigungswarteschlange

Um eine Benachrichtigungswarteschlange zu implementieren, aktivieren Sie zunächst die Warteschlange für die Kachel (siehe [Verwendung der Benachrichtigungswarteschlange mit lokalen Benachrichtigungen](/archive/blogs/tiles_and_toasts/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications)). Der Aufruf zum Aktivieren der Warteschlange muss nur einmal in der Lebensdauer Ihrer App ausgeführt werden, es schadet jedoch nicht, den Aufruf bei jedem Start der App auszuführen.

### <a name="polling-for-more-than-one-notification-at-a-time"></a>Abfragen von mehreren Benachrichtigungen gleichzeitig

Sie müssen einen eindeutigen URI für jede Benachrichtigung angeben, die Windows für Ihre Kachel herunterladen soll. Mit der [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_)-Methode können Sie bis zu fünf URIs gleichzeitig zur Verwendung mit der Benachrichtigungswarteschlange angeben. Jeder URI wird zur gleichen oder fast zur gleichen Zeit für eine einzige Benachrichtigungsnutzlast abgefragt. Jeder abgefragte URI kann seine eigene Ablaufzeit sowie einen Tag-Wert zurückgeben.

## <a name="related-topics"></a>Zugehörige Themen


* [Richtlinien für regelmäßige Benachrichtigungen]()
* [So wird's gemacht: Einrichten regelmäßiger Benachrichtigungen für Signale](/previous-versions/windows/apps/hh761476(v=win.10))
* [So wird's gemacht: Einrichten regelmäßiger Benachrichtigungen für Kacheln](/previous-versions/windows/apps/hh761476(v=win.10))
 