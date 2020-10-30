---
description: Randbasierte Animationen blenden UI-Elemente ein oder aus, die vom Bildschirmrand ausgehen.
title: Animationen für randbasierte Benutzeroberflächenelemente
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a49277a82a3bfffffb478496ab01d353c169463f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034233"
---
# <a name="edge-based-ui-animations"></a>Animationen für randbasierte Benutzeroberflächenelemente





Randbasierte Animationen blenden UI-Elemente ein oder aus, die vom Bildschirmrand ausgehen. Die Aktionen zum Anzeigen und Ausblenden können von Benutzern oder von der App initiiert werden. Die UI-Elemente können die Anwendung überlagern oder Teil der Hauptoberfläche der App sein. Wenn das UI-Element Teil der App-Oberfläche ist, müssen Sie möglicherweise die Größe der restlichen App entsprechend anpassen.

> **Wichtige APIs** : [ **edgeuidermetransition-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Animationen für Randbenutzeroberflächen zum Anzeigen oder Ausblenden einer benutzerdefinierten Meldungs- oder Fehlerleiste, die nicht weit in den Bildschirm hineinreicht.
-   Verwenden Sie Panelanimationen, um Benutzeroberflächen anzuzeigen, die weit in den Bildschirm hineinreichen, beispielsweise ein Aufgabenbereich oder eine benutzerdefinierte Bildschirmtastatur.
-   Lassen Sie die UI von demselben Rand hereingleiten, mit dem sie verbunden ist.
-   Lassen Sie die UI an demselben Rand hinausgleiten, von dem sie hereingekommen ist.
-   Wenn der Inhalt der App aufgrund des Herein- oder Hinausgleitens der Benutzeroberfläche in der Größe angepasst werden muss, verwenden Sie Ein- und Ausblendungsanimationen.
    -   Wenn die Benutzeroberfläche hereingleitet, verwenden Sie nach der Animation für Randbenutzeroberflächen oder Panels eine Ein- oder Ausblendungsanimation.
    -   Wenn die Benutzeroberfläche herausgleitet, verwenden Sie gleichzeitig mit der Animation für Randbenutzeroberflächen oder Panels eine Ein- oder Ausblendungsanimation.
-   Wenden Sie diese Animationen nicht auf Benachrichtigungen an. Benachrichtigungen sollten sich nicht auf der randbasierten Benutzeroberfläche befinden.
-   Wenden Sie die Animation für Randbenutzeroberflächen oder Panels nicht auf Benutzeroberflächencontainer oder Steuerelemente an, die sich nicht am Rand des Bildschirms befinden. Diese Animationen werden nur für das Anzeigen und Schließen von Benutzeroberflächen am Rand des Bildschirms sowie zum Ändern ihrer Größe verwendet. Verwenden Sie zum Verschieben anderer Benutzeroberflächenarten die Animationen für das Ändern der Position.

    ![Zeigt, wann Sie Rand-UI- oder Panelanimationen verwenden sollten und wann Sie Positionen ändern sollten.](images/edgevsreposition.png)

## <a name="related-articles"></a>Verwandte Artikel


**Für Entwickler**
* [Übersicht über Animationen](./xaml-animation.md)
* [Animieren der Rand-UI](/previous-versions/windows/apps/jj649428(v=win.10))
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliotheksanimationen](/previous-versions/windows/apps/hh452703(v=win.10))
* [**EdgeUIThemeTransition-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**PaneThemeTransition-Klasse**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [Animieren von Ein- und Ausblendungen](/previous-versions/windows/apps/jj649429(v=win.10))
* [Animieren von Änderungen der Position](/previous-versions/windows/apps/jj649434(v=win.10))

 

 
