---
Description: Der Bericht Bewertungen im Partner Center können Sie sehen, wie Kunden Ihre app in den Store bewertet.
title: Bericht „Bewertungen“
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, Uwp, Bewertung, Rate, Sterne, Sternen bewertet
ms.localizationpriority: medium
ms.openlocfilehash: 9b507212cd660a025bc3d858fc2938cf59d4219f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640055"
---
# <a name="ratings-report"></a>Bericht „Bewertungen“


Die **Bewertungen** Bericht im [Partner Center](https://partner.microsoft.com/dashboard) können Sie sehen, wie Kunden Ihre app in den Store bewertet. 

Sie können diese Daten anzeigen, im Partner Center oder [Bericht herunterladen](download-analytic-reports.md) offline anzeigen. Alternativ können Sie diese Daten programmgesteuert abrufen, indem Sie mit der [Bewertungen zu Apps abrufen](../monetize/get-app-reviews.md) -Methode in der die [Microsoft Store Analytics-REST-API](../monetize/access-analytics-data-using-windows-store-services.md).

> [!TIP]
> Um Rezensionen, Bewertungen und Benutzerfeedback für alle Ihre Apps in den letzten 30 Tagen anzusehen, erweitern Sie **Einbeziehen** im linken Navigationsmenü und wählen Sie **Kritiken und Feedback** aus. 

## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Rezensionen angezeigt werden sollen. Die Standardeinstellung ist **Lebensdauer**, aber Sie können Rezensionen für 30 Tage, 3, 6 oder 12 Monate anzeigen, oder für einen benutzerdefinierten Zeitraum, den Sie angeben. Beachten Sie, dass die Diagramme **Rezensionsübersicht** und **Durchschnittliche Bewertung im Laufe der Zeit** Daten für die letzten 12 Monate anzeigt. Diese Optionen für den Zeitraum wirken sich nicht auf die Diagramme aus.

Sie können **Filter** erweitern, um alle angezeigten Rezensionen auf dieser Seite mit den folgenden Optionen zu filtern. Diese Filter werden nicht auf die Diagramme **Rezensionsübersicht** und **Durchschnittliche Bewertung im Laufe der Zeit** angewandt.

-   **Markt**: Die Standardeinstellung ist **alle Märkte**. Sie können einen bestimmten Markt auswählen, wenn auf dieser Seite nur Bewertungen der in diesem Markt ansässigen Kunden angezeigt werden sollen.
-   **Gerätetyp**: Der Standardfilter ist **alle Geräte**. Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Bewertungen von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
-   **Version des Betriebssystems**: Die Standardeinstellung ist **alle**. Wenn Sie dieser Seite, um Bewertungen links von Kunden nur für diese Version des Betriebssystems anzeigen möchten, können Sie eine bestimmte Betriebssystemversion.
-   **Kategorienamen**: Der Standardfilter ist **alle**. Sie können auch nur Bewertungen, Reviews, die wir identifiziert haben, Zugehörigkeit zu einer bestimmten zugeordnet anzeigen [überprüfen Sie die Kategorie Insight](reviews-report.md#insight-categories) an nur Bewertungen, dass wir diese Kategorie zugeordnet haben. 

> [!TIP]
> Überprüfen Sie, wenn Sie alle Bewertungen auf der Seite nicht angezeigt wird, um sicherzustellen, dass Ihre Filter, die alle Ihre Bewertungen ausgeschlossen nicht getan haben. Wenn Sie von einem Ziel-Betriebssystem, die Ihre app nicht unterstützt filtern, werden Sie z. B. keine Bewertungen angezeigt.


## <a name="rating-breakdown"></a>Aufschlüsselung der Bewertung

Die **Bewertung Aufschlüsselung** zeigt das Diagramm: 
- Den durchschnittlichen Bewertungsstern der Rezensionsübersicht für die App.
- Die Gesamtanzahl der Bewertungen Ihrer App in den letzten 12 Monaten.
- Die Gesamtanzahl der Bewertungen für jede Bewertung von Sternen.
- Die Anzahl der Bewertungen für jeden Bewertungstyp (neu oder überarbeitet) pro Sterne-Bewertung in den letzten 12 Monaten.
 - **Neue Bewertungen** sind Bewertungen, die Kunden abgegeben, aber nicht geändert haben.
 - **Überarbeitete Bewertungen** Bewertungen, die vom Kunden in keinster Weise geändert wurden, auch wenn es sich nur um Textänderungen handelt.

> [!TIP]
> Bei der durchschnittlichen Bewertung, die einem Kunden im Store angezeigt wird, werden der Markt und der Gerätetyp des Kunden berücksichtigt. Daher können die Inhalte in diesem Bericht davon abweichen.


## <a name="average-rating"></a>Durchschnittliche Bewertung

Die **durchschnittliche Bewertung** Diagramm zeigt, wie die durchschnittliche Bewertung der app in den vergangenen 12 Monaten geändert hat.

Anstatt berechnet den Durchschnitt aller Bewertungen Links während der letzten 12 Monate (wie in der **Bewertungen Aufschlüsselung** Diagramm), wird die **durchschnittliche Bewertung** Diagramm wird gezeigt, wie Kunden die app auf einer bestimmten Woche bewertet. Das hilft Ihnen bei der Ermittlung von Trends oder bei der Bestimmung, ob Bewertungen von Updates oder anderen Faktoren betroffen sind.

## <a name="rating-by-market"></a>Bewertung nach Markt

Die **Bewertung nach Markt** Diagramm zeigt eine Aufschlüsselung der die durchschnittliche Bewertungen in jedem Markt über den gewählten Zeitraum. Standardmäßig zeigen wir diese Daten in einem Visual **Zuordnung** Formular, aber Sie können auch ein-oder Ausblenden des Steuerelements in der oberen rechten Ecke zum Anzeigen als eine **Tabelle**.

Die **Tabelle** Ansicht zeigt fünf Märkte zu einem Zeitpunkt sortiert alphabetisch oder nach der höchsten/niedrigste durchschnittsbewertung. Sie können auch die Daten dieser Tabelle zum Anzeigen von Informationen für alle Märkte zusammen herunterladen.

Sie können auch dieses Diagramm durch Filtern **Bewertung**. Reviews mit allen sternbewertungen sind standardmäßig aktiviert, aber Sie können überprüfen, und deaktivieren Sie die spezifische Bewertungen (von 1 bis 5 Sterne), wenn Sie nur bestimmte sternbewertungen Rezensionen anzeigen möchten.