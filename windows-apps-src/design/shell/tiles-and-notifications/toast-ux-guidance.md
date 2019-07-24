---
Description: Erfahren Sie, wie Sie effektive und benutzerorientierte Benachrichtigungen erstellen, mit denen Ihre Benutzer produktiv und zufrieden sind.
title: Popup-UX-Leitfaden
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10-, UWP-, Benachrichtigungs-, Sammlungs-, Gruppen-, UX-, UX-, Leitfaden-, Aktions-, Popup-, Aktions Center-, nicht interruptive, effektive Benachrichtigungen, nicht eindringliche Benachrichtigungen, Aktions fähig, verwalten, organisieren
ms.localizationpriority: medium
ms.openlocfilehash: 111ac9a216b87e120e42c9db7761bd8588548029
ms.sourcegitcommit: 720d8778d94ef44d2f86d2f1f0ebb05d6420805d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68415081"
---
# <a name="toast-notification-ux-guidance"></a>Leitfaden für Popup Benachrichtigungs-UX
Benachrichtigungen sind ein notwendiger Teil des modernen Lebenszyklus. Sie helfen Benutzern, produktiver zu arbeiten und mit apps und Websites zu arbeiten, und bleiben mit allen Updates auf dem laufenden. Allerdings können Benachrichtigungen schnell von Vorteil zu Überlastung und eindringlichem Verhalten werden, wenn Sie nicht auf Benutzer zentrierte Weise entworfen wurden. Ihre Benachrichtigungen werden von einem Rechtsklick entfernt, und es ist unwahrscheinlich, dass Sie deaktiviert werden, wenn Sie ausgeschaltet sind.  Stellen Sie also sicher, dass Ihre Benachrichtigungen den Bildschirmbereich und die Uhrzeit des Benutzers respektieren, damit Sie diesen Engagement-Kanal geöffnet halten können.

> **Wichtige APIs:** [Nuget-Paket für Windows Community Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Wir haben unsere Windows-Telemetrie und andere Fallstudien von erst-und Drittanbietern analysiert, um vier Regeln für eine gute Benachrichtigung zu erhalten.  Wir sind sicher, dass diese Regeln unabhängig von der Plattform universell anwendbar sind und Ihre Benachrichtigungen bei ihren Benutzern positiv beeinflussen können.

## <a name="1-actionable-notifications"></a>1. Handlungs relevante Benachrichtigungen
Handlungs relevante Benachrichtigungen ermöglichen es Ihren Benutzern, produktiv zu sein, ohne die APP zu öffnen.  Obwohl es sehr gut ist, App-Starts zu starten, ist dies nicht die einzige Erfolgs Maßnahme, und die Benutzer können kleinere Aufgaben ausführen, ohne in Ihre App wechseln zu müssen. Dies ist ein sehr leistungsfähiges Tool zum begrenzen ihrer Benutzer.

![Aktions Benachrichtigung mit Eingabe Textfeld und Schaltflächen zum Festlegen von Erinnerungen und Antworten auf die Benachrichtigung](images/actionable-notification-example01.png)

Oben ist ein Beispiel für eine Benachrichtigung, die Aktionen nutzt. Das Gefühl der Fertigstellung von Aufgaben ist ein universell positives Gefühl, und Sie können dieses Gefühl in Ihre APP oder Website bringen, indem Sie Benachrichtigungen senden, die über handlungsfähige Inhalte verfügen. Handlungs relevante Benachrichtigungen können auch dazu beitragen, die Produktivität zu steigern, sowohl in Unternehmens-als auch in Kunden Szenarien, indem Sie die Zeit verkürzen, die Benutzer durchlaufen, um diese kleineren Aufgaben auszuführen. Es wird empfohlen, Aktionen, die von Ihren Benutzern regelmäßig ausgeführt werden, oder Dinge, die Sie für Ihre Benutzer zu Schulen versuchen, zu erstellen.  Beispiele:
* Vorlieben, bevorzugen, markieren von Inhalten
* Genehmigen oder Verweigern von Ausgaben Berichten, Timeout, Berechtigungen usw.
* Inline Antworten auf Nachrichten, e-Mails, Gruppenchats, Kommentare usw.
* Abschließen von Bestellungen mithilfe des [ausstehenden Updates](toast-pending-update.md)
* Festlegen von Warnungen oder Erinnerungen für einen anderen Zeitpunkt sowie möglicherweise die Zeit für die Reservierung von Zeit in einem Kalender

Umsetzbare Benachrichtigungen sind ein sehr leistungsfähiges Tool, mit dem Ihre Benutzer produktiv arbeiten, Aufgaben erledigen und eine gute und effiziente Erfahrung mit Ihrer APP oder Website haben können.  Hier gibt es viele Möglichkeiten. Wenn Sie Hilfe beim Brainstorming benötigen, können Sie sich an das Windows-Benachrichtigungs Team wenden.

## <a name="2-timing-and-urgency"></a>2. Zeitliche Steuerung und Dringlichkeit
Im Gegensatz dazu, wie wir häufig über Benachrichtigungen nachzudenken, ist Echtzeit nicht unbedingt die beste Lösung. Wir empfehlen Entwicklern, sich über den Benutzer Gedanken zu machen, und wenn es sich bei der gesendeten Benachrichtigung um dringende Informationen handelt. Benutzer können problemlos mit zu vielen Informationen überladen und sind frustriert, wenn Sie unterbrochen werden, während Sie versuchen, sich zu konzentrieren. Windows bietet einige Optionen, um die Eindring Kraft der gesendeten Benachrichtigungen zu überprüfen:

**Unformatierte Benachrichtigungen:** Die Verwendung von unformatierten [Benachrichtigungen](raw-notification-overview.md) kann aus vielen Gründen von Vorteil sein, insbesondere, wenn es um die Minimierung von Unterbrechungen für den Benutzer geht  Wenn Sie unformatierte Benachrichtigungen senden, wird Ihre APP im Hintergrund hochskaliert, sodass Sie beurteilen können, ob die Benachrichtigung für eine sofortige Bereitstellung im Kontext der APP sinnvoll ist. Wenn es etwas ist, das Sie dem Benutzer sofort angezeigt werden sollten, können Sie von dort aus einen [lokalen Toast](send-local-toast.md) Popup anzeigen.  Wenn dies der Fall ist, den der Benutzer momentan nicht benötigt, können Sie einen [geplanten Toast](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) erstellen, der zu einem späteren Zeitpunkt ausgelöst wird.


**Ghost Toast:** Sie können auch eine Benachrichtigung auslösen, die in der unteren rechten Ecke des Bildschirms Pop überspringt, und die Benachrichtigung stattdessen direkt an das Wartungs Center senden. Dies wird erreicht, indem die [supresspopup-Eigenschaft](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) auf true festgelegt wird. Obwohl es möglicherweise gewisse Skepsis bei nicht gesendeten Benachrichtigungen außerhalb des Aktions Centers gibt, wird ein 2-3-Mal höheres Engagement für Popups angezeigt, die sich im Aktions Center über Popup Popup befinden.  Benutzer reagieren besser, wenn Sie für den Empfang von Benachrichtigungen bereit sind, und können steuern, wann Sie unterbrochen werden. aus diesem Grund kann der Inhalt im Aktions Center so viel effektiver sein, wenn die Benutzer nicht mit der invasiven Benachrichtigung benachrichtigt werden.

## <a name="3-clear-out-the-clutter"></a>3. Löschen der Übersichtlichkeit
Benachrichtigungen können für eine recht lange Zeit im Aktions Center beibehalten werden (standardmäßig drei Tage).  Es ist zwingend erforderlich, dass Sie sicherstellen, dass der Inhalt, der sich hier befindet, stets auf dem neuesten Stand ist und jedes Mal relevant ist, wenn der Benutzer das Sie verschwenden den Bildschirmbereich des Benutzers und nehmen Slots ein, die für etwas aktuellere Anwendungen verwendet werden können.  Nehmen wir an, dass der Benutzer Ihre e-Mail-Verwaltungs-App installiert und mit diesen e-Mails zehn e-Mails und zehn Benachrichtigungen empfängt.  Abhängig von der gewünschten Darstellung können Sie diese Benachrichtigungen deaktivieren, wenn der Benutzer die entsprechende e-Mail gelesen hat, oder die APP als Möglichkeit zum Entfernen des alten Clusterings aus dem Aktions Center öffnen.

Wir verfügen über eine Reihe von " [deastnotificationhistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) "-APIs, mit denen Sie sehen können, welcher Inhalt im Aktions Center enthalten ist, und wie Sie diese Benachrichtigungen verwalten. Achten Sie auf den Bildschirmbereich des Benutzers, und achten Sie darauf, dass Sie nur relevante und aktuelle Inhalte für die Benutzer anzeigen.

## <a name="4-keeping-organized"></a>4. Organisieren
Wie bereits erwähnt, wird der Inhalt des Aktions Centers drei Tage lang beibehalten.  Um Ihren Benutzern zu helfen, die gesuchten Informationen schnell auszuwählen, ordnen Sie die Benachrichtigungen im Aktions Center mithilfe von [Headern](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) oder [Sammlungen](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)an. Ein Beispiel für einen Header finden Sie unten.

![Popup Beispiele mit Headern mit der Bezeichnung "Camping!!"](images/toast-headers-action-center.png)

Gruppieren Sie diese Benachrichtigungen auf eine Weise, damit relevante Inhalte unverändert bleiben (d. h. Sie sollten verschiedene Sportgruppen in einer Sport-App aufteilen oder Nachrichten nach Gruppenchat sortieren). Auflistungen sind eine offensichtlichere Methode zum Gruppieren von Benachrichtigungen, wohingegen die Kopfzeile weniger gering ist, aber beide ermöglichen es Benutzern, Benachrichtigungen zu selekieren und schneller auszuwählen.



Diese vier Punkte sind eine Anleitung, die wir durch unsere eigene Analyse der Telemetriedaten und durch die ersten und Drittanbieter Experimente wirksam gefunden haben. Beachten Sie jedoch, dass diese Richtlinien nur die folgenden Richtlinien erfüllen: Richtlinien.  Wir sind sicher, dass diese Regeln dabei helfen, das Engagement und die Produktivität Ihrer Benachrichtigungen zu steigern, aber nichts kann die Benutzer zentrierte Betrachtung und das Erlernen aus ihren eigenen Daten ersetzen.  

## <a name="related-topics"></a>Verwandte Themen

* [Popup Inhalt](adaptive-interactive-toasts.md)
* [Unformatierte Benachrichtigungen](raw-notification-overview.md)
* [Ausstehende Aktualisierung](toast-pending-update.md)
* [Benachrichtigungs Bibliothek auf GitHub (Teil des Windows Community Toolkit)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
