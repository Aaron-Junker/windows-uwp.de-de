---
title: Dreiecksinterpolation
description: Während des Renderns interpoliert die Pipeline Vertexdaten bei jedem Dreieck.
ms.assetid: 1A76DD78-CED7-42BE-BA81-B9050CD3AF9B
keywords:
- Dreiecksinterpolation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e8017cd75ed3dfd4129d6c15d668648792cc8d0a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592905"
---
# <a name="triangle-interpolation"></a>Dreiecksinterpolation


Während des Renderns interpoliert die Pipeline Vertexdaten bei jedem Dreieck. Vertexdaten können eine Vielzahl von Daten sein und enthalten (sind aber nicht beschränkt auf): diffuse Farbe, Glanzfarbe, diffuse Alpha(Transparenz des Dreiecks), Glanzlicht Alpha und Nebelfaktor. Für die programmierbare Scheitelpunkt-Pipeline wird der Nebelfaktor aus dem Nebelregister übernommen. Für die Scheitelpunkt-Pipeline mit fester Funktion wird der Nebelfaktor dem Glanz-Alphawert entnommen.

Bei manchen Scheitelpunktdaten hängt die Interpolation wie folgt vom aktuellen Schattierungsmodus ab:

| Schattierungsmodus | Beschreibung                                                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Flach         | Im Flach-Schattierungsmodus wird nur der Nebelfaktor interpoliert. Für alle anderen interpolierten Werte wird die Farbe des ersten Scheitelpunkts des Dreiecks über die gesamte Fläche angewendet. |
| Gouraud      | Zwischen allen drei Scheitelpunkten wird eine lineare Interpolation durchgeführt.                                                                                                               |

 

Diffus- und Glanzfarbe werden je nach Farbmodell unterschiedlich behandelt. Im RGB-Farbmodell verwendet das System die Farbkomponenten Rot, Grün und Blau in der Interpolation.

Die Alpha-Komponente einer Farbe wird als separater interpolierter Wert behandelt, da Gerätetreiber Transparenz auf zwei unterschiedliche Weisen implementieren können: durch Strukturmischung oder durch Punktierung.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Koordinatensysteme und Geometrie](coordinate-systems-and-geometry.md)

 

 




