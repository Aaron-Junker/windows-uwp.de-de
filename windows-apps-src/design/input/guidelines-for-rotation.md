---
Description: In diesem Thema wird die neue Windows-Benutzeroberfläche für die Rotation beschrieben und Richtlinien zur Benutzeroberfläche bereitstellt, die bei der Verwendung dieses neuen Interaktions Mechanismus in der UWP-App berücksichtigt werden sollten
title: Drehung
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257928"
---
# <a name="rotation"></a>Drehung


In diesem Artikel wird die neue Windows-Benutzeroberfläche beschrieben, die Drehungen unterstützt. Außerdem enthält er Richtlinien für die Benutzeroberfläche, die Sie berücksichtigen sollten, wenn Sie diesen neuen Interaktionsmechanismus in einer UWP-App verwenden.

> **Wichtige APIs**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung


**Übersicht über die Drehung**

Die Drehung ist eine für Touchscreens optimierte Technik, die von UWP-Apps verwendet wird und mit der Benutzer ein Objekt kreisförmig drehen können (im Uhrzeigersinn oder gegen den Uhrzeigersinn).

Abhängig vom Eingabegerät wird für die Drehungsinteraktion Folgendes verwendet:

-   Eine Maus oder ein aktiver Zeichenstift/Eingabestift zum Verschieben des Drehungsziehelements eines ausgewählten Objekts.
-   Berührung oder passiver Zeichen-/Eingabestift zum Drehen des Objekts in die gewünschte Richtung mit der Drehbewegung.

**Verwendung der Drehung**

Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können. In den folgenden Diagrammen sehen Sie einige der unterstützten Fingerpositionen für die Drehungsinteraktion.

![Diagramm zur Veranschaulichung verschiedener Fingerhaltungen, die für Drehungen unterstützt werden](images/ux-rotate-positions.png)

**Beachten Sie**   intuitiv, und in den meisten Fällen ist der Drehpunkt einer der beiden Berührungspunkte, es sei denn, der Benutzer kann einen Drehpunkt angeben, der nicht mit den Kontaktpunkten verknüpft ist (z. b. in einer Zeichnungs-oder layoutanwendung). In den folgenden Abbildungen wird gezeigt, wie die Benutzeroberfläche beeinträchtigt werden kann, wenn der Drehungspunkt nicht auf diese Weise eingeschränkt ist.

In der ersten Abbildung sehen Sie den ersten (Daumen) und den zweiten Berührungspunkt (Zeigefinger): Der Zeigefinger berührt einen Baum, und der Daumen berührt einen Holzblock.

![Bild, das die beiden ersten Berührungspunkte für die Drehbewegung anzeigt.](images/ux-rotate-points1.png)
In dieser zweiten Abbildung wird die Drehung um den anfänglichen Berührungspunkt (Daumen) ausgeführt. Nach der Drehung berührt immer noch der Zeigefinger den Baumstamm und der Daumen den Holzblock (Drehungspunkt).

![Bild, das ein gedrehtes Bild anzeigt, bei dem der Drehpunkt auf einen der beiden ersten Berührungspunkte beschränkt ist.](images/ux-rotate-points2.png)
In dieser dritten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Da das Bild nicht um einen der Finger gedreht wurde, hat der Benutzer nach der Drehung nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![Bild, das ein gedrehtes Bild anzeigt, bei dem der Drehpunkt auf den Mittelpunkt des Bilds beschränkt ist, nicht auf einen der beiden ersten Berührungspunkte.](images/ux-rotate-points3.png)
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
<th align="left">Typ</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Freie Drehung</td>
<td align="left"><p>Mit der freien Drehung können Benutzer Inhalte frei in einem beliebigen Bereich von 360 Grad drehen. Wenn der Benutzer das Objekt ablegt, bleibt das Objekt in der ausgewählten Position. Die freie Drehung ist hilfreich in Zeichen- und Layoutanwendungen wie beispielsweise Microsoft PowerPoint, Word, Visio und Paint sowie Adobe Photoshop, Illustrator und Flash.</p></td>
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
<strong>Hinweis</strong>  eine Benutzeroberflächen Schiene ist eine Funktion, bei der ein Bereich um ein Ziel die Bewegung auf einen bestimmten Wert oder Speicherort beschränkt, um die Auswahl zu beeinflussen.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>Verwandte Themen


**Beispiele**
* [Beispiel für eine einfache Eingabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Eingabe Beispiel mit niedriger Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Archivbeispiele**
* [Eingabe: Beispiel für XAML-Benutzereingabe Ereignisse](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Eingabe: Beispiel für Gerätefunktionen](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Eingabe: Beispiel für Berührungs Treffer Tests](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Eingabe: vereinfachtes Ink-Beispiel](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Eingabe: Gesten und Manipulationen mit GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Eingabe: Manipulationen und Gesten (C++) (Beispiel)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Beispiel für DirectX-Fingereingabe](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




