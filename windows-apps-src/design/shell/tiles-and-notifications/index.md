---
author: mijacobs
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: Kacheln, Signale und Benachrichtigungen
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d1282bb0de6f21b19e43771020d0f1b47d4ea93c
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2017
ms.locfileid: "1393699"
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>Kacheln, Signale und Benachrichtigungen für UWP-Apps
 

Erfahren Sie, wie Sie mithilfe von Kacheln, Signalen, Popups und Benachrichtigungen Einstiegspunkte in Ihre App bereitstellen und Benutzer auf dem neuesten Stand halten können.

> **Wichtige APIs**: [UWP Community Toolkit Notifications-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
Eine Kachel ist die Darstellung einer App im Startmenü. Jede UWP-App verfügt über eine Kachel. Sie können verschiedene Kachelgrößen festlegen (klein, mittel, breit und groß).</p>

<p>Sie können eine <em>Kachelbenachrichtigung</em> verwenden, um die Kachel zu aktualisieren und neue Informationen an den Benutzer zu übermitteln, z.B. neue Schlagzeilen oder den Betreff der letzten ungelesenen Nachricht.</p>

<p>Sie können ein <em>Signal</em> verwenden, um Statusinfos oder zusammenfassende Informationen in Form einer vom System bereitgestellten Glyphe oder einer Zahl von 1 bis 99 bereitzustellen. Signale werden auch auf dem App-Symbol auf der Symbolleiste dargestellt. </p>

<p>Eine <em>Popupbenachrichtigung</em> ist eine Benachrichtigung, die Ihre App über ein Popup-UI-Element namens <em>Popup</em> (oder <em>Banner</em>) an den Benutzer sendet. Die Benachrichtigung kann unabhängig davon gelesen werden, ob sich der Benutzer in Ihrer App befindet oder nicht.</p>
<p>Eine <em>Pushbenachrichtigung</em> oder <em>unformatierte Benachrichtigung</em> ist eine Benachrichtigung, die über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Services, WNS) oder eine Hintergrundaufgabe an Ihre App gesendet wird. Ihre App kann auf diese Benachrichtigungen entweder durch Benachrichtigen des Benutzers reagieren, das etwas von Interesse geschehen ist (über Signal-, Kachel- oder Popupaktualisierungen), oder sie können die gewünschte Reaktion selbst festlegen.</p>

 
## <a name="tiles"></a>Kacheln
| Artikel | Beschreibung |
| --- | --- |
| [Erstellen von Kacheln](creating-tiles.md) | Passen Sie die Standardkachel für Ihre App an, und stellen Sie Ressourcen für unterschiedliche Bildschirmgrößen bereit. |
| [Ressourcen für App-Symbol](app-assets.md) | Ressourcen für App-Symbole, die in einer Vielzahl von Formen innerhalb des Windows 10-Betriebssystems vorkommen, sind die Aushängeschilder für Ihre App für die Universelle Windows-Plattform (UWP). In diesen Richtlinien wird beschrieben, wo Ressourcen für App-Symbole im System angezeigt werden, und Sie erhalten ausführliche Designtipps zum Erstellen ansprechender Symbole. |
| [Primäre Kachel-APIs](primary-tile-apis.md) | Fordern Sie den Benutzer auf, die primäre Kachel Ihrer App anzuheften, und überprüfen Sie, ob die primäre Kachel derzeit angeheftet ist. |
| [Kachelinhalt](create-adaptive-tiles.md) | Kachelbenachrichtigungsinhalt wird mit „Adaptiv“, einem neuen Feature in Windows10, angegeben, das den Entwurf eigener Inhalte für Kachelbenachrichtigungen mithilfe einer einfachen, flexiblen Markupsprache ermöglicht, die sich an unterschiedliche Bildschirmdichten anpasst. Dieser Artikel beschreibt, wie Sie adaptive Livekacheln für Ihre App für die Universelle Windows-Plattform (UWP) erstellen. |
| [Kachelinhaltsschema](../tiles-and-notifications/tile-schema.md) | Im Folgenden werden Elemente und Attribute aufgeführt, mit denen Sie adaptive Kacheln erstellen können. |
| [Spezielle Kachelvorlagen](special-tile-templates-catalog.md) | Spezielle Kachelvorlagen sind individuelle Vorlagen, die animiert sind oder mit denen Sie Vorgänge durchführen können, die mit adaptiven Kacheln nicht möglich sind. |
| [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md) | Hier erfahren Sie, wie Sie eine lokale Kachelbenachrichtigung senden und dabei erweiterten dynamischen Inhalt zu Ihrer Live-Kachel hinzufügen. |


## <a name="notifications"></a>Benachrichtigungen

| Artikel | Beschreibung |
| --- | --- |
| [Popupbenachrichtigungen](adaptive-interactive-toasts.md) | Mit adaptiven und interaktiven Popupbenachrichtigungen können Sie flexible Popupbenachrichtigungen mit mehr Inhalt, optionalen Inlinebildern und optionaler Benutzerinteraktion erstellen. |
| [Senden einer lokalen Popupbenachrichtigung](send-local-toast.md) | Hier erfahren Sie, wie Sie eine interaktive Popupbenachrichtigung senden. |
| [Notifications Visualizer](notifications-visualizer.md) | Notifications Visualizer ist eine neue App für die Universelle Windows-Plattform (UWP) im [Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1), die Entwickler dabei unterstützt, adaptive Live-Kacheln für Windows 10 zu entwerfen. |
| [Auswählen einer Methode für die Übermittlung von Benachrichtigungen](choosing-a-notification-delivery-method.md) | In diesem Artikel werden die vier Benachrichtigungsoptionen – lokal, geplant, periodisch und Push – behandelt, die Kachel- und Signalaktualisierungen sowie Popupbenachrichtigungsinhalte bereitstellen. |
| [Übersicht über regelmäßige Benachrichtigungen](periodic-notification-overview.md) | Regelmäßige Benachrichtigungen – auch als abgerufene Benachrichtigungen bezeichnet – aktualisieren Kacheln und Signale in festgelegten Intervallen, indem sie Inhalte aus einem Clouddienst herunterladen. |
| [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](windows-push-notification-services--wns--overview.md) | Mithilfe des Windows-Pushbenachrichtigungsdiensts (WNS) können Drittanbieterentwickler Popup-, Kachel-, Signalupdates und unformatierte Updates von ihren eigenen Clouddiensten aus senden. Dadurch steht ein Mechanismus zur Verfügung, mit dem Sie Ihren Benutzern auf energieeffiziente und verlässliche Weise neue Updates bereitstellen können. |
| [Vom Assistenten für Pushbenachrichtigungen generierter Code](the-code-generated-by-the-push-notification-wizard.md) | Mithilfe eines Assistenten in Visual Studio können Sie Pushbenachrichtigungen über einen mobilen Dienst generieren, der unter Verwendung von Azure Mobile Services erstellt wurde. Mit dem Visual Studio-Assistenten wird Code als Starthilfe generiert. In diesem Thema wird erläutert, wie der Assistent Ihr Projekt modifiziert, welche Schritte mit dem generierten Code ausgeführt werden, wie der Code verwendet wird und was Sie als Nächstes tun können, um Pushbenachrichtigungen optimal einzusetzen. Weitere Informationen finden Sie unter [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](windows-push-notification-services--wns--overview.md). |
| [Übersicht über unformatierte Benachrichtigungen](raw-notification-overview.md) | Unformatierte Benachrichtigungen sind kurze, allgemeine Pushbenachrichtigungen. Sie dienen ausschließlich zu Anweisungszwecken und enthalten keine UI-Komponente. Wie bei anderen Pushbenachrichtigungen übermittelt das WNS-Feature unformatierte Benachrichtigungen von Ihrem Clouddienst an Ihre App. |
