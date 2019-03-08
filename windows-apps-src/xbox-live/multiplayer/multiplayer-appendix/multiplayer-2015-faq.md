---
title: Multiplayer 2015 – Häufig gestellte Fragen und Problembehandlung
description: Häufig gestellte Fragen zur Xbox Live Multiplayer-2015 und Problembehandlung.
ms.assetid: 75823f10-b342-4e20-b885-e5ad4392bc3d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: 171d80f4fc925d95d80043f40bb387b045a4fe23
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625525"
---
# <a name="multiplayer-2015-faq-and-troubleshooting"></a>Multiplayer 2015 – Häufig gestellte Fragen und Problembehandlung

-   Entwickle ich einen neuen Titel ein. Welche Multiplayer-API-Elemente sollte ich verwenden?
-   Wie kann ich die neue multiplayer-API von einem Dienst zugreifen?
-   Kann mein Titel ändert sich für mehr als eine Sitzung abonnieren?
-   Wird sofort ein Benutzer werden entfernt, wenn er eine Netzwerkverbindung verliert, oder der Konsole deaktiviert?
-   Wie finde ich, welche SCID sitzungsvorlage und Sandbox verwenden?
-   Gibt es ein Beispiel für einen Anforderungstext, in dem ich für meine Titel mit dem verglichen werden können?
-   Beim Aufrufen von MPSD, erhalte ich einen HTTP 403 /-Code.
-   Beim Aufrufen von MPSD, erhalte ich einen HTTP/404-Code.
-   Warum erhalte ich einen HTTP/404-Code beim Aufrufen von **MultiplayerService.WriteSessionByHandleAsync** oder **MultiplayerService.GetCurrentSessionByHandleAsync**?
-   Beim Aufrufen von MPSD, erhalte ich einen HTTP 412 /-Code.
-   Ich erhalte die HTTP/400, 405, 409, 503, usw. Code beim Aufrufen von MPSD.
-   Was kann oder sollte werden geändert in den Vorlagen für die Sitzung für mein Titel?
-   Ich erhalte einen Fehler, der besagt, dass meine Sitzung nicht initialisiert wurde.
-   Meine Sitzung wird nicht erstellt, und ich erhalte einen HTTP/204-Code.
-   Wann sollte ich MPSD abrufen?
-   Was geschieht, wenn ein Player, die reserviert oder eingeladen wurde für die Sitzung ist es nicht verknüpfen?
-   Warum würde eine erstellte Sitzung konnte nicht Vermittlung werden gefunden?
-   Was ist der wesentliche Unterschied zwischen die Möglichkeit, die Parteien von 2015 Multiplayer ordnungsgemäß verwendet werden und die Möglichkeit, die sie in der 2014 Multiplayer verwendet wurden?
-   Wenn eine Spiele-Sitzung öffnen Player Slots und unterstützt von Join ausgeführt wird, warum Benutzer möglicherweise keinen würden, finden die Sitzung nach dem Starten?
-   Wenn eine Spiele-Sitzung geöffnet ist, kann ein Benutzer, der nur ein Spiel beigetreten ist einfach beitreten der Sitzung und experimentieren, ohne zu warten, bis die Reservierung?
-   Bei große Spiele Sitzungen in meiner Titel spielen, warum angezeigt werden nicht alle Mitglieder der Sitzung den game Einladung Toast?
-   Ich sehe inkonsistentes Spiels Verhalten und Protokoll Aktivierungsinformationen verweisen auf Spiele Partei Funktionen erhalten haben.
-   Warum sehe ich die Syntax für v105 Sitzung Dokumente in Mein ablaufverfolgungen, obwohl ich einer sitzungsvorlage für v107 konfiguriert haben?


### <a name="i-am-developing-a-new-title-which-multiplayer-api-elements-should-i-use"></a>Entwickle ich einen neuen Titel ein. Welche Multiplayer-API-Elemente sollte ich verwenden?

Multiplayer-2014-Funktionalität für vorhandene Titel gilt weiterhin, aber die zugehörigen API-Elemente, die am wahrscheinlichsten werden als veraltet markiert. Wir empfehlen dringend die Verwendung von 2015 Multiplayer-Spiele, bei der Vorbereitung Ihrer Clients im 2015 veröffentlicht.


### <a name="how-can-i-access-the-new-multiplayer-api-from-a-service"></a>Wie kann ich die neue multiplayer-API von einem Dienst zugreifen?

Finden Sie unter [aufrufen MPSD](multiplayer-session-directory.md).


### <a name="can-my-title-subscribe-to-changes-for-more-than-one-session"></a>Kann mein Titel ändert sich für mehr als eine Sitzung abonnieren?

Ja, kann ein Titel abonnieren, um Änderungen auf bis zu 10 Sitzungen pro Verbindung.


### <a name="will-a-user-be-removed-immediately-if-heshe-loses-a-network-connection-or-turns-off-the-console"></a>Wird sofort ein Benutzer werden entfernt, wenn er eine Netzwerkverbindung verliert, oder der Konsole deaktiviert?

Die WebSocket-Verbindung ermöglicht MPSD schnell erkennen die Trennung von Client und den Client auf "inaktiv" festlegen. Sitzung Mitglieder werden entfernt, sobald Ablauf des Timeouts für inaktive entfernen. Weitere Informationen finden Sie unter [Sitzungstimeouts](mpsd-session-details.md).


### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>Wie finde ich, welche SCID sitzungsvorlage und Sandbox verwenden?

Würden Sie nicht die anfängliche Registrierung von Titel, beteiligt, wenn diese Informationen erstellt wurde, können Sie nicht auf diese Informationen zugreifen. Wenden Sie sich an Ihren Developer Account Manager, die für Sie diese Daten erhalten können.


### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-to-the-one-for-my-title"></a>Gibt es ein Beispiel für einen Anforderungstext, in dem ich für meine Titel mit dem verglichen werden können?

Finden Sie in der Struktur der Anforderung in **MultiplayerSessionRequest (JSON)**


### <a name="i-am-getting-an-http403-code-when-calling-mpsd"></a>Beim Aufrufen von MPSD, erhalte ich einen HTTP 403 /-Code.

Dies ist normalerweise eine Berechtigungen oder Bereich-Problem. Sammeln Fiddler-ablaufverfolgungen erhalten Sie weitere Informationen und anschließend überprüfen die Nachrichten, die als Teil der HttpResponse-Textkörper für allgemeine 403 Nachrichten zurückgegeben werden:

-   Die angeforderte Dienst-Konfiguration kann nicht zugegriffen werden.
    1.  Vergewissern Sie sich, dass Sie ein Konto verwenden, die Zugriff auf die Sandbox hat.
    2.  Vergewissern Sie sich, dass Sie in der richtigen Sandbox sind.
     3.  Wenn Sie die Zertifikatauthentifizierung verwenden und dieser Fehler angezeigt, wenden Sie sich an Ihren Kundenbetreuer, Entwickler.   Die angeforderte Sitzung kann nicht zugegriffen werden. Der aufrufende Benutzer benötigen Multiplayer-Berechtigungen, und private Sitzungen können nur von Mitgliedern der Sitzung gelesen werden.

    Haben nicht die Möglichkeit, die die Sitzung möglicherweise sehen werden, da Sie versuchen, eine Sitzung zuzugreifen, die Private Sichtbarkeit. Wenn die Sichtbarkeit auf privat festgelegt ist, können nur Mitglieder der Sitzung Sitzung Dokument lesen.

| Hinweis                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Benutzer benötigen ein Konto "Gold", um eine neue Sitzung zu registrieren. Ohne "Gold" Berechtigungen für einen Benutzer mit unterschiedlichen zurückgeben Anforderungen zum Registrieren einer neuen Sitzungs HTTP 403 /. |

-   Hauptteil der Anforderung kann nicht vorhandenen Memberverweise enthalten, es sei denn, der Prinzipal der Authentifizierung ein Servers umfasst.

    Sie können nicht ein anderer Benutzer eine Sitzung im Auftrag eines Benutzers hinzufügen. Sie können nur einladen. Legen Sie den Index auf "reservieren\_&lt;Anzahl&gt;" einen Player einladen.


### <a name="i-am-getting-an-http404-code-when-calling-mpsd"></a>Beim Aufrufen von MPSD, erhalte ich einen HTTP/404-Code.

Sammeln Sie Fiddler-ablaufverfolgungen, um weitere Informationen zu erhalten, und führen Sie dann auf die folgenden:

1.  Überprüfen Sie die Nachricht als Teil des Texts HttpResponse für allgemeine 404-Meldungen zurückgegeben:
    -   Der angeforderte Dienstkonfiguration ist entweder ungültig oder nicht konfiguriert für Sitzungen an. Stellen Sie sicher, dass die verwendeten SCID korrekt ist.
    -   Die angeforderte Sitzung wurde nicht gefunden. Stellen Sie sicher, dass die Sitzung erstellt wird und die sitzungsvorlage richtig ist, bevor Sie sie abrufen. Sie können eine Sitzung mit einem PUT-Aufruf erstellen.

2.  Überprüfen Sie den URI, die Sie verwenden.
3.  Starten Sie die Konsole neu, oder versuchen Sie es mit einem neuen Benutzer.
4.  Überprüfen Sie die Spiel-Entwicklerforen für den Fehlercode oder andere Lösungen.
5.  Vergewissern Sie sich, dass die Sitzung nicht gelöscht wurde, als Ergebnis leer. Sitzungen können leer sein, da das Timeout für Benutzer. In diesem Fall ist es häufig, da alle Member der Sitzung in einem Zustand befinden, auf die ein Timeout, z. B. "ready" oder "inaktiv angewendet wird". Finden Sie unter [Sitzung Benutzerstatus](mpsd-session-details.md) für Benutzerdetails-Zustand.


### <a name="why-am-i-getting-an-http404-code-when-calling-multiplayerservicewritesessionbyhandleasync-or-multiplayerservicegetcurrentsessionbyhandleasync"></a>Warum erhalte ich einen HTTP/404-Code beim Aufrufen von **MultiplayerService.WriteSessionByHandleAsync** oder **MultiplayerService.GetCurrentSessionByHandleAsync**?

Wenn dem Titel des Handle als Reaktion auf eine Join-Protokoll-Aktivierung mit einer Handle-ID eine Sitzung MPSD zugreift, kann die Handle-ID in der protokollaktivierung veralteter für eine der folgenden Gründe sein:

-   Die UI-Ansicht in der Xbox-Shell, die von der der Titel die Verknüpfung initiiert möglicherweise veraltet. Einige Ansichten der Benutzeroberfläche, z. B. des Benutzers Profilkarte, werden nicht automatisch Join Handles aktualisiert, während sie offen sind. Vermeiden des HTTP/404-Codes, sollte der Titel schließen und öffnen Sie erneut die Ansicht, um die Daten vor dem Beitritt zu aktualisieren.
-   Der Benutzer, den der Titel zu verknüpfen, ist möglicherweise Aktivität gewechselt sind Sitzungen nach der Titel der Join-Operation aus der Xbox-Shell-Benutzeroberfläche. Aus diesem Grund für den HTTP/404-Code sollte nur selten.

In beiden Fällen sollte Code Titel dem verknüpften Benutzer anzeigen, eine Fehlermeldung, dass der Join-Fehler.


### <a name="i-am-getting-an-http412-code-when-calling-mpsd"></a>Beim Aufrufen von MPSD, erhalte ich einen HTTP 412 /-Code.

Die folgende Anforderung gibt die HTTP/412 zurück, wenn die Sitzung bereits vorhanden ist:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-None-Match: *


Die folgende Anforderung gibt die HTTP/412 zurück, wenn das Etag für die Sitzung nicht den If-Match-Header entspricht:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D


Finden Sie unter [Sitzung Synchronisierungsupdates](multiplayer-session-directory.md) für Weitere Informationen.


### <a name="i-am-getting-an-http400-405-409-503-etc-code-when-calling-mpsd"></a>Ich erhalte die HTTP/400, 405, 409, 503, usw. Code beim Aufrufen von MPSD.

Erfassen von Fiddler-ablaufverfolgungen, um weitere Informationen zu erhalten, und klicken Sie dann von Meldungen der laufzeitüberprüfung, die als Teil des Texts HttpResponse zurückgegeben. Daraufhin sollte Ihnen genügend Informationen zum Identifizieren und Beheben der Fehler, oder Sie können die Entwicklerforen für Auflösungen suchen.

Erhalten Sie auch den Antworttext bei Verwendung von XSAPI, siehe [Problembehandlung für die Xbox Live Services-API](../../using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md). Sie können auch Ihren Code verwenden die **XboxLiveContextSettings.EnableServiceCallRoutedEvents** Eigenschaft, um die Ausgabe an den Titel Benutzeroberfläche zu senden.


### <a name="what-can-or-should-i-change-in-the-session-templates-for-my-title"></a>Was kann oder sollte werden geändert in den Vorlagen für die Sitzung für mein Titel?

Vorlagen für ereignissitzungen sind Muster für die Sitzungen, und kann nicht überschrieben werden die Konstanten, die bereits in den Vorlagen festgelegt. Allerdings können Sie neue Eigenschaften zu den Vorlagen hinzufügen. Finden Sie unter [Vorlagen für Ereignissitzungen MPSD](multiplayer-session-directory.md) Weitere Details.


### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>Ich erhalte einen Fehler, der besagt, dass meine Sitzung nicht initialisiert wurde.

Beispiel für Antwort-Fehler:

400 – \[ResponseBody\]: In dieser Sitzung wird für verwaltete Initialisierung erfordern mindestens 2 Elemente zu konfigurieren.

Die Sitzung kann nicht erstellt werden, weil nicht genügend Sitzung Member Reservierungen mit dem 'initialize' Feld auf "true" in der Anforderung enthalten sind. Ihr Code kann dieses Feld festlegen, für einen Member mithilfe der *InitializeRequested* -Parameter für die **MultiplayerSession.AddMemberReservation** Methode oder der **MultiplayerSession.Join** Methode.

Bei der Initialisierung in Ihre sitzungsvorlage angegeben ist, stellen Sie sicher, "Initialize": "true" festgelegt ist ausreichend, von der Members-Reservierungen für die Vermittlung QoS zu übergeben. Weitere Informationen finden Sie unter [Sitzungsinitialisierung Ziel und QoS](smartmatch-matchmaking.md).


### <a name="my-session-is-not-being-created-and-im-getting-an-http204-code"></a>Meine Sitzung wird nicht erstellt, und ich erhalte einen HTTP/204-Code.

Dieser Statuscode gibt an, dass keine Benutzer bei der Erstellung der Sitzung hinzugefügt wurden. Wenn kein Benutzer vorhanden sind, in der Sitzung, wenn er erstellt, und das Sitzungstimeout leer ist, NULL (Standard), wird die Sitzung nicht erstellt werden.

Stellen Sie sicher, dass Sie beim Erstellen einer Sitzung mindestens ein einziger Player enthalten. Erhalten Sie für dedizierte Server-Szenarien einen Player, wer versucht, eine Übereinstimmung zu erstellen, oder erstellen eine Übereinstimmung, und machen, dass Spieler ersten in der Übereinstimmung muss. Alternativ können Sie ändern oder entfernen die leere Sitzung ist abgelaufen. Weitere Informationen finden Sie unter [Sitzungstimeouts](mpsd-session-details.md).


### <a name="when-should-i-poll-mpsd"></a>Wann sollte ich MPSD abrufen?

Ihre Titel müssen vermeiden MPSD abrufen. Wenn Sie ein Titel Änderungen MPSD Sitzungen ermittelt werden muss, sollten sie für Sitzungsänderungsereignisse abonnieren. Weitere Informationen finden Sie unter [Vorgehensweise: Melden Sie sich für Änderungsbenachrichtigungen für MPSD Sitzung](multiplayer-how-tos.md).


### <a name="what-happens-if-a-player-who-was-reserved-or-invited-to-the-session-does-not-join-it"></a>Was geschieht, wenn ein Player, die reserviert oder eingeladen wurde für die Sitzung ist es nicht verknüpfen?

Das hängt davon ab, unabhängig davon, ob der Titel ausgeführt wird, wenn der Spieler benachrichtigt wird, dass das Spiel Sitzung bereit. Wenn der Spieler befindet sich im Titel und der game-sitzungsbenachrichtigung aus dem Titel UI nicht akzeptiert, es obliegt der Titel den Spieler das Spiel-Sitzung mit Entfernen der **MultiplayerSession.Leave** Methode.

Wenn der Titel wird oder nicht ausgeführt, die Shell eine Benachrichtigung stellt darüber informiert, dass dem Spieler ein Steckplatz verfügbar ist. Wenn der Spieler ablehnt oder die die systembenachrichtigung ignoriert, entfernt MPSD dieser Person aus der game-Sitzung.


### <a name="why-would-a-created-session-not-be-found-by-matchmaking"></a>Warum würde eine erstellte Sitzung konnte nicht Vermittlung werden gefunden?

Auf der Xbox One reicht es nicht einfach Erstellen einer Sitzung für die Vermittlung, um die neue Sitzung zu finden. Sie müssen eine Match-Ticket, um die beginnen mit der Ankündigung der-Sitzungs mit dem Vermittlungsdienst erstellen. Finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).


### <a name="what-is-the-key-difference-between-the-way-parties-are-properly-used-by-2015-multiplayer-and-the-way-they-were-used-in-2014-multiplayer"></a>Was ist der wesentliche Unterschied zwischen die Möglichkeit, die Parteien von 2015 Multiplayer ordnungsgemäß verwendet werden und die Möglichkeit, die sie in der 2014 Multiplayer verwendet wurden?

In 2015 Multiplayer-Spiele definiert die multiplayer-API keine Spiele Partei auf Systemebene, die nur Chat Parteien. Der Titel statt Spiele Parteien, verwendet Sitzungen, um das Beitreten zu steuern, Einladungen und -bezogene Features. Für Multiplayer-Spiele 2014, die multiplayer-API auf der Xbox One an gut sichtbarer Stelle verwendet des Konzepts des Spiels Partei (**Partei** Klasse), lädt eine auf Systemebene Join Lobby statt Spiel effektiv implementiert.


### <a name="if-a-game-session-has-open-player-slots-and-supports-join-in-progress-why-would-users-not-be-able-to-find-the-session-once-it-has-started"></a>Wenn eine Spiele-Sitzung öffnen Player Slots und unterstützt von Join ausgeführt wird, warum Benutzer möglicherweise keinen würden, finden die Sitzung nach dem Starten?

Wenn eine Spiele-Sitzung gestartet wird, ist es nicht mehr auf dem Vermittlungsdienst angekündigt. Wenn Player Slots innerhalb der Sitzungs verfügbar, und möchte, dass der Arbiter (Host) neue Spieler aufmerksam zu machen, muss der Arbiter ein neues Übereinstimmung Ticket erstellen, bei der laufenden Sitzung mit dem **MatchmakingService.CreateMatchTicketAsync** -Methode. Das Ticket kündigt die Sitzung wieder an und mehr Spieler sucht. Finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).


### <a name="if-a-game-session-is-open-can-a-user-who-has-just-joined-a-game-simply-join-the-session-and-start-playing-without-having-to-wait-for-the-reservation"></a>Wenn eine Spiele-Sitzung geöffnet ist, kann ein Benutzer, der nur ein Spiel beigetreten ist einfach beitreten der Sitzung und experimentieren, ohne zu warten, bis die Reservierung?

Ja. Dies ist besonders in Fällen, in dem Titel des mehrere Sitzungen verwendet, um untergeordnete Gruppen der Spieler innerhalb einer Spiele-Sitzung zu verfolgen. Der verknüpfte Benutzer möglicherweise verknüpfen die Sitzung, seiner Gruppe darstellt, und klicken Sie dann das größeren Spiels beitreten müssen.


### <a name="when-large-game-sessions-are-playing-in-my-title-why-arent-all-session-members-seeing-the-game-invite-toast"></a>Bei große Spiele Sitzungen in meiner Titel spielen, warum angezeigt werden nicht alle Mitglieder der Sitzung den game Einladung Toast?

Wenn der Titel der Sitzung durch Verknüpfen ein Benutzers hinzufügt, legt der Titel immer die **MultiplayerSessionMember.InitializeRequested** Eigenschaft auf "true". Dadurch wird die MPSD für den Rest der Sitzung Elemente zu warten, bevor Sie das Spiel aus der Initialisierungsphase verschieben. Andernfalls müssen Benutzer ein sehr kurzes Zeitfenster, verknüpfen und Sie können Fehler auftreten, Popupbenachrichtigungen und Benachrichtigungen über Änderungen der Sitzung.


### <a name="i-am-seeing-inconsistent-game-behavior-and-have-received-protocol-activation-information-referencing-game-party-features"></a>Ich sehe inkonsistentes Spiels Verhalten und Protokoll Aktivierungsinformationen verweisen auf Spiele Partei Funktionen erhalten haben.

Dies gibt an, dass Sie das Kombinieren von werden Multiplayer für 2014 und 2015 Multiplayer-Funktionalität. Die API für 2015 Multiplayer-Spiele sollten nie mit für 2014 Multiplayer-Spiele geschriebenen Code verwendet werden.


### <a name="why-am-i-seeing-the-syntax-for-v105-session-documents-in-my-traces-although-i-have-configured-a-v107-session-template"></a>Warum sehe ich die Syntax für v105 Sitzung Dokumente in Mein ablaufverfolgungen, obwohl ich einer sitzungsvorlage für v107 konfiguriert haben?

Die MPSD führt die automatische Konvertierung zwischen Sitzung Dokumentversionen basierend auf der Clientanforderung. Alle Xbox-Dienst-APIs wird derzeit v105 für Anforderungen an die MPSD verwenden. Kann dies zu unterschiedlichen Syntax zwischen Vorlagen für ereignissitzungen v107 und v105 verfolgt aber hat keine andere funktionale Auswirkung. Vorlagen für ereignissitzungen sollte als v107 konfiguriert werden.

© 2015 Microsoft Corporation. Alle Rechte vorbehalten.
Übermitteln von Feedback auf <https://forums.xboxlive.com/spaces/22/index.html>.
Version: 2.0.90731.0
