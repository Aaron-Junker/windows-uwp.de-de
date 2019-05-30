---
title: Exemplarische Vorgehensweise -- Tiefenpuffer für Schattenvolumen in Direct3D 11
description: In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Schattenvolumen mit Tiefenkarten unter Verwendung von Direct3D 11 auf Geräten aller Direct3D-Funktionsebenen rendern.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX, Schattenvolumen, Tiefenpuffer, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: 2ce0cbd310ea89c5fa7b5c68033402f559768a24
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368510"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Exemplarische Vorgehensweise: Implementieren Sie Schattenvolumen, die mithilfe von Tiefenpuffern in Direct3D 11



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
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Erstellen von Tiefe Geräteressourcen Puffer</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die zum Unterstützen von Tiefentests für Schattenvolumen erforderlichen Direct3D-Geräteressourcen erstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Die Volumeschattenkopie-Karte, um die Tiefenpuffer Rendern</a></p></td>
<td align="left"><p>Führen Sie das Rendern aus dem Blickwinkel durch, aus dem das Licht kommt, um eine zweidimensionale Tiefenkarte zu erstellen, mit der das Schattenvolumen dargestellt wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Rendern der Szene mit Tiefe testen</a></p></td>
<td align="left"><p>Erstellen Sie einen Schatteneffekt, indem Sie dem Vertex-Shader (bzw. Geometry-Shader) und dem Pixel-Shader einen Tiefentest hinzufügen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Unterstützen Sie Schatten-Zuordnungen für eine Reihe von hardware</a></p></td>
<td align="left"><p>Rendern Sie Schatten in noch besserer Qualität auf schnelleren Geräten und schnellere Schatten auf weniger leistungsfähigen Geräten.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Schattenabbildung – Portieren von Direct3D 9-Desktop-Apps


Windows 8-Adde d Tiefe Vergleichsfunktion auf Funktionsebene 9\_1 und 9\_3. Jetzt können Sie Renderingcode mit Schattenvolumen zu DirectX 11 migrieren. Der Direct3D 11-Renderer ist abwärtskompatibel mit Geräten der Featureebene 9. In dieser exemplarischen Vorgehensweise zeigen wir, wie herkömmliche Schattenvolumen mit Tiefentests in Direct3D 11-Apps oder -Spielen implementiert werden können. Der Code umfasst die folgenden Prozesse:

1.  Erstellen von Direct3D-Geräteressourcen für die Schattenabbildung
2.  Hinzufügen eines Renderingdurchgangs zum Erstellen der Tiefenkarte
3.  Hinzufügen von Tiefentests zum Hauptrenderingdurchgang
4.  Implementieren des erforderlichen Shadercodes
5.  Optionen für schnelles Rendering auf kompatibler Hardware

Nach Abschluss dieser exemplarischen Vorgehensweise, werden Sie wissen, wie eine grundlegende kompatibel Schatten Volume Technik in Direct3D 11 zu implementieren, die kompatibel mit der Funktionsebene 9 ist\_1 und höher.

## <a name="prerequisites"></a>Vorraussetzungen


Führen Sie die Schritte unter [Vorbereiten der Entwicklungsumgebung für die Entwicklung von Spielen für die universelle Windows-Plattform (UWP) und DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md) aus. Ist nicht erforderlich, eine Vorlage noch, jedoch benötigen Sie Microsoft Visual Studio 2015, um das Codebeispiel in dieser exemplarischen Vorgehensweise erstellen.

## <a name="related-topics"></a>Verwandte Themen


**Direct3D**

* [Schreiben von HLSL-Shadern in Direct3D 9](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [Erstellen Sie ein neues DirectX 11-Projekt für UWP](user-interface.md)

**Volumeschattenkopie-Zuordnung – technische Artikel**

* [Allgemeine Techniken zur Verbesserung der Volumeschattenkopie-Depth-Zuordnungen](https://docs.microsoft.com/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)
* [Kaskadierenden Shadow-Zuordnungen](https://docs.microsoft.com/windows/desktop/DxTechArts/cascaded-shadow-maps)

 

 




