---
title: Grundlegendes zu 3D-Grafiken für DirectX-Spiele
description: Im Folgenden zeigen wir Ihnen, wie Sie grundlegende Konzepte von 3D-Grafiken durch die Programmierung mit DirectX umsetzen können.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX, Grafiken
ms.localizationpriority: medium
ms.openlocfilehash: 5dbdf6072f57d12d424f0787cfa2e8993a1624af
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621795"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Grundlegendes zu 3D-Grafiken für DirectX-Spiele



Im Folgenden zeigen wir Ihnen, wie Sie grundlegende Konzepte von 3D-Grafiken durch die Programmierung mit DirectX umsetzen können.

**Ziel:** Erfahren Sie, eine app 3D-Grafiken zu programmieren.

## <a name="prerequisites"></a>Voraussetzungen


Es wird davon ausgegangen, dass Sie mit C+ vertraut sind. Sie müssen außerdem mit den grundlegenden Konzepten der Grafikprogrammierung vertraut sein.

**Die Gesamtzeit zum Abschließen:** 30 Minuten.

## <a name="where-to-go-from-here"></a>Weitere Informationen


Hier sprechen wir von 3D-Grafiken mit DirectX und C++ entwickeln\\Cx. In diesem fünfteiligen Lernprogramm werden die [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466)-API sowie die Konzepte und der Code vorgestellt, die auch in zahlreichen anderen DirectX-Beispielen zum Einsatz kommen. Die einzelnen Teile bauen aufeinander auf. Sie behandeln u. a. das Konfigurieren von DirectX für Ihre UWP-App mit C++ sowie Grundtypen mit Texturen und das Hinzufügen von Effekten.

> **Beachten Sie**  dieses Tutorial verwendet ein rechtshändiges Koordinatensystem mit Spaltenvektoren. Bei vielen DirectX-Beispielen und -Apps wird ein linkshändiges Koordinatensystem mit Zeilenvektoren verwendet. Für eine umfangreichere mathematische Grafiklösung, die ein linkshändiges Koordinatensystem mit Zeilenvektoren unterstützt, sollten Sie [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) verwenden. Weitere Informationen finden Sie unter [Verwenden von DirectXMath mit Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D).

 

Folgende Inhalte werden behandelt:

-   Initialisieren von [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466)-Schnittstellen mit der Windows-Runtime
-   Anwenden von Vertex-Shader-Operationen
-   Einrichten der Geometrie
-   Rastern der Szene (Glätten der 3D-Szene auf eine 2D-Projektion)
-   Culling verborgener Oberflächen

> **Hinweis**  

 

Als Nächstes erstellen wir ein Direct3D-Gerät, eine Swapchain sowie eine Renderingzielansicht und stellen das gerenderte Bild auf dem Display dar.

[Schnellstart: Einrichten von DirectX-Ressourcen und Anzeigen eines Bilds](setting-up-directx-resources.md)

## <a name="related-topics"></a>Verwandte Themen


* [Direct3D 11-Grafiken](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* ["HLSL"](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 




