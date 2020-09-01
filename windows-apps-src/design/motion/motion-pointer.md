---
Description: Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen.
title: Animationen für Zeigerklicks
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cfd507be6ca09818ec07d12a83c2a272514b08f6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172394"
---
# <a name="pointer-click-animations"></a>Animationen für Zeigerklicks



Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen. Bei der Animation für „Zeiger nach unten“ wird das gedrückte Element leicht verkleinert und geneigt. Sie wird wiedergegeben, wenn erstmalig auf ein Element getippt wird. Die Animation für „Zeiger nach oben“, mit der der ursprüngliche Zustand des Elements wiederhergestellt wird, wird beim Loslassen des Zeigers wiedergegeben.


> **Wichtige APIs**: [**pointeruppoinmeanimation-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation), [**pointerdownpoinmeanimation-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Wenn Sie eine Animation für „Zeiger hoch“ verwenden, sollte die Animation ausgelöst werden, nachdem der Benutzer den Zeiger losgelassen hat. Auf diese Weise erhalten Benutzer umgehend Feedback, dass ihre Aktion erkannt wurde. Dies gilt auch, wenn die durch das Tippen ausgelöste Aktion (beispielsweise das Navigieren zu einer neuen Seite) langsamer reagiert.

## <a name="related-articles"></a>Verwandte Artikel

* [Übersicht über Animationen](./xaml-animation.md)
* [Animieren von Zeigerklicks](/previous-versions/windows/apps/jj649432(v=win.10))
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliotheksanimationen](/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 