---
description: Mit dem Bewertungsbericht in Partner Center können Sie sehen, wie Kunden Ihre APP im Store bewertet haben.
title: Bericht „Bewertungen“
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Bewertung, Rate, Stern, Sterne, bewertet
ms.localizationpriority: medium
ms.openlocfilehash: 4d1d0f9ceaa660245c537c4caf1f1770f1c5be6d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030343"
---
# <a name="ratings-report"></a>Bericht „Bewertungen“


Mit dem **Bewertungs** Bericht in [Partner Center](https://partner.microsoft.com/dashboard) können Sie sehen, wie Kunden Ihre APP im Store bewertet haben. 

Sie können diese Daten im Partner Center anzeigen oder [den Bericht herunterladen, um den Bericht](download-analytic-reports.md) offline anzuzeigen. Alternativ können Sie diese Daten mithilfe der [Get App Reviews](../monetize/get-app-reviews.md) -Methode in der [Microsoft Store Analytics-Rest-API](../monetize/access-analytics-data-using-windows-store-services.md)Programm gesteuert abrufen.

> [!TIP]
> Um in den letzten 30 Tagen die Überprüfungen, BEWERTUNGEN und das Benutzer Feedback für alle Ihre apps anzuzeigen, erweitern Sie im linken Navigationsmenü die Option " **einbinden** ", und wählen Sie über **Prüfungen und Feedback aus.** 

## <a name="apply-filters"></a>Anwenden von Filtern

Am oberen Rand der Seite können Sie den Zeitraum auswählen, in dem Sie die Überprüfungen anzeigen möchten. Die Standardauswahl ist die Gültigkeits **Dauer** , aber Sie können die Überprüfungen für 30 Tage, 3 Monate, 6 Monate oder 12 Monate oder für einen von Ihnen angegebenen benutzerdefinierten Datenbereich anzeigen. Beachten Sie, dass die Bewertungs **Aufschlüsselung** und die **durchschnittliche Bewertung über Zeit** Diagramme immer Daten der letzten 12 Monate anzeigen. Diese Zeitperiode-Optionen wirken sich nicht auf diese Diagramme aus.

Sie können **Filter** erweitern, um die auf dieser Seite angezeigten Überprüfungen anhand der folgenden Optionen zu filtern. Diese Filter gelten nicht für die **Bewertungs Aufschlüsselung** und die **durchschnittliche Bewertung im Zeit** Diagramm.

-   **Markt** : Die Standardeinstellung ist **Alle Märkte** . Sie können einen bestimmten Markt auswählen, wenn auf dieser Seite nur Bewertungen der in diesem Markt ansässigen Kunden angezeigt werden sollen.
-   **Gerätetyp** : Der Standardfilter ist **Alle Geräte** . Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Bewertungen von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
-   **Betriebssystemversion** : die Standardeinstellung ist " **alle** ". Sie können eine bestimmte Betriebssystemversion auswählen, wenn Sie möchten, dass auf dieser Seite nur Bewertungen angezeigt werden, die von Kunden in dieser Betriebssystemversion verbleiben.
-   **Kategoriename** : der Standardfilter ist **all** . Sie können auswählen, dass nur Bewertungen angezeigt werden sollen, die mit Überprüfungen verknüpft sind, die wir als zu einer bestimmten Kategorie von [Review Insights](reviews-report.md#insight-categories) gehört haben, damit nur Überprüfungen angezeigt werden, die dieser Kategorie zugeordnet wurden. 

> [!TIP]
> Wenn keine Bewertungen auf der Seite angezeigt werden, stellen Sie sicher, dass Ihre Filter nicht all Ihre Bewertungen ausgeschlossen haben. Wenn Sie z. b. nach einem Ziel Betriebssystem filtern, das Ihre APP nicht unterstützt, werden keine Bewertungen angezeigt.


## <a name="rating-breakdown"></a>Bewertungs Aufschlüsselung

Das Diagramm für die **Bewertungs Aufschlüsselung** zeigt Folgendes: 
- Die durchschnittliche Bewertungs-Stern-Bewertung für die app.
- Die Gesamtanzahl der Bewertungen ihrer app in den letzten 12 Monaten.
- Die Gesamtanzahl der Bewertungen für jede Stern Bewertung.
- Die Anzahl der Bewertungen für jeden Typ der Bewertung (neu oder überarbeitet) pro Stern Bewertung in den letzten 12 Monaten.
 - **Neue Bewertungen** sind Bewertungen, die Kunden übermittelt, aber überhaupt nicht geändert haben.
 - **Überarbeitete Bewertungen** sind Bewertungen, die vom Kunden in beliebiger Weise geändert wurden, auch wenn nur der Text der Überprüfung geändert wurde.

> [!TIP]
> Die durchschnittliche Bewertung, die ein Kunde im Geschäft sieht, berücksichtigt den Markt-und Gerätetyp des Kunden, sodass er sich von den in diesem Bericht angezeigten Werten unterscheiden kann.


## <a name="average-rating"></a>Durchschnittliche Bewertung

Das Diagramm für die **durchschnittliche Bewertung** zeigt, wie sich die durchschnittliche Bewertung der app in den letzten 12 Monaten geändert hat.

Anstatt den Durchschnitt aller Bewertungen zu berechnen, die während der letzten 12 Monate verbleiben (wie im Diagramm mit den **Bewertungs** Diagrammen), zeigt das **Durchschnitts Bewertungs** Diagramm an, wie Kunden die app in einer bestimmten Woche bewertet haben. Dies kann Ihnen dabei helfen, Trends zu identifizieren oder zu ermitteln, ob die Bewertungen von Updates oder anderen Faktoren betroffen sind.

## <a name="rating-by-market"></a>Bewertung nach Markt

Das Diagramm **Bewertung nach Markt** zeigt eine Aufschlüsselung der durchschnittlichen Bewertungen in den einzelnen Märkten für den ausgewählten Zeitraum an. Diese Daten werden standardmäßig in einem visuellen **Karten** Formular angezeigt. Sie können das Steuerelement aber auch in der oberen rechten Ecke umschalten, um es als **Tabelle** anzuzeigen.

In der **Tabellen** Ansicht werden fünf Märkte gleichzeitig angezeigt, entweder alphabetisch oder mit der höchsten/niedrigsten Durchschnittsbewertung. Sie können auch die Daten dieses Diagramms herunterladen, um die Informationen für alle Märkte anzuzeigen.

Sie können dieses Diagramm auch nach **Bewertung** filtern. Standardmäßig werden Reviews mit allen Stern Bewertungen überprüft, Sie können jedoch bestimmte Bewertungen überprüfen und deaktivieren (von 1 bis 5 Sternen), wenn Sie nur Überprüfungen sehen möchten, die mit bestimmten Stern Bewertungen verknüpft sind.
