---
Description: Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen.
title: Animationen für Zeigerklicks in UWP-Apps
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ee87a62796ed51d09d44cabecd0b49873145bd90
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366083"
---
# <a name="pointer-click-animations"></a>Animationen für Zeigerklicks



Verwenden Sie Zeigeranimationen, um Benutzern visuelles Feedback zu liefern, wenn diese auf ein Element tippen. Bei der Animation für „Zeiger nach unten“ wird das gedrückte Element leicht verkleinert und geneigt. Sie wird wiedergegeben, wenn erstmalig auf ein Element getippt wird. Die Animation für „Zeiger nach oben“, mit der der ursprüngliche Zustand des Elements wiederhergestellt wird, wird beim Loslassen des Zeigers wiedergegeben.


> **Wichtige APIs:** [**PointerUpThemeAnimation Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation), [ **PointerDownThemeAnimation-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Wenn Sie eine Animation für „Zeiger hoch“ verwenden, sollte die Animation ausgelöst werden, nachdem der Benutzer den Zeiger losgelassen hat. Auf diese Weise erhalten Benutzer umgehend Feedback, dass ihre Aktion erkannt wurde. Dies gilt auch, wenn die durch das Tippen ausgelöste Aktion (beispielsweise das Navigieren zu einer neuen Seite) langsamer reagiert.

## <a name="related-articles"></a>Verwandte Artikel

* [Übersicht über Animationen](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Die Animation von Zeiger Klicks](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))
* [Schnellstart: Ihre Benutzeroberfläche mit der Bibliothek Animationen animieren](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 




