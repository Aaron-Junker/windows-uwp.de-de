---
title: Exemplarische Vorgehensweise--Direct3D 11 schattenvolumenpuffer
description: In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Schattenvolumen mit Tiefenkarten unter Verwendung von Direct3D 11 auf Geräten aller Direct3D-Funktionsebenen rendern.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, schattenvolumes, Tiefe Puffer, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: e2b54f179d61a9479bdb921ef0a68b4c1772c20c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159384"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Exemplarische Vorgehensweise - Implementieren von Schattenvolumen mit Tiefenpuffern in Direct3D 11



In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Schattenvolumen mit Tiefenkarten unter Verwendung von Direct3D 11 auf Geräten aller Direct3D-Funktionsebenen rendern.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Erstellen von Tiefenpuffer-Geräteressourcen</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die zum Unterstützen von Tiefentests für Schattenvolumen erforderlichen Direct3D-Geräteressourcen erstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Rendern der Schattenmap zum Tiefenpuffer</a></p></td>
<td align="left"><p>Führen Sie das Rendern aus dem Blickwinkel durch, aus dem das Licht kommt, um eine zweidimensionale Tiefenkarte zu erstellen, mit der das Schattenvolumen dargestellt wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Rendern der Szene mit Tiefentest</a></p></td>
<td align="left"><p>Erstellen Sie einen Schatteneffekt, indem Sie dem Vertex-Shader (bzw. Geometry-Shader) und dem Pixel-Shader einen Tiefentest hinzufügen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Unterstützen von Schattenmaps für unterschiedliche Hardware</a></p></td>
<td align="left"><p>Rendern Sie Schatten in noch besserer Qualität auf schnelleren Geräten und schnellere Schatten auf weniger leistungsfähigen Geräten.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Schattenabbildung – Portieren von Direct3D 9-Desktop-Apps


Windows 8 ADDE d-tiefe Vergleichsfunktion mit Featureebene 9 \_ 1 und 9 \_ 3. Jetzt können Sie Renderingcode mit Schattenvolumen zu DirectX 11 migrieren. Der Direct3D 11-Renderer ist abwärtskompatibel mit Geräten der Featureebene 9. In dieser exemplarischen Vorgehensweise zeigen wir, wie herkömmliche Schattenvolumen mit Tiefentests in Direct3D 11-Apps oder -Spielen implementiert werden können. Der Code umfasst die folgenden Prozesse:

1.  Erstellen von Direct3D-Geräteressourcen für die Schattenabbildung
2.  Hinzufügen eines Renderingdurchgangs zum Erstellen der Tiefenkarte
3.  Hinzufügen von Tiefentests zum Hauptrenderingdurchgang
4.  Implementieren des erforderlichen Shadercodes
5.  Optionen für schnelles Rendering auf kompatibler Hardware

Nachdem Sie diese exemplarische Vorgehensweise durchgearbeitet haben, sollten Sie sich mit der Implementierung eines grundlegenden kompatiblen schattenvolumeverfahrens in Direct3D 11 vertraut machen, das mit der Funktionsebene 9 \_ 1 und höher kompatibel ist.

## <a name="prerequisites"></a>Voraussetzungen


Führen Sie die Schritte unter [Vorbereiten der Entwicklungsumgebung für die Entwicklung von Spielen für die universelle Windows-Plattform (UWP) und DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md) aus. Sie benötigen noch keine Vorlage, aber Microsoft Visual Studio 2015, um das Codebeispiel für diese exemplarische Vorgehensweise erstellen zu können.

## <a name="related-topics"></a>Zugehörige Themen


**Direct3D**

* [Schreiben von HLSL-Shadern in Direct3D 9](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [Erstellen eines neuen DirectX 11-Projekts für die UWP](user-interface.md)

**Technische Artikel zur Schattenabbildung**

* [Allgemeine Artikel zum Verbessern von Tiefenkarten für Schatten](/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)
* [Überlappende Schattenkarten](/windows/desktop/DxTechArts/cascaded-shadow-maps)

 

 