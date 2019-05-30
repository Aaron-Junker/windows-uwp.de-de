---
title: Grundlegendes zu 3D-Grafiken für DirectX-Spiele
description: Im Folgenden zeigen wir Ihnen, wie Sie grundlegende Konzepte von 3D-Grafiken durch die Programmierung mit DirectX umsetzen können.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX, Grafiken
ms.localizationpriority: medium
ms.openlocfilehash: 556c5c74e5c8284e56047b4b8a9b2c2bf9c0155c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369072"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Grundlegendes zu 3D-Grafiken für DirectX-Spiele



Im Folgenden zeigen wir Ihnen, wie Sie grundlegende Konzepte von 3D-Grafiken durch die Programmierung mit DirectX umsetzen können.

**Ziel:** Erfahren Sie, eine app 3D-Grafiken zu programmieren.

## <a name="prerequisites"></a>Vorraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

**Die Gesamtzeit zum Abschließen:** 30 Minuten.

## <a name="where-to-go-from-here"></a>Weitere Informationen


Hier sprechen wir von 3D-Grafiken mit DirectX und C++ entwickeln\\Cx. In diesem fünfteiligen Lernprogramm werden die [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d)-API sowie die Konzepte und der Code vorgestellt, die auch in zahlreichen anderen DirectX-Beispielen zum Einsatz kommen. Die einzelnen Teile bauen aufeinander auf. Sie behandeln u. a. das Konfigurieren von DirectX für Ihre UWP-App mit C++ sowie Grundtypen mit Texturen und das Hinzufügen von Effekten.

> **Beachten Sie**  dieses Tutorial verwendet ein rechtshändiges Koordinatensystem mit Spaltenvektoren. Bei vielen DirectX-Beispielen und -Apps wird ein linkshändiges Koordinatensystem mit Zeilenvektoren verwendet. Für eine umfangreichere mathematische Grafiklösung, die ein linkshändiges Koordinatensystem mit Zeilenvektoren unterstützt, sollten Sie [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) verwenden. Weitere Informationen finden Sie unter [Verwenden von DirectXMath mit Direct3D](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration-d3dx).

 

Folgende Inhalte werden behandelt:

-   Initialisieren von [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d)-Schnittstellen mit der Windows-Runtime
-   Anwenden von Vertex-Shader-Operationen
-   Einrichten der Geometrie
-   Rastern der Szene (Glätten der 3D-Szene auf eine 2D-Projektion)
-   Culling verborgener Oberflächen

> **Hinweis**  

 

Als Nächstes erstellen wir ein Direct3D-Gerät, eine Swapchain sowie eine Renderingzielansicht und stellen das gerenderte Bild auf dem Display dar.

[Schnellstart: Einrichten von DirectX-Ressourcen und Anzeigen eines Bilds](setting-up-directx-resources.md)

## <a name="related-topics"></a>Verwandte Themen


* [Direct3D 11-Grafiken](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 




