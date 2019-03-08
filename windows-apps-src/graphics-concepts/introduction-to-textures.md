---
title: Einführung zu Texturen
description: Eine Texturressource ist eine Datenstruktur zum Speichern von Texel, der die kleinste Einheit einer Textur darstellt, die gelesen oder geschrieben werden kann. Wird eine Textur von einem Shader gelesen, kann sie durch Textursampler gefiltert werden.
ms.assetid: 6F3C76A8-F762-4296-AE02-BFBD6476A5A8
keywords:
- Einführung zu Texturen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3cd5ca66635b57b79c2fca3e6ff10b8debb43fd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618165"
---
# <a name="introduction-to-textures"></a>Einführung zu Texturen


Eine Texturressource ist eine Datenstruktur zum Speichern von Texel, der die kleinste Einheit einer Textur darstellt, die gelesen oder geschrieben werden kann. Wird eine Textur von einem Shader gelesen, kann sie durch Textursampler gefiltert werden.

Eine Texturressource ist eine strukturierte Datensammlung, die für das Speichern von Texel entwickelt wurde. Ein Texel repräsentiert die kleinste Einheit einer Textur, die von einer Pipeline gelesen oder geschrieben werden kann. Im Gegensatz zu Puffern können Texturen von Textursamplern gefiltert werden, da sie von Shader-Einheiten gelesen werden. Der Texturtyp hat Einfluss darauf, wie die Textur gefiltert wird. Jede Texel enthält 1 bis 4-Komponenten, die in eines der DXGI-Formate, die durch die DXGI definierten angeordnet\_FORMAT-Enumeration.

Texturen werden als strukturierte Ressource mit einer bekannten Größe erstellt. Jedoch kann jede Textur zum Zeitpunkt des Erstellens von Ressourcen typisiert oder typenlos sein, solange der Typ unter Verwendung einer Ansicht vollständig angegeben ist, wenn die Textur an die Pipeline gebunden wird.

## <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span>Texturtypen


Direct3D unterstützt verschiedene Gleitkommadarstellungen. Alle Gleitkommaberechnungen arbeiten unter einer definierten Teilmenge von IEEE 754, 32-Bit-Gleitkommaregeln mit einfacher Genauigkeit.

Es gibt mehrere Typen von Texturen aus: 1, D, 2D, 3D, von denen jeder mit oder ohne Mipmaps erstellt werden kann. Direct3D unterstützt auch Texturarrays und multi-gesampelte Texturen.

-   [1D Texturen](#texture1d-resource)
-   [Textur 1D-Arrays](#texture1d-array-resource)
-   [2D Texturen und 2D-Textur Arrays](#texture2d-resource)
-   [3D-Strukturen](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-textures"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D Texturen

In ihrer einfachsten Form enthält eine 1D-Textur die Texturdaten, die mit einer einzelnen Texturkoordinate behandelt werden können; sie können als Array von Texeln dargestellt werden, wie in der folgenden Abbildung dargestellt.

Die folgende Abbildung zeigt eine 1D-Textur:

![1D-Textur](images/d3d10-1d-texture.png)

Jeder Texel enthält eine Reihe von Farbkomponenten, die vom Format der gespeicherten Daten abhängig sind. Um eine höhere Komplexität zu erreichen, können Sie eine 1D-Textur mit Mipmap-Ebenen erstellen, wie in der folgenden Abbildung dargestellt.

![1D-Textur mit Mipmap-Ebenen](images/d3d10-resource-texture1d.png)

Eine Mipmap-Ebene ist eine Textur, die um die Zweierpotenz kleiner ist als die Ebene darüber. Die oberste Ebene enthält die meisten Details und jede nachfolgende Ebene ist kleiner. Bei einer 1D-Mipmap enthält die niedrigste Ebene ein Texel. Darüber hinaus reduzieren MIP-Ebenen immer auf 1:1 herunter.

Wenn Mipmaps für eine ungerade Anzahl Texturen erstellt werden, ist die nächst niedrigere Ebene immer eine gerade Zahl (ausgenommen die niedrigste Ebene erreicht 1). Das Diagramm stellt beispielsweise eine 5x1-Textur da, deren nächst kleinere Ebene eine 2x1-Textur bildet. Die nächst kleinere (und letzte) Mip-Ebene bildet eine Textur mit der Größe 1x1. Die unterschiedlichen Ebenen werden durch einen Index bezeichnet, der LOD (Detailstufe) genannt wird; Sie können die LOD verwenden, um auf eine kleinere Textur zuzugreifen, wenn Sie eine Geometrie berechnen und ausgeben, die nicht so nah bei der Kamera liegt.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-arrays"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Textur 1D-Arrays

Direct3D unterstützt auch Arrays mit Texturen. Ein 1D-Texturarray ähnelt vom Konzept her der folgenden Abbildung.

![Ein 1D-Texturarray](images/d3d10-resource-texture1darray.png)

Dieses Texturarray enthält drei Texturen. Jede der drei Texturen verfügt über eine Texturbreite von 5 (bei der es sich um die Anzahl der Elemente in der ersten Ebene handelt). Jede Textur enthält auch eine Mipmap mit drei Ebenen.

Alle Texturarrays in Direct3D sind ein homogenes Texturarray; das bedeutet, dass jede Textur in einem Texturarray dasselbe Datenformat und dieselbe Größe aufweisen muss (einschließlich der Texturbreite und der Anzahl der Mipmap-Ebenen). Sie können Texturarrays mit verschiedenen Größen erstellen, solange alle Texturen in jedem Array von der Größe her übereinstimmen.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-textures-and-2d-texture-arrays"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D Texturen und 2D-Textur Arrays

Eine Texture2D-Ressource enthält ein Raster mit 2D-Texeln. Jedes Texel kann über einen u,v-Vektor angesprochen werden. Da es sich um eine Texturressource handelt, kann sie Mipmap-Ebenen und Unterressourcen enthalten. Eine vollständig ausgefüllte 2D-Texturressource entspricht der folgenden Abbildung.

![einer 2D-Texturressource](images/d3d10-resource-texture2d.png)

Diese Texturressource enthält eine einzelne 3x5-Textur mit drei Mipmap-Ebenen.

Eine 2D-Texturarray-Ressource ist ein homogenes 2D-Texturarray; das heißt, jede Textur hat dasselbe Datenformat und dieselben Dimensionen (z. B. Mipmap-Ebenen). Es verfügt über ein ähnliches Layout wie das 1D-Texturarray, mit der Ausnahme, dass die Texturen jetzt 2D-Daten enthalten, wie in der folgenden Abbildung dargestellt.

![ein 2D-Texturarray](images/d3d10-resource-texture2darray.png)

Dieses Texturarray enthält drei Texturen; jede Textur ist 3x5 mit zwei Mipmap-Ebenen.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-2d-texture-array-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Hierbei wird ein Array der 2D-Textur als Textur Cube

Ein Textur-Würfel ist ein 2D-Texturarray, das sechs Texturen enthält, eine für jede Fläche des Würfels. Ein vollständig ausgefüllter Textur-Würfel entspricht der folgenden Abbildung.

![Ein Array an 2D-Texturressourcen, die einen Textur-Würfel darstellen](images/d3d10-resource-texturecube.png)

Ein 2D-Texturarray mit sechs Texturen kann mit den der Würfelzuordnung innewohnenden Funktionen aus dem Inneren von Shadern gelesen werden, nachdem sie mit einer Würfel-Texturansicht an die Pipeline gebunden wurden. Textur-Würfel werden vom Shader aus mit einem 3D-Vektor angesprochen, der von der Mitte des Textur-Würfels nach außen weist.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-textures"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D-Strukturen

Eine 3D-Texturressource (auch bekannt als Volumentextur) enthält ein 3D-Volumen an Texeln. Da es sich um eine Texturressource handelt, kann sie Mipmap-Ebenen enthalten. Eine vollständig ausgefüllte 3D-Textur entspricht der folgenden Abbildung.

![eine 3D-Texturressource](images/d3d10-resource-texture3d.png)

Wenn ein 3D-Textur-Mipmap-Segment als Zielausgabe der Berechnung und der Ausgabe gebunden ist (mit einer Berechnen-Ziel-Ansicht), verhält sich die 3D-Textur analog zu einem 2D-Texturarray mit n Segmenten. Das konkrete Berechnungssegment wird aus der Geometrie-Shader-Phase ausgewählt.

Es gibt kein Konzept eines 3D-Texturarrays; dementsprechend ist eine 3D-Texturunterressource eine einzelne Mipmap-Ebene.

Koordinatensysteme für Direct3D sind für Pixel und Texel definiert.

## <a name="span-idpixelspanspan-idpixelspanspan-idpixelspanpixel-coordinate-system"></a><span id="Pixel"></span><span id="pixel"></span><span id="PIXEL"></span>Pixel-Koordinatensystem.


Das Pixel-Koordinatensystem in Direct3D definiert den Ursprung eines Renderziels an der oberen linken Ecke, wie im folgenden Diagramm dargestellt. Die Mittelpunkte der Pixel werden um (0,5f,0,5f) von Ihren Ganzzahlpositionen versetzt.

![Diagramm eines Pixel-Koordinatensystems in Direct3D 10](images/d3d10-coordspix10.png)

## <a name="span-idtexelspanspan-idtexelspanspan-idtexelspantexel-coordinate-system"></a><span id="Texel"></span><span id="texel"></span><span id="TEXEL"></span>Koordinatensystem Texel


Das Texel-Koordinatensystem hat seinen Ursprung in der oberen linken Ecke der Textur, wie im folgenden Diagramm dargestellt. Das vereinfacht das Rendern von am Bildschirm ausgerichteten Texturen erheblich, da das Pixel-Koordinatensystem mit dem Texel-Koordinatensystem ausgerichtet wird.

![Diagramm eines Texel-Koordinatensystems](images/d3d10-coordstex10.png)

Texturkoordinaten werden entweder mit einer normalisierten oder skalierten Zahl dargestellt. Jede Texturkoordinate ist, wie nachfolgend dargestellt, einem bestimmten Texel geordnet:

Für eine normalisierte Koordinate:

-   Zeigen Sie die stichprobenerstellung: Texel \# = Floor (U \* Breite)
-   Lineare stichprobenerstellung: Links Texel \# = Floor (U \* Breite), rechts Texel \# = links Texel \# + 1

Für eine skalierte Koordinate:

-   Zeigen Sie die stichprobenerstellung: Texel \# floor(U) =
-   Lineare stichprobenerstellung: Links Texel \# = Floor (U - 0,5), rechts Texel \# = links Texel \# + 1

Wobei die Breite die Texturbreite (in Texel) ist.

Das Adress-Wrapping der Textur findet nach dem Berechnen der Texelposition statt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)
