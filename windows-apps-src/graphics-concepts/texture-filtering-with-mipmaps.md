---
title: Texturfilterung mit Mipmaps
description: Ein Mipmap ist eine Sequenz von Texturen, von denen jede eine Darstellung desselben Bildes mit schrittweise niedrigerer Auflösung ist. Höhe und Breite der einzelnen Bilder bzw. Ebenen des Mipmaps sind jeweils um eine Zweierpotenz geringer als die der vorherigen Ebene.
ms.assetid: 28E863A2-C776-40E4-8551-9851DF7EC93E
keywords:
- Texturfilterung mit Mipmaps
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 474f97f32439c389be8283bb10e0c0ed716b3f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662045"
---
# <a name="texture-filtering-with-mipmaps"></a>Texturfilterung mit Mipmaps


Ein *Mipmap* ist eine Sequenz von Texturen, von denen jede eine Darstellung desselben Bildes mit schrittweise niedrigerer Auflösung ist. Höhe und Breite der einzelnen Bilder bzw. Ebenen des Mipmaps sind jeweils um eine Zweierpotenz geringer als die der vorherigen Ebene. Mipmaps müssen nicht quadratisch sein.

Ein hochauflösendes Mipmap-Bild wird für Objekte verwendet, die sich nahe dem Benutzer befinden. Bilder mit niedrigerer Auflösung werden verwendet, wenn das Objekt weiter entfernt zu sein scheint. Das Mipmapping verbessert die Qualität der gerenderten Texturen, dafür wird aber mehr Arbeitsspeicher benötigt.

Direct3D repräsentiert Mipmaps als Kette angefügter Flächen. Die Textur mit der höchsten Auflösung befindet sich am Anfang der Kette, und die nächste Mipmap-Ebene ist daran angehängt. Diese Ebene hat wiederum die nächste Mipmap-Ebene als Anhang und so weiter bis zur Mipmap-Ebene mit der niedrigsten Auflösung.

Die folgenden Abbildungen zeigen ein Beispiel für diese Ebenen. Die Bitmap-Texturen repräsentieren ein Zeichen auf einem Container in einem 3D-Ego-Spiel. Bei der Erstellung als Mipmap ist die Textur mit der höchsten Auflösung die erste in dem Satz. Jede nachfolgende Textur in dem Mipmap-Satz hat eine um eine Zweierpotenz geringere Höhe und Breite. In diesem Fall ist die maximale Mipmap-Auflösung 256 x 256 Pixel. Die nächste Textur hat den Wert 128 x 128. Die letzte Textur in der Kette hat dann den Wert 64 x 64.

Für das Zeichen gilt eine maximale Entfernung, aus der es noch sichtbar ist. Wenn der Benutzer weit von dem Zeichen entfernt beginnt, zeigt das Spiel die kleinste Textur in der Mipmap-Kette an, in diesem Fall die Textur mit der Auflösung 64 x 64.

![Illustration einer 64 x 64-Textur eines Gefahrenzeichens](images/mip1.jpg)

Wenn sich der Blickpunkt des Benutzers auf das Zeichen zu bewegt, werden Texturen mit zunehmender Auflösung in der Mipmap-Kette verwendet. Die Auflösung in der folgenden Abbildung ist 128 x 128.

![Illustration einer 128 x 128-Textur eines Gefahrenzeichens](images/mip2.jpg)

Die Textur mit der höchsten Auflösung wird verwendet, wenn sich der Blickpunkt des Benutzers im minimal zulässigen Abstand von dem Zeichen befindet, wie in der nachfolgenden Abbildung gezeigt.

![Illustration einer 256 x 256-Textur eines Gefahrenzeichens](images/mip3.jpg)

Dies ist eine effizientere Möglichkeit zur Simulation der Perspektive für Texturen. Anstatt eine einzelne Textur mit vielen Auflösung zu rendern, ist es schneller, mehrere Texturen mit unterschiedlichen Auflösungen zu verwenden.

Direct3D kann feststellen, welche Textur in einem Mipmap-Satz die Auflösung hat, die dem gewünschten Ergebnis am nächsten kommt; dazu können Pixel dem Texelbereich zugeordnet werden. Wenn die Auflösung des endgültigen Bilds zwischen der Auflösung der Mipmap-Texturen liegt, kann Direct3D die Texel in beiden Mipmaps untersuchen und ihre Farbwerte zusammenführen.

Um Mipmaps verwenden zu können, muss Ihre Anwendung einen Satz von Mipmaps erstellen. Anwendungen verwenden Mipmaps durch Auswahl des Mipmap-Satzes als erste Textur im aktuellen Texturensatz. Siehe [Texturvermischung](texture-blending.md).

Anschließend muss Ihre Anwendung das Filterungsverfahren einrichten, das Direct3D für das Texelsampling verwendet. Die schnellste Methode zur Mipmap-Filterung besteht darin, Direct3D das nächste Texel auswählen zu lassen. Verwenden Sie die D3DTEXF\_Punkt-Enumerationswert ab, um dies auszuwählen. Direct3D können erzeugt eine bessere Ergebnisse filtern, wenn Ihre Anwendung die D3DTEXF verwendet\_LINEARE-Enumerationswert. Dadurch wird das nächste Mipmap ausgewählt; anschließend wird der gewichtete Durchschnitt der Texel um den Ort in der Textur berechnet, dem das aktuelle Pixel zugeordnet wird.

Mipmap-Texturen werden in 3D-Szenen verwendet, um den Zeitaufwand für das Rendern einer Szene zu verringern. Dazu kommt, dass sie den Realismus einer Szene verbessern. Andererseits erfordern sie jedoch viel Speicherplatz.

**Beachten Sie**    jede Oberfläche in eine Mipmap-Kette ist Dimensionen, die Hälfte, die die vorherige Oberfläche in der Kette sind. Wenn der Mipmap der obersten Ebene die Abmessungen 256 x 128 hat, hat der Mipmap der zweiten Ebene 128 x 64, der der dritten Ebene 64 x 32 und so weiter bis 1 x 1. Sie können keine Anzahl von Mipmap-Ebenen anfordern, die dazu führen würde, dass die breite oder die Höhe eines der Mipmaps in der Kette kleiner als 1 wäre. Im einem einfachen Fall einer 4 x 2-Mipmap-Oberfläche auf der obersten Ebene ist der maximal zulässige Wert für Ebenen drei. Die Abmessungen der obersten Ebene sind 4 x 2, die der zweiten Ebene 2 x 1, und die der dritten Ebene sind 1 x 1. Ein Wert von mehr als drei Ebenen führt zu einem Bruchwert der Höhe des Mipmaps der zweiten Ebene und ist daher nicht zulässig.

 

Direct3D kann die Mipmap-Texturfilterung automatisch ausführen. Anwendungen können manuell eine Mipmap-Kette durchlaufen, um Bitmap-Daten zu jeder Oberfläche in der Kette zu laden. Dies ist häufig der einzige Grund zum Durchlaufen der Kette. Das automatische Generieren von Mipmaps zum Zeitpunkt der Erstellung der Textur nutzt die Hardware-Filterung, da Mipmaps sich im Videospeicher befinden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturfilterung](texture-filtering.md)

 

 




