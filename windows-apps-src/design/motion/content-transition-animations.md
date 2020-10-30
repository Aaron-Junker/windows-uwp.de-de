---
description: Mithilfe von Inhaltsübergangsanimationen können Sie den Inhalt eines Bildschirmbereichs ändern und gleichzeitig den Container oder Hintergrund unverändert lassen. Neuer Inhalt wird eingeblendet. Muss vorhandener Inhalt ersetzt werden, wird dieser Inhalt ausgeblendet.
title: Richtlinien für Inhaltsübergangsanimationen
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ed70a100b4aec7a2c1490cfc48e4d2f6ceac950e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029781"
---
# <a name="content-transition-animations"></a>Inhaltsübergangsanimationen



Mithilfe von Inhaltsübergangsanimationen können Sie den Inhalt eines Bildschirmbereichs ändern und gleichzeitig den Container oder Hintergrund unverändert lassen. Neuer Inhalt wird eingeblendet. Muss vorhandener Inhalt ersetzt werden, wird dieser Inhalt ausgeblendet.

> **Wichtige APIs** : [ **Content-metransition-Klasse (XAML)**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

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
* [Übersicht über Animationen](./xaml-animation.md)
* [Animieren von Inhaltsübergängen](/previous-versions/windows/apps/jj649426(v=win.10))
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliotheksanimationen](/previous-versions/windows/apps/hh452703(v=win.10))
* [**ContentThemeTransition-Klasse**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

 

 
