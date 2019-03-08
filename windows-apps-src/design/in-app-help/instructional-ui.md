---
Description: Entwerfen Sie eine Anweisungstext Benutzeroberfläche (UI), die Benutzer arbeiten mit UWP-app erläutert.
title: Richtlinien für das Entwerfen von Benutzeroberflächen mit Anleitungen
label: Instructional UI
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: b39507fb1333fdb642601c6b4828c3d160c6ceb5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656435"
---
# <a name="instructional-ui-guidelines"></a>Richtlinien für Benutzeroberflächen mit Anleitungen



In gewissen Fällen kann es hilfreich sein, Anleitungen bereitzustellen, um Benutzern die Funktionen in Ihrer App zu demonstrieren, die möglicherweise nicht offensichtlich sind, z. B. bestimmte Touch-Interaktionen. In diesen Fällen müssen Sie Anleitungen auf der Benutzeroberfläche für den Benutzer bereitstellen, damit sie diese Funktionen, die sie möglicherweise übersehen haben, nutzen können.

## <a name="when-to-use-instructional-ui"></a>Gründe für die Verwendung einer Benutzeroberfläche mit Anleitungen

Benutzeroberflächen mit Anleitungen sollten Sie sparsam einsetzen. Bei übermäßiger Nutzung werden die Anleitungen leicht ignoriert oder verärgern die Benutzer und sind somit wirkungslos.

Benutzeroberflächen mit Anleitungen sollten dazu dienen, dass Benutzer auf wichtige und nicht offensichtliche Funktionen Ihrer App hingewiesen werden, wie z. B. Touchgesten oder Einstellungen, die für sie von Interesse sind. Sie können auch dazu verwendet werden, Benutzer über neue Funktionen und Änderungen in der App aufmerksam zu machen, die sie andernfalls möglicherweise übersehen.

Benuteroberflächen mit Anleitungen sollten nur dann zum Vermitteln grundlegender Funktionen Ihrer App verwendet werden, wenn Ihre App von Touchgesten abhängig ist.

## <a name="principles-of-writing-instructional-ui"></a>Grundsätze für das Entwerfen von Benutzeroberflächen mit Anleitungen

Gute Benutzeroberflächen mit Anleitungen sind hilfreich und lehrreich für Benutzer und optimieren die Benutzerfreundlichkeit. Folgende Merkmale sollten diese Benutzeroberflächen ausmachen:

-   **Einfach:** Benutzer nicht ihre Erfahrungen mit komplizierten Informationen unterbrochen werden sollen.
-   **Unvergesslich:** Benutzer möchten nicht die gleichen Anweisungen finden Sie unter jedem Versuch, eine Aufgabe, damit die Anweisungen so gestaltet sein, die sie wissen müssen.
-   **Unmittelbar relevant:** Wenn es sich bei die Lehrzwecken Benutzeroberfläche einen Benutzer über etwas unterrichten nicht, die sie sofort durchführen möchten, einen Grund für die Aufmerksamkeit darauf keine.

Setzen Sie Benutzeroberflächen mit Anleitungen nicht übermäßig ein, und achten Sie darauf, die richtigen Themen auszuwählen. Stellen Sie sicher, dass Folgendes nicht mithilfe von Benutzeroberflächen mit Anleitungen vermittelt wird:

-   **Grundlegende Features:** Wenn ein Benutzer die Anweisungen zur Verwendung Ihrer app benötigt, markieren Sie die app-Design intuitiver.
-   **Offensichtlichen Features:** Wenn ein Benutzer eine Funktion ohne die Anweisung selbst ermitteln kann, klicken Sie dann erhalten dem Lehrzwecken UI nur auf die Weise.
-   **Die komplexen Funktionen:** Anweisungstext Benutzeroberfläche muss präzise sein, und Benutzer möchten die komplexen Funktionen sind in der Regel bereit, um Anweisungen zu suchen und müssen nicht ihnen zugewiesen werden.

Vermeiden Sie es, die Benutzererfahrung durch Ihre Benutzeroberfläche mit Anleitungen zu beeinträchtigen. Nicht empfohlene Vorgehensweise:

-   **Ungewöhnliche wichtige Informationen:** Anweisungstext Benutzeroberfläche sollte nie im Weg von anderen Funktionen Ihrer App zu erhalten.
-   **Erzwingen Sie Benutzern die Teilnahme an:** Benutzer sollten Anweisungstext Benutzeroberfläche und weiterhin ausgeführt, über die app ignorieren können.
-   **Wiederholen Sie diesen Schritt Informationen angezeigt:** Belästigen Sie nicht den Benutzer mit dem Lehrzwecken Benutzeroberfläche, selbst wenn sie es beim ersten ignorieren. Das Hinzufügen einer Einstellung, mit der Anleitungen erneut auf der Benutzeroberfläche angezeigt werden können, ist die bessere Lösung.

## <a name="examples-of-instructional-ui"></a>Beispiele für Benutzeroberflächen mit Anleitungen

Unten sind einige Fälle aufgeführt, in denen eine Benutzeroberfläche mit Anleitungen für Benutzer sinnvoll ist:

-   **Unterstützen von Benutzern, die Touch-Interaktionen zu ermitteln.** Der folgende Screenshot zeigt eine Benutzeroberfläche mit Anleitungen, mit deren Hilfe ein Spieler lernt, wie er im Spiel „Cut the Rope“ Touchgesten verwenden kann.

    ![Screenshot aus dem Spiel, der die Meldung „Slide across to cut the rope“ (Führen Sie zum Trennen des Seils eine Ziehbewegung aus.) der Benutzeroberfläche mit Anweisungen zeigt.](images/in-game-controls-3.png)

-   **Machen einen guten ersten Eindruck.** Beim ersten Starten von Movie Moments wird der Benutzer mithilfe von Anleitungen auf der Benutzeroberfläche dazu aufgefordert, mit der Erstellung von Filmen zu beginnen, ohne dass seine Benutzererfahrung beeinträchtig wird.

    ![Startbildschirm für die App „Movie Moments“](images/instructional-ui-movie.png)

-   **Hier können Benutzer den nächsten Schritt in eine komplizierte Aufgabe sind.** In der Windows-Mail-App werden die Benutzer mithilfe eines Hinweises am unteren Rand des Posteingangs zu den **Einstellungen** für den Zugriff auf ältere Nachrichten geleitet.

    ![Zugeschnittener Screenshot der Windows Mail-App, der eine Meldung einer Benutzeroberfläche mit Anleitungen zeigt](images/instructional-ui-mail-inbox.png)

    Wenn der Benutzer auf die Meldung klickt, wird auf der rechten Seite des Bildschirms das **Einstellungen**-Flyout der App angezeigt, in dem der Benutzer die Aufgabe ausführen kann. Diese Screenshots zeigen die Mail-App vor und nach dem Klicken auf die Meldung der Benutzeroberfläche mit Anleitungen durch den Benutzer.

    | Vorher                                                               | Nachher                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Screenshot der Windows-Mail-App](images/instructional-ui-mail.png) | ![Screenshot der Windows Mail-App mit einem erweiterten Einstellungen-Flyout](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>Verwandte Artikel

* [Richtlinien zum app-Hilfe](guidelines-for-app-help.md)
