---
Description: Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen.
title: Animationen für Zeigerklicks in UWP-Apps
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f1abd71cda8358e3f7e2fe36091f9c42f05bcb00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663795"
---
# <a name="pointer-click-animations"></a>Animationen für Zeigerklicks



Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen. Bei der Animation für „Zeiger nach unten“ wird das gedrückte Element leicht verkleinert und geneigt. Sie wird wiedergegeben, wenn erstmalig auf ein Element getippt wird. Die Animation für „Zeiger nach oben“, mit der der ursprüngliche Zustand des Elements wiederhergestellt wird, wird beim Loslassen des Zeigers wiedergegeben.


> **Wichtige APIs:** [**PointerUpThemeAnimation Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969168), [ **PointerDownThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Wenn Sie eine Animation für „Zeiger hoch“ verwenden, sollte die Animation ausgelöst werden, nachdem der Benutzer den Zeiger losgelassen hat. Auf diese Weise erhalten Benutzer umgehend Feedback, dass ihre Aktion erkannt wurde. Dies gilt auch, wenn die durch das Tippen ausgelöste Aktion (beispielsweise das Navigieren zu einer neuen Seite) langsamer reagiert.

## <a name="related-articles"></a>Verwandte Artikel

* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Die Animation von Zeiger Klicks](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Schnellstart: Ihre Benutzeroberfläche mit der Bibliothek Animationen animieren](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PointerUpThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**PointerDownThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




