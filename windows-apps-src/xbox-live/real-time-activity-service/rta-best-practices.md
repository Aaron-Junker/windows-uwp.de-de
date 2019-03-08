---
title: RTA Best Practices
description: Erfahren Sie mehr über bewährten Methoden bei Verwendung des Diensts für die Xbox Live-Real-Time-Aktivität.
ms.assetid: 543b78e3-d06b-4969-95db-2cb996a8bbd3
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Real-Time-Aktivität
ms.localizationpriority: medium
ms.openlocfilehash: 7733aab9330c316ad5938cf9a2ef763e06f19b9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613565"
---
# <a name="real-time-activity-rta-best-practices"></a>Real-Time-Aktivität (RTA) bewährte Methoden
Diese empfohlenen Vorgehensweisen helfen Ihnen die Verwendung des Titels, des RTA optimal zu machen.


## <a name="the-basics"></a>Die Grundlagen

RTA verwendet WebSocket-Sitzungen, um eine permanente Verbindung mit dem Client zu erstellen. Das ist wie der Dienst Statistiken an Sie übermittelt. Ein Client muss eine authentifizierte Verbindung-Anforderung senden und RTA wird das Token der Anforderung verwenden, um zu überprüfen, ob die Verbindung hergestellt werden kann, und klicken Sie dann diese einzurichten.

Nachdem die Verbindung eingerichtet wurde, kann Ihre app Anforderungen für bestimmte Statistiken abonnieren vornehmen. Für ein erfolgreiches Abonnement wird die RTA den aktuellen Wert und einige zusätzliche Metadaten, wie der Typ der Statistik, die als Teil der Nutzlast der Antwort zurückgegeben. Klicken Sie dann leitet RTA alle Updates, die für die Statistik auftreten, während der Client abonniert hat.

Wenn Aktualisierungen in Echtzeit auf eine Statistik von Ihrem Titel nicht benötigt wird, sollte es sein Abonnement für diese Statistik beendet werden.


## <a name="handling-disconnects"></a>Behandlung von trennt.

Titel sollten bedenken, dass bei der das Authentifizierungstoken für den Benutzer abgelaufen ist, wird ihre Sitzung vom Dienst getrennt werden. Der Titel muss erkennen, wann die einem solchen Fall geschieht in der Lage sein, erneut eine Verbindung herzustellen, und abonnieren Sie alle Statistiken, denen sie zuvor abonniert wurde.

RTA-Verbindungen werden standardmäßig die zwingt den Client erneut eine Verbindung herzustellen, nach zwei Stunden geschlossen. Dies geschieht, da das Auth-Token für die Verbindung zwischengespeichert werden, um die Nachricht Bandbreite zu sparen. Schließlich wird das Token ablaufen. Durch das Schließen der Verbindung, und dass der Client erneut eine Verbindung herstellen wird der Client gezwungen, um das Authentifizierungstoken zu aktualisieren.

Ein Client kann auch getrennt aufgrund eines Benutzers ISP Probleme oder der Prozess für den Titel angehalten wird. In diesem Fall wird ein WebSocket-Ereignis ausgelöst, um den Client zu informieren. Im Allgemeinen, es empfiehlt sich, verarbeiten können mit dem Dienst getrennt.

> [!WARNING]
> Wenn ein Client RTA für Multiplayer-Sitzungen verwendet, und von 30 Sekunden getrennt wird, die [Multiplayer-Sitzung Directory(MPSD)](../multiplayer/multiplayer-appendix/multiplayer-session-directory.md) erkennt, dass die RTA-Sitzung wird geschlossen, und den Benutzer von der Sitzung startet. Es liegt an den Client RTA erkennen, wenn die Verbindung geschlossen wird und das Initiieren einer erneut hergestellten Verbindung und erneut abonnieren, bevor die MPSD den Sitzungszustandsanbieter beendet.

## <a name="managing-subscriptions"></a>Verwalten von Abonnements

In Bezug auf das Verwalten von Abonnements aus, wenn ein Token abläuft, sollten alle Titel des nachverfolgen Zeiten die Abonnements vorgenommen wurden. Nach erfolgreich abonniert hat, gibt die RTA einen eindeutigen Bezeichner für jeden abonnierten Statistiken zurück. Der Bezeichner wird in alle zukünftigen Updates für diese Statistik anstelle des Namens der Statistik verwendet. Dies erfolgt, um Bandbreite zu sparen. Clients sind verantwortlich für die Verwaltung ihrer eigenen Zuordnung der Statistiken, die RTA-Abonnement-IDs.


## <a name="unsubscribing"></a>Kündigen des Abonnements

Müssen nicht verwendete Abonnements wird nicht empfohlen. Der Dienst beschränkt die Anzahl der Abonnements, die ein Benutzer zu einem bestimmten Zeitpunkt pro Titel verwenden kann. Wenn Sie alles und jedes abonnieren, können Sie diese Anzahl erreicht, und dies kann verhindern, dass Abonnements für einige wichtigen Statistiken. (Weitere Informationen zu Grenzwerten für Abonnements finden Sie unter **drosselt**weiter unten in diesem Thema.)

Beispielsweise benötigen Ihre Titel möglicherweise nur ein Abonnement für eine bestimmte Szene. Wenn der Benutzer diese Szene eingibt, sollte dem Titel des abonnieren. Wenn der Benutzer diese Szene lässt, sollten diese Statistiken abgemeldet werden. Es wird auf ähnliche Weise eine Unsubscribe-All-Nachricht, die verwendet werden kann, wenn keine Abonnements erforderlich sind.

Nach der Kündigung des Abonnements, wird die Abonnement-ID für diese Statistik nicht mehr verwendet werden.


## <a name="awareness-of-latent-items-in-the-queue"></a>Kenntnis der latenten Elemente in der Warteschlange

Beim Kündigen des Abonnements, über eine Statistik, ist es möglich, dass ein Update für die Statistik noch den Client erreichen. Auch wenn Sie der Titel einer Statistik einfach gekündigt hat, werden Sie also Beachten Sie, dass es immer noch ein oder zwei im Zusammenhang mit dieser Statistik Updates abrufen kann.

Es wird empfohlen, um die Updates für Statistiken zu ignorieren, wenn die Abonnement-ID nicht erkannt wird.


## <a name="ignore-messages-you-do-not-understand"></a>Ignorieren Sie Meldungen, die Sie nicht verstehen

Es ist möglich, dass die Message-Protokoll aktualisiert werden. Um Ihre app unabhängig von der alle neuen Nachrichten zu halten, empfehlen wir, dass der Titel des einfach Unbekannter Nachrichtentypen verwerfen.


## <a name="throttles"></a>Drosselungen

RTA erzwingt bestimmte Drosselungen für den Dienst:

-   UserStats: 1000
-   Vorhanden: 2500

Wenn ein Client die drosselungsgrenze erreicht, die es entweder wird, eine Fehlermeldung als Teil eines Aufrufs abonnieren/Kündigen des Abonnements, oder es wird getrennt. In beiden Fällen werden weitere Informationen zur Drosselung Einschränkung, die ermittelt wurde zur Verfügung, an den Client zusammen mit der Fehlermeldung oder trennen die Nachricht.

Wenn Sie den Titel zu entwickeln, sollten Sie diese Konzepte Bedenken. Wenn Sie extrem erhöhen durchführen, müssen Sie möglicherweise eine Beeinträchtigung der app-Erfahrung, da der Dienst ruft Ihre Einschränkung werden konnte. Rechts kann der Dienst nun bei 1.000 Abonnements Statistiken pro Instanz einer Anwendung. Zusätzlich zu den, kann auch eine Instanz einer Anwendung eines Benutzers gesamte Personen die Länge der Liste nach Vorhandensein-Updates abonnieren. Diese Zahl wird optimiert wird, damit es in zukünftigen Versionen ändern kann.
