---
author: PatrickFarley
Description: Describes the structure of the 3D Manufacturing Format file type and how it can be created and manipulated with the Windows.Graphics.Printing3D API.
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Generieren eines 3MF-Pakets
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9d7acfdb6312770f51d8870549218344ad8c4330
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "6670974"
---
# <a name="generate-a-3mf-package"></a>Generieren eines 3MF-Pakets

**Wichtige APIs**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx)

Dieses Handbuch beschreibt die Struktur des 3D Manufacturing Format-Dokuments und wie dieses mit der [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx)-API erstellt und bearbeitet werden kann.

## <a name="what-is-3mf"></a>Was ist 3MF?

Das 3D Manufacturing Format ist ein Satz von Konventionen für die Verwendung von XML, um die Darstellung und Struktur von 3D-Modellen zu beschreiben, die für die Fertigung eingesetzt werden (3D-Druck). Es definiert einen Satz von Teilen (von denen einige erforderlich und andere optional sind) und ihre Beziehungen. Ziel dabei ist es, alle erforderlichen Informationen für ein 3D-Fertigungsgerät bereitzustellen. Ein Datensatz, der dem 3D Manufacturing Format entspricht, kann als Datei mit der Erweiterung „.3mf“ gespeichert werden.

In Windows 10 die [**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.aspx) -Klasse im Namespace **Windows.Graphics.Printing3D** ist vergleichbar mit einer einzelnen 3mf-Datei und andere Klassen entsprechen den spezifischen XML-Elementen in der Datei. In diesem Handbuch wird beschrieben, wie die einzelnen Hauptbestandteile eines 3MF-Dokuments erstellt und programmgesteuert festgelegt werden können, wie die 3MF Materials-Erweiterung genutzt werden kann und wie ein **Printing3D3MFPackage**-Objekt konvertiert und als 3MF-Datei gespeichert werden kann. Weitere Informationen zu den Standards der 3MF- oder der 3MF Materials-Erweiterung finden Sie in der [3MF-Spezifikation](http://3mf.io/what-is-3mf/3mf-specification/).

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>Hauptklassen in der 3MF-Struktur

Die **Printing3D3MFPackage**-Klasse stellt ein vollständiges 3MF-Dokument dar, und den Kern eines 3MF-Dokuments bildet der Modellbestandteil, der durch die [**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmodel.aspx)-Klasse dargestellt wird. Der Großteil der Informationen, die zu einem 3D-Modell angegeben werden, werden gespeichert, indem die Eigenschaften der **Printing3DModel**-Klasse und die Eigenschaften der zugrunde liegenden Klassen festgelegt werden.

[!code-cs[InitClasses](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetInitClasses)]

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>Metadaten

Der Modellteil eines 3MF-Dokuments kann Metadaten in Form von Schlüssel-Wert-Paaren von Zeichenfolgen enthalten, die in der **Metadata**-Eigenschaft gespeichert sind. Es gibt eine Reihe von vordefinierten Metadatennamen, aber es können weitere Paare als Teil einer Erweiterung hinzugefügt werden (dies wird in der [3MF-Spezifikation](http://3mf.io/what-is-3mf/3mf-specification/) ausführlicher beschrieben). Der Empfänger des Pakets (ein 3D-Fertigungsgerät) bestimmt, ob und wie Metadaten behandelt werden. Es empfiehlt sich jedoch, möglichst viele grundlegende Informationen im 3MF-Paket einzuschließen:

[!code-cs[Metadata](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMetadata)]

## <a name="mesh-data"></a>Gitterdaten

Im Kontext dieses Handbuchs bezeichnet ein Gitter einen Körper mit dreidimensionaler Geometrie, der anhand eines einzelnen Satzes von Scheitelpunkten erstellt wird (obwohl das Gitter nicht zwangsläufig als Festkörper angezeigt werden muss). Ein Gitterteil wird durch die [**Printing3DMesh**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmesh.aspx)-Klasse dargestellt. Ein gültiges Gitterobjekt muss Informationen über die Lage aller Scheitelpunkte sowie alle Dreiecksoberflächen enthalten, die zwischen bestimmten Scheitelpunktsätzen vorhanden sind.

Mit der folgenden Methode werden einem Gitter Scheitelpunkte hinzugefügt. Diesen Scheitelpunkten wird dann im 3D-Raum eine Lage zugewiesen:

[!code-cs[Vertices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetVertices)]

Die nächste Methode definiert alle Dreiecke, die über diese Scheitelpunkte gezeichnet werden sollen:

[!code-cs[TriangleIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTriangleIndices)]

> [!NOTE]
> Die Indizes aller Dreiecke müssen gegen den Uhrzeigersinn definiert werden (bei Sicht auf das Dreieck von außerhalb des Gitterobjekts), sodass ihre Oberflächennormalvektoren nach außen zeigen.

Wenn ein Printing3DMesh-Objekt gültige Sätze von Scheitelpunkten und Dreiecken enthält, sollte es anschließend der **Meshes**-Eigenschaft des Modells hinzugefügt werden. Alle **Printing3DMesh**-Objekte in einem Paket müssen unter der **Meshes**-Eigenschaft der **Printing3DModel**-Klasse gespeichert werden.

[!code-cs[MeshAdd](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMeshAdd)]


## <a name="create-materials"></a>Erstellen von Materialen


Ein 3D-Modell kann Daten für mehrere Materialien enthalten. Diese Konvention ist für 3D-Fertigungsgeräte konzipiert, die mehrere Materialien für einen einzelnen Druckauftrag verwenden können. Es gibt auch mehrere *Typen* von Materialgruppen, die jeweils eine Reihe verschiedener einzelner Materialien unterstützen können. Jede Materialgruppe muss eine eindeutige Referenz-ID aufweisen, und jedes Material innerhalb einer Gruppe muss ebenfalls über eine eindeutige ID verfügen.

Die verschiedenen Gitterobjekte in einem Modell können dann auf diese Materialien verweisen. Darüber hinaus können einzelne Dreiecke in jedem Gitter unterschiedliche Materialien angeben. Verschiedene Materialien können sogar innerhalb eines einzelnen Dreiecks dargestellt werden, wobei jedem Dreieckvertex ein anderes Material zugewiesen ist und das Oberflächenmaterial als der Verlauf dazwischen berechnet wird.

In diesem Handbuch wird zunächst beschrieben, wie verschiedene Materialarten innerhalb ihrer jeweiligen Materialgruppen erstellt und als Ressourcen für das Modellobjekt gespeichert werden. Anschließend wird erläutert, wie verschiedene Materialien einzelnen Gittern und Dreiecken zugewiesen werden.

### <a name="base-materials"></a>Basismaterialien

Der Standardmaterialtyp ist **Base Material**, der sowohl einen **Color Material**-Wert (nachfolgend beschrieben) als auch ein Namensattribut aufweist, mit dem der *Typ* des zu verwendenden Materials angegeben werden soll.

[!code-cs[BaseMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetBaseMaterialGroup)]

> [!NOTE]
> Das 3D-Fertigungsgerät bestimmt, welche verfügbaren physischen Materialien den jeweiligen im 3MF gespeicherten virtuellen Materialelementen zugeordnet werden. Die Materialzuordnung muss nicht 1:1 erfolgen: Wenn ein 3D-Drucker nur ein Material verwendet, wird das gesamte Modell in diesem Material gedruckt, unabhängig davon, welchen Objekten oder Oberflächen verschiedene Materialien zugeordnet wurden.

### <a name="color-materials"></a>Farbmaterialien

**Farbmaterialien** ähneln den **Basismaterialien**, enthalten jedoch keinen Namen. Somit enthalten sie keine Anweisungen, welche Materialart vom Gerät verwendet werden soll. Sie enthalten lediglich Farbdaten und überlassen dem Gerät die Auswahl des Materialtyps (das Gerät kann anschließend den Benutzer zur Auswahl auffordern). Im folgenden Code werden die `colrMat`-Objekte aus der vorherigen Methode alleine verwendet.

[!code-cs[ColorMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetColorMaterialGroup)]

### <a name="composite-materials"></a>Zusammengesetzte Materialien

**Zusammengesetzte Materialien** weisen das Fertigungsgerät an, eine einheitliche Mischung aus unterschiedlichen **Basismaterialien** zu verwenden. Jede **zusammengesetzte Materialgruppe** muss auf genau eine **Basismaterialgruppe** verweisen, aus der die Bestandteile entnommen werden sollen. Zusätzlich müssen die **Basismaterialien** in dieser Gruppe, die verfügbar gemacht werden sollen, in einer Liste mit den **Materialindizes** aufgeführt werden, auf die anschließend jedes **zusammengesetzte Material** verweist, wenn die Verhältnisse angeben werden (jedes **zusammengesetzte Material** ist lediglich ein Verhältnis der **Basismaterialien**).

[!code-cs[CompositeMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetCompositeMaterialGroup)]

### <a name="texture-coordinate-materials"></a>Texturkoordinatenmaterialien

3MF unterstützt die Verwendung von 2D-Bildern, um die Oberflächen von 3D-Modellen einzufärben. Auf diese Weise kann das Modell deutlich mehr Farbdaten pro Dreiecksfläche übermitteln (als bei Verwendung eines einzelnen Farbwerts pro Dreiecksvertex). Ebenso wie **Farbmaterialien** können Texturkoordinatenmaterialien ausschließlich Farbdaten enthalten. Um eine 2D-Textur verwenden zu können, muss zunächst eine Texturressource deklariert werden:

[!code-cs[TextureResource](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTextureResource)]

> [!NOTE]
> Texturdaten gehören zum 3MF-Paket selbst und nicht zum Modellteil innerhalb des Pakets.

Als Nächstes müssen die **Texture3Coord-Materialien** angegeben werden. Jedes dieser Materialien verweist auf eine Texturressource und gibt einen bestimmten Punkt im Bild an (in UV-Koordinaten).

[!code-cs[Texture2CoordMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTexture2CoordMaterialGroup)]

## <a name="map-materials-to-faces"></a>Zuordnung von Materialien zu Oberflächen

Um festzulegen, welche Materialien welchen Scheitelpunkten der einzelnen Dreiecke zugeordnet werden sollen, wird noch einmal das Gitterobjekt für das Modell herangezogen (wenn ein Modell mehrere Gitter enthält, müssen die jeweiligen Materialien separat zugewiesen werden). Wie bereits erwähnt, werden die Materialien pro Vertex, pro Dreieck zugewiesen. Der nachfolgende Code veranschaulicht, wie diese Informationen eingegeben und interpretiert werden.

[!code-cs[MaterialIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMaterialIndices)]

## <a name="components-and-build"></a>Komponenten und Erstellung

Die Komponentenstruktur ermöglicht es dem Benutzer, mehrere Gitterobjekte in einem druckbaren 3D-Modell zu platzieren. Ein [**Printing3DComponent**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponent.aspx)-Objekt enthält ein einzelnes Gitter und eine Liste der Verweise auf andere Komponenten. Hierbei handelt es sich eigentlich um eine Liste der [**Printing3DComponentWithMatrix**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponentwithmatrix.aspx)-Objekte. **Printing3DComponentWithMatrix**-Objekte enthalten jeweils eine **Printing3DComponent** und insbesondere eine Transformationsmatrix, die auf das Gitter und die enthaltenen Komponenten der **Printing3DComponent** angewendet wird.

Das Modell eines Autos könnte zum Beispiel aus einer „Karosserie“-**Printing3DComponent** bestehen, die das Gitter für die Karosserie enthält. Die „Karosserie“-Komponente kann dann Verweise auf vier verschiedene **Printing3DComponentWithMatrix**-Objekte enthalten, die alle auf dieselbe **Printing3DComponent** mit dem Gitter „Reifen“ verweisen und vier verschiedene Transformationsmatrizen enthalten (die die Reifen vier verschiedenen Positionen an der Karosserie zuordnen). In diesem Szenario müssen die Gitter „Karosserie“ und „Reifen“ nur jeweils einmal gespeichert werden, obwohl das endgültige Produkt insgesamt fünf Gitter umfassen würde.

Auf alle **Printing3DComponent**-Objekte muss direkt in der **Components**-Eigenschaft des Modells verwiesen werden. Die spezifische Komponente, die für den Druckauftrag verwendet werden soll, wird in der **Build**-Eigenschaft gespeichert.

[!code-cs[Components](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetComponents)]

## <a name="save-package"></a>Speichern des Pakets
Das vorliegende Modell mit den definierten Materialien und Komponenten kann nun im Paket gespeichert werden.

[!code-cs[SavePackage](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSavePackage)]

Jetzt kann ein Druckauftrag in der App gestartet (siehe [3D-Drucken in der App](https://msdn.microsoft.com/library/windows/apps/mt204541.aspx)) oder das **Printing3D3MFPackage** als 3MF-Datei gespeichert werden.

Die folgende Methode akzeptiert ein fertiges **Printing3D3MFPackage** und speichert die Daten in einer 3MF-Datei.

[!code-cs[SaveTo3mf](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSaveTo3mf)]

## <a name="related-topics"></a>Verwandte Themen

[3D-Druck über Ihre App](https://msdn.microsoft.com/windows/uwp/devices-sensors/3d-print-from-app)  
[Beispiel für 3D-Druck– UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 
