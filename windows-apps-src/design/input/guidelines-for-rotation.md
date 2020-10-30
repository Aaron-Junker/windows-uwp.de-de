---
description: In diesem Thema wird die neue Windows-Benutzeroberfläche für die Rotation beschrieben und Richtlinien zur Benutzeroberfläche erläutert, die bei der Verwendung dieses neuen Interaktions Mechanismus in Ihrer Windows-App berücksichtigt werden sollten.
title: Drehung
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 755386b8cffa5c546d20cd561693da5d21b30799
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035103"
---
# <a name="rotation"></a>Drehung


Dieser Artikel beschreibt die neue Windows-Benutzeroberfläche für die Rotation und bietet Richtlinien für die Benutzeroberfläche, die bei der Verwendung dieses neuen Interaktions Mechanismus in Ihrer Windows-App berücksichtigt werden sollten.

> **Wichtige APIs** : [**Windows. UI. Input**](/uwp/api/Windows.UI.Input), [**Windows. UI. XAML. Input**](/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Empfehlungen für die Vorgehensweise

-   Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung


**Übersicht über Drehung**

Rotation ist die von Windows-apps verwendete Touchscreen-Methode, mit der Benutzer ein Objekt in Zirkel Richtung (im Uhrzeigersinn oder gegen den Uhrzeigersinn) umwandeln können.

Abhängig vom Eingabegerät wird für die Drehungsinteraktion Folgendes verwendet:

-   Eine Maus oder ein aktiver Zeichenstift/Eingabestift zum Verschieben des Drehungsziehelements eines ausgewählten Objekts.
-   Berührung oder passiver Zeichen-/Eingabestift zum Drehen des Objekts in die gewünschte Richtung mit der Drehbewegung.

**Gründe für die Verwendung von Drehung**

Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können. In den folgenden Diagrammen sehen Sie einige der unterstützten Fingerpositionen für die Drehungsinteraktion.

![Diagramm zur Veranschaulichung verschiedener Fingerhaltungen, die für Drehungen unterstützt werden](images/ux-rotate-positions.png)

**Hinweis**  
Intuitiv ist der Drehungspunkt in den meisten Fällen einer der zwei Berührungspunkte, es sei denn, der Benutzer kann einen Drehungspunkt angeben, der in keinem Bezug zu den Kontaktpunkten steht (beispielsweise in einer Zeichen- oder Layoutanwendung). In den folgenden Abbildungen wird gezeigt, wie die Benutzeroberfläche beeinträchtigt werden kann, wenn der Drehungspunkt nicht auf diese Weise eingeschränkt ist.

In der ersten Abbildung sehen Sie den ersten (Daumen) und den zweiten Berührungspunkt (Zeigefinger): Der Zeigefinger berührt einen Baum, und der Daumen berührt einen Holzblock.

![Abbildung mit den zwei anfänglichen Berührungspunkten für die Drehbewegung](images/ux-rotate-points1.png)
In dieser zweiten Abbildung wird die Drehung um den anfänglichen Berührungspunkt (Daumen) ausgeführt. Nach der Drehung berührt immer noch der Zeigefinger den Baumstamm und der Daumen den Holzblock (Drehungspunkt).

![Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch einen der zwei anfänglichen Berührungspunkte eingeschränkt ist](images/ux-rotate-points2.png)
In dieser dritten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Da das Bild nicht um einen der Finger gedreht wurde, hat der Benutzer nach der Drehung nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch den Mittelpunkt des Bilds anstatt durch die zwei anfänglichen Berührungspunkte eingeschränkt ist](images/ux-rotate-points3.png)
In dieser letzten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Auch in diesem Fall hat der Benutzer nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch den am weitesten links angeordneten Mittelpunkt des Bilds anstatt durch die zwei anfänglichen Berührungspunkte eingeschränkt ist](images/ux-rotate-points4.png)

 

Windows 10 unterstützt drei Arten der Drehung: kostenlos, eingeschränkt und kombiniert.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">type</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Freie Drehung</td>
<td align="left"><p>Die kostenlose Rotation ermöglicht einem Benutzer, Inhalte an beliebiger Stelle in einem 360-Grad-Bogen zu drehen. Wenn der Benutzer das Objekt freigibt, verbleibt das Objekt an der ausgewählten Position. Die freie Drehung ist hilfreich in Zeichen- und Layoutanwendungen wie beispielsweise Microsoft PowerPoint, Word, Visio und Paint sowie Adobe Photoshop, Illustrator und Flash.</p></td>
</tr>
<tr class="even">
<td align="left">Eingeschränkte Drehung</td>
<td align="left"><p>Die eingeschränkte Drehung unterstützt freie Drehung während der Änderung, erzwingt aber beim Loslassen Andockpunkte in 90-Grad-Schritten (0, 90, 180 und 270). Wenn Benutzer das Objekt loslassen, wird es automatisch zum nächsten Andockpunkt gedreht.</p>
<p>Die eingeschränkte Drehung ist die gängigste Drehungsmethode und funktioniert ähnlich wie ein Bildlauf in Inhalten. Dank der Andockpunkte müssen Benutzer nicht präzise arbeiten und können trotzdem ihr Ziel erreichen. Die eingeschränkte Drehung ist hilfreich für Anwendungen wie beispielsweise Webbrowser und Fotoalben.</p></td>
</tr>
<tr class="odd">
<td align="left">Kombinierte Drehung</td>
<td align="left"><p>Die kombinierte Drehung unterstützt freie Drehung mit Zonen (ähnlich wie Führungsschienen unter <a href="guidelines-for-panning.md">Richtlinien für Verschiebung</a>) an jedem der 90-Grad-Andockpunkte, die durch die eingeschränkte Drehung erzwungen werden. Wenn das Objekt außerhalb einer der 90-Grad-Zonen losgelassen wird, bleibt das Objekt in dieser Position. Anderenfalls wird das Objekt automatisch zu einem Andockpunkt gedreht.</p>
<div class="alert">
<strong>Hinweis</strong>  Eine Benutzeroberflächen Schiene ist eine Funktion, bei der ein Bereich um ein Ziel die Bewegung auf einen bestimmten Wert oder Speicherort beschränkt, um die Auswahl zu beeinflussen.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="related-topics"></a>Verwandte Themen

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
- [Eingabe: Gesten und Manipulationen mit GestureRecognizer](/samples/browse/?redirectedfrom=MSDN-samples)
- [Eingabe: Manipulationen und Gesten (Beispiel)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Beispiel für die DirectX-Fingereingabe](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
