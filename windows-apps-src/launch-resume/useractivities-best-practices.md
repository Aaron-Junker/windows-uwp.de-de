---
title: Bewährte Methoden für Benutzeraktivitäten
description: Dieses Handbuch beschreibt die empfohlenen Methoden zum Erstellen und Aktualisieren von Benutzeraktivitäten.
keywords: benutzeraktivität, benutzeraktivitäten, zeitachse, cortana aufgaben weiterführen, cortana begonnene aufgaben bearbeiten, project rome
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e98f5d73cf2d1afb26a823ed417c8980d118485c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589905"
---
# <a name="user-activities-best-practices"></a>Bewährte Methoden für Benutzeraktivitäten

Dieses Handbuch beschreibt die empfohlenen Methoden zum Erstellen und Aktualisieren von Benutzeraktivitäten. Eine Übersicht über die Benutzeraktivitäten-Funktion auf Windows, finden Sie unter [Benutzeraktivität, sogar auf Geräten weiterhin](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). Alternativ finden Sie unter der [Benutzeraktivitäten Abschnitt](https://docs.microsoft.com/windows/project-rome/user-activities/) von Projekt "ROME" für die Implementierung der Aktivitäten auf anderen Entwicklungsplattformen zu nutzen.

## <a name="when-to-create-or-update-user-activities"></a>Wenn beim Erstellen oder Aktualisieren von Benutzeraktivitäten

Da jede app anders ist, liegt es an jeden Entwickler, um zu bestimmen, die beste Möglichkeit, Benutzeraktivitäten Aktionen innerhalb der app zuzuordnen. Benutzeraktivitäten wird in Cortana und den Zeitplan, der auf die zunehmende Benutzer-Produktivität und Effizienz konzentrieren möchten, indem Sie hilft, die zurück auf Inhalte zu erhalten, die sie in der Vergangenheit besucht präsentiert.

**Allgemeine Richtlinien**

* **Zeichnen Sie eine einzelne Aktivität für eine Gruppe von verwandten Benutzeraktionen.** Dies ist besonders relevant für Wiedergabelisten oder Fernsehsendungen: eine einzelne Aktivität kann in regelmäßigen Abständen entsprechend der Fortschritt des Benutzers aktualisiert werden. In diesem Fall müssen Sie eine einzelne Benutzeraktivität mit mehreren Verlaufselemente Zeiträume von Engagement über mehrere Tage oder Wochen darstellt. Das gleiche gilt für dokumentbasierte Aktivitäten, die auf denen der Benutzer den schrittweisen Fortschritt in der app vorgenommen.
* **Store Benutzerdaten in der Cloud.** Wenn Sie Geräteübergreifende Aktivitäten unterstützen möchten, müssen Sie sicherstellen, dass der Inhalt, der erforderlich, um diese Aktivität erneut Kontakt aufzunehmen, die an einen cloudspeicherort gespeichert ist. Gerätespezifische-Aktivitäten werden auf der Zeitachse auf dem Gerät angezeigt, in die Aktivität erstellt wurde, aber möglicherweise nicht auf anderen Geräten angezeigt.
* **Erstellen Sie keine Aktivitäten für Aktionen, die Benutzer nicht fortsetzen müssen.** Wenn Ihre Anwendung verwendet wird, einfache, einmalige Vorgänge durchführen, die nicht der Status beibehalten werden, müssen wahrscheinlich nicht Sie eine Benutzer-Aktivität erstellt.
* **Für Aktionen, die von anderen Benutzern abgeschlossen werden keine Aktivitäten erstellt werden.** Wenn ein externes Konto dem Benutzer eine Nachricht sendet oder @-mentions sie in der app sollte nicht erstellen Sie eine Aktivität für diesen. Diese Art von Aktion wird von Wartungscenterbenachrichtigungen besser bereitgestellt.
  * Szenarien für die Zusammenarbeit sind eine Ausnahme: Wenn mehrere Benutzer auf der gleichen Aktivität zusammen (z. B. ein Word-Dokument) arbeiten, fallen in Fällen, in denen ein anderer Benutzer nach dem der Benutzer Änderungen vorgenommen hat. In diesem Fall empfiehlt es sich um aktualisieren die vorhandene Aktivität, um Änderungen widerzuspiegeln, die auf das Dokument vorgenommen wurden. Dies würde betreffen, aktualisieren die vorhandenen Inhalt Benutzeraktivität-Daten ohne Erstellen eines neuen History-Elements.

**Richtlinien für bestimmte Typen von apps**

Während jede app anders ist, fallen die meisten apps in einem der folgenden Interaktionsmuster.
* **Dokument-basierte apps** – erstellen Sie eine Aktivität pro Dokument, mit ein oder mehrere Verlaufselemente Verwendung widerspiegelt. Es ist wichtig, um Ihre Aktivität zu aktualisieren, wie das Dokument geändert wird.
* **Spiele** – erstellen Sie eine Aktivität für jedes Spiel speichern "oder" www. Wenn Ihr Spiel nur eine einzigen Sequenz von Ebenen unterstützt, können Sie die gleiche Aktivität im Laufe der Zeit erneut veröffentlichen, obwohl möglicherweise Sie die Inhalte Daten zum Anzeigen der aktuellen Status oder Erfolge zu aktualisieren möchten.
* **Hilfsprogramm apps** – liegt vor "nothing" in Ihrer app, die Benutzer benötigen würde, schließen und wieder aufnehmen, müssen Sie keine Benutzeraktivitäten zu verwenden. Ein gutes Beispiel ist eine einfache app wie Rechner.
* **Line-of-Business-apps** – viele apps, die für die Verwaltung von einfachen Aufgaben oder Workflows vorhanden sind. Erstellen Sie eine Aktivität für jede separate Workflow erfolgt über Ihre app (z. B. Expense Berichte wäre jeweils eine separate-Aktivität, damit der Benutzer klicken kann eine Aktivität, um festzustellen, ob ein bestimmter Bericht genehmigt wurde).
* **Wiedergabe Medienapps** – Erstellen einer Aktivität pro logische Gruppierung von Inhalten (z.B. eine Wiedergabeliste, Programme oder eigenständigen Inhalt). Die zugrunde liegende Frage für app-Entwickler ist, gibt an, ob ein jeder Teil des Inhalts (TV-folgen, "Song") als eigenständige Inhalte oder Teil einer Auflistung betrachtet. Wenn der Benutzer "OPTS", um eine Sammlung oder fortlaufende Inhalte wiederzugeben, ist die Auflistung als Ganzes als allgemeine Regel die Aktivität. Wenn sie sich entscheiden, um einen einzelnen Inhaltselement wiederzugeben, ist diese ein Teil des Inhalts der Aktivität. Finden Sie spezifischere Richtlinien unten.
  * **Musik: Album/Interpreten / "Genre"** , klickt der Benutzer ein Album, Künstler, oder "Genre" und Treffer **spielen**, diese Sammlung ist die Aktivität, eine separate Aktivität für jede "Song" nicht erstellen. Für kurze wie ein einzelnes Album oder Sammlungen in zufälliger Reihenfolge wiedergegeben wird müssen Sie sich nicht um die Aktivität, um die aktuelle Position des Benutzers entsprechend zu aktualisieren. Für die lange sequenzielle Wiedergabe wie z. B. ein Album oder Wiedergabelisten vorliegen kann das Aufzeichnen von Ihrer Position innerhalb des Albums sinnvoll sein.
  * **Musik: intelligente Wiedergabelisten** – Anwendungen, die Wiedergabe von Musik in zufälliger Reihenfolge sollten eine einzelne Aktivität für dieser Wiedergabeliste aufzeichnen. Wenn der Benutzer ein zweites Mal die Wiedergabeliste wiedergibt, erstellen Sie zusätzliche Datensätze für die gleiche Aktivität. Aufzeichnen der aktuellen Position des Benutzers in der Wiedergabeliste ist nicht erforderlich, da die Reihenfolge zufällig ist.
  * **TV-Serie** – Wenn Ihre app konfiguriert ist, um der nächsten Folge wiederzugeben, nach der aktuellen Aktivität abgeschlossen ist, Schreiben Sie eine einzelne Aktivität für die TV-Serie. Wie Sie über mehrere Sitzungen der Anzeige der verschiedenen Episoden spielen, aktualisieren Sie Ihre Aktivität entsprechend die aktuelle Position in der Reihe, und mehrere Datensätze erstellt werden.
  * **Film** – ein Film einen einzelnen Inhaltselement ist, und einen eigenen Datensatz verfügen. Wenn der Benutzer wird beendet, überwacht der Film teilweise wiedergegeben, ist es ratsam, ihre Position aufzeichnen. Wenn sie sie in der Zukunft fortsetzen möchten, konnte die Aktivität Fortsetzen des Films, wo sie aufgehört, oder bitten dem Benutzer sogar bei Bedarf wieder aufgenommen oder beginnen Sie am Anfang.

## <a name="user-activity-design"></a>Gestaltung einer Aktivität

Benutzeraktivitäten besteht aus drei Komponenten: einer Aktivierung URI, visuelle Daten und Metadaten Inhalts.
* Die Aktivierung-URI ist ein URI, der für eine Anwendung oder eine Oberfläche zum Fortsetzen der Anwendung mit einem bestimmten Kontext übergeben werden kann. In der Regel haben es sich bei diesen Links, um die Form der Protokollhandler für ein Schema (z. B. "my-app://page2?action=edit"). Es ist Aufgabe des Entwicklers, um zu bestimmen, wie URI-Parameter von ihrer app verarbeitet werden. Finden Sie unter [behandeln URI Aktivierung](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) für Weitere Informationen.
* Der visuellen Daten, die aus einem Satz von erforderlichen und optionalen Eigenschaften (z. B.: Titel, Beschreibung oder Elemente an Adaptive Card), können Benutzer eine Aktivität visuell zu identifizieren. Nachfolgend finden Sie Richtlinien zum Erstellen von Adaptive Card Visualisierungen für Ihre Aktivität.
* Die Metadaten des Inhalte ist JSON-Daten, die zum Gruppieren und Abrufen von Aktivitäten, die unter einem bestimmten Kontext verwendet werden können. Dies erfolgt in der Regel in Form von http://schema.org Daten. Nachfolgend finden Sie Richtlinien zum Ausfüllen dieser Daten.

### <a name="adaptive-card-design-guidelines"></a>Richtlinien zum Entwerfen von Adaptive Card

Wenn Aktivitäten in der Zeitachse angezeigt werden, werden sie angezeigt, mit der [Adaptive Card Framework](https://docs.microsoft.com/adaptive-cards/). Wenn der Entwickler eine Adaptive Card nicht für jede Aktivität bereitstellt, wird eine einfache Karte, die basierend auf dem app-Namen/Symbol, das erforderliche Feld "Titel" und das optionale Feld "Beschreibung" von Zeitachse automatisch erstellt. 

App-Entwicklern werden empfohlen, benutzerdefinierte Karten, die unter Verwendung des einfachen Adaptive Card-JSON-Schemas bieten. Finden Sie unter den [mit Adaptive Cards Dokumentation](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) technische Anleitungen zur Adaptive Card-Objekte erstellt. Finden Sie in der folgenden Leitlinien für das Entwerfen mit Adaptive Cards in Benutzeraktivitäten.
* Verwenden von images
  * Verwenden Sie möglichst ein eindeutiges Image für jede Aktivität. Ihr Name der Anwendung und das Symbol werden neben die Aktivität der Karte automatisch angezeigt werden; zusätzlichen Bilder erleichtern es Benutzern, die die Aktivität zu suchen, sie suchen.
  * Images darf keinen Text enthalten, die vom Benutzer zum Lesen erwartet wird. Dieser Text nicht für Benutzer mit Anforderungen an die Barrierefreiheit verfügbar und kann nicht durchsucht werden.
  * Wenn das Bild nicht Text enthalten und zu einem Verhältnis 2:1-zugeschnitten werden kann, sollten Sie es als Hintergrundbild verwenden. Dies führt zu einem fett Aktivität Card in Zeitachse abhebt. Das Bild wird etwas dunkler werden, um sicherzustellen, dass der Text bleibt auf der Karte angezeigt werden soll, und es wird empfohlen, den Namen der Aktivität nur in diesem Fall verwenden, da kleinere Text schwer lesbar werden kann.
  * Wenn das Bild kann nicht 2:1 zugeschnitten werden, sollten Sie es auf der Karte für die Aktivität abgelegt.  
    * Ist das Seitenverhältnis Quadrat oder Hochformat, das Bild auf der rechten Seite der Karte ohne Ränder verankert werden.
    * Ist das Seitenverhältnis Querformat festgelegt, das Bild an der oberen rechten Ecke der Karte verankert werden.
* Jede Aktivität ist erforderlich, um einen Aktivitätsnamen ein, geben Sie die immer angezeigt werden soll.
  * Dieser Name sollte in der oberen linken Ecke der Karte mit der Option für große fett formatierter Text angezeigt werden. Es ist wichtig, dass der Name leicht erkennbar, da dies der einzige Teil-Benutzer sehen, wenn die Aktivität im Cortana-Szenarien angezeigt wird. Mit dem gleichen Namen in Zeitachse erleichtert es Benutzern, eine große Anzahl von Aktivitäten zu durchsuchen.
* Verwenden Sie die gleichen visuellen Stil für alle Aktivitäten in Ihrer app, sodass Benutzer die Ihrer app-Aktivitäten in der Zeitachse leicht finden können.
  * Beispielsweise sollten Aktivitäten alle die gleiche Hintergrundfarbe verwenden.
* Verwenden Sie ergänzender Textinformationen nur selten. 
  * Zu verhindern, dass die Karte mit Text, und nur zusätzliche Informationen, die unterstützt die Benutzer bei der Suche nach der richtigen Aktivität oder Statusinformationen (z. B. den aktuellen Status in einer bestimmten Aufgabe) widerspiegelt.

### <a name="content-metadata-guidelines"></a>Inhaltsmetadaten-Richtlinien

Benutzeraktivitäten können auch Inhalt Metadaten enthalten, die Windows und Cortana verwenden, um Aktivitäten zu kategorisieren und Rückschlüsse zu generieren. Aktivitäten können Sie dann zu einem bestimmten Thema, wie Speicherort (wenn der Benutzer Urlaubstage Recherche ist), -Objekt (falls der Benutzer etwas Recherche ist) oder Aktion gruppiert werden, (wenn der Benutzer über verschiedene apps und Websites für ein bestimmtes Produkt Warenkorb ist). Es ist eine gute Idee, die sowohl den Substantiven als auch die Verben in einer Aktivität beteiligt darstellen. 

Im folgenden Beispiel die Metadaten des Inhalts JSON, befolgen die jeweiligen Standards der [Schema.org](https://schema.org/), stellt das Szenario: "John abgespielt Unternehmens mit Steve,".

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>Wichtige APIs

* [UserActivities-namespace](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Verwandte Themen

* [Benutzeraktivitäten (Projekt "ROME" Docs)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Mit Adaptive cards](https://docs.microsoft.com/adaptive-cards/)
* [Mit Adaptive Cards Visualizer, Beispiele](https://adaptivecards.io/)
* [Behandeln der URI-Aktivierung](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interagieren mit Ihren Kunden auf jeder Plattform mit der Microsoft Graph, Aktivitätsfeed und mit Adaptive Cards](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
