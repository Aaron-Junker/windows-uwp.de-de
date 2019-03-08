---
Description: Verwenden Sie Drag & Drop-Animationen, wenn Benutzer Objekte verschieben, z. B. wenn sie ein Element innerhalb einer Liste verschieben oder ein Element auf einem anderen ablegen.
title: Animationen für Drag & Drop-Vorgang in UWP-Apps
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c12acd2148c8c85d69354a71c016d7a7230e590b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605455"
---
# <a name="drag-animations"></a>Animationen für Drag & Drop-Vorgang




Verwenden Sie Drag & Drop-Animationen, wenn Benutzer Objekte verschieben, z. B. wenn sie ein Element innerhalb einer Liste verschieben oder ein Element auf einem anderen ablegen.

> **Wichtige APIs**: [**DragItemThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br243174)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


**Ziehen Sie Start-animation**

-   Verwenden Sie die Animation für das Starten des Ziehens, wenn Benutzer beginnen, ein Objekt zu verschieben.
-   Nehmen Sie betroffene Objekte nur dann in die Animation auf, wenn andere Objekte vorhanden sind, die von dem Drag & Drop-Vorgang betroffen sein können.
-   Verwenden Sie die Animation für das Beenden des Ziehens, um eine mit der Animation für das Starten des Ziehens begonnene Animationssequenz abzuschließen. Dadurch wird die Größenänderung des gezogenen Objekts, die durch die Animation für das Starten des Ziehens ausgelöst wurde, zurückgesetzt.

**Ziehen Sie End-animation**

-   Verwenden Sie die Animation für das Beenden des Ziehens, wenn Benutzer ein gezogenes Objekt ablegen.
-   Verwenden Sie die Animation für das Beenden des Ziehens zusammen mit Animationen für das Hinzufügen und Löschen bei Listen.
-   Nehmen Sie betroffene Objekte nur dann in die Animation für das Beenden des Ziehens auf, wenn Sie die gleichen Objekte in die Animation für das Starten des Ziehens aufgenommen haben.
-   Verwenden Sie die Animation für das Beenden des Ziehens nicht, wenn Sie vorher nicht die Animation für das Starten des Ziehens verwendet haben. Sie müssen beide Animationen verwenden, damit die Objekte nach Abschluss der Ziehsequenz wieder zu ihrer ursprünglichen Größe zurückkehren.

**Zwischen DragEnter animation**

-   Verwenden Sie die Animation für das Starten des Zwischenziehens, wenn Benutzer die Ziehquelle in einen Ablagebereich ziehen, in dem sie zwischen zwei anderen Objekten abgelegt werden kann.
-   Wählen Sie einen angemessenen Zielbereich zum Ablegen aus. Dieser Bereich sollte nicht so klein sein, dass es den Benutzern schwerfällt, die Ziehquelle zum Ablegen zu positionieren.
-   Als Richtung für das Verschieben betroffener Objekte, um den Ablagebereich anzuzeigen, empfiehlt es sich, die Objekte direkt auseinanderzuziehen. Ob es sich um eine vertikale oder horizontale Bewegung handelt, hängt davon ab, wie die betroffenen Objekte zueinander ausgerichtet sind.
-   Verwenden Sie die Animation für das Starten des Zwischenziehens nicht, wenn die Ziehquelle nicht in einem Bereich abgelegt werden kann. An der Animation für das Starten des Zwischenziehens erkennen Benutzer, dass die Ziehquelle zwischen den betroffenen Objekten abgelegt werden kann.

**Ziehen Sie zwischen Leave-animation**

-   Verwenden Sie die Animation für das Beenden des Zwischenziehens, wenn Benutzer ein Objekt von einem Bereich wegziehen, in dem es zwischen zwei anderen Objekten abgelegt werden könnte.
-   Verwenden Sie die Animation für das Beenden des Zwischenziehens nicht, wenn Sie zuvor nicht die Animation für das Starten des Zwischenziehens verwendet haben.


## <a name="related-articles"></a>Verwandte Artikel

**Für Entwickler**
* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animieren von Drag & Drop-Sequenzen](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [Schnellstart: Ihre Benutzeroberfläche mit der Bibliothek Animationen animieren](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**DragItemThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**DropTargetItemThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**DragOverThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br243180)


 




