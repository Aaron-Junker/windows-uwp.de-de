---
author: mtoepke
title: Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11
description: Bei der ersten Portierungsübung beginnen wir mit den Grundlagen - Umstellen eines einfachen Renderers für einen sich drehenden Würfel mit Vertexschattierungen von OpenGLES2.0 auf Direct3D, damit er der Vorlage DirectX 11-App (Universelle Windows-App) aus Visual Studio2015 entspricht.
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP, Spiele, Opengl, Direct3D 11, Portieren
ms.localizationpriority: medium
ms.openlocfilehash: e7541a8f54f64197c17acea5f1737e36b0e6f670
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2018
ms.locfileid: "5705553"
---
# <a name="port-a-simple-opengl-es-20-renderer-to-direct3d-11"></a>Portieren eines einfachen OpenGL ES 2.0-Renderers zu Direct3D 11



In dieser Portierungsübung beginnen wir mit den Grundlagen: Umstellen eines einfachen Renderers für einen sich drehenden Würfel mit Vertexschattierungen von OpenGLES2.0 auf Direct3D, damit er der Vorlage „DirectX11-App (Universelle Windows-App)“ aus Visual Studio2015 entspricht. Beim Durcharbeiten dieses Portierungsprozesses lernen Sie Folgendes:

-   Portieren einer einfachen Gruppe von Vertexpuffern zu Direct3D-Eingabepuffern
-   Portieren von uniform-Elementen und Attributen zu Konstantenpuffern
-   Konfigurieren von Direct3D-Shaderobjekten
-   Verwenden von HLSL-Semantik bei der Entwicklung von Direct3D-Shadern
-   Portieren einfacher GLSL zu HLSL

Dieses Thema beginnt nach der Erstellung eines neuen DirectX11-Projekts. Informationen zum Erstellen eines neuen DirectX 11-Projekts finden Sie unter [Erstellen eines neuen DirectX 11-Projekts für die Universelle Windows-Plattform (UWP)](user-interface.md).

Das über einen dieser Links erstellte Projekt verfügt über den gesamten vorbereiteten Code für die [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476345)-Infrastruktur. Sie können sofort damit beginnen, den Renderer von OpenGL ES 2.0 zu Direct3D 11 zu portieren.

Es werden zwei Codepfade durchgegangen, mit denen jeweils die gleiche grundlegende Grafikaufgabe durchgeführt wird: das Anzeigen eines sich drehenden Würfels mit Vertexschattierung in einem Fenster. In beiden Fällen wird mit dem Code der folgende Prozess abgedeckt:

1.  Erstellen eines Würfelgitters aus hartcodierten Daten. Dieses Gitter wird als Liste mit Scheitelpunkten (Vertices) dargestellt, wobei jeder Scheitelpunkt (Vertex) über eine Position, einen Normalenvektor und einen Farbvektor verfügt. Dieses Gitter wird in einen Vertexpuffer eingefügt, der von der Schattierungspipeline verarbeitet wird.
2.  Erstellen von Shaderobjekten zum Verarbeiten des Würfelgitters. Es gibt zwei Arten von Shadern: einen Vertex-Shader, der die Scheitelpunkte für die Rasterung verarbeitet, und einen Fragmentshader (oder Pixelshader), mit dem die einzelnen Pixel des Würfels nach der Rasterung eingefärbt werden. Diese Pixel werden zur Anzeige in ein Renderziel geschrieben.
3.  Erstellen der Schattierungssprache, die in den Vertex- bzw. Fragmentshadern jeweils für die Vertex- und Pixelverarbeitung verwendet wird.
4.  Anzeige des gerenderten Würfels auf dem Bildschirm.

![Einfacher OpenGL-Würfel](images/simple-opengl-cube.png)

Nach der Durcharbeitung dieser exemplarischen Vorgehensweise sollten Ihnen die folgenden grundlegenden Unterschiede zwischen OpenGL ES 2.0 und Direct3D 11 bekannt sein:

-   Darstellung der Vertexpuffer und Vertexdaten
-   Prozess der Erstellung und Konfiguration von Shadern
-   Schattierungssprachen und die Ein- und Ausgaben von Shaderobjekten
-   Verhalten beim Zeichnen auf dem Bildschirm

In dieser exemplarischen Vorgehensweise wird auf eine einfache und generische OpenGL-Rendererstruktur verwiesen, die wie folgt definiert ist:

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

Diese Struktur verfügt über eine Instanz und enthält alle erforderlichen Komponenten zum Rendern eines sehr einfach aufgebauten Gitters mit Vertexschattierung.

> **Hinweis:** Any OpenGL ES 2.0-Code in diesem Thema basiert auf der von der Khronos Group bereitgestellten Windows-API-Implementierung und Programmiersyntax Windows C verwendet.

 

## <a name="what-you-need-to-know"></a>Wissenswertes


### <a name="technologies"></a>Technologien

-   [Microsoft Visual C++](http://msdn.microsoft.com/library/vstudio/60k1461a.aspx)
-   OpenGL ES 2.0

### <a name="prerequisites"></a>Voraussetzungen

-   Optional. Sehen Sie sich das Thema [Portieren von EGL-Code zu DXGI und Direct3D](moving-from-egl-to-dxgi.md) an. Lesen Sie sich die Informationen dieses Themas durch, um ein besseres Verständnis der von DirectX bereitgestellten Grafikschnittstelle zu entwickeln.

## <a name="span-idkeylinksstepsheadingspansteps"></a><span id="keylinks_steps_heading"></span>Schritte


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="port-the-shader-config.md">Portieren der Shaderobjekte</a></p></td>
<td align="left"><p>Beim Portieren des einfachen Renderers aus OpenGL ES 2.0 ist der erste Schritt das Einrichten der äquivalenten Vertex- und Fragmentshaderobjekte in Direct3D 11. Stellen Sie außerdem sicher, dass das Hauptprogramm mit den Shaderobjekten kommunizieren kann, nachdem diese kompiliert wurden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-the-vertex-buffers-and-data-config.md">Portieren der Vertexpuffer und -daten</a></p></td>
<td align="left"><p>In diesem Schritt definieren Sie die Vertexpuffer für die Gitter und die Indexpuffer, mit deren Hilfe die Shader die Scheitelpunkte (Vertices) in einer angegebenen Reihenfolge durchlaufen können.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="port-the-glsl.md">Portieren des GLSL-Codes</a></p></td>
<td align="left"><p>Nachdem Sie sich um den Code gekümmert haben, mit dem die Puffer und Shaderobjekte erstellt und konfiguriert werden, muss der in diesen Shadern enthaltene Code von der GL Shader Language (GLSL) von OpenGL ES 2.0 in die High-Level Shader Language (HLSL) von Direct3D 11 portiert werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="draw-to-the-screen.md">Zeichnen auf den Bildschirm</a></p></td>
<td align="left"><p>Schließlich wird der Code portiert, der den sich drehenden Würfel auf den Bildschirm zeichnet.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idadditionalresourcesspanadditional-resources"></a><span id="additional_resources"></span>Weitere Ressourcen


-   [Vorbereiten der Entwicklungsumgebung für die Entwicklung von UWP-DirectX-Spielen](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [Erstellen eines neuen DirectX11-Projekts für UWP](user-interface.md)
-   [Zuordnen von OpenGLES2.0-Konzepten und -Infrastruktur zu Direct3D11](map-concepts-and-infrastructure.md)

 

 




