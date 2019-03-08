---
title: Geordnete Rasterizeransicht (ROV)
description: Mit rasterizergesteuerten Ansichten können einige der Einschränkungen von Tiefenpuffern behandelt werden, insbesondere wenn mehrere Texturen mit Transparenz alle für dieselben Pixel gelten.
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- Geordnete Rasterizeransicht (ROV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ed49ba81c44a799e4d827d636f601c77d01333b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603595"
---
# <a name="rasterizer-ordered-view-rov"></a>Geordnete Rasterizeransicht (ROV)


Mit rasterizergesteuerten Ansichten können einige der Einschränkungen von Tiefenpuffern behandelt werden, insbesondere wenn mehrere Texturen mit Transparenz alle für dieselben Pixel gelten.

Mit rasterizergesteuerten Ansichten können OIT-Algorithmen (Order Independent Transparency) auf das Rendern von Pixeln angewendet werden. Mit einem Tiefenpuffer kann ein Pixel lediglich gezeichnet oder verdeckt werden; ein Konzept für ein teilweises Verdecken durch Transparenz ist nicht vorhanden. OIT-Algorithmen wenden transparente Texturen in der richtigen Reihenfolge an, damit das Endergebnis vorhersehbar richtig gezeichnet wird, wenn z. B. ein transparentes, gläsernes Objekt hinter einem Glasfenster angezeigt werden soll, das sich hinter etwas Vegetation befindet, die transparente Texturen verwendet. Ohne ROVs und OIT-Algorithmen war die Reihenfolge, mit der diese transparenten Objekte gezeichnet wurden, unvorhersehbar, und die gerenderte Szene konnte verwirrend und inkorrekt sein.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ansichten](views.md)

 

 




