---
title: Konstantenpufferansicht (CBV)
description: Konstante Puffer enthalten shaderkonstantendaten. Der Wert dafür ist, dass die Daten weiterhin vorhanden sind und von jedem GPU-Shader auf Sie zugegriffen werden kann, bis die Daten geändert werden müssen.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Konstantenpufferansicht (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 50b590ab931f60b67ecd527629b681c9ffe63f71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162744"
---
# <a name="constant-buffer-view-cbv"></a>Konstantenpufferansicht (CBV)


Konstante Puffer enthalten shaderkonstantendaten. Der Wert dafür ist, dass die Daten weiterhin vorhanden sind und von jedem GPU-Shader auf Sie zugegriffen werden kann, bis die Daten geändert werden müssen.

Typische Daten für einen konstanten Puffer wären Welt-, Projektions-und Ansichts Matrizen, die während der Zeichnung eines Frames konstant bleiben.

Das konstantenpufferlayout sollte dem HLSL-Layout entsprechen (siehe [Verpackungs Regeln für konstante Variablen](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ansichten](views.md)

 

 