---
title: Grundlegendes zu 3D-Grafiken für DirectX-Spiele
description: Im Folgenden zeigen wir Ihnen, wie Sie grundlegende Konzepte von 3D-Grafiken durch die Programmierung mit DirectX umsetzen können.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, Grafiken
ms.localizationpriority: medium
ms.openlocfilehash: 68ee694c036d4bc92f76ea75f7c5e5e530e26334
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173194"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Grundlegendes zu 3D-Grafiken für DirectX-Spiele



Im Folgenden zeigen wir Ihnen, wie Sie grundlegende Konzepte von 3D-Grafiken durch die Programmierung mit DirectX umsetzen können.

**Ziel:** Lernen Sie, eine 3D-Grafik-App zu programmieren.

## <a name="prerequisites"></a>Voraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

**Gesamter Zeitaufwand:** 30 Minuten

## <a name="where-to-go-from-here"></a>Weiterführende Informationen


Hier wird erläutert, wie 3D-Grafiken mit DirectX und C++ CX entwickelt werden \\ . In diesem fünfteiligen Lernprogramm werden die [Direct3D](/windows/desktop/direct3d)-API sowie die Konzepte und der Code vorgestellt, die auch in zahlreichen anderen DirectX-Beispielen zum Einsatz kommen. Die einzelnen Teile bauen aufeinander auf. Sie behandeln u. a. das Konfigurieren von DirectX für Ihre UWP-App mit C++ sowie Grundtypen mit Texturen und das Hinzufügen von Effekten.

> **Hinweis**    In diesem Tutorial wird ein rechts händiges Koordinatensystem mit Spalten Vektoren verwendet. Bei vielen DirectX-Beispielen und -Apps wird ein linkshändiges Koordinatensystem mit Zeilenvektoren verwendet. Für eine umfangreichere mathematische Grafiklösung, die ein linkshändiges Koordinatensystem mit Zeilenvektoren unterstützt, sollten Sie [DirectXMath](/windows/desktop/dxmath/directxmath-portal) verwenden. Weitere Informationen finden Sie unter [Verwenden von DirectXMath mit Direct3D](/windows/desktop/dxmath/pg-xnamath-migration-d3dx).

 

Folgende Inhalte werden behandelt:

-   Initialisieren von [Direct3D](/windows/desktop/direct3d)-Schnittstellen mit der Windows-Runtime
-   Anwenden von Vertex-Shader-Operationen
-   Einrichten der Geometrie
-   Rastern der Szene (Glätten der 3D-Szene auf eine 2D-Projektion)
-   Culling verborgener Oberflächen

> **Hinweis**  

 

Als Nächstes erstellen wir ein Direct3D-Gerät, eine Swapchain sowie eine Renderingzielansicht und stellen das gerenderte Bild auf dem Display dar.

[Schnellstart: Einrichten von DirectX-Ressourcen und Anzeigen eines Bilds](setting-up-directx-resources.md)

## <a name="related-topics"></a>Zugehörige Themen


* [Direct3D 11-Grafik](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 