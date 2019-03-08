---
title: Bewährte Methoden zum Aufrufen von Xbox Live
description: Erfahren Sie mehr über bewährten Methoden für das Xbox Live-APIs aufrufen.
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, bewährte Methoden
ms.localizationpriority: medium
ms.openlocfilehash: 55e05ef7de2e2981f9f5af86623a8d8413ce2c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613705"
---
# <a name="best-practices-for-calling-xbox-live"></a>Bewährte Methoden zum Aufrufen von Xbox Live

Die Xbox Live-Dienste können in zwei bevorzugte Möglichkeiten aufgerufen werden: mithilfe der Xbox-Services-API (XSAPI) oder die REST-Endpunkte direkt aufzurufen. Unabhängig davon, wie Code Xbox Live aufruft ist es wichtig, richtige Aufrufmuster aufweisen und Wiederholungslogik zu integrieren.

Um Informationen zum richtigen Wiederholungslogik zu schreiben, es ist notwendig, kennen die beiden Typen von REST-Endpunkte – **Idempotent** und **nicht idempotente**. Jede dieser besprechen unten wir
 
## <a name="non-idempotent-endpoints"></a>Nicht-idempotente-Endpunkte

Wiederholen Sie die HTTP-Methoden mit Nebenwirkungen auf Aufrufe werden als gleich betrachtet **nicht idempotente**. Dies bedeutet, dass wenn ein Client den Endpunkt aufgerufen wurden, eine Zeitüberschreitung im Netzwerk tritt nicht sicher ist auf die Methode zu wiederholen, da die Ressource wurde möglicherweise aktualisiert, aber das Netzwerk nicht können, um den Aufrufer benachrichtigen, dass der Vorgang erfolgreich abgeschlossen wurde. Nach Fehler, anstatt wiederholt wird muss der Client zuerst Abfragen, um festzustellen, ob der Aufruf erfolgreich war. Nur, wenn der Aufruf nicht erfolgreich war, sollten sie wiederholen.

In der Xbox-Services-API werden einige APIs intern als nicht idempotente-Endpunkte aufrufen. Dies bedeutet, dass wenn Fehler auftreten, wenn diese Endpunkte aufrufen, die APIs nicht automatisch den Endpunkt erneut ausgeführt.

Die vollständige Liste der nicht idempotente-APIs sind:

* game\_server\_platform\_service::allocate\_cluster()
<br>
* game\_server\_platform\_service::allocate\_cluster\_inline()
<br>
* game\_server\_platform\_service::allocate\_session\_host()
<br>
* matchmaking\_service::create\_match\_ticket()
<br>
* multiplayer\_service::write\_session()
<br>
* multiplayer\_service::write\_session\_by\_handle()
<br>
* multiplayer\_service::send\_invites()
<br>
* reputation\_service::submit\_batch\_reputation\_feedback()
<br>
* reputation\_service::submit\_reputation\_feedback()
 

## <a name="idempotent-methods"></a>Idempotente Methoden

**Idempotent** HTTP-Methoden auf der anderen Seite benötigen keinen Nebeneffekte. Dies bedeutet wiederum, dass sie wiederholt werden sicher sind. In der Xbox-Services-API sind alle Idempotent Methoden unter bestimmten Bedingungen automatisch wiederholt.

Die vollständige Liste der Idempotent APIs sind alle APIs, die nicht oben aufgeführt wurden, als nicht idempotente.


## <a name="retry-logic-best-practices"></a>Wiederholungslogik Best Practices

Für Idempotent Aufrufe sollten diese Bedingungen automatisch wiederholt werden:

* Alle Netzwerkfehler
* 401: Nicht autorisiert
* 408: RequestTimeout
* 429: Zu viele Anforderungen
* 500: InternalError
* 502: BadGateway
* 503: ServiceUnavailable
* 504: GatewayTimeout


Auf UWP, 401: Nicht autorisiert ist behandelten spezielle. Er gibt an, dass das Xbox Live-Authentifizierungstoken abgelaufen ist, damit die Xbox-API aufruft, in das Betriebssystem aus, um das Token zu aktualisieren, und dann als einen einzelnen Wiederholungsversuch führt.

Wenn eine Wiederholung ausgeführt wird, ist es empfiehlt sich, die nicht den Dienst aufrufen, bis die "Retry-After"-Header Zeitpunkt erreicht ist. XSAPI sind jetzt implementiert diese best Practices. Wenn ein Fehler bei HTTP-Statuscode und "Retry-After"-Header für alle APIs zurückgegeben wurde, werden zusätzliche Aufrufe mit derselben API vor dem Zeitpunkt der Retry-After sofort ohne Verwendung des Diensts mit dem ursprünglichen Fehler zurück.

Bei einen Aufruf zu wiederholen, ist es empfehlenswert, Exponentielles Backoff mit einer zufälligen Jitter, verteilen die Last auf den Dienst ausführen. XSAPI beginnt mit einer Verzögerung von 2 Sekunden die gesteuert wird, verwenden Xbox\_live\_Kontext\_settings::set\_http\_wiederholen\_delay(). Dies bedeutet standardmäßig jede Wiederholung wird ein Exponentielles Backoff von 2, 4, 8, usw. Sekunden und jitters die Verzögerung zwischen dem aktuellen und dem nächsten Backoff-Wert basierend auf der die Antwortzeit weiter verbreiten sich die Last für die Gruppe von Geräten, die versuchen, die Wiederholung.

Titel muss Kontrolle über wie viel Zeit aufwenden, einen Aufruf zu wiederholen. Verwenden XSAPI, Entwickler haben direkte Kontrolle über diese mithilfe der Funktion Xbox\_live\_Kontext\_settings::set\_http\_Timeout\_window(). Standardmäßig ist dies auf 20 Sekunden festgelegt. Wenn dies auf 0 Sekunden wird die Wiederholungslogik effektiv deaktivieren. XSAPI jetzt auch dynamisch angepasst wird das interne basierte auf wie viel Zeit linken bleibt in der HTTP-HTTP-Timeout\_Timeout\_window(). Die internen HTTP-Timeout-Steuerelemente wie lange das Betriebssystem verbringt den Netzwerk-HTTP-Vorgang ausführen, bevor er abgebrochen wird. Der Aufruf wird nicht wiederholt werden, es sei denn, es mindestens 5 Sekunden, die Links in der HTTP bleibt-\_Timeout\_window() einen genug angemessenen Zeit für den Abschluss des Aufrufs zu gewähren. Diese Regel trifft nicht auf dem ersten Aufruf, das Festlegen des http\_Timeout\_window() 0 zulässig ist, und führt zu einem einzigen Aufruf. Diese Logik wirkt sich die http\_Timeout\_window() ist viel besser steuern, wenn die API-Aufruf zurückgegeben wird. Wenn ein "Retry-After"-Header zurückgegeben wurde, werden keine Reties nach "Retry-After" Zeitpunkt erreicht wurde erst vorgenommen werden. Ist die Zeit "Retry-After" nach http\_Timeout\_window(), und klicken Sie dann auf den Aufruf zurückgegeben werden, am Ende des HTTP-\_Timeout\_window().


## <a name="error-handling"></a>Fehlerbehandlung

Titel-Entwickler sollten **immer** verwenden Sie die richtige Fehlerbehandlung für **jeder** Dienstaufruf, müssen sie sicherstellen, dass sie fehlerhafte Antworten ordnungsgemäß behandeln.
 
Es gibt viele reale Bedingungen, die in einer Anforderung, die auf der Xbox Live, zum Zurückgeben von Fehlercodes, z. B. führen können.

1.  Netzwerk ist nicht verfügbar. Beispielsweise wird das Gerät verloren geht, 4G, Wi-Fi, verloren, oder das Netzwerk ausgefallen.
2.  Zu viel Last für Dienste über Load (503)
3.  Ein Fehler aufgetreten ist, für den Dienst (500)
4.  Zu viele Anforderungen, die an den Dienst (429) gesendet
5.  Konflikt (412) zu schreiben. Zum Beispiel übermittelt einen anderen Player in einer Multiplayer-Sitzung eine Änderung zuerst
6.  Der Benutzer wurde gesperrt wurde oder besitzt keine Berechtigung
7.  Benutzer wurde signiert-out

Richtige Fehlerbehandlung ist entscheidend, um sicherzustellen, dass das Spiel ordnungsgemäß unter diesen Umständen funktioniert.

XSAPI verfügt über zwei Arten von Mustern für die Fehlerbehandlung. Ein Muster, die bei Verwendung der WinRT-APIs in C++ / CX, C\#, oder Javascript, und ein weiteres Muster, wenn Sie die neuen C++-APIs verwenden. Ausführliche Informationen zu bewährten Methoden der Fehlerbehandlung, finden Sie auf der Xbox Live Doc-Seite "Fehlerbehandlung" und ein Video, das Dies umfasst, finden Sie in das Gespräch im [ *Xfest 2015 Videos* ](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) namens *XSAPI: C++, die keine Ausnahmen.*


## <a name="best-calling-patterns"></a>Beste Aufrufmuster

### <a name="usebatching-requests"></a>Batchverarbeitung von Anforderungen

Einige Endpunkte unterstützen Batchverarbeitung oder einem Satz von Anforderungen in einem einzigen Aufruf aggregieren. Mit dem Dienst für die Xbox Live-Profil können Sie z. B. für das Profil eines einzelnen Benutzers oder einem Satz von Benutzerprofilen bitten. Also, wenn Sie eine Benutzerprofile für eine Gruppe von Benutzern benötigen, wäre den Endpunkt oder die API eine zu einem Zeitpunkt für jedes Benutzerprofil Aufruf sehr ineffizient es. Jeder Aufruf fügt einiges an Overhead der Authentifizierung. Stattdessen übergeben Sie alle Benutzer, die Sie Informationen zu auf einmal an der API sinnvoll sein, damit der Endpunkt kann alle Benutzerprofile gleichzeitig zu verarbeiten und eine einzelne Antwort zurückzugeben.

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>Verwenden Sie die Echtzeit-Aktivität (RTA)-Dienst anstelle von abrufen

Eine weitere best Practice wird verwendet, die für die Echtzeit-Aktivität (RTA) Dienst stattdessen regelmäßig abgerufen. Dieser Dienst stellt ein Websocket, die eine Benachrichtigung an Clients gesendet werden, wenn Zielressourcen für den Dienst ändern. Der RTA-Dienst bietet Benachrichtigungen auf Vorhandensein Änderungen, Änderungen der Statistik, dokumentänderungen Multiplayer-Sitzung und soziale Beziehung. Um zu erfahren, was der Client Informationen interessiert ist, muss der Client zuerst auf das Element über den Websocket abonnieren. Dadurch wird vermieden, Abrufen des Diensts, um Änderungen zu erkennen, da Sie darauf hingewiesen werden genau, wenn das Element geändert.

XSAPI verfügbar macht, die RTA-Dienst als eine Reihe von APIs, mit denen Clients zu abonnieren. Jede dieser APIs haben entsprechende \* \_geändert\_Handler APIs, die in einer Rückruffunktion zu, die aufgerufen wird nutzen, wenn ein Element geändert wird.

* presence\_service::subscribe\_to\_device\_presence\_change
<br>
* presence\_service::subscribe\_to\_title\_presence\_change
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>Xbox Live-Client-Side-Manager verwenden

Neu in XSAPI wir verfügen jetzt über eine Reihe von Managern, die als Cache und den Status von Computer zu, die alle Arbeit fungieren, heben für bestimmte Szenarien.


### <a name="social-manager"></a>Soziale Medien-Manager

Die Social-Manager führt alle Aufgaben rund um Freundeslisten und Profile aus. Ihre Freunde bleibt auflisten, deren Profile und deren Vorhandensein Daten auf dem neuesten Stand mit dem RTA-Dienst. Der Manager macht eine synchrone API, die sehr Spiele-Engine, die benutzerfreundliche ist verfügbar. Spiele können die APIs aufrufen, die häufig, wie sie einen in-Memory-Cache über die neuesten Informationen aus dem Dienst verwaltet. Weitere Informationen finden Sie in der Xbox Live-Dokumentationsseite "Einführung in soziale Netzwerke Manager"

### <a name="multiplayer-manager"></a>Multiplayer-Manager

Für die Verwaltung von Multiplayer-Sitzung ist der Multiplayer-Manager eine gebrauchsfertige Lösung für herkömmliche Multiplayer-Spiele. Die Multiplayer-Manager-API umfasst das Player-Liste und der angegebenen Sitzung Management, Spiele Einladungen, Join ausgeführt wird, Vermittlung, verarbeitet und wird in Ihre vorhandene Netzwerke Lösung integriert. Dies geschieht alle Aufgaben rund um die Implementierung von herkömmlichen Multiplayer-Flows. Weitere Informationen finden Sie auf der Xbox Live-Dokumentationsseite "Einführung in Multiplayer-Manager"


## <a name="throttling-fine-grained-rate-limiting"></a>Drosselung (in Ordnung differenzierte ratenbegrenzung)

Xbox Live-Dienste verfügen, Drosselung, um zu verhindern, dass jedes einzelnes Gerät extremen Auslastung für den Dienst einfügen. Es ist wichtig zu wissen, wenn der Titel des gedrosselt wurden. Sie können feststellen, ob es sich bei Ihrem Titel auf unterschiedliche Weise gedrosselt wurden:


### <a name="monitor-for-http-status-code-429"></a>Monitor für den HTTP-Statuscode 429

Sie können mithilfe von Fiddler, und sehen Sie sich, wenn HTTP Status Code 429 zurückgegeben wird. Die JSON-Antwort enthält Details wie der Endpunkt gedrosselt wurden. Zum Beispiel:

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

Bei Verwendung von XSAPI APIs einen http zurück\_Status\_429\_zu\_viele\_Fehler anfordert, und legen Sie die Fehlermeldung, werden Details wie die-API gedrosselt wurden.

### <a name="debug-asserts"></a>Debug Assert-Vorgänge

Bei Verwendung von XSAPI, wenn der Aufruf wird gedrosselt, während in einem Sandkasten für Entwickler und verwenden einen Debugbuild des Titels, wird es sofort wissen, dass Entwickler können assert ist eine Drosselung aufgetreten. Dies ist versehentlich fehlende 429 Drosselung Fehler aufgrund von falsch geschriebenen Code vermieden werden. Wenn Sie deaktivieren möchten diese Assertionen, um die Arbeit fortsetzen, ohne den fehlerhaften Code beheben, rufen Sie diese API:


> XboxLiveContext -&gt;Settings()--&gt;deaktivieren\_bestätigt\_für\_Xbox\_live\_Drosselung\_in\_Dev\_ Sandboxes (Xbox\_live\_Kontext\_Drosselung\_setting::this\_Code\_muss\_zu\_werden\_geändert\_zu\_vermeiden\_Drosselung);

Beachten Sie jedoch, dass diese API nicht, dass Ihre Titel von gedrosselt wird. Titel des wird weiterhin eine Drosselung werden. Dies einfach deaktiviert die Assert-Vorgänge bei der Verwendung von eines Debugbuilds in Dev Sandboxes. 

### <a name="xbox-live-trace-analyzer-tool"></a>Xbox Live-Ablaufverfolgung Analyzer-tool

Eine weitere Möglichkeit ist eine Ablaufverfolgung der Xbox Live-Aufrufe aufzeichnen und Analysieren dieser Ablaufverfolgung mithilfe der [Xbox Live-Ablaufverfolgung Analyzer-Tool.](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)

Um eine Ablaufverfolgung aufzuzeichnen, Sie können entweder mithilfe von Fiddler zum Aufzeichnen einer. SAZ-Datei, oder indem Sie die integrierte ablaufverfolgungsprotokollierung von XSAPI. Weitere Informationen wie verwenden, aktivieren Sie die ablaufverfolgungen in XSAPI finden Sie unter der Xbox Live-Dokumentationsseite "Analysieren-Aufrufe von Xbox Live-Dienste". Nachdem Sie eine Ablaufverfolgung haben, gedrosselt des Xbox Live-Ablaufverfolgung Analyzer Tool erkannt gewarnt werden Aufrufe an.

## <a name="is-xbox-live-up"></a>Sie ist Xbox werden Live?

Xbox Live ist eine Sammlung von Microservices, die Features wie z. B. das Profil, Freunde und Anwesenheit, Statistiken, Bestenlisten, Erfolge, Multiplayer-, Xbox Live und Vermittlung verfügbar zu machen. Es gibt keinem einzelnen Server oder den Endpunkt, der definiert, ob es sich bei Xbox Live aktiv ist. Wenn ein einzelnen Server ausfällt, wird der Rest der Microservices Xbox Live sind größtenteils unabhängig und betriebsbereit sein sollte.

Wenn ein einzelner Dienst einen vorübergehender Ausfall auftritt, ist es wichtig zu wissen, ob es sich bei diesem Aufruf unternehmenskritisch für Ihr Spiel ist. Versuchen Sie, eine annehmbare Erfahrung zu bieten, während zeitweilige Netzwerk- oder Problemen vorliegen. Z. B. wenn der Dienst vorhanden, zurückgegeben wird nicht die Fehler, die wahrscheinlich aufrufen, unternehmenskritische für Ihr Spiel. Melden Sie also einfach, die dem Benutzer das letzte bekannte Vorhandensein nicht, dem Xbox Live ausgefallen ist.

Xbox Live folgt auch die Konsistenzmodell der letztlichen Konsistenz. Dies bedeutet: Wenn keine neuen Updates vorgenommen werden, schließlich alle Anforderungen für diese Ressource der letzten aktualisierten Wert gemeldet werden. Dies bedeutet in Kraft, ein kleinen Zeitraum, in dem die Informationen wie die Daten veraltet sind überträgt.
