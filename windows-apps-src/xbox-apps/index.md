---
title: UWP auf Xbox One
description: Informationen zum Erstellen von Apps für die Universelle Windows-Plattform (UWP) auf Xbox One.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: f8f31630fed64e2a22b21e4572eaba76440a82fc
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334768"
---
# <a name="uwp-on-xbox-one"></a>UWP auf Xbox One

Erste Schritte beim Erstellen von Apps für die Universelle Windows-Plattform (UWP) auf Xbox One.

UWP auf Xbox One unterstützt die Entwicklung von Apps und Spielen. Sie müssen nicht an einem Entwicklerprogramm teilnehmen, um auf Xbox zu experimentieren und Spiele oder Apps zu erstellen und zu testen. Sie benötigen lediglich ein [Entwicklerkonto](https://developer.microsoft.com/en-us/store/register) in [Partner Center](https://partner.microsoft.com/dashboard). Wenn Sie zum Veröffentlichen und Verkaufen von Spielen auf Xbox One bereit sind oder Xbox Live unter Windows 10 nutzen, müssen Sie dem [Xbox Live Creators-Programm](https://developer.microsoft.com/games/xbox/xboxlive/creator) beitreten oder ein [ID@Xbox](https://www.xbox.com/Developers/id)-Entwickler sein. Wenn Sie planen, ID@Xbox-Entwickler zu werden, sollten Sie zunächst am Programm teilnehmen, bevor Sie sich für ein Entwicklerkonto registrieren. Weitere Informationen finden Sie unter [Übersicht über das Entwicklerprogramm](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview).

Dieser Abschnitt enthält die Schritte bei der Einrichtung, Informationen zum Authentifizierungsprozess, zur Installation der erforderlichen Version von Visual Studio und Windows 10-Tools sowie zum Einrichten, Ausführen und Debuggen einer einfachen ersten Anwendung. 

| Thema      | Beschreibung |
|------------|-------------|
|[Erste Schritte](getting-started.md)| Leitfaden für erste Schritte bei der UWP-Entwicklung auf Xbox One. |
|[Neuigkeiten](whats-new.md)| Stellt neue Features in UWP auf Xbox One vor. |
|[Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md)| Beschreibt, wie Sie den Entwicklermodus auf Xbox One aktivieren. |
|[Deaktivieren des Entwicklermodus auf Xbox One](devkit-deactivation.md)| Erläutert, wie Sie den Entwicklermodus auf Xbox One deaktivieren. |
|[Einrichten der UWP-Entwicklungsumgebung auf Xbox](development-environment-setup.md)| Beschreibt die Schritte zum Einrichten und Testen der Xbox One-Entwicklungsumgebung. |
|[Beispiele](samples.md)| Verweist auf Github (TVHelpers), wo Sie nützliche XAML- und JavaScript-Beispiele finden, die Ihnen bei den ersten Schritten bei der Entwicklung für Xbox helfen. Die Beispiele umfassen eine vollständige XAML-Medien-App-Vorlage, die automatische Controllernavigation, die Rich-Media-Wiedergabe und die Suche nach webbasierten Technologien. |
|[Bekannte Probleme](known-issues.md)| Listet bekannte Probleme mit UWP auf Xbox One auf. |
|[Häufig gestellte Fragen](frequently-asked-questions.md)| Häufig gestellte Fragen zu UWP auf Xbox One. |
|[Tools](introduction-to-xbox-tools.md)| Beschreibt das Xbox One-spezifische Tool _Dev Home_, die Verwendung des Windows Device Portal und die Einrichtung von Visual Studio für die Entwicklung. In diesem Abschnitt werden neue Entwickler auch durch ihre erste UWP-Anwendung für Xbox geführt und die Verwendung des Fiddler-Tools zur Anzeige des Netzwerkdatenverkehrs erklärt. |
| [Veranstaltung „App-Entwicklung auf Xbox“](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | Die Veranstaltung „App-Entwicklung auf Xbox“ ist ein hervorragender Ausgangspunkt für Entwickler, die noch keine Erfahrung mit dem Erstellen von Apps auf Xbox haben Schauen Sie sich die aufgezeichneten Sitzungen an, und lesen Sie die Blogbeiträge der Veranstaltung. |
|[Entwerfen für Xbox und Fernsehgeräte](../design/devices/designing-for-tv.md)| Beschreibt bewährte Methoden für das Entwerfen einer App, die auf einem Fernsehgerät angezeigt wird und einen Controller für die Eingabe verwendet. |
|[Bewährte Methoden für Xbox](tailoring-for-xbox.md)| Beschreibt, wie Sie den Mausmodus deaktivieren, bis zu den Rändern des Bildschirms zeichnen und die Skalierung deaktivieren. |
|[Einsatz von Sprache zum Aufrufen von UI-Elementen](ves-on-xbox.md)| Hier werden die bewährten Methoden zur Unterstützung der sprachgestützten Shell in UWP-Apps auf Xbox beschrieben. |
|[Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md)| Beschreibt die verfügbaren Ressourcen für die Anwendung, wenn sie auf Xbox One ausgeführt wird. |
|[Einführung in Anwendungen mit mehreren Benutzern](multi-user-applications.md)| Beschreibt Anwendungen mit mehreren Benutzern (Multi-User Applications, MUAs) auf Xbox One. |
| [Automatisieren von Xbox One-Entwicklungsaufgaben](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | Das Projekt „WindowsDevicePortalWrapper“ auf GitHub bietet eine Bibliothek, mit der Sie gängige Entwicklungsaufgaben wie das Bereitstellen oder Starten einer App automatisieren können. Das Projekt enthält ein Beispiel (XboxWdpDriver.exe), das die Verwendung der APIs für allgemeine Aufgaben veranschaulicht. |
|[Portieren vorhandener Spiele zu Xbox](development-lanes-landing.md)|Basierend auf der Technologie, auf der Ihr Spiel entwickelt wurde, können wir Ihnen Schritt-für-Schritt-Anleitungen bereitstellen, die Ihnen helfen, Ihr Spiel mithilfe der UWP schneller für die Xbox bereitzustellen.|
|[UWP-Funktionen, die noch nicht auf Xbox One unterstützt werden](https://go.microsoft.com/fwlink/p/?LinkId=760755)|  Beschreibt die UWP-Funktionen, die auf Xbox One noch nicht voll funktionsfähig sind.|

## <a name="videos"></a>Videos

Die folgenden Gespräche auf Channel 9 sind eine hervorragende Informationsquelle für die Entwicklung beeindruckender Apps auf Xbox:

* [Erstellen von großartigen UWP-Apps (Universelle Windows-Plattform) für Xbox](https://channel9.msdn.com/Events/Build/2016/B883)
* [Passen Sie Ihre App für die Xbox One und den Fernseher an](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [UWP-Entwicklung 1: Erstellen einer adaptiven Benutzeroberfläche](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [Web-Apps über den Browser hinaus: plattformübergreifende und geräteübergreifende Entwicklung](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>Siehe auch

- [Automatisieren des Starts von UWP-Apps unter Windows 10](automate-launching-uwp-apps.md)
- [CPUSets für die Entwicklung von Spielen](cpusets-games.md)
- [Progressive Web-Apps für Xbox One](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/xbox-considerations)