---
Description: Hier erfahren Sie, wann und wo sekundäre Kacheln in Ihrer UWP-App verwendet werden sollen.
title: Sekundäre Kacheln
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, sekundäre Kacheln, Richtlinien, Richtlinien, bewährte Methoden
ms.localizationpriority: medium
ms.openlocfilehash: de3bfa94de1152b3945d42169143a5ae36328c75
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625345"
---
# <a name="secondary-tile-guidance"></a>Anleitung für sekundäre Kacheln


Eine sekundäre Kachel stellt für Benutzer eine einheitliche und effiziente Möglichkeit dar, um vom Startmenü aus direkt auf bestimmte Bereiche innerhalb der App zuzugreifen. Auch wenn Benutzer wählen können, ob eine sekundäre Kachel auf das Startmenü angeheftet werden soll, werden die Anheftbereiche einer App doch vom Entwickler vorgegeben Eine ausführliche Zusammenfassung finden Sie unter [Übersicht über sekundäre Kacheln](secondary-tiles.md). Beachten Sie diese Richtlinien beim Aktivieren von sekundären Kacheln und Entwerfen der entsprechenden UI in der App.

> [!NOTE]
> Sekundäre Kacheln können nur vom Benutzer auf dem Startmenü angeheftet werden. Von Apps können sekundäre Kacheln nicht programmgesteuert angeheftet werden. Darüber hinaus steuern Benutzer die Entfernung der Kachel und können eine sekundäre Kachel vom Startmenü oder aus der übergeordneten App entfernen.


## <a name="recommendations"></a>Empfehlungen

Beachten Sie die folgenden Empfehlungen zur Aktivierung von sekundären Kacheln in Apps:

* Wenn der Inhalt mit dem Fokus angeheftet werden kann, sollte die App-Leiste eine Schaltfläche „An "Start" anheften” zum Erstellen einer sekundären Kachel für den Benutzer enthalten.
* Klickt der Benutzer- auf „An "Start" anheften”, sollten Sie sofort die API vom UI-Thread aufrufen, um [die sekundäre Kachel anzuheften](secondary-tiles-pinning.md).
* Falls der Inhalt mit dem Fokus bereits angeheftet ist, sollte auf der App-Leiste anstelle der Schaltfläche „An "Start" anheften” die Schaltfläche „Von "Start" lösen” angezeigt werden. Die Schaltfläche „Von "Start" lösen” sollte die vorhandene sekundäre Kachel entfernen.
* Wenn der Inhalt mit dem Fokus nicht angeheftet werden kann, sollte auch die Schaltfläche „An "Start" anheften” nicht zu sehen sein (oder die Schaltfläche „An "Start" anheften” sollte deaktiviert sein).
* Verwenden Sie die vom System bereitgestellten Symbole für die Schaltflächen „An "Start" anheften” und „Von "Start" lösen” (siehe Member zum Anheften und Lösen unter Windows.UI.Xaml.Controls.Symbol bzw. WinJS.UI.AppBarIcon).
* Verwenden Sie den Text der Schaltfläche "standard": "An Start anheften" und "Lösen von Anfang". Beim Nutzen der vom System bereitgestellten Glyphen für das Anheften und Lösen müssen Sie den Standardtext überschreiben.
* Verwenden Sie eine sekundäre Kachel nicht als virtuelle Befehlsschaltfläche, um mit der übergeordneten App zu interagieren, z. B. eine Kachel "Zum nächsten Titel springen".


## <a name="additional-usage-guidance-for-devs"></a>Weitere Richtlinien für Entwickler

* Wenn eine App gestartet wird, sollte diese immer ihre sekundären Kacheln aufzählen, um zu überprüfen, ob Kacheln hinzugefügt oder entfernt wurden, ohne dass die App dies erfasst hat. Wenn eine sekundäre Kachel über die App-Leiste auf der Startseite gelöscht wird, entfernt Windows die Kachel einfach. Die App ist jedoch selbst dafür verantwortlich, alle von der sekundären Kachel verwendeten Ressourcen freizugeben. Wenn sekundäre Kacheln über die Cloud kopiert werden, werden aktuelle Kachel- oder Signalbenachrichtigungen der sekundären Kachel, geplante Benachrichtigungen, Pushbenachrichtigungskanäle und bei regelmäßigen Benachrichtigungen verwendete URIs (Uniform Resource Identifier) nicht mit der sekundären Kachel kopiert und müssen neu eingerichtet werden.
* Eine App sollte sinnvolle, reproduzierbare und eindeutige IDs für sekundäre Kacheln verwenden. Der Gebrauch von vorhersagbaren, für eine App sinnvollen IDs für sekundäre Kacheln erleichtert der App deren Verwendung, wenn diese in einer Neuinstallation auf einem neuen Computer erscheinen.
  * Zur Laufzeit kann die App abfragen, ob eine bestimmte Kachel vorhanden ist.
  * Die Plattform für sekundäre Kacheln kann aufgefordert werden, die Gruppe aller sekundären Kacheln zurückzugeben, die zu einer bestimmten App gehören. Wenn für diese Kacheln sinnvolle, eindeutige IDs verwendet werden, kann die App die Gruppe von sekundären Kacheln leichter überprüfen und entsprechende Aktionen ausführen. Bei einer Social-Media-App können mithilfe von IDs beispielsweise einzelne Kontakte angegeben werden, für die Kacheln erstellt wurden.
* Sekundäre Kacheln sind wie alle Kacheln der Startseite dynamische Flächen, für die regelmäßig Updates mit neuen Inhalten durchgeführt werden können. Sekundäre Kacheln können Benachrichtigungen und Updates mithilfe derselben Mechanismen wie alle anderen Kacheln anzeigen. Lesen Sie dazu [Auswählen einer Methode für die Übermittlung von Benachrichtigungen](choosing-a-notification-delivery-method.md).


## <a name="related"></a>Verwandte Themen

* [Übersicht über die sekundäre Kacheln](secondary-tiles.md)
* [Sekundäre Kacheln anheften](secondary-tiles-pinning.md)
* [Kachel "-Objekte](app-assets.md)
* [Kachel "Content-Dokumentation](create-adaptive-tiles.md)
* [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md)
