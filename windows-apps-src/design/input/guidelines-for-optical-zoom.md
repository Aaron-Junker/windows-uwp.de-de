---
Description: This topic describes Windows zooming and resizing elements and provides user experience guidelines for using these interaction mechanisms in your apps.
title: Richtlinien für optischen Zoom und Größenänderung
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5fcbaa0a3db826ef971878acd6a553dd7a836508
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "8695940"
---
# <a name="optical-zoom-and-resizing"></a>Optischer Zoom und Größenänderung



In diesem Artikel werden die Windows-Elemente für das Zoomen und die Größenänderung beschrieben. Außerdem enthält das Thema Richtlinien für die Benutzeroberfläche, um diese Interaktionsmechanismen in Ihren Apps zu verwenden.

> **Wichtige APIs**: [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Input (XAML)**](https://msdn.microsoft.com/library/windows/apps/br227994)

Mithilfe des optischen Zooms können Benutzer die Ansicht des Inhalts in einem Inhaltsbereich vergrößern (die Vergrößerung erfolgt für den gesamten Inhaltsbereich). Bei der Größenänderung hingegen können Benutzer die relative Größe eines oder mehrerer Objekte ändern, ohne die Ansicht des Inhaltsbereichs zu ändern (die Größenänderung erfolgt für die Objekte im Inhaltsbereich).

Der optische Zoom und die Größenänderung (Interaktion) werden mit den Bewegungen für Zusammendrücken und Aufziehen ausgeführt (durch Auseinanderbewegen der Finger wird vergrößert, und durch Zueinanderbewegen der Finger wird verkleinert). Alternativ können Benutzer die STRG-Taste gedrückt halten und gleichzeitig das Mausrad drehen oder die STRG-Taste gedrückt halten (wenn keine Zehnertastatur verfügbar ist, zusammen mit der UMSCHALTTASTE) und die Taste mit dem Pluszeichen (+) oder dem Minuszeichen (-) drücken.

Die folgenden Diagramme verdeutlichen die Unterschiede zwischen Größenänderung und optischem Zoom.

**Optischer Zoom**: Der Benutzer wählt einen Bereich aus und vergrößert den gesamten Bereich.

![Wenn die Finger aufeinander zu bewegt werden, wird der Inhaltsbereich vergrößert, beim Spreizen der Finger wird er verkleinert.](images/areazoom.png)

**Größe ändern**: Der Benutzer wählt ein Objekt in einem Bereich aus und ändert die Größe dieses Objekts.

![Wenn die Finger aufeinander zu bewegt werden, wird das Objekt verkleinert, beim Spreizen der Finger wird es vergrößert.](images/objectresize.png)

**Hinweis:**  optischen Zoom dürfen nicht mit dem [Semantischen Zoom](../controls-and-patterns/semantic-zoom.md)verwechselt werden. Zwar werden bei beiden Interaktionen dieselben Gesten ausgeführt, jedoch bezieht sich der semantische Zoom auf die Darstellung von und die Navigation in Inhalten in einer einzelnen Ansicht (z.B. in der Ordnerstruktur eines Computers, einer Dokumentbibliothek oder einem Fotoalbum).

 

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


Beachten Sie die folgenden Richtlinien für Apps, die entweder Größenänderung oder optischen Zoom unterstützen.

-   Wenn Beschränkungen oder Grenzen der maximalen und minimalen Größe definiert sind, sollte ein visuelles Feedback erfolgen, wenn der Benutzer die Grenzen erreicht oder überschreitet.
-   Mit Andockpunkten kann das Zoom- und Größenänderungsverhalten beeinflusst werden, indem logische Punkte bereitgestellt werden, an denen die Manipulation angehalten und sichergestellt wird, dass eine bestimmte Teilmenge des Inhalts im Viewport angezeigt wird. Stellen Sie Andockpunkte für gängige Zoomfaktoren oder logische Ansichten bereit, um die Auswahl dieser Zoomfaktoren zu erleichtern. In Foto-Apps könnte z.B. ein Andockpunkt bei 100% für Größenänderungen bereitgestellt werden, und bei Karten-Apps könnten Andockpunkte in Ansichten von Städten, Staaten und Ländern/Regionen nützlich sein.

    Andockpunkte ermöglichen den Benutzern ungenaue Gesten, mit denen sie dennoch das Gewünschte erreichen. Wenn Sie mit XAML arbeiten, finden Sie weitere Informationen unter den Eigenschaften der Andockpunkte von [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527). Verwenden Sie für JavaScript und HTML [**-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895).

    Es gibt zwei Arten von Andockpunkten:

    -   Näherung – Nachdem der Kontakt aufhoben wurde, wird ein Andockpunkt ausgewählt, wenn die Trägheitsbewegung innerhalb einer Distanzschwelle zum Andockpunkt anhält. Bei Näherungsandockpunkten kann ein Zoom- oder Größenänderungsvorgang zwischen Andockpunkten enden.
    -   Erforderlich – Der ausgewählte Andockpunkt ist der Punkt direkt vor oder nach dem Andockpunkt, der vor dem Aufheben des Kontakts zuletzt überschritten wurde (abhängig von der Richtung und Geschwindigkeit der Geste). Eine Manipulation muss an einem erforderlichen Andockpunkt enden.
-   Verwenden Sie Trägheitseffekte. Dazu zählen:
    -   Verlangsamung: Findet statt, wenn der Benutzer mit dem Zusammendrücken oder Aufziehen aufhört. Dies ist mit allmählichem Anhalten auf glattem Untergrund vergleichbar.
    -   Springen: Beim Überschreiten einer Größeneinschränkung oder -grenze erfolgt ein leichter Rückpralleffekt.
-   Verteilen Sie die Steuerelemente entsprechend den [Richtlinien für Zielbestimmung](guidelines-for-targeting.md).
-   Stellen Sie für eine eingeschränkte Größenänderung Ziehpunkte für die Skalierung bereit. Wenn keine Ziehpunkte angegeben werden, wird standardmäßig die isometrische bzw. proportionale Größenänderung verwendet.
-   Verwenden Sie nicht den Zoom, um in der UI zu navigieren oder zusätzliche Steuerelemente in der App verfügbar zu machen, sondern verwenden Sie stattdessen Verschiebungen. Weitere Informationen zur Verschiebung finden Sie unter [Richtlinien für Verschiebung](guidelines-for-panning.md).
-   Platzieren Sie keine in der Größe veränderbare Objekte in einem Inhaltsbereich, dessen Größe geändert werden kann. Ausnahmen bilden die folgenden Fälle:
    -   Zeichnungsprogramme, in denen in der Größe anpassbare Elemente in einem Zeichenbereich oder auf einer Zeichenfläche, dessen bzw. deren Größe geändert werden kann, angezeigt werden können
    -   Webseiten mit einem eingebetteten Objekt, z.B. einer Karte

    **Hinweis:**  In allen Fällen wird der Inhaltsbereich geändert, es sei denn, alle Berührungspunkte innerhalb der Größe anpassbaren Objekt.

     

## <a name="related-articles"></a>Verwandte Artikel


**Beispiele**
* [Einfaches Eingabebeispiel](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Beispiel für Eingabe mit niedriger Latenz](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Beispiel für Gerätefunktionen](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für Fingereingabe-Treffertests](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoom](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: vereinfachtes Freihandbeispiel](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Beispiel für Windows8-Bewegungen](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Beispiel für Manipulationen und Gesten (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Beispiel für die DirectX-Fingereingabe](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




