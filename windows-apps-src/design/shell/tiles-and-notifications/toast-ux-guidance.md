---
Description: Erfahren Sie, wie effektive und benutzerorientierte Benachrichtigungen zu erstellen, die Ihre Benutzer produktiver und gerne machen.
title: Toast-UX-Leitfaden
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, Uwp, Notification, Collection, Gruppe, Ux, Ux-Anleitungen, Anleitungen, Aktion, Toast, Info-Centers, Noninterruptive, effektive Benachrichtigungen, nicht intrusiv handlungsrelevant Benachrichtigungen verwalten, Organisieren
ms.localizationpriority: medium
ms.openlocfilehash: 878df85db9ab0e33db06a86ddb726f07dc28f013
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615765"
---
# <a name="toast-notification-ux-guidance"></a>Toast-Notification-UX-Leitfaden
Benachrichtigungen sind ein wichtiger Bestandteil des modernen Lebens. Sie können Benutzer produktiver und engagierte mit apps und Websites als auch auf dem Laufenden bleiben mit Updates werden. Benachrichtigungen können jedoch schnell Aktivieren von nützlich, um overbearing und aufwendig, wenn sie nicht zur benutzerorientierten so entworfen wurden. Benachrichtigungen sind eine mit der rechten Maustaste außerhalb wird deaktiviert, und es ist unwahrscheinlich, nachdem sie deaktiviert sind, sie werden aktiviert, erneut.  So stellen Sie sicher, dass Ihre Benachrichtigungen geeigneten Platz auf dem Bildschirm des Benutzers und die Uhrzeit, damit Sie diesen Kanal Engagement geöffnet halten können.

> **Wichtige APIs:** [Nuget-Paket für Windows-Community-Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Wir haben unsere Windows-Telemetrie und andere erste sowie Drittanbieter-Fallstudien, um vier Regeln eine hervorragende Benachrichtigung Story macht träfen, analysiert.  Wir sind überzeugt, diese Regeln sind universell anwendbar ist, unabhängig von der Plattform und hilft Ihrer Benachrichtigungen auf eine positive Auswirkung auf Ihre Benutzer haben.

## <a name="1-actionable-notifications"></a>1. Aktionen erfordernde Benachrichtigungen
Aktionen erfordernde Benachrichtigungen den Benutzern ermöglicht, produktiv zu sein, ohne Sie zu Ihrer app zu öffnen.  Natürlich es großartig ist, damit die app startet, ist dies nicht der einzige Maßstab des Erfolgs und ermöglicht Benutzern werden kleine erreichen können Aufgaben ohne in Ihre app ein äußerst leistungsfähiges Tool im benutzerumgebung sein.

![Aktionen erfordernde Benachrichtigung mit Eingabetextfeld und Schaltflächen Erinnerungen festlegen, und klicken Sie auf die Benachrichtigung reagieren](images/actionable-notification-example01.png)

Über befindet sich ein Beispiel für eine Benachrichtigung, die Aktionen nutzt. Im Gefühl der Abschließen der Aufgaben ist, ein Gefühl durchgehend positiv, und Sie können diese Emotion für Ihre app oder Website erhöhen, durch Senden von Benachrichtigungen, die anwendbare Inhalte enthalten. Aktionen erfordernde Benachrichtigungen können auch die Produktivität zu steigern, sowohl in Unternehmens- und endbenutzeranwendungen Szenarios verringert werden, da die Zeit für Benutzer der Aktion für diese kleineren Aufgaben durchlaufen. Es wird empfohlen, einschließlich Aktionen, die Ihre Benutzer regelmäßig ausführen, oder Elemente, die Sie versuchen, Ihre Benutzer tun zu trainieren.  Beispiele:
* Wie gewünscht, zu Favoriten hinzugefügt, kennzeichnen oder starring Inhalt
* Genehmigen oder Verweigern von spesenabrechnungen, Urlaub, Berechtigungen überprüfen usw.
* Inline auf Nachrichten antworten, e-Mails, Gruppe chats, Kommentare usw.
* Abschluss Bestellungen mit [Update ausstehend](toast-pending-update.md)
* Festlegen von Warnungen und Erinnerungen für einem späteren Zeitpunkt als auch Buchung möglicherweise Zeit in einem Kalender

Aktionen erfordernde Benachrichtigungen sind ein äußerst leistungsfähiges Tool, um Benutzern können Sie produktiv, Aufgaben und haben eine großartige und effiziente Umgebung mit Ihrer app oder Website.  Es gibt viele Möglichkeiten eröffnet hier! Wenn Sie Hilfe zum brainstorming Ideen möchten, sollten Sie gerne wenden Sie sich an das Team der Windows-Benachrichtigungen.

## <a name="2-timing-and-urgency"></a>2. Zeitliche Steuerung und Dringlichkeit
Im Gegensatz zum Ergebnis, wie wir oft über Benachrichtigungen nachdenken ist Real-Time nicht notwendigerweise die besten! Es wird dringend die Entwickler stellen Sie sich über den Benutzer, und wenn die Benachrichtigung, die sie senden dringend Informationen ist oder nicht. Benutzer können problemlos mit zu vielen Informationen überladen werden, und erhalten frustriert, wenn sie unterbrochen wird werden, während sie, sich zu konzentrieren versuchen. Windows bietet einige Optionen für das Ausmaß der Benachrichtigungen zu beachten, die Sie senden:

**Unformatierte Benachrichtigungen:** Mithilfe von [unformatierte Benachrichtigungen](raw-notification-overview.md) kann vorteilhaft sein viele Gründe geben, besonders beim Minimieren von Störungen des Benutzers zu.  Unformatierte Benachrichtigungen senden, wird Ihre app im Hintergrund aktiviert werden, damit Sie beurteilen können, ob die Benachrichtigung sinnvoll, sofort in Ihrer app-Kontext bereitstellen. Ist dies etwas Sie meinen, dass angezeigt werden soll, die dem Benutzer sofort, können Sie pop einen [lokalen Toast](send-local-toast.md) von dort aus.  Wenn es etwas ist der Benutzer muss nicht, finden Sie unter zurzeit, Sie erstellen eine [geplant Toast](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) , die ausgelöst wird, zu einem späteren Zeitpunkt.


**Inaktiver Datensätze Toast:** können Sie auch eine Benachrichtigung, die überspringen, in der unteren rechten Ecke des Bildschirms anzeigen lassen, und senden Sie stattdessen die Benachrichtigung direkt an den Info-Centers auslösen. Dies geschieht durch Festlegen der [SupressPopup Eigenschaft](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) auf "true". Obwohl es möglicherweise einige Skepsis auf Benachrichtigungen wird außerhalb der Info-Centers nicht entfernt wurde, sehen Sie eine 2-3-Mal höhere Engagement für Popups, die im Info-Center über live per pop ausgelesen Toast.  Benutzer sind reaktionsfähiger, sofern sie steuern können, wenn sie unterbrochen werden deshalb Inhalt im Wartungscenter so viel effektiver noninvasively Benutzer benachrichtigt werden kann, und sind bereit, um Benachrichtigungen zu erhalten.

## <a name="3-clear-out-the-clutter"></a>3. Löschen Sie die Übersichtlichkeit hinsichtlich
Benachrichtigungen können im Info-Center für relativ lange (Standard drei Tage) beibehalten.  Es ist zwingend erforderlich, dass Sie, dass die Inhalte, die hier befindet sich auf dem neuesten Stand und relevant ist sicherstellen, jedes Mal, wenn der Benutzer die Info-Center öffnet. Sie sind Platz auf dem Bildschirm des Benutzers für die Ermittlung aufwenden musste, und verbraucht dabei Steckplätze, die für einen aktuelleren verwendet werden können.  Nehmen wir an, die Benutzer Ihre e-Mail-app installiert und empfängt zehn e-Mails und zehn Benachrichtigungen und e-Mails.  Je nach Ihrer gewünschten Erfahrung sollten Sie erwägen, diese Benachrichtigungen deaktivieren, wenn der Benutzer lesen die entsprechende e-Mail-Adresse hat, oder Sie die app als eine Möglichkeit zum Entfernen von alten Überfrachtung aus Info-Centers geöffnet.

Wir haben eine Reihe von [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) APIs, die ermöglichen es Ihnen, erfahren, welche Inhalte im Info-Center ist, und diese Benachrichtigungen zu verwalten. Respektieren Platz auf dem Bildschirm des Benutzers sein, und achten, dass Sie nur relevante und aktuelle Inhalte für Benutzer angezeigt werden.

## <a name="4-keeping-organized"></a>4. Bleiben organisiert
Wie bereits erwähnt, werden der Inhalt im Wartungscenter drei Tage lang beibehalten.  Um Ihre Benutzer die Informationen auswählen, sie schnell möchten, Organisieren Sie die Benachrichtigungen in Aktion Datencenter [Header](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) oder [Sammlungen](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Sehen Sie ein Beispiel für einen Header unten ein.

![Toast-Beispiele mit Header mit der Bezeichnung "Camping!!"](images/toast-headers-action-center.png)

Diese Benachrichtigungen auf eine Weise zu gruppieren, damit relevante Inhalte zusammen bleibt (d. h. Abtrennen von verschiedenen Sportarten Ligen in einer Sport-app oder Sortieren von Nachrichten nach Gruppenchat denken). Sammlungen sind ein offensichtlicher Möglichkeit zum Gruppe von Benachrichtigungen, während Header weniger offensichtlich sind, aber beide selektieren, und wählen Sie Ihre Benachrichtigungen schneller ermöglichen.

## <a name="other-resources"></a>Weitere Ressourcen
Diese vier oben genannten Punkte sind Anleitungen, die wir effektiv über eigene Analyse von Telemetriedaten sowie über die erste und Drittanbieter-Experimente gefunden haben. Bedenken Sie jedoch, die diese Richtlinien sind nichts anderes: Richtlinien.  Wir sind überzeugt, werden diese Regeln helfen, Interaktion und Produktivität Ihrer Benachrichtigungen zu erhöhen, aber nichts kann zur benutzerorientierten denken, und Lernen aus Ihren eigenen Daten ersetzen.  

Wenn Sie noch heute Benachrichtigungen an Ihre UWP-app senden, können Sie Analytics anzeigen, auf was, können Sie Ihre Benachrichtigungen in passiert [Partner Center](https://partner.microsoft.com/dashboard)! Diese Daten bei Verwendung kostenloser Bestandteil der [Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) oder [WNS-APIs](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview). Diese Metriken bietet Ihnen einen besseren Einblick in was, können Sie Ihre Benachrichtigungen auf der Windows-Plattform geschieht auch wie Benutzer Dank der Benachrichtigungen interagiert werden. Zugriff auf diese Daten durch Navigieren zum Menü auf der linken Seite einbinden > Benachrichtigungen, klicken auf der Registerkarte "Analyse" innerhalb der Seite "Benachrichtigungen".  Dies befindet sich am selben Ort, die Sie zum Senden von Benachrichtigungen über Partner Center wechseln würde.

## <a name="related-topics"></a>Verwandte Themen

* [Inhalt der Popupbenachrichtigung](adaptive-interactive-toasts.md)
* [Unformatierte Benachrichtigungen](raw-notification-overview.md)
* [Update ausstehend](toast-pending-update.md)
* [Benachrichtigungsbibliothek auf GitHub (Teil der Windows-Community-Toolkit)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
