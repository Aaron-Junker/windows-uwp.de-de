---
title: Vergleich von Vollbild- und Fenstermodus
description: Direct3D-Anwendungen können jeweils entweder im Fenstermodus oder im Vollbildmodus ausgeführt werden.
ms.assetid: EE8B9F87-822B-4576-A446-CA603E786862
keywords:
- Vergleich von Vollbild- und Fenstermodus
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d84bbebfaf19b756e6abc6c592187b6b0ee92200
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618465"
---
# <a name="span-iddirect3dconceptswindowedvsfull-screenmodespanwindowed-vs-full-screen-mode"></a><span id="direct3dconcepts.windowed_vs__full-screen_mode"></span>Im Vergleich zu den Vollbildmodus im Fenstermodus


Direct3D-Anwendungen können jeweils entweder im Fenstermodus oder im Vollbildmodus ausgeführt werden. Im *Fenstermodus* teilt sich die Anwendung die verfügbare Bildschirmfläche auf dem Desktop mit allen anderen ausgeführten Programmen. Im *Vollbildmodus* bedeckt das Fenster der Anwendung den gesamten Desktop, alle anderen ausgeführten Programme (einschließlich der Entwicklungsumgebung) werden ausgeblendet. Normalerweise werden beispielsweise Spiele standardmäßig im Vollbildmodus ausgeführt, damit der Benutzer ganz in das Spiel eintauchen kann, ohne durch die anderen Anwendungen abgelenkt zu werden.

Die Codeunterschiede zwischen Vollbildmodus und Fenstermodus sind äußert gering.

Da eine Anwendung, die im Vollbildmodus ausgeführt wird, den gesamten Bildschirm ausfüllt, wird für das Debuggen der Anwendung entweder ein zweiter Bildschirm benötigt oder ein Remotedebugger. Ein Vorteil von Anwendungen im Fenstermodus ist, dass Sie den Code in einem Debugger schrittweise ausführen lassen können, ohne einen zweiten Bildschirm oder einen Remotedebugger verwenden zu müssen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

 

 




