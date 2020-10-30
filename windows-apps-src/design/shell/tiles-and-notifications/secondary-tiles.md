---
description: Mithilfe sekundärer Kacheln können Benutzer bestimmten Inhalt und Tiefe Verknüpfungen aus ihrer app in das Startmenü anheften, um einen einfachen Zugriff auf den Inhalt in Ihrer APP zu ermöglichen.
title: Sekundäre Kacheln
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, sekundäre Kacheln
ms.localizationpriority: medium
ms.openlocfilehash: 066a6dcb3683e2e55f7452b1f09bb834157aee62
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034503"
---
# <a name="secondary-tiles"></a>Sekundäre Kacheln


Mithilfe sekundärer Kacheln können Benutzer bestimmten Inhalt und Tiefe Verknüpfungen aus ihrer app in das Startmenü anheften, um einen einfachen Zugriff auf den Inhalt in Ihrer APP zu ermöglichen.

![Screenshot der sekundären Kacheln](images/secondarytiles.png)

Beispielsweise können Benutzer das Wetter für zahlreiche bestimmte Orte in Ihrem Startmenü anheften. es bietet (1) einfache, Live aufzurufbare Informationen über das aktuelle Wetter aufgrund von Live-Kacheln und (2) einen schnellen Einstiegspunkt auf das Wetter der Stadt. Benutzer können auch bestimmte Bestände, Nachrichten Artikel und weitere wichtige Elemente anheften, die für Sie wichtig sind.

Durch das Hinzufügen sekundärer Kacheln zu Ihrer APP helfen Sie dem Benutzer, schnell und effizient mit Ihrer APP zu kommunizieren. Dadurch wird es Ihnen aufgrund des einfachen Zugriffs, den sekundäre Kacheln bietet, häufiger zu helfen.

**Nur Benutzer können eine sekundäre Kachel anheften. Apps können sekundäre Kacheln ohne Benutzergenehmigung nicht Programm** gesteuert anheften. Der Benutzer muss in der APP explizit auf eine "Pin"-Schaltfläche klicken. an diesem Punkt wird die API zum Erstellen einer sekundären Kachel verwendet, und das System zeigt ein Dialogfeld an, in dem der Benutzer gefragt wird, ob die Kachel angeheftet werden soll.

## <a name="quick-links"></a>Quicklinks

| Artikel | BESCHREIBUNG |
| --- | --- |
| [Leitfaden zu sekundären Kacheln](secondary-tiles-guidance.md) | Erfahren Sie, wann und wo Sie sekundäre Kacheln verwenden sollten. |
| [Heften von sekundären](secondary-tiles-pinning.md) | Erfahren Sie, wie Sie eine sekundäre Kachel anheften. |
| [Pin aus Desktop-Apps](secondary-tiles-desktop-pinning.md) | Dank der Desktop Bridge können Desktop-Apps sekundäre Kacheln anheften. |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>Sekundäre Kacheln in Bezug auf primäre Kacheln

Sekundäre Kacheln sind einer einzelnen übergeordneten App zugeordnet. Sie werden an das Startmenü angeheftet, damit ein Benutzer eine konsistente und effiziente Möglichkeit bietet, direkt in einem häufig verwendeten Bereich der übergeordneten APP zu starten. Dabei kann es sich entweder um einen allgemeinen unter Abschnitt der übergeordneten App handeln, der häufig aktualisierten Inhalt enthält, oder um einen Deep-Link zu einem bestimmten Bereich in der app.

Beispiele für sekundäre Kachel Szenarien:

* Wetter Updates für eine bestimmte Stadt in einer Wetter-App
* Eine Zusammenfassung der bevorstehenden Ereignisse in einer Kalender-APP
* Status und Updates von einem wichtigen Kontakt in einer Social App
* Bestimmte Feeds in einem RSS-Reader
* Eine Musikwiedergabe Liste
* Ein Blog

Jeder häufig geänderte Inhalt, den ein Benutzer überwachen möchte, ist ein guter Kandidat für eine sekundäre Kachel. Nachdem die sekundäre Kachel angeheftet wurde, können Benutzer auf einen Blick über die Kachel auf einen Blick klicken und Sie verwenden, um direkt in der übergeordneten APP zu starten.

Sekundäre Kacheln ähneln in vielerlei Hinsicht den primären Kacheln:

* Sie verwenden Kachel Benachrichtigungen, um umfangreiche Inhalte anzuzeigen.
* Sie müssen ein 150 x 150 Pixel Logo für den Standard Kachel Inhalt enthalten.
* Optional können Sie auch die anderen Logo Größen einschließen, um größere Kachel Größen zu aktivieren.
* Sie können Benachrichtigungen und Kennzeichen anzeigen.
* Sie können im Startmenü neu angeordnet werden.
* Sie werden automatisch gelöscht, wenn die APP deinstalliert wird.
* Der ausführliche Status Text für das Badge und die Sperre kann bei der Sperre angezeigt werden.

Sekundäre Kacheln unterscheiden sich jedoch auf bestimmte Weise von primären Kacheln:

* Benutzer können ihre sekundären Kacheln jederzeit löschen, ohne die übergeordnete APP zu löschen.
* Sekundäre Kacheln können zur Laufzeit erstellt werden. App-Kacheln können nur während der Installation erstellt werden.
* Ein Flyout fordert den Benutzer zur Bestätigung auf, bevor eine sekundäre Kachel hinzugefügt wird.
* Sie können nicht Programm gesteuert für den Sperrbildschirm durch eine Benutzer Anforderung ausgewählt werden. Der Benutzer muss die sekundäre Kachel manuell über die Seite personalisieren in den PC-Einstellungen hinzufügen.

Zum Senden von Benachrichtigungen werden bestimmte Methoden für Kachel-und Badge-Aktualisierer und für mit sekundären Kacheln verwendete pushbenachrichtigungskanäle bereitgestellt. Diese parallel zu den Versionen, die mit primären Kacheln verwendet werden. Zum Beispiel: "foratebadgeupdaterforapplication" und "kreatebadgeupdaterforsecondarytile".


## <a name="guidance-on-secondary-tiles"></a>Leitfaden zu sekundären Kacheln
Informationen dazu, wann und wo Sie sekundäre Kacheln verwenden sollten, sowie andere Verwendungs Anleitungen finden Sie unter [Leitfaden zu sekundären Kacheln](secondary-tiles-guidance.md) .


## <a name="pinning-secondary-tiles"></a>Fixieren sekundärer Kacheln
Weitere Informationen zum Anheften sekundärer Kacheln finden Sie unter anheften [sekundärer](secondary-tiles-pinning.md)


## <a name="desktop-applications-and-secondary-tiles"></a>Desktop Anwendungen und sekundäre Kacheln
Weitere Informationen zur Verwendung sekundärer Kacheln aus Ihrer Desktop Anwendung über die Desktop Bridge finden Sie unter anheften [von sekundären Kacheln aus Desktop-Apps](secondary-tiles-desktop-pinning.md).
