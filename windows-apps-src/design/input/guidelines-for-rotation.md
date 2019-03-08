---
Description: In diesem Thema wird beschrieben, die neue Windows-Benutzeroberfläche für die Rotation und enthält Richtlinien zur benutzerfreundlichkeit, die berücksichtigt werden sollten, wenn Sie dieser neue Mechanismus für die Interaktion in Ihre UWP-app verwenden.
title: Drehung
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f631f3178b4af4fe1c1d2d8b27e8ae6ac25c6ad1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617205"
---
# <a name="rotation"></a>Drehung


In diesem Artikel wird die neue Windows-Benutzeroberfläche beschrieben, die Drehungen unterstützt. Außerdem enthält er Richtlinien für die Benutzeroberfläche, die Sie berücksichtigen sollten, wenn Sie diesen neuen Interaktionsmechanismus in einer UWP-App verwenden.

> **Wichtige APIs**: [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung


**Übersicht über die Drehung**

Die Drehung ist eine für Touchscreens optimierte Technik, die von UWP-Apps verwendet wird und mit der Benutzer ein Objekt kreisförmig drehen können (im Uhrzeigersinn oder gegen den Uhrzeigersinn).

Abhängig vom Eingabegerät wird für die Drehungsinteraktion Folgendes verwendet:

-   Eine Maus oder ein aktiver Zeichenstift/Eingabestift zum Verschieben des Drehungsziehelements eines ausgewählten Objekts.
-   Berührung oder passiver Zeichen-/Eingabestift zum Drehen des Objekts in die gewünschte Richtung mit der Drehbewegung.

**Verwenden der Drehung**

Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können. In den folgenden Diagrammen sehen Sie einige der unterstützten Fingerpositionen für die Drehungsinteraktion.

![Diagramm zur Veranschaulichung verschiedener Fingerhaltungen, die für Drehungen unterstützt werden](images/ux-rotate-positions.png)

**Beachten Sie**    intuitiv, und klicken Sie in den meisten Fällen die Drehung ist ein zwei Touch-Punkt, wenn der Benutzer ein, die nicht mit der Kontaktpunkte (z. B. in einer Zeichnung oder das Layout-Anwendung) Drehpunkt angeben kann. In den folgenden Abbildungen wird gezeigt, wie die Benutzeroberfläche beeinträchtigt werden kann, wenn der Drehungspunkt nicht auf diese Weise eingeschränkt ist.

In der ersten Abbildung sehen Sie den ersten (Daumen) und den zweiten Berührungspunkt (Zeigefinger): Der Zeigefinger berührt einen Baum, und der Daumen berührt einen Holzblock.

![Bild, auf dem die zwei ersten Touch-Punkten für die Rotation Bewegung.](images/ux-rotate-points1.png)
In dieser zweiten Abbildung wird die Drehung um den anfänglichen Berührungspunkt (Daumen) ausgeführt. Nach der Drehung berührt immer noch der Zeigefinger den Baumstamm und der Daumen den Holzblock (Drehungspunkt).

![Bild, auf dem Bild mit den Drehpunkt gedreht, die auf eine der ersten zwei Berührungspunkte beschränkt werden.](images/ux-rotate-points2.png)
In dieser dritten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Da das Bild nicht um einen der Finger gedreht wurde, hat der Benutzer nach der Drehung nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![Bild, auf dem Bild mit den Drehpunkt gedreht, die auf die Mitte des Bildes statt eines der beiden ersten Touch-Punkte beschränkt werden.](images/ux-rotate-points3.png)
In dieser letzten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Auch in diesem Fall hat der Benutzer nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch den am weitesten links angeordneten Mittelpunkt des Bilds anstatt durch die zwei anfänglichen Berührungspunkte eingeschränkt ist](images/ux-rotate-points4.png)

 

Windows 8 unterstützt drei Arten von Rotation: kostenlose, eingeschränkte und kombinierten.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Typ</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Freie Drehung</td>
<td align="left"><p>Bei der freien Drehung können Benutzer Inhalte frei um bis zu 360 Grad drehen. Wenn das Objekt losgelassen wird, bleibt es in der ausgewählten Position. Die freie Drehung ist hilfreich in Zeichen- und Layoutanwendungen wie beispielsweise Microsoft PowerPoint, Word, Visio und Paint sowie Adobe Photoshop, Illustrator und Flash.</p></td>
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
<strong>Beachten Sie</strong>  Rail eine Benutzer-Schnittstelle ist eine Funktion, die in dem ein Bereich, um ein Ziel schränkt den Trend zu einigen spezifischen Wert oder Speicherort, um die Auswahl zu beeinflussen.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>Verwandte Themen


**Beispiele**
* [Grundlegende Eingabebeispiel](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: XAML-benutzerbeispiel Eingabeereignisse](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Funktionen-gerätebeispiel](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel zu Leistungstests in Touch Treffer](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML Bildlauf, schwenken und Zoomen Beispiel](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Vereinfachte Freihand-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Gesten und Bearbeitungen mit GestureRecognizer](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Manipulationen und Beispiel für Bewegungen (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX-Touch-Eingabe-Beispiel](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




