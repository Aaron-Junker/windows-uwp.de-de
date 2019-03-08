---
Description: Entwerfen Sie effektive Hilfe, die reaktiv innerhalb Ihrer App angezeigt wird.
title: Richtlinien zum Entwerfen von In-App-Hilfe.
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: 4783d28e4da6c06df0d0676f4a7d28ef3995481a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610055"
---
# <a name="in-app-help-pages"></a>In-App-Hilfeseiten

In den meisten Fällen empfiehlt es sich, dass die Hilfe in der Anwendung angezeigt wird, wenn der Benutzer dies wünscht.

## <a name="when-to-use-in-app-help-pages"></a>Gründe für die Verwendung von In-App-Hilfeseiten

Die Hilfe für den Benutzer sollte standardmäßig als In-App-Hilfe angezeigt werden. Sie sollte für jede Hilfe eingesetzt werden, die einfach und unkompliziert ist und dem Benutzer keine neuen Inhalte vermittelt. Anweisungen, Ratschläge sowie Tipps & Tricks eignen sich für In-App-Hilfe.

Komplexe Anweisungen oder Lernprogramme sind nicht schnell zu überblicken, und sie belegen viel Speicherplatz. Aus diesem Grund sollten sie extern gehostet und nicht in die App selbst integriert werden.

Benutzer sollten nicht lange nach Hilfe für grundlegende Anweisungen oder nach neuen Features suchen müssen. Wenn Sie Hilfe benötigen, die den Benutzern Lerninhalte vermittelt, verwenden Sie Anweisungen auf der Benutzeroberfläche.

## <a name="types-of-in-app-help"></a>Arten von In-App-Hilfe

In-App-Hilfe kann unterschiedliche Formen annehmen, entspricht jedoch in Bezug auf Gestaltung und Benutzerfreundlichkeit den gleichen allgemeinen Grundsätzen.

#### <a name="help-pages"></a>Hilfeseiten

Nützliche Anweisungen können schnell und einfach auf einer oder mehreren separaten Hilfeseiten in Ihrer App angezeigt werden.

-   **Prägnant sein:** Eine umfangreiche Bibliothek von Hilfethemen ist sehr unhandlich und Zahlenangaben in-app-Hilfe.
-   **Konsistent sein:** Stellen Sie sicher, dass Benutzer Ihre Hilfeseiten die gleiche Weise von einem beliebigen Teil der app erreichen können. Sie sollten nie danach suchen müssen.
-   **Benutzer zu scannen, nicht gelesen werden:** Da die Hilfe, die gesuchten Benutzer auf derselben Seite wie die anderen Hilfethemen sein könnte, stellen Sie sicher, dass sie leicht erkennen können, müssen sie eine konzentrieren.


#### <a name="popups"></a>Popups

Popups bieten die Möglichkeit, stark kontextbezogene Hilfe einzublenden. Das heißt, es werden Anweisungen und Ratschläge angezeigt, die für die jeweilige Aufgabe des Benutzers relevant sind.

-   **Fokus auf ein Problem:** Speicherplatz ist noch mehr als eine Hilfeseite in einem Popupfenster eingeschränkt. Hilfepopups müssen sich speziell auf eine einzelne Aufgabe beziehen, damit sie effektiv sind.
-   **Sichtbarkeit ist wichtig:** Da die Hilfe-Popups nur von einem Ort angezeigt werden können, stellen Sie sicher, dass sie eindeutig für den Benutzer sichtbar sind, ohne dass obstructive. Wenn der Benutzer das Popup nicht sieht, ignoriert er es womöglich und sucht nach einer Hilfeseite.
-   **Verwenden Sie nicht zu viele Ressourcen:** Hilfe, die Verzögerung darf nicht oder werden langsam geladen. Popups mit Videos oder Audiodateien oder Bildern mit hoher Auflösung führen beim Benutzer eher zur Verärgerung anstatt ihm zu helfen.

#### <a name="descriptions"></a>Beschreibungen

In manchen Fällen kann es hilfreich sein, weitere Informationen zu einem Feature bereitzustellen, wenn ein Benutzer es prüft. Beschreibungen ähneln Anweisungen auf der Benutzeroberfläche. Doch der Hauptunterschied besteht darin, dass Anweisungen auf der Benutzeroberfläche versuchen, dem Benutzer Lerninhalte zu Features zu vermitteln, die er noch nicht kennt. Eine ausführliche Beschreibung dagegen vertieft die Kenntnisse des Benutzers über App-Features, an denen er bereits interessiert ist.

-   **Bringen Sie die Grundlagen nicht:** Wird davon ausgegangen Sie, dass der Benutzer bereits über die Grundlagen der Gewusst wie: Verwenden Sie das Element beschriebenen kennt. In dem Fall ist es hilfreich, etwas zu verdeutlichen oder weitere Informationen anzubieten. Bereits Bekanntes zu vermitteln ist dagegen nicht sinnvoll.
-   **Beschreiben Sie die interessante Interaktionen:** Eine der besten Verwendungsoptionen für Beschreibungen ist informieren den Benutzer auf die Interaktion einer Features, denen sie bereits kennen. Dadurch erfahren die Benutzer mehr über Funktionen, die sie bereits gerne verwenden.
-   **Bleiben Sie aus dem Weg:** Viel wie Benutzeroberfläche die Lehre, müssen Beschreibungen vermeiden stören könnte eines Benutzers, der app.

## <a name="related-articles"></a>Verwandte Artikel

* [Richtlinien zum app-Hilfe](guidelines-for-app-help.md)
