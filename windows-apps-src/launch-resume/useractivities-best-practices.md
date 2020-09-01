---
title: Bewährte Methoden für Benutzeraktivitäten
description: Dieser Leitfaden beschreibt die empfohlenen Vorgehensweisen zum Erstellen und Aktualisieren von Benutzeraktivitäten.
keywords: Benutzeraktivität, Benutzeraktivitäten, Zeitachse, Cortana nehmen Sie an der Stelle an, an der Sie aufgehört haben, Cortana an der Stelle, an der ich aufgehört habe, Project
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f4e41ac4f340491e551fccc1c1d4700bbe8d8004
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172954"
---
# <a name="user-activities-best-practices"></a>Bewährte Methoden für Benutzeraktivitäten

Dieser Leitfaden beschreibt die empfohlenen Vorgehensweisen zum Erstellen und Aktualisieren von Benutzeraktivitäten. Eine Übersicht über das Feature "Benutzeraktivitäten" unter Windows finden Sie unter [Fortsetzen der Benutzeraktivität (auch Geräte übergreifend](./useractivities.md)). Weitere Informationen finden Sie im [Abschnitt Benutzeraktivitäten](/windows/project-rome/user-activities/) von Project Rom für die Implementierungen von Aktivitäten auf anderen Entwicklungsplattformen.

## <a name="when-to-create-or-update-user-activities"></a>Wann sollten Benutzeraktivitäten erstellt oder aktualisiert werden?

Da jede APP anders ist, muss jeder Entwickler die beste Methode zum Zuordnen von Aktionen innerhalb der APP zu Benutzeraktivitäten ermitteln. Ihre Benutzeraktivitäten werden in Cortana und Timeline vorgestellt, die sich auf die Produktivität und Effizienz der Benutzer konzentrieren, indem Sie Ihnen helfen, zu den in der Vergangenheit besuchten Inhalten zurückzukehren.

**Allgemeine Richtlinien**

* **Aufzeichnen einer einzelnen Aktivität für eine Gruppe verwandter Benutzeraktionen.** Dies ist besonders relevant für Musikwiedergabe Listen oder Fernsehsendungen: eine einzelne Aktivität kann in regelmäßigen Abständen aktualisiert werden, um den Fortschritt des Benutzers widerzuspiegeln. In diesem Fall verfügen Sie über eine einzelne Benutzeraktivität mit mehreren Verlaufs Elementen, die Zeiträume für die Einbindung mehrerer Tage oder Wochen darstellen. Das gleiche gilt für Dokument basierte Aktivitäten, bei denen der Benutzer einen schrittweisen Fortschritt innerhalb Ihrer APP vornimmt.
* **Speichern Sie Benutzerdaten in der Cloud.** Wenn Sie Geräte übergreifende Aktivitäten unterstützen möchten, müssen Sie sicherstellen, dass die Inhalte, die zum erneuten Einbinden dieser Aktivität erforderlich sind, an einem cloudspeicherort gespeichert werden. Gerätespezifische Aktivitäten werden auf dem Gerät, auf dem die Aktivität erstellt wurde, in der Zeitachse angezeigt, werden aber möglicherweise nicht auf anderen Geräten angezeigt.
* **Erstellen Sie keine Aktivitäten für Aktionen, die von Benutzern nicht fortgesetzt werden müssen.** Wenn Ihre Anwendung verwendet wird, um einfache, einmalige Vorgänge abzuschließen, die den Status nicht beibehalten, müssen Sie wahrscheinlich keine Benutzeraktivität erstellen.
* **Erstellen Sie keine Aktivitäten für Aktionen, die von anderen Benutzern abgeschlossen wurden.** Wenn ein externes Konto dem Benutzer eine Nachricht oder @-mentions diese innerhalb Ihrer APP sendet, sollten Sie keine Aktivität für dieses erstellen. Diese Art von Aktion wird von Benachrichtigungen im Aktions Center besser bedient.
  * Zusammenarbeits Szenarien stellen eine Ausnahme dar: Wenn mehrere Benutzer gleichzeitig an derselben Aktivität Arbeiten (z. b. in einem Word-Dokument), gibt es Fälle, in denen ein anderer Benutzer Änderungen vorgenommen hat. In diesem Fall möchten Sie möglicherweise die vorhandene Aktivität aktualisieren, um Änderungen widerzuspiegeln, die am Dokument vorgenommen wurden. Dies umfasst das Aktualisieren der vorhandenen Inhaltsdaten der Benutzeraktivität ohne das Erstellen eines neuen Verlaufs Elements.

**Richtlinien für bestimmte App-Typen**

Obwohl jede APP anders ist, werden die meisten apps auf eines der folgenden Interaktionsmuster aufgeteilt.
* **Dokument basierte apps** – erstellen Sie eine Aktivität pro Dokument mit einem oder mehreren Verlaufs Elementen, in denen Zeiträume verwendet werden. Es ist wichtig, die Aktivität zu aktualisieren, wenn Änderungen am Dokument vorgenommen werden.
* **Games** – erstellen Sie eine Aktivität für jede Spiele Speicherung oder Welt. Wenn Ihr Spiel nur eine einzelne Sequenz von Ebenen unterstützt, können Sie die gleiche Aktivität im Laufe der Zeit erneut veröffentlichen, obwohl Sie die Inhaltsdaten möglicherweise aktualisieren möchten, um den aktuellen Fortschritt oder die aktuellen Ergebnisse anzuzeigen.
* **Hilfsprogrammanwendungen** – wenn in Ihrer APP nichts vorhanden ist, das Benutzer verlassen und fortsetzen müssen, müssen Sie keine Benutzeraktivitäten verwenden. Ein gutes Beispiel hierfür ist eine einfache APP wie Rechner.
* **Branchenspezifische apps** – viele apps sind zum Verwalten einfacher Aufgaben oder Workflows vorhanden. Erstellen Sie eine Aktivität für jeden separaten Workflow, auf den über Ihre App zugegriffen wird (z. b. wären Ausgaben Berichte eine separate Aktivität, sodass der Benutzer dann auf eine Aktivität klicken kann, um zu überprüfen, ob ein bestimmter Bericht genehmigt wurde).
* **Medienwiedergabe-apps** – erstellen Sie eine Aktivität pro logischer Gruppierung von Inhalt (z. b. eine Wiedergabeliste, ein Programm oder eigenständiger Inhalt). Die zugrunde liegende Frage für App-Entwickler ist, ob jedes Inhalts Element (TV-Episode, Lied) als eigenständiger Inhalt oder Teil einer Auflistung gezählt wird. Als allgemeine Regel gilt: Wenn der Benutzer eine Auflistung oder einen sequenziellen Inhalt wieder gibt, ist die Auflistung als Ganzes die Aktivität. Wenn Sie sich für die Wiedergabe eines einzelnen Inhalts entscheiden, ist dies ein Teil der Inhalte. Weitere spezifische Richtlinien finden Sie unten.
  * **Musik: Album/Artist/Genre** – wenn der Benutzer ein Album, einen Künstler oder ein Genre auswählt und auf **spielen**trifft, ist diese Auflistung die Aktivität. Schreiben Sie für jeden Song keine separate Aktivität. Für kurze Auflistungen wie ein einzelnes Album oder Sammlungen, die in zufälliger Reihenfolge wiedergegeben werden, müssen Sie die Aktivität möglicherweise nicht aktualisieren, um die aktuelle Position des Benutzers widerzuspiegeln. Für eine lange sequenzielle Wiedergabe wie ein Album oder eine Wiedergabeliste ist es möglicherweise sinnvoll, die Position innerhalb des Albums aufzuzeichnen.
  * **Musik: intelligente Wiedergabelisten** – Anwendungen, die Musik in zufälliger Reihenfolge abspielen, sollten eine einzelne Aktivität für diese Wiedergabeliste aufzeichnen. Wenn der Benutzer die Wiedergabeliste ein zweites Mal wieder gibt, würden Sie zusätzliche Verlaufs Datensätze für dieselbe Aktivität erstellen. Das Aufzeichnen der aktuellen Position des Benutzers in der Wiedergabeliste ist nicht erforderlich, da die Reihenfolge zufällig ist.
  * **TV-Serie** – Wenn Ihre APP für die Wiedergabe der nächsten Folge konfiguriert ist, nachdem die aktuelle Folge fertig ist, sollten Sie eine einzelne Aktivität für die TV-Serie schreiben. Wenn Sie die verschiedenen Episoden über mehrere Anzeige Sitzungen hinweg wiedergeben, aktualisieren Sie Ihre Aktivität, um die aktuelle Position in der Reihe widerzuspiegeln, und es werden mehrere Verlaufs Datensätze erstellt.
  * **Movie** – ein Film ist ein einzelner Inhalt und sollte über einen eigenen Verlaufs Daten Satz verfügen. Wenn der Benutzer den Film nicht mehr überwachen kann, ist es wünschenswert, seine Position aufzuzeichnen. Wenn Sie die Aktivität in Zukunft fortsetzen möchten, kann die Aktivität den Film fortsetzen, in dem Sie aufgehört hat, oder den Benutzer Fragen, ob er am Anfang fortgesetzt oder gestartet werden soll.

## <a name="user-activity-design"></a>Entwurf der Benutzeraktivität

Benutzeraktivitäten bestehen aus drei Komponenten: einem Aktivierungs-URI, visuellen Daten und Inhalts Metadaten.
* Der Aktivierungs-URI ist ein URI, der an eine Anwendung oder eine-Funktion übermittelt werden kann, um die Anwendung mit einem bestimmten Kontext fortzusetzen. In der Regel verwenden diese Verknüpfungen die Form des Protokoll Handlers für ein Schema (z. b. "My-App://Page2? Action = Edit"). Der Entwickler muss feststellen, wie URI-Parameter von der APP behandelt werden. Weitere Informationen finden Sie unter [handle-URI-Aktivierung](./handle-uri-activation.md) .
* Die visuellen Daten, die aus einem Satz erforderlicher und optionaler Eigenschaften (z. b. Titel, Beschreibung oder adaptiver Kartenelemente) bestehen, ermöglichen Benutzern eine visuelle Identifizierung einer Aktivität. Im folgenden finden Sie Richtlinien zum Erstellen von adaptiven Karten Visualisierungen für Ihre Aktivität.
* Die Inhalts Metadaten sind JSON-Daten, die zum Gruppieren und Abrufen von Aktivitäten in einem bestimmten Kontext verwendet werden können. In der Regel nimmt dies die Form von http://schema.org Daten an. Im folgenden finden Sie Richtlinien zum Ausfüllen dieser Daten.

### <a name="adaptive-card-design-guidelines"></a>Entwurfs Richtlinien für Adaptive Karten

Wenn Aktivitäten in der Zeitachse angezeigt werden, werden Sie mit dem [adaptiven Karten Framework](/adaptive-cards/)angezeigt. Wenn der Entwickler keine adaptive Karte für jede Aktivität bereitstellt, erstellt die Zeitachse automatisch eine einfache Karte basierend auf dem APP-Namen/-Symbol, dem erforderlichen Titel Feld und dem optionalen Beschreibungsfeld. 

App-Entwicklern wird empfohlen, benutzerdefinierte Karten mithilfe des JSON-Schemas für die einfache Adaptive Karte bereitzustellen. Technische Anweisungen zum Erstellen von adaptiven Karten Objekten finden Sie in der [Dokumentation zu adaptiven Karten](/adaptive-cards/authoring-cards/getting-started) . Beachten Sie die nachstehenden Richtlinien zum Entwerfen von adaptiven Karten in Benutzeraktivitäten.
* Verwenden von Bildern
  * Verwenden Sie, sofern möglich, ein eindeutiges Image für jede Aktivität. Der Name und das Symbol der Anwendung werden automatisch neben der Karte der Aktivität angezeigt. Weitere Images helfen Benutzern, die gesuchte Aktivität zu finden.
  * Bilder sollten keinen Text enthalten, der vom Benutzer erwartet wird. Dieser Text ist nicht für Benutzer mit Barrierefreiheits Anforderungen verfügbar und kann nicht durchsucht werden.
  * Wenn das Bild keinen Text enthält und auf ungefähr ein 2:1-Verhältnis zugeschnitten werden kann, sollten Sie es als Hintergrundbild verwenden. Dies führt zu einer fetten Aktivitäts Karte, die in der Zeitachse angezeigt wird. Das Bild wird leicht abgeblendet, um sicherzustellen, dass der Text auf der Karte sichtbar bleibt, und es wird empfohlen, in diesem Fall nur den Aktivitäts Namen zu verwenden, da kleinerer Text schwer lesbar werden kann.
  * Wenn das Bild nicht auf 2:1 zugeschnitten werden kann, sollten Sie es in der Aktivitäts Karte ablegen.  
    * Wenn das Seitenverhältnis Square oder Hochformat ist, verankern Sie das Bild auf der rechten Seite der Karte ohne Ränder.
    * Wenn das Seitenverhältnis Querformat ist, verankern Sie das Bild in der oberen rechten Ecke der Karte.
* Jede Aktivität muss einen Aktivitäts Namen bereitstellen, der immer angezeigt werden sollte.
  * Dieser Name sollte in der oberen linken Ecke der Karte mit der Option für den großen fetten Text angezeigt werden. Es ist wichtig, dass der Name leicht erkennbar ist, da dies der einzige Teil ist, den Benutzer sehen, wenn die Aktivität in Cortana-Szenarios angezeigt wird. Wenn Sie den gleichen Namen in der Zeitachse anzeigen, ist es für Benutzer einfacher, eine große Anzahl von Aktivitäten zu durchsuchen.
* Verwenden Sie für alle Aktivitäten Ihrer APP denselben visuellen Stil, damit Benutzer die Aktivitäten Ihrer APP auf der Zeitachse leicht finden können.
  * Beispielsweise sollten Aktivitäten alle die gleiche Hintergrundfarbe verwenden.
* Verwenden Sie ergänzende Textinformationen sparsam. 
  * Vermeiden Sie es, die Karte mit Text zu füllen, und verwenden Sie nur ergänzende Informationen, die Benutzern bei der Suche nach der richtigen Aktivität oder der Wiederverwendung von Zustandsinformationen (z. b. dem aktuellen Status einer bestimmten Aufgabe) unterstützt.

### <a name="content-metadata-guidelines"></a>Richtlinien für Inhalts Metadaten

Benutzeraktivitäten können auch Inhalts Metadaten enthalten, die von Windows und Cortana zum Kategorisieren von Aktivitäten und zum Generieren von Rückschlüsse verwendet werden. Aktivitäten können dann nach einem bestimmten Thema gruppiert werden, z. b. nach einem Speicherort (wenn der Benutzer einen Urlaub untersucht), nach dem Objekt (wenn der Benutzer etwas untersucht) oder nach der Aktion (wenn der Benutzer ein bestimmtes Produkt über verschiedene apps und Websites hinweg kauft). Es empfiehlt sich, sowohl die Nomen als auch die Verben zu repräsentieren, die an einer Aktivität beteiligt sind. 

Im folgenden Beispiel stellt der Content Metadata JSON (Content Metadata JSON) gemäß den Standards von [Schema.org](https://schema.org/)das Szenario dar: "John hat wütende Vögel mit Steve gespielt".

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

* [UserActivities-Namespace](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Zugehörige Themen

* [Benutzeraktivitäten (Project Rom-Dokumentation)](/windows/project-rome/user-activities/)
* [Adaptive Karten](/adaptive-cards/)
* [Bildschirm Abbildung von adaptiven Karten, Beispiele](https://adaptivecards.io/)
* [Behandeln der URI-Aktivierung](./handle-uri-activation.md)
* [Verwenden der Microsoft Graph, des Aktivitäts Feeds und adaptiver Karten für Ihre Kunden auf beliebigen Plattformen](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)