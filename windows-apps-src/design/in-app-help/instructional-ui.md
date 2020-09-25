---
Description: Entwerfen Sie eine Benutzeroberfläche für die Benutzeroberfläche, mit der Benutzer erfahren, wie Sie mit Ihrer Windows-App arbeiten.
title: Richtlinien für das Entwerfen von Benutzeroberflächen mit Anleitungen
label: Instructional UI
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: 47779a6d31d60fe87f639dfadd48275bfb15359e
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217123"
---
# <a name="instructional-ui-guidelines"></a>Richtlinien für Benutzeroberflächen mit Anleitungen



In gewissen Fällen kann es hilfreich sein, Anleitungen bereitzustellen, um Benutzern die Funktionen in Ihrer App zu demonstrieren, die möglicherweise nicht offensichtlich sind, z. B. bestimmte Touch-Interaktionen. In diesen Fällen müssen Sie Anleitungen auf der Benutzeroberfläche für den Benutzer bereitstellen, damit sie diese Funktionen, die sie möglicherweise übersehen haben, nutzen können.

## <a name="when-to-use-instructional-ui"></a>Gründe für die Verwendung einer Benutzeroberfläche mit Anleitungen

Benutzeroberflächen mit Anleitungen sollten Sie sparsam einsetzen. Bei übermäßiger Nutzung werden die Anleitungen leicht ignoriert oder verärgern die Benutzer und sind somit wirkungslos.

Benutzeroberflächen mit Anleitungen sollten dazu dienen, dass Benutzer auf wichtige und nicht offensichtliche Funktionen Ihrer App hingewiesen werden, wie z. B. Touchgesten oder Einstellungen, die für sie von Interesse sind. Sie können auch dazu verwendet werden, Benutzer über neue Funktionen und Änderungen in der App aufmerksam zu machen, die sie andernfalls möglicherweise übersehen.

Benuteroberflächen mit Anleitungen sollten nur dann zum Vermitteln grundlegender Funktionen Ihrer App verwendet werden, wenn Ihre App von Touchgesten abhängig ist.

## <a name="principles-of-writing-instructional-ui"></a>Grundsätze für das Entwerfen von Benutzeroberflächen mit Anleitungen

Gute Benutzeroberflächen mit Anleitungen sind hilfreich und lehrreich für Benutzer und optimieren die Benutzerfreundlichkeit. Dieser sollte wie folgt lauten:

-   **Einfach:** Benutzer möchten nicht, dass Ihre Benutzeroberflächen mit komplizierten Informationen unterbrochen werden.
-   **Einprägsam:** Benutzer möchten nicht, dass die gleichen Anleitungen bei jedem Versuch, eine Aufgabe auszuführen, angezeigt werden. Die Anleitungen müssen somit leicht einprägsam sein.
-   **Unmittelbar relevant:** Wenn die Anleitungen auf der Benutzeroberfläche sich nicht auf eine Aktion beziehen, die die Benutzer gerade ausführen möchten, sehen sie keinen Grund, diesen ihre Aufmerksamkeit zu schenken.

Setzen Sie Benutzeroberflächen mit Anleitungen nicht übermäßig ein, und achten Sie darauf, die richtigen Themen auszuwählen. Stellen Sie sicher, dass Folgendes nicht mithilfe von Benutzeroberflächen mit Anleitungen vermittelt wird:

-   **Grundlegende Features:** Wenn Benutzer Anleitungen zum Verwenden Ihrer App benötigen, sollten Sie das App-Design intuitiver gestalten.
-   **Offensichtliche Features:** Wenn eine Funktion für einen Benutzer ohne Anleitung offensichtlich ist, wird ihn eine Benutzeroberfläche mit Anleitungen nur behindern.
-   **Komplexe Features:** Benutzeroberflächen mit Anleitungen müssen präzise sein. Benutzer, für die komplexe Funktionen von Interesse sind, sind in der Regel bereit, selbst nach Anleitungen zu suchen.

Vermeiden Sie es, die Benutzererfahrung durch Ihre Benutzeroberfläche mit Anleitungen zu beeinträchtigen. Vermeiden Sie Folgendes:

-   **Überdecken wichtiger Informationen:** Benutzeroberflächen mit Anleitungen sollten niemals anderen Features Ihrer App im Weg stehen.
-   **Benutzer zum Teilnehmen zwingen:** Benutzer sollten in der Lage sein, Anleitungen auf der Benutzeroberfläche zu ignorieren und mit der Verwendung der App fortzufahren.
-   **Wiederholte Anzeige von Informationen:** Belästigen Sie die Benutzer nicht mit der wiederholten Anzeige von Anleitungen auf der Benutzeroberfläche, auch wenn diese bei der ersten Anzeige ignoriert wurden. Das Hinzufügen einer Einstellung, mit der Anleitungen erneut auf der Benutzeroberfläche angezeigt werden können, ist die bessere Lösung.

## <a name="examples-of-instructional-ui"></a>Beispiele für Benutzeroberflächen mit Anleitungen

Unten sind einige Fälle aufgeführt, in denen eine Benutzeroberfläche mit Anleitungen für Benutzer sinnvoll ist:

-   **Unterstützen der Benutzer beim Kennenlernen der Interaktionen per Toucheingabe.** Der folgende Screenshot zeigt eine Benutzeroberfläche mit Anleitungen, mit deren Hilfe ein Spieler lernt, wie er im Spiel „Cut the Rope“ Touchgesten verwenden kann.

    ![Screenshot aus dem Spiel, der die Meldung „Slide across to cut the rope“ (Führen Sie zum Trennen des Seils eine Ziehbewegung aus.) der Benutzeroberfläche mit Anweisungen zeigt.](images/in-game-controls-3.png)

-   **Erzielen eines ausgezeichneten ersten Eindrucks.** Beim ersten Starten von Movie Moments wird der Benutzer mithilfe von Anleitungen auf der Benutzeroberfläche dazu aufgefordert, mit der Erstellung von Filmen zu beginnen, ohne dass seine Benutzererfahrung beeinträchtig wird.

    ![Startbildschirm für die App „Movie Moments“](images/instructional-ui-movie.png)

-   **Führen der Benutzer zum nächsten Schritt einer komplizierten Aufgabe.** In der Windows-Mail-App werden die Benutzer mithilfe eines Hinweises am unteren Rand des Posteingangs zu den **Einstellungen** für den Zugriff auf ältere Nachrichten geleitet.

    ![Zugeschnittener Screenshot der Windows Mail-App, der eine Meldung einer Benutzeroberfläche mit Anleitungen zeigt](images/instructional-ui-mail-inbox.png)

    Wenn der Benutzer auf die Meldung klickt, wird auf der rechten Seite des Bildschirms das **Einstellungen**-Flyout der App angezeigt, in dem der Benutzer die Aufgabe ausführen kann. Diese Screenshots zeigen die Mail-App vor und nach dem Klicken auf die Meldung der Benutzeroberfläche mit Anleitungen durch den Benutzer.

    | Vorher                                                               | Danach                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Screenshot der Windows-Mail-App](images/instructional-ui-mail.png) | ![Screenshot der Windows Mail-App mit einem erweiterten Einstellungen-Flyout](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>Verwandte Artikel

* [Anleitungen für die App-Hilfe](guidelines-for-app-help.md)
