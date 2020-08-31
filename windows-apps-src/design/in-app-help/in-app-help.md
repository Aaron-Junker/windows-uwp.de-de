---
description: Erfahren Sie mehr über die Verwendung der reaktiven in-App-Hilfe als Standardmethode zum Anzeigen der Hilfe für Benutzer und über Typen der in-App-Hilfe.
title: Richtlinien zum Entwerfen von In-App-Hilfe.
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: d7144a72dd3af1c9c902e0dfd401799f50ea55b0
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054170"
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

-   **Seien Sie präzise:** Eine umfangreiche Bibliothek mit Hilfethemen ist umständlich und für eine In-App-Hilfe nicht geeignet.
-   **Konsistent sein:** Stellen Sie sicher, dass Benutzer die Hilfeseiten in jedem Bereich der App auf die gleiche Weise aufrufen können. Sie sollten nie danach suchen müssen.
-   **Benutzer überfliegen Inhalte und lesen sie nicht:** Da sich das Hilfethema, nach dem ein Benutzer sucht, möglicherweise auf derselben Seite wie andere Hilfethemen befindet, müssen Sie darauf achten, dass das gesuchte Hilfethema leicht zu erkennen ist.


#### <a name="popups"></a>Popups

Popups bieten die Möglichkeit, stark kontextbezogene Hilfe einzublenden. Das heißt, es werden Anweisungen und Ratschläge angezeigt, die für die jeweilige Aufgabe des Benutzers relevant sind.

-   **Auf ein Thema konzentrieren:** Ein Popup bietet noch weniger Platz als eine Hilfeseite. Hilfepopups müssen sich speziell auf eine einzelne Aufgabe beziehen, damit sie effektiv sind.
-   **Die Sichtbarkeit ist wichtig:** Da Hilfepopups nur an einem Ort angezeigt werden können, stellen Sie sicher, dass sie für die Benutzer deutlich sichtbar sind, ohne zu stören. Wenn der Benutzer das Popup nicht sieht, ignoriert er es womöglich und sucht nach einer Hilfeseite.
-   **Beanspruchen Sie nicht zu viele Ressourcen:** Die Hilfe sollte keine Verzögerungen verursachen oder langsam geladen werden. Popups mit Videos oder Audiodateien oder Bildern mit hoher Auflösung führen beim Benutzer eher zur Verärgerung anstatt ihm zu helfen.

#### <a name="descriptions"></a>Beschreibungen

In manchen Fällen kann es hilfreich sein, weitere Informationen zu einem Feature bereitzustellen, wenn ein Benutzer es prüft. Beschreibungen ähneln Anweisungen auf der Benutzeroberfläche. Doch der Hauptunterschied besteht darin, dass Anweisungen auf der Benutzeroberfläche versuchen, dem Benutzer Lerninhalte zu Features zu vermitteln, die er noch nicht kennt. Eine ausführliche Beschreibung dagegen vertieft die Kenntnisse des Benutzers über App-Features, an denen er bereits interessiert ist.

-   **Keine Grundlagen vermitteln:** Angenommen, der Benutzer weiß im Großen und Ganzen bereits, wie das beschriebene Element verwendet wird. In dem Fall ist es hilfreich, etwas zu verdeutlichen oder weitere Informationen anzubieten. Bereits Bekanntes zu vermitteln ist dagegen nicht sinnvoll.
-   **Beschreiben Sie interessante Interaktionen:** Beschreibungen sind ideal geeignet, um Benutzern zu zeigen, wie diesen bereits bekannte Features interagieren können. Dadurch erfahren die Benutzer mehr über Funktionen, die sie bereits gerne verwenden.
-   **Stören Sie nicht:** Ähnlich wie Anweisungen in der Benutzeroberfläche dürfen Beschreibungen die Nutzung der App durch die Benutzer nicht behindern.

## <a name="related-articles"></a>Verwandte Artikel

* [Anleitungen für die App-Hilfe](guidelines-for-app-help.md)
