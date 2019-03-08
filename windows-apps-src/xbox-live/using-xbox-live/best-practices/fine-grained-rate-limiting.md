---
title: Xbox Live in Ordnung differenzierte Ratenlimit
description: Erfahren Sie, wie Xbox Live differenzierte Ratenlimit funktioniert und wie ein, um zu verhindern, dass der Titel des von der rate begrenzt.
ms.assetid: ceca4784-9fe3-47c2-94c3-eb582ddf47d6
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Drosselung, ratenlimits
ms.localizationpriority: medium
ms.openlocfilehash: b79a4f0a873ffaf5dd824a0c9832f61aa4a7d77f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628685"
---
# <a name="xbox-live-fine-grained-rate-limiting"></a>Xbox Live in Ordnung differenzierte Ratenlimit

## <a name="introduction"></a>Einführung

Dieser Artikel enthält einen Überblick über die Xbox Live in Ordnung differenzierte ratenbegrenzung. Sind zusätzlich zum zusammenfassen, welche Ratenlimit ist in diesem Artikel auch können Sie bestimmen, ob Sie eingeschränkt werden soll, und Sie sind, welche Tools und Ressourcen zur Verfügung.

## <a name="terminology-guide"></a>Terminologie-Handbuch

| Begriff         | Definition                                                  |
|--------------|-----------------------------------------------------------------------------
| FGRL         | Fein abgestimmte Begrenzung der Übertragungsrate                                                  
| XSAPI        | Xbox-Services-Anwendungsprogrammierschnittstelle                                 
| CU           | Die Aktualisierung von Inhalt                                                              
| Burst        | Stellt eine Anzahl von Anforderungen, die in einer kurzen Zeitspanne empfangen          
| Bewältigen      | Stellt eine große Anzahl von aufrufen, die ständig über eine bestimmte Zeitspanne empfangen wurden
| Für den Benutzer und Titel | Stellt die Kopplung eines Benutzers und den Titel in einer einzelnen Entität                    
| XLTA         | Xbox Live Ablaufverfolgung Analyzer-Tool, um festzustellen, ob der Titel des Ratenlimit wird verwendet

## <a name="xbox-live-fine-grained-rate-limiting"></a>Xbox Live in Ordnung differenzierte Ratenlimit

**Genau abgestimmte Ratenlimit** wurde bereitgestellte Förderung **fair Nutzung** von freigegebenen Ressourcen, die für unterschiedliche Bezeichnungen die Xbox Live. Diese Lösung ist wie die meisten herkömmlichen begrenzenden Systeme, die verfügen über einen Dienst aus, halten die Anzahl der Anforderungen, die eine Entität in einem bestimmten Zeitraum vorgenommen hat.

Entitäten, die den angegebenen Grenzwert für Dienste erreicht werden dann in einen Zustand ablehnen verschoben, in denen alle eingehende Anforderungen aus der Entität sofort deaktiviert wird. Entitäten können nur auf diesen Zustand zu beenden, wenn die angegebene Zeitspanne abläuft, sodass die Anzahl der Entitäten zugeordnete zurücksetzen.

Fein abgestimmte Übertragungsbegrenzung zuverlässig verwaltet, verwendet die gleiche Funktionsweise erwähnt, jedoch Nachverfolgen einer Entität FGRL die Kombination von verfolgt **Benutzer- und Titel** und vergleicht die Anzahl der zugeordneten **zwei** verschiedene **Grenzwerte** wie auf einen widersetzen. Duale FGRL-Grenzwerte werden für jeden Dienst, was bedeutet, dass die Anzahl von Anforderungen für GameClips keine Auswirkungen auf die Anzahl von Anforderungen auf Vorhandensein wird erzwungen. Weitere Informationen zu den Benutzer und Titel Kopplung, dual beschränken und das begrenzende HTTP 429-Response-Objekt ist in den folgenden Abschnitten geht.

## <a name="fair-usage"></a>Faire Nutzung

Xbox Live ist davon überzeugt, dass jeder Benutzer muss die gleiche hochwertige auftreten egal welche Spiel/app arbeitsbereichsbesitzer wiedergegeben wird. FGRL wurde entwickelt, um das folgende Szenario zu lösen:

Entwickler ein hat nur einen Titel veröffentlicht, der alle die Xbox live, bewährte Methoden, um eine optimale Nutzung der Dienste sicherzustellen, während Entwickler B auch einfach nur einen Titel veröffentlicht hat diese ein unbekanntes Fehlers hat jedoch folgt. Dieser Fehler bewirkt, dass der Titel und Präsenz von spam Was führt der Dienst stark ausgelastet ist jetzt für jeden Benutzer. Der Dienst verlangsamt wird, und schließlich wird angehalten, unterbrechen die Erfahrung für Entwickler, die ein Benutzer, obwohl er Entwickler B Fehler war, die das Problem verursacht hat. Wenn FGRL implementiert, wurde der Dienst hätte keine Anforderungen von der Fehleranzahl verhält, Titel, sodass Entwickler A Titel der fair Segment des Kreises, Ressourcen bereitstellen können.

## <a name="title-and-user-granularity"></a>Titel und die Benutzer-Granularität

Titel und die Benutzer wurden als Schlüssel ausgewählt, um sicherzustellen, dass faire Nutzung der Ressourcen Xbox Live. Nachverfolgen von nur für den Benutzer würde ein Szenario erstellen, in dem die benutzerfreundlichkeit hängt daher von den einzelnen Titeln Integration wäre. Beispielsweise werden die meisten Titel verwenden den Dienst Personen bereits so, als dieses Beispiel, nehmen wir an, dass fein abgestimmte Begrenzung der Übertragungsrate für den Personen-Dienst, sodass nicht mehr als 100 Anforderungen innerhalb von 5 Minuten eingerichtet wurde. Wenn ein Benutzer ein Spiel zu spielen, die 100 in einer Minute Anforderungen würde das Limit überschritten werden, und der Benutzer nicht können keine weiteren Anforderungen an die Personen-Dienst zu senden; Stellen Sie sich, dass im gleichen Zeitraum der Benutzer dann zurück an den Startbildschirm kehrt und auf seine Freundesliste: Da der Benutzer bereits das Limit überschritten haben würden, die Freunde Liste Aufruf bis der letzten 5 Minuten, die Intervall verstrichen ist, nicht würde, obwohl die Startseite nicht verantwortlich für die Benutzer in der eingeschränkten Zustand gesetzt wurde.

Beschränken Alternativ basiert nur auf der Titel ergibt ein Ergebnis gleichermaßen unfair. Eine Begrenzung pro Titel hat die Beliebtheit der Titel ignoriert, und die Anforderungen nur werden zuerst käme zuerst fungieren wird, bis ein Grenzwert erreicht wird.

Die Kopplung von Benutzer- und Titel wird sichergestellt, dass kein Titel mehr Ressourcen verwendet als die entsprechenden in Anbetracht der Anzahl der aktiven Benutzer und jeder Benutzer eine konsistente Segment des Kreises, Ressource.

![](../../images/FGRL.png)

Das obige Diagramm zeigt einen allgemeinen Überblick darüber, wie die Anforderung verarbeitet wird. Zuerst wird die Anforderung generiert und dann durch den gewünschten Dienst empfangen. Beim Empfang der Anforderung, überprüft das System wie oft den Dienst auf dem Benutzer und dem Titel, die zusammen zugegriffen haben. Wenn die Anforderung unterhalb des Limits liegt, wird dann es ganz normal verarbeitet werden. Wenn die Anforderung gefunden wird, höchstens über dem Grenzwert der Services legen Sie es und stattdessen die Antwort 429 zurück. Der Antwort wird angegeben, wie lange bis zum Punkt würfelvorgänge über, und die Benutzer und Title-Anforderungen verarbeitet werden können.

## <a name="burst-and-sustain-limits"></a>Für Burst und aufrechterhalten von Einschränkungen

In der Vergangenheit besteht aus Ratenlimit ein Grenzwert pro Endpunkt, der über einen bestimmten Zeitraum nachverfolgt wird. Dieser Zeitraum die Zeitspanne darstellt, wenn eine Anzahl von Entitäten nachverfolgt werden. Am Ende des Zeitraums wird die Anzahl der Entitäten auf 0 beginnen wieder zurückgesetzt.

Dieser Ansatz funktioniert für die meisten-API des jedoch war es nicht stabil genug ist, für Spiele und apps, die von Aufrufen von Xbox live. Die oben genannten Lösung wird davon ausgegangen, dass Personen in einer konsistenten stetige vorhersagbare Manor aufrufen. Bei Xbox Live, je nach dem Dienst und dem anfordernden Titel/app unterscheiden sich die Aufrufmuster erheblich.

Wählen einfach ein Grenzwert müssten in diesem Fall an beiden Enden des Spektrums Muster Aufruf beeinträchtigen. Die Xbox Live-Lösung verwendet zwei Punkte und Einschränkungen. Der kleineren Zeitraum wird den Burst-Zeitraum aufgerufen, während die größere längere Zeichenfolge als der Zeitraum sichern Sie Ihren bezeichnet wird. Der Burst für FGRL immer 15 Sekunden ist, während die sichern Sie Ihren immer 300 Sekunden ist zeitburst Zeitraum (5 Minuten) daher während einer sichern Sie Ihren Zeitraum von 5 Minuten, die 20 Punkte. Beide burst, und diesen Einschränkungen sind nachverfolgen zur gleichen Zeit und als solche Count-Anforderungen zur gleichen Zeit. Sowohl der Burst-als auch den sichern Sie Ihren Grenzwert werden festgelegt, für den Dienst, was bedeutet, dass jeder Dienst eine eigene Anzahl von Datenverkehrsspitzen und Sichern Sie Ihren hat. Um besser zu verstehen, wie diese beiden Grenzwerte arbeiten zeigt in der folgenden Tabelle zusammen einen Benutzer einen Titel, der eine Anzahl von Anforderungen an einen Dienst stammt, die FGRL implementiert hat, spielen. In diesem Fall der Burst-Grenzwert liegt bei 30 Anforderungen innerhalb von 15 Sekunden, und der sichern Sie Ihren Grenzwert beträgt 100 Anforderungen über 5 Minuten.

| Zeitraum (in Sekunden)  | Anforderungen pro Burst-Zeitraum  | Anforderungen pro tolerieren Zeitraum  | \# gedrosselte Anforderungen innerhalb des Intervalls von 15 Sekunden | Die beschränken? (burst, unterstützen, oder beides)  |
|-------------|--------------|----------------|-----------------------------------------------------|---------------------------|
| 0-15        | 35           | 35             | 5                                                   | Burst                     |
| 15-30       | 28           | 63             | 0                                                   | n. a.                       |
| 30-45       | 21           | 84             | 0                                                   | n. a.                       |
| 45-60       | 36           | 120            | 20                                                  | Beide                      |
| 60-75       | 24           | 144            | 24                                                  | Bewältigen                   |
| …           | …            | …              |                                                     | …                         |
| 285-300     | 4            | 148            |                                                     | Bewältigen                   |

Die Tabelle zeigt, in der ersten 15 Sekunden die Benutzer Fahrten der Spitzenlast 35 Anforderungen begrenzen. Diese 5 zusätzliche Anforderungen werden gelöscht, und 5 429 Antworten gesendet werden. Es ist erwähnenswert, dass diese Anforderung 5 jedoch weiterhin zählen zu den sichern Sie Ihren Grenzwert gedrosselt. Nach der Grenzwerte ausgelöst wird, werden keine Anforderungen passieren, siehe Grenzwerte Fahrt auf die 45 Sekunden-Marke sowohl erneut bei nur 4 mit dem zweiten 285-Anforderungen zu markieren.

## <a name="http-429-response-object"></a>HTTP 429 Response-Objekt

Wenn die Anzahl der zugeordneten Benutzer und Titel oder höher entweder der Spitzenlast oder tolerieren Grenzwert wird der Dienst die Anforderung wird nicht behandelt und stattdessen eine HTTP 429-Antwort zurück. Die HTTP 429-Code steht für "zu viele Anforderungen" werden durch einen Header mit einem Wert "Wiederholung nach X Sekunden" begleitet. Die FGRL 429 enthält eine Wiederholung nach Header, der die Zeitspanne, die die aufrufenden Entitäten warten soll angibt, bevor Sie es erneut zu versuchen. Entwickler, mit denen XSAPI werden keine Gedanken machen, da XSAPI berücksichtigt und den Retry-After-Header behandelt. Die tatsächliche Antwort enthält die folgenden Felder:

| Feldname      | Werttyp | Beispiel                | Definition                       |
|-----------------|------------|------------------------|----------------------------------|
| Version         | Ganze Zahl    | `"version":1`          |                                  |
| currentRequests | Ganze Zahl    | `"currentRequests":13` | Gesamtzahl der gesendeten Anforderungen    |
| maxRequests     | Ganze Zahl    | `"maxRequests":10`     | Gesamtanzahl der Anforderungen, die zulässig |
| periodInSeconds | Ganze Zahl    | `“periodInSeconds”:15` | Zeitfenster                      |
| Typ            | Zeichenfolge     | `“type”:”burst”`       | Drosselungstyp Grenzwert              |

## <a name="implemented-limits"></a>Implementierten Beschränkungen

Die folgenden Dienste implementiert haben FGRL Grenzen, mit der Durchsetzung von diesen Speichergrenzen seit **Mai 2016**. Wie bereits ausgeführt, werden diese Grenzwerte für alle Sandboxes und Titel übereinstimmen. **Alle Titel, der über die Xbox-Entwickler-Plattform oder der Partner Center veröffentlicht wurde, und vor dem Mai 2016 geliefert wird als Legacy und aus diesem Grund ausgenommen.**

| **Name** | **Burst Grenzwert** (15 Sekunden pro Benutzer und Titel) | **Aufrechterhalten der Grenzwert** (300 Sekunden pro Benutzer und Titel) | **Zertifizierung Grenzwert** (10 x Sustained, 300 Sekunden pro Benutzer und Titel) |
|----------------------------|---------------------------|----------------------------|----------------------------|
| Lesen von Statistiken                 | 100                       | 300                        | 3000                       |
| Profil                    | 10                        | 30                         | 300                        |
| MPSD                       | 30                        | 300                        | 3000                       |
| Präsenz                   | Lesen Sie 10 "," Write 3          | Read Write 30 100         | 1000 lesen, Schreiben von 300       |
| Gemeinschaft                     | 10                        | 30                         | 300                        |
| Bestenlisten               | 30                        | 100                        | 1000                       |
| Erfolge               | 100                       | 300                        | 3000                       |
| Intelligenten Übereinstimmung                | 10                        | 100                        | 1000                       |
| Benutzerbeiträge                 | 100                       | 300                        | 3000                       |
| Schreiben von Statistiken                | 100                       | 300                        | 3000                       |
| Vertraulichkeit                    | 10                        | 30                         | 300                        |
| Kreuz                      | 10                        | 30                         | 300                        |

Die obigen Tabelle stellt dar, die aktuelle Liste der Dienste, die für die FGRL ausgewählt wurden. Diese Liste ist nicht letzte als neue Dienste und vorhandene Dienste hinzugefügt werden können. Wenn ein Dienst in der Tabelle hinzugefügt werden soll wird aktualisiert, und eine Ankündigung erfolgt. Die Grenzwerte, die in der Tabelle dargestellt sollte auch nicht angezeigt werden, als abgeschlossen. Wie Dienste ändern und weiterentwickeln, also die Grenzwerte zu werden, aber Sie benachrichtigt werden, und der legacy-Ausnahmen erforderlichen sein.

Als **April 2018** Titel werden die Zertifizierung nicht übergeben, wenn sie die sichern Sie Ihren überschritten (Grenze, die die Rate einschränken wirksam) einschränken, indem Sie 10 X.  Z. B. wenn der längeren Grenzwert, an dem FGRL wirksam wird, innerhalb von 300 Sekunden wie in der obigen Tabelle angegeben auf 300 Aufrufe festgelegt ist, fehl Titel oder höher 3000 Aufrufe in 300 Sekunden Zertifizierung. 

## <a name="service-mapping-and-title-effects-of-rate-limiting"></a>Zuordnung und Title-Auswirkungen der Begrenzung der Übertragungsrate

| **Name** | **Dienstendpunkt** | **Anticipiated Spiel FGRL Auswirkung**
| --- | :---: | --- 
| Lesen von Statistiken | userstats.xboxlive.com | Erfolge oder Bestenlisten-Einträge nicht aktualisiert oder abgerufen.
| Profil | profile.xboxlive.com | Daten des Spielers, nicht aktualisiert oder angezeigt werden.
| MPSD | sessiondirectory.xboxlive.com | Joins/Einladungen würde nicht ordnungsgemäß abgeschlossen wurde, Sitzungen erstellt bzw. ordnungsgemäß aktualisiert, die Title-Fehlern kommen kann.
| Präsenz | presence.xboxlive.com | In-Game-Anwesenheit des Spielers wäre nicht korrekt.
| Gemeinschaft | social.xboxlive.com | Wirkt sich auf alle Freunde Schreibvorgänge (z. B. einen Freund hinzugefügt, sodass jemand bevorzugten usw.) und kann die Friend-Lesevorgänge beeinträchtigen (z. B. meiner Friend-Liste abrufen). Entwicklern wird empfohlen, die Peoplehub für Lesen statt social.xboxlive.com aufrufen.
| Bestenlisten | leaderboards.xboxlive.com | In-Game-Benutzeroberfläche für Bestenlisten würde nicht Auffüllen/aktualisieren.
| Erfolge | achievements.xboxlive.com | In-Game-Benutzeroberfläche für Erfolge entsperrt wird nicht aktualisiert werden.
| Intelligenten Übereinstimmung | momatch.xboxlive.com | Übereinstimmungen würde nicht erfolgreich eingerichtet werden.
| Benutzerbeiträge | userposts.xboxlive.com | Benutzerbeiträge werden nicht angezeigt.
| Schreiben von Statistiken | statswrite.xboxlive.com | Erfolge oder Bestenlisten Einträge, die nicht aktualisiert werden.
| Vertraulichkeit | privacy.xboxlive.com | Datenschutz Fehler möglicherweise blockiert den Zugriff für alle Aufrufer.
| Kreuz | Clubhub.xboxlive.com | Player möglicherweise nicht ihrer in-Game-Clubs angezeigt.

**HINWEIS:** Die neueste API-Zuordnung wird regelmäßig aktualisiert, und finden Sie unter [Live-Ablaufverfolgung Analyzer Verbindungszuordnung](https://github.com/Microsoft/xbox-live-trace-analyzer/blob/master/Source/XboxLiveTraceAnalyzer.APIMap.csv).

## <a name="faq"></a>Häufig gestellte Fragen

## <a name="how-can-i-determine-i-am-being-throttled-and-what-steps-can-i-take"></a>Wie kann ich, ich bin gedrosselt, und welche Schritte kann ich erstellen?

Finden Sie in der [Xbox Live-Best Practices](best-practices-for-calling-xbox-live.md) Dokument darin enthaltenen Schritte für die Verbesserung Ihrer Aufrufmuster sowie eine Erläuterung, wie die XSAPI Assertion und XSAPI soziale Netzwerke und Multiplayer-Manager verwendet werden können, um Sie zu benachrichtigen und Minimierung von Problemen mit Drosselung.

Eine weitere Möglichkeit ist eine Ablaufverfolgung der Xbox Live-Aufrufe aufzeichnen und Analysieren dieser Ablaufverfolgung mithilfe der [Xbox Live-Ablaufverfolgung Analyzer-Tool](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls).  Um eine Ablaufverfolgung aufzuzeichnen, Sie können entweder mithilfe von Fiddler zum Aufzeichnen einer. SAZ-Datei, oder Verwenden der integrierten ablaufverfolgungsprotokollierung von XSAPI. Weitere Informationen wie verwenden, aktivieren Sie die ablaufverfolgungen in XSAPI finden Sie unter der Xbox Live-Dokumentationsseite "Analysieren-Aufrufe von Xbox Live-Dienste". Nachdem Sie eine Ablaufverfolgung haben, gedrosselt des Xbox Live-Ablaufverfolgung Analyzer Tool erkannt gewarnt werden Aufrufe an.

Sie können die bewährten Methoden finden Sie Artikel in der Dokumentation des GDNP "und" SDK "und" xdk-Version 1602 und höher.

### <a name="can-limits-change"></a>Werden Grenzwerte können geändert?

Die Absicht ist, dass die veröffentlichten Grenzwerte im Laufe der Zeit nicht geändert werden. Allerdings würde die Notwendigkeit auftreten, ist es möglich, dass einige der Grenzen strengere hergestellt werden konnte; In diesem Fall würde Titel, die bereits auf "RETAIL" veröffentlicht die aktualisierte Grenzwerte ausgeschlossen vorgenommen werden.

### <a name="are-more-services-going-to-get-limits"></a>Wollen Weitere Dienste Grenzwerte erhalten?

Ja, weitere Dienste und neue Dienste können und werden Einschränkungen erstellen. Jedoch einfach wie dieser ersten Version der FGRL Sie benachrichtigt werden, und entsprechenden Vorsichtsmaßnahmen ausgeführt werden.

### <a name="when-will-these-changes-take-effect"></a>Wann werden diese Änderungen wirksam?

Begrenzung der Datenübertragungsrate haben erzwungen wurde, seit **Mai 2016**.  Als **April 2018**, Titel, die über den angegebenen durchgängig Grenzwerte Faktor 10 oder mehr wird den Xbox-Zertifizierungsprozess nicht bestanden.

### <a name="what-if-we-cant-adhere-to-the-limits"></a>Was geschieht, wenn wir die Grenzen nicht zurecht?

Informieren Sie sich die [Xbox Live-Best Practices](best-practices-for-calling-xbox-live.md) und stellen Sie sicher, Sie werden diese Schritte.  Erwägen Sie auch die Verwendung der [soziale Manager](../../social-platform/intro-to-social-manager.md) , wenn Sie die Rate begrenzt, mit der US-Sozialversicherungsnummern Dienste werden.

Wenn Sie nach dem Ausführen dieser Schritte können Sie immer noch nicht unter die Grenzwerte bleiben, wenden Sie sich an Ihren Developer Account Manager.

**ANMERKUNG: Titel mindestens mit 10 x den angegebenen sichern Sie Ihren Grenzwert werden nicht zugelassen Zertifikat nach dem April 2018 übergeben**.  Z. B. wenn der längeren Grenzwert, an dem FGRL wirksam wird, innerhalb von 300 Sekunden wie in der obigen Tabelle angegeben auf 300 Aufrufe festgelegt ist, fehl Titel oder höher 3000 Aufrufe in 300 Sekunden Zertifizierung.

### <a name="what-about-my-existing-title"></a>Was ist mit meinem vorhandenen Titel?

Alle Titel im Einzelhandel vor April 2018 gelten die Vorgängerversion und sind ausgenommen.

### <a name="content-updates"></a>Die Inhaltsupdates?

Für einen älteren oder ausgeschlossenen werden Inhaltsupdates auch ausgenommen werden sollen, aber wir Ihnen die Nutzung von Tools und Ressourcen empfehlen für die Service Integration Aspekte Ihres Spiels zu optimieren.

### <a name="can-i-get-an-exemption-for-my-game-until-i-can-make-a-content-update"></a>Erhalte ich eine Ausnahme für Mein Spiel bis ich ein Inhaltsupdate machen können?

Sprechen Sie mit Ihrem Developer-Konto-Manager an.
