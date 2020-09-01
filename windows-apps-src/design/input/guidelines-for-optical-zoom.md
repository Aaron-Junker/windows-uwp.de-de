---
Description: In diesem Thema werden die Windows-Elemente für das Zoomen und Ändern der Größe beschrieben. Außerdem enthält das Thema Richtlinien für die Benutzeroberfläche zur Verwendung dieser Interaktionsmechanismen in Ihren Apps.
title: Richtlinien für optischen Zoom und Größenänderung
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f5864be7a4a7f80e376439e8db387e2b2853dfd7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172484"
---
# <a name="optical-zoom-and-resizing"></a>Optischer Zoom und Größenänderung



In diesem Artikel werden die Windows-Elemente für das Zoomen und die Größenänderung beschrieben. Außerdem enthält er Richtlinien für die Benutzeroberfläche, um diese Interaktionsmechanismen in Ihren Apps zu verwenden.

> **Wichtige APIs**: [**Windows. UI. Input**](/uwp/api/Windows.UI.Input), [**Input (XAML)**](/uwp/api/Windows.UI.Xaml.Input)

Mithilfe des optischen Zooms können Benutzer die Ansicht des Inhalts in einem Inhaltsbereich vergrößern (die Vergrößerung erfolgt für den gesamten Inhaltsbereich). Bei der Größenänderung hingegen können Benutzer die relative Größe eines oder mehrerer Objekte ändern, ohne die Ansicht des Inhaltsbereichs zu ändern (die Größenänderung erfolgt für die Objekte im Inhaltsbereich).

Der optische Zoom und die Größenänderung (Interaktion) werden mit den Bewegungen für Zusammendrücken und Aufziehen ausgeführt (durch Auseinanderbewegen der Finger wird vergrößert, und durch Zueinanderbewegen der Finger wird verkleinert). Alternativ können Benutzer die STRG-Taste gedrückt halten und gleichzeitig das Mausrad drehen oder die STRG-Taste gedrückt halten (wenn keine Zehnertastatur verfügbar ist, zusammen mit der UMSCHALTTASTE) und die Taste mit dem Pluszeichen (+) oder dem Minuszeichen (-) drücken.

Die folgenden Diagramme verdeutlichen die Unterschiede zwischen Größenänderung und optischem Zoom.

**Optischer Zoom**: Der Benutzer wählt einen Bereich aus und vergrößert den gesamten Bereich.

![Wenn die Finger aufeinander zu bewegt werden, wird der Inhaltsbereich vergrößert, beim Spreizen der Finger wird er verkleinert.](images/areazoom.png)

**Größenänderung**: Der Benutzer wählt ein Objekt in einem Bereich aus und ändert die Größe dieses Objekts.

![Wenn die Finger aufeinander zu bewegt werden, wird das Objekt verkleinert, beim Spreizen der Finger wird es vergrößert.](images/objectresize.png)

**Hinweis**    Optischer Zoom sollte mit [semantischem Zoom](../controls-and-patterns/semantic-zoom.md)nicht verwechselt werden. Zwar werden bei beiden Interaktionen dieselben Gesten ausgeführt, jedoch bezieht sich der semantische Zoom auf die Darstellung von und die Navigation in Inhalten in einer einzelnen Ansicht (z. B. in der Ordnerstruktur eines Computers, einer Dokumentbibliothek oder einem Fotoalbum).

 

## <a name="dos-and-donts"></a>Empfehlungen für die Vorgehensweise


Beachten Sie die folgenden Richtlinien für Apps, die entweder Größenänderung oder optischen Zoom unterstützen.

-   Wenn Beschränkungen oder Grenzen der maximalen und minimalen Größe definiert sind, sollte ein visuelles Feedback erfolgen, wenn der Benutzer die Grenzen erreicht oder überschreitet.
-   Mit Andockpunkten kann das Zoom- und Größenänderungsverhalten beeinflusst werden, indem logische Punkte bereitgestellt werden, an denen die Manipulation angehalten und sichergestellt wird, dass eine bestimmte Teilmenge des Inhalts im Viewport angezeigt wird. Stellen Sie Andockpunkte für gängige Zoomfaktoren oder logische Ansichten bereit, um die Auswahl dieser Zoomfaktoren zu erleichtern. In Foto-Apps könnte z. B. ein Andockpunkt bei 100 % für Größenänderungen bereitgestellt werden, und bei Karten-Apps könnten Andockpunkte in Ansichten von Städten, Staaten und Ländern/Regionen nützlich sein.

    Andockpunkte ermöglichen den Benutzern ungenaue Gesten, mit denen sie dennoch das Gewünschte erreichen. Wenn Sie mit XAML arbeiten, finden Sie weitere Informationen unter den Eigenschaften der Andockpunkte von [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer). Verwenden Sie für JavaScript und HTML [**-ms-content-zoom-snap-points**](/previous-versions/hh771895(v=vs.85)).

    Es gibt zwei Arten von Andockpunkten:

    -   Näherung – Nachdem der Kontakt aufhoben wurde, wird ein Andockpunkt ausgewählt, wenn die Trägheitsbewegung innerhalb einer Distanzschwelle zum Andockpunkt anhält. Bei Näherungsandockpunkten kann ein Zoom- oder Größenänderungsvorgang zwischen Andockpunkten enden.
    -   Erforderlich – Der ausgewählte Andockpunkt ist der Punkt direkt vor oder nach dem Andockpunkt, der vor dem Aufheben des Kontakts zuletzt überschritten wurde (abhängig von der Richtung und Geschwindigkeit der Geste). Eine Manipulation muss an einem erforderlichen Andockpunkt enden.
-   Verwenden Sie Trägheitseffekte. Dabei handelt es sich z. B. um:
    -   Verlangsamung: Findet statt, wenn der Benutzer mit dem Zusammendrücken oder Aufziehen aufhört. Dies ist mit allmählichem Anhalten auf glattem Untergrund vergleichbar.
    -   Springen: Beim Überschreiten einer Größeneinschränkung oder -grenze erfolgt ein leichter Rückpralleffekt.
-   Verteilen Sie die Steuerelemente entsprechend den [Richtlinien für Zielbestimmung](guidelines-for-targeting.md).
-   Stellen Sie für eine eingeschränkte Größenänderung Ziehpunkte für die Skalierung bereit. Wenn keine Ziehpunkte angegeben werden, wird standardmäßig die isometrische bzw. proportionale Größenänderung verwendet.
-   Verwenden Sie nicht den Zoom, um in der UI zu navigieren oder zusätzliche Steuerelemente in der App verfügbar zu machen, sondern verwenden Sie stattdessen Verschiebungen. Weitere Informationen zur Verschiebung finden Sie unter [Richtlinien für Verschiebung](guidelines-for-panning.md).
-   Platzieren Sie keine in der Größe veränderbare Objekte in einem Inhaltsbereich, dessen Größe geändert werden kann. Ausnahmen bilden die folgenden Fälle:
    -   Zeichnungsprogramme, in denen in der Größe anpassbare Elemente in einem Zeichenbereich oder auf einer Zeichenfläche, dessen bzw. deren Größe geändert werden kann, angezeigt werden können
    -   Webseiten mit einem eingebetteten Objekt, z. B. einer Karte

    **Hinweis**    In allen Fällen wird die Größe des Inhalts Bereichs geändert, es sei denn, alle Berührungspunkte liegen innerhalb des Objekts, dessen Größe geändert werden kann.

## <a name="related-articles"></a>Verwandte Artikel

### <a name="samples"></a>Beispiele

- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Eingabe: Beispiel für Gerätefunktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Eingabe: Beispiel für Fingereingabe-Treffertests](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Eingabe: vereinfachtes Freihandbeispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Eingabe: Beispiel für Windows 8-Bewegungen](/samples/browse/?redirectedfrom=MSDN-samples)
- [Eingabe: Manipulationen und Gesten (Beispiel)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Beispiel für die DirectX-Fingereingabe](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))