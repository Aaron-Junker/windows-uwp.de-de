---
Description: This topic describes the new Windows UI for rotation and provides user experience guidelines that should be considered when using this new interaction mechanism in your UWP app.
title: Drehung
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257928"
---
# <a name="rotation"></a>Drehung


In diesem Artikel wird die neue Windows-Benutzeroberfläche beschrieben, die Drehungen unterstützt. Außerdem enthält das Thema Richtlinien für die Benutzeroberfläche, die Sie berücksichtigen sollten, wenn Sie diesen neuen Interaktionsmechanismus in einer UWP-App verwenden.

> **Wichtige APIs**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung


**Overview of rotation**

Die Drehung ist eine für Touchscreens optimierte Technik, die von UWP-Apps verwendet wird und mit der Benutzer ein Objekt kreisförmig drehen können (im Uhrzeigersinn oder gegen den Uhrzeigersinn).

Abhängig vom Eingabegerät wird für die Drehungsinteraktion Folgendes verwendet:

-   Eine Maus oder ein aktiver Zeichenstift/Eingabestift zum Verschieben des Drehungsziehelements eines ausgewählten Objekts.
-   Berührung oder passiver Zeichen-/Eingabestift zum Drehen des Objekts in die gewünschte Richtung mit der Drehbewegung.

**When to use rotation**

Verwenden Sie Drehung, damit Benutzer leichter UI-Elemente direkt drehen können. In den folgenden Diagrammen sehen Sie einige der unterstützten Fingerpositionen für die Drehungsinteraktion.

![Diagramm zur Veranschaulichung verschiedener Fingerhaltungen, die für Drehungen unterstützt werden](images/ux-rotate-positions.png)

**Note**   Intuitively, and in most cases, the rotation point is one of the two touch points unless the user can specify a rotation point unrelated to the contact points (for example, in a drawing or layout application). In den folgenden Abbildungen wird gezeigt, wie die Benutzeroberfläche beeinträchtigt werden kann, wenn der Drehungspunkt nicht auf diese Weise eingeschränkt ist.

In der ersten Abbildung sehen Sie den ersten (Daumen) und den zweiten Berührungspunkt (Zeigefinger): Der Zeigefinger berührt einen Baum, und der Daumen berührt einen Holzblock.

![image showing the two initial touch points for the rotation gesture.](images/ux-rotate-points1.png)
In dieser zweiten Abbildung wird die Drehung um den anfänglichen Berührungspunkt (Daumen) ausgeführt. Nach der Drehung berührt immer noch der Zeigefinger den Baumstamm und der Daumen den Holzblock (Drehungspunkt).

![image showing a rotated picture with the rotation point constrained to one of the two initial touch points.](images/ux-rotate-points2.png)
In dieser dritten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Da das Bild nicht um einen der Finger gedreht wurde, hat der Benutzer nach der Drehung nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![image showing a rotated picture with the rotation point constrained to the center of the picture rather than either of the two initial touch points.](images/ux-rotate-points3.png)
In dieser letzten Abbildung wurde der Mittelpunkt der Drehung durch die Anwendung als Mittelpunkt des Bilds definiert (oder vom Benutzer entsprechend festgelegt). Auch in diesem Fall hat der Benutzer nicht das Gefühl, direkt etwas geändert zu haben (es sei denn, er hat diese Einstellung ausgewählt).

![Abbildung mit einem gedrehten Bild, dessen Drehungspunkt durch den am weitesten links angeordneten Mittelpunkt des Bilds anstatt durch die zwei anfänglichen Berührungspunkte eingeschränkt ist](images/ux-rotate-points4.png)

 

Windows 10 supports three types of rotation: free, constrained, and combined.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Geben Sie in das Suchfeld auf der Taskleiste</th>
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
<strong>Note</strong>  A user interface rail is a feature in which an area around a target constrains movement towards some specific value or location to influence its selection.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>Verwandte Themen


**Beispiele**
* [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Archivbeispiele**
* [Input: XAML user input events sample](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Input: Device capabilities sample](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Input: Touch hit testing sample](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML scrolling, panning, and zooming sample](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Input: Simplified ink sample](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Input: Gestures and manipulations with GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Input: Manipulations and gestures (C++) sample](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX touch input sample](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




