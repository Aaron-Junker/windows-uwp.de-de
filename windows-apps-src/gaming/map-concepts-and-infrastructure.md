---
title: Zuordnen von OpenGL ES 2.0 zu Direct3D 11
description: Machen Sie sich zu Beginn des Prozesses zur ersten Portierung Ihrer Grafikarchitektur von OpenGL ES 2.0 zu Direct3D mit den Hauptunterschieden zwischen den APIs vertraut.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, OpenGL, Direct3D, Portieren
ms.localizationpriority: medium
ms.openlocfilehash: e09dcb1830e62d17983f564771b4808d132179a0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640825"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Zuordnen von OpenGL ES 2.0 zu Direct3D 11



Machen Sie sich zu Beginn des Prozesses zur ersten Portierung Ihrer Grafikarchitektur von OpenGL ES 2.0 zu Direct3D mit den Hauptunterschieden zwischen den APIs vertraut. Mithilfe der Themen in diesem Abschnitt können Sie die Portierungsstrategie und die API-Änderungen planen, die erforderlich sind, wenn Sie die Grafikverarbeitung auf Direct3D umstellen.
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
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">Planen Sie Ihre Portieren von OpenGL ES 2.0 zu Direct3D</a></p></td>
<td align="left"><p>Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGL ES 2.0 investiert. Beim Vorbereiten der Portierung Ihrer Grafikpipeline-Codebasis zu Direct3D 11 und der Windows-Runtime sind im Vorfeld einige Dinge zu beachten.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">Vergleichen Sie EGL-Code DXGI und Direct3D</a></p></td>
<td align="left"><p>Die DirectX-Grafikschnittstelle (DXGI) und verschiedene Direct3D-APIs erfüllen die gleiche Rolle wie EGL. In diesem Thema werden die DXGI und Direct3D 11 aus Sicht von EGL erläutert.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">Vergleichen von Puffern, Uniformen und Vertex-Attribute zu Direct3D OpenGL ES 2.0</a></p></td>
<td align="left"><p>Während des Portierens zu Direct3D 11 aus OpenGL ES 2.0 müssen Sie die Syntax und das API-Verhalten zum Übergeben von Daten zwischen der App und den Shaderprogrammen ändern.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">Vergleichen Sie die OpenGL ES 2.0-Shader-Pipeline, um Direct3D</a></p></td>
<td align="left"><p>Vom Konzept her ist die Direct3D 11-Shaderpipeline der in OpenGL ES 2.0 sehr ähnlich. Hinsichtlich des API-Entwurfs sind die Hauptkomponenten für die Erstellung und Verwaltung der Shaderstufen jedoch Teile der zwei primären Schnittstellen <a href="https://msdn.microsoft.com/library/windows/desktop/hh404575"><strong>ID3D11Device1</strong></a> und <a href="https://msdn.microsoft.com/library/windows/desktop/hh404598"><strong>ID3D11DeviceContext1</strong></a>. In diesem Thema versuchen wir, allgemeine Muster der OpenGL ES 2.0-Shaderpipeline-API den Direct3D 11-Entsprechungen in diesen Schnittstellen zuzuordnen.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>Hinweise zu bestimmten OpenGL ES 2.0-Anbietern


In diesen Themen wird die OpenGL ES 2.0-Spezifikation von Khronos mit der plattformagnostischen Programmiersprache C verwendet. Sowohl von iOS als auch von Android wird dieselbe Spezifikation genutzt, und der für diese Plattformen entwickelte OpenGL ES 2.0-Code weist große Ähnlichkeit mit den Codeausschnitten auf, die wir durchgehen werden, obwohl diese meist als objektorientierte APIs verfügbar gemacht werden. Aufgrund der Eigenheiten und Sprachunterschiede der einzelnen Plattformen können geringe Unterschiede bestehen, vor allem bei den Typen der Methodenparameter oder bei der allgemeinen Sprachsyntax. Für iOS wird beispielsweise Objective-C genutzt. Für Android kann C++ verwendet werden. Einige Entwickler verlassen sich jedoch möglicherweise auf eine reine Java-Implementierung. Trotz dieser Unterschiede sind diese Themen hilfreich, da bei den allgemeinen Konzepten, dem Aufbau und der Verwendung der OpenGL ES-APIs keine Abweichungen bestehen.

 

 




