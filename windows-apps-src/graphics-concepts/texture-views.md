---
title: Texturansichten
description: Auf Texturressourcen wird in Direct3D mit einer Ansicht zugegriffen, bei der es sich um einen Mechanismus für die Hardware-Interpretation einer Ressource im Speicher handelt.
ms.assetid: 18DABFCE-8A36-4C4E-B08E-10428B05D701
keywords:
- Texturansichten
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9167db4648dd193acaff0a224f3378486d171ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638445"
---
# <a name="texture-views"></a>Texturansichten


Auf Texturressourcen wird in Direct3D mit einer Ansicht zugegriffen, bei der es sich um einen Mechanismus für die Hardware-Interpretation einer Ressource im Speicher handelt. Eine Ansicht ermöglicht einer bestimmten Pipelinephase den Zugriff auf die erforderlichen [Unterressourcen](resource-types.md), in der von der Anwendung gewünschten Darstellung.

Eine Ansicht unterstützt das Konzept einer typenlosen Ressource. Eine typenlose Ressource ist eine Ressource mit einer bestimmten Größe, die aber nicht mit einem bestimmten Datentyp erstellt wurde. Die Daten werden dynamisch interpretiert, wenn sie an die Pipeline gebunden sind.

Die folgende Abbildung zeigt ein Beispiel für die Bindung eines 2D-Textur-Arrays mit 6 Texturen als Shader-Ressource durch die Erstellung einer Shader-Ressourcenansicht dafür. Die Ressource wird dann als Array von Texturen behandelt. (Hinweis: Eine Unterressource kann nichtgleichzeitig als Eingabe und Ausgabe an die Pipeline gebunden werden.)

![Illustration eines Textur-Arrays mit sechs Texturen](images/d3d10-cube-texture-faces.png)

Bei Verwendung eines 2D-Textur-Arrays als Renderziel kann die Ressource als Array von 2D-Texturen (in diesem Beispiel 6) mit Mipmap-Ebenen (in diesem Beispiel 3) betrachtet werden.

Erstellen Sie ein Objekt für ein Renderziel durch Aufrufen von CreateRenderTargetView. Rufen Sie dann OMSetRenderTargets auf, um die Renderzielansicht für die Pipeline festzulegen. Rendern Sie in die Renderziele durch Aufrufen von Draw und die Verwendung von RenderTargetArrayIndex zur Indizierung in der korrekten Textur des Arrays. Sie können eine Unterressource (eine Mipmap-Ebene, eine Array-Index-Kombination) zum Binden eines beliebigen Arrays von Unterressourcen verwenden. Sie können also, wenn Sie wollen, die Bindung an die zweite Mipmap-Ebene durchführen und nur diese Mipmap-Ebene aktualisieren, wie in der folgenden Abbildung gezeigt.

![Illustration der Bindung nur an die zweite Mipmap-Ebene eines Textur-Arrays](images/d3d10-cube-texture-faces-subresource.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ressourcen](resources.md)

 

 




