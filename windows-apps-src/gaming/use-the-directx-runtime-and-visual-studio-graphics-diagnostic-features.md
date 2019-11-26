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

Die Grafik Diagnosetools sind in Windows 10 als optionales Feature verfügbar. Installieren Sie die optionale Grafik Tools-Funktion, um die Grafik Diagnose Features (in der Runtime und in Visual Studio bereitgestellt) zu verwenden, um DirectX-Apps oder-Spiele zu entwickeln.

1. Wechseln Sie zu **Einstellungen** > **apps** > **Apps & Features/optionale Features**.
2. Wenn **Grafik Tools** bereits unter **installierte Funktionen**aufgeführt sind, sind Sie fertig. Klicken Sie andernfalls auf **Funktion hinzufügen**.
3. Suchen Sie nach **Grafik Tools**, und wählen Sie Sie aus, und klicken Sie dann auf **Installieren**.

Die Grafikdiagnosefeatures ermöglichen das Erstellen von Direct3D-Debugging-Geräten (über Direct3D-SDK-Ebenen) in der DirectX-Runtime sowie das Grafikdebugging, die Frame-Analyse und die GPU-Verwendung.

-   Mit dem Grafikdebugging können Sie Direct3D-Aufrufe Ihrer App nachverfolgen. Anschließend können Sie diese Aufrufe wiedergeben, Parameter untersuchen, Shader debuggen und mit ihnen experimentieren sowie Grafikobjekte visualisieren, um Renderingprobleme zu diagnostizieren. Sie können Protokolle auf Windows-PCs, Simulatoren oder Geräten erstellen und diese dann auf unterschiedlicher Hardware wiedergeben.
-   In Visual Studio wird Grafikframe-Analyse in einem Grafik Debugprotokoll ausgeführt, und es werden grundlegende Zeitvorgaben für die Direct3D Draw-Aufrufe gesammelt. Anschließend führt er eine Reihe von Experimenten durch, indem er verschiedene Grafikeinstellungen ändert und eine Tabelle mit Zeit Steuerungs Ergebnissen erzeugt. Diese Daten können Aufschluss über Probleme mit der Grafikleistung Ihrer App geben, und anhand der Ergebnisse der verschiedenen Versuche können Sie feststellen, wie sich die Leistung verbessern lässt.
-   Anhand der GPU-Verwendung in Visual Studio können Sie die GPU-Verwendung in Echtzeit überwachen. Es sammelt und analysiert die zeitlichen Daten der von der CPU und GPU verarbeiteten Workloads, sodass Sie ermitteln können, wo Engpässe vorliegen.

## <a name="related-topics"></a>Verwandte Themen

[Übersicht über Grafikdiagnose in Visual Studio](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)