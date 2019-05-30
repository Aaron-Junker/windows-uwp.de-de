---
title: Ressourcentypen
description: Die verschiedenen Ressourcentypen haben unterschiedliche Layouts (bzw. unterschiedliche Speicheranforderungen).
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- Ressourcentypen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9712b4498b03460568d20d4c8e27172ad5c14360
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362204"
---
# <a name="resource-types"></a>Ressourcentypen


Die verschiedenen Ressourcentypen haben unterschiedliche Layouts (bzw. unterschiedliche Speicheranforderungen). Alle von der Direct3D-Pipeline verwendeten Ressourcen sind von zwei grundlegenden Ressourcentypen abgeleitet: [Puffer](#buffer-resources) und [Texturen](#texture-resources). Ein Puffer ist eine Sammlung von Rohdaten (Elemente); eine Textur ist eine Sammlung von Texeln (Textur-Elementen).

Es gibt zwei Möglichkeiten, das Layout (oder den Speicherbedarf) einer Ressource vollständig anzugeben:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Element</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Typisierte</p></td>
<td align="left"><p>Typ wird bei der Erstellung der Ressource vollständig angegeben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>Typenlosen</p></td>
<td align="left"><p>Typ wird vollständig angegeben, wenn die Ressource an die Pipeline gebunden wird.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbufferresourcesspanspan-idbufferresourcesspanspan-idbufferresourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>Pufferressourcen


Eine Pufferressource ist eine Sammlung vollständig typisierter Daten; intern enthält ein Puffer Elemente. Ein Element besteht aus ein bis vier Komponenten. Beispiele für Datentypen: ein gepackter Datenwert (z. B. R8G8B8A8), eine einzelne 8-Bit-Ganzzahl, vier 32-Bit-Float-Werte. Diese Datentypen dienen dem Speichern von Daten wie Positionsvektoren, Normalenvektoren, Texturkoordinaten in einem Vertexpuffer, Indizes in einem Indexpuffer oder Gerätestatus.

Ein Puffer wird als eine unstrukturierte Ressource erstellt. Da die Puffer unstrukturiert sind, können sie keine Mip-Map-Ebenen enthalten, werden beim Lesen nicht gefiltert und unterstützen kein Multisampling.

### <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Puffer-Typen

-   [Vertex-Puffer](#vertex-buffer)
-   [Indexpuffer](#index-buffer)
-   [Konstantenpuffer](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Vertex-Puffer

Ein Puffer ist eine Sammlung von Elementen; ein Vertexpuffer enthält Daten pro Scheitelpunkt. Das einfachste Beispiel ist ein Vertexpuffer, der eine Art von Daten enthält, z. B. Positionsdaten. Dieser kann dargestellt werden, und sieht aus wie in folgender Abbildung.

![Abbildung eines Vertexpuffers, der Positionsdaten enthält](images/d3d10-resources-single-element-vb2.png)

Häufiger enthält ein Vertexpuffer aber alle erforderlichen Daten, um 3D-Scheitelpunkte vollständig dazustellen. Ein Beispiel dazu könnte ein Vertexpuffer sein, der eine Position pro Scheitelpunkt sowie Normale und Texturkoordinaten enthält. Diese Daten sind in der Regel als Sätze der Elemente pro Scheitelpunkt organisiert, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Vertexpuffers, der Positionsdaten, Normale und Texturdaten enthält](images/d3d10-vertex-buffer-element.png)

Dieser Vertexpuffer enthält Daten pro Scheitelpunkt für acht Scheitelpunkte; jeder Scheitelpunkt speichert drei Elemente (Position, Normale und Texturkoordinaten). Position und Normale werden normalerweise mit drei 32-Bit-Gleitkommawerten und die Texturkoordinaten mit zwei 32-Bit-Gleitkommawerten angegeben.

Um Daten aus einem Vertexpuffer abzurufen, müssen Sie den gewünschten Scheitelpunkt und die restlichen Pufferparameter kennen:

-   *Offset* - Die Anzahl von Bytes vom Anfang des Puffers bis zu den Daten für den ersten Scheitelpunkt.
-   *BaseVertexLocation* - Die Anzahl der Bytes vom Offset bis zum ersten Scheitelpunkt, den der entsprechende Zeichnen-Aufruf verwendet.

Bevor Sie einen Vertexpuffer erstellen, müssen Sie das Layout definieren, indem Sie ein Eingabelayout-Objekt erstellen. Nachdem das Eingabelayout-Objekt erstellt wurde, wird es an die Eingabeassemblerphase (IA-Phase) gebunden.

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Indexpuffer

Ein Indexpuffer enthält eine aufeinanderfolgende Reihe von 16-Bit- oder 32-Bit-Indizes; jeder Index wird verwendet, um einen Scheitelpunkt im Vertexpuffer zu kennzeichnen. Den Vorgang, bei dem ein Indexpuffer mit einem oder mehreren Vertexpuffern Daten an die IA-Phase liefert, nennt man Indexierung. Ein Indexpuffer kann dargestellt werden, wie in der folgenden Abbildung.

![Abbildung eines Indexpuffers](images/d3d10-index-buffer.png)

Die aufeinanderfolgenden Indizes in einem Indexpuffer werden mit den folgenden Parametern lokalisiert:

-   *Offset* - Die Anzahl an Bytes vom Anfang des Puffers bis zum ersten Index.
-   *StartIndexLocation* - Die Anzahl der Bytes vom Offset bis zum ersten Scheitelpunkt, den der entsprechende Zeichnen-Aufruf verwendet.
-   *IndexCount* – Die Anzahl der zu berechnenden und auszugebenden Indizes.

Ein Indexpuffer kann mehrere Zeilen oder Dreiecksstreifen zusammenfügen ([primitive Topologien](primitive-topologies.md)), indem diese jeweils durch einen Streifenschnittindex getrennt werden. Mit einem Streifenschnittindex können mehrere Zeilen oder Dreiecksstreifen mit einem einzigen Zeichnen-Aufruf gezeichnet werden. Ein Streifenschnittindex ist der Maximalwert für den Index (0xffff für einen 16-Bit-Index, 0xffffffff für einen 32-Bit-Index). Der Streifenschnittindex setzt die Windungsreihenfolge in indizierten Primitiven zurück und kann verwendet werden, damit keine fehlerhaften Dreiecke mehr benötigt werden, die andernfalls möglicherweise erforderlich sind, um de richtige Windungsreihenfolge in einem Dreiecksstreifen zu erhalten. Die folgende Abbildung zeigt ein Beispiel für einen Streifenschnittindex.

![Abbildung eines Streifenschnittindex](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Konstantenpuffer

Direct3D verfügt über einen Puffer für die Bereitstellung von Shaderkonstanten, der als Shader-Konstanten-Puffer oder einfach Konstantenpuffer bezeichnet wird. Konzeptionell sieht dieser wie ein Vertexpuffer mit einem Element aus, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Shader-Konstantenpuffers](images/d3d10-shader-resource-buffer.png)

Jedes Element speichert eine Konstante mit ein bis vier Komponenten, die durch das Format der gespeicherten Daten bestimmt wird.

Konstantenpuffer verringern die Bandbreite, die erforderlich ist, um Shaderkonstanten zu aktualisieren, indem sie die Gruppierung von Shaderkonstanten und das gleichzeitige Einchecken erlauben, anstatt einzelne Aufrufe zum Einchecken jeder einzelnen Konstante durchzuführen.

Ein Shader liest Variablen in einem Konstantenpuffer weiterhin direkt mit dem Variablennamen; auf die gleiche Art und Weise werden Variablen gelesen, die nicht Teil eines Konstantenpuffers sind.

Jede Shader-Phase ermöglicht bis zu 15 Shader-Konstantenpuffer; Jeder Puffer kann bis zu 4096 Konstanten vorhalten.

Verwenden Sie einen Konstantenpuffer, um die Ergebnisse der Datenstromausgabephase zu speichern.

Unter [Shaderkonstanten (DirectX HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) finden Sie ein Beispiel zum Erklären eines Konstantenpuffers in einem Shader.

## <a name="span-idtextureresourcesspanspan-idtextureresourcesspanspan-idtextureresourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>Texturressourcen


Eine Texturressource ist eine strukturierte Datensammlung, die für das Speichern von Texel entwickelt wurde. Im Gegensatz zu Puffern können Texturen von Textursamplern gefiltert werden, da sie von Shader-Einheiten gelesen werden. Der Texturtyp hat Einfluss darauf, wie die Textur gefiltert wird. Ein Texel repräsentiert die kleinste Einheit einer Textur, die von einer Pipeline gelesen oder geschrieben werden kann. Jede Texel enthält 1 bis 4-Komponenten, die in einem der Formate DXGI angeordnet sind (siehe [ **DXGI\_FORMAT**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

Texturen werden als strukturierte Ressource erstellt, damit ihre Größe bekannt ist. Jedoch kann jede Textur zum Zeitpunkt des Erstellens von Ressourcen typisiert oder typenlos sein, solange der Typ unter Verwendung einer Ansicht vollständig angegeben ist, wenn die Textur an die Pipeline gebunden ist.

-   [Texturtypen](#texture-types)
-   [Unterressourcen](#subresources)
-   [Starke im Vergleich zu Schwache Typisierung](#typed)

### <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>Texturtypen

Es gibt mehrere Typen von Texturen aus: 1, D, 2D, 3D, von denen jeder mit oder ohne Mipmaps erstellt werden kann. Direct3D unterstützt auch Texturarrays und multi-gesampelte Texturen.

-   [1D Textur](#texture1d-resource)
-   [Textur 1D-array](#texture1d-array-resource)
-   [2D-Textur und 2D-Textur-Array](#texture2d-resource)
-   [3D-Textur](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D Textur

In ihrer einfachsten Form enthält eine 1D-Textur die Texturdaten, die mit einer einzelnen Texturkoordinate behandelt werden können; sie können als Array von Texeln dargestellt werden, wie in der folgenden Abbildung dargestellt.

![Abbildung einer 1D-Textur](images/d3d10-1d-texture.png)

Jeder Texel enthält eine Reihe von Farbkomponenten, die vom Format der gespeicherten Daten abhängig sind. Um eine höhere Komplexität zu erreichen, können Sie eine 1D-Textur mit Mipmap-Ebenen erstellen, wie in der folgenden Abbildung dargestellt.

![Abbildung einer 1D-Textur mit Mip-Map-Ebenen](images/d3d10-resource-texture1d.png)

Eine Mipmap-Ebene ist eine Textur, die um die Zweierpotenz kleiner ist als die Ebene darüber. Die oberste Ebene enthält die meisten Details und jede nachfolgende Ebene ist kleiner; bei einer 1D-Mip-Map enthält die niedrigste Ebene ein Texel. Die unterschiedlichen Ebenen werden durch einen Index bezeichnet, der LOD (Detailtiefe) genannt wird; Sie können die LOD verwenden, um auf eine kleinere Textur zuzugreifen, wenn Sie eine Geometrie berechnen und ausgeben, die nicht so nah bei der Kamera liegt.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Textur 1D-array

Direct3D 10 verfügt außerdem über eine neue Datenstruktur für ein Texturarray. Ein 1D-Texturarray ähnelt vom Konzept her der folgenden Abbildung.

![Abbildung eines 1D-Texturarrays](images/d3d10-resource-texture1darray.png)

Dieses Texturarray enthält drei Texturen. Jede der drei Texturen verfügt über eine Texturbreite von 5 (bei der es sich um die Anzahl der Elemente in der ersten Ebene handelt). Jede Textur enthält auch eine Mip-Map mit 3 Ebenen.

Alle Texturarrays in Direct3D sind ein homogenes Texturarray; das bedeutet, dass jede Textur in einem Texturarray dasselbe Datenformat und dieselbe Größe aufweisen muss (einschließlich der Texturbreite und der Anzahl der Mipmap-Ebenen). Sie können Texturarrays mit verschiedenen Größen erstellen, solange alle Texturen in jedem Array von der Größe her übereinstimmen.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D-Textur und 2D-Textur-Array

Eine Texture2D-Ressource enthält ein Raster mit 2D-Texeln. Jedes Texel kann über einen u,v-Vektor angesprochen werden. Da es sich um eine Texturressource handelt, kann sie Mip-Map-Ebenen und Unterressourcen enthalten. Eine vollständig ausgefüllte 2D-Texturressource entspricht der folgenden Abbildung.

![Abbildung einer 2D-Texturressource](images/d3d10-resource-texture2d.png)

Diese Texturressource enthält eine einzelne 3x5-Textur mit drei Mipmap-Ebenen.

+Eine Texture2DArray-Ressource ist ein homogenes 2D-Texturarray; das heißt, jede Textur hat dasselbe Datenformat und dieselben Dimensionen (z. B. Mip-Map-Ebenen). Es verfügt über ein ähnliches Layout wie das 1D-Texturarray, mit der Ausnahme, dass die Texturen jetzt 2D-Daten enthalten, und dementsprechend entspricht es der folgenden Abbildung.

![Abbildung eines Arrays an 2D-Texturressourcen](images/d3d10-resource-texture2darray.png)

Dieses Texturarray enthält drei Texturen; jede Textur ist 3x5 mit zwei Mipmap-Ebenen.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Verwenden ein Texture2DArray als Textur Cube

Ein Textur-Würfel ist ein 2D-Texturarray, das sechs Texturen enthält, eine für jede Fläche des Würfels. Ein vollständig ausgefüllter Textur-Würfel entspricht der folgenden Abbildung.

![Abbildung eines Arrays an 2D-Texturressourcen, die einen Textur-Würfel darstellen](images/d3d10-resource-texturecube.png)

Ein 2D-Texturarray mit sechs Texturen kann mit den der Würfelzuordnung innewohnenden Funktionen aus dem Inneren von Shadern gelesen werden, nachdem sie mit einer Würfel-Texturansicht an die Pipeline gebunden wurden. Textur-Würfel werden vom Shader aus mit einem 3D-Vektor angesprochen, der von der Mitte des Textur-Würfels nach außen weist.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D-Textur

Eine Texture3D-Ressource (auch bekannt als Volumentextur) enthält ein 3D-Volumen an Texeln. Da es sich um eine Texturressource handelt, kann sie Mip-Map-Ebenen enthalten. Eine vollständig ausgefüllte 3D-Textur entspricht der folgenden Abbildung.

![Abbildung einer 3D-Texturressource](images/d3d10-resource-texture3d.png)

Wenn ein 3D-Textur-Mipmap-Segment als Zielausgabe der Berechnung und der Ausgabe gebunden ist (mit einer Berechnen-Ziel-Ansicht), verhält sich die 3D-Textur analog zu einem 2D-Texturarray mit n Segmenten. Das konkrete Berechnungssegment wird aus der Geometrie-Shader-Phase ausgewählt.

Es gibt kein Konzept eines 3D-Texturarrays; dementsprechend ist eine 3D-Texturunterressource eine einzelne Mipmap-Ebene.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>Unterressourcen

Die Direct3D-API verweist auf gesamte Ressourcen oder Teilmengen von Ressourcen. Um einen Teil der Ressourcen vorzugeben, hat Direct3D den Begriff der *Unterressourcen* geprägt, d.h. eine Teilmenge einer Ressource.

Ein Puffer wird als einzelne Unterressource definiert. Texturen sind etwas komplizierter, da es mehrere unterschiedliche Texturtypen gibt (1D, 2D usw.), von denen einige Mip-Map-Ebenen und/oder Texturarrays unterstützen. Beginnend mit dem einfachsten Fall, wird eine 1D-Textur als einzelne Unterressource definiert, wie in der folgenden Abbildung dargestellt.

![Abbildung einer 1D-Textur](images/d3d10-1d-texture.png)

Das bedeutet, dass das Array an Texeln, die eine 1D-Textur bilden, in einer einzelnen Unterressource enthalten ist.

Wenn Sie eine 1D-Textur mit drei Mip-Map-Ebenen erweitern, kann dies wie folgt dargestellt werden.

![Abbildung einer 1D-Textur mit Mip-Map-Ebenen](images/d3d10-resource-texture1d.png)

Stellen Sie sich dies als eine einzelne Textur vor, die aus drei Untertexturen besteht. Jede Untertextur zählt als Unterressource, so dass diese 1D-Textur 3 Unterressourcen enthält. Eine Untertextur (oder Unterressource) kann mithilfe der Detailtiefe (LOD) für eine einzelne Textur indiziert werden. Wenn Sie ein Texturarray verwenden, erfordert der Zugriff auf eine bestimmte Untertextur sowohl die LOD als auch die bestimmte Textur. Alternativ kombiniert die API diese zwei Informationselemente in einem einzigen nullbasierten Unterressourcenindex, wie hier gezeigt.

![Abbildung eines nullbasierten Unterressourcenindex](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselectingsubresourcesspanspan-idselectingsubresourcesspanspan-idselectingsubresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>Auswählen von Unterressourcen

Einige API greifen auf eine gesamte Ressource zu, andere greifen auf einen Teil einer Ressource zu. Die API, die auf einen Teil einer Ressource zugreifen, verwenden in der Regel eine Ansichtsbeschreibung, um die Unterressourcen festzulegen, auf welche der Zugriff erfolgen soll.

Diese Abbildungen veranschaulichen die Begriffe, die durch eine Ansichtsbeschreibung verwendet werden, wenn auf ein Texturarray zugegriffen wird.

### <a name="span-idarrayslicespanspan-idarrayslicespanspan-idarrayslicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>ArraySegment

In einem Texturarray, jede Textur mit Mip-Maps, umfasst ein Array-Segment (dargestellt durch das weiße Rechteck) eine Textur und alle Untertexturen, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Array-Segmentes](images/d3d10-resource-array-slice.png)

### <a name="span-idmipslicespanspan-idmipslicespanspan-idmipslicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>MIP-Segment

Ein mip-Segment (dargestellt durch das weiße Rechteck) enthält eine Mip-Map-Ebene für jede Textur in einem Array, wie in der folgenden Abbildung dargestellt.

![Abbildung eines mip-Segmentes](images/d3d10-resource-mip-slice.png)

### <a name="span-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>Einzelne untergeordnete Ressourcen auswählen

Sie können diese zwei Arten von Segmenten verwenden, um eine einzelne Unterressource auszuwählen, wie in der folgenden Abbildung dargestellt.

![Abbildung zum Auswählen einer Unterressource durch Verwenden eines Array-Segmentes und eines mip-Segmentes](images/d3d10-resource-subresources-1.png)

### <a name="span-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>Auswählen von mehreren Unterressourcen

Oder Sie können diese zwei Arten von Segmenten mit der Anzahl der Mip-Map-Ebenen bzw. der Anzahl von Texturen verwenden, um mehrere Unterressourcen auszuwählen.

![Abbildung zum Auswählen mehrerer Unterressourcen](images/d3d10-resource-subresources-2.png)

Unabhängig davon, welchen Texturtyp Sie verwenden, mit oder ohne Mip-Maps, mit oder ohne Texturarray, sind häufig Hilfsfunktionen bereitgestellt, um den Index einer bestimmten Unterressource zu berechnen.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Starke im Vergleich zu Schwache Typisierung

Das Erstellen einer vollständig typisierten Ressource beschränkt die Ressource auf das Format, in dem sie erstellt wurde. Dadurch kann die Runtime den Zugriff optimieren, insbesondere, wenn die Ressource mit Flags erstellt wird, die angeben, dass sie von der Anwendung nicht zugeordnet werden kann. Ressourcen, die mit einem bestimmten Typ erstellt wurden, können nicht über den Ansichtsmechanismus neu interpretiert werden.

In einer typenlosen Ressource ist der Datentyp unbekannt, wenn die Ressource erstmalig erstellt wird. Die Anwendung muss aus den verfügbaren typenlosen Formaten auswählen. Sie müssen die Größe des Speichers angeben, die zugeordnet werden soll, und ob die Runtime die Untertexturen in einer Mip-Map generieren muss.

Das genaue Datenformat (ob der Speicher als Ganzzahlen, Gleitkommawerte, vorzeichenlose Ganzzahlen usw. interpretiert wird) wird erst bestimmt, wenn die Ressource mit einer Ansicht an die Pipeline gebunden wird. Da das Texturformat flexibel bleibt, bis die Textur an die Pipeline gebunden ist, wird die Ressource als schwach typisierter Speicher bezeichnet. Ein schwach typisierter Speicher hat den Vorteil, dass er wiederverwendet oder erneut interpretiert werden kann (in einem anderen Format), solange das Komponentenbit des neuen Formats der Anzahl der Bits des alten Formates entspricht.

Eine einzelne Ressource kann an mehrere Pipelinephasen gebunden werden, solange jede über eine eindeutige Ansicht verfügt, welche die Formate an jedem Speicherort vollständig qualifiziert. Beispielsweise könnte eine mit einem typenlosen Format erstellte Ressource an verschiedenen Speicherorten in der Pipeline gleichzeitig als ein FLOAT-Format und ein UINT-Format verwendet werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ressourcen](resources.md)
