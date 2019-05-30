---
Description: Mithilfe von Inhaltsübergangsanimationen können Sie den Inhalt eines Bildschirmbereichs ändern und gleichzeitig den Container oder Hintergrund unverändert lassen. Neuer Inhalt wird eingeblendet. Muss vorhandener Inhalt ersetzt werden, wird dieser Inhalt ausgeblendet.
title: Richtlinien für Inhaltsübergangsanimationen
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5ec8778f590ba9b50c67209eaf4b80e2cbed2f16
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364810"
---
# <a name="content-transition-animations"></a>Inhaltsübergangsanimationen



Mithilfe von Inhaltsübergangsanimationen können Sie den Inhalt eines Bildschirmbereichs ändern und gleichzeitig den Container oder Hintergrund unverändert lassen. Neuer Inhalt wird eingeblendet. Muss vorhandener Inhalt ersetzt werden, wird dieser Inhalt ausgeblendet.

> **Wichtige APIs:** [**ContentThemeTransition-Klasse (XAML)** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.)

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie eine Eingangsanimation, wenn mehrere neue Elemente in einen leeren Container aufgenommen werden sollen. Beispielsweise ist nach dem anfänglichen Laden einer App ein Teil des Inhalts nicht sofort für die Anzeige verfügbar. Wenn der Inhalt bereit zum Anzeigen ist, nehmen Sie diesen verzögerten Inhalt mithilfe einer Inhaltsübergangsanimation in die Ansicht auf.
-   Mit Inhaltsübergängen können Sie einen Satz Inhalt durch einen anderen ersetzen, der sich bereits im gleichen Container in einer Ansicht befindet.
-   Wenn Sie neuen Inhalt aufnehmen, ziehen Sie diesen (von unten nach oben) entgegen dem normalen Seitenfluss oder der Leserichtung in die Ansicht.
-   Stellen Sie neue Inhalte auf logische Art und Weise vor. Geben Sie beispielsweise die wichtigsten Inhalte zuletzt bekannt.
-   Wenn Sie den Inhalt mehrerer Container aktualisieren möchten, lösen Sie alle Übergangsanimationen ohne Staffelung oder Verzögerung gleichzeitig aus.
-   Verwenden Sie Inhaltsübergangsanimationen nicht, wenn sich die gesamte Seite ändert. Verwenden Sie in diesem Fall die Seitenübergangsanimationen.
-   Verwenden Sie Inhaltsübergangsanimationen nicht, wenn der Inhalt nur aktualisiert wird. Inhaltsübergangsanimationen sollen auf Bewegungen hinweisen. Verwenden Sie für Aktualisierungen Ein- und Ausblendungsanimationen.



## <a name="related-articles"></a>Verwandte Artikel

**Für Entwickler (XAML)**
* [Übersicht über Animationen](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animieren von Inhalten der Übergänge](https://docs.microsoft.com/previous-versions/windows/apps/jj649426(v=win.10))
* [Schnellstart: Ihre Benutzeroberfläche mit der Bibliothek Animationen animieren](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**ContentThemeTransition-Klasse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.)

 

 




