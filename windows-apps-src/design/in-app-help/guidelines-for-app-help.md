---
Description: These guidelines describe how to design effective Help content for your app.
title: Anleitungen für die App-Hilfe
label: Guidelines for app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.assetid: c3e73f9b-4839-4804-b379-c95b0ca4fbe8
ms.localizationpriority: medium
ms.openlocfilehash: bd2174c6bbfb84a3ea6c6956e1d0b02ed5c9be33
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "8331528"
---
# <a name="guidelines-for-app-help"></a>Anleitungen für die App-Hilfe



Anwendungen können sehr komplex sein. Sie können die Erfahrung für die Benutzer durch die Bereitstellung einer effektiven Hilfe erheblich verbessern. Nicht alle Anwendungen müssen Hilfe für ihre Benutzer bereitstellen, und welche Art von Hilfe bereitgestellt werden sollte, kann je nach Anwendung unterschiedlich sein.

Wenn Sie Hilfe bereitstellen möchten, befolgen Sie diese Richtlinien beim Erstellen. Hilfe, die nicht nützlich ist, kann schlimmer sein als gar keine Hilfe.

## <a name="intuitive-design"></a>Intuitives Design

Hilfeinhalte können zwar nützlich sein, doch in Ihrer App darf dies nicht das einzige Merkmal für Benutzerfreundlichkeit sein. Wenn der Benutzer die wichtigen Funktionen der App nicht sofort erkennen und nutzen kann, wird der Benutzer die App nicht verwenden. Dieser erste Eindruck kann durch keine Hilfe geändert werden, so umfassend oder hochwertig sie auch sein mag.

Ein intuitives und benutzerfreundliches Design ist der erste Schritt beim Erstellen einer nützlichen Hilfe. Dadurch bleibt nicht nur das Interesse des Benutzers lang genug erhalten, dass dieser anspruchsvollere Features nutzt, sondern es werden ihm außerdem Kenntnisse über die Kernfunktionen der App vermittelt, auf denen der Benutzer bei der weiteren Nutzung der App aufbauen und dazulernen kann.

## <a name="general-instructions"></a>Allgemeine Hinweise

Ein Benutzer wird erst dann nach Hilfeinhalten suchen, wenn bereits ein Problem aufgetreten ist. Daher muss die Hilfe eine schnelle und effektive Lösung für das jeweilige Problem liefern. Wenn Hilfe nicht auf Anhieb sinnvoll oder sie zu kompliziert ist, neigen Benutzer dazu, sie zu ignorieren.

Jede Hilfe sollte unabhängig von ihrer Art folgenden Prinzipien entsprechen:

-   **Leicht verständlich:** Hilfe, die den Benutzer verwirrt, ist schlimmer als gar keine Hilfe.

-   **Geradlinig:** Benutzer, die Hilfe benötigen, möchten klare Antworten, die ihnen direkt angezeigt werden.

-   **Relevant:** Benutzer möchten nicht nach ihrem spezifischen Problem suchen müssen. Sie möchten, dass ihnen möglichst relevante Hilfe unmittelbar angezeigt wird (man spricht von „kontextbezogener Hilfe“), oder sie möchten eine leicht zu bedienende Oberfläche.

-   **Direkt:** Wenn ein Benutzer nach Hilfe sucht, möchte er auch Hilfe bekommen. Enthält die App Seiten zum Melden von Fehlern, für Feedback, zum Anzeigen der Nutzungsbedingungen oder ähnliche Funktionen, ist es in Ordnung, wenn diese Seiten über die Hilfe aufgerufen werden können. Aber sie sollten eine untergeordnete Position auf der Haupthilfeseite und nicht den gleichen oder einen höheren Stellenwert haben.

-   **Konsistent:** Die Hilfe ist unabhängig von ihrer Art ein Teil der App und sollte wie jeder andere Teil der Benutzeroberfläche behandelt werden. Für die von Ihnen angebotene Hilfe sollten die gleichen Gestaltungsprinzipien gelten wie für die eigentliche App, nämlich Benutzerfreundlichkeit, Barrierefreiheit und Stil.

## <a name="types-of-help"></a>Arten von Hilfe

Es gibt drei grundlegende Kategorien von Hilfeinhalten, die jeweils mit unterschiedlichen Stärken verbunden sind und sich für unterschiedliche Zwecke eignen. Verwenden Sie in der App je nach Bedarf eine beliebige Kombination dieser Kategorien.

#### <a name="instructional-ui"></a>Benutzeroberfläche mit Anleitungen

In der Regel sollten Benutzer die Kernfunktionen der App ohne Anweisungen verwenden können. Aber manchmal ist bei der App eine bestimmte Geste erforderlich, oder möglicherweise gibt es sekundäre Features, die nicht sofort offensichtlich sind. In diesem Fall sollte die Benutzeroberfläche Anweisungen enthalten, die Benutzern erklären, wie bestimmte Aufgaben ausgeführt werden.

[Siehe „Richtlinien für Anweisungen auf Benutzeroberflächen“](instructional-ui.md)

#### <a name="in-app-help"></a>In-App-Hilfe

Die Standardmethode zur Darstellung von Hilfe ist das Anzeigen der Hilfe innerhalb der Anwendung auf Anforderung des Benutzers. Es gibt verschiedene Möglichkeiten, dies umzusetzen, z.B. durch Hilfeseiten oder informative Beschreibungen. Diese Methode ist ideal für allgemeine Hilfe, die Fragen eines Benutzers unkompliziert und unmittelbar beantwortet.

[Siehe „Richtlinien für In-App-Hilfe“](in-app-help.md)

#### <a name="external-help"></a>Externe Hilfe

Für ausführliche Lernprogramme, erweiterte Funktionen oder Bibliotheken mit Hilfethemen, die zu groß für die Anwendung sind, sind Links zu externen Webseiten ideal. Diese Links sollten nach Möglichkeit sparsam verwendet werden, da sie die Anwendungsnutzung durch den Benutzer unterbrechen.

[Siehe „Richtlinien für externe Hilfe“](external-help.md)


