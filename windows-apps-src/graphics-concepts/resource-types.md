---
title: Ressourcentypen
description: Unterschiedliche Arten von Ressourcen haben ein unterschiedliches Layout (oder Speicherbedarf).
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- Ressourcentypen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b300747027f0c9466e7ca04b4c4b571882f57a6e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156434"
---
# <a name="resource-types"></a>Ressourcentypen


Unterschiedliche Arten von Ressourcen haben ein unterschiedliches Layout (oder Speicherbedarf). Alle von der Direct3D-Pipeline verwendeten Ressourcen werden aus zwei grundlegenden Ressourcentypen abgeleitet: [Puffer](#buffer-resources) und [Texturen](#texture-resources). Ein Puffer ist eine Auflistung von Rohdaten (Elemente). eine Textur ist eine Auflistung von texeln (Textur Elemente).

Es gibt zwei Möglichkeiten, das Layout (oder den Speicherbedarf) einer Ressource vollständig anzugeben:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Element</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Typ</p></td>
<td align="left"><p>Geben Sie den Typ beim Erstellen der Ressource vollständig an.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>Typen losen</p></td>
<td align="left"><p>Geben Sie den Typ vollständig an, wenn die Ressource an die Pipeline gebunden ist.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>Puffer Ressourcen


Eine Puffer Ressource ist eine Sammlung von vollständig typisierten Daten. intern enthält ein Puffer-Elemente. Ein Element besteht aus 1 bis 4 Komponenten. Beispiele für Element Datentypen: ein gepackter Datenwert (z. b. R8G8B8A8), eine einzelne 8-Bit-Ganzzahl, 4 32-Bit-Float-Werte. Diese Datentypen werden verwendet, um Daten zu speichern, z. b. einen Positions Vektor, einen normalen Vektor, eine Textur Koordinate in einem Vertex-Puffer, einen Index in einem Index Puffer oder einen Gerätezustand.

Ein Puffer wird als unstrukturierte Ressource erstellt. Da es nicht strukturiert ist, kann ein Puffer keine MipMap-Ebenen enthalten, wird beim Lesen nicht gefiltert und kann nicht mit einem Multisampling verwendet werden.

### <a name="span-idbuffer_typesspanspan-idbuffer_typesspanspan-idbuffer_typesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Puffer Typen

-   [Vertex-Puffer](#vertex-buffer)
-   [Index Puffer](#index-buffer)
-   [Konstanter Puffer](#shader-constant-buffer)

### <a name="span-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Vertex-Puffer

Ein Puffer ist eine Auflistung von Elementen. ein Vertex-Puffer enthält pro-Vertex-Daten. Das einfachste Beispiel ist ein Vertex-Puffer, der einen Datentyp enthält, z. b. Positionsdaten. Es kann wie in der folgenden Abbildung dargestellt werden.

![Abbildung eines Scheitelpunkt Puffers, der Positionsdaten enthält](images/d3d10-resources-single-element-vb2.png)

Häufig enthält ein Vertex-Puffer alle Daten, die erforderlich sind, um 3D-Vertices vollständig anzugeben. Ein Beispiel hierfür könnte ein Vertex-Puffer sein, der die Position des einzelnen Vertex, die normalen und die Texturkoordinaten enthält. Diese Daten werden in der Regel als Sätze von pro-Vertex-Elementen organisiert, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Scheitelpunkt Puffers, der Positions-, normal-und Textur Daten enthält](images/d3d10-vertex-buffer-element.png)

Dieser Vertex-Puffer enthält pro-Scheitelpunkt Daten für acht Vertices. jeder Scheitelpunkt speichert drei Elemente (Positions-, normal-und Texturkoordinaten). Die Position und normal werden jeweils in der Regel mit 3 32-Bit-Gleit Komma Werten und den Texturkoordinaten mit 2 32-Bit-Gleit Komma Zahlen angegeben.

Für den Zugriff auf Daten aus einem Vertex-Puffer müssen Sie wissen, auf welchen Scheitelpunkt zugegriffen werden soll, und diese anderen Puffer Parameter angeben:

-   *Offset* : die Anzahl der Bytes vom Anfang des Puffers bis zu den Daten für den ersten Scheitelpunkt.
-   *Basevertexlocation* : die Anzahl der Bytes vom Offset bis zum ersten Vertex, der vom entsprechenden zeichnen-Befehl verwendet wird.

Vor dem Erstellen eines Scheitelpunkt Puffers müssen Sie sein Layout definieren, indem Sie ein Eingabe Layoutobjekt erstellen. Nachdem das Eingabe Layout-Objekt erstellt wurde, binden Sie es an die Stufe Eingabe Assembler (IA).

### <a name="span-idindex_bufferspanspan-idindex_bufferspanspan-idindex_bufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Index Puffer

Ein Index Puffer enthält einen sequenziellen Satz von 16-Bit-oder 32-Bit-Indizes. Jeder Index wird zum Identifizieren eines Scheitel Punkts in einem Scheitelpunkt Puffer verwendet. Die Verwendung eines Index Puffers mit einem oder mehreren Scheitel Punkten zum Bereitstellen von Daten für die IA-Phase wird als Indizierung bezeichnet. Ein Index Puffer kann wie in der folgenden Abbildung dargestellt werden.

![Abbildung eines Index Puffers](images/d3d10-index-buffer.png)

Die in einem Index Puffer gespeicherten sequenziellen Indizes befinden sich mit den folgenden Parametern:

-   *Offset* : die Anzahl von Bytes vom Beginn des Puffers bis zum ersten Index.
-   *Startindexlocation* : die Anzahl der Bytes vom Offset bis zum ersten Vertex, der vom entsprechenden zeichnen-Befehl verwendet wird.
-   *IndexCount* : die Anzahl der zu renderenden Indizes.

Ein Index Puffer kann mehrere Zeilen-oder Dreiecks Streifen (primitive Topologien) Durchtrennen der einzelnen Zeilen-oder Dreiecks Streifen ([primitive Topologien](primitive-topologies.md)) zusammenzufügen. Mit einem Strip-Cut-Index können mehrere Zeilen-oder Dreieck Striche mit einem einzelnen zeichnen-Befehl gezeichnet werden. Ein Strip-Ausschneide Index ist der maximal mögliche Wert für den Index (0xFFFF für einen 16-Bit-Index, 0xFFFFFFFF für einen 32-Bit-Index). Der Strip-Cut-Index setzt die aufzurufende Reihenfolge in indizierten primitiven zurück und kann verwendet werden, um den Bedarf an degenerierten Dreiecken zu entfernen, die andernfalls erforderlich sind, um die richtige Sortierreihenfolge in einem Dreiecks Band beizubehalten Die folgende Abbildung zeigt ein Beispiel für einen Index Ausschneide Index.

![Abbildung eines Index Ausschneide Index](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Konstanter Puffer

Direct3D verfügt über einen Puffer zum Bereitstellen von Shader-Konstanten, die als Shader-Konstantenpuffer oder einfach als konstanter Puffer bezeichnet werden. Konzeptionell sieht es wie ein Einzelelement-Vertexpuffer aus, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Shaders-konstantenpuffers](images/d3d10-shader-resource-buffer.png)

Jedes Element speichert eine 1-zu-4-Komponenten Konstante, die durch das Format der gespeicherten Daten bestimmt wird.

Konstantenpuffer reduzieren die erforderliche Bandbreite, um shaderkonstanten zu aktualisieren, indem Sie zulassen, dass Shader-Konstanten gleichzeitig gruppiert und ein Commit ausgeführt werden, anstatt einzelne Aufrufe zum separaten Commit der einzelnen Konstanten zu erstellen.

Ein Shader liest weiterhin Variablen in einem konstanten Puffer direkt über den Variablennamen in derselben Weise, wie Variablen, die nicht Teil eines konstanten Puffers sind, gelesen werden.

Jede Shader-Stufe ermöglicht bis zu 15 Shader-Konstante Puffer. jeder Puffer kann bis zu 4096 Konstanten enthalten.

Verwenden Sie einen konstanten Puffer, um die Ergebnisse der Stream-Output-Phase zu speichern.

Ein Beispiel für das Deklarieren eines konstanten Puffers in einem Shader finden Sie unter [Shader-Konstanten (DirectX HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) .

## <a name="span-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>Textur Ressourcen


Eine Textur Ressource ist eine strukturierte Sammlung von Daten, die zum Speichern von texeln entwickelt wurden. Im Gegensatz zu puffern können Texturen durch Textur-samplz gefiltert werden, wenn Sie von Shader-Einheiten gelesen werden. Der Typ der Textur wirkt sich darauf aus, wie die Textur gefiltert wird. Ein Texel stellt die kleinste Einheit einer Textur dar, die von der Pipeline gelesen oder geschrieben werden kann. Jeder textextetyp enthält 1 bis 4 Komponenten, die in einem der DXGI-Formate angeordnet sind (siehe [**DXGI- \_ Format**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

Texturen werden als strukturierte Ressource erstellt, sodass ihre Größe bekannt ist. Allerdings kann jede Textur zum Zeitpunkt der Ressourcen Erstellung eingegeben oder typlos sein, solange der Typ vollständig mit einer Sicht angegeben wird, wenn die Textur an die Pipeline gebunden ist.

-   [Typen von Texturen](#texture-types)
-   [Unterressourcen](#subresources)
-   [Starke und schwache Typisierung](#typed)

### <a name="span-idtexture_typesspanspan-idtexture_typesspanspan-idtexture_typesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>Textur Typen

Es gibt mehrere Arten von Texturen: 1D, 2D, 3D, von denen jede mit oder ohne Mipmaps erstellt werden kann. Direct3D unterstützt auch Textur Arrays und Multisampling-Texturen.

-   [1D-Textur](#texture1d-resource)
-   [1D-Textur Array](#texture1d-array-resource)
-   [2D-Textur und 2D-Textur Array](#texture2d-resource)
-   [3D-Textur](#texture3d-resource)

### <a name="span-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D-Textur

Eine 1D-Textur in der einfachsten Form enthält Textur Daten, die mit einer einzelnen Textur Koordinate adressiert werden können. Sie kann als Array von Texels visualisiert werden, wie in der folgenden Abbildung dargestellt.

![Abbildung einer 1D-Textur](images/d3d10-1d-texture.png)

Jede Texel enthält eine Reihe von Farbkomponenten, abhängig vom Format der gespeicherten Daten. Wenn Sie weitere Komplexität hinzufügen, können Sie eine 1D-Textur mit MipMap-Ebenen erstellen, wie in der folgenden Abbildung dargestellt.

![Abbildung einer 1D-Textur mit MipMap-Ebenen](images/d3d10-resource-texture1d.png)

Bei einer MipMap-Ebene handelt es sich um eine Textur, bei der es sich um eine Potenz von zwei kleiner als der darüber liegenden Ebene handelt. Die oberste Ebene enthält die meisten Details, jede nachfolgende Ebene ist kleiner. bei einer 1D-MipMap enthält die kleinste Ebene eine texsebene. Die unterschiedlichen Ebenen werden durch einen Index identifiziert, der als "LOD" (Detailstufe) bezeichnet wird. Sie können den Lod verwenden, um auf eine kleinere Textur zuzugreifen, wenn Sie Geometrie rendern, die nicht so nah wie die Kamera ist.

### <a name="span-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D-Textur Array

Direct3D 10 verfügt auch über eine neue Datenstruktur für ein Array von Texturen. Ein Array von 1D-Texturen sieht konzeptionell wie in der folgenden Abbildung aus.

![Abbildung eines 1D-Textur Arrays](images/d3d10-resource-texture1darray.png)

Dieses Textur Array enthält drei Texturen. Jede der drei Texturen hat eine Textur Breite von 5 (die Anzahl der Elemente in der ersten Ebene). Jede Textur enthält auch eine 3-Ebenen-MipMap.

Alle Textur Arrays in Direct3D sind ein homogenes Array von Texturen. Dies bedeutet, dass jede Textur in einem Textur Array das gleiche Datenformat und die gleiche Größe aufweisen muss (einschließlich der Textur Breite und der Anzahl von MipMap-Ebenen). Sie können Textur Arrays mit unterschiedlichen Größen erstellen, sofern alle Texturen in den einzelnen Arrays in der Größe zueinander passen.

### <a name="span-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D-Textur und 2D-Textur Array

Eine Texture2D-Ressource enthält ein 2D-Raster mit texeln. Jedes texttuton ist durch einen u-v-Vektor adressierbar. Da es sich um eine Textur Ressource handelt, kann Sie MipMap-Ebenen und untergeordnete Quellen enthalten. Eine vollständig aufgefüllte 2D-Textur Ressource sieht wie in der folgenden Abbildung aus.

![Abbildung einer 2D-Textur Ressource](images/d3d10-resource-texture2d.png)

Diese Textur Ressource enthält eine einzelne 3x5-Textur mit drei MipMap-Ebenen.

Eine Texture2DArray-Ressource ist ein homogenes Array von 2D-Texturen. Das heißt, jede Textur hat das gleiche Datenformat und die gleichen Dimensionen (einschließlich MipMap-Ebenen). Es verfügt über ein ähnliches Layout wie das 1D-Textur Array, mit dem Unterschied, dass die Texturen nun 2D-Daten enthalten und daher wie in der folgenden Abbildung aussehen.

![Abbildung eines Arrays von 2D-Textur Ressourcen](images/d3d10-resource-texture2darray.png)

Dieses Textur Array enthält drei Texturen. Jede Textur ist 3x5 mit zwei MipMap-Ebenen.

### <a name="span-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Verwenden eines Texture2DArray als Textur Cube

Ein Textur Cube ist ein 2D-Textur Array, das 6 Texturen enthält, eine für jedes Gesicht des Cubes. Ein vollständig aufgefüllter Textur Cube sieht wie die folgende Abbildung aus.

![Abbildung eines Arrays von 2D-Textur Ressourcen, die einen Textur Cube darstellen](images/d3d10-resource-texturecube.png)

Ein 2D-Textur Array, das 6 Texturen enthält, kann aus Shader mit den intrinsischen cubemap-Funktionen gelesen werden, nachdem Sie mit einer cubetextansicht an die Pipeline gebunden wurden. Textur Cubes werden vom Shader mit einem 3D-Vektor adressiert, der aus der Mitte des Textur Cubes zeigt.

### <a name="span-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D-Textur

Eine Texture3D-Ressource (auch als volumetextur bezeichnet) enthält eine 3D-Menge an texeln. Da es sich um eine Textur Ressource handelt, kann Sie MipMap-Ebenen enthalten. Eine vollständig aufgefüllte 3D-Textur sieht wie in der folgenden Abbildung aus.

![Abbildung einer 3D--Textur Ressource](images/d3d10-resource-texture3d.png)

Wenn ein 3D-Textur-MipMap-Slice als renderzielausgabe (mit einer renderzielansicht) gebunden wird, verhält sich die 3D-Textur identisch mit einem 2D-Textur Array mit n Slices. Der jeweilige Rendering-Slice wird aus der Geometrie-Shader-Stufe ausgewählt.

Es gibt kein Konzept eines 3D-Textur Arrays. Daher ist eine 3D-Textur-unter Quelle eine einzelne MipMap-Ebene.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>Unter Ressourcen

Die Direct3D-API verweist auf ganze Ressourcen oder Teilmengen von Ressourcen. Um einen Teil der Ressourcen *anzugeben, hat Direct3D den Begriff unter*Berichte geprägt, d. h. eine Teilmenge einer Ressource.

Ein Puffer ist als einzelne untergeordnete Quelle definiert. Texturen sind etwas komplizierter, da einige verschiedene Textur Typen (1D, 2D usw.) enthalten, von denen einige MipMap-Ebenen und/oder Textur Arrays unterstützen. Beginnend mit dem einfachsten Fall wird eine 1D-Textur als einzelne untergeordnete Quelle definiert, wie in der folgenden Abbildung dargestellt.

![Abbildung einer 1D-Textur](images/d3d10-1d-texture.png)

Dies bedeutet, dass das Array von texeln, die eine 1D-Textur bilden, in einer einzelnen untergeordneten Quelle enthalten ist.

Wenn Sie eine 1D-Textur mit drei MipMap-Ebenen erweitern, kann Sie wie folgt visualisiert werden.

![Abbildung einer 1D-Textur mit MipMap-Ebenen](images/d3d10-resource-texture1d.png)

Stellen Sie sich dies als eine einzelne Textur vor, die aus drei unter Texturen besteht. Jede Unterstruktur wird als untergeordnete Quelle gezählt, sodass diese 1D-Textur drei unter Ressourcen enthält. Eine Unterstruktur (oder untergeordnete Quelle) kann mit der Detailgenauigkeit (ausführlich) für eine einzelne Textur indiziert werden. Wenn Sie ein Array von Texturen verwenden, erfordert der Zugriff auf eine bestimmte Teil Textur sowohl die Lod als auch die bestimmte Textur. Alternativ kombiniert die API diese beiden Informationen in einem einzelnen Null basierten unter Quell Index, wie hier gezeigt.

![Abbildung eines NULL basierten unter Quell Indexes](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselecting_subresourcesspanspan-idselecting_subresourcesspanspan-idselecting_subresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>Auswählen von unter Ressourcen

Einige APIs greifen auf eine gesamte Ressource zu, von denen andere auf einen Teil einer Ressource zugreifen. Die APIs, die auf einen Teil einer Ressource zugreifen, verwenden im Allgemeinen eine Ansichts Beschreibung, um die unter Ressourcen anzugeben, auf die zugegriffen werden soll.

Diese Abbildungen veranschaulichen die Begriffe, die durch eine Ansichts Beschreibung beim Zugriff auf ein Array von Texturen verwendet werden.

### <a name="span-idarray_slicespanspan-idarray_slicespanspan-idarray_slicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>Array Slice

Bei einem Array von Texturen enthält jede Textur mit Mipmaps, ein Array Slice (dargestellt durch das weiße Rechteck), eine Textur und alle zugehörigen unter Texturen, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Array Splice](images/d3d10-resource-array-slice.png)

### <a name="span-idmip_slicespanspan-idmip_slicespanspan-idmip_slicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>MIP-Slice

Ein MIP-Slice (dargestellt durch das weiße Rechteck) enthält eine MipMap-Ebene für jede Textur in einem Array, wie in der folgenden Abbildung dargestellt.

![Abbildung einer MIP-Splice](images/d3d10-resource-mip-slice.png)

### <a name="span-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>Auswählen einer einzelnen untergeordneten Quelle

Sie können diese beiden Arten von Slices verwenden, um eine einzelne untergeordnete Quelle auszuwählen, wie in der folgenden Abbildung dargestellt.

![Abbildung der Auswahl eines unter Berichts mithilfe eines Array Slice und eines MIP-Splice](images/d3d10-resource-subresources-1.png)

### <a name="span-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>Auswählen mehrerer unter Ressourcen

Oder Sie können diese beiden Arten von Slices mit der Anzahl von MipMap-Ebenen und/oder der Anzahl von Texturen verwenden, um mehrere unter Ressourcen auszuwählen.

![Darstellung der Auswahl mehrerer unter Ressourcen](images/d3d10-resource-subresources-2.png)

Unabhängig davon, welchen Textur Typ Sie verwenden, mit oder ohne Mipmaps mit oder ohne ein Textur Array, werden häufig Hilfsfunktionen bereitgestellt, um den Index eines bestimmten unter Berichts zu berechnen.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Starke und schwache Typisierung

Durch das Erstellen einer vollständig typisierten Ressource wird die Ressource auf das Format beschränkt, mit dem Sie erstellt wurde. Dadurch kann die Laufzeit den Zugriff optimieren, insbesondere dann, wenn die Ressource mit Flags erstellt wird, die angeben, dass Sie nicht von der Anwendung zugeordnet werden kann. Ressourcen, die mit einem bestimmten Typ erstellt wurden, können nicht mit dem Ansichts Mechanismus neu interpretiert werden.

In einer typlosen Ressource ist der Datentyp unbekannt, wenn die Ressource erstmalig erstellt wird. Die Anwendung muss aus den verfügbaren typlosen Formaten ausgewählt werden. Sie müssen die Größe des zuzuordnenden Speichers angeben und angeben, ob die Laufzeit die Unterstrukturen in einer MipMap generieren muss.

Allerdings wird das genaue Datenformat (unabhängig davon, ob der Arbeitsspeicher als ganze Zahlen, Gleit Komma Werte, ganze Zahlen ohne Vorzeichen usw. interpretiert wird) erst bestimmt, wenn die Ressource mit einer Ansicht an die Pipeline gebunden ist. Da das Textur Format flexibel bleibt, bis die Textur an die Pipeline gebunden ist, wird die Ressource als schwach typisierter Speicher bezeichnet. Schwach typisierter Speicher hat den Vorteil, dass er wieder verwendet oder erneut interpretiert werden kann (in einem anderen Format), solange das Komponenten Bit des neuen Formats mit der Bitzahl des alten Formats übereinstimmt.

Eine einzelne Ressource kann an mehrere Pipeline Stufen gebunden werden, solange Sie jeweils über eine eindeutige Ansicht verfügen, die die Formate der einzelnen Speicherorte vollständig qualifiziert. Beispielsweise könnte eine Ressource, die mit einem typlosen Format erstellt wurde, als Float-Format und ein uint-Format an verschiedenen Positionen in der Pipeline gleichzeitig verwendet werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ressourcen](resources.md)