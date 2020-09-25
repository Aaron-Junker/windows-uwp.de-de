---
Description: Erfahren Sie, wann und wo Sie sekundäre Kacheln in Ihrer Windows-App verwenden sollten.
title: Entwurfs Leit Faden für sekundäre Kacheln
label: Secondary tiles
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, sekundäre Kacheln, Anleitungen, Richtlinien, bewährte Methoden
ms.localizationpriority: medium
ms.openlocfilehash: 5414c9d8437ee77e2a4a584dea26f7bf1fadef4a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220233"
---
# <a name="secondary-tile-guidance"></a>Leitfaden für sekundäre Kacheln


Eine sekundäre Kachel bietet Benutzern eine konsistente und effiziente Möglichkeit, direkt über das Startmenü auf bestimmte Bereiche innerhalb einer APP zuzugreifen. Obwohl ein Benutzer entscheidet, ob eine sekundäre Kachel an das Startmenü angeheftet werden soll, werden die in einer APP ausstell baren Bereiche vom Entwickler bestimmt. Eine ausführlichere Zusammenfassung finden Sie unter [Übersicht über sekundäre Kacheln](secondary-tiles.md). Beachten Sie diese Richtlinien, wenn Sie sekundäre Kacheln aktivieren und die zugehörige Benutzeroberfläche in Ihrer APP entwerfen.

> [!NOTE]
> Nur Benutzer können eine sekundäre Kachel an das Startmenü anheften. Apps können sekundäre Kacheln nicht Programm gesteuert anheften. Benutzer steuern auch das Entfernen von Kacheln und können eine sekundäre Kachel aus dem Startmenü oder innerhalb der übergeordneten App entfernen.


## <a name="recommendations"></a>Empfehlungen

Beachten Sie die folgenden Empfehlungen, wenn Sie sekundäre Kacheln in Ihrer APP aktivieren:

* Wenn der Inhalt im Fokus gepingt werden kann, sollte die APP-Leiste eine Schaltfläche "an Start anheften" enthalten, um eine sekundäre Kachel für den Benutzer zu erstellen.
* Wenn der Benutzer auf "an Start anheften" klickt, sollten Sie sofort die API aus dem UI-Thread aufrufen, um [die sekundäre Kachel](secondary-tiles-pinning.md)anzuheften.
* Wenn der Inhalt im Fokus bereits angeheftet ist, ersetzen Sie die Schaltfläche "an Start anheften" in der APP-Leiste mit der Schaltfläche "an Start lösen". Mit der Schaltfläche "vom Start lösen" sollte die vorhandene sekundäre Kachel entfernt werden.
* Wenn der Inhalt im Fokus nicht gepingt werden kann, zeigen Sie die Schaltfläche "an Start anheften" nicht an (oder zeigen Sie die Schaltfläche "an Start anheften" an).
* Verwenden Sie die vom System bereitgestellten Symbole, um die Schaltflächen "anheften" und "aus Start" zu lösen (Weitere Informationen finden Sie unter PIN und lösen-Member in Windows. UI. XAML. Controls. Symbol oder winjs. UI. appbaricon).
* Verwenden Sie den Standard Schaltfläche Text: "an Start anheften" und "vom Start lösen". Sie müssen den Standardtext überschreiben, wenn Sie die vom System bereitgestellte PIN verwenden und Symbole lösen.
* Verwenden Sie keine sekundäre Kachel als virtuelle Befehls Schaltfläche, um mit der übergeordneten APP zu interagieren, z. b. die Kachel "mit nächster Spur überspringen".


## <a name="additional-usage-guidance-for-devs"></a>Zusätzliche Verwendungs Anleitungen für Entwickler

* Wenn eine APP gestartet wird, sollten Sie Ihre sekundären Kacheln immer auflisten, falls es Ergänzungen oder Löschungen gab, von denen Sie nicht wusste. Wenn eine sekundäre Kachel über die APP-Leiste des Start Bildschirms gelöscht wird, wird die Kachel durch Windows einfach entfernt. Die APP selbst ist dafür verantwortlich, alle Ressourcen freizugeben, die von der sekundären Kachel verwendet wurden. Wenn sekundäre Kacheln über die Cloud kopiert werden, werden aktuelle Kachel-oder Badge-Benachrichtigungen auf der sekundären Kachel, geplante Benachrichtigungen, pushbenachrichtigungschannels und URIs (Uniform Resource Identifier), die mit periodischen Benachrichtigungen verwendet werden, nicht mit der sekundären Kachel kopiert und müssen erneut eingerichtet werden.
* Eine APP sollte für sekundäre Kacheln sinnvolle, neu zu verwendende, eindeutige IDs verwenden. Die Verwendung von vorhersagbaren sekundären Kachel-IDs, die für eine APP von Bedeutung sind, hilft der APP zu verstehen, was mit diesen Kacheln zu tun ist, wenn Sie in einer neuen Installation auf einem neuen Computer
  * Zur Laufzeit kann die APP Abfragen, ob eine bestimmte Kachel vorhanden ist.
  * Die sekundäre Kachel Plattform kann aufgefordert werden, den Satz aller sekundären Kacheln zurückzugeben, die zu einer bestimmten app gehören. Die Verwendung aussagekräftiger, eindeutiger IDs für diese Kacheln kann der APP helfen, den Satz sekundärer Kacheln zu untersuchen und entsprechende Aktionen auszuführen. Beispielsweise können IDs für eine Social Media-App einzelne Kontakte identifizieren, für die Kacheln erstellt wurden.
* Sekundäre Kacheln, wie z. b. alle Kacheln auf dem Start Bildschirm, sind dynamische Outlets, die häufig mit neuem Inhalt aktualisiert werden können. Mithilfe der gleichen Mechanismen wie jede andere Kachel können sekundäre Kacheln Benachrichtigungen und Updates anzeigen. Weitere Informationen finden [Sie unter Auswählen einer Benachrichtigungs Bereitstellungs Methode](choosing-a-notification-delivery-method.md) .


## <a name="related"></a>Verwandte Themen

* [Übersicht über sekundäre Kacheln](secondary-tiles.md)
* [Heften von sekundären](secondary-tiles-pinning.md)
* [Kachel Ressourcen](../../style/app-icons-and-logos.md)
* [Dokumentation zu Kachel Inhalt](create-adaptive-tiles.md)
* [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md)
