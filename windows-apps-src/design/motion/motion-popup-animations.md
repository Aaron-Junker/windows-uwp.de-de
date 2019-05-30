---
Description: Verwenden Sie Popupanimationen, um Popup-UI-Elemente für Flyouts oder benutzerdefinierte Popup-UI-Elemente anzuzeigen und auszublenden. Popupelemente sind Container, die über dem Inhalt der App angezeigt werden und ausgeblendet werden, wenn Benutzer außerhalb des Popupelements tippen oder klicken.
title: Animationen für Popupbenutzeroberflächen in UWP-Apps
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ee3d6a7fc29ec2adfeb149a3bc84f27c482c3be7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366703"
---
# <a name="pop-up-ui-animations"></a>Animationen für Popupbenutzeroberflächen



Verwenden Sie Popupanimationen, um Popup-UI-Elemente für Flyouts oder benutzerdefinierte Popup-UI-Elemente anzuzeigen und auszublenden. Popupelemente sind Container, die über dem Inhalt der App angezeigt werden und ausgeblendet werden, wenn Benutzer außerhalb des Popupelements tippen oder klicken.

> **Wichtige APIs:** [**PopInThemeAnimation Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [ **PopupThemeTransition-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Popupanimationen zum Anzeigen oder Ausblenden benutzerdefinierter Popup-UI-Elemente, die nicht Teil der eigentlichen App-Seite sind. In die von Windows bereitgestellten allgemeinen Steuerelemente sind diese Animationen bereits integriert.
-   Verwenden Sie Popupanimationen nicht für QuickInfos oder Dialogfelder.
-   Verwenden Sie Popupanimationen nicht zum Anzeigen oder Ausblenden von Benutzeroberflächen innerhalb des Hauptinhalts der App. Verwenden Sie Popupanimationen nur, um einen Popupcontainer anzuzeigen oder auszublenden, der über dem Hauptinhalt der App angezeigt wird.

## <a name="related-articles"></a>Verwandte Artikel

* [Übersicht über Animationen](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animieren der Popup-Benutzeroberfläche](https://docs.microsoft.com/previous-versions/windows/apps/jj649433(v=win.10))
* [Schnellstart: Ihre Benutzeroberfläche mit der Bibliothek Animationen animieren](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition-Klasse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 




