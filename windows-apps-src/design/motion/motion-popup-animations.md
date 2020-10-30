---
description: Verwenden Sie Popupanimationen, um Popup-UI-Elemente für Flyouts oder benutzerdefinierte Popup-UI-Elemente anzuzeigen und auszublenden. Popupelemente sind Container, die über dem Inhalt der App angezeigt werden und ausgeblendet werden, wenn Benutzer außerhalb des Popupelements tippen oder klicken.
title: Animationen für Popupbenutzeroberflächen
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5d09363d6ecd2eb5909c98da316c7c1395ebec03
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034123"
---
# <a name="pop-up-ui-animations"></a>Animationen für Popupbenutzeroberflächen



Verwenden Sie Popupanimationen, um Popup-UI-Elemente für Flyouts oder benutzerdefinierte Popup-UI-Elemente anzuzeigen und auszublenden. Popupelemente sind Container, die über dem Inhalt der App angezeigt werden und ausgeblendet werden, wenn Benutzer außerhalb des Popupelements tippen oder klicken.

> **Wichtige APIs** : [**popinpoinmeanimation-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [**popuppoinmetransition-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Popupanimationen zum Anzeigen oder Ausblenden benutzerdefinierter Popup-UI-Elemente, die nicht Teil der eigentlichen App-Seite sind. In die von Windows bereitgestellten allgemeinen Steuerelemente sind diese Animationen bereits integriert.
-   Verwenden Sie Popupanimationen nicht für QuickInfos oder Dialogfelder.
-   Verwenden Sie Popupanimationen nicht zum Anzeigen oder Ausblenden von Benutzeroberflächen innerhalb des Hauptinhalts der App. Verwenden Sie Popupanimationen nur, um einen Popupcontainer anzuzeigen oder auszublenden, der über dem Hauptinhalt der App angezeigt wird.

## <a name="related-articles"></a>Verwandte Artikel

* [Übersicht über Animationen](./xaml-animation.md)
* [Animieren von Popupbenutzeroberflächen](/previous-versions/windows/apps/jj649433(v=win.10))
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliotheksanimationen](/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 
