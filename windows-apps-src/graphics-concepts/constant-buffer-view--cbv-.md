---
title: Konstantenpufferansicht (CBV)
description: Konstantenpuffer enthalten Konstantendaten für Shader. Der Nutzen ist, dass die Daten erhalten bleiben und jeder GPU-Shader darauf zugreifen kann, bis eine Änderung der Daten erforderlich wird.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Konstantenpufferansicht (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7179f8644970a24a9e7b9ce50a4bcb4d5d225d46
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370455"
---
# <a name="constant-buffer-view-cbv"></a>Konstantenpufferansicht (CBV)


Konstantenpuffer enthalten Konstantendaten für Shader. Der Nutzen ist, dass die Daten erhalten bleiben und jeder GPU-Shader darauf zugreifen kann, bis eine Änderung der Daten erforderlich wird.

Typische Daten für einen Konstantenpuffer sind globale, Ansichts- und Projektionsmatrizen, die während des Zeichnens eines Frames konstant bleiben.

Das Konstantenpuffer-Layout sollte mit dem HLSL-Layout übereinstimmen (weitere Informationen finden Sie unter [Regeln für das Packen von Konstantenvariablen](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ansichten](views.md)

 

 




