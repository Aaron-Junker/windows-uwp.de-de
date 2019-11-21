---
title: Tools für die Grafikdiagnose
description: Hier erfahren Sie, wie Sie in Visual Studio die Grafikdiagnosefeatures, einschließlich Grafikdebugging, Analyse von Grafikframes und GPU-Verwendung, abrufen und verwenden.
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.date: 11/20/2019
ms.topic: article
keywords: Windows 10, UWP, Spiele, Grafiken, Diagnose, Tools, directx
ms.localizationpriority: medium
ms.openlocfilehash: 14711a356f00ac027bf990554c90547017b9cc4d
ms.sourcegitcommit: f464e5157ca22cfe31075f2f1219b8227587bf03
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263096"
---
# <a name="graphics-diagnostics-tools"></a>Tools für die Grafikdiagnose

The graphics diagnostic tools are available from within Windows 10 as an optional feature. To use the graphics diagnostic features (provided in the runtime and Visual Studio) to develop DirectX apps or games, install the optional Graphics Tools feature.

1. Go to **Settings** > **Apps** > **Apps & features/Optional features**.
2. If **Graphics Tools** is already listed under **Installed features**, then you're done. Otherwise, click **Add a feature**.
3. Search for and/or select **Graphics Tools**, and then click **Install**.

Die Grafikdiagnosefeatures ermöglichen das Erstellen von Direct3D-Debugging-Geräten (über Direct3D-SDK-Ebenen) in der DirectX-Runtime sowie das Grafikdebugging, die Frame-Analyse und die GPU-Verwendung.

-   Mit dem Grafikdebugging können Sie Direct3D-Aufrufe Ihrer App nachverfolgen. Anschließend können Sie diese Aufrufe wiedergeben, Parameter untersuchen, Shader debuggen und mit ihnen experimentieren sowie Grafikobjekte visualisieren, um Renderingprobleme zu diagnostizieren. You can take logs on Windows PCs, simulators, or devices, and then play them back on different hardware.
-   Graphics Frame Analysis in Visual Studio runs on a graphics debugging log, and gathers baseline timing for the Direct3D draw calls. It then performs a set of experiments by modifying various graphics settings, and produces a table of timing results. Diese Daten können Aufschluss über Probleme mit der Grafikleistung Ihrer App geben, und anhand der Ergebnisse der verschiedenen Versuche können Sie feststellen, wie sich die Leistung verbessern lässt.
-   Anhand der GPU-Verwendung in Visual Studio können Sie die GPU-Verwendung in Echtzeit überwachen. It collects and analyzes the timing data of the workloads being handled by the CPU and GPU, so that you can determine where any bottlenecks are.

## <a name="related-topics"></a>Verwandte Themen

[Graphics Diagnostics Overview in Visual Studio](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)