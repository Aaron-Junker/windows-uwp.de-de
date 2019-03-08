---
title: Übersicht über die Beleuchtung
description: Wenn Sie Direct3D-Beleuchtung verwenden, erlauben Sie es Direct3D, die Details der Beleuchtung für Sie zu übernehmen. Fortgeschrittene Benutzer können die Beleuchtung bei Bedarf selbst ausführen.
ms.assetid: FCBF6A92-4EAC-4CCC-A76C-79985AF348AE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e90e460cf5f5bda7d90447440d76cf6898a83747
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660205"
---
# <a name="lighting-overview"></a>Übersicht über die Beleuchtung

Wenn Sie Direct3D-Beleuchtung verwenden, erlauben Sie es Direct3D, die Details der Beleuchtung für Sie zu übernehmen. Fortgeschrittene Benutzer können die Beleuchtung bei Bedarf selbst ausführen.

Wenn die Beleuchtung aktiviert ist, berechnet Direct3D die Farbe für jeden Objekt-Vertex basierend auf einer Kombination der folgenden Parameter:

-   Texel in einer zugeordneten Texturmap.
-   Die diffusen und Glanzfarben am Vertex, falls angegeben.
-   Die Farbe und Intensität des Lichts, das von Lichtquellen in der Szene oder durch die Umgebungshelligkeit der Szene erzeugt wird.

Wie Sie mit Beleuchtung und Materialien arbeiten, hat einen großen Einfluss auf das Erscheinungsbild der gerenderten Szene. Materialien bestimmen, wie Licht von einer Oberfläche reflektiert wird. Direktes Licht und Umgebungshelligkeit bestimmen, welches Licht reflektiert wird. Wenn die Beleuchtung aktiviert ist, müssen Sie Materialien zum Rendern einer Szene verwenden.

Beleuchtung ist zum Rendern einer Szene nicht erforderlich, allerdings sind Details in einer unbeleuchtet gerenderten Szene nicht sichtbar. Das Rendern einer unbeleuchteten Szene hat zum Ergebnis, dass die Objekte der Szene bestenfalls als Silhouette dargestellt werden. Eine solche Darstellung ist für die meisten Verwendungszwecke nicht detailliert genug.

## <a name="span-iddirectlightvsambientlightspanspan-iddirectlightvsambientlightspandirect-light-vs-ambient-light"></a><span id="direct_light_vs._ambient_light"></span><span id="DIRECT_LIGHT_VS._AMBIENT_LIGHT"></span>Direkte Licht im Vergleich zu Umgebungslicht


Obwohl direktes und Umgebungslicht die Objekte einer Szene beleuchten, sind sie unabhängig voneinander; sie haben äußerst unterschiedliche Effekte und erfordern eine vollkommen unterschiedliche Handhabung.

*Direktes Licht* trifft direkt auf das Objekt. Direktes Licht hat immer eine Richtung und eine Farbe, und es ist ein bestimmender Faktor für Schattierungsalgorithmen, wie z. B. die Gouraud-Schattierung. Unterschiedliche Lichtquellen geben direktes Licht auf unterschiedliche Weise ab, wodurch spezielle Dämpfungseffekte entstehen.

*Umgebungslicht* ist praktisch überall in einer Szene zu finden. Umgebungslicht ist ein allgemeines Lichtniveau, das unabhängig von Objekten und deren Position in der Szene eine gesamte Szene ausfüllt. Umgebungslicht hat keine Position oder Richtung, sondern nur Farbe und Intensität. Jede Lichtquelle verstärkt das Umgebungslicht in einer Szene.

Die Farbe des Umgebungslichts wird als RGBA-Wert dargestellt, wobei jede Komponente einen ganzzahligen Wert zwischen 0 und 255 erhält. Dies unterscheidet sich von den meisten Farbwerten in Direct3D.

Die roten, grünen und blauen Komponenten ergeben in Kombination die endgültige Farbe des Umgebungslichts. Die Alpha-Komponente steuert die Transparenz der Farbe. Bei Verwendung von Hardwarebeschleunigung oder RGB-Emulation wird die Alpha-Komponente ignoriert.

## <a name="span-iddirect3dlightmodelvsnaturespanspan-iddirect3dlightmodelvsnaturespandirect3d-light-model-vs-nature"></a><span id="direct3d_light_model_vs._nature"></span><span id="DIRECT3D_LIGHT_MODEL_VS._NATURE"></span>Direct3D-hell-Modell im Vergleich zu der Art


In der Natur wird das von einer Quelle abgegebene Licht von Hunderten, wenn nicht sogar Tausenden oder Millionen von Objekten reflektiert, bevor es das Auge des Benutzers erreicht. Bei jedem Reflexionsvorgang wird ein Teil des Lichts von einer Oberfläche absorbiert, ein weiterer Teil wird in zufällige Richtungen verstreut, und der Rest landet auf einer anderen Oberfläche oder im Auge des Benutzers. Dieser Vorgang setzt sich fort, bis das Licht verschwindet, oder ein Benutzer das Licht wahrnimmt.

Selbstverständlich sind die Berechnungen, die für eine perfekte Simulation des natürlichen Lichtverhaltens erforderlich sind, zu zeitintensiv für Direct3D-Echtzeitgrafiken. Unter dem Aspekt der Schnelligkeit bietet das Direct3D-Lichtmodell daher eine Annäherung an die Funktionsweise des Lichts in der natürlichen Welt. Direct3D beschreibt Licht hinsichtlich der Komponenten Rot, Grün und Blau, aus deren Kombination die endgültige Farbe entsteht.

Wenn in Direct3D Licht von einer Oberfläche reflektiert wird, interagiert die Farbe des Lichts mathematisch mit der Oberfläche, und es entsteht die Farbe, die letztendlich auf dem Bildschirm erscheint. Genauere Informationen zu den von Direct3D verwendeten Algorithmen finden Sie unter [Mathematik der Beleuchtung](mathematics-of-lighting.md).

Das Direct3D-Beleuchtungsmodell generalisiert Licht zu zwei Typen: Umgebungslicht und direktes Licht. Jeder Typ verfügt über unterschiedliche Attribute und interagiert mit dem Material einer Oberfläche auf unterschiedliche Weise. Umgebungslicht ist Licht, das derart verstreut wurde, dass seine Richtung und Quelle unbestimmt sind: es bewahrt überall eine niedrige Intensität. Die von Fotografen verwendete indirekte Beleuchtung ist ein gutes Beispiel für Umgebungslicht.

In Direct3D verfügt das Umgebungslicht, wie auch in der Natur, nicht über eine echte Richtung oder Quelle, sondern nur über eine Farbe und eine Intensität. Tatsächlich ist die Helligkeit des Umgebungslichts vollständig unabhängig von den Objekte in einer Szene, die Licht erzeugen. Umgebungslicht erzeugt keine Glanzlichter-Reflexionen.

Direktes Licht ist das Licht, das von einer Quelle innerhalb einer Szene erzeugt wird. Es verfügt immer über eine Farbe und eine Intensität und bewegt sich in einer bestimmten Richtung. Direktes Licht interagiert mit dem Material einer Oberfläche und erzeugt dabei Glanzlichter. Seine Richtung ist sein Faktor in Schattierungsalgorithmen, einschließlich der Gouraud-Schattierung. Wenn direktes Licht reflektiert wird, hat es keinen Einfluss auf die Umgebungshelligkeit einer Szene. Die Licht erzeugenden Quellen einer Szene haben unterschiedliche Merkmale, die beeinflussen, wie sie eine Szene ausleuchten.

Darüber hinaus hat das Material eines Polygons Eigenschaften, die beeinflussen, wie das Polygon empfangenes Licht reflektiert. Sie legen eine einzelne Reflexionseigenschaft fest, die beschreibt, wie das Material Umgebungslicht reflektiert, und Sie legen einzelne Eigenschaften fest, um die gerichtete und diffuse Reflexion des Materials festzulegen.

## <a name="span-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspancolor-values-for-lights-and-materials"></a><span id="Color_Values_for_Lights_and_Materials"></span><span id="color_values_for_lights_and_materials"></span><span id="COLOR_VALUES_FOR_LIGHTS_AND_MATERIALS"></span>RGB-Werte für die Beleuchtung und Materialien


Direct3D beschreibt Farbe mittels der vier Komponenten (Rot, Grün, Blau und Alpha), die zusammen eine endgültige Farbe erzeugen. Jede Komponente hat einen Bereich von 0,0 bis 1,0. Obwohl Licht und Material die gleiche Struktur zum Beschreiben von Farbe verwenden, werden die Werte von Lichtern und Materialien leicht unterschiedlich verwendet.

Farbwerte für Lichtquellen bezeichnen die Menge einer bestimmten, von der Quelle abgegebenen Lichtkomponente. Da Lichter keine Alpha-Komponente verwenden, sind nur die roten, grünen und blauen-Komponenten einer Farbe relevant. Sie können sich die drei Komponenten als die rote, grüne und blaue Linse eines Videoprojektors vorstellen. Jede Linse kann ausgeschaltet sein (Wert von 0,0 in dem entsprechenden Element), sie kann so hell wie möglich eingestellt sein (Wert von 1,0), oder sie kann auf einen Wert dazwischen eingestellt sein.

Die durch die Linsen kommenden Farben werden zur endgültigen Farbe des Lichts kombiniert. Eine Kombination aus z. B. R(1,0), G(1,0) und B(1,0) erzeugt weißes Licht, und die Kombination R(0,0), G(0,0) und B(0,0) erzeugt gar kein Licht. Sie können ein Licht mit nur einer Komponente erzeugen, wodurch rein rotes, grünes oder blaues Licht entsteht, oder Kombinationen für das Licht verwenden, um Farben wie Gelb oder Violett zu erzeugen. Sie können sogar negative Werte für die Farbkomponenten einstellen, um "dunkles Licht" zu erzeugen, das Licht aus einer Szene entfernt. Sie können die Komponenten auch auf einen Wert festlegen, der größer als 1,0 ist, um äußerst helles Licht zu erzeugen.

Bei Materialien bezeichnen Farbwerte allerdings, wie viel von einer Lichtkomponente von einer gerenderten Oberfläche aus diesem Material reflektiert wird. Ein Material mit den Farbkomponenten R(1,0), G(1,0), B(1,0) und A(1,0) reflektiert alles Licht, das auf dieses Material trifft. Entsprechend reflektiert ein Material mit R(0,0), G(1,0), B(0,0) und A(1,0) sämtliches grünes Licht, das auf dieses Material trifft. Materialien müssen mehrere Reflexionswerte besitzen, um verschiedene Effekttypen zu erzeugen.

Siehe [Lichttypen](light-types.md) und [Lichteigenschaften](light-properties.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Lichter und Materialien](lights-and-materials.md)

 

 




